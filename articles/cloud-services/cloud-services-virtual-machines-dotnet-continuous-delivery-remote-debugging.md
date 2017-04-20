<properties
    pageTitle="Foutopsporing op afstand met continue levering inschakelen | Microsoft Azure"
    description="Meer informatie over hoe u foutopsporing op afstand inschakelen bij het gebruik van continue levering naar Azure implementeren"
    services="cloud-services"
    documentationCenter=".net"
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="enable-remote-debugging-when-using-continuous-delivery-to-publish-to-azure"></a>Foutopsporing op afstand inschakelen bij het gebruik van continue levering publiceren naar Azure

U kunt foutopsporing op afstand inschakelen in Azure, cloud services of virtuele machines, wanneer u [continue levering](cloud-services-dotnet-continuous-delivery.md) op Azure publiceren door de volgende stappen.

## <a name="enabling-remote-debugging-for-cloud-services"></a>Externe foutopsporing voor cloud services inschakelen

1. Op de build-agent ingesteld de aanvankelijke omgeving voor Azure zoals wordt beschreven in [Opdrachtregelopties voor Azure opbouwen](http://msdn.microsoft.com/library/hh535755.aspx).
2. Omdat de externe foutopsporing runtime (msvsmon.exe) vereist voor het pakket is, de [Externe hulpprogramma's voor Visual Studio 2015](http://www.microsoft.com/en-us/download/details.aspx?id=48155) (of de [Externe hulpprogramma's voor Microsoft Visual Studio 2013 Update 5](https://www.microsoft.com/en-us/download/details.aspx?id=48156) als u Visual Studio 2013) installeren. Als alternatief kunt u de binaire bestanden voor externe foutopsporing uit een systeem waarop Visual Studio is geïnstalleerd.
3. Een certificaat maken zoals wordt beschreven in [Certificaten-overzicht voor Azure Cloud Services](cloud-services-certs-create.md). Houd .pfx en RDP vingerafdruk van het certificaat en het certificaat uploaden naar de cloud doelservice.
4. Gebruik de volgende opties op de opdrachtregel MSBuild te verpakken met externe foutopsporing is ingeschakeld. (Vervangen door de werkelijke paden naar de bestanden van uw systeem- en voor de artikelen van de hoek tussen haakjes).

        msbuild /TARGET:PUBLISH /PROPERTY:Configuration=Debug;EnableRemoteDebugger=true;VSX64RemoteDebuggerPath="<remote tools path>";RemoteDebuggerConnectorCertificateThumbprint="<thumbprint of the certificate added to the cloud service>";RemoteDebuggerConnectorVersion="2.7" "<path to your VS solution file>"

    `VSX64RemoteDebuggerPath`is het pad naar de map msvsmon.exe in de externe hulpprogramma's die voor Visual Studio.
    `RemoteDebuggerConnectorVersion`is de versie van Azure SDK in de cloud-service. Het moet ook overeenkomen met de versie met Visual Studio is geïnstalleerd.

5. Publiceren naar het doel cloud-service met behulp van het pakket en .cscfg-bestand gegenereerd in de vorige stap.
6. Importeer het certificaat (.pfx-bestand) met de computer waarop Visual Studio met Azure SDK voor .NET is geïnstalleerd. Zorg ervoor dat aan de `CurrentUser\My` certificaatarchief, anders koppelen aan de foutopsporing in Visual Studio zal mislukken.

## <a name="enabling-remote-debugging-for-virtual-machines"></a>Externe foutopsporing voor virtuele machines inschakelen

1. Een Azure virtuele machine maken. Zie [een virtuele Machine met Windows Server maakt](../virtual-machines/virtual-machines-windows-hero-tutorial.md) of [maken en beheren van Azure Virtual Machines in Visual Studio](../virtual-machines/virtual-machines-windows-classic-manage-visual-studio.md).
2. Weergeven op de [klassieke portaalpagina Azure](http://go.microsoft.com/fwlink/p/?LinkID=269851)dashboard overzicht **RDP CERTIFICAATVINGERAFDRUK**van de virtuele machine de virtuele machine. Deze waarde wordt gebruikt voor de `ServerThumbprint` waarde in de configuratie van de uitbreiding.
3. Een clientcertificaat maken zoals wordt beschreven in [Certificaten-overzicht voor Azure Cloud Services](cloud-services-certs-create.md) (.pfx en RDP certificaatvingerafdruk houden).
4. Azure Powershell installeren (versie 0.7.4 of hoger) zoals beschreven in [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).
5. Voer het volgende script om de RemoteDebug-extensie inschakelen. Vervangen door de paden en de persoonlijke gegevens, zoals de naam van abonnement, naam en vingerafdruk zelf.

    >[AZURE.NOTE] Dit script is geconfigureerd voor Visual Studio 2015. Als u Visual Studio 2013 gebruikt, wijzigt u de `$referenceName` en `$extensionName` toewijzingen te gebruiken hieronder `RemoteDebugVS2013` (in plaats van `RemoteDebugVS2015`).

    <pre>
   Toevoegen-AzureAccount

    "Mijn abonnement op Microsoft" SELECT-AzureSubscription

    $vm = "mytestvm1" get-AzureVM - servicenaam-naam "mytestvm1"

    $endpoints = @( ,@{Name="RDConnVS2013"; PublicPort = 30400; PrivatePort = 30398} ,@{Name="RDFwdrVS2013"; PublicPort = 31400; PrivatePort = 31398})  

    foreach ($endpoint in $endpoints) {$vm Add-AzureEndpoint - VM-naam $endpoint. Name - Protocol tcp - PublicPort $endpoint. PublicPort - LokalePoort $endpoint. PrivatePort}

    $referenceName = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug.RemoteDebugVS2015" $publisher = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug" $extensionName = "RemoteDebugVS2015" $version = "1.*" $publicConfiguration = "<PublicConfig>< Connector.Enabled > < /Connector.Enabled > waar<ClientThumbprint>56D7D1B25B472268E332F7FC0C87286458BFB6B2</ClientThumbprint><ServerThumbprint>E7DCB00CB916C468CC3228261D6E4EE45C8ED3C6</ServerThumbprint><ConnectorPort>30398</ConnectorPort><ForwarderPort>31398</ForwarderPort></PublicConfig>"

    $vm | Set AzureVMExtension `
    -ReferenceName $referenceName ` 
    -uitgever $publisher `
    -ExtensionName $extensionName ` 
    -versie $version "- PublicConfiguration $publicConfiguration

    foreach ($extension in $vm. VM. ResourceExtensionReferences) {als (($extension. Eq - verwijzing $referenceName) `
    -and ($extension.Publisher -eq $publisher) ` 
    - en ($extension. Naam eq - $extensionName) "- en ($extension. Versie - eq $version)) {$extension. ResourceExtensionParameterValues [0]. Toets = einde "config.txt"}}

    $vm | Update AzureVM </pre>

6. Importeer het certificaat (.pfx) met de computer waarop Visual Studio met Azure SDK voor .NET is geïnstalleerd.
