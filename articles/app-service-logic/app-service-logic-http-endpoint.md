<properties
   pageTitle="Apps als callable eindpunten logica"
   description="Het maken en configureren van eindpunten activeren en deze gebruiken in een logica in Azure App Service app"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>


# <a name="logic-apps-as-callable-endpoints"></a>Apps als callable eindpunten logica

Logica Apps zichzelf kan worden blootgesteld een synchrone HTTP-eindpunt als een trigger.  Ook kunt u het patroon van de eindpunten callable logica Apps aanroepen als een geneste workflow via de workflowactie '' in een App logica.

Er zijn 3 soorten triggers die aanvragen kunnen ontvangen:

* Aanvraag
* ApiConnectionWebhook
* HttpWebhook

**Aanvraag** gebruikt als in het voorbeeld voor de rest van het artikel, maar alle van de beginselen identiek aan de overige 2 typen triggers toepassen.

## <a name="adding-a-trigger-to-your-definition"></a>Een trigger toe te voegen aan de definitie
De eerste stap is een trigger toevoegen aan de definitie van de logica-app die binnenkomende aanvragen kan ontvangen.  U kunt zoeken in de ontwerpfunctie voor "HTTP-aanvragen" de trigger-kaart toevoegen. Kunt u het hoofdgedeelte van een aanvraag JSON-Schema en de ontwerper van de tokens parseren en doorgeven van gegevens van de handmatige trigger via de werkstroom wordt gegenereerd.  Ik raden het gebruik van een hulpprogramma zoals [jsonschema.net](http://jsonschema.net) een JSON-schema genereren uit een monster body-nettolading.

![Trigger-kaart aanvragen][2]

Nadat u de definitie van de logica App opslaat, wordt een callback-URL gegenereerd lijkt op deze:
 
``` text
https://prod-03.eastus.logic.azure.com:443/workflows/080cb66c52ea4e9cabe0abf4e197deff/triggers/myendpointtrigger?*signature*...
```

Deze URL bevat een SAS-sleutel in het query-parameters voor verificatie gebruikt.

Ook kun je dit eindpunt in Azure portal:

![][1]

Of door te bellen:

``` text
POST https://management.azure.com/{resourceID of your logic app}/triggers/myendpointtrigger/listCallbackURL?api-version=2015-08-01-preview
```

### <a name="security-for-the-trigger-url"></a>Beveiliging voor de trigger-URL

Logica App callback-URL's worden gegenereerd veilig met een gedeelde Access-handtekening.  De handtekening wordt doorgegeven als een queryparameter en moet worden gevalideerd voordat de app logica wordt geactiveerd.  Deze wordt gegenereerd door een unieke combinatie van een geheime sleutel per app logica, de naam van de trigger en de bewerking wordt uitgevoerd.  Tenzij iemand toegang tot de app logica geheime sleutel heeft, zou zij niet kunnen om een geldige handtekening te genereren.

## <a name="calling-the-logic-app-triggers-endpoint"></a>Het aanroepen van de logica app trigger eindpunt

Zodra u het eindpunt voor de trigger hebt gemaakt, kunt u het activeren via een `POST` tot de volledige URL. U kunt extra kopteksten en alle inhoud opnemen in de hoofdtekst.

Als het content-type is `application/json` en vervolgens is het mogelijk om te verwijzen naar eigenschappen van in de aanvraag. Anders wordt deze behandeld als een binaire eenheid die kan worden doorgegeven aan andere API's, maar zonder de inhoud kunnen omzetten in de werkstroom kan niet worden verwezen.  Als u bijvoorbeeld `application/xml` inhoud kunt u `@xpath()` doen een xpath-extractie of `@json()` te converteren van XML naar JSON.  Meer informatie over het werken met inhoud typen [kan worden gevonden hier](app-service-logic-content-type.md)

Bovendien kunt u een schema JSON in de definitie. Dit zorgt ervoor dat de ontwerper voor het genereren van tokens die u vervolgens in stappen doorgeven kunt.  Bijvoorbeeld de volgende brengt een `title` en `name` token is beschikbaar in de ontwerpfunctie:

```
{
    "properties":{
        "title": {
            "type": "string"
        },
        "name": {
            "type": "string"
        }
    },
    "required": [
        "title",
        "name"
    ],
    "type": "object"
}
```

## <a name="referencing-the-content-of-the-incoming-request"></a>Verwijzen naar de inhoud van de inkomende aanvraag

De `@triggerOutputs()` functie de inhoud van de binnenkomende aanvraag wordt uitgevoerd. Bijvoorbeeld, het zou er als volgt uitzien:

```
{
    "headers" : {
        "content-type" : "application/json"
    },
    "body" : {
        "myprop" : "a value"
    }
}
```

U kunt de `@triggerBody()` snelkoppeling voor toegang tot de `body` eigenschap speciaal. 

## <a name="responding-to-the-request"></a>Reageren op de aanvraag

Voor bepaalde aanvragen die een logica app start, kunt u reageren met een bepaalde inhoud voor de beller. Er is een nieuwe actie kan worden gebruikt voor het maken van de code, hoofdtekst en koppen van uw reactie **antwoord** genoemd. Opmerking dat als er geen **reactie** vorm aanwezig is, het eindpunt van de app logica wordt *onmiddellijk* reageren met **202 geaccepteerd**.

![HTTP-reactie-actie][3]

``` json
"Response": {
            "conditions": [],
            "inputs": {
                "body": {
                    "name": "@{triggerBody()['name']}",
                    "title": "@{triggerBody()['title']}"
                },
                "headers": {
                    "content-type": "application/json"
                },
                "statusCode": 200
            },
            "type": "Response"
        }
```

Antwoorden hebt u het volgende:

| Eigenschap | Beschrijving |
| -------- | ----------- |
| statusCode | De HTTP-statuscode te reageren op de inkomende aanvraag. Elke geldige statuscode die met een 2xx, 4xx of 5xx begint kan zijn. 3xx statuscodes zijn niet toegestaan. | 
| hoofdtekst | Een body-object dat een tekenreeks, een JSON-object of zelfs binaire inhoud waarnaar wordt verwezen vanuit een eerdere stap. | 
| berichtkoppen | U kunt een willekeurig aantal koppen moeten worden opgenomen in het antwoord definiëren | 

Alle stappen in de toepassing van de logica die nodig voor de reactie zijn moet worden voltooid binnen *60 seconden* voor de oorspronkelijke aanvraag ontvangen antwoord **tenzij de werkstroom wordt aangeroepen als een geneste logica App**. Als er geen reactie-actie is bereikt binnen 60 seconden en vervolgens de binnenkomende aanvraag wordt de time-out en **408 Client time-out voor** HTTP-antwoord ontvangen.  Voor toepassingen met geneste logica, de bovenliggende logica App wachten op een antwoord blijft pas voltooid, ongeacht de hoeveelheid tijd nodig die is.

## <a name="advanced-endpoint-configuration"></a>Geavanceerd endpoint-configuratie

Logica apps hebben een ingebouwde ondersteuning voor het eindpunt van de rechtstreekse toegang en gebruik altijd de `POST` methode een run van de logica app starten. De **Http-Listener** API app eerder ook ondersteund voor het wijzigen van de URL-segmenten en de HTTP-methode. Kunt u ook instellen van extra beveiliging of een aangepaste domein toe te voegen aan de API app host (de Web app die als host van de API-app). 

Deze functionaliteit is beschikbaar via de **API-beheer**:
* [De methode van het verzoek wijzigen](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod)
* [Wijzigen van de URL-segmenten van de aanvraag](https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL)
* De API management domeinen op het tabblad **configureren** in de klassieke Azure portal instellen
* Beleid instellen om te controleren voor basisverificatie (**koppeling nodig**)

## <a name="summary-of-migration-from-2014-12-01-preview"></a>Overzicht van de migratie van 2014-12-01-voorbeeld

|  2014-12-01-voorbeeld | 06-01-2016 |
|---------------------|--------------------|
| Klik op **Http-Listener** API app | Klik op **handmatige trigger** (geen API app vereist) |
| HTTP-Listener "*automatisch antwoord verzendt*" instellen | Hetzij een actie **antwoord** opnemen of niet in de workflowdefinitie van de |
| Basisverificatie of OAuth-verificatie configureren | via de API-beheer |
| HTTP-methode configureren | via de API-beheer |
| Relatief pad configureren | via de API-beheer |
| Verwijst naar de binnenkomende instantie via`@triggerOutputs().body.Content` | Verwijzing via`@triggerOutputs().body` |
| **Verzenden van HTTP-reactie** -actie op de HTTP-Listener | Klik op het **reageren op een HTTP-aanvraag** (geen API app vereist)


[1]: ./media/app-service-logic-http-endpoint/manualtriggerurl.png
[2]: ./media/app-service-logic-http-endpoint/manualtrigger.png
[3]: ./media/app-service-logic-http-endpoint/response.png
