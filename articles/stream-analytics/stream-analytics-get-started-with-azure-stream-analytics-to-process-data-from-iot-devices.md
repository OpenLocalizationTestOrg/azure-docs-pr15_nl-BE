<properties
    pageTitle="Aan de slag met Azure Stream Analytics om gegevens te verwerken van IoT-apparaten. | Microsoft Azure"
    description="IoT sensor codes en gegevens gegevensstromen met stream analytics en real-time gegevens verwerken"
    keywords="IOT-oplossing aan de slag met iot"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="10/19/2016"
    ms.author="jeffstok"
/>

# <a name="get-started-with-azure-stream-analytics-to-process-data-from-iot-devices"></a>Aan de slag met Azure Stream Analytics om gegevens te verwerken van IoT-apparaten

In deze zelfstudie leert u het maken van stream processing logica om gegevens te verzamelen van Internet dingen (IoT) apparaten. We gebruiken een gebruiksvoorbeeld real-world Internet dingen (IoT) om aan te tonen hoe uw oplossing snel en economisch.

## <a name="prerequisites"></a>Vereisten

-   [Azure-abonnement](https://azure.microsoft.com/pricing/free-trial/)
-   Query- en voorbeeldbestanden downloaden van [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot)

## <a name="scenario"></a>Scenario

Contoso, een bedrijf in de industriële automatisering-ruimte is, is het productieproces volledig geautomatiseerd. De machines in dit bedrijf heeft sensoren die kunnen stromen van gegevens in real-time uitzenden. In dit scenario wordt wil een productiemanager vloer real-time inzicht van de sensorgegevens om te zoeken naar patronen en deze acties ondernemen. We vinden interessante patronen uit de binnenkomende gegevensstroom Stream Analytics Query Language (SAQL) gebruiken via de sensorgegevens.

Hier wordt gegevens van een apparaat van Texas Instruments sensor-code gegenereerd.

![Texas Instruments sensor tag](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-01.jpg)

De nettolading van de gegevens in JSON-indeling en ziet er als volgt uit:


    {
        "time": "2016-01-26T20:47:53.0000000",  
        "dspl": "sensorE",  
        "temp": 123,  
        "hmdt": 34  
    }  

U kunt honderden die gebeurtenissen genereren als een stroom sensoren hebben in een scenario werkelijkheid. In het ideale geval zou een gateway-apparaat code om deze gebeurtenissen push naar [Azure gebeurtenis Hubs](https://azure.microsoft.com/services/event-hubs/) of [Azure IoT Hubs](https://azure.microsoft.com/services/iot-hub/)uitvoeren. Uw taak Analytics stroom zou nemen deze gebeurtenissen van gebeurtenis Hubs en real-time analytics query's uitvoeren tegen de streams. U kan de resultaten vervolgens verzenden naar een van de [uitgangen ondersteund](stream-analytics-define-outputs.md).

Voor gebruiksgemak biedt deze handleiding voor verkrijgen een voorbeeldgegevensbestand uit echte sensor tag apparaten is vastgelegd. U kunt query's uitvoeren op de voorbeeldgegevens en resultaten te zien. In latere cursussen, u leert hoe u de taak verbinding met invoer en uitvoer en ze implementeren met de Azure service.

## <a name="create-a-stream-analytics-job"></a>Maak een taak Stream Analytics

1. In [Azure portal](http://portal.azure.com), klikt u op het plusteken (+) en typ vervolgens **ANALYTICS stroom** in het tekstvenster naar rechts. Selecteer vervolgens **Analytics Stream-project** in de lijst met resultaten.

    ![Een nieuwe gegevensstroom Analytics taak maken](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)

2. Geef een unieke naam en controleer of dat het abonnement is de juiste is voor uw project. Vervolgens een nieuwe resourcegroep maken of Selecteer een bestaande op uw abonnement.

3. Selecteer een locatie voor uw project. Snelheid van verwerking en vermindering van de kosten in de gegevens van wordt dezelfde locatie als de resourcegroep en de beoogde opslag account selecteren overdracht aanbevolen.

    ![Details voor een nieuwe stroom Analytics-taak maken](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.png)

    > [AZURE.NOTE] U moet deze account opslag slechts eenmaal per regio. Deze opslag worden gedeeld in alle stroom Analytics-projecten die zijn gemaakt in die regio.

4. Schakel het selectievakje in als u wilt uw werk op uw dashboard te plaatsen en klik vervolgens op **maken**.

    ![het maken van taken in uitvoering](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03a.png)

5. Er is een 'inzet gestart...' weergegeven in de bovenste rechts van het browservenster. Binnenkort wordt gewijzigd naar een voltooide venster zoals hieronder wordt weergegeven.

    ![het maken van taken in uitvoering](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03b.png)

### <a name="create-an-azure-stream-analytics-query"></a>Maak een query Azure Stream Analytics

Nadat de taak is gemaakt dan wordt het tijd om te openen en een query opbouwen. U kunt uw werk eenvoudig openen door te klikken op de tegel voor.

![Taak tegel](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-04.png)

Klik in het vak **QUERY** om te gaan met Query Editor in het deelvenster **Project topologie** . **QUERY** editor kunt u een T-SQL-query die voert de transformatie uit via de binnenkomende gegevens in te voeren.

![Query in](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

### <a name="query-archive-your-raw-data"></a>Query: De ruwe gegevens archiveren

De eenvoudigste vorm van query is een Pass Through-query die alle ingevoerde gegevens op de uitvoer van de aangewezen archieven. Het voorbeeldgegevensbestand downloaden van [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot) naar een locatie op uw computer. 

1. Plak de query uit het bestand PassThrough.txt. 

    ![Test invoerstroom](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06.png)

2. Klik op de drie puntjes naast uw invoer en schakel in **Voorbeeldgegevens uit bestand uploaden** .

    ![Test invoerstroom](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06a.png)

3. Er wordt een deelvenster geopend aan de rechterkant als resultaat, in het bestand HelloWorldASA-InputStream.json van uw gedownloade locatie selecteren en klik op **OK** onder aan het deelvenster.

    ![Test invoerstroom](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06b.png)

4. Klik op de gear **testen** in het bovenste gedeelte van het venster naar links en verwerken van uw testquery tegen de dataset monster. Een venster wordt geopend onder de query de verwerking is voltooid.

    ![Testresultaten](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

### <a name="query-filter-the-data-based-on-a-condition"></a>Query: De gegevens filteren op basis van een voorwaarde

We proberen de resultaten op basis van een voorwaarde. We willen graag Toon resultaten voor de gebeurtenissen die afkomstig zijn van "sensorA". De query is in het bestand Filtering.txt.

![Een gegevensstroom filteren](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

Houd er rekening mee dat de hoofdlettergevoelige query wordt een string-waarde vergeleken. Klik op de versnelling van de **Test** opnieuw uit te voeren van de query. De query moet retourneren 389 rijen uit 1860 gebeurtenissen.

![Tweede resultaat van de query testen](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-09.png)

### <a name="query-alert-to-trigger-a-business-workflow"></a>Query: Waarschuwing voor het starten van een zakelijke werkstroom

We maken onze query meer. Voor elk type sensor willen we gemiddelde temperatuur per 30 seconden venster controleren en weergeven van resultaten als de gemiddelde temperatuur boven de 100 graden. We schrijven de volgende query en klik vervolgens op **Test** om de resultaten te bekijken. De query is in het bestand ThresholdAlerting.txt.

![30 seconden filterquery](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

U ziet nu de resultaten bevatten alleen 245 rijen en namen van sensoren waarbij de gemiddelde gematigde klimaatzone groter is dan 100. Deze query worden de stroom van gebeurtenissen door **dspl**, die de naam van de sensor via een **Venster Tumbling** van 30 seconden is. Tijdelijke query's moeten worden vermeld hoe willen we tijd aan de vooruitgang. Met behulp van de **TIMESTAMP-BY** -component, hebben we de **OUTPUTTIME** kolom om te keren koppelen aan alle tijdelijke berekeningen opgegeven. Lees de MSDN-artikelen over [Time Management](https://msdn.microsoft.com/library/azure/mt582045.aspx) en [Windowing functies](https://msdn.microsoft.com/library/azure/dn835019.aspx)voor gedetailleerde informatie.

### <a name="query-detect-absence-of-events"></a>Query: Afwezigheid van gebeurtenissen detecteren

Hoe kunnen we schrijven een query naar een gebrek aan invoer gebeurtenissen vinden? We vinden de laatste keer dat een sensor gegevens verzonden en vervolgens niet heeft verzonden gebeurtenissen voor de volgende minuut. De query is in het bestand AbsenseOfEvent.txt.

![Afwezigheid van gebeurtenissen detecteren](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

Hier gebruiken we een **LEFT OUTER** join aan de dezelfde gegevensstroom (self-join). Voor een **INNER** join, wordt een resultaat alleen geretourneerd als een overeenkomst wordt gevonden.  Een **LEFT OUTER** join als u een gebeurtenis uit de linkerzijde van de join niet-gerelateerde records, wordt een rij met NULL als de kolommen aan de rechterkant van de geretourneerd. Deze techniek is handig om een afwezigheid van gebeurtenissen te vinden. Zie de MSDN-documentatie voor meer informatie over [lid worden](https://msdn.microsoft.com/library/azure/dn835026.aspx).

## <a name="conclusion"></a>Conclusie

Het doel van deze zelfstudie is om aan te tonen hoe verschillende Stream Analytics Query Language-query's te schrijven en bekijk de resultaten in de browser. Echter, dit is alleen aan de slag. U kunt zoveel meer met stroom Analytics doen. Stream Analytics ondersteunt een groot aantal ingangen en wordt de uitvoer en kunt zelfs functies gebruiken in Azure Machine Learning zodat het een krachtig hulpmiddel voor het analyseren van gegevensstromen. U kunt beginnen met het verkennen meer over Analytics stroom met behulp van onze [kaart leren](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/). Lees het artikel over [algemene query-patronen](./stream-analytics-stream-analytics-query-patterns.md)voor meer informatie over het schrijven van query's.
