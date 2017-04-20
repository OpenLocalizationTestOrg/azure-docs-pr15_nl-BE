<properties
    pageTitle="Een app in Azure App-Service beveiligen"
    description="Informatie over het beveiligen van een web app, mobiele app back-end of API app in Azure App-Service."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="01/12/2016"
    ms.author="cephalin"/>


#<a name="secure-an-app-in-azure-app-service"></a>Een app in Azure App-Service beveiligen

Dit artikel helpt u aan de slag voor de beveiliging van uw web app, mobiele app back-end of API app in Azure App-Service. 

Beveiliging in Azure App-Service heeft twee niveaus: 

- **Infrastructuur-en platform** - Azure services moet u dingen veilig in de cloud daadwerkelijk uitvoeren als u vertrouwt.
- **Application security** - moet u de app zelf veilig ontwerpen. Dit omvat het integreren met Azure Active Directory, hoe u certificaten beheren en hoe u ervoor zorgen dat u veilig kunt praten met andere services. 

#### <a name="infrastructure-and-platform-security"></a>Infrastructuur en platform beveiliging
Namelijk App Service de Azure VMs, opslag, netwerkverbindingen, frameworks voor het web, management en van integratiefuncties en nog veel meer houdt, het actief beveiligd en harde en krachtige naleving doorloopt en controleert doorlopend om ervoor te zorgen dat:

- De App Service apps zijn geïsoleerd uit zowel het Internet en andere klanten Azure bronnen.
- Mededeling van geheimen (bv. verbindingsreeksen) tussen uw app App Service en andere Azure bronnen (bijvoorbeeld een SQL-Database) in een resourcegroep blijft in Azure en de grenzen van een netwerk niet kruisen. Geheimen worden altijd gecodeerd.
- Alle communicatie tussen uw app App Service en externe bronnen, zoals PowerShell management, opdrachtregelinterface Azure SDK's, overige API's en hybride verbindingen, correct zijn gecodeerd.
- 24-uurs threat management App Service resources beschermt tegen malware, distributed denial of service (DDoS), man-in-the-middle (MITM) en andere bedreigingen. 

Zie voor meer informatie over de beveiliging van de infrastructuur en het platform in Azure [Azure Vertrouwenscentrum](/support/trust-center/security/).

#### <a name="application-security"></a>Toepassingsbeveiliging

Azure is verantwoordelijk voor het beveiligen van de infrastructuur en het platform waarop de toepassing wordt uitgevoerd, is maar zelf verantwoordelijk voor het beveiligen van uw toepassing zelf. Met andere woorden, wilt u ontwikkelen, implementeren en beheren van uw toepassingen en inhoud op een veilige manier. Zonder deze kan toepassingscode of inhoud nog steeds kwetsbaar voor aanvallen, zoals:

- SQL Injection
- Sessie hacken
- Cross-site scripting
- MITM op toepassingsniveau
- Op toepassingsniveau DDoS

Een volledige bespreking van beveiligingsoverwegingen voor webtoepassingen valt buiten het bestek van dit document. Als uitgangspunt voor verdere informatie over het beveiligen van uw toepassing, Zie de [Open Web Application Security Project (OWASP)](https://www.owasp.org/index.php/Main_Page), met name de [top 10 project.](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project), waarin de huidige top 10 essentiële web application security fouten zoals bepaald door de leden OWASP.

## <a name="perform-penetration-testing-on-your-app"></a>Penetratie van uw app testen uitvoeren

Een van de eenvoudigste manieren om nog gestart met het testen voor beveiligingsproblemen op uw app App-Service is de [integratie met folie beveiliging](/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) met één klik kwestbare op uw app in te voeren. U kunt de resultaten bekijken in een gemakkelijk te begrijpen en informatie over het oplossen van elk beveiligingsprobleem met stapsgewijze instructies.

Als u liever uw eigen penetratie proeven verrichten of een andere scanner suite of provider wilt gebruiken, moet u als volgt het [goedkeuringsproces van Azure penetratie testen](https://security-forms.azure.com/penetration-testing/terms) en verkrijgen van voorafgaande toestemming voor het uitvoeren van de gewenste penetratie testen.

##<a name="https"></a>Beveiligde communicatie met klanten

Als u de ** \*. azurewebsites.net** gemaakt voor uw app App Service domeinnaam u HTTPS, onmiddellijk kunt gebruiken als een SSL-certificaat is bedoeld voor alle ** \*. azurewebsites.net** domeinnamen. Als uw site een [aangepaste domeinnaam](web-sites-custom-domain-name.md)gebruikt, kunt u een SSL-certificaat uploaden naar [HTTPS inschakelen](web-sites-configure-ssl-certificate.md) voor het aangepaste domein.

[HTTPS](https://en.wikipedia.org/wiki/HTTPS) inschakelen kunt helpen beschermen tegen MITM aanvallen op de communicatie tussen uw app en de gebruikers.

## <a name="secure-data-tier"></a>Beveiligen van gegevens

App-Service is sterk geïntegreerd met SQL-Database, die de verbindingsreeksen zijn over de hele linie gecodeerd en worden alleen ontsleuteld op de VM waarop de toepassing wordt uitgevoerd *en* alleen wanneer de toepassing wordt uitgevoerd. Azure SQL-Database bevat bovendien veel beveiligingsfuncties waarmee u beveiligde toepassingsgegevens tegen cyber bedreigingen, inclusief [codering in rust](https://msdn.microsoft.com/library/dn948096.aspx) [Altijd gecodeerd](https://msdn.microsoft.com/library/mt163865.aspx), [Dynamische gegevens maskeren](../sql-database/sql-database-dynamic-data-masking-get-started.md)en [Bedreiging detectie](../sql-database/sql-database-threat-detection-get-started.md). Als u vertrouwelijke gegevens of conformiteitsvereisten, Zie [de SQL-Database beveiligen](../sql-database/sql-database-security.md) voor meer informatie over het beveiligen van uw gegevens.

Als u een externe databaseprovider, zoals ClearDB, moet u contact opnemen met de documentatie van de provider direct op de best practices voor beveiliging.  

##<a name="develop"></a>Ontwikkeling en implementatie beveiligen

### <a name="publishing-profiles-and-publish-settings"></a>Publicatie-profielen en publicatie-instellingen

Bij het ontwikkelen van toepassingen, het uitvoeren van beheertaken of automatiseren met behulp van hulpprogramma's zoals **Visual Studio**, **Web-Matrix**, **Azure PowerShell** of **Azure opdrachtregelinterface (CLI Azure)**, kunt u een bestand *in publicatie-instellingen* of een *profiel voor publiceren*. Beide typen verificatie Azure en moeten worden beveiligd om te voorkomen dat onbevoegde toegang.

* Bevat een bestand met **publicatie-instellingen**

    * Uw Azure abonnements-ID

    * Een certificaat waarmee u beheertaken uitvoeren voor uw abonnement *zonder een accountnaam of wachtwoord bevatten*.

* Een **profiel voor publiceren** bestand bevat

    * Informatie over het publiceren naar uw app

Als u een hulpprogramma dat een bestand publiceren of bestand publiceren-profiel gebruikt gebruiken, moet u het bestand met de publicatie-instellingen of het profiel in het hulpprogramma en **Verwijder** het bestand importeren. Als u moet het bestand delen met anderen, bijvoorbeeld aan het project werken deze opslaan in een veilige plek, bijvoorbeeld in een *gecodeerde* map met beperkte machtigingen.

Bovendien moet u controleren of dat de ingevoerde referenties zijn beveiligd. Bijvoorbeeld **Azure PowerShell** en **Azure opdrachtregelinterface (CLI Azure)** van de geïmporteerde gegevens opgeslagen in de **basismap** (*~* op Linux of OS X-systemen en */users/yourusername* op Windows-systemen.) Voor extra veiligheid kunt u voor het **coderen van** deze locaties met behulp van codering hulpprogramma's voor uw besturingssysteem.

### <a name="configuration-settings-and-connection-strings"></a>Configuratie-instellingen en verbindingsreeksen
Het is gebruikelijk voor het opslaan van tekenreeksen, verificatiereferenties en andere gevoelige informatie in configuratiebestanden. Helaas kunnen deze bestanden worden weergegeven op uw website of ingecheckt in een openbare bibliotheek, deze informatie vrijgeeft. Een eenvoudige zoekopdracht op [GitHub](https://github.com), bijvoorbeeld, kan aan het licht komen talloze configuratiebestanden met blootgestelde geheimen in de openbare archieven.

De beste manier is om deze informatie van uw app in de configuratiebestanden. App-Service kunt u de configuratiegegevens worden opgeslagen als onderdeel van de runtime-omgeving als **app instellingen** en **verbindingsreeksen**. De waarden worden blootgesteld aan de toepassing in runtime door de *omgevingsvariabelen* voor de meeste programmeertalen. Deze waarden worden ingebracht voor .NET-toepassingen in uw configuratie van .NET tijdens runtime. In deze situaties, met uitzondering van blijft deze configuratie-instellingen het gecodeerd tenzij u weergeven of configureren via de [Azure Portal](https://portal.azure.com) of hulpprogramma's zoals PowerShell of de CLI Azure. 

Configuratie-informatie op te slaan in de App-Service, maakt het mogelijk voor de beheerder van de app vergrendelen gevoelige informatie voor de productie-apps. Ontwikkelaars kunnen een afzonderlijke set configuratie-instellingen gebruiken voor de ontwikkeling van de app en de instellingen kunnen automatisch worden vervangen door de instellingen die zijn geconfigureerd in de App-Service. Zelfs niet de ontwikkelaars moeten weten de geheimen die zijn geconfigureerd voor de productie-app. Zie [webtoepassingen configureren](web-sites-configure.md)voor meer informatie over het configureren van toepassingsinstellingen voor en verbindingsreeksen in App-Service.

### <a name="ftps"></a>FTPS

Azure App-Service biedt veilige FTP-toegang tot het bestandssysteem van uw app via **FTPS**. Hiermee kunt u veilig toegang krijgen tot de toepassingscode op het web app als diagnostische logboeken. Het verdient aanbeveling FTPS altijd te gebruiken in plaats van FTP. 

De koppeling FTPS voor uw app vindt u de volgende stappen uit:

1. Open de [Azure Portal](https://portal.azure.com).
2. Selecteer **alle bladeren**.
3. Selecteer in de blade **Bladeren** **App Services**.
4. Selecteer de gewenste app in de blade **App Services** .
5. Selecteer **alle instellingen**van de app-blade.
6. Selecteer de **Eigenschappen**van de bladeserver **Instellingen** .
7. De FTP- en TRANSFEREERT koppelingen vindt u op de **Instellingen voor** blade. 

Zie voor meer informatie over TRANSFEREERT, [File Transfer Protocol](http://en.wikipedia.org/wiki/File_Transfer_Protocol).

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over de beveiliging van de Azure platform, informatie op een **veiligheidsincident of misbruik**melden of Microsoft informeren dat u **penetratie testen** van uw site uitvoeren zult, Zie de Beveiligingssectie van het [Vertrouwenscentrum van Microsoft Azure](https://azure.microsoft.com/support/trust-center/security/).

Voor meer informatie over de bestanden **web.config** of **applicationhost.config** in App Service apps, Zie [configuratieopties ontgrendeld in Azure App Service web apps](https://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/).

Zie voor informatie over de logboekfunctie voor App Service apps die handig zijn kunnen voor aanvallen te detecteren, [Diagnostische logboekregistratie inschakelen](web-sites-enable-diagnostic-log.md).

>[AZURE.NOTE] Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een app tijdelijk starter in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.

## <a name="whats-changed"></a>Wat er veranderd

* Zie voor een handleiding voor het wijzigen van Websites met App-Service: [Azure App Service en de Impact op de bestaande Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)
