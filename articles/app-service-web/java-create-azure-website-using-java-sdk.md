<properties 
    pageTitle="Maken van een Web App in Azure App-Service met de Azure-SDK voor Java" 
    description="Informatie over het maken van een Web App van Azure App-Service via programmacode met behulp van de Azure SDK voor Java." 
    tags="azure-classic-portal"
    services="app-service\web" 
    documentationCenter="Java" 
    authors="donntrenton" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="02/25/2016" 
    ms.author="v-donntr"/>


# <a name="create-a-web-app-in-azure-app-service-using-the-azure-sdk-for-java"></a>Maken van een Web App in Azure App-Service met de Azure-SDK voor Java

<!-- Azure Active Directory workflow is not yet available on the Azure Portal -->

## <a name="overview"></a>Overzicht

In dit scenario wordt beschreven hoe u een Azure SDK voor Java-toepassing die wordt gemaakt van een Web App in [Azure App-Service][]maken en implementeren van een toepassing. Het bestaat uit twee delen:

- Deel 1 ziet u hoe u een Java-toepassing die wordt gemaakt van een web app.
- Deel 2 ziet u het maken van een eenvoudige JSP "Hello World" toepassing en gebruik een FTP-client code-implementatie met App-Service.


## <a name="prerequisites"></a>Vereisten

### <a name="software-installations"></a>Software-installaties

De code van de toepassing AzureWebDemo in dit artikel is geschreven met behulp van Azure Java SDK 0.7.0, die u kunt installeren met de [Web Platform Installer][] (WebPI). Bovendien moet u de meest recente versie van de [Toolkit voor Eclips Azure][]. Nadat u de SDK hebt geïnstalleerd, werken de afhankelijkheden in uw project Eclips door **Index bijwerken** in **Opslagplaatsen Maven**voeren en klik vervolgens opnieuw de meest recente versie van elk pakket in het venster **afhankelijkheden** toevoegen. U kunt controleren of de versie van de geïnstalleerde software in Eclips door te klikken op **Help > Installatiedetails**; u moet minimaal beschikken over de volgende versies:

- Pakket voor Microsoft Azure bibliotheken voor Java 0.7.0.20150309
- Eclips IDE voor ontwikkelaars van Java EE 4.4.2.20150219


### <a name="create-and-configure-cloud-resources-in-azure"></a>Maken en configureren van de bronnen van de wolk in Azure

Voordat u deze procedure begint, moet u een geldig abonnement Azure hebt en een standaardwaarde voor Active Directory (AD) op Azure.


### <a name="create-an-active-directory-ad-in-azure"></a>Maak een Active Directory (AD) in Azure

Als u nog geen een Active Directory (AD) op uw abonnement Azure, log in op de [Azure klassieke portal][] met uw Microsoft-account. Als u meerdere abonnementen hebt, klikt u op **abonnementen** en selecteert u de standaardmap voor het abonnement dat u wilt gebruiken voor dit project. Klik vervolgens op **toepassen** om die weergave abonnement te activeren.

1. Selecteer **Active Directory** in het menu aan de linkerkant. **Klikt u op Nieuw > map > maken van aangepaste**.

2. Selecteer in de **Map toevoegen**, **Nieuwe map maken**.

3. Voer in het vak **naam**de naam van een map.

4. Voer in **domein**een domeinnaam. Dit is een fundamentele domeinnaam die wordt standaard met de directory; het formulier heeft `<domain_name>.onmicrosoft.com`. U kunt een naam op basis van de naam van de map of een andere domeinnaam die u bezit. Later, kunt u toevoegen aan een andere domeinnaam die al in uw organisatie worden gebruikt.

5. Selecteer uw land in een **land of regio**.

Voor meer informatie over AD, Zie [Wat is een directory Azure AD][]?


### <a name="create-a-management-certificate-for-azure"></a>Beheer van certificaten voor Azure maken

De Azure SDK voor Java gebruikt beheer van certificaten voor verificatie met Azure abonnementen. Dit zijn x.509v3-certificaten die u gebruikt voor het verifiëren van een clienttoepassing die gebruikmaakt van de Service Management API om te handelen namens de eigenaar abonnement abonnement bronnen beheren.

De code in deze procedure wordt een zelfondertekend certificaat voor verificatie met Azure. Voor deze procedure moet u een certificaat maken en vooraf aan de [Azure klassieke portal][] te uploaden. Dit omvat de volgende stappen:

- Het certificaat voor een PFX-bestand genereren en lokaal opslaan.
- Een certificaat (CER-bestand) van het PFX-bestand genereren.
- Het CER-bestand uploaden naar uw abonnement Azure.
- Converteren naar JKS, het PFX-bestand omdat Java wordt deze notatie gebruikt voor verificatie met certificaten.
- Schrijf de verificatiecode van de toepassing, die naar het lokale bestand van JKS verwijst.

Wanneer u deze procedure hebt voltooid, het CER-certificaat bevindt zich in uw abonnement Azure en het certificaat JKS wordt opgeslagen op de lokale schijf. Zie voor meer informatie over beheer van certificaten [maken en uploaden van een certificaat voor Azure][].


#### <a name="create-a-certificate"></a>Een certificaat maken

Als u wilt uw eigen zelf-ondertekend certificaat maken, open een opdrachtenconsole voor uw besturingssysteem en voer de volgende opdrachten.

> **Opmerking:**  De computer waarop u deze opdracht uitvoert, moet de JDK geïnstalleerd hebben. Het pad naar het hulpprogramma Keytool gebruikt is ook afhankelijk van de locatie waar u de JDK hebt geïnstalleerd. Zie voor meer informatie de [sleutel en certificaat Management Tool (hulpprogramma Keytool gebruikt)][] in de on line documentatie voor Java.

Het .pfx-bestand maken:

    <java-install-dir>/bin/keytool -genkey -alias <keystore-id>
     -keystore <cert-store-dir>/<cert-file-name>.pfx -storepass <password>
     -validity 3650 -keyalg RSA -keysize 2048 -storetype pkcs12
     -dname "CN=Self Signed Certificate 20141118170652"

Het cer-bestand maken:

    <java-install-dir>/bin/keytool -export -alias <keystore-id>
     -storetype pkcs12 -keystore <cert-store-dir>/<cert-file-name>.pfx
     -storepass <password> -rfc -file <cert-store-dir>/<cert-file-name>.cer

waar:

- `<java-install-dir>`het pad is naar de map waarin u Java hebt geïnstalleerd.
- `<keystore-id>`de item-id keystore is (bijvoorbeeld: `AzureRemoteAccess`).
- `<cert-store-dir>`het pad naar de map waarin u certificaten worden opgeslagen (bijvoorbeeld `C:/Certificates`).
- `<cert-file-name>`de naam van het bestand (bijvoorbeeld `AzureWebDemoCert`).
- `<password>`is het wachtwoord dat u wilt beschermen van het certificaat; het moet ten minste 6 tekens lang zijn. Hoewel dit niet wordt aangeraden, kunt u geen wachtwoord invoeren.
- `<dname>`het X.500 DN-naam moet worden gekoppeld aan de alias is en wordt gebruikt als de velden uitgever en onderwerp in een zelf-ondertekend certificaat.

Zie voor meer informatie, [maken en uploaden van een certificaat voor Azure][].


#### <a name="upload-the-certificate"></a>Het certificaat uploaden

Ga naar de pagina **-Instellingen** in de klassieke portal om een zelfondertekend certificaat uploaden naar Azure, vervolgens klikt u op het tabblad **Beheer van certificaten** . Klik op **uploaden** onder aan de pagina en Ga naar de locatie van het CER-bestand dat u hebt gemaakt.


#### <a name="convert-the-pfx-file-into-jks"></a>Het PFX-bestand converteren naar JKS

In de Windows-opdrachtprompt (als Administrator uitgevoerd), cd naar de map met de certificaten en de volgende opdracht, waarbij `<java-install-dir>` is de map waarin u Java hebt geïnstalleerd op uw computer:

    <java-install-dir>/bin/keytool.exe -importkeystore
     -srckeystore <cert-store-dir>/<cert-file-name>.pfx
     -destkeystore <cert-store-dir>/<cert-file-name>.jks
     -srcstoretype pkcs12 -deststoretype JKS

1. Voer desgevraagd het wachtwoord keystore van bestemming; Dit is het wachtwoord voor het bestand JKS.

2. Voer desgevraagd het wachtwoord voor de gegevensbron keystore; Dit is het wachtwoord dat u hebt opgegeven voor het PFX-bestand.

De twee wachtwoorden hoeft niet hetzelfde te zijn. Hoewel dit niet wordt aangeraden, kunt u geen wachtwoord invoeren.


## <a name="build-a-web-app-creation-application"></a>Bouwen van een Web App-toepassing maken

### <a name="create-the-eclipse-workspace-and-maven-project"></a>De Eclips en Maven Project maken

In deze sectie maakt u een werkruimte en een Maven project web app maken van toepassing met de naam AzureWebDemo.

1. Maak een nieuwe Maven project. Klik op **Bestand > Nieuw > Project Maven**. Selecteer **een eenvoudig project maken** en **de standaardlocatie werkruimte gebruikt**in **Nieuwe Maven Project**.

2. Op de tweede pagina van **Nieuwe Maven-Project**het volgende opgeven:

    - Groep-ID:`com.<username>.azure.webdemo`
    - Artefact-ID: AzureWebDemo
    - Versie: 0.0.1-SNAPSHOT
    - Verpakking: jar
    - Naam: AzureWebDemo

    Klik op **Voltooien**.

3. Het nieuwe project pom.xml bestand openen in Project Explorer. Selecteer het tabblad **afhankelijkheden** . Dit is een nieuw project, worden nog geen pakketten weergegeven.

4. Open de opslagplaatsen van Maven. **Klik op Venster > weergave tonen > Overige > Maven > Maven opslagplaatsen** en klik op **OK**. De **Opslagplaatsen van Maven** -weergave wordt weergegeven aan de onderkant van de IDE.

5. **Globale opslagplaatsen**te openen, klik met de rechtermuisknop op de **centrale** opslagplaats en selecteer **Index opnieuw maken**.

    ![][1]
    
    Deze stap kan enige tijd duren afhankelijk van de snelheid van uw verbinding. Wanneer u de index opnieuw opgebouwd, ziet u de pakketten Microsoft Azure in de **centrale** opslagplaats voor Maven.

6. **Afhankelijkheden**, klik op **toevoegen**. Voer in **De groep Enter ID...** `azure-management`. Selecteer de pakketten voor base management en beheer van de Service Web-Apps App:

        com.microsoft.azure  azure-management
        com.microsoft.azure  azure-management-websites

    > **Opmerking:** Als u de afhankelijkheden na de release van een nieuwe versie bijwerken wilt, moet u elk van de afhankelijkheden in deze lijst opnieuw toevoegen.
    > Nadat u op **toevoegen** en selecteer elke afhankelijkheid, wordt dit weergegeven met het nummer van de nieuwe versie in de lijst **afhankelijkheden** .

Klik op **OK**. De Azure pakketten worden vervolgens weergegeven in de lijst **afhankelijkheden** .


### <a name="writing-java-code-to-create-a-web-app-by-calling-the-azure-sdk"></a>Java-Code voor het maken van een Web App door het aanroepen van de SDK Azure schrijven

Vervolgens de code schrijven die API's in Azure SDK voor Java aanroepen voor het maken van de web-app App-Service.

1. Maak een Java-klasse om het hoofdgegeven punt code bevatten. Klik met de rechtermuisknop op het projectknooppunt in Project Explorer en selecteer **Nieuw > klasse**.

2. Naam van **Nieuwe Java-klasse**, de klasse `WebCreator` en schakel het selectievakje in **openbare statische void main** . De selecties moeten als volgt worden weergegeven:

    ![][2]

3. Klik op **Voltooien**. Het WebCreator.java wordt weergegeven in Project Explorer.


### <a name="calling-the-azure-api-to-create-an-app-service-web-app"></a>Het maken van een Web Service App App Azure API aanroepen


#### <a name="add-necessary-imports"></a>Benodigde invoer toevoegen

WebCreator.java, voeg de volgende invoer; deze invoer bieden toegang tot klassen in het beheer van bibliotheken voor het consumeren van Azure-API's:

    // General imports
    import java.net.URI;
    import java.util.ArrayList;
    
    // Imports for Exceptions
    import java.io.IOException;
    import java.net.URISyntaxException;
    import javax.xml.parsers.ParserConfigurationException;
    import com.microsoft.windowsazure.exception.ServiceException;
    import org.xml.sax.SAXException;
    
    // Imports for Azure App Service management configuration
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.management.configuration.ManagementConfiguration;
    
    // Service management imports for App Service Web Apps creation
    import com.microsoft.windowsazure.management.websites.*;
    import com.microsoft.windowsazure.management.websites.models.*;
    
    // Imports for authentication
    import com.microsoft.windowsazure.core.utils.KeyStoreType;


#### <a name="define-the-main-entry-point-class"></a>De belangrijkste post punt klasse definiëren

Omdat het doel van de toepassing van de AzureWebDemo voor het maken van een Web App van de App-Service, de belangrijkste klasse een naam voor deze toepassing `WebAppCreator`. Deze klasse biedt de hoofdvermelding punt code de API Azure Service Management roept te maken van de web app.

Voeg de volgende parameterdefinities voor het web app en webspace. U moet uw eigen Azure abonnement-ID en het certificaat van de informatie te verstrekken.

    public class WebAppCreator {
    
        // Parameter definitions used for authentication.
        private static String uri = "https://management.core.windows.net/";
        private static String subscriptionId = "<subscription-id>";
        private static String keyStoreLocation = "<certificate-store-path>";
        private static String keyStorePassword = "<certificate-password>";
    
        // Define web app parameter values.
        private static String webAppName = "WebDemoWebApp";
        private static String domainName = ".azurewebsites.net";
        private static String webSpaceName = WebSpaceNames.WESTUSWEBSPACE;
        private static String appServicePlanName = "WebDemoAppServicePlan";

waar:

- `<subscription-id>`is de Azure abonnement-ID die u wilt om de bron te maken.
- `<certificate-store-path>`het pad en de bestandsnaam om het bestand JKS in de lokale certificaat-mailarchiefmap is. Bijvoorbeeld, `C:/Certificates/CertificateName.jks` voor Linux en `C:\Certificates\CertificateName.jks` voor Windows.
- `<certificate-password>`is het wachtwoord dat u hebt opgegeven bij het maken van uw certificaat JKS.
- `webAppName`de naam die u kiest; deze procedure wordt de naam van de `WebDemoWebApp`. De volledige naam van de `webAppName` met de `domainName` toegevoegd, dus in dit geval het volledige domein is `webdemowebapp.azurewebsites.net`.
- `domainName`moet worden opgegeven, zoals hierboven is weergegeven.
- `webSpaceName`moet een van de waarden die zijn gedefinieerd in de klasse [WebSpaceNames][] .
- `appServicePlanName`moet worden opgegeven, zoals hierboven is weergegeven.

> **Opmerking:** Elke keer dat u deze toepassing uitvoert, moet u de waarde van `webAppName` en `appServicePlanName` (of de web-app op de Azure-Portal te verwijderen) voordat u de toepassing opnieuw uitvoert. Anders niet kan worden uitgevoerd omdat dezelfde resource al op Azure bestaat.


#### <a name="define-the-web-creation-method"></a>De webmethode maken definiëren

Definieer vervolgens een methode voor het maken van de web app. Deze methode, `createWebApp`, de parameters van de web app en de webspace. Ook maakt en configureert u de client management Web Apps van App-Service, die wordt gedefinieerd door het [WebSiteManagementClient][] -object. De client management is het maken van Web Apps. RESTful webservices waarmee toepassingen voor het beheren van web-apps (uitvoeren van bewerkingen, zoals het maken, bijwerken en verwijderen) door het aanroepen van de API voor het servicebeheer biedt.

    private static void createWebApp() throws Exception {

        // Specify configuration settings for the App Service management client.
        Configuration config = ManagementConfiguration.configure(
            new URI(uri),
            subscriptionId,
            keyStoreLocation,  // Path to the JKS file
            keyStorePassword,  // Password for the JKS file
            KeyStoreType.jks   // Flag that you are using a JKS keystore
        );

        // Create the App Service Web Apps management client to call Azure APIs
        // and pass it the App Service management configuration object.
        WebSiteManagementClient webAppManagementClient = WebSiteManagementService.create(config);

        // Create an App Service plan for the web app with the specified parameters.
        WebHostingPlanCreateParameters appServicePlanParams = new WebHostingPlanCreateParameters();
        appServicePlanParams.setName(appServicePlanName);
        appServicePlanParams.setSKU(SkuOptions.Free);
        webAppManagementClient.getWebHostingPlansOperations().create(webSpaceName, appServicePlanParams);

        // Set webspace parameters.
        WebSiteCreateParameters.WebSpaceDetails webSpaceDetails = new WebSiteCreateParameters.WebSpaceDetails();
        webSpaceDetails.setGeoRegion(GeoRegionNames.WESTUS);
        webSpaceDetails.setPlan(WebSpacePlanNames.VIRTUALDEDICATEDPLAN);
        webSpaceDetails.setName(webSpaceName);

        // Set web app parameters.
        // Note that the server farm name takes the Azure App Service plan name.
        WebSiteCreateParameters webAppCreateParameters = new WebSiteCreateParameters();
        webAppCreateParameters.setName(webAppName);
        webAppCreateParameters.setServerFarm(appServicePlanName);
        webAppCreateParameters.setWebSpace(webSpaceDetails);

        // Set usage metrics attributes.
        WebSiteGetUsageMetricsResponse.UsageMetric usageMetric = new WebSiteGetUsageMetricsResponse.UsageMetric();
        usageMetric.setSiteMode(WebSiteMode.Basic);
        usageMetric.setComputeMode(WebSiteComputeMode.Shared);

        // Define the web app object.
        ArrayList<String> fullWebAppName = new ArrayList<String>();
        fullWebAppName.add(webAppName + domainName);
        WebSite webApp = new WebSite();
        webApp.setHostNames(fullWebAppName);

        // Create the web app.
        WebSiteCreateResponse webAppCreateResponse = webAppManagementClient.getWebSitesOperations().create(webSpaceName, webAppCreateParameters);

        // Output the HTTP status code of the response; 200 indicates the request succeeded; 4xx indicates failure.
        System.out.println("----------");
        System.out.println("Web app created - HTTP response " + webAppCreateResponse.getStatusCode() + "\n");

        // Output the name of the web app that this application created.
        String shinyNewWebAppName = webAppCreateResponse.getWebSite().getName();
        System.out.println("----------\n");
        System.out.println("Name of web app created: " + shinyNewWebAppName + "\n");
        System.out.println("----------\n");
    }

De code wordt uitgevoerd HTTP-status van de reactie aangeeft geslaagd of mislukt en als dit lukt, wordt de naam van de gemaakte web app-uitvoer.


#### <a name="define-the-main-method"></a>De methode main() definiëren

De code voor de main() die createWebApp() roept wilt maken van de web app bieden.

Tenslotte roept `createWebApp` van `main`:

        public static void main(String[] args)
            throws IOException, URISyntaxException, ServiceException,
            ParserConfigurationException, SAXException, Exception {

            // Create web app
            createWebApp();

        }  // end of main()

    }  // end of WebAppCreator class


#### <a name="run-the-application-and-verify-web-app-creation"></a>Start de toepassing en controleer of web app maken

Als u wilt controleren of uw toepassing wordt uitgevoerd, klikt u op **uitvoeren > uitvoeren**. Wanneer de toepassing actief voltooit, worden de volgende uitvoer in de console Eclips weergegeven:

    ----------
    Web app created - HTTP response 200
    
    ----------
    
    Name of web app created: WebDemoWebApp
    
    ----------

Log in op de klassieke Azure portal en klik op **Web Apps**. De nieuwe web app moet worden weergegeven in de lijst van Web Apps binnen enkele minuten.


## <a name="deploying-an-application-to-the-web-app"></a>Implementeren van een toepassing op het Web App

Nadat u hebt AzureWebDemo uitgevoerd en gemaakt van de nieuwe web app, log in op de klassieke portal op **Web Apps**, en **WebDemoWebApp** te selecteren in de lijst van **Web Apps** . Klik op **Bladeren** in de web-app dashboardpagina (of klik op de URL, `webdemowebapp.azurewebsites.net`) om te navigeren naar het. Ziet u een lege tijdelijke aanduiding pagina, omdat er geen inhoud is gepubliceerd op de web app nog.

U wordt vervolgens "Hello World"-toepassing maken en implementeren om de web app.


### <a name="create-a-jsp-hello-world-application"></a>Een JSP Hello World-toepassing maken

#### <a name="create-the-application"></a>De toepassing maken

Om aan te tonen hoe een toepassing op het web, de volgende procedure wordt beschreven hoe u een eenvoudige "Hello World" Java-toepassing maken en uploaden naar de App Service Web App gemaakt voor uw toepassing.

1. Klik op **Bestand > Nieuw > Project dynamische**. Geef deze de naam `JSPHello`. U hoeft niet alle andere instellingen in dit dialoogvenster te wijzigen. Klik op **Voltooien**.

    ![][3]

2. Vouw het **JSPHello** -project in Project Explorer, **webinhoud**met de rechtermuisknop en klik op **Nieuw > JSP-bestand**. Het nieuwe bestand in het dialoogvenster Nieuwe JSP-bestand de naam `index.jsp`. Klik op **volgende**.

3. **Nieuwe JSP-bestand (html)** selecteren in het dialoogvenster **Selecteer JSP-sjabloon** en klik op **Voltooien**.

4. Index.jsp, voeg de volgende code in de `<head>` en `<body>` code secties:

        <head>
          ...
          java.util.Date date = new java.util.Date();
        </head>
    
        <body>
          Hello, the time is <%= date %> 
        </body>


#### <a name="run-the-hello-world-application-in-localhost"></a>De toepassing HelloWorld in localhost uitgevoerd

Voordat u deze toepassing wilt uitvoeren, moet u een paar eigenschappen configureren.

1. Klik met de rechtermuisknop op het **JSPHello** -project en selecteer **Eigenschappen**.

2. In het dialoogvenster **Eigenschappen** : **Java bouwen pad**selecteren, selecteer het tabblad **volgorde en exporteren** , **JRE systeembibliotheek**controleren en klik vervolgens op **Omhoog** naar de bovenkant van de lijst te verplaatsen.

    ![][4]

3. Ook in het dialoogvenster **Eigenschappen** : **Runtimes gericht** te selecteren en klik op **Nieuw**.

4. Selecteer een server zoals **Apache Tomcat v7.0** in het dialoogvenster **Nieuwe Server Runtime-omgeving** en klik op **volgende**. In het dialoogvenster **Tomcat-Server** , stelt u de gewenste **naam** voor `Apache Tomcat v7.0`, en **Tomcat installatie Directory** ingesteld op de map waarin u de versie van Tomcat-server die u wilt gebruiken hebt geïnstalleerd.

    ![][5]

    Klik op **Voltooien**.

5. U wordt vervolgens terug naar de pagina **Gericht Runtimes** van het dialoogvenster **Eigenschappen** . **Apache Tomcat v7.0**selecteren en klik vervolgens op **OK**.

    ![][6]

6. Klik op **uitvoeren**in het menu Eclips **uitvoeren** . Selecteer **op de Server worden uitgevoerd**in het dialoogvenster **Uitvoeren als** . Selecteer in het dialoogvenster **uitvoeren op de Server** **v7.0 Tomcat Server**:

    ![][7]

    Klik op **Voltooien**.

7. Wanneer de toepassing wordt uitgevoerd, ziet u de **JSPHello** -pagina worden weergegeven in een venster localhost in Eclips (`http://localhost:8080/JSPHello/`), het volgende bericht wordt weergegeven:

    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`


#### <a name="export-the-application-as-a-war"></a>De toepassing exporteren als een WAR

De web project-bestanden exporteren als een webarchiefbestand (WAR), zodat u deze op de web app implementeren kunt. De volgende web project-bestanden bevinden zich in de map webinhoud:

    META-INF
    WEB-INF
    index.jsp

1. Klik met de rechtermuisknop op de map met webinhoud en selecteer **exporteren**.

2. Klik in het dialoogvenster **Exporteren selecteert** **Web > WAR** het bestand en klik op **volgende**.

3. In het dialoogvenster **Exporteren WAR** de src map te selecteren in het huidige project en de naam van het WAR-bestand aan het einde. Bijvoorbeeld:

    `<project-path>/JSPHello/src/JSPHello.war`

Zie voor meer informatie over het implementeren van WAR-bestanden [toevoegen een Java-toepassing naar Azure App Service Web Apps](web-sites-java-add-app.md).


### <a name="deploying-the-hello-world-application-using-ftp"></a>Implementatie van de Hello World-toepassing via FTP

Selecteer een FTP-client van derden voor het publiceren van de toepassing. Deze procedure beschrijft twee opties: de Kudu console ingebouwd in Azure; en FileZilla, een populaire tool met een handige, grafische gebruikersinterface.

> **Opmerking:** De Toolkit Azure voor Eclips implementatie opslag rekeningen en cloud services ondersteunt, maar ondersteunt geen implementatie voor web apps. U kunt opslag rekeningen en cloud services met behulp van een Project Azure-implementatie beschreven in [een Hello World-toepassing voor Azure in Eclips maken](http://msdn.microsoft.com/library/azure/hh690944.aspx), maar niet op het web apps implementeren. Andere methoden zoals FTP of GitHub bestanden overbrengen naar uw web app gebruiken.

> **Opmerking:** Via FTP vanaf de opdrachtprompt van Windows (het FTP.EXE hulpprogramma dat wordt geleverd bij Windows) wordt niet aanbevolen. FTP-clients die gebruikmaken van active FTP, zoals FTP.EXE, vaak niet via firewalls. Actieve FTP geeft een interne LAN-gebaseerde adres, waarop een FTP-server waarschijnlijk geen verbinding.

Voor meer informatie over de implementatie van een App Service web app via FTP, Zie de volgende onderwerpen:

- [Implementeren met behulp van een FTP-programma](web-sites-deploy.md)


#### <a name="set-up-deployment-credentials"></a>Implementatie referenties instellen

Zorg ervoor dat u de toepassing van de **AzureWebDemo** voor het maken van een web app hebt uitgevoerd. U zal bestanden overdragen naar deze locatie.

1. Log in op de klassieke portal en klik op **Web Apps**. Controleer of **dat webdemowebapp** wordt weergegeven in de lijst van web apps en zorg ervoor dat deze wordt uitgevoerd. Klik op **WebDemoWebApp** om het **Dashboard** pagina te openen.

2. Op de **Dashboard** -pagina **Snel bekijken**, klik op **uw implementatie referenties instellen** (als u al implementatie referenties, dit leest **uw implementatie referenties opnieuw**).

    Implementatie van referenties zijn gekoppeld aan een Microsoft-account. U moet een gebruikersnaam en wachtwoord waarmee u kunt implementeren met behulp van Git en FTP opgeven. U kunt deze referenties implementeren op een web app in Azure abonnementen aan uw Microsoft-account is gekoppeld. Referenties van Git en FTP-implementatie in het dialoogvenster en neemt u de gebruikersnaam en het wachtwoord op voor toekomstig gebruik.


#### <a name="get-ftp-connection-information"></a>FTP, verbindingsgegevens ophalen

Als u FTP wilt implementeren toepassingsbestanden naar de zojuist gemaakte web app, moet u verbindingsgegevens ophalen. Er zijn twee manieren verbindingsgegevens ophalen. Eén manier is naar de web-app **Dashboard** te gaan; de andere manier is om te downloaden van het web app van profiel publiceren. Het publicatieprofiel dat is een XML-bestand met informatie over FTP-host en het aanmelden referenties voor uw web apps in Azure App-Service. U kunt deze gebruikersnaam en dit wachtwoord implementeren op een web app in alle abonnementen die zijn gekoppeld aan de Azure account, niet alleen dit exemplaar.

FTP-verbinding om informatie te verkrijgen van de web-app blade in [Azure Portal][]:

1. Zoek onder **Essentials**, en kopieert u de **FTP-hostnaam**. Dit is een URI die vergelijkbaar is met `ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.

2. Onder **Essentials**, zoeken en **kopiëren/implementatie van FTP-gebruikersnaam**. Dit heeft de notatie *webappname\deployment gebruikersnaam*; bijvoorbeeld `WebDemoWebApp\deployer77`.

FTP-verbinding om informatie te verkrijgen van het publicatieprofiel dat:

1. Klik op **Get profiel publiceren**in de web-app-blade. Dit wordt een .publishsettings-bestand downloaden naar uw lokale schijf.

2. Open het bestand .publishsettings in een XML-editor of teksteditor en zoek de `<publishProfile>` element waarin `publishMethod="FTP"`. Het ziet er als volgt uit:

        <publishProfile
            profileName="WebDemoWebApp - FTP"
            publishMethod="FTP"
            publishUrl="ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net/site/wwwroot"
            ftpPassiveMode="True"
            userName="WebDemoWebApp\$WebDemoWebApp"
            userPWD="<deployment-password>"
            ...
        </publishProfile>

3. Houd er rekening mee dat de web-app `publishProfile` instellingen toewijzen aan de sitebeheerder FileZilla-instellingen als volgt:

- `publishUrl`is hetzelfde als de **naam van de FTP-host**, de waarde die u in een **Host**.
- `publishMethod="FTP"`betekent dit **Protocol** in te stellen op de **FTP - File Transfer Protocol**en **codering** voor de **gewone FTP gebruiken**.
- `userName`en `userPWD` zijn de sleutels voor de werkelijke gebruikersnaam en wachtwoord-waarden die u hebt opgegeven bij het herstellen van de referenties van de implementatie. `userName`is hetzelfde als **implementatie / gebruiker van de FTP-**. Ze toewijzen aan de **gebruiker** en het **wachtwoord** in FileZilla.
- `ftpPassiveMode="True"`betekent dit dat de FTP-site maakt gebruik van passieve FTP-overdracht; Selecteer op het tabblad **Instellingen voor** **passieve** .


#### <a name="configure-the-web-app-to-host-a-java-application"></a>De webtoepassing als host voor een Java-toepassing configureren

Voordat u de toepassing uitgeeft, moet u enkele instellingen wijzigen zodat de web app een Java-toepassing biedt.

1. In de klassieke portal naar de pagina van de web-app **Dashboard** en klik op **configureren**. Geef de volgende instellingen op de pagina **configureren** .

2. In **Java versie** is standaard **uitgeschakeld**. Selecteer de versie van Java uw doelen van toepassing; bijvoorbeeld 1.7.0_51. Nadat u dit hebt gedaan, ook voor zorgen dat **Web-container** is ingesteld op een versie van Tomcat-Server.

3. In **Documenten standaard**index.jsp toevoegen en verplaatsen naar het begin van de lijst. (Het standaardbestand voor web apps is hostingstart.html.)

4. Klik op **Opslaan**.


#### <a name="publish-your-application-using-kudu"></a>Publiceer de toepassing met behulp van Kudu

De Kudu debug console ingebouwd in Azure is één methode voor het publiceren van de toepassing. Kudu bekend is stabiel en consistent met de App Service Web Apps en Tomcat-Server. U toegang tot de console voor de web app door te bladeren naar een URL van het volgende formulier:

`https://<webappname>.scm.azurewebsites.net/DebugConsole`

1. Voor deze procedure bevindt de Kudu console zich op de volgende URL; Ga naar deze locatie:

    `https://webdemowebapp.scm.azurewebsites.net/DebugConsole`

2. Selecteer in het bovenste menu **Foutopsporingsconsole > CMD**.

3. In de console opdrachtregel, gaat u naar `/site/wwwroot` (of klik op `site`, vervolgens `wwwroot` in de mapweergave boven aan de pagina):

    `cd /site/wwwroot`

4. Nadat u **Java-versie**hebt opgegeven, moet de Tomcat-server een map webapps maken. Ga naar de map webapps op de opdrachtregel van de console:

    `mkdir webapps`

    `cd webapps`

5. Sleep JSPHello.war van `<project-path>/JSPHello/src/` en zet deze neer in de weergave Kudu directory onder `/site/wwwroot/webapps`. Sleep niet deze naar het gebied "Sleep hier om te uploaden en zip" omdat Tomcat wordt behouden.

  ![][8]

Bij de eerste JSPHello.war wordt weergegeven in de map zelf:

  ![][9]

Tomcat Server zal het WAR-bestand uitpakken naar een map met uitgepakte JSPHello in korte tijd (waarschijnlijk minder dan 5 minuten). Klik op de hoofdmap of index.jsp zijn uitgepakt en gekopieerd. Zo ja, Ga terug naar de map webapps om te zien of de map met uitgepakte JSPHello is gemaakt. Als u deze items niet ziet, wacht en Herhaal.

  ![][10]


#### <a name="publish-your-application-using-filezilla-optional"></a>Publiceer de toepassing met behulp van FileZilla (optioneel)

Een ander hulpprogramma voor het publiceren van de toepassing is FileZilla, een populaire FTP-client van derden met een handige, grafische gebruikersinterface. U kunt downloaden en installeren van FileZilla vanaf [http://filezilla-project.org/](http://filezilla-project.org/) als u nog geen het. Zie de [documentatie van FileZilla](https://wiki.filezilla-project.org/Documentation) en dit blogbericht op voor meer informatie over het gebruik van de client [FTP-Clients - deel 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).

1. In FileZilla, klikt u op **Bestand > Site Manager**.
2. Klik in het dialoogvenster **Site Manager** klikt u op de **Nieuwe Site**. Een nieuwe, lege FTP-site wordt weergegeven in de **Post selecteren** waarin u een naam op te geven. Voor deze procedure, geef deze de naam `AzureWebDemo-FTP`.

    Geef op het tabblad **Algemeen** de volgende instellingen:
    - **Host:** Voer de **Naam van de FTP-Host** die u hebt gekopieerd uit het dashboard.
    - **Poort:** (Laat dit veld leeg is, zoals dit een passieve overdracht is en de server de poort bepaalt te gebruiken.)
    - **Protocol:** FTP File Transfer Protocol
    - **Codering:** De gewone FTP gebruiken
    - **Type aanmelding:** Normaal
    - **Gebruiker:** Voer de installatie / FTP-gebruiker die u hebt gekopieerd uit het dashboard. Dit is de volledige FTP-gebruikersnaam, met het formulier *webappname\username*.
    - **Wachtwoord:** Voer het wachtwoord dat u hebt opgegeven bij het instellen van de referenties van de implementatie.

    Selecteer **passieve**op het tabblad **Instellingen** .

3. Klik op **verbinden**. Als geslaagd, FileZilla de console wordt weergegeven in een `Status: Connected` bericht en probleem een `LIST` opdracht de inhoud van de directory.

4. In het deelvenster **lokale** website, selecteert u de bronmap waarin het bestand JSPHello.war zich bevindt; het pad moet de volgende strekking:

    `<project-path>/JSPHello/src/`

5. Selecteer de doelmap in het deelvenster **externe** website. Implementeert u het WAR-bestand in de `webapps` map in de hoofdmap van de webtoepassing. Ga naar `/site/wwwroot`, met de rechtermuisknop op `wwwroot`, en selecteer **de map maken**. Naam van de map `webapps` en geef deze map.

6. Overzetten van JSPHello.war naar `/site/wwwroot/webapps`. JSPHello.war in de **lokale** lijst selecteren, klik met de rechtermuisknop op het en selecteer **uploaden**. U ziet nu het lijkt `/site/wwwroot/webapps`.

7. Nadat u JSPHello.war naar de map webapps kopieert, Tomcat-Server automatisch worden uitgepakt (uitpakken) de bestanden in het WAR-bestand. Hoewel Tomcat Server begint met het uitpakken van de bijna onmiddellijk, het duurt een lange tijd (eventueel uren) voor de bestanden worden weergegeven in de FTP-client.


#### <a name="run-the-hello-world-application-on-the-web-app"></a>De toepassing HelloWorld uitvoeren op het Web App

1. Nadat u hebt het WAR-bestand wordt geüpload en gecontroleerd dat Tomcat-server is gemaakt met een uitgepakte `JSPHello` map, Ga naar `http://webdemowebapp.azurewebsites.net/JSPHello` voor de toepassing.

    > **Opmerking:** Als u vanuit de klassieke portal op **Bladeren** klikt, kunt u krijgen de webpagina standaard zeggen "deze Java gebaseerd webtoepassing is gemaakt." Mogelijk moet de webpagina wilt bekijken van de uitvoer van de toepassing in plaats van de standaard webpagina vernieuwen.

2. Wanneer de toepassing wordt uitgevoerd, ziet u een webpagina met de volgende uitvoer:

    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`


#### <a name="clean-up-azure-resources"></a>Opschonen Azure resources

Deze procedure maakt een web App Service app. U wordt gefactureerd voor de resource als deze bestaat. Tenzij u van plan bent om door te gaan met de web app voor proefneming of ontwikkeling, moet u overwegen stoppen of verwijderen. Een web app dat is gestopt wordt een kleine vergoeding nog steeds oplopen, maar u kunt deze op elk gewenst moment opnieuw. Wist u alle gegevens die u hebt geüpload naar het verwijderen van een web app.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

  [1]: ./media/java-create-azure-website-using-java-sdk/eclipse-maven-repositories-rebuild-index.png
  [2]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-java-class.png
  [3]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-dynamic-web-project.png
  [4]: ./media/java-create-azure-website-using-java-sdk/eclipse-java-build-path.png
  [5]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-tomcat-server.png
  [6]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-properties-page.png
  [7]: ./media/java-create-azure-website-using-java-sdk/eclipse-run-on-server.png
  [8]: ./media/java-create-azure-website-using-java-sdk/kudu-console-drag-drop.png
  [9]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-1.png
  [10]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-2.png
 

[Azure App-Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Web Platform Installer]: http://go.microsoft.com/fwlink/?LinkID=252838
[Azure Toolkit voor Eclips]: https://msdn.microsoft.com/library/azure/hh690946.aspx
[Azure klassieke portal]: https://manage.windowsazure.com
[Wat is een directory Azure AD]: http://technet.microsoft.com/library/jj573650.aspx
[Maken en uploaden van een certificaat voor Azure]: ../cloud-services/cloud-services-certs-create.md
[Sleutel en certificaat Management Tool (hulpprogramma Keytool gebruikt)]: http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html
[WebSiteManagementClient]: http://azure.github.io/azure-sdk-for-java/com/microsoft/azure/management/websites/WebSiteManagementClient.html
[WebSpaceNames]: http://dl.windowsazure.com/javadoc/com/microsoft/windowsazure/management/websites/models/WebSpaceNames.html
[Azure Portal]: https://portal.azure.com
