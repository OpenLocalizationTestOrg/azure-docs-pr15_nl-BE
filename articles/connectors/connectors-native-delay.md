<properties
    pageTitle="Een vertraging in de logica apps toevoegen | Microsoft Azure"
    description="Overzicht van de vertraging en de vertraging-tot acties en hoe u deze gebruikt met een app Azure logica."
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

# <a name="get-started-with-the-delay-and-delay-until-actions"></a>Aan de slag met vertraging en vertraging-tot acties

Met behulp van de vertraging en "vertraging-totdat ' acties, kunt u scenario's workflow voltooien.

U kunt bijvoorbeeld:

- Wachten tot een weekdag in een statusupdate verzenden via e-mail.
- De werkstroom uitstellen totdat er een HTTP-oproep te voltooien vóór hervatten en de resultaten ophalen.

Zie aan de slag met de actie vertraging in een logica app [maken een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-delay-actions"></a>De vertraging acties gebruiken

Een actie is een bewerking die wordt uitgevoerd door de workflow die is gedefinieerd in een app logica. [Meer informatie over acties](connectors-overview.md).

Hier is een reeks voor een voorbeeld van het gebruik van een vertraging stap in een app logica:

1. Na het toevoegen van een trigger, klik op de **Nieuwe stap** als een actie wilt toevoegen.
2. **Vertraging** om de acties van de vertraging te zoeken. In dit voorbeeld selecteren we **vertraging**.

    ![Acties voor vertraging](./media/connectors-native-delay/using-action-1.png)

3. Voer een van de eigenschappen voor het configureren van de vertraging.

    ![Vertraging config](./media/connectors-native-delay/using-action-2.png)

4. Klik op **Opslaan** als u wilt publiceren en de logica app activeren.


## <a name="action-details"></a>Actiedetails

De herhaling trigger heeft de volgende eigenschappen die kunnen worden geconfigureerd.

### <a name="delay-action"></a>Vertraging actie

Deze actie uitvoeren voor een bepaalde periode uitgesteld.
A * betekent dat het een verplicht veld.

|Weergavenaam|Naam van eigenschap|Beschrijving|
|---|---|---|
|Aantal *|aantal|Het aantal tijdseenheden uitstellen|
|Eenheid *|eenheid|De tijdseenheid: `Second`, `Minute`, `Hour`, of`Day`|
<br>

### <a name="delay-until-action"></a>Vertraging-tot actie

Deze actie uitgesteld tot een opgegeven tijdstip uitvoeren.
A * betekent dat het een verplicht veld.

|Weergavenaam|Naam van eigenschap|Beschrijving|
|---|---|---|
|Jaar *|tijdstempel|Het jaar uit te stellen totdat (GMT)|
|Maand *|tijdstempel|De maand uitstellen tot (GMT)|
|Dag *|tijdstempel|De dag uit te stellen totdat (GMT)|
<br>


## <a name="next-steps"></a>Volgende stappen

Probeer nu het platform en het [maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md). Door te kijken op onze [lijst van API's](apis-list.md)vindt u de beschikbare connectors in logica apps.
