<properties
    pageTitle="Azure App-Service voor web en mobiele apps API | Microsoft Azure"
    description="Informatie over hoe Azure App Service helpt u bij het ontwikkelen, implementeren en beheren van web- en mobiele toepassingen."
    keywords="App-service, azure app service, app servicekosten, schaal, schaalbare, app-implementatie, azure app implementatie, paas, platform as a service, website, website, web, mobiele azure"
    services="app-service"
    documentationCenter=""
    authors="omarkmsft"
    manager="erikre"
    editor="cephalin"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/26/2016"
    ms.author="omark"/>

# <a name="what-is-azure-app-service"></a>Wat is Azure App Service?

*App* is een [platform als een service](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) van Microsoft Azure biedt. Web en mobiele toepassingen voor elk platform of apparaat maken. Automatiseer uw bedrijfsprocessen integreren in uw toepassingen SaaS oplossingen en verbinding maken met toepassingen voor gebouwen. Azure wordt uw apps op volledig beheerde virtuele machines (VMs), uitgevoerd met uw keuze van gedeelde bronnen voor VM of speciale VMs.

App-Service omvat het web en mobiele mogelijkheden die we eerder apart geleverd als Azure Websites en Azure mobiele Services. Het bevat ook nieuwe mogelijkheden voor het automatiseren van bedrijfsprocessen en cloud API's die als host fungeert. Als een geïntegreerde service kunt App-Service u verschillende onderdelen--websites, mobiele app back uiteinden RESTful API's en bedrijfsprocessen--opstellen in een enkele oplossing.

De volgende 4 minuten video geeft een korte uitleg van hoe App Service betrekking heeft op eerdere Azure aanbiedingen en nieuwe functies in deze.

+[AZURE.VIDEO app-service-history-lesson]

## <a name="why-use-app-service"></a>Waarom App Service gebruiken?

Hier volgen enkele belangrijke functies en mogelijkheden van de App:

- **Meerdere talen en frameworks** - App-Service heeft een eersteklas ondersteuning voor ASP.NET, Node.js, Java, Python en PHP. U kunt ook [Windows PowerShell en andere scripts of uitvoerbare bestanden](../app-service-web/web-sites-create-web-jobs.md) uitvoeren op de App Service VMs.

- **Optimalisatie van DevOps** - instellen van [continue integratie en implementatie](../app-service-web/app-service-continuous-deployment.md) met Visual Studio Team Services, GitHub of BitBucket. Bevordering van updates via [testen en staging-omgevingen](../app-service-web/web-sites-staged-publishing.md). Uitvoeren van [A / B testen van](../app-service-web/app-service-web-test-in-production-get-start.md). Uw apps in de App-Service beheren via [Azure PowerShell](../powershell-install-configure.md) of de [cross-platform opdrachtregelinterface (CLI)](../xplat-cli-install.md).

- **Global schalen met hoge beschikbaarheid** - Scale [up](../app-service-web/web-sites-scale.md) of [om](../monitoring-and-diagnostics/insights-how-to-scale.md) handmatig of automatisch. Host uw apps overal in Microsoft globale datacenter infrastructuur en de App Service [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) belooft hoge beschikbaarheid.

- **Verbindingen met SaaS-platforms en gegevens van ruimten** - Kies uit meer dan 50 [connectors](../connectors/apis-list.md) voor enterprise-systemen (zoals SAP, Siebel en Oracle) SaaS-services (zoals televergaderingen en Office 365) en internet-services (zoals Facebook en Twitter). Toegang op ruimten gegevens met behulp van [Hybride verbindingen](../biztalk-services/integration-hybrid-connection-overview.md) en [Virtuele netwerken Azure](../app-service-web/web-sites-integrate-with-vnet.md).

- **Beveiliging en naleving** - App Service is [ISO, SOC, en compatibele PCI](https://www.microsoft.com/TrustCenter/).

- **Toepassingssjablonen** - kiezen uit een uitgebreide lijst met Toepassingssjablonen voor in [Azure Marketplace](https://azure.microsoft.com/marketplace/) u een wizard gebruiken kunt om het installeren van de populaire open source software zoals WordPress, Joomla en Drupal.

- Het werk van maken, implementeren en foutopsporing van **visual Studio-integratie** - specifieke tools in Visual Studio te stroomlijnen.

## <a name="app-types-in-app-service"></a>Typen App in de App-Service

App-Service biedt verschillende *typen app*, die is bedoeld voor het hosten van een bepaald soort belasting:

- [**Web Apps**](../app-service-web/app-service-web-overview.md) - voor het hosten van websites en webtoepassingen.

- [**Mobiele toepassingen**](../app-service-mobile/app-service-mobile-value-prop.md) Back-uiteinden voor het hosten van mobiele app.

- [**API Apps**](../app-service-api/app-service-api-apps-why-best-platform.md) - voor het hosten van RESTful API's.

- [**Logica Apps**](../app-service-logic/app-service-logic-what-are-logic-apps.md) - voor het automatiseren van bedrijfsprocessen en integratie van systemen en gegevens over de wolken-code te schrijven.

Het woord *app* hier verwijst naar de host resources voor het uitvoeren van een werkbelasting. Waarbij 'web app' als voorbeeld, u waarschijnlijk gewend bent om te denken van een web app als de compute-bronnen en de toepassingscode die samen leveren functionaliteit aan een browser. Maar in App-Service is een *web app* de compute-bronnen die Azure biedt voor het hosten van uw toepassingscode. Als uw toepassing is samengesteld van een web-front-end en een RESTful API back-end kan u op een web app implementeren of u kan uw front-code implementeren op een web app en de back-end-code naar een API-app. Uw toepassing kan bestaan uit meerdere App Service apps van verschillende soorten.

## <a name="app-service-plans"></a>App-en serviceplannen

[App serviceplannen](azure-web-sites-web-hosting-plans-in-depth-overview.md) opgeven welk type compute resources die in uw toepassingen worden uitgevoerd. Als u verwacht licht verkeer wordt geladen dat, kunt u gedeelde VMs (**vrije** en **gedeelde** lagen prijzen). Voor hogere belasting, kunt u kiezen uit verschillende formaten van speciale VMs (**Basic**, **Standard**en **Premium** lagen). Meerdere App Service apps hetzelfde plan kunnen delen en deze schaal en de prijzen lagen samen in het plan.

Als u meer schaalbaarheid en geïsoleerd netwerk, kunt u uw toepassingen uitvoeren in een [App-omgeving](../app-service-web/app-service-app-service-environment-intro.md).

## <a name="pricing"></a>Prijzen

Zie voor informatie over hoeveel servicekosten App [App serviceprijzen](https://azure.microsoft.com/pricing/details/app-service/).

## <a name="test-drive-app-service"></a>Test-Drive Service App

[Een monster web app maken, mobiele app, of logica app](http://go.microsoft.com/fwlink/?LinkId=523751) en spelen met het voor een uur zonder creditcard vereist, geen verbintenissen, moeiteloos.

Of een [gratis Azure account](https://azure.microsoft.com/pricing/free-trial/)openen en probeer een van onze introductie zelfstudies:

* [Zelfstudie: Maak een web app.](../app-service-web/app-service-web-get-started.md)
* [Zelfstudie: Een mobiele app maken](../app-service-mobile/app-service-mobile-android-get-started.md)
* [Zelfstudie: Een API-app maken](../app-service-api/app-service-api-dotnet-get-started.md)
* [Zelfstudie: Een logica app maken](../app-service-logic/app-service-logic-create-a-logic-app.md)
