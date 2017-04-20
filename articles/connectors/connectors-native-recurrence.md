<properties
    pageTitle="De trigger voor terugkeerpatroon in apps logica toevoegen | Microsoft Azure"
    description="Overzicht van de trigger terugkeerpatroon en hoe het met een app Azure logica."
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

# <a name="get-started-with-the-recurrence-trigger"></a>Aan de slag met de trigger terugkeerpatroon

Met behulp van de trigger herhaling kunt u krachtige workflows maken in de cloud.

U kunt bijvoorbeeld:

- Een workflow voor het uitvoeren van een opgeslagen SQL-procedure elke dag plannen.
- Per e-mail een overzicht van alle tweets in de afgelopen week over een bepaalde hashtag.

Zie aan de slag met de trigger terugkeerpatroon in een logica app [maken een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-a-recurrence-trigger"></a>Een terugkeerpatroon trigger gebruiken

Een trigger is een gebeurtenis die kan worden gebruikt voor het starten van de werkstroom die is gedefinieerd in een app logica. [Meer informatie over triggers](connectors-overview.md).

Hier is een reeks voor een voorbeeld van het instellen van een trigger terugkeerpatroon in een app logica:

1. De trigger **Terugkeerpatroon** als de eerste stap in een app logica toevoegen.
2. Vul in de parameters voor het interval van het terugkeerpatroon.

De logica app begint nu een reeks na elk tijdsinterval.

![HTTP-trigger](./media/connectors-native-recurrence/using-trigger.png)

## <a name="trigger-details"></a>Trigger-details

De herhaling trigger heeft de volgende eigenschappen die u kunt configureren.

Deze gebeurtenis wordt gestart een logica app na een opgegeven tijdsinterval.
A * betekent dat het een verplicht veld.

|Weergavenaam|Naam van eigenschap|Beschrijving|
|---|---|---|
|Frequentie *|frequentie|The unit of time: `Second`, `Minute`, `Hour`, `Day`, or `Year`.|
|Interval *|interval|Het interval voor het terugkeerpatroon van een bepaalde frequentie.|
|Tijdzone|timeZone|Als u een begintijd wordt geleverd zonder de UTC-afwijking, wordt deze tijdzone gebruikt.|
|Begintijd|Starttijd|De begintijd in [ISO 8601-notatie](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations).|
<br>


## <a name="next-steps"></a>Volgende stappen

Probeer nu het platform en het [maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md). Door te kijken op onze [lijst van API's](apis-list.md)vindt u de beschikbare connectors in logica apps.
