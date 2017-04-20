<properties
    pageTitle="Aanbevolen procedures voor Azure App-Service"
    description="Informatie over best practices en het oplossen van problemen voor Azure App-Service."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="best-practices-for-azure-app-service"></a>Aanbevolen procedures voor Azure App-Service

Dit artikel bevat een overzicht van de beste praktijken voor het gebruik van [App-Service Azure](http://go.microsoft.com/fwlink/?LinkId=529714). 

## <a name="colocation"></a>Onderbrengen
Wanneer Azure bronnen voor het opstellen van een oplossing voor een webtoepassing en een database bevinden zich in verschillende regio's kunnen de gevolgen zijn:

*  Wachttijd in de communicatie tussen resources
*  Monetaire toeslagen voor uitgaande gegevens overbrengen cross-regio zoals vermeld op de [Azure prijzen pagina](https://azure.microsoft.com/pricing/details/data-transfers).

Onderbrengen in hetzelfde gebied, is het beste voor Azure resources voor het opstellen van een oplossing voor een web app en een database of opslag-account voor het opslaan van inhoud of gegevens. Bij het maken van bronnen, die moet u controleren of zijn ze in dezelfde regio Azure tenzij u specifieke zakelijke of ontwerpen reden om niet te worden. U kunt een app App Service verplaatsen naar hetzelfde gebied, als uw database door gebruik te maken van de [App Service klonen functie](app-service-web-app-cloning-portal.md) momenteel beschikbaar voor Premium App Service Plan apps.   

## <a name="memoryresources"></a>Wanneer apps verbruiken meer geheugen dan verwacht
Als u merkt dat een app verbruikt meer geheugen dan verwacht zoals aangegeven via controle of service-aanbevelingen rekening houden met de [functie retoucheren van App-Service automatisch](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites). Een van de opties voor de functie Automatische retoucheren duurt aangepaste acties op basis van een drempelwaarde geheugen. Acties omvatten het spectrum van e-mailberichten aan onderzoek via een geheugendump te beperken de spot door recycling van het werkproces. Automatische retoucheren kan worden geconfigureerd via web.config en via een gebruiksvriendelijke gebruikersinterface zoals beschreven op in dit blogbericht voor de [Extensie App Support Site](https://azure.microsoft.com/blog/additional-updates-to-support-site-extension-for-azure-app-service-web-apps).   

## <a name="CPUresources"></a>Wanneer apps verbruikt meer CPU dan verwacht
Als u merkt dat een app verbruikt meer CPU dan verwacht of ervaringen herhaald Processorbelasting vertoont pieken zoals aangegeven via controle of service-aanbevelingen kunt u verticaal schalen of horizontaal schalen de App serviceplan. Als uw toepassing statefull, is verticaal schalen de enige optie, terwijl als de toepassing is stateless, schaal out u meer flexibiliteit en mogelijkheden voor hogere schaal krijgt. 

U kunt deze video bekijken voor meer informatie over 'statefull' vs 'stateless' toepassingen: [Planning een End to End schaalbare meerlagige toepassing op Microsoft Azure Web App](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B414#fbid=?hashlink=fbid). Lees voor meer informatie over App schalen en autoscaling serviceopties: [schaal van een Web App in Azure App-Service](web-sites-scale.md).  

## <a name="socketresources"></a>Wanneer socket middelen zijn uitgeput
Een veelvoorkomende reden voor vermoeiend uitgaande TCP-verbindingen is het gebruik van clientbibliotheken die niet zijn geïmplementeerd om TCP-verbindingen opnieuw te gebruiken, of in het geval van een hoger niveau protocol zoals HTTP - keepalive-niet gebruikt. Raadpleeg de documentatie voor elk van de bibliotheken waarnaar wordt verwezen door de apps in de App-Service van plan bent om ervoor te zorgen zij zijn geconfigureerd of geopend in uw code voor efficiënt hergebruik van uitgaande verbindingen. Ook de bibliotheek documentatie richtsnoeren voor goede maken en vrijgeven of opruimen om te voorkomen dat lekkende verbindingen volgen. Tijdens deze onderzoeken in voortgang impact zijn clientbibliotheken kunnen worden beperkt door geschaald uitbreiden naar meerdere exemplaren.  

## <a name="appbackup"></a>Als uw app back-up wordt gestart, mislukt
De twee meest voorkomende redenen waarom zijn de app back-up mislukt: ongeldig Opslaginstellingen en ongeldige databaseconfiguratie. Deze storingen worden meestal gebeuren wanneer er wijzigingen in de opslag- of bronnen, of wijzigingen voor toegang tot deze bronnen (bijvoorbeeld referenties voor de database die is geselecteerd in de back-upinstellingen bijgewerkt). Back-ups wordt meestal volgens een schema uitgevoerd en toegang tot opslag (voor het uitvoeren van de back-ups van bestanden) en databases nodig (voor het kopiëren en lezen de inhoud moet worden opgenomen in de back-up). Het resultaat van de toegankelijkheid van een van deze bronnen zijn consistent back-up mislukt. 

Bekijk meest recente resultaten als u wilt weten welk type fout gebeurt wanneer de back-fouten optreden. In het geval van fouten bij toegang tot opslag, controleren en bijwerken van de opslaginstellingen in de reservekopie van de configuratie. In het geval van storingen van de toegang tot database, bekijk en bijwerken van uw verbindingen tekenreeksen als onderdeel van de app instellingen; gaat u verder met de back-up configuratie correct zijn de vereiste databases bijwerken. Raadpleeg de documentatie bij de [Back-up van een web app in Azure App-Service](web-sites-backup.md) voor meer informatie over back-up app.

## <a name="nodejs"></a>Wanneer nieuwe Node.js apps zijn geïmplementeerd op Azure App Service
Azure App Service standaardconfiguratie voor Node.js apps is aanpassen aan de wensen van de meest gebruikte apps bedoeld. Als de configuratie voor uw app Node.js zouden kunnen profiteren van een persoonlijke afstemming om prestaties te verbeteren of Resourcegebruik voor de CPU/geheugen/netwerkbronnen te optimaliseren, kunt u onze beste praktijken en stappen voor probleemoplossing kan bekijken. Deze documentatie wordt beschreven iisnode-instellingen wilt configureren voor uw app Node.js, beschrijft de verschillende scenario's of problemen met de app kan worden gericht en wordt aangegeven hoe deze problemen op te lossen: [Aanbevolen procedures en de gids voor probleemoplossing voor knooppunt toepassingen op Azure App-Service](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md).   


