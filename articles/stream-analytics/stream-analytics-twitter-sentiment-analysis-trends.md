<properties
    pageTitle="Realtime Twitter sentiment analyse met Stream Analytics | Microsoft Azure"
    description="Informatie over het streamen Analytics gebruiken voor realtime Twitter sentiment analyse. Stapsgewijze begeleiding van gebeurtenis genereren met gegevens op een live dashboard."
    keywords="realtime twitter trendanalyse, sentiment analyse, analyse van de sociale media, trend analyse voorbeeld"
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
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="social-media-analysis-real-time-twitter-sentiment-analysis-in-azure-stream-analytics"></a>Sociale media analyse: Real-time Twitter sentiment analyse in Azure Stream Analytics

Leren een sentiment analyse-oplossing voor sociale media analytics maken doordat realtime Twitter gebeurtenissen in Azure gebeurtenis Hubs. U zult een Azure Stream Analytics-query om de gegevens te analyseren. U zult vervolgens opslaan van de resultaten voor latere perusal of gebruik een dashboard en [Power BI](https://powerbi.com/) inzichten in real-time.

Sociale media analytics tools helpen organisaties begrijpen volgen van trends onderwerpen, dat wil zeggen onderwerpen en attitudes die een groot aantal advertenties in sociale media hebben. Sentiment analyse, ook wel genoemd *advies mijnbouw*, via sociale media analysehulpmiddelen houding ten opzichte van een product, idee, enzovoort. Realtime Twitter trendanalyse is een goed voorbeeld omdat het abonnementsmodel hashtag kunt u luisteren naar bepaalde trefwoorden en sentiment analyse op de feed te ontwikkelen.

## <a name="scenario-sentiment-analysis-in-real-time"></a>Scenario: Sentiment analyse in realtime

Een bedrijf met een website nieuws media is geïnteresseerd in het verkrijgen van een voordeel ten opzichte van zijn concurrenten door de inhoud van de site die direct relevant is voor de lezers. Het bedrijf gebruikt sociale media analyse over onderwerpen die relevant voor lezers zijn door real-time sentiment analyse op Twitter gegevens doen. Met name voor onderwerpen volgen van trends in realtime op Twitter moet het bedrijf real-time analytics over het sentiment en tweet volume voor belangrijke onderwerpen. Dus in wezen is het nodig een sentiment analyse analyse-engine die gebaseerd op deze sociale media-kanaal.

## <a name="prerequisites"></a>Vereisten
-   Twitter-account en [OAuth-toegangstoken](https://dev.twitter.com/oauth/overview/application-owner-access-tokens)
-   [TwitterClient.zip](http://download.microsoft.com/download/1/7/4/1744EE47-63D0-4B9D-9ECF-E379D15F4586/TwitterClient.zip) vanaf het Microsoft Download Center
-   Optioneel: Broncode van [GitHub](https://aka.ms/azure-stream-analytics-twitterclient) twitter-client

## <a name="create-an-event-hub-input-and-a-consumer-group"></a>Een gebeurtenis hub invoer en een groep consumenten maken

De voorbeeldtoepassing worden gebeurtenissen genereren en duw ze naar een gebeurtenis Hubs exemplaar (kortweg de hub van een gebeurtenis). Service Bus gebeurtenis hubs zijn de voorkeur van ingestiedosiscoëfficiënten voor Stream Analytics gebeurtenis. Zie de documentatie van de gebeurtenis Hubs in [Bus Service documentatie](/documentation/services/service-bus/).

Gebruik de volgende stappen voor het maken van een hub gebeurtenis.

1.  Klik op **Nieuw**in het portal voor Azure > **APP SERVICES** > **SERVICE BUS** > **GEBEURTENIS HUB** > **Snelle invoer**, en geef een naam, regio en nieuwe of bestaande naamruimte.  
2.  Beste, moet elke taak Analytics stroom van één gebeurtenis Hubs consument groep lezen. We begeleidt u door het proces van het maken van een groep consumenten later. U kunt meer informatie over groepen consumenten op [Azure gebeurtenis Hubs overzicht](https://msdn.microsoft.com/library/azure/dn836025.aspx). Een consument als groep wilt maken, gaat u op de zojuist gemaakte gebeurtenis hub, klikt u op het tabblad **Groepen consumenten** , klikt u op **maken** op de onderkant van de pagina en geef vervolgens een naam voor de groep van de consument.
3.  Om toegang te verlenen aan de gebeurtenis hub, moeten we een beleid voor gedeeld gebruik maken. Klik op het tabblad **configureren** van uw hub-gebeurtenis.
4.  Maak een nieuw beleid met de machtigingen **beheren** onder **Gedeelde-beleid**.

    ![Gedeeld-beleid waar u een beleid met de machtigingen beheren maken kunt.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-ananlytics-shared-access-policies.png)

5.  Klik op **Opslaan** onder aan de pagina.
6.  Ga naar het **DASHBOARD**, **VERBINDINGSGEGEVENS** aan de onderkant van de pagina, klikt u op en kopieer en opslaan van gegevens voor de verbinding. (Gebruik de kopiëren-pictogram dat wordt weergegeven onder het zoekpictogram.)

## <a name="configure-and-start-the-twitter-client-application"></a>Configureren en starten van de toepassing van de Twitter-client

We hebben een clienttoepassing die verbinding met Twitter-gegevens via [Twitter van Streaming API's maken](https://dev.twitter.com/streaming/overview) voor het verzamelen van gebeurtenissen Tweet over een reeks onderwerpen die met parameters beschikbaar. Het hulpprogramma [Sentiment140](http://help.sentiment140.com/) open source wordt sentiment waarde toewijzen aan elke tweet gebruikt.

- 0 = negatief
- 2 = neutral
- 4 = positief

Tweet-gebeurtenissen worden vervolgens naar de gebeurtenis hub geduwd.  

Ga als volgt te werk om de toepassing in te stellen:

1.  [Download de TwitterClient oplossing](http://download.microsoft.com/download/1/7/4/1744EE47-63D0-4B9D-9ECF-E379D15F4586/TwitterClient.zip).
2.  Open TwitterClient.exe.config en oauth_consumer_key, oauth_consumer_secret, oauth_token en oauth_token_secret vervangen door de Twitter-tokens die de waarden hebben.  

    [Stappen voor het genereren van een toegangstoken OAuth](https://dev.twitter.com/oauth/overview/application-owner-access-tokens)  

    Opmerking u moet een lege toepassing om een token te genereren.  
3.  Vervang de waarden EventHubConnectionString en EventHubName in TwitterClient.exe.config met de verbindingsreeks en de naam van uw hub-gebeurtenis. De tekenreeks die u eerder hebt gekopieerd kunt u zowel de verbindingsreeks en de naam van de gebeurtenis hub, dus om te scheiden en elk in het juiste veld plaatsen. Neem bijvoorbeeld de volgende verbindingsreeks:

        Endpoint=sb://your.servicebus.windows.net/;SharedAccessKeyName=yourpolicy;SharedAccessKey=yoursharedaccesskey;EntityPath=yourhub

    Het bestand TwitterClient.exe.config bevatten de instellingen in het volgende voorbeeld:

        add key="EventHubConnectionString" value="Endpoint=sb://your.servicebus.windows.net/;SharedAccessKeyName=yourpolicy;SharedAccessKey=yoursharedaccesskey"
        add key="EventHubName" value="yourhub"

    Is het belangrijk te weten dat de tekst ' EntityPath = ' wordt __niet__ weergegeven in de EventHubName-waarde.

4.  *Optioneel:* Zoeken naar trefwoorden aanpassen.  Standaard wordt deze toepassing zoekt naar "Azure, Skype, XBox, Microsoft, Amsterdam".  De waarden voor **twitter_keywords** in TwitterClient.exe.config, kunt u desgewenst aanpassen.
5.  Uitvoeren van TwitterClient.exe om de toepassing te starten. Tweet-gebeurtenissen ziet u met de **CreatedAt**, **onderwerp**en **SentimentScore** -waarden die worden verzonden naar uw hub-gebeurtenis.

    ![Sentiment analyse: SentimentScore waarden naar een hub gebeurtenis verzonden.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-sentiment-output-to-event-hub.png)

## <a name="create-a-stream-analytics-job"></a>Maak een taak Stream Analytics

Nu in realtime uit Twitter Tweet gebeurtenissen streaming, we kunnen instellen een taak Stream Analytics voor het analyseren van deze gebeurtenissen in real time.

### <a name="provision-a-stream-analytics-job"></a>Een taak Analytics stroom voorziening

1.  Klik op **Nieuw**in [Azure portal](https://manage.windowsazure.com/) > **DATA SERVICES** > **STREAM ANALYTICS** > **Snelle invoer**.
2.  De volgende waarden opgeven en klik vervolgens op **Stroom ANALYTICS taak maken**:

    * **Naam**: Voer de taaknaam van een.
    * **Regio**: Selecteer de regio waar u de taak uit te voeren. Rekening houden met het plaatsen van de taak en de gebeurtenis hub in hetzelfde gebied betere prestaties te garanderen en om ervoor te zorgen dat u niet betaalt voor gegevensoverdracht tussen regio's.
    * **Opslag ACCOUNT**: Kies de Azure opslag-account die u gebruiken wilt voor het opslaan van de meetgegevens voor alle stroom Analytics taken die in dit gebied worden uitgevoerd. U hebt de optie Kies een bestaande account voor opslag of een nieuwe maken.

3.  Klik op **STREAM ANALYTICS** in het linkerdeelvenster om de Stream Analytics taken.  
    ![Pictogram stream Analytics-service](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-service-icon.png)

    Het nieuwe project wordt weergegeven met de status **gemaakt**. U ziet dat de knop **START** op de onderkant van de pagina is uitgeschakeld. Moet u de taak invoer, uitvoer en query voordat u de taak kunt starten.


### <a name="specify-job-input"></a>Invoer van de taak opgeven
1.  In uw Analytics Stream-project, klikt u op **"inputs"** vanaf de bovenkant van de pagina en klik op **Invoer toevoegen**. In het dialoogvenster dat wordt geopend kunt u via een aantal stappen voor het instellen van uw invoer.
2.  **De GEGEVENSSTROOM**op en klik vervolgens op de knop rechts.
3.  Klik op de **HUB van de GEBEURTENIS**en klikt u op de knop rechts.
4.  Typ of Selecteer de volgende waarden op de derde pagina:

    * **Invoer ALIAS**: Geef een beschrijvende naam voor deze taak, zoals een *TwitterStream*worden ingevoerd. Houd er rekening mee dat u deze naam later in de query gebruiken zal.
    **GEBEURTENIS HUB**: als de hub van de gebeurtenis die u hebt gemaakt in het abonnement hetzelfde als de taak van de Stream Analytics, selecteert u de naamruimte waarin de gebeurtenis hub is.

        Als uw hub-gebeurtenis zich in een ander abonnement, **Gebruik gebeurtenis Hub van een ander abonnement**op en voer vervolgens informatie voor **SERVICE BUS NAAMRUIMTE**, **HUB GEBEURTENISNAAM** **GEBEURTENISNAAM HUB beleid**, **Sleutel GEBEURTENIS HUB beleid**en **Aantal HUB partitie**handmatig.

    * **Naam van GEBEURTENIS HUB**: Selecteer de naam van de gebeurtenis hub.

    * **HUB-BELEIDSNAAM GEBEURTENIS**: de gebeurtenis hub-beleid dat u eerder in deze handleiding hebt gemaakt selecteren.

    * **GEBEURTENIS HUB CONSUMER GROUP**: Typ de naam van de groep consumenten die u eerder in deze handleiding hebt gemaakt.
5.  Klik op de knop rechts.
6.  De volgende waarden opgeven:

    * **GEBEURTENIS SERIALIZER indeling**: JSON
    * **Codering**: UTF8

7.  Klik op **controleren** om toe te voegen deze bron en om te controleren dat stroom Analytics verbinding met de hub van de gebeurtenis maken kunnen.

### <a name="specify-job-query"></a>Project-query opgeven

Stream Analytics ondersteunt een eenvoudige, declaratieve query-model dat transformaties worden beschreven. Zie voor meer informatie over de taal, de [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx).  Deze zelfstudie kunt u ontwerpen en testen van meerdere query's over Twitter-gegevens.

#### <a name="sample-data-input"></a>Voorbeeldinvoer

Voor het valideren van uw query op een werkelijke taakgegevens, kunt u de functie **VOORBEELDGEGEVENS** gebeurtenissen ophalen uit de stream en voor het testen van een .json-bestand van de gebeurtenissen maken.

1.  Selecteer uw gebeurtenis hub invoer en klik vervolgens op **VOORBEELDGEGEVENS** onderaan de pagina.
2.  In het dialoogvenster Geef een **BEGINTIJD** te starten met het verzamelen van gegevens en een **duur** voor hoeveel extra gegevens te verbruiken.
3.  Klik op de knop **DETAILS** en klik op de koppeling **Klik hier** om te downloaden en sla het bestand gegenereerd .json.

#### <a name="pass-through-query"></a>Pass Through-query
Om te beginnen, doen wij een eenvoudige Pass Through-query die alle velden in een gebeurtenis van projecten.

1.  Klik op **QUERY** aan de bovenkant van de pagina Analytics Stream-project.
2.  In de code-editor, de oorspronkelijke querysjabloon te vervangen door het volgende:

        SELECT * FROM TwitterStream

    Zorg ervoor dat de naam van de invoerbron overeenkomt met de naam van de invoer die u eerder hebt opgegeven.

3.  Klik op **testen** onder query editor.
4.  Ga naar het voorbeeldbestand .json.
5.  Klik op de knop **controleren** en de resultaten onder de definitie van de query.

    ![Resultaten weergegeven onder de definitie van query](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sentiment-by-topic.png)

#### <a name="count-of-tweets-by-topic-tumbling-window-with-aggregation"></a>Het aantal tweets per onderwerp: venster met aggregatie gewor

Als u wilt vergelijken in het aantal vermeldingen tussen verschillende onderwerpen, gebruiken we een [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) om het aantal vermeldingen per onderwerp elke vijf seconden.

1.  De query in de code-editor te wijzigen:

        SELECT System.Timestamp as Time, Topic, COUNT(*)
        FROM TwitterStream TIMESTAMP BY CreatedAt
        GROUP BY TUMBLINGWINDOW(s, 5), Topic

    Deze query wordt het trefwoord **Tijdstempel door** een tijdstempelveld opgegeven in de nettolading moet worden gebruikt bij het berekenen van de tijdelijke. Als dit veld niet is opgegeven, de windowing bewerking uitgevoerd met behulp van de tijd die elke gebeurtenis in de hub gebeurtenis ontvangen.  Meer informatie in de sectie "Tijd Vs toepassing aankomsttijd" van de [Stream Analytics Query Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Met deze query wordt ook een tijdstempel voor het einde van elk venster via de eigenschap **System.Timestamp** .

2.  Klik op **opnieuw uitvoeren** onder query editor voor een overzicht van de resultaten van de query.

#### <a name="identify-trending-topics-sliding-window"></a>Onderwerpen volgen van trends identificeren: schuifvenster

Voor het volgen van trends onderwerpen zullen we naar de onderwerpen die via een drempelwaarde voor de vermeldingen in een bepaalde hoeveelheid tijd. Voor de toepassing van deze zelfstudie, zullen we controleren voor onderwerpen die meer dan 20 keer worden vermeld in de laatste vijf seconden met behulp van een [SlidingWindow](https://msdn.microsoft.com/library/azure/dn835051.aspx).

1.  Wijzigen van de query in de code-editor: Selecteer System.Timestamp als tijd, onderwerp, COUNT (*) als vermeld vanaf TwitterStream tijdstempel door CreatedAt groep door SLIDINGWINDOW(s, 5), tellen dat onderwerp (*) > 20

2.  Klik op **opnieuw uitvoeren** onder query editor voor een overzicht van de resultaten van de query.

    ![De queryuitvoer venster schuiven](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-query-output.png)

#### <a name="count-of-mentions-and-sentiment-tumbling-window-with-aggregation"></a>Het aantal vermeldingen en sentiment: venster met aggregatie gewor
**TumblingWindow** de laatste query die we testen wordt gebruikt om het aantal vermeldingen, gemiddelde, maximum, minimum en standaarddeviatie van sentiment score voor elk onderwerp elke vijf seconden.

1.  De query in de code-editor te wijzigen:

        SELECT System.Timestamp as Time, Topic, COUNT(*), AVG(SentimentScore), MIN(SentimentScore),
        Max(SentimentScore), STDEV(SentimentScore)
        FROM TwitterStream TIMESTAMP BY CreatedAt
        GROUP BY TUMBLINGWINDOW(s, 5), Topic

2.  Klik op **opnieuw uitvoeren** onder query editor voor een overzicht van de resultaten van de query.
3.  Dit is de query die we voor onze dashboard gebruiken.  Klik op **Opslaan** onder aan de pagina.


## <a name="create-output-sink"></a>Uitvoer-sink maken

Nu dat we hebben gedefinieerd stream van een gebeurtenis, een gebeurtenis hub als invoer gebruikt voor consumptie van gebeurtenissen en een query uit te voeren van een transformatie via de stream, is de laatste stap voor het definiëren van een sink uitvoer voor de taak.  We zullen de geaggregeerde tweet gebeurtenissen van onze taak query schrijven naar Azure Blob-opslag.  U kan de resultaten ook push Azure SQL-database, tabel Azure-opslag of gebeurtenis Hubs, afhankelijk van uw specifieke toepassing nodig heeft.

Gebruik de volgende stappen voor het maken van een container voor Blob-opslag, als u nog niet hebt:

1.  Gebruik een bestaande account voor opslag of maak een nieuwe opslag-account door te klikken op **Nieuw** > **DATA SERVICES** > **opslag** > **Snel maken**en volg vervolgens de instructies op het scherm.
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

## <a name="start-job"></a>Taak starten

Omdat een taak invoer-, query- en uitvoer alle zijn opgegeven, zijn we klaar om de taak van de Stream Analytics.

1.  Klik op **START** onder aan de pagina van het project **DASHBOARD**.
2.  In het dialoogvenster dat wordt geopend, klikt u op **taak starten tijd**en klik vervolgens op de knop **controleren** op de onderkant van het dialoogvenster. Status van de taak wordt gewijzigd in **eerste** en binnenkort **actief**wordt gewijzigd.


## <a name="view-output-for-sentiment-analysis"></a>Uitvoer voor sentiment analyse weergeven

Nadat de taak wordt uitgevoerd en de real-time Twitter stream processing, kiezen hoe u de uitvoer voor sentiment analyse weergeven. Met een hulpprogramma zoals [Azure Opslagverkenner](https://azurestorageexplorer.codeplex.com/) of [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) kunt u de uitvoer van de taak in real-time. Hier kunt u uitbreiden van uw toepassing wilt opnemen, een aangepast dashboard zoals in de volgende schermafdruk [Power BI](https://powerbi.com/) .

![Sociale media analyse: Stream Analytics sentiment analyse (advies mining) uitvoer in een Power BI-dashboard.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-output-power-bi.png)

## <a name="get-support"></a>Ondersteuning krijgen
Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)voor verdere ondersteuning.


## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Analytics Azure Stream](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)
