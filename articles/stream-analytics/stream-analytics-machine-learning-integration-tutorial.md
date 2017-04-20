<properties
    pageTitle="Sentiment analyse met behulp van Azure Stream Analytics en Azure Machine Learning | Microsoft Azure"
    description="Het gebruik van een door de gebruiker gedefinieerde functie en Machine Learning in een Stream Analytics project"
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>


<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="10/04/2016" 
    ms.author="jeffstok"
/>

# <a name="sentiment-analysis-by-using-azure-stream-analytics-and-azure-machine-learning"></a>Sentiment analyse met behulp van Azure Stream Analytics en Azure Machine Learning #

Dit artikel is bedoeld om te helpen u snel een eenvoudig project Azure Stream Analytics, met integratie van de Azure Machine Learning instellen. We gebruiken een sentiment analytics Machine Learning model uit de galerie met Cortana Intelligence streaming gegevens analyseren en bepalen van de score sentiment in real-time. De informatie in dit artikel kunt u inzicht in scenario's zoals analytics real-time sentiment op Twitter gegevensstromen, records van de klant chats met ondersteunend personeel analyseren en evalueren van opmerkingen op forums, blogs en video's, naast vele andere real-time, voorspellende score scenario's.

Dit artikel bevat een voorbeeld van CSV-bestand met tekst als invoer in Azure Blob-opslag in de volgende afbeelding wordt weergegeven. De taak is van toepassing het sentiment analytics model als door de gebruiker gedefinieerde functie (UDF) van de tekst voorbeeldgegevens uit de blob-opslag. Het eindresultaat is geplaatst in het archief met dezelfde blob in een CSV-bestand. 

![Stream Analytics Machine Learning](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-2.png)  

De volgende afbeelding ziet u deze configuratie. Voor een meer realistische scenario kunt u Blob-opslag met streaming Twitter gegevens uit een Azure gebeurtenis Hubs invoer vervangen. Bovendien kan u een [Microsoft Power BI](https://powerbi.microsoft.com/) real-time visualisatie van de statistische sentiment samenstellen.    

![Stream Analytics Machine Learning](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-1.png)  

## <a name="prerequisites"></a>Vereisten

Dit zijn de vereisten voor het uitvoeren van de taken die in dit artikel worden aangetoond:

-   Een abonnement op Azure active.
-   Een CSV-bestand met enkele gegevens erin. U kunt het bestand wordt weergegeven in afbeelding 1 van [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample Data/sampleinput.csv)downloaden of kunt u uw eigen bestand maken. Voor dit artikel, nemen we aan dat u de versie downloaden van GitHub gebruiken.

Op een hoog niveau, om de taken die in dit artikel, u zult het volgende doen:

1.  CSV-bestand uploaden naar Azure Blob-opslag.
2.  Een analytics sentiment model uit de galerie met Cortana Intelligence toevoegen aan uw werkruimte Azure Machine Learning.
3.  Dit model als een webservice in de werkruimte Training computer implementeren.
4.  Maak een taak Stream Analytics waarin deze webservice wordt aangeroepen als functie bepalen sentiment voor de tekst invoeren.
5.  Start de taak Stream Analytics en bekijk de uitvoer.

## <a name="upload-the-csv-input-file-to-blob-storage"></a>CSV-bestand te uploaden naar een Blob-opslag

Voor deze stap kunt u een CSV-bestand, bijvoorbeeld een al beschikbaar voor download op GitHub is opgegeven. U kunt [Opslagverkenner Azure](http://storageexplorer.com/) of Visual Studio voor het uploaden van het bestand of u aangepaste code kunt gebruiken. We gebruiken de voorbeelden op basis van Visual Studio.

1.  Klik in Visual Studio op **Azure** > **opslag** > **Koppelen van externe opslag**. Voer een **accountnaam** en een **sleutel van de Account**.  

    ![Stream Analytics Machine Learning, Server Explorer](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-server-explorer.png)  

2.  Vouw de opslag die u in stap 1 hebt gekoppeld **Blob Container maken**en voert u een logische naam. Nadat u de container gemaakt, openen om de inhoud ervan weer te geven. (Deze is leeg op dit moment).  

    ![Stream-Analytics Machine Learning, blob maken](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-create-blob.png)  

3.  Upload het CSV-bestand en klikt u op **Uploaden Blob**, klikt u op **het bestand op de lokale schijf**.  

## <a name="add-the-sentiment-analytics-model-from-the-cortana-intelligence-gallery"></a>Het model sentiment analytics toevoegen uit de galerie met Cortana Intelligence

1.  Download de [voorspellende sentiment analytics model](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) uit de galerie met Cortana Intelligence.  
2.  In de Studio van Machine Learning, klikt u op **openen in de Studio**.  

    ![Stream-Analytics Machine Learning, open Machine Learning-Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3.  Log in om naar de werkruimte te gaan. Selecteer de locatie die het beste aan uw eigen locatie.
4.  Klik op **uitvoeren** onder aan de pagina.  
5.  Nadat het proces is uitgevoerd, klikt u op **Web-Service implementeert**.
6.  Het sentiment analytics model is klaar voor gebruik. Klik op de knop **testen** om te valideren, en bieden tekstinvoer, zoals "Ik hou van Microsoft." De proef moet een resultaat te retourneren met de volgende strekking:

`'Predictive Mini Twitter sentiment analysis Experiment' test returned ["4","0.715057671070099"]...`  

![Stream Analytics Machine Learning, van analysegegevens](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-analysis-data.png)  

Klik op de koppeling voor **Excel 2010 of een oudere werkmap** voor uw API-sleutel en de URL die u later nodig om de Stream Analytics taak in te stellen in de kolom **Apps** . (Deze stap is vereist alleen een Machine Learning model uit een ander account Azure-werkruimte gebruiken. In dit artikel wordt ervan uitgegaan dat dit het geval is, om op te lossen dat scenario is.)  

Noteer de URL en toegang tot de sleutel voor de webservice van het gedownloade bestand in Excel, zoals hieronder wordt weergegeven:  

![Stream Analytics Machine Learning, overzicht](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  

## <a name="create-a-stream-analytics-job-that-uses-the-machine-learning-model"></a>Een Stream Analytics-functie die gebruikmaakt van de computer leren maken

1.  Ga naar de [portal Azure](https://manage.windowsazure.com).  
2.  Klik op **nieuwe** > **Data Services** > **Stream Analytics** > **snel tabellen maken**. Voer een naam voor de taak in **Naam van de taak**, voert u de juiste regio voor het project in **de regio**en selecteer de rekening in **Regionaal toezicht opslag Account**.    
3.  Klik op **een ingang toevoegen**nadat de taak is gemaakt, klikt u op het tabblad **invoer** .  

    ![Stream-Analytics Machine Learning, Machine Learning invoer toevoegen](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-input-screen.png)  

4.  Klik op **de gegevensstroom**op de eerste pagina van de wizard **Toevoegen** en klik op **volgende**. Op de volgende pagina op de **Blob-opslag** als invoer en klik vervolgens op **volgende**.  
5.  Geef op de pagina **Instellingen van Blob-opslag** van de wizard de opslag account blob containernaam die u eerder hebt gedefinieerd als u de gegevens hebt geüpload. Klik op **volgende**. **Gebeurtenis serialisatie-indeling**, klikt u op **CSV**. Accepteer de standaardwaarden voor de rest van de pagina **Instellingen voor serialisatie** . Klik op **OK**.  
6.  Klik op **uitvoer toevoegen**op het tabblad **uitvoer** .  

    ![Stream-Analytics Machine Learning, uitvoer toevoegen](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-output-screen.png)  

7.  **Blob-opslag**op en voer vervolgens de parameters, met uitzondering van de container. De waarde voor **invoer** is geconfigureerd voor het lezen van de container met de naam 'test' waar het **CSV-** bestand is geüpload. Voor **uitvoer**, geeft u "testoutput". Container-namen moeten verschillend zijn. Controleer of deze container.     
8.  Klik op **volgende** om de uitvoer van **serialisatie-instellingen**te configureren. Net als bij de **invoer**, **CSV**op en klik vervolgens op de knop **OK** .
9.  Klik op het tabblad **functies** **toevoegen een Machine Learning functie**.  

    ![Stream-Analytics Machine Learning, Machine Learning functie toevoegen](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-ml-function.png)  

10. Zoek op de pagina **Instellingen voor Machine Learning webservice** de Machine Learning werkruimte webservice en standaardeindpunt. De instellingen handmatig te krijgen bekend zijn met een webservice voor elke werkruimte configureren als u de URL kent, en de API-sleutel voor dit artikel is van toepassing. Voer de eindpunt- **URL** en **API-sleutel**. Klik op **OK**.    

    ![Stream Analytics Machine Learning, Machine Learning webservice](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-ml-web-service.png)    

11. Klik op het tabblad **Query** door de query als volgt te wijzigen:    

 ```
    WITH subquery AS (  
        SELECT text, sentiment(text) as result from input  
    )  
 
    Select text, result.[Scored Labels]  
    Into output  
    From subquery  
 ```    
12. Klik op **Opslaan** om de query opslaan.

## <a name="start-the-stream-analytics-job-and-observe-the-output"></a>Start de taak Stream Analytics en houd rekening met de uitvoer

1.  Klik op **Start** op de onderkant van de pagina taak.
2.  Klik op **Aangepaste tijd**in het **Dialoogvenster query's starten**en selecteer een tijd voorafgaand aan wanneer u het CSV geüpload naar een Blob-opslag. Klik op **OK**.  

    ![Stream Analytics Machine Learning, aangepaste tijd](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-custom-time.png)  

3.  Ga naar de Blob-opslag met behulp van het hulpprogramma voor het uploaden van het CSV-bestand, bijvoorbeeld Visual Studio.
4.  Een paar minuten nadat de taak is gestart, de uitvoer-container wordt gemaakt en een CSV-bestand wordt geüpload naar deze.  
5.  Open het bestand in de standaardeditor voor CSV. Iets met de volgende strekking worden weergegeven:  

    ![Stream Analytics Machine Learning, CSV weergeven](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  

## <a name="conclusion"></a>Conclusie

In dit artikel wordt beschreven hoe een Stream Analytics taak die streaming gegevens leest en sentiment analytics geldt voor de gegevens in real-time te maken. U kunt dit alles doen zonder u zorgen te maken over de kneepjes van het bouwen van een model sentiment analytics. Dit is een van de voordelen van de Cortana Intelligence Suite.

U kunt ook Azure Machine Learning functie-gerelateerde statistieken bekijken. Hiertoe klikt u op het tabblad **Monitor** . Drie functie-gerelateerde statistieken worden weergegeven.  

- **Functie aanvragen** geeft het aantal aanvragen dat is verzonden naar een webservice Machine Learning.  
- **Functie-gebeurtenissen** geeft het aantal gebeurtenissen in de aanvraag. Standaard bevat elke aanvraag naar een webservice Machine Learning maximaal 1000 gebeurtenissen.  
- **Mislukte aanvragen voor functie** geeft het aantal mislukte aanvragen naar de webservice van Machine Learning.  

    ![Stream Analytics Machine Learning, Machine Learning monitor weergeven](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-ml-monitor-view.png)  
