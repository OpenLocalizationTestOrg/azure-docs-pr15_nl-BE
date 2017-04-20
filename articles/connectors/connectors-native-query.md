<properties
    pageTitle="De queryactie in apps logica toevoegen | Microsoft Azure"
    description="Overzicht van de queryactie voor het uitvoeren van acties, zoals de array met filters."
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
   ms.date="07/20/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-query-action"></a>Aan de slag met de queryactie

Met de queryactie, kunt u werken met batches en matrices werkstromen te doen:

- Een taak maken voor alle essentiële records uit een database.
- Alle PDF-bijlagen van e-mailberichten naar een Azure blob opslaan.

Zie aan de slag met de queryactie in een logica app [maken een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-query-action"></a>De queryactie gebruiken

Een actie is een bewerking die wordt uitgevoerd door de workflow die is gedefinieerd in een app logica. [Meer informatie over acties](connectors-overview.md).  

De actie van de query heeft momenteel een bewerking, genaamd de array met filters, die wordt weergegeven in de ontwerpfunctie. Hiermee kunt u een matrix opvragen en een set gefilterde resultaten retourneren.

Hier ziet u hoe u het kunt toevoegen in een app logica:

1. Klik op de knop **Nieuwe stap** .
2. Kies **een actie toevoegen**.
3. Typ in het vak actie zoeken **filteren** als u de actie **Filter matrix** .

    ![Selecteer de queryactie](./media/connectors-native-query/using-action-1.png)

4. Selecteer een matrix te filteren. (De volgende screenshot toont de matrix met resultaten van een zoekopdracht Twitter.)
5. Een voorwaarde voor de evaluatie van elk item maken. (De volgende schermafdruk filters tweets van gebruikers die beschikken over meer dan 100 PC-gebruikers).

    ![De queryactie voltooien](./media/connectors-native-query/using-action-2.png)

    De actie wordt uitgevoerd een nieuwe array met alleen resultaten die voldoen aan de eisen van het filter.
6. Klik op de linkerbovenhoek van de werkbalk op te slaan en uw app logica wordt opslaan en publiceren (activeren).

## <a name="query-action"></a>Queryactie

Hier vindt u de details voor de actie die deze connector ondersteunt. De connector heeft een mogelijke actie.

|Actie|Beschrijving|
|---|---|
|Met filters|Een voorwaarde voor elk item in een matrix wordt geëvalueerd en de resultaten geretourneerd|

## <a name="action-details"></a>Actiedetails

De actie van de query wordt geleverd met een mogelijke actie. De volgende tabellen beschrijven de vereiste en optionele invoervelden voor de actie en de bijbehorende output details die gekoppeld zijn aan de actie.

### <a name="filter-array"></a>Met filters
De volgende zijn invoervelden voor de actie, waardoor een uitgaande HTTP-aanvraag.
A * betekent dat het een verplicht veld.

|Weergavenaam|Naam van eigenschap|Beschrijving|
|---|---|---|
|Van *|Van|De matrix te filteren|
|Voorwaarde *|waar|De voorwaarde te evalueren voor elk item|
<br>

### <a name="output-details"></a>Uitvoerdetails

Dit zijn gegevens van de uitvoer voor het HTTP-antwoord.

|Naam van eigenschap|Gegevenstype|Beschrijving|
|---|---|---|
|Gefilterde matrix|matrix|Een matrix die een object voor elke gefilterde resultaten bevat|

## <a name="next-steps"></a>Volgende stappen

Probeer nu het platform en het [maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md). Door te kijken op onze [lijst van API's](apis-list.md)vindt u de beschikbare connectors in logica apps.
