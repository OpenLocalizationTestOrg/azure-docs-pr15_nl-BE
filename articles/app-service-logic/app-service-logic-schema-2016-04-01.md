<properties 
    pageTitle="Nieuwe schemaversie 06-01-2016 | Microsoft Azure" 
    description="Meer informatie over het schrijven de JSON-definitie voor de nieuwste versie van Logic-apps" 
    authors="jeffhollan" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jehollan"/>
    
# <a name="new-schema-version-2016-06-01"></a>Nieuwe schemaversie 06-01-2016

Het nieuwe schema en de API voor logica apps-versie heeft een aantal verbeteringen waarmee u de betrouwbaarheid verbeteren en het gemak van gebruik van logica apps. Er zijn 3 belangrijke verschillen:

1. Toevoeging van scopes die zijn acties die een collectie acties bevatten.
1. Voorwaarden en lussen zijn eersteklas acties
1. Uitvoering bestellen via uitgebreidere `runAfter` eigenschap (welke vervangt `dependsOn`)

Voor informatie over het bijwerken van uw logica apps uit het schema 2015-08-01-voorbeeld aan het schema 2016-06-01- [de onderstaande sectie upgrade uitchecken.](#upgrading-to-2016-06-01-schema)


## <a name="1-scopes"></a>1. Scopes

Een van de grootste wijzigingen in dit schema is de toevoeging van scopes en de mogelijkheid om acties binnen een andere nesten.  Dit is handig wanneer een reeks acties groeperen of hoeft te nesten acties binnen een andere (bijvoorbeeld een voorwaarde kan bevatten, een andere voorwaarde).  Meer informatie over de syntaxis van de scope kunnen worden gevonden [hier](app-service-logic-loops-and-scopes.md), maar een bereik eenvoudig voorbeeld vindt u hieronder:


```
{
    "actions": {
        "My_Scope": {
            "type": "scope",
            "actions": {                
                "Http": {
                    "inputs": {
                        "method": "GET",
                        "uri": "http://www.bing.com"
                    },
                    "runAfter": {},
                    "type": "Http"
                }
            }
        }
    }
}
```

## <a name="2-conditions-and-loops-changes"></a>2. voorwaarden en worden de wijzigingen

In de vorige versies van het schema zijn voorwaarden en lussen van parameters die zijn gekoppeld aan een enkele actie.  Deze beperking is opgeheven in dit schema en nu voorwaarden en lussen worden weergegeven als een soort actie.  Meer informatie vindt u [in dit artikel](app-service-logic-loops-and-scopes.md)en een eenvoudig voorbeeld van een actie voorwaarde wordt hieronder weergegeven:

```
{
    "If_trigger_is_foo": {
        "type": "If",
        "expression": "@equals(triggerBody(), 'foo')",
        "runAfter": { },
        "actions": {
            "Http_2": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.bing.com"
                },
                "runAfter": {},
                "type": "Http"
            }
        },
        "else": 
        {
            "if_trigger_is_bar": "..."
        }      
    }
}
```

## <a name="3-runafter-property"></a>3. RunAfter, eigenschap

De nieuwe `runAfter` het vervangen van de eigenschap `dependsOn` om te helpen met meer precisie in de uitvoering te bestellen toestaan.  `dependsOn`is synoniem met 'de actie uitgevoerd en is geslaagd,' maar vaak moet u een actie uitvoeren als de vorige actie geslaagd is, mislukt of overgeslagen.  `runAfter`die flexibiliteit maakt.  Het is een object dat welke actie die wordt uitgevoerd aangeeft nadat alle en definieert een matrix met de status die aan de trigger van acceptabel zijn.  Voorbeeld als u wilt uitvoeren nadat u stap A is geslaagd en B is geslaagd of mislukt, zou u het volgende maken `runAfter` eigenschap:

```
{
    "...",
    "runAfter": {
        "A": ["Succeeded"],
        "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrading-to-2016-06-01-schema"></a>06-01-2016 schema bijwerken

Upgraden naar het nieuwe schema van 06-01-2016 kost slechts een paar stappen.  [In dit artikel](app-service-logic-schema-2016-04-01.md)vindt u meer informatie over de wijzigingen in het schema.  Het upgradeproces bevat waarop het upgrade script op te slaan als een nieuwe toepassing van de logica en waarbij oude logica app als dat nodig is.

1. Open uw huidige logica app.
1. Klik op de knop **Schema bijwerken** op de werkbalk
   
    ![][1]
   
    De bijgewerkte definitie worden geretourneerd.  U kunt kopiëren en plak dit in de definitie van een resource als u nodig hebt, maar we **raden** u de knop **OpslaanAls** gebruiken om alle verwijzingen geldig zijn in de bijgewerkte logica app.
1. Klik op de knop **OpslaanAls** in de werkbalk van de upgrade blade.
1. Vul de status van de app en -logica en klik op **maken** om uw app upgrade logica te implementeren.
1. Controleer of dat uw bijgewerkte logica app werkt zoals verwacht.

    >[AZURE.NOTE] Als u een aanvraag of handmatig trigger gebruikt, wordt de callback-URL in uw nieuwe logica app hebt gewijzigd.  De nieuwe URL gebruiken om te controleren of het end-to-end werkt en kunt u via uw bestaande app logica te behouden van vorige URL's kopiëren.

1. *Optioneel* Gebruik de knop **klonen** in de werkbalk (naast het pictogram **Update Schema** in de bovenstaande afbeelding) naar uw vorige logica app overschrijven met de nieuwe versie.  Dit is alleen nodig als u wilt behouden dezelfde resource-ID of trigger-URL van uw app logica aanvragen.

### <a name="upgrade-tool-notes"></a>Opmerkingen voor upgraden

#### <a name="condition-mapping"></a>Toewijzing van de voorwaarde

Het hulpprogramma maakt een beste inspanning om de vertakking waar en ONWAAR acties samen in een bereik in de definitie van de upgrade is uitgevoerd.  Specifiek het designer patroon van de `@equals(actions('a').status, 'Skipped')` moet worden weergegeven als een `else` actie.  Als het hulpprogramma detecteert patronen dat niet wordt herkend, wordt mogelijk echter afzonderlijke voorwaarden voor zowel de true als de waarde false tak maken.  Acties kunnen worden opnieuw toegewezen upgrade boeken als dat nodig is.

#### <a name="foreach-with-condition"></a>ForEach met voorwaarde
  
Het vorige patroon van een foreach lus met een voorwaarde per artikel kan worden gerepliceerd in het nieuwe schema, waarbij de filteractie.  Dit moet automatisch gebeuren bij een upgrade.  De voorwaarde wordt een filteractie voordat de foreach lus (om terug te keren een reeks items die voldoen aan de voorwaarde) en matrix doorgegeven aan de foreach-actie.  U kunt een voorbeeld van deze [in dit artikel](app-service-logic-loops-and-scopes.md) bekijken

#### <a name="resource-tags"></a>Bron codes

Codes van de bron bij een upgrade worden verwijderd en moet u opnieuw instellen voor de aangepaste workflow.

## <a name="other-changes"></a>Andere wijzigingen

### <a name="manual-trigger-renamed-to-request-trigger"></a>Handmatige trigger gewijzigd in aanvraag trigger

Het type `manual` is afgeschaft en hernoemd naar `request` voor het type van `http`.  Dit is meer consistent zijn met het type patroon dat de trigger wordt gebruikt om te bouwen.

### <a name="new-filter-action"></a>Nieuwe 'filter' actie

Als u met een groot aantal en de noodzaak om te filteren op een kleiner aantal items werkt, kunt u het nieuwe type van 'filter'.  Accepteert een array en een voorwaarde wordt en de voorwaarde voor elk artikel evalueren en retourneert een matrix van items die voldoen aan de voorwaarde.

### <a name="foreach-and-until-action-restrictions"></a>ForEach en tot actie beperkingen

De foreach en tot lus worden beperkt tot één enkele actie.

### <a name="trackedproperties-on-actions"></a>TrackedProperties van acties

Acties hebben nu een extra eigenschap (op hetzelfde niveau te `runAfter` en `type`) genaamd `trackedProperties`.  Het is een object waarmee een bepaalde actie invoer of uitvoer worden opgenomen in de diagnostische Azure telemetrie die als onderdeel van een workflow wordt uitgestoten.  Bijvoorbeeld:

```
{                
    "Http": {
        "inputs": {
            "method": "GET",
            "uri": "http://www.bing.com"
        },
        "runAfter": {},
        "type": "Http",
        "trackedProperties": {
            "responseCode": "@action().outputs.statusCode",
            "uri": "@action().inputs.uri"
        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen
- [De definitie van de logica app workflow gebruiken](app-service-logic-author-definitions.md)
- [Een sjabloon voor de implementatie logica app maken](app-service-logic-create-deploy-template.md)


<!-- Image references -->
[1]: ./media/app-service-logic-schema-2016-04-01/upgradeButton.png
