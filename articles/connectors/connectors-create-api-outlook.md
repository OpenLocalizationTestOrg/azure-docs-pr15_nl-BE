<properties
pageTitle="Outlook.com | Microsoft Azure"
description="Logica apps maken met Azure App-service. Outlook.com-connector kunt u uw e-mail, agenda en contactpersonen beheren. U kunt verschillende acties uitvoeren zoals e-mail verzenden, vergaderingen plannen, toevoegen, contactpersonen, enz."
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

# <a name="get-started-with-the-outlookcom-connector"></a>Aan de slag met de Outlook.com-connector

Outlook.com-connector kunt u uw e-mail, agenda en contactpersonen beheren. U kunt verschillende acties uitvoeren zoals e-mail verzenden, vergaderingen plannen, toevoegen, contactpersonen, enz.

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op de schemaversie van logica apps 2015-08-01-voorbeeld. 

U kunt aan de slag met het maken van een app logica nu, Zie [maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties

De Outlook.com-connector kan worden gebruikt als een actie; trigger(s) heeft. Alle verbindingslijnen ondersteuning in JSON en XML-indelingen voor gegevens. 

 De connector Outlook.com heeft de volgende actie (s) en/of trigger(s) beschikbaar:

### <a name="outlookcom-actions"></a>Acties voor Outlook.com
U kunt deze acties ondernemen:

|Actie|Beschrijving|
|--- | ---|
|[GetEmails](connectors-create-api-outlook.md#GetEmails)|E-mailberichten ophaalt uit een map|
|[SendEmail](connectors-create-api-outlook.md#SendEmail)|Hiermee verzendt u een e-mailbericht|
|[DeleteEmail](connectors-create-api-outlook.md#DeleteEmail)|Hiermee verwijdert u een e-mailbericht op id|
|[MarkAsRead](connectors-create-api-outlook.md#MarkAsRead)|Een e-mailbericht als gelezen markeren|
|[ReplyTo](connectors-create-api-outlook.md#ReplyTo)|Antwoorden op een e-mailbericht|
|[GetAttachment](connectors-create-api-outlook.md#GetAttachment)|Haalt een e-mail bijlage op id|
|[SendMailWithOptions](connectors-create-api-outlook.md#SendMailWithOptions)|Stuur een e-mail met meerdere opties en wacht totdat de ontvanger om te reageren met een van de opties|
|[SendApprovalMail](connectors-create-api-outlook.md#SendApprovalMail)|Stuur een e-mail voor goedkeuring en wachten op een reactie van de ontvanger|
|[CalendarGetTables](connectors-create-api-outlook.md#CalendarGetTables)|Kalenders worden opgehaald|
|[CalendarGetItems](connectors-create-api-outlook.md#CalendarGetItems)|Items worden opgehaald uit een agenda|
|[CalendarPostItem](connectors-create-api-outlook.md#CalendarPostItem)|Hiermee maakt u een nieuwe gebeurtenis|
|[CalendarGetItem](connectors-create-api-outlook.md#CalendarGetItem)|Een specifiek item opgehaald uit een agenda|
|[CalendarDeleteItem](connectors-create-api-outlook.md#CalendarDeleteItem)|Hiermee verwijdert u een agenda-item|
|[CalendarPatchItem](connectors-create-api-outlook.md#CalendarPatchItem)|Gedeeltelijk werkt een agenda-item|
|[ContactGetTables](connectors-create-api-outlook.md#ContactGetTables)|Mappen met contactpersonen opgehaald|
|[ContactGetItems](connectors-create-api-outlook.md#ContactGetItems)|Contactpersonen opgehaald uit een map met contactpersonen|
|[ContactPostItem](connectors-create-api-outlook.md#ContactPostItem)|Hiermee maakt u een nieuwe contactpersoon|
|[ContactGetItem](connectors-create-api-outlook.md#ContactGetItem)|Een specifieke contactpersoon opgehaald uit een map met contactpersonen|
|[ContactDeleteItem](connectors-create-api-outlook.md#ContactDeleteItem)|Hiermee verwijdert u een contactpersoon|
|[ContactPatchItem](connectors-create-api-outlook.md#ContactPatchItem)|Gedeeltelijk bijwerken een contactpersoon|
### <a name="outlookcom-triggers"></a>Triggers met Outlook.com
U kunt voor deze gebeurtenis(sen) luisteren:

|Trigger | Beschrijving|
|--- | ---|
|Op een gebeurtenis die binnenkort worden gestart|Een stroom wordt gegenereerd wanneer een aanstaande gebeurtenis wordt gestart|
|Op nieuwe e-mail|Een stroom wordt gegenereerd wanneer een nieuwe e-mail binnenkomt|
|Op nieuwe items|Geactiveerd wanneer een nieuwe agenda-item is gemaakt.|
|Op de bijgewerkte artikelen|Geactiveerd wanneer u een agenda-item is gewijzigd|


## <a name="create-a-connection-to-outlookcom"></a>Een verbinding maken met Outlook.com
Logica apps maken met Outlook.com, moet u eerst een **verbinding** maken en de informatie geven voor de volgende eigenschappen: 

|Eigenschap| Vereist|Beschrijving|
| ---|---|---|
|Token|Ja|Referenties van Outlook.com|
Nadat u de verbinding hebt gemaakt, kunt u het uitvoeren van de acties te luisteren voor de triggers die in dit artikel beschreven.

>[AZURE.INCLUDE [Steps to create a connection to Outlook.com](../../includes/connectors-create-api-outlook.md)] 

>[AZURE.TIP] U kunt deze verbinding gebruiken in andere apps logica.  

## <a name="reference-for-outlookcom"></a>Verwijzing voor Outlook.com
Van toepassing op versie: 1.0

## <a name="onupcomingevents"></a>OnUpcomingEvents
Op een gebeurtenis die binnenkort worden gestart: een stroom wordt gegenereerd wanneer een aanstaande gebeurtenis wordt gestart 

```GET: /Events/OnUpcomingEvents``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|query|geen|De unieke id van de kalender|
|lookAheadTimeInMinutes|geheel getal|geen|query|15|Tijd (in minuten) om vooruit te kijken voor toekomstige gebeurtenissen|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is voltooid|
|202|Bewerking is voltooid|
|400|BadRequest|
|401|Niet-geautoriseerde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|


## <a name="getemails"></a>GetEmails
E-mails ophalen: e-mailberichten ophaalt uit een map 

```GET: /Mail``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|folderPath|tekenreeks|geen|query|Postvak in|Pad van de map voor het ophalen van e-mailberichten (standaard: 'Postvak in')|
|Boven|geheel getal|geen|query|10|Aantal e-mailberichten op te halen (standaardwaarde: 10)|
|fetchOnlyUnread|Boole-waarde|geen|query|True|Alleen ongelezen e-mails ophalen?|
|includeAttachments|Boole-waarde|geen|query|False|Als is ingesteld op true, bijlagen zal ook worden opgehaald samen met het e-mailbericht|
|searchQuery|tekenreeks|geen|query|geen|Zoekopdracht voor het filteren van e-mailberichten|
|overslaan|geheel getal|geen|query|0|Aantal e-mails over te slaan (standaard: 0)|
|skipToken|tekenreeks|geen|query|geen|Token ophalen nieuwe pagina overslaan|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is voltooid|
|400|BadRequest|
|401|Niet-geautoriseerde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|


## <a name="sendemail"></a>SendEmail
Per e-mail: een e-mailbericht wordt verzonden 

```POST: /Mail``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|Er| |Ja|hoofdtekst|geen|E-mail|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is voltooid|
|400|BadRequest|
|401|Niet-geautoriseerde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|


## <a name="deleteemail"></a>DeleteEmail
E-mailadres verwijderen: Hiermee verwijdert u een e-mailbericht op id 

```DELETE: /Mail/{messageId}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|messageId|tekenreeks|Ja|pad|geen|Id van het e-mailbericht verwijderen|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is voltooid|
|400|BadRequest|
|401|Niet-geautoriseerde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|


## <a name="markasread"></a>MarkAsRead
Markeren als gelezen: een e-mailbericht als gelezen markeren 

```POST: /Mail/MarkAsRead/{messageId}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|messageId|tekenreeks|Ja|pad|geen|Id van het e-mailbericht moet worden gemarkeerd als gelezen|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is voltooid|
|400|BadRequest|
|401|Niet-geautoriseerde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|


## <a name="replyto"></a>ReplyTo
Antwoord op e-mails: antwoorden op een e-mailbericht 

```POST: /Mail/ReplyTo/{messageId}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|messageId|tekenreeks|Ja|pad|geen|Id van het e-mailbericht te beantwoorden|
|Opmerking|tekenreeks|Ja|query|geen|Commentaar|
|Allen beantwoorden|Boole-waarde|geen|query|False|Alle ontvangers beantwoorden|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is voltooid|
|400|BadRequest|
|401|Niet-geautoriseerde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|


## <a name="getattachment"></a>GetAttachment
Bijlage ophalen: haalt e-mailbijlage door id 

```GET: /Mail/{messageId}/Attachments/{attachmentId}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|messageId|tekenreeks|Ja|pad|geen|Id van het e-mailbericht|
|attachmentId|tekenreeks|Ja|pad|geen|Id van de bijlage te downloaden|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is voltooid|
|400|BadRequest|
|401|Niet-geautoriseerde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|


## <a name="onnewemail"></a>OnNewEmail
Op nieuwe e-mailadres: een stroom wordt gegenereerd wanneer een nieuwe e-mail binnenkomt 

```GET: /Mail/OnNewEmail``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|folderPath|tekenreeks|geen|query|Postvak in|E-map op te halen (standaard: Postvak in)|
|Aan|tekenreeks|geen|query|geen|Ontvangende e-mailadressen|
|Van|tekenreeks|geen|query|geen|Van adres|
|belang|tekenreeks|geen|query|Normaal|Belang van het e-mailbericht (hoog, normaal, laag) (standaard: normaal)|
|fetchOnlyWithAttachment|Boole-waarde|geen|query|False|Alleen e-mailberichten met een bijlage ophalen|
|includeAttachments|Boole-waarde|geen|query|False|Bijlagen opnemen|
|subjectFilter|tekenreeks|geen|query|geen|De tekenreeks om te zoeken in het onderwerp|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is voltooid|
|202|Geaccepteerd|
|400|BadRequest|
|401|Niet-geautoriseerde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|


## <a name="sendmailwithoptions"></a>SendMailWithOptions
Met opties per e-mail: stuur een e-mail met meerdere opties en wacht totdat de ontvanger om te reageren met een van de opties 

```POST: /mailwithoptions/$subscriptions``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|optionsEmailSubscription| |Ja|hoofdtekst|geen|Abonnementaanvraag voor e-mail opties|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|201|Abonnement gemaakt|
|400|BadRequest|
|401|Niet-geautoriseerde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|


## <a name="sendapprovalmail"></a>SendApprovalMail
Goedkeuring per e-mail: stuur een e-mail voor goedkeuring en wachten op een reactie van de ontvanger 

```POST: /approvalmail/$subscriptions``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|approvalEmailSubscription| |Ja|hoofdtekst|geen|Abonnementaanvraag voor goedkeuring e-mail|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|201|Abonnement gemaakt|
|400|BadRequest|
|401|Niet-geautoriseerde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|


## <a name="calendargettables"></a>CalendarGetTables
Ophalen van agenda's: agenda's opgehaald 

```GET: /datasets/calendars/tables``` 

Er zijn geen parameters voor deze oproep
#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


## <a name="calendargetitems"></a>CalendarGetItems
Gebeurtenissen ophalen: items opgehaald uit een agenda 

```GET: /datasets/calendars/tables/{table}/items``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|pad|geen|De unieke id van de kalender op te halen|
|$filter|tekenreeks|geen|query|geen|Een query ODATA filter het aantal items beperken|
|$orderby|tekenreeks|geen|query|geen|Een query ODATA sorteren op voor het opgeven van de volgorde van items|
|$skip|geheel getal|geen|query|geen|Aantal items over te slaan (standaard = 0)|
|$top|geheel getal|geen|query|geen|Maximum aantal items op te halen (standaard = 256)|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


## <a name="calendarpostitem"></a>CalendarPostItem
Gebeurtenis maken: Hiermee maakt u een nieuwe gebeurtenis 

```POST: /datasets/calendars/tables/{table}/items``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|pad|geen|De unieke id van een kalender|
|artikel| |Ja|hoofdtekst|geen|Agenda-item maken|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


## <a name="calendargetitem"></a>CalendarGetItem
Gebeurtenis ophalen: een specifiek item opgehaald uit een agenda 

```GET: /datasets/calendars/tables/{table}/items/{id}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|pad|geen|De unieke id van een kalender|
|ID|tekenreeks|Ja|pad|geen|De unieke id van een agenda-item op te halen|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


## <a name="calendardeleteitem"></a>CalendarDeleteItem
Gebeurtenis verwijderen: Hiermee verwijdert u een agenda-item 

```DELETE: /datasets/calendars/tables/{table}/items/{id}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|pad|geen|De unieke id van een kalender|
|ID|tekenreeks|Ja|pad|geen|De unieke id van een agenda-item verwijderen|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


## <a name="calendarpatchitem"></a>CalendarPatchItem
Gebeurtenis bijwerken: gedeeltelijk werkt een agenda-item 

```PATCH: /datasets/calendars/tables/{table}/items/{id}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|pad|geen|De unieke id van een kalender|
|ID|tekenreeks|Ja|pad|geen|De unieke id van een agenda-item bij te werken|
|artikel| |Ja|hoofdtekst|geen|Agenda-item bij te werken|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


## <a name="calendargetonnewitems"></a>CalendarGetOnNewItems
Op nieuwe items: geactiveerd wanneer een nieuwe agenda-item is gemaakt. 

```GET: /datasets/calendars/tables/{table}/onnewitems``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|pad|geen|De unieke id van een kalender|
|$filter|tekenreeks|geen|query|geen|Een query ODATA filter het aantal items beperken|
|$orderby|tekenreeks|geen|query|geen|Een query ODATA sorteren op voor het opgeven van de volgorde van items|
|$skip|geheel getal|geen|query|geen|Aantal items over te slaan (standaard = 0)|
|$top|geheel getal|geen|query|geen|Maximum aantal items op te halen (standaard = 256)|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


## <a name="calendargetonupdateditems"></a>CalendarGetOnUpdatedItems
Artikelen zijn bijgewerkt op: geactiveerd wanneer u een agenda-item is gewijzigd 

```GET: /datasets/calendars/tables/{table}/onupdateditems``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|pad|geen|De unieke id van een kalender|
|$filter|tekenreeks|geen|query|geen|Een query ODATA filter het aantal items beperken|
|$orderby|tekenreeks|geen|query|geen|Een query ODATA sorteren op voor het opgeven van de volgorde van items|
|$skip|geheel getal|geen|query|geen|Aantal items over te slaan (standaard = 0)|
|$top|geheel getal|geen|query|geen|Maximum aantal items op te halen (standaard = 256)|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


## <a name="contactgettables"></a>ContactGetTables
Mappen met contactpersonen ophalen: haalt de mappen Contactpersonen 

```GET: /datasets/contacts/tables``` 

Er zijn geen parameters voor deze oproep
#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


## <a name="contactgetitems"></a>ContactGetItems
Contactpersonen: contactpersonen opgehaald uit een map met contactpersonen 

```GET: /datasets/contacts/tables/{table}/items``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|pad|geen|De unieke id van de contactpersonenmap op te halen|
|$filter|tekenreeks|geen|query|geen|Een query ODATA filter het aantal items beperken|
|$orderby|tekenreeks|geen|query|geen|Een query ODATA sorteren op voor het opgeven van de volgorde van items|
|$skip|geheel getal|geen|query|geen|Aantal items over te slaan (standaard = 0)|
|$top|geheel getal|geen|query|geen|Maximum aantal items op te halen (standaard = 256)|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


## <a name="contactpostitem"></a>ContactPostItem
Contact maken: Hiermee maakt u een nieuwe contactpersoon 

```POST: /datasets/contacts/tables/{table}/items``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|pad|geen|De unieke id van een map met contactpersonen|
|artikel| |Ja|hoofdtekst|geen|Contact maken|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


## <a name="contactgetitem"></a>ContactGetItem
Contact te krijgen: een specifieke contactpersoon opgehaald uit een map met contactpersonen 

```GET: /datasets/contacts/tables/{table}/items/{id}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|pad|geen|De unieke id van een map met contactpersonen|
|ID|tekenreeks|Ja|pad|geen|De unieke id van een contactpersoon op te halen|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


## <a name="contactdeleteitem"></a>ContactDeleteItem
Contactpersoon verwijderen: Hiermee verwijdert u een contactpersoon 

```DELETE: /datasets/contacts/tables/{table}/items/{id}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|pad|geen|De unieke id van een map met contactpersonen|
|ID|tekenreeks|Ja|pad|geen|De unieke id van een contactpersoon verwijderen|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


## <a name="contactpatchitem"></a>ContactPatchItem
Contactpersoon bijwerken: gedeeltelijk een contactpersoon bijwerken 

```PATCH: /datasets/contacts/tables/{table}/items/{id}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|pad|geen|De unieke id van een map met contactpersonen|
|ID|tekenreeks|Ja|pad|geen|De unieke id van een contactpersoon bijwerken|
|artikel| |Ja|hoofdtekst|geen|Contact item bijwerken|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


## <a name="object-definitions"></a>Objectdefinities 

### <a name="triggerbatchresponseidictionarystringobject"></a>TriggerBatchResponse [IDictionary [tekenreeks, Object]]


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|waarde|matrix|Nee |



### <a name="object"></a>Object


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|



### <a name="sendmessage"></a>SendMessage


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Bijlagen|matrix|Nee |
|Van|tekenreeks|Nee |
|CC|tekenreeks|Nee |
|BCC|tekenreeks|Nee |
|Onderwerp|tekenreeks|Ja |
|Hoofdtekst|tekenreeks|Ja |
|Belang|tekenreeks|Nee |
|IsHtml|Boole-waarde|Nee |
|Aan|tekenreeks|Ja |



### <a name="sendattachment"></a>SendAttachment


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|@odata.type|tekenreeks|Nee |
|Naam|tekenreeks|Ja |
|ContentBytes|tekenreeks|Ja |



### <a name="receivemessage"></a>ReceiveMessage


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|tekenreeks|Nee |
|Isgelezen|Boole-waarde|Nee |
|Heeftbijlage|Boole-waarde|Nee |
|DateTimeReceived|tekenreeks|Nee |
|Bijlagen|matrix|Nee |
|Van|tekenreeks|Nee |
|CC|tekenreeks|Nee |
|BCC|tekenreeks|Nee |
|Onderwerp|tekenreeks|Ja |
|Hoofdtekst|tekenreeks|Ja |
|Belang|tekenreeks|Nee |
|IsHtml|Boole-waarde|Nee |
|Aan|tekenreeks|Ja |



### <a name="receiveattachment"></a>ReceiveAttachment


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|tekenreeks|Ja |
|ContentType|tekenreeks|Ja |
|@odata.type|tekenreeks|Nee |
|Naam|tekenreeks|Ja |
|ContentBytes|tekenreeks|Ja |



### <a name="digestmessage"></a>DigestMessage


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Onderwerp|tekenreeks|Ja |
|Hoofdtekst|tekenreeks|Nee |
|Belang|tekenreeks|Nee |
|Verificatiesamenvatting|matrix|Ja |
|Bijlagen|matrix|Nee |
|Aan|tekenreeks|Ja |



### <a name="triggerbatchresponsereceivemessage"></a>TriggerBatchResponse [ReceiveMessage]


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|waarde|matrix|Nee |



### <a name="datasetsmetadata"></a>DataSetsMetadata


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|in tabelvorm|niet gedefinieerd|Nee |
|BLOB|niet gedefinieerd|Nee |



### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|bron|tekenreeks|Nee |
|displayName|tekenreeks|Nee |
|urlEncoding|tekenreeks|Nee |
|tableDisplayName|tekenreeks|Nee |
|tablePluralName|tekenreeks|Nee |



### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|bron|tekenreeks|Nee |
|displayName|tekenreeks|Nee |
|urlEncoding|tekenreeks|Nee |



### <a name="tablemetadata"></a>TableMetadata


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|naam|tekenreeks|Nee |
|titel|tekenreeks|Nee |
|x-ms-machtiging|tekenreeks|Nee |
|x-ms-mogelijkheden|niet gedefinieerd|Nee |
|schema|niet gedefinieerd|Nee |



### <a name="tablecapabilitiesmetadata"></a>TableCapabilitiesMetadata


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|sortRestrictions|niet gedefinieerd|Nee |
|filterRestrictions|niet gedefinieerd|Nee |
|filterFunctions|matrix|Nee |



### <a name="tablesortrestrictionsmetadata"></a>TableSortRestrictionsMetadata


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|sorteerbare|Boole-waarde|Nee |
|unsortableProperties|matrix|Nee |
|ascendingOnlyProperties|matrix|Nee |



### <a name="tablefilterrestrictionsmetadata"></a>TableFilterRestrictionsMetadata


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Filterbaar|Boole-waarde|Nee |
|nonFilterableProperties|matrix|Nee |
|requiredProperties|matrix|Nee |



### <a name="optionsemailsubscription"></a>OptionsEmailSubscription


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|NotificationUrl|tekenreeks|Nee |
|Bericht|niet gedefinieerd|Nee |



### <a name="messagewithoptions"></a>MessageWithOptions


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Onderwerp|tekenreeks|Ja |
|Opties|tekenreeks|Ja |
|Hoofdtekst|tekenreeks|Nee |
|Belang|tekenreeks|Nee |
|Bijlagen|matrix|Nee |
|Aan|tekenreeks|Ja |



### <a name="subscriptionresponse"></a>SubscriptionResponse


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|tekenreeks|Nee |
|resource|tekenreeks|Nee |
|notificationType|tekenreeks|Nee |
|notificationUrl|tekenreeks|Nee |



### <a name="approvalemailsubscription"></a>ApprovalEmailSubscription


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|NotificationUrl|tekenreeks|Nee |
|Bericht|niet gedefinieerd|Nee |



### <a name="approvalmessage"></a>ApprovalMessage


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Onderwerp|tekenreeks|Ja |
|Opties|tekenreeks|Ja |
|Hoofdtekst|tekenreeks|Nee |
|Belang|tekenreeks|Nee |
|Bijlagen|matrix|Nee |
|Aan|tekenreeks|Ja |



### <a name="approvalemailresponse"></a>ApprovalEmailResponse


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|SelectedOption|tekenreeks|Nee |



### <a name="tableslist"></a>TablesList


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|waarde|matrix|Nee |



### <a name="table"></a>Tabel


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Naam|tekenreeks|Nee |
|DisplayName|tekenreeks|Nee |



### <a name="item"></a>Artikel


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ItemInternalId|tekenreeks|Nee |



### <a name="calendaritemslist"></a>CalendarItemsList


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|waarde|matrix|Nee |



### <a name="calendaritem"></a>CalendarItem


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ItemInternalId|tekenreeks|Nee |



### <a name="contactitemslist"></a>ContactItemsList


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|waarde|matrix|Nee |



### <a name="contactitem"></a>ContactItem


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ItemInternalId|tekenreeks|Nee |



### <a name="datasetslist"></a>DataSetsList


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|waarde|matrix|Nee |



### <a name="dataset"></a>DataSet


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Naam|tekenreeks|Nee |
|DisplayName|tekenreeks|Nee |


## <a name="next-steps"></a>Volgende stappen
[Maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md)