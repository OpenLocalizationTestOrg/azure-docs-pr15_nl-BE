<properties
    pageTitle="Web Apps-overzicht | Microsoft Azure"
    description="Meer informatie over hoe Azure App kun je ontwikkelen en webtoepassingen"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/28/2016"
    ms.author="cephalin"/>

# <a name="web-apps-overview"></a>Web Apps-overzicht

*App Service Web Apps* is een volledig beheerde compute platform dat is geoptimaliseerd voor het hosten van websites en webtoepassingen. Dit [platform als een service](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS)-aanbod van Microsoft Azure kunt u zich concentreren op uw bedrijfslogica terwijl Azure zorgt voor de infrastructuur uit te voeren en uw apps schalen.

De volgende 5 minuten video introduceert Azure App Service Web Apps.

[AZURE.VIDEO azure-app-service-web-apps-with-yochay-kiriaty]

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="what-is-a-web-app-in-app-service"></a>Wat is een web app in de App-Service?

In de App-Service is een *web app* de compute-bronnen die Azure biedt voor het hosten van een website of web-toepassing.  

De compute-bronnen mogelijk op gedeelde of exclusieve virtuele machines (VMs), afhankelijk van de prijzen laag die u kiest. De toepassingscode uitgevoerd in een beheerde VM dat geïsoleerd is van andere klanten.

Uw code kan bestaan uit een taal of een kader dat wordt ondersteund door [Azure App Service](../app-service/app-service-value-prop-what-is.md), zoals ASP.NET, Node.js, Java, Python of PHP. U kunt ook scripts die [PowerShell en andere scripttalen](web-sites-create-web-jobs.md#acceptablefiles) in een web app gebruiken uitvoeren.

Zie voor voorbeelden van typische toepassingsscenario kunt u Web-Apps voor [Web app scenario's](https://azure.microsoft.com/documentation/scenarios/web-app/) en de sectie **scenario's en aanbevelingen** van [Azure App Service, vergelijking van virtuele Machines, Service, en Cloud Services](choose-web-site-cloud-service-vm.md#scenarios).

## <a name="why-use-web-apps"></a>Waarom Web Apps gebruiken?

Hier volgen enkele belangrijke functies van App-Service voor Web Apps:

- **Meerdere talen en frameworks** - App-Service heeft een eersteklas ondersteuning voor ASP.NET, Node.js, Java, Python en PHP. U kunt ook [PowerShell en andere scripts of uitvoerbare bestanden](../app-service-web/web-sites-create-web-jobs.md) uitvoeren op de App Service VMs.

- **Optimalisatie van DevOps** - instellen van [continue integratie en implementatie](../app-service-web/app-service-continuous-deployment.md) met Visual Studio Team Services, GitHub of BitBucket. Bevordering van updates via [testen en staging-omgevingen](../app-service-web/web-sites-staged-publishing.md). Uitvoeren van [A / B testen van](../app-service-web/app-service-web-test-in-production-get-start.md). Uw apps in de App-Service beheren via [Azure PowerShell](../powershell-install-configure.md) of de [cross-platform opdrachtregelinterface (CLI)](../xplat-cli-install.md).

- **Global schalen met hoge beschikbaarheid** - Scale [up](../app-service-web/web-sites-scale.md) of [om](../monitoring-and-diagnostics/insights-how-to-scale.md) handmatig of automatisch. Host uw apps overal in Microsoft globale datacenter infrastructuur en de App Service [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) belooft hoge beschikbaarheid.

- **Verbindingen met SaaS-platforms en gegevens van ruimten** - Kies uit meer dan 50 [connectors](../connectors/apis-list.md) voor enterprise-systemen (zoals SAP, Siebel en Oracle) SaaS-services (zoals televergaderingen en Office 365) en internet-services (zoals Facebook en Twitter). Toegang op ruimten gegevens met behulp van [Hybride verbindingen](../biztalk-services/integration-hybrid-connection-overview.md) en [Virtuele netwerken Azure](../app-service-web/web-sites-integrate-with-vnet.md).

- **Beveiliging en naleving** - App Service is [ISO, SOC, en compatibele PCI](https://www.microsoft.com/TrustCenter/).

- **Toepassingssjablonen** - kiezen uit een uitgebreide lijst met Toepassingssjablonen voor in [Azure Marketplace](https://azure.microsoft.com/marketplace/) u een wizard gebruiken kunt om het installeren van de populaire open source software zoals WordPress, Joomla en Drupal.

- Het werk van maken, implementeren en foutopsporing van **visual Studio-integratie** - specifieke tools in Visual Studio te stroomlijnen.

Bovendien kunt een web app profiteren van de functies van de [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) (zoals ondersteuning voor CORS) en [Mobiele Apps](../app-service-mobile/app-service-mobile-value-prop.md) (zoals push-meldingen). Zie [Azure App-Service-Overzicht](../app-service/app-service-value-prop-what-is.md)voor meer informatie over typen app in de App-Service.

Azure biedt naast Web Apps in App-Service, andere services die kunnen worden gebruikt voor het hosten van websites en webtoepassingen. Voor de meeste scenario's is Web Apps de beste keuze.  [Fabric Service](https://azure.microsoft.com/documentation/services/service-fabric)kunt u voor architectuur, microservice, en als u meer controle over het VMs waarop de code wordt uitgevoerd, kunt u de [Azure virtuele Machines](https://azure.microsoft.com/documentation/services/virtual-machines/). Zie voor meer informatie over het kiezen tussen deze Azure services [Azure App Service, virtuele Machines, Service, en Cloud Services vergelijking](choose-web-site-cloud-service-vm.md).

## <a name="getting-started"></a>Aan de slag

Volg de zelfstudie [distribueren uw eerste web app met Azure 5 minuten](app-service-web-get-started.md) om te beginnen met het implementeren van voorbeeldcode naar een nieuwe web app in de App-Service. Je hebt een gratis account Azure.

Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een starter tijdelijk web app in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.
