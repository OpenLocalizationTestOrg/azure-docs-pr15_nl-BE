<properties
    pageTitle="Azure Active Directory hybride identiteit ontwerpoverwegingen - strategie voor gegevensbescherming definiëren | Microsoft Azure"
    description="U zult uw hybride identiteit oplossing om te voldoen aan de eisen die u hebt gedefinieerd voor de strategie voor gegevensbescherming definiëren."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="define-data-protection-strategy-for-your-hybrid-identity-solution"></a>Strategie voor gegevensbescherming voor uw oplossing hybride identiteit definiëren

In deze taak, zult u uw hybride identiteit oplossing om te voldoen aan de eisen die u hebt gedefinieerd in de strategie voor gegevensbescherming te definiëren:

- [Bepalen van vereisten voor de bescherming van gegevens](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)
- [Vereisten voor beheer van webinhoud bepalen](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)
- [Voorschriften inzake de controle van de toegang bepalen](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)
- [Beveiligingsplan vereisten bepalen](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="define-data-protection-options"></a>Opties voor beveiliging van gegevens definiëren
Zoals wordt uitgelegd in de [vereisten voor bepalen directory synchronisatie](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md), bevindt Microsoft Azure AD kunt synchroniseren met uw Active Directory Domain Services (AD DS) op gebouwen. Dankzij deze integratie kan organisaties profiteren van Azure AD om te controleren of de referenties van de gebruiker wanneer deze probeert toegang te krijgen tot bedrijfsnetwerken. Dit is mogelijk voor beide scenario's: gegevens op de rest van gebouwen en in de cloud.  Toegang tot gegevens in Azure AD vereist gebruikersverificatie via een beveiligingstokenservice (STS).

Zodra u geverifieerd, wordt de UPN (user Principal name) van de verificatietoken en de gerepliceerde partitie lezen en container overeenkomt met het domein van de gebruiker wordt bepaald. Informatie over de aanwezigheid, status ingeschakeld en rol van de gebruiker wordt gebruikt door het systeem van de vergunning om te bepalen of de gevraagde toegang tot de huurder doel in deze sessie van deze gebruiker is gemachtigd. Bepaalde geautoriseerde acties (met name maken gebruiker, wachtwoord opnieuw instellen) een controlepad die door een beheerder van een huurder kan worden gebruikt voor het beheren van conformiteit inspanningen of onderzoeken.

Verplaatsen van de gegevens van uw datacenter op gebouwen in Azure opslag via een internetverbinding altijd mogelijk niet haalbaar door gegevensvolume, bandbreedte, beschikbaarheid of andere overwegingen. De [Azure opslag Import/Export Service](../storage/storage-import-export-service.md) biedt een hardware-optie voor het brengen/ophalen van grote hoeveelheden gegevens in de blobopslag. U kunt [BitLocker gecodeerde](https://technet.microsoft.com/library/dn306081#BKMK_BL2012R2) harde schijven rechtstreeks naar een Azure datacenter waar operators cloud de inhoud aan uw account voor opslag uploadt of Azure gegevens te downloaden naar uw stations om terug te keren naar u verzenden. Alleen versleutelde schijven worden geaccepteerd voor dit proces (met een BitLocker-sleutel die door de service zelf tijdens de installatie van de taak). De sleutel van BitLocker wordt geleverd met Azure afzonderlijk, aldus van belangrijke delen van band.

Omdat gegevens tijdens de overdracht in verschillende scenario's plaatsvinden kan, is ook relevant zijn om te weten dat Microsoft Azure [virtueel netwerk](https://azure.microsoft.com/documentation/services/virtual-network/) huurders om gegevensverkeer te isoleren van elkaar, die gebruikmaakt van maatregelen zoals firewalls host en Gast-niveau, IP-pakketfiltering poortblokkering en eindpunten HTTPS gebruikt. Echter worden de meeste Azure van interne communicatie, met inbegrip van infrastructuur op infrastructuur en infrastructuur klant (op-locatie), ook gecodeerd. Een andere belangrijke scenario is de communicatie binnen Azure datacenters; Microsoft beheert netwerken om te verzekeren dat er geen VM kunt imiteren of van het IP-adres van een andere afluisteren. TLS/SSL wordt gebruikt bij het benaderen van Azure opslag of SQL-Databases, of wanneer een verbinding met de Cloud Services. In dit geval is de beheerder van de klant verantwoordelijk voor het verkrijgen van een TLS/SSL-certificaat en de implementatie daarvan aan de infrastructuur van de huurder. Verkeer verplaatsen van gegevens tussen virtuele Machines in de dezelfde installatie of huurders in een implementatie via Microsoft Azure virtueel netwerk kan worden beveiligd via gecodeerde communicatieprotocollen zoals HTTPS, SSL/TLS of anderen.

Afhankelijk van hoe u beantwoord de vragen in de [vereisten voor bescherming van de gegevens bepalen](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md), moet u bepalen hoe u wilt uw gegevens te beschermen en hoe de identiteit hybride oplossing zal u helpen op die. De tabel bevat de opties worden ondersteund door Azure die beschikbaar voor elk scenario van de bescherming van gegevens zijn.


| Opties voor de beveiliging         | In de rust in de cloud | Op de rest van gebouwen | In transit |
|---------------------------------|----------------------|---------------------|------------|
| BitLocker-stationsversleuteling      | X                    | X                   |            |
| SQL Server databases coderen | X                    | X                   |            |
| VM-VM-codering             |                      |                     | X          |
| SSL/TLS                         |                      |                     | X          |
| VPN                             |                      |                     | X          |


>[AZURE.NOTE]
Lees de [naleving door de functie](https://azure.microsoft.com/support/trust-center/services/) in [Microsoft Azure Vertrouwenscentrum](https://azure.microsoft.com/support/trust-center/) voor meer informatie over de certificaten die compatibel is met elke Azure service.
Omdat de opties voor gegevensbescherming gebruiken een gelaagde benadering, vergelijking van deze opties zijn niet van toepassing voor deze taak. Zorg ervoor dat u gebruik van alle beschikbare opties voor elke staat die de gegevens.

## <a name="define-content-management-options"></a>Opties voor beheer van webinhoud bepalen
Een voordeel van een hybride identiteit infrastructuur beheren met Azure AD is dat het proces volledig transparant vanuit het oogpunt van de eindgebruiker is. De gebruiker probeert toegang te krijgen tot een gedeelde bron, verificatie vereist is voor de resource, moet de gebruiker een verificatie verzendt naar Azure AD de token verkrijgen en toegang tot de bron. Dit hele proces gebeurt op de achtergrond, zonder tussenkomst van de gebruiker. Het is ook mogelijk om machtiging te verlenen aan een [groep](active-directory-manage-groups.md#getting-started-with-access-management) gebruikers zodat zij bepaalde gemeenschappelijke acties uit te voeren.

Organisaties die bezorgdheid over de gegevensprivacy meestal zijn vereisen classificatie van gegevens voor de oplossing. Als hun huidige op gebouwen infrastructuur al data classificatie wordt gebruikt, is het mogelijk om te profiteren van Azure AD als belangrijkste opslagplaats voor de identiteit van de gebruiker. Een veelgebruikte hulpprogramma dat gebruikt in ruimten is voor de indeling van de gegevens [Data classificatie Toolkit](https://msdn.microsoft.com/library/Hh204743.aspx) voor Windows Server 2012 R2 wordt genoemd. Dit hulpprogramma kunt te identificeren, te classificeren en beveiligen van gegevens op bestandsservers in uw persoonlijke cloud. Het is ook mogelijk om de [Automatische indeling van het bestand](https://technet.microsoft.com/library/hh831672.aspx) in Windows Server 2012 om dit te bereiken.

Als uw organisatie geen indeling van de gegevens in de plaats, maar moet de bescherming van gevoelige bestanden zonder Servers op-nieuwbouw toe te voegen, kan Microsoft [Azure Rights Management Service](https://technet.microsoft.com/library/JJ585026.aspx)gebruikt.  Azure RMS-codering, identiteit en autorisatiebeleid wordt gebruikt voor de beveiliging van uw bestanden en e-mailadres en het werkt via meerdere apparaten, telefoons, tablets en pc's. Omdat RMS Azure een cloud-service is, behoeft niet expliciet vertrouwensrelaties met andere organisaties configureren voordat u beveiligde inhoud met hen delen kunt. Als ze al een Office 365- of een directory AD Azure, wordt automatisch de samenwerking tussen verschillende organisaties ondersteund. U kunt ook alleen de directorykenmerken die nodig zijn ter ondersteuning van een gemeenschappelijke identiteit voor uw Active Directory-accounts op de gebouwen, met Azure Active Directory Synchronization Services (AAD synchronisatie) of Azure verbinden met AD RMS Azure synchroniseren.

Een essentieel onderdeel van het beheer van de inhoud is om te weten wie toegang heeft tot welke resource, een mogelijkheid voor uitgebreide logboekregistratie is daarom van belang voor de oplossing voor identiteitsbeheer. Azure AD vindt u meer dan 30 dagen, met inbegrip van logboek:

- Wijzigingen in het lidmaatschap van de rol (ex: gebruiker toegevoegd aan de rol Admin globale)
- Referentie-updates (ex: wijzigen van wachtwoorden)
- Domain management (ex: een aangepaste domein, verwijdering van een domein controleren)
- Toevoegen of verwijderen van toepassingen
- Gebruikersbeheer (ex: toevoegen, verwijderen, bijwerken van een gebruiker)
- Toevoegen of verwijderen van licenties

>[AZURE.NOTE]
Lees [Microsoft Azure Security and Audit Log-beheer](http://download.microsoft.com/download/B/6/C/B6C0A98B-D34A-417C-826E-3EA28CDFC9DD/AzureSecurityandAuditLogManagement_11132014.pdf) meer weten over de mogelijkheden voor logboekregistratie in Azure.
Afhankelijk van hoe u beantwoord de vragen in de [vereisten voor beheer van webinhoud bepalen](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md), moet u bepalen hoe u wilt dat de inhoud moet worden beheerd in uw identiteit hybride oplossing. Alle opties die beschikbaar zijn in tabel 6 zijn kan worden geïntegreerd met AD Azure, is het belangrijk om te definiëren die meer geschikt is voor uw zakelijke behoeften.

| Opties voor beheer van webinhoud                                                               | Voordelen                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Nadelen                                                                                                                                                                                                                               |
|------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Gecentraliseerde on-premises (Active Directory Rights Management Services)                      | Volledige controle over de infrastructuur van de server die verantwoordelijk is voor de indeling van de gegevens <br> Ingebouwde functie in Windows Server, hoeft u geen extra licentie of abonnement <br> Kan worden geïntegreerd met Azure AD in een hybride scenario <br> Ondersteunt informatie rights management (IRM) mogelijkheden in Microsoft Online services, zoals Exchange Online en SharePoint Online, evenals Office 365 <br> Over lokalen serverproducten van Microsoft, zoals Exchange Server en SharePoint Server bestandsservers waarop Windows Server en bestand classificatie infrastructuur (FCI) ondersteunt. | Hogere, onderhoud (gelijke tred houden met updates, de configuratie en de mogelijke upgrades), sinds IT eigenaar is van de Server <br> Vereist een serverinfrastructuur van gebouwen<br> Doesn'tleverage mogelijkheden van Azure native                                     |
| Centraal in de cloud (Azure RMS)                                                     | Gemakkelijker te beheren in vergelijking met de oplossing van gebouwen <br> Kan worden geïntegreerd met AD DS in een hybride scenario <br>  Volledig geïntegreerd met AD Azure <br> Een server in-premises implementeren de service nodig niet <br> Ondersteunt op-bedrijfsruimten Microsoft serverproducten zoals Exchange Server, SharePoint-Server en bestandsservers waarop Windows Server en bestand indeling, infrastructuur (FCI) <br> IT, kan volledige controle over de sleutel van de huurder met mogelijkheid voor BYOK hebben.                                                                                    | Uw organisatie hebt u een abonnement wolk die door RMS worden ondersteund <br> Uw organisatie moet een Azure AD-map voor de verificatie van de gebruiker ondersteuning voor RMS                                                                                  |
| Hybride (Azure RMS met geïntegreerd, On-Premises Active Directory Rights Management Services) | In dit scenario worden de voordelen van beide centrale over-ruimten en in de cloud bij elkaar opgeteld.                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Uw organisatie hebt u een abonnement wolk die door RMS worden ondersteund <br> Uw organisatie moet een Azure AD-map voor de verificatie van de gebruiker ondersteuning voor RMS, hebben <br> Vereist een verbinding tussen de Azure cloud en infrastructuur ruimten |

## <a name="define-access-control-options"></a>Opties voor toegangsbeheer definiëren
Gebruik te maken van de verificatie, autorisatie en toegang bepalen mogelijkheden beschikbaar in Azure AD is het mogelijk om uw bedrijf gebruik van een identiteit centrale opslagplaats terwijl gebruikers en partners voor eenmalige aanmelding (SSO) zoals aangegeven in de onderstaande afbeelding:

![](./media/hybrid-id-design-considerations/centralized-management.png)

Gecentraliseerd beheer en volledige integratie met andere mappen

Azure Active Directory biedt eenmalige aanmelding tot duizenden toepassingen SaaS en webtoepassingen op gebouwen. Lees de [lijst met compatibele Azure Active Directory federation: identiteitsproviders van derden die kunnen worden gebruikt voor het implementeren van eenmalige aanmelding](https://msdn.microsoft.com/library/azure/jj679342.aspx) artikel voor meer informatie over de SSO-derde die door Microsoft zijn getest. Hierdoor kan een organisatie een groot aantal scenario's B2B behoud de controle over het beheer van identiteits- en toegangsbeheer implementeren. Tijdens het B2B is proces ontwerpen echter belangrijk dat u begrijpt de verificatiemethode die wordt gebruikt door de partner en als deze methode wordt ondersteund door Azure valideren. Momenteel zijn methoden die worden ondersteund door AD Azure:

- Security Assertion Markup Language (SAML)
- OAuth
- Kerberos
- Tokens
- Certificaten


>[AZURE.NOTE] [Azure Active Directory-verificatieprotocollen](https://msdn.microsoft.com/library/azure/dn151124.aspx) voor meer gedetailleerde informatie over elk protocol en de mogelijkheden ervan in Azure lezen.

Met de ondersteuning van Azure AD, mobiele bedrijfstoepassingen ervaring dezelfde eenvoudige mobiele diensten verificatie werknemers aan te melden in hun mobiele toepassingen met hun corporate Active Directory-referenties mogen gebruiken. Met deze functie wordt Azure AD ondersteund als een identiteitsprovider in Mobile Services samen met de andere identiteit die we al ondersteunen (zoals Microsoft Accounts, Facebook-ID ID Google en Twitter-ID). Als de apps in de ruimten van de gebruiker referenties vinden op het adres van het bedrijf AD DS worden gebruikt, moet de toegang van partners en gebruikers die afkomstig zijn uit de cloud transparant zijn. U kunt beheren van voorwaardelijke beheer van gebruikerstoegang voor webtoepassingen (cloud-gebaseerde), web API Microsoft cloud services, 3de partij SaaS-toepassingen en native (mobiele) client-toepassingen, en de voordelen van beveiliging, controle, rapportage op één plaats. Het is echter raadzaam voor het valideren van dit in een niet-productieomgeving of met een beperkt aantal gebruikers.


>[AZURE.TIP] het is belangrijk te vermelden dat Azure AD geen groepsbeleid als AD DS. Om het afdwingen van beleid voor apparaten moet u een oplossing voor mobiele apparaten zoals [Microsoft Intune](https://technet.microsoft.com/library/jj676587.aspx).

Als de gebruiker is geverifieerd met AD Azure, is het belangrijk om te evalueren van het toegangsniveau dat de gebruiker heeft het. Het niveau van de toegang van de gebruiker wordt via een bron kan variëren, Azure AD kunt u een extra beveiligingslaag toevoegen door de toegang tot bronnen te beheren, moet u ook houden er rekening mee dat het middel zelf ook een eigen toegangslijst afzonderlijk, zoals het toegangsbeheer voor bestanden op een bestandsserver hebben kan. In de volgende afbeelding geeft een overzicht van de niveaus van toegangsbeheer dat u in een scenario met hybride kan hebben:

![](./media/hybrid-id-design-considerations/accesscontrol.png)


Elke interactie in het diagram weergegeven in figuur X vertegenwoordigt één access control scenario die kan worden gedekt door AD Azure. U hebt hieronder een beschrijving van elk scenario:

1. voorwaardelijke toegang tot toepassingen die worden gehost op ruimten: kunt u geregistreerde apparaten met-beleidsregels voor toepassingen die zijn geconfigureerd voor het gebruik van AD FS met Windows Server 2012 R2. Zie voor meer informatie over het instellen van voorwaardelijke toegang op ruimten [instellen van On-premises voorwaardelijke toegang met behulp van Azure Active Directory de registratie](active-directory-conditional-access-on-premises-setup.md).
2. toegangsbeheer Azure Management Portal: Azure heeft ook de mogelijkheid om te bepalen of de toegang tot de Portal Management via RBAC (rollen gebaseerd toegangsbeheer). Deze methode kunt het bedrijf voor het beperken van de hoeveelheid bewerkingen die een persoon doen kan als hij toegang tot Azure Management Portal heeft. Via RBAC toegang tot de portal ca IT-beheerders toegang delegeren met behulp van de volgende benaderingen voor toegang beheren:

 - Op basis van een groep roltoewijzing: U kunt toegang toewijzen aan Azure AD groepen die kunnen worden gesynchroniseerd van uw lokale Active Directory. Hierdoor kunt u gebruikmaken van de bestaande investeringen die uw organisatie heeft gemaakt in gereedschappen en processen voor het beheren van groepen. U kunt ook de functie van AD-premie Azure overgedragen groep.
 - Ingebouwde functies in Azure hefboomwerking: kunt u drie rollen, eigenaar Inzender en Reader, om ervoor te zorgen dat gebruikers en groepen gemachtigd zijn om alleen de taken die ze nodig hebben om hun werk te doen.
 - Gedetailleerde toegang tot bronnen: U kunt rollen toewijzen aan gebruikers en groepen voor een bepaald abonnement, resourcegroep of een afzonderlijke Azure resource zoals een website of een database. Op deze manier kunt u ervoor zorgen dat gebruikers hebben toegang tot alle bronnen die ze nodig hebben en geen toegang tot bronnen die ze niet hoeven te beheren.

>[AZURE.NOTE] [toegangsbeheer op basis van rollen in Azure](https://azure.microsoft.com/updates/role-based-access-control-in-azure-preview-portal/) als u wilt weten meer informatie over deze mogelijkheid gelezen. Ontwikkelaars die toepassingen bouwt en u wilt het access-besturingselement aanpassen, is het ook Azure AD toepassingsrollen gebruiken voor verificatie. Bekijk deze [WebApp-RoleClaims-DotNet voorbeeld](https://github.com/AzureADSamples/WebApp-RoleClaims-DotNet) over het bouwen van uw app voor het gebruik van deze mogelijkheid.

3. voorwaardelijke toegang voor Office 365-toepassingen met Microsoft Intune: IT-beheerders apparaatbeleid voorwaardelijke toegang wilt beveiligen, bedrijfsnetwerken, terwijl tegelijkertijd waardoor de informatiewerkers op compatibele apparaten toegang krijgen tot de services kunt inrichten. Zie [Voorwaardelijke-apparaatbeleid voor Office 365-services](active-directory-conditional-access-device-policies.md)voor meer informatie.

4. voorwaardelijke toegang voor Saas-toepassingen: [met deze functie](http://blogs.technet.com/b/ad/archive/2015/06/25/azure-ad-conditional-access-preview-update-more-apps-and-blocking-access-for-users-not-at-work.aspx) kunt u toegangsregels per toepassing meerledige verificatie en de mogelijkheid voor het blokkeren van toegang voor gebruikers die niet in een vertrouwd netwerk configureren. U kunt de meerledige verificatie-regels toepassen voor alle gebruikers die zijn toegewezen aan de toepassing, of alleen voor gebruikers binnen de opgegeven beveiligingsgroepen. Gebruikers kunnen worden uitgesloten van de verplichting meerledige verificatie als ze toegang hebben tot de toepassing van een IP-adres dat binnen de organisatie van het netwerk.

Omdat de opties voor toegangsbeheer gebruiken voor een gelaagde benadering, vergelijking van deze opties zijn niet van toepassing voor deze taak. Zorg ervoor dat u gebruik van alle opties die beschikbaar zijn voor elk scenario die vereist dat u toegang tot bronnen te beheren.

## <a name="define-incident-response-options"></a>Beveiligingsplan opties definiëren
Azure AD kan helpen IT identiteit van de potentiële veiligheidsrisico's in het milieu door het controleren van de activiteit van de gebruiker, IT can Azure AD toegang benutten en gebruiksrapporten mogelijkheid krijgen inzicht in de integriteit en beveiliging van de directory van uw organisatie. Met deze informatie kan een IT-beheerder beter bepalen waar mogelijke beveiligingsrisico's kunnen liggen, zodat zij adequaat plannen kunnen die risico's te beperken.  [Azure AD Premium-abonnement](active-directory-get-started-premium.md) heeft een reeks beveiligingsrapporten waardoor IT om deze informatie te verkrijgen. [Azure AD reports](active-directory-view-access-usage-reports.md) worden gecategoriseerd als volgt:

- **Afwijking rapporten**: aanmelden bij gebeurtenissen die we gevonden worden afwijkende bevatten. Ons doel is u op de hoogte van dergelijke activiteiten en u kunt bepalen of een gebeurtenis is verdacht.
- **Geïntegreerde toepassing rapport**: biedt inzicht in hoe cloud-toepassingen in uw organisatie worden gebruikt. Azure Active Directory biedt integratie met duizenden cloud-toepassingen.
- **Foutrapporten**: fouten die optreden kunnen tijdens het inrichten van accounts voor externe toepassingen geven.
- **Gebruiker-specifieke rapporten**: apparaat/teken in de activiteitsgegevens voor een specifieke gebruiker weergeven.
- **Logboeken voor faxactiviteit**: bevat een overzicht van alle gecontroleerde gebeurtenissen binnen de afgelopen 24 uur, laatste 7 dagen of laatste 30 dagen, alsmede wijzigingen in activiteiten en wachtwoord opnieuw instellen en de registratie van de activiteit.

>[AZURE.TIP]
Een ander rapport waarmee u kunt ook het Incident Response team werken aan een aanvraag is de [gebruiker met referenties uitgelekte](http://blogs.technet.com/b/ad/archive/2015/06/15/azure-active-directory-premium-reporting-now-detects-leaked-credentials.aspx) rapport.  Dit rapport oppervlakken eventuele overeenkomsten tussen deze lijst uitgelekte referenties en de huurder.

Andere belangrijke ingebouwde rapporten in Azure AD die kan worden gebruikt bij het onderzoek van een beveiligingsplan en zijn:

- **Activiteit opnieuw instellen van wachtwoord**: admin bieden inzicht in hoe actief wachtwoord opnieuw instellen in de organisatie wordt gebruikt.
- **Wachtwoordhersteldiskette-registratie activiteit**: biedt inzichten waarin gebruikers hun methoden voor het opnieuw instellen van wachtwoorden hebt geregistreerd en welke methoden zij hebben geselecteerd.
- **Groepsactiviteit**: biedt een overzicht van de wijzigingen aan de groep (ex: gebruikers toevoegen of verwijderen) die zijn aangegaan in het Access-venster.

Naast de core rapportage mogelijkheden beschikbaar in Azure AD premie die kunnen worden gebruikt tijdens een Incident Response onderzoek IT kan ook gebruikmaken van controleverslag om informatie te verkrijgen, zoals:

- Wijzigingen in het lidmaatschap van de rol (ex: gebruiker toegevoegd aan de rol Admin globale)
- Referentie-updates (ex: wijzigen van wachtwoorden)
- Domain management (ex: een aangepaste domein, verwijdering van een domein controleren)
- Toevoegen of verwijderen van toepassingen
- Gebruikersbeheer (ex: toevoegen, verwijderen, bijwerken van een gebruiker)
- Toevoegen of verwijderen van licenties

Omdat de opties voor het incident response gebruiken een gelaagde benadering, vergelijking van deze opties zijn niet van toepassing voor deze taak. Zorg ervoor dat u gebruik van alle opties die beschikbaar zijn voor elk scenario waarbij u Azure AD rapportage mogelijkheid gebruiken als onderdeel van de responsprocedure van uw bedrijf.

## <a name="next-steps"></a>Volgende stappen
[Hybride identiteit beheertaken bepalen](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)


## <a name="see-also"></a>Zie ook
[Overzicht van overwegingen bij het ontwerp](active-directory-hybrid-identity-design-considerations-overview.md)
