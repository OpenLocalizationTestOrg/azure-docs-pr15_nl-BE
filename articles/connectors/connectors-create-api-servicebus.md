<properties
pageTitle="Leren werken met de connector Azure Service Bus in de logica apps | Microsoft Azure"
description="Logica apps maken met Azure App-service. Verbinding maken met Azure Service Bus voor het verzenden en ontvangen van berichten. U kunt acties uitvoeren zoals het verzenden naar de wachtrij verzenden naar onderwerp, ontvangen van de wachtrij en van abonnement ontvangt."
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
ms.date="08/02/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-azure-service-bus-connector"></a>Aan de slag met Azure Service Bus connector

Verbinding maken met Azure Service Bus voor het verzenden en ontvangen van berichten. U kunt acties uitvoeren zoals het verzenden naar de wachtrij verzenden naar onderwerp, ontvangen van de wachtrij en van abonnement ontvangt.

[Een connector](./apis-list.md)gebruikt, moet u eerst een logica app maken. U kunt aan de slag door het [maken van een logica app nu](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-service-bus"></a>Verbinding maken met de Service Bus

Voordat uw logica app toegang een service tot, moet u eerst een verbinding maken met de service. Een [verbinding](./connectors-overview.md) biedt connectiviteit tussen een app logica en een andere service.  

>[AZURE.INCLUDE [Steps to create a connection to Azure Service Bus](../../includes/connectors-create-api-servicebus.md)]

## <a name="use-a-service-bus-trigger"></a>Gebruik een trigger Service Bus

Een trigger is een gebeurtenis die kan worden gebruikt voor het starten van de werkstroom in een app logica gedefinieerd. [Meer informatie over triggers](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

>[AZURE.INCLUDE [Steps to create a Service Bus trigger](../../includes/connectors-create-api-servicebus-trigger.md)]  

## <a name="use-a-service-bus-action"></a>Bus Service actie

Een actie is een bewerking die wordt uitgevoerd door de werkstroom in een app logica gedefinieerd. [Meer informatie over acties](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

[AZURE.INCLUDE [Steps to create a Service Bus action](../../includes/connectors-create-api-servicebus-action.md)]  

## <a name="technical-details"></a>Technische details

Hier vindt u de details van de triggers, acties en reacties die deze verbinding ondersteunt.

### <a name="service-bus-triggers"></a>De triggers-Service Bus

Bus-service heeft de volgende triggers:  

|Trigger | Beschrijving|
|--- | ---|
|[Wanneer een bericht wordt ontvangen in een wachtrij](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-queue)|Deze bewerking wordt gegenereerd wanneer een bericht wordt ontvangen in een wachtrij een stroom.|
|[Wanneer een bericht wordt ontvangen van een abonnement op onderwerp](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-topic-subscription)|Deze bewerking wordt een stroom gegenereerd wanneer een bericht wordt ontvangen van een abonnement op onderwerp.|


### <a name="service-bus-actions"></a>Acties Service Bus

Bus-service heeft de volgende acties:


|Actie|Beschrijving|
|--- | ---|
|[Bericht verzenden](connectors-create-api-servicebus.md#send-message)|Deze bewerking wordt een bericht verzonden naar een wachtrij of onderwerp.|
### <a name="action-and-trigger-details"></a>Actie en trigger details

Hier vindt u de details van de acties en triggers voor deze connector, samen met hun antwoorden.



#### <a name="send-message"></a>Bericht verzenden

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|ContentData *|Inhoud|De inhoud van het bericht.|
|ContentType|Type inhoud|Het inhoudstype van de inhoud van het bericht.|
|Eigenschappen|Eigenschappen|Sleutel / waarde-paren voor elk brokered eigenschap.|
|entityName *|De naam van wachtrij/onderwerp|De naam van de wachtrij of onderwerp.|

Deze geavanceerde parameters zijn ook beschikbaar:

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|MessageId|Bericht-Id|Een door de gebruiker gedefinieerde waarde die Service Bus met dubbele berichten identificeren als ingeschakeld.|
|Aan|Aan|Adres te verzenden.|
|ReplyTo|Beantwoorden|Adres van de wachtrij te beantwoorden.|
|ReplyToSessionId|Antwoord op de sessie-Id|De id van de sessie te beantwoorden.|
|Label|Label|Toepassingsspecifieke label.|
|ScheduledEnqueueTimeUtc|ScheduledEnqueueTimeUtc|Datum en tijd in UTC, wanneer het bericht wordt toegevoegd aan de wachtrij.|
|Sessie-id|Sessie-Id|De id van de sessie.|
|CorrelationId|Correlatie-Id|Id van de correlatie.|
|TimeToLive|Levensduur|De duur in tikken, dat een bericht ongeldig is. De duur wordt gestart wanneer het bericht is verzonden naar Service Bus.|



Een * geeft aan dat een eigenschap vereist is.


#### <a name="when-a-message-is-received-in-a-queue"></a>Wanneer een bericht wordt ontvangen in een wachtrij

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|Wachtrijnaam *|Wachtrijnaam|De naam van de wachtrij.|


Een * geeft aan dat een eigenschap vereist is.


##### <a name="output-details"></a>Uitvoerdetails

ServiceBusMessage: Dit object is de inhoud en eigenschappen van een bericht Service Bus.


| Naam van eigenschap | Gegevenstype | Beschrijving |
|---|---|---|
|ContentData|tekenreeks|De inhoud van het bericht.|
|ContentType|tekenreeks|Het inhoudstype van de inhoud van het bericht.|
|Eigenschappen|object|Sleutel / waarde-paren voor elk brokered eigenschap.|
|MessageId|tekenreeks|Een door de gebruiker gedefinieerde waarde die Service Bus met dubbele berichten identificeren als ingeschakeld.|
|Aan|tekenreeks|Verzenden naar het adres.|
|ReplyTo|tekenreeks|Adres van de wachtrij te beantwoorden.|
|ReplyToSessionId|tekenreeks|De id van de sessie te beantwoorden.|
|Label|tekenreeks|Toepassingsspecifieke label.|
|ScheduledEnqueueTimeUtc|tekenreeks|Datum en tijd in UTC, wanneer het bericht wordt toegevoegd aan de wachtrij.|
|Sessie-id|tekenreeks|De id van de sessie.|
|CorrelationId|tekenreeks|Id van de correlatie.|
|TimeToLive|tekenreeks|De duur in tikken, dat een bericht ongeldig is. De duur wordt gestart wanneer het bericht is verzonden naar Service Bus.|




#### <a name="when-a-message-is-received-in-a-topic-subscription"></a>Wanneer een bericht wordt ontvangen van een abonnement op onderwerp

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|topicName *|De naam van onderwerp|De naam van het onderwerp.|
|subscriptionName *|Onderwerp de naam van abonnement|De naam van het onderwerp-abonnement.|


Een * geeft aan dat een eigenschap vereist is.


##### <a name="output-details"></a>Uitvoerdetails

ServiceBusMessage: Dit object is de inhoud en eigenschappen van een bericht Service Bus.


| Naam van eigenschap | Gegevenstype | Beschrijving |
|---|---|---|
|ContentData|tekenreeks|De inhoud van het bericht.|
|ContentType|tekenreeks|Het inhoudstype van de inhoud van het bericht.|
|Eigenschappen|object|Sleutel / waarde-paren voor elk brokered eigenschap.|
|MessageId|tekenreeks|Een door de gebruiker gedefinieerde waarde die Service Bus met dubbele berichten identificeren als ingeschakeld.|
|Aan|tekenreeks|Verzenden naar het adres.|
|ReplyTo|tekenreeks|Adres van de wachtrij te beantwoorden.|
|ReplyToSessionId|tekenreeks|De id van de sessie te beantwoorden.|
|Label|tekenreeks|Toepassingsspecifieke label.|
|ScheduledEnqueueTimeUtc|tekenreeks|Datum en tijd in UTC, wanneer het bericht wordt toegevoegd aan de wachtrij.|
|Sessie-id|tekenreeks|De id van de sessie.|
|CorrelationId|tekenreeks|Id van de correlatie.|
|TimeToLive|tekenreeks|De duur in tikken, dat een bericht ongeldig is. De duur wordt gestart wanneer het bericht is verzonden naar Service Bus.|



### <a name="http-responses"></a>HTTP-antwoorden

De voorgaande triggers en acties kunnen u een of meer van de volgende HTTP-statuscodes retourneren:

|Naam|Beschrijving|
|---|---|
|200|OK|
|202|Geaccepteerd|
|400|Ongeldig verzoek|
|401|Niet-geautoriseerde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Er is een onbekende fout opgetreden.|
|Standaard|De bewerking is mislukt.|

## <a name="next-steps"></a>Volgende stappen
[Maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).
