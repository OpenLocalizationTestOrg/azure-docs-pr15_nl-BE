<properties
pageTitle="GitHub | Microsoft Azure"
description="Logica apps maken met Azure App-service. GitHub is een Git webgebaseerde opslagplaats hostingservice. Beschikt u over de herziening van gedistribueerde controle- en code management (SCM) functionaliteit van Git, alsmede een eigen functies toe te voegen."
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

# <a name="get-started-with-the-github-connector"></a>Aan de slag met de GitHub-connector

GitHub is een Git webgebaseerde opslagplaats hostingservice. Beschikt u over de herziening van gedistribueerde controle- en code management (SCM) functionaliteit van Git, alsmede een eigen functies toe te voegen.

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op de schemaversie van logica apps 2015-08-01-voorbeeld. 

U kunt aan de slag met het maken van een app logica nu, Zie [maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties

De GitHub-connector kan worden gebruikt als een actie; trigger(s) heeft. Alle verbindingslijnen ondersteuning in JSON en XML-indelingen voor gegevens. 

 De GitHub-connector is de volgende actie (s) en/of trigger(s) beschikbaar:

### <a name="github-actions"></a>GitHub acties
U kunt deze acties ondernemen:

|Actie|Beschrijving|
|--- | ---|
|[CreateIssue](connectors-create-api-github.md#createissue)|Hiermee maakt u een probleem|
### <a name="github-triggers"></a>GitHub triggers
U kunt voor deze gebeurtenis(sen) luisteren:

|Trigger | Beschrijving|
|--- | ---|
|Wanneer een probleem wordt geopend|Een probleem wordt geopend.|
|Wanneer er een probleem is gesloten|Een probleem is gesloten|
|Wanneer een probleem wordt toegewezen|Een probleem is toegewezen.|


## <a name="create-a-connection-to-github"></a>Een verbinding maken met GitHub
Logica apps maken met GitHub, moet u eerst een **verbinding** maken en de informatie geven voor de volgende eigenschappen: 

|Eigenschap| Vereist|Beschrijving|
| ---|---|---|
|Token|Ja|Referenties van GitHub|
Nadat u de verbinding hebt gemaakt, kunt u het uitvoeren van de acties te luisteren voor de triggers die in dit artikel beschreven. 

>[AZURE.INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]

>[AZURE.TIP] U kunt deze verbinding gebruiken in andere apps logica.

## <a name="reference-for-github"></a>Verwijzing voor GitHub
Van toepassing op versie: 1.0

## <a name="createissue"></a>CreateIssue
Een kwestie maken: Hiermee maakt u een probleem 

```POST: /repos/{repositoryOwner}/{repositoryName}/issues``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|repositoryOwner|tekenreeks|Ja|pad|geen|De eigenaar van de opslagplaats|
|repositoryName|tekenreeks|Ja|pad|geen|Naam opslagplaats|
|issueBasicDetails| |Ja|hoofdtekst|geen|Probleemdetails|

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


## <a name="issueopened"></a>IssueOpened
Wanneer een probleem wordt geopend: een probleem wordt geopend. 

```GET: /trigger/issueOpened``` 

Er zijn geen parameters voor deze oproep
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


## <a name="issueclosed"></a>IssueClosed
Wanneer er een probleem is gesloten: een probleem is gesloten 

```GET: /trigger/issueClosed``` 

Er zijn geen parameters voor deze oproep
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


## <a name="issueassigned"></a>IssueAssigned
Wanneer een probleem wordt toegewezen: een probleem is toegewezen. 

```GET: /trigger/issueAssigned``` 

Er zijn geen parameters voor deze oproep
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

### <a name="issuebasicdetailsmodel"></a>IssueBasicDetailsModel


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|titel|tekenreeks|Ja |
|hoofdtekst|tekenreeks|Ja |
|de toegewezen gebruiker|tekenreeks|Ja |



### <a name="issuedetailsmodel"></a>IssueDetailsModel


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|titel|tekenreeks|Ja |
|hoofdtekst|tekenreeks|Ja |
|de toegewezen gebruiker|tekenreeks|Ja |
|nummer|tekenreeks|Nee |
|staat|tekenreeks|Nee |
|created_at|tekenreeks|Nee |
|repository_url|tekenreeks|Nee |


## <a name="next-steps"></a>Volgende stappen
[Maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md)