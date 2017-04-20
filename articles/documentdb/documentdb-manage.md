<properties
    pageTitle="DocumentDB opslag en prestaties | Microsoft Azure" 
    description="Informatie over opslag van gegevens en de opslag van documenten in DocumentDB en hoe u kunt de schaal van DocumentDB aan de capaciteitsbehoeften van uw toepassing." 
    keywords="opslag van documenten"
    services="documentdb" 
    authors="syamkmsft" 
    manager="jhubbard" 
    editor="cgronlun" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/18/2016" 
    ms.author="syamk"/>

# <a name="learn-about-storage-and-predictable-performance-provisioning-in-documentdb"></a>Meer informatie over opslag en voorspelbare prestaties ingericht in DocumentDB
Azure DocumentDB is een volledig beheerde, schaalbare document-georiënteerde NoSQL databaseservice voor JSON-documenten. Met DocumentDB hoeft u niet te verhuren van virtuele machines, implementeren van software of databases controleren. DocumentDB wordt geëxploiteerd en continu gecontroleerd door Microsoft engineers leveren wereld klasse beschikbaarheid, prestaties en beveiliging van gegevens.  

U kunt aan de slag met DocumentDB door het [maken van een databaseaccount](documentdb-create-account.md) en een [database met DocumentDB](documentdb-create-database.md) via de [portal Azure](https://portal.azure.com/). DocumentDB databases worden aangeboden in eenheden van solid-state drive (SSD) back-opslag en doorvoer. Deze opslageenheden zijn ingericht door de [verzamelingen van de database maken](documentdb-create-collection.md) in uw databaseaccount elke collectie met gereserveerde doorvoer die kan worden geschaald omhoog of omlaag te allen tijde voldoen aan de vereisten van uw toepassing. 

Als uw toepassing uw gereserveerde doorvoer voor een of meerdere collecties overschrijdt, worden aanvragen beperkt op basis van per collectie. Dit betekent dat sommige aanvragen terwijl anderen kunnen de snelheid van de kans van slagen.

Dit artikel bevat een overzicht van de middelen en de parameters die beschikbaar zijn voor de opslag van gegevens plannen en beheren van capaciteit. 

## <a name="database-account"></a>Database-account
Als abonnee op een Azure, kunt u één of meer DocumentDB database rekeningen voor het beheren van uw resources database inrichten. Elk abonnement is gekoppeld aan een enkele Azure abonnement. 

DocumentDB rekeningen kunnen worden gemaakt via de [portal Azure](documentdb-create-account.md)of met behulp van [een sjabloon ARM of Azure CLI](documentdb-automation-resource-manager-cli.md).

## <a name="databases"></a>Databases
Een database met één DocumentDB kan bevatten nagenoeg een onbeperkte omvang voor de opslag van documenten gegroepeerd in verzamelingen. Verzamelingen bieden prestaties isolatie - elke collectie kan worden ingericht met doorvoer die niet worden gedeeld met andere collecties in dezelfde database of hetzelfde account. Een database DocumentDB is in grootte, variërend van GB tot TBs van SSD back documentopslag en doorvoer ingerichte elastisch. In tegenstelling tot een traditionele database RDBMS, een database in DocumentDB is niet binnen het bereik van één computer en kan gegevens voor meerdere machines of clusters.  

Met DocumentDB, als u nodig hebt voor het schalen van toepassingen, kunt u meer collecties of databases of beide. Databases kunnen worden gemaakt via de [Azure portal](documentdb-create-database.md) of via een van de [DocumentDB SDK's](documentdb-dotnet-samples.md).   

## <a name="database-collections"></a>Database-collecties
Elke database DocumentDB kan een of meer collecties bevatten. Verzamelingen fungeren als gegevenspartities van maximaal beschikbare voor verwerking en opslag van documenten. Elke collectie kunt u documenten met heterogene schema opslaan. Querymogelijkheden en de automatische indexering van DocumentDB kunnen u eenvoudig filteren en ophalen van documenten. Een collectie vindt u het bereik voor document opslag- en kan worden uitgevoerd. Een collectie is ook een transactie domein voor alle documenten die zich daarin. Verzamelingen worden toegewezen op basis van de waarde is ingesteld in de Azure portal of via de SDK's doorvoer. 

Collecties met DocumentDB automatisch ingedeeld in een of meer fysieke servers. Als u een verzameling maakt, kunt u de doorvoer ingerichte uitgedrukt in eenheden van de aanvraag per seconde en een belangrijke eigenschap van de partitie. De waarde van deze eigenschap wordt gebruikt door de DocumentDB voor het distribueren van documenten tussen de partities en route aanvragen zoals query's. De waarde van de partitie fungeert ook als de transactiegrens voor opgeslagen procedures en triggers. Elke collectie heeft een gereserveerde deel van de doorvoer van die collectie, wordt niet gedeeld met andere collecties in dezelfde account. Daarom kunt u de schaal van uw toepassing, zowel in termen van opslag en doorvoer. 

Verzamelingen kunnen worden gemaakt via de [Azure portal](documentdb-create-collection.md) of via een van de [DocumentDB SDK's](documentdb-sdk-dotnet.md).   
 
## <a name="request-units-and-database-operations"></a>Eenheden en databasebewerkingen aanvragen

Als u een verzameling maakt, kunt u reserveren doorvoersnelheid uitgedrukt in [eenheden van de aanvraag (RU)](documentdb-request-units.md) per seconde. In plaats daarvan van gedachten over en het beheer van hardwarebronnen, u kunt een **aanvraag eenheid** beschouwen als een enkele maateenheid voor de resources verschillende databasebewerkingen uitvoeren en vereist een aanvraag van toepassing. Het lezen van een document van 1 KB verbruikt hetzelfde 1 RU ongeacht het aantal artikelen die zijn opgeslagen in de collectie of het aantal gelijktijdige aanvragen dat tegelijkertijd wordt uitgevoerd. Alle aanvragen tegen de DocumentDB, met inbegrip van complexe bewerkingen zoals SQL-query's hebben een voorspelbare RU-waarde die op tijd kan worden bepaald. Als u de grootte van uw documenten en de frequentie van elke bewerking (lezen, schrijven en query's) voor uw toepassing te ondersteunen, kunt u het exacte bedrag van de doorvoer op de behoeften van uw toepassing inrichten en schalen van uw database omhoog en omlaag de prestaties van uw behoeften wijzigen. 

Elke collectie kan worden gereserveerd met doorvoer in blokken van 100 eenheden van de aanvraag per seconde van honderden tot miljoenen eenheden aanvraag per seconde. De ingerichte doorvoer kan gedurende de levensduur van een verzameling aan te passen aan de veranderende behoeften van de verwerking en patronen van uw toepassing worden aangepast. Zie voor meer informatie [DocumentDB prestaties](documentdb-performance-levels.md). 

>[AZURE.IMPORTANT] Collecties zijn factureerbare eenheden. De kostprijs wordt bepaald door de ingerichte doorvoer van de collectie die wordt gemeten in eenheden van de aanvraag per seconde met de totale verbruikte opslag in de gigabytes. 

Hoeveel eenheden van het verzoek zal verbruiken voor een bepaalde bewerking, zoals insert, delete, query of opgeslagen procedure kan worden uitgevoerd? Een eenheid voor de aanvraag is een gestandaardiseerde meting van kosten verwerking van aanvragen. Het lezen van een document van 1 KB is 1 RU, maar een aanvraag voor het invoegen, vervangen of verwijderen van hetzelfde document verbruiken meer verwerking van de service en daardoor meer eenheden van de aanvraag. Elk antwoord van de service bestaat uit een aangepaste header (`x-ms-request-charge`) die de aanvraag eenheden verbruikt voor de aanvraag wordt gemeld. Deze header is ook toegankelijk via het [SDK's](documentdb-sdk-dotnet.md). In de SDK voor .NET is [RequestCharge](https://msdn.microsoft.com/library/azure/dn933057.aspx#P:Microsoft.Azure.Documents.Client.ResourceResponse`1.RequestCharge) het een eigenschap van het object [ResourceResponse](https://msdn.microsoft.com/library/azure/dn799209.aspx) . Als u schatten uw behoeften doorvoer wilt voordat u een enkele aanroep, kunt u de [planning van de capaciteit](documentdb-request-units.md#estimating-throughput-needs) om te helpen bij deze schatting. 

>[AZURE.NOTE] De basislijn van 1 eenheid van de aanvraag voor een document van 1 KB correspondeert met een eenvoudige ophalen van het document met de [Consistentie van de sessie](documentdb-consistency-levels.md). 

Er zijn verschillende factoren die invloed hebben op de aanvraag eenheden verbruikt voor een bewerking tegen de account van een DocumentDB database. Deze factoren zijn onder andere:

- Grootte van het document. Documentgrootten vergroten de eenheden als u wilt lezen of schrijven van dat gegevens meer verbruikt.
- De eigenschap count. Uitgaande standaard het indexeren van alle eigenschappen van de eenheden die zijn gebruikt om een document te schrijven zal toenemen als de eigenschap count wordt verhoogd.
- Consistentie van de gegevens. Wanneer u gegevens consistent niveaus van sterke- of Staleness wordt begrensd, wordt extra eenheden verbruikt om documenten te lezen.
- Geïndexeerde eigenschappen. Een index-beleid voor elke collectie bepaalt welke eigenschappen worden standaard geïndexeerd. U kunt uw aanvraag eenheid consumptie door het beperken van het aantal geïndexeerde eigenschappen verminderen. 
- Document wordt geïndexeerd. Elk document wordt automatisch geïndexeerd standaard verbruiken u minder eenheden van de aanvraag als u niet wilt indexeren van uw documenten.

Zie voor meer informatie [DocumentDB aanvraag eenheden](documentdb-request-units.md). 

Hier is bijvoorbeeld een tabel waarin wordt aangegeven hoeveel eenheden van de aanvraag om in te richten op drie verschillende document formaten (1KB, 4KB en 64KB) en op twee verschillende prestatieniveaus (500 Paginaleesbewerkingen per seconde + 100 schrijfopdrachten per seconde en leesbewerkingen per seconde van 500 + 500 schrijfopdrachten per seconde). De consistentie van de gegevens in de sessie is geconfigureerd en de indexing-beleid is ingesteld op geen.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Documentgrootte</strong></p></td>
            <td valign="top"><p><strong>Paginaleesbewerkingen per seconde</strong></p></td>
            <td valign="top"><p><strong>Schrijfopdrachten per seconde</strong></p></td>
            <td valign="top"><p><strong>Aanvraag eenheden</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *1) + (100* 5) = 1000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *5) + (100* 5) = 3.000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *1.3) + (100* 7) = 1,350 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *1.3) + (500* 7) = 4,150 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *10) + (100* 48) = 9,800 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *10) + (500* 48) = 29.000 RU/s</p></td>
        </tr>
    </tbody>
</table>

Query's, opgeslagen procedures en triggers aanvraag eenheden op basis van de complexiteit van de bewerkingen worden verbruikt. Inspecteer de aanvraagheader kosten beter wilt begrijpen hoe elke bewerking aanvraag eenheid capaciteit verbruikt tijdens het ontwikkelen van uw toepassing.  


## <a name="choice-of-consistency-level-and-throughput"></a>Keuze van het niveau van de consistentie en de doorvoer
De keuze van het standaardniveau voor de consistentie van is invloed op de doorvoer en latentie. Het standaardniveau voor de consistentie kunt u instellen via programmering zowel via de portal Azure. U kunt ook het niveau van de samenhang op basis van per aanvraag negeren. Het niveau van de consistentie is standaard ingesteld op **sessie**monotone leest/schrijft en lees uw garanties schrijven. Consistentie van de sessie is ideaal voor de gebruiker-georiënteerde toepassingen en biedt een ideaal evenwicht tussen consistentie en prestaties-en nadelen.    

Zie voor instructies over het wijzigen van uw niveau van consistentie op de Azure portal, [een DocumentDB-Account beheren](documentdb-manage-account.md#consistency). Of, voor meer informatie over de niveaus samenhang, [consistentie-niveaus gebruiken](documentdb-consistency-levels.md).

## <a name="provisioned-document-storage-and-index-overhead"></a>Ingerichte document opslag- en overhead
DocumentDB ondersteunt het maken van één partitie en het gepartitioneerde collecties. Elke partitie in DocumentDB ondersteunt 10 GB SSD back-opslag. De 10GB van de opslag van documenten bevat de documenten en opslag voor de index. Een collectie DocumentDB is standaard geconfigureerd voor alle documenten automatisch geïndexeerd zonder expliciet een secundaire indices of schema. Op basis van toepassingen met behulp van DocumentDB, is de typische index overhead tussen 2-20%. De indexing-technologie die wordt gebruikt door DocumentDB zorgt ervoor dat ongeacht de waarden van de eigenschappen van de overhead van de index niet meer dan meer dan 80% van de grootte van de documenten met de standaardinstellingen. 

Standaard worden alle documenten door de DocumentDB automatisch geïndexeerd. Echter als u verfijnen van de overhead van de index wilt, kunt u bepaalde documenten op het moment van invoegen of vervangen van een document wordt geïndexeerd, zoals wordt beschreven in [DocumentDB indexing beleid](documentdb-indexing-policies.md)verwijderen. U kunt een DocumentDB-collectie als u wilt uitsluiten van alle documenten in de collectie worden geïndexeerd. U kunt ook een verzameling DocumentDB voor het indexeren selectief bepaalde eigenschappen of paden met jokertekens van JSON-documenten zoals beschreven in de [configuratie van de indexing-beleid van een collectie](documentdb-indexing-policies.md#configuring-the-indexing-policy-of-a-collection). Met uitzondering van de eigenschappen of documenten verbetert tevens de schrijfbewerkingen – wat betekent dat u minder eenheden van de aanvraag zal verbruiken.   

## <a name="next-steps"></a>Volgende stappen

Zie [partitionering en schaling in Azure DocumentDB](documentdb-partition-data.md)om door te gaan met meer informatie over de werking van DocumentDB.

Zie voor instructies over het controleren van prestaties op de Azure portal [Monitor een DocumentDB account](documentdb-monitor-accounts.md). Zie voor meer informatie over het kiezen van prestaties voor verzamelingen [prestatieniveaus in DocumentDB](documentdb-performance-levels.md).
 
