<properties
    pageTitle="Azure meerledige verificatie - volgende"
    description="Dit is de pagina Azure meerledige verificatie die wat u beschrijft wilt doen met de MVR gesloten.  Dit geldt ook voor rapporten, waarschuwing, eenmalige bypass, aangepaste voicemailberichten in cache opslaan van wachtwoorden voor vertrouwde IP-adressen en app."
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
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="kgremban"/>

# <a name="configuring-azure-multi-factor-authentication"></a>Azure meerledige verificatie configureren

Dit artikel helpt u bij Azure meerledige verificatie nu dat u actief bent.  Het wordt een groot aantal onderwerpen die u helpen bij het optimaal Azure meerledige verificatie.  Niet al deze functies zijn beschikbaar in elke versie van Azure meerledige verificatie.

De configuratie voor sommige van de onderstaande functies is gevonden in de Portal voor beheer van Azure meerledige verificatie. Er zijn twee verschillende manieren die u toegang hebt tot de MVR gesloten management portal, die zowel via de portal Azure worden uitgevoerd. De eerste is door het beheer van een meerledige verificatie Provider als MVR gesloten op basis van verbruik. De tweede is via de instellingen van de MVR gesloten. De tweede optie is een meerledige verificatie-Provider of een Azure MVR gesloten, Azure AD Premium of Enterprise Mobility Suite-licentie vereist.

De Azure portal als beheerder aanmelden voor toegang tot de Portal van de MVR gesloten beheer via een Provider Azure meerledige verificatie, en selecteer de optie Active Directory. Klik op het tabblad **Meerledige verificatie Providers** en vervolgens selecteert u de map en klik op de knop **beheren** onder.

De Azure portal als beheerder aanmelden voor toegang tot de Portal van de MVR gesloten beheer via de pagina instellingen van de MVR gesloten, en selecteer de optie Active Directory. Klik op de map en klik vervolgens op het tabblad **configureren** . Selecteer in de sectie meerledige verificatie **service-instellingen beheren**. Onderaan op de pagina instellingen van de MVR gesloten, klikt u op de koppeling **gaat u naar de portal** .


Functie| Beschrijving| Wat is gedekt
:------------- | :------------- | :------------- |
[Fraudewaarschuwing](#fraud-alert)|Fraudewaarschuwing kan worden geconfigureerd en zo instellen dat uw gebruikers kunnen rapporteren frauduleuze pogingen toegang te krijgen tot de bronnen ervan.|Het instellen, configureren en fraude melden
[Eenmalige overslaan](#one-time-bypass) |Een eenmalige bypass kan een gebruiker één keer door te "negeren" meerledige verificatie verifiëren.|Het instellen en configureren van een eenmalige overslaan
[Aangepaste gesproken berichten](#custom-voice-messages) |Aangepaste gesproken berichten kunnen u uw eigen opnamen of greetings met meerledige verificatie gebruiken. |Het instellen en configureren van aangepaste begroetingen en berichten
[In cache plaatsen](#caching-in-azure-multi-factor-authentication)|Caching kunt u een specifiek tijdstip periode zo instellen dat van daaropvolgende verificatiepogingen automatisch mislukt. |Informatie over het instellen en configureren van verificatie in de cache opslaan.
[Vertrouwde IP-adressen](#trusted-ips)|Vertrouwde dat IP-adressen is een functie van een meerledige verificatie waarmee beheerders van een huurder beheerde of federatieve meerledige verificatie voor gebruikers die vanaf het lokale bedrijfsintranet aanmelden te omzeilen.|Configureren en IP-adressen die vrijgesteld voor meerledige verificatie zijn instellen
[App-wachtwoorden](#app-passwords)|Een app wachtwoord kan een toepassing die geen MVR gesloten bewust meerledige verificatie overslaan en doorgaan met werken.|Informatie over wachtwoorden app.
[Meerledige verificatie onthouden voor apparaten onthouden en browsers](#remember-multi-factor-authentication-for-devices-users-trust)|Hiermee kunt u apparaten onthouden voor een bepaald aantal dagen nadat een gebruiker is aangemeld met behulp van de MVR gesloten.|Informatie over het inschakelen van deze functie en het instellen van het aantal dagen.
[Selecteerbare verificatiemethoden](#selectable-verification-methods)|U kunt kiezen de verificatiemethoden die beschikbaar zijn voor gebruikers om te gebruiken.|Informatie over het in- of uitschakelen van specifieke verificatiemethoden zoals call of tekst berichten.



## <a name="fraud-alert"></a>Fraudewaarschuwing
Fraudewaarschuwing kan worden geconfigureerd en zo instellen dat uw gebruikers kunnen rapporteren frauduleuze pogingen toegang te krijgen tot de bronnen ervan.  Gebruikers kunnen rapporteren fraude met de mobiele app of via de telefoon.

### <a name="to-set-up-and-configure-fraud-alert"></a>Instellen en configureren van fraudewaarschuwing

1.  Meld u aan bij http://azure.microsoft.com
2.  Ga naar de MVR gesloten Management Portal per de instructies boven aan deze pagina.
3.  Klik op instellingen in de sectie configureren in Azure meerledige verificatie beheerportal.
4.  Controleer onder de fraude Alert sectie van de pagina-instellingen, de gebruikers toestaan om Fraudewaarschuwingen checkbox indienen.
5.  Als u wilt dat gebruikers worden geblokkeerd wanneer fraude wordt gemeld, schakel dit selectievakje in gebruiker blokkeren wanneer fraude wordt gemeld.
6.  Voer een numerieke code die kan worden gebruikt tijdens de controle van de oproep in het tekstvak **Code naar rapport fraude tijdens de eerste begroeting** . Als een gebruiker deze code plus # in plaats van alleen het teken #, wordt een waarschuwing worden gerapporteerd.
7.  Aan de onderkant, klik op opslaan.

>[AZURE.NOTE]
>Microsoft standaard gesproken begroetingen vraag gebruikers op 0# in te dienen een fraudewaarschuwing. Als u wilt met een code die gelijk zijn aan 0, moet u opnemen en uploaden van uw eigen aangepaste gesproken begroetingen met passende instructies.


![Wolk](./media/multi-factor-authentication-whats-next/fraud.png)

### <a name="to-report-fraud-alert"></a>Rapport fraude signalering
Fraudewaarschuwing kan op twee manieren worden gerapporteerd.  Hetzij via de mobiele app of via de telefoon.  

### <a name="to-report-fraud-alert-with-the-mobile-app"></a>Met een fraudewaarschuwing rapport met de mobiele app



1. Wanneer een controle is verzonden naar uw telefoon, selecteert u het starten van de Microsoft-Authenticator app.
2. Fraude melden, klikt u op Annuleren en rapport fraude. Verschijnt een vak dat medewerkers van de IT-ondersteuning van uw organisatie krijgt.
3. Klik op rapport fraude.
4. Op de app, klikt u op sluiten.

![Wolk](./media/multi-factor-authentication-whats-next/report1.png)


![Wolk](./media/multi-factor-authentication-whats-next/fraud2.png)

### <a name="to-report-fraud-alert-with-the-phone"></a>Met een fraudewaarschuwing rapport met de telefoon

1. Als een controle-oproep op uw telefoon binnenkomt, beantwoordt u deze.  
2. Fraude melden, geeft u de code die is geconfigureerd om te corresponderen met het melden van fraude via de telefoon en vervolgens het teken #. U krijgt een waarschuwing is ingediend.
3. De oproep beëindigen.

### <a name="to-view-the-fraud-report"></a>De fraude rapport weergeven

1. Meld u aan bij [http://azure.microsoft.com](https://azure.microsoft.com/)
2. Selecteer Active Directory aan de linkerkant.
3. Selecteer boven meerledige verificatie Providers. Verschijnt een lijst met uw Providers meerledige verificatie.
4. Als u meer dan een meerledige verificatie-Provider hebt, selecteer de gewenste fraude alert rapport weergeven en klik op beheren onder aan de pagina. Als er slechts één, klik op beheren. Hiermee opent u de Portal voor beheer van Azure meerledige verificatie.
5. Klik op fraude Alert op de Azure meerledige verificatie Management Portal, links, onder de A-rapport weergeven.
6. Geef het datumbereik dat u wilt weergeven in het rapport. U kunt ook een specifieke gebruikersnamen, telefoonnummers en de status van de gebruiker opgeven.
7. Klik op uitvoeren. Dit verschijnt lijkt op het onderstaande rapport. Als u het rapport exporteert, kunt u exporteren naar CSV klikken.

## <a name="one-time-bypass"></a>Eenmalige overslaan

Een eenmalige bypass kan een gebruiker één keer door te "negeren" meerledige verificatie verifiëren. De bypass is tijdelijk en verloopt na het opgegeven aantal seconden.  Dus in situaties waar de mobiele app of de telefoon niet een melding of een telefoongesprek ontvangt kunt u een eenmalige omzeilen zodat de gebruiker toegang heeft tot de gewenste resource.

### <a name="to-create-a-one-time-bypass"></a>Voor het maken van een eenmalige overslaan

1.  Meld u aan bij http://azure.microsoft.com
2.  Ga naar de MVR gesloten Management Portal per de instructies boven aan deze pagina.
3.  In de Azure meerledige verificatie Management Portal, als u de naam van de huurder of Azure MVR gesloten Provider Zie aan de linkerkant met een + ernaast, klik op de + Zie verschillende MVR gesloten replicatie servergroepen en de groep standaard Azure. Klik op de gewenste groep.
4.  Klik op **Overslaan One-Time**onder beheer van de gebruiker.
![Wolk](./media/multi-factor-authentication-whats-next/create1.png)
5.  Klik op de pagina One-Time Bypass **Nieuwe One-Time overslaan**.
6.  De gebruikersnaam, het aantal seconden dat de bypass zal bestaan, de reden voor het overslaan en klik op **overslaan**.
![Wolk](./media/multi-factor-authentication-whats-next/create2.png)
7.  Op dit moment de gebruiker zich aanmelden voor de eenmalige bypass verloopt.



### <a name="to-view-the-one-time-bypass-report"></a>De eenmalige bypass-rapport weergeven

1. Meld u aan bij [http://azure.microsoft.com](https://azure.microsoft.com/)
2. Selecteer Active Directory aan de linkerkant.
3. Selecteer boven meerledige verificatie Providers. Verschijnt een lijst met uw Providers meerledige verificatie.
4. Als u meer dan een meerledige verificatie-Provider hebt, selecteer de gewenste fraude alert rapport weergeven en klik op beheren onder aan de pagina. Als er slechts één, klik op beheren. Hiermee opent u de Portal voor beheer van Azure meerledige verificatie.
5. Klik op de Azure meerledige verificatie Management Portal, links, onder A-rapport weergeven, One-Time overslaan.
6. Geef het datumbereik dat u wilt weergeven in het rapport. U kunt ook een specifieke gebruikersnamen, telefoonnummers en de status van de gebruiker opgeven.
7. Klik op uitvoeren. Dit verschijnt lijkt op het onderstaande rapport. Als u het rapport exporteert, kunt u exporteren naar CSV klikken.

<center>![Wolk](./media/multi-factor-authentication-whats-next/report.png)</center>

## <a name="custom-voice-messages"></a>Aangepaste gesproken berichten

Aangepaste gesproken berichten kunnen u uw eigen opnamen of greetings met meerledige verificatie gebruiken.  Deze kunnen worden gebruikt in aanvulling op of ter vervanging van de Microsoft-records.

Voordat u op de hoogte van de volgende zijn:

- De ondersteunde indelingen zijn wav en MP3.
- De maximale bestandsgrootte is 5 MB.
- Het wordt aanbevolen dat voor verificatie dat berichten het niet langer zijn dan 20 seconden. Iets meer dan dit kan ervoor zorgen dat de verificatie is mislukt omdat de gebruiker reageert niet meer voordat het bericht is voltooid en de time-out voor de verificatie.



### <a name="to-set-up-custom-voice-messages-in-azure-multi-factor-authentication"></a>Aangepaste voicemailberichten in Azure meerledige verificatie instellen
1.  Maak een aangepaste voicemailbericht met behulp van een van de ondersteunde bestandsindelingen.
2.  Meld u aan bij http://azure.microsoft.com
3.  Ga naar de MVR gesloten Management Portal per de instructies boven aan deze pagina.
4.  Klik in de Portal Azure meerledige verificatie Management gesproken berichten onder de sectie configureren.
5.  Klik op **Nieuw bericht**in de sectie gesproken berichten.
![Wolk](./media/multi-factor-authentication-whats-next/custom1.png)
6.  Op de configureren: nieuwe voicemailberichten pagina, klikt u op **Geluidsbestanden beheren**.
![Wolk](./media/multi-factor-authentication-whats-next/custom2.png)
7.  Op de configureren: Sound-bestanden, pagina, klikt u op **Bestand uploaden**.
![Wolk](./media/multi-factor-authentication-whats-next/custom3.png)
8.  Op de configureren: geluidsbestand uploaden, klikt u op **Bladeren** en navigeer naar het gesproken bericht, klik op **openen**.
![Wolk](./media/multi-factor-authentication-whats-next/custom4.png)
9.  Een beschrijving toevoegen en klik op uploaden.
10. Nadat dit is voltooid, wordt een bericht wordt bevestigd dat het bestand is geüpload.
11. Klik op de links, gesproken berichten.
12. Klik op Nieuw bericht in de sectie gesproken berichten.
13. Selecteer een taal in de vervolgkeuzelijst taal.
14. Als dit bericht voor een specifieke toepassing is, geeft u in het vak.
15. Selecteer het soort bericht dat moet worden overschreven met onze nieuwe aangepaste bericht van het Type bericht.
16. Selecteer in de vervolgkeuzelijst geluidsbestand uw geluidsbestand.
17. Klik op **maken**. Een bericht wordt bevestigd dat u hebt gemaakt met een gesproken bericht.
![Wolk](./media/multi-factor-authentication-whats-next/custom5.png)</center>



## <a name="caching-in-azure-multi-factor-authentication"></a>Opslaan in cache in Azure meerledige verificatie

Caching kunt u een specifiek tijdstip periode zo instellen dat van daaropvolgende verificatiepogingen automatisch mislukt. Dit wordt vooral gebruikt wanneer op ruimten systemen, zoals VPN-meerdere aanvragen voor verificatie verzendt terwijl het eerste verzoek nog steeds in behandeling is. Dit kan de volgende aanvragen automatisch uitgevoerd nadat de gebruiker de verificatie bezig is gelukt. Opmerking: caching niet is bedoeld om te worden gebruikt voor aanmeldingen naar Azure AD.


### <a name="to-set-up-caching-in-azure-multi-factor-authentication"></a>Opslaan in cache in Azure meerledige verificatie instellen

1.  Meld u aan bij http://azure.microsoft.com
2.  Ga naar de MVR gesloten Management Portal per de instructies boven aan deze pagina.
3.  Klik op cache onder de sectie configureren in Azure meerledige verificatie beheerportal.
4.  Klik op de pagina caching configureren op nieuwe Cache
5.  Selecteer het type Cache en de cache seconden. Klik op maken.

<center>![Wolk](./media/multi-factor-authentication-whats-next/cache.png)</center>

## <a name="trusted-ips"></a>Vertrouwde IP-adressen

Vertrouwde dat IP-adressen is een functie van een meerledige verificatie waarmee beheerders van een huurder beheerde of federatieve meerledige verificatie voor gebruikers die vanaf het lokale bedrijfsintranet aanmelden te omzeilen. De functies zijn beschikbaar voor huurders Azure AD die Azure AD Premium, Enterprise Mobility Suite of Azure meerledige verificatie-licenties hebt.


Type Azure AD huurder| Vertrouwde IP-opties
:------------- | :------------- |
Beheerd|Specifieke IP-adresbereiken-beheerders kunnen een bereik van IP-adressen die kunnen omzeilen meerledige verificatie voor gebruikers die vanaf het intranet van het bedrijf aanmelden opgeven.
Federatieve|<li>Alle federatieve gebruikers - alle federatieve gebruikers die zich aanmelden vanaf binnen de organisatie meerledige verificatie met behulp van een claim die door AD FS wordt omzeild.</li><li>Specifieke IP-adresbereiken-beheerders kunnen een bereik van IP-adressen die kunnen omzeilen meerledige verificatie voor gebruikers die vanaf het intranet van het bedrijf aanmelden opgeven.

Dit werkt alleen uit binnen het intranet van een bedrijf overslaan. Dus bijvoorbeeld, als u alleen alle federatieve gebruikers geselecteerd en een gebruiker zich aanmeldt heeft van buiten het intranet van het bedrijf, die gebruiker voor het verifiëren van meerledige verificatie gebruiken, zelfs als de gebruiker aanbiedt in een AD FS-claims. De volgende tabel wordt beschreven wanneer meerledige verificatie en app wachtwoorden zijn vereist binnen uw corpnet en buiten uw corpnet bij vertrouwde IP-adressen is ingeschakeld.


|Vertrouwde IP-adressen ingeschakeld| Vertrouwde IP-adressen uitgeschakeld
:------------- | :------------- | :------------- |
Inside corpnet|Voor browser-stromen meerledige verificatie niet vereist.|Voor browser-stromen meerledige verificatie vereist
|Wachtwoorden regelmatig werken voor rijke toepassingen, als de gebruiker wachtwoorden app niet gemaakt. Zodra een app wachtwoord is aangemaakt, zijn app wachtwoorden vereist.|Voor rijke apps, app wachtwoorden vereist
Buiten corpnet|Voor browser-stromen meerledige verificatie vereist.|Voor browser-stromen meerledige verificatie vereist.
|Voor rijke apps, app wachtwoorden vereist.|Voor rijke apps, app wachtwoorden vereist.

### <a name="to-enable-trusted-ips"></a>Vertrouwde IP-adressen inschakelen

1. Aanmelden bij de klassieke Azure portal.
2. Aan de linkerkant, klikt u op Active Directory.
3. Klik onder op de map op de map die u wilt instellen van vertrouwde IPsing op.
4. Klik op de map die u hebt geselecteerd, klikt u op configureren.
5. Klik op service-instellingen beheren in de sectie meerledige verificatie.
6. Selecteer op de pagina instellingen van de Service onder vertrouwde IP-adressen, hetzij:

    - Voor het aanvragen van federatieve gebruikers die afkomstig zijn uit mijn intranet – alle federatieve zullen gebruikers die zich wilt met het bedrijfsnetwerk aanmelden meerledige verificatie met behulp van een claim die door AD FS overslaan.
    - Voor het aanvragen van een bereik van openbare IP-adressen: Voer de IP-adressen in de vakken met de CIDR-notatie. Bijvoorbeeld: xxx.xxx.xxx.0/24 voor IP-adressen in het bereik xxx.xxx.xxx.1 – xxx.xxx.xxx.254 of xxx.xxx.xxx.xxx/32 voor een enkel IP-adres. U kunt maximaal 50 IP-adresbereiken opgeven.

7. Klik op opslaan.
8. Nadat de updates zijn toegepast, klikt u op sluiten.



![Vertrouwde IP-adressen](./media/multi-factor-authentication-whats-next/trustedips3.png)




## <a name="app-passwords"></a>App-wachtwoorden

In sommige toepassingen, zoals Office 2010 of ouder en Apple Mail u meerledige verificatie niet gebruiken.  Als u wilt deze apps gebruikt, moet u 'wachtwoorden app' te gebruiken in plaats van de traditionele wachtwoord.  Het app-wachtwoord kan de toepassing meerledige verificatie overslaan en doorgaan met werken.

>[AZURE.NOTE] Moderne verificatie voor de Clients Office 2013
>
> Office 2013-clients (inclusief Outlook) nu nieuwe verificatieprotocollen ondersteunen en ondersteuning meerledige verificatie kunnen worden ingeschakeld.  Dit betekent dat eenmaal is ingeschakeld, app wachtwoorden zijn niet vereist voor gebruik met Office 2013-clients.  Zie voor meer informatie, [Office 2013 moderne verificatie public preview aangekondigd](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).



### <a name="important-things-to-know-about-app-passwords"></a>Belangrijke dingen te weten over app wachtwoorden

Hier volgt een lijst met belangrijke dingen die u over app wachtwoorden weten moet.

- Gebruikers kunnen meerdere app wachtwoorden, waardoor het oppervlak voor diefstal hebben. Aangezien app wachtwoorden moeilijk zijn te onthouden zijn, kan deze stimuleren dit opschrijven. Dit wordt niet aanbevolen en moet worden afgeraden omdat slechts één factor vereist voor aanmelding bij het app-wachtwoord is.
- Apps gebruiken in scenario's voor ruimten die wachtwoorden in de cache mogelijk start mislukt omdat het app-wachtwoord buiten de organisatie-id is niet bekend. Een voorbeeld is Exchange e-mails die op de bedrijfsruimten worden maar gearchiveerde e-mail in de cloud. Het wachtwoord werkt niet.
- Het wachtwoord zelf wordt automatisch gegenereerd en niet door de gebruiker worden verstrekt. Dit is omdat de automatisch gegenereerde wachtwoord moeilijker door een aanvaller te raden en is veiliger.
- Er is momenteel een maximum van 40 wachtwoorden per gebruiker. U wordt gevraagd een van uw bestaande app wachtwoorden verwijderen om een nieuwe maken.
- Eenmaal meerledige verificatie is ingeschakeld voor de account van een gebruiker, app wachtwoorden kunnen worden gebruikt met de meeste browser-clients zoals Outlook en Lync, maar administratieve acties kunnen niet worden uitgevoerd met behulp van wachtwoorden via de browser-toepassingen zoals Windows PowerShell app zelfs als die gebruiker een Administrator-account heeft.  Zorg dat u een service-account maken met een sterk wachtwoord PowerShell scripts worden uitgevoerd en die account voor meerledige verificatie niet inschakelen.

>[AZURE.WARNING]  App wachtwoorden werken niet in hybride omgevingen waarin clients communiceren met beide ruimten op en autodiscover endpoints van de wolk. Dit is omdat het domeinwachtwoorden zijn vereist voor de verificatie op gebouwen en app wachtwoorden zijn vereist voor het verifiëren van de wolk.


### <a name="naming-guidance-for-app-passwords"></a>Richtlijnen voor naamgeving voor wachtwoorden App
Het wordt aanbevolen dat app wachtwoord namen moeten overeenkomen met het apparaat waarop zij worden gebruikt. Bijvoorbeeld als u een laptop met een browser-apps zoals Outlook, Word en Excel hebt, hoeft u alleen een app wachtwoord met de naam Laptop maken en gebruiken van dit app wachtwoord in al deze toepassingen. U kunt afzonderlijke wachtwoorden voor al deze toepassingen maken, wordt het niet aanbevolen. De aanbevolen is een app wachtwoord per apparaat te gebruiken.


<center>![Wolk](./media/multi-factor-authentication-whats-next/naming.png)</center>


### <a name="federated-sso-app-passwords"></a>Federatieve (SSO) App wachtwoorden
Azure AD ondersteunt federation met voor bedrijfsruimten Windows Server Active Directory Domain Services (AD DS). Als uw organisatie is een federated(SSO) met Azure Active Directory en u worden met behulp van Azure meerledige verificatie, gaat dan is de volgende belangrijke informatie die u rekening houden moet bij het gebruik van app-wachtwoorden. Dit geldt alleen voor federated(SSO) klanten.

- Het App-wachtwoord is geverifieerd door AD Azure en vandaar omzeilt federation. Federatie alleen actief wordt gebruikt bij het instellen van het App-wachtwoord.
- Voor federated(SSO) gebruikers gaat we nooit u naar de identiteitsprovider (IdP) in tegenstelling tot de passieve stroom. De wachtwoorden worden opgeslagen in de organisatie-id. Als de gebruiker het bedrijf verlaat, heeft dat info en organisatie-id met DirSync in real time. Account uitschakelen/verwijderen duurt tot drie uur voor synchronisatie, uitschakelen/verwijderen van het App-wachtwoord in Azure AD vertragen.
- Client Access Control-instellingen op de ruimten worden niet gehonoreerd door App wachtwoord
- Er is geen verificatie op ruimten logging / controle mogelijkheden beschikbaar voor App-wachtwoord
- Meer opleiding van eindgebruikers is vereist voor de Microsoft Lync 2013 client. Zie hoe u het wachtwoord wijzigt in uw e-mailadres het app-wachtwoord voor de vereiste stappen.
- Bepaalde geavanceerde bouwkundige ontwerpen moet u mogelijk met behulp van een combinatie van organisatie-gebruikersnaam en wachtwoorden app wanneer meerledige verificatie mogelijk met clients, afhankelijk van waar ze worden geverifieerd. Voor clients die worden geverifieerd bij een lokale infrastructuur, gebruikt u een organisatie-gebruikersnaam en wachtwoord. Voor clients die worden geverifieerd bij AD Azure, gebruikt u het wachtwoord voor de app.

Stel bijvoorbeeld dat u beschikt over een architectuur die uit de volgende opties bestaat:

- U bent uw lokale exemplaar van Active Directory met Azure AD federeren
- U via Exchange online
- Gebruikt u Lync die specifiek lokale
- U gebruikt Azure meerledige verificatie


![Proofup](./media/multi-factor-authentication-whats-next/federated.png)

 In deze gevallen moet u het volgende doen:

- Bij het aanmelden bij Lync, gebruik de gebruikersnaam en wachtwoord van uw organisatie.
- Wanneer u probeert toegang te krijgen tot het adresboek via Outlook-client maakt verbinding met Exchange online, moet u een wachtwoord app gebruiken.

### <a name="allowing-app-password-creation"></a>App wachtwoord maken toestaan
Standaard kunnen gebruikers kunnen geen wachtwoorden app maken.  Deze functie moet worden ingeschakeld.  Als u wilt dat gebruikers de mogelijkheid om wachtwoorden app, gebruik de volgende procedure.

#### <a name="to-enable-users-to-create-app-passwords"></a>Om gebruikers wachtwoorden app maken



1. Log in om de klassieke Azure portal.
2. Aan de linkerkant, klikt u op Active Directory.
3. Klik onder op de map op de map voor de gebruiker die u wilt inschakelen.
4. Aan de bovenkant, klikt u op gebruikers.
5. Klik onderaan de pagina op beheren meerledige Auth.  
6. Klik op instellingen boven aan de pagina meerledige verificatie.
7. Zorg ervoor dat het keuzerondje naast de gebruikers kunnen app wachtwoorden aan te melden in browser-toepassingen te maken is geselecteerd.


![App wachtwoorden maken](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="creating-app-passwords"></a>App wachtwoorden maken
Gebruikers kunnen app wachtwoorden maken tijdens hun eerste inschrijving.  Ze krijgen een optie aan het einde van het registratieproces waarmee ze te maken.

Daarnaast maken gebruikers ook app wachtwoorden later door het wijzigen van de instellingen in de Azure portal voor de Office 365 portal of

### <a name="to-create-app-passwords-in-the-office-365-portal"></a>App wachtwoorden maken in de Office 365 portal
--------------------------------------------------------------------------------


1. Aanmelden bij de Office 365 portal
2. Selecteer de widget instellingen in de rechterbovenhoek.
3. Aan de linkerzijde, selecteert u aanvullende beveiliging verificatie
4. Aan de rechterzijde, selecteert u **Mijn telefoonnummers gebruikt voor accountbeveiliging**
5. Selecteer op de pagina proofup boven de app wachtwoorden
6. Klik op **maken**
7. Voer een naam voor het app-wachtwoord en klik op **volgende**
8. Het app-wachtwoord naar het Klembord kopiëren en plakken in uw app.

<center>![Wolk](./media/multi-factor-authentication-whats-next/security.png)</center>


### <a name="to-create-app-passwords-in-the-azure-portal"></a>App wachtwoorden maken in de portal voor Azure
--------------------------------------------------------------------------------
1. Log in om de klassieke Azure portal.
3. Boven, met de rechtermuisknop op de naam van de gebruiker en selecteert u aanvullende beveiliging-verificatie.
5. Selecteer op de pagina proofup boven de app wachtwoorden
6. Klik op **maken**
7. Voer een naam voor het app-wachtwoord en klik op **volgende**
8. Het app-wachtwoord naar het Klembord kopiëren en plakken in uw app.


![App-wachtwoorden](./media/multi-factor-authentication-whats-next/app2.png)

### <a name="to-create-app-passwords-if-you-do-not-have-an-office-365-or-azure-subscription"></a>App wachtwoorden maken als u beschikt niet over een abonnement op Office 365 of Azure
--------------------------------------------------------------------------------
1. Aanmelden bij [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Boven het profiel te selecteren.
3. Klik op uw gebruikersnaam en selecteer aanvullende beveiliging-verificatie.
5. Selecteer op de pagina proofup boven de app wachtwoorden
6. Klik op **maken**
7. Voer een naam voor het app-wachtwoord en klik op **volgende**
8. Het app-wachtwoord naar het Klembord kopiëren en plakken in uw app.

![App-wachtwoorden](./media/multi-factor-authentication-whats-next/myapp.png)

## <a name="remember-multi-factor-authentication-for-devices-users-trust"></a>Meerledige verificatie voor apparaten gebruikers vertrouwen onthouden

Meerledige verificatie onthouden voor apparaten en dat gebruikers vertrouwen een gratis functie voor alle gebruikers van de MVR gesloten is browsers.  U kunt geven gebruikers de mogelijkheid te overbruggen MVR gesloten voor een bepaald aantal dagen na het uitvoeren van een succesvolle aanmelden met behulp van de MVR gesloten. Dit kan het gebruiksgemak voor uw gebruikers verbeteren.

Aangezien de gebruikers onthouden MVR gesloten voor vertrouwde apparaten zijn toegestaan, kan deze functie echter accountbeveiliging verminderen. Account uit beveiligingsoverwegingen moet u meerledige verificatie voor de apparaten weer voor een van de volgende scenario's:

- Als de zakelijke account niet meer betrouwbaar
- Als een apparaat te onthouden is verloren of gestolen

> [AZURE.NOTE] Deze functie is geïmplementeerd als een cache van de browser cookie. Werkt niet als uw browsercookies niet zijn ingeschakeld.

### <a name="how-to-enabledisable-remember-multi-factor-authentication"></a>Hoe onthouden meerledige verificatie inschakelen/uitschakelen

1. Log in om de klassieke Azure portal.
2. Aan de linkerkant, klikt u op Active Directory.
3. Klik op de map die u wilt onthouden meerledige verificatie instellen voor apparaten onder Active Directory.
4. Klik op de map die u hebt geselecteerd, klikt u op configureren.
5. Klik op service-instellingen beheren in de sectie meerledige verificatie.
6. Klik op de pagina instellingen van de Service onder apparaat gebruikersinstellingen beheren, selecteren/selectie opheffen de **gebruikers toestaan om te onthouden meerledige verificatie op apparaten die ze vertrouwen**.
![Apparaten onthouden](./media/multi-factor-authentication-whats-next/remember.png)
8. Stel het aantal dagen dat u wilt toestaan dat de schorsing. De standaardwaarde is 14 dagen.
9. Klik op opslaan.
10. Klik op sluiten.


## <a name="selectable-verification-methods"></a>Selecteerbare verificatiemethoden
In de cloud en op gebouwen-versies kunt u kiezen welke verificatiemethoden beschikbaar zijn voor uw gebruikers. In de onderstaande tabel biedt een beknopt overzicht van elke methode.

Wanneer hun accounts gebruikers voor MVR gesloten inschrijven, kiest u de gewenste verificatiemethode van de opties die u hebt ingeschakeld. De richtlijnen voor het inschrijvingsproces worden behandeld in [Mijn account voor verificatie in twee stappen instellen](multi-factor-authentication-end-user-first-time.md)

Methode|Beschrijving
:------------- | :------------- |
Bel naar telefoon |  Hiermee plaatst u een geautomatiseerd telefoongesprek op de telefoon voor verificatie. De oproep wordt beantwoord en de gebruiker indrukt # in het toetsenblok van de telefoon te verifiëren. Dit telefoonnummer is niet gesynchroniseerd in Active Directory voor gebouwen.
SMS-bericht naar telefoon | Een tekstbericht met een verificatiecode naar de gebruiker verzendt. De gebruiker wordt gevraagd om een antwoord op het tekstbericht met de verificatiecode of Voer de verificatiecode in de interface van aanmelden.
Kennisgeving via mobiele app | In deze modus wordt de Microsoft Authenticator app voorkomt onbevoegde toegang tot accounts en frauduleuze transacties stopt. Dit wordt gedaan via een push-bericht naar je telefoon of apparaat geregistreerd. Weer te geven, de kennisgeving en als het rechtmatig is de integriteit controleren. Anders kan u weigeren of weigeren en rapporteren van de frauduleuze melding instellen. Zie voor meer informatie over het melden van frauduleuze berichten weigeren en fraude functie voor meerledige verificatie gebruiken.</br></br>De Microsoft Authenticator app is beschikbaar voor [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)en [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).|
Verificatiecode van mobiele app | In deze modus wordt kan de Microsoft Authenticator app worden gebruikt als een token software voor het genereren van een EED verificatiecode in. Deze verificatiecode kan vervolgens worden ingevoerd en de gebruikersnaam en wachtwoord op te geven van de tweede vorm van verificatie.</li><br><p> De Microsoft Authenticator app is beschikbaar voor [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)en [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

### <a name="how-to-enabledisable-authentication-methods"></a>Hoe u verificatiemethoden inschakelen/uitschakelen

1. Log in om de klassieke Azure portal.
2. Aan de linkerkant, klikt u op Active Directory.
3. Active Directory, klik op de map die u wilt in- of uitschakelen van verificatiemethoden.
4. Klik op de map die u hebt geselecteerd, klikt u op configureren.
5. Klik op service-instellingen beheren in de sectie meerledige verificatie.
6. Op de pagina instellingen onder de opties voor verificatie selecteren/selectie opheffen de opties die u wilt gebruiken.</br></br>
![Opties voor verificatie](./media/multi-factor-authentication-whats-next/authmethods.png)
9. Klik op opslaan.
10. Klik op sluiten.
