<properties
   pageTitle="Azure AD verbinden sync: operationele taken en aandachtspunten | Microsoft Azure"
   description="Dit onderwerp beschrijft de operationele taken voor synchronisatie Azure AD verbinden en het voorbereiden voor de exploitatie van dit onderdeel."
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
   ms.date="09/01/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-operational-tasks-and-consideration"></a>Azure AD verbinden sync: operationele taken en vergoeding
Het doel van dit onderwerp wordt beschreven, operationele taken voor synchronisatie Azure AD verbinden.

## <a name="staging-mode"></a>Staging-modus
Staging-modus kan worden gebruikt voor verschillende scenario's, met inbegrip van:

-   Hoge beschikbaarheid.
-   Testen en implementeren van nieuwe wijzigingen in de configuratie.
-   Invoering van een nieuwe server en ontmantelen van de oude.

U kunt wijzigingen aanbrengen in de configuratie en de wijzigingen te bekijken voordat u de server actief maken met een server in de modus voor gefaseerde installatie. Ook kunt u volledige importbewerking en volledige synchronisatie te verifiëren dat alle wijzigingen worden verwacht voordat u deze wijzigingen in uw productieomgeving aanbrengt uitvoeren.

Tijdens de installatie kunt u de server in de **staging-modus**. Hiermee wordt de server actief is voor synchronisatie en importeren, maar eventuele uitvoer niet wordt uitgevoerd. Een server in de staging-modus is niet actief sync wachtwoord of wachtwoord Write-back, zelfs als u deze functies hebt ingeschakeld tijdens de installatie. Wanneer u een staging-modus uitschakelt, de server exporteren wordt gestart kunt synchroniseren wachtwoord en kan wachtwoord Write-back.

U kunt nog steeds exporteren met behulp van Synchronisatiebeheer service forceren.

Een server in de modus voor gefaseerde installatie nog steeds wijzigingen van Active Directory en Azure AD ontvangen. Heeft altijd een kopie van de laatste wijzigingen en kan erg snel nemen over de verantwoordelijkheden van een andere server. Als u wijzigingen in de configuratie met de primaire server, is het uw verantwoordelijkheid om dezelfde wijzigingen aanbrengen in de staging-modus naar de server.

De staging-modus is voor mensen met kennis van de oudere technologieën voor synchronisatie, verschillende sinds de server een eigen SQL-database heeft. Deze architectuur kan de testserver modus te bevinden in een ander datacenter.

### <a name="verify-the-configuration-of-a-server"></a>Controleer de configuratie van een server
Als u deze methode toepast, moet u deze stappen volgen:

1. [Voorbereiden](#prepare)
2. [Importeren en synchroniseren](#import-and-synchronize)
3. [Controleer of](#verify)
4. [Active switchserver](#switch-active-server)

#### <a name="prepare"></a>Voorbereiden

1. Installeer Azure AD Connect, **staging-modus**selecteren en deselecteren **synchronisatie starten** op de laatste pagina van de installatiewizard. In deze modus kunt u de synchronisatie-engine handmatig uitvoeren.
![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)
2. Log uit/inloggen en selecteer **Tijdsynchronisatie-Service**uit het startmenu.

#### <a name="import-and-synchronize"></a>Importeren en synchroniseren

1. Selecteer de **verbindingslijnen**en selecteert u de eerste verbindingslijn met het type **Active Directory Domain Services**. Klik op **uitvoeren**en selecteer **volledige importbewerking**en vervolgens op **OK**. Voer deze stappen uit voor alle verbindingen van dit type.
2. Selecteer de verbindingslijn met het type **Azure Active Directory (Microsoft)**. Klik op **uitvoeren**en selecteer **volledige importbewerking**en vervolgens op **OK**.
3. Zorg ervoor dat het tabblad verbindingslijnen nog steeds is geselecteerd. Voor elke verbindingslijn met het type **Active Directory Domain Services**, klik op **uitvoeren**, selecteert u **Delta-synchronisatie**en **OK**.
4. Selecteer de verbindingslijn met het type **Azure Active Directory (Microsoft)**. Klik op **uitvoeren**, selecteert u **Delta-synchronisatie**en vervolgens op **OK**.

U hebt nu gefaseerd export verandert in Azure AD en op-premises AD (als u van Exchange hybride deployment gebruikmaakt). De volgende stappen kunnen u controleren wat wordt gewijzigd voordat u daadwerkelijk naar de mappen exporteren.

#### <a name="verify"></a>Controleer of

1. Start een cmd-prompt en Ga naar`%ProgramFiles%\Microsoft Azure AD Sync\bin`
2. Uitvoeren:`csexport "Name of Connector" %temp%\export.xml /f:x`  
De naam van de Connector vindt u in de tijdsynchronisatie-Service. Een naam die vergelijkbaar is met 'contoso.com – AAD' voor Azure advertentie.
3. Uitvoeren:`CSExportAnalyzer %temp%\export.xml > %temp%\export.csv`
4. U hebt een bestand in % temp % met de naam export.csv die in Microsoft Excel kan worden onderzocht. Dit bestand bevat alle wijzigingen die moeten worden geëxporteerd.
5. Noodzakelijke wijzigingen aanbrengen in de gegevens of de configuratie en voer deze stappen opnieuw (importeren en synchroniseren en controleren) totdat de wijzigingen die moeten worden geëxporteerd, worden verwacht.

**Informatie over het bestand export.csv**

Het merendeel van het bestand is geen uitleg. Sommige afkortingen te begrijpen van de inhoud:

- OMODT – wijziging objecttype. Geeft aan of de bewerking op een niveau van een toevoegen, bijwerken of verwijderen.
- AMODT – wijziging kenmerktype. Geeft aan of de bewerking op het kenmerkniveau van een een toevoegen, bijwerken of verwijderen.

Als de waarde van het kenmerk met meerdere waarden is, wordt niet elke wijziging weergegeven. Het aantal waarden toegevoegd en verwijderd, wordt weergegeven.

#### <a name="switch-active-server"></a>Active switchserver

1. De server (FIM-DirSync/Azure AD synchronisatie) zodat deze niet worden geëxporteerd naar uitschakelen Azure AD of stelt u deze in de staging-modus (Azure AD verbinden) op de actieve server.
2. De installatiewizard uitvoeren op de server in de **staging-modus** en **tijdelijke modus**uitschakelen.
![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)

## <a name="disaster-recovery"></a>Noodherstel
Deel van het implementatieontwerp is voor het plannen van wat u moet doen als er een ramp wanneer u de server synchroniseren verliest. Er zijn verschillende modellen en waarvan één te gebruiken is afhankelijk van verschillende factoren zoals:

-   Wat is de tolerantie voor niet dat deze wijzigingen kunnen aanbrengen aan objecten in Azure AD tijdens de uitvaltijd?
-   Als u Wachtwoordsynchronisatie, de gebruikers accepteert dat zij hebben het oude wachtwoord gebruiken in Azure AD wanneer zij op het bedrijf wijzigen?
-   Hebt u een afhankelijkheid op real-time bewerkingen, zoals het wachtwoord Write-back?

Afhankelijk van de antwoorden op deze vragen en het beleid van uw organisatie, kan een van de volgende strategieën worden geïmplementeerd:

-   Opnieuw opbouwen wanneer dat nodig is.
-   Een reserveonderdeel stand-by-server genoemd **staging-modus**hebt.
-   Gebruik van virtuele machines.

Als u niet de ingebouwde SQL Express-database, moet u ook de sectie [SQL hoge beschikbaarheid](#sql-high-availability) bekijken.

### <a name="rebuild-when-needed"></a>Opnieuw opbouwen wanneer dat nodig is
Er is een levensvatbare strategie voor het plannen van een server opnieuw maken als dat nodig is. Normaal gesproken installeert het sync-engine en dat de eerste importeren en synchroniseren binnen een paar uur kunnen worden voltooid. Als er niet een reserve server, is het mogelijk om tijdelijk een domeincontroller als host voor de synchronisatie-engine gebruiken.

De server sync engine slaat geen Staten over de objecten, zodat de database kan worden gemaakt van de gegevens in Active Directory en Azure AD. Het kenmerk **sourceAnchor** wordt gebruikt voor het deelnemen aan de objecten uit de lokale en de cloud. Als u de server met bestaande objecten in-ruimten en de cloud, opnieuw opbouwen en vervolgens de synchronisatie-engine overeenkomt met die objecten elkaar opnieuw op een nieuwe installatie. De dingen die u moet vastleggen en opslaan zijn configuratiewijzigingen naar de server, zoals filteren en synchronisatie regels. Deze aangepaste configuraties moeten opnieuw worden toegepast voordat u synchroniseren gaat.

### <a name="have-a-spare-standby-server---staging-mode"></a>Een reserveonderdeel stand-by-server - staging-modus
Als u een complexere omgeving en vervolgens met een of meer van de stand-by-servers wordt aanbevolen. U kunt tijdens de installatie van een server in de **staging-modus**inschakelen.

Zie voor meer informatie de [staging-modus](#staging-mode).

### <a name="use-virtual-machines"></a>Gebruik van virtuele machines
Een gemeenschappelijke en ondersteunde methode is de sync-engine in een virtuele machine wordt uitgevoerd. Als de host een probleem heeft, kan de afbeelding met de synchronisatie-engine-server naar een andere server worden gemigreerd.

### <a name="sql-high-availability"></a>Hoge beschikbaarheid van SQL
Als u niet de SQL Server Express die wordt geleverd met Azure AD verbinden, moet vervolgens hoge beschikbaarheid voor SQL Server ook worden overwogen. De oplossing voor beschikbaarheid alleen ondersteund is SQL clustering. Niet-ondersteunde oplossingen bevatten mirroring en altijd op.

## <a name="next-steps"></a>Volgende stappen

**Van overzichtsonderwerpen**  

- [Azure AD verbinden sync: begrijpen en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)  
- [Integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md)  
