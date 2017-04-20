<properties
    pageTitle="Kiezen tussen stroom, logica Apps, functies en WebJobs | Microsoft Azure"
    description="Vergelijken en het contrast de voor wolk integratie van Microsoft-services en bepalen welke service (s) die u moet gebruiken."
    services="functions,app-service\logic"
    documentationCenter="na"
    authors="cephalin"
    manager="wpickett"
    tags=""
    keywords="Microsoft stroom, stroom, logica apps, azure functies, functies, azure, webjobs, webjobs, verwerking, dynamische compute, zonder server architectuur van gebeurtenis"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="09/08/2016"
    ms.author="chrande; glenga"/>

# <a name="choose-between-flow-logic-apps-functions-and-webjobs"></a>Kiezen tussen stroom, logica Apps, functies en WebJobs

Dit artikel vergelijkt en verschilt van de volgende services in de Microsoft cloud, waarmee u alle van problemen met de integratie en automatisering van bedrijfsprocessen oplossen kunt:

- [Microsoft-stroom](https://flow.microsoft.com/)
- [Azure logica Apps](https://azure.microsoft.com/services/logic-apps/)
- [Azure functies](https://azure.microsoft.com/services/functions/)
- [Azure App Service WebJobs](../app-service-web/web-sites-create-web-jobs.md)

Al deze services zijn nuttig als 'lijmen' samen ongelijksoortige systemen. Zij moet alle definiëren invoer, acties, voorwaarden en uitvoer. U kunt elk van hen op een schema of een trigger uitvoeren. Echter elke service een unieke set van waarde wordt toegevoegd en deze te vergelijken is niet een kwestie van "welke service is het beste?" maar een van de "welke service is het beste geschikt zijn voor deze situatie?" Een combinatie van deze services is vaak de beste manier om snel een schaalbare, volledig uitgeruste integratie oplossing te bouwen.

<a name="flow"></a>
## <a name="flow-vs-logic-apps"></a>Stroom versus logica Apps

We kunt bespreken Flow Microsoft Azure logica Apps samen omdat ze beide *configuratie eerste* integration-services, processen en werkstromen te bouwen en te integreren met verschillende SaaS en enterprise toepassingen eenvoudig zijn. 

- Stroom is gebaseerd op logica Apps
- Ze hebben dezelfde werkstroomontwerper
- Werk in een, ook in de andere werken kan [verbindingslijnen](../connectors/apis-list.md)

Stromen in staat stelt elke werknemer office voor het uitvoeren van eenvoudige integratie (b.v. SMS voor belangrijke e-mails ophalen) zonder tussenkomst van ontwikkelaars of IT. Aan de andere kant kunt logica Apps geavanceerde of bedrijfskritieke integraties (bv. een B2B-processen) waar het DevOps- en beveiligingspraktijken op ondernemingsniveau vereist zijn. Het is typisch voor een zakelijke werkstroom toenemen in complexiteit overuren. Dienovereenkomstig, kunt u beginnen met een stroom bij eerste vervolgens converteren naar een app logica als nodig is.

De volgende tabel kunt u bepalen of de stroom of logica Apps geschikt voor een bepaalde integratie is.

|               | Stroom                                                                             | Logica Apps                                                                                          |
|---------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| Doelgroep      | medewerkers, zakelijke gebruikers                                                   | IT-professionals, ontwikkelaars                                                                                 |
| Scenario 's     | Self-service                                                                     | Essentiële                                                                                    |
| Design Tool   | Browser, alleen de gebruikersinterface                                                              | Browser- en [Visual Studio](../app-service/logic/app-service-logic-deploy-from-vs.md) [codeweergave](../app-service-logic/app-service-logic-author-definitions.md) beschikbaar |
| DevOps        | Ad-hoc ontwikkelen in productie                                                    | bronbeheer, testen, ondersteuning, en automatisering en beheerbaarheid in [Azure Resource Management](../app-service-logic/app-service-logic-arm-provision.md)|
| Admin-ervaring| [https://flow.Microsoft.com](https://flow.microsoft.com)                       | [https://Portal.Azure.com](https://portal.azure.com)                                                |
| Beveiliging      | Standaardprocedures: [gegevens-soevereiniteit](https://wikipedia.org/wiki/Technological_Sovereignty), [codering in rust](https://wikipedia.org/wiki/Data_at_rest#Encryption) voor gevoelige gegevens, enz. | Een veilig gevoel van Azure: [Azure, Security](https://www.microsoft.com/trustcenter/Security/AzureSecurity), [Security Center](https://azure.microsoft.com/services/security-center/) [controlelogboeken](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/)en meer. |

<a name="function"></a>
## <a name="functions-vs-webjobs"></a>Functies versus WebJobs

We kunnen bespreken Azure functies en Azure App Service WebJobs samen omdat ze beide *code eerst* integratieservices en ontworpen voor ontwikkelaars. Hiermee kunt u een script of een stukje code uitvoeren als reactie op de verschillende gebeurtenissen, zoals [Nieuwe opslag BLOB's](functions-bindings-storage.md) of [een WebHook aanvragen](functions-bindings-http-webhook.md). Hier zijn hun overeenkomsten: 

- Beide zijn gebaseerd op [Azure App Service](../app-service/app-service-value-prop-what-is.md) en genieten van functies als [Bronbeheer](../app-service-web/app-service-continuous-deployment.md), [verificatie](../app-service/app-service-authentication-overview.md)en [controle](../app-service-web/web-sites-monitor.md).
- Beide zijn services developer gericht.
- Zowel standaard scripting en programmeertalen ondersteunen.
- Beide hebben NuGet en NPM ondersteunen.

Functies is de natuurlijke ontwikkeling van de WebJobs dat het nodig is de beste dingen over WebJobs en ze aanzienlijk verbeterd. De verbeteringen omvatten: 

- Gestroomlijnde dev, testen en uitvoeren van de code rechtstreeks in de browser.
- Ingebouwde integratie met meer Azure en 3de partij diensten zoals [GitHub WebHooks](https://developer.github.com/webhooks/creating/).
- Pay-per-use, hoeft u niet te betalen voor een [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
- Automatische, [dynamische schalen](functions-scale.md).
- Voor bestaande klanten van App-Service, op App serviceplan nog steeds mogelijk (profiteren van de onder gebruikte bronnen).
- Integratie met logica Apps.

De volgende tabel bevat een overzicht van de verschillen tussen functies en WebJobs:

|                        | Functies                                                                                                                                                                | WebJobs                            |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------|
| Schalen                | Configurationless schalen                                                                                                                                                | schalen met App-serviceplan        |
| Prijzen                | Pay-per-use of een deel van de App-serviceplan                                                                                                                                  | Onderdeel van App-serviceplan           |
| Run-type               | geactiveerd, worden gepland (door de timer-trigger)                                                                                                                                  | geactiveerd, continue, geplande   |
| Trigger-gebeurtenissen         | [timer](functions-bindings-timer.md), [Azure, DocumentDB](functions-bindings-documentdb.md), [Azure gebeurtenis Hubs](functions-bindings-event-hubs), [HTTP/WebHook (GitHub, toegestane vertraging)](functions-bindings-http-webhook.md), [Azure App Service Mobile Apps](functions-bindings-mobile-apps.md), [Azure melding Hubs](functions-bindings-notification-hubs.md), [Azure Service Bus](functions-bindings-service-bus.md), [Azure opslag](articles/functions-bindings-storage.md) | [Azure opslag](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), [Azure Service Bus](websites-dotnet-webjobs-sdk-service-bus.md)         |
| Browser-ontwikkeling | x                                                                                                                                                                        |                                    |
| Venster scripting       | experimentele                                                                                                                                                             | x                                  |
| PowerShell             | experimentele                                                                                                                                                             | x                                  |
| C#                     | x                                                                                                                                                                        | x                                  |
| F#                     | x                                                                                                                                                                        |                                    |
| Bash                   | experimentele                                                                                                                                                             | x                                  |
| PHP                    | experimentele                                                                                                                                                             | x                                  |
| Python                 | experimentele                                                                                                                                                             | x                                  |
| JavaScript             | x                                                                                                                                                                        | x                                  |
| Java                   | experimentele                                                                                                                                                             | x                                  |

Of u functies of WebJobs uiteindelijk afhankelijk van wat u al met App-Service doet. Als u een App Service app die u wilt uitvoeren van codefragmenten hebt en u wilt samen beheren in dezelfde omgeving DevOps, moet u WebJobs gebruiken. Als u wilt uitvoeren de codefragmenten voor andere Azure diensten of zelfs een 3de partij apps, of als u wilt uw codefragmenten integratie afzonderlijk vanuit de App Service apps beheren of als u wilt uw codefragmenten aanroepen vanuit een logica app, moet u gebruikmaken van alle verbeteringen in de functies.  

<a name="together"></a>
## <a name="flow-logic-apps-and-functions-together"></a>Stroom, logica Apps en functies samen

Zoals eerder vermeld, welke service is het meest geschikt voor u is afhankelijk van uw situatie. 

- Voor eenvoudige business optimalisatie, dan stroom gebruiken.
- Als uw integratiescenario te voor stroom is geavanceerde of u mogelijkheden voor DevOps en conformiteit van de beveiliging moet, vervolgens logica Apps te gebruiken.
- Als een stap in de integratiescenario uiterst aangepaste transformatie of een speciale code nodig, vervolgens een functie app schrijven en vervolgens een functie activeren als een actie in uw app logica.

U kunt een app logica aanroepen in een stroom. U kunt ook een functie in een app logica en een app logica in een functie aanroepen. De integratie tussen stroom, logica Apps en functies blijven verbeteren overuren. U kunt iets in één service maken en gebruiken in de andere diensten. Een investering die u in deze drie technologieën aanbrengt dus waard.

## <a name="next-steps"></a>Volgende stappen

Met elk van de services aan de slag met het maken van uw eerste stroom, logica app, functie app of WebJob. Klik op een van de volgende koppelingen:

- [Aan de slag met Microsoft Flow](https://flow.microsoft.com/en-us/documentation/getting-started/)
- [Maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [Maak uw eerste Azure-functie](../azure-functions/functions-create-first-azure-function.md)
- [Implementeren met behulp van Visual Studio WebJobs](../app-service-web/websites-dotnet-deploy-webjobs.md)

Of meer informatie over deze integratieservices met de volgende koppelingen:

- [Gebruik van Azure functies & Azure App Service voor scenario's voor integratie met Christopher Anderson](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
- [Integratie in alle eenvoud door Charles Lamanna](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
- [Logica Apps Live Webcast](http://aka.ms/logicappslive)
- [Microsoft vloeien vaak gestelde vragen](https://flow.microsoft.com/documentation/frequently-asked-questions/)
- [Azure documentatiebronnen voor WebJobs](../app-service-web/websites-webjobs-resources.md)
