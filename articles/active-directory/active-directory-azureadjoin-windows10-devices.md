<properties
    pageTitle="Met Windows 10-apparaten in uw werkplek | Microsoft Azure"
    description="Biedt een overzicht van de mogelijkheden voor gebruikers en IT, het contrast van de verschillende manieren waarop een apparaat kan worden ingericht en gebruikt in een bedrijfsomgeving met Windows 10."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="femila"/>

# <a name="using-windows-10-devices-in-your-workplace"></a>Via Windows 10-apparaten in uw werkplek

Van toepassing op: Windows 10-pc's

Windows 10 biedt drie modellen voor organisaties die gebruikers kunnen toegang krijgen tot bronnen van werk in een veilige en gemakkelijke manier.

- **Lid worden van Azure Active Directory** (Azure AD Join) voor werknemers die toegang hebben tot bronnen, zoals Office 365, voornamelijk in de cloud. Azure AD Join is zelf werk ervaring die nieuw zijn in Windows 10 is ingericht.
- **Aan domein toevoegen**, voor organisaties die investeringen in apps op gebouwen en bronnen hebben. Aan domein toevoegen, biedt een betere ervaring in Windows 10 wanneer verbonden met Azure AD.
- **Een nieuwe vereenvoudigde BYOD ervaring**voor gebruikers die u wilt toevoegen van een werk of school aan Windows en eenvoudig toegang tot bronnen op persoonlijke apparaten.

De volgende tabel vindt een overzicht van de mogelijkheden voor gebruikers en IT-beheerders, de verschillende manieren waarop een apparaat kan worden ingericht en gebruikt in een bedrijfsomgeving met Windows 10 contrasterende:

|                                                                                                                                                                 | Aan domein toevoegen     | Azure AD Join | Persoonlijke apparaat |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|---------------|-----------------|
| Windows apparaat inloggen voor werk of school.                                                                                                                      | Ja             | Ja           | Nee              |
| Gebruiker eenmalige aanmelding (SSO) Office 365 en Azure AD apps. Eenmalige aanmelding is de mogelijkheid slechts eenmaal inloggen organisatie-bronnen. | Ja             | Ja           | Ja             |
| Eenmalige aanmelding gebruiker met Kerberos/NTLM-apps.                                                                                                                                  | Ja             | Beperkt       | Ja, via VPN         |
| Sterke verificatie en gemakkelijk aanmelden voor werk of school met Microsoft Passport en Windows Hello.                                                                   | Ja             | Ja           | Ja             |
| De toegang tot de Windows Store onderneming met een werk of school-account (dus niet door een Microsoft-account).                                                                                    | Ja             | Ja           | Ja             |
| Enterprise-compatibele zwervende gebruiker instellingen via werk of school-apparaten.                                                                                 | Ja             | Ja           | Ja             |
| De mogelijkheid toegang te beperken tot de organisatie-apps voor apparaten die compatibel met het apparaatbeleid voor organisatie zijn.                                                      | Ja             | Ja           | Ja             |
| Gebruiker Self-service inrichten van apparaten voor "werk overal."                                                                                                | Nee              | Ja           | Ja             |
| De mogelijkheid om apparaten te beheren.                                                                                                                                       | Ja, via GP/SCCM | Ja           | Ja             |

## <a name="use-work-owned-devices-with-azure-ad-join-and-domain-join-in-windows-10"></a>Gebruik werk eigendom apparaten met Azure AD Join en domein toevoegen in Windows 10

Windows 10 biedt voor apparaten toegang krijgen tot bronnen werk werk eigendom op twee manieren:

- Azure AD Join
- Aan domein toevoegen

 Beide kunnen geldige opties zijn afhankelijk van de wensen en behoeften van een organisatie zijn. In sommige gevallen kunnen organisaties profiteren van inschakelen van beide methoden voor de implementatie.

## <a name="when-to-use-azure-active-directory-join"></a>Wanneer gebruikt u Azure Active Directory toevoegen

Azure AD Join is een nieuw werk zelf inrichten ervaring in Windows 10.  Het is gericht op werknemers die toegang hebben tot werkresources zoals Office 365, voornamelijk in de cloud. Het is een lichtgewicht manier voor het configureren van computers, tablets en telefoons voor de onderneming. Apparaten worden beheerd via een mobiel Apparaatbeheer, met consistente besturingselementen voor Windows-platforms.

**Gebruik Azure AD lid worden van een van de volgende redenen**:

- Wilt u de self-service-aanbod van apparaten voor werknemers die onderweg inschakelen.
- U geeft gebruikers werk eigendom mobiele apparaten zoals tablets en telefoons.
- Wilt u een groep gebruikers in Azure AD in plaats van in Active Directory beheren zoals seizoenarbeiders, aannemers of studenten.
- U wilt bieden mogelijkheden worden beperkt op lokalen, infrastructuur voor werknemers in externe filialen.
- U beschikt niet over een Active Directory op gebouwen.

In sommige organisaties wordt Azure AD Join gebruiken als de primaire manier implementeren apparaten werken eigendom, vooral als ze de meeste of alle van hun bronnen naar de cloud migreren. Hybride organisaties met Active Directory en Azure AD kunnen ook kiezen voor de implementatie van een methode of een andere afhankelijk van de gebruiker of de afdeling.

Scholen en universiteiten, bijvoorbeeld kunnen beheren in Active Directory-medewerkers en studenten in Azure AD. Sommige bedrijven kunt beheren van externe kantoren of verkoopafdelingen in Azure AD. Zowel AD-Join Azure en domein join methoden kunnen worden gebruikt binnen een organisatie hybride. Azure AD Join is een geweldige aanvulling op aan domein toevoegen voor de implementatie van apparaten in een werkomgeving.

**Als de meest gebruikelijke toegang tot bedrijfsbronnen die via de cloud, is uw organisatie kan profiteren van extra voordelen als**:

- U kunt afhankelijkheden voor bedrijfsruimten identiteit infrastructuur verwijderen.
- U kunt uw apparaten deployment-model ophalen van imaging-oplossingen door zelf configuratie vereenvoudigen.
- U kunt mobiele Apparaatbeheer beheren van al uw apparaten op verschillende platforms.

Zie voor meer informatie over AD lid worden van Azure [cloud-mogelijkheden uitbreiden naar Windows 10-apparaten via Azure Active Directory deelnemen aan](active-directory-azureadjoin-overview.md).

## <a name="when-to-use-domain-join-or-keep-using-it"></a>Wanneer gebruikt u domein join (of houd deze)

Voor de laatste 15 jaar hebben veel organisaties aan domein toevoegen gebruikt om apparaten werken. Hiermee kunnen gebruikers zich aanmelden bij hun apparaten met hun Active Directory-werk of school accounts. Aan domein toevoegen kan ook IT centraal en volledig deze apparaten te beheren. Organisaties zijn meestal afhankelijk van imaging methoden voor bepaling apparaten en vaak gebruik van System Center Configuration Manager (SCCM) of Group Policy (GP) te beheren.

**Uw onderneming moet domein join (of houd deze) gebruiken om deze redenen**:

- U hebt geïmplementeerd op deze apparaten die gebruikmaken van NTLM of Kerberos Win32-toepassingen.
- U moet de GP of SCCM/DCM om apparaten te beheren.
- Wilt u doorgaan met het imaging-oplossingen gebruiken om apparaten te configureren voor uw werknemers.

**Aan domein toevoegen in Windows 10 kunt u ook de volgende voordelen wanneer verbonden met Azure AD**:

- Sterke verificatie van apparaat-gebonden en gemakkelijk aanmelden voor werk of school met Microsoft Passport en Windows Hello.
- Toegang tot de onderneming Windows Store voor apparaten die gebruikmaken van werk of school accounts (geen Microsoft-account vereist).
- Enterprise-compatibele zwervende gebruiker instellingen via apparaten die gebruikmaken van werk of school accounts (geen Microsoft-account vereist).
- De mogelijkheid toegang te beperken tot de organisatie-apps voor apparaten die compatibel met het apparaatbeleid voor organisatie zijn.

Zie voor meer informatie over AD lid worden van Azure [cloud-mogelijkheden uitbreiden naar Windows 10-apparaten via Azure Active Directory deelnemen aan](active-directory-azureadjoin-overview.md).

## <a name="enable-joining-of-personally-owned-devices-for-work-or-school"></a>Lid worden van apparaten voor werk- of persoonlijk eigendom inschakelen

Windows 10 geeft de gebruiker de mogelijkheid een werk of school-account toevoegen aan hun computer, tablet of telefoon voor BYOD in de onderneming. Nadat de gebruiker een werk of school-account toevoegt, wordt het apparaat met Azure Active Directory is geregistreerd en eventueel ingeschreven in het mobiele apparaat management systeem dat de organisatie is geconfigureerd. De map wordt deze apparaten weergegeven als 'Geregistreerd' versus 'Azure AD verbonden'. IT-administraors kan verschillende soorten beleid op basis van deze informatie, met een tintje lichter over apparaten dan persoonlijk eigendom op apparaten die eigendom zijn van werk desgewenst kunt toepassen.

Gebruikers kunnen toevoegen van een werk of school rekening met hun persoonlijke apparaat zeer eenvoudig. Ze kunnen bij de toegang tot de werktoepassing van een voor de eerste keer, of kunnen ze dit handmatig doen via het menu instellingen. Deze account biedt eenmalige aanmelding met organisatie-bronnen.

Zie [verbinding maken met een domein behoren apparaten Azure AD voor Windows 10 ervaringen](active-directory-azureadjoin-devices-group-policy.md)voor meer informatie over Azure AD deelnemen.

## <a name="enable-domain-join-or-azure-ad-join"></a>Aan domein toevoegen of Azure AD Join inschakelen

Alle methoden die eerder zijn beschreven (aan domein toevoegen en Azure AD Join toevoegen werk of school account) hebben de ingangspunten in de gebruikerservaring van Windows 10. Alle vereisen echter een IT-beheerder voor het inschakelen van de functionaliteit in de infrastructuur voordat de ervaring werkt.

## <a name="requirements-for-deploying-azure-ad-join"></a>Vereisten voor het implementeren van Azure AD Join

Als u wilt deelnemen aan Azure AD implementeren voor alle gebruikers hebt u het volgende nodig:

- Een abonnement op Azure AD.
- Een Azure AD Premium-abonnement, zoals een mobiel apparaat beheer van automatische inschrijving, als u meer functies nodig hebben.
- Mobiel Apparaatbeheer--bijvoorbeeld een abonnement op Microsoft Intune, mobiel Apparaatbeheer voor Office 365, of met de partner mobiel apparaat beheer leveranciers met AD Azure integreren. (Zie de [sectie Veelgestelde vragen](#frequently-asked-questions) in de buurt van het einde van dit artikel voor meer informatie).

Als uw faciliteiten hybride, raden we het implementeren van Azure AD verbinden om de map op het bedrijf uitbreiden naar Azure AD.

## <a name="requirements-for-using-domain-join-with-azure-ad"></a>Vereisten voor het gebruik aan domein toevoegen met Azure Active Directory

Aan domein toevoegen blijft werken zoals altijd heeft. Echter, als u de voordelen van Azure AD moet u het volgende:

- Een abonnement op Azure AD.
- Een implementatie van Azure AD verbinding maken met de map op het bedrijf uitbreiden naar Azure AD.
- Een beleid waarmee apparaten Azure AD voorwaardelijke toegang krijgen tot een domein behoren.
- Een beleid waarmee toegang tot apparaten 'domein ' toegevoegd als u de toegang voor sommige apparaten te beperken.
- System Center Configuration Manager versie 1509 voor technische Preview, regels voor het vereisen van compatibele apparaten inschakelen. (Zie het TechNet-documentatie en boeken blog).

Zie voor meer informatie over aan domein toevoegen in Windows 10, [verbinding maken met een domein behoren apparaten Azure AD voor Windows 10 ervaringen](active-directory-azureadjoin-devices-group-policy.md)

## <a name="requirements-for-using-byod-and-add-a-work-or-school-account"></a>Vereisten voor het gebruik van BYOD en "Een werk of school toevoegen"

'Uw eigen apparaat brengen' inschakelen (BYOD) met werk of school-accounts, moet u het volgende:

- Een abonnement op Azure AD.
- Een Azure AD Premium-abonnement, zoals een mobiel apparaat beheer van automatische inschrijving, als u meer functies nodig hebben.

## <a name="requirements-for-using-microsoft-passport"></a>Vereisten voor het gebruik van Microsoft Passport

Als u wilt dat Microsoft Passport, moet u het volgende:

- Een openbare-sleutelinfrastructuur (PKI) voor ondersteuning van verificatie op basis van certificaten die gebruikmaakt van Microsoft Passport.
- Een abonnement op Intune voor ondersteuning van verificatie op basis van certificaten die gebruikmaakt van Microsoft Passport voor Azure AD deelnemen aan en accounts voor werk of school.
- System Center Configuration Manager versie 1509 voor technische Preview (Zie het TechNet-documentatie en boeken blog) voor ondersteuning van verificatie op basis van certificaten die gebruikmaakt van Microsoft Passport voor domeinlidmaatschap.
- Een beleid voor het inschakelen van Microsoft Passport in de organisatie.

Als alternatief voor het gebruik van een PKI kunt u op basis van sleutel Microsoft Passport als volgt inschakelen:

- Implementatie van Windows Server 2016 'Productie voorbeeld 1' DCs (geen noodzaak voor een domein of forest-functionaliteitsniveaus; diverse DCs redundantie halen die elk Active Directory-site is voldoende).
- Microsoft Passport is ingeschakeld in de organisatie instellen.

Zie < link-to-MS-Passport-and-Windows-Hello-document > voor meer informatie over Passport van Microsoft en Windows Hello in Windows 10.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen
### <a name="which-partner-mobile-device-management-products-integrate-with-azure-ad"></a>Welke partner mobile device management producten integreren met AD Azure?

De volgende producten van de leverancier worden geïntegreerd met Azure AD voor centrale inschrijving en voorwaardelijke toegang in Windows 10:

- AirWatch door VMware
- Citrix Xenmobile
- Lightspeed Mobile Manager
- SOTI op locatie mobiele Apparaatbeheer
- MobileIron

### <a name="what-about-workplace-join-in-windows-10"></a>Hoe zit het met werkplek lid worden van Windows 10?
Werkplek Join is gebruikt in Windows 8.1 voor BYOD. BYOD is ingeschakeld via "Toevoegen van een werk of school account" in Windows 10, zoals eerder in dit document wordt uitgelegd. Voor organisaties die niet in hun mobiele Apparaatbeheer Azure AD integreren, gebruikers kunnen inschrijven voor het apparaat in het beheer via **Instellingen**handmatig > **rekeningen** > **toegang tot werk**.


### <a name="can-users-connect-their-microsoft-account-to-their-domain-account-in-windows-10"></a>Kunnen gebruikers hun Microsoft-account verbinding met hun domeinaccount in Windows 10?
Niet in Windows 10. In Windows 8.1, kunnen gebruikers van een domein behoren apparaten 'verbinden' hun Microsoft-account (bijvoorbeeld Hotmail, Live, Outlook, Xbox, etc.) bij hun domeinaccount waarmee bepaalde ervaringen als eenmalige aanmelding voor Live-services, het gebruik van de Windows Store en zwervende gebruiker instellingen tussen apparaten. In Windows 10, is het Microsoft-account 'verbinden'-functionaliteit teruggetrokken. De gebruiker kan een of meer Microsoft-accounts toevoegen als extra accounts eenmalige aanmelding inschakelen voor consumenten services zoals de Windows Store. Dit gebeurt in de **Instellingen** > **rekeningen** > **uw account**.

Gebruikers die een upgrade van Windows 8.1-apparaten domein behoren en die hun Microsoft-account was aangesloten, automatisch hun verbonden Microsoft account toegevoegd aan de lijst met extra accounts die ze hebben.


## <a name="additional-information"></a>Als u meer informatie
* [Windows 10 voor de onderneming: apparaten gebruik voor werk](active-directory-azureadjoin-windows10-devices-overview.md)
* [Cloud-mogelijkheden aan Windows 10-apparaten via Azure Active Directory deelnemen aan uitbreiden](active-directory-azureadjoin-user-upgrade.md)
* [Meer informatie over gebruiksscenario's voor Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Domein behoren apparaten aansluiten op Azure AD voor Windows 10 ervaringen](active-directory-azureadjoin-devices-group-policy.md)
* [Azure AD Join instellen](active-directory-azureadjoin-setup.md)
