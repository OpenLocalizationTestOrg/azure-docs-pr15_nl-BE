<properties
    pageTitle="Het werken met de Node.js backend server SDK voor mobiele Apps | Azure App-Service"
    description="Informatie over het werken met de Node.js backend server SDK voor Mobile Apps in Azure App-Service."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="node"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-azure-mobile-apps-nodejs-sdk"></a>Het gebruik van de SDK Azure Mobile Apps Node.js

[AZURE.INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Dit artikel bevat gedetailleerde informatie en voorbeelden ziet u hoe u werkt met een end Node.js in Azure App Service Mobile Apps.

## <a name="Introduction"></a>Inleiding

Azure Mobile Apps in App-Service biedt de mogelijkheid een mobile-geoptimaliseerde API voor gegevenstoegang Web toevoegen aan een webtoepassing.  Azure App Service Mobile Apps SDK is bestemd voor ASP.NET en Node.js webtoepassingen.  De SDK bevat de volgende bewerkingen:

- Bewerkingen (lezen, Insert, Update, Delete) voor toegang tot gegevens
- API voor aangepaste activiteiten

Met beide bewerkingen wordt voorzien voor verificatie via alle leveranciers van de identiteit van Azure App Service, met inbegrip van de sociale identiteitsproviders zoals Facebook, Twitter, Google en Microsoft Azure Active Directory voor de identiteit van de onderneming toestaat.

Voorbeelden vindt u voor elk geval gebruik in de [map samples op GitHub].

## <a name="supported-platforms"></a>Ondersteunde Platforms

De SDK Azure Mobile Apps knooppunt ondersteunt in de huidige LTS-versie van het knooppunt en hoger.  Op het schrijven is de meest recente versie van LTS knooppunt v4.5.0.  Andere versies van het knooppunt kunnen werken, maar worden niet ondersteund.

De Azure Mobile Apps knooppunt SDK ondersteunt twee databasestuurprogramma's - het knooppunt mssql-stuurprogramma ondersteunt SQL Azure en lokale exemplaren van SQL Server.  Het stuurprogramma sqlite3 ondersteunt SQLite databases op één exemplaar.

### <a name="howto-cmdline-basicapp"></a>Hoe: Maak een eenvoudige Node.js backend via de opdrachtregel

Elke Azure App Service Mobile App Node.js backend wordt gestart als een toepassing ExpressJS.  ExpressJS is het meest populaire web service raamwerk dat beschikbaar is voor Node.js.  Kunt u een basic application [Express] als volgt:

1. Maak een map voor uw project in een opdracht of het venster PowerShell.

        mkdir basicapp

2. Npm init om te initialiseren van de pakketstructuur worden uitgevoerd.

        cd basicapp
        npm init

    De opdracht van npm init wordt gevraagd vragen initialiseren van het project.  Zie het voorbeeld van de uitvoer:

    ![De uitvoer van de init npm][0]

3. De bibliotheken express en azure-mobile-apps installeren uit de opslagplaats van de npm.

        npm install --save express azure-mobile-apps

4. Maak een bestand app.js om de fundamentele mobiele server implementeren.

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table
        mobile.tables.add('TodoItem');

        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

Deze toepassing maakt een mobile-geoptimaliseerde WebAPI met een enkelvoudig eindpunt (`/tables/TodoItem`) en waarmee niet-geverifieerde toegang tot de onderliggende SQL gegevensopslag met behulp van een dynamische schema.  Het is geschikt voor het volgen van de client-bibliotheek snel aan de slag:

- [Android Client QuickStart]
- [Apache Cordova Client QuickStart]
- [iOS Client QuickStart]
- [Windows Store Client QuickStart]
- [QuickStart Xamarin.iOS Client]
- [QuickStart Xamarin.Android Client]
- [QuickStart Xamarin.Forms Client]

U vindt de code voor deze eenvoudige toepassing in het [basicapp monster op GitHub].

### <a name="howto-vs2015-basicapp"></a>Procedure: een backend knooppunt maken met Visual Studio 2015

Visual Studio 2015 is een uitbreiding op het ontwikkelen van toepassingen binnen de IDE Node.js vereist.  Om te beginnen, installeren de [Node.js extra 1.1 voor Visual Studio].  Zodra de Node.js 's voor Visual Studio zijn geïnstalleerd, maakt u een toepassing Express 4.x:

1. Open het dialoogvenster **Nieuw Project** (uit **bestand** > **Nieuw** > **Project...**).

2. **Sjablonen** > **JavaScript** > **Node.js**.

3. Selecteer de **toepassing standaard Azure Node.js Express 4**.

4. Vul de naam van het project.  Klik op *OK*.

    ![Nieuw Project voor Visual Studio 2015][1]

5. Klik met de rechtermuisknop op het knooppunt **npm** en selecteer **... installeren nieuwe npm-pakketten**.

6. Wellicht moet u de npm catalogus op het maken van de eerste toepassing van Node.js vernieuwen.  Klik op **vernieuwen** als nodig is.

7. _Azure-mobile-apps_ invoeren in het zoekvak.  Het pakket **azure mobile apps 2.0.0** Klik **Installeer pakket**.

    ![Nieuwe npm pakketten installeren][2]

8. Klik op **sluiten**.

9. Open het bestand _app.js_ om ondersteuning voor de SDK Azure Mobile Apps toevoegen.  Op regel 6 at van de bibliotheek onder instructies nodig, voeg de volgende code toe:

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

    In de regel ongeveer 27 na de app.use-instructies, voeg de volgende code:

        app.use('/users', users);

        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

    Sla het bestand.

10. Voer de toepassing lokaal (de API wordt aangeboden op http://localhost:3000) of publiceren naar Azure.

### <a name="create-node-backend-portal"></a>Procedure: een Node.js backend met de Azure portal maken

In de [Azure portal]kunt u een back-end mobiele App recht. Voer de volgende stappen uit of samen een client en een server maken door de zelfstudie voor het [maken van een mobiele app](app-service-mobile-ios-get-started.md) . De zelfstudie bevat een vereenvoudigde versie van deze instructies en is het meest geschikt voor het bewijs van concept projecten.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Kies in het blad _aan de slag_ onder **een tabel API maken**, **Node.js** als de **Backend taal**. Het selectievakje '**ik erken dat dit wordt overschreven alle site inhoud.**' en klik vervolgens op **tabel maken TodoItem**.

### <a name="download-quickstart"></a>Procedure: de Node.js backend quickstart project coderen met behulp van Git downloaden

Wanneer u een backend Node.js Mobile App maakt met behulp van de portal blade **Quick start** , wordt een project Node.js voor u gemaakt en geïmplementeerd op uw site. U kunt API's en tabellen toevoegen en bewerken van codebestanden voor de backend Node.js in de portal. U kunt diverse hulpprogramma's voor implementatie ook gebruiken voor het downloaden van de back-end project zodat u kunt toevoegen of API's en tabellen wijzigen en vervolgens opnieuw publiceren van het project. Zie de [Azure App Service Deployment Guide]voor meer informatie. de volgende procedure wordt een Git repository te downloaden van de projectcode quickstart.

1. Installeer Git, als u dat nog niet hebt gedaan. Vereiste stappen voor het installeren van Git verschillen tussen besturingssystemen. Zie [Git installeren](http://git-scm.com/book/en/Getting-Started-Installing-Git) voor installatie-instructies en besturingssysteem-specifieke verdelingen.

2. Volg de stappen in [de opslagplaats van de app App-Service inschakelen](../app-service-web/app-service-deploy-local-git.md#Step3) zodat de Git repository voor uw site back-end, zodat een notitie van de implementatie van gebruikersnaam en wachtwoord.

3. Maak een notitie van de instelling **Git clone URL** in de blade voor uw mobiele App backend.

4. Uitvoeren van de `git clone` opdracht met het Git clone URL invoeren van het wachtwoord, indien vereist, zoals in het volgende voorbeeld:

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git

5. Ga naar de lokale map die in het voorgaande voorbeeld /todolist is, en u ziet de project-bestanden zijn gedownload. Zoek de `todoitem.json` -bestand in de `/tables` directory.  Dit bestand definieert machtigingen voor de tabel.  Ook de `todoitem.js` -bestand in dezelfde map waarin die CRUD-bewerking, scripts voor de tabel.

6. Nadat u wijzigingen hebt aangebracht in Microsoft project-bestanden, uitvoeren van de volgende opdrachten voor het toevoegen, doorvoeren en vervolgens de wijzigingen uploaden naar de site:

        $ git commit -m "updated the table script"
        $ git push origin master

    Wanneer u nieuwe bestanden aan het project toevoegt, moet u eerst uit te voeren het `git add .` opdracht.

De site wordt gepubliceerd, telkens wanneer een nieuwe set doorvoercoördinatie geduwd naar de site.

### <a name="howto-publish-to-azure"></a>Procedure: uw backend Node.js publiceren naar Azure

Microsoft Azure biedt een groot aantal mechanismen voor het publiceren van uw backend Azure App Service Mobile Apps Node.js naar Azure service.  Deze omvatten de implementatieprogramma's geïntegreerd in Visual Studio, opdrachtregelprogramma's en opties voor continue distributie op basis van het besturingselement.  Zie de [Azure App Service Deployment Guide]voor meer informatie over dit onderwerp.

Azure App-Service heeft een specifiek advies voor Node.js-toepassing die u voordat u implementeert controleren moet:

- Het [opgeven van de versie van het knooppunt]
- Het [gebruik van knooppunt modules]

### <a name="howto-enable-homepage"></a>Procedure: een introductiepagina voor uw toepassing inschakelen

Veel toepassingen zijn een combinatie van web- en mobiele toepassingen en de ExpressJS framework kunt u de twee facetten combineren.  Soms echter, kunt u alleen een mobiele interface implementeren.  Het is handig om een landingspagina te waarborgen van de app-service actief is.  Geef uw eigen introductiepagina of een tijdelijke startpagina inschakelen.  Als u wilt dat een tijdelijke startpagina, gebruikt u de volgende Azure Mobile Apps instantiëren:

    var mobile = azureMobileApps({ homePage: true });

Als u wilt dat alleen deze optie beschikbaar bij het ontwikkelen van lokaal, kunt u deze instelling toevoegen uw `azureMobile.js` bestand.

## <a name="TableOperations"></a>Bewerkingen 

De azure-mobile-apps Node.js Server SDK biedt mechanismen om gegevenstabellen worden opgeslagen in Azure SQL Database als een WebAPI weer te geven.  Er zijn vijf operations beschikbaar.

| Bewerking | Beschrijving |
| --------- | ----------- |
| /Tables/_tablename_ ophalen | Ophalen van alle records in de tabel |
| /Tables/_tablename_/:id ophalen | Een bepaalde record in de tabel ophalen |
| /Tables/_tablename_ boeken | Een record in de tabel maken |
| PATCH /tables/_tablename_/:id | Een record in de tabel bijwerken |
| /Tables/_tablename_/:id verwijderen | Een record in de tabel verwijderen |

Deze WebAPI ondersteunt [OData] en breidt het tabelschema ter ondersteuning van de [synchronisatie van off line gegevens].

### <a name="howto-dynamicschema"></a>Procedure: met behulp van een schema voor dynamische tabellen definiëren

Voordat u een tabel kunt gebruiken, moet worden gedefinieerd.  Tabellen kunnen worden gedefinieerd met een statisch schema (de ontwikkelaar bepaalt waar de kolommen in het schema) of dynamisch (de SDK bepaalt wanneer het schema op basis van binnenkomende aanvragen). De ontwikkelaar kan bovendien specifieke aspecten van de WebAPI bepalen door Javascript-code toe te voegen aan de definitie.

Beste, moet u elke tabel in een Javascript-bestand in de map met tabellen definiëren en vervolgens de methode tables.import() gebruikt om de tabellen te importeren.  Uitbreiding van de basic-app, zou het bestand app.js worden aangepast:

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define the database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

Definieer de tabel in. / tables/TodoItem.js:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for the table goes here

    module.exports = table;

Tabellen dynamisch schema standaard gebruikt.  Uit te schakelen wordt globaal bij dynamische schema de instelling App **MS_DynamicSchema** ingesteld op false binnen de portal Azure.

Een volledig voorbeeld vindt u in het [voorbeeld todo op GitHub].

### <a name="howto-staticschema"></a>Procedure: tabellen met behulp van een statisch schema definiëren

U kunt expliciet aangeven welke kolommen toegankelijk te maken via de WebAPI.  De azure-mobile-apps Node.js SDK automatisch vereist is voor de synchronisatie van off line gegevens aan de lijst die u extra kolommen toegevoegd.  De clienttoepassingen QuickStart moeten bijvoorbeeld een tabel met twee kolommen: tekst (string) en voltooien (boolean).  
De tabel kan worden gedefinieerd in de tabel definitie JavaScript-bestand (te vinden in de map met tabellen) als volgt:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

Als u statisch tabellen definieert, moet u ook schema van de database maken bij het opstarten van de methode tables.initialize() aanroepen.  De methode tables.initialize() retourneert een [belofte] zodat de webservice dienen geen aanvragen voordat de database wordt geïnitialiseerd.

### <a name="howto-sqlexpress-setup"></a>Procedure: Gebruik SQL Express als gegevensarchief op de lokale computer ontwikkeling

De Azure Mobile Apps in de AzureMobile Apps knooppunt SDK vindt u drie opties voor het ophalen van gegevens uit de doos: SDK bevat drie opties voor het ophalen van gegevens uit de doos:

- Het stuurprogramma **geheugen** gebruiken om een voorbeeld van de niet-permanente archief
- Gebruik de **mssql** -stuurprogramma voor een gegevensarchief SQL Express bieden voor ontwikkeling
- De **mssql** -stuurprogramma gebruikt om een gegevensarchief Azure SQL-Database voor de productie

De SDK Azure Mobile Apps Node.js gebruikt de [mssql Node.js pakket] tot stand brengen en gebruiken van een verbinding met de SQL Express en SQL-Database.  Dit pakket moet TCP-verbindingen in te schakelen op SQL Express-exemplaar.

> [AZURE.TIP]Het stuurprogramma geheugen beschikt niet over een volledige set van faciliteiten voor het testen.  Als u uw backend lokaal testen wilt, raden wij het gebruik van een gegevensarchief SQL Express en de mssql-stuurprogramma.

1. Download en installeer [Microsoft SQL Server 2014 Express].  Zorg ervoor dat u de SQL Server 2014 Express installeren met extra editie.  Tenzij u expliciet 64-bits ondersteuning nodig, verbruikt de 32-bits versie minder geheugen wordt uitgevoerd.

2. De SQL Server 2014 Configuration Manager uitvoeren.

  1. Vouw het knooppunt voor **Configuratie van SQL Server** in het menu links structuur.
  2. Klik op **protocollen voor SQLEXPRESS**.
  3. **TCP/IP** met de rechtermuisknop en selecteer **inschakelen**.  Klik op **OK** in het pop-upvenster.
  4. **TCP/IP** met de rechtermuisknop en selecteer **Eigenschappen**.
  5. Klik op het tabblad **IP-adressen** .
  6. Het knooppunt **IPAll** vinden.  Voer in het veld **TCP-poort** **1433**.

         ![Configure SQL Express for TCP/IP][3]

  7. Klik op **OK**.  Klik op **OK** in het pop-upvenster.
  8. Klik op **SQL Server-Services** in het menu links structuur.
  9. Klik met de rechtermuisknop op **SQL Server (SQLEXPRESS)** en selecteer **opnieuw opstarten**
  10. Sluit SQL Server 2014 Configuration Manager.

3. Uitvoeren van SQL Server 2014 Management Studio en verbinding maken met uw lokale SQL Express-exemplaar

  1. Uw exemplaar van het Object Explorer met de rechtermuisknop en selecteer **Eigenschappen**
  2. Selecteer het tabblad **beveiliging** .
  3. Controleer of dat de **SQL Server en Windows-verificatiemodus** is geselecteerd.
  4. Klik op **OK**

        ![SQL Express-verificatie configureren][4]

  5. **Beveiliging** > **aanmeldingen** in het Object Explorer
  6. Klik met de rechtermuisknop op **aanmeldingen** en selecteer **Nieuwe aanmelding...**
  7. Voer een aanmeldingsnaam.  Selecteer **SQL Server-verificatie**.  Voer een wachtwoord in en geef vervolgens hetzelfde wachtwoord in **wachtwoord bevestigen**.  De wachtwoorden moeten voldoen aan complexiteitsvereisten voor Windows.
  8. Klik op **OK**

        ![Een nieuwe gebruiker toevoegen aan de SQL Express][5]

  9. Klik met de rechtermuisknop op uw nieuwe aanmelding en selecteer **Eigenschappen**
  10. De pagina **Serverrollen** selecteren
  11. Schakel het selectievakje naast de serverrol **dbcreator**
  12. Klik op **OK**
  13. Sluit SQL Server 2015 Management Studio

Zorg ervoor dat u vastleggen dat de gebruikersnaam en het wachtwoord dat u hebt geselecteerd.  Wellicht moet u extra rollen of machtigingen zijn afhankelijk van uw specifieke vereisten toewijzen.

De toepassing Node.js leest de omgevingsvariabele **SQLCONNSTR_MS_TableConnectionString** voor de verbindingsreeks voor deze database.  U kunt deze variabele instellen in uw omgeving.  U kunt bijvoorbeeld PowerShell deze omgevingsvariabele instellen:

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

Toegang tot de database via een TCP/IP-verbinding en een gebruikersnaam en wachtwoord voor de verbinding.

### <a name="howto-config-localdev"></a>Procedure: het project voor de ontwikkeling van lokale configureren

Azure Mobile Apps leest een JavaScript-bestand _azureMobile.js_ aangeroepen vanuit het lokale bestandssysteem.  Gebruik dit bestand niet naar de SDK Azure Mobile Apps configureren in de productie - instellingen binnen de [portal Azure] App te gebruiken.  Het bestand _azureMobile.js_ moet een configuratieobject exporteren.  De meest gebruikte instellingen zijn:

- Database-instellingen
- Instellingen voor vastleggen van diagnostische gegevens
- Alternatieve instellingen voor CORS

Er volgt een voorbeeldbestand _azureMobile.js_ uitvoering van de voorgaande database-instellingen:

    module.exports = {
        cors: {
            origins: [ 'localhost' ]
        },
        data: {
            provider: 'mssql',
            server: '127.0.0.1',
            database: 'mytestdatabase',
            user: 'azuremobile',
            password: 'T3stPa55word'
        },
        logging: {
            level: 'verbose'
        }
    };

Wij raden aan dat u _azureMobile.js_ aan uw _.gitignore_ bestand toevoegen (of andere broncodebeheer bestand negeren) om te voorkomen dat wachtwoorden worden opgeslagen in de cloud.  Altijd productie-instellingen configureren in App instellingen binnen de [Azure portal].

### <a name="howto-appsettings"></a>Procedure: App instellingen configureren voor uw mobiele App

De meeste instellingen in het bestand _azureMobile.js_ hebt een equivalente instelling voor de App in de [portal Azure].  De volgende lijst gebruiken voor het configureren van uw app in App-instellingen:

| App-instelling                 | _azureMobile.js_ instellen  | Beschrijving                               | Geldige waarden                                |
| :-------------------------- | :------------------------ | :---------------------------------------- | :------------------------------------------ |
| **MS_MobileAppName**        | naam                      | De naam van de app.                       | tekenreeks                                      |
| **MS_MobileLoggingLevel**   | Logging.level             | Minimale logboekniveau van berichten aan te melden      | fout, waarschuwing, informatie, uitgebreide, opsporen, lol |
| **MS_DebugMode**            | foutopsporing                     | Inschakelen of uitschakelen debug-modus              | True, false                                 |
| **MS_TableSchema**          | Data.schema               | Naam van de standaard schema voor SQL-tabellen        | tekenreeks (standaard: dbo)                       |
| **MS_DynamicSchema**        | data.dynamicSchema        | Inschakelen of uitschakelen debug-modus              | True, false                                 |
| **MS_DisableVersionHeader** | versie (ingesteld op undefined)| Schakelt de header X-ZUMO-Server-versie | True, false                                 |
| **MS_SkipVersionCheck**     | skipversioncheck          | Schakelt de versiecontrole van client-API     | True, false                                 |

Een App instellen:

1. Log in op de [Azure portal].
2. Selecteer **alle resources** of **App Services** Klik op de naam van uw mobiele App.
3. De blade instellingen wordt standaard geopend. Als dat niet het geval is, klikt u op **Instellingen**.
4. **Klik in het menu Algemeen.**
5. Ga naar de sectie Toepassingsinstellingen.
6. Als uw app instelt, al bestaat, klikt u op de waarde van de instelling van de app om de waarde te bewerken.
7. Als uw instelling app niet bestaat, voert u de instelling van de App in het vak sleutel en de waarde in het vak waarde.
8. Eenmaal is voltooid, klikt u op **Opslaan**.

De meeste app-instellingen wijzigt een service opnieuw opstarten vereist.

### <a name="howto-use-sqlazure"></a>Procedure: SQL-Database gebruiken als uw productie-gegevensarchief

<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Azure SQL-Database gebruiken als gegevensarchief is identiek voor alle Azure App toepassing servicetypen. Als u dit nog niet hebt gedaan, als volgt te werk als u wilt maken van een mobiele App backend.

1. Log in op de [Azure portal].

2. In de bovenste links van het venster, klikt u op de knop **+ Nieuw** > **Web + Mobile** > **Mobile App**, Geef een naam op voor uw mobiele App backend.

3. Typ dezelfde naam als uw app in de **Resourcegroep** .

4. Het plan App-Service standaard is geselecteerd.  Als u uw serviceplan App wijzigen wilt, kunt u doen door te klikken op het abonnement App > **+ nieuwe maken**.  Geef een naam op de nieuwe App serviceplan en selecteer de juiste locatie.  Klik op de prijzen laag en selecteer een juiste prijsstelling laag voor de service. Selecteer **Alles weergeven** bekijken meer prijsopties, zoals **vrije** en **gedeeld**.  Zodra u de prijzen laag hebt geselecteerd, klikt u op de knop **selecteren** .  Klik op **OK**in het blad **App serviceplan** .

5. Klik op **maken**. Inrichten van een mobiele App backend kan enkele minuten duren.  Als de backend Mobile App wordt ingericht, Open de portal blade van de **Instellingen** voor de back-end mobiele App.

Zodra de back-end mobiele App is gemaakt, kunt u een nieuwe SQL-database maken of een bestaande SQL-database verbinding met uw mobiele App backend.  In deze sectie maken we een SQL-database.

> [AZURE.NOTE]Hebt u al een database op dezelfde locatie als de backend mobiele app, kunt u in plaats daarvan kiest u **een bestaande database gebruiken** en selecteer vervolgens de database. Het gebruik van een database op een andere locatie wordt afgeraden vanwege de hogere vertragingstijden.

6. Klik op **Instellingen**in de nieuwe mobiele App back-end > **Mobile App** > **Data** > **+ toevoegen**.

7. In het blad **gegevensverbinding toevoegen** , klikt u op **SQL-Database - instellingen vereist** > **een nieuwe database maken**.  Voer de naam van de nieuwe database in het veld **naam** .

8. Klik op de **Server**.  Voer een unieke naam in het veld **servernaam** in de **nieuwe server** -blade en een geschikte **Server admin login** en **wachtwoord**.  Controleer **dat toestaan azure services voor toegang tot de server** is ingeschakeld.  Klik op **OK**.

    ![Azure SQL-Database maken][6]

9. Klik op **OK**in de **nieuwe database** -blade.

10. Terug op het blad **gegevensverbinding toevoegen** **verbindingsreeks**selecteert, voert u de aanmeldingsnaam en het wachtwoord die u hebt opgegeven bij het maken van de database.  Als u een bestaande database gebruikt, bieden de inloggegevens voor die database.  Nadat u hebt ingevoerd, klikt u op **OK**.

11. Weer in de blade **gegevensverbinding toevoegen** , klikt u op **OK** om de database te maken.

<!--- END OF ALTERNATE INCLUDE -->

Het maken van de database kan enkele minuten duren.  Gebruik het gebied met **meldingen** op de voortgang van de implementatie.  Voortgang niet totdat de database is geïmplementeerd.  Zodra met succes geïmplementeerd, wordt een verbindingsreeks voor de SQL-Database-instantie in uw mobiele backend App instellingen gemaakt.  U kunt deze instelling app in de **Instellingen**zien > **Toepassingsinstellingen** > **verbindingsreeksen**.

### <a name="howto-tables-auth"></a>Procedure: verificatie vereist is voor toegang tot tabellen

Desgewenst App Service om verificatie te gebruiken met het eindpunt van de tabellen, moet u eerst App Service verificatie configureren in [Azure portal] .  Lees de handleiding voor de configuratie voor de id-provider die u wilt gebruiken voor meer informatie over het configureren van verificatie in een App Azure Service:

- [Azure Active Directory-verificatie configureren]
- [Facebook-verificatie configureren]
- [Google-verificatie configureren]
- [Het configureren van Microsoft Authentication]
- [Twitter-verificatie configureren]

Elke tabel heeft een access-eigenschap die kan worden gebruikt voor toegang tot de tabel.  In het volgende voorbeeld ziet u een statisch gedefinieerde tabel met verificatie vereist.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

De eigenschap access kunnen drie waarden

  - *anonieme* geeft aan dat de clienttoepassing voor het lezen van gegevens zonder verificatie is toegestaan
  - *geverifieerde* geeft aan dat een geldige verificatietoken voor de aanvraag door de clienttoepassing moet verzenden
  - *uitgeschakeld* geeft aan dat deze tabel is uitgeschakeld

Als de access-eigenschap gedefinieerd is, is niet-geverifieerde toegang toegestaan.

### <a name="howto-tables-getidentity"></a>Procedure: verificatie vorderingen met tabellen gebruiken

U kunt instellen van verschillende claims die worden aangevraagd wanneer verificatie is ingesteld.  Deze claims zijn niet normaal beschikbaar via de `context.user` object.  Echter, ze kunnen worden opgehaald met behulp van de `context.user.getIdentity()` methode.  De `getIdentity()` methode geeft als resultaat een belofte die wordt omgezet in een object.  Het object wordt ingeschakeld door de verificatiemethode (facebook, google, twitter, microsoftaccount of aad).

Als u een Microsoft-Account verificatie en aanvraag die aanspraak maken op de e-mailadressen hebt ingesteld, kunt u het e-mailadres toevoegen aan de record met de controller van de volgende tabel:

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition
    var table = azureMobileApps.table();

    table.columns = {
        "emailAddress": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;
    table.access = 'authenticated';

    /**
    * Limit the context query to those records with the authenticated user email address
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds the email address from the claims to the context item - used for
    * insert operations
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when the client does a request
    // READ - only return records belonging to the authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite the userId based on the authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong to the authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong to the authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

Als u wilt zien welke vorderingen beschikbaar zijn, kunt u een webbrowser gebruiken om weer te geven de `/.auth/me` eindpunt van uw site.

### <a name="howto-tables-disabled"></a>Procedure: toegang tot specifieke bewerkingen uitschakelen

Naast de in de tabel wordt weergegeven, kan de access-eigenschap waarmee afzonderlijke bewerkingen worden gebruikt.  Er zijn vier bewerkingen:

  - *Lees* zijn de RESTful GET-bewerking in de tabel
  - *Invoegen* is de bewerking RESTful boeken in de tabel
  - *de update* is de bewerking RESTful PATCH op de tabel
  - *verwijderen* is de bewerking RESTful verwijderen in de tabel

U wilt bijvoorbeeld een niet-geverifieerde tabel alleen-lezen geven:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <a name="howto-tables-query"></a>Procedure: de query die wordt gebruikt voor tabelbewerkingen aanpassen

Een algemene vereiste voor bewerkingen is een beperkte weergave van de gegevens.  U kunt bijvoorbeeld een tabel die is gecodeerd met de geverifieerde gebruiker-ID, dat u alleen kunt lezen of uw eigen records bijwerken opgeven.  De definitie van de volgende tabel worden deze functionaliteit biedt:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for the table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for the authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite the userId with the authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

Bewerkingen die normaal uitvoeren van een query is een queryeigenschap die u met een positie aanpassen kunt hebben component. De queryeigenschap is een [QueryJS] -object dat wordt gebruikt voor het converteren van een OData-query naar iets dat de backend gegevens kan verwerken.  Een kaart kan worden gebruikt voor eenvoudige gelijke gevallen (zoals in de voorgaande). U kunt ook specifieke SQL-componenten toevoegen:

    context.query.where('myfield eq ?', 'value');

### <a name="howto-tables-softdelete"></a>Procedure: zachte verwijderen op een tabel configureren

Soft Delete verwijderd records niet daadwerkelijk.  In plaats daarvan markeren het als verwijderd in de database door de verwijderde kolom instellen op true.  Zachte verwijderde records verwijderd de SDK Azure Mobile Apps automatisch uit de resultaten tenzij de mobiele Client SDK wordt IncludeDeleted() gebruikt.  Configureren van een tabel voor zachte verwijderen, stelt u de `softDelete` in het definitiebestand van de tabel de eigenschap:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

U moet een mechanisme voor het verwijderen van records - ofwel van een clienttoepassing via een WebJob, Azure functie of via een aangepaste API instellen.

### <a name="howto-tables-seeding"></a>Procedure: zaad, de database met gegevens

Wanneer u een nieuwe toepassing maakt, wilt u mogelijk een tabel met gegevens te vullen.  Dit u kunt doen in de tabel definitie JavaScript-bestand als volgt:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Seeding van gegevens wordt alleen uitgevoerd wanneer de tabel wordt gemaakt door de SDK Azure Mobile Apps.  Als de tabel al in de database bestaat, wordt er geen gegevens in de tabel toegevoegd.  Als dynamische schema is ingeschakeld, wordt het schema afgeleid van de vooraf ingestelde gegevens.

Het is raadzaam dat u expliciet aanroepen de `tables.initialize()` methode voor het maken van de tabel als de service is gestart.

### <a name="Swagger"></a>Procedure: ondersteuning voor Swagger

Azure Mobile Apps in App-Service wordt geleverd met ingebouwde ondersteuning voor [Swagger] .  Swagger als ondersteuning wilt inschakelen, moet u eerst de swagger-gebruikersinterface installeren als een afhankelijkheid:

    npm install --save swagger-ui

Eenmaal geïnstalleerd, kunt u ondersteuning in de constructor Azure Mobile Apps Swagger inschakelen:

    var mobile = azureMobileApps({ swagger: true });

U waarschijnlijk alleen wilt inschakelen Swagger ondersteuning in ontwikkeling edities.  U kunt dit doen met behulp van de `NODE_ENV` app instellen:

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

Het eindpunt swagger bevindt zich op http://_uw site_.azurewebsites.net/swagger.  U kunt openen via de gebruikersinterface van Swagger de `/swagger/ui` eindpunt.  Als u verificatie vereist is voor de gehele toepassing, Swagger, treedt een fout.  Voor de beste resultaten wilt toestaan dat niet-geverifieerde aanvragen via de Azure App Service verificatie / controle autorisatie-instellingen vervolgens verificatie met behulp van de `table.access` eigenschap.

U kunt ook de optie Swagger voor het toevoegen de `azureMobile.js` als u wilt dat alleen Swagger ondersteuning bij het ontwikkelen van lokaal bestand.

## <a name="a-namepushpush-notifications"></a><a name="push">Push-meldingen

Mobiele Apps geïntegreerd met Azure melding Hubs kunt u gerichte push-meldingen verzenden naar miljoenen apparaten voor alle belangrijke platforms. U kunt via Hubs melding push meldingen verzenden aan iOS, Android en Windows apparaten. Voor meer informatie over alle kunt doen met Hubs melding, [kennisgeving Hubs](../notification-hubs/notification-hubs-push-notification-overview.md)overzicht.

### </a><a name="send-push"></a>Procedure: push-meldingen verzenden

De volgende code ziet u hoe het object push met een broadcast-push-bericht verzenden naar geregistreerde iOS-apparaten:

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do the push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Door het maken van een sjabloon push-registratie van de client, kunt u in plaats daarvan een sjabloon push-bericht verzenden naar apparaten op alle ondersteunde platforms. De volgende code toont hoe u een sjabloon bericht verzenden:

    // Define the template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do the push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }


###<a name="push-user"></a>Procedure: push-meldingen verzenden aan een geverifieerde gebruiker met behulp van tags

Wanneer een geverifieerde gebruiker geregistreerd voor push-meldingen, ID-code aan een gebruiker automatisch toegevoegd aan de registratie. Met behulp van deze code, kunt u push-meldingen verzenden naar alle apparaten die zijn geregistreerd door een bepaalde gebruiker. De volgende code wordt de SID van de gebruiker die de aanvraag en een sjabloon push-bericht verzendt naar elke registratie voor die gebruiker:

    // Only do the push if configured
    if (context.push) {
        // Send a notification to the current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Wanneer u registreert voor push-meldingen van een geverifieerde client, moet u ervoor zorgen dat de verificatie is voltooid voordat u de registratie.

## <a name="CustomAPI"></a>Aangepaste API 's

###  <a name="howto-customapi-basic"></a>Procedure: een aangepaste API definiëren


Naast de API voor gegevenstoegang via het eindpunt van de /tables, kan de Azure Mobile Apps aangepaste API dekking bieden.  Aangepaste API's zijn gedefinieerd op een soortgelijke manier aan de tabeldefinities en toegang tot alle faciliteiten, met inbegrip van verificatie.

Desgewenst App Service om verificatie te gebruiken met een aangepaste API, moet u eerst App Service verificatie configureren in [Azure portal] .  Lees de handleiding voor de configuratie voor de id-provider die u wilt gebruiken voor meer informatie over het configureren van verificatie in een App Azure Service:

- [Azure Active Directory-verificatie configureren]
- [Facebook-verificatie configureren]
- [Google-verificatie configureren]
- [Het configureren van Microsoft Authentication]
- [Twitter-verificatie configureren]

Aangepaste API's zijn op ongeveer dezelfde manier als de API tabellen gedefinieerd.

1. Een **api** -map maken
2. Maak een API definitie JavaScript-bestand in de directory **api** .
3. Gebruik de importeermethode importeren de directory **api** .

Hier is de definitie van de prototype api op basis van de basic-app steekproef die we eerder gebruikt.

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

We nemen een voorbeeld API die de datum van de server met de methode _Date.now()_ retourneert.  Dit is het bestand api/date.js:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

Elke parameter is een van de RESTful standaardbewerkingen - GET, POST, PATCH of verwijderen.  De methode is een standaard [ExpressJS Middleware] -functie die de vereiste uitvoer worden verzonden.

### <a name="howto-customapi-auth"></a>Procedure: verificatie vereist is voor toegang tot een aangepaste API

Azure Mobile Apps SDK wordt de verificatie op dezelfde manier als voor het eindpunt van de tabellen en de aangepaste API's geïmplementeerd.  Toevoegen als verificatie aan de API die is ontwikkeld in de vorige sectie, een **access** -eigenschap:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

U kunt ook verificatie opgeven voor specifieke bewerkingen:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // The GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

Het dezelfde token dat wordt gebruikt voor het eindpunt van de tabellen moet worden gebruikt voor aangepaste API's waarvoor verificatie is vereist.

### <a name="howto-customapi-auth"></a>Procedure: grote geüploade bestanden verwerken

Azure Mobile Apps SDK maakt gebruik van de [body-parser middleware](https://github.com/expressjs/body-parser) te accepteren en het decoderen van de inhoud van de hoofdtekst in uw inzending.  Body-parser voor het accepteren van grotere bestanden kunnen uploaden, kunt u vooraf configureren:

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Het bestand is een Base64-gecodeerd voor verzending.  Hierdoor neemt de grootte van de werkelijke upload (en dus de grootte u moet rekening houden).

### <a name="howto-customapi-sql"></a>Procedure: aangepaste SQL-instructies uitvoeren

De Azure Mobile Apps SDK biedt toegang tot de gehele Context via het request-object, zodat u kunt SQL-instructies met parameters voor de gegevensprovider gedefinieerde gemakkelijk uitvoeren:

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on to a later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define the query - anything that can be handled by the mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute the query.  The context for Azure Mobile Apps is available through
            // request.azureMobile - the data object contains the configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <a name="Debugging"></a>Foutopsporing, eenvoudige tabellen en eenvoudige API 's

### <a name="howto-diagnostic-logs"></a>Procedure: foutopsporing, vaststellen en oplossen van problemen met Azure mobiele apps

De App Azure Service biedt verschillende foutopsporing en probleemoplossing van technieken voor Node.js toepassingen.
Raadpleeg de volgende artikelen aan de slag bij het oplossen van uw mobiele Node.js backend:

- [Een Azure App-Service controleren]
- [Diagnostische gegevens vastleggen in Azure App-Service inschakelen]
- [Problemen oplossen met een Azure Service App in Visual Studio]

Node.js toepassingen hebben toegang tot een breed scala van het diagnoselogboek van hulpmiddelen.  De SDK Azure Mobile Apps Node.js gebruikt intern, [Winston] voor vastleggen van diagnostische gegevens.  Logboekregistratie is standaard ingeschakeld door de foutopsporingsmodus inschakelen of door de **MS_DebugMode** app te stellen op true in de [portal Azure]. Gegenereerde logboeken worden weergegeven in de diagnostische logboeken op de [Azure portal].

### <a name="in-portal-editing"></a><a name="work-easy-tables"></a>Procedure: werken met eenvoudige tabellen in Azure portal

Eenvoudige tabellen in de portal kunnen u maken en werken met tabellen rechts in de portal. U kunt ook bewerkingen met de Service App Editor bewerken.

Als u **eenvoudige tabellen** in de back-end site-instellingen klikt, kunt u toevoegen, wijzigen of verwijderen van een tabel. U kunt ook gegevens in de tabel zien.

![Werken met eenvoudige tabellen](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

De volgende opdrachten zijn beschikbaar op de werkbalk voor een tabel:

+ **Machtigingen wijzigen** - wijzigen van de machtiging voor lezen, invoegen, bijwerken en verwijderen van bewerkingen op de tabel. 
  Er zijn opties voor anonieme toegang, verificatie vereisen, of alle toegang tot de bewerking uitschakelen. 
+ **Script bewerken** - scriptbestand voor de tabel geopend in de Editor App Service.
+ **Schema beheren** - toevoegen of verwijderen van kolommen of de tabelindex wijzigen.
+ **Tabel wissen** - een bestaande tabel wordt afgekapt als u alle gegevensrijen maar waarbij het schema ongewijzigd.
+ **Rijen verwijderen** - verwijderen van afzonderlijke rijen met gegevens.
+ **Weergave logboeken streaming** - Hiermee gaat u naar de streaming log-service voor uw site.

###<a name="work-easy-apis"></a>Procedure: werken met eenvoudige API's in de portal voor Azure

Eenvoudige API's in de portal kunt u maken en werken met aangepaste API's rechts in de portal. U kunt API scripts met de Service App Editor bewerken.

Als u op **Eenvoudige API's** in uw back-end site-instellingen, kunt u toevoegen, wijzigen of verwijderen van een aangepaste API-eindpunt.

![Werken met eenvoudige API 's](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

In de portal kunt u de toegangsrechten voor een bepaalde HTTP-actie, het scriptbestand API App Service Editor bewerken of bekijken van de logboeken voor streaming.

###<a name="online-editor"></a>Procedure: code App Service Editor bewerken

De Azure portal kunt u uw scriptbestanden van Node.js backend App Service Editor zonder te hoeven downloaden van het project op uw lokale computer bewerken. Om scriptbestanden te bewerken in de online-editor:

1. Klik op **alle instellingen** in uw mobiele App back-end-blade > **gemakkelijk tabellen** of **Eenvoudige API's**, klikt u op een tabel of een API en klik op **script bewerken**. Het scriptbestand wordt geopend in de App Service Editor.

    ![App Service Editor](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)

2. Breng de gewenste wijzigingen aan het codebestand in de online-editor. Wijzigingen worden automatisch opgeslagen terwijl u typt.


<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Android Client QuickStart]: app-service-mobile-android-get-started.md
[Apache Cordova Client QuickStart]: app-service-mobile-cordova-get-started.md
[iOS Client QuickStart]: app-service-mobile-ios-get-started.md
[QuickStart Xamarin.iOS Client]: app-service-mobile-xamarin-ios-get-started.md
[QuickStart Xamarin.Android Client]: app-service-mobile-xamarin-android-get-started.md
[QuickStart Xamarin.Forms Client]: app-service-mobile-xamarin-forms-get-started.md
[Windows Store Client QuickStart]: app-service-mobile-windows-store-dotnet-get-started.md
[HTML/Javascript Client QuickStart]: app-service-html-get-started.md
[synchronisatie van off line gegevens]: app-service-mobile-offline-data-sync.md
[Azure Active Directory-verificatie configureren]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook-verificatie configureren]: app-service-mobile-how-to-configure-facebook-authentication.md
[Google-verificatie configureren]: app-service-mobile-how-to-configure-google-authentication.md
[Het configureren van Microsoft Authentication]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter-verificatie configureren]: app-service-mobile-how-to-configure-twitter-authentication.md
[Azure App Service Deployment Guide]: ../app-service-web/web-sites-deploy.md
[Een Azure App-Service controleren]: ../app-service-web/web-sites-monitor.md
[Diagnostische gegevens vastleggen in Azure App-Service inschakelen]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Problemen oplossen met een Azure Service App in Visual Studio]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md
[de versie van het knooppunt opgeven]: ../nodejs-specify-node-version-azure-apps.md
[Knooppunt modules gebruiken]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service-web/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Swagger]: http://swagger.io/

[Azure portal]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[voorbeeld basicapp op GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[Todo monster op GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[de map Samples op GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js extra 1.1, voor Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[MSSQL Node.js pakket]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
