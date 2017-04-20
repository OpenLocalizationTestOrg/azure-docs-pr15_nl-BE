<properties
pageTitle=" Toegestane Connector gebruiken in uw logica apps | Microsoft Azure"
description="Aan de slag met de Connector van de toegestane vertraging in uw Microsoft Azure App Service logica apps"
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

# <a name="get-started-with-the-slack-connector"></a>Aan de slag met de toegestane connector

Toegestane vertraging is een teamcommunicatie die bij elkaar brengt alle van uw team communicatie in een plaats, direct doorzoekbaar en beschikbaar waar u ook bent.

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op de schemaversie van logica apps 2015-08-01-voorbeeld.

U kunt met de toegestane connector:

* Gebruiken voor het bouwen van de logica apps

Zie als u wilt een bewerking in apps logica toevoegen, [maken een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="lets-talk-about-triggers-and-actions"></a>Laten we praten over triggers en acties

De toegestane connector kan worden gebruikt als een actie; Er zijn geen triggers. Alle verbindingslijnen ondersteuning in JSON en XML-indelingen voor gegevens. 

 De toegestane connector heeft de volgende actie (s) en/of trigger(s) beschikbaar:

### <a name="slack-actions"></a>Toegestane acties
U kunt deze acties ondernemen:

|Actie|Beschrijving|
|--- | ---|
|PostMessage|Een bericht posten in een bepaald kanaal.|
## <a name="create-a-connection-to-slack"></a>Een verbinding maken met een toegestane vertraging
Voor het gebruik van de toegestane connector, u eerst een **verbinding** maken en vervolgens de details voor deze eigenschappen bieden: 

|Eigenschap| Vereist|Beschrijving|
| ---|---|---|
|Token|Ja|Referenties van de toegestane vertraging|

Volgt aanmelden bij vertraging en het voltooien van de configuratie van de toegestane **verbinding** in uw app logica:

1. Selecteer **Terugkeerpatroon**
2. Selecteer een **frequentie** en een **Interval** invoeren
3. Selecteer **een actie toevoegen**  
![Toegestane vertraging configureren][1]  
4. Toegestane vertraging opgeven in het zoekvak en wacht om te zoeken naar alle posten met een toegestane vertraging in de naam retourneren
5. **Toegestane vertraging - bericht posten** selecteren
6. Selecteer **aanmelden bij vertraging**:  
![Toegestane vertraging configureren][2]
7. Uw toegestane referenties invoeren om aan te melden voor het machtigen van de toepassing    
![Toegestane vertraging configureren][3]  
8. U zult worden omgeleid naar de pagina aanmelden van uw organisatie. **Machtigen** Toegestane vertraging voor de interactie met uw app logica:      
![Toegestane vertraging configureren][5] 
9. Nadat de verificatie is voltooid zult u omgeleid naar uw app logica om deze te voltooien door de sectie **vertraging - alle berichten** te configureren. Voeg andere triggers en acties die u nodig hebt.  
![Toegestane vertraging configureren][6]
10. Uw werk opslaan door **Opslaan** boven in het menu.


>[AZURE.TIP] U kunt deze verbinding gebruiken in andere apps logica.

## <a name="slack-rest-api-reference"></a>Toegestane REST API: naslag
#### <a name="this-documentation-is-for-version-10"></a>Deze documentatie is voor versie: 1.0


### <a name="post-a-message-to-a-specified-channel"></a>Een bericht posten in een bepaald kanaal.
**```POST: /chat.postMessage```** 



| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|kanaal|tekenreeks|Ja|query|geen|Kanaal, persoonlijke groep of IM kanaal bericht moet worden verzonden. Een naam (ex: #general) of een gecodeerde ID.|
|tekst|tekenreeks|Ja|query|geen|De tekst van het bericht te verzenden. Zie https://api.slack.com/docs/formatting voor het opmaken van de opties.|
|gebruikersnaam|tekenreeks|geen|query|geen|Naam van de bot|
|as_user|Boole-waarde|geen|query|geen|Geeft de waarde true als de geverifieerde gebruiker in plaats van als een bot boeken|
|parseren|tekenreeks|geen|query|geen|Wijzigen hoe berichten worden behandeld. Zie https://api.slack.com/docs/formatting voor meer informatie.|
|link_names|geheel getal|geen|query|geen|Zoek- en kanaal namen en gebruikersnamen.|
|unfurl_links|Boole-waarde|geen|query|geen|Tot het inschakelen van voornamelijk tekstinhoud unfurling doorgeven.|
|unfurl_media|Boole-waarde|geen|query|geen|False als u wilt uitschakelen van media-inhoud unfurling doorgeven.|
|icon_url|tekenreeks|geen|query|geen|URL van een afbeelding wilt gebruiken als een pictogram voor dit bericht|
|icon_emoji|tekenreeks|geen|query|geen|Emoji te gebruiken als een pictogram voor dit bericht|


### <a name="here-are-the-possible-responses"></a>Hier zijn de mogelijke reacties:

|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldig verzoek|
|408|Time-out voor aanvraag|
|429|Te veel aanvragen|
|500|Interne serverfout. Er is een onbekende fout opgetreden|
|503|Toegestane Service is niet beschikbaar|
|504|Gateway time-out|
|Standaard|De bewerking is mislukt.|
------



## <a name="object-definitions"></a>Object (s): 

 **Bericht**: Yammer-bericht

Vereiste eigenschappen voor het bericht:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|ID|geheel getal|
|content_excerpt|tekenreeks|
|sender_id|geheel getal|
|replied_to_id|geheel getal|
|created_at|tekenreeks|
|network_id|geheel getal|
|message_type|tekenreeks|
|sender_type|tekenreeks|
|URL|tekenreeks|
|web_url|tekenreeks|
|group_id|geheel getal|
|hoofdtekst|niet gedefinieerd|
|thread_id|geheel getal|
|direct_message|Boole-waarde|
|client_type|tekenreeks|
|client_url|tekenreeks|
|taal|tekenreeks|
|notified_user_ids|matrix|
|privacy|tekenreeks|
|liked_by|niet gedefinieerd|
|system_message|Boole-waarde|



 **PostOperationRequest**: Hiermee geeft u een post-aanvraag voor Yammer-Connector te boeken naar yammer

Vereiste eigenschappen voor PostOperationRequest:

hoofdtekst

**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|hoofdtekst|tekenreeks|
|group_id|geheel getal|
|replied_to_id|geheel getal|
|direct_to_id|geheel getal|
|uitzending|Boole-waarde|
|Onderwerp1|tekenreeks|
|onderwerp2|tekenreeks|
|Topic3|tekenreeks|
|Topic4|tekenreeks|
|Topic5|tekenreeks|
|topic6|tekenreeks|
|Topic7|tekenreeks|
|Topic8|tekenreeks|
|topic9|tekenreeks|
|topic10|tekenreeks|
|topic11|tekenreeks|
|topic12|tekenreeks|
|topic13|tekenreeks|
|topic14|tekenreeks|
|topic15|tekenreeks|
|topic16|tekenreeks|
|topic17|tekenreeks|
|topic18|tekenreeks|
|topic19|tekenreeks|
|topic20|tekenreeks|



 **MessageList**: lijst met berichten

Vereiste eigenschappen voor MessageList:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|berichten|matrix|



 **MessageBody**: berichttekst

Vereiste eigenschappen voor MessageBody:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|geparseerd|tekenreeks|
|zonder opmaak|tekenreeks|
|rijke|tekenreeks|



 **LikedBy**: door beviel

Vereiste eigenschappen voor LikedBy:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|aantal|geheel getal|
|namen|matrix|



 **YammmerEntity**: door beviel

Vereiste eigenschappen voor YammmerEntity:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|type|tekenreeks|
|ID|geheel getal|
|full_name|tekenreeks|


## <a name="next-steps"></a>Volgende stappen
[Maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md)
## <a name="object-definitions"></a>Object (s): 

 **WebResultModel**: Bing zoekresultaten

Vereiste eigenschappen voor WebResultModel:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|Titel|tekenreeks|
|Beschrijving|tekenreeks|
|DisplayUrl|tekenreeks|
|ID|tekenreeks|
|FullUrl|tekenreeks|



 **VideoResultModel**: Bing video zoekresultaten

Vereiste eigenschappen voor VideoResultModel:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|Titel|tekenreeks|
|DisplayUrl|tekenreeks|
|ID|tekenreeks|
|MediaUrl|tekenreeks|
|Runtime|geheel getal|
|Miniatuur|niet gedefinieerd|



 **ThumbnailModel**: eigenschappen van het element multimedia miniatuur

Vereiste eigenschappen voor ThumbnailModel:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|MediaUrl|tekenreeks|
|ContentType|tekenreeks|
|Breedte|geheel getal|
|Hoogte|geheel getal|
|FileSize|geheel getal|



 **ImageResultModel**: Bing image search-resultaten

Vereiste eigenschappen voor ImageResultModel:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|Titel|tekenreeks|
|DisplayUrl|tekenreeks|
|ID|tekenreeks|
|MediaUrl|tekenreeks|
|Vervolgens kunnen|tekenreeks|
|Miniatuur|niet gedefinieerd|



 **NewsResultModel**: zoekresultaten van Bing nieuws

Vereiste eigenschappen voor NewsResultModel:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|Titel|tekenreeks|
|Beschrijving|tekenreeks|
|DisplayUrl|tekenreeks|
|ID|tekenreeks|
|Bron|tekenreeks|
|Datum|tekenreeks|



 **SpellResultModel**: Bing spelling suggesties resultaten

Vereiste eigenschappen voor SpellResultModel:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|ID|tekenreeks|
|Waarde|tekenreeks|



 **RelatedSearchResultModel**: Bing zoekresultaten betrekking

Vereiste eigenschappen voor RelatedSearchResultModel:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|Titel|tekenreeks|
|ID|tekenreeks|
|BingUrl|tekenreeks|



 **CompositeSearchResultModel**: samengestelde zoekresultaten van Bing

Vereiste eigenschappen voor CompositeSearchResultModel:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|WebResultsTotal|geheel getal|
|ImageResultsTotal|geheel getal|
|VideoResultsTotal|geheel getal|
|NewsResultsTotal|geheel getal|
|SpellSuggestionsTotal|geheel getal|
|WebResults|matrix|
|ImageResults|matrix|
|VideoResults|matrix|
|NewsResults|matrix|
|SpellSuggestionResults|matrix|
|RelatedSearchResults|matrix|


## <a name="object-definitions"></a>Object (s): 

 **PostOperationResponse**: antwoord van post-bewerking van vertraging verbindingslijn vertegenwoordigt voor het boeken van de toegestane vertraging

Vereiste eigenschappen voor PostOperationResponse:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|OK|Boole-waarde|
|kanaal|tekenreeks|
|TS|tekenreeks|
|Bericht|niet gedefinieerd|
|Fout|tekenreeks|



 **MessageItem**: een kanaalbericht.

Vereiste eigenschappen voor MessageItem:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|tekst|tekenreeks|
|ID|tekenreeks|
|gebruiker|tekenreeks|
|gemaakt|geheel getal|
|is_user verwijderd|Boole-waarde|


## <a name="next-steps"></a>Volgende stappen
[Maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md)

[1]: ./media/connectors-create-api-slack/connectionconfig1.png
[2]: ./media/connectors-create-api-slack/connectionconfig2.png 
[3]: ./media/connectors-create-api-slack/connectionconfig3.png
[4]: ./media/connectors-create-api-slack/connectionconfig4.png
[5]: ./media/connectors-create-api-slack/connectionconfig5.png
[6]: ./media/connectors-create-api-slack/connectionconfig6.png
