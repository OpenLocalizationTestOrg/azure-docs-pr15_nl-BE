<properties
    pageTitle="Uw app voor Azure App Service implementeren"
    description="Informatie over het implementeren van uw app voor Azure App-Service."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="cephalin;dariac"/>
    
# <a name="deploy-your-app-to-azure-app-service"></a>Uw app voor Azure App Service implementeren

Dit artikel helpt u bij het bepalen van de beste optie voor de implementatie van de bestanden voor uw web app, mobiele app back-end of API app [Azure App](http://go.microsoft.com/fwlink/?LinkId=529714)service en leidt u naar geschikte resources met instructies die specifiek zijn voor de optie van uw voorkeur.

## <a name="overview"></a>Azure App Service implementatie-overzicht

Azure App-Service houdt het kader van de toepassing voor u (ASP.NET, PHP, Node.js, enz.). Sommige frameworks standaard terwijl anderen, zoals Java, Python en zijn ingeschakeld, moet een vinkje voor eenvoudige configuratie inschakelen. U kunt bovendien uw toepassing kader, zoals de PHP-versie of de bitness van de runtime. Zie [uw app in Azure App-Service configureren](web-sites-configure.md)voor meer informatie.

Aangezien u geen zorgen te maken over het web server of toepassing kader, levert uw app App-Service is een kwestie van de implementatie van uw code, binaire bestanden, bestanden met inhoud en hun respectieve mapstructuur in de [map **/site/wwwroot** ](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) in Azure (of de **/site/wwwroot/App_Data/taken/** map voor WebJobs). App-Service ondersteunt de volgende implementatieopties: 

- [FTP- of FTPS](https://en.wikipedia.org/wiki/File_Transfer_Protocol): gebruik uw favoriete FTP- of FTPS hulpmiddel om uw bestanden naar Azure, van [FileZilla](https://filezilla-project.org) naar complete IDEs zoals [NetBeans](https://netbeans.org)ingeschakeld. Dit is een proces van bestand uploaden. Geen extra services worden geleverd door de App Service, zoals versiebeheer, beheer van structuur, enz. 

- [Kudu (Git/volgt of OneDrive/Dropbox)](https://github.com/projectkudu/kudu/wiki/Deployment): gebruik van de [installatie-engine](https://github.com/projectkudu/kudu/wiki) in App-Service. Uw code push naar Kudu rechtstreeks uit een bibliotheek. Kudu biedt ook extra services wanneer code wordt geduwd, met inbegrip van versiebeheer, pakket herstellen, MSBuild en [web haken](https://github.com/projectkudu/kudu/wiki/Web-hooks) voor continue implementatie en andere automatiseringstaken. De Kudu implementatie engine ondersteunt 3 soorten implementatie bronnen:   
    * Inhoud synchroniseren vanuit OneDrive en Dropbox   
    * Continue implementatie op basis van een bibliotheek met automatische synchronisatie van GitHub, Bitbucket en Visual Studio Team Services  
    * Implementatie op basis van een bibliotheek met handmatige synchronisatie van lokale Git  

- [Web implementeren](http://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy): code implementeren met App-Service rechtstreeks vanuit uw favoriete Microsoft tools zoals Visual Studio met hetzelfde gereedschap dat automatiseert de implementatie tot de IIS-servers. Dit hulpprogramma ondersteunt alleen diff-implementatie, maken van de database, transformaties van verbindingsreeksen, enz. Web Deploy verschilt van de Kudu toepassing binaire bestanden voordat ze worden geïmplementeerd op Azure worden gebouwd. Net als FTP wordt geen extra services worden geleverd door App-Service.

Een of meer van deze implementatieprocessen van ondersteuning voor populaire web development tools. De werkelijke DevOps functies tot uw beschikking staan terwijl het gereedschap dat u kiest de implementatieprocessen die u gebruik wilt maken bepaalt kan, is afhankelijk van de combinatie van het implementatieproces en de specifieke hulpmiddelen die u kiest. Bijvoorbeeld, als u Web implementeren vanuit [Visual Studio met Azure SDK](#vspros), zelfs als er geen automatisering van Kudu, krijgt u pakket herstellen en MSBuild automatisering in Visual Studio. 

>[AZURE.NOTE] Deze implementatieprocessen niet daadwerkelijk [de Azure bronnen ter beschikking stellen](../resource-group-template-deploy-portal.md) die uw app wellicht. Echter de meeste van de gekoppelde artikelen laten zien hoe u de toepassing inrichten en implementeren van de code aan het end-to-end. Ook vindt u aanvullende opties voor het inrichten van Azure bronnen in het gedeelte [automatisch implementeren met behulp van opdrachtregelprogramma's](#automate) .
     
## <a name="ftp"></a>Bestanden handmatig kopiëren naar Azure geïmplementeerd via FTP
Als u webinhoud handmatig kopiëren naar een webserver worden gebruikt, kunt u een [FTP-](http://en.wikipedia.org/wiki/File_Transfer_Protocol) programma voor het kopiëren van bestanden, zoals Windows Verkenner of [FileZilla](https://filezilla-project.org/).

De professionals van het handmatig kopiëren van bestanden zijn:

- Vertrouwdheid en minimale complexiteit voor FTP-tooling. 
- Weten precies waar uw bestanden gaat.
- Beveiliging met FTPS.

De nadelen van het handmatig kopiëren van bestanden zijn:

- Gelet op de hoogte van het implementeren van bestanden naar de juiste mappen in de App-Service. 
- Geen versiebeheer voor ongedaan maken als er fouten optreden.
- Geen ingebouwde implementatie geschiedenis voor het oplossen van problemen bij de implementatie.
- Mogelijke lange implementatie tijden omdat vele FTP-hulpprogramma's niet alleen diff-kopiëren bieden en kopieer alle bestanden.  

### <a name="howtoftp"></a>Door bestanden te kopiëren naar Azure handmatig implementeren
Kopiëren van bestanden naar Azure bestaat uit een paar eenvoudige stappen:

1. Ervan uitgaande dat u al referenties van de implementatie van de FTP-verbindingsgegevens verkrijgen door naar **Instellingen**te gaan > **Eigenschappen**en vervolgens de waarden voor **Gebruiker/implementatie van FTP**, **FTP-hostnaam**en **FTPS hostnaam**te kopiëren. Kopieer de waarde van de **gebruiker/Implementatie van FTP-gebruiker** door de Azure Portal met inbegrip van de naam van de app om de juiste context bieden voor de FTP-server weergegeven.
2. Gebruik de verbindingsinformatie die u voor de verbinding met uw app verzameld van uw FTP-client.
3. Uw bestanden en hun respectieve mapstructuur kopiëren naar de [map **/site/wwwroot** ](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) in Azure (of de **/site/wwwroot/App_Data/taken/** map voor WebJobs).
4. Blader naar de URL van uw toepassing om te controleren of dat de toepassing goed werkt. 

Zie de volgende bronnen voor meer informatie:

* [Een PHP-MySQL web app maken en implementeren via FTP](web-sites-php-mysql-deploy-use-ftp.md).

## <a name="dropbox"></a>Synchroniseren met een map wolk implementeren
Een goed alternatief voor het [handmatig kopiëren van bestanden](#ftp) wordt gesynchroniseerd bestanden en mappen naar de App Service van cloud storage Services, zoals OneDrive en Dropbox. Synchroniseren met een map cloud maakt gebruik van de Kudu proces voor implementatie (Zie [overzicht van implementatieprocessen](#overview)).

De professionals van synchroniseren met een map cloud zijn:

- Eenvoudig te implementeren. Services, zoals OneDrive en Dropbox bieden desktop sync-clients, zodat uw lokale werkmap ook de directory van uw implementatie wordt.
- Implementatie met één klik.
- Alle functies in de installatie-engine Kudu beschikbaar is (bijvoorbeeld een pakket herstellen, automatisering).

De nadelen van synchroniseren met een map cloud zijn:

- Geen versiebeheer voor ongedaan maken als er fouten optreden.
- Er is geen geautomatiseerde implementatie handmatige synchronisatie vereist.

### <a name="howtodropbox"></a>Het synchroniseren met een map wolk implementeren
In [Azure Portal](https://portal.azure.com)kunt u een map voor synchronisatie van inhoud in OneDrive of Dropbox cloud opslag, werken met uw app en inhoud in de map en service App synchroniseren met het klikken op een knop.

* [Inhoud synchroniseren vanuit een map wolk naar Azure App-Service](app-service-deploy-content-sync.md). 

## <a name="continuousdeployment"></a>Continu implementeren vanaf een cloud-gebaseerde source control-service
Als uw ontwikkelingsteam wordt cloud-gebaseerde bron code management (SCM) Services, zoals [Visual Studio Team Services](http://www.visualstudio.com/), [GitHub](https://www.github.com)of [BitBucket](https://bitbucket.org/)gebruikt, kunt u App Service te integreren in de opslagplaats en voortdurend implementeren. 

Pros van de implementatie van een cloud-gebaseerde source control-service zijn:

- Versiebeheer inschakelen ongedaan maken.
- Doorlopende implementatie voor Git (en volgt indien van toepassing) configureren opslagplaatsen. 
- Branchespecifieke implementatie, kunt verschillende filialen distribueren aan andere [sleuven](web-sites-staged-publishing.md).
- Alle functies in de installatie-engine Kudu beschikbaar is (bijvoorbeeld implementatie versies, terugdraaien, pakket terugzetten, automatisering).

CON van de implementatie van een cloud-gebaseerde source control-service is:

- Enige kennis van de respectieve SCM-service vereist.

###<a name="vsts"></a>Het voortdurend uit een cloud-gebaseerde source control-service implementeren
In de [Azure Portal](https://portal.azure.com)kunt u doorlopend implementatie van GitHub, Bitbucket en Visual Studio Team Services configureren.

* [Continu-implementatie naar Azure App-Service](app-service-continuous-deployment.md). 

## <a name="localgitdeployment"></a>Implementeren vanaf lokale Git
Als uw ontwikkelingsteam een op ruimten lokale bron code management (SCM)-service op basis van Git gebruikt, kunt u dit als bron voor een implementatie met App-Service. 

Pros van de implementatie van lokale Git zijn:

- Versiebeheer inschakelen ongedaan maken.
- Branchespecifieke implementatie, kunt verschillende filialen distribueren aan andere [sleuven](web-sites-staged-publishing.md).
- Alle functies in de installatie-engine Kudu beschikbaar is (bijvoorbeeld implementatie versies, terugdraaien, pakket terugzetten, automatisering).

CON van de implementatie van lokale Git is:

- Enige kennis van de respectieve SCM systeem vereist.
- Geen turn-key oplossingen voor continu gebruik. 

###<a name="vsts"></a>Het implementeren van lokale Git
In de [Azure Portal](https://portal.azure.com)kunt u lokaal Git te implementeren.

* [Lokale Git distributie naar Azure App-Service](app-service-deploy-local-git.md). 
* [Publiceren op het Web Apps van een git/hg repo](http://blog.davidebbo.com/2013/04/publishing-to-azure-web-sites-from-any.html).  

## <a name="deploy-using-an-ide"></a>De implementatie uitvoeren via een IDE
Als u al met een [Azure SDK](https://azure.microsoft.com/downloads/)of andere IDE suites zoals [Xcode](https://developer.apple.com/xcode/), [Eclips](https://www.eclipse.org)en [IntelliJ IDEE](https://www.jetbrains.com/idea/) [Visual Studio](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) , kunt u uw IDE Azure rechtstreeks vanuit implementeren. Deze optie is ideaal voor een individuele ontwikkelaars.

Visual Studio ondersteunt alle drie implementatieprocessen (FTP, Git en implementeren van Web), afhankelijk van uw voorkeur, terwijl andere IDEs voor App-Service implementeren kunt als deze geïntegreerd met FTP of Git is (Zie [overzicht van implementatieprocessen](#overview)).

De professionals van de implementatie van een IDE met zijn:

- De gereedschappen voor uw toepassing voor end-to-end-levenscyclus mogelijk minimaliseren. Ontwikkelen, opsporen, volgen en implementeren van uw app voor Azure alles zonder te verplaatsen buiten de IDE. 

De nadelen van de implementatie van een IDE met zijn:

- Extra complexiteit in tooling.
- Een bronbeheersysteem nog steeds vereist voor een teamproject.

<a name="vspros"></a>Extra-professionals van de implementatie van Azure SDK met Visual Studio zijn:

- Azure SDK maakt Azure resources eersteklas burgers in Visual Studio. Maken, verwijderen, bewerken, start, en apps stoppen, de back-end SQL query, de Azure app en nog veel meer live-foutopsporing. 
- Live codebestanden op Azure worden bewerkt.
- Live op Azure foutopsporing van apps.
- Geïntegreerde Azure explorer.
- Diff-implementatie. 

###<a name="vs"></a>Het implementeren van rechtstreeks vanuit Visual Studio

* [Aan de slag met Azure en ASP.NET](web-sites-dotnet-get-started.md). Informatie over het maken en implementeren van een eenvoudig project voor ASP.NET MVC web met behulp van Visual Studio en webonderdelen implementeren.
* [Hoe Azure WebJobs implementeren met behulp van Visual Studio](websites-dotnet-deploy-webjobs.md). Het Console-programma projecten zodanig configureren dat ze als WebJobs implementeren.  
* [Een beveiligde ASP.NET MVC 5 app met lidmaatschap, OAuth, en SQL-Database op het Web Apps implementeren](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). Informatie over het maken en implementeren van een ASP.NET-MVC web-project met een SQL-database met behulp van Visual Studio Web implementeren en entiteit Framework Code eerste migraties.
* [ASP.NET Web-distributie met behulp van Visual Studio](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/introduction). Een zelfstudie serie 12 deel, die betrekking heeft op een volledige reeks implementatietaken dan andere in deze lijst. Sommige functies van Azure-implementatie zijn toegevoegd sinds de zelfstudie is geschreven, maar later toegevoegd notities wordt uitgelegd wat er nog ontbreekt.
* [Een ASP.NET-Website en Azure in Visual Studio 2012 uit een Git Repository rechtstreeks implementeren](http://www.dotnetcurry.com/ShowArticle.aspx?ID=881). Wordt uitgelegd hoe u een ASP.NET-webpagina-project in Visual Studio, met de invoegtoepassing Git Git en aansluitende Azure aan de Git repository vastleggen de code implementeren. Starten in Visual Studio 2013, Git ondersteuning is ingebouwd en u hoeft de installatie van een invoegtoepassing.

###<a name="aztk"></a>Het implementeren van de Azure Toolkits voor Eclips en IntelliJ IDEE gebruiken

Microsoft stelt de Web Apps implementeren op Azure rechtstreeks vanuit Eclips en IntelliJ via de [Toolkit voor Eclips Azure](../azure-toolkit-for-eclipse.md) en [Azure Toolkit voor IntelliJ](../azure-toolkit-for-intellij.md). De volgende zelfstudies illustratie van de stappen die betrokken zijn bij de implementatie eenvoudig een "Hallo" world Web App met Azure met beide IDE:

*  [Maak een Hello World Web App voor Azure in Eclips](./app-service-web-eclipse-create-hello-world-web-app.md). In deze zelfstudie wordt beschreven hoe u met behulp van de Toolkit Azure voor Eclips maken en implementeren van een Hello World Web App voor Azure.
*  [Maak een Hello World Web App voor Azure in IntelliJ](./app-service-web-intellij-create-hello-world-web-app.md). In deze zelfstudie wordt beschreven hoe u met de Azure Toolkit voor IntelliJ kunt maken en implementeren van een Hello World Web App voor Azure.


## <a name="automate"></a>Automatisering met behulp van opdrachtregelprogramma 's

* [Automatisering met MSBuild](#msbuild)
* [Bestanden kopiëren met het FTP-programma's en scripts](#ftp)
* [Automatisering met Windows PowerShell](#powershell)
* [Automatisering met .NET API voor beheer](#api)
* [Implementeren vanaf Azure opdrachtregelinterface (CLI Azure)](#cli)
* [Implementeren vanaf opdrachtregel Web implementeren](#webdeploy)
* [Werken met batchscripts FTP](http://support.microsoft.com/kb/96269).
 
Een andere optie is het gebruik van een cloud-gebaseerde service zoals [Octopus implementeren](http://en.wikipedia.org/wiki/Octopus_Deploy). Zie [implementatie van ASP.NET-toepassingen op Azure-websites](https://octopusdeploy.com/blog/deploy-aspnet-applications-to-azure-websites)voor meer informatie.

###<a name="msbuild"></a>Automatisering met MSBuild

Als u de [Visual Studio IDE](#vs) voor ontwikkeling, kunt u [MSBuild](http://msbuildbook.com/) voor het automatiseren van alles wat die u in de IDE doen kunt. U kunt MSBuild als u wilt [Implementeren Web](#webdeploy) of [FTP/FTPS](#ftp) gebruiken om bestanden te kopiëren. Web Deploy kan ook veel andere implementatie-taken, zoals het implementeren van databases automatiseren.

Zie de volgende bronnen voor meer informatie over het opdrachtregelprogramma met behulp van MSBuild-implementatie:

* [ASP.NET Web-distributie met behulp van Visual Studio: implementatie van de opdrachtregel](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). Tiende in een reeks cursussen over de implementatie van Azure met behulp van Visual Studio. Laat zien hoe met behulp van de opdrachtregel implementeren nadat Publicatieprofielen instellen in Visual Studio.
* [In de Microsoft Build Engine: met behulp van MSBuild en Team Foundation Build](http://msbuildbook.com/). Gedrukte boek met hoofdstukken over het gebruik van MSBuild voor de implementatie.

###<a name="powershell"></a>Automatisering met Windows PowerShell

MSBuild- of FTP-implementaties kunt u uitvoeren vanaf de [Windows PowerShell](http://msdn.microsoft.com/library/dd835506.aspx). Als u dat doet, kunt u ook een verzameling Windows PowerShell-cmdlets waarmee de Azure REST API voor beheer eenvoudig aan te roepen.

Zie de volgende bronnen voor meer informatie:

* [Een web app gekoppeld aan een opslagplaats GitHub implementeren](app-service-web-arm-from-github-provision.md)
* [Inrichten van een web app met een SQL-Database.](app-service-web-arm-with-sql-database-provision.md)
* [Inrichten en implementeren van microservices voorspelbaar in Azure](app-service-deploy-complex-application-predictably.md)
* [Gebouw wolk Real-World Apps met Azure - alles automatiseren](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything). E-book hoofdstuk waarin wordt uitgelegd hoe de voorbeeldtoepassing weergegeven in het e-book Windows PowerShell-scripts gebruikt een Azure testomgeving maken en te implementeren. Zie de sectie [bronnen](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything#resources) voor koppelingen naar aanvullende documentatie bij de Azure PowerShell.
* [Met behulp van Windows PowerShell-Scripts publiceren naar Dev en testomgevingen](../vs-azure-tools-publishing-using-powershell-scripts.md). Hoe het gebruik van Windows PowerShell implementatie op scripts die door Visual Studio gegenereerd.

###<a name="api"></a>Automatisering met .NET API voor beheer

U kunt C# code schrijven voor MSBuild- of FTP-functies voor implementatie uitvoeren. Als u dat doet, kunt u toegang tot de Azure management REST API site management functies uitvoeren.

Zie de volgende bronnen voor meer informatie:

* [Alles wat met Azure Management bibliotheken en .NET te automatiseren](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Inleiding tot de .NET API voor beheer en koppelingen naar meer documentatie.

###<a name="cli"></a>Implementeren vanaf Azure opdrachtregelinterface (CLI Azure)

U kunt de opdrachtregel in Windows, Mac of Linux-systemen implementeren met behulp van FTP. Als u dat doet, kunt u ook toegang tot het beheer van Azure REST API met behulp van de CLI Azure.

Zie de volgende bronnen voor meer informatie:

* [Azure-opdracht op de opdrachtregel](/downloads/#cmd-line-tools). Portal-pagina in Azure.com voor informatie over het programma vanaf de opdrachtregel.

###<a name="webdeploy"></a>Implementeren vanaf opdrachtregel Web implementeren

[Implementeren van web](http://www.iis.net/downloads/microsoft/web-deploy) is Microsoft-software voor implementatie van IIS die niet alleen intelligente bestand sync-functies biedt, maar ook kunt uitvoeren of coördineren van veel andere implementatie van taken die niet automatisch worden uitgevoerd wanneer u FTP gebruikt. Bijvoorbeeld kunt Web implementeren implementeren van een nieuwe database of een database-updates samen met uw web app. Web Deploy kunt ook minimaliseren de tijd die nodig is voor het bijwerken van een bestaande site omdat deze op intelligente wijze alleen gewijzigde bestanden kunt kopiëren. Microsoft Visual Studio en Team Foundation Server bieden ondersteuning voor webonderdelen implementeren ingebouwd, maar ook kunt u webonderdelen implementeren rechtstreeks vanaf de opdrachtregel voor het automatiseren van de implementatie. Web Deploy opdrachten zeer krachtig zijn, maar de leercurve te steil.

Zie de volgende bronnen voor meer informatie:

* [Eenvoudige Web Apps: implementatie van](https://azure.microsoft.com/blog/2014/07/28/simple-azure-websites-deployment/). Blog door David Ebbo over een hulpprogramma dat hij schreef het makkelijker implementeren van Web gebruiken.
* [Web Deployment Tool](http://technet.microsoft.com/library/dd568996). Officiële documentatie op de Microsoft TechNet-website. Gedateerd maar nog steeds een goede plaats om te starten.
* [Met behulp van webonderdelen implementeren](http://www.iis.net/learn/publish/using-web-deploy). Officiële documentatie op de website Microsoft IIS.NET. Ook de datum maar een goede plaats om te starten.
* [ASP.NET Web-distributie met behulp van Visual Studio: implementatie van de opdrachtregel](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). MSBuild is de build-engine die wordt gebruikt door Visual Studio en kan ook worden gebruikt vanaf de opdrachtregel voor het distribueren van webtoepassingen naar Web Apps. Deze zelfstudie is onderdeel van een serie die voornamelijk over de implementatie van Visual Studio.

##<a name="nextsteps"></a>Volgende stappen

In sommige gevallen wilt u mogelijk kunt eenvoudig heen en weer schakelen tussen een staging- en productieversie van uw app. Zie voor meer informatie de [Gefaseerde implementatie op Web Apps](web-sites-staged-publishing.md).

Over een plan maken en terugzetten van back-up beschikt, is een belangrijk onderdeel van de implementatieworkflow. Voor meer informatie over de App Service back-up en herstellen van de functie, Zie [Back-ups van Web Apps](web-sites-backup.md).  

Zie voor meer informatie over het gebruik van Azure op rollen gebaseerde toegangscontrole voor het beheren van toegang tot de App-Service implementatie [RBAC en Web App Publishing](https://azure.microsoft.com/blog/2015/01/05/rbac-and-azure-websites-publishing/).


 
