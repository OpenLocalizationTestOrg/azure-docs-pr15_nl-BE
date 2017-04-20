<properties
    pageTitle="Wat is Azure zoeken | Microsoft Azure | De zoekservice hosted cloud"
    description="Azure Search is een zoekservice gehost wolk volledig beheerd. Meer informatie in het Functieoverzicht van deze."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="what-is-azure-search"></a>Wat is Azure zoeken?

Azure Search is een wolk zoeken als een service-oplossing die gedelegeerd beheer van infrastructuur en server bij Microsoft, zodat u met een kant-en-klare service die u kunt vullen met uw gegevens en aan de zoekopdracht toevoegen aan uw website of mobiele toepassing. Azure Search kunt u gemakkelijk een krachtige zoekfunctie toevoegen aan uw toepassingen met behulp van een eenvoudige [REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) of [.NET SDK](search-howto-dotnet-sdk.md) zonder infrastructuurbeheer zoeken of steeds een deskundige bij zoeken.

## <a name="give-your-users-a-powerful-search-experience"></a>Geef uw gebruikers een krachtige zoekfunctie

**Krachtige query's** kunnen worden geformuleerd, met de [eenvoudige query-syntaxis](https://msdn.microsoft.com/library/azure/dn798920.aspx), waardoor de logische operators, woordgroep zoeken operatoren, operatoren achtervoegsel, operators voorrang. De [syntaxis van de query Lucene](https://msdn.microsoft.com/library/azure/mt589323.aspx) kunt bovendien zoeken bij benadering, nabijheid search term verhogen en reguliere expressies inschakelen. Azure zoeken ondersteunt ook aangepaste lexicale analyzers, zodat uw toepassing om complexe zoekopdrachten met behulp van fonetische overeenstemming en reguliere expressies te verwerken.

**Ondersteuning voor taal** is [opgenomen voor 56 verschillende talen](https://msdn.microsoft.com/library/azure/dn879793.aspx). Met behulp van Lucene analyzers en Microsoft analyzers (verfijnd door jaren van natuurlijke taal verwerking in Office en Bing), kan Azure Search tekst in het zoekvak van de toepassing voor het verwerken van taalspecifieke linguistics, met inbegrip van werkwoordsvormen, geslacht, onregelmatig meervoud zelfstandige naamwoorden (bijvoorbeeld ' muis' versus 'muizen'), wordt samengesteld uit word, woordafbreking (voor talen zonder spaties) en meer intelligente analyseren.

**Zoeksuggesties** kan worden ingeschakeld voor de zoekbalken autocompleted en type-ahead query's. [De werkelijke documenten in de index worden voorgesteld](https://msdn.microsoft.com/library/azure/dn798936.aspx) als gebruikers zoeknaam gedeeltelijke invoer.

**Klik op markeren** [kunnen](https://msdn.microsoft.com/library/azure/dn798927.aspx) gebruikers zien het fragment van de tekst in elk resultaat dat de overeenkomsten voor de query bevat. U kunt kiest welke velden geselecteerde fragmenten terug.

**Beperkt navigatie** is eenvoudig toegevoegd aan de pagina met zoekresultaten met Azure Search. Met behulp van [slechts een enkele queryparameter](https://msdn.microsoft.com/library/azure/dn798927.aspx)retourneert Azure zoekactie alle nodige informatie om te bouwen van een beperkt zoekfunctie in de gebruikersinterface van uw app waarmee uw gebruikers kunnen inzoomen zoekresultaten en filteren (bv. catalogusitems filteren op prijs bereik of merk).

**Geo-ruimtelijke** ondersteuning kunt u op intelligente wijze verwerken, filteren en weergeven van geografische locaties. Azure Search kan gebruikers gegevens op basis van de nabijheid van een zoekresultaat op een opgegeven locatie of op basis van een bepaalde geografische regio verkennen. In deze video wordt uitgelegd hoe het werkt: [Channel 9: Azure Search en georuimtelijke gegevens](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data).

**Filters** kunnen worden gebruikt om eenvoudig beperkt navigatie opnemen in de gebruikersinterface van de toepassing, de query formulering verbeteren en filter op basis van gebruiker of ontwikkelaar-opgegeven criteria. Maak krachtige filters met behulp van de [OData-syntaxis](https://msdn.microsoft.com/library/azure/dn798921.aspx).

## <a name="empower-your-developers-with-an-easy-to-use-service"></a>De ontwikkelaars met een gemakkelijk te gebruiken service bieden

**Hoge beschikbaarheid** garandeert een uiterst betrouwbare service zoekfunctie. Wanneer geschaald naar behoren, [Azure Search biedt een SLA 99,9%](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

**Volledig beheerd** als een end-to-end oplossing, Azure Search is absoluut geen beheer van de infrastructuur vereist. Uw service kan eenvoudig worden afgestemd op uw behoeften door schaling in twee dimensies voor het verwerken van meer opslag van documenten, hogere querybelasting of beide.

**Integratie van gegevens** met behulp van [indexeerfuncties](https://msdn.microsoft.com/library/azure/dn946891.aspx) kunnen Azure zoeken automatisch verkennen Azure SQL-Database, Azure, DocumentDB of [Azure Blob-opslag](search-howto-indexing-azure-blob-storage.md) om te synchroniseren van inhoud van de zoekindex met uw primaire gegevensopslag.

**Document kraken** is beschikbaar (in het voorbeeld) [te lezen en te indexeren grote bestandsindelingen](search-howto-indexing-azure-blob-storage.md) zoals Microsoft Office en het PDF- en HTML-documenten.

**Zoekopdracht verkeer analytics** zijn [gemakkelijk verzameld en geanalyseerd](search-traffic-analytics.md) om inzicht te krijgen in wat gebruikers in het zoekvak typt.

**Eenvoudige scoren** is een belangrijk voordeel van Azure Search. [Score van profielen](https://msdn.microsoft.com/library/azure/dn798928.aspx) worden gebruikt om te kunnen organisaties model relevantie als functie van de waarden in de documenten zelf. Zoals wellicht u nieuwere producten of om hoger in de zoekresultaten verschijnen producten met korting. U kunt ook de score profielen voor het persoonlijke scoren op basis van voorkeuren klant zoeken u apart opgeslagen en bijgehouden met behulp van tags maken.

**Sorteren** is aangeboden voor meerdere velden via de index-schema en klik vervolgens op query-moment met een enkele zoekparameter ingeschakeld.

**Het wisselbestand** en de zoekresultaten beperken is [eenvoudig met het besturingselement goed afgestelde](search-pagination-page-layout.md) Azure Search biedt via de zoekresultaten.  

**Search explorer** kunt u query's probleem tegen alle van de indexen rechts van Azure portal van uw account zodat u gemakkelijk kunt testen van query's en profielen score verfijnen.

## <a name="how-it-works"></a>Hoe het werkt

### <a name="1-provision-service"></a>1. levering service
U kunt een Azure Search-service met behulp van de [Portal Azure](https://portal.azure.com/) of de [Azure Resource Management API](https://msdn.microsoft.com/library/azure/dn832684.aspx)draaien.

Afhankelijk van hoe u de search-service configureren, gebruikt u de gratis laag van de service die wordt gedeeld met andere abonnees Azure zoeken, ofwel een [laag betaald](https://azure.microsoft.com/pricing/details/search/) die resources alleen worden gebruikt door de service dedicates. Tijdens het inrichten van uw service, u het gebied van het datacenter die fungeert als host voor uw service.

Afhankelijk van welke laag van de service die u kiest, kunt u uw service schalen in twee dimensies: 1) replica's toevoegen om te groeien uw capaciteit om zware querybelasting verwerken en 2) partities opslag toevoegen aan meer documenten toevoegen. Document opslag- en doorvoer afzonderlijk verwerkt, kunt u de zoekservice voor uw specifieke behoeften aanpassen.

### <a name="2-create-index"></a>2. index maken
Voordat u uw inhoud naar uw Azure Search-service uploaden kunt, moet u eerst een zoekindex Azure definiëren. Een index is vergelijkbaar met een databasetabel die de gegevens bevat en zoekopdrachten kunt accepteren. U definieert het index-schema toe te wijzen aan de structuur van de documenten die u zoeken wilt, vergelijkbaar met de velden in een database.

Het schema van deze indexen kan worden gemaakt in de Azure-Portal of via programmacode [met behulp van de SDK voor .NET](search-howto-dotnet-sdk.md) of [REST API](https://msdn.microsoft.com/library/azure/dn798941.aspx). Zodra de index is gedefinieerd, kunt u uw gegevens vervolgens uploaden naar de Azure zoekservice waar het vervolgens wordt geïndexeerd.

### <a name="3-index-data"></a>3. gegevens in Index
Als u de velden en de kenmerken van de index hebt gedefinieerd, bent u klaar voor het uploaden van uw inhoud in de index. Gegevens uploaden naar de index kunt u een push- of pull model.

Het pull-model is beschikbaar via indexeerfuncties die kunnen worden geconfigureerd voor op de vraag of de geplande updates (Zie [indexering bewerkingen (Azure Search Service REST API)](https://msdn.microsoft.com/library/azure/dn946891.aspx)), zodat u naar gegevens en gegevenswijzigingen van een DocumentDB Azure, Azure SQL-Database, Azure Blob-opslag of SQL Server gehost in een VM Azure gemakkelijk te nemen.

De push-model is beschikbaar via de SDK of REST API's gebruikt om bijgewerkte documenten te verzenden aan een index. U kunt gegevens push vanuit vrijwel elke gegevensset met behulp van de JSON-indeling. Zie [toevoegen, bijwerken, of documenten verwijderen](https://msdn.microsoft.com/library/azure/dn798930.aspx) of [het gebruik van de .NET SDK)](search-howto-dotnet-sdk.md) voor hulp bij het laden van gegevens.

### <a name="4-search"></a>4. zoeken
Zodra u uw Azure zoekindex hebt gevuld, kunt u nu [probleem zoekquery's](https://msdn.microsoft.com/library/azure/dn798927.aspx) aan uw service-eindpunt met eenvoudige HTTP-aanvragen met REST API of de SDK voor .NET.

## <a name="try-it-now-for-free"></a>Probeer het nu (gratis!)
U kunt zoeken in Azure vandaag! Als u al een Azure-account hebt, kunt u [dient een service in de vrij laag](search-create-service-portal.md).

Als u geen kunt u proberen een gratis, 60 minuten sessie zonder Azure account aanmelden vereist. Ga naar de [Azure App-Service probeert](http://go.microsoft.com/fwlink/p/?LinkId=618214) en selecteer 'Web App'. Selecteer de sjabloon 'ASP.NET + Azure Search' aan de slag.
