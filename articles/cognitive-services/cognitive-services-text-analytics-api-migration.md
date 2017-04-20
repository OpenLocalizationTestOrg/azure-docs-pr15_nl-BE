<properties
    pageTitle="Een upgrade naar versie 2 van de tekst Analytics API | Microsoft Azure"
    description="Azure Machine Learning Analytics tekst - Upgrade naar versie 2"
    services="cognitive-services"
    documentationCenter=""
    authors="onewth"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="onewth"/>

# <a name="upgrading-to-version-2-of-the-text-analytics-api"></a>Een upgrade naar versie 2 van de tekst Analytics API #

Deze handleiding gaat u aan de hand van de code van de [eerste versie van de API](../machine-learning/machine-learning-apps-text-analytics.md) voor het gebruik van de tweede versie upgraden. 

Als u de API niet hebt gebruikt en wilt weten, kunt u **[meer informatie over de API hier](//go.microsoft.com/fwlink/?LinkID=759711)** of **[Volg de gids aan de slag](//go.microsoft.com/fwlink/?LinkID=760860)**. Zie de **[Definitie van API](//go.microsoft.com/fwlink/?LinkID=759346)**voor technische achtergrondinformatie.

### <a name="part-1-get-a-new-key"></a>Deel 1. Een nieuwe sleutel ophalen ###

Eerst moet u een nieuwe API-sleutel ophalen uit de **Azure Portal**:

1. Ga naar de tekst Analytics-service door de [Galerie met Cortana Intelligence](//gallery.cortanaintelligence.com/MachineLearningAPI/Text-Analytics-2). Hier vindt u ook koppelingen naar de documentatie en codevoorbeelden.

1. Klik op **aanmelden**. Deze koppeling gaat u naar de beheerportal Azure waar u zich kunt aanmelden voor de service.

1. Selecteer een schema. U mag de **gratis tier voor 5.000 transacties per maand**. Een gratis plan is, wordt niet in rekening gebracht voor het gebruik van de service. U moet inloggen om uw abonnement op Azure. 

1. Nadat u zich voor tekst Analytics aanmeldt, krijgt u een **API-sleutel**. Deze sleutel is, moet u het gebruik van de API-services kopiëren.

### <a name="part-2-update-the-headers"></a>Deel 2. De headers bijwerken ###

Werk de kop ingediende waarden zoals hieronder wordt weergegeven. Houd er rekening mee dat de sleutel van de account niet is gecodeerd.

**Versie 1**

    Authorization: Basic base64encode(<your Data Market account key>)
    Accept: application/json

**Versie 2**

    Content-Type: application/json
    Accept: application/json
    Ocp-Apim-Subscription-Key: <your Azure Portal account key>


### <a name="part-3-update-the-base-url"></a>Deel 3. Bijwerken van de basis-URL ###

**Versie 1**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/

**Versie 2**

    https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/

### <a name="part-4a-update-the-formats-for-sentiment-key-phrases-and-languages"></a>Deel 4a. Update van de indelingen voor sentiment, belangrijke zinnen en talen ###

#### <a name="endpoints"></a>Eindpunten ####

GET eindpunten zijn nu afgekeurd, zodat alle invoer als een POST-aanvraag moet worden ingediend. Werk de eindpunten aan de filters die hieronder wordt weergegeven.

| |Één eindpunt versie 1|Batch-eindpunt versie 1|Eindpunt van versie 2|
|---|---|---|---|
|Type gesprek|Toevoegen|Verzenden|Verzenden|
|Sentiment|```GetSentiment```|```GetSentimentBatch```|```sentiment```|
|Belangrijke zinnen|```GetKeyPhrases```|```GetKeyPhrasesBatch```|```keyPhrases```|
|Talen|```GetLanguage```|```GetLanguageBatch```|```languages```|

#### <a name="input-formats"></a>Invoer-indelingen ####

Houd er rekening mee dat enige advertentie-indeling is nu geaccepteerd, zodat u alle input waar vroeger de eindpunten van één document dienovereenkomstig moet formatteren. "Inputs" zijn niet hoofdlettergevoelig.

**Versie 1 (batchverwerking)**

    {
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**Versie 2**

    {
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### <a name="output-from-sentiment"></a>Uitvoer van sentiment ####

**Versie 1**

    {
      "SentimentBatch":[{
        "Id":"string",
        "Score":"double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Versie 2**

    {
      "documents":[{
        "id":"string",
        "score":"double"
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### <a name="output-from-key-phrases"></a>Uitvoer van belangrijke zinnen ####

**Versie 1**

    {
      "KeyPhrasesBatch":[{
        "Id":"string",
        "KeyPhrases":["string"]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Versie 2**

    {
      "documents":[{
        "id":"string",
        "keyPhrases":["string"]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### <a name="output-from-languages"></a>Uitvoer van talen ####


**Versie 1**

    {
      "LanguageBatch":[{
        "id":"string",
        "detectedLanguages": [{
          "Score":"double"
          "Name":"string",
          "Iso6391Name":"string"
        }]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Versie 2**

    {
      "documents":[{
        "id":"string",
        "detectedLanguages": [{
          "score":"double"
          "name":"string",
          "iso6391Name":"string"
        }]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }


### <a name="part-4b-update-the-formats-for-topics"></a>Deel 4b. De indelingen voor onderwerpen bijwerken ###

#### <a name="endpoints"></a>Eindpunten ####

| |Eindpunt van versie 1 | Eindpunt van versie 2|
|---|---|---|
|Indienen voor de detectie van onderwerp (POST)|```StartTopicDetection```|```topics```|
|Veld onderwerp resultaten ophalen (GET)|```GetTopicDetectionResult?JobId=<jobId>```|```operations/<operationId>```|

#### <a name="input-formats"></a>Invoer-indelingen ####

**Versie 1**

    {
      "StopWords": [
        "string"
      ],
      "StopPhrases": [
        "string"
      ], 
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**Versie 2**

    {
      "stopWords": [
        "string"
      ],
      "stopPhrases": [
        "string"
      ],
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### <a name="submission-results"></a>Resultaten van indienen ####

**Versie 1 (POST)**

Wanneer de taak is voltooid, ontvangt u eerder, de volgende JSON-uitvoer, waarbij de taak-id zou worden toegevoegd aan een URL op te halen van de uitvoer.

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

**Versie 2 (POST)**

Het antwoord zal bevatten een header-waarde als volgt, waar `operation-location` wordt gebruikt als het eindpunt voor de resultaten controleren:

    'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'

#### <a name="operation-results"></a>Resultaat van de bewerking ####

**Versie 1 (GET)**

    {
      "TopicInfo" : [{
        "TopicId" : "string"
        "Score" : "double"
        "KeyPhrase" : "string"
      }],
      "TopicAssignment" : [{
        "Id" : "string",
        "TopicId" : "string",
        "Distance" : "double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Versie 2 (GET)**

Als voorheen, **regelmatig controleren van de uitvoer** (de voorgestelde periode is elke minuut) tot de uitvoer wordt geretourneerd. 

Wanneer de onderwerpen API is voltooid, een status lezen `succeeded` wordt geretourneerd. Dit zal het resultaat dan in de onderstaande notatie bevatten:

    {
        "status": "succeeded",
        "createdDateTime": "string",
        "operationType": "topics",
        "processingResult": {
            "topics" : [{
            "id" : "string"
            "score" : "double"
            "keyPhrase" : "string"
          }],
          "topicAssignments" : [{
            "topicId" : "string",
            "documentId" : "string",
            "distance" : "double"
          }],
          "errors" : [{
              "id":"string",
              "message":"string"
          }]
        }
    }

### <a name="part-5-test-it"></a>Deel 5. Het testen! ###

Nu moet u aan de slag! Test uw code met een kleine steekproef om ervoor te zorgen dat u uw gegevens kunt verwerken.
