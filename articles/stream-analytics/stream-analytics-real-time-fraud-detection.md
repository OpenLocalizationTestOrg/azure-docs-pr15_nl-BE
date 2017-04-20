<properties
    pageTitle="Stream Analytics: Real-time fraude-opsporing | Microsoft Azure"
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

Informatie over het maken van een end-to-end oplossing voor real-time fraude-opsporing met Azure Stream Analytics. Gebeurtenissen doen in Azure gebeurtenis Hubs, Stream Analytics query's voor aggregatie of waarschuwingen schrijven en verzenden van de resultaten naar een sink uitvoer om inzicht te krijgen over gegevens met real-time verwerking. Afwijking real-time detectie voor telecommunicatie wordt uitgelegd, maar het voorbeeld van de techniek is even geschikt voor andere soorten fraude-opsporing, zoals creditcard of identity theft-scenario's.

Stream Analytics is een volledig beheerde service voor verwerking van de lage latentie, hoge beschikbaarheid, schaalbaar, complexe gebeurtenis via streaming gegevens in de cloud. Zie [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)voor meer informatie.


## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Scenario: Telecommunicatie en SIM fraude-opsporing in realtime

Een telecommunicatie-bedrijf heeft een grote hoeveelheid gegevens voor binnenkomende gesprekken. Het bedrijf heeft de volgende gegevens nodig:

* Gegevens beperken tot een beheersbaar bedrag en inzichten over het gebruik van de klant na verloop van tijd en verschillende geografische regio's te verkrijgen.
* Detecteren in real-time SIM fraude (meerdere aanroepen uit dezelfde identiteit rond dezelfde tijd, maar in geografische vestigingen), zodat het bedrijf snel reageren kan door kennisgeving aan klanten of service wordt afgesloten.

Canonieke Internet dingen (IoT) scenario's hebben een ton van telemetrie of gegevens van sensoren. Klanten willen aggregatie van de gegevens of het ontvangen van waarschuwingen over anomalieën in real-time.

## <a name="prerequisites"></a>Vereisten

- [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) downloaden van het Microsoft Download Center
- Optioneel: De broncode van de gebeurtenis generator van [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator)

## <a name="create-azure-event-hubs-input-and-consumer-group"></a>Azure gebeurtenis Hubs invoer- en groep maken

De voorbeeldtoepassing worden gebeurtenissen genereren en duw ze naar een exemplaar van de gebeurtenis Hubs voor real-time verwerking. Service Bus gebeurtenis Hubs zijn de voorkeur van ingestiedosiscoëfficiënten voor Stream Analytics gebeurtenis. U kunt meer informatie over de gebeurtenis Hubs in [Azure Service Bus documentatie](/documentation/services/service-bus/).

Een hub gebeurtenis maken:

1.  Klik op **Nieuw**in [Azure portal](https://manage.windowsazure.com/) > **APP SERVICES** > **SERVICE BUS** > **GEBEURTENIS HUB** > **Snelle invoer**. Geef een naam, regio, en nieuwe of bestaande naamruimte voor het maken van een nieuwe gebeurtenis hub.  
2.  Elke taak Stream Analytics dient als beste uit een enkele gebeurtenis hub consument groep lezen. We begeleidt u door het proces van het maken van een groep consumenten later. [Meer informatie over groepen consumenten](https://msdn.microsoft.com/library/azure/dn836025.aspx). Een consument als groep wilt maken, gaat u op de zojuist gemaakte gebeurtenis hub, klikt u op het tabblad **Groepen consumenten** , klikt u op **maken** op de onderkant van de pagina en geef vervolgens een naam voor de groep van de consument.
3.  Om toegang te verlenen aan de gebeurtenis hub, moeten we een beleid voor gedeeld gebruik maken. Klik op het tabblad **configureren** van uw hub-gebeurtenis.
4.  Maak een nieuw beleid met machtigingen **beheren** onder **Gedeelde-beleid**.

    ![Gedeeld-beleid waar u een beleid met de machtigingen beheren maken kunt.](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-shared-access-policies.png)

5.  Klik op **Opslaan** onder aan de pagina.
6.  Ga naar het **Dashboard**, **VERBINDINGSGEGEVENS** aan de onderkant van de pagina, klikt u op en kopieer en opslaan van gegevens voor de verbinding.

## <a name="configure-and-start-the-event-generator-application"></a>Configureren en starten van de gebeurtenis generator-toepassing

We hebben een clienttoepassing die worden gegenereerd monster binnenkomende oproep metagegevens en druk in de gebeurtenis Hubs verstrekt. Gebruik de volgende stappen uit om deze toepassing in te stellen.  

1.  Download het [bestand TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip)en unzip het naar een map.

    > [AZURE.NOTE] Windows blokkeert mogelijk de gedownloade zip-bestand. Klik met de rechtermuisknop op het bestand en selecteer **Eigenschappen**. Als het bericht 'dit bestand is afkomstig van een andere computer en mogelijk geblokkeerd om deze computer beter te beveiligen', selecteert u de **blokkering opheffen** en klik vervolgens op toepassen op het zip-bestand.

2.  De waarden Microsoft.ServiceBus.ConnectionString en EventHubName in de telcodatagen.exe.config vervangen door de gebeurtenis hub-verbindingsreeks en de naam.

    De tekenreeks die u hebt gekopieerd vanaf de portal Azure plaatst de naam van de verbinding aan het einde. Verwijder "; EntityPath =<value>"uit de" sleutel = "veld.

3.  Start de toepassing. De syntaxis is als volgt:

    telcodatagen.exe [#NumCDRsPerHour] [SIM-kaart fraude kans] [#DurationHours]

In het volgende voorbeeld genereert 1000 gebeurtenissen met een kans van 20 procent van de fraude in de loop van twee uur.

    telcodatagen.exe 1000 .2 2

Records die worden verzonden naar uw hub-gebeurtenis wordt weergegeven. Hier definieert u sommige belangrijke velden die we in deze toepassing real-time fraude detectie gebruiken:

| Record | Definitie |
| ------------- | ------------- |
| CallrecTime | De tijdstempel voor de begintijd van de oproep. |
| SwitchNum | Telefooncentrale kunt u verbinding maken. |
| CallingNum | Het telefoonnummer van de beller. |
| CallingIMSI | Internationale Mobile Subscriber Identity (IMSI).  De unieke id van de beller. |
| CalledNum | Het telefoonnummer van de ontvanger. |
| CalledIMSI | Internationale Mobile Subscriber Identity (IMSI).  De unieke id van de ontvanger. |


## <a name="create-a-stream-analytics-job"></a>Maak een taak Stream Analytics
Nu hebben we een reeks gebeurtenissen telecommunicatie, we kunnen instellen een taak Stream Analytics voor het analyseren van deze gebeurtenissen in real time.

### <a name="provision-a-stream-analytics-job"></a>Een taak Analytics stroom voorziening

1.  Klik op **Nieuw**in het portal voor Azure > **DATA SERVICES** > **STREAM ANALYTICS** > **Snelle invoer**.
2.  De volgende waarden opgeven en klik vervolgens op **Stroom ANALYTICS taak maken**:

    * **Naam**: Voer de taaknaam van een.

    * **Regio**: Selecteer de regio waar u de taak uit te voeren. Rekening houden met het plaatsen van de taak en de gebeurtenis hub in hetzelfde gebied betere prestaties te garanderen en om ervoor te zorgen dat u niet betaalt voor gegevensoverdracht tussen regio's.

    * **Opslag ACCOUNT**: Kies de Azure opslag-account die u gebruiken wilt voor het opslaan van de meetgegevens voor alle stroom Analytics taken die in dit gebied worden uitgevoerd. U hebt de optie voor het kiezen van een bestaande account voor opslag of maak een nieuwe.

3.  Klik op **STREAM ANALYTICS** in het linkerdeelvenster om de Stream Analytics taken.

    ![Pictogram stream Analytics-service](./media/stream-analytics-real-time-fraud-detection/stream-analytics-service-icon.png)

    Het nieuwe project wordt weergegeven met de status **gemaakt**. U ziet dat de knop **START** op de onderkant van de pagina is uitgeschakeld. Moet u de taak invoer, uitvoer en query voordat u de taak kunt starten.

### <a name="specify-job-input"></a>Invoer van de taak opgeven
1.  Klik op **"inputs"** bij de bovenkant van de pagina en klik op **Invoer toevoegen**in uw project Stream Analytics. In het dialoogvenster dat wordt geopend kunt u bij de verschillende stappen voor het instellen van uw invoer.
2.  **De GEGEVENSSTROOM**op en klik vervolgens op de knop rechts.
3.  Klik op de **HUB van de GEBEURTENIS**en klikt u op de knop rechts.
4.  Typ of Selecteer de volgende waarden op de derde pagina:

    * **ALIAS INPUT**: Geef een beschrijvende naam, bijvoorbeeld *CallStream*, voor deze taak. Houd er rekening mee dat u deze naam in de query later gebruikt.

    * **GEBEURTENIS HUB**: als de hub van de gebeurtenis die u hebt gemaakt in het abonnement hetzelfde als de taak van de Stream Analytics, selecteert u de naamruimte waarin de gebeurtenis hub is.

        Als uw hub-gebeurtenis zich in een ander abonnement, selecteert u **Gebeurtenis-Hub voor gebruik van een ander abonnement**en voer vervolgens informatie voor **SERVICE BUS NAAMRUIMTE**, **HUB GEBEURTENISNAAM** **GEBEURTENISNAAM HUB beleid**, **Sleutel GEBEURTENIS HUB beleid**en **Aantal HUB partitie**handmatig.

    * **Naam van GEBEURTENIS HUB**: Selecteer de naam van de gebeurtenis hub.

    * **HUB-BELEIDSNAAM GEBEURTENIS**: de gebeurtenis hub-beleid dat u eerder in deze handleiding hebt gemaakt selecteren.

    * **GEBEURTENIS HUB CONSUMER GROUP**: Typ de naam van de groep consumenten die u eerder in deze handleiding hebt gemaakt.

5.  Klik op de knop rechts.
6.  De volgende waarden opgeven:

    * **GEBEURTENIS SERIALIZER indeling**: JSON
    * **Codering**: UTF8
7.  Klik op **controleren** om toe te voegen deze bron en om te controleren dat stroom Analytics verbinding met de hub van de gebeurtenis maken kunnen.

### <a name="specify-job-query"></a>Project-query opgeven

Stream Analytics ondersteunt een eenvoudige, declaratieve query-model dat transformaties voor real-time verwerking wordt beschreven. Zie voor meer informatie over de taal, de [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/dn834998.aspx). Deze zelfstudie kunt u ontwerpen en testen van meerdere query's via uw real-time gegevensstroom oproep.

#### <a name="optional-sample-input-data"></a>Optioneel: Invoer voorbeeldgegevens
Voor het valideren van uw query op een werkelijke taakgegevens, kunt u de functie **VOORBEELDGEGEVENS** gebeurtenissen ophalen uit uw stroom en maken een. JSON-bestand van de gebeurtenissen voor het testen.  De volgende stappen uit hoe u dit moet doen. We hebben ook een voorbeeldbestand [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json) voor testdoeleinden beschikbaar.

1.  Selecteer uw gebeurtenis hub invoer en klik vervolgens op **VOORBEELDGEGEVENS** onderaan de pagina.
2.  In het dialoogvenster Geef een **BEGINTIJD** te starten met het verzamelen van gegevens en een **duur** voor hoeveel extra gegevens te verbruiken.
3.  Klik op **controleren** om te beginnen de bemonstering gegevens van de invoer.  Het kan een paar minuten duren voor het gegevensbestand dat moet worden geproduceerd.  Wanneer het proces is voltooid, klikt u op **DETAILS**, de gegenereerde downloaden. JSON-bestand en deze opslaan.

    ![Download en verwerkte gegevens opslaan in een bestand JSON](./media/stream-analytics-real-time-fraud-detection/stream-analytics-download-save-json-file.png)

#### <a name="pass-through-query"></a>Pass Through-query

Als u wilt dat elke gebeurtenis te archiveren, kunt u een Pass Through-query de velden in de nettolading van de gebeurtenis of het bericht te lezen. Doen om te starten, een eenvoudige Pass Through-query die alle velden in een gebeurtenis van projecten.

1.  Klik op de **QUERY** van de bovenkant van de pagina Analytics Stream-project.
2.  Voeg de volgende code-editor:

        SELECT * FROM CallStream

    > [AZURE.IMPORTANT] Zorg ervoor dat de naam van de invoerbron overeenkomt met de naam van de invoer die u eerder hebt opgegeven.

3.  Klik op **testen** onder query editor.
4.  Een testbestand opgeven. Gebruikt dat u hebt de vorige stappen hebt gemaakt, of [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/SampleDataFiles/Telco.json).
5.  Klik op de knop **controleren** en de resultaten die onder de definitie van de query weergegeven.

    ![De resultaten van de query-definitie](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sim-fraud-output.png)


### <a name="column-projection"></a>Kolom projectie

Nu verlagen we de geretourneerde velden naar een kleinere set.

1.  De query in de code-editor te wijzigen:

        SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum
        FROM CallStream

2.  Klik op **opnieuw uitvoeren** onder query editor voor een overzicht van de resultaten van de query.

    ![Uitvoer in de query editor.](./media/stream-analytics-real-time-fraud-detection/stream-analytics-query-editor-output.png)

### <a name="count-of-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Het aantal binnenkomende gesprekken per regio: venster met aggregatie gewor

Om het aantal binnenkomende gesprekken per regio te vergelijken, gebruikt u een [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) om het aantal binnenkomende gesprekken gegroepeerd op **SwitchNum** elke vijf seconden.

1.  De query in de code-editor te wijzigen:

        SELECT System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount
        FROM CallStream TIMESTAMP BY CallRecTime
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

    Deze query wordt het trefwoord **Tijdstempel door** een tijdstempelveld opgegeven in de nettolading moet worden gebruikt bij het berekenen van de tijdelijke. Als dit veld niet is opgegeven, de windowing bewerking uitgevoerd met behulp van de tijd die elke gebeurtenis in de hub gebeurtenis ontvangen. Zie ['tijd Vs toepassing aankomsttijd"in de stroom Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Houd er rekening mee dat u toegang een tijdstempel voor het einde van elk venster tot via de eigenschap **System.Timestamp** .

2.  Klik op **opnieuw uitvoeren** onder query editor voor een overzicht van de resultaten van de query.

    ![Queryresultaten voor Timestand door](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-query-editor-rerun.png)

### <a name="sim-fraud-detection-with-a-self-join"></a>Fraude-opsporing SIM-kaart met een self Join

Voor het frauduleuze gebruik zullen we voor gesprekken die afkomstig van dezelfde gebruiker, maar op verschillende locaties in minder dan 5 seconden zijn.  We [join](https://msdn.microsoft.com/library/azure/dn835026.aspx) de stroom van gebeurtenissen gesprek met zichzelf te controleren voor deze gevallen.

1.  De query in de code-editor te wijzigen:

        SELECT System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1,
        CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
        ON CS1.CallingIMSI = CS2.CallingIMSI
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum

2.  Klik op **opnieuw uitvoeren** onder query editor voor een overzicht van de resultaten van de query.

    ![De resultaten van de query een join](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-query-editor-join.png)

### <a name="create-output-sink"></a>Uitvoer-sink maken

Nu dat we hebben gedefinieerd stream van een gebeurtenis, een gebeurtenis hub als invoer gebruikt voor consumptie van gebeurtenissen en een query uit te voeren van een transformatie via de stream, is de laatste stap voor het definiëren van een sink uitvoer voor de taak. We zullen gebeurtenissen voor frauduleus gedrag schrijven naar Azure Blob-opslag.

Gebruik de volgende stappen voor het maken van een container voor Blob-opslag als u er nog geen hebt.

1.  Gebruik een bestaande account voor opslag of maak een nieuwe opslag-account door te klikken op **Nieuw > DATA SERVICES > opslag > Snelle invoer**, en volg de instructies.
2.  Selecteer de account opslag **CONTAINERS** aan de bovenkant van de pagina en klik vervolgens op **toevoegen**.
3.  Geef een **naam** voor de container en de **toegang** tot **Openbare Blob**instellen.

## <a name="specify-job-output"></a>Uitvoer van de taak opgeven

1.  Klik op **uitvoer** aan de bovenkant van de pagina en klik op **Uitvoer toevoegen**in uw project Stream Analytics. In het dialoogvenster dat wordt geopend kunt u bij de verschillende stappen voor het instellen van de uitvoer.
2.  **BLOB-opslag**op en klik vervolgens op de knop rechts.
3.  Typ of Selecteer de volgende waarden op de derde pagina:

    * **UITVOERALIAS**: Geef een beschrijvende naam voor de uitvoer van deze taak.
    * **Abonnement**: als de Blob-opslag die u hebt gemaakt in het abonnement hetzelfde als de stroom Analytics taak, klikt u op **Gebruik opslag Account van het huidige abonnement**. Als de opslag in een ander abonnement, klik op **Opslag-Account gebruiken vanaf een ander abonnement**en handmatig gegevens invoeren voor **Rekening van de opslag**, **Opslag ACCOUNT sleutel**en **CONTAINER**.
    * **Opslag ACCOUNT**: Selecteer de naam van de account van de opslag.
    * **CONTAINER**: Selecteer de naam van de container.
    * **VOORVOEGSEL bestandsnaam**: Typ een voorvoegsel moet worden gebruikt bij het schrijven van blob-uitvoer.

4.  Klik op de knop rechts.
5.  De volgende waarden opgeven:

    * **GEBEURTENIS SERIALIZER indeling**: JSON
    * **Codering**: UTF8

6.  Klik op **controleren** om toe te voegen deze bron en om te controleren dat Stream Analytics verbinding met de account van de opslag maken kan.

## <a name="start-job-for-real-time-processing"></a>Taak voor real-time verwerking starten

Omdat een taak invoer-, query- en uitvoer alle zijn opgegeven, zijn we klaar om de taak van de Stream Analytics voor real-time fraude-opsporing.

1.  Klik op **START** onder aan de pagina van het project **DASHBOARD**.
2.  In het dialoogvenster dat wordt geopend, klikt u op **taak starten tijd**en klik vervolgens op de knop **controleren** op de onderkant van het dialoogvenster. Status van de taak wordt gewijzigd in **eerste** en binnenkort **actief**wordt gewijzigd.

## <a name="view-fraud-detection-output"></a>Weergave fraude detectie uitvoer

Gebruik een hulpprogramma zoals [Azure Opslagverkenner](http://storageexplorer.com/) of [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) frauduleuze u gebeurtenissen wilt weergeven zoals ze zijn geschreven om de uitvoer in real-time.  

![Fraude-opsporing: frauduleuze gebeurtenissen weergegeven in real time](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-view-real-time-fraudent-events.png)

## <a name="get-support"></a>Ondersteuning krijgen
Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)voor verdere ondersteuning.


## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Analytics Azure Stream](stream-analytics-introduction.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)
