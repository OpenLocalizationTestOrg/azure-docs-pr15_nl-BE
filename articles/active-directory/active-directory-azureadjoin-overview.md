<properties
    pageTitle="Cloud-mogelijkheden aan Windows 10-apparaten via Azure Active Directory deelnemen aan uitbreiden | Microsoft Azure"
    description="Een gedetailleerd overzicht van hoe Windows 10-apparaten gebruikmaken van Azure AD lid worden geregistreerd in Azure Active Directory."
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
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="extending-cloud-capabilities-to-windows-10-devices-through-azure-active-directory-join"></a>Cloud-mogelijkheden aan Windows 10-apparaten via Azure Active Directory deelnemen aan uitbreiden

## <a name="what-is-azure-active-directory-join"></a>Wat is Azure Active Directory toevoegen?
Azure Active Directory Join (Join Azure AD) is de functionaliteit die in Azure Active Directory inschakelen gecentraliseerd beheer van het apparaat een apparaat voor het bedrijf eigendom wordt geregistreerd. Dit maakt het mogelijk voor gebruikers van de werknemers en leerlingen verbinding maken met de enterprise cloud via Azure Active Directory. Hiermee kunnen eenvoudig Windows-implementaties en toegang tot organisatie-apps en bronnen van elk Windows-apparaat, zowel corporate eigendom en persoonlijk eigendom (BYOD).

Azure AD Join is bedoeld voor ondernemingen die cloud-eerste/cloud alleen--zijn meestal kleine en middelgrote bedrijven die niet een Windows Server Active Directory-infrastructuur op gebouwen. Dat genoemde, Azure AD Join kan en zal ook worden gebruikt door grote organisaties op apparaten die niet van de traditionele domeinlidmaatschap (bijvoorbeeld mobiele apparaten) of voor gebruikers die voornamelijk nodig voor toegang tot Office 365 of andere Azure AD SaaS-apps.

Hoewel de traditionele domeinlidmaatschap nog steeds dat de beste biedt op-ruimten ervaring op apparaten die kunnen lid worden van domein, is Azure AD Join geschikt voor apparaten die niet aan domein toevoegen. Azure AD Join is ook geschikt voor gebruikers beheren in de cloud. Dit gebeurt met behulp van mobiele apparaat mogelijkheden in plaats van met behulp van traditionele domain management tools zoals Groepsbeleid en System Center Configuration Manager (SCCM).

![Overzicht van apparaten zakelijke en persoonlijke apparaten met Active Directory op gebouwen en Azure AD](./media/active-directory-azureadjoin/active-directory-azureadjoin-overview.png)


## <a name="why-should-enterprises-adopt-azure-ad-join"></a>Waarom ondernemingen vaststellen Azure AD Join?

* **Bedrijven die grotendeels in de cloud zijn**: als u hebt verplaatst of wilt verplaatsen naar een model waarin u vermindert de kooldioxide in ruimten en wilt werken in de cloud, Azure AD Join kan voordelen. Misschien hebt u Azure AD accounts handmatig of via de synchronisatie van uw Active Directory op locatie gemaakt. In beide gevallen hebt u een account in Azure AD en kunt u het aanmelden bij Windows 10. De gebruikers kunnen hun computers toevoegen aan Azure AD via ofwel de out-of-box experience (OOBE) of via het menu instellingen. Na lid te worden, kunnen profiteren van eenmalige aanmelding (SSO) toegang tot cloud resources zoals Office 365 in hun browser of in Office-toepassingen.

* **Educatieve instellingen**: een van de scenario's die we vaak gehoord is dat onderwijsinstellingen twee gebruikerstypen: docenten en studenten. Faculteitleden beschouwd als op de langere termijn van de leden van de organisatie, waardoor op gebouwen-accounts maken voor deze wenselijk is. Maar studenten korterlopende lid zijn van de organisatie en dus kunnen worden beheerd in Azure AD. Dit betekent dat directory schaal kan worden geduwd naar de cloud in plaats van opgeslagen op gebouwen. Het betekent ook dat studenten kunnen bij Windows met hun Azure AD-account aanmelden en toegang tot bronnen voor Office 365, in de browser of in Office-toepassingen krijgen.

* **Detailhandel bedrijven**: een ander scenario die we van klanten heb gehoord over hun behoefte aan seizoenarbeiders gemakkelijker te beheren is.  Nogmaals, rekeningen voor langere termijn, de voltijdse werknemers worden meestal gemaakt als accounts op domein behoren machines ruimten. Maar seizoenarbeiders korterlopende lid zijn van de organisatie, dus het is wenselijk te beheren waar gebruikerslicenties gemakkelijk rond kunnen worden verplaatst. Deze gebruikersaccounts maken in de cloud met Office 365 licenties kan de gebruikers de voordelen van het aanmelden bij Windows en Office-toepassingen met een Azure AD-account. Ondertussen onderhouden u meer flexibiliteit met hun licenties nadat ze verlaten.
* **Andere bedrijven**: zelfs als u gebruikers in Active Directory op gebouwen onderhouden, kunt u nog steeds profiteren van dat gebruikers Azure AD toegevoegd worden. Namelijk AD Azure biedt een vereenvoudigde gekoppelde ervaring, efficiënt beheer, automatische mobiel apparaat beheer inschrijving en single sign-on mogelijkheden voor Azure AD en resources ruimten.  

## <a name="what-capabilities-does-azure-ad-join-offer"></a>Welke mogelijkheden biedt Azure AD Join?
Met Azure AD Join krijgt u het volgende:

* **Zelf inrichten van ondernemingen die eigendom zijn van apparaten**: met Windows 10, kunnen gebruikers een geheel nieuwe, krimp-apparaat in de out-of-box-ervaring, zonder de rol van IT.


* **Ondersteuning voor moderne vormfactoren**: Azure AD Join werkt op apparaten die niet de traditionele domein deelnemen aan mogelijkheden.  


* **Ondersteuning voor bestaande organisatie-accounts**: gebruikers niet langer te maken en onderhouden van een persoonlijke Microsoft-account voor de beste ervaring op bedrijf uitgegeven apparaten, net zo als met Windows 8. Ze kunnen hun bestaande accounts voor werk in Azure AD in plaats daarvan gebruiken. In veel organisaties wordt betekent dit in principe dat gebruikers kunnen instellen en zich bij Windows aanmelden met dezelfde referenties die worden gebruikt voor toegang tot Office 365.


* **Automatische mobiel apparaat beheer inschrijving**: apparaten automatisch kunnen worden ingeschreven in een mobiel Apparaatbeheer wanneer verbonden met Azure AD. Dit proces werkt met Microsoft Intune en partner oplossingen voor mobiele apparaten. Als Apparaatbeheer wordt gedaan met Intune, kunnen IT-beheerders monitor/beheren Azure AD verbonden apparaten naast de apparaten in de beheerconsole SCCM domein behoren.


* **Eenmalige aanmelding bedrijf bronnen**: gebruikers genieten van eenmalige aanmelding van het Windows-bureaublad op apps en bronnen in de cloud, zoals Office 365 en duizenden bedrijfstoepassingen die afhankelijk van Azure AD voor verificatie via [Azure AD verbinden zijn](active-directory-azureadjoin-deployment-aadjoindirect.md). Zakelijk eigendom apparaten die lid zijn van AD Azure ook eenmalige aanmelding geniet van bronnen voor gebouwen wanneer het apparaat is in een bedrijfsnetwerk en overal wanneer deze bronnen beschikbaar worden gemaakt via de [Proxy voor Azure AD](https://msdn.microsoft.com/library/azure/Dn768219.aspx).


* **OS staat Roaming**: instellingen voor toegankelijkheid, websites, Wi-Fi-wachtwoorden en andere instellingen worden gesynchroniseerd via corporate eigendom-apparaten zonder een persoonlijke Microsoft-account.


* **Enterprise-ready Windows Store**: de Windows Store app aanschaf en licenties met Azure AD accounts ondersteunt. Organisaties kunnen een volumelicentie apps en beschikbaar maken voor gebruikers binnen hun organisatie.

## <a name="how-do-different-devices-work-with-azure-ad-join"></a>Hoe verschillende apparaten werkt met Azure AD Join?

| Corporate-apparaat (lid is van een domein voor ruimten)                                                                                                                                                                                         | Corporate-apparaat (lid is van de wolk)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Persoonlijke apparaat                                                                                                         |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| Gebruikers kunnen inloggen op Windows met werk referenties (zoals vandaag).                                                                                                                                                                        | Gebruikers zich aanmelden bij Windows met werk referenties die worden beheerd in Azure AD. Dit is relevant voor corporate apparaten in drie gevallen: 1) de organisatie heeft geen Active Directory in ruimten (bijvoorbeeld een klein bedrijf). 2) de organisatie niet alle gebruikersaccounts in Active Directory maken (bijvoorbeeld gebruikersaccounts voor studenten, consultants of seizoenarbeiders worden gemaakt in Active Directory). 3) de organisatie heeft corporate apparaten die niet kunnen worden toegevoegd aan een domein (op locatie), zoals telefoons of tablets met een Mobile-SKU (bijvoorbeeld een tweede apparaat genomen om een vloer fabriek/detailhandel). Azure AD Join ondersteunt het koppelen van corporate apparaten voor beheerde en federatieve organisaties. | Gebruikers aanmelden bij Windows met hun referenties persoonlijke Microsoft-account (geen wijzigingen).                                                |
| Gebruikers hebben toegang tot de zwervende instellingen en de onderneming Windows Store. Deze services werken met accounts werken en geen persoonlijke Microsoft-account nodig. Hiervoor moet Active Directory op ruimten verbinden naar Azure AD.                                        | Setup zelf u kunt doen. Ze kunnen gaan tot en met de eerste sessie (FRX) via hun werk rekening als alternatief voor de bepaling van IT met de apparaten, hoewel beide methoden worden ondersteund.                                                                                                                                                                                                                                                                                                                                                                             | Gebruikers kunnen gemakkelijk toevoegen van een werk-account die wordt beheerd in Active Directory of AD Azure.                                                      |
| Gebruikers hebben de SSO-mogelijkheid van het bureaublad te werken apps, websites en bronnen--waaronder bronnen op gebouwen en cloud-apps die Azure AD voor verificatie gebruiken.                                                                                                            | Apparaten worden automatisch geregistreerd in de enterprise-map (Azure AD) en mobiel Apparaatbeheer automatisch ingeschreven. (Azure AD Premium functie).                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Gebruikers hebben de mogelijkheid voor eenmalige aanmelding over apps en websites/resources met deze werkzaamheden rekening.                                              |
| Gebruikers kunnen hun persoonlijke Microsoft-account voor toegang tot hun eigen foto's en bestanden zonder invloed op Ondernemingsgegevens toevoegen. (Zwervende instellingen blijven werken met de rekeningen van hun werk.) Het Microsoft-account kunt eenmalige aanmelding en niet meer stations zwervende instellingen.  | U kunt doen zelf wachtwoord opnieuw instellen (SSPR) in winlogon, wat betekent dat ze een vergeten wachtwoord opnieuw kunnen instellen. (Azure AD Premium functie).                                                                                                                                                                                                                                                                                                                                                                                                                                   | Gebruikers hebben toegang tot de onderneming Windows Store, zodat ze kunnen verwerven en bedrijfs apps op hun persoonlijke apparaten gebruiken. |                                                               |


## <a name="additional-information"></a>Als u meer informatie
* [Windows 10 voor de onderneming: apparaten gebruik voor werk](active-directory-azureadjoin-windows10-devices-overview.md)
* [Cloud-mogelijkheden aan Windows 10-apparaten via Azure Active Directory deelnemen aan uitbreiden](active-directory-azureadjoin-user-upgrade.md)
* [Verificatie van identiteiten zonder wachtwoorden via Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Meer informatie over gebruiksscenario's voor Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Domein behoren apparaten aansluiten op Azure AD voor Windows 10 ervaringen](active-directory-azureadjoin-devices-group-policy.md)
* [Azure AD Join instellen](active-directory-azureadjoin-setup.md)
