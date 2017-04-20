<properties
   pageTitle="Algemene SQL-Connector | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe Microsoft algemene SQL-Connector configureren."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="generic-sql-connector-technical-reference"></a>Technische naslaginformatie over algemene SQL-Connector

Dit artikel beschrijft de algemene SQL-Connector. Het artikel is van toepassing op de volgende producten:

- Microsoft Identity Manager 2016 (MIM2016)
- Forefront Identiteitenbeheer 2010 R2 (FIM2010R2)
    -   Moet hotfix 4.1.3671.0 of later [KB3092178](https://support.microsoft.com/kb/3092178)gebruiken.

De Connector is voor MIM2016 en FIM2010R2, worden gedownload vanaf het [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

Deze Connector in actie, Zie het artikel [Stapsgewijze algemene SQL-Connector](active-directory-aadconnectsync-connector-genericsql-step-by-step.md) .

## <a name="overview-of-the-generic-sql-connector"></a>Overzicht van de algemene SQL-Connector

De algemene SQL-Connector kunt u de synchronisatieservice integreren met een databasequery die ODBC-verbindingen biedt.  

Vanuit een perspectief op hoog niveau, worden de volgende functies worden ondersteund door de huidige versie van de verbindingslijn:

Functie | Ondersteuning
--- | ---
Verbonden gegevensbron | De Connector wordt voor alle 64-bits ODBC-stuurprogramma's ondersteund. Het is getest met de volgende: <li>Microsoft SQL Server en SQL Azure</li><li>IBM DB2 10.x</li><li>IBM DB2 9.x</li><li>Oracle 10 & 11 g</li><li>MySQL 5.x</li>
Scenario 's   | <li>Levenscyclusbeheer voor uw object</li><li>Wachtwoordbeheer</li>
Bewerkingen | <li>Volledige Import- en Delta-Import, Export</li><li>Voor uitvoer: Toevoegen, verwijderen, bijwerken en vervangen</li><li>Wachtwoord, wachtwoord wijzigen</li>
Schema | <li>Dynamische detectie van objecten en kenmerken</li>

### <a name="prerequisites"></a>Vereisten
Voordat u de verbindingslijn gebruiken, moet u het volgende op de server voor de synchronisatie:

- 4.5.2 van Microsoft .NET Framework of later
- 64-bits ODBC-stuurprogramma's voor client

### <a name="permissions-in-connected-data-source"></a>Machtigingen in de verbonden gegevensbron
Als u wilt maken of een van de ondersteunde bewerkingen uitvoeren in algemene SQL-connector, moet u hebben:

- db_datareader
- db_datawriter

### <a name="ports-and-protocols"></a>Poorten en protocollen
Raadpleeg de documentatie van de databaseleverancier voor de poorten die vereist zijn voor het ODBC-stuurprogramma werkt.

## <a name="create-a-new-connector"></a>Een nieuwe verbindingslijn maken
Voor het maken van een algemene SQL-connector in de **Tijdsynchronisatie-Service** selecteert **Management Agent** en **maken**. Selecteer de verbindingslijn **generieke SQL (Microsoft)** .

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/createconnector.png)

### <a name="connectivity"></a>Connectiviteit
De verbindingslijn wordt een ODBC DSN-bestand gebruikt voor verbindingen. Maak het DSN-bestand met behulp van **ODBC-gegevensbronnen** worden gevonden in het startmenu onder **Systeembeheer**. In het beheerprogramma een **Bestands-DSN** te maken zodat deze kan worden geleverd aan de verbindingslijn.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/connectivity.png)

Het scherm connectiviteit is de eerste bij het maken van een nieuwe algemene SQL-Connector. Eerst moet u de volgende gegevens:

- Pad naar het DSN-bestand
- Verificatie
    - Gebruikersnaam
    - Wachtwoord

De database moet een van deze verificatiemethoden ondersteunt:

- **Windows-verificatie**: de geverifieerde switchverbindingen database maakt gebruik van de Windows-referenties om te controleren of de gebruiker. De gebruikersnaam en wachtwoord opgegeven wordt voor de verificatie van de database gebruikt. Deze account moet machtigingen voor de database.
- **SQL-verificatie**: verificatie database gebruikt de het gebruikersnaam en wachtwoord gedefinieerd het scherm verbindingen verbinding maken met de database. Als u de gebruiker de naam/wachtwoord in de DSN-bestand opslaat, hebben de referenties die op het scherm connectiviteit voorrang.
- **Azure SQL Database-verificatie**: voor meer informatie Zie [verbinding maken met SQL Database met behulp van Azure Active Directory-verificatie](..\sql-database\sql-database-aad-authentication.md).

**DN-naam anker is**: als u deze optie selecteert, de DN-naam wordt ook gebruikt als het ankerkenmerk. Het kan worden gebruikt voor een eenvoudige implementatie, maar ook heeft de volgende beperking:

-   Connector ondersteunt slechts één objecttype. Alle kenmerken van de verwijzing kunnen daarom alleen verwijzen naar hetzelfde objecttype.

**Type exporteren: Object vervangen**: tijdens het exporteren als u alleen bepaalde kenmerken zijn gewijzigd, het hele object met alle kenmerken wordt geëxporteerd en vervangt u het bestaande object.

### <a name="schema-1-detect-object-types"></a>Schema 1 (objecttypen analyse)
Op deze pagina gaat u configureren hoe de Connector gaat de verschillende objecttypen in de database vinden.

Elk objecttype is aangeboden als een partitie en geconfigureerd verdere op **partities configureren en hiërarchieën**.

![schema1a](./media/active-directory-aadconnectsync-connector-genericsql/schema1a.png)

**Methode voor de detectie Type object**: de Connector ondersteunt deze detectiemethoden object type.

- **Vaste waarde**: bieden u de lijst met objecttypen met een door komma's gescheiden lijst. Bijvoorbeeld: `User,Group,Department`.  
![schema1b](./media/active-directory-aadconnectsync-connector-genericsql/schema1b.png)
- **Tabel/weergave/opgeslagen Procedure**: de naam van de tabel/weergave/opgeslagen procedure en vervolgens de naam van de kolom die de lijst met objecttypen. Als u een opgeslagen procedure, ook geeft u parameters voor het in de indeling **[naam]: [richting]: [waarde]**. Elke parameter opgeven op een aparte regel (Gebruik Ctrl + Enter om een nieuwe regel).  
![schema1c](./media/active-directory-aadconnectsync-connector-genericsql/schema1c.png)
- **SQL-Query**: met deze optie kunt u een SQL-query geeft als resultaat een enkele kolom met objecttypen, zoals `SELECT [Column Name] FROM TABLENAME`. De resulterende kolom moet van het type string (varchar) zijn.

### <a name="schema-2-detect-attribute-types"></a>Schema 2 (kenmerktypen analyse)
Op deze pagina gaat u configureren hoe de namen en typen wilt worden gedetecteerd. De configuratieopties worden weergegeven voor elk objecttype gevonden op de vorige pagina.

![schema2a](./media/active-directory-aadconnectsync-connector-genericsql/schema2a.png)

**Methode voor de detectie Type kenmerk**: de Connector ondersteunt deze kenmerk type detectiemethoden met elk objecttype aangetroffen in Schema 1 scherm.

- **Tabel/weergave/opgeslagen Procedure**: de naam van de tabel/weergave/opgeslagen procedure die moet worden gebruikt om de namen te vinden. Als u een opgeslagen procedure, ook geeft u parameters voor het in de indeling **[naam]: [richting]: [waarde]**. Elke parameter opgeven op een aparte regel (Gebruik Ctrl + Enter om een nieuwe regel). Als u wilt de kenmerknamen in een kenmerk met meerdere waarden worden gevonden, een door komma's gescheiden lijst met tabellen of weergaven te bieden. Scenario's voor meerdere waarden worden niet ondersteund als bovenliggende en onderliggende tabel dezelfde kolomnamen hebben.
- **SQL-query**: met deze optie kunt u een SQL-query die één kolom met namen, bijvoorbeeld retourneert `SELECT [Column Name] FROM TABLENAME`. De resulterende kolom moet van het type string (varchar) zijn.

### <a name="schema-3-define-anchor-and-dn"></a>Schema 3 (anker definiëren en DN)
Op deze pagina kunt u configureren anker en DN-kenmerk voor elk objecttype gevonden. U kunt meerdere kenmerk uniek te maken het anker.

![schema3a](./media/active-directory-aadconnectsync-connector-genericsql/schema3a.png)

- Kenmerken met meerdere waarden en Boolean worden niet weergegeven.
- Hetzelfde kenmerk niet gebruiken voor de DN-naam en het anker, tenzij de **DN-naam is een anker** op de pagina connectiviteit is geselecteerd.
- Als de **DN-naam is een anker** op de pagina connectiviteit is geselecteerd, wordt deze pagina alleen het kenmerk DN vereist. Dit kenmerk kan ook worden gebruikt als het ankerkenmerk.
![schema3b](./media/active-directory-aadconnectsync-connector-genericsql/schema3b.png)

### <a name="schema-4-define-attribute-type-reference-and-direction"></a>Schema 4 (definiëren kenmerktype, referentie en richting)
Op deze pagina kunt u het kenmerktype als integer, binair, of Boolean en richting voor elk kenmerk configureren. Alle kenmerken van de pagina **schema 2** staan, met inbegrip van kenmerken met meerdere waarden.

![schema4a](./media/active-directory-aadconnectsync-connector-genericsql/schema4a.png)

- **Gegevenstype**: het kenmerktype toewijzen aan dergelijke bekend is bij de synchronisatie-engine gebruikt. Standaard wordt het gebruik van hetzelfde type, zoals vastgesteld in het SQL-schema, maar DateTime en verwijzing niet gemakkelijk waarneembaar zijn. Moet u **datum/tijd** of een **verwijzing**opgeven.
- **Richting**: U kunt de richting van het kenmerk instellen op importeren, exporteren of ImportExport. ImportExport is standaard.
![schema4b](./media/active-directory-aadconnectsync-connector-genericsql/schema4b.png)

Opmerkingen:

- Als u een kenmerktype niet aantoonbaar met de Connector is, wordt het gegevenstype String.
- **Geneste tabellen** kunnen worden beschouwd als één kolom databasetabellen. Oracle worden de rijen van een geneste tabel in een bepaalde volgorde opgeslagen. Wanneer u de geneste tabel in een variabele met de PL/SQL ophaalt, worden de rijen echter opeenvolgende subscript vanaf 1 gegeven. Die geeft u matrix-achtige toegang tot afzonderlijke rijen.
- **VARRYS** worden niet ondersteund in de connector.

### <a name="schema-5-define-partition-for-reference-attributes"></a>Schema 5 (partitie voor verwijzing kenmerken definiëren)
Op deze pagina configureert u voor alle verwijzing kenmerken welke partitie (objecttype) een kenmerk naar verwijst.

![schema5](./media/active-directory-aadconnectsync-connector-genericsql/schema5.png)

Als u **DN anker wordt**gebruikt, moet u hetzelfde objecttype gebruiken als die u vanuit verwijst. U kan niet verwijzen naar een ander objecttype.

### <a name="global-parameters"></a>Algemene Parameters
De pagina Algemene Parameters wordt gebruikt om de Delta-Import, datum-en tijdnotatie en wachtwoordmethode configureren.

![globalparameters1](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters1.png)

De algemene SQL-Connector ondersteunt de volgende methoden voor het importeren van Delta:

- **Trigger**: Zie [Delta weergaven met behulp van Triggers worden gegenereerd](https://technet.microsoft.com/library/cc708665.aspx).
- **Watermerk**: een algemene aanpak die kan worden gebruikt met de database. De query watermerk is vooraf ingestelde op basis van de leverancier van de database. Een watermerk kolom moet aanwezig zijn op elke tabel/weergave gebruikt. Deze kolom ingevoegd moet bijhouden en bijwerken van de tabellen als en de afhankelijke (met meerdere waarden of onderliggende) tabellen. De klokken tussen tijdsynchronisatie-Service en de databaseserver moeten worden gesynchroniseerd. Als dat niet het geval is, kunnen sommige gegevens in de delta-import worden weggelaten.  
Beperking:
    - Watermerk strategie biedt geen voor verwijderde ondersteuningsobjecten.
- **Momentopname**: (werkt alleen met Microsoft SQL Server) [genereren Delta weergaven met behulp van momentopnamen](https://technet.microsoft.com/library/cc720640.aspx)
- **Wijzigingen bijhouden**: (werkt alleen met Microsoft SQL Server) [bijhouden](https://msdn.microsoft.com/library/bb933875.aspx)  
Beperkingen:
    - Anker & kenmerk DN moet deel uitmaken van een primaire sleutel voor het geselecteerde object in de tabel.
    - SQL-query wordt niet ondersteund tijdens het importeren en exporteren met wijzigingen bijhouden.

**Aanvullende Parameters**: Database servertijdzone opgeven die aangeeft waar de Database-server zich bevindt. Deze waarde wordt gebruikt voor de ondersteuning van de verschillende indelingen van datum en tijd kenmerken.

De Connector slaat altijd de datum en de datum / tijd in UTC-indeling. Om te kunnen converteren naar behoren de datum en tijd, de tijdzone van de databaseserver en de indeling die wordt gebruikt moet worden opgegeven. De indeling moet worden uitgedrukt in .net-indeling.

Tijdens het exporteren moet elk kenmerk van de tijd datum bij de Connector van de UTC-tijdnotatie worden opgegeven.

![globalparameters2](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters2.png)

**Configuratie van wachtwoord**: wachtwoord synchronisatie mogelijkheden vindt u de connector en ondersteunt en wachtwoord wijzigen.

De Connector biedt twee methoden om ondersteuning voor synchronisatie van wachtwoorden:

- **Opgeslagen Procedure**: bij deze methode moet twee opgeslagen procedures ter ondersteuning van wijziging & instellen wachtwoord. Typ alle parameters voor het toevoegen en wijzigen van de bewerking wachtwoord in **Wachtwoord SP instellen** en **Wijzigen wachtwoord SP** Parameters respectievelijk als per hieronder voorbeeld.
![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters3.png)
- **Wachtwoord-extensie**: bij deze methode moet u wachtwoord-extensie DLL (moet u de extensie dll-naam die u de interface [IMAExtensible2Password implementeert](https://msdn.microsoft.com/library/microsoft.metadirectoryservices.imaextensible2password.aspx) ). Wachtwoord extensie assemblage moet worden geplaatst in de map uitbreiding zodat de connector het DLL-bestand bij uitvoering kunt laden.
![globalparameters4](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters4.png)

Ook moet u het wachtwoord op de pagina **Configureren-extensie** inschakelen.
![globalparameters5](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters5.png)

### <a name="configure-partitions-and-hierarchies"></a>Configureren, partities en hiërarchieën
Selecteer op de pagina partities en hiërarchieën alle objecttypen. Elk object is een eigen partitie.

![partitions1](./media/active-directory-aadconnectsync-connector-genericsql/partitions1.png)

U kunt ook de waarden die zijn gedefinieerd op de pagina **connectiviteit** of **Globale Parameters** negeren.

![partitions2](./media/active-directory-aadconnectsync-connector-genericsql/partitions2.png)

### <a name="configure-anchors"></a>Ankers configureren
Deze pagina is alleen-lezen, omdat het anker is al gedefinieerd. Het ankerkenmerk van de geselecteerde is altijd met het objecttype zodat het unieke blijft over objecttypen toegevoegd.

![ankers](./media/active-directory-aadconnectsync-connector-genericsql/anchors.png)

## <a name="configure-run-step-parameter"></a>Voer stap Parameter configureren
Deze stappen zijn geconfigureerd op de profielen van de uitvoering op de verbindingslijn. Deze configuraties worden de werkelijke hoeveelheid werk van het importeren en exporteren van gegevens.

### <a name="full-and-delta-import"></a>Volledige en Delta importeren
Algemene ondersteuning voor SQL-Connector volledig en Delta importeren met behulp van deze methoden:

- Tabel
- Weergave
- Opgeslagen Procedure
- SQL-Query

![runstep1](./media/active-directory-aadconnectsync-connector-genericsql/runstep1.png)

**Tabel/weergave**  
Voor het importeren van meerdere waarden van kenmerken voor een object, hebt u de naam van de tabel/weergave door komma's gescheiden in **naam van meerdere waarden tabelweergaven** en respectieve join-voorwaarden in de **Join-voorwaarde** voorzien van de bovenliggende tabel.

Voorbeeld: U wilt het werknemer-object en alle bijbehorende kenmerken met meerdere waarden importeren. Er zijn twee tabellen, met de naam werknemer (primaire tabel) en de afdeling (met meerdere waarden).
Voer de volgende handelingen uit:

- Type **werknemer** **Tabel/weergave/SP**.
- Typ **de naam van meerdere waarden tabelweergaven**afdeling.
- Typ de join-voorwaarde tussen werknemer en afdeling **Join-voorwaarde**, bijvoorbeeld `Employee.DEPTID=Department.DepartmentID`.
![runstep2](./media/active-directory-aadconnectsync-connector-genericsql/runstep2.png)

**Opgeslagen procedures**  
![runstep3](./media/active-directory-aadconnectsync-connector-genericsql/runstep3.png)

- Als u veel gegevens hebt, verdient het pagineren met opgeslagen Procedures implementeren.
- De opgeslagen Procedure ter ondersteuning van het pagineren, moet u Index Start en einde-Index op te geven. Zie: [efficiënt paginering van grote hoeveelheden gegevens](https://msdn.microsoft.com/library/bb445504.aspx).
- @StartIndexen @EndIndex bij uitvoering worden vervangen door de respectieve paginaformaat geconfigureerd op de pagina **Stap configureren** . Bijvoorbeeld, wanneer de connector haalt eerste pagina en de paginagrootte is ingesteld 500, in een dergelijke situatie @StartIndex zou dan 1 en @EndIndex 500. Deze waarden verhogen wanneer connector haalt de volgende pagina's en wijzigt de @StartIndex & @EndIndex waarde.
- Geef de parameters in voor het uitvoeren van de opgeslagen Procedure met parameters `[Name]:[Direction]:[Value]` indeling. Voer elke parameter op een aparte regel (Gebruik Ctrl + Enter om een nieuwe regel).
- Algemene SQL-connector ondersteunt ook de importbewerking uit gekoppelde Servers in Microsoft SQL Server. Als u gegevens uit een tabel in gekoppelde server moet worden opgehaald, moet in de indeling tabel voorwaarde:`[ServerName].[Database].[Schema].[TableName]`
- Algemene SQL-Connector ondersteunt alleen objecten die een vergelijkbare structuur hebben (type zowel alias naam en gegevens) tussen gegevens en schema's detectie van stappen uitvoeren. Als het geselecteerde object uit het schema en de opgegeven gegevens bij stap uitvoeren is, zijn SQL-Connector is geen ondersteuning voor dit type scenario's.

**SQL-Query**  
![runstep4](./media/active-directory-aadconnectsync-connector-genericsql/runstep4.png)

![runstep5](./media/active-directory-aadconnectsync-connector-genericsql/runstep5.png)

- Meerdere query's niet ondersteund voor resultaatset.
- SQL-query ondersteunt de paginering en start bieden Index en einde-Index als een variabele voor de ondersteuning van paginering.

### <a name="delta-import"></a>Delta-Import
![runstep6](./media/active-directory-aadconnectsync-connector-genericsql/runstep6.png)

Delta importeren configuratie vereist een meer configuratie vergeleken met de volledige importbewerking.

- Als u de Trigger of momentopname aanpak van deltawijzigingen bijhouden, geef database tabel Historie of opname in **geschiedenistabel of momentopname databasenaam** .
- Ook moet u join-voorwaarde tussen geschiedenistabel en bovenliggende tabel, bijvoorbeeld`Employee.ID=History.EmployeeID`
- Voor het bijhouden van de transactie op de bovenliggende tabel uit de van geschiedenistabel, dient u de naam van de kolom met de bewerkingsgegevens (toevoegen, bijwerken of verwijderen).
- Als u een watermerk voor het bijhouden van deltawijzigingen kiest, geeft u de naam van de kolom met de bewerkingsgegevens in de **Kolomnaam van Water is ingeschakeld**.
- De kolom **typekenmerk wijzigen** is vereist voor het wijzigingstype. Deze kolom is toegewezen een wijziging die in de primaire tabel of met meerdere waarden om een wijzigingstype in de weergave van de delta plaatsvindt. Deze kolom kunt plaatsen, de Modify_Attribute wijzigen voor het kenmerk niveau wijzigen of toevoegen, wijzigen of verwijderen voor een type object niveau wijzigen wijzigen. Als het is iets anders dan de standaardwaarde toevoegen, wijzigen of verwijderen, moet u deze waarden met deze optie kunt definiëren.

### <a name="export"></a>Exporteren
![runstep7](./media/active-directory-aadconnectsync-connector-genericsql/runstep7.png)

Exporteren met behulp van de vier ondersteunde methoden, zoals ondersteuning voor algemene SQL-Connector:

- Tabel
- Weergave
- Opgeslagen Procedure
- SQL-Query

**Tabel/weergave**  
Als u de optie tabel/weergave kiest, wordt de verbindingslijn gegenereerd de respectieve worden gebruikt voor het uitvoeren van de Export.

**Opgeslagen procedures**  
![runstep8](./media/active-directory-aadconnectsync-connector-genericsql/runstep8.png)

Als u de optie opgeslagen Procedure, exporteren, moet drie verschillende opgeslagen procedures uit te voeren bewerkingen invoegen, bijwerken of verwijderen.

- **SP-naam toevoegen**: deze SP wordt uitgevoerd als u een object kunt connector voor opneming in de desbetreffende tabel.
- **Naam van SP bijwerken**: deze SP wordt uitgevoerd als u een object kunt connector voor update in de bijbehorende tabel.
- **SP-naam verwijderen**: deze SP wordt uitgevoerd als u een object kunt connector voor verwijdering in de bijbehorende tabel.
- Een kenmerk in het schema dat wordt gebruikt als een parameterwaarde naar de opgeslagen procedure hebt geselecteerd. Bijvoorbeeld, `EmployeeName: INPUT: @EmployeeName` (EmployeeName in het schema van de connector is geselecteerd en de verbindingslijn wordt vervangen door de desbetreffende waarde tijdens het exporteren)
- Geef parameters in opgeslagen procedure met parameters uitvoeren, `[Name]:[Direction]:[Value]` indeling. Voer elke parameter op een aparte regel (Gebruik Ctrl + Enter om een nieuwe regel).

**SQL-query**  
![runstep9](./media/active-directory-aadconnectsync-connector-genericsql/runstep9.png)

Als u de optie SQL-query, exporteren, moet drie verschillende query's invoegen, bijwerken of verwijderen bewerkingen uit te voeren.

- **Query invoegen**: deze query wordt uitgevoerd als u een object kunt connector voor opneming in de desbetreffende tabel.
- **Query bijwerken**: deze query wordt uitgevoerd als u een object kunt connector voor update in de bijbehorende tabel.
- **Verwijderquery**: deze query wordt uitgevoerd als u een object kunt connector voor verwijdering in de bijbehorende tabel.
- Geselecteerd uit het schema bijvoorbeeld gebruikt als een waarde voor de parameter aan de query, kenmerk`Insert into Employee (ID, Name) Values (@ID, @EmployeeName)`

## <a name="troubleshooting"></a>Het oplossen van problemen

-   Zie voor meer informatie over het inschakelen van logboekregistratie voor het oplossen van de connector [hoe inschakelen ETW-tracering voor verbindingslijnen](http://go.microsoft.com/fwlink/?LinkId=335731).
