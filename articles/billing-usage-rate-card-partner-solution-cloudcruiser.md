<properties
   pageTitle="Cruiser en Microsoft Azure facturering API integratie cloud | Microsoft Azure"
   description="Biedt een uniek perspectief van Microsoft Azure facturering partner Cloud-Cruiser, op hun ervaringen de Azure facturering-API's integreren in hun product.  Dit is vooral handig voor Azure en de Cloud Cruiser klanten die zijn geïnteresseerd in het gebruik van/het wolk Cruiser voor Microsoft Azure Pack."
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"
   />

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="09/08/2016"
   ms.author="mobandyo;sirishap;bryanla"/>

# <a name="cloud-cruiser-and-microsoft-azure-billing-api-integration"></a>Cloud Cruiser en Microsoft Azure facturering API-integratie

In dit artikel wordt beschreven hoe de gegevens van de nieuwe Microsoft Azure facturering API's kan worden gebruikt in de Cloud Cruiser voor workflow kosten simulatie en analyse.

## <a name="azure-ratecard-api"></a>Azure RateCard API
Gegevens over biedt de API RateCard van Azure. Na verificatie van met de juiste referenties opgeeft, kunt u een query de API voor het verzamelen van metagegevens over de beschikbare services op Azure, samen met de tarieven die zijn gekoppeld aan uw aanbieding.

Het volgende is een voorbeeld-reactie van de API met de prijzen voor de A0 exemplaar (Windows):

    {
        "MeterId": "0e59ad56-03e5-4c3d-90d4-6670874d7e29",
        "MeterName": "Compute Hours",
        "MeterCategory": "Virtual Machines",
        "MeterSubCategory": "A0 VM (Windows)",
        "Unit": "Hours",
        "MeterRates":
        {
            "0": 0.029
        },
        "EffectiveDate": "2014-08-01T00:00:00Z",
        "IncludedQuantity": 0.0,
        "MeterStatus": "Active"
    },

### <a name="cloud-cruisers-interface-to-azure-ratecard-api"></a>Cruiser van Azure RateCard API-Interface van de wolk
Cloud-Cruiser kunt gebruikmaken van de API RateCard informatie op verschillende manieren. Voor dit artikel, zullen we zien hoe deze kan worden gebruikt om IaaS werkbelasting simulatie en analyse.

Voorbeelden van deze use-case, stel een werklast van meerdere exemplaren die worden uitgevoerd op Microsoft Azure Pack (WAP). Het doel is om de werklast op Azure dezelfde simuleren en schatting van de kosten van de migratie van dergelijke. Wilt maken van deze simulatie, zijn er twee belangrijke taken uitvoeren:

1. **Importeren en verwerken van de API RateCard verzamelde gegevens van de service.** Deze taak wordt ook uitgevoerd op de werkmappen, waarbij het uittreksel uit de API RateCard getransformeerd en gepubliceerd naar een nieuw plan voor de snelheid. Het nieuwe tarief plan wordt gebruikt in de simulaties te schatten de Azure prijzen.

2. **WAP-services en Azure services voor IaaS normaliseren.** Standaard WAP-services zijn gebaseerd op de afzonderlijke resources (CPU, geheugen, schijf, enz.) tijdens het Azure services zijn gebaseerd op grootte instantie (A0, A1, A2, enz.). Deze eerste taak kunt uitgevoerd door de wolk Cruiser ETL-engine, genaamd werkmappen, waar deze bronnen kunnen worden gebundeld op de instantie grootte, analoog aan services Azure-instantie.

### <a name="import-data-from-the-ratecard-api"></a>Gegevens importeren uit de RateCard-API

Wolk Cruiser werkmappen bieden een geautomatiseerde manier verzamelen en verwerken van gegevens van de API RateCard.  ETL (extract-transformatie-load) werkmappen kunnen u de collectie, transformatie en publiceren van de gegevens in de Cloud Cruiser-database configureren.

Elke werkmap kan een of meer collecties, zodat u gegevens uit verschillende bronnen naar een aanvulling of uitbreiding van de gebruiksgegevens correleren hebben. De volgende twee screenshots laten zien hoe een nieuwe *collectie* maken in een bestaande werkmap en importeren van gegevens in de *collectie* van de API RateCard:

![Figuur 1 - een nieuwe collectie maken][1]

![Figuur 2 - gegevens importeren uit de nieuwe collectie][2]

Na het importeren van gegevens in de werkmap, is het mogelijk te maken van meerdere stappen en processen voor transformatie, te wijzigen en het model van de gegevens. In dit voorbeeld omdat we alleen geïnteresseerd in infrastructuur-as-a-Service (IaaS), we kunnen de transformatie stappen gebruiken om overbodige rijen te verwijderen of de records gerelateerd aan services dan IaaS.

Het volgende screenshot toont de transformatie stappen gebruikt voor het verwerken van de gegevens die worden verzameld uit RateCard API:

![Figuur 3 - stappen voor het verwerken van verzamelde gegevens van de API RateCard transformatie][3]

### <a name="defining-new-services-and-rate-plans"></a>Definiëren van nieuwe diensten en tarief plannen

Er zijn verschillende manieren Cruiser Cloud services definiëren. Een van de opties is de diensten van de gebruiksgegevens importeren. Deze methode wordt meestal gebruikt bij het werken met openbare wolken, waar al de services worden gedefinieerd door de provider.

Een tarief van Plan is een verzameling tarieven of prijzen die kunnen worden toegepast op andere diensten, op basis van begindatums of groep klanten uit andere opties. Tarieven kunnen ook worden gebruikt op een wolk Cruiser simulatie of 'Wat als'-scenario's om te begrijpen welke invloed wijzigingen in services kunnen op de totale kosten van een werkbelasting te maken.

In dit voorbeeld gebruiken we de service-informatie van de API RateCard voor het definiëren van nieuwe services in de Cloud Cruiser. Op dezelfde manier kunt we de tarieven die zijn gekoppeld aan de services maken een nieuw tarief plannen op wolk Cruiser.

Aan het einde van het proces van transformatie is het mogelijk om een nieuwe stap maken en publiceren van de gegevens van de API RateCard als nieuwe services en tarieven.

![Figuur 4 - de gegevens van de API RateCard publiceren als nieuwe Services en tarieven][4]

### <a name="verify-azure-services-and-rates"></a>Controleer of Azure Services en tarieven

Na het publiceren van services en tarieven, kunt u de lijst met geïmporteerde services in het tabblad van de Cruiser Cloud- *Services* controleren:

![Figuur 5 - de nieuwe Services controleren][5]

U kunt het nieuwe tarief plan genaamd "AzureSimulation" met de tarieven van de API RateCard geïmporteerd controleren op het tabblad *Tarief plannen* .

![Afbeelding 6 - het nieuwe tarief plannen en de bijbehorende tarieven controleren][6]

### <a name="normalize-wap-and-azure-services"></a>Normaliseren van de WAP en Azure Services

WAP, biedt informatie over het gebruik op basis van het gebruik van de compute-, geheugen- en netwerkbronnen. In de Cloud Cruiser, definieert u de services die zijn gebaseerd op de toewijzing of gemeten gebruik van deze bronnen. U kunt bijvoorbeeld een basistarief instellen voor elk uur van de CPU-gebruik, of de GB geheugen toegewezen aan een instantie in rekening brengen.

In dit voorbeeld om het vergelijken van kosten tussen de WAP en Azure, moeten we het gebruik van bronnen op het draadloze Toegangspunt in bundels, die vervolgens kunnen worden toegewezen aan Azure services statistische. Deze transformatie kan gemakkelijk worden geïmplementeerd in de werkmappen:

![Figuur 7 - omzetten van WAP-gegevens voor het normaliseren van services][7]

De laatste stap in de werkmap is de gegevens in de Cloud Cruiser database publiceren. Tijdens deze stap wordt de gebruiksgegevens nu gebundeld in services (die zijn toegewezen aan de Azure Services) en is gebonden aan de standaardtarieven voor het maken van de kosten.

Nadat de werkmap is voltooid, kunt u de verwerking van de gegevens, automatiseren door een taak toe te voegen op de scheduler en geven de frequentie en het tijdstip voor de werkmap uit te voeren.

![Figuur 8 - werkmap plannen][8]

### <a name="create-reports-for-workload-cost-simulation-analysis"></a>Rapporten maken voor analyse van werkbelasting kosten simulatie

Nadat het gebruik worden verzameld en toeslagen in de Cloud Cruiser-database zijn geladen, kunnen we gebruikmaken van de module wolk Cruiser inzichten om de werkbelasting kosten simulatie die we wensen te maken.

Om aan te tonen in dit scenario, we hebben het volgende rapport gemaakt:

![Kostenvergelijking][9]

Het bovenste diagram bevat een kostenvergelijking door services vergelijken de prijs van de werklast voor elke specifieke service tussen WAP (Donkerblauw) en Azure (Lichtblauw) wordt uitgevoerd.

De onderste grafiek ziet u dezelfde gegevens maar uitgesplitst per afdeling. Hierdoor worden de kosten voor elke afdeling uit te voeren hun werkbelasting op WAP en Azure, samen met het verschil tussen de besparingen balk (groen).

## <a name="azure-usage-api"></a>Azure gebruik API


### <a name="introduction"></a>Inleiding

Microsoft introduceerde onlangs de Azure gebruik API, waardoor abonnees via programmacode ophalen van gegevens te verkrijgen van inzicht in hun verbruik. Dit is goed nieuws voor Cloud Cruiser klanten van de rijkere dataset beschikbaar via deze API profiteren kunnen.

Cloud-Cruiser kunt profiteren van de integratie met de API voor gebruik op verschillende manieren. De granulatie (elk uur informatie over het gebruik) en metagegevens informatie over de resources beschikbaar via de API biedt de dataset nodig ter ondersteuning van flexibele Showback of terugboeking modellen. 

In deze zelfstudie vertonen wij een voorbeeld van hoe Cloud Cruiser van het gebruik van API-informatie profiteren kunt. We zullen meer in het bijzonder een resourcegroep maken op Azure, koppel de labels voor de rekeningstructuur en beschrijf het proces van het ophalen en verwerken van de tag-informatie in de Cloud Cruiser.
 
Het uiteindelijke doel is te kunnen maken van rapporten zoals de volgende en kunnen analyseren van kosten en verbruik op basis van de rekeningstructuur gevuld met de codes.

![Figuur 10 - rapport met de uitsplitsingen met tags][10]

### <a name="microsoft-azure-tags"></a>Microsoft Azure Tags

De gegevens die beschikbaar zijn via de API voor het gebruik van Azure bevat niet alleen informatie over het energieverbruik, maar ook bron-metagegevens met inbegrip van eventuele labels gekoppeld. Tags een eenvoudige manier voor het ordenen van uw resources, maar om doeltreffend te bieden, moet u ervoor zorgen dat:

- Labels worden correct toegepast op de resources op moment van levering
- Tags worden goed gebruikt in het proces Showback/terugboeking te koppelen van het gebruik van de rekeningstructuur van de organisatie.

Beide van deze vereisten kunnen lastig zijn, vooral wanneer er is een handmatig proces op de levering of opladen van kant. Typefout, verkeerd of zelfs ontbrekende codes zijn veelvoorkomende klachten van klanten als met labels en deze fouten kan leven aan de geladen erg moeilijk maken.

Met de nieuwe syntaxis-API Azure Cloud Cruiser pull-tagging resourcegegevens, en via een geavanceerde ETL tool werkmappen genoemd, deze gemeenschappelijke tagging fouten zijn opgelost. Via een transformatie met behulp van reguliere expressies en gegevens correlatie, Cloud Cruiser verkeerd gelabelde resources identificeren en de juiste codes toegepast ervoor te zorgen dat de juiste koppeling van de middelen met de consument.

Aan de geladen Cruiser wolk automatiseert het proces Showback/terugboeking en kunt profiteren van de tag-informatie voor het koppelen van het gebruik van de juiste gebruiker (afdeling, divisie, Project, enz.). Deze automatisering biedt een enorme verbetering en ervoor kan zorgen dat een consistente en controleerbare geladen proces.
 

### <a name="creating-a-resource-group-with-tags-on-microsoft-azure"></a>Een resourcegroep maken met Microsoft Azure-tags
De eerste stap in deze zelfstudie is een resourcegroep maken in de portal Azure, maak nieuwe codes om te koppelen aan de bronnen. In dit voorbeeld is de volgende codes maken: afdeling, milieu, eigenaar van Project.

Het screenshot hieronder toont een voorbeeld van een resourcegroep met de bijbehorende tags.

![Figuur 11 - resourcegroep met bijbehorende labels op Azure portal][11]

De volgende stap is het ophalen van de informatie van de API voor gebruik in de Cloud Cruiser. Het gebruik van API biedt momenteel antwoorden in JSON-indeling. Hier volgt een voorbeeld van de gegevens die worden opgehaald:


    {
      "id": "/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXX/providers/Microsoft.Commerce/UsageAggregates/Daily_BRSDT_20150623_0000",
      "name": "Daily_BRSDT_20150623_0000",
      "type": "Microsoft.Commerce/UsageAggregate",
      "properties":
      {
        "subscriptionId": "bb678b04-0e48-4b44-XXXX-XXXXXXXXX",
        "usageStartTime": "2015-06-22T00:00:00+00:00",
        "usageEndTime": "2015-06-23T00:00:00+00:00",
        "meterName": "Compute Hours",
        "meterRegion": "",
        "meterCategory": "Virtual Machines",
        "meterSubCategory": "Standard_D1 VM (Non-Windows)",
        "unit": "Hours",
        "instanceData": "{\"Microsoft.Resources\":{\"resourceUri\":\"/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXXX/resourceGroups/DEMOUSAGEAPI/providers/Microsoft.Compute/virtualMachines/MyDockerVM\",\"location\":\"eastus\",\"tags\":{\"Department\":\"Sales\",\"Project\":\"Demo Usage API\",\"Environment\":\"Test\",\"Owner\":\"RSE\"},\"additionalInfo\":{\"ImageType\":\"Canonical\",\"ServiceType\":\"Standard_D1\"}}}",
        "meterId": "e60caf26-9ba0-413d-a422-6141f58081d6",
        "infoFields": {},
        "quantity": 8

      },
    },


### <a name="import-data-from-the-usage-api-into-cloud-cruiser"></a>Gegevens importeren vanuit de API voor gebruik in Cloud-Cruiser

Wolk Cruiser werkmappen bieden een geautomatiseerde manier verzamelen en verwerken van gegevens van de API-gebruik. Een werkmap ETL (extract-transformatie-load) kunt u de collectie, transformatie en publiceren van de gegevens in de Cloud Cruiser-database configureren.

Elke werkmap kan een of meerdere collecties hebben. Hiermee kunt u gegevens uit verschillende bronnen naar een aanvulling of uitbreiding van de gebruiksgegevens correleren. In dit voorbeeld wordt maken we een nieuw blad in de werkmap Azure-sjabloon (_UsageAPI)_ en stelt u een nieuwe _collectie_ voor het importeren van informatie van de API-gebruik.

![Figuur 3 - API gebruiksgegevens importeren in het blad UsageAPI][12]

U ziet dat deze werkmap al andere bladen services van Azure (_ImportServices_) importeren en verwerken van de informatie over het energieverbruik van de API voor facturering (_PublishData_).

Volgende we het gebruik van API gebruikt voor het vullen van het blad _UsageAPI_ en de informatie met de gegevens over het verbruik van de API voor facturering op het blad _PublishData_ correleren.

### <a name="processing-the-tag-information-from-the-usage-api"></a>Verwerking van de tag-informatie van de API-gebruik

We gaan na het importeren van de gegevens in de werkmap transformatie stappen maken in het blad _UsageAPI_ verwerking van de gegevens van de API. Eerste stap is het uitpakken van de labels van een veld en vervolgens velden maken voor elk van hen (afdeling, Project, eigenaar en omgeving) met een processor 'JSON splitsen'.

![Figuur 4 - nieuwe velden maken voor de tag-informatie][13]

Aankondiging van de "Netwerk"-service ontbreekt in de tag-informatie (geel vak), maar we kunnen controleren of deel uitmaken van dezelfde bronnengroep is door te kijken in het veld _ResourceGroupName_ . Aangezien we de codes voor de andere resources van de resourcegroep hebt, kunnen we gebruiken deze informatie toe te passen de labels ontbreken op deze bron later in het proces.

De volgende stap is het maken van een opzoektabel koppelt de informatie uit de codes aan de _ResourceGroupName_. Deze opzoektabel wordt gebruikt op de volgende stap te verrijken het verbruiksgegevens tag informatie.

### <a name="adding-the-tag-information-to-the-consumption-data"></a>De tag-informatie toe te voegen aan de gegevens over het verbruik

We kunnen nu springen naar het blad _PublishData_ , die de informatie over het energieverbruik van de facturering-API worden verwerkt, en de velden opgehaald uit de tags toevoegen. Dit proces wordt uitgevoerd door te kijken in de opzoektabel die is gemaakt in de vorige stap, met behulp van de _ResourceGroupName_ als de sleutel voor zoekopdrachten.

![Figuur 5 - vullen met de rekeningstructuur met de informatie van de zoekopdrachten][14]

Merk op dat de juiste account structuurvelden voor de service "Netwerk" zijn toegepast, tot vaststelling van het probleem met de labels ontbreken. We ook de structuurvelden voor resources dan onze doel resourcegroep met "Andere" gevuld om ze te onderscheiden van de verslagen.

Nu moeten we alleen toe te voegen, een stap voor het publiceren van de gegevens over het gebruik. Tijdens deze stap wordt de juiste tarieven voor elke service die is gedefinieerd op onze tarief plannen worden toegepast op de informatie over het gebruik, met de resulterende kosten in de database laadt.

Het mooiste is dat u slechts eenmaal dit proces doorlopen. Wanneer de werkmap is voltooid, hoeft u alleen maar toe te voegen aan de scheduler en deze per uur of dagelijks op het geplande tijdstip wordt uitgevoerd. Dan is het gewoon een kwestie van nieuwe rapporten maken of bestaande bestanden aanpassen om het analyseren van de gegevens uit uw gebruik van de cloud om zinvolle inzichten.

### <a name="next-steps"></a>Volgende stappen

+ Raadpleeg voor gedetailleerde instructies over het maken van werkmappen wolk Cruiser en rapporten wolk Cruiser van on line [documentatie](http://docs.cloudcruiser.com/) (geldig aanmelden vereist).  Voor meer informatie over Cloud Cruiser, neem contact op met [info@cloudcruiser.com](mailto:info@cloudcruiser.com).
+ Zie [inzicht in uw verbruik van Microsoft Azure krijgen](billing-usage-rate-card-overview.md) voor een overzicht van het Resourcegebruik Azure en RateCard APIs.
+ Bekijk de [Azure facturering REST API Reference](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) voor meer informatie over beide API's die deel van de verzameling API's die door de Azure Resource Manager uitmaken.
+ Bekijk onze Microsoft Azure facturering API-codevoorbeelden op [Azure codevoorbeelden](https://azure.microsoft.com/documentation/samples/?term=billing)in als u wilt direct Duik in de voorbeeldcode.

### <a name="learn-more"></a>Meer informatie
+ Zie het artikel [Azure Resource Manager-overzicht](azure-resource-manager/resource-group-overview.md) voor meer informatie over de Azure Resource Manager.

<!--Image references-->
 
[1]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Create-New-Workbook-Collection.png "Figuur 1 - een nieuwe collectie maken"
[2]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Import-Data-From-RateCard.png "Figuur 2 - gegevens importeren uit de nieuwe collectie"
[3]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transformation-Steps-Process-RateCard-Data.png "Figuur 3 - stappen voor het verwerken van verzamelde gegevens van de API RateCard transformatie"
[4]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Publish-RateCard-Data-New-Services-Rates.png "Figuur 4 - de gegevens van de API RateCard publiceren als nieuwe Services en tarieven"
[5]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing1.png "Figuur 5 - de nieuwe Services controleren"
[6]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing2.png "Afbeelding 6 - het nieuwe tarief plannen en de bijbehorende tarieven controleren"
[7]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transforming-WAP-Normalize-Services.png "Figuur 7 - omzetten van WAP-gegevens voor het normaliseren van services"
[8]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workbook-Scheduling.png "Figuur 8 - werkmap plannen"
[9]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workload-Cost-Simulation-Report.png "Figuur 9 - voorbeeldrapport voor het scenario werkbelasting kosten vergelijking"
[10]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/1_ReportWithTags.png "Figuur 10 - rapport met de uitsplitsingen met tags"
[11]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/2_ResourceGroupsWithTags.png "Figuur 11 - resourcegroep met bijbehorende labels op Azure portal"
[12]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/3_ImportIntoUsageAPISheet.png "Figuur 12 - API gebruiksgegevens importeren in het blad UsageAPI"
[13]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/4_NewTagField.png "Figuur 13 - nieuwe velden maken voor de tag-informatie"
[14]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/5_PopulateAccountStructure.png "Figuur 14 - vullen met de rekeningstructuur met de informatie van de zoekopdrachten"
