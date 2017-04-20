<properties
   pageTitle="Handleiding voor het maken van een sjabloon oplossing voor Marketplace | Microsoft Azure"
   description="Gedetailleerde instructies over het maken, certificeren en implementeren van een Multi-VM afbeelding oplossing sjabloon voor kopen op de markt Azure."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="07/27/2016"
      ms.author="hascipio; v-divte" />

# <a name="guide-to-create-a-solution-template-for-azure-marketplace"></a>Handleiding voor het maken van een sjabloon oplossing voor Azure Marketplace
Na het voltooien van stap 1, [het maken van accounts en registratie][link-acct-creation], wij u over het maken van een sjabloon Azure-compatibele oplossing aan de [technische vereisten voor het maken van een sjabloon oplossing](marketplace-publishing-solution-template-creation-prerequisites.md)geleid. Nu we u door de stappen begeleidt voor het maken van een sjabloon oplossing voor meerdere VMs op de [Portal voor publiceren] [ link-pubportal] voor Azure Marketplace.

## <a name="create-your-solution-template-offer-in-the-publishing-portal"></a>Uw voorstel oplossing sjabloon maken in de Portal voor publiceren
Ga naar [https://publish.windowsazure.com](http://publish.windowsazure.com). Als u zich aanmeldt voor de eerste keer aan de [Portal voor publiceren](https://publish.windowsazure.com/), gebruik de account waaraan uw bedrijfsprofiel verkoper is geregistreerd. U kunt later een medewerker van uw bedrijf toevoegen als een co-beheerder in de Portal voor publiceren.

### <a name="1-select-solution-templates"></a>1. Selecteer 'Sjablonen oplossing'

  ![tekening][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a>2. Maak een nieuwe sjabloon van de oplossing

  ![tekening][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a>3. start met topologieën
Een sjabloon oplossing is een "bovenliggende" op al haar topologieën. Topologieën met meerdere kunt u definiëren in een sjabloon voor aanbod/oplossing. Wanneer een voorstel tot tijdelijke geduwd, is het met alle bijbehorende topologieën geduwd. Volg de stappen hieronder om uw voorstel te definiëren:     

- Een topologie maken: "ID" is meestal de naam van de topologie van de sjabloon voor de oplossing. De id wordt gebruikt in de URL, zoals hieronder wordt weergegeven:

  Azure Marketplace: http://azure.microsoft.com/marketplace/partners/ {PublisherNamespace} / {OfferIdentifier} {TopologyIdentifier}

  Azure Portal: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {TopologyIdentifier}

- Voeg een nieuwe versie.

### <a name="4-get-your-topology-versions-certified"></a>4. Zorg dat uw topologie versies gecertificeerd
Upload een zip-bestand met alle vereiste bestanden om te creëren die bepaalde versie van de topologie. Dit zip-bestand moet het volgende bevatten:

- *mainTemplate.json* en *createUiDefinition.json* -bestand in de hoofdmap.
- De gekoppelde sjablonen en alle vereiste scripts.

  > [AZURE.TIP] Terwijl de ontwikkelaars werken aan de oplossing topologieën sjabloon maken en hen gecertificeerd, het bedrijf, kunnen op de marketing- en juridische inhoud marketing-en/of juridische afdelingen van uw bedrijf werken.

## <a name="next-steps"></a>Volgende stappen
Nu dat u de sjabloon van uw oplossing hebt gemaakt en het zip-bestand wordt geüpload, volgt u de instructies in de [handleiding Marketplace marketing inhoud](marketplace-publishing-push-to-staging.md) voordat het duwen van de tijdelijke aanbieding. De volledige verzameling artikelen publiceren marketplace vindt u op [aan de slag: het publiceren van een aanbod op de markt Azure](marketplace-publishing-getting-started.md).

Ook worden geïnteresseerd in deze artikelen:

- VM-afbeeldingen: [Over Virtual Machine afbeeldingen in Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)
- VM-extensies: [VM Agent en VM extensies overzicht](https://msdn.microsoft.com/library/azure/dn832621.aspx) en [Azure VM uitbreidingen en functies](https://msdn.microsoft.com/library/azure/dn606311.aspx)
- Azure Resource Manager: [Azure ARM sjablonen ontwerpen](../resource-group-authoring-templates.md) en [voorbeelden van eenvoudige ARM sjabloon](https://github.com/rjmax/ArmExamples)
- Opslag account bandbreedte: [het beeldscherm voor opslag Account beperken](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) en [Premium-opslag](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage)

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com
