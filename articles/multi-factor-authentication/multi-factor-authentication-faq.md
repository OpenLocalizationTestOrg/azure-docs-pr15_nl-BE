<properties
    pageTitle="Veelgestelde vragen over Azure meerledige verificatie"
    description="Bevat een lijst met veelgestelde vragen en antwoorden die betrekking hebben op Azure meerledige verificatie. Meerledige verificatie is een methode om te controleren de identiteit van een gebruiker die meer dan een gebruikersnaam en wachtwoord vereist. Het biedt een extra beveiligingslaag gebruiker aanmelden en transacties."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="kgremban"/>

# <a name="azure-multi-factor-authentication-faq"></a>Veelgestelde vragen over Azure meerledige verificatie


Deze Veelgestelde vragen antwoorden op veelgestelde vragen over Azure meerledige verificatie en het gebruik van de service meerledige verificatie, met inbegrip van vragen over de facturering model en bruikbaarheid.

## <a name="general"></a>Algemeen

**V: hoe wordt Azure meerledige verificatieserver gebruikersgegevens verwerkt?**

Meerledige verificatie server gebruikersgegevens opgeslagen alleen op de servers op gebouwen. Geen permanente gebruikersgegevens worden opgeslagen in de cloud. Wanneer de gebruiker verificatie in twee stappen uitvoert, verzendt meerledige verificatieserver gegevens naar de Azure meerledige verificatie cloud service voor verificatie. Communicatie tussen een meerledige verificatieserver en de service van de wolk meerledige verificatie maakt gebruik van Secure Sockets Layer (SSL) of Transport Layer Security (TLS) via poort 443 uitgaande.

Wanneer verificatie-aanvragen worden verzonden naar de cloud-service, worden gegevens verzameld voor verificatie en het gebruik van rapporten. Gegevensvelden zijn opgenomen in twee stappen controle logboeken zijn als volgt:

- **Unieke ID** (op naam of op locatie meerledige verificatie Server gebruikersnaam)
- **Voornaam en achternaam** (optioneel)
- **E-mailadres** (optioneel)
- **Telefoonnummer** (als u een telefoongesprek of een SMS-verificatie)
- **Apparaat Token** (als u de mobiele app-verificatie)
- **Verificatiemodus**
- **Verificatieresultaat**
- **De naam van de Server meerledige verificatie**
- **Meerledige verificatieserver IP**
- **Client IP** (indien beschikbaar)

Optionele velden kunnen worden geconfigureerd in een meerledige verificatieserver.

Het resultaat van de controle (geslaagd of weigering) en de reden als geweigerd, worden opgeslagen met de verificatiegegevens en is beschikbaar in de verificatie-en gebruiksrapporten.


## <a name="billing"></a>Facturering

De meeste vragen over facturen kunnen worden beantwoord door te verwijzen naar de [pagina met prijzen van meerledige verificatie](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

**V: Mijn organisatie belast met telefoongesprekken of SMS-berichten gebruikt om de gebruikers te verifiëren?**

Organisaties worden niet berekend voor de afzonderlijke telefoongesprekken geplaatst of tekst worden verzonden naar gebruikers via Azure meerledige verificatie. Telefoon eigenaren mogelijk in rekening gebracht voor de telefoongesprekken of tekstberichten, afhankelijk van hun persoonlijke telefoonservice ontvangen.

**V: wordt de toeslag van per gebruiker facturatie model op basis van het aantal gebruikers die zijn geconfigureerd voor het gebruik van meerledige verificatie of het aantal gebruikers van die keuringen uitvoeren?**

Facturering is gebaseerd op het aantal gebruikers dat is geconfigureerd voor het gebruik van meerledige verificatie.

**V: hoe werkt facturering meerledige verificatie?**

Als u werkt met de 'per gebruiker' of ' verificatie' model, Azure MVR gesloten is een bron op basis van verbruik. Eventuele kosten worden verrekend met Azure abonnement net als virtuele machines, websites, enzovoort van de organisatie.

Wanneer u de licentiemodel, worden Azure meerledige verificatie licenties gekocht en vervolgens toegewezen aan gebruikers, net als voor Office 365 en andere producten abonnement.

**V: is er een gratis versie van Azure meerledige verificatie voor beheerders?**

In sommige gevallen, Ja. Meerledige verificatie voor beheerders Azure biedt een subset van de MVR gesloten Azure functies zonder kosten. Deze aanbieding geldt voor leden van de beheerdersgroep van Azure globale in Azure Active Directory-exemplaren die niet zijn gekoppeld aan een provider Azure meerledige verificatie op basis van verbruik. Alle beheerders en gebruikers in de directory die zijn geconfigureerd met de volledige versie van Azure meerledige verificatie meerledige verificatie met behulp van een meerledige verificatie worden bijgewerkt.

**V: is er een gratis versie van Azure meerledige verificatie voor gebruikers van Office 365?**

In sommige gevallen, Ja. Meerledige verificatie voor Office 365 biedt een subset van de MVR gesloten Azure functies zonder kosten. Deze aanbieding geldt voor gebruikers met een Office 365 licentie toegewezen wanneer een provider Azure meerledige verificatie op basis van verbruik is niet gekoppeld aan het bijbehorende exemplaar van Azure Active Directory. De provider meerledige verificatie upgrades alle beheerders en gebruikers in de directory die zijn geconfigureerd met de volledige versie van Azure meerledige verificatie meerledige verificatie.

**V: kan mijn organisatie tussen per gebruiker en per verificatie verbruik facturering modellen op elk gewenst moment schakelen?**

De organisatie kiest een factuuradres model wanneer een resource wordt gemaakt. U kunt een factuuradres model niet wijzigen nadat de bron is ingericht. U kunt echter een ander meerledige verificatie middel ter vervanging van de oorspronkelijke maken. Instellingen en configuratie-opties kunnen niet worden overgedragen aan de nieuwe resource.

**V: kan mijn organisatie schakelen tussen het verbruik facturering en licentiemodel op elk gewenst moment?**

Wanneer licenties worden toegevoegd aan een map die al een Azure meerledige verificatie per gebruiker provider, is facturering op basis van het verbruik verlaagd door het aantal licenties in bezit. Als alle gebruikers die zijn geconfigureerd voor het gebruik van meerledige verificatie licenties die zijn toegewezen, kan de beheerder de Azure meerledige verificatie-provider te verwijderen.

U kunt verbruik-facturering per verificatie met een licentiemodel niet samen. Wanneer een per-meerledige verificatie verificatieprovider is gekoppeld aan een map, worden de organisatie wordt gefactureerd voor alle meerledige verificatie verificatieaanvragen, ongeacht de licenties in bezit.

**V: Mijn organisatie hoeft te gebruiken en identiteiten voor Azure meerledige verificatie synchroniseren?**

Wanneer een organisatie een factuuradres model op basis van verbruik, is Azure Active Directory niet vereist. De aanbieder van een meerledige verificatie koppelen aan een map is optioneel. Als uw organisatie is niet gekoppeld aan een map, kan deze implementeren Azure meerledige verificatieserver of de Azure meerledige verificatie SDK op gebouwen.

Azure Active Directory is vereist voor het licentiemodel omdat licenties worden toegevoegd aan de map wanneer u aanschaft en deze aan gebruikers in de directory toewijzen.


## <a name="usability"></a>Bruikbaarheid

**V: wat doet een gebruiker als ze een antwoord op hun telefoon niet ontvangen of als de telefoon is niet beschikbaar voor de gebruiker?**

Als de gebruiker heeft een back-up telefoon geconfigureerd, moeten ze opnieuw en selecteer die telefoon wanneer u wordt gevraagd op de aanmeldingspagina. Als de gebruiker niet beschikt over een andere methode is geconfigureerd, kan de beheerder van de organisatie het nummer toegewezen aan het primaire telefoonnummer van de gebruiker bijwerken.


**V: wat doet de beheerder als de gebruiker contact op met de beheerder van een account die de gebruiker geen toegang meer?**

De beheerder kunt herstellen account van de gebruiker om de procedure opnieuw doorlopen. Meer informatie over het [beheren van gebruikers- en apparaatinstellingen met Azure meerledige verificatie in de cloud](multi-factor-authentication-manage-users-and-devices.md).

**V: wat doet een beheerder als telefoon van de gebruiker die met behulp van wachtwoorden app verloren of gestolen is?**

De beheerder kan verwijderen app-wachtwoord van de gebruiker om onbevoegde toegang te voorkomen. Nadat de gebruiker een vervangend apparaat heeft, kan de gebruiker de wachtwoorden opnieuw. Meer informatie over het [beheren van gebruikers- en apparaatinstellingen met Azure meerledige verificatie in de cloud](multi-factor-authentication-manage-users-and-devices.md).

**V: Wat gebeurt er als de gebruiker zich niet aanmelden voor browser-apps?**

Een gebruiker die is geconfigureerd voor gebruik van meerledige verificatie vereist een app wachtwoord aanmelden bij een bepaalde browser-toepassingen. Een gebruiker moet verwijderen (delete)-in informatie, start de app en aanmelden met hun naam en app gebruikerswachtwoord.

Meer informatie over het maken van app wachtwoorden en andere [helpen met een app wachtwoord](multi-factor-authentication-end-user-app-passwords.md)opvragen.


>[AZURE.NOTE] Moderne-verificatie voor clients Office 2013
>
> Office 2013-clients (inclusief Outlook) ondersteunen nieuwe verificatieprotocollen. U kunt Office 2013 meerledige verificatie ondersteunt. Na het configureren van Office 2013, zijn app wachtwoorden niet vereist voor clients met Office 2013. Voor meer informatie, Zie de [aankondiging van Office 2013 moderne verificatie openbare preview](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

**V: wat doet een gebruiker als de gebruiker een SMS-bericht niet ontvangt, of als de gebruiker een bericht tekstberichten beantwoordt maar time-out voor de keuring?**

Afleveren van berichten met tekst en de ontvangst van de antwoorden in twee richtingen SMS is niet gegarandeerd omdat er hebt factoren die van invloed kunnen zijn op de betrouwbaarheid van de service. Deze factoren zijn het land van bestemming, de mobiele telefoon vervoerder en de signaalsterkte.

Gebruikers die problemen bij het ontvangen van tekstberichten op betrouwbare wijze moeten in plaats daarvan de mobiele app of telefoongesprek methode selecteren. De mobiele app kunt berichten zowel op cellulair en Wi-Fi-verbindingen worden ontvangen. Bovendien kan de mobiele app controle codes genereren, zelfs wanneer het apparaat geen signaal op alle heeft. De Microsoft Authenticator app is beschikbaar voor [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)en [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

Als u tekstberichten wilt gebruiken, kunt u het beste met enkelvoudige SMS in plaats van twee richtingen SMS indien mogelijk. Enkelvoudige SMS is betrouwbaarder en het voorkomt dat gebruikers telefoontikken globale SMS van het beantwoorden van een bericht op dat in een ander land is verzonden.


**V: kan ik met Azure meerledige verificatieserver hardware-tokens gebruiken?**

Als u Azure meerledige verificatieserver gebruikt, kunt u derden Open verificatie (EED) op basis van tijd, eenmalige wachtwoord (TOTP) tokens importeren en deze vervolgens gebruiken voor verificatie in twee stappen.

U kunt ActiveIdentity-tokens die EED TOTP-tokens zijn als u het bestand met de geheime sleutel in een CSV-bestand plaatst en in Azure meerledige verificatieserver importeren. U kunt EDE tokens gebruiken met Active Directory Federation Services (ADFS), RADIUS Remote Authentication Dial-In gebruiker Service () wanneer het clientsysteem kan access-challenge-antwoorden worden verwerkt en op formulieren gebaseerde verificatie voor Internet Information Server (IIS).

U kunt EDE TOTP-tokens derde deel in de volgende indelingen importeren:  
- Draagbare symmetrische sleutelcontainer (PSKC)  
- CSV als het bestand een serienummer en een geheime sleutel 32 Base indeling een tijdsinterval bevat  

**V: kan ik Azure meerledige verificatieserver beveiliging van Terminal Services gebruiken?**

Ja, maar als u met behulp van Windows Server 2012 R2 of later, alleen via Extern bureaublad-Gateway (RD-Gateway).

Wijzigingen in de beveiliging in Windows Server 2012 R2 hebt gewijzigd van de manier waarop de Azure meerledige verificatieserver verbinding met het beveiligingspakket Local Security Authority (LSA) in Windows Server 2012 en eerdere versies. Voor versies van Terminal Services in Windows Server 2012 of eerder kunt u [een toepassing met Windows-verificatie wordt beveiligd](multi-factor-authentication-get-started-server-windows.md#to-secure-an-application-with-windows-authentication-use-the-following-procedure). Als u Windows Server 2012 R2 gebruikt, moet u de RD-Gateway.

**V: Waarom zou een gebruiker wordt meerledige verificatie gebeld door een anonieme beller na de beller-ID?**

Meerledige verificatie-oproepen via het openbare telefoonnetwerk worden geplaatst, worden soms verzonden in een luchtvaartmaatschappij die geen ondersteuning biedt voor beller-ID. Om deze reden is beller-ID niet gegarandeerd, zelfs als het systeem meerledige verificatie altijd verzonden.


## <a name="errors"></a>Fouten

**V: wat moeten gebruikers doen als ze een foutbericht 'verificatieaanvraag is niet voor een account geactiveerd' wordt weergegeven wanneer de mobiele app-meldingen gebruiken?**

Vertel ze Volg deze procedure om de account te verwijderen uit de mobiele app, en vervolgens opnieuw toe te voegen:

1. Ga naar het [profiel van uw Azure portal](https://account.activedirectory.windowsazure.com/profile/) en meld u aan met uw organisatie-account.
2. Schakel **verificatie voor extra beveiliging**.
4. De bestaande account verwijderen uit de mobiele app.
5. Klik op **configureren**en volg de instructies voor het configureren van de mobiele app.


**V: wat moeten gebruikers doen als ze een 0x800434D4L-foutbericht wordt weergegeven tijdens het aanmelden bij een browser-toepassing?**

Op dit moment kan een gebruiker extra beveiliging verificatie alleen gebruiken in toepassingen en services die de gebruiker toegang via een browser. Niet-browsertoepassingen (ook wel *rich client-toepassingen*genoemd) die zijn geïnstalleerd op een lokale computer, zoals Windows PowerShell, werkt niet met accounts waarvoor extra beveiliging verificatie vereist. In dit geval ziet de gebruiker de toepassing een 0x800434D4L-fout gegenereerd.

Een tijdelijke oplossing voor dit is om afzonderlijke rekeningen voor admin-gerelateerde en niet admin bewerkingen. Later kunt u postbussen tussen uw beheerdersaccount en niet admin account koppelen zodat u met Outlook aanmelden kunt met uw account zonder beheerdersrechten. Voor meer informatie over deze informatie geeft [een beheerder de mogelijkheid om te openen en de inhoud van het postvak van een gebruiker weergeven](http://help.outlook.com/141/gg709759.aspx?sl=1).

## <a name="next-steps"></a>Volgende stappen

Als uw vraag hier niet wordt beantwoord, laat u deze in de opmerkingen onderaan de pagina. Of Hier volgen enkele extra opties voor Help-informatie opvragen:


**V: hoe krijg ik hulp met Azure meerledige verificatie**

- Zoeken in de [Microsoft Knowledge Base voor ondersteuning](https://www.microsoft.com/en-us/Search/result.aspx?form=mssupport&q=phonefactor&form=mssupport) voor oplossingen voor technische problemen.

- Zoeken en bladeren, technische vragen en antwoorden uit de Gemeenschap of uw eigen vraag in de [forums Azure Active Directory](https://social.msdn.microsoft.com/Forums/azure/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

- Als u een oudere PhoneFactor-klant bent en u vragen hebt of hulp bij het opnieuw instellen van een wachtwoord, gebruik de link [opnieuw instellen van wachtwoorden](mailto:phonefactorsupport@microsoft.com) voor het openen van een aanvraag voor ondersteuning.

- Neem contact op met een medewerker tot en met [ondersteuning van Azure meerledige verificatieserver (PhoneFactor)](https://support.microsoft.com/oas/default.aspx?prid=14947). Wanneer u contact met ons opneemt, is het handig als u zo veel mogelijk informatie over uw probleem mogelijk kunt opnemen. Informatie die u kunt leveren bevat de pagina waar u de fout, de specifieke foutcode, de specifieke sessie-ID en de ID van de gebruiker die de fout werd gezien.
