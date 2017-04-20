<properties 
    pageTitle="Voertuig telemetrie analytics oplossing playbook: grondige Kennismaking in de oplossing | Microsoft Azure" 
    description="Met de functionaliteit van Cortana Intelligence te krijgen van real-time en voorspellende inzichten voertuig gezondheid en rijden gewoonten." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-the-solution"></a>Voertuig telemetrie analytics oplossing playbook: grondige Kennismaking in de oplossing

Dit **menu** bevat koppelingen naar de secties van deze playbook: 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

Deze oefeningen sectie naar beneden in elk van de fasen in de architectuur van de oplossing met instructies en aanwijzingen voor de aanpassing. 

## <a name="data-sources"></a>Gegevensbronnen

De oplossing maakt gebruik van twee verschillende gegevensbronnen:

- **gesimuleerde voertuig signalen en diagnostische dataset** en 
- **voertuig catalogus**

Een voertuig telematica simulator maakt deel uit van deze oplossing. Het zendt diagnostische gegevens en signalen die de toestand van het voertuig en de bestuurder patroon op een bepaald moment in de tijd. Klik op het [Voertuig telematica Simulator](http://go.microsoft.com/fwlink/?LinkId=717075) om te downloaden van het **Voertuig telematica Simulator Visual Studio-oplossing** voor aanpassingen op basis van uw behoeften. De voertuig-catalogus bevat een verwijzing naar dataset met een Chassisnummer model toewijzen aan.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig2-vehicle-telematics-simulator.png)

*Figuur 2 – voertuig telematica Simulator*

Dit is een dataset JSON-indeling, met het volgende schema.

Kolom | Beschrijving | Waarden 
 ------- | ----------- | --------- 
VIN | Willekeurig gegenereerde voertuigidentificatienummer | Dit wordt verkregen uit een hoofdlijst met 10.000 voertuig willekeurig gegenereerde identificatienummers.
Buitentemperatuur | De buitentemperatuur waar het voertuig wordt bestuurd | Willekeurig gegenereerd getal van 0-100
Temperatuur van motor | De temperatuur van de motor van het voertuig | Willekeurige getal tussen 0-500
Snelheid | Het toerental waarbij het besturen van het voertuig | Willekeurig gegenereerd getal van 0-100
Brandstof | Het niveau van de brandstof van het voertuig | Willekeurig gegenereerd getal van 0-100 (geeft brandstof niveau percentage)
EngineOil | Het niveau van de olie motor van het voertuig | Willekeurig gegenereerd getal van 0-100 (geeft aan dat de motor olie niveau percentage)
Druk van de band | Druk op de band van het voertuig | Willekeurig gegenereerd getal van 0-50 (Bandenspecialist druk niveau percentage geeft)
Kilometerstand | De kilometerstand van het voertuig | Willekeurig gegenereerd getal van 0 200000
Accelerator_pedal_position | Het gaspedaal van de vorm van het voertuig | Willekeurig gegenereerd getal van 0-100 (accelerator niveau percentage geeft)
Parking_brake_status | Geeft aan of het voertuig geparkeerd is of niet | Waar of ONWAAR
Headlamp_status | Geeft aan waar het koplicht of op is | Waar of ONWAAR
Brake_pedal_status | Geeft aan of het rempedaal is ingedrukt of niet | Waar of ONWAAR
Transmission_gear_position | De positie van het vistuig overdracht van het voertuig | Staten: eerste, tweede, derde, vierde, vijfde, zesde, zevende, achtste
Ignition_status | Hiermee wordt aangegeven of het voertuig uitgevoerd of is gestopt | Waar of ONWAAR
Windshield_wiper_status | Geeft aan of de combinatie van de voorruit is ingeschakeld of niet | Waar of ONWAAR
ABS | Hiermee wordt aangegeven of ABS of niet uitoefent | Waar of ONWAAR
Tijdstempel | Het tijdstip waarop het gegevenspunt wordt gemaakt. | Datum
Stad | De locatie van het voertuig | 4 plaatsen in deze oplossing: Bellevue, Redmond, Sammamish, Amsterdam


VIN bevat de dataset voertuig model verwijzing naar de modeltoewijzing. 

VIN | Model |
--------------|------------------
FHL3O1SA4IEHB4WU1 | Sedan |
8J0U8XCPRGW4Z3NQE | Hybride |
WORG68Z2PLTNZDBI7 | Familie sedan |
JTHMYHQTEPP4WBMRN | Sedan |
W9FTHG27LZN1YWO0Y | Hybride |
MHTP9N792PHK08WJM | Familie sedan |
EI4QXI2AXVQQING4I | Sedan |
5KKR2VB4WHQH97PF8 | Hybride |
W9NSZ423XZHAONYXB | Familie sedan |
26WJSGHX4MA5ROHNL | Convertible |
GHLUB6ONKMOSI7E77 | Station Wagon |
9C2RHVRVLMEJDBXLP | Compacte auto |
BRNHVMZOUJ6EOCP32 | Kleine SUV |
VCYVW0WUZNBTM594J | Sportwagen |
HNVCE6YFZSA5M82NY | Middelgrote SUV |
4R30FOR7NUOBL05GJ | Station Wagon |
WYNIIY42VKV6OQS1J | Grote SUV |
8Y5QKG27QET1RBK7I | Grote SUV |
DF6OX2WSRA6511BVG | Coupé |
Z2EOZWZBXAEW3E60T | Sedan |
M4TV6IEALD5QDS3IR | Hybride |
VHRA1Y2TGTA84F00H | Familie sedan |
R0JAUHT1L1R3BIKI0 | Sedan |
9230C202Z60XX84AU | Hybride |
T8DNDN5UDCWL7M72H | Familie sedan |
4WPYRUZII5YV7YA42 | Sedan |
D1ZVY26UV2BFGHZNO | Hybride |
XUF99EW9OIQOMV7Q7 | Familie sedan
8OMCL3LGI7XNCC21U | Convertible |
…….  |   |


### <a name="to-generate-simulated-data"></a>Gesimuleerde gegevens genereren
1.  Als u wilt de gegevens simulator-pakket downloaden, klikt u op de pijl in de rechterbovenhoek van het voertuig telematica Simulator knooppunt. Opslaan en pak de bestanden lokaal op uw computer. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig3-vehicle-telemetry-blueprint.png)*Figuur 3 – voertuig telemetrie Analytics oplossing blauwdruk*

2.  Ga naar de map waarin u het voertuig telematica Simulator-pakket hebt uitgepakt op uw lokale computer. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-simulator-folder.png)*Figuur 4 – map voertuig telematica Simulator*

3.  De **CarEventGenerator.exe**van de toepassing worden uitgevoerd.

### <a name="references"></a>Verwijzingen

[Voertuig telematica Simulator Visual Studio-oplossing](http://go.microsoft.com/fwlink/?LinkId=717075) 

[Azure gebeurtenis Hub](https://azure.microsoft.com/services/event-hubs/)

[Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/)


## <a name="ingestion"></a>Opname
Combinaties van Azure gebeurtenis Hubs, Stream Analytics en Data Factory wordt gebruikt om aan te nemen van de signalen van het voertuig, de diagnostische gebeurtenissen en real-time en batch-analytics. Alle onderdelen zijn gemaakt en geconfigureerd als onderdeel van de implementatie van de oplossing. 

### <a name="real-time-analysis"></a>Real-time analyse
De gebeurtenissen die worden gegenereerd door de telematica voertuig Simulator worden op de Hub van de gebeurtenis met de gebeurtenis Hub SDK gepubliceerd. De taak van de Stream Analytics deze gebeurtenissen van de Hub van de gebeurtenis ingests en verwerkt de gegevens in real-time voor het analyseren van de toestand van het voertuig. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-event-hub-dashboard.png) 

*Figuur 5 - gebeurtenis Hub dashboard*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-processing-data.png) 

*Afbeelding 6 - Stream analytics taak verwerking van gegevens*

De stream analytics taak;

- gegevens van de gebeurtenis Hub ingests 
- een join met de verwijzingsgegevens naar het Chassisnummer van het voertuig worden toegewezen aan het bijbehorende model uitvoert 
- blijft deze bestaan in Azure blob-opslag voor rijke batch analytics. 

De volgende stream analytics-query wordt gebruikt om de gegevens in Azure blob-opslag. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 

*Figuur 7 - Stream analytics taak query voor ingestie van gegevens*

### <a name="batch-analysis"></a>Batchanalyse
We zijn ook een extra volume van gesimuleerde voertuig signalen en diagnostische dataset voor rijkere batch analytics genereren. Dit is nodig om een goede representatieve gegevensvolume voor batchverwerking. Voor dit doel gebruiken we een pijpleiding met de naam 'PrepareSampleDataPipeline' in de workflow Azure Data Factory voor het genereren van één jaar lang gesimuleerde voertuig signalen en diagnostische dataset. Klik op [aangepaste activiteit Data Factory](http://go.microsoft.com/fwlink/?LinkId=717077) downloaden Data Factory aangepaste DotNet activiteit Visual Studio-oplossing voor aanpassingen op basis van uw behoeften. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 

*Figuur 8 - voorbeeldgegevens voorbereiden voor batch-verwerking workflow*

De pijplijn bestaat uit een aangepaste ADF .net activiteit, hier weergeven:

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline.png) 

*Figuur 9 - PrepareSampleDataPipeline*

Zodra de pijplijn met succes uitgevoerd en 'RawCarEventsTable' dataset is gemarkeerd als 'Gereed', een jaar ter waarde van gesimuleerde voertuig signalen en diagnostische worden gegevens geproduceerd. Ziet u de volgende map en bestand in uw account opslag onder de container 'connectedcar' hebt gemaakt:

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

*Figuur 10 - PrepareSampleDataPipeline uitvoer*

### <a name="references"></a>Verwijzingen

[Azure gebeurtenis Hub SDK voor ingestie van stream](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

[Mogelijkheden van Azure Data Factory data verkeer](../data-factory/data-factory-data-movement-activities.md)
[Azure Data Factory DotNet activiteit](../data-factory/data-factory-use-custom-activities.md)

[Azure Data Factory DotNet activiteit visual studio-oplossing voor de voorbeeldgegevens voorbereiden](http://go.microsoft.com/fwlink/?LinkId=717077) 


## <a name="partition-the-dataset"></a>De dataset partitie

De ruwe semi-gestructureerde voertuig signalen en diagnostische dataset worden gepartitioneerd in de stap van de voorbereiding gegevens in een indeling van het jaar/maand. Deze partities bevordert efficiënter opvragen en schaalbare opslag op lange termijn doordat veroorzaakt dan van een blob-account naar het volgende als de eerste rekening wordt opgevuld. 

>[AZURE.NOTE] Deze stap in de oplossing is alleen van toepassing op batch-verwerking.

Invoer- en uitvoerfilters beheer van gegevens:

- **Uitvoergegevens** (aangeduid als *PartitionedCarEventsTable*) is een lange periode worden bewaard als het basissysteem / "rawest" vorm van gegevens in de van de klant "meer gegevens". 
- De **gegevens die zijn ingevoerd** met deze pijpleiding zou meestal worden verwijderd, omdat de uitvoergegevens vol vertrouwen naar de invoer heeft-net opgeslagen (gepartitioneerd) beter voor later gebruik.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-workflow.png)

*Figuur 11: partitie auto gebeurtenissen workflow*

De onbewerkte gegevens is gepartitioneerd met behulp van een component HDInsight activiteit in 'PartitionCarEventsPipeline'. De voorbeeldgegevens die in stap 1 wordt gegenereerd voor een jaar is per jaar/maand gepartitioneerd. De partities worden gebruikt voor het genereren van voertuig signalen en diagnostische gegevens voor elke maand (totaal 12 partities) van een jaar. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partition-car-events-pipeline.png)

*Figuur 12 - PartitionCarEventsPipeline*

Het volgende script component, met de naam 'partitioncarevents.hql', wordt gebruikt voor het partitioneren en bevindt zich in de map '\demo\src\connectedcar\scripts' van de gedownloade zip. 


    SET hive.exec.dynamic.partition=true;
    SET hive.exec.dynamic.partition.mode = nonstrict;
    set hive.cli.print.header=true;

    DROP TABLE IF EXISTS RawCarEvents; 
    CREATE EXTERNAL TABLE RawCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RAWINPUT}'; 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
    ) partitioned by (YearNo int, MonthNo int) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDOUTPUT}';

    DROP TABLE IF EXISTS Stage_RawCarEvents; 
    CREATE TABLE IF NOT EXISTS Stage_RawCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string,
                YearNo                          int,
                MonthNo                         int) 
    ROW FORMAT delimited fields terminated by ',' LINES TERMINATED BY '10';

    INSERT OVERWRITE TABLE Stage_RawCarEvents
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        Year(gendate),
        Month(gendate)

    FROM RawCarEvents WHERE Year(gendate) = ${hiveconf:Year} AND Month(gendate) = ${hiveconf:Month}; 

    INSERT OVERWRITE TABLE PartitionedCarEvents PARTITION(YearNo, MonthNo) 
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        YearNo,
        MonthNo
    FROM Stage_RawCarEvents WHERE YearNo = ${hiveconf:Year} AND MonthNo = ${hiveconf:Month};

*Figuur 13 - component PartitionConnectedCarEvents Script*

Nadat de pijplijn met succes is uitgevoerd, ziet u de volgende partities in je account opslag onder de container 'connectedcar' heeft gegenereerd.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-partitioned-output.png)

*Figuur 14 - gepartitioneerde uitvoer*

De gegevens nu is geoptimaliseerd, is klaar voor verdere verwerking om rijke batch inzicht krijgen en te beheren. 

## <a name="data-analysis"></a>Analyse van gegevens

In deze sectie kunt zien u hoe Azure Stream Analytics, Azure Machine Learning Azure Data Factory en Azure HDInsight combineren voor rich analytics advanced op de gezondheid van het voertuig en gewoonten te rijden. Er zijn hier drie subsecties:

1.  **Machine Learning**: deze subsectie bevat informatie over de detectie-experiment voor afwijking die we in deze oplossing gebruikt om te voorspellen vereisen onderhoud onderhoud voertuigen en voertuigen vereisen terughalen door problemen van de veiligheid.
2.  **Real-time analyse**: deze subsectie bevat informatie over de real-time analytics met behulp van de querytaal Stream Analytics en de dwarsas de machine learning experiment in real-time met behulp van een aangepaste toepassing.
3.  **Batchanalyse**: deze subsectie bevat informatie met betrekking tot het transformeren en verwerking van de gegevens van de batch met Azure HDInsight en Azure Machine Learning geoperationaliseerd door Azure Data Factory.

### <a name="machine-learning"></a>Machine Learning

Ons doel hier is om te voorspellen van de voertuigen waarvoor onderhoud of intrekken op basis van bepaalde statistieken Health. We maken de volgende veronderstellingen

- Als een van de volgende drie voorwaarden voldaan wordt, de voertuigen is **printeronderhoud**vereist onderhoud:
    - Bandenspecialist druk is laag
    - Motor olie niveau is laag
    - Temperatuur van motor is hoog.

- Als een van de volgende voorwaarden voldaan wordt, kunnen de voertuigen hebben een **probleem van de veiligheid** en vereisen **intrekken**:
    - Motor temperatuur is hoog maar buitentemperatuur laag is.
    - Motor temperatuur laag is maar de buitentemperatuur is hoog

Op basis van de vorige vereisten, hebben wij twee afzonderlijke modellen detecteren anomalieën: een voor de detectie van voertuig onderhoud en een voor de detectie van voertuig intrekken. In beide modellen, de ingebouwde Principal onderdeel Analysis (PCA) algoritme gebruikt voor de detectie van de afwijking. 

**Onderhoud detectie model**

Als een van de drie indicatoren - Bandenspecialist druk motorolie of motor temperatuur - voldoet aan de desbetreffende voorwaarde, meldt het model onderhoud detectie van een afwijking. Als gevolg hiervan moeten we alleen rekening te houden met deze drie variabelen bij het bouwen van het model. Bij ons experiment in Azure Machine Learning gebruiken we een module **Kolommen selecteren in de Dataset** eerst uitpakken van deze drie variabelen. We gebruiken de detectiemodule afwijking op basis van Pso vervolgens bouwen van het model afwijking detecteren. 

Principaal onderdeel Analysis (PCA) is een gevestigde techniek computer weten dat functies selecteren, classificatie en detectie van de afwijking kan worden toegepast. PCA zet een reeks geval mogelijk gecorreleerde variabelen, die in een reeks waarden principal onderdelen genoemd. Het belangrijkste idee van modellering op basis van Pso is om gegevens naar een lager-dimensionale ruimte zodat de functies en afwijkingen kunnen gemakkelijker worden geïdentificeerd.
 
Voor elke nieuwe invoer naar het model van de detectie van de detector afwijking berekent eerst de projectie van de eigenvectors en berekent vervolgens de genormaliseerde wederopbouw fout. Deze fout genormaliseerde is de score van de afwijking. Hoe hoger de fout, de afwijkende het exemplaar is. 

Elke record kan het probleem van de detectie van onderhoud worden beschouwd als een punt in een 3-dimensionale ruimte gedefinieerd door druk van de band, motorolie en motor temperatuur coördinaten. Voor het vastleggen van deze afwijkingen kunnen we de oorspronkelijke gegevens in de 3-dimensionale ruimte naar een 2-dimensionale ruimte met behulp van Pso project. Zo stelt u de parameter aantal onderdelen die u wilt gebruiken in de PCA 2. Deze parameter speelt een belangrijke rol bij de toepassing van de afwijking op basis van Pso detectie. Na projecteren van gegevens met behulp van PCA, kunnen we deze anomalieën gemakkelijker identificeren.

**Afwijking detectie model intrekken** In het model intrekken afwijking detecteren, gebruiken we de kolommen selecteren in de Dataset en afwijking op basis van Pso detectiemodules op een soortgelijke manier. Specifiek, uitpakken we eerst drie variabelen - motor temperatuur, buitentemperatuur en snelheid - met behulp van de module **Kolommen selecteren in de Dataset** . We ook de variabele snelheid aangezien de temperatuur van de motor is meestal gecorreleerd aan de snelheid. We vervolgens afwijking op basis van Pso detectiemodule gebruikt om de gegevens van de 3-dimensionale ruimte naar een 2-dimensionale ruimte. Het terughalen criteria is voldaan en het voertuig vereist intrekken wanneer de temperatuur van de motor en de buitentemperatuur zijn zeer negatief gecorreleerd. Algoritme voor de detectie op basis van Pso afwijking kunnen we de afwijkingen van de vastleggen na het uitvoeren van Pso. 

Wanneer het model training, moeten we normale welke gegevens geen vereist onderhoud of terughalen als de ingevoerde gegevens naar het model van de detectie op basis van Pso afwijking van de trein. In het experiment score gebruiken we het model opgeleide afwijking detecteren om te detecteren of het voertuig vereist onderhoud of intrekken. 


### <a name="real-time-analysis"></a>Real-time analyse

De volgende Stream Analytics SQL-Query wordt gebruikt om het gemiddelde van alle belangrijke voertuig parameters zoals de snelheid van het voertuig, brandstof niveau temperatuur motor, kilometerstand, Bandenspecialist druk, motor olie niveau en anderen. De gemiddelden worden gebruikt om de anomalieën, waarschuwingen, uitgeven en bepalen het algehele gezondheidsvoorschriften van voertuigen die in specifieke regio en deze vervolgens overeenstemmen met demografische gegevens. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig15-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

Figuur 15 – Stream analytics query voor real-time verwerking

Alle gemiddelden berekend over een TumblingWindow van 3 seconden. We gebruiken TubmlingWindow in dit geval omdat we is vereist voor niet-overlappende en aangrenzende tijdsintervallen. 

Klik voor meer informatie over alle mogelijkheden in Azure Stream Analytics "Windowing", [Windowing (Analytics Azure Stream)](https://msdn.microsoft.com/library/azure/dn835019.aspx).

**Real-time voorspelling**

Een toepassing maakt deel uit van de oplossing voor het model van de machine learning in real-time mogelijk maken. Deze toepassing met de naam 'RealTimeDashboardApp' wordt gemaakt en geconfigureerd als onderdeel van de implementatie van de oplossing. De toepassing wordt het volgende uitgevoerd:

1.  Luistert naar een gebeurtenis Hub exemplaar waar stroom Analytics de gebeurtenissen in een patroon voortdurend publiceert. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-stream-analytics-query-for-publishing.png)*Figuur 16 – Stream analytics query voor het publiceren van de gegevens op de output van een exemplaar van de gebeurtenis Hub* 

2.  Voor elke gebeurtenis die deze toepassing ontvangt: 

    - De gegevens met behulp van Machine Learning verzoek-antwoord scoren (RR's) eindpunt verwerkt. Het eindpunt van de Bronrecords automatisch gepubliceerd als onderdeel van de implementatie.
    - De uitvoer van Bronrecords is gepubliceerd naar een PowerBI dataset met behulp van de push API's.

Dit patroon is ook van toepassing op scenario's waarin u een toepassing Line of Business (LoB) integreren met de stroom real-time analytics voor scenario's zoals waarschuwingen, meldingen en berichten.

Klik op [RealtimeDashboardApp downloaden](http://go.microsoft.com/fwlink/?LinkId=717078) om te downloaden van de RealtimeDashboardApp Visual Studio-oplossing voor aanpassingen. 

**De Real-time dashboardtoepassing uitvoeren**

1.  Klik op het knooppunt PowerBI in de diagramweergave en klik op de koppeling 'Real-time dashboardtoepassing downloaden' in het deelvenster Eigenschappen. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17-vehicle-telematics-powerbi-dashboard-setup.png)*Figuur 17 – PowerBI dashboard installatie-instructies*
2.  Uitpakken en lokaal opslaan ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-realtimedashboardapp-folder.png) *figuur 18 – RealtimeDashboardApp-map*
3.  De RealtimeDashboardApp.exe van de toepassing uitvoeren
4.  Geldige referenties voor Power BI, log in en klik op accepteren ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png)![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

*Figuur 19: RealtimeDashboardApp: Aanmelden bij PowerBI*

>[AZURE.NOTE] Als u leegmaken van de dataset PowerBI wilt, de RealtimeDashboardApp met de parameter 'flushdata' worden uitgevoerd: 

    RealtimeDashboardApp.exe -flushdata

### <a name="batch-analysis"></a>Batchanalyse

Het doel hier is om weer te geven hoe Contoso Motors maakt gebruik van de mogelijkheden van Azure compute aan te wenden, grote gegevens te krijgen van de rijke inzichten op het patroon, gebruik gedrag en de gezondheid van het voertuig te rijden. Dit maakt het mogelijk om:

- Verbetering van de gebruikerservaring en goedkoper maken door middel van inzichten op gewoonten en efficiënte aangedreven gedrag brandstof rijden
- Proactief informatie over klanten en hun aangedreven patters zakelijke beslissingen te bepalen en de beste in zijn klasse producten & services

In deze oplossing willen we de volgende parameters:

1.  **Agressief gedrag van de bestuurder**: geeft de trend van de modellen, locaties, aangedreven voorwaarden en tijd van het jaar tot het inzicht in agressieve rijpatronen krijgen. Contoso Motors kunt deze inzichten voor marketingcampagnes, nieuwe persoonlijke functies en verzekering op basis van gebruik.
2.  **Efficiënte groot probleem brandstof**: geeft de trend van de modellen, locaties, aangedreven voorwaarden en tijd van het jaar krijgen inzichten op efficiënte rijpatronen van brandstof. Contoso Motors kunt deze inzichten voor marketingcampagnes, nieuwe functies te rijden en proactieve rapportage aan de stuurprogramma's voor effectieve en milieu vriendelijke aangedreven gewoonten. 
3.  **Intrekken van modellen**: modellen terughalen waarvoor de afwijking detectie machine learning experiment dwarsas identificeert

We bekijken in de details van elk van deze statistieken,


**Agressieve aangedreven patroon**

De gepartitioneerde voertuig signalen en diagnostische gegevens worden verwerkt in de pijplijn met de naam 'AggresiveDrivingPatternPipeline' component met de modellen, locatie, voertuig, rijomstandigheden en andere parameters te bepalen die agressieve aangedreven patroon vertoont.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-aggressive-driving-pattern.png) 
*Figuur 20 – agressief rijden patroon workflow*

Het onderdeel script met de naam "aggresivedriving.hql" gebruikt voor het analyseren van agressieve aangedreven voorwaarde patroon is te vinden in de map '\demo\src\connectedcar\scripts' van de gedownloade zip. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS CarEventsAggresive; 
    CREATE EXTERNAL TABLE CarEventsAggresive
    (
                vin                         string, 
                model                       string,
                timestamp                   string,
                city                        string,
                speed                       string,
                transmission_gear_position  string,
                brake_pedal_status          string,
                Year                        string,
                Month                       string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:AGGRESIVEOUTPUT}';



    INSERT OVERWRITE TABLE CarEventsAggresive
    select
    vin,
    model,
    timestamp,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND brake_pedal_status = '1' AND speed >= '50'

*Figuur 21: agressief rijden patroon component query*

De combinatie van transmission vistuig positie, rem pedaalkracht status en snelheid van het voertuig wordt gebruikt voor het detecteren van roekeloze/agressief aangedreven gedrag op basis van een patroon met hoge snelheid te remmen. 

Nadat de pijplijn met succes is uitgevoerd, ziet u de volgende partities in je account opslag onder de container 'connectedcar' heeft gegenereerd.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aggressive-driving-pattern-output.png) 

*Figuur 22 – AggressiveDrivingPatternPipeline uitvoer*


**Efficiënte aangedreven patroon brandstof**

De gepartitioneerde voertuig signalen en diagnostische gegevens worden verwerkt in de pijplijn met de naam 'FuelEfficientDrivingPatternPipeline'. Component wordt gebruikt voor het bepalen van de modellen, locatie voertuig, rijomstandigheden en andere eigenschappen die brandstof efficiënte aangedreven patroon vertonen.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-fuel-efficient-driving-pattern.png) 

*Figuur 23: brandstof efficiënte aangedreven patroon workflow*

Het onderdeel script met de naam "fuelefficientdriving.hql" gebruikt voor het analyseren van agressieve aangedreven voorwaarde patroon is te vinden in de map '\demo\src\connectedcar\scripts' van de gedownloade zip. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS FuelEfficientDriving; 
    CREATE EXTERNAL TABLE FuelEfficientDriving
    (
                vin                         string, 
                model                       string,
                city                        string,
                speed                       string,
                transmission_gear_position  string,                
                brake_pedal_status          string,            
                accelerator_pedal_position  string,                             
                Year                        string,
                Month                       string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:FUELEFFICIENTOUTPUT}';



    INSERT OVERWRITE TABLE FuelEfficientDriving
    select
    vin,
    model,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    accelerator_pedal_position,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND parking_brake_status = '0' AND brake_pedal_status = '0' AND speed <= '60' AND accelerator_pedal_position >= '50'


*Figuur 24: brandstof efficiënte aangedreven patroon component query*

De combinatie van de positie van het voertuig transmission vistuig wordt gebruikt, rem pedaalkracht status, snelheid en accelerator pedaal positie voor het detecteren van brandstof efficiënte aangedreven werking van versnelling, remmen, en patronen te versnellen. 

Nadat de pijplijn met succes is uitgevoerd, ziet u de volgende partities in je account opslag onder de container 'connectedcar' heeft gegenereerd.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

*Figuur 25: FuelEfficientDrivingPatternPipeline uitvoer*


**Voorspellingen intrekken**

De machine learning experiment is ingericht en gepubliceerd als een webservice als onderdeel van de implementatie van de oplossing. De batch scoren eindpunt wordt overgenomen in deze workflow, geregistreerd als een service factory gekoppeld en geoperationaliseerd met data factory batch-activiteit te scoren.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-machine-learning-endpoint.png) 

*Figuur 26: Machine learning eindpunt is geregistreerd als een gekoppelde service in data factory*

De geregistreerde gekoppelde service wordt gebruikt in de DetectAnomalyPipeline voor het verkrijgen van de gegevens met behulp van het model afwijking detecteren. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-aml-batch-scoring.png) 

*Figuur 27: Azure Machine Learning Batch scoren activiteit in data factory* 

Er zijn enkele stappen die worden uitgevoerd in deze pijpleiding voor gegevensvoorbereiding zodat deze met de webservice scoren batch kan worden geoperationaliseerd. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-pipeline-predicting-recalls.png) 

*Figuur 28 – DetectAnomalyPipeline voor het voorspellen van voertuigen waarvoor terughalen* 

Als het scoren is voltooid, wordt een HDInsight activiteit te verwerken en aggregatie van de gegevens die gecategoriseerd als afwijkingen van de door het model met een score van de waarschijnlijkheid van 0,60 of hoger zijn gebruikt.

    DROP TABLE IF EXISTS CarEventsAnomaly; 
    CREATE EXTERNAL TABLE CarEventsAnomaly 
    (
                vin                         string,
                model                       string,
                gendate                     string,
                outsidetemperature          string,
                enginetemperature           string,
                speed                       string,
                fuel                        string,
                engineoil                   string,
                tirepressure                string,
                odometer                    string,
                city                        string,
                accelerator_pedal_position  string,
                parking_brake_status        string,
                headlamp_status             string,
                brake_pedal_status          string,
                transmission_gear_position  string,
                ignition_status             string,
                windshield_wiper_status     string,
                abs                         string,
                maintenanceLabel            string,
                maintenanceProbability      string,
                RecallLabel                 string,
                RecallProbability           string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:ANOMALYOUTPUT}';

    DROP TABLE IF EXISTS RecallModel; 
    CREATE EXTERNAL TABLE RecallModel 
    (

                vin                         string,
                model                       string,
                city                        string,
                outsidetemperature          string,
                enginetemperature           string,
                speed                       string,
                Year                        string,
                Month                       string              
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RECALLMODELOUTPUT}';

    INSERT OVERWRITE TABLE RecallModel
    select
    vin,
    model,
    city,
    outsidetemperature,
    enginetemperature,
    speed,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from CarEventsAnomaly
    where RecallLabel = '1' AND RecallProbability >= '0.60'


Nadat de pijplijn met succes is uitgevoerd, ziet u de volgende partities in je account opslag onder de container 'connectedcar' heeft gegenereerd.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-detect-anamoly-pipeline-output.png) 

*Figuur 30 – figuur 30 – DetectAnomalyPipeline uitvoer*


## <a name="publish"></a>Publiceren

### <a name="real-time-analysis"></a>Real-time analyse

Een query in de stream analytics taak publiceert de gebeurtenissen naar uitvoer gebeurtenis Hub exemplaar. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig31-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

*Figuur 31 – Stream analytics project naar de output van een publiceert exemplaar van de gebeurtenis Hub*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig32-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

*Figuur 32 – analytics query om te publiceren naar de output Stream gebeurtenis Hub exemplaar*

Deze stroom van gebeurtenissen wordt verbruikt door de RealTimeDashboardApp die deel uitmaken van de oplossing. Deze toepassing maakt gebruik van de Machine Learning verzoek-antwoord-webservice voor het scoren van real-time en de resulterende gegevens publiceert naar een dataset PowerBI voor consumptie. 

### <a name="batch-analysis"></a>Batchanalyse

De resultaten van de batchverwerking en real-time verwerking worden gepubliceerd naar de Azure SQL-Database-tabellen voor consumptie. De Azure SQL Server-Database en de tabellen worden automatisch gemaakt als onderdeel van het installatiescript. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig33-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

*Figuur 33: resultaten kopiëren naar data mart workflow batchverwerking*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig34-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

*In figuur 34 – Stream analytics project naar de datamart publiceert*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig35-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

*Figuur 35-datamart in stream analytics taak instellen*


## <a name="consume"></a>Verbruiken

Power BI biedt deze oplossing een rijke dashboard voor realtime-gegevens en voorspellende analytics visualisaties. 

Klik hier voor gedetailleerde instructies voor het instellen van de rapporten van de PowerBI en het dashboard. Het uiteindelijke dashboard ziet er zo uit:

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig36-vehicle-telematics-powerbi-dashboard.png)

*Figuur 36 - PowerBI-Dashboard*

## <a name="summary"></a>Samenvatting

Dit document bevat gedetailleerde DrillDown voertuig telemetrie Analytics oplossing. Dit patroon met een lambda-architectuur voor toonbeeld geeft real-time en batch-analytics met voorspellingen en acties. Dit patroon is van toepassing op een breed scala van use-cases waarvoor hot pad (real-time) en analytics koude pad (batch). 
