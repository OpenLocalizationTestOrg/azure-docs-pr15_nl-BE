<properties
    pageTitle="Azure AD verbinden sync: hoe een wijziging aanbrengt in de standaardconfiguratie | Microsoft Azure"
    description="Doorloopt u hoe u een wijziging aanbrengt in de configuratie in Azure AD verbinden sync."
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
    ms.date="08/31/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-how-to-make-a-change-to-the-default-configuration"></a>Azure AD verbinden sync: hoe een wijziging aanbrengt in de standaardconfiguratie
Het doel van dit onderwerp is kunt u wijzigingen aanbrengen in de standaardconfiguratie Azure AD verbinding synchroon. Het bevat stappen voor enkele veelvoorkomende scenario's. Met deze kennis moet u enkele eenvoudige wijzigingen aanbrengen in uw eigen configuratie op basis van zakelijke regels.

## <a name="synchronization-rules-editor"></a>Regeleditor synchronisatie
De regeleditor synchronisatie wordt weergeven en wijzigen van de standaardconfiguratie gebruikt. Deze kunt u vinden in het Menu Start onder de groep **Azure AD verbinden** .  
![Het Menu Start met Sync regeleditor](./media/active-directory-aadconnectsync-change-the-configuration/startmenu2.png)

Wanneer u het opent, kunt u de standaard kant-en-klare regels bekijken.

![Regeleditor Sync](./media/active-directory-aadconnectsync-change-the-configuration/sre2.png)

### <a name="navigating-in-the-editor"></a>Navigeren in de editor
Vervolgkeuzelijsten boven de editor kunnen u snel zoeken naar een bepaalde regel. Als u zien van de regels waarin het kenmerk proxyAddresses is opgenomen wilt, zou u de vervolgkeuzelijsten wijzigen met de volgende:  
![SRE filteren](./media/active-directory-aadconnectsync-change-the-configuration/filtering.png)  
Voor het opnieuw filteren en een nieuwe configuratie laden, drukt u op **F5** op het toetsenbord.

U hebt een knop **nieuwe regel toevoegen**bovenaan rechts. Deze knop wordt gebruikt om uw eigen aangepaste regel maken.

Onderaan hebt u knoppen voor het optreden op een regel van de geselecteerde sync. **Bewerken** en **verwijderen** kunt u doen wat u verwacht. **Exporteren** , maakt een PowerShell script voor de regel voor de synchronisatie opnieuw te maken. Met deze procedure kunt u een regel voor de synchronisatie van de ene server naar de andere verplaatsen.

## <a name="create-your-first-custom-rule"></a>De eerste aangepaste regel maken
De meest voorkomende wijziging is wijzigingen in de stromen kenmerk. De gegevens in de bronmap mogelijk niet zoals in Azure AD. In het voorbeeld in deze sectie, die u wilt controleren of dat de naam van een gebruiker is altijd in de **BEGINLETTERS**.

### <a name="disable-the-scheduler"></a>De Taakplanner uitschakelen
De [scheduler](active-directory-aadconnectsync-feature-scheduler.md) wordt standaard elke 30 minuten uitgevoerd. Wilt u controleren of dat deze niet start terwijl u wijzigingen aanbrengt en het oplossen van problemen met de nieuwe regels. Schakel tijdelijk de scheduler, starten van PowerShell en uitvoeren`Set-ADSyncScheduler -SyncCycleEnabled $false`

![De Taakplanner uitschakelen](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)  

### <a name="create-the-rule"></a>De regel maken

1. Klik op **nieuwe regel toevoegen**.
2. Geef het volgende op de pagina **Beschrijving** :  
![Binnenkomende regel filteren](./media/active-directory-aadconnectsync-change-the-configuration/description2.png)  
    - Naam: Geef de regel een beschrijvende naam.
    - Beschrijving: Sommige uitleg zodat iemand anders wat de regel begrijpen te voor is.
    - Systeem verbonden: het object kan worden gevonden in het-systeem. In dit geval selecteert u de Active Directory Connector.
    - Objecttype verbonden systeem/Metaverse: **Gebruiker** en **persoon** respectievelijk selecteren.
    - Koppelingstype: Deze waarde wijzigen **deelnemen aan**.
    - Prioriteit: Een waarde opgeeft die uniek is in het systeem. Een lager getal geeft een hogere prioriteit.
    - Tags: Leeg laten. Alleen kant-en-klare regels van Microsoft hebt in dit vak een waarde ingevuld.
3. Voer op de pagina **filter Scoping** **givenName ISNOTNULL**.  
![Binnenkomende regel scoping filter](./media/active-directory-aadconnectsync-change-the-configuration/scopingfilter.png)  
In deze sectie wordt gebruikt om te definiëren die de regel moet worden toegepast op objecten. Als dit veld leeg is, zou de regel van toepassing op alle gebruikersobjecten. Maar dat zou vergaderruimten, serviceaccounts en andere niet-mensen gebruikersobjecten.
4. Op de **regels deelnemen aan**het leeg laten.
5. Klik op de pagina **transformaties** wijzigen in de FlowType **expressie**. Selecteer het doelkenmerk **givenName**en voer in de bron `PCase([givenName])`.
![Binnenkomende regel transformaties](./media/active-directory-aadconnectsync-change-the-configuration/transformations.png)  
De synchronisatie-engine is hoofdlettergevoelig zowel op de naam van de functie en de naam van het kenmerk. Als u iets verkeerd typt, ziet u een waarschuwing wanneer u de regel toevoegen. De editor kunt u opslaan en doorgaan, zodat u de regel open en corrigeer de regel zou moeten zijn.
6. Klik op **toevoegen** om de regel opslaan.

De nieuwe aangepaste regel zijn met de andere synchronisatie-regels zichtbaar in het systeem.

### <a name="verify-the-change"></a>Controleer de wijziging
Met deze nieuwe wijziging die u wilt controleren of deze werkt zoals verwacht en is niet alle fouten genereren. Afhankelijk van het aantal objecten dat u hebt, zijn er twee verschillende manieren om deze stap te doen.

1. Een volledige synchronisatie uitvoeren op alle objecten
2. Een voorbeeld en een volledige synchronisatie uitvoeren op een enkel object

**Synchronisatie** starten vanuit het startmenu. De stappen in deze sectie zijn alle in dit hulpprogramma.

1. **Volledige synchronisatie van alle objecten**  
Selecteer de **verbindingslijnen** boven. Identificatie van de verbindingslijn die u een wijziging hebt aangebracht in de vorige sectie, in dit geval de Active Directory Domain Services en selecteer deze. Selecteer het **uitvoeren** van acties en selecteer **Volledige synchronisatie** en **OK**.
![Volledige synchronisatie](./media/active-directory-aadconnectsync-change-the-configuration/fullsync.png)  
De objecten zijn nu bijgewerkt in de metaverse. Nu wilt u het object in de metaverse bekijken.

2. **Preview en volledige synchronisatie op één object**  
Selecteer de **verbindingslijnen** boven. Identificatie van de verbindingslijn die u een wijziging hebt aangebracht in de vorige sectie, in dit geval de Active Directory Domain Services en selecteer deze. Selecteer **Search Connector ruimte**. Scope gebruiken om te zoeken naar een object dat u gebruiken wilt voor het testen van de wijziging. Selecteer het object en klikt u op **voorbeeld**. Selecteer **Voorvertoning doorvoeren**in het nieuwe scherm.
![Voorbeeld doorvoeren](./media/active-directory-aadconnectsync-change-the-configuration/commitpreview.png)  
De wijziging is nu vastgelegd in de metaverse.

**Kijk naar het object in de metaverse**  
Nu wilt u kiezen een paar monster objecten om ervoor te zorgen dat de waarde wordt verwacht en dat de regel wordt toegepast. Selecteer **Metaverse zoeken** vanaf de bovenkant. Elk filter dat u wilt zoeken naar relevante objecten toevoegen. Open een object in de zoekresultaten. Bekijk de kenmerkwaarden en ook controleren in de kolom **Sync regels** die de regel wordt toegepast als verwacht.  
![Metaverse zoeken](./media/active-directory-aadconnectsync-change-the-configuration/mvsearch.png)  
### <a name="enable-the-scheduler"></a>De scheduler inschakelen
Als alles correct is, kunt u de planner opnieuw inschakelen. Uitvoeren van PowerShell, `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="other-common-attribute-flow-changes"></a>Andere veelvoorkomende kenmerkwijzigingen stroom
Het vorige gedeelte beschreven hoe u kunt wijzigingen aanbrengen in een stroom van het kenmerk. In dit gedeelte vindt u enkele extra voorbeelden. De stappen voor het maken van de regel sync is afgekort, maar u vindt de volledige stappen in de vorige sectie.

### <a name="use-another-attribute-than-the-default"></a>Een ander kenmerk dan de standaardwaarde gebruiken
Bij Fabrikam is er een forest waarin het lokale alfabet wordt gebruikt voor voornaam, achternaam en weergavenaam. De Latijnse representatie van deze kenmerken vindt u in de kenmerken van de uitbreiding. Bij het maken van de algemene adreslijst in AD Azure en Office 365, de organisatie wil dat deze kenmerken in plaats daarvan worden gebruikt.

Met een standaardconfiguratie weergegeven een object uit het lokale forest als volgt:  
![Kenmerk stroom 1](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp1.png)

Als u wilt een regel maken met andere kenmerk stromen, het volgende doen:

- **Regeleditor synchronisatie** starten vanuit het startmenu.
- **Inkomende** links nog steeds is geselecteerd, klik op de knop **nieuwe regel toevoegen**.
- Geef de regel een naam en beschrijving. Selecteer Active Directory op de gebouwen en de relevante objecttypen.  Selecteer bij **Type koppeling** **Join**. Kies voor de prioriteit, een getal dat wordt gebruikt door een andere regel. De kant-en-klare regels beginnen met 100, zodat de waarde 50 in dit voorbeeld kan worden gebruikt.
![Kenmerk stroom 2](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp2.png)
- Bereik leeg laat (dat wil zeggen, gelden voor alle gebruikersobjecten in het forest).
- Join regels leeg laat (dat wil zeggen, laat de out-of-box-regel voor het verwerken van joins).
- Maak de volgende stromen in transformaties:  
![Kenmerk stroom 3](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp3.png)
- Klik op **toevoegen** om de regel opslaan.
- Ga naar **Service Synchronisatiebeheer**. Op de **Connectors**, selecteert u de verbindingslijn waar we de regel toegevoegd. Selecteer **uitvoeren**en **volledige synchronisatie**. Een volledige synchronisatie worden alle objecten met behulp van de huidige regels.

Dit is het resultaat voor hetzelfde object met deze aangepaste regel:  
![Kenmerk stroom 4](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp4.png)

### <a name="length-of-attributes"></a>Lengte van kenmerken
Kenmerken van verbindingsreeksen zijn standaard ingesteld als worden geïndexeerd en de maximale lengte is 448 tekens. Als u met een reeks kenmerken die mogelijk meer werkt, Controleer of het volgende opnemen in de stroom van het kenmerk:  
`attributeName` <- `Left([attributeName],448)`

### <a name="changing-the-userprincipalsuffix"></a>De userPrincipalSuffix wijzigen
Het kenmerk userPrincipalName in Active Directory is niet altijd bekend is door de gebruikers en mogelijk niet geschikt als de aanmeldings-ID. De Azure AD verbinding maken met de installatiewizard sync kunt picken, een ander kenmerk bijvoorbeeld mail. Maar in sommige gevallen het kenmerk moet worden berekend. Het bedrijf Contoso heeft bijvoorbeeld twee Azure AD mappen: een voor de productie en een voor het testen. De gebruikers in hun test huurder een ander achtervoegsel gebruiken in de aanmeldings-ID.  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`

In deze expressie wordt alles nemen links van de eerste @-sign (Word) en tekst met een vaste tekenreeks.

### <a name="convert-a-multi-value-to-a-single-value"></a>Een met meerdere waarden converteren naar een enkele waarde
Bepaalde kenmerken in Active Directory zijn met meerdere waarden in het schema, zelfs als ze op zoek één waarde in Active Directory: gebruikers en Computers. Een voorbeeld is het beschrijvingskenmerk.  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`

In deze expressie in het geval het kenmerk een waarde heeft, we nemen het eerste item (artikel) in het kenmerk verwijdert voorloopspaties en volgspaties (spaties) en behoudt de eerste 448 tekens (links) in de tekenreeks.

### <a name="do-not-flow-an-attribute"></a>Een kenmerk niet stromen
Zie voor achtergrondinformatie over het scenario voor deze sectie, [het kenmerk stroom proces beheren](active-directory-aadconnectsync-understanding-declarative-provisioning.md#control-the-attribute-flow-process).

Er zijn twee manieren een kenmerk niet overlopen. De eerste is beschikbaar in de wizard installeren en kunt u [geselecteerde kenmerken](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering)verwijderen. Deze optie werkt als u het kenmerk voordat nooit hebt gesynchroniseerd. Echter, als u dit kenmerk synchroniseren en later verwijderen met deze functie hebt gestart, vervolgens het sync-engine reageert beheer van het kenmerk en de bestaande waarden blijven Azure AD.

Als u wilt dat de waarde van een kenmerk verwijderen en zorg ervoor dat in de toekomst niet doorloopt, moet u een aangepaste regel maken.

We hebben bij Fabrikam gerealiseerd dat bepaalde kenmerken die we naar de cloud synchroniseren niet er worden moeten. We willen om ervoor te zorgen dat deze kenmerken zijn verwijderd uit AD Azure.  
![Onjuiste extensie kenmerken](./media/active-directory-aadconnectsync-change-the-configuration/badextensionattribute.png)

- Een nieuwe binnenkomende regel voor synchronisatie maken en vullen van de beschrijving van de ![beschrijvingen](./media/active-directory-aadconnectsync-change-the-configuration/syncruledescription.png)
- Kenmerk stromen van het type **expressie** en met de bron **AuthoritativeNull**maken. De letterlijke **AuthoritativeNull** geeft aan dat de waarde is leeg in de MV zelfs als wordt geprobeerd een lagere prioriteit sync-regel voor het vullen van de waarde.
![Transformatie voor uitbreiding kenmerken](./media/active-directory-aadconnectsync-change-the-configuration/syncruletransformations.png)
- De Sync-regel opslaan. **Synchronisatie**starten, vinden de verbindingslijn, selecteert u **uitvoeren**en **Volledige synchronisatie**. Deze stap wordt opnieuw berekend met alle kenmerk stromen.
- Controleer of de voorgenomen wijzigingen wilt exporteren door te zoeken in de ruimte van de connector.
![Gefaseerde verwijderen](./media/active-directory-aadconnectsync-change-the-configuration/deletetobeexported.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het configuratiemodel in [Wat declaratieve inrichten](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
- Lees meer over de taal voor sjabloonexpressies in [Wat declaratieve inrichten van expressies](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

**Van overzichtsonderwerpen**

- [Azure AD verbinden sync: begrijpen en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
- [Integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md)
