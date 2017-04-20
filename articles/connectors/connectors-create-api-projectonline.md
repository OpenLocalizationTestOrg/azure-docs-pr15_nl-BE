<properties
pageTitle="ProjectOnline | Microsoft Azure"
description="Logica apps maken met Azure App-service. Project Online is een flexibele online oplossing voor project portfolio management (PPM) en het dagelijkse werk van Microsoft. Geleverd via Office 365, Project Online kunnen organisaties snel aan de slag met krachtige mogelijkheden voor projectbeheer plannen, prioriteit en beheren van projecten en investeringen van project portfolio, vrijwel overal op vrijwel elk apparaat."
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

# <a name="get-started-with-the-projectonline-connector"></a>Aan de slag met de ProjectOnline-connector

Project Online is een flexibele online oplossing voor project portfolio management (PPM) en het dagelijkse werk van Microsoft. Geleverd via Office 365, Project Online kunnen organisaties snel aan de slag met krachtige mogelijkheden voor projectbeheer plannen, prioriteit en beheren van projecten en investeringen van project portfolio, vrijwel overal op vrijwel elk apparaat.

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op de schemaversie van logica apps 2015-08-01-voorbeeld. 

U kunt aan de slag met het maken van een app logica nu, Zie [maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties

De ProjectOnline-connector kan worden gebruikt als een actie; trigger(s) heeft. Alle verbindingslijnen ondersteuning in JSON en XML-indelingen voor gegevens. 

 De ProjectOnline-connector is de volgende actie (s) en/of trigger(s) beschikbaar:

### <a name="projectonline-actions"></a>ProjectOnline acties
U kunt deze acties ondernemen:

|Actie|Beschrijving|
|--- | ---|
|[ListProjects](connectors-create-api-projectonline.md#listprojects)|Geeft een overzicht van de projecten in uw project on line site|
|[CreateProject](connectors-create-api-projectonline.md#createproject)|Hiermee maakt u een nieuw project in uw project on line site|
|[CreateTask](connectors-create-api-projectonline.md#createtask)|Hiermee maakt u een nieuwe taak in project voor u|
|[CreateResource](connectors-create-api-projectonline.md#createresource)|Hiermee maakt u een ondernemingsresources in uw project on line site|
|[ListTasks](connectors-create-api-projectonline.md#listtasks)|Geeft een overzicht van de gepubliceerde taken in een project|
|[CheckoutProject](connectors-create-api-projectonline.md#checkoutproject)|Controles van een project op uw site|
|[PublishProject](connectors-create-api-projectonline.md#publishproject)|Inchecken, publiceren en bestaande project op uw site|
### <a name="projectonline-triggers"></a>ProjectOnline-triggers
U kunt voor deze gebeurtenis(sen) luisteren:

|Trigger | Beschrijving|
|--- | ---|
|Wanneer u een nieuw project wordt gemaakt|Een stroom wordt gegenereerd wanneer een nieuw project wordt gemaakt.|
|Wanneer een nieuwe resource wordt gemaakt|Een nieuwe stroom wordt gegenereerd wanneer een nieuwe resource wordt gemaakt.|
|Wanneer u een nieuwe taak wordt gemaakt|Een stroom wordt gegenereerd wanneer een nieuwe taak wordt gemaakt.|


## <a name="create-a-connection-to-projectonline"></a>Een verbinding maken met ProjectOnline
Logica apps maken met ProjectOnline, moet u eerst een **verbinding** maken en de informatie geven voor de volgende eigenschappen: 

|Eigenschap| Vereist|Beschrijving|
| ---|---|---|
|Token|Ja|ProjectOnline referenties|

>[AZURE.INCLUDE [Steps to create a connection to ProjectOnline](../../includes/connectors-create-api-projectonline.md)]

>[AZURE.TIP] U kunt deze verbinding gebruiken in andere apps logica.

## <a name="reference-for-projectonline"></a>Verwijzing naar ProjectOnline
Van toepassing op versie: 1.0

## <a name="onnewproject"></a>OnNewProject
Wanneer u een nieuw project wordt gemaakt: een stroom wordt gegenereerd wanneer een nieuw project wordt gemaakt. 

```GET: /trigger/_api/ProjectData/Projects``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|siteUrl|tekenreeks|Ja|query|geen|De hoofd-url van uw projectsite site (voorbeeld: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="onnewresource"></a>OnNewResource
Wanneer een nieuwe resource wordt gemaakt: een nieuwe stroom wordt gegenereerd wanneer een nieuwe resource wordt gemaakt. 

```GET: /trigger/_api/ProjectData/Resources``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|siteUrl|tekenreeks|Ja|query|geen|De hoofd-url van uw projectsite site (voorbeeld: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="onnewtask"></a>OnNewTask
Wanneer u een nieuwe taak wordt gemaakt: een stroom wordt gegenereerd wanneer een nieuwe taak wordt gemaakt. 

```GET: /trigger/_api/ProjectData/Tasks``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|siteUrl|tekenreeks|Ja|query|geen|De hoofd-url van uw projectsite site (voorbeeld: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="listprojects"></a>ListProjects
Lijst van projecten: geeft een overzicht van de projecten in uw project on line site 

```GET: /_api/ProjectServer/Projects``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|siteUrl|tekenreeks|Ja|query|geen|De hoofd-url van uw projectsite site (voorbeeld: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="createproject"></a>CreateProject
Met deze methode maakt een nieuw project: een nieuw project maakt in uw project on line site 

```POST: /_api/ProjectServer/Projects``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|siteUrl|tekenreeks|Ja|query|geen|De hoofd-url van uw projectsite site (voorbeeld: https://sampletenant.sharepoint.com/teams/sampleteam)|
|Proj| |Ja|hoofdtekst|geen|Nieuw project maken|

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


## <a name="createtask"></a>CreateTask
Met deze methode maakt een nieuwe taak: Hiermee maakt u een nieuwe taak in project voor u 

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Tasks/Add``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|siteUrl|tekenreeks|Ja|query|geen|De hoofd-url van uw projectsite site (voorbeeld: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|tekenreeks|Ja|pad|geen|Unieke ID van het project dat de taak toevoegen|
|taak| |Ja|hoofdtekst|geen|Nieuwe taak toe te voegen aan het project|

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


## <a name="createresource"></a>CreateResource
Nieuwe resource maken: maakt een ondernemingsresources in uw project on line site 

```POST: /_api/ProjectServer/EnterpriseResources``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|siteUrl|tekenreeks|Ja|query|geen|De hoofd-url van uw projectsite site (voorbeeld: https://sampletenant.sharepoint.com/teams/sampleteam)|
|resource| |Ja|hoofdtekst|geen|Nieuwe ondernemingsresource toe te voegen aan het project|

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


## <a name="listtasks"></a>ListTasks
Geeft een overzicht van taken: geeft een overzicht van de gepubliceerde taken in een project 

```GET: /_api/ProjectServer/Projects('{project_id}')/Tasks``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|siteUrl|tekenreeks|Ja|query|geen|De hoofd-url van uw projectsite site (voorbeeld: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|tekenreeks|Ja|pad|geen|De unieke ID van het project voor het ophalen van taken|

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


## <a name="checkoutproject"></a>CheckoutProject
Afhandeling van een project: een project in uw site uitgecheckt 

```POST: /_api/ProjectServer/Projects('{project_id}')/checkOut``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|siteUrl|tekenreeks|Ja|query|geen|De hoofd-url van uw projectsite site (voorbeeld: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|tekenreeks|Ja|pad|geen|Unieke ID van het project dat de taak toevoegen|

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


## <a name="publishproject"></a>PublishProject
Inchecken en project publiceren: inchecken en publiceren en bestaande project op uw site 

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Publish(true)``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|siteUrl|tekenreeks|Ja|query|geen|De hoofd-url van uw projectsite site (voorbeeld: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|tekenreeks|Ja|pad|geen|De unieke ID van het project wilt inchecken|

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

### <a name="triggerprojectswrapper"></a>TriggerProjectsWrapper


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|waarde|matrix|Nee |



### <a name="triggerproject"></a>TriggerProject


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ProjectStartDate|tekenreeks|Nee |
|ProjectFinishDate|tekenreeks|Nee |
|ProjectCreatedDate|tekenreeks|Nee |
|ProjectId|tekenreeks|Nee |
|ProjectModifiedDate|tekenreeks|Nee |
|ProjectType|geheel getal|Nee |
|Projectnaam|tekenreeks|Nee |



### <a name="triggerresourceswrapper"></a>TriggerResourcesWrapper


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|waarde|matrix|Nee |



### <a name="triggerresource"></a>TriggerResource


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ResourceId|tekenreeks|Nee |
|ResourceBaseCalendar|tekenreeks|Nee |
|ResourceBookingType|geheel getal|Nee |
|ResourceCanLevel|Boole-waarde|Nee |
|ResourceCostPerUse|nummer|Nee |
|ResourceCreatedDate|tekenreeks|Nee |
|ResourceEarliestAvailableFrom|tekenreeks|Nee |
|ResourceEmail|tekenreeks|Nee |
|ResourceInitials|tekenreeks|Nee |
|ResourceIsActive|Boole-waarde|Nee |
|ResourceIsGeneric|Boole-waarde|Nee |
|ResourceLatestAvailableTo|tekenreeks|Nee |
|ResourceModifiedDate|tekenreeks|Nee |
|ResourceName|tekenreeks|Nee |
|ResourceStatsuName|tekenreeks|Nee |
|Brontype|geheel getal|Nee |
|TypeDescription|tekenreeks|Nee |
|TypeName|tekenreeks|Nee |



### <a name="triggertaskswrapper"></a>TriggerTasksWrapper


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|waarde|matrix|Nee |



### <a name="triggertask"></a>TriggerTask


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ProjectId|tekenreeks|Nee |
|TaskId|tekenreeks|Nee |
|Projectnaam|tekenreeks|Nee |
|Taaknaam|tekenreeks|Nee |
|TaskCreatedDate|tekenreeks|Nee |
|TaskModifieddate|tekenreeks|Nee |
|TaskStartDate|tekenreeks|Nee |
|TaskFinishDate|tekenreeks|Nee |
|TaskPriority|geheel getal|Nee |
|TaskIsActive|Boole-waarde|Nee |



### <a name="newproject"></a>NewProject


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Naam|tekenreeks|Ja |
|Beschrijving|tekenreeks|Nee |
|Start|tekenreeks|Nee |



### <a name="newreource"></a>NewReource


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Naam|tekenreeks|Ja |
|IsBudget|Boole-waarde|Nee |
|IsGeneric|Boole-waarde|Nee |
|IsInactive|Boole-waarde|Nee |



### <a name="project"></a>Project


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ApprovedStart|tekenreeks|Nee |
|ApprovedEnd|tekenreeks|Nee |
|CheckedOutDate|tekenreeks|Nee |
|CheckOutDescription|tekenreeks|Nee |
|CheckOutId|tekenreeks|Nee |
|CreatedDate|tekenreeks|Nee |
|ID|tekenreeks|Nee |
|IsCheckedOut|Boole-waarde|Nee |
|LastPublishedDate|tekenreeks|Nee |
|Datum laatst opgeslagen|tekenreeks|Nee |
|OptimizerDecision|geheel getal|Nee |
|PlannerDecision|geheel getal|Nee |
|ProjectType|geheel getal|Nee |
|Naam|tekenreeks|Nee |
|WinprojVersion|tekenreeks|Nee |



### <a name="projectswrapper"></a>ProjectsWrapper


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|waarde|matrix|Nee |



### <a name="newtask"></a>NewTask


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|parameters|niet gedefinieerd|Ja |



### <a name="taskparameters"></a>TaskParameters


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Naam|tekenreeks|Ja |
|Notities|tekenreeks|Nee |
|Start|tekenreeks|Nee |
|Duur|tekenreeks|Nee |



### <a name="enterpriseresource"></a>EnterpriseResource


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|CanLevel|Boole-waarde|Nee |
|Code|tekenreeks|Nee |
|CostAccrual|geheel getal|Nee |
|CostCenter|tekenreeks|Nee |
|Gemaakt|tekenreeks|Nee |
|DefaultBookingType|geheel getal|Nee |
|E-mail|tekenreeks|Nee |
|ExternalId|tekenreeks|Nee |
|Groep|tekenreeks|Nee |
|In dienst|tekenreeks|Nee |
|ID|tekenreeks|Nee |
|Initialen|tekenreeks|Nee |
|IsActive|Boole-waarde|Nee |
|IsBudget|Boole-waarde|Nee |
|IsCheckedOut|Boole-waarde|Nee |
|IsGeneric|Boole-waarde|Nee |
|IsTeam|Boole-waarde|Nee |
|MaterialLabel|tekenreeks|Nee |
|Gewijzigd|tekenreeks|Nee |
|Naam|tekenreeks|Nee |
|Fonetiek|tekenreeks|Nee |
|Brontype|geheel getal|Nee |
|TerminationDate|tekenreeks|Nee |



### <a name="taskswrapper"></a>TasksWrapper


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|waarde|matrix|Nee |



### <a name="task"></a>Taak


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Gemaakt|tekenreeks|Nee |
|Gewijzigd|tekenreeks|Nee |
|Start|tekenreeks|Nee |
|Voltooien|tekenreeks|Nee |
|Naam|tekenreeks|Nee |
|ID|tekenreeks|Nee |
|Prioriteit|geheel getal|Nee |
|PercentComplete|geheel getal|Nee |
|Notities|tekenreeks|Nee |
|Contact|tekenreeks|Nee |


## <a name="next-steps"></a>Volgende stappen
[Maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md)