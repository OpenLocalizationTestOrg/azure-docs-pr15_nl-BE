<properties
   pageTitle="Implementatie van toepassingen met de extensies van de virtuele Machine automatiseren | Microsoft Azure"
   description="Azure Virtual Machine DotNet Core zelfstudie"
   services="virtual-machines-windows"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="nepeters"/>

# <a name="application-deployment-with-azure-resource-manager-templates"></a>Implementatie van toepassingen met Azure Resource Manager-sjablonen

Zodra alle Azure infrastructurele eisen zijn geïdentificeerd en vertaald in een sjabloon voor de implementatie, de werkelijke toepassingsimplementatie aandacht wordt besteed. Toepassingsimplementatie hier verwijst naar de feitelijke binaire waarden van webtoepassingen naar Azure bronnen te installeren. Voor de winkel monster .net Core en IIS moeten worden geïnstalleerd en geconfigureerd op elke virtuele machine. De binaire bestanden voor muziek winkel moeten worden geïnstalleerd op de virtuele machine en de muziek database vooraf gemaakt.

Dit document details over hoe extensies van de virtuele Machine kunnen automatiseren installatie van toepassingen en configuratie van Azure virtual machines. Alle afhankelijkheden en unieke configuraties zijn gemarkeerd. Voor de beste ervaring, een exemplaar van de oplossing voor uw abonnement Azure en werk samen met de sjabloon Azure Resource Manager vooraf te implementeren. De volledige sjabloon vindt u hier – [Muziek winkel Deployment in Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-Windows).

## <a name="configuration-script"></a>Configuratiescript

Virtual Machine-extensies zijn speciale programma's die ten opzichte van virtuele machines voor automatisering van de configuratie uitvoeren. Extensies zijn beschikbaar voor veel specifieke doeleinden, zoals antivirusprogramma's en configuratie voor logboekregistratie, Docker configuratie. De extensie aangepast Script kan worden gebruikt om een script uitvoeren voor een virtuele machine. Met het monster muziek winkel is tot aan de uitbreiding aangepast script voor het Windows virtuele machines configureren en installeer de toepassing muziek winkel.

Voordat u met informatie over hoe extensies van de virtuele machine worden gedefinieerd in een sjabloon Azure Resource Manager, onderzoekt het script dat wordt uitgevoerd. Dit script wordt geconfigureerd voor de virtuele Windows-computer als host voor de toepassing van de winkel. Wanneer uitvoert, het script worden alle geïnstalleerd nodig software, installeert u de toepassing van de winkel muziek van het besturingselement en de database voorbereiden. 

> In dit voorbeeld is bedoeld voor demonstratiedoeleinden.

```none
Param (
    [string]$user,
    [string]$password,
    [string]$sqlserver
)

# firewall
netsh advfirewall firewall add rule name="http" dir=in action=allow protocol=TCP localport=80

# folders
New-Item -ItemType Directory c:\temp
New-Item -ItemType Directory c:\music

# install iis
Install-WindowsFeature web-server -IncludeManagementTools

# install dot.net core sdk
Invoke-WebRequest http://go.microsoft.com/fwlink/?LinkID=615460 -outfile c:\temp\vc_redistx64.exe
Start-Process c:\temp\vc_redistx64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkID=809122 -outfile c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe
Start-Process c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkId=817246 -outfile c:\temp\DotNetCore.WindowsHosting.exe
Start-Process c:\temp\DotNetCore.WindowsHosting.exe -ArgumentList '/quiet' -Wait

# download / config music app
Invoke-WebRequest  https://github.com/Microsoft/dotnet-core-sample-templates/raw/master/dotnet-core-music-windows/music-app/music-store-azure-demo-pub.zip -OutFile c:\temp\musicstore.zip
Expand-Archive C:\temp\musicstore.zip c:\music
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replaceserver>", $sqlserver } | Set-Content C:\music\config.json
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replaceuser>", $user } | Set-Content C:\music\config.json
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replacepass>", $password } | Set-Content C:\music\config.json

# workaround for db creation bug
Start-Process 'C:\Program Files\dotnet\dotnet.exe' -ArgumentList 'c:\music\MusicStore.dll'

# configure iis
Remove-WebSite -Name "Default Web Site"
Set-ItemProperty IIS:\AppPools\DefaultAppPool\ managedRuntimeVersion ""
New-Website -Name "MusicStore" -Port 80 -PhysicalPath C:\music\ -ApplicationPool DefaultAppPool
& iisreset
```

## <a name="vm-script-extension"></a>VM Script extensie

VM-extensies kan worden uitgevoerd tegen een virtuele machine tegelijk maken door de bron van de uitbreiding in de sjabloon Azure Resource Manager. De uitbreiding kan worden toegevoegd met de wizard Resources toevoegen Visual Studio of door middel van geldige JSON in de sjabloon. De resource Script extensie is genest in de bron van de virtuele Machine; Deze kunnen worden gezien in het volgende voorbeeld.

Volg deze koppeling als de JSON voorbeeld binnen de sjabloon Resource Manager – [VM Script extensie](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L339)wilt zien. 

U ziet in de onderstaande JSON dat het script is opgeslagen in de GitHub. Dit script kan ook worden opgeslagen in Azure Blob-opslag. Azure Resource Manager-sjablonen kunnen ook zo gebouwd zijn dat de waarden van parameters kunnen worden gebruikt als parameters voor de uitvoering van het script de uitvoering van Scripttekenreeks. In dit geval gegevens die worden verstrekt bij de implementatie van de sjablonen en deze waarden kunnen vervolgens worden gebruikt bij het uitvoeren van het script.

```none
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
  }
}
```

Zie voor meer informatie over het gebruik van het aangepaste script extensie [scriptextensies aangepaste sjablonen Resource Manager](./virtual-machines-windows-extensions-customscript.md).

## <a name="next-step"></a>Volgende stap

<hr>

[Ontdek meer Azure Resource Manager-sjablonen](https://github.com/Azure/azure-quickstart-templates)
