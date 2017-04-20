
<properties
    pageTitle="Toevoegen van de HTTP- + Swagger actie in logica apps | Microsoft Azure"
    description="Overzicht van de HTTP- + Swagger actie en bewerkingen"
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

# <a name="get-started-with-the-http--swagger-action"></a>Aan de slag met de HTTP- + Swagger actie

Met de HTTP- + Swagger actie, kunt u een eersteklas verbindingslijn naar een eindpunt van de REST door middel van een [document Swagger](https://swagger.io). U kunt ook een app logica om te roepen een eindpunt van de REST met een eersteklas logica App Designer ervaring uitbreiden.

Aan de slag met de HTTP- + Swagger actie in een app logica Zie [maken een nieuwe logica app](../app-service-logic/app-service-logic-create-a-logic-app.md).

---

## <a name="use-http--swagger-as-a-trigger-or-an-action"></a>Gebruik HTTP- + Swagger als een signaal of een actie

De HTTP- + Swagger trigger- en werken op dezelfde manier als de [actie in HTTP](connectors-native-http.md) , maar bieden een betere ervaring met ontwerp door de vorm van de API- en uitvoer in de ontwerpfunctie van de [Swagger van metagegevens](https://swagger.io)weer te geven. Daarnaast kunt u HTTP + Swagger als een trigger. Als u wilt dat een trigger polling implementeren, moet deze voldoen aan de polling-patroon dat wordt beschreven in [een aangepaste API gebruiken met logica apps maken](../app-service-logic/app-service-logic-create-api-app.md#polling-triggers).

[Meer informatie over logic app triggers en acties.](connectors-overview.md)

Hier volgt een voorbeeld van het gebruik van het HTTP + Swagger bewerking als een actie in een werkstroom in een app logica.

1. Klik op de knop **Nieuwe stap** .
2. Selecteer **een actie toevoegen**.
3. Typ **swagger** op de lijst het HTTP-Swagger actie in het zoekvak van de actie.

    ![Selecteer HTTP + Swagger actie](./media/connectors-native-http-swagger/using-action-1.png)

4. Typ de URL voor een document Swagger:
    - Om uit de logica App Designer, moet de URL een HTTPS-eindpunt en CORS hebt ingeschakeld.
    - Als het document Swagger niet aan deze eis te voldoen, kunt u [Azure opslag met CORS ingeschakeld](#hosting-swagger-from-storage) voor het opslaan van het document.
5. Klik op **volgende** om te lezen en uit het document Swagger renderen.
6. Toevoegen in de parameters die vereist voor de HTTP-oproep zijn.

    ![HTTP-actie](./media/connectors-native-http-swagger/using-action-2.png)

1. Klik op **Opslaan** in de linkerbovenhoek van de werkbalk en de logica app zal beide opslaan en publiceren (activeren).

### <a name="host-swagger-from-azure-storage"></a>Swagger uit de opslag voor Azure host

U kunt verwijzen naar een document Swagger die niet als host fungeert, of die niet voldoet aan de beveiliging en de vereisten voor de ontwerper van de cross-oorsprong. U kunt dit probleem oplossen door het Swagger document opgeslagen in Azure opslag en inschakelen van CORS om te verwijzen naar het document.  

Hier volgen de stappen voor het maken, configureren en Swagger documenten opslaan in Azure opslag:

1. [Een account Azure opslag met Azure Blob-opslag maken](../storage/storage-create-storage-account.md). (U doet dit door machtigingen instellen om de **toegang van het publiek**.)
2. CORS inschakelen op de blob. [Deze PowerShell-script](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1) kunt u configureren dat automatisch de instelling.
3. Upload het bestand Swagger in de blob. U kunt dit doen vanuit de [Azure portal](https://portal.azure.com) of een hulpprogramma zoals [Opslagverkenner Azure](http://storageexplorer.com/).
1. Verwijzen naar een HTTPS-koppeling naar het document in Azure Blob-opslag. (De indeling volgt op de link `https://*storageAccountName*.blob.core.windows.net/*container*/*filename*`.)



## <a name="technical-details"></a>Technische details

Hieronder vindt u de details voor de triggers en acties die deze HTTP + Swagger connector ondersteunt.

## <a name="http--swagger-triggers"></a>HTTP- + Swagger triggers

Een trigger is een gebeurtenis die kan worden gebruikt voor het starten van de werkstroom die gedefinieerd in een app logica. [Meer informatie over triggers.](connectors-overview.md) De HTTP- + Swagger connector heeft één trigger.

|Trigger|Beschrijving|
|---|---|
|HTTP- + Swagger|Maken van een HTTP-oproep en de inhoud van het antwoord wordt geretourneerd|

## <a name="http--swagger-actions"></a>HTTP- + Swagger acties

Een actie is een bewerking die wordt uitgevoerd door de workflow die gedefinieerd in een app logica. [Meer informatie over acties.](connectors-overview.md) De HTTP- + Swagger connector is een mogelijke actie.

|Actie|Beschrijving|
|---|---|
|HTTP- + Swagger|Maken van een HTTP-oproep en de inhoud van het antwoord wordt geretourneerd|

### <a name="action-details"></a>Actiedetails

De HTTP- + Swagger connector wordt geleverd met een mogelijke actie. Hieronder vindt u informatie over elk van de acties, de vereiste en optionele invoervelden en de bijbehorende uitvoerdetails die gekoppeld aan hun gebruik zijn.

#### <a name="http--swagger"></a>HTTP- + Swagger

Uitgaande HTTP-aanvragen met hulp van Swagger metagegevens maken.
Een sterretje (*) betekent een verplicht veld.

|Weergavenaam|Naam van eigenschap|Beschrijving|
|---|---|---|
|Methode *|methode|HTTP-woord te gebruiken.|
|URI *|URI|De URI voor de HTTP-aanvraag.|
|Berichtkoppen|berichtkoppen|Een JSON-object HTTP-headers op te nemen.|
|Hoofdtekst|hoofdtekst|Het hoofdgedeelte van de HTTP-aanvraag.|
|Verificatie|verificatie|Verificatie voor de aanvraag. [Zie voor meer informatie de HTTP](./connectors-native-http.md#authentication).|

**Uitvoerdetails**

HTTP-antwoord

|Naam van eigenschap|Gegevenstype|Beschrijving|
|---|---|---|
|Berichtkoppen|object|Antwoordheaders|
|Hoofdtekst|object|Response-object|
|Statuscode|int|HTTP-statuscode|

### <a name="http-responses"></a>HTTP-antwoorden

Als u verschillende acties, krijgt u mogelijk bepaalde antwoorden. Hieronder staat een tabel met een en beschrijvingen van de bijbehorende reacties overzicht.

|Naam|Beschrijving|
|---|---|
|200|OK|
|202|Geaccepteerd|
|400|Ongeldig verzoek|
|401|Niet-geautoriseerde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Er is een onbekende fout opgetreden.|

---

## <a name="next-steps"></a>Volgende stappen

Probeer het platform en het [maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md) nu. Door te kijken op onze [lijst van API's](apis-list.md)vindt u de beschikbare connectors in logica apps.
