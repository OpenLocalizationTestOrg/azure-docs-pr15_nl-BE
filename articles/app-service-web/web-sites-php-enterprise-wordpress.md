<properties
    pageTitle="Bedrijfsniveau WordPress Azure App service | Microsoft Azure"
    description="Meer informatie over het hosten van een enterprise-klasse WordPress-site in Azure App Service"
    services="app-service\web"
    documentationCenter=""
    authors="sunbuild"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.devlang="php"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="web"
    ms.date="10/24/2016"
    ms.author="sumuth"/>

# <a name="enterprise-class-wordpress-on-azure-app-service"></a>Bedrijfsniveau WordPress Azure App-service

Azure App-Service biedt een schaalbare, veilige en gebruiksvriendelijke omgeving voor mission critical, grote schaal [WordPress] [ wordpress] sites. Microsoft zelf sites zoals [Office] enterprise-klasse wordt uitgevoerd[ officeblog] en [Bing] [ bingblog] blogs. Dit document beschrijft hoe u kunt Azure App Service Web Apps opzetten en onderhouden van een enterprise-klasse, een cloud-gebaseerde WordPress-site die een groot aantal bezoekers kan verwerken.

## <a name="architecture-and-planning"></a>Planning en architectuur

Een eenvoudige installatie van WordPress heeft slechts twee vereisten.

* **MySQL-Database** - beschikbaar via [ClearDB op de markt Azure][cdbnstore], of u kunt uw eigen MySQL installatie op Azure virtuele Machines met behulp van [Windows] beheren[ mysqlwindows] of [Linux][mysqllinux].

  > [AZURE.NOTE] ClearDB biedt verschillende MySQL-configuraties, met verschillende prestatie-eigenschappen voor elke configuratie. Zie de [Azure winkel] [ cdbnstore] voor meer informatie over het aanbod via de winkel Azure of direct zichtbaar op [de website ClearDB](http://www.cleardb.com/pricing.view).

* **PHP 5.2.4 groter of** -Azure App Service bieden momenteel [PHP versie 5.4, 5.5 en 5.6][phpwebsite].

    > [AZURE.NOTE] Het is raadzaam altijd op de meest recente versie van PHP, zodat u de meest recente beveiligingsupdates uitgevoerd.

### <a name="basic-deployment"></a>Eenvoudige implementatie

Met alleen de essentiële eisen, kunt u een eenvoudige oplossing in een gebied op de Azure.

![een Azure web app en de MySQL-Database wordt gehost in een bepaalde regio Azure][basic-diagram]

Terwijl op deze manier kunt u naar de scale-out uw toepassing door meerdere Web Apps exemplaren van de site te maken, wordt alles in het midden van de gegevens in een bepaalde geografische regio gehost. Bezoekers van buiten deze regio ziet trage responstijden bij het gebruik van de site, en als de datacenters in dit gebied, zo wordt uw toepassing.


### <a name="multi-region-deployment"></a>Implementatie in meerdere landen/regio

Met Azure [Verkeer Manager][trafficmanager], kunt u uw WordPress site schaal tussen de verschillende geografische regio's terwijl slechts één URL voor bezoekers. Alle bezoekers binnenkomen via beheer van netwerkverkeer en worden vervolgens doorgestuurd naar een gebied op basis van de configuratie van load-balancing.

![een Azure web app, gehost in meerdere regio's met hoge beschikbaarheid CDBR router routeren naar MySQL tussen de regio's.][multi-region-diagram]

Binnen elk gebied, de WordPress site zou nog steeds worden geschaald over meerdere exemplaren van het Web Apps, maar deze schaal is regio-specifieke; hoge verkeer gebieden kunnen anders dan weinig verkeer die worden geschaald.

Replicatie en routering naar meerdere MySQL Databases kunnen worden uitgevoerd met behulp van ClearDB, [Hoge beschikbaarheid-Router CDBR] [ cleardbscale] (Zie links) of [MySQL Cluster CGE][cge].

### <a name="multi-region-deployment-with-media-storage-and-caching"></a>Implementatie van meerdere regio met Mediaopslag en caching
Als de site uploads of host mediabestanden, Azure Blob-opslag gebruiken accepteert. Als u opslaan in de cache moet, kunt u het [bestand Vgx. cache][rediscache], [Memcache wolk](http://azure.microsoft.com/gallery/store/garantiadata/memcached/), [MemCachier](http://azure.microsoft.com/gallery/store/memcachier/memcachier/)of een van de cache aanbiedingen in de [Winkel Azure](http://azure.microsoft.com/gallery/store/).

![een Azure web app, die zijn ondergebracht in verschillende regio's met hoge beschikbaarheid CDBR router voor MySQL met Cache beheerd, Blob-opslag en CDN.][performance-diagram]

BLOB-opslag is geo verdeeld tussen de regio's standaard ingeschakeld, zodat u hoeft niet bang te zijn voor het repliceren van bestanden voor alle locaties. Ook kunt u de Azure [Content Distribution Network (CDN)] [ cdn] voor Blob-opslag die bestanden worden geïnstalleerd door knooppunten die dichter bij uw bezoekers te beëindigen.

### <a name="planning"></a>Planning

#### <a name="additional-requirements"></a>Aanvullende vereisten

Om dit te doen... | Gebruik dit...
------------------------|-----------
**Uploaden of grote bestanden opslaan** | [WordPress plugin voor het gebruik van Blob-opslag][storageplugin]
**E-mail verzenden** | [SendGrid] [ storesendgrid] en de [WordPress plugin voor het gebruik van SendGrid][sendgridplugin]
**Aangepaste domeinnamen** | [Een aangepaste domeinnaam in Azure App-Service configureren][customdomain]
**HTTPS** | [HTTPS voor een web app in Azure App-Service inschakelen][httpscustomdomain]
**In preproductie validatie** | [Staging-omgevingen voor web apps in Azure App-Service instellen][staging] <p>Schakelen tussen een web app van ontwikkeling naar productie ook de configuratie van WordPress verplaatst. U moet ervoor zorgen dat alle instellingen aan de vereisten voor uw app productie worden bijgewerkt voordat u overschakelt van de gefaseerde app in productie.</p>
**Controle en probleemoplossing** | [Diagnostische logboekregistratie inschakelen voor web apps in Azure App Service] [ log] en [Monitor Web Apps in Azure App-Service][monitor]
**Implementatie van uw site** | [Een web app in Azure App Service implementeren][deploy]

#### <a name="availability-and-disaster-recovery"></a>Beschikbaarheid en calamiteitenherstel

Om dit te doen... | Gebruik dit...
------------------------|-----------
**Belasting saldo sites** of **geo-sites distribueren** | [Routeren van netwerkverkeer met Azure verkeer Manager][trafficmanager]
**Back-up en terugzetten** | [Back-up van een web app in Azure App Service] [ backup] en het [herstellen van een web app in Azure App-Service.][restore]

#### <a name="performance"></a>Prestaties

Prestaties in de cloud wordt bereikt, voornamelijk door middel van caching en scale-out; maar het geheugen, bandbreedte en andere kenmerken van Web Apps hosting overwogen worden.

Om dit te doen... | Gebruik dit...
------------------------|-----------
**App-exemplaar mogelijkheden begrijpen** | [Informatie, zoals de mogelijkheden van de App Service tiers prijzen][websitepricing]
**Bronnen voor de systeemcache** | [Cache bestand Vgx.][rediscache], [Memcache wolk](/gallery/store/garantiadata/memcached/), [MemCachier](/gallery/store/memcachier/memcachier/)of een van de cache aanbiedingen in de [Winkel Azure](/gallery/store/)
**Schalen van uw toepassing** | [Schaal van een web app in Azure App Service] [ websitescale] [ClearDB hoge beschikbaarheid routering]en[cleardbscale]. Als u ervoor kiest te hosten en beheren van uw eigen MySQL-installatie, kunt u overwegen [MySQL Cluster CGE] [ cge] voor schalen

#### <a name="migration"></a>Migratie

Er zijn twee methoden voor het migreren van een bestaande WordPress site naar Azure App-Service.

* ** [WordPress exporteren] [ export] ** -zo exporteert u de inhoud van uw blog, kan vervolgens worden geïmporteerd in een nieuwe WordPress-site in Azure App Service met behulp van de [plugin voor WordPress-importeur][import].

    > [AZURE.NOTE] Tijdens dit proces u de inhoud migreert kunt, worden dus niet gemigreerd alle plugins, thema's of andere aanpassingen. Deze moeten handmatig opnieuw worden geïnstalleerd.

* **Handmatige migratie** - [Back-up van uw site] [ wordpressbackup] en de [database][wordpressdbbackup], vervolgens handmatig terugzet op een web app in Azure App Service en om uiterst aangepaste sites migreren en verspillen van het handmatig installeren van plugins, thema's en andere aanpassingen te voorkomen dat MySQL-database is gekoppeld.

## <a name="step-by-step-instructions"></a>Stapsgewijze instructies

### <a name="create-a-wordpress-site"></a>Een WordPress-site maken

1. Gebruik de [Azure Marketplace] [ cdbnstore] voor het maken van een MySQL-database van de grootte die u in de sectie [planning en architectuur](#planning) in de regio('s) dat u uw site host geïdentificeerd.

2. Volg de stappen in het [maken van een WordPress web app in Azure App Service] [ createwordpress] voor het maken van een WordPress web app. Bij het maken van de web app **gebruik van een MySQL-Database** selecteren en selecteert u de database hebt gemaakt in stap 1.

Als u een bestaande WordPress site migreert, Zie [migreren Azure een bestaande WordPress site](#Migrate-an-existing-WordPress-site-to-Azure) na het maken van een nieuwe web app.

### <a name="migrate-an-existing-wordpress-site-to-azure"></a>Een bestaande WordPress site migreren naar Azure

Zoals vermeld in de sectie [planning en architectuur](#planning) , zijn er twee manieren om te migreren van een WordPress site.

* **importeren en exporteren** - voor sites zonder veel aanpassingen, of alleen waar u de inhoud verplaatsen.

* **back-up en terugzetten** - voor sites met veel aanpassingen waar u alles verplaatsen.

Gebruik een van de volgende secties voor het migreren van uw site.

#### <a name="the-export-and-import-method"></a>De methode voor importeren en exporteren

1. Gebruik [WordPress exporteren] [ export] voor het exporteren van uw bestaande site.

2. Maak een web app met de stappen in de sectie [een WordPress-site maken](#Create-a-new-WordPress-site) .

3. Log in op uw WordPress site op het Web Apps en **Plugins**op -> **Nieuwe toevoegen**. Zoeken en installeren, de **Importeur van WordPress** plugin.

4. Nadat de importeur-invoegtoepassing is geïnstalleerd, klikt u op **Extra** -> **importeren**en selecteer vervolgens **WordPress** de importeur WordPress plugin gebruiken.

5. Klik op **Bestand kiezen**op de pagina **Importeren van WordPress** . Blader naar het bestand WXR is geëxporteerd vanuit uw bestaande WordPress site en kies vervolgens **bestand uploaden en importeren**.

6. Klik op **indienen**. U wordt gevraagd of u het importeren is voltooid.

8. Als u al deze stappen hebt voltooid, start opnieuw op de site van de web app blade in [Azure portal][mgmtportal].

Na het importeren van de site, moet u de volgende stappen als u geen instellingen in het importbestand.

Als u dit... | Dit doen...
------------------ | ----------
**Permalinks** | Uit het dashboard WordPress van de nieuwe site, klikt u op **Instellingen** -> **Permalinks** en vervolgens update de structuur van Permalinks
**Image/media koppelingen** | Als koppelingen wilt bijwerken naar de nieuwe locatie, gebruikt u de [invoegtoepassing Velvet Blues bijwerken URL's][velvet], een hulpprogramma voor zoeken en vervangen, of handmatig in uw database
**Thema 's** | Ga naar de **weergave** -> **thema** en het thema van de site zo nodig bijwerken
**Menu 's** | Als uw thema menu's ondersteunt, kan koppelingen naar de startpagina van de oude onderliggende map ingesloten in deze nog steeds. Ga naar de **weergave** -> **menu's** en werk deze bij

#### <a name="the-backup-and-restore-method"></a>De methode back-up en terugzetten

1. Back-up van uw bestaande WordPress site met de gegevens op de [back-ups van WordPress][wordpressbackup].

2. Back-up van uw bestaande database die de informatie bij het [maken van back-up van de database]met[wordpressdbbackup].

3. Een database maken en terugzetten van de back-up.

    1. Een nieuwe database van [Azure Marketplace]kopen[cdbnstore], of een MySQL-database op een [Windows] setup-[ mysqlwindows] of [Linux] [ mysqllinux] VM.

    2. Met behulp van een MySQL-client zoals [MySQL Workbench][workbench], verbinding maken met de nieuwe database en uw WordPress database importeren.

    3. Bijwerken van de database als u de posten van het domein wilt uw nieuwe Azure App-domein. Bijvoorbeeld mywordpress.azurewebsites.net. [Zoeken en vervangen voor WordPress Databases Script] [ searchandreplace] veilig alle exemplaren wijzigen.

4. Een web app in Azure portal maken en publiceren van de back-up van WordPress.

    1. Een webtoepassing maken in de [Azure portal] [ mgmtportal] met een database met behulp van **nieuwe** -> **Web + Mobile** -> **Marketplace Azure** -> **Web Apps** -> -> **Create****Web app + SQL** (of **Web app + MySQL**). De vereiste instellingen als u wilt maken van een leeg web app configureren.

    2. Zoek het bestand **wp-config.php** en open het in een editor in uw reservekopie WordPress. De volgende items vervangen door de gegevens voor uw nieuwe MySQL-database.

        * **DB_NAME** - de naam van de database

        * **DB_USER** - de gebruikersnaam die wordt gebruikt voor toegang tot de database

        * **DB_PASSWORD** - wachtwoord van de gebruiker

        Na het wijzigen van deze vermeldingen opslaan en sluit het bestand **wp-config.php** .

    3. Het [implementeren van een web app in Azure App-Service] gebruiken[ deploy] informatie om de implementatiemethode die u wilt gebruiken en vervolgens implementeren back-up van uw WordPress op uw web app in Azure App-Service.

5. Zodra de site WordPress is geïmplementeerd, moet u toegang kunnen krijgen tot de nieuwe site (als een App Service web app) met behulp van de *. azurewebsite.net-URL voor de site.

### <a name="configure-your-site"></a>Uw site configureren

Na de WordPress-site is gemaakt of gemigreerd, gebruik de volgende gegevens te verbeteren of extra functionaliteit inschakelen.

Om dit te doen... | Gebruik dit...
------------- | -----------
**App Service plan modus, de grootte en schalen inschakelen instellen** | [Schaal van een web app in Azure App-Service][websitescale]
**Persistent databaseverbindingen inschakelen** | Standaard WordPress gebruikt geen persistent databaseverbindingen, waardoor de verbinding met de database na meerdere verbindingen worden verminderd. Als u permanente verbindingen installeert de (permanente verbindingen adapter plugin) [https://wordpress.org/plugins/persistent-database-connection-updater/installation/]. 
**De prestaties verbeteren** | <ul><li><p><a href="http://ppe.blogs.msdn.com/b/windowsazure/archive/2013/11/18/disabling-arr-s-instance-affinity-in-windows-azure-web-sites.aspx">Uitschakelen van de cookie ARR</a> - kan de prestaties verbeteren wanneer WordPress uitgevoerd op meerdere exemplaren van het Web Apps</p></li><li><p>Caching inschakelen. <a href="http://msdn.microsoft.com/library/azure/dn690470.aspx">Cache bestand Vgx.</a> (voorbeeld) kan worden gebruikt met de <a href="https://wordpress.org/plugins/redis-object-cache/">object cache WordPress plugin bestand Vgx.</a>, of gebruik een van de cache aanbiedingen uit de <a href="/gallery/store/">Winkel Azure</a></p></li><li><p><a href="http://ruslany.net/2010/03/make-wordpress-faster-on-iis-with-wincache-1-1/">WordPress sneller te maken met de Wincache</a> - Wincache is standaard ingeschakeld voor Web Apps</p></li><li><p><a href="../web-sites-scale/">Schaal van een web app in Azure App Service</a> en het gebruik van <a href="http://www.cleardb.com/developers/cdbr/introduction">Routering ClearDB hoge beschikbaarheid</a> of <a href="http://www.mysql.com/products/cluster/">MySQL Cluster CGE</a></p></li></ul>
**BLOB's gebruiken voor opslag** | <ol><li><p><a href="../storage-create-storage-account/">Maak een account Azure opslag</a></p></li><li><p>Informatie over het gebruik van <a href="../cdn-how-to-use/">De Content Distribution Network (CDN)</a> geo-gegevens opgeslagen in de BLOB's distribueren.</p></li><li><p>Installeer en configureer de <a href="https://wordpress.org/plugins/windows-azure-storage/">Azure opslag voor WordPress plugin</a>.</p><p>Raadpleeg de <a href="http://plugins.svn.wordpress.org/windows-azure-storage/trunk/UserGuide.docx">gebruikershandleiding</a>voor gedetailleerde installatie- en configuratie-informatie voor de invoegtoepassing.</p> </li></ol>
**E-mail inschakelen** | <a href="https://azure.microsoft.com/en-us/marketplace/partners/sendgrid/sendgrid-azure/">SendGrid</a> met behulp van de winkel Azure inschakelen. De <a href="http://wordpress.org/plugins/sendgrid-email-delivery-simplified">SendGrid plugin</a> voor WordPress installeren.
**Een aangepaste domeinnaam configureren** | [Een aangepaste domeinnaam in Azure App-Service configureren][customdomain]
**HTTPS inschakelen voor een aangepaste domeinnaam** | [HTTPS voor een web app in Azure App-Service inschakelen][httpscustomdomain]
**Laden van saldo of geo-distribueren van uw site** | [Routeren van verkeer met Azure verkeer Manager][trafficmanager]. Als u een aangepast domein, [een aangepaste domeinnaam in Azure App-Service configureren] Zie[ customdomain] voor meer informatie over het gebruik van beheer van netwerkverkeer met aangepaste domeinnamen
**Automatische back-ups** | [Back-up van een web app in Azure App-Service.][backup]
**Diagnostische logboekregistratie inschakelen** | [Diagnostische logboekregistratie inschakelen voor web apps in Azure App-Service][log]

## <a name="next-steps"></a>Volgende stappen

* [WordPress optimalisatie](http://codex.wordpress.org/WordPress_Optimization)

* [WordPress converteren naar meerdere locaties in Azure App-Service](web-sites-php-convert-wordpress-multisite.md)

* [ClearDB upgrade van de wizard voor Azure](http://www.cleardb.com/store/azure/upgrade)

* [Hosting WordPress in een submap van uw web app in Azure App-Service.](http://blogs.msdn.com/b/webapps/archive/2013/02/13/hosting-wordpress-in-a-subfolder-of-your-windows-azure-web-site.aspx)

* [Stapsgewijze: Een WordPress site maken met Azure](http://blogs.technet.com/b/blainbar/archive/2013/08/07/article-create-a-wordpress-site-using-windows-azure-read-on.aspx)

* [Host voor uw bestaande WordPress blog op Azure](http://blogs.msdn.com/b/msgulfcommunity/archive/2013/08/26/migrating-a-self-hosted-wordpress-blog-to-windows-azure.aspx)

* [Mooie permalinks in WordPress inschakelen](http://www.iis.net/learn/extensions/url-rewrite-module/enabling-pretty-permalinks-in-wordpress)

* [Migreren en uitvoeren van uw WordPress blog op Azure App Service](http://www.kefalidis.me/2012/06/how-to-migrate-and-run-your-wordpress-blog-on-windows-azure-websites/)

* [WordPress gratis op Azure App Service uitvoeren](http://architects.dzone.com/articles/how-run-wordpress-azure)

* [WordPress op Azure in 2 minuten of minder](http://www.sitepoint.com/wordpress-windows-azure-2-minutes-less/)

* [Verplaatsen van een WordPress blog naar Azure - deel 1: het maken van een WordPress blog op Azure](http://www.davebost.com/2013/07/10/moving-a-wordpress-blog-to-windows-azure-part-1)

* [Verplaatsen van een WordPress blog naar Azure - deel 2: het overbrengen van de inhoud van uw](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-transferring-your-content)

* [Verplaatsen van een WordPress blog naar Azure - deel 3: het instellen van uw aangepaste domein](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-3-setting-up-your-custom-domain)

* [Verplaatsen van een WordPress blog naar Azure - deel 4: mooie permalinks en herschrijven van URL-regels](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-4-pretty-permalinks-and-url-rewrite-rules)

* [Verplaatsen van een WordPress blog naar Azure - deel 5: verplaatsen van een submap in de hoofdmap](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-5-moving-from-a-subfolder-to-the-root)

* [Het instellen van een WordPress web app in uw account Azure](http://www.itexperience.net/2014/01/20/how-to-set-up-a-wordpress-website-in-your-windows-azure-account/)

* [Propping van WordPress op Azure](http://www.johnpapa.net/wordpress-on-azure/)

* [Tips voor WordPress op Azure](http://www.johnpapa.net/azurecleardbmysql/)

>[AZURE.NOTE] Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een starter tijdelijk web app in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.

## <a name="whats-changed"></a>Wat er veranderd
* Zie voor een handleiding voor het wijzigen van Websites met App-Service: [Azure App Service en de Impact op de bestaande Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- URL List -->

[performance-diagram]: ./media/web-sites-php-enterprise-wordpress/performance-diagram.png
[basic-diagram]: ./media/web-sites-php-enterprise-wordpress/basic-diagram.png
[multi-region-diagram]: ./media/web-sites-php-enterprise-wordpress/multi-region-diagram.png
[wordpress]: http://www.microsoft.com/web/wordpress
[officeblog]: http://blogs.office.com/
[bingblog]: http://blogs.bing.com/
[cdbnstore]: http://www.cleardb.com/store/azure
[storageplugin]: https://wordpress.org/plugins/windows-azure-storage/
[sendgridplugin]: http://wordpress.org/plugins/sendgrid-email-delivery-simplified/
[phpwebsite]: web-sites-php-configure.md
[customdomain]: web-sites-custom-domain-name.md
[trafficmanager]: ../traffic-manager/traffic-manager-overview.md
[backup]: web-sites-backup.md
[restore]: web-sites-restore.md
[rediscache]: https://azure.microsoft.com/documentation/services/redis-cache/
[managedcache]: http://msdn.microsoft.com/library/azure/dn386122.aspx
[websitescale]: web-sites-scale.md
[managedcachescale]: http://msdn.microsoft.com/library/azure/dn386113.aspx
[cleardbscale]: http://www.cleardb.com/developers/cdbr/introduction
[staging]: web-sites-staged-publishing.md
[monitor]: web-sites-monitor.md
[log]: web-sites-enable-diagnostic-log.md
[httpscustomdomain]: web-sites-configure-ssl-certificate.md
[mysqlwindows]: ../virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md
[mysqllinux]: ../virtual-machines/virtual-machines-linux-classic-mysql-on-opensuse.md
[cge]: http://www.mysql.com/products/cluster/
[websitepricing]: /pricing/details/app-service/
[export]: http://en.support.wordpress.com/export/
[import]: http://wordpress.org/plugins/wordpress-importer/
[wordpressbackup]: http://wordpress.org/plugins/wordpress-importer/
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[createwordpress]: web-sites-php-web-site-gallery.md
[velvet]: https://wordpress.org/plugins/velvet-blues-update-urls/
[mgmtportal]: https://portal.azure.com/
[wordpressbackup]: http://codex.wordpress.org/WordPress_Backups
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[workbench]: http://www.mysql.com/products/workbench/
[searchandreplace]: http://interconnectit.com/124/search-and-replace-for-wordpress-databases/
[deploy]: web-sites-deploy.md
[posh]: ../powershell-install-configure.md
[Azure CLI]: ../xplat-cli-install.md
[storesendgrid]: https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/
[cdn]: ../cdn/cdn-overview.md
