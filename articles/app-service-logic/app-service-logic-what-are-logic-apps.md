<properties 
    pageTitle="Wat zijn logica Apps?" 
    description="Meer informatie over App Service logica Apps" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article" 
    ms.date="10/12/2016"
    ms.author="klam"/>

# <a name="what-are-logic-apps"></a>Wat zijn logica Apps?

Logica Apps bieden een manier om te vereenvoudigen en de implementatie van workflows en schaalbare integratie in de cloud. Het biedt een visueel ontwerpprogramma als model en het automatiseren van het proces als een reeks stappen een werkstroom genoemd.  Er zijn [veel verbindingslijnen](../connectors/apis-list.md) tussen de wolk en op ruimten snel integreren via services en protocollen.  Een app logica begint met een trigger (like 'wanneer een account wordt toegevoegd aan de Dynamics CRM') en na het starten veel combinaties van acties, conversies en voorwaarde logica kan beginnen.

De voordelen van het gebruik van logica Apps zijn:  

- Tijd besparen met het ontwerpen van complexe processen met behulp van gemakkelijk te begrijpen ontwerphulpmiddelen
- Naadloze implementatie van patronen en werkstromen, die anders zouden zijn moeilijk te implementeren in code
- Snel aan de slag van sjablonen
- Aanpassen van uw app logica met uw eigen aangepaste API's, code en acties
- Verbinding maken en synchroniseren van ongelijksoortige systemen via lokale en de cloud
- Bouwen van BizTalk server API, Azure functies en het beheer Azure Service Bus met integratie met eersteklas ondersteuning

Logica Apps is een volledig beheerde iPaaS (integratie Platform als Service) zodat ontwikkelaars geen zorgen te maken over het bouwen, hosten, schaalbaarheid, beschikbaarheid en beheerbaarheid.  Apps logica wordt automatisch opgewaardeerd naar het voldoen aan de vraag.

![Flow app designer](./media/app-service-logic-what-are-logic-apps/LogicAppCapture2.png)

Zoals gezegd, met logica Apps, kunt u bedrijfsprocessen automatiseren. Hier volgen een paar voorbeelden:  
 
* Verplaats bestanden naar een FTP-server geüpload in Azure opslag
* Orders en de route via op gebouwen en cloud systemen
* Alle tweets over een bepaald onderwerp bewaken, analyseren van het sentiment en waarschuwingen en taken voor het opvolgen van ingeladen maken.

Scenario's zoals deze worden geconfigureerd vanuit de visual designer en zonder één regel code te schrijven. Gestart [maken van uw app logica nu]ophalen[create].  Nadat geschreven - kan een app logica worden [snel geïmplementeerd en geconfigureerd](app-service-logic-create-deploy-template.md) voor meerdere omgevingen en regio's.

## <a name="why-logic-apps"></a>Waarom Logic Apps?

Apps logica brengt de snelheid en schaalbaarheid in de bedrijfsruimte van de integratie.  Het gebruiksgemak van de ontwerper van de verschillende beschikbare triggers en acties en krachtige beheerprogramma's moet centraliseren uw API's eenvoudiger dan ooit.  Als bedrijven gaan naar digitalization, logica Apps Hiermee kunt u oude en zeer geavanceerde systemen met elkaar verbinden.

Bovendien met onze [Enterprise Integration rekening] [ biztalk] u kunt schalen om scenario's voor integratie met de kracht van een [XML-berichtenservice]vervallen[xml], [het beheer van handelspartners][tpm], en nog veel meer.

- **Eenvoudig te gebruiken hulpmiddelen** - logica Apps kunnen worden ontworpen end-to-end in de browser of met Visual Studio tools. Met een trigger - van een eenvoudige planning te starten wanneer er een probleem met GitHub wordt gemaakt. Vervolgens goedkeuringen door een willekeurig aantal acties met de rijke galerie van verbindingslijnen.

- **Verbinding-API's eenvoudig** -zelfs samenstelling taken die gemakkelijk zijn te beschrijven zijn moeilijk uit te voeren in de code. Logica Apps kunt u eenvoudig verbinding maken met ongelijksoortige systemen. Wilt u verbinding maken met uw cloud oplossing op uw locatie op marketing facturering systeem? Wilt centraliseren messaging API's en systemen met een Enterprise Service Bus? Logica apps zijn de snelste, meest betrouwbare manier om oplossingen te bieden voor deze problemen.

- **Van sjablonen snel aan de slag** - om u op weg te helpen hebben we een [galerie met sjablonen] geleverd[ templates] waardoor u snel enkele algemene oplossingen. Geavanceerde van B2B-oplossingen voor eenvoudige aansluiting van SaaS, en zelfs een paar dat 'voor de lol' - de galerie is de snelste manier om aan de slag met de kracht van logica Apps.

- **Uitbreidbaarheid baked-in** - de connector die u nodig hebt niet ziet? Logica Apps is ontworpen voor gebruik met uw eigen API's en code; u kunt eenvoudig uw eigen app API aanroepen in een [Functie Azure](https://functions.azure.com) fragmenten van code op afroep uitvoeren of gebruiken als een aangepaste verbindingslijn maken. 

- **Echte integratie paardenkracht** - gemakkelijk starten en groeien als u nodig hebt. Logica Apps kan eenvoudig gebruikmaken van de kracht van BizTalk, Microsoft industrie toonaangevende integratie oplossing om integratie-professionals om de benodigde oplossingen te bouwen. Meer weten over de [Enterprise Integration Pack](./app-service-logic-enterprise-integration-overview.md).

## <a name="logic-app-concepts"></a>Logica App concepten

Dit zijn enkele van de belangrijkste stukken waaruit de logica Apps ervaring. 

- **Workflow** - logica Apps biedt een grafische manier om uw bedrijfsprocessen te modelleren als een reeks stappen of een werkstroom.
- **Verbindingslijnen beheerd** - uw logica apps moeten toegang hebben tot gegevens en services. Beheerde connectoren zijn speciaal om u te helpen bij het maken van verbinding met en werken met gegevens gemaakt. Zie de lijst van verbindingslijnen nu beschikbaar in [beheerde verbindingslijnen][managedapis].
- **Triggers** - sommige verbindingslijnen beheerd kan ook fungeren als een trigger. Een trigger wordt gestart voor een nieuw exemplaar van een werkstroom op basis van een bepaalde gebeurtenis, zoals de ontvangst van een e-mailbericht of een wijziging in uw account Azure opslag.
-  **Acties** - elke stap nadat de trigger in een werkstroom een actie wordt genoemd. Elke actie wordt meestal toegewezen aan een bewerking in de beheerde connector of aangepaste API apps.
- **Enterprise Integration Pack** - omvat mogelijkheden van BizTalk voor meer geavanceerde integratie-scenario's, logica Apps. BizTalk is Microsoft industrie toonaangevende platform voor integratie van. De verbindingslijnen Enterprise Integration Pack kunnen u gemakkelijk opnemen validatie, transformatie, en meer in uw werkstromen logica App.

## <a name="getting-started"></a>Aan de slag  

- Volg het [maken van een App logica] om te beginnen met logica Apps[ create] zelfstudie.  
- [Veelvoorkomende scenario's en voorbeelden weergeven](app-service-logic-examples-and-scenarios.md)
- [U kunt automatiseren van bedrijfsprocessen met logica Apps](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Leren hoe u uw systemen integreren met logica Apps](http://channel9.msdn.com/Events/Build/2016/P462)

[biztalk]: app-service-logic-enterprise-integration-accounts.md
[appservice]: ../app-service/app-service-value-prop-what-is.md
[create]: app-service-logic-create-a-logic-app.md
[managedapis]: ../connectors/apis-list.md
[tpm]: app-service-logic-enterprise-integration-accounts.md
[xml]: app-service-logic-enterprise-integration-b2b.md
[templates]: app-service-logic-use-logic-app-templates.md
