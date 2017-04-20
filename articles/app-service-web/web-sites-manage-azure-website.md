<properties 
    pageTitle="Een web app in Azure App-Service beheren" 
    description="Koppelingen naar bronnen voor het beheren van een web app in Azure App-Service." 
    services="app-service\web" 
    documentationCenter="" 
    authors="erikre" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="rachelap"/>

# <a name="manage-a-web-app-in-azure-app-service"></a>Een web app in Azure App-Service beheren

Dit onderwerp bevat koppelingen naar bronnen voor het beheren van een web app in [Azure App-Service](http://go.microsoft.com/fwlink/?LinkId=529714). Management omvat alle taken die uw web app soepel te houden. 

Gedurende de levensduur van een web app zal u verschillende beheertaken kunt uitvoeren, uitvoeren terwijl u vanaf de eerste installatie naar de normale werking, onderhoud en updates.

Veel web app beheertaken kunnen worden uitgevoerd in de Portal Azure.

## <a name="before-you-deploy-your-web-app-to-production"></a>Voordat u uw web app voor de productie implementeert

### <a name="choose-a-tier"></a>Kies een laag

Azure App Service wordt aangeboden in vijf niveaus: vrije, gedeeld, Basic, Standard en Premium. Zie voor informatie over de functies en prijzen voor elke laag [prijsdetails](/pricing/details/app-service/). 

- [Serviceplannen App](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) kunt u meerdere web apps onder hetzelfde niveau groeperen.
- U kunt altijd [overschakelen lagen](web-sites-scale.md) nadat u uw web app maken.

### <a name="configuration"></a>Configuratie

De [Azure Portal](https://portal.azure.com/) gebruiken om verschillende configuratie-opties instellen. Zie [webtoepassingen configureren in Azure App-Service](web-sites-configure.md)voor meer informatie. Hier is een snelle checklist:

- Selecteer **runtime-versies** voor .NET, PHP, Java of Python, als dat nodig is.
- **WebSockets** inschakelen als de WebSocket-protocol wordt gebruikt voor uw web app. (Inclusief toepassingen die gebruikmaken van [ASP.NET-SignalR](http://www.asp.net/signalr) of [socket.io](web-sites-nodejs-chat-app-socketio.md).)
- Voert u doorlopend web projecten? In dat geval **Altijd op**inschakelen.
- Stel het **standaarddocument**, bijvoorbeeld index.html.

Naast deze basisconfiguratie-instellingen kunt u het volgende configureren:

- **Secure Socket Layer (SSL)** versleuteling. Voor gebruik van SSL met een aangepaste domeinnaam, moet u een SSL-certificaat verkrijgen en configureren van uw web app om het te gebruiken. Zie [HTTPS inschakelen voor een web app in Azure App-Service](web-sites-configure-ssl-certificate.md).
- **Aangepaste domeinnaam.** Uw web app heeft automatisch een subdomein onder azurewebsites.net. U kunt een aangepaste domeinnaam, bijvoorbeeld contoso.com koppelen. Zie [een aangepaste domeinnaam in Azure App-Service configureren](web-sites-custom-domain-name.md).

Taal-specifieke configuratie:

- **PHP**: [PHP in Azure App Service Web Apps configureren](web-sites-php-configure.md).
- **Python**: [Python met Web-Apps Azure App-Service configureren](web-sites-python-configure.md)


## <a name="while-your-web-app-is-running"></a>Tijdens het uitvoeren van uw web app

Als uw web app actief is, wilt u controleren of deze beschikbaar is en dat wordt aangepast om te voldoen aan de gebruikersverkeer. Mogelijk moet u ook fouten.

### <a name="monitoring"></a>Monitoring

- Via de Portal Azure kunt u [prestatiegegevens toevoegen](web-sites-monitor.md) zoals CPU-gebruik en het aantal aanvragen van clients.
- [Schaal van uw web app](web-sites-scale.md) in reactie op het verkeer. Afhankelijk van uw niveau, kunt u het nummer van het VMs en/of de grootte van de VM-exemplaren te schalen. In de Standard- en Premium-lagen, kunt u ook instellen autoscaling, zodat uw web app automatisch wordt aangepast volgens een vaste planning of reactie te laden.  
 
### <a name="backups"></a>Back-ups

- Stel [Automatische back-ups](web-sites-backup.md) van uw web app. Meer informatie over back-ups in [deze video](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/).
- Meer informatie over de opties voor het [herstellen van databases](../sql-database/sql-database-business-continuity.md) in Azure SQL-Database.

### <a name="troubleshooting"></a>Het oplossen van problemen

- Als er iets misgaat, kunt u [oplossen in Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), met diagnostische logboeken en live foutopsporing in de cloud. 
- Er zijn verschillende manieren voor het verzamelen van diagnostische logboeken buiten Visual Studio. Zie [Diagnostische gegevens vastleggen voor web apps in Azure App-Service inschakelen](web-sites-enable-diagnostic-log.md).
- Zie [fouten opsporen in een Node.js in Azure App Service web app](web-sites-nodejs-debug.md)voor Node.js-toepassingen.

### <a name="restoring-data"></a>Gegevens terugzetten

- [Terugzetten](web-sites-restore.md) een web app dat is eerder een back-up.


## <a name="when-you-update-your-web-app"></a>Wanneer het bijwerken van uw web app

Als u geen automatische back-ups hebt ingeschakeld, kunt u een [handmatige back-up](web-sites-backup.md)maken.

Overweeg het gebruik van [gefaseerde implementatie](web-sites-staged-publishing.md). Met deze optie kunt u updates publiceren naar een staging-distributie die naast elkaar wordt uitgevoerd met de productie-implementatie. 

Als u Visual Studio Team Services gebruikt, kunt u doorlopend implementatie van het besturingselement instellen:

- [Met behulp van versiebeheer voor Team Foundation (TFVC)](../cloud-services/cloud-services-continuous-delivery-use-vso.md) 
- [Met behulp van Git](../cloud-services/cloud-services-continuous-delivery-use-vso-git.md)
 
<!-- Anchors. -->

[Before you deploy your site to production]: #before-you-deploy-your-site-to-production
[While your website is running]: #while-your-website-is-running
[When you update your website]: #when-you-update-your-website

  
