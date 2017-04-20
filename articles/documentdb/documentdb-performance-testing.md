<properties 
    pageTitle="DocumentDB schaal en prestaties testen | Microsoft Azure" 
    description="Informatie over het uitvoeren van schaal en prestaties testen met Azure DocumentDB"
    keywords="prestaties testen"
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="arramac"/>

# <a name="performance-and-scale-testing-with-azure-documentdb"></a>Prestaties en testen met Azure DocumentDB schaal
Prestaties en schaal testen is een belangrijke stap in de ontwikkeling van toepassingen. Voor veel toepassingen de databaselaag heeft een belangrijke invloed op de algehele prestaties en schaalbaarheid, en is daarom een essentieel onderdeel van prestatietests. [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) is voor elastisch schaal en voorspelbare prestaties gebouwde en dus uitstekend geschikt voor toepassingen die een krachtige databaselaag nodig. 

In dit artikel wordt een referentie voor ontwikkelaars uitvoering van testsuites prestaties voor hun werkbelasting DocumentDB of DocumentDB voor krachtige toepassing scenario's te evalueren. Het richt zich voornamelijk op geïsoleerde prestaties testen van de database, maar bevat ook aanbevolen procedures voor productietoepassingen.

Na het lezen van dit artikel, is het mogelijk om de volgende vragen te beantwoorden:   

- Waar vind ik een monster .NET clienttoepassing voor prestaties testen van Azure DocumentDB 
- Hoe ik niveaus met Azure DocumentDB hoge doorvoersnelheden van mijn clienttoepassing bereikt?

Download het project van [DocumentDB prestaties testen voorbeeld](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)om te beginnen met de code. 

> [AZURE.NOTE] Het doel van deze toepassing is om aan te tonen, aanbevolen procedures voor het extraheren van betere prestaties van DocumentDB met een klein aantal clientcomputers. Dit is niet gemaakt om aan te tonen van de capaciteit van de piek van de service, die u kunt de schaal van limitlessly.

Zie [tips voor betere prestaties van DocumentDB](documentdb-performance-tips.md)als u op zoek bent voor client-side configuratieopties DocumentDB prestaties te verbeteren.

## <a name="run-the-performance-testing-application"></a>De toepassing testen uitvoeren
De snelste manier om te beginnen is om te compileren en uitvoeren van het monster .NET, zoals beschreven in de volgende stappen uit. Ook kunt u bekijken van de broncode en soortgelijke configuraties implementeren voor uw eigen clienttoepassingen.

**Stap 1:** Het project uit [DocumentDB prestaties testen monster](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)downloaden of zich splitsen de opslagplaats Github.

**Stap 2:** Wijzig de instellingen voor EndpointUrl, AuthorizationKey, CollectionThroughput en DocumentTemplate App.config (optioneel).

> [AZURE.NOTE] Raadpleeg de [Pagina met prijzen](https://azure.microsoft.com/pricing/details/documentdb/) voor het schatten van de kosten per collectie voor collecties met hoge doorvoer wordt ingericht. DocumentDB wissels opslag en doorvoer onafhankelijk op uurbasis, zo u kosten besparen kunt door te verwijderen of verlagen van de doorvoer van DocumentDB collecties na het testen.

**Stap 3:** Compileren en de toepassing van de console vanaf de opdrachtregel uitvoeren. Hier ziet u de volgende uitvoer:

    Summary:
    ---------------------------------------------------------------------
    Endpoint: https://docdb-scale-demo.documents.azure.com:443/
    Collection : db.testdata at 50000 request units per second
    Document Template*: Player.json
    Degree of parallelism*: 500
    ---------------------------------------------------------------------

    DocumentDBBenchmark starting...
    Creating database db
    Creating collection testdata
    Creating metric collection metrics
    Retrying after sleeping for 00:03:34.1720000
    Starting Inserts with 500 tasks
    Inserted 661 docs @ 656 writes/s, 6860 RU/s (18B max monthly 1KB reads)
    Inserted 6505 docs @ 2668 writes/s, 27962 RU/s (72B max monthly 1KB reads)
    Inserted 11756 docs @ 3240 writes/s, 33957 RU/s (88B max monthly 1KB reads)
    Inserted 17076 docs @ 3590 writes/s, 37627 RU/s (98B max monthly 1KB reads)
    Inserted 22106 docs @ 3748 writes/s, 39281 RU/s (102B max monthly 1KB reads)
    Inserted 28430 docs @ 3902 writes/s, 40897 RU/s (106B max monthly 1KB reads)
    Inserted 33492 docs @ 3928 writes/s, 41168 RU/s (107B max monthly 1KB reads)
    Inserted 38392 docs @ 3963 writes/s, 41528 RU/s (108B max monthly 1KB reads)
    Inserted 43371 docs @ 4012 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 48477 docs @ 4035 writes/s, 42282 RU/s (110B max monthly 1KB reads)
    Inserted 53845 docs @ 4088 writes/s, 42845 RU/s (111B max monthly 1KB reads)
    Inserted 59267 docs @ 4138 writes/s, 43364 RU/s (112B max monthly 1KB reads)
    Inserted 64703 docs @ 4197 writes/s, 43981 RU/s (114B max monthly 1KB reads)
    Inserted 70428 docs @ 4216 writes/s, 44181 RU/s (115B max monthly 1KB reads)
    Inserted 75868 docs @ 4247 writes/s, 44505 RU/s (115B max monthly 1KB reads)
    Inserted 81571 docs @ 4280 writes/s, 44852 RU/s (116B max monthly 1KB reads)
    Inserted 86271 docs @ 4273 writes/s, 44783 RU/s (116B max monthly 1KB reads)
    Inserted 91993 docs @ 4299 writes/s, 45056 RU/s (117B max monthly 1KB reads)
    Inserted 97469 docs @ 4292 writes/s, 44984 RU/s (117B max monthly 1KB reads)
    Inserted 99736 docs @ 4192 writes/s, 43930 RU/s (114B max monthly 1KB reads)
    Inserted 99997 docs @ 4013 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 100000 docs @ 3846 writes/s, 40304 RU/s (104B max monthly 1KB reads)

    Summary:
    ---------------------------------------------------------------------
    Inserted 100000 docs @ 3834 writes/s, 40180 RU/s (104B max monthly 1KB reads)
    ---------------------------------------------------------------------
    DocumentDBBenchmark completed successfully.


**Stap 4 (indien nodig):** De doorvoer gerapporteerd (RU/s) van het hulpmiddel moet hetzelfde of hoger is dan de ingerichte doorvoer van de collectie. Als dat niet het geval is, verhoogt de DegreeOfParallelism in kleine stappen kunt u de limiet is bereikt. Als u de doorvoer van uw clienttoepassing plateaus, kunt starten meerdere exemplaren van een toepassing op dezelfde of op verschillende computers u de ingerichte limiet bereikt tussen de verschillende instanties. Als u hulp nodig hebt bij deze stap, schrijf een e-mail naar askdocdb@microsoft.com of vult u een support ticket.

Zodra u de app uitgevoerd hebt, kunt u proberen verschillende [indexering beleid](documentdb-indexing-policies.md) en [consistentie niveaus](documentdb-consistency-levels.md) om te begrijpen van de invloed daarvan op de doorvoer en latentie. Ook kunt u de broncode bekijken en implementeren van vergelijkbare configuraties voor uw eigen testsuites of productietoepassingen.

## <a name="next-steps"></a>Volgende stappen
In dit artikel wordt bekeken hoe u de prestaties en schaal testen met een console .NET app met DocumentDB kunt uitvoeren. Raadpleeg de onderstaande koppelingen voor meer informatie over het werken met DocumentDB.

* [DocumentDB prestaties testen monster](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [Client configuratie-opties DocumentDB prestaties te verbeteren](documentdb-performance-tips.md)
* [Server-side partitioneren in DocumentDB](documentdb-partition-data.md)
* [DocumentDB collecties en prestaties](documentdb-performance-levels.md)
* [DocumentDB .NET SDK-documentatie op MSDN](https://msdn.microsoft.com/library/azure/dn948556.aspx)
* [Monsters DocumentDB .NET](https://github.com/Azure/azure-documentdb-net)
* [DocumentDB-blog over tips voor betere prestaties](https://azure.microsoft.com/blog/2015/01/20/performance-tips-for-azure-documentdb-part-1-2/)
