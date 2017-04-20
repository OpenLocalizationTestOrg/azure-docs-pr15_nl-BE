<properties 
    pageTitle="DocumentDB gegevens met volgorde sorteren | Microsoft Azure" 
    description="Informatie in query's voor DocumentDB in LINQ en SQL ORDER BY gebruiken en hoe u een indexering beleid voor de ORDER BY-query's opgeven." 
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="cgronlun" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="arramac"/>

# <a name="sorting-documentdb-data-using-order-by"></a>Gegevens sorteren DocumentDB Order By gebruiken
DocumentDB voor Microsoft Azure documenten opvragen via JSON-documenten met behulp van SQL. Queryresultaten kunnen worden besteld via de ORDER BY-component in SQL-instructies van query.

Na het lezen van dit artikel, zult u de volgende vragen beantwoorden: 

- Hoe ik query met een Order By
- Hoe configureer een beleid voor indexering voor Order By?
- Wat er nieuw volgende?

[Voorbeelden](#samples) en een [FAQ](#faq) zijn ook beschikbaar.

Zie voor een volledig overzicht van SQL-query de [DocumentDB Query zelfstudie](documentdb-sql-query.md).

## <a name="how-to-query-with-order-by"></a>Hoe u de Query met een Order By
Net als in ANSI SQL kunt u nu opnemen een optionele Order By-component in SQL-instructies bij het opvragen van DocumentDB. De component kan bevatten een optioneel argument ASC/DESC de volgorde waarin de resultaten moeten worden opgehaald. 

### <a name="ordering-using-sql"></a>Met behulp van SQL te bestellen
Hier is bijvoorbeeld een query op te halen van de top 10 boeken in dalende volgorde van hun titels. 

    SELECT TOP 10 * 
    FROM Books 
    ORDER BY Books.Title DESC

### <a name="ordering-using-sql-with-filtering"></a>Met filteren met behulp van SQL te bestellen
U kunt bestellen met een geneste eigenschap binnen documenten, zoals Books.ShippingDetails.Weight en kunt u extra filters in de WHERE-component in combinatie met een Order By, zoals in het volgende voorbeeld:

    SELECT * 
    FROM Books 
    WHERE Books.SalePrice > 4000
    ORDER BY Books.ShippingDetails.Weight

### <a name="ordering-using-the-linq-provider-for-net"></a>Bestellen via de LINQ Provider voor .NET
Met de SDK voor .NET versie 1.2.0 en hoger, ook kunt u de OrderBy() of OrderByDescending()-component binnen het LINQ-query's, zoals in het volgende voorbeeld:

    foreach (Book book in client.CreateDocumentQuery<Book>(UriFactory.CreateDocumentCollectionUri("db", "books"))
        .OrderBy(b => b.PublishTimestamp)
        .Take(100))
    {
        // Iterate through books
    }

DocumentDB ondersteunt met extra querytypen binnenkort te bestellen met één numerieke, tekenreeks of Booleaanse eigenschap per query. Zie [wat er nieuw volgende](#Whats_coming_next) voor meer informatie.

## <a name="configure-an-indexing-policy-for-order-by"></a>Een beleid voor indexering configureren voor Order By

Intrekken DocumentDB biedt ondersteuning voor twee soorten indexen (Hash en bereik), die kunnen worden ingesteld voor specifieke paden/eigenschappen voor gegevenstypen (tekenreeksen/nummers) en bij verschillende betrouwbaarheidsparameters (maximale nauwkeurigheid of waarde met een vaste precisie). Aangezien DocumentDB Hash indexeren als standaard gebruikt, moet u een nieuwe collectie maken met een aangepaste indexing beleid met een bereik van getallen, tekenreeksen of beide, Order By gebruiken. 

>[AZURE.NOTE] String bereik indexen werden op 7 juli 2015 met REST API versie 2015-06-03 geïntroduceerd. Om beleid voor Order By tegen tekenreeksen maken, moet u versie 1.2.0 van de SDK voor .NET of versie 1.1.0 van Python, Node.js of SDK voor Java SDK.
>
>Voor de REST API versie 2015-06-03 is het standaardbeleid voor collectie indexing Hash voor tekenreeksen en getallen. Dit is gewijzigd in Hash voor tekenreeksen en bereik voor getallen. 

Zie [indexing DocumentDB-beleid](documentdb-indexing-policies.md)voor meer informatie.

### <a name="indexing-for-order-by-against-all-properties"></a>Indexering voor Order By tegen alle eigenschappen
Hier volgt hoe kan een verzameling maken met 'Alle Range' indexeren voor Order By tegen elke/alle numerieke of eigenschappen die worden weergegeven in JSON-documenten in deze tekenreeks. Hier negeren we het indextype voor string-waarden voor bereik en op de maximale nauwkeurigheid (-1).
                   
    DocumentCollection books = new DocumentCollection();
    books.Id = "books";
    books.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    
    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), books);  

>[AZURE.NOTE] Houd er rekening mee dat Order By alleen levert resultaten van de gegevenstypen (String en nummer) die zijn geïndexeerd met een RangeIndex. Bijvoorbeeld, hebt u standaard indexing beleid alleen met RangeIndex op nummers, weer een Order tegen een pad met string-waarden geen documenten.

### <a name="indexing-for-order-by-for-a-single-property"></a>Indexering voor Order By voor één eigenschap
Hier ziet u hoe u een collectie met indexeren voor Order By tegen alleen de eigenschap Title, die een tekenreeks kunt maken. Er zijn twee paden, één voor de eigenschap Title ("/ titel /?') met bereik indexering en de andere voor elke andere eigenschap met de standaard indexing schema dat Hash voor tekenreeksen en bereik voor getallen is.                    
    
    booksCollection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/Title/?", 
            Indexes = new Collection<Index> { 
                new RangeIndex(DataType.String) { Precision = -1 } } 
            });
    
    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), booksCollection);  


## <a name="samples"></a>Voorbeelden
Bekijk dit [project van Github monsters](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries) die laat hoe u Order By zien, waaronder maken beleid indexeren en sorteren met behulp van het wisselbestand. De monsters zijn open-source en raden we u aan pull-aanvragen met de bijdragen die andere ontwikkelaars DocumentDB kon profiteren indienen. Raadpleeg de [richtlijnen van de bijdrage](https://github.com/Azure/azure-documentdb-net/blob/master/Contributing.md) voor instructies over het te dragen.  

## <a name="faq"></a>FAQ

**Wat is de verwachte aanvragen eenheid (RU) verbruik van Order By-query's?**

Na Order By wordt gebruikgemaakt van de DocumentDB-index voor zoekopdrachten, zijn het aantal eenheden van aanvraag worden gebruikt door een Order By-query's vergelijkbaar met de equivalente query's zonder Order By. Net als elke andere bewerking op DocumentDB afhankelijk het aantal eenheden van de aanvraag van de grootte/vorm van documenten, alsmede de complexiteit van de query. 


**Wat is de verwachte indexeren overhead voor Order By?**

De indexering opslagoverhead is evenredig aan het aantal eigenschappen. In het ergste geval worden de overhead index 100% van de gegevens. Er is geen verschil in overhead doorvoer (aanvraag eenheden) tussen indexing bereik/Order By en standaard Hash indexeren.

**Hoe ik mijn bestaande gegevens in met behulp van Order By DocumentDB opvragen?**

Om de queryresultaten met volgorde sorteert, wijzigt u het beleid voor indexering van de collectie te gebruiken type index bereik ten opzichte van de eigenschap die wordt gebruikt om te sorteren. Zie [Indexing beleid wijzigen](documentdb-indexing-policies.md#modifying-the-indexing-policy-of-a-collection). 

**Wat zijn de huidige beperkingen van de Order By?**

Order By alleen tegen een eigenschap, een numerieke kan worden opgegeven of tekenreeks als het bereik is geïndexeerd met de maximale nauwkeurigheid (-1).

U kan het volgende niet uitvoeren:
 
- Order By met interne reeks eigenschappen, zoals id, _rid en _self (binnenkort beschikbaar).
- Order By met eigenschappen die zijn afgeleid van het resultaat van een intra-document join (binnenkort beschikbaar).
- Sorteren op meerdere eigenschappen (binnenkort beschikbaar).
- Order By met query's voor databases, collecties, gebruikers, machtigingen of bijlagen (binnenkort beschikbaar).
- Volgorde van door met berekende eigenschappen zoals het resultaat van een expressie of een UDF/ingebouwde-in functie.

Order By wordt momenteel niet ondersteund voor query's cross-partitie wanneer Query Explorer in Azure portal.

## <a name="troubleshooting"></a>Het oplossen van problemen

Als er een foutbericht dat Order By wordt niet ondersteund, controleren om ervoor te zorgen dat u gebruikt een versie van de [SDK](documentdb-sdk-dotnet.md) die Order By ondersteunt. 

## <a name="next-steps"></a>Volgende stappen

De [Github monsters project](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries) zich splitsen en start uw gegevens te bestellen! 

## <a name="references"></a>Verwijzingen
* [DocumentDB Queryreferentie](documentdb-sql-query.md)
* [DocumentDB indexeren beleidsverwijzing](documentdb-indexing-policies.md)
* [DocumentDB SQL Reference](https://msdn.microsoft.com/library/azure/dn782250.aspx)
* [DocumentDB Order By-voorbeelden](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries)
 

