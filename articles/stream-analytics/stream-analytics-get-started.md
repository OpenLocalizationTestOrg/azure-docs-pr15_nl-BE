<properties
    pageTitle="Aan de slag met Analytics Stream: Real-time fraude-opsporing | Microsoft Azure"
    description="Informatie over het maken een real-time fraude detectie oplossing met Analytics Stream. Gebruik een hub gebeurtenis voor gebeurtenis real-time verwerking."
    keywords="afwijking detectie, opsporing van fraude, real-time afwijking detectie"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok" />



# <a name="get-started-using-azure-stream-analytics-real-time-fraud-detection"></a>Aan de slag met Azure Stream Analytics: Real-time fraude-opsporing

Informatie over het maken van een end-to-end oplossing voor real-time fraude-opsporing met Azure Stream Analytics. Gebeurtenissen op een gebeurtenis Azure hub brengen, Stream Analytics query's voor aggregatie of waarschuwingen schrijven en verzenden van de resultaten naar een sink uitvoer om inzicht te krijgen over gegevens met real-time verwerking. Real time afwijking detectie voor telecommunicatie valt, maar het voorbeeld van de techniek is even geschikt voor andere soorten fraude-opsporing, zoals creditcard of identity theft-scenario's.

Stream Analytics is een volledig beheerde service met lage latentie, hoge beschikbaarheid, schaalbaar en complexe gebeurtenis verwerken via streaming gegevens in de cloud. Zie [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)voor meer informatie.


## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Scenario: Telecommunicatie en SIM fraude-opsporing in real-time

Een telecommunicatie-bedrijf heeft een grote hoeveelheid gegevens voor binnenkomende gesprekken. Het bedrijf heeft de volgende gegevens nodig:
* Deze gegevens tot een beheersbaar bedrag vergelijken en inzichten over het gebruik van de klant verkrijgen voor tijd en geografische regio's.
* SIM-fraude (meerdere aanroepen die afkomstig zijn van dezelfde identiteit rond dezelfde tijd, maar in geografische vestigingen) in detecteren real-time zodat ze gemakkelijk reageren kunnen door kennisgeving aan klanten of service wordt afgesloten.

In canonieke Internet dingen (IoT) scenario's er een berg gegevens telemetrie of sensor wordt gegenereerd – en klanten willen ze samen te voegen of het signaal via de anomalieën in real-time.

## <a name="prerequisites"></a>Vereisten

- [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) downloaden van het Microsoft Download Center 
- Optioneel: De broncode van de gebeurtenis generator van [GitHub](https://github.com/Azure/azure-stream-analytics/tree/master/DataGenerators/TelcoGenerator)

## <a name="create-an-azure-event-hubs-input-and-consumer-group"></a>Een invoer Azure gebeurtenis Hubs en consument groep maken

De voorbeeldtoepassing worden gebeurtenissen genereren en duw ze naar een exemplaar van de gebeurtenis Hub voor real-time verwerking. Service Bus gebeurtenis Hubs zijn de voorkeur van gebeurtenis ingestiedosiscoëfficiënten voor Stream Analytics en kunt u meer informatie over de gebeurtenis Hubs in [Azure Service Bus documentatie](/documentation/services/service-bus/).

Een Hub gebeurtenis maken:

1.  Klik op **Nieuw**in de [Azure portal](https://manage.windowsazure.com/)  > **App Services** > **Service Bus** > **Gebeurtenis Hub** > **Snelle invoer**. Geef een naam, regio, en nieuwe of bestaande naamruimte voor het maken van een nieuwe gebeurtenis Hub.  
2.  Beste, moet elke taak Analytics stroom van één gebeurtenis Hub consument groep lezen. We begeleidt u stapsgewijs door het proces van het maken van een groep consumenten hieronder en u kunt [meer informatie over groepen consumenten](https://msdn.microsoft.com/library/azure/dn836025.aspx). Een consument als groep wilt maken, gaat u naar de zojuist gemaakte gebeurtenis Hub en klikt u op het tabblad **Groepen consumenten** en vervolgens klikt u op **maken** op de onderkant van de pagina en een naam voor de groep van de consument.
3.  Toegang verlenen tot de Hub gebeurtenis moeten we een beleid voor gedeeld gebruik maken.  Klik op het tabblad **configureren** van uw Hub-gebeurtenis.
4.  Maak een nieuw beleid met de machtigingen **beheren** onder **Gedeelde-beleid**.

    ![Gedeeld-beleid waar u een beleid met de machtigingen beheren maken kunt.](./media/stream-analytics-get-started/stream-ananlytics-shared-access-policies.png)

5.  Klik op **Opslaan** onder aan de pagina.
6.  Ga naar het **Dashboard** en **Verbindingsgegevens** aan de onderkant van de pagina, klikt u op en kopieer en opslaan van gegevens voor de verbinding.

## <a name="configure-and-start-event-generator-application"></a>Configureren en starten van de gebeurtenis generator toepassing

We hebben een clienttoepassing die worden gegenereerd monster binnenkomende oproep metagegevens en druk in de gebeurtenis Hub verstrekt. Volg de onderstaande stappen voor het instellen van deze toepassing.  

1.  Download het [bestand TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip). Deze vervolgens uitpakken naar een map.

    **Opmerking**: Windows blokkeert het gedownloade zip-bestand. Klik met de rechtermuisknop op het bestand en selecteer Eigenschappen. Als het bericht ' dit bestand is afkomstig van een andere computer en mogelijk geblokkeerd om deze computer te beschermen." Vink vervolgens het vak 'Deblokkeren' en klik op toepassen op het zip-bestand.

2.  De waarden Microsoft.ServiceBus.ConnectionString en EventHubName in de **telcodatagen.exe.config** vervangen door de gebeurtenis Hub-verbindingsreeks en de naam.

    **Opmerking**: de verbindingsreeks wordt opgehaald uit de Azure portal plaatsen de naam van de verbinding aan het einde. Verwijder de "; EntityPath =<value>"= veld uit de sleutel toevoegen.

3.  Start de toepassing. De syntaxis is als volgt:

   telcodatagen.exe [#NumCDRsPerHour] [SIM-kaart fraude kans] [#DurationHours]

In het volgende voorbeeld genereert gebeurtenissen 1000 met een kans van 20 procent van de fraude in de loop van 2 uur.

    telcodatagen.exe 1000 .2 2

Records die worden verzonden naar uw Hub-gebeurtenis wordt weergegeven. Hier definieert u sommige belangrijke velden die we in deze toepassing real-time fraude detectie gebruiken:

| Record | Definitie |
| ------------- | ------------- |
| CallrecTime | De tijdstempel voor de begintijd van de oproep. |
| SwitchNum | Telefooncentrale kunt u verbinding maken. |
| CallingNum | Het telefoonnummer van de beller. |
| CallingIMSI | Internationale Mobile Subscriber Identity (IMSI).  De unieke id van de beller. |
| CalledNum | Het telefoonnummer van de ontvanger. |
| CalledIMSI | Internationale Mobile Subscriber Identity (IMSI).  De unieke id van de ontvanger. |


## <a name="create-stream-analytics-job"></a>Stream Analytics taak maken
Nu hebben we een reeks gebeurtenissen telecommunicatie, we kunnen instellen een taak Stream Analytics voor het analyseren van deze gebeurtenissen in real time.

### <a name="provision-a-stream-analytics-job"></a>Een taak Analytics stroom voorziening

1.  In de portal Azure, klikt u op **Nieuw > Data Services > Stream Analytics > Snelle invoer**.
2.  De volgende waarden opgeven en klik vervolgens op **Stroom Analytics taak maken**:

    * **Naam**: Voer de taaknaam van een.

    * **Regio**: Selecteer de regio waar u de taak uit te voeren. Rekening houden met het plaatsen van de taak en de gebeurtenis hub in hetzelfde gebied betere prestaties te garanderen en om ervoor te zorgen dat u niet betaalt voor gegevensoverdracht tussen regio's.

    * **Opslag Account**: Kies de Azure opslag-account die u gebruiken wilt voor het opslaan van de meetgegevens voor alle stroom Analytics taken die worden uitgevoerd binnen deze regio. U hebt de optie Kies een bestaande account voor opslag of een nieuwe maken.

3.  Klik op **Stream Analytics** in het linkerdeelvenster om de Stream Analytics taken.

    ![Pictogram stream Analytics-service](./media/stream-analytics-get-started/stream-analytics-service-icon.png)

4.  Het nieuwe project wordt weergegeven met de status **gemaakt**. U ziet dat de knop **Start** op de onderkant van de pagina is uitgeschakeld. Moet u de taak invoer, uitvoer en query voordat u de taak kunt starten.

### <a name="specify-job-input"></a>Invoer van de taak opgeven
1.  In de Stream Analytics taak klikt u op **"inputs"** vanaf de bovenkant van de pagina en klik op **Invoer toevoegen**. In het dialoogvenster dat wordt geopend kunt u via een aantal stappen voor het instellen van uw invoer.
2.  **Gegevensstroom**selecteren en klik vervolgens op de knop rechts.
3.  **Hub gebeurtenis**selecteren en klik vervolgens op de knop rechts.
4.  Typ of Selecteer de volgende waarden op de derde pagina:

    * **Invoer Alias**: Geef een beschrijvende naam voor deze taak invoeren zoals *CallStream*. Houd er rekening mee dat u deze naam in de query later gebruikt.
    * **Gebeurtenis Hub**: als de gebeurtenis Hub die u hebt gemaakt in het abonnement hetzelfde als de taak van de Stream Analytics, selecteert u de naamruimte waarin de gebeurtenis hub is.

    Als uw hub-gebeurtenis zich in een ander abonnement, selecteert u **Gebeurtenis-Hub voor gebruik van een ander abonnement** en informatie voor **Service Bus Namespace**, **Hub gebeurtenisnaam** **Gebeurtenisnaam Hub beleid**, **Sleutel gebeurtenis Hub beleid**en **Aantal Hub partitie**handmatig invoeren.

    * **Naam van gebeurtenis Hub**: Selecteer de naam van de gebeurtenis Hub.

    * **Gebeurtenis Hub beleidsnaam**: Selecteer de gebeurtenis hub beleid eerder in deze handleiding hebt gemaakt.

    * **Gebeurtenis Hub Consumer Group**: Typ de consument groep eerder in deze handleiding hebt gemaakt.
5.  Klik op de knop rechts.
6.  De volgende waarden opgeven:

    * **Gebeurtenis Serializer indeling**: JSON
    * **Codering**: UTF8
7.  Klik op de knop controleren om toe te voegen deze bron en om te controleren dat stroom Analytics verbinding met de hub van de gebeurtenis maken kunnen.

### <a name="specify-job-query"></a>Project-query opgeven

Stream Analytics ondersteunt een eenvoudige, declaratieve query-model voor het beschrijven van transformaties voor real-time verwerking. Zie voor meer informatie over de taal, de [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/dn834998.aspx). Deze zelfstudie kunt u ontwerpen en testen van meerdere query's via uw real-time gegevensstroom oproep.

#### <a name="optional-sample-input-data"></a>Optioneel: Invoer voorbeeldgegevens
Voor het valideren van uw query op een werkelijke taakgegevens, kunt u de functie **Voorbeeldgegevens** gebeurtenissen ophalen uit uw stroom en maken een. JSON-bestand van de gebeurtenissen voor het testen.  De volgende stappen laten zien hoe om dit te doen en we hebben bovendien een voorbeeldbestand [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json) voor testdoeleinden.

1.  Uw invoer Hub gebeurtenis en klik op **Voorbeeldgegevens** onderaan de pagina.
2.  Geef in het dialoogvenster een **Begintijd** om te beginnen met het verzamelen van gegevens en een **duur** voor hoeveel extra gegevens te verbruiken.
3.  Klik op het selectievakje om te beginnen met gegevens voor de bemonstering van de invoer.  Het kan een paar minuten duren voor het gegevensbestand dat moet worden geproduceerd.  Wanneer het proces is voltooid, klikt u op **Details** en download en sla de. JSON-bestand dat wordt gegenereerd.

    ![Download en verwerkte gegevens opslaan in een bestand JSON](./media/stream-analytics-get-started/stream-analytics-download-save-json-file.png)

#### <a name="passthrough-query"></a>Passthrough-query

Als u wilt dat elke gebeurtenis te archiveren, kunt u een passthrough-query alle velden in de nettolading van de gebeurtenis of het bericht te lezen. Beginnen met, doen een eenvoudige passthrough-query die alle velden in een gebeurtenis van projecten.

1.  Klik op de **Query** van de bovenkant van de pagina Analytics Stream-project.
2.  Voeg de volgende code-editor:

        SELECT * FROM CallStream

    > Zorg ervoor dat de naam van de invoerbron overeenkomt met de naam van de invoer die u eerder hebt opgegeven.

3.  Klik op **testen** onder query editor.
4.  Leveren van een testbestand, een die u de vorige stappen hebt gemaakt of [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json)gebruiken.
5.  Klik op de knop controleren en de resultaten die onder de definitie van de query weergegeven.

    ![De resultaten van de query-definitie](./media/stream-analytics-get-started/stream-analytics-sim-fraud-output.png)


### <a name="column-projection"></a>Kolom projectie

We zullen nu omlaag naar een kleinere set geretourneerde velden vergelijken.

1.  De query in de code-editor te wijzigen:

        SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum
        FROM CallStream

2.  Klik op **opnieuw uitvoeren** onder query editor voor een overzicht van de resultaten van de query.

    ![Uitvoer in de query editor.](./media/stream-analytics-get-started/stream-analytics-query-editor-output.png)

### <a name="count-of-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Het aantal binnenkomende gesprekken per regio: venster met aggregatie gewor

Als u wilt vergelijken, het bedrag dat binnenkomende gesprekken per regio we zult maken gebruik van een [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) als u het aantal binnenkomende gesprekken gegroepeerd op SwitchNum elke vijf seconden.

1.  De query in de code-editor te wijzigen:

        SELECT System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount
        FROM CallStream TIMESTAMP BY CallRecTime
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

    Deze query wordt het trefwoord **Tijdstempel door** een tijdstempelveld opgegeven in de nettolading moet worden gebruikt bij het berekenen van de tijdelijke. Als dit veld niet is opgegeven, zou de windowing-bewerking worden uitgevoerd met behulp van de tijd die elke gebeurtenis op de gebeurtenis Hub aangekomen. Zie ['tijd Vs toepassing aankomsttijd"in de stroom Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Houd er rekening mee dat u toegang een tijdstempel voor het einde van elk venster tot via de eigenschap **System.Timestamp** .

2.  Klik op **opnieuw uitvoeren** onder query editor voor een overzicht van de resultaten van de query.

    ![Queryresultaten voor Timestand door](./media/stream-analytics-get-started/stream-ananlytics-query-editor-rerun.png)

### <a name="sim-fraud-detection-with-a-self-join"></a>Fraude-opsporing SIM-kaart met een self Join

Om aan te duiden, potentieel frauduleus gebruik zullen we voor gesprekken afkomstig van dezelfde gebruiker, maar op verschillende locaties in minder dan 5 seconden.  We [join](https://msdn.microsoft.com/library/azure/dn835026.aspx) de stroom van gebeurtenissen gesprek met zichzelf te controleren voor deze gevallen.

1.  De query in de code-editor te wijzigen:

        SELECT System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1,
        CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
        ON CS1.CallingIMSI = CS2.CallingIMSI
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum

2.  Klik op **opnieuw uitvoeren** onder query editor voor een overzicht van de resultaten van de query.

    ![De resultaten van de query een join](./media/stream-analytics-get-started/stream-ananlytics-query-editor-join.png)

### <a name="create-output-sink"></a>Uitvoer-sink maken

Nu dat we hebben gedefinieerd stream van een gebeurtenis, een gebeurtenis Hub als invoer gebruikt voor consumptie van gebeurtenissen en een query uit te voeren van een transformatie via de stream, is de laatste stap voor het definiëren van een sink uitvoer voor de taak.  We zullen gebeurtenissen voor frauduleus gedrag schrijven naar een Blob-opslag.

De volgende stappen voor het maken van een container voor Blob-opslag als u er nog geen hebt.

1.  Een bestaande account voor opslag of maak een nieuwe opslag-account door te klikken op **Nieuw > DATA SERVICES > opslag > Snelle invoer van** en de instructies te volgen.
2.  Selecteer de account opslag **CONTAINERS** aan de bovenkant van de pagina en klik vervolgens op **toevoegen**.
3.  Geef een **naam** voor de container en de **toegang** tot openbare Blob instellen.

## <a name="specify-job-output"></a>Uitvoer van de taak opgeven

1.  Klik op **uitvoer** vanaf de bovenkant van de pagina en klik op **Uitvoer toevoegen**in uw project Stream Analytics. In het dialoogvenster dat wordt geopend kunt u via een aantal stappen uit om de uitvoer in te stellen.
2.  **BLOB-opslag**selecteren en klik vervolgens op de knop rechts.
3.  Typ of Selecteer de volgende waarden op de derde pagina:

    * **UITVOERALIAS**: Geef een beschrijvende naam voor de uitvoer van deze taak.
    * **Abonnement**: als de Blob-opslag die u hebt gemaakt in het abonnement hetzelfde als de taak van de Stream Analytics, selecteer **Gebruik opslag rekening van het huidige abonnement**. Als de opslag in een ander abonnement, selecteer **Gebruik opslag rekening van een ander abonnement** en handmatig invoeren voor de **Rekening voor opslag**, **Opslag ACCOUNT sleutel**, **CONTAINER**.
    * **Opslag ACCOUNT**: Selecteer de naam van de account van de opslag.
    * **CONTAINER**: Selecteer de naam van de container.
    * **VOORVOEGSEL bestandsnaam**: Type in een voorvoegsel moet worden gebruikt bij het schrijven van blob-uitvoer.

4.  Klik op de knop rechts.
5.  De volgende waarden opgeven:

    * **GEBEURTENIS SERIALIZER indeling**: JSON
    * **Codering**: UTF8

6.  Klik op de knop controleren om toe te voegen deze bron en om te controleren dat Stream Analytics verbinding met de account van de opslag maken kan.

## <a name="start-job-for-real-time-processing"></a>Taak voor real-time verwerking starten

Omdat een taak invoer-, query- en uitvoer alle zijn opgegeven, zijn we klaar om de taak van de Stream Analytics voor real-time fraude-opsporing.

1.  Klik op **START** onder aan de pagina van het project **DASHBOARD**.
2.  **BEGINTIJD taak** selecteren in het dialoogvenster en klik vervolgens op de knop controleren op de onderkant van het dialoogvenster. Status van de taak wordt gewijzigd in **eerste** en binnenkort **actief**bewegen.

## <a name="view-fraud-detection-output"></a>Weergave fraude detectie uitvoer

Gebruik een hulpprogramma zoals [Azure Opslagverkenner](https://azurestorageexplorer.codeplex.com/) of [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) frauduleuze u gebeurtenissen wilt weergeven zoals ze zijn geschreven om de uitvoer in real-time.  

![Fraude-opsporing: frauduleuze gebeurtenissen weergegeven in real time](./media/stream-analytics-get-started/stream-ananlytics-view-real-time-fraudent-events.png)

## <a name="get-support"></a>Ondersteuning krijgen
Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)voor verdere ondersteuning.


## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Analytics Azure Stream](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)
