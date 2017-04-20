<properties
    pageTitle="De extensie CustomScript gebruikt op een VM Linux | Microsoft Azure"
    description="Informatie over het implementeren van toepassingen op Linux virtuele Machines in Azure is gemaakt met behulp van het implementatiemodel klassiek met de extensie CustomScript."
    editor="tysonn"
    manager="timlt"
    documentationCenter=""
    services="virtual-machines-linux"
    authors="gbowerman"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="multiple"
    ms.tgt_pltfrm="linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="guybo"/>

#<a name="deploy-a-lamp-app-using-the-azure-customscript-extension-for-linux"></a>Implementeren van een licht app met de extensie Azure CustomScript voor Linux#

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


De Microsoft Azure CustomScript uitbreiding voor Linux kunt u uw virtuele machines (VMs) aanpassen door het uitvoeren van willekeurige programmacode die is geschreven in een scripttaal die wordt ondersteund door de VM (bijvoorbeeld Python en Bash). Dit biedt een zeer flexibele manier voor het automatiseren van de implementatie van toepassingen op meerdere computers.

U kunt de uitbreiding van de CustomScript met de klassieke Azure portal, Windows PowerShell of de Azure opdrachtregelinterface (CLI Azure) implementeren.

In dit artikel we gebruiken de CLI Azure voor de implementatie van een eenvoudige toepassing LAMP voor een VM Ubuntu gemaakt met behulp van het implementatiemodel klassiek.

## <a name="prerequisites"></a>Vereisten

In dit voorbeeld maakt u eerst twee Azure VMs met Ubuntu 14.04 of hoger. Het VMs worden *script vm* en *licht vm*genoemd. Unieke namen gebruiken bij het maken van het VMs. Een wordt gebruikt voor het uitvoeren van de CLI-opdrachten en een implementatie van de toepassing van de LAMP wordt gebruikt.

Ook moet u een account Azure opslag en een sleutel te krijgen (u krijgt dit van de klassieke Azure portal).

Als u hulp bij het maken van Linux VMs op Azure verwijzen naar [een virtuele Machine wordt uitgevoerd op Linux maken](virtual-machines-linux-classic-createportal.md).

De opdrachten install Ubuntu Stel kunt, maar u de installatie voor alle ondersteunde Linux distro.

De script-vm VM moet Azure CLI geïnstalleerd met een verbinding met Azure. Raadpleeg voor hulp bij dit [installeren](../xplat-cli-install.md)en configureren van de opdrachtregelinterface van Azure.

## <a name="upload-a-script"></a>Een script uploaden

We gebruiken de extensie CustomScript een script uitvoeren op een externe VM te installeren van de LAMP stack en maak een PHP-pagina. Het script toegang vanaf elke locatie zullen we het uploaden als een Azure blob.

### <a name="script-overview"></a>Script-overzicht

Het scriptvoorbeeld een LAMP stack aan (zoals het instellen van een stille installatie van MySQL) Ubuntu installeert, schrijft een eenvoudig PHP-bestand en Apache wordt gestart.

    #!/bin/bash
    # set up a silent install of MySQL
    dbpass="mySQLPassw0rd"

    export DEBIAN_FRONTEND=noninteractive
    echo mysql-server-5.6 mysql-server/root_password password $dbpass | debconf-set-selections
    echo mysql-server-5.6 mysql-server/root_password_again password $dbpass | debconf-set-selections

    # install the LAMP stack
    apt-get -y install apache2 mysql-server php5 php5-mysql  

    # write some PHP
    echo \<center\>\<h1\>My Demo App\</h1\>\<br/\>\</center\> > /var/www/html/phpinfo.php
    echo \<\?php phpinfo\(\)\; \?\> >> /var/www/html/phpinfo.php

    # restart Apache
    apachectl restart

### <a name="upload-script"></a>Script uploaden

Sla het script op als een tekstbestand, bijvoorbeeld *install_lamp.sh*, en deze vervolgens uploaden naar Azure opslag. U kunt dit gemakkelijk doen met Azure CLI. In het volgende voorbeeld wordt het bestand geüpload naar een opslag container met de naam 'scripts'. Als de container niet bestaat moet u deze eerst maken.

    azure storage blob upload -a <yourStorageAccountName> -k <yourStorageKey> --container scripts ./install_lamp.sh

Ook een JSON-bestand met instructies voor het downloaden van het script van Azure opslag maken. Dit opslaan als *public_config.json* ("mystorage" vervangen door de naam van de account van uw opslag):

    {"fileUris":["https://mystorage.blob.core.windows.net/scripts/install_lamp.sh"], "commandToExecute":"sh install_lamp.sh" }


## <a name="deploy-the-extension"></a>De extensie implementeren

Nu kunt u de volgende opdracht voor de implementatie van de Linux-CustomScript-extensie voor externe VM met behulp van de CLI Azure.

    azure vm extension set -c "./public_config.json" lamp-vm CustomScript Microsoft.Azure.Extensions 2.0

De vorige opdracht gedownload en wordt het script *install_lamp.sh* uitgevoerd op de VM *licht vm*genoemd.

Aangezien de app een webserver bevat, moet u een HTTP-poort voor luisteren op de externe VM openen met de volgende opdracht.

    azure vm endpoint create -n Apache -o tcp lamp-vm 80 80

## <a name="monitoring-and-troubleshooting"></a>Controle en probleemoplossing

U kunt controleren op hoe goed het aangepaste script wordt uitgevoerd door te kijken naar het logboekbestand op de externe VM. SSH *licht vm* en de staart van het bestand met de volgende opdracht.

    cd /var/log/azure/customscript
    tail -f handler.log

Nadat u de extensie CustomScript uitvoert, kunt u bladeren naar de PHP-pagina die u hebt gemaakt voor meer informatie. De PHP-pagina voor het voorbeeld in dit artikel is *http://lamp-vm.cloudapp.net/phpinfo.php*.

## <a name="additional-resources"></a>Aanvullende bronnen

De eenvoudige stappen kunt u complexere toepassingen implementeren. In dit voorbeeld is het script voor installatie opgeslagen als een openbare blob in Azure opslag. Een veiligere optie zou zijn voor het opslaan van het script voor installatie als een blob beveiligd met een [Handtekening voor beveiligde toegang](https://msdn.microsoft.com/library/azure/ee395415.aspx) (SAS).

Aanvullende bronnen voor Azure CLI, Linux en de extensie CustomScript worden vervolgens weergegeven.

[Aanpassingstaken Linux VM met de extensie CustomScript automatiseren](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)

[Azure Linux-extensies (GitHub)](https://github.com/Azure/azure-linux-extensions)

[Linux en Open-Source Computing op Azure](virtual-machines-linux-opensource-links.md)
