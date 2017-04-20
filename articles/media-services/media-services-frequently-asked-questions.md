<properties 
    pageTitle="Veelgestelde vragen | Microsoft Azure" 
    description="Veelgestelde vragen (FAQ's)" 
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


#<a name="frequently-asked-questions"></a>Veelgestelde vragen

##<a name="general-ams-faqs"></a>Algemene AMS Veelgestelde vragen

V: hoe u de schaal van indexering?

A: de gereserveerde eenheden zijn hetzelfde voor codering en indexering taken. Volg de instructies voor [het coderen gereserveerde schaaleenheden](media-services-scale-media-processing-overview.md). **Opmerking** de prestaties van de Indexer wordt niet beïnvloed door eenheidstype gereserveerd.

V: ik geüpload, gecodeerd en een video gepubliceerd. Wat is de reden van de video niet wordt afgespeeld wanneer ik het streamen?

A: een van de meest voorkomende redenen is geen ten minste één gereserveerde streaming unit op het streaming eindpunt van waaruit u om het afspelen te probeert worden toegewezen.  Volg de instructies voor [het Streaming gereserveerde schaaleenheden](media-services-portal-scale-streaming-endpoints.md).

V: kan ik samenstellen op een live gegevensstroom?

A: compositing op live gegevensstromen is momenteel niet beschikbaar in Azure Media Services, zodat u moet voor het opstellen van vooraf op uw computer.

V: kan ik met Live Streaming CDN Azure gebruiken?

A: Media Services ondersteunt integratie met Azure CDN (Zie voor meer informatie [hoe u Streaming eindpunten op een rekening van Media Services](media-services-portal-manage-streaming-endpoints.md)).  Kunt u Live streaming met CDN. Azure Media Services biedt goede Streaming, HLS en MPEG-streep-uitgangen. Deze indelingen alle HTTP gebruiken voor het overbrengen van gegevens en voordelen van de HTTP. In de live streaming van werkelijke gegevens voor video en audio op fragmenten is verdeeld en deze afzonderlijke fragmenten ophalen in het cachegeheugen van CDN. Alleen gegevens moeten worden vernieuwd, zijn de manifest gegevens. Manifest gegevens periodiek worden vernieuwd door CDN.

V: is Azure Media services ondersteuning voor afbeeldingen opslaan?

A: als u gewoon op zoek bent voor het opslaan van JPEG- of PNG-afbeeldingen, moet u die in Azure Blob-opslag. Er is geen voordeel voor uw account Media Services plaatsen, tenzij u wilt behouden die zijn gekoppeld aan uw Video of Audio activa. Of als u mogelijk de afbeeldingen gebruiken als overlays in de video encoder. Media Encoder standaard ondersteunt hierbij afbeeldingen op video's, en dat is deze lijsten JPEG en PNG als ondersteunde indelingen worden ingevoerd. Zie [Bedekkingen maken](media-services-custom-mes-presets-with-dotnet.md#overlay)voor meer informatie.

V: hoe kan ik activa van een Media-Services-account naar de andere kopiëren.

A: elementen te kopiëren van de ene Media Services-account naar de andere met .NET, [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) -extensie-methode gebruiken in de opslagplaats [Azure Media Services .NET SDK-uitbreidingen](https://github.com/Azure/azure-sdk-for-media-services-extensions/) . Zie [deze](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) thread forum voor meer informatie.

V: wat zijn de ondersteunde tekens voor de naamgeving van bestanden tijdens het werken met AMS?

A: Media Services wordt de waarde van de eigenschap IAssetFile.Name gebruikt bij het maken van URL's voor de stroomsgewijze inhoud (bijvoorbeeld http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters). Om deze reden is procent codering niet toegestaan. De waarde van de eigenschap **Name** geen van de volgende [tekens voor percentage-codering-gereserveerd](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Bovendien kunnen alleen er een '.' voor de bestandsextensie.


V: hoe kan ik verbinding maken met behulp van de REST?

A: na verbinding tot stand te https://media.windows.net, ontvangt u een 301 redirect URI van een andere Media Services opgeven. U moet volgende aanroepen naar de nieuwe URI zoals beschreven in [verbinding maken met Media-Services met behulp van REST API](media-services-rest-connect-programmatically.md). 


V: hoe kan ik een video draaien tijdens het coderingsproces.

A: de [Media Encoder standaard](media-services-dotnet-encode-with-media-encoder-standard.md) ondersteunt draaien door een hoek van 180-90/270. De standaardinstelling is "Auto", waar wordt geprobeerd om de metagegevens van de rotatie in de inkomende MP4/MOV-bestand detecteren en het compenseren. De volgende **bronnen** element op een van de json voorinstellingen gedefinieerde [hier](http://msdn.microsoft.com/library/azure/mt269960.aspx)omvatten:
    
    "Version": 1.0,
    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [
    
    ...




##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
