<properties
    pageTitle="De Facebook-connector toevoegen in uw logica Apps | Microsoft Azure"
    description="Overzicht van de Facebook-connector met REST API parameters"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-facebook-connector"></a>Aan de slag met Facebook-connector
Verbinding maken met Facebook en boeken op een tijdlijn, krijgen een feed pagina en nog veel meer. 

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op de schemaversie van logica apps 2015-08-01-voorbeeld.


Met Facebook kunt u:

- Bouw uw bedrijf flow op basis van de gegevens die u van Facebook krijgt. 
- Gebruik een trigger wanneer een nieuw bericht is ontvangen.
- Gebruik acties die boeken aan de tijdlijn, krijgen een feed pagina en nog veel meer. Deze acties een reactie krijgt en vervolgens de uitvoer beschikbaar maken voor andere acties. Bijvoorbeeld wanneer er een nieuw bericht op de tijdlijn, kunt u dat bericht en druk op uw Twitter-feed. 



Zie als u wilt een bewerking in apps logica toevoegen, [maken een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties
De Facebook-connector bevat de volgende trigger en acties. 

| Triggers | Acties|
| --- | --- |
| <ul><li>Als er een nieuw bericht op mijn tijdlijn</li></ul> |<ul><li>Feed ophalen uit mijn tijdlijn</li><li>Boeken op mijn tijdlijn</li><li>Als er een nieuw bericht op mijn tijdlijn</li><li>Feed pagina ophalen</li><li>Tijdlijn van de gebruiker ophalen</li><li>Op de pagina plaatsen</li></ul>

Alle verbindingslijnen ondersteuning in JSON en XML-indelingen voor gegevens.

## <a name="create-a-connection-to-facebook"></a>Een verbinding maken met Facebook
Wanneer u deze connector aan uw apps logica toevoegen, moet u de logica apps verbinding maken met uw Facebook machtigen.

1. Aanmelden bij uw Facebook-account
2. Selecteer **machtigen**en laat uw logica apps verbinding maken en gebruiken van uw Facebook. 

>[AZURE.INCLUDE [Steps to create a connection to Facebook](../../includes/connectors-create-api-facebook.md)]

>[AZURE.TIP] Kunt u deze verbinding met dezelfde Facebook in andere apps logica.

## <a name="swagger-rest-api-reference"></a>Swagger REST API: naslag
Van toepassing op versie: 1.0.

### <a name="get-feed-from-my-timeline"></a>Feed ophalen uit mijn tijdlijn
De feeds zijn afkomstig uit de tijdlijn van de gebruiker is aangemeld.  
```GET: /me/feed```

| Naam|Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|velden|tekenreeks|geen|query|geen |De velden die u als resultaat wilt geven. Voorbeeld (-id, naam, afbeelding).|
|limiet|geheel getal|geen|query| geen|Maximum aantal berichten moeten worden opgehaald|
|met|tekenreeks|geen|query| geen|De lijst van het aantal ambten beperken tot alleen de locatie is gekoppeld.|
|filter|tekenreeks|geen|query| geen|Alleen berichten die overeenkomen met het filter van een bepaalde stroom worden opgehaald.|

#### <a name="response"></a>Reactie
|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldig verzoek|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|


### <a name="post-to-my-timeline"></a>Boeken op mijn tijdlijn
Status bericht in de tijdlijn van de gebruiker is aangemeld.  
```POST: /me/feed```

| Naam|Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|Verzenden|tekenreeks |Ja|hoofdtekst|geen |Nieuw bericht moet worden geboekt|

#### <a name="response"></a>Reactie
|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldig verzoek|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|


### <a name="when-there-is-a-new-post-on-my-timeline"></a>Als er een nieuw bericht op mijn tijdlijn
Een nieuwe stroom wordt gegenereerd wanneer er een nieuw bericht op de tijdlijn van de gebruiker is aangemeld.  
```GET: /trigger/me/feed```

Er zijn geen parameters. 

#### <a name="response"></a>Reactie
|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldig verzoek|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|


### <a name="get-page-feed"></a>Feed pagina ophalen
Berichten ophalen van de feed van een bepaalde pagina.  
```GET: /{pageId}/feed```

| Naam|Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|pageId|tekenreeks|Ja|pad| geen|De id van de pagina die de berichten moeten worden opgehaald.|
|limiet|geheel getal|geen|query| geen|Maximum aantal berichten moeten worden opgehaald|
|include_hidden|Boole-waarde|geen|query|geen |Al dan niet opnemen van alle berichten die zijn verborgen door de pagina|
|velden|tekenreeks|geen|query|geen |De velden die u als resultaat wilt geven. Voorbeeld (-id, naam, afbeelding).|

#### <a name="response"></a>Reactie
|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldig verzoek|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|


### <a name="get-user-timeline"></a>Tijdlijn van de gebruiker ophalen
Berichten ophalen uit de tijdlijn van de gebruiker.  
```GET: /{userId}/feed```

| Naam|Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|gebruikers-id|tekenreeks|Ja|pad|geen |De id van de gebruiker wiens tijdlijn moet worden opgehaald.|
|limiet|geheel getal|geen|query|geen |Maximum aantal berichten moeten worden opgehaald|
|met|tekenreeks|geen|query|geen |De lijst van het aantal ambten beperken tot alleen de locatie is gekoppeld.|
|filter|tekenreeks|geen|query| geen|Alleen berichten die overeenkomen met het filter van een bepaalde stroom worden opgehaald.|
|velden|tekenreeks|geen|query| geen|De velden die u als resultaat wilt geven. Voorbeeld (-id, naam, afbeelding).|

#### <a name="response"></a>Reactie
|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldig verzoek|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|


### <a name="post-to-page"></a>Op de pagina plaatsen
Een bericht posten in een Facebook-Page als de gebruiker is aangemeld.  
```POST: /{pageId}/feed```

| Naam|Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|pageId|tekenreeks|Ja|pad|geen |De id van de pagina om te boeken.|
|Verzenden|veel |Ja|hoofdtekst|geen |Nieuw bericht moet worden geboekt.|

#### <a name="response"></a>Reactie
|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldig verzoek|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|


## <a name="object-definitions"></a>Objectdefinities

#### <a name="getfeedresponse"></a>GetFeedResponse

|Naam van eigenschap | Gegevenstype | Vereist|
|---|---|---|
|gegevens|matrix|geen|

#### <a name="triggerfeedresponse"></a>TriggerFeedResponse

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|gegevens|matrix|geen|

#### <a name="postitem-a-single-entry-in-a-profiles-feed"></a>PostItem: Eén vermelding in een profiel de feed.
Het profiel kan een gebruiker, pagina, toepassing of groep zijn. 

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|ID|tekenreeks|geen|
|admin_creator|matrix|geen|
|bijschrift|tekenreeks|geen|
|created_time|tekenreeks|geen|
|Beschrijving|tekenreeks|geen|
|feed_targeting|niet gedefinieerd|geen|
|Van|niet gedefinieerd|geen|
|pictogram|tekenreeks|geen|
|is_hidden|Boole-waarde|geen|
|is_published|Boole-waarde|geen|
|koppeling|tekenreeks|geen|
|Bericht|tekenreeks|geen|
|naam|tekenreeks|geen|
|object_id|tekenreeks|geen|
|afbeelding|tekenreeks|geen|
|plaats|niet gedefinieerd|geen|
|privacy|niet gedefinieerd|geen|
|Eigenschappen|matrix|geen|
|bron|tekenreeks|geen|
|status_type|tekenreeks|geen|
|verhaal|tekenreeks|geen|
|Doelitems van het type|niet gedefinieerd|geen|
|Aan|matrix|geen|
|type|tekenreeks|geen|
|updated_time|tekenreeks|geen|
|with_tags|niet gedefinieerd|geen|

#### <a name="triggeritem-a-single-entry-in-a-profiles-feed"></a>TriggerItem: Één vermelding in een profiel de feed.
Het profiel kan een gebruiker, pagina, toepassing of groep zijn.

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|ID|tekenreeks|geen|
|created_time|tekenreeks|geen|
|Van|niet gedefinieerd|geen|
|Bericht|tekenreeks|geen|
|type|tekenreeks|geen|

#### <a name="adminitem"></a>AdminItem

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|ID|tekenreeks|geen|
|koppeling|tekenreeks|geen|

#### <a name="propertyitem"></a>PropertyItem

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|naam|tekenreeks|geen|
|tekst|tekenreeks|geen|
|href|tekenreeks|geen|

#### <a name="userpostfeedrequest"></a>UserPostFeedRequest

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|Bericht|tekenreeks|Ja|
|koppeling|tekenreeks|geen|
|afbeelding|tekenreeks|geen|
|naam|tekenreeks|geen|
|bijschrift|tekenreeks|geen|
|Beschrijving|tekenreeks|geen|
|plaats|tekenreeks|geen|
|tags|tekenreeks|geen|
|privacy|niet gedefinieerd|geen|
|object_attachment|tekenreeks|geen|

#### <a name="pagepostfeedrequest"></a>PagePostFeedRequest

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|Bericht|tekenreeks|Ja|
|koppeling|tekenreeks|geen|
|afbeelding|tekenreeks|geen|
|naam|tekenreeks|geen|
|bijschrift|tekenreeks|geen|
|Beschrijving|tekenreeks|geen|
|acties|matrix|geen|
|plaats|tekenreeks|geen|
|tags|tekenreeks|geen|
|object_attachment|tekenreeks|geen|
|Doelitems van het type|niet gedefinieerd|geen|
|feed_targeting|niet gedefinieerd|geen|
|gepubliceerd|Boole-waarde|geen|
|scheduled_publish_time|tekenreeks|geen|
|backdated_time|tekenreeks|geen|
|backdated_time_granularity|tekenreeks|geen|
|child_attachments|matrix|geen|
|multi_share_end_card|Boole-waarde|geen|

#### <a name="postfeedresponse"></a>PostFeedResponse

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|ID|tekenreeks|geen|

#### <a name="profilecollection"></a>ProfileCollection

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|gegevens|matrix|geen|

#### <a name="useritem"></a>UserItem

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|ID|tekenreeks|geen|
|Voornaam|tekenreeks|geen|
|Achternaam|tekenreeks|geen|
|naam|tekenreeks|geen|
|geslacht|tekenreeks|geen|
|over|tekenreeks|geen|

#### <a name="actionitem"></a>ActionItem

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|naam|tekenreeks|geen|
|koppeling|tekenreeks|geen|

#### <a name="targetitem"></a>TargetItem

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|landen|matrix|geen|
|Landinstellingen|matrix|geen|
|regio 's|matrix|geen|
|steden|matrix|geen|

#### <a name="feedtargetitem-object-that-controls-news-feed-targeting-for-this-post"></a>FeedTargetItem: Object dat news bepaalt feed voor dit bericht is gericht
Iedereen in deze groepen waarschijnlijk meer zien van dit bericht, anderen zijn minder waarschijnlijk. Alleen van toepassing op pagina's.

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|landen|matrix|geen|
|regio 's|matrix|geen|
|steden|matrix|geen|
|age_min|geheel getal|geen|
|age_max|geheel getal|geen|
|geslachten|matrix|geen|
|relationship_statuses|matrix|geen|
|interested_in|matrix|geen|
|college_years|matrix|geen|
|belangen|matrix|geen|
|relevant_until|geheel getal|geen|
|education_statuses|matrix|geen|
|Landinstellingen|matrix|geen|

#### <a name="placeitem"></a>PlaceItem

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|ID|tekenreeks|geen|
|naam|tekenreeks|geen|
|overall_rating|nummer|geen|
|locatie|niet gedefinieerd|geen|

#### <a name="locationitem"></a>LocationItem

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|stad|tekenreeks|geen|
|land|tekenreeks|geen|
|Latitude|nummer|geen|
|located_in|tekenreeks|geen|
|lengtegraad|nummer|geen|
|naam|tekenreeks|geen|
|regio|tekenreeks|geen|
|staat|tekenreeks|geen|
|straat|tekenreeks|geen|
|ZIP|tekenreeks|geen|

#### <a name="privacyitem"></a>PrivacyItem

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|Beschrijving|tekenreeks|geen|
|waarde|tekenreeks|Ja|
|toestaan|tekenreeks|geen|
|weigeren|tekenreeks|geen|
|vrienden|tekenreeks|geen|

#### <a name="childattachmentsitem"></a>ChildAttachmentsItem

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|koppeling|tekenreeks|geen|
|afbeelding|tekenreeks|geen|
|image_hash|tekenreeks|geen|
|naam|tekenreeks|geen|
|Beschrijving|tekenreeks|geen|

#### <a name="postphotorequest"></a>PostPhotoRequest

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|URL|tekenreeks|Ja|
|bijschrift|tekenreeks|geen|

#### <a name="postphotoresponse"></a>PostPhotoResponse

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|ID|tekenreeks|Ja|
|post_id|tekenreeks|Ja|

#### <a name="postvideorequest"></a>PostVideoRequest

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|videoData|tekenreeks|Ja|
|Beschrijving|tekenreeks|Ja|
|titel|tekenreeks|Ja|
|uploadedVideoName|tekenreeks|geen|

#### <a name="getphotoresponse"></a>GetPhotoResponse

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|gegevens|niet gedefinieerd|Ja|

#### <a name="getphotoresponseitem"></a>GetPhotoResponseItem

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|URL|tekenreeks|Ja|
|is_silhouette|Boole-waarde|Ja|
|hoogte|tekenreeks|geen|
|Breedte|tekenreeks|geen|

#### <a name="geteventresponse"></a>GetEventResponse

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|gegevens|matrix|Ja|

#### <a name="geteventresponseitem"></a>GetEventResponseItem

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|ID|tekenreeks|Ja|
|naam|tekenreeks|Ja|
|start_time|tekenreeks|geen|
|end_time|tekenreeks|geen|
|TimeZone|tekenreeks|geen|
|locatie|tekenreeks|geen|
|Beschrijving|tekenreeks|geen|
|ticket_uri|tekenreeks|geen|
|rsvp_status|tekenreeks|Ja|


## <a name="next-steps"></a>Volgende stappen

[Maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).
