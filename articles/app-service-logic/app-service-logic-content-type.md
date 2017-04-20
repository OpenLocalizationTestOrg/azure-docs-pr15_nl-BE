<properties
   pageTitle="Logica apps inhoud typen behandeling | Microsoft Azure"
   description="Begrijpen hoe logica Apps omgaat met inhoudstypen op ontwerp en uitvoering"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-content-type-handling"></a>Logica Apps inhoudstype verwerking

Er zijn verschillende soorten inhoud die door een App logica - met inbegrip van JSON, XML-bestanden en binaire gegevens kan stromen.  Terwijl alle inhoudstypen worden ondersteund, sommige kunnen worden begrepen door de logica Apps Engine en anderen mogelijk casting of verbouwing als nodig is.  Het volgende artikel wordt beschreven hoe verschillende inhoudstypen worden verwerkt door de motor en hoe ze kunnen worden goed verwerkt wanneer dat nodig is.

## <a name="content-type-header"></a>Header Content-Type

Om te beginnen de eenvoudige, we kijken naar de twee `Content-Types` die niet vereisen een conversie of casting te gebruiken binnen een logica App - `application/json` en `text/plain`.

### <a name="applicationjson"></a>Application/json

Workflow-engine is gebaseerd op de `Content-Type` header van HTTP wordt aangeroepen om te bepalen van de juiste verwerking.  Elk verzoek aan het inhoudstype `application/json` worden opgeslagen en verwerkt als een JSON-Object.  JSON-inhoud kan bovendien standaard zonder een casting worden geparseerd.  Dus een aanvraag met de header content-type `application/json ` als volgt:

```
{
    "data": "a",
    "foo": [
        "bar"
    ]
}
```

kan worden geparseerd in een workflow met een expressie zoals `@body('myAction')['foo'][0]` een waarde op te halen (in dit geval `bar`).  Er is geen extra casting nodig.  Als u met gegevens die JSON is maar heeft een header die is opgegeven werkt, u kunt handmatig cast deze naar JSON met behulp van de `@json()` functie (bijvoorbeeld: `@json(triggerBody())['foo']`).

### <a name="textplain"></a>Tekst/plain

Vergelijkbaar met `application/json`, HTTP-berichten ontvangen met de `Content-Type` kop van `text/plain` in de ruwe vorm worden opgeslagen.  Bovendien als opgenomen in een verdere acties zonder een casting het verzoek gaat uit met een `Content-Type`: `text/plain` kop.  Bijvoorbeeld, als u werkt met een plat bestand verschijnt de volgende HTTP-inhoud:

```
Date,Name,Address
Oct-1,Frank,123 Ave.
```

as `text/plain`.  Als u in de volgende actie u verzonden als hoofdtekst van een andere aanvraag (`@body('flatfile')`), het verzoek moet een `text/plain` header Content-Type.  Als u met gegevens die als tekst zonder opmaak is maar heeft een header die is opgegeven werkt, u kunt handmatig cast deze naar tekst met de `@string()` functie (bijvoorbeeld: `@string(triggerBody())`)

### <a name="applicationxml-and-applicationoctet-stream-and-converter-functions"></a>Application/xml Application/octet-stream en Converter functies

De logica App Engine behoudt altijd de `Content-Type` die op de HTTP-aanvraag of antwoord is ontvangen.  Wat dit betekent dat als een inhoud wordt ontvangen met `Content-Type` van `application/octet-stream`, met inbegrip van die in een latere actie met geen gietstuk resulteert in een uitgaande aanvraag met `Content-Type`: `application/octet-stream`.  Op deze manier de engine kunt guaruntee gegevens worden niet verloren in de workflow doorloopt.  De actietoestand (ingangen en uitgangen) zijn echter opgeslagen in een object JSON als het de workflow doorloopt.  Dit betekent om te houden van sommige gegevenstypen, de motor wordt de inhoud converteren naar een binary base64 gecodeerd met de juiste metagegevens beide behouden `$content` en `$content-type` -die automatisch worden geconverteerd.  U kunt ook handmatig converteren tussen inhoud typen met behulp van ingebouwde functies conversieprogramma:

* `@json()`-gegevens wordt geworpen`application/json`
* `@xml()`-gegevens wordt geworpen`application/xml`
* `@binary()`-gegevens wordt geworpen`application/octet-stream`
* `@string()`-gegevens wordt geworpen`text/plain`
* `@base64()`-inhoud wordt omgezet in een base64-tekenreeks
* `@base64toString()`-een base64-gecodeerde tekenreeks converteren naar`text/plain`
* `@base64toBinary()`-een base64-gecodeerde tekenreeks converteren naar`application/octet-stream`
* `@encodeDataUri()`-codeert een string als byte-matrix dataUri
* `@decodeDataUri()`-decodeert een dataUri in een matrix van bytes

Als u een HTTP-aanvraag met ontvangen bijvoorbeeld `Content-Type`: `application/xml` van:

```
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

Ik kan casten en later met iets als `@xml(triggerBody())`, of in een functie als `@xpath(xml(triggerBody()), '/CustomerName')`.

### <a name="other-content-types"></a>Andere inhoudstypen

Andere typen inhoud worden ondersteund en werkt met een App logica, maar moet u mogelijk handmatig ophalen van de hoofdtekst van het bericht door het decoderen van de `$content`.  Bijvoorbeeld, als ik zijn die van een `application/x-www-url-formencoded` aanvraag die als volgt gezocht:

```
CustomerName=Frank&Address=123+Avenue
```

omdat dit een niet als tekst zonder opmaak of JSON, worden deze in de actie als opgeslagen:

```
...
"body": {
    "$content-type": "application/x-www-url-formencoded",
    "$content": "AAB1241BACDFA=="
}
```

Waarbij `$content` is de nettolading gecodeerd als een base64-tekenreeks om alle gegevens te behouden.  Aangezien er een eigen functie voor het formulier gegevens momenteel niet, kan nog steeds deze gegevens binnen een werkstroom gebruikt handmatig toegang tot de gegevens met een functie als `@string(body('formdataAction'))`.  Als ik mijn uitgaande verzoek om ook de `application/x-www-url-formencoded` de header content-type, ik kan alleen toe te voegen aan het hoofdgedeelte zonder eventuele staking zoals `@body('formdataAction')`.  Echter, dit werkt alleen als hoofdtekst is de enige parameter in de `body` invoeren.  Als u probeert te doen `@body('formdataAction')` binnen een `application/json` aanvraag ontvangt u een runtime-fout als de gecodeerde tekst wordt verzonden.
