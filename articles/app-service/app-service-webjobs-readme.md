<properties
    pageTitle="WebJobs in Azure App-Service"
    description="Informatie over het bouwen van WebJobs achtergrond tests uitgevoerd, communiceren met services, zoals opslag en Service Bus en geplande taken maken."
    services="app-service"
    documentationCenter=""
    authors="christopheranderson"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/10/2015"
    ms.author="chrande"/>

# <a name="using-webjobs-in-azure-app-service"></a>Met behulp van WebJobs in Azure App-Service

Dit artikel bevat koppelingen naar documentatie over het gebruik van Azure WebJobs en de Azure WebJobs SDK. Azure WebJobs bieden een eenvoudige manier om het uitvoeren van scripts of programma's als achtergrondprocessen op [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). U kunt uploaden en uitvoeren van een uitvoerbaar bestand bijvoorbeeld als cmd, bat-, exe (.NET), ps1, v, php, py, js en jar. Deze programma's worden uitgevoerd als WebJobs op een schema (cron) of continu.

De WebJobs SDK gemakkelijker Azure opslag gebruiken. De WebJobs SDK heeft een bindende en trigger-systeem dat met Microsoft Azure Storage Blobs, wachtrijen en tabellen, alsmede Service Bus wachtrijen werkt.

Maken, implementeren en beheren van WebJobs is naadloos met geïntegreerde gereedschappen in Visual Studio. WebJobs van sjablonen maken, publiceren en beheren (uitvoeren/stop/monitor/debug) ze.

Het dashboard van WebJobs in het portal voor Azure biedt krachtige mogelijkheden waarmee u volledige controle over de uitvoering van WebJobs, waaronder de mogelijkheid om op te roepen van afzonderlijke functies binnen de WebJobs. Het dashboard wordt ook weergegeven functie runtimes en logboekregistratie van uitvoer.

[AZURE.INCLUDE [app-service-blueprint-webjobs](../../includes/app-service-blueprint-webjobs.md)]
