<properties
    pageTitle="De HTTP-actie in apps logica toevoegen | Microsoft Azure"
    description="Overzicht van de HTTP-actie met eigenschappen"
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
   ms.date="07/15/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-http-action"></a>Aan de slag met de HTTP-actie

U kunt met de actie HTTP-workflows voor uw organisatie uitbreiden en naar elk eindpunt communiceren via HTTP.

U kunt:

- Logica app werkstromen maken die (trigger) activeren wanneer een website die u beheert het begeeft.
- Een eindpunt delen via HTTP naar uw werkstromen uitbreiden naar andere diensten.

Zie aan de slag met de HTTP-actie in een logica app [maken een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-http-trigger"></a>De HTTP-trigger te gebruiken

Een trigger is een gebeurtenis die kan worden gebruikt voor het starten van de werkstroom die is gedefinieerd in een app logica. [Meer informatie over triggers](connectors-overview.md).

Hier is een reeks voor een voorbeeld van het instellen van de HTTP-trigger in de logica App Designer.

1. De HTTP-trigger in uw app logica toevoegen.
2. Vul de parameters voor de HTTP-eindpunt dat u wilt controleren.
3. Wijzigen van het interval van het terugkeerpatroon op hoe vaak het moet worden gecontroleerd.
4. De logica-app wordt nu gestart met enige inhoud die wordt geretourneerd bij elke controle.

![HTTP-trigger](./media/connectors-native-http/using-trigger.png)

### <a name="how-the-http-trigger-works"></a>De werking van de HTTP-trigger

De HTTP-trigger belt naar een HTTP-eindpunt op een terugkerend interval. Standaard code alle HTTP-antwoord minder dan 300 resultaten in een logica app uitvoeren. U kunt een voorwaarde toevoegen in de codeweergave die na de aanroep HTTP om te bepalen als de app logica moet worden geactiveerd, worden geëvalueerd. Hier volgt een voorbeeld van een HTTP-trigger die wordt geactiveerd wanneer de geretourneerde statuscode is groter dan of gelijk is aan `400`.

```javascript
"Http":
{
    "conditions": [
        {
            "expression": "@greaterOrEquals(triggerOutputs()['statusCode'], 400)"
        }
    ],
    "inputs": {
        "method": "GET",
        "uri": "https://blogs.msdn.microsoft.com/logicapps/",
        "headers": {
            "accept-language": "en"
        }
    },
    "recurrence": {
        "frequency": "Second",
        "interval": 15
    },
    "type": "Http"
}
```

Volledige gegevens over de trigger HTTP parameters zijn beschikbaar op [MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger).

## <a name="use-the-http-action"></a>De HTTP-actie

Een actie is een bewerking die wordt uitgevoerd door de workflow die is gedefinieerd in een app logica. [Meer informatie over acties](connectors-overview.md).

1. Klik op de knop **Nieuwe stap** .
2. Kies **een actie toevoegen**.
3. Typ in het zoekvak actie **http** als u de HTTP-actie.

    ![Selecteer de HTTP-actie](./media/connectors-native-http/using-action-1.png)

4. Toevoegen in de parameters die vereist voor de HTTP-oproep zijn.

    ![De HTTP-bewerking heeft voltooid](./media/connectors-native-http/using-action-2.png)

5. Klik op de linkerbovenhoek van de werkbalk op te slaan. Uw app logica wordt opslaan en publiceren (activeren).

## <a name="http-trigger"></a>HTTP-trigger

Hier vindt u de details voor de trigger die deze connector ondersteunt. De HTTP-connector heeft één trigger.

|Trigger|Beschrijving|
|---|---|
|HTTP|Een HTTP-oproep maakt en geeft als resultaat de inhoud van het antwoord.|

## <a name="http-action"></a>HTTP-actie

Hier vindt u de details voor de actie die deze connector ondersteunt. De HTTP-connector heeft een mogelijke actie.

|Actie|Beschrijving|
|---|---|
|HTTP|Een HTTP-oproep maakt en geeft als resultaat de inhoud van het antwoord.|

## <a name="http-details"></a>HTTP-details

De volgende tabellen beschrijven de vereiste en optionele invoervelden voor de actie en de bijbehorende output details die gekoppeld zijn aan de actie.


#### <a name="http-request"></a>HTTP-aanvraag
De volgende zijn invoervelden voor de actie, waardoor een uitgaande HTTP-aanvraag.
A * betekent dat het een verplicht veld.

|Weergavenaam|Naam van eigenschap|Beschrijving|
|---|---|---|
|Methode *|methode|Het HTTP-woord te gebruiken|
|URI *|URI|De URI voor de HTTP-aanvraag|
|Berichtkoppen|berichtkoppen|Een JSON-object van HTTP-headers op te nemen|
|Hoofdtekst|hoofdtekst|Het hoofdgedeelte van de HTTP-aanvraag|
|Verificatie|verificatie|Gegevens in de sectie [verificatie](#authentication)|
<br>

#### <a name="output-details"></a>Uitvoerdetails

Dit zijn gegevens van de uitvoer voor het HTTP-antwoord.

|Naam van eigenschap|Gegevenstype|Beschrijving|
|---|---|---|
|Berichtkoppen|object|Antwoordheaders|
|Hoofdtekst|object|Response-object|
|Statuscode|int|HTTP-statuscode|

## <a name="authentication"></a>Verificatie

De functie Apps logica van Azure App-Service kunt u verschillende soorten verificatie ten opzichte van HTTP-eindpunten gebruiken. U kunt deze verificatie met **HTTP**, **[http- + Swagger](./connectors-native-http-swagger.md)**en **[Http-Webhook](./connectors-native-webhook.md)** -connectors. De volgende typen verificatie worden geconfigureerd:

* [Basisverificatie](#basic-authentication)
* [Verificatie van clientcertificaten](#client-certificate-authentication)
* [Azure Active Directory (AD Azure) OAuth-verificatie](#azure-active-directory-oauth-authentication)

#### <a name="basic-authentication"></a>Basisverificatie

Het volgende object in de verificatie is vereist voor basisverificatie.
A * betekent dat het een verplicht veld.

|Naam van eigenschap|Gegevenstype|Beschrijving|
|---|---|---|
|Type *|type|Type verificatie (moet `Basic` voor basisverificatie)|
|Gebruikersnaam *|gebruikersnaam|De naam van de gebruiker te verifiëren|
|Wachtwoord *|wachtwoord|Wachtwoord voor verificatie|

>[AZURE.TIP] Als u met een wachtwoord dat niet kan worden opgehaald van de definitie, gebruik een `securestring` parameter en de `@parameters()` [workflow definitie functie](http://aka.ms/logicappdocs).

Zo maakt u een object als volgt in het veld verificatie:

```javascript
{
    "type": "Basic",
    "username": "user",
    "password": "test"
}
```

#### <a name="client-certificate-authentication"></a>Verificatie van clientcertificaten

Het volgende object in de verificatie is vereist voor verificatie van clientcertificaten. A * betekent dat het een verplicht veld.

|Naam van eigenschap|Gegevenstype|Beschrijving|
|---|---|---|
|Type *|type|Het type verificatie (moet `ClientCertificate` voor clientcertificaten voor SSL)|
|PFX *|PFX|De Base64-gecodeerde inhoud van het bestand met persoonlijke informatie-uitwisseling (PFX)|
|Wachtwoord *|wachtwoord|Het wachtwoord voor toegang tot het PFX-bestand|

>[AZURE.TIP] U kunt een `securestring` parameter en de `@parameters()` [workflow definitie functie](http://aka.ms/logicappdocs) gebruik van een parameter die niet leesbaar in de definitie van na het opslaan van de app logica.

Bijvoorbeeld:

```javascript
{
    "type": "ClientCertificate",
    "pfx": "aGVsbG8g...d29ybGQ=",
    "password": "@parameters('myPassword')"
}
```

#### <a name="azure-ad-oauth-authentication"></a>Azure AD OAuth-verificatie

Het volgende object in de verificatie is vereist voor verificatie van AD-OAuth Azure. A * betekent dat het een verplicht veld.

|Naam van eigenschap|Gegevenstype|Beschrijving|
|---|---|---|
|Type *|type|Het type verificatie (moet `ActiveDirectoryOAuth` voor AD-OAuth Azure)|
|Huurder *|huurder|De id van de huurder voor de huurder Azure AD|
|Doelgroep *|doelgroep|Ingesteld op`https://management.core.windows.net/`|
|Client -ID *|clientId|De client-id voor de toepassing van Azure AD|
|Geheim *|geheim|Het geheim van de client waarmee het token wordt aangevraagd|

>[AZURE.TIP] U kunt een `securestring` parameter en de `@parameters()` [workflow definitie functie](http://aka.ms/logicappdocs) gebruik van een parameter die niet leesbaar in de definitie van opslaan.

Bijvoorbeeld:

```javascript
{
    "type": "ActiveDirectoryOAuth",
    "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "audience": "https://management.core.windows.net/",
    "clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
    "secret": "hcqgkYc9ebgNLA5c+GDg7xl9ZJMD88TmTJiJBgZ8dFo="
}
```

## <a name="next-steps"></a>Volgende stappen

Probeer nu het platform en het [maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md). Door te kijken op onze [lijst van API's](apis-list.md)vindt u de beschikbare connectors in logica Apps.
