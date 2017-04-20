<properties 
    pageTitle="Het maken van een Mediaprocessor | Microsoft Azure" 
    description="Informatie over het maken van een onderdeel van de media-processor als u wilt coderen, indeling converteren, coderen of decoderen van media-inhoud voor Azure Media Services." 
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
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="how-to-get-a-media-processor-instance"></a>Procedure: een exemplaar van de Processor Media ophalen


> [AZURE.SELECTOR]
- [.NET](media-services-get-media-processor.md)
- [REST](media-services-rest-get-media-processor.md)

##<a name="overview"></a>Overzicht

In Media Services die een Mediaprocessor een onderdeel dat zorgt voor verwerking van specifieke taken is, zoals codering, conversie, codering of decodering van media-inhoud van de indeling. Meestal maakt u een Mediaprocessor bij het maken van een taak wilt coderen, coderen of de inhoud van media-indeling converteren.

De volgende tabel bevat de naam en beschrijving van elke beschikbare media-processor.

Mediumnaam Processor|Beschrijving|Meer informatie
---|---|---
Media Encoder-standaard|Standaard mogelijkheden biedt voor codering op aanvraag. |[Overzicht en vergelijking van Azure op vraag Media Encoders](media-services-encode-asset.md)
Media Encoder Premium Workflow|Kunt u Media Encoder Premium Workflow codering taken uitvoeren.|[Overzicht en vergelijking van Azure op vraag Media Encoders](media-services-encode-asset.md)
De indexeerfunctie Azure Media| Hiermee kunt u media-bestanden en inhoud doorzoekbaar maken als gesloten closed captioning tracks en trefwoorden te genereren.|[De indexeerfunctie Azure Media](media-services-index-content.md)
Azure Media Hyperlapse (voorbeeld)|Hiermee kunt u de "schokken" in uw video met video stabilisatie vloeiend. U kunt ook uw inhoud in een clip verbruikbare versnellen.|[Hyperlapse Azure Media](media-services-hyperlapse-content.md)
Azure Media Encoder|Afgeschreven
Opslag-decodering| Afgeschreven|
Azure Media Packager|Afgeschreven|
Azure Media coderen|Afgeschreven|

##<a name="get-mediaprocessor"></a>MediaProcessor ophalen

>[AZURE.NOTE] Als u werkt met de overige Media Services API, gelden de volgende overwegingen:
>
>Bij de toegang tot diensten in Media Services, moet u specifieke header-velden en waarden instellen in uw HTTP-aanvragen. Zie [Instellingen voor de ontwikkeling van Media Services REST API](media-services-rest-how-to-use.md)voor meer informatie.

>Nadat de verbinding tot stand te https://media.windows.net, ontvangt u een 301 redirect URI van een andere Media Services opgeven. U moet volgende aanroepen naar de nieuwe URI zoals beschreven in [verbinding maken met Media-Services met behulp van REST API](media-services-rest-connect-programmatically.md). 


De volgende aanroep van de REST wordt aangegeven hoe u een exemplaar van de processor media op naam (in dit geval, **Media Encoder standaard**). 



    
Aanvraag:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.11
    Host: media.windows.net
    
Antwoord:
        
    . . .
    
    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-steps"></a>Volgende stappen

Als u weet hoe u een exemplaar van de processor media, gaat u naar het onderwerp voor [het coderen van een actief](media-services-rest-get-started.md) die hoe u met Media Encoder standaard weergeven voor het coderen van een actief.
