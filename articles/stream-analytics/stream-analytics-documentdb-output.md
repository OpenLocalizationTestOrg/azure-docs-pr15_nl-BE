<properties
    pageTitle="JSON-uitvoer voor Stream Analytics | Microsoft Azure"
    description="Informatie over hoe Stream Analytics Azure DocumentDB voor JSON-uitvoer voor het archiveren van gegevens en lage latentie query's op ongestructureerde JSON-gegevens kunt richten."
    keywords="JSON-uitvoer"
    documentationCenter=""
    services="stream-analytics,documentdb"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="target-azure-documentdb-for-json-output-from-stream-analytics"></a>Doel Azure DocumentDB voor uitvoer uit de Stream Analytics JSON

Stream Analytics kunt [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) richten voor JSON-uitvoer, lage latentie en archiveren query op ongestructureerde JSON-gegevens inschakelt. Informatie over het implementeren van deze integratie best.

Voor degenen die niet bekend met DocumentDB zijn, kijk eens naar [de DocumentDB leren pad](https://azure.microsoft.com/documentation/learning-paths/documentdb/) aan de slag.

## <a name="basics-of-documentdb-as-an-output-target"></a>Basisbeginselen van het DocumentDB als een doel output
De Azure DocumentDB uitvoer in Analytics Stream kunt schrijven de resultaten als JSON uitvoer verwerken in uw collection(s) DocumentDB stream. Stream Analytics maakt geen collecties in de database, in plaats daarvan moeten ze voorhand maken. Dit is zodat de kosten facturering van DocumentDB collecties zijn transparant voor de gebruiker en zodat u de prestaties, de samenhang en de capaciteit van uw collecties via de [DocumentDB-API's](https://msdn.microsoft.com/library/azure/dn781481.aspx)kunt afstemmen. Beste Database voor één DocumentDB per project streaming logische scheiding aanbrengen tussen collecties voor een streaming functie gebruiken.

Enkele van de collectie DocumentDB opties worden hieronder beschreven.

## <a name="tune-consistency-availability-and-latency"></a>Consistentie, beschikbaarheid en latentie afstemmen

Om te voldoen aan uw toepassing, kunt DocumentDB u fijn afstemmen van de Database en de collecties en afweging tussen consistentie, beschikbaarheid en latentie. Afhankelijk van welke mate van consistentie lezen uw behoeften scenario tegen lezen en schrijven latentie, dat kunt u een consistent niveau van uw databaseaccount. Standaard kunnen DocumentDB ook synchrone indexering op elke CRUD bewerking aan de collectie. Dit is een andere handige optie om de prestaties van het schrijven/lezen in DocumentDB. Bekijk het artikel voor [uw database- en consistentie niveaus wijzigen](../documentdb/documentdb-consistency-levels.md) voor meer informatie over dit onderwerp.

## <a name="choose-a-performance-level"></a>Kies een prestatieniveau

DocumentDB collecties kunnen worden gemaakt op 3 verschillende prestatieniveaus (S1, S2 of S3), de doorvoer beschikbaar voor CRUDs vast te stellen voor die collectie. Bovendien is de prestaties beïnvloed door de niveaus indexing/consistentie in uw verzameling. Raadpleeg [dit artikel](../documentdb/documentdb-performance-levels.md) voor informatie over deze prestaties in detail.

## <a name="upserts-from-stream-analytics"></a>Upserts van de Stream Analytics

Stream Analytics integratie met DocumentDB kunt u invoegen of bijwerken van records in de collectie van de DocumentDB op basis van een bepaald Document-ID-kolom. Dit is ook een *Upsert*genoemd.

Stream Analytics maakt gebruik van een optimistische Upsert benadering, waarbij updates worden alleen uitgevoerd wanneer invoegen vanwege een conflict met Document-ID mislukt. Met deze update wordt uitgevoerd door Analytics Stream als een PATCH, zodat u kunt gedeeltelijke updates aan het document, dat wil zeggen de toevoeging van nieuwe eigenschappen of vervangen van die een bestaande eigenschap stapsgewijs wordt uitgevoerd. Houd er rekening mee dat wijzigingen in de waarden van Matrixeigenschappen in het resultaat van de JSON-document in de hele matrix ophalen overschreven, dat wil zeggen de matrix niet is samengevoegd.

## <a name="data-partitioning-in-documentdb"></a>Gegevenspartitionering in DocumentDB

DocumentDB collecties kunnen u uw gegevens op basis van de patronen van de query en de prestatiebehoeften van uw toepassing te partitioneren. Elke collectie kan maximaal 10GB aan gegevens (maximaal) bevatten en er is momenteel geen manier om te vergroten (of overloop) een collectie. Voor het horizontaal schalen, Stream Analytics kunt u schrijven naar meerdere verzamelingen met een bepaald voorvoegsel (Zie hieronder details van gebruik). De consistente [Hash-partitie Resolver](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.partitioning.hashpartitionresolver.aspx) strategie op basis van de gebruiker die de kolom PartitionKey stream Analytics gebruikt om de records van de uitvoer. Het aantal collecties met het opgegeven voorvoegsel aan het begin van de taak van de streaming wordt gebruikt als het aantal uitvoer partities, waarop de taak naar parallel schrijft (verzamelingen DocumentDB = uitvoer partities). Voor een enkele S3 collectie met lazy indexing doen alleen ingevoegd, over 0,4 schrijfbewerkingen MB/s kan worden verwacht. Met behulp van meerdere collecties kunt u een hogere doorvoer en grotere capaciteit.

Als u het aantal partities in de toekomst te verhogen wilt, moet u de taak stoppen, opnieuw partitioneren van de gegevens uit de bestaande collecties in de nieuwe collecties en start de taak Stream Analytics. Meer informatie over het gebruik van PartitionResolver en opnieuw partitioneren met voorbeeldcode, worden opgenomen in een follow-up post. Het artikel van de [partities in de DocumentDB](../articles/documentdb-partition-data.md#developing-a-partitioned-application) ook worden details weergegeven over dit.

## <a name="documentdb-settings-for-json-output"></a>Instellingen voor uitvoer van JSON DocumentDB

DocumentDB maken als een uitvoer in Stream Analytics genereert een prompt voor meer informatie, zoals hieronder wordt weergegeven. Deze sectie bevat een uitleg van de definitie van eigenschappen.

![documentdb stroom analytics uitvoerscherm](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output.png)  

-   **Uitvoeralias** – een alias voor deze uitvoer in uw query ASA verwijzen  
-   **Naam** : de naam of het eindpunt van de URI van de account DocumentDB.  
-   **Account sleutel** : de sleutel gedeelde toegang voor de account DocumentDB.  
-   **Database** – de naam van de DocumentDB database.  
-   **Collectie naampatroon** – het naampatroon collectie voor de collecties worden gebruikt. De indeling van de collectie kan worden samengesteld met behulp van het token optionele {partitie} waar partities beginnen bij 0. Geldige invoer voorbeeld volgen:  
   1\) MyCollection – een collectie met de naam 'MyCollection' moet aanwezig zijn.  
   2\) MyCollection {partitie} – deze collecties moeten bestaan: 'MyCollection0', 'MyCollection1', 'MyCollection2', enzovoort.  
-   **Partitiesleutel** – de naam van het veld in de uitvoer gebeurtenissen gebruikt voor het opgeven van de sleutel voor het partitioneren van uitvoer in collecties. Voor de uitvoer van één collectie elke willekeurige uitvoerkolom kan worden gebruikt, bijvoorbeeld PartitionId.  
-   **Document-ID** : optioneel. De naam van het veld in de uitvoer-gebeurtenissen die worden gebruikt voor de primaire sleutel opgeven op welke invoegen of bijwerken van bewerkingen zijn gebaseerd.  
