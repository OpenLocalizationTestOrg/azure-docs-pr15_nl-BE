<properties 
    pageTitle="Het coderen van een actief gebruik van Media Encoder standaard | Microsoft Azure" 
    description="Informatie over het gebruik van Media Encoder standaard voor het coderen van media-inhoud op Media Services. Codevoorbeelden gebruiken REST API." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako"/>


#<a name="how-to-encode-an-asset-using-media-encoder-standard"></a>Het coderen van een actief gebruik van Media Encoder standaard


> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
- [REST](media-services-rest-encode-asset.md)
- [Portal](media-services-portal-encode.md)

##<a name="overview"></a>Overzicht
Voor het leveren van digitale video via het internet, moet u de media comprimeren. Digitale video's zijn vrij groot en mogelijk te groot om aan te bieden via het internet of voor uw klanten apparaten goed weer te geven. Codering is het proces van het comprimeren van video en audio, zodat uw klanten kunnen uw media bekijken.

Codering taken zijn een van de meest voorkomende bewerkingen in Media Services. U maakt codering taken om mediabestanden te converteren van één codering naar de andere. Wanneer u codeert, kunt u de Media Services ingebouwde encoder (Media Encoder Standard). U kunt ook een coderingsprogramma geleverd door een partner van Media Services; derden encoders zijn beschikbaar via de markt Azure. U kunt de details van taken te coderen met behulp van vooraf ingestelde tekenreeksen die zijn gedefinieerd voor de encoder of met behulp van vooraf ingestelde configuratiebestanden opgeven. Zie [Voorinstellingen voor Media Encoder standaard taak](http://msdn.microsoft.com/library/mt269960)de soorten voorinstellingen die beschikbaar zijn.

Elke taak kan een of meer taken afhankelijk van het soort behandeling die u wilt bereiken hebben. Via de API REST kunt u taken en hun verwante taken op twee manieren maken:

- Taken kunnen worden gedefinieerd in line via de eigenschap Tasks navigatie op entiteiten van de taak, of
- via de batchverwerking OData.


Het is raadzaam altijd de mezzanine-bestanden te coderen in een adaptieve bitsnelheid MP4 instellen en vervolgens de set converteren naar de gewenste indeling met behulp van de [Dynamische verpakking](media-services-dynamic-packaging-overview.md). Als u wilt profiteren van dynamische verpakking, moet eerst krijgt u ten minste één On-demand streaming eenheid voor het streaming eindpunt waaruit u leveren uw inhoud wilt. Zie [schaal Media Services](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.

Als uw uitvoer activum opslag gecodeerd, moet u beleid voor levering van activa. Zie [configureren activa levering beleid](media-services-rest-configure-asset-delivery-policy.md)voor meer informatie.


>[AZURE.NOTE]Voordat u verwijst naar de media-processors, controleert u of de juiste media processor-ID. Zie voor meer informatie [Ophalen Media Processors](media-services-rest-get-media-processor.md).

##<a name="create-a-job-with-a-single-encoding-task"></a>Een taak maken met een enkele taak codering

>[AZURE.NOTE] Als u werkt met de overige Media Services API, gelden de volgende overwegingen:
>
>Bij de toegang tot diensten in Media Services, moet u specifieke header-velden en waarden instellen in uw HTTP-aanvragen. Zie [Instellingen voor de ontwikkeling van Media Services REST API](media-services-rest-how-to-use.md)voor meer informatie.

>Nadat de verbinding tot stand te https://media.windows.net, ontvangt u een 301 redirect URI van een andere Media Services opgeven. U moet volgende aanroepen naar de nieuwe URI zoals beschreven in [verbinding maken met Media-Services met behulp van REST API](media-services-rest-connect-programmatically.md).
>
>Als JSON en voor het gebruik van het trefwoord **__metadata** in de aanvraag te geven (bijvoorbeeld naar verwijst naar een gekoppeld object) moet u de **Accept** -header ingesteld op [uitgebreide JSON-indeling](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/): accepteren: application/json; odata = verbose.

In het volgende voorbeeld ziet u hoe u kunt maken en boeken van een taak met een taak die is ingesteld voor het coderen van een video op een specifieke resolutie en kwaliteit. Bij het coderen met Media Encoder Standard, kunt u voorinstellingen voor taak configuratie opgegeven [hier](http://msdn.microsoft.com/library/mt269960).

Aanvraag:

POST https://media.windows.net/API/Jobs HTTP/1.1 Content-Type: application/json; odata = uitgebreide accepteren: application/json; odata = uitgebreide DataServiceVersion: 3.0 MaxDataServiceVersion: 3,0 x-ms-versie: 2.11 vergunning: aan toonder <token value> 
 x-ms-client-aanvraag-id: 00000000-0000-0000-0000-000000000000-Host: media.windows.net

    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aaab7f15b-3136-4ddf-9962-e9ecb28fb9d2')"}}],  "Tasks" : [{"Configuration" : "H264 Multiple Bitrate 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",  "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"}]}

Antwoord:
    
    HTTP/1.1 201 Created

    . . . 

###<a name="set-the-output-assets-name"></a>Naam van de uitvoer van het vaste activum instellen

In het volgende voorbeeld wordt het kenmerk assetName ingesteld:

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

##<a name="considerations"></a>Overwegingen met betrekking tot

- TaskBody eigenschappen moeten letterlijke XML gebruiken om het aantal invoer of uitvoer van activa die worden gebruikt door de taak. Het onderwerp bevat de XML-schemadefinitie voor het XML-bestand.
- In de definitie van TaskBody, elk binnenste waarde voor <inputAsset> en <outputAsset> als JobInputAsset(value) of JobOutputAsset(value) moet worden ingesteld.
- Een taak kan meerdere output activa hebben. Een JobOutputAsset(x) kan slechts eenmaal worden gebruikt als een output van een taak in een project.
- Kunt u JobInputAsset of JobOutputAsset als een input-element van een taak.
- Taken moeten niet een cyclus vormen.
- De waardeparameter die u aan JobInputAsset of JobOutputAsset doorgeeft geeft de indexwaarde voor een activum. De feitelijke elementen worden gedefinieerd in de eigenschappen InputMediaAssets en OutputMediaAssets op de definitie van de entiteit. 
- Omdat Media Services is gebaseerd op de OData-v3 en de afzonderlijke elementen in InputMediaAssets en OutputMediaAssets navigatie eigenschappenreeksen worden verwezen via een ' __metadata: uri "naam / waarde-paar.
- InputMediaAssets wordt toegewezen aan een of meer activa die u hebt gemaakt in de Media Services. OutputMediaAssets worden gemaakt door het systeem. Ze niet verwijzen naar een bestaand activum.
- OutputMediaAssets kan de naam van het kenmerk assetName. Als dit kenmerk niet aanwezig is, wordt de naam van de OutputMediaAsset wordt ongeacht de interne tekstwaarde van de <outputAsset> -element is met een achtervoegsel van de naam van de waarde of de taak-Id-waarde (in het geval waarin de eigenschap Name niet is gedefinieerd). Bijvoorbeeld, als u een waarde voor assetName voor 'Voorbeeld', zou vervolgens de eigenschap Name van de OutputMediaAsset worden ingesteld op "Voorbeeld". Als u een waarde voor assetName niet ingesteld, maar de taaknaam op 'NewJob' heeft ingesteld, zou vervolgens de naam van de OutputMediaAsset wel "_NewJob JobOutputAsset (waarde)". 


##<a name="create-a-job-with-chained-tasks"></a>Een taak maken met gekoppelde taken

In veel scenario's van toepassing willen ontwikkelaars maken van een reeks taken verwerkt. In de Media-Services kunt u een reeks gekoppelde taken. Elke taak voert verschillende stappen en processors van verschillende media kunt gebruiken. De gekoppelde taken kunnen afleveren activa van de ene taak naar de andere een lineaire reeks taken uitvoeren voor het activum. De taken in een project uitgevoerd hoeven echter niet te worden in een reeks. Wanneer u een gekoppelde taak maakt, worden de gekoppelde **ITask** objecten in een **IJob** -object gemaakt.

>[AZURE.NOTE] Er is een limiet van 30 taken per taak. Als u meer dan 30 taken koppelt, maakt meer dan een taak om de taken te bevatten.


    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://testrest.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A910ffdc1-2e25-4b17-8a42-61ffd4b8914c')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          },
          {  
             "Configuration":"H264 Smooth Streaming 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-16\"?><taskBody><inputAsset>JobOutputAsset(0)</inputAsset><outputAsset>JobOutputAsset(1)</outputAsset></taskBody>"
          }
       ]
    }


###<a name="considerations"></a>Overwegingen met betrekking tot

Opdat de taak koppelen:

- Een project moet ten minste 2 taken hebben
- Er moet ten minste één taak waarvan de invoer is de uitvoer van een andere taak in het project.

## <a name="use-odata-batch-processing"></a>Gebruik de batchverwerking OData 

In het volgende voorbeeld ziet u hoe een project en taken maken met OData batch-verwerking. Zie voor informatie over de batch-verwerking, [Open Data Protocol (OData) Batch-verwerking](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).
 
    POST https://media.windows.net/api/$batch HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: multipart/mixed; boundary=batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Accept: multipart/mixed
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net
    
    
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Content-Type: multipart/mixed; boundary=changeset_122fb0a4-cd80-4958-820f-346309967e4d
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-ID: 1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {"Name" : "NewTestJob", "InputMediaAssets@odata.bind":["https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A2a22445d-1500-80c6-4b34-f1e5190d33c6')"]}
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/$1/Tasks HTTP/1.1
    Content-ID: 2
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {  
       "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
       "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
       "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"Custom output name\">JobOutputAsset(0)</outputAsset></taskBody>"
    }

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d--
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e--
 


## <a name="create-a-job-using-a-jobtemplate"></a>Een taak met behulp van een taaksjabloon maken


Bij de verwerking van meerdere elementen met een gemeenschappelijke set taken komen JobTemplates geeft de taak standaardvoorinstellingen, volgorde van taken, enzovoort.

In het volgende voorbeeld ziet u hoe een taaksjabloon met een TaskTemplate gedefinieerd in line maken. De TaskTemplate maakt gebruik van Media Encoder standaard als de MediaProcessor voor het coderen van het bestand actief. andere MediaProcessors kan echter ook worden gebruikt. 


    POST https://media.windows.net/API/JobTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net

    
    {"Name" : "NewJobTemplate25", "JobTemplateBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><jobTemplate><taskBody taskTemplateId=\"nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789\"><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody></jobTemplate>", "TaskTemplates" : [{"Id" : "nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789", "Configuration" : "H264 Smooth Streaming 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56", "Name" : "SampleTaskTemplate2", "NumberofInputAssets" : 1, "NumberofOutputAssets" : 1}] }
 

>[AZURE.NOTE]In tegenstelling tot andere entiteiten Media Services, moet u een nieuwe GUID-identificatie definiëren voor elke TaskTemplate en plaatst u deze in de taskTemplateId en de eigenschap Id in het hoofdgedeelte van de aanvraag. Het schema voor de identificatie van inhoud moet volgen het kleurenschema dat wordt beschreven in Azure Media Services diensten identificeren. Ook kan niet JobTemplates worden bijgewerkt. In plaats daarvan moet u een nieuw bestand met de wijzigingen bijgewerkt.
 

Als dit lukt, wordt het volgende antwoord geretourneerd:
    
    HTTP/1.1 201 Created
    
    . . .


In het volgende voorbeeld ziet u hoe een taak verwijst naar een taaksjabloon-Id wilt maken:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net

    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}
     

Als dit lukt, wordt het volgende antwoord geretourneerd:
    
    HTTP/1.1 201 Created
    
    . . . 



##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-steps"></a>Volgende stappen
Als u weet hoe u een taak voor het coderen van een assset wilt maken, gaat u naar het onderwerp [Hoe om te controleren voortgang van het project met Media-Services](media-services-rest-check-job-progress.md) .


##<a name="see-also"></a>Zie ook

[Ophalen Media-Processors](media-services-rest-get-media-processor.md)
