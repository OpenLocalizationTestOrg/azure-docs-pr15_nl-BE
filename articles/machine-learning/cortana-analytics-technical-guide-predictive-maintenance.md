<properties
    pageTitle="Technische handleiding voor de sjabloon Cortana Intelligence-oplossing voor preventief onderhoud in de ruimtevaart en andere bedrijven | Microsoft Azure"
    description="Een technische handleiding voor de sjabloon oplossing met Microsoft Cortana Intelligence voor preventief onderhoud in de ruimtevaart, hulpprogramma's en vervoer."
    services="cortana-analytics"
    documentationCenter=""
    authors="fboylu"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="fboylu" />

# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-predictive-maintenance-in-aerospace-and-other-businesses"></a>Technische handleiding voor de sjabloon Cortana Intelligence-oplossing voor preventief onderhoud in de ruimtevaart en andere bedrijven

## <a name="acknowledgements"></a>**Bevestigingen**
Dit artikel is geschreven door wetenschappers gegevens Yan Zhang, Gauher Shaheen, Fidan Boylu Uz en software engineering Dan Grecoe bij Microsoft.

## <a name="overview"></a>**Overzicht**

Sjablonen van de oplossing zijn te versnellen het proces van het bouwen van een demo E2E op Cortana Intelligence Suite ontworpen. Een geïmplementeerde sjabloon wordt je abonnement met de benodigde onderdelen voor Cortana Intelligence richt en hun onderlinge relaties opbouwen. Deze ook de seeding van de pijpleiding gegevens met voorbeeldgegevens gegenereerd op basis van een generator toepassing die u zal downloaden en installeren op uw lokale computer nadat u de sjabloon oplossing implementeren. De gegevens die zijn gegenereerd op basis van de generator wordt de pijpleiding gegevens en genereren van machine learning voorspellingen die vervolgens kunnen worden weergegeven op het dashboard Power BI start hydrate. Het implementatieproces begeleidt u bij de verschillende stappen voor het instellen van de referenties van uw oplossing. Zorg ervoor dat u deze referenties zoals oplossingsnaam, gebruikersnaam en wachtwoord dat u tijdens de installatie opgeeft vastleggen.  

Het doel van dit document wordt uitgelegd van de referentiearchitectuur en de verschillende onderdelen in uw abonnement als onderdeel van deze sjabloon oplossing ingericht. Het document is ook gesprekken voert over hoe u kunt de voorbeeldgegevens vervangen door echte gegevens van uw eigen inzichten en voorspellingen van uw eigen gegevens te zien zijn. Bovendien wordt het document beschreven de onderdelen van de sjabloon oplossing die worden gewijzigd moeten zou als u de oplossing met uw eigen gegevens aanpassen. Instructies over het bouwen van de Power BI-dashboard voor deze oplossing sjabloon beschikbaar aan het einde.

>[AZURE.TIP] U kunt downloaden en afdrukken van een [PDF-versie van dit document](http://download.microsoft.com/download/F/4/D/F4D7D208-D080-42ED-8813-6030D23329E9/cortana-analytics-technical-guide-predictive-maintenance.pdf).

## <a name="big-picture"></a>**Grote afbeelding**

![](media/cortana-analytics-technical-guide-predictive-maintenance\predictive-maintenance-architecture.png)

Wanneer de oplossing wordt geïmplementeerd, verschillende Azure services binnen Cortana Analytics Suite zijn geactiveerd (*dat wil zeggen* gebeurtenis Hub, Stream Analytics, HDInsight, Data Factory, Machine Learning, *enz.*). De van bovenstaande Architectuurdiagram wordt weergegeven, op een hoog niveau hoe het preventief onderhoud voor ruimtevaart oplossing sjabloon is opgebouwd uit een end-to-end. U zult kunnen deze services in azure portal door erop te klikken in de oplossing sjabloon diagram gemaakt met de implementatie van de oplossing, met uitzondering van HDInsight als deze service is ingericht op de vraag wanneer de pipeline gerelateerde activiteiten uitvoeren en verwijderde achteraf moeten onderzoeken.
U kunt een [volledige versie van het diagram](http://download.microsoft.com/download/1/9/B/19B815F0-D1B0-4F67-AED3-A40544225FD1/ca-topologies-maintenance-prediction.png)kunt downloaden.

De volgende secties beschrijven elk stuk.

## <a name="data-source-and-ingestion"></a>**Gegevensbron en opname**

### <a name="synthetic-data-source"></a>Synthetische gegevensbron

De gegevensbron gebruikt wordt voor deze sjabloon gegenereerd vanuit een bureaubladtoepassing die u wilt downloaden en lokaal worden uitgevoerd na een succesvolle implementatie. U vindt de instructies voor het downloaden en installeren van deze toepassing in de werkbalk Eigenschappen wanneer u het eerste knooppunt met de naam Generator voor preventief onderhoud gegevens in het diagram oplossing sjabloon selecteert. Deze toepassing-feeds voor de service [Azure gebeurtenis Hub](#azure-event-hub) met gegevenspunten of gebeurtenissen die worden gebruikt in de rest van de stroom van de oplossing. Deze gegevensbron bestaat uit of afgeleid van publiek beschikbare gegevens uit de [opslagplaats van NASA gegevens](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/) met behulp van de [gegevensset van turbofanmotoren Engine afbraak simulatie](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan).

Toepassing voor het genereren van de gebeurtenis wordt de gebeurtenis Azure Hub vullen alleen terwijl deze wordt uitgevoerd op uw computer.

### <a name="azure-event-hub"></a>Azure gebeurtenis hub

De service [Azure gebeurtenis Hub](https://azure.microsoft.com/services/event-hubs/) is de ontvanger van de invoer verstrekt door de synthetische gegevensbron hierboven beschreven.

## <a name="data-preparation-and-analysis"></a>**Voorbereiden van gegevens en analyse**


### <a name="azure-stream-analytics"></a>Azure Stream Analytics

De [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) -service wordt gebruikt in de buurt van real-time analytics op de invoerstroom uit de [Azure gebeurtenis Hub](#azure-event-hub) service bieden en publiceren van resultaten naar een dashboard voor [Power BI](https://powerbi.microsoft.com) , alsmede alle rauwe binnenkomende gebeurtenissen met de [Azure Storage](https://azure.microsoft.com/services/storage/) -service voor latere verwerking door de [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) service archiveren.

### <a name="hd-insights-custom-aggregation"></a>Aangepaste samenvoeging HD inzichten

De Azure HD inzicht service wordt [component](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) om scripts te voeren (ingedeeld door Azure Data Factory genoemd) om aggregaties op de ruwe gebeurtenissen die zijn gearchiveerd met de Azure Stream Analytics-service gebruikt.

### <a name="azure-machine-learning"></a>Azure Machine Learning

De service [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) (ingedeeld door Azure Data Factory genoemd) wordt gebruikt om voorspellingen over de resterende gebruiksduur (RUL) van een bepaalde vliegtuigen motor gegeven van de ontvangen invoer.

## <a name="data-publishing"></a>**Gegevens publiceren**


### <a name="azure-sql-database-service"></a>Azure SQL Database-Service

De service [Azure SQL-Database](https://azure.microsoft.com/services/sql-database/) wordt gebruikt om op te slaan (beheerd door Azure Data Factory genoemd) de voorspellingen ontvangen door de Azure Machine Learning-service die wordt verbruikt in de [Power BI](https://powerbi.microsoft.com) -dashboard.

## <a name="data-consumption"></a>**Data verbruik**

### <a name="power-bi"></a>Power BI

De [Power BI](https://powerbi.microsoft.com) -service wordt gebruikt voor het weergeven van een dashboard met aggregaties en waarschuwingen die door de [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) -service, alsmede RUL-voorspellingen in [Azure SQL-Database](https://azure.microsoft.com/services/sql-database/) opgeslagen die zijn gemaakt met behulp van de service [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) . Raadpleeg de sectie hieronder voor instructies over het bouwen van de Power BI-dashboard voor deze sjabloon oplossing.

## <a name="how-to-bring-in-your-own-data"></a>**Hoe om uw eigen gegevens**

In deze sectie wordt beschreven hoe u uw eigen gegevens naar Azure brengen en welke gebieden zouden moeten worden gewijzigd voor de gegevens die u wilt naar deze architectuur overbrengen.

Het is onwaarschijnlijk dat de gegevensset die wordt gebruikt door de [gegevensset van turbofanmotoren Engine afbraak simulatie](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan) voor deze sjabloon oplossing zou overeenkomen met elke gegevensset die u brengen. Wat zijn uw gegevens en de vereisten zijn cruciaal in hoe u deze sjabloon om te werken met uw eigen gegevens kunt wijzigen. Als dit uw eerste blootstelling aan de Azure Machine Learning-service, krijgt u een inleiding tot het met behulp van het voorbeeld in [het maken van uw eerste experiment](machine-learning-create-experiment.md).

In de volgende secties wordt uitgelegd hoe de secties van de sjabloon waarin wordt gevraagd wijzigingen als een nieuwe gegevensset wordt geïntroduceerd.

### <a name="azure-event-hub"></a>Azure gebeurtenis Hub

De service Azure gebeurtenis Hub is erg algemeen, zodat gegevens kunnen worden geboekt op de hub in CSV- of JSON-indeling. Geen speciale verwerking vindt plaats in de gebeurtenis Azure Hub, maar het is belangrijk dat u begrijpt dat de gegevens die worden ingevoerd in het.

Dit document wordt niet beschreven hoe u uw gegevens nemen, maar u kunt eenvoudig verzenden gebeurtenissen of gegevens op een Hub Azure gebeurtenis de gebeurtenis Hub-API gebruiken.

### <a name="azure-stream-analytics"></a>Azure Stream Analytics

De Azure Stream Analytics-service wordt gebruikt om door te lezen van gegevensstromen en het uitvoeren van gegevens naar een willekeurig aantal bronnen in de buurt van real-time analytics.

Voor het preventief onderhoud voor ruimtevaart oplossing sjabloon bestaat de Azure Stream Analytics query uit vier subquery's, elk beslag gebeurtenissen uit de service Azure gebeurtenis Hub en uitgangen op vier verschillende locaties hebben. Deze uitgangen bestaan uit drie Power BI datasets en één opslaglocatie Azure.

De query Azure Stream Analytics kunt u vinden door:

-   Aanmelden bij de Azure portal

-   Zoeken naar de stream analytics taken ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-stream-analytics.png) die zijn gegenereerd tijdens de oplossing is geïmplementeerd (*bijvoorbeeld*, **maintenancesa02asapbi** en **maintenancesa02asablob** voor de oplossing van preventief onderhoud)

-   Selecteren

    -   ***Ingangen*** voor de invoer voor de query weergeven

    -   De ***QUERY*** om de query zelf weer te geven

    -   - ***Hiermee kunt u*** de verschillende resultaten te bekijken

Informatie over het samenstellen van query Azure Stream Analytics vindt u in de [Stream Analytics Query Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) op MSDN.

De query's uitvoeren in deze oplossing, drie gegevenssets met in de buurt van real-time analytics informatie over de binnenkomende gegevensstroom naar een Power BI-dashboard dat geleverd als onderdeel van de sjabloon voor deze oplossing. Omdat er is een impliciete kennis over de gegevensindeling van de binnenkomende, zou deze query's moeten worden gewijzigd op basis van de gegevensindeling van uw.

De query in de tweede stroom analytics job- **maintenancesa02asablob** wordt gewoon alle [Hub gebeurtenis](https://azure.microsoft.com/services/event-hubs/) gebeurtenissen naar [Azure opslag](https://azure.microsoft.com/services/storage/) en dus geen verandering, ongeacht de gegevensindeling vereist, zoals gegevens van de volledige gebeurtenis wordt gestreamd naar opslag.

### <a name="azure-data-factory"></a>Azure Data Factory

De service [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) orchestrates het verkeer en de verwerking van gegevens. In de preventief onderhoud voor ruimtevaart oplossing sjabloon de fabriek gegevens bestaat uit drie [pijpleidingen](../data-factory/data-factory-create-pipelines.md) die verplaatsen en verwerken van de gegevens met behulp van verschillende technologieën.  U hebt toegang tot de fabriek gegevens via het het knooppunt Data Factory onderaan in het diagram oplossing sjabloon gemaakt met de implementatie van de oplossing. Hiermee gaat u naar de fabriek van de gegevens op de portal Azure. Als u fouten onder uw datasets ziet, kunt u die negeren omdat zij door data factory genoemd worden gedistribueerd voordat de generator van gegevens is gestart. Deze fouten dat niet uw data factory functioneert.

![](media/cortana-analytics-technical-guide-predictive-maintenance/data-factory-dataset-error.png)

In deze sectie wordt beschreven, de nodige [pijpleidingen](../data-factory/data-factory-create-pipelines.md) en [activiteiten](../data-factory/data-factory-create-pipelines.md) in de [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/). Hieronder vindt u de diagramweergave van de oplossing.

![](media/cortana-analytics-technical-guide-predictive-maintenance/azure-data-factory.png)

Twee van de pijpleidingen van deze fabriek [component](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) scripts bevatten die worden gebruikt voor het partitioneren en de gegevens samen te voegen. Wanneer wordt vermeld, zich de scripts worden bevindt in de [Opslag Azure](https://azure.microsoft.com/services/storage/) -account tijdens de installatie gemaakt. De locatie is: maintenancesascript\\\\script\\\\component\\ \\ (of https://[Your oplossing name].blob.core.windows.net/maintenancesascript).

Net als bij de [Azure Stream Analytics](#azure-stream-analytics-1) query's, de scripts [component](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) impliciete kennis over de gegevensindeling van de binnenkomende hebben, deze query's zou moeten worden gewijzigd op basis van de opmaak en [functie engineering](machine-learning-feature-selection-and-engineering.md) gegevensvereisten.

#### <a name="aggregateflightinfopipeline"></a>*AggregateFlightInfoPipeline*

Deze [pijpleiding](../data-factory/data-factory-create-pipelines.md) bevat een enkele activiteit - een [HDInsightHive](../data-factory/data-factory-hive-activity.md) met een [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) met een [component](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) -script voor het partitioneren van de gegevens die tijdens het project [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) in [Azure opslag](https://azure.microsoft.com/services/storage/) plaatsen.

De [component](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) van het script voor deze taak partitionering is ***AggregateFlightInfo.hql***

#### <a name="mlscoringpipeline"></a>*MLScoringPipeline*

Deze [pijpleiding](../data-factory/data-factory-create-pipelines.md) bevat verschillende activiteiten en waarvan het eindresultaat is dat de scored voorspellingen van de [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experimenteren gekoppeld aan deze sjabloon oplossing.

De activiteiten van dit deel zijn:

-   [HDInsightHive](../data-factory/data-factory-hive-activity.md) activiteit met behulp van een [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) waarop een script [component](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) aggregaties en nodig zijn voor het experiment [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) functie-engineering wordt uitgevoerd.
    De [component](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) van het script voor deze taak partitionering is ***PrepareMLInput.hql***.

-   [Kopie](https://msdn.microsoft.com/library/azure/dn835035.aspx) activiteit die de resultaten van de activiteit van de [HDInsightHive](../data-factory/data-factory-hive-activity.md) verplaatst naar een enkele [Opslag Azure](https://azure.microsoft.com/services/storage/) blob die door de [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) -activiteit worden kan.

-   [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) -activiteit die roept de [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experimenteren wat resulteert in de resultaten worden opgeslagen in een enkel [Opslag Azure](https://azure.microsoft.com/services/storage/) blob.

#### <a name="copyscoredresultpipeline"></a>*CopyScoredResultPipeline*

Deze [pijpleiding](../data-factory/data-factory-create-pipelines.md) bevat een enkele activiteit - een [kopie](https://msdn.microsoft.com/library/azure/dn835035.aspx) die de resultaten van de [Azure Machine Learning](#azure-machine-learning) experimenteren verplaatst van de ***MLScoringPipeline*** naar de [Azure SQL-Database](https://azure.microsoft.com/services/sql-database/) die is ingericht als onderdeel van de installatie van de sjabloon oplossing.

### <a name="azure-machine-learning"></a>Azure Machine Learning

De [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experiment gebruikt voor deze sjabloon oplossing biedt de resterende nuttige levensduur (RUL) van de motor van een vliegtuig. Het experiment is specifiek voor de gegevensset verbruikt en daarom moet u wijziging of vervanging van de gegevens die in wordt gebracht.

Zie voor meer informatie over hoe het experiment Azure Machine Learning is gemaakt [preventief onderhoud: stap 1 van 3, gegevens voorbereiden en engineering van de functie](http://gallery.cortanaanalytics.com/Experiment/Predictive-Maintenance-Step-1-of-3-data-preparation-and-feature-engineering-2).

## <a name="monitor-progress"></a>**Voortgang bewaken**
 Zodra de Generator van gegevens wordt gestart, de pijpleiding begint om gehydrateerd ophalen en de verschillende onderdelen van uw oplossing starten bij de start in actie volgende opdrachten door de Data Factory uitgegeven. Er zijn twee manieren waarop u kunt controleren de pijpleiding.

1. Een van de Stream Analytics taak schrijft de rauwe binnenkomende gegevens naar de blob-opslag. Als je op Blob-opslag-onderdeel van de oplossing van het scherm u met succes de oplossing wordt geïmplementeerd en klik vervolgens op openen in het rechterdeelvenster, ga je naar de [portal beheren](https://portal.azure.com/). Eenmaal, klikt u op BLOB's. In het volgende venster ziet u een lijst van Containers. Klik op **maintenancesadata**. In het volgende venster ziet u de map **Nieuwrap:** . In de map Nieuwrap: ziet u mappen met namen zoals uur = 17 uur = 18 enz. Als u deze mappen ziet, betekent dit dat de onbewerkte gegevens met succes is wordt gegenereerd op uw computer en opgeslagen in de blob-opslag. U ziet de CSV-bestanden die beperkte grootte in MB in die mappen moeten hebben.

2. De laatste stap van de pijpleiding is om gegevens te schrijven (bv. de voorspellingen van machine learning) in de SQL-Database. Mogelijk moet wacht maximaal drie uur voor de gegevens in een SQL-Database wordt weergegeven. Er is een manier om te controleren hoeveel gegevens zijn beschikbaar in de SQL-Database via [azure portal](https://manage.windowsazure.com/). Zoek in het linkerpaneel SQL-DATABASES ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-SQL-databases.png) en klik erop. Zoek uw database **pmaintenancedb** en vervolgens klikt u op. Klik op de volgende pagina onder Klik op beheren

    ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-manage.png).

    Hier kunt u op de nieuwe Query en de query voor het aantal rijen (bijvoorbeeld select count(*) van PMResult). Als uw database groeit, moet het aantal rijen in de tabel verhogen.


## <a name="power-bi-dashboard"></a>**Power BI-Dashboard**

### <a name="overview"></a>Overzicht

In deze sectie wordt beschreven hoe Power BI-dashboard instellen van Azure stream analytics (hot pad) real-time gegevens visualiseren en batch voorspelling resultaten van Azure machine learning (koude pad).

### <a name="setup-cold-path-dashboard"></a>Koude pad dashboard instellen

In de koude pad gegevens pipeline is de essentiële doelstelling om de voorspellende RUL van elke motor vliegtuig (resterende gebruiksduur) als een vlucht (cyclus) is voltooid. Het resultaat voorspelling wordt elke drie uur voor het voorspellen van de vliegtuigmotoren die een vlucht tijdens de afgelopen 3 uur hebt bijgewerkt.

Power BI maakt verbinding met een Azure SQL-database als gegevensbron, waarin de resultaten voorspelling worden opgeslagen. Opmerking: 1) bij de implementatie van uw oplossing, een echte voorspelling wordt weergegeven in de database binnen 3 uur.
Het pbix-bestand dat bij het downloaden van de Generator bevat bepaalde gegevens zodat u meteen de Power BI-dashboard kunt maken. 2) in deze stap is de vereiste download en installeer de gratis software [Power BI-bureaublad](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/).

De volgende stappen begeleiden u op het bestand pbix verbinding met de SQL-Database die is gesponnen van bij de implementatie van de oplossing met gegevens (*bv*. resultaten voorspelling) voor visualisatie.

1.  De databasereferenties ophalen.

    Voordat u naar de volgende stappen hebt u **database servernaam, databasenaam, gebruikersnaam en wachtwoord** nodig. Hier zijn de stappen om u te begeleiden hoe ze vinden.

    -   Als **' Azure SQL Database'** op uw oplossing sjabloon diagram groen wordt, klik erop en klik vervolgens op **'Openen'**.

    -   Hier ziet u een nieuw tabblad/venster waarin de Azure portal-pagina wordt weergegeven. Klik op **Resource-groepen** in het linkerpaneel.

    -   Selecteer het abonnement dat u gebruikt voor de implementatie van de oplossing en selecteer **' YourSolutionName\_ResourceGroup'**.

    -   In het nieuwe pop uit het Configuratiescherm, klikt u op de ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-sql.png) pictogram toegang tot uw database. De naam van de database bevindt zich naast het pictogram (*bijvoorbeeld*, **'pmaintenancedb'**) en de **naam van de databaseserver** wordt weergegeven bij de eigenschap Server name en moet uitzien **YourSoutionName.database.windows.net**.

    -   Uw **gebruikersnaam** en **wachtwoord** zijn hetzelfde als de gebruikersnaam en het wachtwoord eerder opgenomen tijdens de implementatie van de oplossing.

2.  De gegevensbron voor het rapport koude pad bijwerken met Power BI bureaublad.

    -   Dubbelklik in de map op uw PC waar u gedownload en uitgepakt, het bestand Generator op de **PowerBI\\PredictiveMaintenanceAerospace.pbix** bestand. Als u waarschuwingsberichten ziet wanneer u het bestand opent, deze genegeerd. Boven aan het bestand, klikt u op **Query's bewerken**.

        ![](media\cortana-analytics-technical-guide-predictive-maintenance\edit-queries.png)

    -   Hier ziet u twee tabellen, **RemainingUsefulLife** en **PMResult**. Selecteer de eerste tabel en klik op ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-query-settings.png) naast **'Bron'** onder **Stappen uit toegepast** op het rechterpaneel **Query-instellingen** . Negeer eventuele waarschuwingsberichten worden weergegeven.

    -   In de pop uit het venster **'Server'** en **'Database'** vervangen door uw eigen server en de databasenamen en klik op **'OK'**. Controleer of u de poort 1433 (**YourSoutionName.database.windows.net, 1433**) voor de naam van de server. Laat het veld Database als **pmaintenancedb**. Negeer de waarschuwingsberichten die worden weergegeven op het scherm.

    -   In de volgende pop uit het venster ziet u twee opties in het linkerdeelvenster (**Windows** en **Database**). Klik op **'Database'**en vul uw **'Gebruikersnaam'** en **'Wachtwoord'** (dit is de gebruikersnaam en het wachtwoord dat u hebt ingevoerd toen u eerst de oplossing wordt geïmplementeerd en Azure SQL-database wordt gemaakt). Controleer ***welk niveau toe te passen, deze instellingen te selecteren***, een optie voor database. Klik op **'Verbinding maken'**.

    -   Klikt u op de tweede tabel **PMResult** ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-navigation.png) naast **'Bron'** onder **Stappen uit toegepast** op het rechterpaneel van **Query-instellingen** en de namen en de database als in de bovenstaande stappen en klikt u op OK.

    -   Als u terug naar de vorige pagina bent geleid, sluit u het venster. Een bericht wordt pop-out - Klik op **toepassen**. Klik ten slotte op de knop **Opslaan** om de wijzigingen opslaan. De Power BI-bestand heeft nu vastgesteld voor verbinding met de server. Als uw visualisaties leeg zijn, moet dat u de selecties op de visualisaties alle gegevens visualiseren door te klikken op het pictogram van de gum op de rechterbovenhoek van de legenda's wissen. Gebruik de knop Vernieuwen om nieuwe gegevens op de visualisaties te weerspiegelen. In eerste instantie alleen ziet u de gegevens op uw visualisaties zoals de fabriek van de gegevens te vernieuwen om de 3 uur is gepland. Na 3 uur worden er nieuwe voorspellingen in uw visualisaties weergegeven wanneer u de gegevens vernieuwt.

3.  (Optioneel) Het dashboard koude pad naar [Power BI online](http://www.powerbi.com/)publiceren. Houd er rekening mee dat deze stap moet een Power BI-account (of Office 365-account).

    -   Klik op **'Publiceren'** en enkele seconden later verschijnt er een venster weergeven "Publicatie voor Power BI succes!" met een groen vinkje. Klik op de link onder 'Open PredictiveMaintenanceAerospace.pbix in Power BI'. Ga voor gedetailleerde instructies Zie [publiceren van Power BI-bureaublad](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).

    -   Een nieuw dashboard maken: klik op de **+** (+) naast de sectie **Dashboards** in het linkerdeelvenster. Voer de naam 'Demo van preventief onderhoud' voor deze nieuwe dashboard.

    -   Als u het rapport hebt geopend, klikt u op ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-pin.png) de visualisaties aan uw dashboard vastmaken. Gedetailleerde instructies, Zie [Pin een tegel om een Power BI-dashboard van een rapport](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
    Ga naar de dashboardpagina en aanpassen van de grootte en de locatie van uw visualisaties en bewerken van hun titels. Gedetailleerde instructies voor het bewerken van uw tegels, vindt [een naast elkaar: vergroten/verkleinen, verplaatsen, hernoemen, PIN-code, bewerken, verwijderen, hyperlink toevoegen](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Hier volgt een voorbeeld van de dashboard met bepaalde visualisaties koude pad is vastgemaakt aan het.  Afhankelijk van hoe lang u uw gegevens generator uitvoert, afwijken de nummers op de visualisaties.
    <br/>
    ![](media\cortana-analytics-technical-guide-predictive-maintenance\final-view.png)
<br/>
    -   Schema vernieuwen van de gegevens, de muiswijzer boven de dataset **PredictiveMaintenanceAerospace** , klikt u op ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-elipsis.png) en kies **Planning vernieuwen**.
<br/>
        **Opmerking:** Als er een waarschuwing massage, klik op **Referenties bewerken** en zorg ervoor dat zijn uw databasereferenties hetzelfde zijn als die welke zijn beschreven in stap 1.
<br/>
    ![](media\cortana-analytics-technical-guide-predictive-maintenance\schedule-refresh.png)
<br/>
    -   Vouw de sectie **Planning vernieuwen** . Schakel 'uw gegevens up-to-date houden'.
    <br/>
    -   Plan het vernieuwen op basis van uw behoeften. Ga voor meer informatie, Zie [gegevens vernieuwen in Power BI](https://support.powerbi.com/knowledgebase/articles/474669-data-refresh-in-power-bi).

### <a name="setup-hot-path-dashboard"></a>Hot pad dashboard instellen

De volgende stappen helpen u het visualiseren van real-time gegevensuitvoer van Stream Analytics taken die zijn gegenereerd op het moment van de implementatie van de oplossing. Een [On line Power BI](http://www.powerbi.com/) -account is vereist voor het uitvoeren van de volgende stappen uit. Als u geen account hebt, kunt u [er een maken](https://powerbi.microsoft.com/pricing).

1.  Output Power BI in Azure Stream Analytics (ASA) toevoegen.

    -  Moet u de instructies in [Azure Stream Analytics & Power BI: een real-time analytics dashboard voor real-time zichtbaarheid van gegevensstromen](stream-analytics-power-bi-dashboard.md) voor het instellen van de uitvoer van uw project Azure Stream Analytics als uw Power BI-dashboard.
    - De ASA-query heeft drie uitgangen die **aircraftmonitor**, **aircraftalert**en **flightsbyhour zijn**. U kunt de query weergeven door te klikken op het tabblad query. Overeenkomt met elk van deze tabellen, moet u een uitvoer toevoegen aan ASA. Wanneer u de eerste uitvoer (*bv.* **aircraftmonitor**) toevoegt zorg de **Uitvoeralias**, de **Naam gegevensset** en **Tabelnaam** de dezelfde (**aircraftmonitor**). Herhaal de stappen voor het toevoegen van outputs voor **aircraftalert**en **flightsbyhour**. Als u alle drie tabellen uitvoer en de ASA taak gestart hebt toegevoegd, ontvangt u een bevestigingsbericht wordt weergegeven (*bijvoorbeeld*, "Starting stream analytics taak maintenancesa02asapbi geslaagd").

2. Log in op [On line Power BI](http://www.powerbi.com)

    -   In het linkerpaneel Datasets sectie in mijn werkruimte de ***DATASET*** namen **aircraftmonitor**, **aircraftalert**en **flightsbyhour** moeten worden weergegeven. Dit is het stroomsgewijs verzenden van gegevens die u in de vorige stap van Azure Stream Analytics wordt geduwd. De dataset **flightsbyhour** mogelijk niet weergegeven op hetzelfde moment als de twee gegevenssets vanwege de aard van de onderliggende SQL-query. Echter, deze moet weergegeven na een uur.
    -   Zorg ervoor dat het deelvenster ***Visualisaties*** is geopend en aan de rechterkant van het scherm wordt weergegeven.

3. Zodra u de gegevens die in Power BI hebt, kunt u beginnen met het visualiseren van het stroomsgewijs verzenden van gegevens. Hieronder is een voorbeeld van de dashboard met bepaalde visualisaties hot pad vastgemaakt aan. U kunt andere dashboard tegels op basis van passende datasets. Afhankelijk van hoe lang u uw gegevens generator uitvoert, afwijken de nummers op de visualisaties.


    ![](media\cortana-analytics-technical-guide-predictive-maintenance\dashboard-view.png)

4. Hier volgen enkele stappen voor het maken van een van de bovenstaande tegels – naast de 'vloot weergave van Sensor 11 vs. drempel 48,26":

    -   Klik op de dataset **aircraftmonitor** in het linkerpaneel Datasets sectie.

    -   Klik op het pictogram **Lijndiagram** .

    -   Klik in het deelvenster **velden** **verwerkt** zodat onder "As" in het deelvenster **Visualisaties wordt** .

    -   Klik op "s11" en "s11\_waarschuwing ' dat ze beide worden weergegeven onder 'Waarden'. Klik op het pijltje naast de **s11** en **s11\_waarschuwing**, "Som" wijzigen in "Gemiddelde".

    -   Klik op **Opslaan** op de voorgrond en het rapport een naam 'aircraftmonitor'. Het rapport met de naam 'aircraftmonitor' wordt weergegeven in de sectie **rapporten** in het deelvenster **Navigator** aan de linkerkant.

    -   Klik op de **Pin visuele** pictogram in de rechterbovenhoek van deze lijndiagram. Een 'Pin-Dashboard'-venster kan voor u op een dashboard weergegeven. Selecteer 'Preventief onderhoud Demo' en klik vervolgens op "Pincode".

    -   Plaats de muis op deze naast elkaar op het dashboard, klik op het pictogram 'bewerken' in de rechterbovenhoek 'Vloot weergave van Sensor 11 vs. drempel 48,26' de titel en ondertitel "Gemiddelde over vloot na verloop van tijd" te wijzigen.

## <a name="how-to-delete-your-solution"></a>**Het verwijderen van uw oplossing**
Zorg ervoor dat u de generator van gegevens te stoppen wanneer de oplossing niet actief wordt gebruikt als de generator gegevens uitgevoerd hoger kosten zal. Verwijder de oplossing als u deze niet gebruikt. Uw oplossing verwijdert, worden alle onderdelen in uw abonnement ingericht als u de oplossing hebt geïmplementeerd. De oplossing op op de oplossingsnaam van uw in het linkerpaneel van de oplossing-sjabloon en klik op verwijderen om te verwijderen.

## <a name="cost-estimation-tools"></a>**Extra kostenraming**

De volgende twee hulpprogramma's zijn beschikbaar waarmee u beter inzicht in de totale kosten die betrokken zijn bij het preventief onderhoud van uw abonnement voor ruimtevaart oplossing sjabloon:

-   [Microsoft Azure kosten Estimator Tool (online)](https://azure.microsoft.com/pricing/calculator/)

-   [Microsoft Azure kosten Estimator Tool (desktop)](http://www.microsoft.com/download/details.aspx?id=43376)
