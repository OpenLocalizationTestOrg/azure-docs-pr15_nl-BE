
<properties
    pageTitle="Aanbevelingen ophalen in batches: Machine learning aanbevelingen API | Microsoft Azure"
    description="Azure machine learning aanbevelingen--aanbevelingen ophalen in batches"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="luisca"/>

# <a name="get-recommendations-in-batches"></a>Aanbevelingen krijgen in batches

>[AZURE.NOTE] Aanbevelingen ophalen in batches is ingewikkelder dan aanbevelingen één tegelijk ophalen. De API's voor informatie over het verkrijgen van aanbevelingen voor een enkele aanvraag controleren:

> [Aanbevelingen van item naar Item](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d4)<br>
> [Gebruiker-Item-aanbevelingen](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3dd)
>
> Batch scoren werkt alleen voor builds die zijn gemaakt na 21 juli 2016.


Er zijn situaties waarin u moet aanbevelingen krijgen voor meer dan één item tegelijk. Bijvoorbeeld, u mogelijk bent geïnteresseerd in een cache aanbevelingen maken of zelfs analyseren welke aanbevelingen die u krijgt.

Score batchbewerkingen zijn als we deze worden aangeroepen, asynchrone bewerkingen. U moet de aanvraag indienen en verzamelt de resultaten vervolgens wachten tot de bewerking is voltooid.  

Als u meer nauwkeurige, dit zijn de stappen te volgen:

1.  Een container Azure opslag maken als u geen sjabloon hebt.
2.  Upload een invoerbestand op dat de beschrijving van elk van uw aanbeveling aanvragen voor Azure Blob-opslag.
3.  De batchverwerking score snel beginnen.
4.  Wacht tot de asynchrone bewerking is voltooid.
5.  Als de bewerking is voltooid, verzamelt de resultaten van de Blob-opslag.

We doorlopen die elk van deze stappen.

## <a name="create-a-storage-container-if-you-dont-have-one-already"></a>Container opslag maken als u geen sjabloon hebt

Ga naar de [Azure portal](https://portal.azure.com) en maak een nieuwe opslag-account als u nog geen één hebt. Ga hiervoor naar **Nieuw** > **Data** + **opslag** > **Opslag Account**.

Nadat u een opslag-account hebt, moet u voor het maken van de blob containers waar u de invoer en uitvoer van de Batchuitvoering wilt opslaan.

Upload een invoerbestand op dat de beschrijving van elk van uw aanbeveling om een Blob-opslag vraagt we bellen het bestand input.json.
Nadat u een container hebt, moet u voor het uploaden van een bestand met een beschrijving van elk van de aanvragen die u nodig hebt voor het uitvoeren van de service voor aanbevelingen.

Een partij kan slechts één type aanvraag van een specifieke versie uitvoeren. Het definiëren van deze gegevens in de volgende sectie wordt uitgelegd. Nu gaan we ervan uit dat we aanbevelingen uit een build van het specifieke item zal uitvoeren. Het invoerbestand bevat de informatie over (in dit geval de artikelen zaad) voor elk van de aanvragen.

Dit is een voorbeeld van het bestand input.json ziet er als volgt:

    {
      "requests": [
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F34-03453" ] },
      { "SeedItems": [ "D16-3244" ] },
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F43-01467" ] },
      { "SeedItems": [ "BD5-06013" ] },
      { "SeedItems": [ "P45-00163", "FKF-00689" ] },
      { "SeedItems": [ "C9A-69320" ] }
      ]
    }

Zoals u ziet, is het bestand een JSON-bestand, waarbij elk van de aanvragen de informatie die nodig is is voor het verzenden van een aanvraag aanbevelingen. Voor de aanvragen die u nodig hebt om te voldoen aan een soortgelijke JSON-bestand maken en kopiëren naar de container die u zojuist hebt gemaakt in de Blob-opslag.

## <a name="kick-start-the-batch-job"></a>Begin van de batchverwerking.

De volgende stap is een nieuwe batch-verwerking indienen. Controleer de [API reference](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/)voor meer informatie.

Het hoofdgedeelte van de aanvraag van de API moet voor het definiëren van de locaties waar de invoer, uitvoer en bestanden moeten worden opgeslagen. Tevens moet de referenties die nodig zijn voor toegang tot deze locaties te definiëren. Bovendien moet u sommige parameters opgeven die gelden voor de hele batch (het type van de aanbevelingen voor het aanvragen van het model/build te gebruiken, het aantal resultaten per oproep, enzovoort).

Dit is een voorbeeld van wat het hoofdgedeelte van de aanvraag moet er als volgt uitzien:

    {
      "input": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchInput.json",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "output": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchOutput.json ",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "error": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/errors.txt",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "job": {
        "apiName": "ItemRecommend",
        "modelId": "9ac12a0a-1add-4bdc-bf42-c6517942b3a6",
        "buildId": 1015703,
        "numberOfResults": 10,
        "includeMetadata": true,
        "minimalScore": 0.0
      }
    }

Hier een paar belangrijke dingen te weten:

-   Op dit moment moet **authenticationType** altijd worden ingesteld op **PublicOrSas**.

-   U moet een gedeelde Access handtekening (SAS) token waarmee de API aanbevelingen voor lezen en schrijven van/naar uw account Blob storage ophalen. Meer informatie over het genereren van tokens SAS vindt op [de pagina API aanbevelingen](../storage/storage-dotnet-shared-access-signature-part-1.md).

-   De enige **apiName** die momenteel wordt ondersteund, is **ItemRecommend**, die wordt gebruikt voor het artikel naar aanbevelingen. Batchen ondersteunt momenteel geen gebruiker aan Item aanbevelingen.

## <a name="wait-for-the-asynchronous-operation-to-finish"></a>Wacht tot de asynchrone bewerking is voltooid

Wanneer u de batchverwerking start, wordt het antwoord de header bewerking locatie waarmee u de informatie die nodig is voor het bijhouden van de bewerking.
U volgen de bewerking met behulp van de [API voor bewerking Status ophalen]( https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3da), zoals voor het bijhouden van de werking van een bewerking maken.

## <a name="get-the-results"></a>De resultaten

Nadat de bewerking is voltooid, ervan uitgaande dat er geen fouten zijn, kunt u de resultaten van de uitvoer verzamelen Blob-opslag.

In het voorbeeld hieronder wordt weergegeven wat de uitvoer als volgt uitzien. In dit voorbeeld wordt gedemonstreerd resultaten voor een batch met slechts twee aanvragen (voor kort te houden).

    {
      "results":
      [   
        {
          "request": { "seedItems": [ "DAF-00500", "P3T-00003" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "F5U-00011",
                  "name": "L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.722,
              "reasoning": [ "People who like the selected items also like 'L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr'" ]
            },
            {
              "items": [
                {
                  "itemId": "G5Y-00001",
                  "name": "Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.718,
              "reasoning": [ "People who like the selected items also like 'Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr'" ]
            }
          ]
        },
        {
          "request": { "seedItems": [ "C9F-00163" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "C9F-00172",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Green",
                  "metadata": ""
                }
              ],
              "rating": 0.649,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Green'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00171",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Orange",
                  "metadata": ""
                }
              ],
              "rating": 0.647,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Orange'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00170",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Yellow",
                  "metadata": ""
                }
              ],
              "rating": 0.646,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Yellow'" ]
            }       
          ]
        }
    ]}


## <a name="learn-about-the-limitations"></a>Meer informatie over de beperkingen

-   Slechts één batchverwerking kan per abonnement per keer worden aangeroepen.
-   Een batch job-invoerbestand kan niet meer dan 2 MB.
