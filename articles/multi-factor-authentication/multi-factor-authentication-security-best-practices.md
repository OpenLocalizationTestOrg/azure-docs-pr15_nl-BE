<properties 
    pageTitle="Security Best Practices voor het gebruik van Azure MVR gesloten"
    description="Dit document bevat de aanbevolen procedures rond het gebruik van Azure MVR gesloten met Azure accounts"
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
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="security-best-practices-for-using-azure-multi-factor-authentication-with-azure-ad-accounts"></a>Security Best Practices voor het gebruik van Azure meerledige verificatie met Azure AD accounts

Wanneer het gaat om het verificatieproces te verbeteren, is meerledige verificatie mogelijk de beste keuze voor de meeste organisaties. MVR-verificatie (Azure meerledige gesloten) kunnen bedrijven hun beveiliging en naleving voldoen en bieden een eenvoudige ervaring-in voor gebruikers. In dit artikel wordt uitgelegd hoe enkele aanbevelingen waarmee u rekening houden moet bij de planning voor de aanneming van de MVR gesloten Azure.

## <a name="best-practices-for-azure-multi-factor-authentication-in-the-cloud"></a>Aanbevolen procedures voor Azure meerledige verificatie in de cloud
Om al uw gebruikers voorzien van meerledige verificatie en voordelen van de uitgebreide functies voor Azure meerledige verificatie biedt, neemt moet u Azure meerledige verificatie inschakelen op alle gebruikers.  Dit wordt gedaan met behulp van een van de volgende opties:

- Azure AD Premium of de Enterprise Mobility Suite
- Meerledige verificatie Provider

### <a name="azure-ad-premiumenterprise-mobility-suite"></a>Azure AD Premium/Enterprise Mobility Suite

![EMS](./media/multi-factor-authentication-security-best-practices/ems.png)

De eerste aanbevolen stap voor vaststelling Azure MVR gesloten in de cloud met Azure AD Premium of de Enterprise Mobility Suite is voor het toewijzen van licenties voor uw gebruikers.  Azure meerledige verificatie maakt deel uit van deze pakketten en als zodanig uw organisatie hoeft niet iets meer uit te breiden de mogelijkheid meerledige verificatie voor alle gebruikers.

Bij het instellen van een meerledige verificatie kunt u volgende:

- U hoeft niet een meerledige verificatie-Provider maken.  Azure AD premie en de Enterprise Mobility Suite wordt geleverd met Azure meerledige verificatie.  Als u een verificatie-Provider kan u een dubbele rekening.
- Azure AD verbinden is alleen vereist als u Active Directory-omgeving op gebouwen met een directory Azure AD synchroniseert. Als u alleen een Azure AD-map die niet is gesynchroniseerd met een exemplaar van lokalen van Active Directory gebruikt, hoeft u niet verbinden met Azure AD.


### <a name="multi-factor-auth-provider"></a>Meerledige verificatie Provider

![Meerledige verificatie Provider](./media/multi-factor-authentication-security-best-practices/authprovider.png)

Als u nog geen Azure AD Premium of de Enterprise Mobility Suite wordt aanbevolen eerst voor vaststelling Azure MVR gesloten in de cloud een MVR gesloten Auth-Provider maken. Hoewel MVR gesloten standaard beschikbaar voor globale beheerders met Azure Active Directory is wanneer u MVR gesloten voor uw organisatie implementeert, moet u uitbreiden de mogelijkheid meerledige verificatie voor alle gebruikers en te doen, moet u een meerledige verificatie-Provider.
Als u de Provider-verificatie selecteert, moet u een map selecteren en houd rekening met het volgende:

- U hoeft niet een Azure AD-map voor het maken van een meerledige verificatie-Provider.
- U moet de Provider meerledige verificatie koppelen met een directory Azure AD desgewenst uitbreiden meerledige verificatie voor alle gebruikers en/of wilt uw globale beheerders kunnen profiteren van de voorzieningen voor beheerportal, aangepaste begroetingen en rapporten.
- DirSync of AAD synchroniseren zijn alleen vereist als u Active Directory-omgeving op gebouwen met een directory Azure AD synchroniseert. Als u alleen een Azure AD-map die niet is gesynchroniseerd met een exemplaar van lokalen van Active Directory gebruikt, hoeft niet DirSync of AAD synchroniseren.
- Als u Azure AD Premium of de Enterprise Mobility Suite hebt, hoeft u niet om een meerledige verificatie-provider te maken. U hoeft alleen een licentie voor een gebruiker toewijzen en vervolgens beginnen MVR gesloten inschakelen voor gebruikers.
- Zorg dat u de juiste syntaxis model voor uw bedrijf (per auth of per gebruiker ingeschakeld), zodra u het gebruiksmodel selecteert u deze niet wijzigen.

### <a name="user-account"></a>Gebruikersaccount
Wanneer de MVR gesloten inschakelen voor uw gebruikers, gebruikersaccounts op een van de drie lidstaten van de core kunnen zijn: uitgeschakeld, ingeschakeld of afgedwongen.
Gebruik de onderstaande richtlijnen om ervoor te zorgen gebruikt u de meest geschikte optie voor uw implementatie:

- Wanneer de status van de gebruiker is ingesteld op uitgeschakeld, wordt die gebruiker meerledige verificatie niet gebruiken. Dit is de standaardstatus.
- Wanneer de status van de gebruiker is ingesteld op ingeschakeld, betekent dit dat de gebruiker is ingeschakeld, maar niet de registratieprocedure heeft voltooid. Zij wordt gevraagd het proces op het volgende teken in te voeren. Deze instelling heeft geen invloed op de browser niet apps. Alle apps blijven werken totdat de registratie is voltooid.
- Wanneer de status van de gebruiker is ingesteld op afgedwongen, betekent dit dat de gebruiker kan of mag geen inschrijving hebt voltooid. Als ze het registratieproces hebt voltooid zijn zij meerledige verificatie gebruiken. Anders wordt wordt de gebruiker gevraagd om het registratieproces volledig doorlopen op de volgende aanmelden. Deze instelling heeft invloed op de browser niet apps. Deze apps zal niet werken totdat de app wachtwoorden worden gemaakt en gebruikt.
- Gebruik de melding gebruikerssjabloon in het artikel [aan de slag met Azure meerledige verificatie in de cloud](multi-factor-authentication-get-started-cloud.md) een e-mailbericht verzenden naar uw gebruikers met betrekking tot de vaststelling van de MVR gesloten.

### <a name="supportability"></a>Ondersteuning

Aangezien alleen wachtwoorden gebruikt voor het verifiëren van de overgrote meerderheid van de gebruikers gewend zijn, is het belangrijk dat uw bedrijf bewustzijn voor alle gebruikers met betrekking tot dit proces brengen. Deze kennis kan de kans verkleinen dat gebruikers uw helpdesk voor kleine problemen met betrekking tot de MVR gesloten belt.
Er zijn echter enkele scenario's MVR gesloten tijdelijk uit te schakelen wanneer nodig is. Gebruik de onderstaande richtlijnen om te begrijpen hoe de scenario's:

- Zorg ervoor dat uw technisch personeel worden opgeleid greep scenario's waarbij de mobiele app of de telefoon niet een melding of een telefoongesprek ontvangt en om deze reden dat de gebruiker zich niet aanmelden. Een eenmalige bypass optie één keer door te "negeren" meerledige verificatie verifiëren van een gebruiker kan worden ingeschakeld. De bypass is tijdelijk en verloopt na het opgegeven aantal seconden.
- Indien nodig, kunt u gebruikmaken van de functie vertrouwde IP-adressen in Azure MVR gesloten. Deze functie kan beheerders van een huurder beheerde of federatieve meerledige verificatie voor gebruikers die vanaf het lokale bedrijfsintranet aanmelden te omzeilen. De functies zijn beschikbaar voor huurders Azure AD die Azure AD Premium, Enterprise Mobility Suite of Azure meerledige verificatie-licenties hebt.


## <a name="best-practices-for-azure-multi-factor-authentication-on-premises"></a>Aanbevolen procedures voor Azure meerledige verificatie on-premises
Als uw bedrijf besloten om een eigen infrastructuur voor het inschakelen van de MVR gesloten, zal het implementeren van een Azure meerledige verificatieserver op locatie nodig zijn. De MVR gesloten serveronderdelen worden weergegeven in het onderstaande diagram:

![Meerledige verificatie Provider](./media/multi-factor-authentication-security-best-practices/server.png)
*niet standaard geïnstalleerd ** geïnstalleerd maar niet standaard is ingeschakeld


Azure meerledige verificatieserver kunt beveiligen op ruimten middelen die worden gebruikt door AD Azure accounts en cloud worden gebruikt.  Dit kan echter alleen uitgevoerd via de Federatie.  Dat wil zeggen, als u AD FS zijn en deze met uw huurder Azure AD federatieve.
Bij het instellen van een meerledige verificatieserver rekening houden met volgende:

- Als u AD Azure resources met behulp van Active Directory Federation Services beveiligen en vervolgens de 1ste factor voor de verificatie wordt uitgevoerd op-ruimten met behulp van AD FS en de 2de factor is uitgevoerd op ruimten door de claim behouden blijft.
- Het is niet een vereiste dat de Azure meerledige verificatieserver van uw AD FS-federation-server worden geïnstalleerd, maar de Adapter meerledige verificatie van AD FS moet worden geïnstalleerd op een Windows Server 2012 R2 met AD FS. U kunt de server installeren op een andere computer als deze versie ondersteund wordt en afzonderlijk de AD FS-adapter installeren op de AD FS-federation-server. Zie de procedure hieronder voor informatie over het installeren van de adapter afzonderlijk.
- De installatiewizard meerledige verificatie AD FS-Adapter maakt u een beveiligingsgroep met de naam PhoneFactor Admins in Active Directory en vervolgens de AD FS-account van de federation-service toegevoegd aan deze groep. Het wordt aanbevolen dat u op uw domeincontroller controleren dat de groep PhoneFactor Admins inderdaad is gemaakt en dat de AD FS-serviceaccount lid van deze groep is. Indien nodig, de AD FS-serviceaccount handmatig toevoegen aan de groep PhoneFactor Admins op uw domeincontroller.

### <a name="user-portal"></a>User Portal
Deze portal wordt uitgevoerd in een Internet Information Server (IIS) de website kunt zelf mogelijkheden en biedt een volledige reeks gebruiker beheermogelijkheden. Gebruik de onderstaande richtlijnen om dit onderdeel te configureren:

- IIS 6 of hoger is vereist
- ASP.NET-v2.0.507207 moet worden geïnstalleerd en geregistreerd
- Deze server kan worden geïmplementeerd in een perimeternetwerk.



### <a name="app-passwords"></a>App-wachtwoorden
Als uw organisatie is federatieve eenmalige aanmelding gebruiken met Azure AD en u gaat worden met behulp van Azure MVR gesloten, vervolgens worden op de hoogte van de volgende handelingen uit bij het gebruik van app wachtwoorden (Houd er rekening mee dat dit alleen voor federatieve (SSO geldt) wordt gebruikt):

- Het App-wachtwoord is geverifieerd door AD Azure en daarom worden overgeslagen federation. Federatie wordt alleen gebruikt bij het instellen van het App-wachtwoord.
- Voor federatieve (SSO) worden gebruikers wachtwoorden opgeslagen in de organisatie-id. Als de gebruiker het bedrijf verlaat, heeft dat info en organisatie-id met DirSync in real time. Account uitschakelen/verwijderen kan duren tot 3 uur te synchroniseren, uitschakelen/verwijderen van het App-wachtwoord in Azure AD vertragen.
- Client Access Control-instellingen op de ruimten worden niet gehonoreerd door App wachtwoord
- Er is geen verificatie op ruimten logging / controle mogelijkheden beschikbaar voor App-wachtwoord
- Meer opleiding van eindgebruikers is vereist voor de Microsoft Lync 2013 client.
- Bepaalde geavanceerde bouwkundige ontwerpen moet u mogelijk met behulp van een combinatie van organisatie-gebruikersnaam en wachtwoorden app wanneer meerledige verificatie mogelijk met clients, afhankelijk van waar ze worden geverifieerd. Voor clients die worden geverifieerd bij een infrastructuur op lokalen, gebruikt u een organisatie-gebruikersnaam en wachtwoord. Voor clients die worden geverifieerd bij AD Azure, gebruikt u het wachtwoord voor de app.
- Standaard kunnen gebruikers app-wachtwoorden maken als uw bedrijf is het nodig dat of als u dat gebruikers wilt kunnen app wachtwoord maken in bepaalde scenario's, ervoor te zorgen dat de optie toestaan gebruikers wachtwoorden app aanmelden bij browser-toepassingen te maken is geselecteerd.

## <a name="additional-considerations"></a>Aanvullende overwegingen
Gebruik de lijst hieronder te begrijpen sommige extra overwegingen en aanbevolen procedures voor elk onderdeel dat wordt geïmplementeerd op ruimten:

Methode|Beschrijving
:------------- | :------------- |
[Active Directory Federation-Service](multi-factor-authentication-get-started-adfs.md)|Informatie over het instellen van Azure meerledige verificatie met AD FS.
[RADIUS-Authenticaton](multi-factor-authentication-get-started-server-radius.md)|  Informatie over het instellen en configureren van de Server van de MVR gesloten Azure met straal.
[IIS-verificatie](multi-factor-authentication-get-started-server-iis.md)|Informatie over het instellen en configureren van de MVR gesloten Azure Server met IIS.
[Windows Authenticaton](multi-factor-authentication-get-started-server-windows.md)|  Informatie over het instellen en configureren van de MVR gesloten Azure Server met Windows-verificatie.
[LDAP-verificatie](multi-factor-authentication-get-started-server-ldap.md)|Informatie over het instellen en configureren van de Server van de MVR gesloten Azure met LDAP-verificatie.
[Extern bureaublad-Gateway en Azure meerledige verificatieserver RADIUS gebruiken](multi-factor-authentication-get-started-server-rdg.md)|  Informatie over het instellen en configureren van de Server van de MVR gesloten Azure met extern bureaublad-Gateway RADIUS gebruiken.
[Synchronisatie met Windows Server Active Directory](multi-factor-authentication-get-started-server-dirint.md)|Informatie over het instellen en configureren van synchronisatie tussen Active Directory en de Server van de MVR gesloten Azure.
[De Azure meerledige verificatie Server mobiele App Web Service implementeren](multi-factor-authentication-get-started-server-webservice.md)|Informatie over het instellen en configureren van de MVR gesloten Azure server webservice.
[Geavanceerde VPN-configuratie met Azure meerledige verificatie](multi-factor-authentication-advanced-vpn-configurations.md)|Informatie over het configureren van Cisco ASA, Citrix Netscaler en jeneverbes/Pulse beveiligde VPN-apparaten, LDAP of van RADIUS.


## <a name="additional-resources"></a>Aanvullende bronnen
In dit artikel worden enkele aanbevelingen voor Azure MVR gesloten, maar er zijn andere bronnen die u ook gebruiken kunt bij het plannen van de implementatie van de MVR gesloten. De onderstaande lijst bevat enkele belangrijke artikelen die u tijdens dit proces helpen kunnen:

- [Rapporten in Azure meerledige verificatie](multi-factor-authentication-manage-reports.md)
- [De mogelijkheden van Azure meerledige verificatie instellen](multi-factor-authentication-end-user-first-time.md)
- [Veelgestelde vragen over Azure meerledige verificatie](multi-factor-authentication-faq.md)
