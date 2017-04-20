<properties 
    pageTitle="Inleiding tot Analytics Stream | Microsoft Azure" 
    description="Meer informatie over de Stream Analytics, een beheerde service helpt u bij het analyseren van het Internet van dingen (IoT) stroomsgewijs verzenden van gegevens in real-time." 
    keywords="Analytics beheerd als een service, services, stream processing, streaming analytics, wat is stream analytics"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>


# <a name="what-is-stream-analytics"></a>Wat is Stream Analytics?

Azure Stream Analytics is een volledig beheerde, kostenbesparende real-time gebeurtenis verwerkings-engine waarmee diepe inzichten gegevens ontgrendelen. Stream Analytics gemakkelijk om real-time analytische berekeningen op streaming vanaf apparaten, sensoren, websites, sociale media, toepassingen,, infrastructuursystemen en meer gegevens in te stellen.

Met een paar klikken in de portal Azure een Stream Analytics taak geven de invoerbron van de streaming-gegevens, de sink uitvoer voor de resultaten van uw project te kunnen ontwerpen en een gegevenstransformatie uitgedrukt in een SQL-achtige taal. U kunt controleren en aanpassen van de schaal/snelheid van uw werk in de Azure portal voor het schalen van een paar kilobytes aan een gigabyte of meer van de gebeurtenissen dat per seconde wordt verwerkt.

Stream Analytics maakt gebruik van jaren werk bij het ontwikkelen van Microsoft Research sterk afgestemd streaming motoren voor tijdgevoelige verwerking, alsmede integratie van intuïtieve specificaties van deze taal.

## <a name="what-can-i-use-stream-analytics-for"></a>Wat kan ik gebruiken Analytics voor stroom?
Grote hoeveelheden gegevens zijn die met hoge snelheid over het netwerk vandaag. Organisaties die kunnen verwerken en reageren op deze stroomsgewijs verzenden van gegevens in real-time kunnen aanzienlijk efficiëntie te verhogen en zich te onderscheiden in de markt. Scenario's van real-time streaming analytics kunnen worden gevonden in alle industrieën: persoonlijke, real-time analyse van aandelen handel en waarschuwingen die worden aangeboden door bedrijven financiële diensten; real-time fraude-opsporing; diensten van de bescherming van gegevens en identiteiten; betrouwbare via ingestie en analyse van gegevens die zijn gegenereerd door de sensoren en actuatoren ingesloten in fysieke objecten (Internet van dingen of IoT); Web clickstream-analyse; en de toepassingen van de klant relatie management (CRM) afgifte waarschuwingen wanneer de ervaring van de klant in een periode is verslechterd. Bedrijven zijn op zoek naar de meest flexibele, betrouwbare en voordelige manier een dergelijke analyse gebeurtenis-stream real-time gegevens te slagen in het uiterst competitieve moderne zakenwereld.

## <a name="key-capabilities-and-benefits"></a>Belangrijke functies en voordelen
-   **Eenvoudig in gebruik:** Stream Analytics ondersteunt een eenvoudige, declaratieve query-model voor het beschrijven van transformaties. Wilt optimaliseren voor gebruiksgemak, Stream Analytics maakt gebruik van een T-SQL variant en klanten omgaan met de technische complexiteit van verwerkingssystemen stroom moet worden verwijderd. Met behulp van de [Stream Analytics query language](https://msdn.microsoft.com/library/azure/dn834998.aspx) in de browser-query editor, krijgt u intelli-sense automatisch aanvullen kunt u snel en gemakkelijk implementeren tijd reeks query's, joins op tijdelijke basis, vensters aggregaten temporal filters en andere algemene bewerkingen zoals joins, aggregaten, projecties en filters. Bovendien kan in de browser querytests tegen een voorbeeldbestand dat u snel en iteratieve ontwikkeling.  

-   **Schaalbaarheid:** Stream Analytics is kan verwerken gebeurtenis hoge doorvoer van 1GB/sec. Integratie met [Azure gebeurtenis Hubs](https://azure.microsoft.com/services/event-hubs/) en [Azure IoT Hubs](https://azure.microsoft.com/services/iot-hub/) kunnen miljoenen gebeurtenissen per tweede afkomstig zijn van de aangesloten apparaten, clickstreams, nemen de oplossing en logboekbestanden om een paar te noemen. Om dit te bereiken, Stream Analytics borduurt voort op de voorziening voor partitionering van gebeurtenis Hubs, waarvan 1 MB/s per partitie kan worden verkregen. Gebruikers kunnen de berekening in een aantal logische stappen binnen de querydefinitie te partitioneren, elk met de mogelijkheid om verder worden gepartitioneerd als schaalbaarheid te vergroten.  

-   **Betrouwbaarheid, herhaalbaarheid en snel herstel:** Een beheerde service in de cloud, Analytics Stream, voorkomt u gegevensverlies en bedrijfscontinuïteit in geval van storingen tot en met de ingebouwde mogelijkheden biedt. De service biedt de mogelijkheid tot intern herhaalbare resultaten te waarborgen is het mogelijk om te archiveren van gebeurtenissen en het opnieuw verwerken in de toekomst, altijd dezelfde resultaten ophalen. Hierdoor kunnen klanten terug in de tijd en onderzoekt u berekeningen bij het uitvoeren van de onderliggende oorzaak analyseren, wat-als-analyse, enz.  

-   **Lage kosten:** Stream Analytics is als een cloud-service, zodat gebruikers een erg lage kosten om aan de slag en real-time analytics oplossingen onderhouden geoptimaliseerd. De service is voor u omslagstelsel op basis van het gebruik van Streaming-eenheid en de hoeveelheid gegevens die worden verwerkt door het systeem gebouwd. Gebruik is op basis van het aantal gebeurtenissen verwerkt en het bedrag van rekenkracht ingericht binnen het cluster voor het verwerken van de respectieve taken van de Stream Analytics afgeleid.  

-   **Verwijzen naar gegevens:** Stream Analytics biedt gebruikers de mogelijkheid geven en referentiegegevens gebruiken. Dit kan zijn historische gegevens of gewoon niet-streaming die minder vaak na verloop van tijd verandert. Het systeem vereenvoudigt het gebruik van referentiegegevens worden behandeld als andere binnenkomende gebeurtenis stream deelnemen aan met andere gebeurtenis streams in real-time transformaties uitvoeren ingenomen.  

-   **Door de gebruiker gedefinieerde functies:** Stream Analytics heeft integratie met Azure Machine Learning functieaanroepen in de Machine Learning-service als onderdeel van een Stream Analytics query definiëren. Uitbreiding van de mogelijkheden van de Stream Analytics gebruikmaken van bestaande Azure Machine Learning solutions. Raadpleeg de [zelfstudie van Machine Learning-integratie](stream-analytics-machine-learning-integration-tutorial.md)voor meer informatie over dit.

-   **Verbinding:** Stream Analytics maakt rechtstreeks verbinding met Azure gebeurtenis Hubs en Azure IoT Hubs voor ingestie van stroom en de service Azure Blob consumptie van historische gegevens. Resultaten kunnen van de Stream Analytics naar Azure Storage Blobs of tabellen, Azure SQL DB-gegevensarchieven Lake Azure, DocumentDB, gebeurtenis Hubs, Azure Service Bus onderwerpen of wachtrijen en Power BI, waar kan vervolgens weergegeven, verder verwerkt door werkstromen, gebruikt bij de analyse van de batch via [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) of opnieuw worden verwerkt als een reeks gebeurtenissen worden geschreven. Bij het gebruik van Hubs gebeurtenis kan meerdere Stream Analytics samen met andere gegevensbronnen en verwerking motoren opstellen zonder verlies van de streaming aard van de berekeningen.  

## <a name="get-help"></a>Help-informatie opvragen
Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) voor verdere ondersteuning

## <a name="next-steps"></a>Volgende stappen
U hebt nu enkele gegevensstroom Analytics, een beheerde service voor het streamen van analytics op gegevens uit de Internet zaken. Voor meer informatie over deze service, Zie:

- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)

