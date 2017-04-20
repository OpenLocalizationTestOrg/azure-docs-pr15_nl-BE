<properties
    pageTitle="Azure AD verbinden sync: filtering configureren | Microsoft Azure"
    description="Hoe filters synchroon Azure AD verbinden configureren."
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
    ms.date="09/13/2016"
    ms.author="andkjell;markvi"/>


# <a name="azure-ad-connect-sync-configure-filtering"></a>Azure AD verbinden sync: Filtering configureren
Met filteren kunt u bepalen welke objecten moeten worden weergegeven in Azure AD uit de map op locatie. De standaardconfiguratie neemt alle objecten in alle domeinen van de geconfigureerde forests. In het algemeen, is dit de aanbevolen configuratie. Met Office 365, werkbelasting zoals Exchange Online en Skype voor bedrijven, eindgebruikers profiteren van een volledige algemene adreslijst zodat ze kunnen e-mail verzenden en iedereen. Ze zou dezelfde ervaring die met een op het bedrijf de implementatie van Exchange of Lync zouden krijgen met de standaardconfiguratie.

In sommige gevallen is het bepaalde wijzigingen aanbrengen in de standaardconfiguratie vereist. Hier volgen enkele voorbeelden:

- U wilt gebruiken de [AD-directory-topologie met meerdere Azure](active-directory-aadconnect-topologies.md#each-object-only-once-in-an-azure-ad-directory). Moet u een filter toepassen om te bepalen welk object moet worden gesynchroniseerd naar een bepaalde directory AD Azure.
- Uitvoeren van een proefproject voor Azure of Office 365 en wilt u alleen een subset van gebruikers in Azure AD. In de kleine leider is niet belangrijk dat u een volledige algemene adreslijst te demonstreren de functies.
- U hebt veel serviceaccounts en andere niet-persoonlijke accounts die u niet dat in Azure AD wilt.
- Om redenen van compatibiliteit u niet iedere gebruiker accounts op-ruimte verwijdert. U alleen uitschakelen. Maar in Azure AD alleen actieve accounts aanwezig te zijn.

In dit artikel wordt beschreven hoe u de verschillende filtermethoden configureren.

> [AZURE.IMPORTANT]Microsoft biedt geen ondersteuning voor wijziging of de exploitatie van de synchronisatie Azure AD verbinden buiten die formeel beschreven acties. Een van deze acties kan leiden tot een inconsistente of niet-ondersteunde status van synchronisatie Azure AD verbinden en daardoor Microsoft biedt geen technische ondersteuning voor dergelijke implementaties.

## <a name="basics-and-important-notes"></a>Basisbegrippen en belangrijke notities
Azure AD verbinden te synchroniseren kunt u filteren op elk gewenst moment. Als u met een standaardconfiguratie van adreslijstsynchronisatie begint en filtering configureren, worden de objecten die worden uitgefilterd niet meer gesynchroniseerd naar Azure AD. Als gevolg hiervan zijn in Azure AD objecten in Azure AD die eerder zijn gesynchroniseerd, maar werden vervolgens gefilterd verwijderd.

Voordat u begint met het maken van wijzigingen te filteren, moet u ervoor zorgen u [de geplande taak uitschakelen](#disable-scheduled-task) zodat u niet per ongeluk wijzigingen die u nog niet hebt gecontroleerd om juist exporteren.

Aangezien filtering veel objecten tegelijkertijd verwijderen kan, die u wilt controleren of dat uw nieuwe filters juist zijn voordat u de wijzigingen exporteren naar Azure AD. Nadat u de configuratieprocedures hebt voltooid, is het raadzaam de [controle stappen](#apply-and-verify-changes) te volgen voordat u wijzigingen in Azure AD aanbrengt en exporteren.

Als u wilt voorkomen dat u veel objecten per ongeluk verwijderen, is de functie [niet per ongeluk verwijderen](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) standaard ingeschakeld. Als u veel objecten door filteren (500 standaard) verwijdert, moet u de stappen in dit artikel om de verwijderde items doorlopen naar Azure AD.

Als u een build voordat November 2015 ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)), een wijziging aanbrengt in de configuratie van de filter en u Wachtwoordsynchronisatie gebruikt, moet u een volledige synchronisatie van alle wachtwoorden activeren nadat u de configuratie hebt voltooid. Volledige synchronisatie Zie [Trigger een volledige synchronisatie van alle wachtwoorden](active-directory-aadconnectsync-implement-password-synchronization.md#trigger-a-full-sync-of-all-passwords)voor stapsgewijze instructies voor het activeren van een wachtwoord. Als u op 1.0.9125 of hoger, vervolgens berekend de actie regelmatig **volledige synchronisatie** ook als wachtwoorden moeten worden gesynchroniseerd en dit een extra stap vereist is.

Als **user** -objecten zijn in Azure AD per ongeluk verwijderd vanwege een fout filteren, kunt u de gebruikersobjecten opnieuw in Azure AD door het verwijderen van uw configuraties filteren en vervolgens de mappen opnieuw synchroniseren. Deze actie wordt de gebruikers uit de Prullenbak hersteld in Azure AD. U kunt geen echter andere objecttypen ongedaan. Worden bijvoorbeeld als u per ongeluk een groep verwijdert en het is gebruikt voor de Toegangsbeheerlijst van de groep en de ACL's van een resource kunnen niet hersteld.

Azure AD verbinden alleen objecten die eenmaal valt deze in de scope worden verwijderd. Er zijn objecten die zijn gemaakt door een andere synchronisatie-engine in Azure AD en deze objecten niet in het bereik, toe te voegen filtering doen niet verwijderen. Bijvoorbeeld als u met een server DirSync Start en deze een kopie van uw gehele map in Azure AD gemaakt en u een nieuwe Azure AD verbinden synchronisatieserver parallel installeert met het filteren ingeschakeld vanaf het begin, verwijdert het niet de extra objecten die zijn gemaakt door DirSync.

De configuratie van de filtering behouden bij het installeren of upgraden naar een nieuwere versie van Azure AD verbinden. Is altijd het beste om te controleren of de configuratie is niet per ongeluk gewijzigd na een upgrade naar een nieuwere versie voordat u de synchronisatie van de eerste cyclus.

Als er meer dan één forest, moeten de filtering configuraties beschreven in dit onderwerp worden toegepast op elk forest (ervan uitgaande dat u wilt dat dezelfde configuratie voor al deze).

### <a name="disable-scheduled-task"></a>Uitschakelen, geplande taak
Als u wilt uitschakelen van de ingebouwde scheduler die een synchronisatie cyclus elke 30 minuten activeert, als volgt te werk:

1. Ga naar een PowerShell-prompt.
2. Uitvoeren van `Set-ADSyncScheduler -SyncCycleEnabled $False` de Taakplanner uitschakelen.
3. Breng de wijzigingen zoals beschreven in dit onderwerp.
4. Uitvoeren van `Set-ADSyncScheduler -SyncCycleEnabled $True` de scheduler opnieuw inschakelen.

**Als u een build Azure AD verbinden voor 1.1.105.0**  
De geplande taak die een cyclus synchronisatie activeert uitschakelen om 3 uur als volgt:

1. **Taakplanner** starten vanuit het startmenu.
2. Zoek direct onder **Bibliotheek voor Taakplanner**, de taak met de naam **Azure AD Sync Scheduler**, klik met de rechtermuisknop en selecteer **uitschakelen**.  
![Taakplanner](./media/active-directory-aadconnectsync-configure-filtering/taskscheduler.png)  
3. U kunt nu configuratiewijzigingen aanbrengen en de motor van de synchronisatie handmatig uitvoeren vanuit de **synchronisatie servicemanager** -console.

Nadat u uw filter wijzigingen hebt voltooid, moet u niet vergeten om te komen terug en **schakelen** de taak opnieuw.

## <a name="filtering-options"></a>Filteropties
De volgende typen filters configuratie kunnen worden toegepast op de Directory Synchronization tool:

- [**Op basis van groep**](active-directory-aadconnect-get-started-custom.md#sync-filtering-based-on-groups): filteren op basis van een groep kan alleen worden geconfigureerd op de oorspronkelijke installatie met de wizard installeren. Het is verder niet beschreven in dit onderwerp.

- [**Op basis van een domein**](#domain-based-filtering): met deze optie kunt u selecteren welke domeinen die met AD Azure synchroniseren. Ook kunt u toevoegen en verwijderen van domeinen uit de configuratie synchronisatie-engine als u wijzigingen in uw infrastructuur op gebouwen aanbrengt na de installatie van Azure AD verbinden sync.

- [**Op basis van organisatie-eenheid**](#organizational-unitbased-filtering): dit filteroptie kunt u selecteren welke organisatie-eenheden naar Azure AD synchroniseren. Deze optie is voor alle objecttypen in de geselecteerde organisatie-eenheden.

- [**Op basis van kenmerk**](#attribute-based-filtering): met deze optie kunt u filteren op basis van waarden van kenmerken op de objecten objecten. U kunt ook verschillende filters voor verschillende objecttypen hebben.

Tegelijkertijd kunt u meerdere opties voor het filter. Bijvoorbeeld, kunt u filteren op basis van de organisatie-eenheid met alleen objecten in één organisatie-eenheid en op dezelfde tijd op basis van het kenmerk filters om de objecten verder filteren. Wanneer u meerdere methoden voor filteren, gebruikt u de filters een logische en tussen de filters.

## <a name="domain-based-filtering"></a>Filteren op basis van een domein
Deze sectie bevat de stappen voor het configureren van het filter van uw domein. Als u hebt toegevoegd of domeinen in het forest verwijderd nadat u Azure AD Connect hebt geïnstalleerd, hebt u ook de filtering configuratie bij te werken.

Er is de beste manier om te filteren op basis van een domein wijzigen door met de installatie wizard en wijziging [domein en organisatie-eenheden filteren](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering). De installatiewizard wordt automatisch alle taken die in dit onderwerp wordt beschreven.

Volg deze stappen alleen als u om welke reden dan ook niet de installatiewizard uitvoeren.

Configuratie van filteren op basis van een domein bestaat uit volgende stappen uit:

- [Selecteer de domeinen](#select-domains-to-be-synchronized) die moeten worden opgenomen in de synchronisatie.
- Pas de [profielen worden uitgevoerd](#update-run-profiles)voor elk domein verwijderd en toegevoegd.
- [Toepassen en wijzigingen controleren](#apply-and-verify-changes).

### <a name="select-domains-to-be-synchronized"></a>Selecteer domeinen moeten worden gesynchroniseerd
**Als het domein filter instelt, moet u de volgende stappen uitvoeren:**

1. Aanmelden bij de server waarop sync Azure AD verbinding maken met een account die lid is van de groep **ADSyncAdmins** .
2. **Synchronisatie** starten vanuit het startmenu.
3. Selecteer de **verbindingslijnen** en in de lijst met **verbindingslijnen** , selecteert u de verbindingslijn met het type **Active Directory Domain Services**. **Acties**en selecteer **Eigenschappen**.  
![Connectoreigenschappen](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Klik op **mappartities configureren**.
5. Selecteer in de lijst **Selecteer de Active directory-partities** en hef de selectie van de domeinen waar nodig. Controleer of alleen de partities die u wilt synchroniseren zijn geselecteerd.  
![Partities](./media/active-directory-aadconnectsync-configure-filtering/connectorpartitions.png)  
Als u uw locatie op hebt gewijzigd AD-infrastructuur en toegevoegde of verwijderde domeinen van het forest, vervolgens klikt u op de knop **vernieuwen** om een bijgewerkte lijst. Wanneer u vernieuwt, wordt u gevraagd om referenties. Referenties lezen toegang bieden aan Active Directory op gebouwen. Het hoeft niet te worden van de vooraf ingestelde gebruiker in het dialoogvenster.  
![Vernieuwen vereist](./media/active-directory-aadconnectsync-configure-filtering/refreshneeded.png)  
6. Wanneer u klaar bent, sluit u het dialoogvenster **Eigenschappen** door te klikken op **OK**. Als u domeinen van het forest hebt verwijderd, wordt de pop-up bericht dat een domein is verwijderd en die configuratie worden opgeruimd.
7. Verder aan te passen de [profielen worden uitgevoerd](#update-run-profiles).

### <a name="update-run-profiles"></a>Update uitvoeren profielen
Als u het filter van uw domein hebt bijgewerkt, moet u ook de uitvoering profielen bijwerken.

1. Controleer of de Connector die u hebt gewijzigd in de vorige stap hebt geselecteerd in de lijst **connectoren** . Selecteer **Acties** **Uitvoeren profielen configureren**.  
![Connector profielen uitvoeren](./media/active-directory-aadconnectsync-configure-filtering/connectorrunprofiles1.png)  

U moet de volgende profielen aanpassen:

- Volledige importbewerking
- Volledige synchronisatie
- Delta-Import
- Deltasynchronisatie
- Exporteren

Voer de volgende stappen voor elk domein **toegevoegd** voor elk van de vijf profielen:

1. Selecteer het profiel uitvoeren en klik op **Nieuwe stap**.
2. Selecteer op de pagina **Stap configureren** in de vervolgkeuzelijst **Type** het staptype met dezelfde naam als het profiel dat u wilt configureren. Klik vervolgens op **volgende**.  
![Connector profielen uitvoeren](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep1.png)  
3. Selecteer de naam van het domein dat u hebt toegevoegd aan het domein filter in de vervolgkeuzelijst **partitie** op de pagina **Configuratie van Connector** .  
![Connector profielen uitvoeren](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep2.png)  
4. Het **Uitvoeren van profiel configureren** als dialoogvenster wilt sluiten, klikt u op **Voltooien**.

Voer de volgende stappen voor elk domein **verwijderd** voor elk van de vijf profielen:

1. Selecteer het profiel uitvoeren.
2. Als de **waarde** van het kenmerk **partitie** een GUID is, selecteert u de stap uitvoeren en klik op **Stap verwijderen**.  
![Connector profielen uitvoeren](./media/active-directory-aadconnectsync-configure-filtering/runprofilesdeletestep.png)  

Het resultaat moet zijn dat elk domein dat u wilt synchroniseren moet worden vermeld als een stap in elk profiel uitvoeren.

Het **Uitvoeren van profielen configureren** als dialoogvenster wilt sluiten, klikt u op **OK**.

- Voor het voltooien van de configuratie, [toepassen en wijzigingen controleren](#apply-and-verify-changes).

## <a name="organizational-unitbased-filtering"></a>Filteren op basis van organisatie-eenheid
De beste manier om te filteren op basis van de organisatie-eenheid wijzigen is door het uitvoeren van de installatie wizard en wijziging [domein en organisatie-eenheden filteren](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering). De installatiewizard wordt automatisch alle taken die in dit onderwerp wordt beschreven.

Volg deze stappen alleen als u om welke reden dan ook niet de installatiewizard uitvoeren.

**Ga de volgende stappen uit om organisatie-eenheid-gebaseerde filtering configureren:**

1. Aanmelden bij de server waarop sync Azure AD verbinding maken met een account die lid is van de groep **ADSyncAdmins** .
2. **Synchronisatie** starten vanuit het startmenu.
3. Selecteer de **verbindingslijnen** en in de lijst met **verbindingslijnen** , selecteert u de verbindingslijn met het type **Active Directory Domain Services**. **Acties**en selecteer **Eigenschappen**.  
![Connectoreigenschappen](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Klik op **Mappartities configureren**, selecteert u het domein dat u wilt configureren en klik op **Containers**.
5. Desgevraagd alle referenties met leestoegang aan Active Directory op gebouwen. Het hoeft niet te worden van de vooraf ingestelde gebruiker in het dialoogvenster.
6. Schakel in het dialoogvenster **Containers selecteren** de organisatie-eenheden die u niet wilt synchroniseren met de cloud-map en klik op **OK**.  
![ORGANISATIE-EENHEID](./media/active-directory-aadconnectsync-configure-filtering/ou.png)  
  - De container **Computers** moet zijn ingeschakeld voor uw Windows 10-computers worden gesynchroniseerd met Azure AD. Als lid van het domein computers bevinden zich in een andere organisatie-eenheden, of die zijn ingeschakeld.
  - De **ForeignSecurityPrincipals** -container moet worden geselecteerd als er meerdere forests met vertrouwensrelaties. Deze container kunt interforest-beveiligingsgroep worden omgezet.
  - De **RegisteredDevices** organisatie-eenheid moet worden geselecteerd als u het apparaat Write-back-functie hebt ingeschakeld. Als u een ander onderdeel van de Write-back, zoals groep Write-back, zorgt ervoor dat deze locaties zijn geselecteerd.
  - Selecteer een andere organisatie-eenheid waar gebruikers, iNetOrgPersons, contactpersonen, groepen en Computers zich bevinden. In de afbeelding bevinden deze zich in de ManagedObjects OU.
7. Wanneer u klaar bent, sluit u het dialoogvenster **Eigenschappen** door te klikken op **OK**.
8. Voor het voltooien van de configuratie, [toepassen en wijzigingen controleren](#apply-and-verify-changes).

## <a name="attribute-based-filtering"></a>Filteren op basis van kenmerk
Zorg ervoor dat u op de November 2015 ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)) of hoger maken voor de volgende stappen uit om te werken.

Filteren op basis van kenmerk is de meest flexibele manier om filter-objecten. U kunt de kracht van [declaratieve ingericht](active-directory-aadconnectsync-understanding-declarative-provisioning.md) bijna elk aspect van bepalen wanneer een object moet worden gesynchroniseerd naar Azure AD.

Filters worden toegepast op de [binnenkomende](#inbound-filtering) van Active Directory naar de metaverse en [uitgaande](#outbound-filtering) van de metaverse naar Azure AD. Het verdient aanbeveling toe te passen met het filteren van inkomende omdat die het gemakkelijkst te handhaven. Uitgaande filters moet alleen worden gebruikt indien nodig deel te nemen aan objecten van meer dan één forest voordat de beoordeling kan plaatsvinden.

### <a name="inbound-filtering"></a>Binnenkomende filters
Inkomende gebaseerde filtering maakt gebruik van de standaardconfiguratie waar objecten naar Azure AD moet de metaverse kenmerk cloudFiltered niet instellen op een waarde moet worden gesynchroniseerd. Als de waarde van dit kenmerk is ingesteld op **True**, wordt niet het object gesynchroniseerd. Deze moet niet worden ingesteld op **False** in het ontwerp. Om te zorgen dat andere regels hebben de mogelijkheid om bij te dragen een waarde, dit kenmerk is alleen moet zijn de waarden **waar** of **NULL** (niet aanwezig).

In de inkomende filtering gebruikt u de kracht van de **scope** om te bepalen welke objecten moeten worden of niet worden gesynchroniseerd. Dit is waar u aanpassingen om te voldoen aan de vereisten van uw eigen organisatie maken. De module scope heeft **groep** en de **component** om te bepalen of een synchronisatie-regel in het bereik moet. Een **groep** bevat een of meer **component**. Er is een logische en tussen meerdere componenten en een logische of tussen meerdere groepen.

Laten we kijken naar een voorbeeld:  
![Scope](./media/active-directory-aadconnectsync-configure-filtering/scope.png) moet dit worden gelezen als **(afdeling = IT) of (afdeling = verkoop en c = US)**.

Het user-object als u een voorbeeld in de monsters en de volgende stappen uit, maar u kunt dit gebruiken voor alle objecttypen.

Start met 500 de prioriteit in de onderstaande voorbeelden. Deze waarde zorgt voor dat deze regels na de out-of-box-regels (lagere prioriteit, hogere numerieke waarde) worden geëvalueerd.

#### <a name="negative-filtering-do-not-sync-these"></a>Filteren, 'Synchroniseer niet deze' negatieve
In het volgende voorbeeld, u filteren (niet synchroniseren) alle gebruikers waar **extensionAttribute15** hebben de waarde **NoSync**.

1. Aanmelden bij de server waarop sync Azure AD verbinding maken met een account die lid is van de groep **ADSyncAdmins** .
2. **Regeleditor synchronisatie** starten vanuit het startmenu.
3. Zorg **dat voor binnenkomend verkeer** is geselecteerd en klik op **Nieuwe regel toevoegen**.
4. Geef de regel een beschrijvende naam, zoals "*In uit Active Directory-gebruiker DoNotSyncFilter*". Selecteer het **type MV-object**het juiste forest, de **gebruiker** als het **objecttype CS**en de **persoon** . Selecteer **koppelen** als **Type koppeling**, en prioriteit typt u een waarde op dit moment niet worden gebruikt door een andere synchronisatie-regel (bijvoorbeeld 500) en klik op **volgende**.  
![Inkomende 1 Beschrijving](./media/active-directory-aadconnectsync-configure-filtering/inbound1.png)  
5. In **Scoping filter**, klikt u op **Groep toevoegen**, klik op **Component toevoegen**en selecteer **ExtensionAttribute15**in kenmerk. Controleer of dat de Operator is ingesteld op **gelijk** en typt u de waarde **NoSync** in het vak waarde. Klik op **volgende**.  
![Inkomende bereik 2](./media/active-directory-aadconnectsync-configure-filtering/inbound2.png)  
6. Niets in de regels voor het **deelnemen aan** en klik vervolgens op **volgende**.
7. Klik op **Transformatie toevoegen**de **FlowType** aan een **constante**, selecteert u het kenmerk Target **cloudFiltered** en selecteer in het vak bron typt u **True**. Klik op **toevoegen** om de regel opslaan.  
![Inkomende 3-transformatie](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)
8. Voor het voltooien van de configuratie, [toepassen en wijzigingen controleren](#apply-and-verify-changes).

#### <a name="positive-filtering-only-sync-these"></a>Filteren, 'alleen synchroniseren deze' positief
Uitdrukken positieve filteren worden veeleisender omdat u ook rekening houden met objecten die niet worden gesynchroniseerd moet, zoals vergaderruimten voor de hand liggende.

De positieve filteroptie vereist twee regels voor synchronisatie. Een (of meer) met het juiste bereik van objecten die u wilt synchroniseren en een tweede regel afvangen sync dat filter op alle objecten die nog niet is geïdentificeerd als een object dat moet worden gesynchroniseerd.

In het volgende voorbeeld wordt synchroniseren u alleen gebruikersobjecten, waarbij het kenmerk afdeling de waarde **verkoop**heeft.

1. Aanmelden bij de server waarop sync Azure AD verbinding maken met een account die lid is van de groep **ADSyncAdmins** .
2. **Regeleditor synchronisatie** starten vanuit het startmenu.
3. Zorg **dat voor binnenkomend verkeer** is geselecteerd en klik op **Nieuwe regel toevoegen**.
4. Geef de regel een beschrijvende naam, bijvoorbeeld '*In uit Active Directory-gebruiker verkoop synchroniseren*'. Selecteer het **type MV-object**het juiste forest, de **gebruiker** als het **objecttype CS**en de **persoon** . Selecteer **koppelen** als **Type koppeling**, en prioriteit typt u een waarde op dit moment niet worden gebruikt door een andere synchronisatie-regel (bijvoorbeeld 501) en klik op **volgende**.  
![Inkomende 4 beschrijving](./media/active-directory-aadconnectsync-configure-filtering/inbound4.png)  
5. In **Scoping filter**, klikt u op **Groep toevoegen**, klik op **Component toevoegen**en selecteer **afdeling**in het kenmerk. Controleer of dat de Operator is ingesteld op **gelijk** en typ de waarde **verkoop** in het vak waarde. Klik op **volgende**.  
![Inkomende scope 5](./media/active-directory-aadconnectsync-configure-filtering/inbound5.png)  
6. Niets in de regels voor het **deelnemen aan** en klik vervolgens op **volgende**.
7. Klik op **Transformatie toevoegen**de **FlowType** aan een **constante**, selecteert u het kenmerk Target **cloudFiltered** en selecteer in het vak bron tekst typt u **False**. Klik op **toevoegen** om de regel opslaan.  
![Inkomende 6-transformatie](./media/active-directory-aadconnectsync-configure-filtering/inbound6.png)  
Dit is een speciaal geval waarbij u cloudFiltered expliciet op False instellen.

    We hebben nu de catch-all sync-regel maken.

8. Geef de regel een beschrijvende naam, zoals "*In uit Active Directory-gebruiker Catch-all filter*". Selecteer het **type MV-object**het juiste forest, de **gebruiker** als het **objecttype CS**en de **persoon** . Als **Type koppeling**, selecteert u **deelnemen aan** en typ een waarde die op dit moment niet worden gebruikt door een andere synchronisatie-regel (bijvoorbeeld 600) in voorrang. U hebt een prioriteit waarde hogere (lagere prioriteit) dan de vorige regel voor synchronisatie geselecteerd maar ook ruimte blijft, zodat we meer regels voor filteren sync later toevoegen als u wilt beginnen met synchroniseren van aanvullende diensten. Klik op **volgende**.  
![Inkomende 7 beschrijving](./media/active-directory-aadconnectsync-configure-filtering/inbound7.png)  
9. **Scoping filter** leeg laat en op **volgende**. Een leeg filter geeft aan dat de regel moet worden toegepast op alle objecten.
10. Niets in de regels voor het **deelnemen aan** en klik vervolgens op **volgende**.
11. Klik op **Transformatie toevoegen**de **FlowType** aan een **constante**, selecteert u het kenmerk Target **cloudFiltered** en selecteer in het vak bron typt u **True**. Klik op **toevoegen** om de regel opslaan.  
![Inkomende 3-transformatie](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)  
12. Voor het voltooien van de configuratie, [toepassen en wijzigingen controleren](#apply-and-verify-changes).

Als u wilt, kunt u meer regels van het eerste type waar u meer objecten in onze synchronisatie opnemen maken.

### <a name="outbound-filtering"></a>Uitgaande filters
In sommige gevallen is het noodzakelijk de filtering pas nadat u lid zijn van de objecten in de metaverse. Bijvoorbeeld mogelijk nodig te kijken naar het e-mailkenmerk in het forest met de bronnen en het kenmerk userPrincipalName uit het accountforest om te bepalen als een object moet worden gesynchroniseerd. In deze gevallen kunt u het filteren op de uitgaande regel.

In dit voorbeeld wijzigt u filteren zodat alleen gebruikers waar e-mail- en userPrincipalName met eindigen @contoso.com worden gesynchroniseerd:

1. Aanmelden bij de server waarop sync Azure AD verbinding maken met een account die lid is van de groep **ADSyncAdmins** .
2. **Regeleditor synchronisatie** starten vanuit het startmenu.
3. Klik onder **Type regels** **Uitgaand**.
4. Zoek de regel met de naam **Out naar AAD – gebruikers deelnemen aan SOAInAD**. Klik op **bewerken**.
5. In het pop-upmenu, antwoord **Ja** om een kopie van de regel te maken.
6. Klik op de pagina **Beschrijving** wijzigen voorrang op een niet-gebruikte waarde, bijvoorbeeld 50.
7. Klik op **filter Scoping** op de linkernavigatiebalk. Klik op **component toevoegen**, kenmerk select **e**, Operator select **ENDSWITH**en het soort **@contoso.com**. Klik op **component toevoegen**, kenmerk select **userPrincipalName**, Operator select **ENDSWITH**en het soort **@contoso.com**.
8. Klik op **Opslaan**.
9. Voor het voltooien van de configuratie, [toepassen en wijzigingen controleren](#apply-and-verify-changes).

## <a name="apply-and-verify-changes"></a>Toepassen en wijzigingen controleren
Nadat u uw wijzigingen hebt aangebracht, moeten deze worden toegepast op de objecten die al in het systeem aanwezig. Dit kan ook worden dat objecten die momenteel niet in de synchronisatie-engine moeten worden verwerkt en de synchronisatie-engine moet het bronsysteem opnieuw om te controleren of de inhoud te lezen.

Als u de configuratie met behulp van **domein** of **organisatie-eenheid** filteren hebt gewijzigd, moet u **volledige importbewerking** gevolgd door **Deltasynchronisatie**uitvoeren.

Als u een configuratie met **kenmerk** filter gewijzigd, moet u **volledige synchronisatie**uitvoeren.

De volgende stappen uitvoeren:

1. **Synchronisatie** starten vanuit het startmenu.
2. Selecteer de **verbindingslijnen** en in de lijst met **verbindingslijnen** , selecteert u de verbindingslijn waarvoor u eerder een configuratiewijziging uitgevoerd. **Acties**en selecteer **uitvoeren**.  
![Connector uitvoeren](./media/active-directory-aadconnectsync-configure-filtering/connectorrun.png)  
3. Selecteer de bewerking die wordt vermeld in de vorige sectie in de **profielen worden uitgevoerd**. Als u twee acties uitvoeren, de tweede na eerste uitvoeren (de kolom **staat** is **niet-actief** voor de geselecteerde verbindingslijn).

Na de synchronisatie, worden alle wijzigingen klaargezet worden geëxporteerd. Voordat u daadwerkelijk wijzigingen in Azure AD aanbrengen, die u wilt controleren of alle van deze wijzigingen juist zijn.

1. Start een cmd-prompt en Ga naar`%Program Files%\Microsoft Azure AD Sync\bin`
2. Uitvoeren:`csexport "Name of Connector" %temp%\export.xml /f:x`  
De naam van de Connector vindt u in de tijdsynchronisatie-Service. Een naam die vergelijkbaar is met 'contoso.com – AAD' voor Azure advertentie.
3. Uitvoeren:`CSExportAnalyzer %temp%\export.xml > %temp%\export.csv`
4. U hebt nu een bestand in % temp % met de naam export.csv die in Microsoft Excel kan worden onderzocht. Dit bestand bevat alle wijzigingen die moeten worden geëxporteerd.
5. Noodzakelijke wijzigingen aanbrengen in de gegevens of de configuratie en voer deze stappen opnieuw (importeren, synchroniseren en controleren) totdat de wijzigingen die moeten worden geëxporteerd, worden verwacht.

Als u tevreden bent, de wijzigingen wilt exporteren naar Azure AD.

1. **Aansluitingen** selecteren en selecteer in de lijst **connectoren** de Azure AD-Connector. **Acties**en selecteer **uitvoeren**.
2. Selecteer **exporteren**in de **profielen worden uitgevoerd**.
3. Als uw wijzigingen in de configuratie vele objecten verwijdert, vervolgens ziet u een fout op de uitvoer als het getal groter dan de geconfigureerde drempel (standaard 500 is). Als u deze fout ziet, moet u de functie [niet per ongeluk verwijderen](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)tijdelijk kunt uitschakelen.

Nu is het tijd om de scheduler opnieuw inschakelen.

1. **Taakplanner** starten vanuit het startmenu.
2. Zoek direct onder **Bibliotheek voor Taakplanner**, de taak met de naam **Azure AD Sync Scheduler**, klik met de rechtermuisknop en selecteert u **inschakelen**.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het configureren van [Azure AD Connect worden gesynchroniseerd](active-directory-aadconnectsync-whatis.md) .

Meer informatie over de [integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md).
