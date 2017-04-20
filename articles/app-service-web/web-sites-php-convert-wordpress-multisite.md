<properties 
    pageTitle="WordPress converteren naar meerdere locaties in Azure App-Service" 
    description="Meer informatie over het maken van een bestaand WordPress web app gemaakt door de galerie in Azure en converteren naar WordPress meerdere locaties" 
    services="app-service\web" 
    documentationCenter="php" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>



# <a name="convert-wordpress-to-multisite-in-azure-app-service"></a>WordPress converteren naar meerdere locaties in Azure App-Service

## <a name="overview"></a>Overzicht

*Door [Ben Lobaugh][ben-lobaugh], [Microsoft Open Technologies Inc.][ms-open-tech]*

In deze zelfstudie leert u hoe u een bestaande WordPress web app gemaakt door de galerie in Azure nemen en deze omzetten in een installatie met meerdere locaties WordPress. Daarnaast leert u hoe u een aangepast domein toewijzen aan elk van de subsites binnen de installatie.

Er wordt verondersteld dat u beschikt over een bestaande installatie van WordPress. Volg de richtlijnen van de [website van de galerie in Azure WordPress maken]als u dit niet doet,[website-from-gallery].

Converteren van een bestaande WordPress één site installeren op meerdere locaties is meestal vrij eenvoudig, en veel van de eerste stappen die hier worden geleverd rechtstreeks vanaf het [Maken van een netwerk] [ wordpress-codex-create-a-network] pagina op de [WordPress Codex](http://codex.wordpress.org).

We gaan aan de slag.

## <a name="allow-multisite"></a>Toestaan dat meerdere locaties

Eerst moet u inschakelen meerdere locaties via de `wp-config.php` het bestand met de **WO\_toestaan\_meerdere locaties** constant. Er zijn twee methoden voor het bewerken van de bestanden van uw web app: de eerste is via FTP, en de tweede tot en met Git. Als u onbekend met een van deze methoden instellen bent, raadpleegt u de volgende cursussen:

* [Website van PHP met MySQL en FTP][website-w-mysql-and-ftp-ftp-setup]

* [Website van PHP met MySQL en Git][website-w-mysql-and-git-git-setup]

Open de `wp-config.php` het bestand met de editor van uw keuze en voeg de volgende boven de `/* That's all, stop editing! Happy blogging. */` lijn.

    /* Multisite */

    define( 'WP_ALLOW_MULTISITE', true );

Zorg ervoor dat u het bestand opslaan en deze uploaden naar de server!

## <a name="network-setup"></a>Netwerk instellen

Meld u in het *wp-admin* gebied op uw web app en u ziet een nieuw item in het menu **Extra** **Netwerk**genoemd. Klik op **Netwerk** en vul de details van uw netwerk.

![Netwerk Setup-scherm][wordpress-network-setup]

In deze zelfstudie wordt het schema van de site *submappen* omdat we om de aangepaste domeinen voor elke subsite verderop in de zelfstudie instellen en het altijd werkt. Echter, moet het mogelijk zijn om setup een subdomein te installeren als u een domein via het jokerteken [Azure Portal](https://portal.azure.com) en setup DNS correct worden toegewezen.

Voor meer informatie over het onderliggende domein vs submap instellingen overzicht van de [typen meerdere locaties netwerk] [ wordpress-codex-types-of-networks] -artikel op de WordPress Codex.

## <a name="enable-the-network"></a>Het netwerk inschakelen

Het netwerk is nu geconfigureerd in de database, maar er is één van de resterende stap waarmee de netwerkfunctionaliteit. Voltooien het `wp-config.php` instellingen en `web.config` elke site goed gerouteerd.


Wanneer u op de knop **installeren** op de pagina *Netwerkinstellingen* , WordPress zal proberen om te werken de `wp-config.php` en `web.config` bestanden. U moet echter altijd de bestanden, zodat de updates zijn geslaagd controleren. Als dit niet het geval is, wordt dit scherm u de vereiste updates worden aangeboden. Bewerken en opslaan van de bestanden.


Na het maken back-deze updates u moet Meld u af en meld u op de wp admin dashboard.

Er worden nu een extra menu op admin bar gelabelde **Mijn Sites**. In dit menu kunt u uw nieuwe netwerk via het **Netwerk Admin** dashboard.

## <a name="adding-custom-domains"></a>Aangepaste domeinen toevoegen

De [Toewijzing van WordPress MU domein] [ wordpress-plugin-wordpress-mu-domain-mapping] plugin kunt u probleemloos aangepaste domeinen toevoegen aan een site in uw netwerk. Voor de invoegtoepassing werkt moet, u extra instellingen opgeven op de Portal en op uw domeinregistratieservice doen.

## <a name="enable-domain-mapping-to-the-web-app"></a>Domein toewijzen aan de web app.

De **vrije** [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) plan modus biedt geen ondersteuning voor het toevoegen van aangepaste domeinen aan Web Apps. U wilt overschakelen naar de modus **gedeeld** of **standaard** . Om dit te doen:

* Log in op de Portal Azure en zoek uw web app. 
* Klik op het tabblad **schaal omhoog** in de **Instellingen**.
* Schakel onder **Algemeen** *gedeeld* of *STANDARD*
* Klik op **Opslaan**

U wordt gevraagd om te controleren of de wijziging en erkent dat uw web app kan nu kosten in rekening gebracht een, afhankelijk van het gebruik en de andere configuratie die u instelt.

Het duurt een paar seconden voor het verwerken van de nieuwe instellingen is nu een goed moment om te starten, het instellen van uw domein.

## <a name="verify-your-domain"></a>Controleer of uw domein

Voordat de Azure Web Apps kunt u een domein worden toegewezen aan de site, moet u eerst controleren of u de vergunning voor het toewijzen van het domein. Hiervoor moet u een nieuwe CNAME-record toevoegen aan de DNS-vermelding.

* Aanmelden bij uw domein van DNS-beheer
* Maak een nieuwe CNAME- *awverify*
* *Awverify* *awverify wijst. YOUR_DOMAIN.azurewebsites.NET*

Het kan even duren voor de DNS-wijzigingen in volledige kracht, dus gaat u als volgt werken niet direct, koffie, maken en vervolgens komen terug en probeer het opnieuw.

## <a name="add-the-domain-to-the-web-app"></a>Het domein toevoegen aan de web app.

Terug naar uw web app via de Portal Azure, klikt u op **Instellingen**en klik op **Custom domains en SSL**.

Als de *SSL-instellingen* wordt weergegeven, ziet u de velden waarin u de domeinen die u wilt toewijzen aan uw web app ingevoerd. Als een domein hier niet wordt vermeld, is het niet mogelijk voor toewijzing in WordPress, ongeacht hoe het DNS-domein ingesteld is.

![Dialoogvenster aangepaste domeinen beheren][wordpress-manage-domains]

Nadat u uw domein in het tekstvak te typen, Azure controleert of de CNAME-record die u eerder hebt gemaakt. Als de DNS-server is niet volledig wordt doorgegeven, wordt een rode indicator weergegeven. Als het is gelukt, ziet u een groen vinkje. 

Noteer het IP-adres weergegeven onder aan het dialoogvenster. U moet dit voor het instellen van de A-record voor uw domein.

## <a name="setup-the-domain-a-record"></a>Een record van het domein instellen

Als u de overige stappen zijn voltooid, kunt u het domein kan nu toewijzen aan uw Azure web app via een DNS-A-record. 

Het is belangrijk om te weten, dat Azure web apps zowel CNAME en A-records accepteert, maar u *moet* een A-record gebruiken om het juiste domein toewijzen. Een CNAME kan niet worden doorgestuurd naar een andere CNAME, dat is wat Azure met YOUR_DOMAIN.azurewebsites.net voor u gemaakt.

Met behulp van het IP-adres uit de vorige stap terug te keren naar de DNS-beheer en de A-record aan te wijzen op dat IP-adres instellen.


## <a name="install-and-setup-the-plugin"></a>Installeren en instellen van de invoegtoepassing

WordPress meerdere locaties momenteel geen ingebouwde methode voor het toewijzen van aangepaste domeinen. Er is echter een plugin genaamd [WordPress MU domein toewijzing] [ wordpress-plugin-wordpress-mu-domain-mapping] waarmee u de functionaliteit worden toegevoegd. Aanmelden bij het netwerk Admin gedeelte van uw website en installeer de plugin **WordPress MU domein toewijzen** .

Na het installeren en activeren van de plugin, Ga naar **Instellingen** > **Domein toewijzen** voor het configureren van de invoegtoepassing. Invoer in het eerste tekstvak, *IP-adres van de Server*, het IP-adres dat u gebruikt voor het instellen van de A-record voor het domein. Stel de gewenste *Opties voor domein* (de standaardwaarden zijn vaak fijn) en klik op **Opslaan**.

## <a name="map-the-domain"></a>Plattegrond van het domein

Ga naar het **Dashboard** voor de site die u wilt het domein dat u wilt toewijzen. Klik op **Extra** > **Domein toewijzen** en typt u het nieuwe domein in het tekstvak en klik op **toevoegen**.

Standaard wordt het nieuwe domein het domein automatisch gegenereerde site worden herschreven. Als u dat alle verkeer dat wordt verzonden naar het nieuwe domein wilt, het selectievakje *primaire domeincontroller voor dit blog* voordat u opslaat. U kunt een onbeperkt aantal domeinen toevoegen aan een site, maar slechts één primaire kan worden.

## <a name="do-it-again"></a>Dit opnieuw doen

Azure Web Apps kunt u een onbeperkt aantal domeinen toevoegen aan een web app. Als u wilt toevoegen van een ander domein moet u de secties **uw domein verifiëren** en **een record van het domein instellen** voor elk domein worden uitgevoerd.  

>[AZURE.NOTE] Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een starter tijdelijk web app in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.

## <a name="whats-changed"></a>Wat er veranderd
* Zie voor een handleiding voor het wijzigen van Websites met App-Service: [Azure App Service en de Impact op de bestaande Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

[ben-lobaugh]: http://ben.lobaugh.net
[ms-open-tech]: http://msopentech.com
[website-from-gallery]: https://www.windowsazure.com/develop/php/tutorials/website-from-gallery/
[wordpress-codex-create-a-network]: http://codex.wordpress.org/Create_A_Network
[website-w-mysql-and-ftp-ftp-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-ftp/#header-0
[website-w-mysql-and-git-git-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-git/#header-1
[wordpress-network-setup]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-network-setup.png
[wordpress-codex-types-of-networks]: http://codex.wordpress.org/Before_You_Create_A_Network#Types_of_multisite_network
[wordpress-plugin-wordpress-mu-domain-mapping]: http://wordpress.org/extend/plugins/wordpress-mu-domain-mapping/

[wordpress-manage-domains]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-manage-domains.png

 
