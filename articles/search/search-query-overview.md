<properties
    pageTitle="De zoekindex Azure query | Microsoft Azure | De zoekservice hosted cloud"
    description="Een zoekopdracht in Azure zoekopdracht samenstellen en gebruiken van zoekparameters zoekresultaten filteren en sorteren."
    services="search"
    manager="jhubbard"
    documentationCenter=""
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="query-your-azure-search-index"></a>Uw Azure Search-index opvragen
> [AZURE.SELECTOR]
- [Overzicht](search-query-overview.md)
- [Portal](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [REST](search-query-rest-api.md)

Bij het indienen van aanvragen naar Azure Search zoeken, zijn er een aantal parameters dat naast de werkelijke woorden die u in het zoekvak van de toepassing typt kan worden opgegeven. Deze queryparameters kunnen u sommige diepere controle van de ervaring van volledige-tekstzoekacties te bereiken.

Hieronder vindt u een lijst met veelvoorkomende toepassingen van de queryparameters in Azure zoeken in het kort uitgelegd. Zie de gedetailleerde pagina's voor de [REST API](https://msdn.microsoft.com/library/azure/dn798927.aspx) en [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.searchparameters_properties.aspx)voor volledige dekking van de queryparameters en hun gedrag.

## <a name="types-of-queries"></a>Typen query 's

Azure Search biedt een groot aantal opties voor het maken van zeer krachtige query's. Zijn de twee belangrijkste soorten query, u gebruikt `search` en `filter`. A `search` query gezocht naar een of meer termen in alle _doorzoekbare_ velden in de index en werkt zoals u een zoekmachine zoals Google of Bing verwacht te werken. A `filter` query wordt geëvalueerd als een Boole-expressie via alle _Filterbaar_ velden in een index. In tegenstelling tot `search` query `filter` query's komen overeen met de exacte inhoud van een veld, wat betekent dat voor reeks velden hoofdlettergevoelig zijn.

U kunt zoekopdrachten en filters samen of afzonderlijk. Als u ze samen gebruikt, wordt eerst het filter wordt toegepast op de hele index en vervolgens de zoekactie wordt uitgevoerd op de resultaten van het filter. Filters kunnen dus een nuttige techniek voor het verbeteren van prestaties van query's nadat ze besparen op de set van documenten die in de query moet worden verwerkt.

De syntaxis voor filterexpressies is een subset van de [taal van OData-filter](https://msdn.microsoft.com/library/azure/dn798921.aspx). Voor zoekopdrachten kunt u de [syntaxis van de vereenvoudigde](https://msdn.microsoft.com/library/azure/dn798920.aspx) of [Lucene query-syntaxis](https://msdn.microsoft.com/library/azure/mt589323.aspx) die hieronder worden besproken.

### <a name="simple-query-syntax"></a>Eenvoudige query-syntaxis
De [eenvoudige query-syntaxis](https://msdn.microsoft.com/library/azure/dn798920.aspx) is de standaardtaal van de query die wordt gebruikt in Azure zoeken. De eenvoudige query-syntaxis ondersteunt een aantal algemene zoekopdracht-operators, met inbegrip van de functies en, of niet, woordgroep, achtervoegsel en operators voorrang.

### <a name="lucene-query-syntax"></a>Lucene query-syntaxis
De [syntaxis van de query Lucene](https://msdn.microsoft.com/library/azure/mt589323.aspx) kunt u de wijd verbreide en expressieve querytaal is ontwikkeld als deel van een [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)gebruiken.

Met deze querysyntaxis kunt u gemakkelijk bereiken van de volgende mogelijkheden: [veld-binnen het bereik van query's](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_fields), [Zoeken bij benadering](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_fuzzy) [nabijheid zoeken](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_proximity), [waarbij de term](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_termboost), [reguliere expressie zoeken](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_regex), [zoeken met jokertekens](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_wildcard), [syntaxis fundamentals](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_syntax)en [query's met behulp van Booleaanse operatoren](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_boolean).



## <a name="ordering-results"></a>Resultaten te bestellen
U kunt aanvragen dat Azure Search de volgorde van waarden in een bepaald veld resultaten speelt bij de ontvangst van de resultaten van een query. De zoekresultaten op basis van de positie van de score van elk document zoeken die is afgeleid van [TF IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)de standaard Azure Search orders.

Als u wilt dat Azure zoeken om terug te keren uw resultaten gesorteerd op een andere waarde dan de score zoeken kunt u de `orderby` parameter zoeken. Kunt u de waarde van de `orderby` parameter met veldnamen en aanroepen van de [ `geo.distance()` functie](https://msdn.microsoft.com/library/azure/dn798921.aspx) voor georuimtelijke waarden. Elke expressie kan worden gevolgd door `asc` om aan te geven dat de resultaten in oplopende volgorde, worden aangevraagd en `desc` om aan te geven dat de resultaten in aflopende volgorde worden aangevraagd. De waarde standaard in oplopende volgorde.

## <a name="paging"></a>Wisselbestand
Azure Search eenvoudig te implementeren paginering van de zoekresultaten. Met behulp van de `top` en `skip` parameters kunt u probleemloos search-aanvragen waarmee u het totale aantal zoekresultaten in subsets te beheren, bestelde waarmee gemakkelijk zoeken op goede UI praktijken ontvangen uitgeven. Bij de ontvangst van deze kleinere subsets van de resultaten, kunt u ook het aantal documenten in de totale set zoekresultaten ontvangen.

U kunt meer informatie over wisselbestanden zoekresultaten in het artikel [pagina hoe zoekresultaten in Azure zoeken](search-pagination-page-layout.md).


## <a name="hit-highlighting"></a>Klik op markeren
In Azure Search, benadrukken de exacte gedeelte van zoekresultaten die overeenkomen met de zoekopdracht is in een handomdraai met behulp van de `highlight`, `highlightPreTag`, en `highlightPostTag` parameters. U kunt opgeven welke _doorzoekbare_ velden moet hebben hun overeenkomende tekst gewezen en geven de exacte reeks tags toe te voegen aan het begin en einde van de overeenkomende tekst die Azure Search als resultaat gegeven.
