<properties
pageTitle="De Office 365 Video-connector gebruiken in uw logica apps | Microsoft Azure"
description="Aan de slag met de Office 365 Video-connector in uw Microsoft Azure App service logica apps"
services=""    
documentationCenter=""     
authors="msftman"    
manager="erikre"    
editor=""
tags="connectors"/>

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="na"
ms.date="05/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-office365-video-connector"></a>Aan de slag met Office365 Video-connector
Verbinding maken met Office 365 Video informatie opvragen over een Office 365 video, een lijst van video's en nog veel meer. De Office 365 Video-connector kan worden gebruikt vanuit:

- Logica apps 

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op de schemaversie van logica apps 2015-08-01-voorbeeld. Deze connector wordt niet ondersteund in eerdere versies schema.

U kunt met Office 365 Video:

- Uw business-flow op basis van de gegevens uit Office 365 Video dat u maakt. 
- Acties die de video portal-status controleren, een lijst met alle video in een kanaal en meer gebruiken. Deze acties een reactie krijgt en vervolgens de uitvoer beschikbaar maken voor andere acties. U kunt bijvoorbeeld Bing Search connector gebruiken om te zoeken naar Office 365 video's en vervolgens de Office 365 video connector gebruiken voor informatie over deze video. Als de video aan uw vereisten voldoet, kunt u deze video op Facebook posten. 

Zie als u wilt een bewerking in apps logica toevoegen, [maken een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties

De Office 365 Video-connector heeft de volgende acties beschikbaar. Er zijn geen triggers.

| Triggers | Acties|
| --- | --- |
| Geen | <ul><li>Controleert of de video portal-status</li><li>Alle zichtbare kanalen ophalen</li><li>Afspelen van url van het manifest Azure Media Services ophalen voor een video</li><li>Ophalen van het token aan toonder voor toegang tot de video te decoderen</li><li>Met deze eigenschap wordt informatie over een bepaalde office365 video</li><li>Hier worden alle video office365 's aanwezig zijn in een kanaal</li></ul>

Alle verbindingslijnen ondersteuning in JSON en XML-indelingen voor gegevens. 

## <a name="create-a-connection-to-office365-video-connector"></a>Een verbinding maken met Office365 Video-connector
Wanneer u deze connector aan uw logica apps toevoegen, moet u aanmelden bij uw account voor Office 365 Video en logica apps verbinding maken met uw account.

>[AZURE.INCLUDE [Steps to create a connection to Office 365 Video](../../includes/connectors-create-api-office365video.md)]

Nadat u de verbinding maakt, u het Office 365 video-eigenschappen, zoals de naam van de huurder of kanaal-ID. De **REST API reference** in dit onderwerp worden deze eigenschappen beschreven.

>[AZURE.TIP] In andere logica apps kunt u deze dezelfde Office 365 Video verbinding.

## <a name="swagger-rest-api-reference"></a>Swagger REST API: naslag
Van toepassing op versie: 1.0.

### <a name="checks-video-portal-status"></a>Controleert of de video portal-status 
Controleert of de video portal-status of video services zijn ingeschakeld.  
```GET: /{tenant}/IsEnabled``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|huurder|tekenreeks|Ja|pad|geen|De naam van de huurder voor de map van de gebruiker uitmaakt deel van|


#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is voltooid|
|400|BadRequest|
|401|Niet-geautoriseerde|
|404|Niet gevonden|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|



### <a name="get-all-viewable-channels"></a>Alle zichtbare kanalen ophalen 
Deze eigenschap haalt de kanalen die de gebruiker heeft toegang tot bekijken.  
```GET: /{tenant}/Channels``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|huurder|tekenreeks|Ja|pad|geen|De naam van de huurder voor de map van de gebruiker uitmaakt deel van|


#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is voltooid|
|400|BadRequest|
|401|Niet-geautoriseerde|
|404|Niet gevonden|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|




### <a name="lists-all-the-office365-videos-present-in-a-channel"></a>Hier worden alle video office365 's aanwezig zijn in een kanaal 
Hier worden alle video office365 's aanwezig zijn in een kanaal.  
```GET: /{tenant}/Channels/{channelId}/Videos``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|huurder|tekenreeks|Ja|pad|geen|De naam van de huurder voor de map van de gebruiker uitmaakt deel van|
|channelId|tekenreeks|Ja|pad|geen|De kanaal-id van die video's moeten worden opgehaald|


#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is voltooid|
|400|BadRequest|
|401|Niet-geautoriseerde|
|404|Niet gevonden|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|




### <a name="gets-information-about-a-particular-office365-video"></a>Met deze eigenschap wordt informatie over een bepaalde office365 video 
Met deze eigenschap wordt informatie over een bepaalde office365 video.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|huurder|tekenreeks|Ja|pad|geen|De naam van de huurder voor de map van de gebruiker uitmaakt deel van|
|channelId|tekenreeks|Ja|pad|geen|De kanaal-id|
|videoId|tekenreeks|Ja|pad|geen|De video-id|


#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is voltooid|
|400|BadRequest|
|401|Niet-geautoriseerde|
|404|Niet gevonden|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|




### <a name="get-playback-url-of-the-azure-media-services-manifest-for-a-video"></a>Afspelen van url van het manifest Azure Media Services ophalen voor een video 
Afspelen van url van het manifest Azure Media Services ophalen voor een video.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/playbackurl``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|huurder|tekenreeks|Ja|pad|geen|De naam van de huurder voor de map van de gebruiker uitmaakt deel van|
|channelId|tekenreeks|Ja|pad|geen|De kanaal-id|
|videoId|tekenreeks|Ja|pad|geen|De video-id|
|streamingFormatType|tekenreeks|Ja|query|geen|Streaming format-type. 1 - vloeiend Streaming of MPEG-streepje. 0 - Streaming HLS|


#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is voltooid|
|400|BadRequest|
|401|Niet-geautoriseerde|
|404|Niet gevonden|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|




### <a name="get-the-bearer-token-to-get-access-to-decrypt-the-video"></a>Ophalen van het token aan toonder voor toegang tot de video te decoderen 
Ophalen van het token aan toonder voor toegang tot de video te decoderen.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/token```

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|huurder|tekenreeks|Ja|pad|geen|De naam van de huurder voor de map van de gebruiker uitmaakt deel van|
|channelId|tekenreeks|Ja|pad|geen|De kanaal-id|
|videoId|tekenreeks|Ja|pad|geen|De video-id|


#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is voltooid|
|400|BadRequest|
|401|Niet-geautoriseerde|
|404|Niet gevonden|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|


## <a name="object-definitions"></a>Objectdefinities

#### <a name="channel-channel-class"></a>Kanaal: Channel klasse

| Naam | Gegevenstype | Vereist|
|---|---|---|
|ID|tekenreeks|geen|
|Titel|tekenreeks|geen|
|Beschrijving|tekenreeks|geen|


#### <a name="video"></a>Video 

| Naam | Gegevenstype |Vereist|
|---|---|---|
|ID|tekenreeks|geen|
|Titel|tekenreeks|geen|
|Beschrijving|tekenreeks|geen|
|CreationDate|tekenreeks|geen|
|Eigenaar|tekenreeks|geen|
|ThumbnailUrl|tekenreeks|geen|
|VideoUrl|tekenreeks|geen|
|VideoDuration|geheel getal|geen|
|VideoProcessingStatus|geheel getal|geen|
|ViewCount|geheel getal|geen|


## <a name="next-steps"></a>Volgende stappen
[Maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).
