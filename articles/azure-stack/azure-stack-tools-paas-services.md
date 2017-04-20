<properties
    pageTitle="Hulpprogramma's en PaaS services voor Azure Stack | Microsoft Azure"
    description="Informatie over het aan de slag met services in Azure Stack PaaS."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="tools-for-azure-stack"></a>Hulpprogramma's voor Azure Stack

De hulpprogramma's die hieronder worden beschreven, kunt u downloaden. Als u worden geïnformeerd over nieuwe services en hulpprogramma's wilt, volg #AzureStack op Twitter.

## <a name="template-tools"></a>Hulpprogramma's voor sjabloon

### <a name="azure-stack-github-templates"></a>Azure Stack Github sjablonen
Ontdek de groeiende collectie van [Azure Stack GitHub sjablonen](https://github.com/Azure/AzureStack-QuickStart-Templates). Net als [Azure](https://github.com/Azure/azure-quickstart-templates)beogen deze sjablonen Azure Resource Manager 'Snel starten' om u aan de slag met eenvoudige bouwstenen en voorbeelden implementeren op de Microsoft Azure Stack technische Preview bewijs van Concept omgeving. Wordt geleverd met voorbeelden van eerste partij werkbelasting voor [ad-niet-ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/ad-non-ha), [sql-2014-niet-ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sql-2014-non-ha), [sharepoint-2013-niet-ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sharepoint-2013-non-ha), als ook door de verschillende sjablonen voor eenvoudige 101 [101 eenvoudige-windows vm](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-simple-windows-vm).


### <a name="marketplace-item-packaging-tool-and-sample"></a>Marketplace-item verpakking en
[Download en gebruik het hulpprogramma verpakking](http://www.aka.ms/azurestackmarketplaceitem) marketplace items voor uw eigen aangepaste sjablonen toe te voegen aan de stapel Azure marketplace te maken. Instructies voor het marketplace items maken en deze beschikbaar te maken voor uw huurders vindt u in [artikel Marketplace maken](azure-stack-create-and-publish-marketplace-item.md).

## <a name="developer-tools"></a>Hulpprogramma's voor ontwikkelaars


### <a name="use-visual-studio-and-azure-stack-tp2-on-the-mas-con01-virtual-machine"></a>Visual Studio en Azure Stack TP2 op de virtuele machine van MAS CON01 gebruiken
Als u Visual Studio op de VM-console gebruiken om te werken met Azure Stack sjablonen wilt, moet u de juiste versie van de vereiste hulpprogramma's installeren. Gebruik de volgende procedure de ondersteunde versies voor TP2 te installeren.

1. Verbinding met extern bureaublad gebruiken voor aanmelding bij de MAS CON01 virtuele machine met de referenties van de azurestack\azurestackadmin.
2. Installeer en open Web Platform Installer.
3. Zoeken en installeren van **Visual Studio Community 2015 met Microsoft Azure SDK - 2.9.5**.
4. Met het **Onderdeel Software**, verwijdert **Microsoft Azure PowerShell (sep)** dat wordt geïnstalleerd als onderdeel van de SDK.
5. Open het Web Platform Installer.
6. Zoeken en installeren van de **Microsoft Azure PowerShell - Azure Stack technische Preview 2**. 
7. Start de virtuele machine van MAS CON01.
8. Verbinding met extern bureaublad gebruiken voor aanmelding bij de MAS CON01 virtuele machine met de referenties van de azurestack\azurestackadmin.
9. Visual Studio openen en controleren of u kunt verbinding maken met de stapel Azure-omgeving, sjablonen, enzovoort. 

### <a name="azure-powershell-sdk"></a>Azure PowerShell SDK
Azure PowerShell is een module waarmee u de cmdlets voor het beheren van Azure en Azure Stack met Windows PowerShell. U kunt de cmdlets gebruiken om te maken, testen, implementeren en beheren oplossingen en services die worden geleverd via het platform Azure Stack.
[Azure PowerShell SDK downloaden](http://aka.ms/azStackPsh) en [installeren PowerShell](azure-stack-connect-powershell.md).

### <a name="azure-cross-platform-command-line-interfaces"></a>Azure cross-platform opdracht regel interfaces
Snel de Azure opdrachtregelinterface (CLI Azure) om een reeks open source shell-opdrachten gebruiken voor het maken en beheren van bronnen in Microsoft Azure Stack installeren.

[De Windows-CLI downloaden](http://aka.ms/azstack-windows-cli)

[De Mac-CLI downloaden](http://aka.ms/azstack-linux-cli)

[De Linux-CLI downloaden](http://aka.ms/azstack-mac-cli)

>[AZURE.NOTE]
>
> + Als u op een Mac- of Linux-machine, ook krijgt u de CLI met behulp van de opdracht`npm install -g azure-cli@0.9.11`</br>
> + Als u een certificaat validatieproblemen krijgt, voert u de opdracht`set NODE_TLS_REJECT_UNAUTHORIZED=0`
