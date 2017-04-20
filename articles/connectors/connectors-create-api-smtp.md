<properties
pageTitle="SMTP | Microsoft Azure"
description="Logica apps maken met Azure App-service. Verbinding maken met SMTP om e-mail te verzenden."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="app-service-logic"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="07/15/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-smtp-connector"></a>Aan de slag met de SMTP-connector

Verbinding maken met SMTP om e-mail te verzenden.

[Een connector](./apis-list.md)gebruikt, moet u eerst een logica app maken. U kunt aan de slag door het [maken van een logica app nu](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-smtp"></a>Verbinding maken met SMTP

Voordat uw logica app toegang een service tot, moet u eerst een *verbinding* met de service. Een [verbinding](./connectors-overview.md) biedt connectiviteit tussen een app logica en een andere service. Bijvoorbeeld, de verbinding met SMTP, moet u eerst een SMTP- *verbinding*. Een verbinding wilt maken, moet u de referenties gebruikt voor toegang tot de service die u verbinding wilt maken. Dus in het voorbeeld SMTP moet u de referenties voor uw naam, adres van de SMTP-server en aanmeldingsgegevens van de gebruiker om de verbinding maken met SMTP. [Meer informatie over verbindingen]()  

### <a name="create-a-connection-to-smtp"></a>Een verbinding maken met SMTP

>[AZURE.INCLUDE [Steps to create a connection to SMTP](../../includes/connectors-create-api-smtp.md)]

## <a name="use-an-smtp-trigger"></a>Gebruik een SMTP-trigger

Een trigger is een gebeurtenis die kan worden gebruikt voor het starten van de werkstroom in een app logica gedefinieerd. [Meer informatie over triggers](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

In dit voorbeeld omdat SMTP niet een signaal van zijn eigen, gebruikt de trigger **televergaderingen - als een object is gemaakt** . Deze trigger wordt geactiveerd wanneer een nieuw object wordt gemaakt in de televergaderingen. In ons voorbeeld kunt wij instellen dat telkens wanneer een nieuwe lead is gemaakt in televergaderingen, een actie voor het *verzenden van e-mail* vindt plaats via de SMTP-connector met een melding van de nieuwe potentiële klant wordt gemaakt.

1. *Televergaderingen* invoeren in het zoekvak op de logica apps designer en selecteer vervolgens de trigger **televergaderingen - als een object is gemaakt** .  
 ![](../../includes/media/connectors-create-api-salesforce/trigger-1.png)  

2. Het besturingselement **wanneer een object wordt gemaakt** , wordt weergegeven.
 ![](../../includes/media/connectors-create-api-salesforce/trigger-2.png)  

3. Selecteer het **Type Object** en selecteer vervolgens *leiden* in de lijst met objecten. In deze stap geeft u aan dat u dat uw app logica krijgt een melding maakt wanneer er een nieuwe lead is gemaakt in televergaderingen.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger3.png)  

4. De trigger is gemaakt.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger-4.png)  

## <a name="use-an-smtp-action"></a>SMTP-actie

Een actie is een bewerking die wordt uitgevoerd door de werkstroom in een app logica gedefinieerd. [Meer informatie over acties](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

Nu dat de trigger is toegevoegd, als volgt te werk om een SMTP-actie dat plaatsvindt wanneer een nieuwe lead is gemaakt in televergaderingen te voegen.

1. Selecteer **+ nieuwe stap** toe te voegen van de actie die u uitvoeren wilt wanneer een nieuwe lead is gemaakt.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger4.png)  

2. Selecteer **een actie toevoegen**. Dit opent het zoekvak waarin u naar elke actie u zoeken kunt wilt uitvoeren.  
 ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-2.png)  

3. Voer *smtp* om te zoeken naar acties met betrekking tot de SMTP.  

4. Selecteer **SMTP - e-mailbericht verzenden** als de actie moet worden ondernomen wanneer de nieuwe potentiële klant wordt gemaakt. De actie in het Configuratiescherm wordt geopend. U hebt uw SMTP-verbinding in het ontwerp blok tot stand brengen als u dit niet eerder hebt gedaan.  
 ![](../../includes/media/connectors-create-api-smtp/smtp-2.png)    

5. Invoer van de gewenste e-informatie in het blok van **SMTP - e-mailbericht verzenden** .  
 ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-4.PNG)  

6. Uw werk opslaan om uw workflow activeren.  

## <a name="technical-details"></a>Technische details

Hier vindt u de details van de triggers, acties en reacties die deze verbinding ondersteunt:

## <a name="smtp-triggers"></a>SMTP-triggers

SMTP heeft geen schakelaars. 

## <a name="smtp-actions"></a>SMTP-acties

SMTP is de volgende actie:


|Actie|Beschrijving|
|--- | ---|
|[E-mail verzenden](connectors-create-api-smtp.md#send-email)|Deze bewerking wordt een e-mailbericht verzendt naar een of meer geadresseerden.|

### <a name="action-details"></a>Actiedetails

Hier zijn de details voor de actie van deze verbindingslijn, samen met de bijbehorende antwoorden:


### <a name="send-email"></a>E-mail verzenden
Deze bewerking wordt een e-mailbericht verzendt naar een of meer geadresseerden. 


|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|Aan|Aan|Geef de e-mailadressen, gescheiden door puntkomma's alsrecipient1@domain.com;recipient2@domain.com|
|CC|CC|Geef de e-mailadressen, gescheiden door puntkomma's alsrecipient1@domain.com;recipient2@domain.com|
|Onderwerp|Onderwerp|E-mail onderwerp|
|Hoofdtekst|Hoofdtekst|Hoofdtekst van e-mail|
|Van|Van|E-mailadres van de afzender zoalssender@domain.com|
|IsHtml|HTML|Het e-mailbericht verzenden als HTML (true/false)|
|BCC|BCC|Geef de e-mailadressen, gescheiden door puntkomma's alsrecipient1@domain.com;recipient2@domain.com|
|Belang|Belang|Belang van het e-mailbericht (hoog, normaal of laag)|
|ContentData|Inhoudsgegevens bijlagen|Gegevens van inhoud (voor gegevensstromen en als base64-gecodeerde-tekenreeks is)|
|ContentType|Type inhoud van bijlagen|Type inhoud|
|ContentTransferEncoding|Bijlagen inhoud codering voor overdracht|Content-Transfer-Encoding (base64 of geen)|
|Bestandsnaam|Bestandsnaam van de bijlagen|Bestandsnaam|
|ContentId|Bijlagen Content-ID|Content-id|

Een * geeft aan dat een eigenschap vereist is


## <a name="http-responses"></a>HTTP-antwoorden

De bovenstaande triggers en acties kunnen u een of meer van de volgende HTTP-statuscodes retourneren: 

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
[Maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md)