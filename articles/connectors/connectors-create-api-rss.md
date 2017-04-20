<properties
pageTitle="RSS | Microsoft Azure"
description="Logica apps maken met Azure App-service. RSS-connector kan de gebruikers om te publiceren en feed items ophalen. De gebruikers kunnen naar bewerkingen wordt geactiveerd wanneer een nieuw artikel is gepubliceerd op de feed."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-rss-connector"></a>Aan de slag met de RSS-connector
RSS is een veelgebruikte indeling gebruikt om te publiceren regelmatig bijgewerkte inhoud — zoals blogberichten en nieuws.  Veel uitgevers bieden een zodat gebruikers kunnen zich abonneren op deze RSS-feed.  Gebruik de connector RSS feed stromen van informatie en trigger ophalen wanneer nieuwe items worden gepubliceerd in een RSS-feed.

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op de schemaversie van logica apps 2015-08-01-voorbeeld. 

U kunt aan de slag met het maken van een app logica nu, Zie [maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties

De RSS-connector kan worden gebruikt als een actie; trigger(s) heeft. Alle verbindingslijnen ondersteuning in JSON en XML-indelingen voor gegevens. 

 De RSS-connector is de volgende actie (s) en/of trigger(s) beschikbaar:

### <a name="rss-actions"></a>RSS-acties
U kunt deze acties ondernemen:

|Actie|Beschrijving|
|--- | ---|
|[ListFeedItems](connectors-create-api-rss.md#listfeeditems)|Alle RSS-feed items ophalen.|
### <a name="rss-triggers"></a>RSS-triggers
U kunt voor deze gebeurtenis(sen) luisteren:

|Trigger | Beschrijving|
|--- | ---|
|Wanneer een nieuw item in de feed is gepubliceerd|Een werkstroom wordt geactiveerd wanneer een nieuwe feed wordt gepubliceerd.|


## <a name="create-a-connection-to-rss"></a>Een verbinding maken met RSS

>[AZURE.INCLUDE [Steps to create a connection to an RSS feed](../../includes/connectors-create-api-rss.md)]

>[AZURE.TIP] U kunt deze verbinding gebruiken in andere apps logica.

## <a name="reference-for-rss"></a>Verwijzing voor RSS
Van toepassing op versie: 1.0

## <a name="onnewfeed"></a>OnNewFeed
Wanneer een nieuw item in de feed wordt gepubliceerd: een werkstroom wordt geactiveerd wanneer een nieuwe feed wordt gepubliceerd. 

```GET: /OnNewFeed``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|feedUrl|tekenreeks|Ja|query|geen|Feed url|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|202|Geaccepteerd|
|400|Ongeldig verzoek|
|401|Niet-geautoriseerde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Er is een onbekende fout opgetreden|
|Standaard|De bewerking is mislukt.|


## <a name="listfeeditems"></a>ListFeedItems
Lijst van alle RSS-feed items.: alle RSS-feed items ophalen. 

```GET: /ListFeedItems``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|feedUrl|tekenreeks|Ja|query|geen|Feed url|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|202|Geaccepteerd|
|400|Ongeldig verzoek|
|401|Niet-geautoriseerde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Er is een onbekende fout opgetreden|
|Standaard|De bewerking is mislukt.|


## <a name="object-definitions"></a>Objectdefinities 

### <a name="triggerbatchresponsefeeditem"></a>TriggerBatchResponse [FeedItem]


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|waarde|matrix|Nee |



### <a name="feeditem"></a>FeedItem


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|tekenreeks|Ja |
|titel|tekenreeks|Ja |
|inhoud|tekenreeks|Ja |
|koppelingen|matrix|Nee |
|updatedOn|tekenreeks|Nee |


## <a name="next-steps"></a>Volgende stappen
[Maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md)