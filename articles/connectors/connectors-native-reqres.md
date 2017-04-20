<properties
    pageTitle="Acties voor aanvraag en antwoord | Microsoft Azure"
    description="Overzicht van de aanvraag en het antwoord trigger en een actie in een app Azure logica"
    services=""
    documentationCenter=""
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-request-and-response-components"></a>Aan de slag met de onderdelen van de aanvraag en het antwoord

Met de aanvraag en het antwoord componenten in een logica app, kunt u in real-time reageren op gebeurtenissen.

U kunt bijvoorbeeld:

- Reageren op een HTTP-aanvraag met gegevens uit een database op het bedrijf via een app logica.
- Een app logica van een externe webhook-gebeurtenis wordt geactiveerd.
- Bel een app logica met een aanvraag en het antwoord in actie binnen een andere logica app.

Zie aan de slag met de acties van de aanvraag en het antwoord in een logica app [maken een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-http-request-trigger"></a>Gebruik de HTTP-aanvraag-trigger

Een trigger is een gebeurtenis die kan worden gebruikt voor het starten van de werkstroom die is gedefinieerd in een app logica. [Meer informatie over triggers](connectors-overview.md).

Hier is een reeks voor een voorbeeld van het instellen van een HTTP-aanvraag in de logica App Designer.

1. De trigger **aanvraag - als een HTTP-aanvraag wordt ontvangen** in uw app logica toevoegen. U kunt desgewenst een JSON-schema (met behulp van een hulpprogramma zoals [JSONSchema.net](http://jsonschema.net)) voor het hoofdgedeelte van de aanvraag. Hierdoor kan de ontwerper voor het genereren van tokens voor eigenschappen in de HTTP-aanvraag.
2. Een andere actie toevoegen zodat u de logica app kunt opslaan.
3. Na de logica app opslaat, krijgt u de HTTP-aanvraag-URL van de kaart.
4. Een HTTP POST (u kunt een hulpprogramma zoals [Postman](https://www.getpostman.com/)) voor de URL wordt de logica app geactiveerd.

>[AZURE.NOTE] Als u niet een actie reactie definieert een `202 ACCEPTED` is direct antwoord geretourneerd naar de aanroeper. U kunt de actie reactie aanpassen van een antwoord.

![Antwoord-trigger](./media/connectors-native-reqres/using-trigger.png)

## <a name="use-the-http-response-action"></a>De HTTP-reactie-actie

De HTTP-reactie-actie is alleen geldig wanneer in een werkstroom die wordt geactiveerd door een HTTP-aanvraag. Als u niet een actie reactie definieert een `202 ACCEPTED` is direct antwoord geretourneerd naar de aanroeper.  Bij elke stap in de werkstroom kunt u een actie reactie toevoegen. De app logica alleen zorgt ervoor dat de binnenkomende aanvraag open gedurende 1 minuut op een reactie.  Na één minuut, als er geen antwoord is verzonden vanuit de werkstroom (en een reactie actie in de definitie bestaat) een `504 GATEWAY TIMEOUT` wordt geretourneerd naar de aanroepende functie.

Dit is het toevoegen van een HTTP-reactie-actie:

1. Klik op de knop **Nieuwe stap** .
2. Kies **een actie toevoegen**.
3. Typ in het zoekvak actie **reactie** als u de actie reactie.

    ![Selecteer de actie reactie](./media/connectors-native-reqres/using-action-1.png)

4. In de parameters die vereist voor de HTTP response-bericht zijn toevoegen.

    ![De reactie actie niet voltooien](./media/connectors-native-reqres/using-action-2.png)

5. Klik op de linkerbovenhoek van de werkbalk op te slaan en uw app logica wordt opslaan en publiceren (activeren).

## <a name="request-trigger"></a>Aanvraag trigger

Hier vindt u de details voor de trigger die deze connector ondersteunt. Er is een trigger één verzoek.

|Trigger|Beschrijving|
|---|---|
|Aanvraag|Deze gebeurtenis vindt plaats wanneer een HTTP-aanvraag wordt ontvangen|

## <a name="response-action"></a>Actie reactie

Hier vindt u de details voor de actie die deze connector ondersteunt. Er is een actie één antwoord kan alleen worden gebruikt wanneer het vergezeld van een trigger aanvraag gaat.

|Actie|Beschrijving|
|---|---|
|Reactie|Deze eigenschap retourneert een reactie op een HTTP-aanvraag gecorreleerde|

### <a name="trigger-and-action-details"></a>Trigger- en details

De volgende tabellen beschrijven de invoervelden voor de trigger en de actie en de bijbehorende details uitvoer.

#### <a name="request-trigger"></a>Aanvraag trigger
Hier volgt een invoerveld voor de trigger van een binnenkomende HTTP-aanvraag.

|Weergavenaam|Naam van eigenschap|Beschrijving|
|---|---|---|
|JSON-Schema|schema|De JSON-schema van het hoofdgedeelte van de HTTP-aanvraag|
<br>

**Uitvoerdetails**

Dit zijn gegevens van de uitvoer voor de aanvraag.

|Naam van eigenschap|Gegevenstype|Beschrijving|
|---|---|---|
|Berichtkoppen|object|Aanvraagheaders|
|Hoofdtekst|object|Request-object|

#### <a name="response-action"></a>Actie reactie

Hier volgen de invoervelden voor de HTTP-reactie-actie. A * betekent dat het een verplicht veld.

|Weergavenaam|Naam van eigenschap|Beschrijving|
|---|---|---|
|Status Code *|statusCode|De HTTP-statuscode|
|Berichtkoppen|berichtkoppen|Een JSON-object van een antwoordheaders opnemen|
|Hoofdtekst|hoofdtekst|Het hoofdgedeelte van de response|

## <a name="next-steps"></a>Volgende stappen

Probeer nu het platform en het [maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md). Door te kijken op onze [lijst van API's](apis-list.md)vindt u de beschikbare connectors in logica apps.
