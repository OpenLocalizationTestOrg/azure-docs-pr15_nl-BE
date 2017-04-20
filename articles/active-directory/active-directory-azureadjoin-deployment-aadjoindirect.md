<properties
    pageTitle="Gebruiksscenario's en overwegingen voor de implementatie voor Azure AD deelnemen | Microsoft Azure"
    description="Wordt beschreven hoe beheerders kunnen instellen Azure AD Join voor hun eindgebruikers (werknemers, studenten en andere gebruikers). Ook komen de verschillende concrete scenario's voor het gebruik van Azure AD deelnemen."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

< tags "active directory" ms.service= ms.workload="identity" ms.tgt_pltfrm="na" ms.devlang="na" ms.topic="article" ms.date="09/27/2016"

    ms.author="femila"/>

# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a>Gebruiksscenario's en overwegingen voor de implementatie voor Azure AD Join

## <a name="usage-scenarios-for-azure-ad-join"></a>Gebruiksscenario's voor Azure AD Join
### <a name="scenario-1-businesses-largely-in-the-cloud"></a>Scenario 1: Bedrijven grotendeels in de cloud

Azure Active Directory Join (Azure AD Join) profiteert u als u momenteel werkt en identiteiten voor uw bedrijf in de cloud beheren of snel naar de cloud verplaatsen wilt. U kunt een account die u hebt gemaakt in Azure AD aanmelden bij Windows 10. Bij [de eerste run ervaring (FRX)](active-directory-azureadjoin-user-frx.md), of door lid te worden van Azure AD in [het menu instellingen](active-directory-azureadjoin-user-upgrade.md), kunnen uw gebruikers hun computers toevoegen aan Azure AD.  De gebruikers kunnen ook genieten van enkele aanmelding (SSO) toegang tot bronnen van de wolk als Office 365, in de browser of in Office-toepassingen.

### <a name="scenario-2-educational-institutions"></a>Scenario 2: Educatieve instellingen

Educatieve instellingen hebben meestal twee gebruikerstypen: docenten en studenten. Faculteitleden worden beschouwd als op de langere termijn van de leden van de organisatie. Het is wenselijk in lokalen accounts maken voor hen. Maar studenten korterlopende lid zijn van de organisatie en hun accounts kunnen worden beheerd in Azure AD. Dit betekent dat directory schaal kan worden geduwd naar de cloud worden opgeslagen in ruimten. Het betekent ook dat studenten is het mogelijk om te melden bij Windows met hun Azure AD-account en toegang krijgen tot bronnen voor Office 365 in Office-toepassingen.

### <a name="scenario-3-retail-businesses"></a>Scenario 3: Detailhandelaren

Detailhandelaren hebben seizoenarbeiders en op lange termijn werknemers. U meestal Maak accounts op gebouwen en machines domein behoren voor langerlopende voltijdse werknemers. Maar seizoenarbeiders korterlopende lid zijn van de organisatie en het is wenselijk voor het beheer van hun rekeningen waar gebruikerslicenties gemakkelijk rond kunnen worden verplaatst. Als u de gebruikersaccounts in de cloud met Office 365 licenties maakt, krijgen deze gebruikers de voordelen van het aanmelden bij Windows en Office-toepassingen met een Azure AD-account, terwijl u meer flexibiliteit met hun licenties onderhouden nadat ze verlaten.

### <a name="scenario-4-additional-scenarios"></a>Scenario 4: Nog meer scenario 's

Samen met de voordelen die eerder is besproken, profiteert u dat uw gebruikers hun apparaten toevoegen aan AD Azure vanwege een vereenvoudigde gekoppelde ervaring, efficiënt beheer, automatische mobiel apparaat beheer inschrijving en eenmalige aanmelding naar Azure AD en resources bedrijfsruimten.  


## <a name="deployment-considerations-for-azure-ad-join"></a>Aandachtspunten bij de implementatie voor Azure AD Join

### <a name="enable-your-users-to-join-a-company-owned-device-directly-to-azure-ad"></a>Uw gebruikers kunnen deelnemen aan een bedrijf eigendom apparaat rechtstreeks naar Azure AD


Ondernemingen kunnen alleen cloud-accounts bieden aan partnerbedrijven en organisaties. Deze partners vervolgens eenvoudig toegang tot bedrijf apps en resources met eenmalige aanmelding. In dit scenario is van toepassing op gebruikers die toegang hebben tot bronnen, voornamelijk in de cloud, zoals Office 365 of SaaS-toepassingen die afhankelijk van Azure AD voor verificatie zijn.

### <a name="prerequisites"></a>Vereisten
**Op ondernemingsniveau (administrator)**

*   Azure abonnement met Azure Active Directory  

**Op het gebruikersniveau van de**

*   Windows 10 (Professional en Enterprise editions)

### <a name="administrator-tasks"></a>Beheerderstaken
* [Instellen van de registratie](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Gebruikerstaken
* [Een nieuwe Windows 10-apparaat met Azure Active Directory tijdens de installatie instellen](active-directory-azureadjoin-user-frx.md)
* [Een Windows 10-apparaat met Azure Active Directory instellen in het menu instellingen](active-directory-azureadjoin-user-upgrade.md)
* [Een persoonlijke Windows 10-apparaat toevoegen aan uw organisatie](active-directory-azureadjoin-personal-device.md)



## <a name="enable-byod-in-your-organization-for-windows-10"></a>BYOD inschakelen in uw organisatie voor Windows 10
U kunt instellen van uw gebruikers en werknemers hun persoonlijke apparaten voor Windows (BYOD) voor toegang tot bedrijf apps en bronnen te gebruiken. Uw gebruikers kunnen hun accounts Azure AD (werk of school rekeningen) toevoegen aan een Windows-apparaat voor toegang tot bronnen in een veilige en geschikte manier persoonlijke.

### <a name="prerequisites"></a>Vereisten
**Op ondernemingsniveau (administrator)**

*   Azure AD-abonnement

**Op het gebruikersniveau van de**

*   Windows 10 (Professional en Enterprise editions)


### <a name="administrator-tasks"></a>Beheerderstaken

* [Instellen van de registratie](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Gebruikerstaken
* [Een persoonlijke Windows 10-apparaat toevoegen aan uw organisatie](active-directory-azureadjoin-personal-device.md)


## <a name="additional-information"></a>Als u meer informatie
* [Windows 10 voor de onderneming: apparaten gebruik voor werk](active-directory-azureadjoin-windows10-devices-overview.md)
* [Cloud-mogelijkheden aan Windows 10-apparaten via Azure Active Directory deelnemen aan uitbreiden](active-directory-azureadjoin-user-upgrade.md)
* [Verificatie van identiteiten zonder wachtwoorden via Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Meer informatie over gebruiksscenario's voor Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Domein behoren apparaten aansluiten op Azure AD voor Windows 10 ervaringen](active-directory-azureadjoin-devices-group-policy.md)
* [Azure AD Join instellen](active-directory-azureadjoin-setup.md)
