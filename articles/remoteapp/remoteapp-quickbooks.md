<properties 
    pageTitle="QuickBooks in Azure RemoteApp implementeren | Microsoft Azure" 
    description="Informatie over het delen van QuickBooks met Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-do-you-deploy-quickbooks-in-azure-remoteapp"></a>Hoe u QuickBooks in Azure RemoteApp implementeren?

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Gebruik de volgende gegevens naar QuickBooks delen als een app in Azure RemoteApp.


U kunt QuickBooks 2015 onderneming delen met Azure RemoteApp in een hybride of een cloud-collectie. Het bedrijfsbestand moet zich bevinden op een VM met QuickBooks-databaseserver die losstaat van de Azure RemoteApp-servers. Nooit het bedrijfsbestand opslaat op uw afbeelding Azure RemoteApp - gegevens verloren gaan als u dit doet wordt verwacht. Alleen QuickBooks Enterprise ondersteunt de QuickBooks-bestand op een externe share met QuickBooks databaseserver toegankelijk via een standaard Windows-netwerken die als host fungeert.   

> [AZURE.IMPORTANT] De QuickBooks-databaseserver die als host voor het bedrijfsbestand fungeert moet zich bevinden op een aparte VM binnen de dezelfde VNET als de Azure RemoteApp-collectie.  

## <a name="steps-to-deploy-quickbooks"></a>Stappen voor het implementeren van QuickBooks

1. Een Azure VM maken en QuickBooks, QuickBooks-databaseserver te installeren en het bestand van het bedrijf op een Azure VM.  Controleer of firewall-regels correct te configureren.
2. QuickBooks installeren op een [aangepaste afbeelding](remoteapp-imageoptions.md) en maak een [collectie RemoteApp Azure](remoteapp-collections.md), cloud of hybride binnen de exacte dezelfde VNET waar de VM de QuickBooks-databaseserver hosten met bedrijfsbestanden zich bevindt. 
3.  [Publiceren](remoteapp-publish.md) QuickBooks-app voor gebruikers
4.  Start de client QuickBooks Azure RemoteApp gehost, navigeren met behulp van de standaard Windows-netwerk voor VM die als host fungeert voor de databaseserver QuickBooks en open het bedrijfsbestand. 

## <a name="documentation-references"></a>Documentatie bij verwijzingen

- QuickBooks [ondersteunde configuraties](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)
- QuickBooks- [implementatie-opties](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)

U kunt ook uitchecken mijn Ignite presentatie, [Fundamentals van Microsoft Azure RemoteApp-beheer en administratie](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) - vooruit om naar het QuickBooks-deel 1:02:45.

## <a name="deployment-architecture"></a>Implementatie-architectuur

![QuickBooks + Azure RemoteApp-implementatie](./media/remoteapp-quickbooks/ra-quickbooks.png)