<properties
   pageTitle="Logica Apps scenario's en voorbeelden | Microsoft Azure"
   description="Zie algemene logica apps voorbeelden en informatie over het implementeren van veelvoorkomende scenario 's"
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

# <a name="logic-apps-examples-and-common-scenarios"></a>Voorbeelden van logica Apps en veelvoorkomende scenario 's

Dit document details veelvoorkomende scenario's en voorbeelden kunt u enkele van de manieren waarop u begrijpen kunnen logica apps gebruiken om bedrijfsprocessen te automatiseren. 

## <a name="custom-triggers-and-actions"></a>Aangepaste Triggers en acties

Er zijn verschillende manieren een app logica uit een andere toepassing kan worden geactiveerd. Hier ziet u enkele algemene voorbeelden:

- [Een actie of aangepaste trigger maken](app-service-logic-create-api-app.md)
- [Langlopende acties](app-service-logic-create-api-app.md)
- [HTTP-verzoek trigger (POST)](app-service-logic-http-endpoint.md)
- [Webhook-triggers en acties](app-service-logic-create-api-app.md)
- [Polling-triggers](app-service-logic-create-api-app.md)

### <a name="scenarios"></a>Scenario 's

- [Synchrone antwoord vragen](app-service-logic-http-endpoint.md)
- [Antwoord met SMS aanvragen](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)

## <a name="error-handling-and-logging"></a>Registratie en foutafhandeling

- [Uitzondering en foutafhandeling](app-service-logic-exception-handling.md)
- [Azure waarschuwingen en diagnostische gegevens configureren](app-service-logic-monitor-your-logic-apps.md)

### <a name="scenarios"></a>Scenario 's

- [Use-Case: Fout en afhandeling van uitzonderingen](app-service-logic-scenario-error-and-exception-handling.md)

## <a name="deploying-and-managing"></a>Implementeren en beheren

- [Maken van een geautomatiseerde implementatie](app-service-logic-create-deploy-template.md)
- [Bouw en implementeer logica apps vanuit Visual Studio](app-service-logic-deploy-from-vs.md)
- [Monitor logica apps](app-service-logic-monitor-your-logic-apps.md)

## <a name="content-types-conversions-and-transformations"></a>Inhoudstypen, conversies en transformaties

De logica Apps [workflow definition language](http://aka.ms/logicappsdocs) bevat veel functies als u wilt converteren en werken met verschillende typen inhoud.  Bovendien wordt de motor al dat om inhoud typen behouden als overdrachten via de werkstroom kan het doen.

- [Verwerking van inhoud typen](app-service-logic-content-type.md) zoals application/json, application/xml en tekst zonder opmaak /
- [Definities van de werkstroom ontwerpen](app-service-logic-author-definitions.md)
- [Naslaggids voor workflow-definitie](http://aka.ms/logicappsdocs)

## <a name="batches-and-looping"></a>Batches en herhalen

- [SplitOn](app-service-logic-loops-and-scopes.md)
- [ForEach](app-service-logic-loops-and-scopes.md)
- [Tot en met](app-service-logic-loops-and-scopes.md)

## <a name="integrating-with-azure-functions"></a>Integratie met Azure functies

- [Azure integratie van functies](app-service-logic-azure-functions.md)

### <a name="scenarios"></a>Scenario 's

- [Azure functie als een trigger Service Bus](app-service-logic-scenario-function-sb-trigger.md)

## <a name="http-rest-and-soap"></a>HTTP, REST en ZEEP

 - [Het aanroepen van ZEEP](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)


Wij zullen houden toe te voegen scenario's en voorbeelden in dit document. Gebruik de onderstaande sectie opmerkingen te laten weten welke voorbeelden of scenario's u zou willen zien hier.