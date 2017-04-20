<properties
pageTitle="De Connector Yammer in uw Apps logica toevoegen | Microsoft Azure"
description="Overzicht van de verbindingslijn Yammer met REST API parameters"
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

# <a name="get-started-with-the-yammer-connector"></a>Aan de slag met Yammer-connector

Verbinding maken met Yammer aan gesprekken toegang in uw bedrijfsnetwerk.

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op de schemaversie van logica apps 2015-08-01-voorbeeld.

U kunt met Yammer:

- Bouw uw bedrijf flow op basis van de gegevens die u van Yammer ontvangt. 
- Gebruik de gebeurtenis voor wanneer er een nieuw bericht in een groep of een feed uw volgende.
- Acties gebruiken om een bericht, alle berichten, en nog veel meer te vinden. Deze acties een reactie krijgt en vervolgens de uitvoer beschikbaar maken voor andere acties. Bijvoorbeeld wanneer een nieuw bericht wordt weergegeven, kunt je een e-mail sturen met behulp van Office 365.

Zie als u wilt een bewerking in apps logica toevoegen, [maken een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties
Yammer bevat de volgende triggers en acties. 

Trigger | Acties
--- | ---
<ul><li>Wanneer er een nieuw bericht in een groep</li><li>Als er kanaal een nieuw bericht in mijn volgende</li></ul>| <ul><li>Alle berichten ophalen</li><li>Deze eigenschap haalt de berichten in een groep</li><li>Haalt de berichten uit mijn volgende kanaal</li><li>Post bericht</li><li>Wanneer er een nieuw bericht in een groep</li><li>Als er kanaal een nieuw bericht in mijn volgende</li></ul>

Alle verbindingslijnen ondersteuning in JSON en XML-indelingen voor gegevens. 

## <a name="create-a-connection-to-yammer"></a>Een verbinding maken met Yammer
Voor het gebruik van de connector Yammer, u eerst een **verbinding** maken en vervolgens de details voor deze eigenschappen bieden: 

|Eigenschap| Vereist|Beschrijving|
| ---|---|---|
|Token|Ja|Yammer-referenties|

>[AZURE.INCLUDE [Steps to create a connection to Yammer](../../includes/connectors-create-api-yammer.md)]


>[AZURE.TIP] U kunt deze verbinding gebruiken in andere apps logica.

## <a name="yammer-rest-api-reference"></a>Yammer REST API: naslag
Deze documentatie is voor versie: 1.0


### <a name="get-all-public-messages-in-the-logged-in-users-yammer-network"></a>Alle openbare berichten in Yammer-netwerk van de aangemelde gebruiker ophalen
Komt overeen met 'Alle' gesprekken in de webinterface van Yammer.  
```GET: /messages.json```

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|older_then|geheel getal|geen|query|geen|Berichten die ouder zijn dan de bericht-ID die is opgegeven als een numerieke tekenreeks geretourneerd. Dit is handig voor het pagineren van berichten. Als u momenteel bekijkt 20 berichten en het oudste nummer is bijvoorbeeld 2912, die u kan toevoegen "? older_than = 2912″ uw verzoek om de 20 berichten voor die u ziet.|
|newer_then|geheel getal|geen|query|geen|Berichten nieuwer is dan de bericht-ID die is opgegeven als een numerieke tekenreeks geretourneerd. Dit moet worden gebruikt als polling voor nieuwe berichten. Als u berichten bekijkt, en het meest recente bericht geretourneerd 3516 is, kunt u een verzoek met de parameter "? newer_than = 3516″ om ervoor te zorgen dat er kopieën van berichten al op uw pagina.|
|limiet|geheel getal|geen|query|geen|Het opgegeven aantal berichten terug.|
|pagina|geheel getal|geen|query|geen|De opgegeven pagina opvragen. Als gegevens groter is dan de limiet die wordt geretourneerd, kunt u dit veld gebruiken voor toegang tot de volgende pagina 's|


### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldig verzoek|
|408|Time-out voor aanvraag|
|429|Te veel aanvragen|
|500|Interne serverfout. Er is een onbekende fout opgetreden|
|503|Yammer-Service is niet beschikbaar|
|504|Gateway time-out|
|Standaard|De bewerking is mislukt.|


### <a name="post-a-message-to-a-group-or-all-company-feed"></a>Een bericht aan een groep of bedrijf alle diervoeders
Als groep-ID wordt opgegeven, wordt de opgegeven groep anders die wordt geboekt in alle bedrijf-kanaal bericht geboekt.    
```POST: /messages.json``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|invoer| |Ja|hoofdtekst|geen|Post bericht-aanvraag|


### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|201|Gemaakt|



## <a name="object-definitions"></a>Objectdefinities

#### <a name="message-yammer-message"></a>: Bericht Yammer

| Naam | Gegevenstype | Vereist |
|---|---| --- | 
|ID|geheel getal|geen|
|content_excerpt|tekenreeks|geen|
|sender_id|geheel getal|geen|
|replied_to_id|geheel getal|geen|
|created_at|tekenreeks|geen|
|network_id|geheel getal|geen|
|message_type|tekenreeks|geen|
|sender_type|tekenreeks|geen|
|URL|tekenreeks|geen|
|web_url|tekenreeks|geen|
|group_id|geheel getal|geen|
|hoofdtekst|niet gedefinieerd|geen|
|thread_id|geheel getal|geen|
|direct_message|Boole-waarde|geen|
|client_type|tekenreeks|geen|
|client_url|tekenreeks|geen|
|taal|tekenreeks|geen|
|notified_user_ids|matrix|geen|
|privacy|tekenreeks|geen|
|liked_by|niet gedefinieerd|geen|
|system_message|Boole-waarde|geen|

#### <a name="postoperationrequest-represents-a-post-request-for-yammer-connector-to-post-to-yammer"></a>PostOperationRequest: Hiermee geeft u een post-aanvraag voor Yammer-Connector te boeken naar yammer

| Naam | Gegevenstype | Vereist |
|---|---| --- | 
|hoofdtekst|tekenreeks|Ja|
|group_id|geheel getal|geen|
|replied_to_id|geheel getal|geen|
|direct_to_id|geheel getal|geen|
|uitzending|Boole-waarde|geen|
|Onderwerp1|tekenreeks|geen|
|onderwerp2|tekenreeks|geen|
|Topic3|tekenreeks|geen|
|Topic4|tekenreeks|geen|
|Topic5|tekenreeks|geen|
|topic6|tekenreeks|geen|
|Topic7|tekenreeks|geen|
|Topic8|tekenreeks|geen|
|topic9|tekenreeks|geen|
|topic10|tekenreeks|geen|
|topic11|tekenreeks|geen|
|topic12|tekenreeks|geen|
|topic13|tekenreeks|geen|
|topic14|tekenreeks|geen|
|topic15|tekenreeks|geen|
|topic16|tekenreeks|geen|
|topic17|tekenreeks|geen|
|topic18|tekenreeks|geen|
|topic19|tekenreeks|geen|
|topic20|tekenreeks|geen|

#### <a name="messagelist-list-of-messages"></a>MessageList: Lijst met berichten

| Naam | Gegevenstype | Vereist |
|---|---| --- | 
|berichten|matrix|geen|


#### <a name="messagebody-message-body"></a>MessageBody: Berichttekst

| Naam | Gegevenstype | Vereist |
|---|---| --- | 
|geparseerd|tekenreeks|geen|
|zonder opmaak|tekenreeks|geen|
|rijke|tekenreeks|geen|

#### <a name="likedby-liked-by"></a>LikedBy: Door beviel

| Naam | Gegevenstype | Vereist |
|---|---| --- | 
|aantal|geheel getal|geen|
|namen|matrix|geen|

#### <a name="yammmerentity-liked-by"></a>YammmerEntity: Door beviel

| Naam | Gegevenstype | Vereist |
|---|---| --- | 
|type|tekenreeks|geen|
|ID|geheel getal|geen|
|full_name|tekenreeks|geen|


## <a name="next-steps"></a>Volgende stappen
[Maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

[1]: ./media/connectors-create-api-yammer/connectionconfig1.png
[2]: ./media/connectors-create-api-yammer/connectionconfig2.png 
[3]: ./media/connectors-create-api-yammer/connectionconfig3.png
[4]: ./media/connectors-create-api-yammer/connectionconfig4.png
[5]: ./media/connectors-create-api-yammer/connectionconfig5.png
