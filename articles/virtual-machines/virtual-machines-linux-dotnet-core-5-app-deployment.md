<properties
   pageTitle="Implementatie van toepassingen met de extensies van de virtuele Machine automatiseren | Microsoft Azure"
   description="Azure Virtual Machine DotNet Core zelfstudie"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# <a name="application-deployment-with-azure-resource-manager-templates"></a>Implementatie van toepassingen met Azure Resource Manager-sjablonen

Zodra alle Azure infrastructurele eisen zijn geïdentificeerd en vertaald in een sjabloon voor de implementatie, de werkelijke toepassingsimplementatie aandacht wordt besteed. Toepassingsimplementatie hier verwijst naar de feitelijke binaire waarden van webtoepassingen naar Azure bronnen te installeren. Voor de winkel monster .net Core, NGINX en Supervisor moeten worden geïnstalleerd en geconfigureerd op elke virtuele machine. De binaire bestanden voor muziek winkel moeten worden geïnstalleerd op de virtuele machine en de muziek database vooraf gemaakt.

Dit document details over hoe extensies van de virtuele Machine kunnen automatiseren installatie van toepassingen en configuratie van Azure virtual machines. Alle afhankelijkheden en unieke configuraties zijn gemarkeerd. Voor de beste ervaring, een exemplaar van de oplossing voor uw abonnement Azure en werk samen met de sjabloon Azure Resource Manager vooraf te implementeren. De volledige sjabloon vindt u hier – [Muziek winkel implementatie op Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="configuration-script"></a>Configuratiescript

Virtual Machine-extensies zijn speciale programma's die ten opzichte van virtuele machines voor automatisering van de configuratie uitvoeren. Extensies zijn beschikbaar voor veel specifieke doeleinden, zoals antivirusprogramma's en configuratie voor logboekregistratie, Docker configuratie. De extensie van een aangepast script kan worden gebruikt om een script uitvoeren voor een virtuele machine. Met het monster muziek winkel is tot aan de uitbreiding aangepast script de Ubuntu virtuele machines configureren en installeren van de toepassing van de winkel.

Voordat u met informatie over hoe extensies van de virtuele machine worden gedefinieerd in een sjabloon Azure Resource Manager, onderzoekt het script dat wordt uitgevoerd. Dit script wordt geconfigureerd voor de virtuele machine van Ubuntu als host voor de toepassing van de winkel. Wanneer uitvoert, het script worden alle geïnstalleerd nodig software, installeert u de toepassing van de winkel muziek van het besturingselement en de database voorbereiden. 

Voor meer informatie over het hosten van een .net Core-toepassingen op Linux, Zie [publiceren naar een Linux-productieomgeving](https://docs.asp.net/en/latest/publishing/linuxproduction.html). 

> In dit voorbeeld is bedoeld voor demonstratiedoeleinden.

```none
#!/bin/bash

# install dotnet core
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
sudo apt-get update
sudo apt-get install -y dotnet-dev-1.0.0-preview2-003121

# download application
sudo wget https://raw.github.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/music-store-azure-demo-pub.tar /
sudo mkdir /opt/music
sudo tar -xf music-store-azure-demo-pub.tar -C /opt/music

# install nginx, update config file
sudo apt-get install -y nginx
sudo service nginx start
sudo touch /etc/nginx/sites-available/default
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/nginx-config/default -O /etc/nginx/sites-available/default
sudo cp /opt/music/nginx-config/default /etc/nginx/sites-available/
sudo nginx -s reload

# update and secure music config file
sed -i "s/<replaceserver>/$1/g" /opt/music/config.json
sed -i "s/<replaceuser>/$2/g" /opt/music/config.json
sed -i "s/<replacepass>/$3/g" /opt/music/config.json
sudo chown $2 /opt/music/config.json
sudo chmod 0400 /opt/music/config.json

# config supervisor
sudo apt-get install -y supervisor
sudo touch /etc/supervisor/conf.d/music.conf
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/supervisor/music.conf -O /etc/supervisor/conf.d/music.conf
sudo service supervisor stop
sudo service supervisor start

# pre-create music store database
/usr/bin/dotnet /opt/music/MusicStore.dll &
```

## <a name="vm-script-extension"></a>VM Script extensie

VM-extensies kan worden uitgevoerd tegen een virtuele machine tegelijk maken door de bron van de uitbreiding in de sjabloon Azure Resource Manager. De uitbreiding kan worden toegevoegd met de wizard Resources toevoegen Visual Studio of door middel van geldige JSON in de sjabloon. De resource Script extensie is genest in de bron van de virtuele Machine; Deze kunnen worden gezien in het volgende voorbeeld.

Volg deze koppeling als de JSON voorbeeld binnen de sjabloon Resource Manager – [VM Script extensie](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L359)wilt zien. 

U ziet in de onderstaande JSON dat het script is opgeslagen in de GitHub. Dit script kan ook worden opgeslagen in Azure Blob-opslag. Azure Resource Manager-sjablonen kunnen ook gebouwd sjabloon parameters kunnen worden gebruikt als parameters voor de uitvoering van het script dat de uitvoering van Scripttekenreeks. In dit geval gegevens die worden verstrekt bij de implementatie van de sjablonen en deze waarden kunnen vervolgens worden gebruikt bij het uitvoeren van het script.

```none
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

Zie voor meer informatie over het gebruik van het aangepaste script extensie [scriptextensies aangepaste sjablonen Resource Manager](./virtual-machines-linux-extensions-customscript.md).

## <a name="next-step"></a>Volgende stap

<hr>

[Ontdek meer Azure Resource Manager-sjablonen](https://github.com/Azure/azure-quickstart-templates)
