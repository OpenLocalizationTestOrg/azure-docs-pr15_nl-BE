<properties
    pageTitle="Azure AD verbinden sync: wat zijn de standaardconfiguratie | Microsoft Azure"
    description="Dit artikel beschrijft de standaardconfiguratie Azure AD verbinden te synchroniseren."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-sync-understanding-the-default-configuration"></a>Azure AD verbinden sync: Wat is de standaardconfiguratie?
In dit artikel worden de out-of-box configuratieregels uitgelegd. Deze worden de regels en welke gevolgen deze regels hebben voor de configuratie. Ook begeleidt u bij de standaardconfiguratie van Azure AD verbinden sync. Het doel is dat de lezer begrijpt hoe de configuratiemodel declaratieve ingericht met de naam werkt in een praktijkvoorbeeld. In dit artikel wordt ervan uitgegaan dat u al hebt geïnstalleerd en synchronisatie van Azure AD verbinding maken met de installatiewizard configureren.

Om te begrijpen van de details van het configuratiemodel, Lees [Wat declaratieve inrichten](active-directory-aadconnectsync-understanding-declarative-provisioning.md).

## <a name="out-of-box-rules-from-on-premises-to-azure-ad"></a>Kant-en-klare regels op ruimten naar Azure AD
De volgende expressies vindt u in de configuratie van de out-of-box.

### <a name="user-out-of-box-rules"></a>Kant-en-klare regels
Deze regels gelden ook voor het type van het object iNetOrgPerson.

Een user-object moet voldoen aan de volgende worden gesynchroniseerd:

- Moet een sourceAnchor hebben.
- SourceAnchor wijzigen niet nadat het object is gemaakt in Azure AD. Als de waarde gewijzigd in lokalen is, stopt het object gesynchroniseerd totdat de sourceAnchor terug naar de vorige waarde is gewijzigd.
- Hebben moet het kenmerk accountEnabled (userAccountControl) ingevuld. Met een Active Directory op gebouwen is dit kenmerk altijd aanwezig en ingevuld.

De volgende gebruikersobjecten zijn **niet** gesynchroniseerd met Azure AD:

- `IsPresent([isCriticalSystemObject])`. Zorgen de vele kant-en-klare objecten in Active Directory, zoals de ingebouwde administrator-account, worden niet gesynchroniseerd.
- `IsPresent([sAMAccountName]) = False`. Ervoor te zorgen zonder sAMAccountName-kenmerk user-objecten worden niet gesynchroniseerd. In dit geval zou vrijwel alleen in een domein opgewaardeerd NT4 gebeuren.
- `Left([sAMAccountName], 4) = "AAD_"`, `Left([sAMAccountName], 5) = "MSOL_"`. De serviceaccount die door synchronisatie Azure AD verbinding maken en de eerdere versies worden niet gesynchroniseerd.
- Niet synchroniseren met Exchange-accounts die niet in Exchange Online werkt.
    - `[sAMAccountName] = "SUPPORT_388945a0"`
    - `Left([mailNickname], 14) = "SystemMailbox{"`
    - `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`
    - `(Left([sAMAccountName], 4) = "CAS_" && (InStr([sAMAccountName], "}")> 0))`
- Objecten die niet met Exchange Online werkt niet gesynchroniseerd.
`CBool(IIF(IsPresent([msExchRecipientTypeDetails]),BitAnd([msExchRecipientTypeDetails],&H21C07000) > 0,NULL))`  
Dit bitmasker (& H21C07000) zou filteren om de volgende objecten:
    - E-mailadres van openbare map
    - System Attendant-postvak
    - Mailbox Database Mailbox (Postvak)
    - Universele groep (wouldn't toepassen voor een gebruiker, maar omwille van de oude aanwezig is)
    - Niet-universele groep (wouldn't toepassen voor een gebruiker, maar omwille van de oude aanwezig is)
    - Postbus Plan
    - Discovery-postbus
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Een slachtoffer replicatieobjecten niet gesynchroniseerd.

De regels van de volgende kenmerken van toepassing:

- `sourceAnchor <- IIF([msExchRecipientTypeDetails]=2,NULL,..)`. Het kenmerk sourceAnchor wordt een gekoppelde postbus niet bijgedragen. Er wordt aangenomen dat als een gekoppelde postbus is gevonden, de werkelijke account lid is later.
- Exchange verwante kenmerken worden alleen gesynchroniseerd als u een waarde van het kenmerk **mailNickName** heeft.
- Als er meerdere forests, worden de kenmerken verbruikt in de volgende volgorde:
    1. Het forest met een ingeschakelde account zijn kenmerken die betrekking hebben op-in (bijvoorbeeld userPrincipalName) bijgedragen.
    2. Kenmerken die kunnen worden gevonden in een Exchange-GAL (Global Address List) worden van het forest met een Exchange-postbus bijgedragen.
    3. Als er geen postvak kan worden gevonden, kunnen deze kenmerken komen uit een forest.
    4. Exchange verwante kenmerken (technische kenmerken niet zichtbaar in de GAL) worden aangeleverd in het forest waar `mailNickname ISNOTNULL`.
    5. Als er meerdere forests die zou voldoen aan deze regels, wordt de order maken (datum en tijd) van de verbindingslijnen (forests) gebruikt om te bepalen welke forest draagt de kenmerken.

### <a name="contact-out-of-box-rules"></a>Neem contact op met kant-en-klare regels
Contact-object moet voldoen aan de volgende worden gesynchroniseerd:

- De contactpersoon moet worden ingeschakeld voor e-mail. Wordt gecontroleerd of dit met de volgende regels:
    - `IsPresent([proxyAddresses]) = True)`. Het kenmerk proxyAddresses moet worden ingevuld.
    - Een primair e-mailadres kan worden gevonden in het kenmerk proxyAddresses of het e-mailkenmerk. De aanwezigheid van een @ wordt gebruikt om te controleren of de inhoud is een e-mailadres. Een van deze twee regels moet worden beoordeeld op True.
        - `(Contains([proxyAddresses], "SMTP:") > 0) && (InStr(Item([proxyAddresses], Contains([proxyAddresses], "SMTP:")), "@") > 0))`. Is er een post met ' SMTP: "en als er is, kan een @ vindt u in de tekenreeks?
        - `(IsPresent([mail]) = True && (InStr([mail], "@") > 0)`. Het e-mailkenmerk gevuld is en als het is, kunt een @ in de tekenreeks worden gevonden?

De volgende contactpersoon objecten zijn **niet** gesynchroniseerd met Azure AD:

- `IsPresent([isCriticalSystemObject])`. Ervoor te zorgen geen contactpersoonobjecten gemarkeerd als kritiek worden gesynchroniseerd. Hoeft niet elk met een standaardconfiguratie.
- `((InStr([displayName], "(MSOL)") > 0) && (CBool([msExchHideFromAddressLists])))`.
- `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`. Deze objecten goed niet in Exchange Online.
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Een slachtoffer replicatieobjecten niet gesynchroniseerd.

### <a name="group-out-of-box-rules"></a>Groep kant-en-klare regels
Een groepsobject moet voldoen aan de volgende worden gesynchroniseerd:

- Minder dan 50.000 leden hebt. Deze waarde is het aantal leden in de groep van gebouwen.
    - Als er meer leden voordat de synchronisatie voor het eerst start, wordt de groep niet gesynchroniseerd.
    - Als het aantal leden groeit uit als het is in eerste instantie gemaakt, vervolgens wanneer het 50.000 leden bereikt wordt gestopt totdat het aantal lidmaatschap lager dan 50.000 opnieuw is synchroniseren.
    - Opmerking: Het aantal 50.000 lidmaatschap wordt ook afgedwongen door AD Azure. U bent geen groepen synchroniseren met meer leden, zelfs als u wijzigen of verwijderen van deze regel.
- Als de groep een **Distributiegroep**is, moet deze ook e-mailberichten ingeschakeld zijn. Zie [Contact kant-en-klare regels](#contact-out-of-box-rules) voor deze regel wordt toegepast.

De volgende groepsobjecten zijn **niet** gesynchroniseerd met Azure AD:

- `IsPresent([isCriticalSystemObject])`. Veel kant-en-klare objecten in Active Directory, zoals de ingebouwde groep administrators te waarborgen, worden niet gesynchroniseerd.
- `[sAMAccountName] = "MSOL_AD_Sync_RichCoexistence"`. Oude groep gebruikt door DirSync.
- `BitAnd([msExchRecipientTypeDetails],&amp;H40000000)`. Rol-groep.
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Een slachtoffer replicatieobjecten niet gesynchroniseerd.

### <a name="foreignsecurityprincipal-out-of-box-rules"></a>ForeignSecurityPrincipal kant-en-klare regels
FSP's zijn toegevoegd aan de 'een' (\*)-object in de metaverse. In feite gebeurt dit join alleen voor gebruikers en beveiligingsgroepen. Deze configuratie zorgt ervoor dat verschillende forests lidmaatschappen zijn opgelost correct weergegeven in Azure AD.

### <a name="computer-out-of-box-rules"></a>Computer kant-en-klare regels
Een computerobject moet voldoen aan de volgende worden gesynchroniseerd:

- `userCertificate ISNOTNULL`. Alleen computers met Windows 10 vullen dit kenmerk. Alle computerobjecten met een waarde in dit kenmerk worden gesynchroniseerd.

## <a name="understanding-the-out-of-box-rules-scenario"></a>Wat zijn de regels van de out-of-box-scenario
In dit voorbeeld gebruiken we een implementatie met één accountforest (A), een bronforest (R) en één Azure AD-directory.

![Afbeelding met scenario-beschrijving](./media/active-directory-aadconnectsync-understanding-default-configuration/scenario.png)

In deze configuratie wordt ervan uitgegaan dat er een account worden ingeschakeld in het accountforest en een uitgeschakelde account in het forest met de bronnen met een gekoppelde postbus is.

Het is ons doel met de standaard-configuratie:

- Kenmerken die verband houden met het aanmelden worden in het forest met het ingeschakelde account gesynchroniseerd.
- Kenmerken die kunnen worden gevonden in de algemene Adreslijst (Global Address List) worden van het forest met het postvak gesynchroniseerd. Als u geen postvak kan worden gevonden, wordt een andere forest gebruikt.
- Als een gekoppelde postbus wordt gevonden, moet de gekoppelde account worden ingeschakeld voor het object worden geëxporteerd naar Azure AD worden gevonden.

### <a name="synchronization-rule-editor"></a>Regeleditor synchronisatie
De configuratie kan worden bekeken en gewijzigd met de knop synchronisatie regels Editor (SRE) en een snelkoppeling kunt u vinden in het startmenu.

![Synchronisatie regeleditor pictogram](./media/active-directory-aadconnectsync-understanding-default-configuration/sre.png)

Het SRE is een resource kit-hulpprogramma en met Azure AD verbinden synchronisatie is geïnstalleerd. Om te kunnen starten, moet u lid zijn van de groep ADSyncAdmins. Wanneer deze wordt gestart, ziet er ongeveer als volgt:

![Binnenkomende synchronisatieregels voor](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

In dit deelvenster ziet u alle synchronisatieregels gemaakt voor uw configuratie. Elke regel in de tabel is één regel voor synchronisatie. Links onder regeltypen, de twee verschillende typen worden vermeld: inkomend en uitgaand. Inkomend en uitgaand van de weergave van de metaverse is. U gaat voornamelijk concentreren op de binnenkomende regels in dit overzicht. De feitelijke lijst met regels voor synchronisatie is afhankelijk van de gedetecteerde schema in AD. In de afbeelding, het accountforest (fabrikamonline.com) heeft geen services, zoals Exchange en Lync, en geen synchronisatieregels voor deze services zijn gemaakt. Echter in het bronforest (res.fabrikamonline.com) vindt u synchronisatieregels voor deze services. De inhoud van de regels is afhankelijk van de versie gevonden. Bijvoorbeeld in een implementatie met Exchange 2013 zijn er meer kenmerk stromen dan geconfigureerd in Exchange 2010/2007.

### <a name="synchronization-rule"></a>Regel voor synchronisatie
Een synchronisatie-regel is een configuratieobject met een set kenmerken die als een voorwaarde is voldaan. Ook wordt gebruikt om te beschrijven hoe een object in de ruimte van een verbindingslijn is gerelateerd aan een object in de metaverse, **join** of **overeenkomt met**genoemd. De synchronisatieregels hebben een hogere prioriteitswaarde die aangeeft hoe ze aan elkaar gerelateerd. Een regel voor synchronisatie met een lager getal heeft een hogere prioriteit en een kenmerk stroom tegenstrijdig hogere prioriteit wint de conflictoplossing.

Bekijk als voorbeeld de synchronisatie regel **In van AD-AccountEnabled van de gebruiker**. Deze regel in het SRE markeren en selecteer **bewerken**.

Aangezien deze regel een out-of-box-regel is, ontvangt u een waarschuwing bij het openen van de regel. U maakt [wijzigingen aan kant-en-klare regels](active-directory-aadconnectsync-best-practices-changing-default-configuration.md), zodat u zeker weet wat uw bedoelingen zijn. In dit geval wilt u alleen de regel weergeven. Klik op **Nee**.

![Synchronisatie regels waarschuwing](./media/active-directory-aadconnectsync-understanding-default-configuration/warningeditrule.png)

Een regel synchronisatie heeft vier configuratiesecties: Beschrijving van scopes filter, Join regels en transformaties.

#### <a name="description"></a>Beschrijving
De eerste sectie bevat basisinformatie, zoals een naam en beschrijving.

![Beschrijving tab synchroon regeleditor ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruledescription.png)

Ook vindt u informatie over welk systeem verbonden deze regel is gekoppeld, die-type in het verbonden systeem dat is van toepassing object op het objecttype metaverse. Het objecttype van de metaverse is altijd onafhankelijk persoon wanneer het objecttype van de bron een gebruiker, contactpersoon of iNetOrgPerson is. Het objecttype metaverse moet nooit wijzigen zodat deze als een generiek type wordt gemaakt. Het koppelingstype kan worden ingesteld op Join, StickyJoin of voorziening. Deze instelling werkt samen met de sectie regels samenvoegen en later valt.

U kunt ook zien dat deze regel sync wordt gebruikt voor synchronisatie met wachtwoord. Als een gebruiker in het bereik voor deze regel sync, het wachtwoord gesynchroniseerd op ruimten cloud (ervan uitgaande dat u de functie wachtwoord synchronisatie hebt ingeschakeld).

#### <a name="scoping-filter"></a>Scoping filter
De sectie Filter Scoping wordt gebruikt om te configureren wanneer u een synchronisatie-regel geldt. Omdat de naam van de regel van de synchronisatie wordt weergegeven moet slechts worden toegepast voor de ingeschakelde gebruikers geeft, de scope is geconfigureerd, zodat de AD kenmerk **userAccountControl** moet de bit 2 niet hebt ingesteld. Als de synchronisatie-engine vindt een gebruiker in AD, geldt deze regel sync wanneer **userAccountControl** is ingesteld op de decimale waarde 512 (normale gebruiker ingeschakeld). De regel is niet van toepassing wanneer de gebruiker **userAccountControl** is ingesteld op 514 (normale gebruiker uitgeschakeld heeft).

![Tabblad in Sync regeleditor scoping ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilter.png)

Het filter scope heeft groepen en componenten die kunnen worden genest. Alle componenten in een groep moeten worden voldaan voor een synchronisatie-regel toe te passen. Wanneer meerdere groepen zijn gedefinieerd, moet ten minste één groep worden voldaan voor de regel wilt toepassen. Dat wil zeggen, een logische OR tussen groepen en een logische wordt geëvalueerd en wordt geëvalueerd binnen een groep. Een voorbeeld van deze configuratie vindt u in de uitgaande regel synchronisatie- **Out naar AAD – lid van de groep**. Er zijn verschillende synchronisatie filter groepen, bijvoorbeeld voor beveiligingsgroepen (`securityEnabled EQUAL True`) en één voor distributiegroepen (`securityEnabled EQUAL False`).

![Tabblad in Sync regeleditor scoping ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilterout.png)

Deze regel wordt gebruikt om te bepalen welke groepen moeten worden ingericht naar Azure AD. Distributiegroepen e-mailberichten moeten worden gesynchroniseerd met Azure Active Directory ingeschakeld moeten zijn, maar voor beveiligingsgroepen een e-mailbericht is niet vereist.

#### <a name="join-rules"></a>Deelnemen aan de regels
Het derde gedeelte wordt gebruikt om te configureren hoe objecten in de ruimte connector betrekking hebben op objecten in de metaverse. De regel die u eerder hebt bekeken is geen configuratie voor deelnemen aan regels dus in plaats daarvan gaat u te kijken naar **In uit Active Directory: gebruikers deelnemen aan**.

![Deelnemen aan de regeleditor synchronisatie tabblad regels ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulejoinrules.png)

De inhoud van de join-regel is afhankelijk van de overeenkomende optie is geselecteerd in de wizard installeren. De evaluatie wordt gestart met een object in de kleurruimte van de connector voor een binnenkomende regel en elke groep in de join-regels in de reeks wordt geëvalueerd. Als een bronobject aan precies één object in de metaverse met behulp van een van de regels van de join wordt geëvalueerd, worden de objecten gekoppeld. Als u alle regels hebt is geëvalueerd en er is geen overeenkomst, wordt het koppelingstype op de pagina beschrijving gebruikt. Als deze configuratie is ingesteld om in te **richten**, wordt een nieuw object gemaakt in het doel, de metaverse. Om het inrichten van een nieuw object aan de metaverse is ook bekend als **Project** een object aan de metaverse.

De join-regels worden slechts eenmaal geëvalueerd. Als een verbindingslijn ruimte-object en een object metaverse worden gekoppeld, blijven ze verbonden als het bereik van de synchronisatie-regel nog steeds tevreden is.

Bij de beoordeling van de regels voor synchronisatie, moet slechts één regel voor synchronisatie met join regels die zijn gedefinieerd in het bereik. Als u meerdere regels voor synchronisatie met join regels voor één object worden gevonden, wordt een fout gegenereerd. Daarom is de beste manier om slechts één regel voor synchronisatie met join wanneer er meerdere regels voor synchronisatie in het bereik van een object worden gedefinieerd. In de configuratie van de out-of-box voor Azure AD verbinden sync, deze regels worden gevonden door te kijken naar de naam en die het woord **toevoegen** aan het einde van de naam. Stromen van het kenmerk is van toepassing een synchronisatie regel zonder join-regels die zijn gedefinieerd als een andere regel voor synchronisatie objecten worden samengevoegd of een nieuw object in het doel ingericht.

Als u de bovenstaande afbeelding bekijkt, ziet u dat de regel probeert deel te nemen aan **objectSID** met **msExchMasterAccountSid** (Exchange) en **msRTCSIP OriginatorSid** (Lync), dat is wat we verwachten in een topologie rekening bron-forest. Vindt u dezelfde regel op alle forests. De veronderstelling is dat elk forest kan een account of een bron-forest. Deze configuratie werkt ook als u accounts die live in een forest en hoeft niet te worden gekoppeld.

#### <a name="transformations"></a>Transformaties
De sectie transformatie definieert alle kenmerk stromen die van toepassing zijn wanneer de objecten worden samengevoegd en de Scopefilter is voldaan aan het doelobject. Teruggaan naar de **In uit Active Directory-gebruiker AccountEnabled** synchronisatie in de regel vindt u de volgende transformaties:

![Transformaties tabblad synchroon regeleditor ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruletransformations.png)

Om deze configuratie in de context in een implementatie rekening bron-forest naar verwachting een ingeschakelde om rekening te vinden in het accountforest en een uitgeschakelde account in het bronforest met instellingen voor Exchange en Lync. De synchronisatie-regel die u bekijkt bevat de kenmerken die vereist zijn voor het aanmelden en deze kenmerken van het forest moeten de stroming waarbij een account ingeschakeld. Alle deze kenmerk stromen worden samengesteld in een regel voor synchronisatie.

Een transformatie kan verschillende typen hebben: constante, Direct en expressie.

- Een constante stroom loopt altijd een vastgelegde waarde. In dit geval altijd wordt de waarde **True** in de metaverse kenmerk met de naam **accountEnabled**.
- Een directe stroom loopt altijd de waarde van het kenmerk in de gegevensbron met het kenmerk target als-is.
- Het derde stroomtype is de expressie en maakt het mogelijk geavanceerde configuraties.

De taal voor sjabloonexpressies is VBA (Visual Basic for Applications), zodat mensen met ervaring van Microsoft Office of VBScript de indeling wordt herkend. Kenmerken worden tussen vierkante haken, [attributeName]. Namen en functienamen zijn hoofdlettergevoelig, maar de regeleditor synchronisatie worden de expressies geëvalueerd en een waarschuwing geven als de expressie is niet geldig. Alle expressies worden op één regel met geneste functies uitgedrukt. Om weer te geven in de kracht van de taal van de configuratie, is de stroom voor pwdLastSet, maar met extra opmerkingen ingevoegd:

```
// If-then-else
IIF(
// (The evaluation for IIF) Is the attribute pwdLastSet present in AD?
IsPresent([pwdLastSet]),
// (The True part of IIF) If it is, then from right to left, convert the AD time format to a .Net datetime, change it to the time format used by Azure AD, and finally convert it to a string.
CStr(FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")),
// (The False part of IIF) Nothing to contribute
NULL
)
```

Zie [Wat declaratieve inrichten van expressies](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) voor meer informatie over de taal voor sjabloonexpressies voor kenmerk stromen.

### <a name="precedence"></a>Prioriteit
U hebt nu enkele afzonderlijke regels voor synchronisatie hebt gekeken, maar de regels werken samen in de configuratie. In sommige gevallen wordt een waarde van het kenmerk uit meerdere synchronisatieregels voor bijgedragen tot hetzelfde kenmerk target. In dit geval wordt voorrang kenmerk gebruikt om te bepalen welk kenmerk wint. Als u bijvoorbeeld het kenmerk sourceAnchor bekijken. Dit kenmerk is een belangrijk kenmerk om te kunnen melden bij AD Azure. Vindt u een kenmerk stroom voor dit kenmerk in twee verschillende synchronisatieregels **In uit Active Directory-gebruiker AccountEnabled** en **In uit Active Directory-gebruiker gemeenschappelijke**. Door de prioriteit van de regels voor synchronisatie, wordt het kenmerk sourceAnchor bijgedragen het forest met een ingeschakelde account eerst als er meerdere objecten zijn toegevoegd aan het object metaverse. Als er geen accounts ingeschakeld zijn, wordt de synchronisatie-engine maakt gebruik van de catch-all synchronisatie regel **In uit Active Directory-gebruiker gemeenschappelijke**. Deze configuratie zorgt ook voor accounts die zijn uitgeschakeld, nog steeds een sourceAnchor.

![Binnenkomende synchronisatieregels voor](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

De prioriteit van regels voor synchronisatie wordt ingesteld in groepen met de installatiewizard. Alle regels in een groep dezelfde naam hebben, maar ze zijn verbonden met verschillende gekoppelde mappen. De installatiewizard geeft de regel **In uit Active Directory-gebruiker toevoegen** hoogste prioriteit en het itereert over alle aangesloten AD-mappen. Vervolgens wordt verder met de volgende groepen regels in een vooraf gedefinieerde volgorde. In een groep, worden de regels in de volgorde waarin die de verbindingslijnen zijn toegevoegd aan de wizard toegevoegd. Als een andere verbindingslijn wordt toegevoegd met behulp van de wizard, de synchronisatieregels worden opnieuw gerangschikt en regels van de nieuwe verbindingslijn laatst in elke groep worden ingevoegd.

### <a name="putting-it-all-together"></a>Alles samenbrengen
We nu weten voldoende over synchronisatieregels om te kunnen begrijpen hoe de configuratie werkt met de verschillende regels voor synchronisatie. Als u een gebruiker en de kenmerken die zijn bijgedragen aan de metaverse bekijkt, worden de regels in de volgende volgorde toegepast:

Naam | Opmerking
:------------- | :-------------
In uit Active Directory-gebruiker Join | De regel voor het lidmaatschap van de connector ruimteobjecten met metaverse.
In uit Active Directory-gebruikersaccount ingeschakeld | Kenmerken die vereist zijn voor het aanmelden bij AD Azure en Office 365. Wij willen deze kenmerken van de account ingeschakeld.
In uit Active Directory – gemeenschappelijke gebruiker van Exchange | Kenmerken gevonden in de algemene adreslijst. We ervan uitgaan dat de kwaliteit van de gegevens is het meest geschikt in het forest waar we de postbus van gebruiker hebt gevonden.
In uit Active Directory-gebruiker gemeenschappelijke | Kenmerken gevonden in de algemene adreslijst. In het geval er een postvak gevonden, kan andere gekoppelde object de waarde van het kenmerk bijdragen.
In uit Active Directory-gebruiker Exchange | Bestaat alleen als Exchange is aangetroffen. Deze loopt door alle kenmerken van de infrastructuur voor Exchange.
In uit Active Directory-gebruiker Lync | Is alleen aanwezig als Lync is aangetroffen. Deze loopt door alle kenmerken van de infrastructuur Lync.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het configuratiemodel in [Wat declaratieve inrichten](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
- Lees meer over de taal voor sjabloonexpressies in [Wat declaratieve inrichten van expressies](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).
- Gaan met de werking van de out-of-box configuratie in [wat gebruikers en contactpersonen](active-directory-aadconnectsync-understanding-users-and-contacts.md)
- Zie praktische wijzigen met behulp van declaratieve ingericht in [het maken van een wijziging in de standaardconfiguratie](active-directory-aadconnectsync-change-the-configuration.md).

**Van overzichtsonderwerpen**

- [Azure AD verbinden sync: begrijpen en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
- [Integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md)
