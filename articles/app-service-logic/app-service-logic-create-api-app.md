<properties 
    pageTitle="Een API voor logica Apps maken" 
    description="Een aangepaste API gebruiken met logica Apps maken" 
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
    ms.date="10/18/2016"
    ms.author="jehollan"/>
    
# <a name="creating-a-custom-api-to-use-with-logic-apps"></a>Een aangepaste API gebruiken met logica Apps maken

Als u het platform logica Apps uitbreiden wilt, zijn er vele manieren die kunt u bellen naar API's of systemen die niet beschikbaar zijn als een van onze vele out-of-the-box-connectors.  Een van deze methoden voor het maken van een App-API kunt u bellen uit binnen een workflow App logica.

## <a name="helpful-tools"></a>Handige hulpmiddelen

API's voor Apps logica wordt u aangeraden een [swagger](http://swagger.io) document met informatie over de ondersteunde bewerkingen en parameters voor de API genereren.  Er zijn veel bibliotheken (zoals [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle)) die de swagger voor u automatisch gegenereerd.  U kunt ook [TRex](https://github.com/nihaue/TRex) gebruiken om aantekeningen toevoegen aan de swagger goed samenwerken met logica Apps (weergeven de namen, typen eigenschappen, enz.).  Voor sommige monsters van API Apps ontwikkeld voor Apps logica worden Bekijk onze [GitHub opslagplaats](http://github.com/logicappsio) of [blog](http://aka.ms/logicappsblog).

## <a name="actions"></a>Acties

De standaard actie voor een App logica is een domeincontroller die een HTTP-aanvraag accepteren en het resultaat (meestal 200).  Er zijn echter verschillende patronen die u volgen kunt om acties op basis van uw behoeften uitbreiden.

Standaard wordt de logica App engine time-out voor een aanvraag na 1 minuut.  U kunt echter wel uw API uitvoeren van acties die langer duren en hebben de motor die wachten op voltooiing, door een asynchrone of een webhook patroon worden hieronder beschreven.

Standaard acties, te schrijven, een HTTP-aanvraagmethode in de API die beschikbaar wordt gesteld via swagger.  Hier ziet u monsters van API apps die met de logica van Apps in onze [opslagplaats GitHub werken](https://github.com/logicappsio).  Hieronder vindt u manieren doen algemene patronen met een aangepaste verbindingslijn.

### <a name="long-running-actions---async-pattern"></a>Langlopende acties - Async patroon

Wanneer een lange stap of de taak wordt uitgevoerd, is het eerste wat dat u moet doen Zorg ervoor dat de motor weet dat u nog niet is verlopen. Ook moet u communiceren met de motor, hoe zal weten wanneer u klaar met de taak bent en ten slotte moet u relevante gegevens terug naar de motor, zodat deze met de werkstroom kan worden voortgezet. U kunt die via een API voltooien door de onderstaande stroom. Deze stappen zijn van de punt-van-weergave van de aangepaste API:

1. Wanneer een aanvraag wordt ontvangen, onmiddellijk een antwoord retourneren (voordat het werk wordt uitgevoerd). Dit antwoord is een `202 ACCEPTED` antwoord, zodat de motor kent u de gegevens hebt gekregen de nettolading geaccepteerd en wordt nu verwerkt. De 202 antwoord moet bevatten de volgende headers: 
 * `location`koptekst (vereist): dit is een absoluut pad naar de URL logica Apps kunt gebruiken om de status van de taak.
 * `retry-after`(optioneel, wordt standaard ingesteld op 20 voor acties). Dit is het aantal seconden dat de motor moet worden gewacht voordat de polling-URL voor de locatie als status wilt controleren.

2. Wanneer de status van een taak is ingeschakeld, worden de volgende controles uitgevoerd: 
 * Als de taak wordt uitgevoerd: retourneren een `200 OK` reactie met de nettolading van het antwoord.
 * Als de taak nog wordt verwerkt: een ander resultaat `202 ACCEPTED` reactie met dezelfde kopteksten als de eerste reactie

Dit patroon kunt u zeer lang taken in een van uw aangepaste API-thread uitgevoerd maar keepalive een actieve verbinding met de logica Apps engine zodat er geen time-out of gaan voordat het werk is voltooid. Wanneer u dit in uw logica App toevoegt, is het belangrijk te weten hoeft u niet alles in de definitie van de logica App te peilen en de status controleren. Zodra de motor een 202 antwoord geaccepteerd met een geldige locatie-header ziet, worden de async patroon voldoen en blijven opvragen van de koptekst van de locatie tot een niet-202 is geretourneerd.

U ziet een voorbeeld van dit patroon in de GitHub [hier](https://github.com/jeffhollan/LogicAppsAsyncResponseSample)

### <a name="webhook-actions"></a>Webhook acties

U kunt tijdens uw workflow, de logica App wacht 'terugbellen' om door te gaan hebben.  Deze retouraanroep wordt geleverd in de vorm van een HTTP POST.  Voor het implementeren van dit patroon, moet u twee eindpunten op de domeincontroller: aanmelden en afmelden.

Op 'abonneren' wordt de logica App maken en registreren van een callback-URL die de API kunt opslaan en terugbellen met klaar als een HTTP POST.  Eventuele inhoud/headers worden doorgegeven in de App logica en in de rest van de workflow kunnen worden gebruikt.  De logica App engine roept het punt abonneren op worden uitgevoerd zodra het aantal treffers die stap.

Als de uitvoering is geannuleerd, wordt de logica App engine gebeld naar het eindpunt 'afmelden'.  Uw API kan vervolgens de URL van de callback registratie indien nodig.

Op dit moment ondersteunt de logica App Designer geen ontdekken een eindpunt webhook via swagger, zodat voor het gebruik van dit type actie moet u de actie 'Webhook' toevoegen en geef de URL, koppen en hoofdtekst van uw aanvraag.  U kunt de `@listCallbackUrl()` workflow-functie in een van deze velden als nodig is om door te geven in de URL voor terugbellen.

U ziet een voorbeeld van een webhook patroon in GitHub [hier](https://github.com/jeffhollan/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs)

## <a name="triggers"></a>Triggers

Naast acties, kunt u uw aangepaste API act hebben als een trigger voor een App logica.  Er zijn twee patronen die u volgen kunt hieronder voor het starten van een App logica:

### <a name="polling-triggers"></a>Polling-Triggers

Polling-triggers werken net zoals de bovenstaande acties van lange asynchroon uitgevoerd.  De logica App engine roept het trigger-eindpunt na een bepaalde periode (afhankelijk van de SKU, 15 seconden voor de premie, standaard, 1 minuut en 1 uur voor gratis).

De trigger geretourneerd als er geen gegevens beschikbaar is, een `202 ACCEPTED` als antwoord wordt met een `location` en `retry-after` kop.  Echter voor triggers is het raadzaam de `location` de kop bevat een queryparameter van `triggerState`.  Dit is een id voor de API te weten wanneer de laatste keer dat de logica App geactiveerd.  Als er gegevens beschikbaar is, de trigger geeft als resultaat een `200 OK` reactie met de inhoud-nettolading.  Dit wordt de logica App geactiveerd.

Bijvoorbeeld als ik polling is om te zien of een bestand beschikbaar is, kan u polling trigger die zou als volgt samenstellen:

* Als een aanvraag is ontvangen met geen triggerState retourneert de API een `202 ACCEPTED` met een `location` kop met een triggerState van de huidige tijd en een `retry-after` 15.
* Als een aanvraag is ontvangen met een triggerState:
 * Controleer of alle bestanden zijn toegevoegd na de triggerState datum/tijd. 
  * Als er is 1 bestand, een `200 OK` reactie met de inhoud nettolading, verhogen de triggerState op de datum/tijd van het bestand geretourneerd, en stelt u de `retry-after` tot en met 15.
  * Als er meerdere bestanden zijn, kan ik 1 terug tegelijk met een `200 OK`, verhoog mijn triggerState in de `location` kop en een set `retry-after` op 0.  U kunt de motor weet er zijn meer gegevens beschikbaar en deze wordt direct aanvragen op de `location` header die is opgegeven.
  * Als er geen bestanden beschikbaar zijn, een `202 ACCEPTED` respons en laat de `location` triggerState hetzelfde.  Stel `retry-after` tot en met 15.

U ziet een voorbeeld van een trigger polling in GitHub [hier](https://github.com/jeffhollan/LogicAppTriggersExample/tree/master/LogicAppTriggers)

### <a name="webhook-triggers"></a>Webhook-Triggers

Triggers Webhook fungeren als Webhook acties.  De logica App engine roept het eindpunt 'abonneren' wanneer een trigger webhook is toegevoegd en opgeslagen.  De API kan de URL webhook en registreren via HTTP POST aanroepen wanneer gegevens beschikbaar zijn.  Inhoud-nettolading en kopteksten wordt doorgegeven in de logica App uitvoeren.

Als een trigger webhook ooit wordt verwijderd (ofwel de logica App geheel of alleen de trigger webhook), de motor wordt een aanroep naar de URL 'afmelden' waar uw API kan registratie van de callback-URL en stop alle processen zo nodig.

Op dit moment ondersteunt de logica App Designer geen ontdekken een trigger webhook door swagger, zodat voor het gebruik van dit type actie moet u de trigger 'Webhook' toevoegen en geef de URL, koppen en hoofdtekst van uw aanvraag.  U kunt de `@listCallbackUrl()` workflow-functie in een van deze velden als nodig is om door te geven in de URL voor terugbellen.

U ziet een voorbeeld van een trigger webhook in GitHub [hier](https://github.com/jeffhollan/LogicAppTriggersExample/tree/master/LogicAppTriggers)