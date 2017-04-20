<properties
    pageTitle="Quick start guide: Machine Learning tekst Analytics API's | Microsoft Azure"
    description="Azure Machine Learning Analytics tekst - Quick Start Guide"
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

# <a name="getting-started-with-the-text-analytics-apis-to-detect-sentiment-key-phrases-topics-and-language"></a>Aan de slag met de tekst Analytics API's voor het detecteren van sentiment, belangrijke zinnen, onderwerpen en taal

<a name="HOLTop"></a>

Dit document wordt beschreven hoe aan boord de service of toepassing naar de [Tekst Analytics API's](//go.microsoft.com/fwlink/?LinkID=759711)gebruiken.
U kunt deze API's voor het detecteren van sentiment, belangrijke zinnen, onderwerpen en de taal van uw tekst. [Klik hier voor een interactieve demo van de ervaring.](//go.microsoft.com/fwlink/?LinkID=759712)

Raadpleeg de [API-definities](//go.microsoft.com/fwlink/?LinkID=759346) voor de technische documentatie voor API's.

Deze handleiding is bedoeld voor de API's van versie 2. Voor informatie over versie 1 van de API's, [raadpleegt u dit document](../machine-learning/machine-learning-apps-text-analytics.md).

Aan het einde van deze zelfstudie, is het mogelijk om via programmacode detecteren:

- **Sentiment** - tekst Is positief of negatief?

- **Sleutel zinnen** - wat mensen zijn in een enkel artikel bespreken?

- **Onderwerpen** - wat mensen zijn op veel artikelen bespreken?

- **Talen** - welke taal de tekst is geschreven in?

Houd er rekening mee dat deze API 1 transactie per document ingediend toeslagen. Als u bijvoorbeeld als u verzoekt om sentiment voor documenten in één aanroep 1000 wordt 1000 transacties afgetrokken.



<a name="Overview"></a>
## <a name="general-overview"></a>Algemeen overzicht ##

Dit document is een stapsgewijze handleiding. Ons doel is u stapsgewijs door de stappen die nodig zijn een model van de trein en wijs hulpbronnen, waarmee u plaatsen in productie. In deze oefening duurt ongeveer 30 minuten.

Voor deze taken, u moet een editor en de eindpunten RESTful aanroepen in uw taal van keuze.

We gaan aan de slag!

## <a name="task-1---signing-up-for-the-text-analytics-apis"></a>Taak 1 - handtekeningcertificaat voor de tekst Analytics API's ####

In deze taak zal u zich aanmelden voor de tekst analytics-service.

1. Ga naar **Cognitieve Services** in de [Portal Azure](//go.microsoft.com/fwlink/?LinkId=761108) en Controleer **Dat Analytics tekst** is geselecteerd als het type van het API.

1. Selecteer een schema. U mag de **gratis tier voor 5.000 transacties per maand**. Een gratis plan is, wordt niet in rekening gebracht voor het gebruik van de service. U moet inloggen om uw abonnement op Azure. 

1. Vul de overige velden in en maak uw account.

1. Nadat u zich voor tekst Analytics aanmelden uw **API-sleutel**niet vinden. De primaire sleutel, moet u het gebruik van de API-services kopiëren.


## <a name="task-2---detect-sentiment-key-phrases-and-languages"></a>Taak 2 - sentiment, belangrijke zinnen en talen detecteren ####

Het is eenvoudig te detecteren sentiment, belangrijke zinnen en talen in de tekst. Programmatisch krijgt u hetzelfde resultaat als de [demo ervaring](//go.microsoft.com/fwlink/?LinkID=759712) als resultaat gegeven.

>[AZURE.TIP] Sentiment analyse, wordt u aangeraden dat u de tekst in zinnen splitsen. Dit leidt meestal tot een hogere precisie in sentiment voorspellingen.

Houd er rekening mee dat de ondersteunde talen als volgt zijn:

| Functie | Ondersteunde talen |
|:-----|:----|
| Sentiment | `en`(Engels), `es` (Spaans), `fr` (Frans), `pt` (Portugees) |
| Belangrijke zinnen | `en`(Engels), `es` (Spaans), `de` (Duits), `ja` (Japans) |


1. U moet de headers instellen op de volgende. Opmerking JSON is momenteel de enige geaccepteerde invoer indeling voor API's. XML wordt niet ondersteund.

        Ocp-Apim-Subscription-Key: <your API key>
        Content-Type: application/json
        Accept: application/json

1. Vervolgens wordt uw invoer rijen in JSON opmaken. Voor sentiment, belangrijke zinnen en taal is de indeling hetzelfde. Houd er rekening mee dat elke-ID uniek zijn moet en de ID geretourneerd door het systeem. De maximale grootte van een document dat kan worden ingediend is 10KB en de totale maximumgrootte van ingediende input is 1MB. Niet meer dan 1000 documenten mogen worden ingediend in een gesprek. Snelheidsbeperking bestaat met een snelheid van 100 oproepen per minuut - daarom raden wij aan dat u grote hoeveelheden documenten in één aanroep indienen. De taal is een optionele parameter die moet worden opgegeven als niet-Engelse tekst te analyseren. Een voorbeeld van invoer hieronder, waar de optionele parameter `language` voor sentiment analyse of sleutel zin extractie is opgenomen:

        {
            "documents": [
                {
                    "language": "en",
                    "id": "1",
                    "text": "First document"
                },
                ...
                {
                    "language": "en",
                    "id": "100",
                    "text": "Final document"
                }
            ]
        }

1. **POST** gebeld op het systeem met de input voor sentiment, belangrijke zinnen en taal. De URL's ziet er als volgt:

        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages

1. Deze aanroep retourneert een JSON opgemaakt antwoord met de id's en eigenschappen gedetecteerd. Een voorbeeld van de uitvoer voor sentiment hieronder (met foutgegevens uitgesloten). In het geval van sentiment, wordt een score tussen 0 en 1 voor elk document worden geretourneerd:

        // Sentiment response
        {
            "documents": [
                {
                    "id": "1",
                    "score": "0.934"
                },
                ...
                {
                    "id": "100",
                    "score": "0.002"
                },
            ]
        }

        // Key phrases response
        {
            "documents": [
                {
                    "id": "1",
                    "keyPhrases": ["key phrase 1", ..., "key phrase n"]
                },
                ...
                {
                    "id": "100",
                    "keyPhrases": ["key phrase 1", ..., "key phrase n"]
                },
            ]
        }

        // Languages response
        {
            "documents": [
                {
                    "id": "1",
                    "detectedLanguages": [
                        {
                            "name": "English",
                            "iso6391Name": "en",
                            "score": "1"
                        }
                    ]
                },
                ...
                {
                    "id": "100",
                    "detectedLanguages": [
                        {
                            "name": "French",
                            "iso6391Name": "fr",
                            "score": "0.985"
                        }
                    ]
                }
            ]
        }


## <a name="task-3---detect-topics-in-a-corpus-of-text"></a>Taak 3 - onderwerpen in een corpus van tekst detecteren ####

Dit is een nieuw uitgebrachte API welke geeft de top onderwerpen voor een lijst met gevonden tekstrecords ingediend. Een onderwerp wordt aangeduid met een belangrijke zin die een of meer verwante woorden. De API is ontworpen om geschikt voor korte, menselijke geschreven tekst zoals beoordelingen en feedback van gebruikers.

Deze API vereist **een minimum van 100 tekstrecords** moet worden ingediend, maar is bedoeld voor het detecteren van onderwerpen over honderden tot duizenden records. Een niet-Engelse records of records met minder dan 3 woorden worden genegeerd en daarom wordt niet toegewezen aan onderwerpen. De maximale grootte van een document dat kan worden ingediend, wordt 30KB voor de detectie van onderwerp, en de totale maximumgrootte van invoer ingediend is 30MB. Onderwerp-detectie is de snelheid beperkt tot 5 inzendingen elke 5 minuten.

Er zijn twee extra **optionele** invoerparameters die u helpen kunnen bij het verbeteren van de kwaliteit van de resultaten:

- **Stopwoorden.**  Deze woorden en hun formulieren sluiten (bv. meervouden) worden uitgesloten van het hele onderwerp detectie pijpleiding. Gebruik deze voor gewone woorden (voor bijvoorbeeld 'probleem', 'fout' en 'gebruiker' mogelijk geschikte keuzen voor klachten over software). Elke tekenreeks moet een enkel woord.
- **Stop zinnen** - deze zinnen zal worden uitgesloten van de lijst van de geretourneerde onderwerpen. Gebruik deze algemene onderwerpen die u niet wilt zien in de resultaten uitgesloten. 'Microsoft' en "Azure" zou bijvoorbeeld keuzen die onderwerpen uit te sluiten. Tekenreeksen kunnen meerdere woorden bevatten.

Volg deze stappen om onderwerpen te vinden in de tekst.

1. De invoer in JSON opmaken. Deze keer definiëren stopwoorden en zinnen te stoppen.

        {
            "documents": [
                {
                    "id": "1",
                    "text": "First document"
                },
                ...
                {
                    "id": "100",
                    "text": "Final document"
                }
            ],
            "stopWords": [
                "issue", "error", "user"
            ],
            "stopPhrases": [
                "Microsoft", "Azure"
            ]
        }

1. Met dezelfde kopteksten als omschreven in de taak 2, moet een **POST** bellen naar het eindpunt van de onderwerpen:

        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/topics

1. Het resultaat is een `operation-location` als de koptekst in het antwoord, waarvan de waarde de URL van de query voor de resulterende onderwerpen is:

        'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'

1. De resulterende query `operation-location` regelmatig met een **GET** -verzoek. Eenmaal per minuut wordt aanbevolen.

        GET https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>

1. Het eindpunt wordt geretourneerd, een reactie met inbegrip van `{"status": "notstarted"}` vóór de verwerking, `{"status": "running"}` tijdens de verwerking en `{"status": "succeeded"}` met de uitvoer na voltooiing. U kunt vervolgens verbruiken de uitvoer die in de volgende indeling (Opmerking details zoals fout indeling en datums niet in dit voorbeeld opgenomen zijn):

        {
            "status": "succeeded",
            "operationProcessingResult": {
                "topics": [
                    {
                        "id": "8b89dd7e-de2b-4a48-94c0-8e7844265196"
                        "score": "5"
                        "keyPhrase": "first topic name"
                    },
                    ...
                    {
                        "id": "359ed9cb-f793-4168-9cde-cd63d24e0d6d"
                        "score": "3"
                        "keyPhrase": "final topic name"
                    }
                ],
                "topicAssignments": [
                    {
                        "topicId": "8b89dd7e-de2b-4a48-94c0-8e7844265196",
                        "documentId": "1",
                        "distance": "0.354"
                    },
                    ...
                    {
                        "topicId": "359ed9cb-f793-4168-9cde-cd63d24e0d6d",
                        "documentId": "55",
                        "distance": "0.758"
                    },            
                ]
            }
        }

Houd rekening met het antwoord op de succesvolle onderwerpen uit de `operations` eindpunt hebben het volgende schema:

    {
            "topics" : [{
                "id" : "string",
                "score" : "number",
                "keyPhrase" : "string"
            }],
            "topicAssignments" : [{
                "documentId" : "string",
                "topicId" : "string",
                "distance" : "number"
            }],
            "errors" : [{
                "id" : "string",
                "message" : "string"
            }]
        }

Uitleg over elk onderdeel van dit antwoord zijn als volgt:

**onderwerpen**

| Sleutel | Beschrijving |
|:-----|:----|
| ID | Een unieke id voor elk onderwerp. |
| score | Het aantal documenten dat is toegewezen aan een onderwerp. |
| keyPhrase | Een samengevat of meer woorden van het onderwerp. |

**topicAssignments**

| Sleutel | Beschrijving |
|:-----|:----|
| documentId | De id voor het document. Is gelijk aan de ID is opgenomen in de invoer. |
| onderwerpklasse | De onderwerp-ID die aan het document is toegewezen. |
| afstand | Document naar onderwerp aansluiting score tussen 0 en 1. De laagste een afstand score hoe sterker de connectie onderwerp is. |

**fouten**

| Sleutel | Beschrijving |
|:-----|:----|
| ID | Document de unieke id die de fout heeft betrekking op invoer. |
| Bericht | Het foutbericht verschijnt. |

## <a name="next-steps"></a>Volgende stappen ##

Gefeliciteerd! U hebt nu voltooid met tekst analytics op uw gegevens. U kunt nu zoeken in uw gegevens visualiseren met behulp van een hulpprogramma zoals [Power BI](//powerbi.microsoft.com) , evenals uw inzichten, zodat u een realtime weergave van uw gegevens te automatiseren.

Hoe tekst Analytics mogelijkheden, zoals sentiment, kunnen worden gebruikt als onderdeel van een bot, Zie het voorbeeld [Emotionele Bot](http://docs.botframework.com/en-us/bot-intelligence/language/#example-emotional-bot) op de site Bot Framework.
