<properties
    pageTitle="Azure AD verbinden: Aangepaste installatie | Microsoft Azure"
    description="Dit document informatie over de aangepaste installatie-opties voor Azure AD verbinden. Gebruik deze instructies voor de installatie van Active Directory via Azure AD verbinden."
    services="active-directory"
    keywords="Wat is Azure AD verbinden, vereiste onderdelen voor AD Azure Active Directory installeren"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="billmath"/>

# <a name="custom-installation-of-azure-ad-connect"></a>Aangepaste installatie van Azure AD verbinden
Azure AD verbinding maken met **aangepaste instellingen** wordt gebruikt als u meer opties voor de installatie. Het wordt gebruikt als er meerdere forests of als u optionele functies die niet worden besproken in de express-installatie te configureren. Het wordt gebruikt in alle gevallen waarin de optie [**Snelle installatie**](active-directory-aadconnect-get-started-express.md) niet voldoet aan de implementatie of de topologie.

Voordat u begint met het installeren van Azure AD verbinden, zorg ervoor dat [Azure AD verbinden](http://go.microsoft.com/fwlink/?LinkId=615771) downloaden en volledig de voorafgaande vereiste stappen in [Azure AD verbinden: Hardware en vereisten](../active-directory-aadconnect-prerequisites.md). Controleer ook of dat u accounts beschikbaar zoals beschreven in [Azure AD verbinden accounts en machtigingen](active-directory-aadconnect-accounts-permissions.md)zijn vereist.

Als u aangepaste instellingen komt niet overeen met de topologie, bijvoorbeeld om te upgraden DirSync, [gerelateerde documentatie](#related-documentation) voor andere scenario's bekijken.

## <a name="custom-settings-installation-of-azure-ad-connect"></a>Instellingen voor aangepaste installatie van Azure AD verbinden

### <a name="express-settings"></a>Express-instellingen
Op deze pagina, klikt u op **aanpassen** om een aangepaste instellingen-installatie te starten.

### <a name="install-required-components"></a>Vereiste onderdelen installeren
Als u de Synchronisatieservices hebt geïnstalleerd, kunt u de optionele sectie niet-gecontroleerde en Azure AD verbinden om alles wordt automatisch ingesteld. Een exemplaar van SQL Server 2012 Express LocalDB ingesteld, de juiste groepen maken en machtigingen toe te wijzen. Als u wilt dat de standaardwaarden hebt gewijzigd, kunt u in de volgende tabel om te begrijpen van de optionele configuratie-opties die beschikbaar zijn.

![Vereiste onderdelen](./media/active-directory-aadconnect-get-started-custom/requiredcomponents.png)

Optionele configuratie  | Beschrijving
------------- | -------------
Een bestaande SQL Server gebruiken | Kunt u de naam van de SQL Server en de naam van het exemplaar opgeven. Kies deze optie als u al een databaseserver die u wilt gebruiken. Voer de instantienaam gevolgd door een komma en poort nummer in de **Naam van de instantie** als de SQL-Server heeft geen bladeren is ingeschakeld.
Een bestaande service-account gebruiken | Azure AD-verbinding maakt standaard een account lokale service voor de synchronisatieservices te gebruiken. Het wachtwoord wordt automatisch gegenereerd en onbekend bij de persoon die installeert Azure AD verbinden. Als u een externe SQL-Server of een proxyserver waarvoor verificatie vereist is, moet u een service account in het domein en het wachtwoord kent. Voer in dat geval de serviceaccount te gebruiken. Controleer of de gebruiker die de installatie uitvoert is een SA in SQL zodat een aanmelding voor de serviceaccount kan worden gemaakt. Zie [Azure AD verbinden accounts en machtigingen](active-directory-aadconnect-accounts-permissions.md#custom-settings-installation)
Aangepaste synchronisatie groepen opgeven | Azure AD-verbinding maakt standaard vier groepen lokaal op de server wanneer de Synchronisatieservices zijn geïnstalleerd. Deze groepen zijn: de groep Administrators, groep Operators bladeren groep en de groep opnieuw instellen van wachtwoorden. U kunt hier uw eigen groepen. De groepen moeten lokaal op de server en kunnen niet worden gevonden in het domein.

### <a name="user-sign-in"></a>Gebruiker aanmelden
Na het installeren van de vereiste onderdelen, wordt u gevraagd uw gebruikers één aanmelding methode selecteren. De volgende tabel bevat een korte beschrijving van de beschikbare opties. Zie voor een volledige beschrijving van de methoden aanmelden [gebruiker aanmelden](../active-directory-aadconnect-user-signin.md).

![Inloggen gebruiker](./media/active-directory-aadconnect-get-started-custom/usersignin.png)

Single Sign On, optie | Beschrijving
------------- | -------------
Password Sync | Gebruikers kunnen zich aanmelden bij Microsoft cloud services, zoals Office 365, die ze in hun netwerk op ruimten gebruiken hetzelfde wachtwoord gebruiken. Azure AD wachtwoord hash worden gesynchroniseerd met de wachtwoorden van gebruikers en verificatie vindt plaats in de cloud. Zie [Wachtwoordsynchronisatie](../active-directory-aadconnectsync-implement-password-synchronization.md) voor meer informatie.
Federatie met AD FS | Gebruikers kunnen zich aanmelden bij Microsoft cloud services, zoals Office 365, die ze in hun netwerk op ruimten gebruiken hetzelfde wachtwoord gebruiken.  De gebruikers worden omgeleid tot hun gebouwen voor AD FS om aan te melden instantie en verificatie vindt plaats op locatie.
Configureer geen | Geen functie is geïnstalleerd en geconfigureerd. Kies deze optie als u al een 3e partij federation-server of een andere bestaande oplossing in plaats.

### <a name="connect-to-azure-ad"></a>Verbinding maken met Azure AD
Voer op de verbinding maken met Azure AD-scherm, een algemene admin-account en wachtwoord. Als u een **Federatie met AD FS** op de vorige pagina hebt geselecteerd, niet aanmeldt met een account in een domein dat u wilt inschakelen voor de Federatie. Een aanbeveling is het gebruik van een account in het standaarddomein van **onmicrosoft.com** die wordt geleverd bij de Azure AD-map.

Deze rekening wordt alleen gebruikt voor het maken van een serviceaccount in Azure AD en wordt niet gebruikt nadat de wizard is voltooid.  
![Inloggen gebruiker](./media/active-directory-aadconnect-get-started-custom/connectaad.png)

Als uw account admin globale MVR gesloten ingeschakeld is, moet u het wachtwoord opnieuw in het pop-in en voltooi de uitdaging van de MVR gesloten. De uitdaging kan zijn een bieden een verificatiecode of een telefoongesprek.  
![Gebruiker inloggen MVR gesloten](./media/active-directory-aadconnect-get-started-custom/connectaadmfa.png)

De globale account kan ook zijn [Bevoorrechte Identity Management](../active-directory-privileged-identity-management-getting-started.md) ingeschakeld.

Als u een foutbericht en problemen met verbindingen, Zie vervolgens [verbindingsproblemen oplossen](../active-directory-aadconnect-troubleshoot-connectivity.md).

## <a name="pages-under-the-section-sync"></a>Pagina's in de sectie synchroniseren

### <a name="connect-your-directories"></a>Verbinding maken met uw mappen
Als u verbinding maakt met uw Active Directory Domain Services, moet Azure AD verbinding maken met de referenties van een account met machtigingen. U kunt het domeingedeelte in NetBios of FQDN-indeling, dat wil zeggen, FABRIKAM\syncuser of fabrikam.com\syncuser. Deze account is een normale gebruikersaccount omdat er alleen de standaard leesmachtigingen. Echter, afhankelijk van het scenario moet u mogelijk meer machtigingen. Zie voor meer informatie, [Azure AD verbinding Accounts en machtigingen](../active-directory-aadconnect-accounts-permissions.md#create-the-ad-ds-account)

![Verbinding maken met de map](./media/active-directory-aadconnect-get-started-custom/connectdir.png)

### <a name="azure-ad-sign-in-configuration"></a>Azure AD-in configuratie
Op deze pagina kunt u de UPN-domeinen die aanwezig zijn op ruimten AD DS en waaraan in Azure AD zijn geverifieerd. Op deze pagina kunt u het kenmerk moet worden gebruikt voor de userPrincipalName.

![Niet-geverifieerde domeinen](./media/active-directory-aadconnect-get-started-custom/aadsigninconfig.png)  
Bekijk elk domein is gemarkeerd als **Niet toegevoegd** en wordt **Niet gecontroleerd**. Zorg ervoor dat deze domeinen die u gebruikt in Azure AD zijn geverifieerd. Wanneer u uw domeinen hebt gecontroleerd, klikt u op het symbool voor vernieuwing. Zie voor meer informatie [toevoegen en controleer of het domein](../active-directory-add-domain.md)

**UserPrincipalName** - het kenmerk userPrincipalName is het kenmerk gebruikers gebruiken wanneer zij zich voor AD Azure aanmelden en Office 365. De domeinen gebruikt, ook wel bekend als het UPN-achtervoegsel, moeten worden geverifieerd in Azure AD voordat de gebruikers worden gesynchroniseerd. Microsoft raadt aan dat de standaard kenmerk userPrincipalName. Als dit kenmerk niet routeerbaar is en kan niet worden gecontroleerd, is het mogelijk om te selecteren, een ander kenmerk. U kunt bijvoorbeeld e-mail selecteren als het kenmerk met de aanmeldings-ID. Met behulp van een ander kenmerk dan userPrincipalName staat bekend als **Alternatieve ID**. De waarde van het kenmerk ID alternatieve moet voldoen aan de standaard RFC822. Een alternatieve ID kan worden gebruikt met wachtwoord synchronisatie en federation.

>[AZURE.WARNING]
Een alternatieve ID is niet compatibel met alle Office 365 werkbelasting. Ga naar [Alternatieve aanmeldings-ID configureren](https://technet.microsoft.com/library/dn659436.aspx)voor meer informatie.

### <a name="domain-and-ou-filtering"></a>Domein en organisatie-eenheid filteren
Alle domeinen en organisatie-eenheden worden standaard gesynchroniseerd. Als er bepaalde domeinen of organisatie-eenheden u wilt synchroniseren met AD Azure, kunt u deze domeinen en organisatie-eenheden deselecteren.  
![Filteren van DomainOU](./media/active-directory-aadconnect-get-started-custom/domainoufiltering.png) deze pagina in de wizard configureert filteren op basis van een domein. Zie [filteren op basis van een domein](../active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering)voor meer informatie.

Het is ook mogelijk dat sommige domeinen niet bereikbaar vanwege beperkingen van de firewall zijn. Deze domeinen zijn standaard uitgeschakeld en wordt een waarschuwing.  
![Onbereikbaar domeinen](./media/active-directory-aadconnect-get-started-custom/unreachable.png)  
Als u deze waarschuwing ziet, Controleer of deze domeinen zijn immers niet bereikbaar en de waarschuwing wordt verwacht.

### <a name="uniquely-identifying-your-users"></a>Een unieke identificatie van uw gebruikers
De Matching tussen forests functie kunt u definiëren hoe gebruikers uit uw AD DS-forests in Azure AD worden weergegeven. Een gebruiker kan ofwel slechts één keer weergegeven in alle forests of een combinatie van ingeschakelde en uitgeschakelde accounts hebben. De gebruiker kan ook worden weergegeven als een contactpersoon in enkele bossen.

![De unieke](./media/active-directory-aadconnect-get-started-custom/unique.png)

Instelling | Beschrijving
------------- | -------------
[Gebruikers slechts éénmaal weergegeven in alle forests](../active-directory-aadconnect-topologies.md#multiple-forests-separate-topologies) | Alle gebruikers worden gemaakt als afzonderlijke objecten in AD Azure. De objecten zijn geen lid van de metaverse.
[E-mailkenmerk](../active-directory-aadconnect-topologies.md#multiple-forests-full-mesh-with-optional-galsync) | Deze optie voegt gebruikers en contactpersonen als het e-mailkenmerk dezelfde waarde in verschillende forests heeft. Gebruik deze optie als u uw contactpersonen zijn gemaakt met behulp van GALSync.
[ObjectSID en msExchangeMasterAccountSID / msRTCSIP OriginatorSid](../active-directory-aadconnect-topologies.md#multiple-forests-account-resource-forest) | Deze optie voegt een ingeschakelde gebruiker in een forest rekening met een uitgeschakelde gebruiker in een forest met bronnen. Deze configuratie wordt in Exchange een gekoppelde postbus genoemd. Deze optie kan ook worden gebruikt als u alleen Lync en Exchange niet aanwezig in het bronforest is.
sAMAccountName en MailNickName | Deze optie voegt op kenmerken waar naar verwachting dat de aanmeldings-ID voor de gebruiker kan worden gevonden.
Een specifiek kenmerk | Met deze optie kunt u uw eigen kenmerk selecteren. **Beperking:** Zorg ervoor dat een kenmerk dat al kan worden gevonden in de metaverse te kiezen. Als u een aangepast kenmerk ophalen (niet in de metaverse), kan de wizard niet voltooien.

**Bron anker** - het kenmerk sourceAnchor is een kenmerk dat gedurende de levensduur van een gebruikersobject onveranderbaar is. Het is de primaire sleutel als de gebruiker op de gebouwen met de gebruikers koppelen in Azure AD. Aangezien het kenmerk kan niet worden gewijzigd, moet u plannen voor een goede kenmerk moet worden gebruikt. Een goede kandidaat is objectGUID. Dit kenmerk wordt niet gewijzigd, tenzij de account wordt verplaatst tussen forests/domeinen. In een omgeving met meerdere forests als u accounts tussen forests verplaatst een ander kenmerk moet worden gebruikt, zoals een kenmerk met de werknemer-id. Kenmerken die u wilt wijzigen wanneer een persoon marries of toewijzingen voorkomen. U kan geen kenmerken met een @-sign, zodat het e-mailadres en userPrincipalName kunnen niet worden gebruikt. Ook het kenmerk onderscheid wordt dus als u een object tussen forests verplaatst, zorg voor het behoud van de hoofdletters/kleine letters. Binaire kenmerken base64-gecodeerd, maar andere kenmerktypen blijven in ongecodeerde staat. Dit kenmerk is in de federation-scenario's en sommige Azure AD-interfaces, ook bekend als immutableID. Meer informatie over het bron-anker vindt u in de [ontwerpconcepten](../active-directory-aadconnect-design-concepts.md#sourceAnchor).

### <a name="sync-filtering-based-on-groups"></a>Sync filteren op basis van groepen
De functie van groepen te kunt filteren u een kleine subset van objecten synchroniseren voor een leider. Deze functie wilt gebruiken, maakt u een groep voor dit doel in de Active Directory op gebouwen. Vervolgens gebruikers en groepen die moeten worden gesynchroniseerd naar Azure AD als direct leden toevoegen. U kunt later toevoegen en verwijderen van gebruikers aan deze groep aan de lijst met objecten die aanwezig zijn in Azure AD onderhouden. Alle objecten die u wilt synchroniseren moeten direct lid van de groep. Gebruikers, groepen, contactpersonen en computers/apparaten moeten alle directe lid zijn. Lidmaatschap van geneste groepen is niet opgelost. Wanneer u een groep wilt toevoegen als lid, alleen de groep zelf wordt toegevoegd en niet de leden.

![Sync filteren](./media/active-directory-aadconnect-get-started-custom/filter2.png)

>[AZURE.WARNING]
Deze functie is alleen bedoeld ter ondersteuning van een pilot. Gebruik deze niet in een volledige productie-implementatie.

In een implementatie van de volledige productie zal het moeilijk te onderhouden een enkele groep met alle objecten die u wilt synchroniseren. In plaats daarvan moet u een van de methoden in [Configure filters](../active-directory-aadconnectsync-configure-filtering.md).

### <a name="optional-features"></a>Optionele functies
In dit scherm kunt u de optionele onderdelen voor uw specifieke scenario's te selecteren.

![Optionele functies](./media/active-directory-aadconnect-get-started-custom/optional.png)

>[AZURE.WARNING]
Als u momenteel DirSync of Azure AD synchroniseren active, activeer van de functies van de Write-back in Azure AD verbinding maken.

Optionele functies | Beschrijving
------------------- | -------------
Implementatie van Exchange-hybride | De implementatie van Exchange hybride functie kunt u de coëxistentie van Exchange-postbussen beide ruimten op en in Office 365. Azure AD verbinden wordt een specifieke set van [kenmerken](../active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) van Azure AD gesynchroniseerd terug naar de map op locatie.
Azure AD app en kenmerk filteren | Doordat Azure AD app en kenmerk filteren kan de set met gesynchroniseerde kenmerken worden aangepast. Deze optie voegt twee meer configuratie-pagina's naar de wizard. Zie [Azure AD app en kenmerk filteren](#azure-ad-app-and-attribute-filtering)voor meer informatie.
Wachtwoordsynchronisatie | Als u de oplossing-in federation geselecteerd, kunt u deze optie inschakelen. Wachtwoordsynchronisatie kan vervolgens worden gebruikt als back-up optie. Zie [Wachtwoordsynchronisatie](../active-directory-aadconnectsync-implement-password-synchronization.md)voor meer informatie.
Wachtwoord Write-back | Doordat wachtwoord writeback wachtwoordwijzigingen die afkomstig zijn uit de Azure AD teruggeschreven naar de map op locatie. Zie [aan de slag met wachtwoordbeheer](../active-directory-passwords-getting-started.md)voor meer informatie.
Groep Write-back | Als u de functie voor **Office 365-groepen** gebruikt, kunt u deze groepen weergegeven in de Active Directory op gebouwen hebben. Deze optie is alleen beschikbaar als u Exchange in uw Active Directory op het bedrijf aanwezig zijn. Voor meer informatie Zie [groep Write-back](../active-directory-aadconnect-feature-preview.md#group-writeback).
Apparaat Write-back | U kunt terugschrijven apparaatobjecten in Azure AD aan Active Directory in de lokalen voor voorwaardelijke toegang scenario's. Zie [inschakelen apparaat Write-back in Azure AD verbinding maken](../active-directory-aadconnect-feature-device-writeback.md)voor meer informatie.
Directory-synchronisatie voor uitbreiding kenmerk | Doordat de map extensies kenmerk sync worden attributen gesynchroniseerd met Azure AD. Zie [extensies Directory](../active-directory-aadconnectsync-feature-directory-extensions.md)voor meer informatie.

### <a name="azure-ad-app-and-attribute-filtering"></a>Azure AD app en kenmerk filteren
Als u wilt om te beperken welke kenmerken te synchroniseren naar Azure AD, start vervolgens door te selecteren welke services u gebruikt. Als u wijzigingen in de configuratie op deze pagina, heeft een nieuwe service expliciet worden geselecteerd door de installatiewizard opnieuw.

![Optionele functies Apps](./media/active-directory-aadconnect-get-started-custom/azureadapps2.png)

Deze pagina is gebaseerd op de services die in de vorige stap hebt geselecteerd, worden alle kenmerken die worden gesynchroniseerd. Deze lijst is een combinatie van alle objecttypen worden gesynchroniseerd. Als er bepaalde bijzondere kenmerken die u moet niet synchroniseren, kunt u die kenmerken deselecteren.

![Optionele functies kenmerken](./media/active-directory-aadconnect-get-started-custom/azureadattributes2.png)

>[AZURE.WARNING]
Functionaliteit kan van invloed zijn kenmerken verwijderen. Zie [kenmerken gesynchroniseerd](../active-directory-aadconnectsync-attributes-synchronized.md#attributes-to-synchronize)voor best practices en aanbevelingen.

### <a name="directory-extension-attribute-sync"></a>Directory-synchronisatie voor uitbreiding kenmerk
U kunt het schema uitbreiden in Azure AD met aangepaste kenmerken die zijn toegevoegd door uw organisatie of andere kenmerken in Active Directory. Deze functie wilt gebruiken, selecteert u **map extensie kenmerk sync** op de pagina **Optionele functies** . U kunt meer kenmerken wilt synchroniseren op deze pagina.

![Directory-extensies](./media/active-directory-aadconnect-get-started-custom/extension2.png)

Zie [extensies Directory](../active-directory-aadconnectsync-feature-directory-extensions.md)voor meer informatie.

## <a name="configuring-federation-with-ad-fs"></a>Federatie met AD FS configureren
AD FS configureren met Azure AD verbinding maken is eenvoudig met een paar muisklikken. Het volgende is vereist voordat de configuratie.

- Een server met Windows Server 2012 R2 voor de federation-server met extern beheer is ingeschakeld
- Een server met Windows Server 2012 R2 voor het Web Application Proxy server met extern beheer is ingeschakeld
- Een SSL-certificaat voor de federation-service-naam die u wilt gebruiken (bijvoorbeeld sts.contoso.com)

### <a name="ad-fs-configuration-pre-requisites"></a>De vereisten voor AD FS-configuratie
Configureren van uw AD FS-farm met Azure AD verbinden zorgen dat WinRM is ingeschakeld op de externe servers. Bovendien gaat u via de vereiste poorten is vermeld in [tabel 3 - Azure AD Connect en Federation-Servers/WAP](../active-directory-aadconnect-ports.md#table-3---azure-ad-connect-and-federation-serverswap).

### <a name="create-a-new-ad-fs-farm-or-use-an-existing-ad-fs-farm"></a>Een nieuwe AD FS-farm maken of een bestaande AD FS-serverfarm wilt gebruiken
U kunt een bestaande AD FS-serverfarm of kunt u een nieuwe AD FS-farm te maken. Als u een nieuwe opmerking te maken, moet u het SSL-certificaat te leveren. Als het SSL-certificaat is beveiligd met een wachtwoord, wordt u gevraagd het wachtwoord op te geven.

![AD FS-Farm](./media/active-directory-aadconnect-get-started-custom/adfs1.png)

Als u kiest voor een bestaande AD FS-serverfarm, komt u rechtstreeks aan het configureren van de vertrouwensrelatie tussen AD FS en Azure AD scherm.

### <a name="specify-the-ad-fs-servers"></a>De AD FS-servers opgeven
Voer de gewenste AD FS te installeren op servers. U kunt een of meer servers op basis van uw behoeften voor de resourcecapaciteit toevoegen. Lid worden van alle servers in Active Directory voordat u deze configuratie uitvoert. Microsoft raadt het installeren van een enkele server met AD FS voor test- en pilot-implementaties. Voeg vervolgens en implementeren op uw behoeften aanpassen door het uitvoeren van Azure AD verbinding maken opnieuw na de initiële configuratie meer servers.

>[AZURE.NOTE]
Zorg ervoor dat alle servers zijn gekoppeld aan een AD-domein voordat u deze configuratie.

![AD FS-Servers](./media/active-directory-aadconnect-get-started-custom/adfs2.png)

### <a name="specify-the-web-application-proxy-servers"></a>De servers Web Application Proxy opgeven
Voer de servers die u wilt gebruiken als uw webtoepassing proxyservers. Web application proxyserver wordt geïmplementeerd in de DMZ (facing extranet) en ondersteunt verificatieaanvragen van het extranet. U kunt een of meer servers op basis van uw behoeften voor de resourcecapaciteit toevoegen. Microsoft raadt u aan een enkele toepassing webproxyserver voor test- en pilot-implementaties te installeren. Voeg vervolgens en implementeren op uw behoeften aanpassen door het uitvoeren van Azure AD verbinding maken opnieuw na de initiële configuratie meer servers. Het aan te raden een gelijk aantal proxyservers die voldoen aan de verificatie van het intranet.

>[AZURE.NOTE]
<li> Als de account die u gebruikt niet een lokale beheerder op de AD FS-servers, wordt u gevraagd voor Administrator-referenties.</li>
<li> Zorg ervoor dat er HTTP/HTTPS-verbinding tussen de Azure AD verbinden en de Web Application Proxy server is voordat u deze stap uitvoert.</li>
<li> Zorg ervoor dat er verbinding tussen de Web Application Server en de AD FS-server om verificatie-aanvragen via HTTP/HTTPS is.</li>

![Web App](./media/active-directory-aadconnect-get-started-custom/adfs3.png)

U wordt gevraagd referenties op te geven dat de web application server kan een beveiligde verbinding met de AD FS-server maken. Deze referenties moeten een lokale beheerder op de AD FS-server.

![Proxy](./media/active-directory-aadconnect-get-started-custom/adfs4.png)

### <a name="specify-the-service-account-for-the-ad-fs-service"></a>Geef de serviceaccount voor de AD FS-service
De AD FS-service is een service-account van domein voor de verificatie van gebruikers en de gebruikersgegevens opzoeken in Active Directory. Twee soorten accounts-service kan worden ondersteund:

- **Groep beheerd serviceaccount** - geïntroduceerd in Active Directory Domain Services in Windows Server 2012. Dit type account biedt services, zoals AD FS, één account zonder het wachtwoord regelmatig bijwerken. Gebruik deze optie als u al Windows Server 2012-domeincontrollers in het domein die deel uitmaken van uw AD FS-servers aan.
- **Gebruikersaccount voor domein** - dit type account moet u een wachtwoord en het wachtwoord regelmatig bijgewerkt als het wachtwoord gewijzigd of verloopt. Gebruik deze optie alleen als er geen Windows Server 2012-domeincontrollers in het domein die deel uitmaken van uw AD FS-servers aan.

Als u groep beheerd serviceaccount hebt geselecteerd en deze functie is niet in Active Directory gebruikt, wordt u gevraagd om referenties voor ondernemingsadministrator. Deze referenties worden gebruikt voor het initiëren van de belangrijkste winkel- en inschakelen in de Active Directory.

![AD FS-serviceaccount](./media/active-directory-aadconnect-get-started-custom/adfs5.png)

### <a name="select-the-azure-ad-domain-that-you-wish-to-federate"></a>Selecteer de Azure AD-domein dat u wilt communiceren
Deze configuratie wordt gebruikt voor het instellen van de federation-relatie tussen AD FS en Azure AD. Het configureren van AD FS-beveiligingstokens probleem naar Azure AD en Azure AD voor vertrouwen in de tokens van dit specifieke exemplaar van AD FS configureert. Deze pagina kunt u alleen één domein configureren in de oorspronkelijke installatie. U kunt later meer domeinen configureren door Azure AD-verbinding opnieuw uit te voeren.

![Azure AD-domein](./media/active-directory-aadconnect-get-started-custom/adfs6.png)

### <a name="verify-the-azure-ad-domain-selected-for-federation"></a>Controleer de Azure AD domein geselecteerd voor Federatie
Wanneer u het domein worden federatieve selecteert, biedt Azure AD Connect u benodigde informatie om te controleren of een niet-geverifieerde domein. Zie [toevoegen en controleer of het domein](../active-directory-add-domain.md) voor het gebruik van deze informatie.

![Azure AD-domein](./media/active-directory-aadconnect-get-started-custom/verifyfeddomain.png)

>[AZURE.NOTE]
Verbinding maken met AD probeert om te controleren of het domein in de fase configureren. Als u doorgaat zonder de benodigde DNS-records toe te voegen configureren, kan de wizard niet om de configuratie te voltooien.

## <a name="configure-and-verify-pages"></a>Configureren en controleren of de pagina 's
De configuratie gebeurt op deze pagina.

>[AZURE.NOTE]
Voordat u doorgaan met de installatie en u federation hebt geconfigureerd, moet u [naamomzetting voor federatieservers](../active-directory-aadconnect-prerequisites.md#name-resolution-for-federation-servers)hebt geconfigureerd.

![Configureren](./media/active-directory-aadconnect-get-started-custom/readytoconfigure2.png)

### <a name="staging-mode"></a>Staging-modus
Het is mogelijk om een nieuwe synchronisatieserver parallel met de staging-modus in te stellen. Dit wordt alleen ondersteund als u een synchronisatieserver exporteren naar een map in de cloud. Maar als u wilt verplaatsen van een andere server, bijvoorbeeld een lopend DirSync, vervolgens kunt u inschakelen Azure AD verbinden in de staging-modus. Wanneer ingeschakeld, de synchronisatie-engine importeren en synchroniseren van gegevens als normaal, maar deze exporteert niet iets naar Azure AD of AD. Het terugschrijven functies wachtwoord synchronisatie en het wachtwoord in de staging-modus worden uitgeschakeld.

![Staging-modus](./media/active-directory-aadconnect-get-started-custom/stagingmode.png)

In de staging-modus, is het mogelijk om vereiste wijzigingen aanbrengen in de synchronisatie-engine en bekijk wat is geëxporteerd. Wanneer de configuratie er goed uitziet, voert u de installatiewizard opnieuw uit en staging-modus uitschakelt. Gegevens van deze server nu geëxporteerd naar Azure AD. Zorg ervoor dat de andere server op hetzelfde moment dus slechts één server actief is exporteren uit te schakelen.

Zie voor meer informatie de [Staging-modus](../active-directory-aadconnectsync-operations.md#staging-mode).

### <a name="verify-your-federation-configuration"></a>Controleer de configuratie van de Federatie
Azure AD verbinden wordt gecontroleerd of de DNS-instellingen voor u wanneer u klikt op de knop controleren.

![Voltooien](./media/active-directory-aadconnect-get-started-custom/completed.png)

![Controleer of](./media/active-directory-aadconnect-get-started-custom/adfs7.png)

Bovendien als volgt controleren:

- U kunt ondertekenen via een browser van een domein gekoppelde computer op het intranet valideren: verbinding maken met https://myapps.microsoft.com en controleer of het aanmelden met uw account is aangemeld. De ingebouwde administrator-account van AD DS is niet gesynchroniseerd en kan niet worden gebruikt voor verificatie.
- Valideren dat kunt u zich vanaf een apparaat van het extranet. Op een computer thuis of op een mobiel apparaat, verbinding maken met https://myapps.microsoft.com en uw referenties.
- Aanmelden bij de rijke valideren. Verbinding maken met https://testconnectivity.microsoft.com, kies het tabblad **Office 365** en kies de **Office 365 Single Sign-On-Test**.

## <a name="next-steps"></a>Volgende stappen
Nadat de installatie is voltooid, afmelden en opnieuw aanmelden bij Windows voordat u servicebeheer synchronisatie of synchronisatie regeleditor gebruiken.

Nu u hebt die Azure AD Connect geïnstalleerd kunt u [controleren of de installatie en het toewijzen van licenties](../active-directory-aadconnect-whats-next.md).

Meer informatie over deze functies zijn ingeschakeld bij de installatie: [voorkomen dat per ongeluk wordt verwijderd](../active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) en [Azure AD verbinding maken met de gezondheid](../active-directory-aadconnect-health-sync.md).

Meer informatie over deze onderwerpen gemeenschappelijke: [Taakplanner en het synchroniseren starten](../active-directory-aadconnectsync-feature-scheduler.md).

Meer informatie over de [integratie van uw identiteiten op ruimten met Azure Active Directory](../active-directory-aadconnect.md).

## <a name="related-documentation"></a>Gerelateerde documentatie

Onderwerp |  
--------- | ---------
Azure AD-verbinding-overzicht | [Integratie van uw identiteiten op ruimten met Azure Active Directory](../active-directory-aadconnect.md)
Met behulp van instellingen voor Express installeren | [Snelle installatie van Azure AD verbinden](active-directory-aadconnect-get-started-express.md)
DirSync upgraden | [Een upgrade van Azure AD synchronisatietool (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)
Accounts die worden gebruikt voor de installatie | [Meer informatie over het verbinden van Azure AD accounts en machtigingen](active-directory-aadconnect-accounts-permissions.md)
