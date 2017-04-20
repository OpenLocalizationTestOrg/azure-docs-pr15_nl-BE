<properties
pageTitle="SendGrid | Microsoft Azure"
description="Logica apps maken met Azure App-service. SendGrid Connection-Provider kunt u e-mail verzenden en beheren van lijsten met geadresseerden."
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

# <a name="get-started-with-the-sendgrid-connector"></a>Aan de slag met de SendGrid-connector

SendGrid Connection-Provider kunt u e-mail verzenden en beheren van lijsten met geadresseerden.

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op de schemaversie van logica apps 2015-08-01-voorbeeld. 

U kunt aan de slag met het maken van een app logica nu, Zie [maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties

De SendGrid-connector kan worden gebruikt als een actie; trigger(s) heeft. Alle verbindingslijnen ondersteuning in JSON en XML-indelingen voor gegevens. 

 De SendGrid-connector heeft de volgende acties beschikbaar. Er zijn geen triggers.

### <a name="sendgrid-actions"></a>SendGrid acties
U kunt deze acties ondernemen:

|Actie|Beschrijving|
|--- | ---|
|[SendEmail](connectors-create-api-sendgrid.md#sendemail)|Hiermee verzendt u een e-mailbericht met SendGrid-API (beperkt tot 10.000 geadresseerden)|
|[AddRecipientToList](connectors-create-api-sendgrid.md#addrecipienttolist)|Een individuele geadresseerde toevoegen aan een lijst met geadresseerden|


## <a name="create-a-connection-to-sendgrid"></a>Een verbinding maken met SendGrid
Logica apps maken met SendGrid, moet u eerst een **verbinding** maken en de informatie geven voor de volgende eigenschappen: 

|Eigenschap| Vereist|Beschrijving|
| ---|---|---|
|ApiKey|Ja|De SendGrid Api-sleutel opgeven|
 

>[AZURE.INCLUDE [Steps to create a connection to SendGrid](../../includes/connectors-create-api-sendgrid.md)]

>[AZURE.TIP] U kunt deze verbinding gebruiken in andere apps logica.

Nadat u de verbinding hebt gemaakt, kunt u het uitvoeren van de acties te luisteren voor de triggers die in dit artikel beschreven.

## <a name="reference-for-sendgrid"></a>Verwijzing naar SendGrid
Van toepassing op versie: 1.0

## <a name="sendemail"></a>SendEmail
E-mail verzenden: Hiermee verzendt u een e-mailbericht met SendGrid-API (beperkt tot 10.000 geadresseerden) 

```POST: /api/mail.send.json``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|aanvraag| |Ja|hoofdtekst|geen|E-mailbericht verzenden|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldig verzoek|
|401|Niet-geautoriseerde|
|403|Verboden|
|404|Niet gevonden|
|429|Te veel aanvragen|
|500|Interne serverfout. Er is een onbekende fout opgetreden|
|Standaard|De bewerking is mislukt.|


## <a name="addrecipienttolist"></a>AddRecipientToList
Ontvanger toevoegen aan de lijst: een individuele geadresseerde toevoegen aan een lijst met geadresseerden 

```POST: /v3/contactdb/lists/{listId}/recipients/{recipientId}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|listId|tekenreeks|Ja|pad|geen|De unieke id van de lijst met geadresseerden|
|recipientId|tekenreeks|Ja|pad|geen|De unieke id van de ontvanger|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldig verzoek|
|401|Niet-geautoriseerde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Er is een onbekende fout opgetreden|
|Standaard|De bewerking is mislukt.|


## <a name="object-definitions"></a>Objectdefinities 

### <a name="emailrequest"></a>EmailRequest


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Van|tekenreeks|Ja |
|FromName|tekenreeks|Nee |
|Aan|tekenreeks|Ja |
|aannaam|tekenreeks|Nee |
|Onderwerp|tekenreeks|Ja |
|hoofdtekst|tekenreeks|Ja |
|ishtml|Boole-waarde|Nee |
|CC|tekenreeks|Nee |
|ccnaam|tekenreeks|Nee |
|BCC|tekenreeks|Nee |
|bccnaam|tekenreeks|Nee |
|ReplyTo|tekenreeks|Nee |
|datum|tekenreeks|Nee |
|berichtkoppen|tekenreeks|Nee |
|bestanden|matrix|Nee |
|bestandsnamen|matrix|Nee |



### <a name="emailresponse"></a>EmailResponse


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Bericht|tekenreeks|Nee |



### <a name="recipientlists"></a>RecipientLists


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|lijsten|matrix|Nee |



### <a name="recipientlist"></a>RecipientList


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|geheel getal|Nee |
|naam|tekenreeks|Nee |
|recipient_count|geheel getal|Nee |



### <a name="recipients"></a>Geadresseerden


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|geadresseerden|matrix|Nee |



### <a name="recipient"></a>Ontvanger


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|E-mail|tekenreeks|Nee |
|Achternaam|tekenreeks|Nee |
|Voornaam|tekenreeks|Nee |
|ID|tekenreeks|Nee |


## <a name="next-steps"></a>Volgende stappen
[Maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md)