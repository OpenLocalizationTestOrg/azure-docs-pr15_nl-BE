<properties 
    pageTitle="Azure documentatiebronnen voor WebJobs" 
    description="Aanbevolen materialen voor het leren gebruiken van Azure WebJobs en de Azure WebJobs SDK." 
    services="app-service" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="tdykstra"/>

# <a name="azure-webjobs-documentation-resources"></a>Azure documentatiebronnen voor WebJobs

## <a name="overview"></a>Overzicht

Dit onderwerp bevat koppelingen naar documentatie over het gebruik van Azure WebJobs en de Azure WebJobs SDK. Azure WebJobs bieden een gemakkelijke manier voor het uitvoeren van scripts of programma's als achtergrondprocessen in de context van een [App Service web app, API-app of mobiele app](../app-service/app-service-value-prop-what-is.md). U kunt uploaden en uitvoeren van een uitvoerbaar bestand bijvoorbeeld als cmd, bat-, exe (.NET), ps1, v, php, py, js en jar. Deze programma's worden uitgevoerd als WebJobs op een schema (cron) of continu.

Het doel van de [WebJobs SDK](websites-webjobs-resources.md) is voor het vereenvoudigen van de code die u voor veelvoorkomende taken schrijft die een WebJob kunt uitvoeren, zoals beeldverwerking, verwerking van de wachtrij, RSS aggregatie, onderhoud van bestanden en e-mails verzenden. De WebJobs SDK bevat ingebouwde voorzieningen voor het werken met Azure opslag en Service Bus voor het plannen van taken en het afhandelen van fouten en voor veel andere veelvoorkomende scenario's. Bovendien ontworpen om te worden verlengd, en er is een [open source opslagplaats voor uitbreidingen](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview). [Azure-functies](../azure-functions/functions-overview.md) (momenteel in het voorbeeld) is gebaseerd op een versie van de SDK van WebJobs die geschikt is voor C# script, Node.js en andere talen. 

Maken, implementeren en beheren van WebJobs is naadloos met geïntegreerde gereedschappen in Visual Studio. WebJobs van sjablonen maken, publiceren en beheren (uitvoeren/stop/monitor/debug) ze. 

Het dashboard van WebJobs in het portal voor Azure biedt krachtige mogelijkheden waarmee u volledige controle over de uitvoering van WebJobs, waaronder de mogelijkheid om op te roepen van afzonderlijke functies binnen de WebJobs. Het dashboard wordt ook weergegeven functie runtimes en logboekregistratie van uitvoer. 

##<a name="getstarted"></a>Aan de slag met WebJobs en de WebJobs SDK

* [Inleiding tot Azure WebJobs](http://www.hanselman.com/blog/IntroducingWindowsAzureWebJobs.aspx)
* [Azure WebJobs zijn indrukwekkend en te gaan gebruiken ze nu!](http://www.troyhunt.com/2015/01/azure-webjobs-are-awesome-and-you.html) (Blog post door Troy Hunt).
* [Azure WebJobs functies](/blog/2014/10/22/webjobs-goes-into-full-production/)
* [Wat is de WebJobs SDK](websites-dotnet-webjobs-sdk.md)
* [Achtergrond taken begeleiding door Microsoft Patterns and Practices](/documentation/articles/best-practices-background-jobs/)
* [Aankondiging van de 1.1.0 RTM van Microsoft Azure WebJobs SDK](/blog/azure-webjobs-sdk-1-1-0-rtm/)
* [Aan de slag met de Azure WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md)
* [Azure wachtrij opslag gebruiken met de WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)
* [Azure blob-opslag gebruiken met de WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
* [Azure tabelopslag gebruiken met de WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)
* [Het gebruik van Azure Service Bus met de WebJobs SDK](websites-dotnet-webjobs-sdk-service-bus.md)
* [Het gebruik van WebHooks met de WebJobs SDK, met voorbeelden van GitHub, IFTTT en HTTP](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/WebHooks-Walkthrough)
* [Azure WebJobs SDK naslag (PDF download)](http://go.microsoft.com/fwlink/?LinkID=524028&clcid=0x409)
* [WebJobs instellingen documentatie in de GitHub](https://github.com/projectkudu/kudu/wiki/Web-jobs).
* Video 's
    * [WebJobs en de WebJobs SDK](http://channel9.msdn.com/Shows/Cloud+Cover/Episode-153-WebJobs-with-Pranav-Rastogi?utm_source=dlvr.it&utm_medium=twitter)
    * [Azure WebJobs video series op Channel 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)
    * [Introductie van WebJobs gereedschap voor Visual Studio](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
    * [WebJobs Tooling en foutopsporing op afstand](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster)
    * [Azure WebJobs Update met Pranav Rastogi - uitbreiding in versie 1.1](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-183-Azure-WebJobs-Update-with-Pranav-Rastogi)

Zie ook de volgende secties voor [WebJobs implementeren](#deploy) en [testen en debuggen van WebJobs](#debug).

##<a name="deploy"></a>Implementatie van WebJobs

* [Hoe Azure WebJobs implementeren met behulp van Visual Studio](websites-dotnet-deploy-webjobs.md)
* [Het implementeren van WebJobs met behulp van de Portal Azure](web-sites-create-web-jobs.md)
* [Inschakelen vanaf de opdrachtregel of continue levering van Azure WebJobs](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/)
* [GIT levert een .NET console app Azure met WebJobs](http://blog.amitapple.com/post/73574681678/git-deploy-console-app/)
* [Een F # WebJob implementeren op Azure](http://blogs.msdn.com/b/dave_crooks_dev_blog/archive/2015/02/18/deploying-f-web-job-to-azure.aspx)
* [Aangepaste services implementeren als Azure Webjobs](http://withouttheloop.com/articles/2015-06-23-deploying-custom-services-as-azure-webjobs/)
* Video 's
    * [Introductie van WebJobs gereedschap voor Visual Studio](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
    * [WebJobs Tooling en foutopsporing op afstand](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

##<a name="schedule"></a>WebJobs plannen

* [De Azure WebJob dialoogvenster toevoegen](websites-dotnet-deploy-webjobs.md#configure)
* [Een geplande WebJob maken in de Portal voor Azure](web-sites-create-web-jobs.md#CreateScheduled)
* [Aansluiting van een geplande taak om een WebJob](http://blog.davidebbo.com/2015/05/scheduled-webjob.html)
* [Azure WebJobs plannen met cron-expressies](http://blog.amitapple.com/post/2015/06/scheduling-azure-webjobs/)
* [Plannen van afzonderlijke WebJob functies met behulp van de SDK WebJobs TimerTrigger](websites-dotnet-webjobs-sdk.md#schedule)

##<a name="debug"></a>Testen en debuggen van WebJobs

* [Nieuwe Developer en Debugging functies voor Azure WebJobs in Visual Studio](http://blogs.msdn.com/b/webdev/archive/2014/11/12/new-developer-and-debugging-features-for-azure-webjobs-in-visual-studio.aspx)
* [Het Dashboard WebJobs weergeven](websites-dotnet-webjobs-sdk-get-started.md#view-the-webjobs-sdk-dashboard)
* [Hoe u zich aanmeldt met de SDK WebJobs schrijven en deze weergeven in het Dashboard](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs)
* [WebJobs voor externe foutopsporing](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebugwj)
* [Wie schreef dat blob?](http://blogs.msdn.com/b/jmstall/archive/2014/02/19/who-wrote-that-blob.aspx) 
* [Interactieve code in de Cloud hosting](http://blogs.msdn.com/b/jmstall/archive/2014/04/26/hosting-interactive-code-in-the-cloud.aspx)
* [Trace aan Azure WebJobs toe te voegen.](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx)
* [Controleren, opsporen en oplossen van problemen met Microsoft Azure opslag](../storage/storage-monitoring-diagnosing-troubleshooting.md)
* Video 's
    * [WebJobs Tooling en foutopsporing op afstand](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

##<a name="scale"></a>WebJobs schalen

* [Schaalbaarheid van uw webtoepassing met Azure Websites](http://msdn.microsoft.com/magazine/dn786914.aspx)
* [Azure App Service: uitbreidt enorme schaal klaar voor Business Web Apps](https://channel9.msdn.com/Events/Build/2014/3-626). Kaften schalen van web apps met WebJobs, met inbegrip van de WebJobs SDK.
* Video 's
    * [WebJobs schalen](http://channel9.msdn.com/Shows/Azure-Friday/Azure-WebJobs-105-Scaling-out-Web-Jobs)

##<a name="additional"></a>Aanvullende bronnen voor WebJobs

* [Azure WebJobs NH blogbericht door Magnus Mårtensson](http://magnusmartensson.com/azure-webjobs-ga)
* [Powershell Web projecten op Azure App-Service wordt uitgevoerd](http://blogs.msdn.com/b/nicktrog/archive/2014/01/22/running-powershell-web-jobs-on-azure-websites.aspx)
* [Ophalen van een melding wanneer uw Azure WebJobs geactiveerd is voltooid](http://blog.amitapple.com/post/2014/03/webjobs-notification/)
* [Bewaarbeleid voor eenvoudige back-up van Web App met WebJobs](https://azure.microsoft.com/blog/2014/04/28/simple-web-site-backup-retention-policy-with-webjobs/)
* [Azure Web Apps en Cloud Services langzaam op eerste verzoek](http://wp.sjkp.dk/windows-azure-websites-and-cloud-services-slow-on-first-request/). Het gebruik van WebJobs voor het simuleren van de AlwaysOn-functie is alleen beschikbaar voor de standaard prijzen laag bevat.
* [WebJobs systeem correct wordt afgesloten](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.U72Il_5OWUl). Voor de WebJobs SDK systeem correct wordt afgesloten, Zie [systeem correct wordt afgesloten](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).)
* [Automatiseren van back-ups met Azure WebJobs & AzCopy](http://markjbrown.com/azure-webjobs-azcopy/)
* Video 's
    * [Azure WebJobs video's door Magnus Mårtensson](https://www.youtube.com/playlist?list=PLqp1ZOYYUSd81yEzMYLTw8cz91wx_LU9r)
    * [Azure WebJobs video series op Channel 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

##<a name="additionalsdk"></a>Aanvullende bronnen voor WebJobs SDK

* [WebJobs SDK Release-opmerkingen](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes)
* [Broncode WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk)
* [WebJobs SDK extensies broncode](https://github.com/Azure/azure-webjobs-sdk-extensions), met [gedetailleerde uitleg van het extensibility-objectmodel](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).  
* [WebJobs SDK Script-broncode](https://github.com/Azure/azure-webjobs-sdk-script/) (gebruikt voor [Azure functies](../azure-functions/functions-overview.md))
* [WebJob FREB om bestanden te uploaden naar Azure opslag met behulp van de WebJobs SDK](http://thenextdoorgeek.com/post/WAWS-WebJob-to-upload-FREB-files-to-Azure-Storage-using-the-WebJobs-SDK)
* [Azure webjobs buiten Azure hosting, met de voordelen van registratie van een Azure gehost webjob](http://bypassion.dk/?p=510)
* [Bouwen van een hulpprogramma voor het importeren van gegevens met Azure WebJobs](http://www.freshconsulting.com/building-data-import-tool-azure-webjobs/)
* [Overzicht van functies voor Azure](../azure-functions/functions-overview.md)
* Video 's
    * [Azure WebJobs video series op Channel 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

##<a name="samples"></a>Voorbeeldtoepassingen WebJob

* [Monster toepassingen die worden geleverd door het team WebJobs op GitHub](https://github.com/azure/azure-webjobs-sdk-samples)
* [Eenvoudige Azure Web App met WebJobs Backend met de WebJobs SDK](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb)
* [SiteMonitR](http://code.msdn.microsoft.com/SiteMonitR-dd4fcf77). Gebruik van geplande en gebeurtenisgestuurde WebJobs ziet. Zie de blog posten [opnieuw samenstellen van de SiteMonitR met Azure WebJobs SDK](http://www.bradygaster.com/post/rebuilding-the-sitemonitr-using-windows-azure-webjobs).

##<a name="blogs"></a>Blogs

* [Azure blog](/blog)
* [Blog van Amit Apple](http://blog.amitapple.com/). Focussen op WebJobs (niet de SDK).
* [Blog van Magnus Mårtensson](http://magnusmartensson.com/)

##<a name="gethelp"></a>Help opvragen bij WebJobs

* [StackOverflow voor WebJobs](http://stackoverflow.com/questions/tagged/azure-webjobs)
* [StackOverflow voor de WebJobs SDK](http://stackoverflow.com/questions/tagged/azure-webjobssdk)
* [StackOverflow voor Azure functies](http://stackoverflow.com/questions/tagged/azure-functions)
* [Azure en ASP.NET-forum](http://forums.asp.net/1247.aspx)
* [Azure App Service Web Apps-forum](http://social.msdn.microsoft.com/Forums/azure/home?forum=windowsazurewebsitespreview)
* [Azure Web Apps gebruiker Voice-site](https://feedback.azure.com/forums/169385-websites/)
* [Twitter](http://twitter.com/). Gebruik de hashtag #AzureWebJobs.
* [Een WebJobs-fout of een probleem melden](https://github.com/projectkudu/kudu/wiki/Reporting-WebJobs-issues)

