<properties
    pageTitle="Machine Learning API's: Tekst Analytics | Microsoft Azure"
    description="Microsoft Machine Learning tekst Analytics API's kunnen worden gebruikt voor het analyseren van niet-gestructureerde tekst voor sentiment analyse, belangrijke woordgroep extractie, taaldetectie en detectie van onderwerp."
    services="machine-learning"
    documentationCenter=""
    authors="onewth"
    manager="jhubbard"
    editor="cgronlun"/> 

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="onewth"/>


# <a name="machine-learning-apis-text-analytics-for-sentiment-key-phrase-extraction-language-detection-and-topic-detection"></a>Machine Learning API's: Tekst Analytics voor Sentiment, belangrijke woordgroep extractie, taaldetectie en detectie van onderwerp

>[AZURE.NOTE] Deze handleiding is bedoeld voor versie 1 van de API. Versie 2, die [**verwijzen naar dit document**](../cognitive-services/cognitive-services-text-analytics-quick-start.md). Versie 2 is nu de beste versie van deze API.

## <a name="overview"></a>Overzicht

De tekst Analytics API is een suite van tekst analytics [webservices](https://datamarket.azure.com/dataset/amla/text-analytics) zijn gebouwd met Azure Machine Learning. De API kan worden gebruikt voor het analyseren van niet-gestructureerde tekst voor taken zoals het sentiment analyse, belangrijke woordgroep extractie, taaldetectie en detectie van onderwerp. Er hoeven geen trainingsgegevens gebruiken deze API: gewoon doen om uw gegevens. Deze API maakt gebruik van geavanceerde natuurlijke taal verwerking technieken best in class voorspellingen te leveren.

Op onze [demo-site](https://text-analytics-demo.azurewebsites.net/), waar u ook kunt vinden [voorbeelden](https://text-analytics-demo.azurewebsites.net/Home/SampleCode) over het implementeren van analytics tekst in C# en Python, kunt u tekst analytics in actie zien.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 

---

## <a name="sentiment-analysis"></a>Sentiment analyse

De API geeft als resultaat een numerieke score tussen 0 en 1. Scores dicht bij 1 geeft positieve sentiment, terwijl scores dicht bij 0 negatieve sentiment geven. Score van sentiment wordt gegenereerd op basis van classificatie-technieken. De input functies aan de classificatie bevatten n-g, gegenereerd op basis van een deel van speech tags en word insluitingen functies. Engels is momenteel de enige ondersteunde taal.
 
## <a name="key-phrase-extraction"></a>Extractie van belangrijke woordgroep

De API geeft als resultaat een lijst met tekenreeksen die de belangrijkste te bespreken punten in de ingevoerde tekst. Wij gebruiken technieken van geavanceerde verwerking van natuurlijke taal toolkit van Microsoft Office. Engels is momenteel de enige ondersteunde taal.

## <a name="language-detection"></a>Taaldetectie

De API retourneert de gedetecteerde taal en een numerieke score tussen 0 en 1. Scores dicht bij 1 geeft 100% zekerheid dat de vastgestelde taal ingesteld op true is. Een totaal van 120 talen worden ondersteund.

## <a name="topic-detection"></a>Detectie van onderwerp

Dit is een nieuw uitgebrachte API welke geeft de top onderwerpen voor een lijst met gevonden tekstrecords ingediend. Een onderwerp wordt aangeduid met een belangrijke zin die een of meer verwante woorden. Deze API vereist een minimum van 100 tekstrecords moeten worden ingediend, maar is bedoeld voor het detecteren van onderwerpen over honderden tot duizenden records. Houd er rekening mee dat deze API 1 transactie per record van tekst ingediend toeslagen. De API is ontworpen om geschikt voor korte, menselijke geschreven tekst zoals beoordelingen en feedback van gebruikers.

---

## <a name="api-definition"></a>API, definitie

### <a name="headers"></a>Berichtkoppen

Zorgen ervoor dat u de juiste kop in uw aanvraag, als volgt moet:

    Authorization: Basic <creds>
    Accept: application/json
               
    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

De sleutel van uw account kunt van uw account u vinden op de [Markt van Azure-gegevens](https://datamarket.azure.com/account/keys). Houd er rekening mee dat op dit moment alleen JSON voor invoer en uitvoer-indelingen wordt geaccepteerd. XML wordt niet ondersteund.

---

## <a name="single-response-apis"></a>Één antwoord API 's

### <a name="getsentiment"></a>GetSentiment

**URL** 

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment

**Voorbeeld van de aanvraag**

Tijdens het gesprek hieronder, verzoeken wij om sentiment analyse voor het woord "Hello World":

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment?Text=hello+world

Het resultaat is een antwoord als volgt:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
        "Score":1.0
    }

---

### <a name="getkeyphrases"></a>GetKeyPhrases

**URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases

**Voorbeeld van de aanvraag**

In de onderstaande oproep, verzoeken wij om dat de belangrijkste zinnen gevonden in de tekst "Was een geweldig hotel om te blijven, met een uniek decor en vriendelijke personeel":

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases?
    Text=It+was+a+wonderful+hotel+to+stay+at,+with+unique+decor+and+friendly+staff

Het resultaat is een antwoord als volgt:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
      "KeyPhrases":[
        "wonderful hotel",
        "unique decor",
        "friendly staff"
      ]
    }
 
---

### <a name="getlanguage"></a>GetLanguage

**URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguage

**Voorbeeld van de aanvraag**

In de onderstaande oproep krijgen aanvragen we voor het sentiment voor de belangrijkste zinnen in de tekst *Hallo wereld*

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguages?
    Text=Hello+World

Het resultaat is een antwoord als volgt:

    {
      "UnknownLanguage": false,
      "DetectedLanguages": [{
        "Name": "English",
        "Iso6391Name": "en",
        "Score": 1.0
      }]
    }

**Optionele parameters**

`NumberOfLanguagesToDetect`is een optionele parameter. De standaardwaarde is 1.

---

## <a name="batch-apis"></a>Batch-API 's

De tekst Analytics-service kunt u dit sentiment en sleutel zin extracties in de batchmodus. Houd er rekening mee dat elk van de records telt als één transactie gescoord. Als u bijvoorbeeld als u het sentiment voor 1000 records in één aanroep aanvragen wordt 1000 transacties afgetrokken.

Houd er rekening mee dat de id's in het systeem ingevoerd de id's geretourneerd door het systeem zijn. De webservice controleert niet of deze id's uniek zijn. Het is de verantwoordelijkheid van de beller uniekheid controleren. 


### <a name="getsentimentbatch"></a>GetSentimentBatch

**URL** 

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch

**Voorbeeld van de aanvraag**

In de aanroep van POST hieronder, verzoeken wij om voor de teksten van de vermeldingen "Hello World", "Hello Foo World" en "Hallo Mijn wereld' in de hoofdtekst van de aanvraag:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch 

Het hoofdgedeelte van de aanvraag:

    {"Inputs":
    [
        {"Id":"1","Text":"hello world"},
        {"Id":"2","Text":"hello foo world"},
        {"Id":"3","Text":"hello my world"},
    ]}

U krijgt de lijst van scores die is gekoppeld aan de id's van de tekst in de onderstaande reactie:

    {
      "odata.metadata":"<url>", 
      "SentimentBatch":
      [
        {"Score":0.9549767,"Id":"1"},
        {"Score":0.7767222,"Id":"2"},
        {"Score":0.8988889,"Id":"3"}
      ],  
      "Errors":[]
    }


---

### <a name="getkeyphrasesbatch"></a>GetKeyPhrasesBatch

**URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

**Voorbeeld van de aanvraag**

In dit voorbeeld, verzoeken wij om de lijst met patronen voor de belangrijkste zinnen in de volgende teksten: 

* "Het was een geweldig hotel om te blijven, met een uniek decor en vriendelijke personeel"
* 'Het was een fantastische build-conferentie, met zeer interessante lezingen'
* "Het verkeer is puberteit heeft weten, ik besteed aan drie uur op de luchthaven"

Deze aanvraag wordt ingediend als een POST-aanroep naar het eindpunt:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

Het hoofdgedeelte van de aanvraag:

    {"Inputs":
    [
        {"Id":"1","Text":"It was a wonderful hotel to stay at, with unique decor and friendly staff"},
        {"Id":"2","Text":"It was an amazing build conference, with very interesting talks"},
        {"Id":"3","Text":"The traffic was terrible, I spent three hours going to the airport"}
    ]}

U krijgt de lijst met belangrijke termen die zijn gekoppeld aan de id's van de tekst in het volgende antwoord:

    { "odata.metadata":"<url>",
        "KeyPhrasesBatch":
        [
           {"KeyPhrases":["unique decor","friendly staff","wonderful hotel"],"Id":"1"},
           {"KeyPhrases":["amazing build conference","interesting talks"],"Id":"2"},
           {"KeyPhrases":["hours","traffic","airport"],"Id":"3" }
        ],
        "Errors":[]
    }

---

### GetLanguageBatch

In the POST call below, we are requesting language detection for two text inputs:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguageBatch

Request body:

    {
      "Inputs": [
        {"Text": "hello world", "Id": "1"},
        {"Text": "c'est la vie", "Id": "2"}
      ]
    }

This returns the following response, where English is detected in the first input and French in the second input:

    {
       "LanguageBatch": [{
         "Id": "1",
         "DetectedLanguages": [{
            "Name": "Engels",
            "Iso6391Name": "en",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       },
       {
         "Id": "2",
         "DetectedLanguages": [{
            "Name": "Frans",
            "Iso6391Name": "fr",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       }],
       "Errors": []
    }

---

## <a name="topic-detection-apis"></a>Onderwerp detectie-API 's

Dit is een nieuw uitgebrachte API welke geeft de top onderwerpen voor een lijst met gevonden tekstrecords ingediend. Een onderwerp wordt aangeduid met een belangrijke zin die een of meer verwante woorden. Houd er rekening mee dat deze API 1 transactie per record van tekst ingediend toeslagen.

Deze API vereist een minimum van 100 tekstrecords moeten worden ingediend, maar is bedoeld voor het detecteren van onderwerpen over honderden tot duizenden records.


### <a name="topics--submit-job"></a>Onderwerpen: taak verzenden

**URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection

**Voorbeeld van de aanvraag**


In de onderstaande oproep boeken vragen we onderwerpen voor een set van 100 artikelen, waarbij de eerste en de laatste invoer artikelen worden weergegeven en de twee StopPhrases zijn opgenomen.

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection HTTP/1.1

Het hoofdgedeelte van de aanvraag:

    {"Inputs":[
        {"Id":"1","Text":"I loved the food at this restaurant"},
        ...,
        {"Id":"100","Text":"I hated the decor"}
    ],
    "StopPhrases":[
        "restaurant", “visitor"
    ]}

In de onderstaande reactie krijgt u de taak-id van de ingediende taak:

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

Een lijst met een enkel woord of meerdere word-zinnen die niet als onderwerpen moeten worden geretourneerd. Kan worden gebruikt voor het filteren van onderwerpen erg algemeen. Bijvoorbeeld in een dataset over hotel beoordelingen 'hotel' en 'hostel' mogelijk verstandige stop zinnen.  

### <a name="topics--poll-for-job-results"></a>Onderwerpen: Poll voor de resultaten van de taak

**URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult

**Voorbeeld van de aanvraag**

Geeft de taak-id geretourneerd van de ' Submit' taakstap om de resultaten ophalen. Het is raadzaam dat u dit eindpunt elke minuut totdat de Status belt 'Voltooid' = in het antwoord. Het duurt ongeveer 10 minuten voor een project voltooid of langer bij de vele duizenden records.

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult?JobId=<JobId>


Tijdens het verwerken, is het antwoord als volgt:

    {
        "odata.metadata":"<url>",
        "Status":"Running",
        "TopicInfo":[],
        "TopicAssignment":[],
        "Errors":[]
    }


De API wordt in JSON-indeling de volgende uitvoer weergegeven:

    {
        "odata.metadata":"<url>",
        "Status":"Finished",
        "TopicInfo":[
        {
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Score":8.0,
            "KeyPhrase":"food"
        },
        ...
        {
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Score":6.0,
            "KeyPhrase":"decor"
            }
        ],
        "TopicAssignment":[
        {
            "Id":"1",
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Distance":0.7809
        },
        ...
        {
            "Id":"100",
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Distance":0.8034
        }
        ],
        "Errors":[]


De eigenschappen voor elk deel van het antwoord zijn als volgt:

**TopicInfo eigenschappen**

| Sleutel | Beschrijving |
|:-----|:----|
| Onderwerpklasse | Een unieke id voor elk onderwerp. |
| Score | Het aantal records dat is toegewezen aan een onderwerp. |
| KeyPhrase | Een samengevat of meer woorden van het onderwerp. 1 of meerdere woorden kan zijn. |

**TopicAssignment eigenschappen**

| Sleutel | Beschrijving |
|:-----|:----|
| ID | De id van de record. Is gelijk aan de ID is opgenomen in de invoer. |
| Onderwerpklasse | De onderwerp-ID van de record is toegewezen. |
| Afstand | Vertrouwen dat de record bij het onderwerp hoort. Afstand dichter bij nul geeft een hogere betrouwbaarheid. |
