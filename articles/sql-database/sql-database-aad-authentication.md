<properties
   pageTitle="Via Azure Active Directory-verificatie verbinding maken met de SQL-Database of SQL datawarehouse | Microsoft Azure"
   description="Informatie over hoe u verbinding maakt met de SQL-Database via Azure Active Directory-verificatie."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/16/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="connecting-to-sql-database-or-sql-data-warehouse-by-using-azure-active-directory-authentication"></a>Verbinding maken met de SQL-Database of SQL datawarehouse met Azure Active Directory-verificatie

Azure Active Directory-verificatie is een mechanisme met behulp van identiteiten in Azure Active Directory (AD Azure) verbinding te maken met Microsoft Azure SQL-Database en [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) . Azure Active Directory-verificatie, kunt u de identiteit van gebruikers en andere Microsoft-services op één centrale locatie centraal beheren. Centraal beheer van ID vormt een handig voor het beheren van gebruikers en vereenvoudigt het beheer van machtigingen. Voordelen zijn onder andere:

- Het biedt een alternatief voor de SQL Server-verificatie.
- Houdt de verspreiding van de identiteit van gebruikers over databaseservers.
- Wachtwoord draaien op één plaats kunt
- Klanten kunnen met behulp van externe (AAD) groepen machtigingen beheren.
- Het kunt opslaan van wachtwoorden elimineren door geïntegreerde Windows-verificatie en andere vormen van verificatiemethoden die worden ondersteund door Azure Active Directory.
- Azure Active Directory-verificatie wordt gebruikt die databasegebruikers verifiëren identiteiten op databaseniveau.
- Azure Active Directory ondersteunt verificatie op basis van het token voor toepassingen die verbinding maken met een SQL-Database.
- Azure Active Directory-verificatie ondersteunt AD FS (domein federation) of eigen gebruikerswachtwoord verificatie voor een lokale Azure Active Directory zonder domein synchroniseren.  
- Azure Active Directory ondersteunt verbindingen met SQL Server Management Studio die universele verificatie van Active Directory, waaronder (MVR gesloten meerledige verificatie) gebruiken.  MVR gesloten bevat sterke verificatie met tal van opties voor eenvoudige verificatie, telefoongesprek, tekstbericht, smartcards met pin of mobiele app melding. Zie [ondersteuning van SSMS voor Azure AD MVR gesloten met de SQL-Database en SQL Data Warehouse](sql-database-ssms-mfa-authentication.md)voor meer informatie.

De configuratiestappen omvatten de volgende procedures voor het configureren en Azure Active Directory-verificatie gebruiken.

1. Maken en een Azure Active Directory te vullen.
2. Controleer uw database in Azure SQL Database V12. (Niet noodzakelijk zijn voor SQL-datawarehouse.)
3. Optioneel: Koppelen of wijzigen die is gekoppeld aan uw abonnement Azure active directory.
4. Een beheerder Azure Active Directory maken voor Azure SQL Server of [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
5. De clientcomputers configureren.
6. Gebruikers van de opgenomen in de database toegewezen aan AD Azure identiteiten maken.
7. Verbinding maken met uw database met behulp van Azure AD identiteiten.


## <a name="trust-architecture"></a>Architectuur van vertrouwen

In het volgende diagram op hoog niveau wordt een overzicht gegeven van de oplossingsarchitectuur van het AD Azure verificatie met Azure SQL-Database. Dezelfde concepten die van toepassing op SQL Data Warehouse. Ter ondersteuning van de eigen gebruikerswachtwoord Azure Active Directory alleen het deel van de wolk en Azure AD/Azure SQL-Database wordt beschouwd als. Voor de ondersteuning van federatieve verificatie (of gebruikerswachtwoord voor Windows-referenties), de communicatie met AD FS blok is vereist. De pijlen geven de communicatiepaden.

![AAD auth-diagram][1]

In het volgende diagram geeft de Federatie, vertrouwen en hosting relaties waarmee een client verbinding maakt met een database door het indienen van een token. Het token is geverifieerd door een Azure advertentie en wordt vertrouwd door de database. Klant 1 kan een Azure Active Directory met native gebruikers of een Azure Active Directory met de federatieve gebruikers vertegenwoordigen. Klant 2 staat voor een mogelijke oplossing, met inbegrip van geïmporteerde gebruikers; in dit voorbeeld is afkomstig van een federatieve Azure Active Directory met AD FS met Azure Active Directory worden gesynchroniseerd. Het is belangrijk te begrijpen dat de toegang tot een database met Azure AD-verificatie vereist is dat de hosting-abonnement gekoppeld aan de Azure Active Directory is. Het abonnement hetzelfde moet worden gebruikt voor het maken van de SQL-Server die de host Azure SQL-Database of SQL Data Warehouse.

![relatie met abonnement][2]

## <a name="administrator-structure"></a>Structuur van de beheerder

Als u verificatie van AD Azure, zijn er twee Administrator-accounts voor de Database van SQL server. de oorspronkelijke SQL Server-beheerder en de beheerder AD Azure. Dezelfde concepten die van toepassing op SQL Data Warehouse. Alleen de beheerder op basis van een Azure AD-account kunt maken van de eerste gebruiker van Azure AD opgenomen in de database van een gebruiker. De aanmelding als beheerder AD Azure is een Azure AD-gebruiker of een groep AD Azure. Wanneer de beheerder een groepsaccount is, kan het worden gebruikt met elk groepslid om meerdere AD Azure beheerders voor het exemplaar van SQL Server. Verbetert de beheersbaarheid doordat u centraal toevoegen en verwijderen van leden van de groep in Azure AD zonder het wijzigen van de gebruikers of de machtigingen in SQL-Database met behulp van de groepsaccount als beheerder. Slechts één AD Azure beheerder (een gebruiker of groep) kan op elk gewenst moment worden geconfigureerd.

![Admin-structuur][3]

## <a name="permissions"></a>Machtigingen

Als u nieuwe gebruikers maakt, hebt u de `ALTER ANY USER` machtiging in de database. De `ALTER ANY USER` machtigingen kan worden toegekend aan elke databasegebruiker. De `ALTER ANY USER` machtiging wordt ook aangehouden door de server administrator-accounts en databasegebruikers met de `CONTROL ON DATABASE` of `ALTER ON DATABASE` machtiging voor die database, en door leden van de `db_owner` databaserol.

U maakt een ingesloten databasegebruiker in Azure SQL-Database of SQL Data Warehouse, moet u verbinding maken met de database met behulp van een identiteit Azure AD. De eerste opgenomen om databasegebruiker te maken, moet u verbinding met de database met behulp van een Azure Active Directory-beheerder (die eigenaar is van de database). Dit wordt geïllustreerd in stap 4 en 5 hieronder. De Azure Active Directory-verificatie is alleen mogelijk als de admin Azure Active Directory is gemaakt voor Azure SQL-Database of Data Warehouse voor SQL server. Als de admin Azure Active Directory is verwijderd van de server, kunnen bestaande Azure Active Directory-gebruikers gemaakt eerder in SQL Server niet langer verbinding met de database met hun Azure Active Directory-referenties.

## <a name="azure-ad-features-and-limitations"></a>Azure AD functies en beperkingen

De volgende leden van Azure Active Directory kunnen worden ingericht in Azure SQL Server of SQL Data Warehouse:

- Native leden: lid gemaakt in Azure AD in de beheerde domein of in een domein van de klant. Zie [uw eigen domeinnaam naar Azure AD toevoegen](../active-directory/active-directory-add-domain.md)voor meer informatie.

- Federatieve leden van een domein: lid in Azure AD met een federatieve domein gemaakt. Zie [Microsoft Azure nu Federatie met Windows Server Active Directory ondersteunt](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/)voor meer informatie.

- Geïmporteerde leden van andere Azure voor Active Directory's die leden van een domein in native modus of federatieve zijn.

- Active Directory-groepen gemaakt als beveiligingsgroepen.

Microsoft-accounts (bijvoorbeeld outlook.com, hotmail.com, live.com) of andere gastaccounts (zoals gmail.com, yahoo.com) worden niet ondersteund. Als u zich voor [https://login.live.com](https://login.live.com) met behulp van de account en het wachtwoord aanmelden kunt, gebruikt u een Microsoft-account wordt niet ondersteund voor de verificatie van AD Azure voor Azure SQL-Database of Azure SQL Data Warehouse.

### <a name="additional-considerations"></a>Aanvullende overwegingen

- Betere beheerbaarheid, is het raadzaam dat bij het inrichten van een gespecialiseerde groep Azure Active Directory als een beheerder.
- Slechts één AD Azure beheerder (een gebruiker of groep) kan worden geconfigureerd voor een Azure SQL Server of Azure SQL Data Warehouse op elk gewenst moment.
- Alleen een beheerder voor SQL Server Azure Active Directory kan in eerste instantie verbinden met de Azure SQL Server of Azure SQL Data Warehouse met een Azure Active Directory-account. De beheerder van Active Directory kunt volgende Azure Active Directory-databasegebruikers configureren.
- Het is raadzaam de verbindingstime-out ingesteld op 30 seconden.
- Ondersteuning voor verificatie van Active Directory Azure 2016 Management Studio voor SQL Server en SQL Server Data Tools voor Visual Studio 2015 (versie 14.0.60311.1April 2016 of hoger). (Azure Active Directory-verificatie wordt ondersteund door de **.NET Framework Data Provider voor SQL Server**; op minimaal versie van .NET Framework 4.6). De nieuwste versies van deze hulpprogramma's en gegevens-tier toepassingen (DAC en .bacpac) kunnen daarom Azure Active Directory-verificatie gebruiken.
- [ODBC-versie 13,1](https://www.microsoft.com/download/details.aspx?id=53339) Azure Active Directory-verificatie ondersteunt echter `bcp.exe` kan geen verbinding maken met behulp van verificatie van Azure Active Directory omdat het een oudere ODBC provider gebruikt.
- `sqlcmd`Azure Active Directory-verificatie die begint met 13.1 beschikbaar via het [Download Center](http://go.microsoft.com/fwlink/?LinkID=825643)versie ondersteunt.  
- SQL Server Data Tools voor Visual Studio 2015 moet ten minste de April 2016-versie van de gegevens (versie 14.0.60311.1). Op dit moment worden Azure Active Directory-gebruikers niet weergegeven in SSDT Object Explorer. Als tijdelijke oplossing de gebruikers in [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx)te bekijken.
- [6.0 van Microsoft JDBC-stuurprogramma voor SQL Server](https://www.microsoft.com/en-us/download/details.aspx?id=11774) ondersteunt Azure Active Directory-verificatie. Zie ook [het instellen van de eigenschappen van de verbinding](https://msdn.microsoft.com/library/ms378988.aspx).
- PolyBase kan niet worden geverifieerd met Azure Active Directory-verificatie.
- Met sommige hulpmiddelen zoals BI en Excel worden niet ondersteund.
- Azure Active Directory-verificatie wordt ondersteund voor SQL-Database door de Azure portal **Database importeren** en **Exporteren van Database** -blades. Importeren en exporteren met behulp van Azure Active Directory-verificatie ook met de PowerShell-opdracht wordt ondersteund.


## <a name="1-create-and-populate-an-azure-ad"></a>1. het maken en vullen van een advertentie Azure

Maak een Azure Active Directory en deze vult met gebruikers en groepen. Azure Active Directory is het eerste domein Azure AD beheerde domein. Azure Active Directory is ook een in de lokalen Active Directory Domain Services die federated is met de Azure Active Directory.

Zie [identiteiten in ruimten met Azure Active Directory integreren](../active-directory/active-directory-aadconnect.md), [toevoegen van uw eigen domeinnaam naar Azure AD](../active-directory/active-directory-add-domain.md) [nu Microsoft Azure federation met Windows Server Active Directory ondersteunt](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [beheert de directory Azure AD](https://msdn.microsoft.com/library/azure/hh967611.aspx)en [Azure AD met Windows PowerShell beheren](https://msdn.microsoft.com/library/azure/jj151815.aspx)voor meer informatie.

## <a name="2-ensure-your-sql-database-is-version-12"></a>2. Controleer de SQL-Database versie 12

Azure Active Directory-verificatie wordt ondersteund in de meest recente SQL Database V12. Zie voor informatie over de SQL-Database V12 en of deze in uw regio beschikbaar is, [Wat is nieuw in de meest recente SQL Database Update V12](sql-database-v12-whats-new.md). Deze stap is niet nodig voor Azure SQL Data Warehouse omdat SQL Data Warehouse alleen beschikbaar in V12 is.

Als u een bestaande database hebt, controleert u of dat deze is opgenomen in de SQL-Database V12 door verbinding te maken met de database (bijvoorbeeld met behulp van SQL Server Management Studio) en het uitvoeren van `SELECT @@VERSION;`. De verwachte output voor een database in SQL Database V12 is ten minste **Microsoft SQL Azure (RTM) - 12.0**. Als uw database niet wordt gehost in een SQL-Database V12, Zie [plannen en voorbereiden voor een upgrade naar SQL Database V12](sql-database-v12-plan-prepare-upgrade.md), en Ga naar de klassieke Portal Azure om te migreren van de database naar een SQL-Database V12.

U kunt ook een nieuwe database maken in SQL Database V12 door de stappen die in [de eerste Azure SQL-database maken](sql-database-get-started.md). **Tip**: de volgende stap te lezen voordat u een abonnement voor de nieuwe database.

## <a name="3-optional-associate-or-change-the-active-directory-that-is-currently-associated-with-your-azure-subscription"></a>3. optioneel: Koppelen of te wijzigen die is gekoppeld aan uw abonnement Azure active directory

Als u wilt uw database te koppelen aan de Azure AD-map voor uw organisatie, de map een vertrouwde map maken voor het hosten van de database Azure-abonnement. Zie [hoe Azure abonnementen zijn gekoppeld met Azure Active Directory](https://msdn.microsoft.com/library/azure/dn629581.aspx)voor meer informatie.

**Als u meer informatie:** Elke Azure abonnement heeft een vertrouwensrelatie met een instantie Azure AD. Dit betekent dat deze vertrouwensrelaties die map voor het verifiëren van gebruikers, services en apparaten. Meerdere abonnementen dezelfde map kunnen vertrouwen, maar een abonnement vertrouwt slechts één map. U kunt zien welke map wordt vertrouwd door uw abonnement op het tabblad **Instellingen** op [https://manage.windowsazure.com/](https://manage.windowsazure.com/). Deze vertrouwensrelatie met een abonnement met een directory is in tegenstelling tot de relatie waarvoor een abonnement met alle andere bronnen in Azure (websites, databases, enzovoort), die meer lijken op onderliggende bronnen van een abonnement. Als u een abonnement verloopt, stopt toegang tot de andere resources die zijn gekoppeld aan het abonnement ook. Maar de map in Azure blijft en kunt u doorgaan met het beheren van de directory-gebruikers en een ander abonnement gekoppeld aan die map. Zie voor meer informatie over bronnen [informatie over toegang tot bronnen in Azure](https://msdn.microsoft.com/library/azure/dn584083.aspx).

De volgende procedures bieden stapsgewijze instructies voor het wijzigen van de gekoppelde map voor een bepaald abonnement.

1. Verbinding maken met uw [Azure klassieke Portal](https://manage.windowsazure.com/) met behulp van een beheerder Azure abonnement.
2. Klik op de banner links, selecteer **Instellingen**.
3. Uw abonnementen worden weergegeven in het scherm instellingen. Als het gewenste abonnement niet wordt weergegeven, klik op **abonnementen** boven **FILTER door DIRECTORY** vervolgkeuzelijst en selecteer de map waarin uw abonnementen en klik vervolgens op **toepassen**.

    ![Selecteer abonnement][4]
4. In het gebied **Instellingen** op uw abonnement en klik vervolgens op **Map bewerken** onder aan de pagina.

    ![AD-instellingen-portal][5]
5. Azure Active Directory die is gekoppeld aan uw SQL Server of SQL Data Warehouse selecteren in het vak **Map bewerken** en klikt u vervolgens op de pijl voor het volgende.

    ![map bewerken selecteren][6]
6. In het dialoogvenster **bevestigen** directory-toewijzing bevestigen '**alle CO-beheerders worden verwijderd.**"

    ![bewerken directory bevestigen][7]
7. Klik op het selectievakje om het laden van de portal.

> [AZURE.NOTE] Wanneer u in een andere map, toegang tot CO-beheerders, Azure AD-gebruikers en groepen, en resource directory-backed gebruikers worden verwijderd en ze niet langer toegang hebben tot dit abonnement of de bijbehorende bronnen. Alleen kunt, als servicebeheerder u toegang voor beveiligings-principals op basis van de nieuwe map. Deze wijziging kan duren voordat een aanzienlijke hoeveelheid tijd doorgeven aan alle resources. Als de map ook wijzigt, de beheerder AD Azure voor SQL-Database en SQL Data Warehouse en weigeren van toegang tot de database voor alle gebruikers van Azure AD. Azure AD admin moet opnieuw instellen (zoals hieronder beschreven) en nieuwe Azure AD-gebruikers moeten worden gemaakt.

## <a name="4-create-an-azure-ad-administrator-for-azure-sql-server"></a>4. een beheerder AD Azure voor Azure SQL Server maken

Elke Azure SQL Server (die host is van een SQL-Database of SQL Data Warehouse) begint met een enkele server administrator-account die door de beheerder van de hele Azure SQL Server is. Een tweede SQL Server-beheerder moet worden gemaakt, die een Azure AD-account is. Deze principal is gemaakt als een ingesloten databasegebruiker in de hoofddatabase. Als beheerders van de server administrator-accounts lid zijn van **de rol db_owner in elke gebruikersdatabase** en elke gebruikersdatabase invoeren als de gebruiker **dbo** . Zie voor meer informatie over de server administrator-accounts [beheren van Databases en Azure SQL-Database-aanmeldingen](sql-database-manage-logins.md) en de sectie **aanmeldingen en gebruikers** van [Azure SQL Database beveiligingsrichtlijnen en beperkingen](sql-database-security-guidelines.md).

Als u Active Directory Azure met Geo-replicatie, moet de Azure Active Directory-beheerder worden geconfigureerd voor zowel de primaire als secundaire servers. Als een server geen Azure Active Directory-beheerder vervolgens ontvangen Azure Active Directory-aanmeldingen en gebruikers een "kan geen verbinding maken' server-fout.

> [AZURE.NOTE] Gebruikers die niet zijn gebaseerd op een Azure AD-account (inclusief de Azure SQL Server administrator-account) kan Azure AD-gebruikers, maken, omdat ze niet zijn gemachtigd voor het valideren van gebruikers met de Azure AD voorgestelde.

### <a name="provision-an-azure-active-directory-administrator-for-your-azure-sql-server-by-using-the-azure-portal"></a>Inrichten van een Azure Active Directory-beheerder voor uw Azure SQL Server met behulp van de portal voor Azure

1. Klik op de verbinding met een lijst van mogelijke Active Directory's in de [portal Azure](https://portal.azure.com/), in de rechterbovenhoek. Kies het juiste Active Directory als de standaard Azure AD. Deze stap koppelt u de koppeling van het abonnement met Active Directory met Azure SQL Server is ervoor te zorgen dat het hetzelfde abonnement wordt gebruikt voor zowel AD Azure en SQL Server. (De Azure SQL Server kan worden hosting Azure SQL-Database of Azure SQL Data Warehouse.)

    ![Kies ad][8]
2. In de banner links **SQL servers**selecteren, selecteert u de **SQL server**en klik vervolgens in de **SQL Server** -blade boven op **Instellingen**.

    ![AD-instellingen][9]
3. Klik op in het blad **Instellingen** ** beheerder van Active Directory.
4. In de **Active Directory-beheer** -blade klikt u op **Active Directory-beheer**en klik op **Set admin**bovenaan.
5. Selecteer de gebruiker of groep aan een beheerder in de blade **admin toevoegen** , zoeken naar een gebruiker, en klik op **selecteren**. (De actieve Directory admin blade bevat alle leden en groepen van Active Directory. Gebruikers of groepen die grijs worden weergegeven, kunnen niet worden geselecteerd omdat ze niet worden ondersteund als beheerders Azure AD. (Zie de lijst met ondersteunde admins in **Azure AD functies en beperkingen** hierboven). Op rollen gebaseerde toegangscontrole (RBAC) is alleen van toepassing op de portal en wordt niet doorgegeven aan SQL Server.
6. Aan de bovenkant van de **Active Directory admin** blade, klikt u op **Opslaan**.
    ![Kies admin][10]

    Het proces van het wijzigen van de beheerder kan enkele minuten duren. Vervolgens verschijnt de nieuwe beheerder in het **Active Directory-beheer** .

> [AZURE.NOTE] Bij het instellen van AD Azure admin, kan niet de nieuwe admin naam (gebruiker of groep) al aanwezig zijn in de hoofddatabase virtuele als SQL Server-verificatie. Indien aanwezig, mislukt de Azure AD admin setup; terugdraaien van het maken en aan te geven dat deze een admin (naam) al bestaat. Sinds die SQL Server-verificatie gebruiker geen deel van de advertentie Azure uitmaakt, wordt elke inspanning verbinding maken met de server met AD Azure verificatie mislukt.

Naar een Admin, aan de bovenkant van de **beheerder van Active Directory** -blade later verwijderen, klikt u op **beheer verwijderen**en klik vervolgens op **Opslaan**.

### <a name="provision-an-azure-ad-administrator-for-azure-sql-server-by-using-powershell"></a>Een beheerder AD Azure voor Azure SQL Server inrichten met PowerShell

PowerShell-cmdlets wordt uitgevoerd, moet u Azure PowerShell geïnstalleerd en uitgevoerd. Zie voor gedetailleerde informatie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

Om het inrichten van een admin Azure AD de volgende Azure PowerShell-opdrachten worden uitgevoerd:

- Toevoegen-AzureRmAccount
- Selecteer AzureRmSubscription


Cmdlets die wordt gebruikt voor het inrichten en beheren van Azure AD admin:

| Naam van cmdlet                                       | Beschrijving                                                                                                     |
|---------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| [Set AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt603544.aspx)    | Azure Active Directory-beheerder bepalingen voor Azure SQL Server of Azure SQL Data Warehouse. (Moet van het huidige abonnement.) |
| [Verwijderen AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt619340.aspx) | Hiermee verwijdert u een Azure Active Directory-beheerder voor Azure SQL Server of Azure SQL Data Warehouse. |
| [Get-AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt603737.aspx)    | Retourneert informatie over een Azure Active Directory-beheerder die momenteel zijn geconfigureerd voor de Azure SQL Server of Azure SQL Data Warehouse. |

Gebruikt u PowerShell-opdracht get-help voor meer details voor elk van deze opdrachten, bijvoorbeeld ``get-help Set-AzureRmSqlServerActiveDirectoryAdministrator``.

Het volgende script bepalingen een beheerder Azure AD groep met de naam **DBA_Group** (object-id `40b79501-b343-44ed-9ce7-da4c8cc7353f`) voor de **demo_server** van de server in een groep met de naam **groep 23**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23"
–ServerName "demo_server" -DisplayName "DBA_Group"
```

De invoerparameter **DisplayName** accepteert de weergavenaam Azure AD of de UPN-naam. Bijvoorbeeld, ``DisplayName="John Smith"`` en ``DisplayName="johns@contoso.com"``. Voor Azure AD groepen alleen de Azure advertentie wordt weergegeven naam ondersteund.

> [AZURE.NOTE] De opdracht Azure PowerShell ```Set-AzureRmSqlServerActiveDirectoryAdministrator``` voorkomt niet dat u Azure AD admins voor niet-ondersteunde gebruikers ingericht. Een niet-ondersteunde gebruiker kan worden ingericht, maar kan geen verbinding maken met een database. (Zie de lijst met ondersteunde admins in **Azure AD functies en beperkingen** hierboven).

In het volgende voorbeeld wordt de optionele **object-id**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23"
–ServerName "demo_server" -DisplayName "DBA_Group" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353f"
```

> [AZURE.NOTE] De Azure AD **object-id** is vereist wanneer de **weergavenaam** niet uniek is. De **object-id** en de **weergavenaam** om waarden te vinden, gebruikt het Active Directory-gedeelte van de klassieke Azure-Portal, en de eigenschappen van een gebruiker of groep.

In het volgende voorbeeld geeft als resultaat informatie over de huidige Azure AD admin voor Azure SQL Server:

```
Get-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23" –ServerName "demo_server" | Format-List
```

In het volgende voorbeeld wordt een beheerder AD Azure verwijderd:
```
Remove-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" –ServerName "demo_server"
```

U kunt ook een beheerder Azure Active Directory met behulp van de API's van REST inrichten. Zie voor meer informatie, [Service Management REST API Reference en bewerkingen voor Azure SQL Databases bewerkingen voor Azure SQL-Databases](https://msdn.microsoft.com/library/azure/dn505719.aspx)

## <a name="5-configure-your-client-computers"></a>5. de clientcomputers configureren

Op alle clientcomputers waarop toepassingen of gebruikers verbinding maken met Azure SQL-Database of Azure SQL Data Warehouse met Azure AD identiteiten, moet u de volgende software installeren:

- .NET framework 4.6 of hoger van [https://msdn.microsoft.com/library/5a4x27ek.aspx](https://msdn.microsoft.com/library/5a4x27ek.aspx).
- Azure Active Directory-verificatie-bibliotheek voor SQL Server (**ADALSQL. DLL-bestand**) is beschikbaar in meerdere talen (x86 en amd64) van het download center op de [Microsoft Active Directory verificatie-bibliotheek voor Microsoft SQL Server](http://www.microsoft.com/download/details.aspx?id=48742).

### <a name="tools"></a>Hulpprogramma 's

- Installeren van [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) of [SQL Server Data Tools voor Visual Studio 2015](https://msdn.microsoft.com/library/mt204009.aspx) voldoet aan de vereiste .NET Framework 4.6.
- Versie van SSMS wordt geïnstalleerd op de x86 van **ADALSQL. DLL-bestand**.
- SSDT installeert van **ADALSQL de amd64-versie. DLL-bestand**.
- De nieuwste Visual Studio van [Visual Studio downloadt](https://www.visualstudio.com/downloads/download-visual-studio-vs) voldoet aan de vereiste .NET Framework 4.6, maar niet de vereiste amd64-versie van **ADALSQL geïnstalleerd. DLL-bestand**.

## <a name="6-create-contained-database-users-in-your-database-mapped-to-azure-ad-identities"></a>6. gebruikers opgenomen in de database toegewezen aan AD Azure identiteiten maken

### <a name="about-contained-database-users"></a>Over opgenomen databasegebruikers

Azure verificatie van Active Directory moet databasegebruikers worden gemaakt als de databasegebruikers in die. Een ingesloten databasegebruiker op basis van een identiteit Azure AD is een gebruiker die geen een aanmelding in de hoofd database en die wordt toegewezen aan een identiteit in de directory Azure AD die is gekoppeld aan de database. De identiteit van AD Azure is een afzonderlijke gebruikersaccount of groep. Zie [Gebruikers Database opgenomen om uw Database draagbare](https://msdn.microsoft.com/library/ff929188.aspx)voor meer informatie over databasegebruikers in die.

> [AZURE.NOTE] Databasegebruikers (met uitzondering van beheerders) kunnen niet worden gemaakt met behulp van de portal. RBAC-rollen worden niet doorgegeven aan SQL Server, SQL-Database of SQL Data Warehouse. Azure RBAC-rollen worden gebruikt voor het beheren van bronnen Azure en niet van toepassing op machtigingen. De rol van **Inzender voor SQL Server** verleent bijvoorbeeld geen toegang tot de verbinding met de SQL-Database of SQL Data Warehouse. De machtiging tot toegang moet worden verleend rechtstreeks in de database met behulp van Transact-SQL-instructies.

### <a name="connect-to-the-user-database-or-data-warehouse-by-using-sql-server-management-studio-or-sql-server-data-tools"></a>Verbinding maken met de gebruiker-database of het magazijn met behulp van SQL Server Management Studio of SQL Server Data Tools

Ter bevestiging van de beheerder AD Azure is juist instellen, verbinding maken met de **master** -database met de beheerdersaccount Azure AD.
Als u wilt inrichten een Azure AD-gebruiker op basis van die database (met uitzondering van de beheerder van de server die eigenaar is van de database), verbinding met de database met een Azure AD identiteit die toegang tot de database heeft.

> [AZURE.IMPORTANT] Ondersteuning voor Azure Active Directory-verificatie is met [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) en [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) in Visual Studio 2015 beschikbaar. De release augustus 2016 van SSMS biedt ook ondersteuning voor universele verificatie van Active Directory, zodat beheerders meerledige verificatie met behulp van een telefoongesprek, een tekstbericht, smartcards met pin of mobiele app-meldingen nodig.

#### <a name="connect-using-active-directory-integrated-authentication"></a>Verbinding maken met behulp van geïntegreerde verificatie van Active Directory

Gebruik deze methode als u bent aangemeld bij Windows met behulp van uw referenties Azure Active Directory-domein een federatieve.

1. Start Management Studio of hulpmiddelen voor gegevens en selecteer in het dialoogvenster **verbinding maken met Server** (of **verbinding maken met de Database-Engine**) in het vak **verificatie** **Geïntegreerde verificatie van Active Directory**. Geen wachtwoord nodig is of kan worden ingevoerd omdat de bestaande referenties voor de verbinding worden weergegeven.
    ![Selecteer geïntegreerde verificatie van AD][11]

2. Klik op de knop **Opties** en typ de naam van de database die u verbinding wilt maken in het vak **verbinding maken met database** op de pagina **Eigenschappen van de verbinding** .
    ![Selecteer de naam van de database][13]


#### <a name="connect-using-active-directory-password-authentication"></a>Verbinding maken met Active Directory-wachtwoordverificatie

Gebruik deze methode wanneer u verbinding maakt met een AD Azure principal-naam met behulp van de advertentie Azure domein beheerd. U kunt het ook gebruiken voor federatieve account zonder toegang tot het domein, bijvoorbeeld bij het werken op afstand.

Gebruik deze methode als u bent aangemeld bij Windows met behulp van de referenties van een domein dat niet met Azure federated is of wanneer u met Azure AD Azure AD-verificatie op basis van de oorspronkelijke of de domeinnaam van de client.

1. Start Management Studio of hulpmiddelen voor gegevens en selecteer in het dialoogvenster **verbinding maken met Server** (of **verbinding maken met de Database-Engine**) in het vak **verificatie** **Verificatie van Active Directory-wachtwoord**.
2. Typ in het vak **gebruikersnaam** de gebruikersnaam Azure Active Directory in de indeling **username@domain.com**. Dit moet een van de Azure Active Directory-account of een account van een domein communiceren met Azure Active Directory.
3. Typ in het vak **wachtwoord** het gebruikerswachtwoord voor de Azure Active Directory-account of domeinaccount federatieve.
    ![Selecteer AD-wachtwoordverificatie][12]

4. Klik op de knop **Opties** en typ de naam van de database die u verbinding wilt maken in het vak **verbinding maken met database** op de pagina **Eigenschappen van de verbinding** . (Zie de afbeelding in de vorige optie.)


### <a name="create-an-azure-ad-contained-database-user-in-a-user-database"></a>Maken van een databasegebruiker Azure AD opgenomen in een gebruikersdatabase

U maakt een Azure AD-gebruiker op basis van die database (met uitzondering van de beheerder van de server die eigenaar is van de database), verbinding met de database met een identiteit Azure AD als een gebruiker met ten minste de machtiging van **Een gebruiker wijzigen** . Gebruik vervolgens de volgende Transact-SQL-syntaxis:

    CREATE USER <Azure_AD_principal_name>
    FROM EXTERNAL PROVIDER;


*Azure_AD_principal_name* zijn de UPN-naam van een gebruiker Azure AD of de weergegeven naam voor een groep AD Azure.

**Voorbeelden:** Een ingesloten database gebruiker die een advertentie Azure federatieve of domeingebruiker beheerd:

    CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
    CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;

Een ingesloten om databasegebruiker te maken die een Azure advertentie of federatieve domeingroep, bevatten de weergegeven naam van de groep:

    CREATE USER [ICU Nurses] FROM EXTERNAL PROVIDER;

Een ingesloten databasegebruiker die een toepassing die verbinding maakt via een token Azure AD maken:

    CREATE USER [appName] FROM EXTERNAL PROVIDER;

Zie voor meer informatie over het maken van de die databasegebruikers op basis van Azure Active Directory identiteiten [Maken gebruiker (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx).


> [AZURE.NOTE] De Azure Active Directory-beheerder voor Azure SQL Server verwijderen, voorkomt u dat elke gebruiker Azure AD-verificatie verbinding met de server. Indien nodig, onbruikbaar Azure AD-gebruikers kunnen handmatig worden verwijderd door een beheerder van de SQL-Database.

Als u een databasegebruiker maakt, wordt die gebruiker ontvangt de machtiging **VERBINDEN** en als lid van de rol van de **openbare** verbinding kunt maken met die database. In eerste instantie zijn de enige beschikbaar voor de gebruiker machtigingen machtigingen verleend aan de **openbare** rol of machtigingen verleend aan Windows-groepen waarin ze lid van zijn. Zodra u een databasegebruiker Azure opgenomen op basis van AD inrichten, kunt u de gebruiker extra machtigingen toekennen, net zoals u toestemming aan een andere gebruiker verlenen. Meestal machtigen om databaserollen en gebruikers aan rollen toevoegen. Zie voor meer informatie [Basisbeginselen van Database Engine machtiging](http://social.technet.microsoft.com/wiki/contents/articles/4433.database-engine-permission-basics.aspx). Zie voor meer informatie over de specifieke functies van SQL-Database [beheren van Databases en Azure SQL-Database-aanmeldingen](sql-database-manage-logins.md).
Een federatieve gebruiker die wordt geïmporteerd in een domein beheren, moet de identiteit van de beheerde domein gebruiken.

> [AZURE.NOTE] Azure AD-gebruikers zijn gemarkeerd in de databasemetagegevens met E (EXTERNAL_USER) en voor groepen met type X (EXTERNAL_GROUPS). Zie [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx)voor meer informatie.


## <a name="7-connect-by-using-azure-ad-identities"></a>7. verbinding via Azure AD identiteiten

Azure Active Directory-verificatie ondersteunt de volgende methoden van het maken van verbinding met een database met identiteiten Azure AD:

- Met behulp van geïntegreerde Windows-verificatie
- Een Azure AD principal-naam en wachtwoord
- Aanvraag token-verificatie

### <a name="71-connecting-using-integrated-windows-authentication"></a>7.1. Verbinding maken met behulp van geïntegreerde verificatie van (Windows)

Voor het gebruik van geïntegreerde Windows-verificatie, moet de Active Directory van uw domein worden federatieve met Azure Active Directory. De clienttoepassing (of service) verbinden met de database moet worden uitgevoerd op een computer deel uitmaakt van een domein onder de domeinreferenties van een gebruiker.

Als u wilt verbinden met een database met behulp van geïntegreerde verificatie en de identiteit van een Azure AD, moet het sleutelwoord verificatie in de verbindingsreeks voor de database met Active Directory geïntegreerde worden ingesteld. De volgende C#-voorbeeldcode gebruikt ADO .NET.

    string ConnectionString =
    @"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Integrated; Initial Catalog=testdb;";
    SqlConnection conn = new SqlConnection(ConnectionString);
    conn.Open();

Opmerking de connection string, trefwoord dat ``Integrated Security=True`` wordt niet ondersteund voor verbinding met de Azure SQL-Database.
Houd er rekening mee dat wanneer u een ODBC-verbinding u moet spaties verwijderen en het instellen van verificatie van 'ActiveDirectoryIntegrated'.

### <a name="72-connecting-with-an-azure-ad-principal-name-and-a-password"></a>7.2. Verbinding maken met een AD Azure principal-naam en wachtwoord
Als u wilt verbinden met een database met behulp van geïntegreerde verificatie en de identiteit van een Azure AD, moet het sleutelwoord verificatie in Active Directory-wachtwoord worden ingesteld. De verbindingsreeks moet gebruikers-ID/gebruikers-id en wachtwoord/PWD sleutelwoorden en waarden bevatten. De volgende C#-voorbeeldcode gebruikt ADO .NET.

    string ConnectionString =
      @"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Password; Initial Catalog=testdb;  UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!";
    SqlConnection conn = new SqlConnection(ConnectionString);
    conn.Open();

Meer informatie over AD Azure verificatiemethoden met behulp van de codevoorbeelden demo op [Azure AD verificatie GitHub Demo](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth).


### <a name="73-connecting-with-an-azure-ad-token"></a>7.3 verbinding maken met een token Azure AD
Deze verificatiemethode staat middenlaag services verbinding maken met Azure SQL-Database of Azure SQL Data Warehouse met het verkrijgen van een token van Azure Active Directory (AAD). U kunt geavanceerde scenario's, met inbegrip van op certificaten gebaseerde verificatie. Vier basisstappen voor Azure AD token verificatie uit te voeren:

1. Uw toepassing met Azure Active Directory registreren en de client-id ophalen voor uw code. 
2. Maak een databasegebruiker die de toepassing vertegenwoordigt. (In stap 6 eerder voltooid.)
3. Een certificaat op de client-computer werkt de toepassing maken.
4. Het certificaat wordt toegevoegd als een sleutel voor uw toepassing.

Voorbeeld-verbindingsreeks:

```
string ConnectionString =@"Data Source=n9lxnyuzhv.database.windows.net; Initial Catalog=testdb;"
SqlConnection conn = new SqlConnection(ConnectionString);
connection.AccessToken = "Your JWT token"
conn.Open();
```

Zie [SQL Server Security Blog](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/)voor meer informatie.

### <a name="connecting-with-sqlcmd"></a>Verbinding maken met sqlcmd  
De volgende instructies verbinding maken met versie 13,1 van sqlcmd, dat verkrijgbaar is bij het [Download Center](http://go.microsoft.com/fwlink/?LinkID=825643).

```
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net  -G  
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -U bob@contoso.com -P MyAADPassword -G -l 30
```

## <a name="see-also"></a>Zie ook

[Beheer van Databases en Azure SQL-Database-aanmeldingen](sql-database-manage-logins.md)

[De opgenomen gebruikers](https://msdn.microsoft.com/library/ff929071.aspx)

[GEBRUIKER (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx)


<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[9]: ./media/sql-database-aad-authentication/9ad-settings.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/11connect-using-int-auth.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db.png

