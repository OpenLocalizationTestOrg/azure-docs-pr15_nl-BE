<properties
  pageTitle="Efficiënt gebruik van DevOps omgevingen voor uw web app"
  description="Informatie over het gebruik van "slots" implementatie instellen en beheren van meerdere ontwikkelomgevingen voor uw toepassing"
  services="app-service\web"
  documentationCenter=""
  authors="sunbuild"
  manager="yochayk"
  editor=""/>

<tags
  ms.service="app-service"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="web"
  ms.date="10/24/2016"
  ms.author="sumuth"/>

# <a name="use-devops-environments-effectively-for-your-web-apps"></a>Efficiënt gebruik van DevOps omgevingen voor uw web-apps

In dit artikel ziet u kunt instellen en beheren van web application implementaties voor meerdere versies van uw toepassing, zoals ontwikkeling, fasering, QA en productie. Elke versie van uw toepassing kan worden beschouwd als ontwikkelomgeving voor de specifieke behoefte van binnen het implementatieproces. QA-omgeving kan bijvoorbeeld door het team van ontwikkelaars om de kwaliteit van de toepassing te testen alvorens u de wijzigingen voor de productie worden gebruikt.
Instellen van verschillende ontwikkelomgevingen kan een uitdaging zijn als u wilt bijhouden, beheren van de middelen (compute, webtoepassing, database, cache enz.) en code implementeren in omgevingen.

## <a name="setting-up-a-non-production-environment-stagedevqa"></a>Het instellen van een niet-productieomgeving (fase, dev, QA)
Zodra er een productie web app omhoog en wordt uitgevoerd, is de volgende stap voor het maken van een niet-productieve omgeving. Om te kunnen gebruiken zorg "slots" implementatie dat in de **Standard** of **Premium** App Service plan-modus wordt uitgevoerd. Implementatie sleuven zijn daadwerkelijk live web apps met hun eigen host-namen. Inhoud en configuratie van elementen van web app kunnen tussen twee sleuven van distributie, met inbegrip van de productie-sleuf worden verwisseld. Implementatie van een sleuf voor de implementatie van de toepassing heeft de volgende voordelen:

1. U kunt web app wijzigingen in een gefaseerde installatie sleuf voor de implementatie valideren alvorens deze te verwisselen met de sleuf voor de productie.
2. Eerst implementeren van een web app in een sleuf en wisselen zij in productie zorgt ervoor dat alle exemplaren van de sleuf worden opgewarmd voordat de productie wordt geruild. Hierdoor wordt de uitvaltijd bij het implementeren van uw web app. De omleiding van verkeer is naadloos en geen aanvragen worden weggehaald door bewerkingen in de swap. Deze hele workflow worden geautomatiseerd door te [Wisselen automatisch](web-sites-staged-publishing.md#configure-auto-swap-for-your-web-app) configureren als validatie swap vooraf is niet nodig.
3. Na een swap heeft de sleuf met eerder gefaseerde web app nu de vorige productie web app. Als de wijzigingen die zijn omgezet in de sleuf van de productie zijn niet zoals u verwacht, kunt u uitvoeren met de dezelfde swap onmiddellijk als u de ' laatste bekende goede web app."terug.

Als u een gefaseerde installatie implementatie sleuf instelt, Zie [staging-omgevingen voor web apps in Azure App-Service](web-sites-staged-publishing.md). Elke omgeving moet een eigen set van middelen, bevatten voor het voorbeeld als u het web app een database gebruikt en vervolgens de productie- en tijdelijke web app moeten worden met behulp van verschillende databases. Staging ontwikkeling omgeving bronnen zoals database, de opslag of de cache voor het instellen van uw testomgeving ontwikkeling toevoegen.

## <a name="examples-of-using-multiple-development-environments"></a>Voorbeelden van het gebruik van meerdere ontwikkelomgevingen

Elk project moet volgen voor een bron codebeheer met ten minste twee omgevingen, ontwikkeling en productie-omgeving, maar wanneer met behulp van Content management systemen, toepassingskaders enz we ondervindt problemen waarin de toepassing dit scenario uit de verpakking niet ondersteunen. Dit geldt voor sommige van de gebruikte frameworks die hieronder worden beschreven. Veel vragen komen te binnen schieten bij het werken met een CMS/frameworks zoals

1. Hoe u dit uit opsplitsen in verschillende omgevingen
2. Welke bestanden kan ik wijzigen en lukt invloed framework versie updates
3. Het beheren van configuratie per omgeving
4. Het beheren van modules/plugins versie updates, updates voor kern framework versie

Er zijn vele manieren om een omgeving met meerdere voor uw project in te stellen en de onderstaande voorbeelden zijn een slechts één van deze manieren de respectieve toepassingen.

### <a name="wordpress"></a>WordPress
In deze sectie leert u het instellen van de implementatieworkflow van een met sleuven voor WordPress. WordPress net als de meeste CMS oplossingen biedt geen ondersteuning voor het werken met verschillende ontwikkelomgevingen uit de doos. Web App Service Apps heeft een aantal functies die voor het opslaan van configuratie-instellingen van buiten uw code te vereenvoudigen.

Voordat u een staging-sleuf, uw toepassingscode instellen voor de ondersteuning van meerdere omgevingen. Ondersteuning voor omgevingen met meerdere in WordPress die u nodig hebt voor het bewerken van `wp-config.php` de volgende code op uw lokale ontwikkeling web app toevoegen aan het begin van het bestand. Deze kan de toepassing de juiste configuratie op basis van de geselecteerde omgeving kiezen.

```
// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) {
// local development
 $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) || (strpos(getenv('WP_ENV'),'prod' )!== false ))
//single file for all azure development environments
 $config_file = 'config/wp-config.azure.php';
}
$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
// include the config file if it exists, otherwise WP is going to fail
require_once $path. $config_file;
```

Maak een map in de hoofdmap van web app genoemd `config` en het toevoegen van een twee bestanden: `wp-config.azure.php` en `wp-config.local.php` respectievelijk de azure en de lokale omgeving.

Kopieer de volgende in `wp-config.local.php` :

```
<?php
// MySQL settings
/** The name of the database for WordPress */

define('DB_NAME', 'yourdatabasename');

/** MySQL database username */
define('DB_USER', 'yourdbuser');

/** MySQL database password */
define('DB_PASSWORD', 'yourpassword');

/** MySQL hostname */
define('DB_HOST', 'localhost');
/**
 * For developers: WordPress debugging mode.
 * * Change this to true to enable the display of notices during development.
 * It is strongly recommended that plugin and theme developers use WP_DEBUG
 * in their development environments.
 */
define('WP_DEBUG', true);

//Security key settings
define('AUTH_KEY', 'put your unique phrase here');
define('SECURE_AUTH_KEY','put your unique phrase here');
define('LOGGED_IN_KEY','put your unique phrase here');
define('NONCE_KEY', 'put your unique phrase here');
define('AUTH_SALT', 'put your unique phrase here');
define('SECURE_AUTH_SALT', 'put your unique phrase here');
define('LOGGED_IN_SALT', 'put your unique phrase here');
define('NONCE_SALT', 'put your unique phrase here');

/**
 * WordPress Database Table prefix.
 *
 * You can have multiple installations in one database if you give each a unique
 * prefix. Only numbers, letters, and underscores please!
 */
$table_prefix = 'wp_';
```

Instellen van de bovenstaande sleutels kunt helpen voorkomen dat uw web app gehackte wordt, dus met unieke waarden. Als u de tekenreeks voor de hierboven vermelde sleutels genereren, gaat u naar de automatische generator voor het maken van nieuwe sleutels/waarden met behulp van deze [link] (https://api.wordpress.org/secret-key/1.1/salt)

Kopieer de volgende code in `wp-config.azure.php`:


``` <?php
    // MySQL settings
    /** The name of the database for WordPress */
    
    define('DB_NAME', getenv('DB_NAME'));
    
    /** MySQL database username */
    define('DB_USER', getenv('DB_USER'));
    
    /** MySQL database password */
    define('DB_PASSWORD', getenv('DB_PASSWORD'));
    
    /** MySQL hostname */
    define('DB_HOST', getenv('DB_HOST'));
    
    /**
    * For developers: WordPress debugging mode.
    *
    * Change this to true to enable the display of notices during development.
    * It is strongly recommended that plugin and theme developers use WP_DEBUG
    * in their development environments.
    * Turn on debug logging to investigate issues without displaying to end user. For WP_DEBUG_LOG to
    * do anything, WP_DEBUG must be enabled (true). WP_DEBUG_DISPLAY should be used in conjunction
    * with WP_DEBUG_LOG so that errors are not displayed on the page */
    
    */
    define('WP_DEBUG', getenv('WP_DEBUG'));
    define('WP_DEBUG_LOG', getenv('TURN_ON_DEBUG_LOG'));
    define('WP_DEBUG_DISPLAY',false);
    
    //Security key settings
    /** If you need to generate the string for security keys mentioned above, you can go the automatic generator to create new keys/values: https://api.wordpress.org/secret-key/1.1/salt **/
    define('AUTH_KEY',getenv('DB_AUTH_KEY'));
    define('SECURE_AUTH_KEY', getenv('DB_SECURE_AUTH_KEY'));
    define('LOGGED_IN_KEY', getenv('DB_LOGGED_IN_KEY'));
    define('NONCE_KEY', getenv('DB_NONCE_KEY'));
    define('AUTH_SALT', getenv('DB_AUTH_SALT'));
    define('SECURE_AUTH_SALT', getenv('DB_SECURE_AUTH_SALT'));
    define('LOGGED_IN_SALT',  getenv('DB_LOGGED_IN_SALT'));
    define('NONCE_SALT',  getenv('DB_NONCE_SALT'));
    
    /**
    * WordPress Database Table prefix.
    *
    * You can have multiple installations in one database if you give each a unique
    * prefix. Only numbers, letters, and underscores please!
    */
    $table_prefix = getenv('DB_PREFIX');
```

#### <a name="use-relative-paths"></a>Relatieve paden gebruiken
Tot slot is het configureren van de WordPress app voor het gebruik van relatieve paden. WordPress slaat de URL-gegevens in de database. Hierdoor kunt u verplaatsen van inhoud van de ene omgeving naar de andere moeilijker als nodig de database bijwerken telkens wanneer u van lokale naar het werkgebied of stage naar productieomgevingen verplaatst. Beperk het risico van de problemen die kunnen worden veroorzaakt bij het gebruik van een database, telkens wanneer u implementeren van de ene omgeving naar een ander gebruik [Root relatieve koppelingen plugin](https://wordpress.org/plugins/root-relative-urls/) die kan worden geïnstalleerd met behulp van WordPress beheerder dashboard of handmatig te downloaden vanaf [hier](https://downloads.wordpress.org/plugin/root-relative-urls.zip).


De volgende items aan uw `wp-config.php` bestand voordat het `That's all, stop editing!` Opmerking:

```

  define('WP_HOME', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_SITEURL', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_CONTENT_URL', '/wp-content');
    define('DOMAIN_CURRENT_SITE', filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
```

Activeer de plugin via de `Plugins` menu in WordPress beheerder dashboard. Sla uw instellingen permalink voor WordPress app.

#### <a name="the-final-wp-configphp-file"></a>Het uiteindelijke `wp-config.php` bestand
Eventuele updates WordPress Core heeft geen invloed op de `wp-config.php`, `wp-config.azure.php` en `wp-config.local.php` bestanden. Uiteindelijk hoe u deze `wp-config.php` bestand er als volgt

```
<?php
/**
 * The base configurations of the WordPress.
 *
 * This file has the following configurations: MySQL settings, Table Prefix,
 * Secret Keys, and ABSPATH. You can find more information by visiting
 *
 * Codex page. You can get the MySQL settings from your web host.
 *
 * This file is used by the wp-config.php creation script during the
 * installation. You don't have to use the web web app, you can just copy this file
 * to "wp-config.php" and fill in the values.
 *
 * @package WordPress
 */

// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) { // local development
  $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) ||(strpos(getenv('WP_ENV'),'prod' )!== false )){
  $config_file = 'config/wp-config.azure.php';
}


$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
  // include the config file if it exists, otherwise WP is going to fail
  require_once $path. $config_file;
}

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');


/* That's all, stop editing! Happy blogging. */

define('WP_HOME', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_SITEURL', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_CONTENT_URL', '/wp-content');
define('DOMAIN_CURRENT_SITE', $_SERVER['HTTP_HOST']);

/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
    define('ABSPATH', dirname(__FILE__). '/');

/** Sets up WordPress vars and included files. */
require_once(ABSPATH. 'wp-settings.php');
```

#### <a name="set-up-a-staging-environment"></a>Een Staging-omgeving instellen
Ervan uitgaande dat u al een WordPress web app op Azure Web, aanmelden bij de [portal Azure Management voorvertoning](http://portal.azure.com) en Ga naar uw WordPress web app. Apps als u dit niet kunt maken van de markt. Voor meer informatie, [Klik hier](web-sites-php-web-site-gallery.md).
Klik op Instellingen -> implementatie "slots" -> toevoegen om te maken van een sleuf voor de implementatie met de naam stage. Een sleuf voor de implementatie is een andere webtoepassing delen dezelfde bronnen als de primaire web app dat hierboven is gemaakt.

![Werkgebied implementatie sleuf maken](./media/app-service-web-staged-publishing-realworld-scenarios/1setupstage.png)

Toevoegen van een andere database MySQL, zegt `wordpress-stage-db` aan de resourcegroep `wordpressapp-group`.

 ![MySQL-database toevoegen aan resourcegroep](./media/app-service-web-staged-publishing-realworld-scenarios/2addmysql.png)

De tekenreeksen voor de sleuf van uw werkgebied implementatie om te verwijzen naar de zojuist gemaakte database bijwerken `wordpress-stage-db`. Opmerking dat de productie web app, `wordpressprodapp` en tijdelijke web app `wordpressprodapp-stage` moeten verwijzen naar andere databases.

#### <a name="configure-environment-specific-app-settings"></a>App-omgeving-specifieke instellingen configureren
Ontwikkelaars kunnen sleutel / waarde-reeksparen opgeslagen in Azure als onderdeel van de configuratie-informatie die is gekoppeld aan een web app App instellingen genoemd. Tijdens runtime, App Service Web Apps automatisch voor u deze waarden worden opgehaald en beschikbaar maken voor code die wordt uitgevoerd in uw web app. Vanuit een zekerheid perspectief dat is een mooie zijde profiteren sinds gevoelige informatie, zoals tekenreeksen voor een databaseverbinding met wachtwoorden nooit verschijnen als normale tekst in een bestand, zoals `wp-config.php`.

Deze hieronder omschreven procedure is handig als u uitvoeren zoals het bevat wijzigingen in bestanden en wijzigingen in de database voor WordPress app:
- WordPress versie-upgrade
- Nieuwe toevoegen of bewerken of bijwerken van Plug-ins
- Nieuwe toevoegen of bewerken of thema's bijwerken

App instellingen configureren voor:

- database-informatie
- in- / uitschakelen WordPress-logboekregistratie inschakelen
- Beveiligingsinstellingen voor WordPress

![Instellingen voor Wordpress web app App](./media/app-service-web-staged-publishing-realworld-scenarios/3configure.png)

Zorg ervoor dat u de volgende app-instellingen voor uw productie web app en fase-sleuf hebt toegevoegd. Houd er rekening mee dat de productie web app en klaarzetten van web app verschillende databases gebruiken.
Schakel de **Instelling sleuf** checkbox voor alle parameters behalve WP_ENV instellingen. Hiermee wordt de configuratie voor uw web app, samen met de inhoud van bestanden en database verwisselen. Als **Sleuf** **ingeschakeld is**, de web-app app instellingen en configuratie van de verbinding string wordt niet verplaatst via omgevingen bij het uitvoeren van een SWAP en derhalve als alle wijzigingen in de database aanwezig zijn dit geen regeleinde wordt ingevoegd uw productie web app.

De plaatselijke ontwikkeling omgeving web app op werkgebied web app en de database met behulp van WebMatrix of hulpprogramma('s) van uw keuze, zoals FTP, Git of PhpMyAdmin implementeren.

![Web publiceren Matrix dialoogvenster voor WordPress web app](./media/app-service-web-staged-publishing-realworld-scenarios/4wmpublish.png)

Bladeren en test uw tijdelijke web app. Gezien een scenario waarbij het thema van de web app moet worden bijgewerkt, is hier de tijdelijke web app.

![Tijdelijke web app voor het wisselen van sleuven bladeren](./media/app-service-web-staged-publishing-realworld-scenarios/5wpstage.png)


 Als alles er goed uitziet, klik op de knop **wisselen** in uw tijdelijke web app uw inhoud te verplaatsen naar de productieomgeving. In dit geval verwisselt u de web app en de database via omgevingen tijdens elke **Swap** .

![Voorbeeld van wijzigingen vervangen door WordPress](./media/app-service-web-staged-publishing-realworld-scenarios/6swaps1.png)

 > [AZURE.NOTE]
 >Als u een scenario waarin u alleen push-bestanden (geen updates van de database moet), vervolgens gerelateerde **controleert u** de **Instelling sleuf** voor alle database *app instellingen* en *Instellingen voor connection strings* in web app instelling blade binnen de portal Azure voorbeeld voordat u de SWAP. In dit geval DB_NAME, DB_HOST, DB_PASSWORD, DB_USER, moet standaardinstelling connection string niet weergegeven in het voorbeeld van wijzigingen bij het uitvoeren van een **Swap**. Op deze tijd, wanneer u volledig de bewerking **wisselen** de WordPress web app heeft de updates **alleen**bestanden.

Voordat u een WISSELBESTAND, hier is de productie WordPress web app ![productie web app voor het wisselen van sleuven](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)

Na de operatie WISSELEN is het thema op uw productie web app bijgewerkt.

![Productie web app na het verwisselen van "slots".](./media/app-service-web-staged-publishing-realworld-scenarios/8afswap.png)

In een situatie als u **terugdraaien wilt**, kunt u gaan naar de productie web app-instellingen en klik op de knop **wisselen** te verwisselen van de web app en de database van de productie naar de staging-sleuf. Een belangrijk ding om te onthouden is dat als u wijzigingen in de database zijn opgenomen in een bewerking **wisselen** op elk gewenst moment, de volgende keer dat u opnieuw naar uw tijdelijke web app moet u de database implementeren distribueren wordt vervolgens naar de huidige database voor uw tijdelijke web app die kan de vorige productie of het werkgebied.

#### <a name="summary"></a>Samenvatting
Het proces voor elke toepassing een database generaliseren

1. Installeer de toepassing op uw lokale omgeving
2. Bevatten de specifieke configuratie van omgeving (lokale en Azure Web App)
3. Instellen van uw App Service Web Apps – Staging, productie-omgevingen
4. Als er een productietoepassing al wordt uitgevoerd op Azure, synchroniseren uw productie-inhoud (bestanden/code + database) naar lokale en staging omgeving.
5. Ontwikkelen van uw toepassing op uw lokale omgeving
6. Plaats uw productie web app onder onderhoud of vergrendelde modus en synchronisatiedatabase inhoud van productie naar de fasering en dev omgevingen
7. Staging omgeving en Test implementeren
8. Voor de productie-omgeving implementeren
9. Herhaal stap 4 tot en met 6

### <a name="umbraco"></a>Umbraco
In deze sectie leert u hoe de Umbraco CMS maakt gebruik van een aangepaste module van implementeren in een omgeving met meerdere DevOps. In dit voorbeeld beschikt u over een andere aanpak voor het beheren van meerdere ontwikkelomgevingen.

[Umbraco CMS](http://umbraco.com/) is een van de popular.NET CMS oplossingen die worden gebruikt door veel ontwikkelaars met [Courier2](http://umbraco.com/products/more-add-ons/courier-2) -module voor het implementeren van ontwikkeling op het klaarzetten van productieomgevingen. U kunt gemakkelijk een lokale ontwikkelomgeving voor een Umbraco CMS web app met behulp van Visual Studio of WebMatrix maken.

1. Maak een Umbraco web app met Visual Studio, [klikt u hier](https://our.umbraco.org/documentation/Installation/install-umbraco-with-nuget).
2. Voor het maken van een Umbraco web app met WebMatrix, [Klik hier](http://umbraco.com/help-and-support/video-tutorials/getting-started/working-with-webmatrix).

Vergeet voor het verwijderen van de `install` map in uw toepassing en nooit uploaden naar het werkgebied of productie web apps. Voor deze zelfstudie gebruik ik WebMatrix

#### <a name="set-up-a-staging-environment"></a>Een testomgeving instellen
- Maak een sleuf voor de implementatie voor Umbraco CMS web app, ervan uitgaande dat u al een Umbraco CMS web app van en het uitvoeren van bovengenoemde. Als dit niet kunt u een van de marketplace.

- De verbindingsreeks voor de sleuf van uw werkgebied implementatie om te verwijzen naar de zojuist gemaakte database, **umbraco-fase-db**bijwerken. Uw productie web app (umbraositecms-1) en de tijdelijke web app (umbracositecms-1-fase) **moet** verwijzen naar andere databases.

![De verbindingsreeks voor de gefaseerde web app met nieuwe tijdelijke database bijwerken](./media/app-service-web-staged-publishing-realworld-scenarios/9umbconnstr.png)

- Klik op **ophalen publicatie-instellingen** voor de implementatie sleuf **fase**. Dit zal een instellingenbestand publiceren waarin alle informatie die vereist zijn voor Visual Studio of Matrix Web voor het publiceren van de toepassing van de plaatselijke ontwikkeling web app Azure web app downloaden.

 ![Get publicatie-instelling van de tijdelijke web app.](./media/app-service-web-staged-publishing-realworld-scenarios/10getpsetting.png)

- Open uw web app in plaatselijke ontwikkeling in de **WebMatrix** of **Visual Studio**. In deze zelfstudie ik Matrix Web gebruik en moet u eerst het bestand publiceren met instellingen voor uw tijdelijke web app importeren

![Publicatie-instellingen voor Umbraco met behulp van de Matrix Web importeren](./media/app-service-web-staged-publishing-realworld-scenarios/11import.png)

- Wijzigingen in het dialoogvenster bekijken en implementeren van uw lokale web app voor uw Azure web app, *umbracositecms-1-fase*. Als u bestanden rechtstreeks in uw tijdelijke web app implementeren laat u zal weg alle bestanden in de `~/app_data/TEMP/` map als deze worden gegenereerd wanneer het werkgebied web app eerst gestart. U moet ook weglaten de `~/app_data/umbraco.config` , als dit bestand opnieuw gegenereerd.

![Wijzigingen in de matrix web publiceren](./media/app-service-web-staged-publishing-realworld-scenarios/12umbpublish.png)

- Na het publiceren de Umbraco lokale web app met succes naar de staging-web app, Ga naar uw tijdelijke web app en sprake is van problemen op de volgende tests worden uitgevoerd.

#### <a name="set-up-courier2-deployment-module"></a>Courier2 implementatie module instellen
Met [Courier2](http://umbraco.com/products/more-add-ons/courier-2) -module kunt u push inhoud, stylesheets, ontwikkeling, modules en meer met een eenvoudige klik met de rechtermuisknop in een gefaseerde installatie web app aan productie web app voor een meer gedoe gratis implementaties en terugdringen van uw productie web app beschadigen wanneer u een update implementeert.
Een licentie aanschaffen voor Courier2 voor het domein `*.azurewebsites.net` en uw aangepaste domein (bijvoorbeeld http://abc.com) als u de licentie hebt aangeschaft, plaatst u de gedownloade licentie (. WEERDE-bestand) in het `bin` map.

![Licentie onder de map bin neerzetten](./media/app-service-web-staged-publishing-realworld-scenarios/13droplic.png)

Het pakket Courier2 downloaden vanaf [hier](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/). Meld u aan bij uw werkgebied web app, zegt http://umbracocms-site-stage.azurewebsites.net/umbraco en klik op Menu **Developer** en selecteer **pakketten**. Klik op lokale pakket **installeren**

![Umbraco Package installer](./media/app-service-web-staged-publishing-realworld-scenarios/14umbpkg.png)

Het courier2 pakket met behulp van de installer uploaden.

![Pakket voor module courier uploaden](./media/app-service-web-staged-publishing-realworld-scenarios/15umbloadpkg.png)

Als u wilt configureren dat u wilt bijwerken courier.config bestand in de map **Config** van uw web app.

```xml
<!-- Repository connection settings -->
 <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
 <repositories>
    <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
    <repository name="production web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
      <url>http://umbracositecms-1.azurewebsites.net</url>
      <user>0</user>
      <!--<login>user@email.com</login> -->
      <!-- <password>user_password</password>-->
      <!-- <passwordEncoding>Clear</passwordEncoding>-->
      </repository>
 </repositories>
 ```

Onder `<repositories>`, voer de informatie productie site URL- en gebruikersgegevens. Als u van standaard Umbraco lidmaatschapsprovider gebruikmaakt, voegt u de ID voor de gebruiker beheer in <user> sectie. Als u een aangepaste Umbraco lidmaatschapsprovider gebruiken `<login>`,`<password>` Courier2 module weet hoe u verbinding maken met de productiesite. Raadpleeg de [documentatie](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation) voor Courier-module voor meer informatie.

Op dezelfde manier Courier-module installeren op uw productiesite en configureer deze fase web app, wijs in het bestand van de respectieve courier.config zoals hier wordt weergegeven

```xml
 <!-- Repository connection settings -->
 <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
 <repositories>
    <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
    <repository name="Stage web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
      <url>http://umbracositecms-1-stage.azurewebsites.net</url>
      <user>0</user>
      </repository>
 </repositories>
```

Klik op de tab Courier2 in Umbraco CMS web app dashboard en locaties te selecteren. U ziet de naam van de bibliotheek zoals vermeld in `courier.config`. Dit doen op uw productie- en tijdelijke web apps.

![Weergave bestemming web app opslagplaats](./media/app-service-web-staged-publishing-realworld-scenarios/16courierloc.png)

Nu kunt bepaalde inhoud van een staging-site aan productiesite te implementeren. Ga naar inhoud en een bestaande pagina selecteren of een nieuwe pagina maken. Ik selecteer een bestaande pagina in mijn web app waar de titel van de pagina is gewijzigd om **Aan de slag: nieuwe** en klik nu op **Opslaan en publiceren**.

![Titel van de pagina wijzigen en publiceren](./media/app-service-web-staged-publishing-realworld-scenarios/17changepg.png)

Selecteer nu de gewijzigde pagina en *Klik met de rechtermuisknop* om alle opties weer te geven. Klik op **Courier** implementatie dialoogvenster weergeven. Klik op **Deploy** starten implementatie

![Courier module implementatie, dialoogvenster](./media/app-service-web-staged-publishing-realworld-scenarios/18dialog1.png)

Controleer de wijzigingen en klik op Doorgaan.

![Courier module implementatie dialoogvenster wijzigingen verwerken](./media/app-service-web-staged-publishing-realworld-scenarios/19dialog2.png)

Implementatie logboek bevat als de installatie geslaagd is.

 ![Implementatie Courier-module Logboeken weergeven](./media/app-service-web-staged-publishing-realworld-scenarios/20successdlg.png)

Ga uw productie web app om te zien als de wijzigingen zijn doorgevoerd.

 ![Surfen web app productie.](./media/app-service-web-staged-publishing-realworld-scenarios/21umbpg.png)

Raadpleeg de documentatie voor meer informatie over het gebruik van Courier.

#### <a name="how-to-upgrade-umbraco-cms-version"></a>Umbraco CMS versie upgraden

Courier helpt niet implementeren met één versie van Umbraco CMS upgraden. Bij het upgraden van versie Umbraco CMS, moet u controleren voor compatibiliteitsproblemen met uw aangepaste modules of modules van derden en de bibliotheken Umbraco Core. Beste

1. ALTIJD back-up van uw web app en de database voordat u een upgrade uitvoert. Op Azure Web App, kunt u instellen van automatische back-ups voor uw websites met behulp van de back-up functie en herstellen van uw site als nodig is met behulp van kunt u functie herstellen. Zie voor meer informatie [hoe u een back-up van uw web app](web-sites-backup.md) en [het herstellen van uw web app](web-sites-restore.md).

2. Controleer of de derden-pakketten die u gebruikt compatibel met de versie die u een uitvoert zijn naar upgrade. Pagina op van het pakket te downloaden, controleert u de compatibiliteit van het Project met Umbraco CMS versie.

Volg de richtlijnen als vermeld [hier](https://our.umbraco.org/documentation/getting-started/set up/upgrading/general)voor meer informatie over het bijwerken van uw web app lokaal.

Nadat de upgrade van uw site voor plaatselijke ontwikkeling wijzigingen publiceren naar de staging-web app. Test uw toepassing en als alles er goed uitziet, **wisselen** knop **wisselen** de staging-site aan productie web app gebruiken. Bij het uitvoeren van de bewerking **wisselen** , kunt u de wijzigingen die worden beïnvloed in de configuratie van uw web app weergeven. Met deze bewerking **wisselen** zijn we wisselen van web apps en databases. Dit betekent, nadat de SWAP de productie web app nu wijs database umbraco-fase-db en staging web app wordt met umbraco-prod-db-database.

![Voorbeeld voor het implementeren van Umbraco CMS wisselen](./media/app-service-web-staged-publishing-realworld-scenarios/22umbswap.png)

Het voordeel van het wisselen van de web app en de database:
1. Kunt u terugkeren naar de vorige versie van uw web app met een andere **wisselen** als er problemen van toepassing zijn.
2. U moet bestanden en de database van een staging-web app productie web app en de database implementeren voor een upgrade. Er zijn veel dingen die fout kunnen gaan bij het distribueren van bestanden en de database. We kunnen met de functie **swap** van "slots", minder uitval tijdens een upgrade en verminderen het risico van storingen die optreden kunnen bij het implementeren van wijzigingen.
3. Biedt u de mogelijkheid om te doen **A / B testen** [testen in de productie](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/) gebruiken

In dit voorbeeld ziet u de flexibiliteit van het platform waarop u aangepaste modules, vergelijkbaar met Umbraco Courier-module voor het beheren van de installatie in omgevingen kunt samenstellen.

## <a name="references"></a>Verwijzingen
[Flexibele softwareontwikkeling met Azure App-Service](app-service-agile-software-development.md)

[Staging-omgevingen voor web apps in Azure App-Service instellen](web-sites-staged-publishing.md)

[Het blokkeren van webtoegang tot niet-productie-implementatie-sleuven](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
