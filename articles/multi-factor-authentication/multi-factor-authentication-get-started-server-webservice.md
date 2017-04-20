<properties 
    pageTitle="De webservice MVR gesloten Server Mobile App aan de slag"
    description="De App Azure meerledige verificatie biedt een extra verificatie van out-of-band-optie.  Hiermee kunt de MVR gesloten server push-meldingen aan gebruikers."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="getting-started-the-mfa-server-mobile-app-web-service"></a>De webservice MVR gesloten Server Mobile App aan de slag

De App Azure meerledige verificatie biedt een extra verificatie van out-of-band-optie. In plaats van een geautomatiseerd telefoongesprek of een SMS naar de gebruiker tijdens het aanmelden plaatsen, duwt Azure meerledige verificatie een melding Azure meerledige verificatie App op de smartphone of Tablet PC van de gebruiker. De gebruiker kranen eenvoudigweg 'Verifiëren' (of een PINCODE invoert en kranen 'Verifiëren') in de app aan te melden.

Om de App Azure meerledige verificatie gebruikt, zijn vereist zodat de app met de webservice voor mobiele App communiceren kunt:

- Zie Hardware- en softwarevereisten voor hardware- en softwarevereisten
- Moet u v6.0 of hoger van de Azure meerledige verificatieserver
- Mobile App Web Service moet zijn geïnstalleerd op de webserver van een internetverbinding met Microsoft® Internet Information Services (IIS) IIS 7.x of hoger.  Zie [IIS.NET](http://www.iis.net/)voor meer informatie over IIS.
- Controleer v4.0.30319 ASP.NET is geïnstalleerd, geregistreerd en ingesteld op toegestaan
- Vereiste functieservices zijn ASP.NET en compatibiliteit met IIS 6-Metabase
- Mobiele App-webservice moet toegankelijk is via een openbare URL worden
- Mobile App Web Service moet worden beveiligd met een SSL-certificaat.
- De SDK Azure meerledige verificatie Web-Service in IIS moet zijn geïnstalleerd 7.x of hoger op de server die de Azure meerledige verificatieserver
- Azure meerledige verificatie Web Service SDK moet worden beveiligd met een SSL-certificaat.
- Mobile App Web Service moet verbinding kunnen maken met de SDK Azure meerledige verificatie Web Service via SSL
- Mobile App Web Service moet toegang hebben tot de Azure meerledige verificatie Web Service SDK met de referenties van een service-account die lid is van de groep 'PhoneFactor Administrators' genoemd. Deze service-account en de groep bestaan in Active Directory als de Server Azure meerledige verificatie wordt uitgevoerd op een server deel uitmaakt van een domein. Deze service-account en de groep bestaat lokaal op de Server Azure meerledige verificatie als deze geen deel van een domein uitmaakt.


De portal gebruiker installeren op een andere server dan de Server Azure meerledige verificatie vereist de volgende drie stappen:

1. De webservice SDK installeren
2. De webservice voor mobiele app installeren
3. De mobiele app-instellingen configureren in de Azure meerledige verificatieserver
4. Activeer de App Azure meerledige verificatie voor eindgebruikers

## <a name="install-the-web-service-sdk"></a>De webservice SDK installeren

Azure meerledige verificatie Web Service SDK niet al is geïnstalleerd op de Server Azure meerledige verificatie, gaat u naar die server als de Server Azure meerledige verificatie te openen. Klik op het pictogram Web Service SDK, klikt u op het Web Service SDK installeren... knop en volg de aanwijzingen. De SDK Web Service moet worden beveiligd met een SSL-certificaat. Een zelfondertekend certificaat voor dit doel is geen probleem, maar heeft te importeren in het archief 'Vertrouwde basiscertificeringsinstanties' van de lokale account op de webserver User Portal zodat het certificaat vertrouwen wanneer de SSL-verbinding tot stand brengen.

<center>![Setup](./media/multi-factor-authentication-get-started-server-webservice/sdk.png)</center>

## <a name="install-the-mobile-app-web-service"></a>De webservice voor mobiele app installeren
Voordat u de mobiele app web-service installeert, worden op de hoogte van de volgende opties:

- Als de Portal Azure meerledige verificatie gebruiker al is geïnstalleerd op de server verbonden met Internet, kunnen de gebruikersnaam, wachtwoord en URL van de Web Service SDK van de User Portal web.config-bestand worden gekopieerd.
- Is het handig om open een webbrowser op de webserver verbonden met Internet en Ga naar de URL van de Web Service SDK die is ingevoerd in het bestand web.config. Als u de browser kunt u met de webservice is, moet het om referenties gevraagd. Voer de gebruikersnaam en het wachtwoord die zijn ingevoerd in het bestand web.config, precies zoals deze wordt weergegeven in het bestand. Zorg ervoor dat er geen certificaat waarschuwingen of fouten worden weergegeven.
- Als een reverse-proxy of firewall is zit aan de webserver Mobile App webservice en het uitvoeren van SSL-offloading, kunt u de webservice voor mobiele App web.config-bestand bewerken en toevoegen de volgende sleutel aan het <appSettings> sectie zodat de webservice Mobile App http in plaats van https gebruiken kunt. SSL is echter nog steeds vereist van de mobiele App naar de reverse-firewall-proxy. <add key="SSL_REQUIRED" value="false"/>

### <a name="to-install-the-mobile-app-web-service"></a>De webservice voor mobiele app installeren

<ol>
<li>Open Windows Verkenner op de Server Azure meerledige verificatie en navigeer naar de map waarin de Azure meerledige verificatieserver is geïnstalleerd (bijvoorbeeld C:\Program Files\Azure meerledige verificatie). Kies de 32-bits of 64-bits versie van het Azure meerledige AuthenticationPhoneAppWebServiceSetup installatiebestand voor de server die mobiele App Web Service wordt geïnstalleerd op. Kopieer het bestand voor installatie op de server verbonden met Internet.</li>

<li>Op de webserver verbonden met Internet, moet het setup-bestand worden uitgevoerd met administrator-rechten. De eenvoudigste manier om dit te doen is open een opdrachtprompt als administrator en Ga naar de locatie waar het installatiebestand is gekopieerd.</li>  

<li>Uitvoeren van het installatiebestand van meerdere factoren, AuthenticationMobileAppWebServiceSetup, de Site desgewenst wijzigen en de virtuele map wijzigen in een korte naam, bijvoorbeeld "PA". Een virtuele map korte naam wordt aanbevolen, omdat gebruikers de mobiele App webservice-URL in het mobiele apparaat tijdens de activering invoeren moeten.</li>

<li>Nadat de installatie van de Azure meerledige AuthenticationMobileAppWebServiceSetup is voltooid, Ga naar C:\inetpub\wwwroot\PA (of de juiste map op basis van de naam van de virtuele map) en bewerk het bestand web.config.</li>  

<li>Zoek de sleutels WEB_SERVICE_SDK_AUTHENTICATION_USERNAME en WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD en stelt u de waarden op de gebruikersnaam en het wachtwoord van de serviceaccount die lid is van de PhoneFactor Admins beveiliging (Zie de bovenstaande sectie). Mogelijk dezelfde account wordt gebruikt als de identiteit van de Azure meerledige verificatie User Portal, als dat eerder is geïnstalleerd. Geef de gebruikersnaam en het wachtwoord tussen de aanhalingstekens aan het einde van de regel (waarde = "" / >). Het verdient aanbeveling het gebruik van een gekwalificeerde gebruikersnaam (bijvoorbeeld domein\gebruikersnaam of machine\username).</li>  

<li>Zoek de instelling pfMobile App Web Service_pfwssdk_PfWsSdk en wijzig de waarde van "http://localhost:4898/PfWsSdk.asmx" aan de URL van de Web Service SDK die wordt uitgevoerd op de Server in Azure meerledige verificatie (bv. https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx). Omdat SSL wordt gebruikt voor deze verbinding, als u verwijst naar de SDK Web Service door de servernaam geen IP-adres omdat het SSL-certificaat zal worden uitgegeven voor de servernaam en de URL die wordt gebruikt moet overeenkomen met de naam van het certificaat. Als de naam van de server wordt niet omgezet in een IP-adres van de server verbonden met Internet, kunt u een vermelding toevoegen aan het hosts-bestand op de server de naam van de Azure meerledige verificatie-Server het IP-adres toewijzen. Sla het bestand web.config nadat wijzigingen zijn aangebracht.</li>  

<li>Als voor de website of mobiele App webservice onder (bijvoorbeeld standaardwebsite) is geïnstalleerd niet al is binded met een openbaar ondertekend certificaat, het certificaat installeren op de server al is geïnstalleerd, opent u IIS-beheer en het certificaat aan de website binden.</li>  

<li>Open een webbrowser vanaf elke computer en Ga naar de URL waar de Mobile App Web Service is geïnstalleerd (bijvoorbeeld https://www.publicwebsite.com/PA). Zorg ervoor dat er geen certificaat waarschuwingen of fouten worden weergegeven.</li>

### <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>De mobiele app-instellingen configureren in de Azure meerledige verificatieserver
Nu de app mobiele web-service is geïnstalleerd, moet u de Server Azure meerledige verificatie om te werken met de portal te configureren.

#### <a name="to-configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>De mobiele app in Azure meerledige verificatieserver configureren

1. Klik op het pictogram User Portal in Azure meerledige verificatieserver. Als gebruikers besturingselement kunnen controleren hun verificatiemethoden op het tabblad instellingen onder gebruikers toestaan om te selecteren, Mobile App. Zonder deze functie is ingeschakeld, eindgebruikers moet contact opnemen met de helpdesk om activering van de mobiele App te voltooien.
2. Controleer of de gebruikers toestaan om vak Mobile App activeren.
3. Schakel het selectievakje toestaan dat gebruikers inschrijven.
4. Klik op het pictogram van de mobiele App.
5. Voer de URL die wordt gebruikt in combinatie met de virtuele map die is gemaakt tijdens de installatie van de Azure meerledige AuthenticationMobileAppWebServiceSetup. Een naam kan worden ingevoerd in de daarvoor bestemde ruimte. Deze bedrijfsnaam wordt weergegeven in de mobiele toepassing. Als leeg laat, wordt de naam van uw meerledige verificatie Provider gemaakt in Azure Management Portal worden weergegeven.



<center>![Setup](./media/multi-factor-authentication-get-started-server-webservice/mobile.png)</center>
