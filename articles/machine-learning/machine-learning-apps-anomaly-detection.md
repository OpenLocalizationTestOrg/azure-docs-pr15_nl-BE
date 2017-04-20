<properties 
    pageTitle="Machine Learning app: afwijking Detection-Service | Microsoft Azure" 
    description="Afwijking detectie API is een voorbeeld gemaakt met Microsoft Azure Machine Learning die detecteert anomalieën in tijd reeksgegevens met numerieke waarden dat gelijke tussenruimten in de tijd hebben." 
    services="machine-learning" 
    documentationCenter="" 
    authors="alokkirpal" 
    manager="jhubbard"
    editor="cgronlun" /> 

<tags 
    ms.service="machine-learning" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="multiple" 
    ms.date="10/11/2016" 
    ms.author="alokkirpal"/>


# <a name="machine-learning-anomaly-detection-service"></a>Machine Learning afwijking Detection-Service#

##<a name="overview"></a>Overzicht

[Afwijking detectie API](https://datamarket.azure.com/dataset/aml_labs/anomalydetection) is een voorbeeld gemaakt met Azure Machine Learning die detecteert anomalieën in tijd reeksgegevens met numerieke waarden dat gelijke tussenruimten in de tijd hebben. 

Deze API kan detecteren van de volgende typen afwijkende patronen in tijd reeksgegevens:

* **Positieve en negatieve trends**: bijvoorbeeld wanneer geheugengebruik in een opwaartse trend computergebruik controleren zijn mogelijk van belang als het indicatief voor een geheugenlek zijn

* **Wijzigingen in het dynamische bereik van waarden**: bijvoorbeeld, als de uitzonderingen die door een cloud-service controleren, eventuele wijzigingen in het dynamische bereik van waarden kunnen duiden op instabiliteit van de gezondheid van de service, en

* **Bereikt en daalt**: bijvoorbeeld, als u het aantal mislukte aanmelding in een service of een aantal opdrachten om uitchecken in een e-commerce site controleren, spikes of Spanningsdips kunnen duiden op abnormaal gedrag.

Deze machine learning detectoren voor het bijhouden van dergelijke wijzigingen in waarden over tijd- en voortdurende wijzigingen in hun waarden als afwijking scores. Ze hoeven geen ad hoc drempel afstemmen en hun scores kunnen worden gebruikt om positieve voet false. De afwijking detectie API nuttig in verschillende scenario's is zoals service monitoring door KPI's bijhouden over de tijd, gebruik controleren door middel van statistieken zoals aantal zoekopdrachten, het aantal klikken, het controleren van de prestaties door middel van items, zoals geheugen, CPU, bestand wordt gelezen, enz. na verloop van tijd.

Afwijking detectie aangeboden wordt geleverd met handige hulpprogramma's om u aan de slag. 

* De [webtoepassing](http://anomalydetection-aml.azurewebsites.net/) helpt u bij het evalueren en de resultaten van de afwijking detectie API's op uw gegevens visualiseren. 

* De [voorbeeldcode](http://adresultparser.codeplex.com/) ziet u hoe u via programmacode toegang tot de API en de resultaten in C# worden geparseerd.

>[AZURE.NOTE]
>Probeer **inzichten die afwijking IT-oplossing** , aangedreven door [Deze API](https://datamarket.azure.com/dataset/aml_labs/anomalydetection)
>
>Voor deze complete oplossing geïmplementeerd op Azure abonnement <a href="https://gallery.cortanaintelligence.com/Solution/Anomaly-Detection-Pre-Configured-Solution-1" target="_blank"> **Begin hier >**</a>


##<a name="api-definition"></a>API, definitie

De service biedt een REST gebaseerde API via HTTPS, die kan worden gebruikt op verschillende manieren, met inbegrip van een web of mobiele toepassing, R, Python, Excel, enz.  U uw tijd reeksgegevens verzenden naar deze service via een REST API-aanroep en wordt een combinatie van de hierboven beschreven afwijking drie typen uitgevoerd. De service wordt uitgevoerd op de Azure Machine Learning platform dat naadloos wordt aangepast aan de behoeften van uw bedrijf en sla's.

###<a name="headers"></a>Berichtkoppen
Zorgen ervoor dat u de juiste kop in uw aanvraag, als volgt moet:

    Authorization: Basic <creds>
    Accept: application/json
               
    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

De sleutel van uw account kunt van uw account u vinden op de [Markt van Azure-gegevens](https://datamarket.azure.com/account/keys). 

###<a name="score-api"></a>Score-API

De Score-API wordt gebruikt voor het uitvoeren van detectie afwijking op tijd niet seizoensgebonden reeksgegevens. De API een aantal detectoren afwijking wordt uitgevoerd op de gegevens en hun scores afwijking retourneert. De onderstaande figuur toont een voorbeeld van de afwijkingen die de Score-API kan detecteren. Deze tijd is 2 verschillende verandert en 3 pieken. De rode stippen geven de tijd waarop de wijzigingen in het wordt ontdekt, terwijl de zwarte punten de pieken van de gedetecteerde geven.

![Score-API][1]
    
**URL**

    https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/Score 

**Voorbeeld hoofdgedeelte van de aanvraag**

In de onderstaande aanvraag verzenden we een tijdreeks met gegevenspunten van 1-3/2016 tot en met 10-3/2016 en de parameters van de Prikker detectoren (afgekapt weergegeven) naar de API voor het opsporen van als een van deze gegevenspunten afwijkende. 

    {
      "data": [
        [ "9/21/2014 11:05:00 AM", "3" ],
        [ "9/21/2014 11:10:00 AM", "9.09" ],
        [ "9/21/2014 11:15:00 AM", "0" ]
      ],
      "params": {
        "tspikedetector.sensitivity": "3",
        "zspikedetector.sensitivity": "3",
        "trenddetector.sensitivity": "3.25",
        "bileveldetector.sensitivity": "3.25",
        "postprocess.tailRows": "2"
      }
    }

Het antwoord hierop is: 

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/$metadata#AnomalyDetection.FrontEndService.Models.AnomalyDetectionResult",
      "ADOutput":"{
        "ColumnNames":["Time","Data","TSpike","ZSpike","rpscore","rpalert","tscore","talert"],
        "ColumnTypes":["DateTime","Double","Double","Double","Double","Int32","Double","Int32"],
        "Values":[
          ["9/21/2014 11:10:00 AM","9.09","0","0","-1.07030497733224","0","-0.884548154298423","0"],
          ["9/21/2014 11:15:00 AM","0","0","0","-1.05186237440962","0","-1.173800281031","0"]
        ]
      }"
    }

###<a name="scorewithseasonality-api"></a>ScoreWithSeasonality API

De API ScoreWithSeasonality wordt gebruikt voor afwijking detecteren in de tijdreeks die seizoensgebonden patronen hebben. Deze API is handig voor het detecteren van afwijkingen in seizoensgebonden patronen.  

In de volgende afbeelding toont een voorbeeld van de anomalieën gevonden in een tijdreeks seizoen. De tijd is een Prikker (1e zwarte stip), twee Spanningsdips (de 2e zwarte stip en één aan het einde) en een niveau wijzigen (rode punt). Merk op dat zowel de dip in het midden van de tijdreeksen en de wijzigingen in het alleen discernable na seizoen onderdelen worden verwijderd uit de serie.

![Seizoensgebonden API][2]

**URL**

    https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/ScoreWithSeasonality 

**Voorbeeld hoofdgedeelte van de aanvraag**

We sturen een tijdreeks (afgekapt weergegeven) met punten van 1-3/2016 tot en met 10-3/2016 en parameters voor de API voor het opsporen van als een van deze gegevenspunten afwijkende in de onderstaande aanvraag. 

    {
      "data": [
        [ "9/21/2014 11:10:00 AM", "9" ],
        [ "9/21/2014 11:15:00 AM", "12" ],
        [ "9/21/2014 11:20:00 AM", "10" ]
      ],
      "params": {
        "preprocess.aggregationInterval": "0",
        "preprocess.aggregationFunc": "mean",
        "preprocess.replaceMissing": "lkv",
        "postprocess.tailRows": "1",
        "zspikedetector.sensitivity": "3",
        "tspikedetector.sensitivity": "3",
        "upleveldetector.sensitivity": "3.25",
        "bileveldetector.sensitivity": "3.25",
        "trenddetector.sensitivity": "3.25",
        "detectors.historywindow": "500",
        "seasonality.enable": "true",
        "seasonality.transform": "deseason",
        "seasonality.numSeasonality": "1"
      }
    }

Het antwoord hierop is: 

    {
        "odata.metadata": "https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/$metadata#AnomalyDetection.FrontEndService.Models.AnomalyDetectionResult",
        "ADOutput": "{
            "ColumnNames":["Time","OriginalData","ProcessedData","TSpike","ZSpike","PScore","PAlert","RPScore","RPAlert","TScore","TAlert"],
            "ColumnTypes":["DateTime","Double","Double","Double","Double","Double","Int32","Double","Int32","Double","Int32"],
            "Values":[
                ["9/21/201411: 20: 00AM","10","10","0","0","-1.30229513613974","0","-1.30229513613974","0","-1.173800281031","0"]
            ]
        }"
    }

###<a name="detectors"></a>Detectoren

De afwijking detectie API ondersteunt detectoren in 3 categorieën. In de volgende tabel vindt u meer informatie over specifieke invoerparameters en outputs voor elke detector.

|Categorie detector|Detector|Beschrijving|Invoerparameters|Uitgangen
|---|---|---|---|---|
|Prikker detectoren|TSpike Detector|Spikes en Spanningsdips op basis van ver de waarden afkomstig zijn van de eerste en derde kwartielen detecteren|*tspikedetector.sensitivity:* neemt geheel getal in het bereik 1-10, standaard: 3; Hogere waarden wordt meer extreme waarden waardoor deze minder gevoelig variabel|TSpike: binaire waarden: '1' als een Prikker/dip is gedetecteerd, '0' anders|
||ZSpike Detector|Spikes en op basis van hoe ver de datapoints van hun gemiddelde Spanningsdips detecteren|*zspikedetector.sensitivity:* nemen geheel getal in het bereik 1-10, standaard: 3; Hogere waarden wordt meer extreme waarden waardoor deze minder gevoelig variabel|ZSpike: binaire waarden: '1' als een Prikker/dip is gedetecteerd, '0' anders|
|Detectie van langzame Trend|Detectie van langzame Trend|Detecteren van trage positieve trend aan de hand van de gevoeligheid instellen|*trenddetector.sensitivity:* drempel bij detector score (standaard: 3,25, 3,25 – 5 is een redelijk bereik selecteren uit; Hoe hoger de minder gevoelig)|TScore: aantal drijvende die afwijking score op trend|
|De wijzigingen in detectoren|De wijzigingen in één richting Detector|Niveau omhoog detecteren wijzigen aan de hand van de gevoeligheid instellen|*upleveldetector.sensitivity:* drempel bij detector score (standaard: 3,25, 3,25 – 5 is een redelijk bereik selecteren uit; Hoe hoger de minder gevoelig)|PScore: zwevende getal afwijking score omhoog op dat niveau wijzigen|
||Bidirectionele niveau wijzigen Detector|Detecteren van opwaartse en neerwaartse wijziging niveau aan de hand van de gevoeligheid instellen|*bileveldetector.sensitivity:* drempel bij detector score (standaard: 3,25, 3,25 – 5 is een redelijk bereik selecteren uit; Hoe hoger de minder gevoelig)|RPScore: drijvende-aantal die afwijking score op naar boven en naar beneden een niveau wijzigen

###<a name="parameters"></a>Parameters

Meer gedetailleerde informatie over deze invoerparameters wordt vermeld in de onderstaande tabel:

|Invoerparameters|Beschrijving|Standaardinstelling|Type|Geldig bereik|Voorgestelde bereik|
|---|---|---|---|---|---|
|preprocess.aggregationInterval|Aggregatie-interval in seconden voor het verzamelen van input tijdreeks|0 (geen samenvoeging wordt uitgevoerd)|geheel getal|0: aggregatie, 0 > anders overslaan|5 minuten voor 1 dag, afhankelijk van de tijd-serie
|preprocess.aggregationFunc|Functie die wordt gebruikt voor het verzamelen van gegevens in de opgegeven AggregationInterval|gemiddelde|geïnventariseerd|gemiddelde, som, lengte|N.V.T.|
|preprocess.replaceMissing|Waarden voor de ontbrekende gegevens worden toegerekend|lkv (laatste bekende waarde)|geïnventariseerd|nul, lkv, gemiddelde|N.V.T.|
|detectors.historyWindow|Geschiedenis (in aantal gegevenspunten) gebruikt voor het berekenen van afwijking-score|500|geheel getal|10-2000|Afhankelijk van de tijd-serie|
|upleveldetector.Sensitivity|Gevoeligheid voor opwaartse niveau detector wijzigen. |3,25|dubbel|Geen|3,25-5(Lesser values mean more sensitive)|
|bileveldetector.Sensitivity|Gevoeligheid voor bidirectionele niveau detector wijzigen. |3,25|dubbel|Geen|3,25-5(Lesser values mean more sensitive)|
|trenddetector.Sensitivity|De gevoeligheid van de detector positieve trend. |3,25|dubbel|Geen|3,25-5(Lesser values mean more sensitive)|
|tspikedetector.Sensitivity |Gevoeligheid voor TSpike Detector|3|geheel getal|1-10|3-5(Lesser values mean more sensitive)|
|zspikedetector.Sensitivity |Gevoeligheid voor ZSpike Detector|3|geheel getal|1-10 |3-5(Lesser values mean more sensitive)|
|seasonality.Enable|Of seizoensgebonden analyse wordt uitgevoerd|True|Boole-waarde|True, false|Afhankelijk van de tijd-serie|
|seasonality.numSeasonality |Maximum aantal periodieke cycli worden gedetecteerd|1|geheel getal|1, 2|1-2|
|seasonality.transform |Of seizoen (en) trend onderdelen moeten worden verwijderd voordat de detectie afwijking toe te passen|deseason|geïnventariseerd|geen, deseason, deseasontrend|N.V.T.|
|postprocess.tailRows |Het nummer van de meest recente gegevenspunten moeten worden behouden in het resultaat|0|geheel getal|0 (alle gegevenspunten houden), of geef het aantal punten in de resultaten|N.V.T.|

###<a name="output"></a>Uitvoer
De API alle detectoren de tijd reeksgegevens wordt uitgevoerd en geeft als resultaat de afwijking scores en binaire Prikker indicatoren voor elk punt in de tijd. De volgende tabel worden de resultaten van de API. 

|Uitgangen|Beschrijving|
|---|---|
|Tijd|Tijdstempels van onbewerkte gegevens of gegevens in geaggregeerde (en/of) toegerekende als aggregatie (en/of) ontbreekt aanrekening van de gegevens wordt toegepast.|
|OriginalData|Als de waarden van onbewerkte gegevens of gegevens in geaggregeerde (en/of) toegerekende aggregatie (en/of) ontbreekt aanrekening van de gegevens wordt toegepast.|
|ProcessedData|Een van de volgende handelingen uit: <ul><li>Seizoengezuiverd tijdreeks als significante seizoensgebonden is ontdekt en deseason optie geselecteerd.</li><li>seizoen aangepast en detrended tijdreeksen als significante seizoensgebonden is ontdekt en deseasontrend optie geselecteerd</li><li>anders is dit hetzelfde als OriginalData</li>|
|TSpike|Binaire aanduiding om aan te geven of een Prikker wordt gedetecteerd door TSpike Detector|
|ZSpike|Binaire aanduiding om aan te geven of een Prikker wordt gedetecteerd door ZSpike Detector|
|Pscore|Een getal met drijvende die afwijking score op opwaartse niveau wijzigen|
|Palert|1/0-waarde die aangeeft wordt er een niveau omhoog afwijking op basis van de gevoeligheid van de invoer wijzigen|
|RPScore|Een zwevende nummer die afwijking score op de wijzigingen in twee richtingen|
|RPAlert|1/0-waarde die aangeeft wordt er een niveau bidirectionele afwijking op basis van de gevoeligheid van de invoer wijzigen|
|TScore|Een zwevende nummer die afwijking score op positieve trend|
|TAlert|1/0-waarde die aangeeft er is een positieve trend afwijking op basis van de input gevoeligheid|


Deze uitvoer worden geanalyseerd met behulp van een [eenvoudige parser](https://adresultparser.codeplex.com/) - voorbeeldcode die laat zien hoe verbinding maken met de API en de uitvoer parseren heeft. De afwijkingen van de gevonden kunnen worden weergegeven op een dashboard en/of doorgegeven aan menselijke deskundigen voor corrigerende maatregelen of geïntegreerd in een ticketing systemen.


[1]: ./media/machine-learning-apps-anomaly-detection/anomaly-detection-score.png
[2]: ./media/machine-learning-apps-anomaly-detection/anomaly-detection-seasonal.png

 

 
