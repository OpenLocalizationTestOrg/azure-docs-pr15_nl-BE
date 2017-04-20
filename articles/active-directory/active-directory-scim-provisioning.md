<properties
    pageTitle="Inschakelen automatisch inrichten van gebruikers en groepen van Azure Active Directory voor toepassingen met SCIM | Microsoft Azure"
    description="Azure Active Directory kan automatisch gegevens leveren voor gebruikers en groepen aan toepassing of identiteit-archief dat is fronted door een webservice met de interface die is gedefinieerd in de specificatie SCIM protocol"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="using-scim-to-enable-automatic-provisioning-of-users-and-groups-from-azure-active-directory-to-applications"></a>Met behulp van SCIM automatisch inrichten van gebruikers en groepen van Azure Active Directory voor toepassingen inschakelen

##<a name="overview"></a>Overzicht

Azure Active Directory kan automatisch gegevens leveren voor gebruikers en groepen aan toepassing of identiteit-archief dat is fronted door een webservice met de interface die is gedefinieerd in de [specificatie 2.0 SCIM-protocol](https://tools.ietf.org/html/draft-ietf-scim-api-19). Azure Active Directory kunt verzenden van aanvragen voor het maken, wijzigen en verwijderen toegewezen gebruikers en groepen voor deze webservice, die vervolgens op aanvragen in bewerkingen bij de winkel van de identiteit doel kunt vertalen. 

![][1]
*Afbeelding: Inrichten van Azure Active Directory naar een winkel identiteit via een webservice*

Deze mogelijkheid kan worden gebruikt in combinatie met de mogelijkheid van '[eigen app brengen](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)' in Azure AD-on en Automatische gebruikersaanvragen voor toepassingen die op of zijn door een webservice SCIM fronted inschakelen.

Er zijn twee use-cases voor SCIM in Azure Active Directory:

* **Inrichten van gebruikers en groepen voor toepassingen die ondersteuning voor SCIM** - toepassingen die SCIM 2.0 ondersteunen en OAuth aan toonder tokens gebruiken voor verificatie, werkt met Azure AD van het vak.

* **Maak uw eigen provisioning oplossing voor toepassingen die andere inrichten op basis van API ondersteuning** - voor niet-SCIM toepassingen, kunt u een eindpunt SCIM vertalen tussen Azure AD SCIM eindpunt en ongeacht de toepassing-API ondersteunt voor de gebruiker wordt ingericht.  Steun bij de ontwikkeling van een eindpunt SCIM, bieden wij CLI bibliotheken en codevoorbeelden waarin wordt beschreven hoe u SCIM berichten te vertalen en een eindpunt SCIM bieden.  

##<a name="provisioning-users-and-groups-to-applications-that-support-scim"></a>Inrichten van gebruikers en groepen van toepassingen die ondersteuning bieden voor SCIM

Azure Active Directory kan worden geconfigureerd voor automatisch voorziening toegewezen gebruikers en groepen die moeten worden geïmplementeerd, een [systeem voor het interdomein - Identity Management (SCIM) 2](https://tools.ietf.org/html/draft-ietf-scim-api-19) Web service en OAuth aan toonder tokens voor verificatie accepteren. In de specificatie 2.0 SCIM toepassingen voldoen aan deze vereisten:

* Ondersteunt het maken van gebruikers en/of groepen, zoals bepaald in punt 3.3 van de SCIM-protocol.  

* Ondersteunt het wijzigen van gebruikers en/of groepen met patch aanvragen zoals bepaald in punt 3.5.2 van het SCIM-protocol.  

* Ondersteunt het ophalen van een bekende bron zoals bepaald in punt 3.4.1 van de SCIM-protocol.  

*  Ondersteunt bij het controleren van gebruikers als groepen, zoals bepaald in punt 3.4.2 van het SCIM-protocol.  Standaard, wordt gebruikers gevraagd door externalId en groepen worden ondervraagd door displayName.  

* Ondersteunt het gebruiker-ID en -beheer zoals bepaald in punt 3.4.2 van het protocol SCIM opvragen.  

* Ondersteunt groepen door-ID en door de lidstaten zoals bepaald in punt 3.4.2 van het protocol SCIM opvragen.  

* OAuth aan toonder tokens voor een vergunning zoals bepaald in punt 2.1 van het protocol SCIM accepteert.

Raadpleeg uw toepassingsprovider of de documentatie van de toepassingsprovider van uw voor overzichten van de compatibiliteit met deze vereisten.
 
###<a name="getting-started"></a>Aan de slag

Toepassingen die ondersteuning bieden voor de hierboven beschreven SCIM-profiel kunnen worden verbonden met Azure Active Directory met de functie 'custom' app in de galerie met Azure AD toepassing. Zodra verbonden, voert Azure AD een synchronisatieproces om de 5 minuten waar het SCIM-eindpunt van de toepassing voor toegewezen gebruikers en groepen, query's en maakt of wijzigt deze overeenkomstig de toewijzing.

**Verbinding maken met een toepassing die SCIM ondersteunt:**

1.  In een webbrowser starten de Azure management portal op https://manage.windowsazure.com.
2.  Ga naar **Active Directory > Directory > [uw map] > toepassingen**, en selecteer **toevoegen > een toepassing toevoegen uit de galerie met**.
3.  Klik op het tabblad **aangepast** aan de linkerkant, voer een naam voor de toepassing en klikt u op het vinkje om een app-object te maken.

![][2]

4.  Selecteer de tweede knop voor het **inrichten van de account configureren** in het resulterende scherm.
5.  Voer in het veld **Ingericht eindpunt-URL** de URL van SCIM-eindpunt van de toepassing.
6.  Als het eindpunt SCIM een token OAuth aan toonder uit een uitgevende instelling dan AD Azure vereist, Kopieer de vereiste OAuth aan toonder token in het veld **Verificatie-Token (optioneel)** . Is dit veld leeg en vervolgens AD Azure een OAuth aan toonder token verleend van Azure AD elke aanvraag worden opgenomen. Apps die Azure AD gebruiken als een idenity-provider kan deze Azure AD valideren-token verleend.
7.  Klik op **volgende**en klik op de knop **Test starten** met Azure Active Directory probeert te verbinden met het eindpunt SCIM. Als de pogingen mislukken, wordt diagnostische gegevens weergegeven.  
8.  Als **de verbinding probeert te maken met het succesvol toepassing Klik op** de resterende schermen en klik op **Voltooien** om het dialoogvenster te sluiten.
9.  Selecteer de derde **Accounts toewijzen** in het resulterende scherm. Toewijzen in de resulterende sectie gebruikers en groepen, gebruikers of groepen die u wilt om in te richten naar de toepassing.
10. Zodra de gebruikers en groepen zijn toegewezen, klikt u op het tabblad **configureren** in de buurt van de bovenkant van het scherm.
11. Onder **Account inrichten**, bevestigen dat de instelling op. 
12. Klik op **opnieuw opstarten ingericht account** aan het begin van het inrichtingsproces onder **Extra**.

Houd er rekening mee dat 5-10 minuten verstrijken mag voordat het inrichtingsproces aanvragen verzenden naar het eindpunt SCIM begint.  Een overzicht van de verbindingspogingen is beschikbaar op de Dashboard-tab van de toepassing en een rapport van provisioning activiteit zowel provisioning fouten kunnen worden gedownload van de map op het tabblad rapporten.

##<a name="building-your-own-provisioning-solution-for-any-application"></a>Bouwen van uw eigen oplossing voor elke toepassing inrichten

Als u een webservice voor SCIM die interfaces met Azure Active Directory, kunt u single sign-on en Automatische gebruikersaanvragen voor vrijwel elke toepassing waarmee de gebruiker REST of SOAP API wordt ingericht.

Hier ziet u hoe het werkt:

1.  AD Azure biedt een gemeenschappelijke taal infrastructuur bibliotheek met de naam [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/). Systeemintegrators en ontwikkelaars kunnen deze bibliotheek maken en implementeren van een SCIM-gebaseerd web service-eindpunt verbinding Azure AD naar identiteit-winkel van een toepassing kan gebruiken.
2.  Toewijzingen worden in de webservice voor het schema voor de gestandaardiseerde toewijzen aan de gebruikersschema en het protocol vereist door de toepassing geïmplementeerd.
3.  De eindpunt-URL is geregistreerd in Azure AD als onderdeel van een aangepaste toepassing in de galerie van toepassing.
4.  Gebruikers en groepen zijn toegewezen aan deze toepassing in Azure AD. Bij de toewijzing, worden ze in een wachtrij moeten worden gesynchroniseerd naar de doeltoepassing geplaatst. Het synchronisatieproces verwerking van de wachtrij wordt uitgevoerd om de 5 minuten.

###<a name="code-samples"></a>Codevoorbeelden

Om dit proces te vergemakkelijken, zijn een aantal [voorbeelden van code](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) die een SCIM web-service-eindpunt te maken en tonen automatisch inrichten. Een voorbeeld is van een provider die wordt onderhouden door een bestand met door komma's gescheiden waarden die gebruikers en groepen rijen.  De andere is van de provider die wordt toegepast op de Amazon Web Services Identity and Access Management-service.  

**Vereisten**

* Visual Studio 2013 of later
* [Azure SDK voor .NET](https://azure.microsoft.com/downloads/)
* Windows-computer waarop de ASP.NET framework 4.5 moet worden gebruikt als het eindpunt SCIM. Deze computer moet toegankelijk zijn vanuit de cloud
* [Een Azure met een proefperiode of gelicentieerde versie van Azure AD Premium-abonnement](https://azure.microsoft.com/services/active-directory/)
* Het Amazon AWS bibliotheken van de [Toolkit voor Visual Studio (AWS)](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html)vereist. Zie het Leesmij-bestand bij het monster voor meer informatie

###<a name="getting-started"></a>Aan de slag

De eenvoudigste manier voor het implementeren van een eindpunt SCIM waarin leveringsaanvragen uit Azure Active Directory is kunnen bouwen en implementeren van het voorbeeld dat de ingerichte gebruikers naar een bestand met door komma's gescheiden waarden (CSV) oplevert.

**Voor het maken van een monster SCIM-eindpunt:**

1.  De code monster downloaden op [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)
2.  Unzip het pakket en plaatst u deze op uw Windows-computer op een locatie zoals C:\AzureAD-BYOA-Provisioning-Samples\.
3.  Starten in deze map, de FileProvisioningAgent-oplossing in Visual Studio.
4.  Selecteer **Extra > Library Package Manager > Package Manager-Console**, en de opdrachten onder voor het project FileProvisioningAgent op te lossen de oplossing verwijzingen uit te voeren:

    Installatie pakket installatiepakket Microsoft.SystemForCrossDomainIdentityManagement Microsoft.IdentityModel.Clients.ActiveDirectory-installatiepakket installatiepakket Microsoft.Owin.Diagnostics Microsoft.Owin.Host.SystemWeb

5.  Compileer het project FileProvisioningAgent.
6.  Start de opdrachtprompt in Windows-toepassing (zoals beheerder) en gebruik de opdracht **cd** naar de map wijzigen in de map **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** .
7.  Voer de opdracht hieronder < IP-adres > vervangt door de naam of het IP-domein van de Windows-computer.

    FileAgnt.exe http://<ip-address>:9000 TargetFile.csv

8.  In Windows onder **Windows-instellingen > netwerk- en Internet-instellingen**, selecteer het **Windows Firewall > Geavanceerde instellingen**, en het maken van een **Binnenkomende regel** waarmee binnenkomende toegang tot poort 9000.
9.  Als de Windows-computer zich achter een router, moet de router worden geconfigureerd voor het uitvoeren van Network Access vertaling tussen de poort 9000 die toegankelijk is via internet en 9000-poort op de Windows-computer. Dit is vereist voor Azure advertentie kunnen toegang krijgen tot dit eindpunt in de cloud.


**Het monster SCIM eindpunt in Azure AD registreren:**

1.  In een webbrowser starten de Azure management portal op https://manage.windowsazure.com.
2.  Ga naar **Active Directory > Directory > [uw map] > toepassingen**, en selecteer **toevoegen > een toepassing toevoegen uit de galerie met**.
3.  Selecteer het tabblad **aangepast** aan de linkerkant, voer een naam zoals "App SCIM Test" en klikt u op het pictogram vinkje u maakt een object app. Merk op dat het application-object is gemaakt wilt geven het doel app u zou aan het inrichten en uitvoering van eenmalige aanmelding voor en niet alleen het eindpunt SCIM.

![][2]

4.  Selecteer de tweede knop voor het **inrichten van de account configureren** in het resulterende scherm.
5.  Voer in het dialoogvenster de blootgesteld internet URL en de poort van uw eindpunt SCIM. Is dit iets zoals http://testmachine.contoso.com:9000 of http://<ip-address>:9000/, waarbij < IP-adres > staat voor het internet beschikbaar IP adres.  
6.  Klik op **volgende**en klik op de knop **Test starten** met Azure Active Directory probeert te verbinden met het eindpunt SCIM. Als de pogingen mislukken, wordt diagnostische gegevens weergegeven.  
7.  Als de verbinding probeert te maken met uw webservice is gelukt, klikt u op **volgende** in de overige schermen en klik op **Voltooien** om het dialoogvenster te sluiten.
8.  Selecteer de derde **Accounts toewijzen** in het resulterende scherm. Toewijzen in de resulterende sectie gebruikers en groepen, gebruikers of groepen die u wilt om in te richten naar de toepassing.
9.  Zodra de gebruikers en groepen zijn toegewezen, klikt u op het tabblad **configureren** in de buurt van de bovenkant van het scherm.
10. Onder **Account inrichten**, bevestigen dat de instelling op. 
11. Klik op **opnieuw opstarten ingericht account** aan het begin van het inrichtingsproces onder **Extra**.

Houd er rekening mee dat 5-10 minuten verstrijken mag voordat het inrichtingsproces aanvragen verzenden naar het eindpunt SCIM begint.  Een overzicht van de verbindingspogingen is beschikbaar op de Dashboard-tab van de toepassing en een rapport van provisioning activiteit zowel provisioning fouten kunnen worden gedownload van de map op het tabblad rapporten.

Open het bestand TargetFile.csv in de map \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug op uw Windows-computer is de laatste stap in het monster te controleren. Zodra het inrichtingsproces wordt uitgevoerd, bevat dit bestand de gegevens van alle toegewezen en ingericht voor gebruikers en groepen.

###<a name="development-libraries"></a>Ontwikkeling bibliotheken

Voor het ontwikkelen van uw eigen Web-service die aan de specificatie SCIM voldoet eerst vertrouwd met de volgende bibliotheken geleverd door Microsoft om het ontwikkelproces te versnellen: 

**1:**  Common Language Infrastructure-bibliotheken zijn beschikbaar voor gebruik met talen op basis van die infrastructuur, zoals C#.  Een van deze bibliotheken, [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), verklaart dat een interface, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, dat is weergegeven in de onderstaande figuur.  Een ontwikkelaar die met behulp van de bibliotheken zouden implementeren die interface aan een klasse die kan worden verwezen, algemeen, als een provider.  De bibliotheken het ontwikkelaars gemakkelijk implementeren een webservice die voldoet aan de specificatie SCIM inschakelen, hetzij in Internet Information Services of uitvoerbare Common Language Infrastructure assembly's die worden gehost.  Verzoeken aan die webservice worden vertaald in het aanroepen van de methoden van de provider, die door de ontwikkelaar zou worden geprogrammeerd om te rijden op een winkel van de identiteit.    

![][3]

**2:** [Express route handlers](http://expressjs.com/guide/routing.html) voor het parseren van node.js aanvraag objecten beschikbaar zijn die gesprekken (zoals gedefinieerd door de specificatie SCIM), aangebracht in een node.js webservice.     

###<a name="building-a-custom-scim-endpoint"></a>Bouwen van een aangepaste SCIM eindpunt

Met behulp van de hierboven beschreven bibliotheken kunnen ontwikkelaars die bibliotheken met hun diensten binnen de uitvoerbare Common Language Infrastructure assembly's, of Internet Information Services hosten.  Hier is voorbeeldcode voor het hosten van een service binnen een uitvoerbare assembly, op het adres http://localhost:9000: 

    private static void Main(string[] arguments)
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IProvider and 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  
    
    Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
      new DevelopersMonitor();
    Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
      new DevelopersProvider(arguments[1]);
    Microsoft.SystemForCrossDomainIdentityManagement.Service webService = null;
    try
    {
        webService = new WebService(monitor, provider);
        webService.Start("http://localhost:9000");

        Console.ReadKey(true);
    }
    finally
    {
        if (webService != null)
        {
            webService.Dispose();
            webService = null;
        }
    }
    }

    public class WebService : Microsoft.SystemForCrossDomainIdentityManagement.Service
    {
    private Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor;
    private Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider;

    public WebService(
      Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitoringBehavior, 
      Microsoft.SystemForCrossDomainIdentityManagement.IProvider providerBehavior)
    {
        this.monitor = monitoringBehavior;
        this.provider = providerBehavior;
    }

    public override IMonitor MonitoringBehavior
    {
        get
        {
            return this.monitor;
        }

        set
        {
            this.monitor = value;
        }
    }

    public override IProvider ProviderBehavior
    {
        get
        {
            return this.provider;
        }

        set
        {
            this.provider = value;
        }
    }
    }

Het is belangrijk dat deze service moet beschikken over een HTTP-adres en server certificaat voor clientverificatie dat de basiscertificeringsinstantie een van de volgende is: 

* CNNIC
* Comodo
* CyberTrust
* DigiCert
* GeoTrust
* GlobalSign
* Komen jullie gaan
* VeriSign
* WoSign

Een certificaat voor serververificatie kan worden gebonden aan een poort op een Windows-host met behulp van het onderdeel netwerk-shell als volgt: 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  
 
De waarde voor het argument certhash is hier de blauwdruk van het certificaat, terwijl de waarde voor het argument appid een willekeurige globaal unieke id is.  

Als host voor de service in Internet Information Services, zou een ontwikkelaar een assembly Common Language Infrastructure code bibliotheek bouwen met een klasse met de naam opstarten in de standaardnaamruimte van de algemene vergadering.  Hier volgt een voorbeeld van een dergelijke klasse: 

    public class Startup
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor and  
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter starter;

    public Startup()
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
          new DevelopersMonitor();
        Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
          new DevelopersProvider();
        this.starter = 
          new Microsoft.SystemForCrossDomainIdentityManagement.WebApplicationStarter(
            provider, 
            monitor);
    }

    public void Configuration(
      Owin.IAppBuilder builder) // Defined in in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

###<a name="handling-endpoint-authentication"></a>Eindpunt-verificatie

Aanvragen van Azure Active Directory bevatten een token OAuth 2.0 aan toonder.   Elke service die de aanvraag ontvangt moet de uitgevende instelling ten behoeve van de verwachte Azure Active Directory huurder voor toegang tot het Graph-webservice Azure Active Directory als Azure Active Directory te verifiëren.  In het token, de uitgevende instelling wordt geïdentificeerd door een claim in het iss, zoals 'iss': 'https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/'.  In dit voorbeeld wordt het basisadres van de waarde van de claim, https://sts.windows.net, Azure Active Directory als de uitgevende instelling, geeft het relatieve adres segment, cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, is een unieke id van de Active Directory Azure huurder die het token is uitgegeven.  Als het token is uitgegeven voor de toegang tot de Azure Active Directory grafiek webservice, moet de id van die dienst, 00000002-0000-0000-c000-000000000000, worden in de waarde van het token van aud claim.  

Ontwikkelaars die de Common Language Infrastructure bibliotheken voor het bouwen van een SCIM-service door Microsoft geleverde kunnen verifiëren aanvragen van Azure Active Directory met behulp van het pakket Microsoft.Owin.Security.ActiveDirectory door de volgende stappen: 

**1:**  Implementeren in een provider, de eigenschap Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior van het resultaat van een methode die wordt aangeroepen wanneer de service wordt gestart: 

    public override Action\<Owin.IAppBuilder, System.Web.Http.HttpConfiguration.HttpConfiguration\> StartupBehavior
    {
      get
      {
        return this.OnServiceStartup;
      }
    }

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder,  // Defined in Owin.dll.  
      System.Web.Http.HttpConfiguration configuration)  // Defined in System.Web.Http.dll.  
    {
    }

**2:**  Voeg de volgende code met deze methode wordt een aanvraag voor een geverifieerd als een token dat is uitgegeven door Azure Active Directory voor een opgegeven huurder, voor toegang tot de webservice van Azure Active Directory grafiek voorzien van de service-endpoints: 

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder IAppBuilder applicationBuilder, 
      System.Web.Http.HttpConfiguration HttpConfiguration configuration)
    {
      // IFilter is defined in System.Web.Http.dll.  
      System.Web.Http.Filters.IFilter authorizationFilter = 
        new System.Web.Http.AuthorizeAttribute(); // Defined in System.Web.Http.dll.configuration.Filters.Add(authorizationFilter);

      // SystemIdentityModel.Tokens.TokenValidationParameters is defined in    
      // System.IdentityModel.Token.Jwt.dll.
      SystemIdentityModel.Tokens.TokenValidationParameters tokenValidationParameters =     
        new TokenValidationParameters()
        {
          ValidAudience = "00000002-0000-0000-c000-000000000000"
        };

      // WindowsAzureActiveDirectoryBearerAuthenticationOptions is defined in 
      // Microsoft.Owin.Security.ActiveDirectory.dll
      Microsoft.Owin.Security.ActiveDirectory.
      WindowsAzureActiveDirectoryBearerAuthenticationOptions authenticationOptions =
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions()    {
        TokenValidationParameters = tokenValidationParameters,
        Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute the appropriate tenant’s 
                                                      // identifier for this one.  
      };

      applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
    }

##<a name="user-and-group-schema"></a>Gebruiker en groep Schema

Azure Active Directory kunnen twee typen resources met webservices SCIM inrichten.  Deze typen resources zijn de gebruikers en groepen.  

Gebruikersresources worden aangeduid met de schema-id urn: ietf:params:scim:schemas:extension:enterprise:2.0:User, dat van dit protocolspecificatie uitmaakt deel: http://tools.ietf.org/html/draft-ietf-scim-core-schema.  De standaardtoewijzing van de kenmerken van de gebruikers in Azure Active Directory in de kenmerken van de urn: ietf:params:scim:schemas:extension:enterprise:2.0:User hieronder vindt u in tabel 1.  

Groep resources worden aangeduid met de schema-id http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  Tabel 2 hieronder ziet u de standaard-koppeling van de kenmerken van de groepen in Azure Active Directory in de kenmerken van de http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  

###<a name="table-1-default-user-attribute-mapping"></a>Tabel 1: Standaard kenmerk Gebruikerskoppeling

| Azure Active Directory-gebruiker | urn: ietf:params:scim:schemas:extension:enterprise:2.0:User |
| ------------- | ------------- |
| IsSoftDeleted | actieve |
| displayName | displayName |
| Fax TelephoneNumber | phoneNumbers type eq 'fax'.value |
| givenName | name.givenName |
| Functie | titel |
| e-mail | .value e-mails type eq "werk" |
| mailNickname | externalId |
| Manager | Manager |
| mobiel | phoneNumbers type eq 'mobiele'.value |
| object-id | ID |
| Postcode | type eq "werk" adressen .postalCode |
| proxy-adressen | e-mails [Typ eq 'andere']. Waarde |
| fysieke-levering-OfficeName | adressen [Typ eq 'andere']. Opgemaakt |
| streetAddress | type eq "werk" adressen .streetAddress |
| Achternaam | name.familyName |
| Telefoonnummer | phoneNumbers type eq "werk".value |
| gebruiker-PrincipalName | Gebruikersnaam |


###<a name="table-2-default-group-attribute-mapping"></a>Tabel 2: Standaard groep kenmerktoewijzing

| Azure Active Directory-groep | http://schemas.Microsoft.com/2006/11/ResourceManagement/ADSCIM/Group |
| ------------- | ------------- |
| displayName | externalId |
| e-mail | .value e-mails type eq "werk" |
| mailNickname | displayName |
| leden | leden |
| object-id | ID |
| proxyAddresses | e-mails [Typ eq 'andere']. Waarde |


##<a name="user-provisioning-and-de-provisioning"></a>Gebruikersaanvragen en verwijderen van gegevens

In de afbeelding hieronder ziet u de berichten dat Azure Active Directory wordt verzonden naar een SCIM-service voor het beheren van de levenscyclus van een gebruiker in een andere identiteit opgeslagen.  Het diagram toont ook hoe een SCIM-service geïmplementeerd met behulp van de Common Language Infrastructure bibliotheken voor het bouwen van dergelijke diensten door Microsoft geleverd die aanvragen vertalen in aanroepen van de methoden van een provider.  

![][4]
*Afbeelding: Gebruikersaanvragen en reeks verwijderen van gegevens*

**1:**  Azure Active Directory wordt de service voor een gebruiker een query met een waarde van het kenmerk externalId overeenkomt met de waarde van het kenmerk mailNickname van een gebruiker in Azure Active Directory.  De query zal worden uitgedrukt als een aanvraag voor Hypertext Transfer Protocol zoals deze, waarin jyoung een voorbeeld van een mailNickname van een gebruiker in Azure Active Directory is: 

    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...

Als de service is gemaakt met behulp van de Common Language Infrastructure-bibliotheken die Microsoft voor het implementeren van SCIM-services, wordt de aanvraag worden omgezet in een aanroep van de methode Query van de provider van de service.  Hier wordt de handtekening van deze methode: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]> Query(
      Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
      string correlationIdentifier);

Hier is de definitie van de Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters-interface: 

    public interface IQueryParameters: 
      Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
        System.Collections.Generic.IReadOnlyCollection <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
        { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
      system.Collections.Generic.IReadOnlyCollection<string> ExcludedAttributePaths 
      { get; }
      System.Collections.Generic.IReadOnlyCollection<string> RequestedAttributePaths 
      { get; }
      string SchemaIdentifier 
      { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IFilter
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IFilter AdditionalFilter 
          { get; set; }
        string AttributePath 
          { get; } 
        Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator FilterOperator 
          { get; }
        string ComparisonValue 
          { get; }
    }
    
    public enum Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator
    {
        Equals
    }

In het geval van het voorgaande voorbeeld van een query voor een gebruiker met een bepaalde waarde voor het kenmerk externalId is waarden van de argumenten doorgegeven aan de methode Query als volgt: 

* parameters. AlternateFilters.Count: 1
* parameters. AlternateFilters.ElementAt(0). AttributePath: "externalId"
* parameters. AlternateFilters.ElementAt(0). Vergelijkingsoperator: ComparisonOperator.Equals
* parameters. AlternateFilter.ElementAt(0). ComparisonValue: "jyoung"
* correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin. Aanvraag-id'] 

**2:**  Als het antwoord op een query met de service voor een gebruiker met een waarde van het kenmerk externalId overeenkomt met de waarde van het kenmerk mailNickname van een gebruiker in Azure Active Directory niet alle gebruikers, vervolgens Azure Active Directory vraagt om de service gegevens leveren voor een gebruiker die overeenkomt met een in Azure Active Directory.  Hier volgt een voorbeeld van een dergelijk verzoek: 

    POST https://.../scim/Users HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas":
      [
        "urn:ietf:params:scim:schemas:core:2.0:User",
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0User"],
      "externalId":"jyoung",
      "userName":"jyoung",
      "active":true,
      "addresses":null,
      "displayName":"Joy Young",
      "emails": [
        {
          "type":"work",
          "value":"jyoung@Contoso.com",
          "primary":true}],
      "meta": {
        "resourceType":"User"},
       "name":{
        "familyName":"Young",
        "givenName":"Joy"},
      "phoneNumbers":null,
      "preferredLanguage":null,
      "title":null,
      "department":null,
      "manager":null}

De Common Language Infrastructure-bibliotheken die Microsoft voor het implementeren van SCIM services zou dat verzoek vertalen in een aanroep van de methode Create van de provider van de service.  De methode Create heeft deze handtekening: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create(
      Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
      string correlationIdentifier);

In het geval van een aanvraag voor het inrichten van een gebruiker, is de waarde van het argument bron een exemplaar van de Microsoft.SystemForCrossDomainIdentityManagement. Core2EnterpriseUser klasse is gedefinieerd in de bibliotheek Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  Als de aanvraag voor het inrichten van de gebruiker is voltooid en vervolgens wordt verwacht dat de uitvoering van de methode retourneert een instantie van de de Microsoft.SystemForCrossDomainIdentityManagement. Core2EnterpriseUser klasse met de waarde van de eigenschap id is ingesteld op de unieke id van de gebruiker zojuist ingericht.  

**3:**  Als u wilt bijwerken een gebruiker bekend in een identiteit winkel fronted door een SCIM, Azure Active Directory gaat door door te vragen de huidige status van die gebruiker aan de service met een verzoek zoals dit: 

    GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...

In een service die is gebouwd met behulp van de Common Language Infrastructure bibliotheken die door Microsoft wordt geleverd voor de uitvoering van services voor SCIM, worden de aanvraag vertaald in een aanroep van de methode van het ophalen van de provider van de service.  Hier wordt de handtekening van de methode ophalen: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource and 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
    // are defined in Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> 
       Retrieve(
         Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
           parameters, 
           string correlationIdentifier);
    
    public interface 
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters:   
        IRetrievalParameters
        {
          Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
            ResourceIdentifier 
              { get; }
    }
    public interface Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier
    {
        string Identifier 
          { get; set; }
        string Microsoft.SystemForCrossDomainIdentityManagement.SchemaIdentifier 
          { get; set; }
    }

In het voorgaande voorbeeld met een verzoek voor het ophalen van de huidige status van een gebruiker is de waarden van de eigenschappen van het object dat is opgegeven als de waarde van het argument parameters als volgt: 

* ID: "54D382A4-2050-4C03-94D1-E769F1D15682"
* SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

**4:**  Als een kenmerk van de verwijzing moet worden bijgewerkt, wordt Azure Active Directory wordt de service om te bepalen of de huidige waarde van het kenmerk verwijzing in de winkel van de identiteit op de al fronted door de service query komt overeen met de waarde van dat kenmerk in Azure Active Directory.  Gebruikers is het kenmerk alleen de huidige waarde die wordt gezocht op deze manier het kenmerk van de manager.  Hier volgt een voorbeeld van een aanvraag om te bepalen of het kenmerk van de manager van een bepaald object in een bepaalde waarde heeft: 

    GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq 2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
    Authorization: Bearer ...

De waarde van de queryparameter kenmerken-id, betekent dat als een gebruikersobject bestaat die voldoet aan de opgegeven als de waarde van de parameter filter query-expressie, wordt de service om te reageren met een resource urn: ietf:params:scim:schemas:core:2.0:User of urn: ietf:params:scim:schemas:extension:enterprise:2.0:User, met inbegrip van alleen de waarde van het kenmerk van de resource-id wordt verwacht.  Natuurlijk, de waarde van het kenmerk id voor de aanvrager bekend is, wordt opgenomen in de waarde van de queryparameter filter; het doel van vragen voor het is daadwerkelijk een minimale vertegenwoordiging van een bron aanvragen die voldoen aan de filterexpressie als indicatie van het al dan niet een dergelijk object bestaat.   

Als de service is gemaakt met behulp van de Common Language Infrastructure-bibliotheken die Microsoft voor het implementeren van SCIM-services, wordt de aanvraag worden omgezet in een aanroep van de methode Query van de provider van de service.  De waarde van de eigenschappen van het object dat is opgegeven als de waarde van het argument parameters zijn als volgt: 

* parameters. AlternateFilters.Count: 2
* parameters. AlternateFilters.ElementAt(x). AttributePath: "id"
* parameters. AlternateFilters.ElementAt(x). Vergelijkingsoperator: ComparisonOperator.Equals
* parameters. AlternateFilter.ElementAt(x). ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"
* parameters. AlternateFilters.ElementAt(y). AttributePath: "manager"
* parameters. AlternateFilters.ElementAt(y). Vergelijkingsoperator: ComparisonOperator.Equals
* parameters. AlternateFilter.ElementAt(y). ComparisonValue: "2819c223-7f76-453a-919d-413861904646"
* parameters. RequestedAttributePaths.ElementAt(0): "id"
* parameters. SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

Hier de waarde van de index x is 0 en de waarde van de index j 1, is of de waarde van x 1 is en de waarde van y is 0, afhankelijk van de volgorde van de expressies van de queryparameter filter.   

**5:**  Hier volgt een voorbeeld van een aanvraag van Azure Active Directory een SCIM-service voor het bijwerken van een gebruiker: 

    PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas": 
      [
        "urn:ietf:params:scim:api:messages:2.0:PatchOp"],
      "Operations":
      [
        {
          "op":"Add",
          "path":"manager",
          "value":
            [
              {
                "$ref":"http://.../scim/Users/2819c223-7f76-453a-919d-413861904646",
                "value":"2819c223-7f76-453a-919d-413861904646"}]}]}

De bibliotheken van Microsoft Common Language Interface voor de uitvoering van services voor SCIM zou het verzoek vertalen in een aanroep van de methode Update van de provider van de service.  Hier wordt de handtekening van deze methode: 

    // System.Threading.Tasks.Tasks and 
    // System.Collections.Generic.IReadOnlyCollection<T>
    // are defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IPatch, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationName, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IPath and 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationValue 
    // are all defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 

    System.Threading.Tasks.Task Update(
      Microsoft.SystemForCrossDomainIdentityManagement.IPatch patch, 
      string correlationIdentifier);

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IPatch
    {
    Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase 
      PatchRequest 
        { get; set; }
    Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
      ResourceIdentifier 
        { get; set; }        
    }

    public class PatchRequest2: 
      Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase
    {
    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation> 
        Operations
        { get;}

    public void AddOperation(
      Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation operation);
    }

    public class PatchOperation
    {
    public Microsoft.SystemForCrossDomainIdentityManagement.OperationName 
      Name
      { get; set; }
    
    public Microsoft.SystemForCrossDomainIdentityManagement.IPath 
      Path
      { get; set; }

    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.OperationValue> Value
      { get; }

    public void AddValue(
      Microsoft.SystemForCrossDomainIdentityManagement.OperationValue value);
    }

    public enum OperationName
    {
      Add,
      Remove,
      Replace
    }

    public interface IPath
    {
      string AttributePath { get; }
      System.Collections.Generic.IReadOnlyCollection<IFilter> SubAttributes { get; }
      Microsoft.SystemForCrossDomainIdentityManagement.IPath ValuePath { get; }
    }

    public class OperationValue
    {
      public string Reference
      { get; set; }
      
      public string Value
      { get; set; }
    }



In het voorgaande voorbeeld van een aanvraag voor het bijwerken van een gebruiker wordt het object dat is opgegeven als de waarde van het argument patch waarden van deze eigenschappen hebben: 

* ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"
* (PatchRequest als PatchRequest2). Operations.Count: 1
* (PatchRequest als PatchRequest2). Operations.ElementAt(0). OperationName: OperationName.Add
* (PatchRequest als PatchRequest2). Operations.ElementAt(0). Path.AttributePath: "manager"
* (PatchRequest als PatchRequest2). Operations.ElementAt(0). Value.Count: 1
* (PatchRequest als PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Referentie: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646
* (PatchRequest als PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Waarde: 2819c223-7f76-453a-919d-413861904646

**6:**  Om een gebruiker uit een identiteit winkel door een service SCIM fronted opgeheven bepaling stuurt Azure Active Directory een verzoek zoals dit: 

    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    
Als de service is gemaakt met behulp van de Common Language Infrastructure-bibliotheken die Microsoft voor het implementeren van SCIM-services, wordt de aanvraag worden omgezet in een aanroep van de methode Delete van de provider van de service.   Deze methode heeft deze handtekening: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
 
Het object dat is opgegeven als de waarde van het argument resourceIdentifier wordt in het voorgaande voorbeeld van een aanvraag voor het inrichten van een gebruiker uit de waarden van deze eigenschappen hebben: 

* ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

##<a name="group-provisioning-and-de-provisioning"></a>Groep ingericht en verwijderen van gegevens

In de afbeelding hieronder ziet u de berichten dat Azure Active Directory wordt verzonden naar een SCIM-service voor het beheren van de levenscyclus van een groep in een andere identiteit opgeslagen.  De berichten verschillen van de berichten die betrekking hebben op gebruikers op drie manieren: 

* Het schema van een resource voor de groep wordt aangeduid als http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  
* Aanvragen voor het ophalen van groepen wordt bepaald dat de leden-kenmerk is van een bron die wordt verstrekt in antwoord op de aanvraag moeten worden uitgesloten.  
* Aanvragen om te bepalen of een kenmerk van de verwijzing naar een bepaalde waarde worden aanvragen over het kenmerk leden.  

![][5]
*Afbeelding: Groep ingericht en reeks verwijderen van gegevens*

##<a name="related-articles"></a>Verwante artikelen

- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Automatiseren van de gebruiker wordt ingericht/Deprovisioning voor SaaS-Apps](active-directory-saas-app-provisioning.md)
- [Kenmerktoewijzingen voor gebruikersaanvragen aanpassen](active-directory-saas-customizing-attribute-mappings.md)
- [Het schrijven van expressies voor het toewijzen van kenmerken](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Filters voor gebruikersaanvragen scoping](active-directory-saas-scoping-filters.md)
- [Account meldingen inrichten](active-directory-saas-account-provisioning-notifications.md)
- [Lijst met zelfstudies over het integreren van SaaS-toepassingen](active-directory-saas-tutorial-list.md)


    
<!--Image references-->
[1]: ./media/active-directory-scim-provisioning/scim-figure-1.PNG
[2]: ./media/active-directory-scim-provisioning/scim-figure-2.PNG
[3]: ./media/active-directory-scim-provisioning/scim-figure-3.PNG
[4]: ./media/active-directory-scim-provisioning/scim-figure-4.PNG
[5]: ./media/active-directory-scim-provisioning/scim-figure-5.PNG
