<properties
pageTitle="Wunderlist | Microsoft Azure"
description="Logica apps maken met Azure App-service. Wunderlist bieden een todo lijst en task manager waarmee mensen hun dingen voor elkaar krijgen.  Of u een boodschappenlijst met een een dierbare deelt, gemakkelijk werken aan een project of het plannen van een vakantie, Wunderlist vastleggen, delen en uw to¬dos te voltooien. Wunderlist direct wordt gesynchroniseerd tussen uw telefoon, tablet en computer, zodat u alle taken vanaf elke locatie openen kunt."
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

# <a name="get-started-with-the-wunderlist-connector"></a>Aan de slag met de connector Wunderlist

Wunderlist bieden een todo lijst en task manager waarmee mensen hun dingen voor elkaar krijgen.  Of u een boodschappenlijst met een een dierbare deelt, gemakkelijk werken aan een project of het plannen van een vakantie, Wunderlist vastleggen, delen en uw to¬dos te voltooien. Wunderlist direct wordt gesynchroniseerd tussen uw telefoon, tablet en computer, zodat u alle taken vanaf elke locatie openen kunt.

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op de schemaversie van logica apps 2015-08-01-voorbeeld. 

U kunt aan de slag met het maken van een app logica nu, Zie [maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties

De Wunderlist-connector kan worden gebruikt als een actie; trigger(s) heeft. Alle verbindingslijnen ondersteuning in JSON en XML-indelingen voor gegevens. 

 De connector Wunderlist heeft de volgende actie (s) en/of trigger(s) beschikbaar:

### <a name="wunderlist-actions"></a>Wunderlist acties
U kunt deze acties ondernemen:

|Actie|Beschrijving|
|--- | ---|
|[RetrieveLists](connectors-create-api-wunderlist.md#retrievelists)|De lijsten die zijn gekoppeld aan uw account worden opgehaald.|
|[CreateList](connectors-create-api-wunderlist.md#createlist)|Een lijst maken.|
|[ListTasks](connectors-create-api-wunderlist.md#listtasks)|Taken ophalen uit een specifieke lijst.|
|[CreateTask](connectors-create-api-wunderlist.md#createtask)|Een taak maken|
|[ListSubTasks](connectors-create-api-wunderlist.md#listsubtasks)|Subtaken worden opgehaald uit een specifieke lijst of een specifieke taak.|
|[CreateSubTask](connectors-create-api-wunderlist.md#createsubtask)|Binnen een bepaalde taak instellen als subtaak maken|
|[ListNotes](connectors-create-api-wunderlist.md#listnotes)|Notities voor een specifieke lijst of een bepaalde taak ophalen.|
|[CreateNote](connectors-create-api-wunderlist.md#createnote)|Een notitie toevoegen aan een bepaalde taak|
|[ListComments](connectors-create-api-wunderlist.md#listcomments)|Opmerkingen voor een specifieke lijst of een bepaalde taak taak ophalen.|
|[CreateComment](connectors-create-api-wunderlist.md#createcomment)|Een opmerking toevoegen aan een bepaalde taak|
|[RetrieveReminders](connectors-create-api-wunderlist.md#retrievereminders)|Herinneringen voor een specifieke lijst of een bepaalde taak ophalen.|
|[CreateReminder](connectors-create-api-wunderlist.md#createreminder)|Een herinnering instellen.|
|[RetrieveFiles](connectors-create-api-wunderlist.md#retrievefiles)|Ophalen van bestanden voor een specifieke lijst of een specifieke taak.|
|[GetList](connectors-create-api-wunderlist.md#getlist)|Haalt een specifieke lijst.|
|[DeleteList](connectors-create-api-wunderlist.md#deletelist)|Een lijst verwijderen|
|[UpdateList](connectors-create-api-wunderlist.md#updatelist)|Een specifieke lijst bijwerken|
|[GetTask](connectors-create-api-wunderlist.md#gettask)|Een specifieke taak opgehaald|
|[UpdateTask](connectors-create-api-wunderlist.md#updatetask)|Een specifieke taak werkt|
|[DeleteTask](connectors-create-api-wunderlist.md#deletetask)|Hiermee verwijdert u een specifieke taak|
|[GetSubTask](connectors-create-api-wunderlist.md#getsubtask)|Een specifieke subtaak opgehaald|
|[UpdateSubTask](connectors-create-api-wunderlist.md#updatesubtask)|Een specifieke subtaak wordt bijgewerkt|
|[DeleteSubTask](connectors-create-api-wunderlist.md#deletesubtask)|Hiermee verwijdert u een specifieke subtaak|
|[GetNote](connectors-create-api-wunderlist.md#getnote)|Ophalen van een specifieke notitie|
|[UpdateNote](connectors-create-api-wunderlist.md#updatenote)|Een specifieke notitie bijwerken|
|[DeleteNote](connectors-create-api-wunderlist.md#deletenote)|Een specifieke notitie verwijderen|
|[GetComment](connectors-create-api-wunderlist.md#getcomment)|De opmerking van een specifieke taak ophalen|
|[UpdateReminder](connectors-create-api-wunderlist.md#updatereminder)|Herinnering voor een specifieke update|
|[DeleteReminder](connectors-create-api-wunderlist.md#deletereminder)|Een bepaalde herinnering verwijderen|
### <a name="wunderlist-triggers"></a>Wunderlist triggers
U kunt voor deze gebeurtenis(sen) luisteren:

|Trigger | Beschrijving|
|--- | ---|
|Wanneer een taak moet worden betaald|Een nieuwe stroom wordt gegenereerd wanneer een taak in de lijst moet betaald worden|
|Wanneer u een nieuwe taak wordt gemaakt|Een nieuwe stroom wordt gegenereerd wanneer een nieuwe taak wordt gemaakt in de lijst|
|Wanneer een herinnering wordt uitgevoerd|Een nieuwe stroom wordt gegenereerd als een herinnering|


## <a name="create-a-connection-to-wunderlist"></a>Een verbinding maken met Wunderlist
Om logica apps met Wunderlist maakt, moet u eerst een **verbinding** maken en leveren de details voor de volgende eigenschappen: 

|Eigenschap| Vereist|Beschrijving|
| ---|---|---|
|Token|Ja|Wunderlist referenties opgeven|
Nadat u de verbinding hebt gemaakt, kunt u het uitvoeren van de acties te luisteren voor de triggers die in dit artikel beschreven. 


>[AZURE.INCLUDE [Steps to create a connection to Wunderlist](../../includes/connectors-create-api-wunderlist.md)] 


>[AZURE.TIP] U kunt deze verbinding gebruiken in andere apps logica.

## <a name="reference-for-wunderlist"></a>Verwijzing voor Wunderlist
Van toepassing op versie: 1.0

## <a name="triggertaskdue"></a>TriggerTaskDue
Wanneer een taak vervalt: een nieuwe stroom wordt gegenereerd wanneer een taak in de lijst moet betaald worden 

```GET: /trigger/tasksdue``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|geheel getal|Ja|query|geen|Lijst-ID|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|


## <a name="triggertasknew"></a>TriggerTaskNew
Wanneer u een nieuwe taak wordt gemaakt: een nieuwe stroom wordt gegenereerd wanneer een nieuwe taak wordt gemaakt in de lijst 

```GET: /trigger/tasksnew``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|geheel getal|Ja|query|geen|Lijst-ID|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|


## <a name="triggerreminder"></a>TriggerReminder
Wanneer een herinnering wordt uitgevoerd: een nieuwe stroom wordt gegenereerd als een herinnering 

```GET: /trigger/reminders``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|geheel getal|Ja|query|geen|Lijst-ID|
|TASK_ID|geheel getal|geen|query|geen|Taak-ID|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|


## <a name="retrievelists"></a>RetrieveLists
Lijsten opvragen: ophalen van de lijsten die zijn gekoppeld aan uw account. 

```GET: /lists``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|Ongeldig verzoek|
|500|Interne serverfout. Er is een onbekende fout opgetreden|
|Standaard|De bewerking is mislukt.|


## <a name="createlist"></a>CreateList
Een lijst maken: een lijst maken. 

```POST: /lists``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|Verzenden| |Ja|hoofdtekst|geen|Nieuwe lijst maken|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|Standaard|De bewerking is mislukt.|


## <a name="listtasks"></a>ListTasks
Ophalen van taken: taken ophalen uit een specifieke lijst. 

```GET: /tasks``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|geheel getal|Ja|query|geen|Lijst-ID|
|voltooid|Boole-waarde|geen|query|geen|Voltooid|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|Ongeldig verzoek|
|500|Interne serverfout. Er is een onbekende fout opgetreden|
|Standaard|De bewerking is mislukt.|


## <a name="createtask"></a>CreateTask
Maak een taak: een taak maken 

```POST: /tasks``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|Verzenden| |Ja|hoofdtekst|geen|Nieuwe taak maken|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|201|Gemaakt|


## <a name="listsubtasks"></a>ListSubTasks
Subtaken: subtaken ophalen uit een specifieke lijst of van een specifieke taak. 

```GET: /subtasks``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|geheel getal|Ja|query|geen|Lijst-ID|
|TASK_ID|geheel getal|geen|query|geen|Taak-ID|
|voltooid|Boole-waarde|geen|query|geen|Voltooid|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|Ongeldig verzoek|
|500|Interne serverfout. Er is een onbekende fout opgetreden|
|Standaard|De bewerking is mislukt.|


## <a name="createsubtask"></a>CreateSubTask
Maak een subtaak: een subtaak binnen een bepaalde taak maken 

```POST: /subtasks``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|Verzenden| |Ja|hoofdtekst|geen|Nieuwe subtaak moet worden gemaakt|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|201|Gemaakt|


## <a name="listnotes"></a>ListNotes
Notities: notities voor een specifieke lijst of een bepaalde taak ophalen. 

```GET: /notes``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|geheel getal|Ja|query|geen|Lijst-ID|
|TASK_ID|geheel getal|geen|query|geen|Taak-ID|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|Ongeldig verzoek|
|500|Interne serverfout. Er is een onbekende fout opgetreden|
|Standaard|De bewerking is mislukt.|


## <a name="createnote"></a>CreateNote
Een notitie maken: een notitie toevoegen aan een bepaalde taak 

```POST: /notes``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|Verzenden| |Ja|hoofdtekst|geen|Nieuwe opmerking wilt maken|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|201|Gemaakt|


## <a name="listcomments"></a>ListComments
Taak opmerkingen: opmerkingen voor een specifieke lijst of een bepaalde taak taak ophalen. 

```GET: /task_comments``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|geheel getal|Ja|query|geen|Lijst-ID|
|TASK_ID|geheel getal|geen|query|geen|Taak-ID|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|Ongeldig verzoek|
|500|Interne serverfout. Er is een onbekende fout opgetreden|
|Standaard|De bewerking is mislukt.|


## <a name="createcomment"></a>CreateComment
Een opmerking toevoegen aan een taak: een opmerking toevoegen aan een bepaalde taak 

```POST: /task_comments``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|Verzenden| |Ja|hoofdtekst|geen|Nieuwe taak opmerking moet worden gemaakt|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|201|Gemaakt|


## <a name="retrievereminders"></a>RetrieveReminders
Herinneringen ophalen: ophalen van herinneringen voor een specifieke lijst of een specifieke taak. 

```GET: /reminders``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|geheel getal|Ja|query|geen|Lijst-ID|
|TASK_ID|geheel getal|geen|query|geen|Taak-ID|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|Ongeldig verzoek|
|500|Interne serverfout. Er is een onbekende fout opgetreden|
|Standaard|De bewerking is mislukt.|


## <a name="createreminder"></a>CreateReminder
Een herinnering instellen: een herinnering instellen. 

```POST: /reminders``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|Verzenden| |Ja|hoofdtekst|geen|Nieuwe aanmaning moet worden gemaakt|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|Standaard|De bewerking is mislukt.|


## <a name="retrievefiles"></a>RetrieveFiles
Bestanden ophalen: ophalen van bestanden voor een specifieke lijst of een specifieke taak. 

```GET: /files``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|geheel getal|Ja|query|geen|Lijst-ID|
|TASK_ID|geheel getal|geen|query|geen|Taak-ID|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|Ongeldig verzoek|
|500|Interne serverfout. Er is een onbekende fout opgetreden|
|Standaard|De bewerking is mislukt.|


## <a name="getlist"></a>GetList
Overzicht: een specifieke lijst opgehaald 

```GET: /lists/{id}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|tekenreeks|Ja|pad|geen|Lijst-ID|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|


## <a name="deletelist"></a>DeleteList
Lijst verwijderen: Hiermee verwijdert u een lijst 

```DELETE: /lists/{id}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|geheel getal|Ja|pad|geen|Lijst-ID|
|revisie|geheel getal|Ja|query|geen|Revisie|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|204|Geen inhoud|


## <a name="updatelist"></a>UpdateList
Een lijst bijwerken: een specifieke lijst bijwerken 

```PATCH: /lists/{id}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|geheel getal|Ja|pad|geen|Lijst-ID|
|Verzenden| |Ja|hoofdtekst|geen|Lijst met details|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|


## <a name="gettask"></a>GetTask
Taak ophalen: haalt u een specifieke taak 

```GET: /tasks/{id}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|geheel getal|Ja|query|geen|Lijst-ID|
|ID|geheel getal|Ja|pad|geen|Taak-ID|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|


## <a name="updatetask"></a>UpdateTask
Een taak bijwerken: een specifieke taak werkt 

```PATCH: /tasks/{id}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|geheel getal|Ja|query|geen|Lijst-ID|
|ID|geheel getal|Ja|pad|geen|Taak-ID|
|Verzenden| |Ja|hoofdtekst|geen|Taakdetails|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|


## <a name="deletetask"></a>DeleteTask
Taak verwijderen: Hiermee verwijdert u een specifieke taak 

```DELETE: /tasks/{id}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|geheel getal|Ja|query|geen|Lijst-ID|
|ID|geheel getal|Ja|pad|geen|Taak-ID|
|revisie|geheel getal|Ja|query|geen|Revisie|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|204|Geen inhoud|


## <a name="getsubtask"></a>GetSubTask
Ophalen van de subtaak: een specifieke subtaak opgehaald 

```GET: /subtasks/{id}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|tekenreeks|Ja|pad|geen|Subtaak-ID|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|


## <a name="updatesubtask"></a>UpdateSubTask
Bijwerken van een subtaak: een specifieke subtaak wordt bijgewerkt 

```PATCH: /subtasks/{id}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|geheel getal|Ja|pad|geen|Subtaak-ID|
|Verzenden| |Ja|hoofdtekst|geen|Subtaak details|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|


## <a name="deletesubtask"></a>DeleteSubTask
Een subtaak verwijderen: Hiermee verwijdert u een specifieke subtaak 

```DELETE: /subtasks/{id}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|geheel getal|Ja|pad|geen|Subtaak-ID|
|revisie|geheel getal|Ja|query|geen|Revisie|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|204|Geen inhoud|


## <a name="getnote"></a>GetNote
Een notitie ophalen: ophalen van een specifieke notitie 

```GET: /notes/{id}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|tekenreeks|Ja|pad|geen|Opmerking-ID|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|


## <a name="updatenote"></a>UpdateNote
Een notitie bijwerken: een specifieke notitie bijwerken 

```PATCH: /notes/{id}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|geheel getal|Ja|pad|geen|Opmerking-ID|
|Verzenden| |Ja|hoofdtekst|geen|Opmerking details|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|


## <a name="deletenote"></a>DeleteNote
Een notitie verwijderen: een specifieke notitie verwijderen 

```DELETE: /notes/{id}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|geheel getal|Ja|pad|geen|Opmerking-ID|
|revisie|geheel getal|Ja|query|geen|Revisie|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|204|Geen inhoud|


## <a name="getcomment"></a>GetComment
Opmerking taak ophalen: ophalen van een bepaalde taak Opmerking 

```GET: /task_comments/{id}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|tekenreeks|Ja|pad|geen|Opmerking-ID|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|


## <a name="updatereminder"></a>UpdateReminder
Een herinnering bijwerken: een specifieke aanmaning bijwerken 

```PATCH: /reminders/{id}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|geheel getal|Ja|pad|geen|Herinnering-ID|
|Verzenden| |Ja|hoofdtekst|geen|Herinnering-details|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|


## <a name="deletereminder"></a>DeleteReminder
Een herinnering verwijderen: verwijderen van een bepaalde herinnering 

```DELETE: /reminders/{id}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|geheel getal|Ja|pad|geen|De ID van de aanmaning.|
|revisie|geheel getal|Ja|query|geen|Revisie|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|204|Geen inhoud|


## <a name="object-definitions"></a>Objectdefinities 

### <a name="list"></a>Lijst


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|geheel getal|Nee |
|created_at|tekenreeks|Nee |
|titel|tekenreeks|Nee |
|list_type|tekenreeks|Nee |
|type|tekenreeks|Nee |
|revisie|geheel getal|Nee |



### <a name="createdlist"></a>CreatedList


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|geheel getal|Nee |
|created_at|tekenreeks|Nee |
|titel|tekenreeks|Nee |
|revisie|geheel getal|Nee |
|type|tekenreeks|Nee |



### <a name="task"></a>Taak


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|geheel getal|Nee |
|assignee_id|geheel getal|Nee |
|assigner_id|geheel getal|Nee |
|created_at|tekenreeks|Nee |
|created_by_id|geheel getal|Nee |
|due_date|tekenreeks|Nee |
|list_id|geheel getal|Nee |
|revisie|geheel getal|Nee |
|starred|Boole-waarde|Nee |
|titel|tekenreeks|Nee |



### <a name="subtask"></a>Subtaak


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|geheel getal|Nee |
|TASK_ID|geheel getal|Nee |
|created_at|tekenreeks|Nee |
|created_by_id|geheel getal|Nee |
|revisie|tekenreeks|Nee |
|titel|tekenreeks|Nee |



### <a name="note"></a>Opmerking


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|geheel getal|Nee |
|TASK_ID|geheel getal|Nee |
|inhoud|tekenreeks|Nee |
|created_at|tekenreeks|Nee |
|updated_at|tekenreeks|Nee |
|revisie|geheel getal|Nee |



### <a name="comment"></a>Opmerking


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|geheel getal|Nee |
|TASK_ID|geheel getal|Nee |
|revisie|geheel getal|Nee |
|tekst|tekenreeks|Nee |
|type|tekenreeks|Nee |
|created_at|tekenreeks|Nee |



### <a name="reminder"></a>Herinnering


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|geheel getal|Nee |
|datum|tekenreeks|Nee |
|TASK_ID|geheel getal|Nee |
|revisie|geheel getal|Nee |
|type|tekenreeks|Nee |
|created_at|tekenreeks|Nee |
|updated_at|tekenreeks|Nee |



### <a name="createdreminder"></a>CreatedReminder


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|geheel getal|Nee |
|datum|tekenreeks|Nee |
|TASK_ID|geheel getal|Nee |
|revisie|geheel getal|Nee |
|created_at|tekenreeks|Nee |
|updated_at|tekenreeks|Nee |



### <a name="file"></a>Bestand


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|geheel getal|Nee |
|URL|tekenreeks|Nee |
|TASK_ID|geheel getal|Nee |
|list_id|geheel getal|Nee |
|USER_ID|geheel getal|Nee |
|bestandsnaam|tekenreeks|Nee |
|CONTENT_TYPE|tekenreeks|Nee |
|file_size|geheel getal|Nee |
|local_created_at|tekenreeks|Nee |
|created_at|tekenreeks|Nee |
|updated_at|tekenreeks|Nee |
|type|tekenreeks|Nee |
|revisie|geheel getal|Nee |



### <a name="newtask"></a>NewTask


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|list_id|geheel getal|Ja |
|titel|tekenreeks|Ja |
|assignee_id|geheel getal|Nee |
|voltooid|Boole-waarde|Nee |
|recurrence_type|tekenreeks|Nee |
|recurrence_count|geheel getal|Nee |
|due_date|tekenreeks|Nee |
|starred|Boole-waarde|Nee |



### <a name="newlist"></a>NewList


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|titel|tekenreeks|Ja |



### <a name="newsubtask"></a>NewSubtask


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|list_id|geheel getal|Ja |
|TASK_ID|geheel getal|Ja |
|titel|tekenreeks|Ja |
|voltooid|Boole-waarde|Nee |



### <a name="newnote"></a>NewNote


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|list_id|geheel getal|Ja |
|TASK_ID|geheel getal|Ja |
|inhoud|tekenreeks|Ja |



### <a name="newcomment"></a>NieuweOpmerking


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|list_id|geheel getal|Ja |
|TASK_ID|geheel getal|Ja |
|tekst|tekenreeks|Ja |



### <a name="newreminder"></a>NewReminder


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|list_id|geheel getal|Ja |
|TASK_ID|geheel getal|Ja |
|datum|tekenreeks|Ja |



### <a name="updatetask"></a>UpdateTask


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|revisie|geheel getal|Nee |
|titel|tekenreeks|Nee |
|assignee_id|geheel getal|Nee |
|voltooid|Boole-waarde|Nee |
|recurrence_type|tekenreeks|Nee |
|recurrence_count|geheel getal|Nee |
|due_date|tekenreeks|Nee |
|starred|Boole-waarde|Nee |



### <a name="updatelist"></a>UpdateList


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|revisie|geheel getal|Nee |
|titel|tekenreeks|Nee |



### <a name="updatesubtask"></a>UpdateSubtask


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|revisie|geheel getal|Nee |
|titel|tekenreeks|Nee |
|voltooid|Boole-waarde|Nee |



### <a name="updatenote"></a>UpdateNote


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|revisie|geheel getal|Nee |
|inhoud|tekenreeks|Nee |



### <a name="updatereminder"></a>UpdateReminder


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|datum|tekenreeks|Nee |
|revisie|geheel getal|Nee |


## <a name="next-steps"></a>Volgende stappen
[Maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md)