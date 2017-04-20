<properties 
    pageTitle="De portal van de gebruiker implementeren voor de Azure meerledige verificatieserver"
    description="Dit is de pagina Azure meerledige verificatie die wordt beschreven hoe u aan de slag met Azure MVR gesloten en de portal van de gebruiker."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="kgremban"/>

# <a name="deploying-the-user-portal-for-the-azure-multi-factor-authentication-server"></a>De portal van de gebruiker implementeren voor de Azure meerledige verificatieserver

De User Portal kan de beheerder installeert en configureert de Azure meerledige verificatie User Portal. De User Portal is een IIS-website waarmee gebruikers kunnen inschrijven in Azure meerledige verificatie en behouden hun accounts. Een gebruiker kan het telefoonnummer wijzigen, hun PINCODE wijzigen of Azure meerledige verificatie overslaan tijdens hun volgende teken op.

Gebruikers wordt naar de User Portal met hun normale gebruikersnaam en wachtwoord aanmelden en zal een oproep Azure meerledige verificatie of beveiliging vragen om de verificatie te voltooien. Als de inschrijving van de gebruiker is toegestaan, configureert een gebruiker hun telefoonnummer en PINCODE de eerste keer dat ze zich op de Portal van de gebruiker aanmelden.

Portal-beheerders kunnen instellen en de machtiging om nieuwe gebruikers toevoegen en bijwerken van bestaande gebruikers.

<center>![Setup](./media/multi-factor-authentication-get-started-portal/install.png)</center>

## <a name="deploying-the-user-portal-on-the-same-server-as-the-azure-multi-factor-authentication-server"></a>De portal gebruiker op dezelfde server als de Server Azure meerledige verificatie implementeren

De volgende minimumvereisten zijn vereist voor het installeren van de Portal voor gebruikers op dezelfde server als de Server Azure meerledige verificatie:

- IIS moet worden geïnstalleerd, met inbegrip van asp.net en IIS 6 meta basis compatibiliteit (voor IIS 7 of hoger)
- Aangemelde gebruiker moet beschikken over Administrator-rechten voor de computer en het domein indien van toepassing.  Dit komt omdat de account moet machtigingen voor het maken van Active Directory-beveiligingsgroepen.

### <a name="to-deploy-the-user-portal-for-the-azure-multi-factor-authentication-server"></a>De portal van de gebruiker voor de Azure meerledige verificatieserver implementeren

1. Binnen de Azure meerledige verificatieserver: klik op het pictogram User Portal in het linkermenu, klikt u op de knop User Portal installeren.
1. Klik op volgende.
1. Klik op volgende.
1. Als de computer deel van een domein uitmaakt en de configuratie van Active Directory voor het beveiligen van communicatie tussen de User Portal en de Azure meerledige verificatie-service niet compleet is, wordt de Active Directory-stap weergegeven. Klik op de knop Volgende om deze configuratie automatisch worden uitgevoerd.
1. Klik op volgende.
1. Klik op volgende.
1. Klik op sluiten.
1. Open een webbrowser vanaf elke computer en Ga naar de URL waar de User Portal is geïnstalleerd (bijvoorbeeld https://www.publicwebsite.com/MultiFactorAuth). Zorg ervoor dat er geen certificaat waarschuwingen of fouten worden weergegeven.

<center>![Setup](./media/multi-factor-authentication-get-started-portal/portal.png)</center>

## <a name="deploying-the-azure-multi-factor-authentication-server-user-portal-on-a-separate-server"></a>De Azure meerledige verificatie Server User Portal op een afzonderlijke Server implementeren

Om de App Azure meerledige verificatie gebruikt, zijn vereist zodat de toepassing met User Portal communiceren kan:

Zie Hardware- en softwarevereisten voor hardware- en softwarevereisten:

- Moet u v6.0 of hoger van de Server Azure meerledige verificatie.
- User Portal moet zijn geïnstalleerd op de webserver van een internetverbinding met Microsoft® Internet Information Services (IIS) IIS 6.x 7.x of hoger.
- Wanneer u IIS 6.x ASP.NET v2.0.50727 is geïnstalleerd, geregistreerd en ingesteld op toegestaan.
- Vereiste functieservices bij gebruik van IIS 7.x of hoger zijn ASP.NET en compatibiliteit met IIS 6-Metabase.
- User Portal moet worden beveiligd met een SSL-certificaat.
- De SDK Azure meerledige verificatie Web-Service in IIS moet zijn geïnstalleerd 6.x IIS 7.x of hoger op de server die op de Azure meerledige verificatieserver is geïnstalleerd.
- Azure meerledige verificatie Web Service SDK moet worden beveiligd met een SSL-certificaat.
- User Portal moet verbinding kunnen maken met de SDK Azure meerledige verificatie Web Service via SSL.
- User Portal moet toegang hebben tot de Azure meerledige verificatie Web Service SDK met de referenties van een service-account die lid is van de groep 'PhoneFactor Administrators' genoemd. Deze service-account en de groep bestaan in Active Directory als de Server Azure meerledige verificatie wordt uitgevoerd op een server deel uitmaakt van een domein. Deze service-account en de groep bestaat lokaal op de Server Azure meerledige verificatie als deze geen deel van een domein uitmaakt.

De portal gebruiker installeren op een andere server dan de Server Azure meerledige verificatie vereist de volgende drie stappen:

1. De webservice SDK installeren
2. De portal gebruiker installeren
3. De Portal-instellingen configureren in de Azure meerledige verificatieserver


### <a name="install-the-web-service-sdk"></a>De webservice SDK installeren

Azure meerledige verificatie Web Service SDK niet al is geïnstalleerd op de Server Azure meerledige verificatie, gaat u naar die server als de Server Azure meerledige verificatie te openen. Klik op het pictogram Web Service SDK, klikt u op het Web Service SDK installeren... knop en volg de aanwijzingen. De SDK Web Service moet worden beveiligd met een SSL-certificaat. Een zelfondertekend certificaat voor dit doel is geen probleem, maar heeft te importeren in het archief 'Vertrouwde basiscertificeringsinstanties' van de lokale account op de webserver User Portal zodat het certificaat vertrouwen wanneer de SSL-verbinding tot stand brengen.

<center>![Setup](./media/multi-factor-authentication-get-started-portal/sdk.png)</center>

### <a name="install-the-user-portal"></a>De portal gebruiker installeren

Voordat u de portal gebruiker installeert op een afzonderlijke server, worden op de hoogte van de volgende opties:

- Is het handig om open een webbrowser op de webserver verbonden met Internet en Ga naar de URL van de Web Service SDK die is ingevoerd in het bestand web.config. Als u de browser kunt u met de webservice is, moet het om referenties gevraagd. Voer de gebruikersnaam en het wachtwoord die zijn ingevoerd in het bestand web.config, precies zoals deze wordt weergegeven in het bestand. Zorg ervoor dat er geen certificaat waarschuwingen of fouten worden weergegeven.
- Als een reverse-proxy of firewall is zit aan de User Portal server en uitvoeren van SSL-offloading, kunt u de User Portal web.config-bestand bewerken en toevoegen van de volgende sleutel aan het <appSettings> sectie zodat de User Portal http in plaats van https gebruiken kunt. <add key="SSL_REQUIRED" value="false"/>

#### <a name="to-install-the-user-portal"></a>De portal gebruiker installeren

1. Open Windows Verkenner op de server Azure meerledige verificatie en navigeer naar de map waarin de Azure meerledige verificatieserver is geïnstalleerd (bijvoorbeeld C:\Program Files\Multi-Factor Authentication Server). Kies de 32-bits of 64-bits versie van het MultiFactorAuthenticationUserPortalSetup installatiebestand voor de server waarop User Portal zijn geïnstalleerd. Kopieer het bestand voor installatie op de server verbonden met Internet.
2. Op de webserver verbonden met Internet, moet het setup-bestand worden uitgevoerd met administrator-rechten. De eenvoudigste manier om dit te doen is open een opdrachtprompt als administrator en Ga naar de locatie waar het installatiebestand is gekopieerd.
3. Voer het installatiebestand van MultiFactorAuthenticationUserPortalSetup64, desgewenst de naam van de Site en virtuele map wijzigen.
4. Nadat de installatie van de User Portal is voltooid, Ga naar C:\inetpub\wwwroot\MultiFactorAuth (of de juiste map op basis van de naam van de virtuele map) en bewerk het bestand web.config.
5. Ga naar de sleutel USE_WEB_SERVICE_SDK en wijzig de waarde van false op true. Zoek de sleutels WEB_SERVICE_SDK_AUTHENTICATION_USERNAME en WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD en stelt u de waarden op de gebruikersnaam en het wachtwoord van de serviceaccount die lid is van de PhoneFactor Admins beveiliging (Zie de bovenstaande sectie). Geef de gebruikersnaam en het wachtwoord tussen de aanhalingstekens aan het einde van de regel (waarde = "" / >). Het wordt aanbevolen het gebruik van een gekwalificeerde gebruikersnaam (bijvoorbeeld domein\gebruikersnaam of machine\username)
6. Zoek de instelling pfup_pfwssdk_PfWsSdk en wijzig de waarde van "http://localhost:4898/PfWsSdk.asmx" aan de URL van de Web Service SDK die wordt uitgevoerd op de Server in Azure meerledige verificatie (bv. https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx). Omdat SSL wordt gebruikt voor deze verbinding, als u verwijst naar de SDK Web Service door de servernaam geen IP-adres omdat het SSL-certificaat zal worden uitgegeven voor de servernaam en de URL die wordt gebruikt moet overeenkomen met de naam van het certificaat. Als de naam van de server wordt niet omgezet in een IP-adres van de server verbonden met Internet, kunt u een vermelding toevoegen aan het hosts-bestand op de server de naam van de Azure meerledige verificatie-Server het IP-adres toewijzen. Sla het bestand web.config nadat wijzigingen zijn aangebracht.
7. Als voor de website dat User Portal onder (bijvoorbeeld standaardwebsite) is geïnstalleerd niet al is binded met een openbaar ondertekend certificaat, het certificaat installeren op de server al is geïnstalleerd, opent u IIS-beheer en het certificaat koppelt aan de website.
8. Open een webbrowser vanaf elke computer en Ga naar de URL waar de User Portal is geïnstalleerd (bijvoorbeeld https://www.publicwebsite.com/MultiFactorAuth). Zorg ervoor dat er geen certificaat waarschuwingen of fouten worden weergegeven.



## <a name="configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>De portal-instellingen configureren in de Azure meerledige verificatieserver
Nu de portal is geïnstalleerd, moet u de Server Azure meerledige verificatie om te werken met de portal te configureren.

Azure meerledige verificatie server biedt verschillende opties voor de portal van de gebruiker.  De volgende tabel bevat een lijst met deze opties en een verklaring van de waarvoor ze worden gebruikt.

Portal-instellingen voor gebruikers|Beschrijving|
:------------- | :------------- |
Gebruiker Portal-URL| Hiermee kunt u het invoeren van de URL van de plaats waar de portal wordt gehost.
Primaire authentication| U kunt opgeven welk type verificatie u wilt gebruiken bij het aanmelden bij de portal.  Windows, RADIUS- of LDAP-verificatie.
Toestaan dat gebruikers zich aanmelden|Hiermee kunnen gebruikers een gebruikersnaam en wachtwoord op de aanmeldingspagina invoert voor de User portal.  Als deze optie niet is geselecteerd, worden de vakken grijs.
Gebruikers inschrijven|Hiermee kan de gebruiker zich inschrijven voor meerledige verificatie door ze naar een setup-scherm dat gevraagd aanvullende informatie zoals een telefoonnummer.  Vragen voor het reservetelefoonnummer kan gebruikers een tweede telefoonnummer opgeven.  Waarschuwen voor derden EDE token kan gebruikers een 3e partij EDE token opgeven.
Gebruikers toestaan te initiëren One-Time overslaan| Hierdoor kunnen gebruikers een eenmalige overslaan te starten.  Als een gebruiker wordt dit gaat invloed hebben op de volgende keer dat de gebruiker zich aanmeldt.  Vragen om bypass seconden biedt de gebruiker een vak dat zij de standaardwaarde van 300 seconden kunnen wijzigen.  Anders wordt is de eenmalige overslaan alleen goed voor 300 seconden.
Gebruikers met de methode select| Hiermee kunnen gebruikers op te geven hun primaire contactpersoon.  Dit kan telefoongesprek, tekstbericht, mobiele app of EED token zijn.
Toestaan dat gebruikers de taal selecteren|  Hiermee kan de gebruiker de taal wijzigen die wordt gebruikt voor het telefoongesprek, tekstbericht, mobiele app of EED token.
Gebruikers met mobiele app activeren| Kunnen de gebruikers voor het genereren van een activeringscode om het activeringsproces mobiele app die met de server wordt gebruikt.  U kunt ook het aantal apparaten dat ze dit kunnen activeren op instellen.  Tussen 1 en 10.
Vragen over beveiliging voor terugval gebruiken|Hiermee kunt u gebruiken als meerledige verificatie mislukt.  U kunt het aantal beveiliging vragen die beantwoord moeten worden voltooid.
Gebruikers met derden EDE token koppelen| Hiermee kunnen gebruikers een EED derde token opgeven.
EDE-token voor terugval gebruiken|Kan voor het gebruik van een EED token in het geval dat meerledige verificatie niet gelukt is.  Ook kunt u de time-out voor sessies opgeven in minuten.
Logboekregistratie inschakelen|Hiermee kunt u zich aanmelden op de portal van de gebruiker.  De logboekbestanden bevinden zich op: C:\Program Files\Multi-Factor verificatie Server\Logs.

De meeste van deze instellingen zijn zichtbaar voor de gebruiker zodra ze zijn ingeschakeld en de gebruiker tekenen in de portal van de gebruiker.

![Portal-instellingen voor gebruikers](./media/multi-factor-authentication-get-started-portal/portalsettings.png)



### <a name="to-configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>De portal-instellingen configureren in de Azure meerledige verificatieserver




1. Klik op het pictogram User Portal in Azure meerledige verificatieserver. Voer de URL naar de User Portal in het tekstvak URL van de Portal gebruiker op het tabblad instellingen. Deze URL wordt ingevoegd in e-mailberichten die worden verzonden naar gebruikers wanneer ze worden geïmporteerd in de Azure meerledige verificatieserver als de e-mailfunctionaliteit is ingeschakeld.
2. Kies de instellingen die u wilt gebruiken in de User Portal. Bijvoorbeeld, als de gebruikers kunnen hun verificatiemethoden bepalen, zorgen ervoor dat gebruikers toestaan om methode te selecteren is ingeschakeld en de methoden waaruit zij kunnen kiezen.
3. Klik op de koppeling Help in de rechterbovenhoek voor help-informatie over de instellingen weergegeven.

<center>![Setup](./media/multi-factor-authentication-get-started-portal/config.png)</center>


## <a name="administrators-tab"></a>Tabblad voor beheerders
Op dit tabblad alleen kunt u gebruikers met beheerdersmachtigingen toevoegen.  Bij het toevoegen van een beheerder, kunt u de machtigingen die ze ontvangen goed afstemmen.  Op deze manier kunt u zeker zijn alleen de benodigde machtigingen toekennen aan de beheerder.  Klik op de knop toevoegen en selecteer vervolgens en gebruikers en hun machtigingen en klik vervolgens op toevoegen.

![Portal-beheerders](./media/multi-factor-authentication-get-started-portal/admin.png)


## <a name="security-questions"></a>Vragen over beveiliging
Op dit tabblad kunt u de beveiliging vragen die gebruikers antwoorden te bieden willen als de vragen gebruiken beveiliging voor mogelijke optie is geselecteerd.  Azure meerledige Authenticaton Server wordt geleverd met standaard vragen die u kunt gebruiken.  U kunt ook de volgorde wijzigen of uw eigen vragen toevoegen.  Bij het toevoegen van uw eigen vragen, kunt u de taal die u deze vraag dat wilt verschijnt ook.

![Gebruiker security portal vragen](./media/multi-factor-authentication-get-started-portal/secquestion.png)


## <a name="passed-sessions"></a>Doorgegeven sessies

## <a name="saml"></a>SAML
Hiermee kunt u de portal gebruiker voor het accepteren van claims van een identiteitsprovider SAML met setup.  U kunt de time-out voor sessie opgeven, het verificatiecertificaat en het logboek van de omleidings-URL opgeven.

![SAML](./media/multi-factor-authentication-get-started-portal/saml.png)

## <a name="trusted-ips"></a>Vertrouwde IP-adressen
Op dit tabblad kunt u één IP-adressen of IP-adresbereiken meerledige verificatie overgeslagen als een gebruiker bij een van deze IP-adressen aanmeldt zich, die kunnen worden toegevoegd.

![Gebruiker portal vertrouwde IP-adressen](./media/multi-factor-authentication-get-started-portal/trusted.png)

## <a name="self-service-user-enrollment"></a>Inschrijving van de gebruiker zelf
Als u wilt dat gebruikers zich aanmelden en inschrijven selecteert u de gebruikers toestaan om in te loggen in en gebruikersopties inschrijving toestaan. Houd er rekening mee dat de gebruikerservaring-in heeft invloed op de instellingen die u selecteert.

Bijvoorbeeld wanneer een gebruiker zich op de Portal van de gebruiker aanmeldt en klikt op de knop Log In, zijn ze wordt genomen op de installatiepagina van Azure meerledige verificatie gebruiker.  Afhankelijk van hoe u Azure meerledige verificatie hebt geconfigureerd, de gebruiker mogelijk de verificatiemethode selecteren.  

Selecteer de verificatiemethode telefoongesprek of zijn vooraf geconfigureerd met behulp van deze methode, wordt de pagina de gebruiker hun telefoonnummer en indien van toepassing-extensie in te voeren.  Ze mogen ook een alternatieve telefoonnummer invoeren.  

![Gebruiker portal vertrouwde IP-adressen](./media/multi-factor-authentication-get-started-portal/backupphone.png)

Als de gebruiker een PINCODE gebruiken moet wanneer ze worden geverifieerd, zal de pagina ook vragen om een pincode.  Na het invoeren van hun telefoonnummer (s) en de PIN-code (indien van toepassing), de gebruiker klikt op de bel Me nu op de knop verifiëren.  Azure meerledige verificatie uitvoert een telefoongesprek verificatie van het primaire telefoonnummer van de gebruiker.  De gebruiker moet het telefoongesprek te beantwoorden en hun pincode (indien van toepassing) en druk op # op verplaatsen naar de volgende stap van het inschrijvingsproces zichzelf.   

Als de gebruiker Hiermee selecteert u de verificatiemethode voor de SMS-tekstberichten of vooraf geconfigureerd is met behulp van deze methode, wordt de pagina de gebruiker voor hun gsm-nummer.  Als de gebruiker een PINCODE gebruiken moet wanneer ze worden geverifieerd, zal de pagina ook vragen om een pincode.  Na het invoeren van hun telefoonnummer en PINCODE (indien van toepassing), de gebruiker klikt op de tekst Me nu op de knop verifiëren.  Azure meerledige verificatie uitvoert een SMS verificatie van de gebruiker van de mobiele telefoon.  De gebruiker moet ontvangen van de SMS die een één-keer-code (OTP) en een antwoord op het bericht met die OTP plus hun PIN-code bevat, indien van toepassing) op naar de volgende stap van het inschrijvingsproces zichzelf.

![Gebruiker portal SMS](./media/multi-factor-authentication-get-started-portal/text.png)   

Als de gebruiker de verificatiemethode mobiele app selecteert of vooraf geconfigureerd is met behulp van deze methode, de pagina wordt de gebruiker gevraagd om de Azure meerledige verificatie-app op hun apparaat installeren en een activeringscode genereren.  Na het installeren van de app Azure meerledige verificatie van de gebruiker op de knop activeringscode genereren klikt.    

>[AZURE.NOTE]De app Azure meerledige verificatie gebruiken, moet de gebruiker push-meldingen voor hun apparaat inschakelen.

Vervolgens verschijnt de pagina een activeringscode en een URL als een afbeelding van de streepjescode.  Als de gebruiker een PINCODE gebruiken moet wanneer ze worden geverifieerd, zal de pagina ook vragen om een pincode.  De gebruiker voert de activeringscode en de URL in de app Azure meerledige verificatie of de streepjescodescanner gebruikt voor het scannen van de afbeelding van de streepjescode en op de knop activeren.    

Nadat de activering voltooid is, wordt de gebruiker de knop Me nu verifiëren.  Azure meerledige verificatie uitvoert een verificatie van de mobiele app van de gebruiker.  De gebruiker moet de pincode (indien van toepassing) en druk op de knop verifiëren in hun mobiele app op verplaatsen naar de volgende stap van het inschrijvingsproces zelf.  


Als de beheerders de Azure meerledige verificatieserver beveiliging vragen en antwoorden verzamelen hebt geconfigureerd, wordt de gebruiker vervolgens naar de pagina beveiliging vragen uitgevoerd.  De gebruiker selecteert vier beveiliging vragen en antwoorden op hun vragen geselecteerde.    

![Gebruiker security portal vragen](./media/multi-factor-authentication-get-started-portal/secq.png)  

De gebruiker zelf inschrijving is nu voltooid en de gebruiker is aangemeld bij de User Portal.  Gebruikers kunnen zich aanmelden in de portal van de gebruiker op elk gewenst moment in de toekomst te wijzigen hun telefoonnummers, pennen, verificatiemethoden en beveiliging vragen mits toegestaan door de beheerder.
