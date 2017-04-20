<properties 
    pageTitle="Hoe gebruikt u Blitline voor afbeelding verwerking - Azure guide functie" 
    description="Informatie over het gebruik van de service Blitline verwerken van afbeeldingen in een toepassing Azure." 
    services="" 
    documentationCenter=".net" 
    authors="blitline-dev" 
    manager="jason@blitline.com" 
    editor="jason@blitline.com"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/09/2014" 
    ms.author="support@blitline.com"/>
# <a name="how-to-use-blitline-with-azure-and-azure-storage"></a>Het gebruik van Blitline met Azure en Azure opslag

Deze handleiding wordt uitgelegd hoe toegang tot Blitline services en hoe taken kunnen verzenden naar Blitline.

## <a name="what-is-blitline"></a>Wat is Blitline?

Blitline is een cloud-gebaseerde verwerking van afbeeldingen service voor enterprise level beeldverwerking voor een fractie van de prijs die u zelf maakt de kosten.

Het feit is dat verwerking van afbeeldingen is nog steeds weer, meestal voor elke website opnieuw opgebouwd. Wij realiseren ons dat dit omdat we hebt gemaakt ze een miljoen keer te. Één dag die we besloten dat misschien doen NET we het voor iedereen. We weten hoe u deze snel en efficiënt te doen en Iedereen werkt in de tussentijd opslaan.

Zie [http://www.blitline.com](http://www.blitline.com)voor meer informatie.

## <a name="what-blitline-is-not"></a>Wat Blitline is niet...

Om te verduidelijken wat Blitline is handig voor, is het vaak gemakkelijker te bepalen wat Blitline niet doet voordat u verdergaat.

- Blitline heeft geen HTML-widgets om afbeeldingen te uploaden. Openbaar of met beperkte machtigingen beschikbaar voor Blitline te bereiken, moet u afbeeldingen beschikbaar hebben.

- Blitline geen live beeldbewerking, zoals Aviary.com

- Blitline uploaden van afbeeldingen niet accepteert, kan niet u uw afbeeldingen direct push naar Blitline. U moet deze push Azure opslag of andere plaatsen waar Blitline ondersteunt en vervolgens waar u ze in Blitline.

- Blitline massively parallel en geen synchrone verwerking. Wat betekent dat u moet geven ons een postback_url en kunnen we u als we klaar zijn verwerkt.

## <a name="create-a-blitline-account"></a>Maak een account Blitline

[AZURE.INCLUDE [blitline-signup](../includes/blitline-signup.md)]

## <a name="how-to-create-a-blitline-job"></a>Het maken van een project Blitline

Blitline maakt gebruik van JSON te definiëren van de acties die u wilt uitvoeren op een afbeelding. Deze JSON bestaat uit een paar eenvoudige velden.

Het eenvoudigste voorbeeld is als volgt:

        json : '{
       "application_id": "MY_APP_ID",
       "src" : "http://cdn.blitline.com/filters/boys.jpeg",
       "functions" : [ {
           "name": "resize_to_fit",
           "params" : { "width": 240, "height": 140 },
           "save" : { "image_identifier" : "external_sample_1" }
       } ]
    }'

Hier hebben we JSON die een "bron" afbeelding "... boys.jpeg" en vervolgens die afbeelding naar 240 x 140.

De toepassings-ID is iets dat die u in het tabblad **CONNECTION INFO** of **beheren** op Azure vinden kunt. Het is de geheime id waarmee u taken uitvoeren op Blitline.

De parameter 'opslaan' geeft informatie over waar u de afbeelding plaatsen zodra we het hebben verwerkt. In dit geval trivial nog niet we gedefinieerd. Als er geen vestiging is gedefinieerd Blitline slaat het lokaal (en tijdelijk) op een unieke cloud-locatie. Kun je die locatie ophalen uit de JSON geretourneerd door Blitline bij het maken van de Blitline. De aanduiding "image" is vereist en u wordt geretourneerd wanneer deze bijzonder identificeren afbeelding opgeslagen.

U vindt meer informatie over de *functies* die we hier ondersteuning: <http://www.blitline.com/docs/functions>

Kunt u documentatie over de opties voor reservekopietaak hier vinden: <http://www.blitline.com/docs/api>

Zodra u uw JSON is hoeft u **POST** naar`http://api.blitline.com/job`

U krijgt terug JSON die er ongeveer als volgt uitziet:

    {
     "results":
         {"images":
            [{
              "image_identifier":"external_sample_1",
              "s3_url":"https://s3.amazonaws.com/dev.blitline/2011110722/YOUR_APP_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg"
            }],
          "job_id":"4eb8c9f72a50ee2a9900002f"
         }
    }


Hierdoor weet u Blitline uw aanvraag heeft ontvangen, deze is geplaatst in een wachtrij voor verwerking en voltooiing van de afbeelding is beschikbaar op: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_APP\_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg**

## <a name="how-to-save-an-image-to-your-azure-storage-account"></a>Het opslaan van een afbeelding in uw account Azure opslag

Als u een opslag Azure-account hebt, kunt u gemakkelijk Blitline push verwerkte afbeeldingen naar uw Azure container laten. U definieert de locatie en de machtigingen voor Blitline te drukken door een "azure_destination" toe te voegen.

Hier volgt een voorbeeld:

    job : '{
      "application_id": "YOUR_APP_ID",
      "src" : "http://www.google.com/logos/2011/houdini11-hp.jpg",
         "functions" : [{
         "name": "blur",
         "save" : {
             "image_identifier" : "YOUR_IMAGE_IDENTIFIER",
             "azure_destination" : {
                 "account_name" : "YOUR_AZURE_CONTAINER_NAME",
                 "shared_access_signature" : "SAS_THAT_GIVES_BLITLINE_PERMISSION_TO_WRITE_THIS_OBJECT_TO_CONTAINER",
               }
           }
         }]
       }'


Door het invullen van de waarden van de CAPITALIZED met uw eigen, kunt u deze JSON te http://api.blitline.com/job indienen en de "bron" afbeelding worden verwerkt met een vervagend filter en vervolgens geduwd naar Azure bestemming.

###<a name="please-note"></a>Let op:

De SA's bevat de volledige url van SAS, inclusief de bestandsnaam van het doelbestand.

Voorbeeld:

    http://blitline.blob.core.windows.net/sample/image.jpg?sr=b&sv=2012-02-12&st=2013-04-12T03%3A18%3A30Z&se=2013-04-12T04%3A18%3A30Z&sp=w&sig=Bte2hkkbwTT2sqlkkKLop2asByrE0sIfeesOwj7jNA5o%3D


Ook kunt u de meest recente uitgave van het Blitline van Azure opslag documenten lezen [hier](http://www.blitline.com/docs/azure_storage).


## <a name="next-steps"></a>Volgende stappen

Ga naar blitline.com voor meer informatie over onze functies:

* Blitline API eindpunt Docs <http://www.blitline.com/docs/api>
* Blitline API-functies <http://www.blitline.com/docs/functions>
* Blitline API voorbeelden <http://www.blitline.com/docs/examples>
* Het derde deel Nuget bibliotheek <http://nuget.org/packages/Blitline.Net>
