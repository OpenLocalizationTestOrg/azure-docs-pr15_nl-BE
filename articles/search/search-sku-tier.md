<properties
    pageTitle="Kies een SKU of prijzen laag voor Azure zoeken | Microsoft Azure"
    description="Azure Search kan worden ingericht op deze SKU's: vrij, Basic en Standard, waarbij standaard is beschikbaar in verschillende configuraties van resources en capaciteit niveaus."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="10/24/2016"
    ms.author="heidist"/>

# <a name="choose-a-sku-or-pricing-tier-for-azure-search"></a>Kies een SKU of prijzen laag voor Azure zoeken

In Azure zoeken een [service is ingericht](search-create-service-portal.md) op een specifieke prijzen laag of SKU. Beschikbare opties zijn **gratis**, **Basic**of **Standard**, waarbij **standaard** beschikbaar in meerdere configuraties en capaciteiten is. 

Het doel van dit artikel is bij het kiezen van een laag. Als de capaciteit van een bepaalde laag blijkt te laag, moet u een nieuwe service in de hogere laag inrichten en vervolgens opnieuw laden van de indexen. Er is geen in-place upgrade van dezelfde service uit één SKU naar de andere. 

> [AZURE.NOTE] Nadat u een laag en [een zoekservice bepaling](search-create-service-portal.md)kiest, kunt u replica verhogen en partitie geteld in de service. Zie [resource schaalniveaus voor query's en indexering werkbelasting](search-capacity-planning.md)voor instructies.

## <a name="how-to-approach-a-pricing-tier-decision"></a>Benadering van een prijsbesluit laag

De laag bepaalt in Azure Search, capaciteit, niet beschikbaarheid van functies. In het algemeen zijn functies beschikbaar op elke laag, met inbegrip van de functies van de voorbeeldweergave. De enige uitzondering hierop is geen ondersteuning voor indexeerfuncties in HD S3.

> [AZURE.TIP] Wij raden aan dat u altijd een **gratis** service (één per abonnement zonder vervaldatum) inrichten zodat zijn direct beschikbaar voor projecten met een licht gewicht. Gebruik de **gratis** service voor het testen en evaluatie; Maak een tweede factureerbare service in de **Basic** - of **Standard** -laag voor productie of grotere werklast van de test.

Capaciteit en de kosten van het uitvoeren van de service gaat hand in hand. De informatie in dit artikel kunt u bepalen welke SKU biedt de juiste verhouding, maar een van deze nuttig zijn, moet u ten minste ruwe schattingen van het volgende:

- Aantal en de grootte van de indexen die u wilt maken
- Aantal en de grootte van documenten te uploaden
- Een idee van query volume, in termen van query's Per tweede (QPS)

Aantal en de grootte zijn belangrijk omdat de maximale limiet wordt bereikt door middel van een vaste limiet op het aantal indexen of documenten in een service of bronnen (opslag of replica's) die worden gebruikt door de service. De werkelijke limiet voor uw service is afhankelijk van wat eerst wordt gebruikt: bronnen of objecten.

Met de ramingen in de hand, moeten de volgende stappen uit het proces vereenvoudigen:

- **Stap 1** Bekijk de SKU beschrijvingen hieronder voor meer informatie over de beschikbare opties.
- **Stap 2** Beantwoord de vragen hieronder om te komen tot een voorlopig besluit.
- **Stap 3** Uw beslissing voltooien door vaste limieten voor opslag te controleren en de prijzen.

## <a name="sku-descriptions"></a>SKU-beschrijvingen

De volgende tabel vindt u beschrijvingen van elke laag. 

Laag|Basisscenario 's
----|-----------------
**Gratis**|Een gedeelde service gratis gebruikt voor evaluatie, onderzoek of kleine workloads. Omdat deze wordt gedeeld met andere abonnees, query-doorvoer en indexering is afhankelijk van wie de service wordt gebruikt. Capaciteit is klein (50 MB of 3 indexen met van 10.000 documenten elk).
**Basic**|Kleine productie werkbelasting op specifieke hardware. Hoge beschikbaarheid. Capaciteit is 3 replica's en 1 partitie (2 GB).
**S1**|Standaard 1 ondersteunt flexibele combinaties van partities (12) en de replica's (12), die wordt gebruikt voor middelgrote productie werkbelasting op specifieke hardware. U kunt partities en replica's in de combinaties worden ondersteund door een maximum aantal 36 zoeken factureerbare eenheden toewijzen. Op dit niveau, zijn 25 GB voor partities en QPS is ongeveer 15 query's per seconde.
**S2**|Standaard 2 grotere productie werklast met dezelfde zoekopdracht 36 eenheden als S1 maar met grotere partities van grootte en replica's uitgevoerd. Op dit niveau, zijn 100 GB partities en QPS is ongeveer 60 query's per seconde.
**S3**|Standaard 3 uitgevoerd proportioneel grotere werklast voor productie op de hogere end-systemen in configuraties van maximaal 12 partities of 12 replica's zoeken onder 36 eenheden. Op dit niveau, partities zijn 200 GB en QPS is meer dan 60 query's per seconde. 
**S3 HD**|Standaard 3 hoge dichtheid is ontworpen voor een groot aantal kleinere indexen. U kunt maximaal 3 partities hebben van 200 GB. QPS is meer dan 60 query's per seconde. 

> [AZURE.NOTE] Maximumwaarden replica en de partitie worden gefactureerd als zoekopdracht eenheden (36 maximum per service unit), legt de effectieve ondergrens dan de maximale impliceert tegen de nominale waarde. Bijvoorbeeld voor het gebruik van het maximum van 12 replica's kan er maximaal 3 partities (12 * 3 = 36 eenheden). Minder op dezelfde manier om maximale partities gebruikt, replica's tot en met 3. Zie [resource schaalniveaus voor query's en indexering werklasten in Azure zoeken](search-capacity-planning.md) voor een grafiek op de toegestane combinaties.

## <a name="review-limits-per-tier"></a>Bekijk de limieten per laag

De volgende grafiek is een subset van de grenzen van de [Maxima in Azure Search Service](search-limits-quotas-capacity.md). Hiermee geeft u de factoren kunnen invloed hebben op een besluit van de SKU. U kunt deze grafiek verwijzen bij het controleren van de onderstaande vragen.

Resource|Gratis|Basic|S1|S2|S3 |S3 HD
---|---|---|---|----|---|----
Service Level Agreement (SLA)|Geen <sup>1</sup> |Ja |Ja  |Ja |Ja  |Ja 
Index-beperkingen|3|5|50|200|200|1000 <sup>2</sup>
Limieten van document|10.000 in totaal|1 miljoen per service|15 miljoen per partitie |60 miljoen per partitie|120 miljoen per partitie |1 miljoen per index
Maximale partities|N.V.T. |1 |12  |12 |12|3 <sup>2</sup>
De grootte van partitie|Totaal 50 MB|2 GB per service|25 GB per partitie |100 GB per partitie (tot maximaal 1,2 TB per service)|200 GB per partitie (tot een maximum van 2,4 TB per service)|200 GB (tot een maximum van 600 GB per service)
Maximale replica 's|N.V.T. |3 |12 |12 |12|12
Query's per seconde|N.V.T.|~ 3 per replica|~ 15 per replica|~ 60 per replica|> 60 per replica|> 60 per replica

<sup>1</sup> gratis en voorbeeld SKU's niet afkomstig zijn met SLA's. Sla's worden afgedwongen wanneer een SKU over het algemeen beschikbaar wordt.

<sup>2</sup> S3 en S3 HD worden ondersteund door identieke hoge capaciteit, infrastructuur, maar elk op verschillende manieren de maximale limiet is bereikt. S3 is bedoeld voor een klein aantal zeer grote indexen. Als zodanig, de bron afhankelijk is van de maximale limiet (2,4 TB voor elke service). S3 HD is bedoeld voor een groot aantal zeer kleine indexen. S3 HD bereikt 1000 indexen, de grenzen in de vorm van de indexbeperkingen. Als u een S3-HD-klant die meer dan 1000 indexen nodig heeft, neem dan contact op met Microsoft Support voor meer informatie over hoe verder te gaan.

## <a name="eliminate-skus-that-dont-meet-requirements"></a>SKU's die niet voldoen aan de vereisten te elimineren 

De volgende vragen kunt u de juiste beslissing van de SKU voor uw werkbelasting aankomen.

1. Hebt u **Service niveau overeenkomst (SLA)** eisen? Het besluit van de SKU-standaard Basic of niet-voorbeeld beperken.
2. **Hoeveel indexen** hebt u nodig? Een van de grootste variabelen factoring in de beslissing van een SKU is het aantal indexen worden ondersteund door elke SKU. Ondersteuning van de index is op aanzienlijk verschillende niveaus in de lagere prijzen lagen. Vereisten voor het aantal indexen kunnen worden een primaire de determinant van een besluit van de SKU.
3. **Het aantal documenten** in elke index wordt geladen? De uiteindelijke grootte van de index wordt bepaald door het aantal en de grootte van documenten. Als dat u de geschatte grootte van de index kunt schatten, kunt u dat getal ten opzichte van de grootte van de partitie per SKU, verlengd met het aantal partities vereist zijn voor het opslaan van een index van die grootte vergelijken. 
4. **Wat is de belasting query verwacht**? Zodra er opslagruimte beschikbaar komt, kunt u query werkbelasting. S2 en beide S3 SKU's in de buurt van gelijkwaardige doorvoer bieden, maar SLA eisen zal sprake is van een voorbeeld van SKU's. 
5. Als u de S2 of S3-laag overweegt, moet u bepalen of u [indexeerfuncties](search-indexer-overview.md)nodig. Indexeerfuncties zijn nog niet beschikbaar voor de S3 HD-laag. Alternatieve methode is met een push-model voor index-updates, waar u toepassing programmacode schrijft waarin een gegevensset push naar een index.

De meeste klanten kunnen een specifieke SKU of regel op basis van hun antwoorden op bovenstaande vragen. Als u nog steeds niet weet welke SKU om te gaan met, kunt u vragen naar MSDN of StackOverflow forums of Azure-ondersteuning voor verdere informatie contact op met.

## <a name="decision-validation-does-the-sku-offer-sufficient-storage-and-qps"></a>Beschikking van de validatie: de SKU biedt voldoende opslag en QPS?

Als laatste stap bezoeken [per service en per index secties in de grenzen van de Service](search-limits-quotas-capacity.md) en de [prijzen van de pagina](https://azure.microsoft.com/pricing/details/search/) om te controleren tegen een service-abonnement en schattingen. 

Als de prijs of de opslag vereisten valt buiten het bereik, wilt u mogelijk refactoring van de werklast over meerdere kleinere services (bijvoorbeeld). Op een meer gedetailleerd niveau kan u indexen kleiner ontwerpen of filters gebruiken om query's efficiënter te maken.

> [AZURE.NOTE] Opslagvereisten zijn te weinig opgepompte als documenten overbodige gegevens bevatten. In het ideale geval bevatten documenten alleen doorzoekbare gegevens of metagegevens. Binaire gegevens worden niet-doorzoekbare en moet gescheiden worden opgeslagen (bijvoorbeeld in een Azure tabel of blob storage) met een veld in de index voor het opslaan van een URL-verwijzing naar de externe gegevens. De maximale grootte van een afzonderlijk document is 16 MB (of minder als je bulk uploaden van meerdere documenten in één aanvraag). Zie [limieten in Azure Search Service](search-limits-quotas-capacity.md) voor meer informatie.

## <a name="next-step"></a>Volgende stap

Als u eenmaal welke SKU is de juiste keuze te maken weet, gaat u verder met de volgende stappen uit:

- [Het maken van een zoekservice in de portal](search-create-service-portal.md)
- [De verdeling van de partities en de replica's voor het schalen van uw service wijzigen](search-capacity-planning.md)

