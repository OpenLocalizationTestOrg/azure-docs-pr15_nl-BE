<properties
   pageTitle="Azure AD verbinden: Versie Release-geschiedenis | Microsoft Azure"
   description="In dit onderwerp worden alle releases van Azure AD verbinden en Azure AD Sync"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-version-release-history"></a>Azure AD verbinden: Versie Release-geschiedenis

Azure AD verbinden de Azure Active Directory-team regelmatig bijgewerkt met nieuwe functies en functionaliteit. Niet alle toevoegingen zijn van toepassing op alle doelgroepen.

Dit artikel is bedoeld, kunt u de versies die beschikbaar zijn en dat u begrijpt of die u wilt bijwerken naar de nieuwste versie of niet.

Dit is een lijst met verwante onderwerpen:

Onderwerp |  
--------- | --------- |
Stappen voor het bijwerken van Azure AD Connect | Methoden om een [upgrade van een vorige versie naar de nieuwste](active-directory-aadconnect-upgrade-previous-version.md) Azure AD verbinden release.
Vereiste machtigingen | Zie [accounts en machtigingen](./connect/active-directory-aadconnect-accounts-permissions.md#upgrade) voor de vereiste machtigingen voor een update,
Downloaden| [Azure AD downloaden verbinding](http://go.microsoft.com/fwlink/?LinkId=615771)

## <a name="112810"></a>1.1.281.0
Uitgebracht: 2016 augustus

**Vaste problemen:**

- Wijzigingen worden gesynchroniseerd interval vindt niet plaats pas nadat de volgende cyclus van de synchronisatie is voltooid.
- Azure AD verbinding maken met wizard accepteert geen Azure AD-account met gebruikersnaam met een onderstrepingsteken begint (\_).
- Azure AD verbinding maken met wizard niet verifiëren Azure AD-account die is gedefinieerd als het wachtwoord te veel speciale tekens bevat. Foutbericht 'kan geen referenties te valideren. Een onverwachte fout opgetreden." als resultaat gegeven.
- Staging-server verwijderen, schakelt Wachtwoordsynchronisatie in Azure AD huurder en Wachtwoordsynchronisatie met actieve server storing veroorzaakt.
- Wachtwoordsynchronisatie mislukt in weinig gevallen als er geen opgeslagen op de hash van het wachtwoord.
- Bij het verbinden van Azure AD server voor staging-modus is ingeschakeld, is niet tijdelijk wachtwoord writeback uitgeschakeld.
- Azure AD verbinding maken met wizard wordt niet weergegeven de werkelijke Wachtwoordsynchronisatie en de configuratie van wachtwoord-Write-back als de server in de staging-modus. Hierin zien altijd ze uitgeschakeld.
- Configuratiewijzigingen van Wachtwoordsynchronisatie en wachtwoord Write-back zijn niet blijvend door de wizard Azure AD verbinden wanneer server in de staging-modus.

**Verbeteringen:**

- Start ADSyncSyncCycle-cmdlet bijgewerkt om aan te geven of een nieuwe cyclus van de synchronisatie is gestart of niet.
- Toegevoegde cmdlet Stop ADSyncSyncCycle beëindigen sync cyclus en bewerking die momenteel uitgevoerd worden.
- Bijgewerkte cmdlet Stop ADSyncScheduler beëindigen sync cyclus en bewerking die momenteel uitgevoerd worden.
- Bij het configureren van [Directory-extensies](active-directory-aadconnectsync-feature-directory-extensions.md) in Azure AD verbinding maken met wizard, kan AD-kenmerk van het type 'Teletex string' nu worden geselecteerd.

## <a name="111890"></a>1.1.189.0
Uitgebracht: 2016 juni

**Opgeloste problemen en verbeteringen:**

- Azure AD Connect kan nu worden geïnstalleerd op een FIPS-compatibele server.
    - Zie [Password Sync en FIPS](active-directory-aadconnectsync-implement-password-synchronization.md#password-synchronization-and-fips) voor Wachtwoordsynchronisatie,
- Een probleem opgelost waarbij een NetBIOS-naam kan niet worden omgezet in de FQDN-naam in Active Directory Connector.

## <a name="111800"></a>1.1.180.0
Uitgebracht: 2016 mei

**Nieuwe functies:**

- Waarschuwt en helpt u bij het verifiëren van domeinen als u dit niet hebt gedaan voordat u Azure AD verbinden.
- Ondersteuning toegevoegd voor [Microsoft Cloud Duitsland](active-directory-aadconnect-instances.md#microsoft-cloud-germany).
- Ondersteuning toegevoegd voor de meest recente [Microsoft Azure overheid cloud](active-directory-aadconnect-instances.md#microsoft-azure-government-cloud) infrastructuur met nieuwe eisen van de URL.

**Opgeloste problemen en verbeteringen:**

- Toegevoegd aan Sync regel Editor waarmee u gemakkelijk vinden sync regels filteren.
- Verbeterde prestaties bij het verwijderen van de ruimte van een connector.
- Problemen met een vaste wanneer hetzelfde object is verwijderd en toegevoegd in dezelfde uitvoeren (wel verwijderen/toevoegen).
- Een uitgeschakelde Sync-regel wordt niet meer opnieuw in te schakelen opgenomen objecten en kenmerken van de upgrade of Active directory-schema te vernieuwen.

## <a name="111300"></a>1.1.130.0
Uitgebracht: 2016 April

**Nieuwe functies:**

- Ondersteuning toegevoegd voor kenmerken met meerdere waarden [Extensions Directory](active-directory-aadconnectsync-feature-directory-extensions.md).
- Ondersteuning toegevoegd voor meer variaties van de configuratie voor [Automatische upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) in aanmerking komt voor een upgrade worden beschouwd.
- Sommige cmdlets voor [aangepaste scheduler](active-directory-aadconnectsync-feature-scheduler.md#custom-scheduler)toegevoegd.

## <a name="111190"></a>1.1.119.0
Uitgebracht: 2016 maart

**Vaste problemen:**

- Gemaakt wordt of Express installeren op Windows Server 2008 (pre-R2) kan niet worden gebruikt na synchronisatie wachtwoord niet ondersteund op dit besturingssysteem.
- Upgrade van DirSync met een aangepast filter configuratie werkt niet zoals verwacht.
- Bij het upgraden naar een nieuwere versie en er zijn geen wijzigingen in de configuratie, moet niet een volledige import/synchronisatie worden gepland.

## <a name="111100"></a>1.1.110.0
Uitgebracht: februari van 2016

**Vaste problemen:**

- Upgrade van eerdere versies werken niet als de installatie niet in de standaardmap **C:\Program Files** .
- Als u installeert en **Start de synchronisatie proces..** deselecteren aan het einde van de installatiewizard van de installatiewizard opnieuw uit te voeren niet mogelijk de scheduler.
- De scheduler werkt niet zoals verwacht op datum-en tijdnotatie waar geen Amerikaanse-en is-servers. Blokkeert tevens `Get-ADSyncScheduler` op de juiste tijden terug.
- Als u een eerdere versie van Azure AD verbinden met AD FS als de optie aanmelden en een upgrade hebt geïnstalleerd, niet kunt u de installatiewizard opnieuw uitvoeren.

## <a name="111050"></a>1.1.105.0
Uitgebracht: februari van 2016

**Nieuwe functies:**

- [Automatische upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) functie voor klanten van Express-instellingen.
- Ondersteuning voor de globale beheerder met MVR gesloten en PIM in de installatiewizard.
    - U moet de proxy waarmee ook verkeer naar https://secure.aadcdn.microsoftonline-p.com als u MVR gesloten.
    - U moet de https://secure.aadcdn.microsoftonline-p.com toevoegen aan uw lijst met vertrouwde websites voor MVR gesloten correct werken.
- -In methode van de gebruiker wijzigen na de initiële installatie toegestaan.
- [Domein en organisatie-eenheid filteren](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) toestaan in de wizard installeren. Hierdoor kunnen ook verbinding maken met forests waarin niet alle domeinen beschikbaar zijn.
- [Taakplanner](active-directory-aadconnectsync-feature-scheduler.md) is ingebouwd in de synchronisatie-engine.

**Kenmerken van voorbeeld gepromoveerd tot GA:**

- [Apparaat Write-back](active-directory-aadconnect-feature-device-writeback.md).
- [Directory-extensies](active-directory-aadconnectsync-feature-directory-extensions.md).

**Nieuwe functies van voorbeeld:**

- De nieuwe standaard gesynchroniseerd interval is 30 minuten cyclus. 3 uur voor alle eerdere versies worden gebruikt. Ondersteuning als het gedrag van de [Taakplanner](active-directory-aadconnectsync-feature-scheduler.md) wilt wijzigen.

**Vaste problemen:**

- De pagina controleren of DNS-domeinen niet altijd herkend door de domeinen.
- Domein admin referenties bij het configureren van AD FS wordt gevraagd.
- De lokalen op AD-accounts worden niet herkend door de wizard installeren als zich in een domein met een andere DNS-structuur dan het hoofddomein.

## <a name="1091310"></a>1.0.9131.0
Uitgebracht: 2015 December

**Vaste problemen:**

- Wachtwoord synchronisatie werkt mogelijk niet als u wachtwoorden in AD DS, maar werkt wijzigen wanneer u wachtwoord instelt.
- Als er een proxyserver, Azure AD-verificatie mislukken tijdens de installatie of de VN bijwerken op de configuratiepagina.
- Een eerdere versie van Azure AD verbinding maken met een volledige mislukt SQL Server als u niet de SA in SQL.
- Een eerdere versie van Azure AD verbinding maken met een externe wordt SQL Server de fout 'Kan geen toegang tot de database SQL ADSync' weergegeven.

## <a name="1091250"></a>1.0.9125.0
Uitgebracht: 2015 November

**Nieuwe functies:**

- Kan de configuratie van de ADFS naar Azure AD vertrouwen.
- Kan Active Directory-schema vernieuwen en Sync regels genereren.
- Kan een regel synchronisatie uitschakelen.
- Kan 'AuthoritativeNull' definiëren als een nieuwe letterlijke waarde in een regel voor synchronisatie.

**Nieuwe functies van voorbeeld:**

- [Azure AD verbinding maken met de gezondheid voor synchronisatie](active-directory-aadconnect-health-sync.md).
- Ondersteuning voor Wachtwoordsynchronisatie [Azure AD Domain Services](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) .

**Nieuwe ondersteund scenario:**

- Ondersteunt meerdere Exchange-organisaties op gebouwen. [Hybride implementaties met meerdere Active Directory-forests](https://technet.microsoft.com/library/jj873754.aspx) Zie voor meer informatie.

**Vaste problemen:**

- Synchronisatieproblemen wachtwoord:
    - Een object verplaatst van out-het bereik naar in het bereik hebben niet het wachtwoord gesynchroniseerd. Deze incudes organisatie-eenheid en kenmerk filteren.
    - Selecteren van een nieuwe organisatie-eenheid op te nemen synchroon vereist een synchronisatie volledig wachtwoord niet.
    - Het wachtwoord wordt niet gesynchroniseerd wanneer een uitgeschakelde gebruiker is ingeschakeld.
    - Het wachtwoord opnieuw wachtrij is oneindig en de vorige limiet van 5000 objecten moeten worden ingetrokken is verwijderd.
    - [Verbeterde probleemoplossing](active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization).
- Kan verbinding maken met Active Directory met Windows Server 2016 forest-functionaliteitsniveau.
- Kan de groep die wordt gebruikt voor het filteren van groep na de oorspronkelijke installatie wijzigen.
- Maakt een nieuw gebruikersprofiel niet langer op de Azure AD Connect-server voor elke gebruiker een wachtwoordwijziging doen met wachtwoord writeback is ingeschakeld.
- Niet met lange Integer waarden synchroon regels bereiken.
- Het selectievakje "Write-back apparaat" blijft uitgeschakeld als er domeincontrollers niet bereikbaar.

## <a name="1086670"></a>1.0.8667.0
Uitgebracht: 2015 augustus

**Nieuwe functies:**

- De installatiewizard Azure AD verbinden is nu op alle Windows Server-talen vertaald.
- Ondersteuning toegevoegd voor account ontgrendelen wanneer u AD Azure wachtwoordbeheer.

**Vaste problemen:**

- Azure verbinden met AD-installatiewizard loopt vast als een andere gebruiker heeft nog steeds installeren in plaats van de persoon die de installatie voor het eerst startte.
- Als een eerdere installatie van Azure AD verbinding mislukt Azure AD verbinden sync correct te verwijderen, is het niet mogelijk zijn om opnieuw te installeren.
- Azure AD verbinden via een snelle installatie als de gebruiker niet in het hoofddomein van het forest of als een niet-Engelstalige versie van Active Directory wordt gebruikt, kan niet worden geïnstalleerd.
- Als de FQDN-naam van het Active Directory-gebruikersaccount niet kan worden opgelost, wordt misleidende foutbericht 'Mislukt het schema doorvoeren' weergegeven.
- Als de account die wordt gebruikt op de Active Directory Connector niet met de wizard wordt gewijzigd, wordt de wizard niet tijdens latere uitvoeringen.
- Azure AD Connect kan soms op een domeincontroller installeren.
- Kan niet in- en uitschakelen 'Staging-modus' als extensie kenmerken zijn toegevoegd.
- Wachtwoord worden teruggeboekt in sommige configuratie vanwege een onjuist wachtwoord in de Active Directory Connector.
- DirSync kan niet worden bijgewerkt als de DN-naam wordt gebruikt in het kenmerk filteren.
- Veel CPU-gebruik wanneer u opnieuw instellen van wachtwoorden.

**Voorbeeld van verwijderde functies:**

- De voorbeeldfunctie [die gebruiker Write-back](active-directory-aadconnect-feature-preview.md#user-writeback) is tijdelijk verwijderd op basis van de feedback van onze klanten preview. Het wordt opnieuw later worden toegevoegd wanneer de gegeven feedback is verholpen.

## <a name="1086410"></a>1.0.8641.0
Uitgebracht: 2015 juni

**Eerste versie van Azure AD verbinden.**

Gewijzigde naam van Azure AD Sync Azure AD verbinden.

**Nieuwe functies:**

- [Instellingen voor snelle](./connect/active-directory-aadconnect-get-started-express.md) installatie
- Kunt u [AD FS configureren](./connect/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)
- Kunt u [een upgrade uitvoert van DirSync](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md)
- [Niet per ongeluk verwijderen](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)
- Geïntroduceerd [in staging-modus](active-directory-aadconnectsync-operations.md#staging-mode)

**Nieuwe functies van voorbeeld:**

- [Gebruiker Write-back](active-directory-aadconnect-feature-preview.md#user-writeback)
- [Groep Write-back](active-directory-aadconnect-feature-preview.md#group-writeback)
- [Apparaat Write-back](active-directory-aadconnect-feature-device-writeback.md)
- [Directory-extensies](active-directory-aadconnect-feature-preview.md#directory-extensions)


## <a name="104940501"></a>1.0.494.0501
Uitgebracht: 2015 mei

**Nieuwe vereiste:**

- Azure AD synchronisatie vereist nu .net framework versie 4.5.1 moet worden geïnstalleerd.

**Vaste problemen:**

- Wachtwoord writeback uit Azure Active Directory is mislukt met een ODBC-fout servicebus.

## <a name="104910413"></a>1.0.491.0413
Uitgebracht: 2015 April

**Opgeloste problemen en verbeteringen:**

- De Active Directory Connector wordt verwijderd niet juist verwerkt als de Prullenbak is ingeschakeld en er meerdere domeinen in het forest zijn.
- De prestaties van de import is verbeterd voor de Azure Active Directory Connector.
- Wanneer een groep is dan het lidmaatschap (standaard de limiet is ingesteld op 50k objecten), de groep in Azure Active Directory is verwijderd. Het nieuwe gedrag is dat de groep blijft, wordt een fout gegenereerd en geen nieuwe wijzigingen in groepslidmaatschappen worden geëxporteerd.
- Een nieuw object kan niet worden ingericht als een gefaseerde verwijderen met de DN-naam dezelfde al aanwezig in de ruimte van de connector is.
- Sommige objecten worden worden gesynchroniseerd tijdens een synchronisatie delta, maar er geen wijziging klaargezet in het object is gemarkeerd.
- Een wachtwoord synchronisatie afdwingen, verwijdert ook de voorkeurslijst DC.
- CSExportAnalyzer heeft problemen met sommige lidstaten objecten.

**Nieuwe functies:**

- Een join kunt nu verbinding maken met 'ANY' objecttype in de MV.

## <a name="104850222"></a>1.0.485.0222
Uitgebracht: 2015 februari

**Verbeteringen:**

- Verbeterde import-prestaties.

**Vaste problemen:**

- Password Sync respecteert het cloudFiltered attribuut met een kenmerk filter gebruikt. Gefilterde objecten langer niet in het bereik voor Wachtwoordsynchronisatie.
- In zeldzame gevallen waar de topologie zeer veel domeincontrollers heeft werkt sync wachtwoord niet.
- 'Gestopt-server' bij het importeren van de Connector Azure AD na het apparaat is ingeschakeld in Azure AD/Intune.
- Lid worden van afwijkende beveiligings-Principals (FSP's) vanuit meerdere domeinen in hetzelfde forest veroorzaakt een dubbelzinnige join-fout.

## <a name="104751202"></a>1.0.475.1202
Uitgebracht: 2014-December

**Nieuwe functies:**

- Het wordt nu ondersteund u Wachtwoordsynchronisatie met kenmerk op basis van filtering doet. Zie [Wachtwoordsynchronisatie met filteren](active-directory-aadconnectsync-configure-filtering.md)voor meer informatie.
- Het kenmerk msDS-ExternalDirectoryObjectID wordt opgeslagen in AD. Hiermee wordt ondersteuning toegevoegd voor Office 365-toepassingen met behulp van OAuth2, Online toegang tot en het On-Premises postvakken in een Exchange-implementatie van hybride.

**Vaste upgrade problemen:**

- Een nieuwere versie van de assistent-in is beschikbaar op de server.
- Een aangepaste installatiepad gebruikt Azure AD Sync.
- Een ongeldige aangepaste join criterium blokkeert de upgrade.

**Andere correcties:**

- De sjablonen vastgesteld voor Office Pro Plus.
- Problemen met de vaste installatie veroorzaakt door namen die met een streepje beginnen.
- Vastgesteld dat de instelling van de sourceAnchor verloren gaat bij het uitvoeren van de installatiewizard op een tweede keer.
- Vaste ETW-tracering voor Wachtwoordsynchronisatie

## <a name="104701023"></a>1.0.470.1023
Uitgebracht: 2014 oktober

**Nieuwe functies:**

- Wachtwoordsynchronisatie van meerdere in-premises AD naar Azure AD.
- Gelokaliseerde gebruikersinterface tijdens installatie op alle Windows Server-talen.

**Een upgrade van AADSync 1.0 GA**

Als u al Azure AD Sync is geïnstalleerd, is er een extra stap die u nemen moet als u een van de out-of-box-synchronisatieregels zijn gewijzigd. Nadat u een upgrade hebt uitgevoerd naar de 1.0.470.1023 loslaat, de synchronisatie regels die u hebt gewijzigd, worden gedupliceerd. Voor elke gewijzigde Sync-regel doen het volgende:

- Zoek de regel Sync u hebt gewijzigd en noteert u de wijzigingen.
- De Sync-regel verwijderen.
- Zoek de nieuwe synchronisatie regel gemaakt met Azure AD Sync en de wijzigingen opnieuw toe te passen.

**Machtigingen voor de AD-account**

De AD-account moet extra machtigingen kunnen de wachtwoord-hashes van AD lezen worden verleend. De machtigingen toe te kennen de naam 'Directory-wijzigingen repliceren' en "Repliceren van Directory alle wijzigingen". Beide machtigingen zijn vereist om te kunnen lezen van de wachtwoord-hashes

## <a name="104190911"></a>1.0.419.0911
Uitgebracht: 2014 September

**Eerste versie van Azure AD Sync.**

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md).
