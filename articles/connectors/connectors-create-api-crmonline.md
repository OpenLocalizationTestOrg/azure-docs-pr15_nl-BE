<properties
    pageTitle="De connector Dynamics CRM Online toevoegen aan uw logica Apps | Microsoft Azure"
    description="Logica apps maken met Azure App-service. Dynamics CRM Online verbindingsprovider biedt een API om te werken met entiteiten in Dynamics CRM Online."
    services="logic-apps"    
    documentationCenter=""     
    authors="MandiOhlinger"    
    manager="erikre"    
    editor="" 
    tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/15/2016"
ms.author="mandia"/>

# <a name="get-started-with-the-dynamics-crm-online-connector"></a>Aan de slag met de connector Dynamics CRM Online
Verbinding maken met Dynamics CRM Online naar een nieuwe record maken, bijwerken van een artikel en nog veel meer. U kunt met CRM Online:

- Bouw uw bedrijf flow op basis van de gegevens die u van CRM Online ontvangt. 
- Gebruik acties die een record verwijdert, vinden de entiteiten en meer. Deze acties een reactie krijgt en vervolgens de uitvoer beschikbaar maken voor andere acties. Bijvoorbeeld wanneer een artikel wordt bijgewerkt in CRM, kunt je een e-mail sturen met behulp van Office 365.

In dit onderwerp ziet u de Dynamics CRM Online-connector gebruiken in een app logica, en van de triggers en acties.

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica Apps algemene beschikbaarheid (GA).

Zie voor meer informatie over Logic Apps, [Wat zijn apps logica](../app-service-logic/app-service-logic-what-are-logic-apps.md) en [maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-dynamics-crm-online"></a>Verbinding maken met Dynamics CRM Online

Voordat uw logica app toegang een service tot, maakt u een *verbinding* met de service. Een verbinding biedt connectiviteit tussen een app logica en een andere service. Bijvoorbeeld als u wilt verbinding maken met Dynamics, moet u eerst een Dynamics CRM Online *verbinding*. Een verbinding wilt maken, voert u de referenties gebruikt voor toegang tot de service die u verbinding wilt maken. Voer dus de referenties aan uw Dynamics CRM Online-account om de verbinding te maken met Dynamics.


### <a name="create-the-connection"></a>Maak de verbinding

>[AZURE.INCLUDE [Steps to create a connection to Dynamics CRM Online Connection Provider](../../includes/connectors-create-api-crmonline.md)]

## <a name="use-a-trigger"></a>Een trigger gebruiken

Een trigger is een gebeurtenis die kan worden gebruikt voor het starten van de werkstroom in een app logica gedefinieerd. Triggers vragen' ' de service op een interval en de gewenste frequentie. [Meer informatie over triggers](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Typ in de app logica "dynamics" om een lijst van triggers:  

    ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)

2. Selecteer **Dynamics CRM Online - wanneer een record wordt gemaakt**. Als al een verbinding bestaat, selecteert u een organisatie en een entiteit uit de vervolgkeuzelijst.

    ![](./media/connectors-create-api-crmonline/select-organization.png)

    Als u wordt gevraagd aan te melden, geeft u het teken in details om de verbinding te maken. [De verbinding maken](connectors-create-api-crmonline.md#create-the-connection) in dit onderwerp vindt u de stappen. 

    > [AZURE.NOTE] In dit voorbeeld wordt de logica app uitgevoerd wanneer een record wordt gemaakt. De resultaten van deze trigger, toe te voegen een andere actie die u een e-mailbericht worden verzonden. Toevoegen, bijvoorbeeld de actie Office 365 *stuurt u een e-mail* die u e-mails wanneer een nieuwe record is toegevoegd. 

3. Klik op de knop **bewerken** en stel de **frequentie** en **Interval** . Bijvoorbeeld, als u wilt dat de trigger te vragen voor elke 15 minuten, vervolgens de **frequentie** instelt op de **minuut**en het **Interval** instellen op **15**. 

    ![](./media/connectors-create-api-crmonline/edit-properties.png)

4. **Sla** uw wijzigingen (linksboven aan de werkbalk). Uw app logica wordt opgeslagen en automatisch kan worden ingeschakeld.


## <a name="use-an-action"></a>Actie

Een actie is een bewerking die wordt uitgevoerd door de werkstroom in een app logica gedefinieerd. [Meer informatie over acties](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Selecteer het plus-teken. Ziet u verschillende mogelijkheden: **een actie toevoegen**, **toevoegen van een voorwaarde**of een van de **meer** opties.

    ![](./media/connectors-create-api-crmonline/add-action.png)

2. Kies **een actie toevoegen**.

3. Typ in het tekstvak "dynamics" om een lijst van alle beschikbare acties.

    ![](./media/connectors-create-api-crmonline/dynamics-actions.png)

4. In ons voorbeeld kiezen **Dynamics CRM Online - een record bijwerken**. Als er al een verbinding bestaat, kiest u de **Naam van de organisatie**, de **Naam van de entiteit**en andere eigenschappen:  

    ![](./media/connectors-create-api-crmonline/sample-action.png)

    Als u wordt gevraagd om de verbindingsgegevens, voert u de details van de verbinding te maken. Deze eigenschappen van [de verbinding maken](connectors-create-api-crmonline.md#create-the-connection) in dit onderwerp worden beschreven. 

    > [AZURE.NOTE] In dit voorbeeld bijwerken gaan we een bestaande record in CRM Online. Uitvoer van een andere trigger kunt u de record bij te werken. Toevoegen, bijvoorbeeld de trigger SharePoint *wanneer een bestaand item is gewijzigd* . Voeg vervolgens de actie met CRM Online *bijwerken van een record* die de SharePoint-velden worden gebruikt voor het bijwerken van de bestaande record in CRM Online. 

5. **Sla** uw wijzigingen (linksboven aan de werkbalk). Uw app logica wordt opgeslagen en automatisch kan worden ingeschakeld.


## <a name="technical-details"></a>Technische Details

## <a name="triggers"></a>Triggers

|Trigger | Beschrijving|
|--- | ---|
|[Wanneer een record wordt gemaakt](connectors-create-api-crmonline.md#when-a-record-is-created)|Een stroom wordt gegenereerd wanneer een object wordt gemaakt in CRM.|
|[Wanneer een record wordt bijgewerkt](connectors-create-api-crmonline.md#when-a-record-is-updated)|Een stroom wordt gegenereerd wanneer een object wordt gewijzigd in CRM.|
|[Wanneer een record wordt verwijderd](connectors-create-api-crmonline.md#when-a-record-is-deleted)|Een stroom wordt gegenereerd wanneer een object wordt verwijderd in CRM.|


## <a name="actions"></a>Acties

|Actie|Beschrijving|
|--- | ---|
|[De lijst](connectors-create-api-crmonline.md#list-records)|Deze bewerking wordt de records voor een entiteit.|
|[Een nieuwe record maken](connectors-create-api-crmonline.md#create-a-new-record)|Hiermee maakt u een nieuwe record van een entiteit.|
|[Record ophalen](connectors-create-api-crmonline.md#get-record)|Deze bewerking wordt de opgegeven record voor een entiteit.|
|[Een record verwijderen](connectors-create-api-crmonline.md#delete-a-record)|Deze bewerking verwijderd een record uit de collectie van een entiteit.|
|[Een record bijwerken](connectors-create-api-crmonline.md#update-a-record)|Deze bewerking werkt een bestaande record voor een entiteit.|

### <a name="trigger-and-action-details"></a>Trigger- en details

In dit gedeelte ziet u de specifieke details over elke trigger en maatregelen, met inbegrip van eventuele verplichte of optionele eigenschappen voor de invoer en de bijbehorende uitvoer die is gekoppeld aan de verbindingslijn.

#### <a name="when-a-record-is-created"></a>Wanneer een record wordt gemaakt
Een stroom wordt gegenereerd wanneer een object wordt gemaakt in CRM. 

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|DataSet *|Naam van organisatie|Naam van de CRM-organisatie zoals Contoso|
|tabel *|Naam van de entiteit|Naam van de entiteit|
|$skip|Aantal overgeslagen|Aantal items over te slaan (standaard = 0)|
|$top|Get maximum aantal|Maximum aantal items ophalen (standaard = 256)|
|$filter|Filteren van Query|Een filterquery ODATA de geretourneerde items beperken|
|$orderby|Order By|Een query ODATA sorteren op voor het opgeven van de volgorde van items|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Uitvoerdetails
ItemsList

| Naam van eigenschap | Gegevenstype |
|---|---|
|waarde|matrix|


#### <a name="when-a-record-is-updated"></a>Wanneer een record wordt bijgewerkt
Een stroom wordt gegenereerd wanneer een object wordt gewijzigd in CRM. 

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|DataSet *|Naam van organisatie|Naam van de CRM-organisatie zoals Contoso|
|tabel *|Naam van de entiteit|Naam van de entiteit|
|$skip|Aantal overgeslagen|Aantal items over te slaan (standaard = 0)|
|$top|Get maximum aantal|Maximum aantal items ophalen (standaard = 256)|
|$filter|Filteren van Query|Een filterquery ODATA de geretourneerde items beperken|
|$orderby|Order By|Een query ODATA sorteren op voor het opgeven van de volgorde van items|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Uitvoerdetails
ItemsList

| Naam van eigenschap | Gegevenstype |
|---|---|
|waarde|matrix|


#### <a name="when-a-record-is-deleted"></a>Wanneer een record wordt verwijderd
Een stroom wordt gegenereerd wanneer een object wordt verwijderd in CRM. 

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|DataSet *|Naam van organisatie|Naam van de CRM-organisatie zoals Contoso|
|tabel *|Naam van de entiteit|Naam van de entiteit|
|$skip|Aantal overgeslagen|Aantal items over te slaan (standaard = 0)|
|$top|Get maximum aantal|Maximum aantal items ophalen (standaard = 256)|
|$filter|Filteren van Query|Een filterquery ODATA de geretourneerde items beperken|
|$orderby|Order By|Een query ODATA sorteren op voor het opgeven van de volgorde van items|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Uitvoerdetails
ItemsList

| Naam van eigenschap | Gegevenstype |
|---|---|
|waarde|matrix|


#### <a name="list-records"></a>De lijst
Deze bewerking wordt de records voor een entiteit. 

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|DataSet *|Naam van organisatie|Naam van de CRM-organisatie zoals Contoso|
|tabel *|Naam van de entiteit|Naam van de entiteit|
|$skip|Aantal overgeslagen|Aantal items over te slaan (standaard = 0)|
|$top|Get maximum aantal|Maximum aantal items ophalen (standaard = 256)|
|$filter|Filteren van Query|Een filterquery ODATA de geretourneerde items beperken|
|$orderby|Order By|Een query ODATA sorteren op voor het opgeven van de volgorde van items|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Uitvoerdetails
ItemsList

| Naam van eigenschap | Gegevenstype |
|---|---|
|waarde|matrix|


#### <a name="create-a-new-record"></a>Een nieuwe record maken
Hiermee maakt u een nieuwe record van een entiteit. 

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|DataSet *|Naam van organisatie|Naam van de CRM-organisatie zoals Contoso|
|tabel *|Naam van de entiteit|Naam van de entiteit|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Uitvoerdetails
Geen.


#### <a name="get-record"></a>Record ophalen
Deze bewerking wordt de opgegeven record voor een entiteit. 

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|DataSet *|Naam van organisatie|Naam van de CRM-organisatie zoals Contoso|
|tabel *|Naam van de entiteit|Naam van de entiteit|
|ID *|Item-id|Geef de id van de record|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Uitvoerdetails
Geen.


#### <a name="delete-a-record"></a>Een record verwijderen
Deze bewerking verwijderd een record uit de collectie van een entiteit. 

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|DataSet *|Naam van organisatie|Naam van de CRM-organisatie zoals Contoso|
|tabel *|Naam van de entiteit|Naam van de entiteit|
|ID *|Item-id|Geef de id van de record|

Een sterretje (*) betekent dat de eigenschap is vereist.


#### <a name="update-a-record"></a>Een record bijwerken
Deze bewerking werkt een bestaande record voor een entiteit. 

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|DataSet *|Naam van organisatie|Naam van de CRM-organisatie zoals Contoso|
|tabel *|Naam van de entiteit|Naam van de entiteit|
|ID *|Record-id|Geef de id van de record|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Uitvoerdetails
Geen.


## <a name="http-responses"></a>HTTP-antwoorden

De triggers en acties kunnen u een of meer van de volgende HTTP-statuscodes retourneren: 

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

[Maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md). Bekijk de beschikbare connectors in Apps logica op onze [lijst van API's](apis-list.md).

