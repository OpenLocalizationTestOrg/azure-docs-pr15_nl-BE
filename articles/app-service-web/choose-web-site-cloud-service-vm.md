<properties
    pageTitle="Azure App Service, virtuele Machines, Service weefsel en Cloud Services vergelijking | Microsoft Azure"
    description="Informatie over het kiezen tussen Azure App Service, virtuele Machines, Service weefsel en Cloud Services voor het hosten van webtoepassingen."
    services="app-service\web, virtual-machines, cloud-services"
    documentationCenter=""
    authors="tdykstra"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/07/2016"
    ms.author="tdykstra"/>

# <a name="azure-app-service-virtual-machines-service-fabric-and-cloud-services-comparison"></a>Vergelijking van Azure App Service, virtuele Machines, Service weefsel en Cloud Services

## <a name="overview"></a>Overzicht

Azure biedt verschillende manieren om de websites host: [Azure App Service][], [virtuele Machines][], [Service weefsel][]en [Cloud Services][]. In dit artikel kunt u de opties te begrijpen en de juiste keuze voor uw webtoepassing.

Azure App-Service is de beste keuze voor de meeste web apps. Implementatie en beheer zijn geïntegreerd in het platform, sites snel kunnen schalen voor het verwerken van verkeer van hoge belastingen en de ingebouwde load balancing en verkeer manager bieden een hoge beschikbaarheid. U kunt bestaande sites met Azure App Service eenvoudig verplaatsen met een [online-migratieprogramma](https://www.migratetoazure.net/), een open source app uit de galerie met webonderdelen toepassing gebruiken of een nieuwe site maken met het framework en de hulpprogramma's van uw keuze. De functie [WebJobs][] gemakkelijk toe te voegen achtergrondtaak verwerken van uw web app in App-Service.

Service-structuur is een goede keuze als u een nieuwe toepassing maken of een bestaande app voor het gebruik van een microservice architectuur opnieuw geschreven. Toepassingen die worden uitgevoerd op een gedeelde groep van machines, klein beginnen en groeien op grote schaal met honderden of duizenden computers zo nodig. Stateful-services kunnen u gemakkelijk consistente en betrouwbare wijze opslaan app staat en Fabric-Service beheert automatisch partitioneren service, schaal en beschikbaarheid voor u.  Fabric-service ondersteunt ook WebAPI met Open Web Interface voor .NET (OWIN) en ASP.NET-Core.  Vergeleken met de App Service, biedt Fabric-Service ook meer controle over of directe toegang tot de onderliggende infrastructuur. U kunt externe in uw servers of server opstarttaken configureren. Cloud Services is vergelijkbaar met de Service-structuur in de mate van controle en gebruiksgemak, maar is nu een oudere service en Service weefsel wordt aanbevolen voor de ontwikkeling van nieuwe.

Als u een bestaande toepassing waarvoor substantiële wijzigingen in App Service of Fabric-Service uit te voeren, kunt u virtuele Machines ter vereenvoudiging van de migratie naar de cloud. Correct vereist configureren, beveiligen en beheren van VMs echter veel meer tijd en IT-expertise in vergelijking met Azure App Service en Service-Fabric. Als u Azure virtuele Machines overweegt, moet dat u rekening houden met het voortdurende onderhoud inspanning patch, bijwerken en beheren van uw VM-omgeving. Azure virtuele Machines is Infrastructure-as-a-Service (IaaS), terwijl de App Service en Service Fabric zijn Platform-as-a-Service (Paas). 

## <a name="features"></a>Eigenschappenvergelijking

De volgende tabel ziet de mogelijkheden van de App Service, Cloud-Services, virtuele Machines en Fabric-Service waarmee u de beste keuze te maken. Zie voor actuele informatie over de SERVICEOVEREENKOMST voor elke optie [Azure Service Level Agreements](/support/legal/sla/).

Functie|App-Service (web apps)|Cloud Services (web-rollen)|Virtuele Machines|Service-Fabric|Notities
---|---|---|---|---|---
In de buurt van instant-implementatie|X|||X|Implementeren van een toepassing of een update naar een Cloud-Service of het maken van een VM, duurt enkele minuten voordat ten minste; implementeren van een toepassing aan een web app duurt seconden.
Uitgebreid naar grotere machines zonder opnieuw distribueren|X|||X|
Web serverinstanties delen de inhoud en configuratie, wat betekent dat u niet hoeft te implementeren of opnieuw configureren als u de schaal.|X|||X|
Meerdere omgevingen (productie en staging)|X|X||X|Fabric-service kunt u meerdere omgevingen voor uw apps hebt of verschillende versies van uw app side-by-side implementatie.
Automatisch beheer van de OS-update|X|X|||Automatische updates voor het besturingssysteem zijn gepland voor een toekomstige Service Fabric vrijgeven.
Naadloze platform switching (eenvoudig verplaatsen tussen de 32-bits en 64-bits)|X|X|||
Code met GIT, FTP-implementatie|X||X||
Code-implementatie met Web implementeren|X||X||Cloud-Services ondersteunt het gebruik van Web implementeren voor het implementeren van updates aan de rol van de afzonderlijke exemplaren. Echter, u deze niet gebruiken voor de eerste installatie van een rol en als u Web implementeren voor een update die u moet implementeren afzonderlijk voor elke instantie van een rol. Meerdere exemplaren zijn nodig om in aanmerking komen voor de SLA Cloud Service voor productieomgevingen.
Ondersteuning voor WebMatrix|X||X||
Toegang tot services, zoals Service Bus-, opslag-, SQL-Database|X|X|X|X|
Web host of web services-laag van een architectuur met meerdere niveaus|X|X|X|X|
Host van de middelste laag van een architectuur met meerdere niveaus|X|X|X|X|App Service web apps kunnen gemakkelijk host een middenlaag REST API en de [WebJobs](http://go.microsoft.com/fwlink/?linkid=390226) functie kan taken achtergrondverwerking host. U kunt WebJobs uitvoeren in een eigen website te bereiken onafhankelijke schaalbaarheid voor de laag. De voorbeeldfunctie van de [API apps](../app-service-api/app-service-api-apps-why-best-platform.md) biedt nog meer functies voor het hosten van andere services.
Geïntegreerde ondersteuning van MySQL als een service|X|X|X||MySQL-as-a-service cloud-Services kunnen worden geïntegreerd via aanbiedingen van de ClearDB, maar niet als onderdeel van de workflow Azure Portal.
Ondersteuning voor ASP.NET, klassieke ASP, Node.js, PHP, Python|X|X|X|X|Fabric-service ondersteunt het maken van een web front-end met [ASP.NET 5](../service-fabric/service-fabric-add-a-web-frontend.md) of u elk type toepassing (Node.js, Java, enz) als [Gast uitvoerbare](../service-fabric/service-fabric-deploy-existing-app.md)kunt implementeren.
Schaalbaar uitbreiden naar meerdere exemplaren zonder opnieuw distribueren|X|X|X|X|Virtuele Machines kunnen worden aangepast om aan meerdere exemplaren, maar de diensten die daarop worden uitgevoerd voor het verwerken van deze schalen moeten worden geschreven. U hebt voor het configureren van een load balancer aanvragen doorsturen via de machines en een groep affiniteit om te voorkomen dat gelijktijdige opnieuw alle exemplaren vanwege onderhoud of hardware-fouten maken.
Ondersteuning voor SSL|X|X|X|X|Voor App Service web apps, wordt SSL voor aangepaste domeinnamen alleen ondersteund voor Basic en Standard-modus. Zie [een SSL-certificaat voor een Website Azure configureren](../app-service-web/web-sites-configure-ssl-certificate.md)voor meer informatie over het gebruik van SSL met web apps.
Integratie met Visual Studio|X|X|X|X|
Foutopsporing op afstand|X|X|X||
TFS-code implementeren|X|X|X|X|
Netwerkisolatie met [Azure Virtual Network](/services/virtual-network/)|X|X|X|X|Zie ook de [Websites van Azure Virtual Network Integration](/blog/2014/09/15/azure-websites-virtual-network-integration/)
Ondersteuning voor [Azure verkeer Manager](/services/traffic-manager/)|X|X|X|X|
Geïntegreerde eindpunt controleren|X|X|X||
Extern bureaublad-toegang tot servers||X|X|X|
Een aangepast MSI installeren||X|X|X|Fabric-service kunt u voor het hosten van een uitvoerbaar bestand als [Gast uitvoerbare](../service-fabric/service-fabric-deploy-existing-app.md) of op de VMs kunt u een app installeren.
Opstarten taken definiëren/uitvoeren||X|X|X|
ETW-gebeurtenissen kunt beluisteren||X|X|X|

## <a name="scenarios"></a>Scenario's en aanbevelingen

Hier volgen enkele algemene scenario's van de toepassing uit met aanbevelingen over welke Azure webhosting-optie mogelijk meest geschikt zijn voor elk.

- [Moet ik een web-front-end met achtergrond verwerking en database backend zakelijke toepassingen geïntegreerd met op lokale activa.](#onprem)
- [Ik moet een betrouwbare manier voor het hosten van mijn bedrijfswebsite die goed schaalbaar en globale voorstellen bereiken.](#corp)
- [Ik heb een IIS6 toepassing wordt uitgevoerd op Windows Server 2003.](#iis6)
- [Ik ben eigenaar van een klein bedrijf en moet ik een goedkope manier voor het hosten van Mijn site, maar met toekomstige groei in gedachten.](#smallbusiness)
- [Ik ben een web of een grafisch ontwerper en ik wil ontwerpen en bouwen van websites voor mijn klanten.](#designer)
- [Ik ben mijn toepassing meerdere lagen met een web-front-migreren naar de Cloud.](#multitier)
- [Mijn toepassing hangt in hoge mate aangepaste Windows- of Linux-omgevingen en deze wilt verplaatsen naar de cloud.](#custom)
- [Mijn site maakt gebruik van open-sourcesoftware, en ik wil deze host in Azure.](#oss)
- [Ik heb een bedrijfs toepassing die verbinding met het bedrijfsnetwerk.](#lob)
- [Ik wil voor het hosten van een REST API of de webservice voor mobiele clients.](#mobile)


### <a id="onprem"></a>Moet ik een web-front-end met achtergrond verwerking en database backend zakelijke toepassingen geïntegreerd met op lokale activa.

Azure App-Service is een geweldige oplossing voor complexe bedrijfstoepassingen. U kunt ontwikkelen apps automatisch schalen op een evenwichtige belasting-platform, zijn beveiligd met Active Directory en verbinding maken met uw bronnen op gebouwen. Het wordt het beheren van deze apps gemakkelijk door middel van een hoogwaardige portal en API's en kunt u inzicht in hoe klanten ze worden gebruikt met de app inzicht's. De [Webjobs][] -functie kunt u de achtergrondprocessen worden uitgevoerd en taken als onderdeel van de weblaag bij hybride verbindingen en VNET functies kunnen u eenvoudig verbinding maken met bronnen voor lokalen terug. Azure App-Service biedt drie 9 SLA voor web apps en kunt u:

* Betrouwbaar uw toepassingen uitvoeren op een zelfherstellende automatisch patches cloud platform.
* Schaal automatisch via een wereldwijd netwerk van datacenters.
* Een back-up en herstel voor noodherstel.
* ISO, SOC2 en PCI-compliant zijn.
* Integreren met Active Directory

### <a id="corp"></a>Ik moet een betrouwbare manier voor het hosten van mijn bedrijfswebsite die goed schaalbaar en globale voorstellen bereiken.

Azure App-Service is een geweldige oplossing voor het hosten van websites voor bedrijven. U kunt web apps schaal snel en eenvoudig om vraag te voldoen via een wereldwijd netwerk van datacenters. Biedt lokale reach, fouttolerantie en intelligente verkeersleiding. Alles op een platform dat beschikt over hulpprogramma's voor beheer van wereldklasse, zodat u inzicht in de gezondheid van de site en siteverkeer snel en gemakkelijk. Azure App-Service biedt drie 9 SLA voor web apps en kunt u:

* Uw websites op betrouwbare wijze uitgevoerd op een zelfherstellende automatisch patches cloud platform.
* Schaal automatisch via een wereldwijd netwerk van datacenters.
* Een back-up en herstel voor noodherstel.
* Beheren van Logboeken en verkeer met geïntegreerde hulpmiddelen.
* ISO, SOC2 en PCI-compliant zijn.
* Integreren met Active Directory

### <a id="iis6"></a>Ik heb een IIS6 toepassing wordt uitgevoerd op Windows Server 2003.

Azure App-Service kunt u gemakkelijk om te voorkomen dat de kosten van de infrastructuur die is gekoppeld aan de migratie van oudere IIS6-toepassingen. Microsoft heeft gemaakt [en gedetailleerde migratieplannen richtlijnen voor de hulpprogramma's voor migratie te gebruiken](https://www.movemetowebsites.net/) waarmee u kunt de compatibiliteit controleren en aangeven van alle wijzigingen die moeten worden aangebracht. Integratie met Visual Studio, TFS en veelgebruikte CMS's eenvoudig implementeren IIS6 toepassingen rechtstreeks naar de cloud. Eenmaal geïmplementeerd, biedt de Azur Portal krachtige beheerprogramma's waarmee u kosten beheren en vraag zo nodig tot het voldoet aan verkleinen. U kunt met het hulpprogramma voor migratie:

* Snel en eenvoudig migreren uw oude Windows Server 2003-webtoepassing naar de cloud.
* Ervoor kiezen om uw gekoppelde SQL-database lokaal als een hybride-toepassing wilt maken.
* De SQL-database met uw oude toepassing verplaatsen automatisch.

### <a id="smallbusiness"></a>Ik ben eigenaar van een klein bedrijf en moet ik een goedkope manier voor het hosten van Mijn site, maar met toekomstige groei in gedachten.

Azure App-Service is een geweldige oplossing voor dit scenario, omdat u kunt beginnen met het gratis en vervolgens meer mogelijkheden wanneer u ze nodig hebt. Elke vrije web app wordt geleverd met een domein dat wordt geleverd door Azure (*your_company*. azurewebsites.net), en het platform omvat geïntegreerde hulpmiddelen voor implementatie en beheer, alsmede een galerie toepassing waarmee u snel aan de slag. Er zijn vele andere services en schaalopties waardoor de site te ontwikkelen met de toegenomen gebruikersvraag. U kunt met Azure App-Service:

- Beginnen met de vrij laag en vervolgens schalen als nodig is.
- Gebruik de galerie toepassing snel instellen van populaire webtoepassingen zoals WordPress.
- Extra Azure services en functies toevoegen aan uw toepassing als nodig is.
- Beveilig uw web app met HTTPS.

### <a id="designer"></a>Ik ben een web of een grafisch ontwerper en ontwerpen en bouwen van websites voor mijn klanten

Voor webontwikkelaars en ontwerpers Azure App-Service kan eenvoudig worden geïntegreerd met tal van frameworks en tools biedt ondersteuning voor Git en FTP-implementatie en biedt naadloze integratie met hulpprogramma's en services, zoals Visual Studio en SQL-Database. U kunt met de App-Service:

- Werken met opdrachtregeltoepassingen voor [geautomatiseerde taken][scripting].
- Werken met populaire talen als [.net][dotnet], [PHP][], [Node.js][nodejs], en [Python][].
- Selecteer drie verschillende schalen niveaus voor verticaal schalen met zeer hoge capaciteit.
- Integreren met andere Azure diensten, zoals [SQL-Database][sqldatabase], [Service Bus] [ servicebus] en [opslag][]- of partner aanbiedingen uit de [Winkel Azure][azurestore], zoals MySQL en MongoDB.
- Integreren met hulpprogramma's zoals Visual Studio, Git, WebMatrix, WebDeploy, TFS en FTP.

### <a id="multitier"></a>Ik ben mijn toepassing meerdere lagen met een web-front-migreren naar de Cloud

Als u werkt met een toepassing met meerdere niveaus, zoals een webserver die is verbonden met een database, is Azure App Service een goede optie biedt naadloze integratie met Azure SQL-Database. En u kunt de functie WebJobs gebruiken voor back-end-processen.

Kies voor een of meer van de niveaus Fabric-Service als u moet meer controle over de server-omgeving, zoals de mogelijkheid om externe in uw server of server opstarttaken configureren.

Kies voor een of meer van de niveaus virtuele Machines als u wilt dat de afbeelding van uw eigen computer of Serversoftware of services die u niet op Fabric-Service configureren.

### <a id="custom"></a>Mijn toepassing hangt in hoge mate aangepaste Windows- of Linux-omgevingen en deze wilt verplaatsen naar de cloud.

Als uw toepassing vereist complexe installatie of configuratie van de software en het besturingssysteem, virtuele Machines is het waarschijnlijk de beste oplossing. U kunt met virtuele Machines:

- Gebruik de galerie met virtuele Machine te starten met een besturingssysteem, zoals Windows of Linux, en vervolgens voor de toepassingsvereisten van uw aanpassen.
- Maken en uploaden van een aangepaste installatiekopie van een bestaande server op locatie uit te voeren op een virtuele machine in Azure.

### <a id="oss"></a>Mijn site maakt gebruik van open-sourcesoftware, en ik wil deze host in Azure

Als de open-source-framework wordt ondersteund op App-Service, de talen en kaders die nodig is voor uw toepassing automatisch voor u geconfigureerd. App-Service kunt u:

- Veel populaire open source talen, zoals [.NET]gebruiken[dotnet], [PHP][], [Node.js][nodejs], en [Python][].
- WordPress, Drupal, Umbraco, DNN en vele andere webtoepassingen van derden instellen.
- Migreren van een bestaande toepassing of maakt u een nieuwe uit de galerie van toepassing.

Als de open-source-framework van App-Service niet wordt ondersteund, kunt u deze uitvoeren op een van de andere Azure webhosting-opties. Met virtuele Machines, installeert en configureert u de software op de computerinstallatiekopie van Windows worden kunt of Linux-gebaseerde.

### <a id="lob"></a>Ik heb een bedrijfs toepassing die verbinding met het bedrijfsnetwerk.

Als u een bedrijfs toepassing maken wilt, nodig uw website rechtstreeks toegang tot de gegevens op het bedrijfsnetwerk of services. Dit is mogelijk op de App-Service, Service, stof en virtuele Machines met de [Azure Virtual Network service](/services/virtual-network/). App-service kunt u de [functie voor integratie met VNET](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/), waarmee uw Azure toepassingen uit te voeren alsof ze op uw bedrijfsnetwerk.

### <a id="mobile"></a>Ik wil voor het hosten van een REST API of de webservice voor mobiele clients

HTTP-gebaseerde webservices kunnen u een groot aantal clients, met inbegrip van mobiele clients ondersteunen. Frameworks zoals ASP.NET Web API integratie met Visual Studio gemakkelijker te maken en de overige services in beslag nemen.  Deze services zijn blootgesteld aan een eindpunt web zodat het mogelijk is een webhosting-techniek op Azure gebruiken ter ondersteuning van dit scenario is. App-Service is echter een heel goede keus voor het hosten van REST API's. U kunt met de App-Service:

- [Mobiele app](../app-service-mobile/app-service-mobile-value-prop.md) of [API app](../app-service-api/app-service-api-apps-why-best-platform.md) als host voor het HTTP-webservice op een van de wereldwijd verspreide datacentra van Azure snel maken.
- Migreren van bestaande services of een nieuwe maken.
- SLA bereiken voor beschikbaarheid met een enkel exemplaar of meerdere speciale machines schaalt.
- Gebruik de website REST API's voor HTTP-clients, met inbegrip van mobiele clients.

> [AZURE.NOTE]
> Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account, Ga naar <a href="https://trywebsites.azurewebsites.net/">https://trywebsites.azurewebsites.net</a>, waar u kunt onmiddellijk maken een app tijdelijk starter in Azure App Service gratis. Geen creditcard vereist, geen verplichtingen.

## <a id="nextsteps"></a>Volgende stappen

Voor meer informatie over de drie web hosting opties, Zie [Kennismaking met Azure](../fundamentals-introduction-to-azure.md).

Om te beginnen met de optie (s) die u voor uw toepassing kiest, Zie de volgende bronnen:

* [Azure App-Service](/documentation/services/app-service/)
* [Azure Cloud Services](/documentation/services/cloud-services/)
* [Azure virtuele Machines](/documentation/services/virtual-machines/)
* [Service-Fabric](/documentation/services/service-fabric)

<!-- URL List -->

[Azure App-Service]: /services/app-service/
[Cloud Services]: http://go.microsoft.com/fwlink/?LinkId=306052
[Virtuele Machines]: http://go.microsoft.com/fwlink/?LinkID=306053
[Service-Fabric]: /services/service-fabric
[ClearDB]: http://www.cleardb.com/
[WebJobs]: http://go.microsoft.com/fwlink/?linkid=390226&clcid=0x409
[Configuring an SSL certificate for an Azure Website]: http://www.windowsazure.com/develop/net/common-tasks/enable-ssl-web-site/
[azurestore]: http://www.windowsazure.com/gallery/store/
[scripting]: http://www.windowsazure.com/documentation/scripts/?services=web-sites
[dotnet]: http://www.windowsazure.com/develop/net/
[nodejs]: http://www.windowsazure.com/develop/nodejs/
[PHP]: http://www.windowsazure.com/develop/php/
[Python]: http://www.windowsazure.com/develop/python/
[servicebus]: http://www.windowsazure.com/documentation/services/service-bus/
[sqldatabase]: http://www.windowsazure.com/documentation/services/sql-database/
[Opslag]: http://www.windowsazure.com/documentation/services/storage/

<!-- IMG List -->

[ChoicesDiagram]: ./media/choose-web-site-cloud-service-vm/Websites_CloudServices_VMs_3.png
