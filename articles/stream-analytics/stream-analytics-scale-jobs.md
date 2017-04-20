<properties
    pageTitle="Stream Analytics taken hogere doorvoersnelheid schalen | Microsoft Azure"
    description="Informatie over het schalen Stream Analytics taken door invoer partities configureren, aanpassing van de definitie van de query en het instellen van taak streaming eenheden."
    keywords="gegevens streaming, afstemmen streaming gegevensverwerking analytics"
    services="stream-analytics"
    documentationCenter=""
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

# <a name="scale-azure-stream-analytics-jobs-to-increase-stream-data-processing-throughput"></a>Azure Stream Analytics taken waardoor de stroom gegevens verwerken doorvoercapaciteit schalen

Informatie over het afstemmen van analytics taken en *streaming eenheden* berekenen voor Stream Analytics, Analytics Stream taken schaalaanpassing door invoer partities configureren, de querydefinitie analytics afstemmen en streaming eenheden taak. 

## <a name="what-are-the-parts-of-a-stream-analytics-job"></a>Wat zijn de onderdelen van een taak Stream Analytics?
Een taakdefinitie Stream Analytics bevat inputs, een query- en uitvoer. "Inputs" zijn van waar de taak wordt de gegevensstroom wordt gelezen, de query wordt gebruikt om te zetten van de gegevensstroom van de invoer en de uitvoer is waar de taak stuurt de resultaten van de taak in.  

Een taak moet ten minste één invoerbron voor gegevensstromen. De stream-invoerbron gegevens kan worden opgeslagen in een Hub Azure Service Bus gebeurtenis of Azure Blob-opslag. Zie [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md) en [aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)voor meer informatie.

## <a name="configuring-streaming-units"></a>Streaming eenheden configureren
Streaming-eenheden (SUs) vertegenwoordigen de middelen en rekenkracht nodig voor het uitvoeren van een taak Azure Stream Analytics. SUs biedt een manier om te beschrijven de relatieve gebeurtenis verwerken op basis van een gemengde eenheid van CPU, geheugen, capaciteit en lezen en schrijven van tarieven. Elke eenheid streaming komt overeen met ongeveer 1MB doorvoer per seconde. 

Kiezen hoeveel SUs vereist zijn voor een bepaald project afhankelijk van de partitieconfiguratie voor de invoer en de query is gedefinieerd voor het project is. U kunt tot de quota in streaming eenheden voor een taak met de klassieke Azure-Portal. Elk abonnement Azure standaard heeft een contingent van maximaal 50 eenheden van streaming voor alle analytics taken in een bepaalde regio. Neem contact op met [Microsoft Support](http://support.microsoft.com)zodat streaming eenheden voor uw abonnementen.

Het aantal streaming eenheden die van een taak gebruikmaken, is afhankelijk van de partitieconfiguratie voor de invoer en de query is gedefinieerd voor het project. Let ook op dat een geldige waarde voor de stream-eenheden moet worden gebruikt. De geldige waarden begint bij 1, 3, 6 en vervolgens naar boven in stappen van 6, zoals hieronder wordt weergegeven.

![Analytics Azure Stream streamen eenheden schaal][img.stream.analytics.streaming.units.scale]

In dit artikel wordt beschreven hoe u berekenen en afstemmen van de query waardoor de doorvoercapaciteit voor analytics taken.

## <a name="embarrassingly-parallel-job"></a>Taken embarrassingly parallel
De embarrassingly parallelle taak is het meest schaalbare scenario we in Azure Stream Analytics hebben. Het maakt verbinding met één partitie van de invoer van één exemplaar van de query een partitie van de uitvoer. Dit parallellisme te bereiken zijn een paar dingen nodig:

1.  Als uw query logica afhankelijk van dezelfde sleutel door hetzelfde exemplaar van de query wordt verwerkt is, vervolgens moet u ervoor zorgen dat de gebeurtenissen gaan naar dezelfde partitie van uw invoer. Dit betekent dat gegevens van de gebeurtenis moet hebben **PartitionKey** instellen of u afzenders van gepartitioneerde kunt voor gebeurtenis-Hubs. Voor Blob betekent dit dat de gebeurtenissen in dezelfde partitie worden verzonden. Als uw query logica niet nodig dezelfde sleutel worden verwerkt door het exemplaar met dezelfde query, kunt u deze vereiste negeren. Een voorbeeld hiervan is een eenvoudige select/Projectfilter query.  
2.  Nadat de gegevens is de lay-out zoals nodig is aan de kant van de invoer, moeten we ervoor zorgen dat uw query is gepartitioneerd. Dit is vereist voor de **Partitie** in alle stappen. Meerdere stappen zijn toegestaan, maar ze allemaal door dezelfde sleutel worden gepartitioneerd. Een ander ding om te weten is dat, op dit moment de partitionering sleutel worden ingesteld op **PartitionId moet** om een volledig parallelle baan.  
3.  Op dit moment ondersteunen alleen gebeurtenis Hubs en Blob gepartitioneerde uitvoer. Voor uitvoer van de gebeurtenis Hubs moet u het veld **PartitionKey** **PartitionId**configureren. Voor Blob hebt u geen te doen.  
4.  Een ander ding om te weten, het aantal partities van invoer moet gelijk zijn aan het aantal partities van de uitvoer. BLOB uitvoer ondersteunt momenteel geen partities, maar dit is geen probleem omdat het partitieschema van de upstream-query worden overgenomen. Voorbeelden van partitie-waarden waarmee een taak volledig parallel:  
    1.  8 gebeurtenis Hubs invoer partities en 8 gebeurtenis Hubs uitvoer partities
    2.  8 gebeurtenis Hubs invoer partities en Blob-uitvoer  
    3.  8 input blob-partities en Blob-uitvoer  
    4.  8 input blob-partities en 8 gebeurtenis Hubs uitvoer partities  

Hier vindt u enkele voorbeeldscenario's die embarrassingly parallel zijn.

### <a name="simple-query"></a>Eenvoudige query
– Gebeurtenis Hubs met 8 partities uitvoer – invoer gebeurtenis Hub met 8 partities

**Query:**

    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100

Deze query is een eenvoudig filter en als zodanig, hoeven we geen zorgen te maken over het partitioneren van de invoer die wij naar de gebeurtenis Hubs verzenden. Ziet u dat de query **Partitie door** van **PartitionId**, heeft zodat we #2 hierboven behoefte. Voor de uitvoer moet de gebeurtenis Hubs uitvoer configureren in de taak aan het **PartitionKey** -veld ingesteld op **PartitionId**. Een laatste controle, invoer partities == uitvoer partities. Deze topologie is embarrassingly parallel.

### <a name="query-with-grouping-key"></a>Query met groeperingssleutel
Blob-gebeurtenis Hubs met 8 partities uitvoer – invoer

**Query:**

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Deze query heeft een groeperingssleutel en als zodanig dezelfde sleutel moet worden verwerkt door het exemplaar met dezelfde query. Dit betekent dat we moeten onze gebeurtenissen verzenden naar gebeurtenissen Hubs op een gepartitioneerde wijze. Welke toets Wij hechten? **Id van partitie** is een logische taak concept, is de echte sleutel die is belangrijk voor ons **TollBoothId**. Dit betekent dat we moeten de **PartitionKey** van de gebeurtenisgegevens we sturen met Hubs gebeurtenis worden de **TollBoothId** van de gebeurtenis. De query heeft **Partitie door** van **PartitionId**, dus we er goed zijn. Voor de uitvoer van de Blob, omdat hoeft we niet te hoeven maken over het configureren van **PartitionKey**. Vereiste #4, nogmaals, is dit Blob, zodat we niet hoeven te hoeven maken over. Deze topologie is embarrassingly parallel.

### <a name="multi-step-query-with-grouping-key"></a>Query met groeperingssleutel stap multi ###
Gebeurtenis Hub met 8 partities Input-Output-gebeurtenis Hub met 8 partities

**Query:**

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Deze query heeft een groeperingssleutel en als zodanig dezelfde sleutel moet worden verwerkt door het exemplaar met dezelfde query. We kunnen de dezelfde strategie als de vorige query gebruiken. De query bevat meerdere stappen. Heeft elke stap een **Partitie door** van **PartitionId**? Ja, zodat we goed zijn. Voor de uitvoer, moeten we de **PartitionKey** ingesteld op **PartitionId** zoals hierboven besproken en ook kunnen we zien er hetzelfde aantal partities als invoer. Deze topologie is embarrassingly parallel.


## <a name="example-scenarios-that-are-not-embarrassingly-parallel"></a>Voorbeeld van de scenario's die niet zijn embarrassingly parallel

### <a name="mismatched-partition-count"></a>Aantal niet-overeenkomende partities ###
– Gebeurtenis Hubs met 8 partities uitvoer – invoer gebeurtenis Hub met 32 partities

Het maakt niet uit wat de query is in dit geval omdat de invoer het aantal partities! = aantal output-partities.

### <a name="not-using-event-hubs-or-blobs-as-output"></a>Niet met behulp van gebeurtenis Hubs of BLOB's als uitvoer
PowerBI-gebeurtenis Hubs met 8 partities uitvoer – invoer

PowerBI uitvoer ondersteunt op dit moment niet partitioneren.

### <a name="multi-step-query-with-different-partition-by-values"></a>Multi stap Query met verschillende waarden partitie door
Gebeurtenis Hub met 8 partities Input-Output-gebeurtenis Hub met 8 partities

**Query:**

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId
    
Zoals u ziet, wordt de tweede stap **TollBoothId** gebruikt als de partitionering sleutel. Dit is niet hetzelfde als de eerste stap en daarom moet u ons een willekeurige volgorde doen. 

Dit zijn enkele voorbeelden en counterexamples van Stream Analytics taken die kunnen bereiken een embarrassingly parallelle topologie en daarmee de kans op maximumschaal. Voor taken die niet voldoen aan één van deze profielen, wordt toekomstige updates bewaartemperatuur schaalaanpassing van sommige van de andere canonieke Stream Analytics scenario's waarin gesteld.

Voor nu het gebruik van de volgende algemene richtlijnen:

## <a name="calculate-the-maximum-streaming-units-of-a-job"></a>Het maximum aantal eenheden van een taak streaming berekenen
Het totaal aantal stroomsgewijze eenheden die kunnen worden gebruikt door een stroom Analytics taak is afhankelijk van het aantal stappen in de query is gedefinieerd voor het project en het aantal partities voor elke stap.

### <a name="steps-in-a-query"></a>Stappen in een query
Een query kan één of meerdere stappen hebben. Elke stap is een onderliggende query gedefinieerd met het sleutelwoord **WITH** . De enige query buiten het sleutelwoord **WITH** wordt ook geteld als een stap, bijvoorbeeld de **SELECT** -instructie in de volgende query:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId

De vorige query bestaat uit twee stappen.

> [AZURE.NOTE] Deze voorbeeldquery worden later in dit artikel beschreven.

### <a name="partition-a-step"></a>Partitie een stap

Partitionering van een stap, moet de volgende voorwaarden:

- De invoerbron worden gepartitioneerd. Zie de [Gebeurtenis Hubs Programming Guide](../event-hubs/event-hubs-programming-guide.md)voor meer informatie.
- De **SELECT** -instructie van de query moet lezen vanaf een gepartitioneerde invoerbron.
- De query in de stap moet de **Partitie door** trefwoord

Wanneer een query is gepartitioneerd, de input gebeurtenissen worden verwerkt en samengevoegd in afzonderlijke partitiegroepen en uitgangen gebeurtenissen worden gegenereerd voor elk van de groepen. Als een gecombineerde aggregaat wenselijk is, moet u een tweede niet-gepartitioneerde stap maken om samen te voegen.

### <a name="calculate-the-max-streaming-units-for-a-job"></a>Berekenen van de maximale streaming eenheden voor een taak

Alle niet-gepartitioneerde stappen kunnen samen maximaal zes streaming eenheden voor een taak Stream Analytics schalen. Om toe te voegen extra eenheden, streaming een stap worden gepartitioneerd. Elke partitie kan zes streaming eenheden hebben.

<table border="1">
<tr><th>Query van een taak</th><th>Maximum aantal eenheden voor de taak streaming</th></td>

<tr><td>
<ul>
<li>De query bevat één stap.</li>
<li>De stap is niet gepartitioneerd.</li>
</ul>
</td>
<td>6</td></tr>

<tr><td>
<ul>
<li>De stroom van gegevens die zijn ingevoerd is door 3 gepartitioneerd.</li>
<li>De query bevat één stap.</li>
<li>De stap is gepartitioneerd.</li>
</ul>
</td>
<td>18</td></tr>

<tr><td>
<ul>
<li>De query bevat twee stappen.</li>
<li>Geen van de stappen is gepartitioneerd.</li>
</ul>
</td>
<td>6</td></tr>



<tr><td>
<ul>
<li>De stream gegevensinvoer is door 3 gepartitioneerd.</li>
<li>De query bevat twee stappen. De invoer stap is gepartitioneerd en de tweede stap is het niet.</li>
<li>De SELECT-instructie van de gepartitioneerde invoer wordt gelezen.</li>
</ul>
</td>
<td>24 (18 voor de gepartitioneerde stappen) + 6 voor de niet-gepartitioneerde stappen</td></tr>
</table>

### <a name="examples-of-scale"></a>Voorbeelden van schaal
De volgende query berekent het aantal auto's gaat via een gratis station met drie tollbooths in een venster met drie minuten. Deze query kan geschaald worden uitgebreid tot zes streaming eenheden.

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Meer streaming eenheden gebruikt voor de query, zowel de data stream input en de query moet worden gepartitioneerd. Gezien het feit dat de stroom data partitie is ingesteld op 3, kan de volgende gewijzigde query worden vergroot tot 18 streaming eenheden:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Wanneer een query is gepartitioneerd, zijn de invoer gebeurtenissen verwerkt en samengevoegd in groepen afzonderlijke partitie. Uitvoer-gebeurtenissen worden ook gegenereerd voor elk van de groepen. Partities kan sommige onverwachte resultaten opleveren wanneer u het veld **groeperen op** is niet de sleutel van de partitie in de input data stream. Het veld **TollBoothId** in de vorige voorbeeldquery is bijvoorbeeld geen sleutel partitie van Input1. De gegevens uit het stencil #1 kan worden verdeeld in meerdere partities.

Alle partities Input1 afzonderlijk zal worden verwerkt door de stroom Analytics en meerdere records van het aantal auto-pass-door middel van voor de dezelfde tolhuisje in hetzelfde venster gewor worden gemaakt. Als de partitiesleutel invoer kan niet worden gewijzigd, kunt u dit probleem opgelost door bijvoorbeeld toe te voegen een extra stap van de niet-partitie:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Deze query kan geschaald worden uitgebreid tot en met 24 eenheden streaming.

>[AZURE.NOTE] Als u twee stromen, ervoor te zorgen dat de stromen door de partitiesleutel van de kolom dat u joins doen en u hetzelfde aantal partities in beide stromen worden gepartitioneerd.


## <a name="configure-stream-analytics-job-partition"></a>Stream Analytics taak configureren

**Een streaming-eenheid voor een project aanpassen**

1. Log in om de [portal beheren](https://manage.windowsazure.com).
2. Klik op **Stream Analytics** in het linkerdeelvenster.
3. Klik op de Stream Analytics taak die u wilt schalen.
4. Klik op **schaal** boven aan de pagina.

![Analytics Azure Stream streamen eenheden schaal][img.stream.analytics.streaming.units.scale]

In de Portal Azure schaal kunt u deze instellingen:

![Configuratie van Portal-Stream Analytics Azure][img.stream.analytics.preview.portal.settings.scale]

## <a name="monitor-job-performance"></a>Prestaties van de taak controleren

Met behulp van de portal beheren, kunt u volgen de doorvoer van een taak in gebeurtenissen per seconde:

![Azure Stream Analytics taken controleren][img.stream.analytics.monitor.job]

Bereken de verwachte doorvoer van de werkbelasting in gebeurtenissen per seconde. Als de doorvoersnelheid minder dan verwacht, de invoer partitie afstemmen afstemmen van de query en extra streaming eenheden toevoegen aan uw project.

## <a name="stream-analytics-throughput-at-scale---raspberry-pi-scenario"></a>Stream Analytics doorvoer bij schaal - Pi frambozen scenario


Om te begrijpen hoe stream analytics taken schalen in een typisch scenario voor verwerking van doorvoer over meerdere Streaming, is hier een experiment dat sensorgegevens (clients) gebeurtenis hub verzendt, verwerkt deze en verzendt waarschuwing of statistieken als een uitvoer naar een andere gebeurtenis Hub.

De client is kunstmatige sensorgegevens verzenden naar gebeurtenis Hubs in JSON-Stream Analytics indeling en de gegevensuitvoer ook in JSON-indeling.  De voorbeeldgegevens uitzien zou zoals:  

    {"devicetime":"2014-12-11T02:24:56.8850110Z","hmdt":42.7,"temp":72.6,"prss":98187.75,"lght":0.38,"dspl":"R-PI Olivier's Office"}

Query: ' een waarschuwing verzonden wanneer het licht wordt uitgeschakeld"  

    SELECT AVG(lght),
     “LightOff” as AlertText
    FROM input TIMESTAMP
    BY devicetime
     WHERE
        lght< 0.05 GROUP BY TumblingWindow(second, 1)

Meten van doorvoer: De doorvoer in deze context is de hoeveelheid invoergegevens verwerkt door Analytics Stream in een vaste hoeveelheid tijd (10 minuten). Voor beste verwerking doorvoer voor de ingevoerde gegevens, zowel de data stream input en de query moet worden gepartitioneerd. **COUNT()**is ook opgenomen in de query om te meten hoe veel input gebeurtenissen zijn verwerkt. Elke partitie van de invoer gebeurtenis Hub is om dat de taak niet gewoon wacht op invoer gebeurtenissen te komen, vooraf geladen met voldoende invoergegevens (ongeveer 300MB).  

Hieronder vindt u de resultaten met het toenemende aantal eenheden voor Streaming en bijbehorende partitie geteld in de gebeurtenis Hubs.  

<table border="1">
<tr><th>Invoer-partities</th><th>Uitvoer-partities</th><th>Streaming-eenheden</th><th>Aanhoudende doorvoer
</th></td>

<tr><td>12</td>
<td>12</td>
<td>6</td>
<td>4.06 MB/s</td>
</tr>

<tr><td>12</td>
<td>12</td>
<td>12</td>
<td>8.06 MB/s</td>
</tr>

<tr><td>48</td>
<td>48</td>
<td>48</td>
<td>38.32 MB/s</td>
</tr>

<tr><td>192</td>
<td>192</td>
<td>192</td>
<td>172.67 MB/s</td>
</tr>

<tr><td>480</td>
<td>480</td>
<td>480</td>
<td>454.27 MB/s</td>
</tr>

<tr><td>720</td>
<td>720</td>
<td>720</td>
<td>609.69 MB/s</td>
</tr>
</table>

![IMG.Stream.Analytics.perfgraph][img.stream.analytics.perfgraph]

## <a name="get-help"></a>Help-informatie opvragen
Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)voor verdere ondersteuning.


## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Analytics Azure Stream](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)


<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings.png

<!--Link references-->

[microsoft.support]: http://support.microsoft.com
[azure.management.portal]: http://manage.windowsazure.com
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
 
