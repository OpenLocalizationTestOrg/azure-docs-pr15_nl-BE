<properties
   pageTitle="Beveiligen van een database in SQL Data Warehouse | Microsoft Azure"
   description="Tips voor het beveiligen van een Azure SQL Data Warehouse-database voor het ontwikkelen van oplossingen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/24/2016"
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="secure-a-database-in-sql-data-warehouse"></a>Een SQL Data Warehouse-database beveiligen

> [AZURE.SELECTOR]
- [Beveiliging-overzicht](sql-data-warehouse-overview-manage-security.md)
- [Verificatie](sql-data-warehouse-authentication.md)
- [Codering (Portal)](sql-data-warehouse-encryption-tde.md)
- [Codering (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

Dit artikel helpt bij de basisbeginselen van de beveiliging van uw Azure SQL Data Warehouse-database. In het bijzonder in dit artikel krijgt u aan de slag met de middelen voor het beperken van toegang, bescherming van gegevens en controle van de activiteiten van een database.

## <a name="connection-security"></a>Beveiliging van de verbinding

Beveiliging van de verbinding verwijst naar hoe u beperken en beveiligde verbindingen met uw database met behulp van firewall-regels en versleuteling voor verbinding.

Firewallregels worden gebruikt door de server en de database te weigeren van verbindingspogingen van IP-adressen die niet expliciet whitelisted zijn. Als u verbindingen van uw toepassing of het openbare IP-adres van de clientcomputer, moet u eerst een firewallregel voor een niveau van de server met behulp van de Portal Azure, PowerShell of REST API maken. Beste, moet u de IP-adresbereiken die via de firewall van uw server toegestaan voor zoveel mogelijk te beperken.  Voor toegang tot Azure SQL Data Warehouse van uw lokale computer, zorgen dat de firewall op het netwerk en de lokale computer kan uitgaande communicatie via TCP-poort 1433.  Zie [firewall Azure SQL-Database][], [sp_set_firewall_rule][]en [sp_set_database_firewall_rule][]voor meer informatie.

Verbindingen met uw magazijn SQL-gegevens worden standaard gecodeerd.  Wijzigen instellingen codering uitschakelen, worden genegeerd.

## <a name="authentication"></a>Verificatie

Verificatie verwijst naar de manier waarop u uw identiteit bewijzen wanneer u verbinding maakt met de database. SQL Data Warehouse ondersteunt momenteel de SQL Server-verificatie met een gebruikersnaam en wachtwoord, alsmede een Azure Active Directory. 

Wanneer u de logische server voor de database hebt gemaakt, moet u een 'server admin' aanmelding met een gebruikersnaam en wachtwoord opgegeven. Deze referenties gebruikt, kunt u verifiëren met de database op die server als database-eigenaar of 'dbo' door middel van SQL Server-verificatie.

Beste, dient gebruikers van uw organisatie echter gebruiken een andere account te verifiëren. Op deze manier kunt u de machtigingen die zijn toegekend aan de toepassing beperken en verminderen de risico's van schadelijke activiteiten als uw toepassingscode kwetsbaar voor een aanval van SQL-injecties is. 

Een SQL Server-geverifieerde gebruiker maken, verbinding maken met de **master** -database op de server met uw server admin login en maak een nieuwe server-aanmelding.  Bovendien is het een goed idee om een gebruiker in de hoofddatabase Azure SQL Data Warehouse-gebruikers maken. Maken van een gebruiker in het model kan een gebruiker aan te melden met behulp van hulpprogramma's zoals SSMS zonder de naam van een database.  Ook kunnen ze het object explorer gebruiken om weer te geven van alle databases op een SQL server.

```sql
-- Connect to master database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'Str0ng_password';
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Vervolgens verbinding maken met de **SQL Data Warehouse-database** met uw server admin aanmelden en een database maken op basis van de server-aanmelding die u zojuist hebt gemaakt.

```sql
-- Connect to SQL DW database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Als een gebruiker extra bewerkingen zoals het maken van aanmeldingen of nieuwe databases doen gaan, moeten ze ook worden toegewezen aan de `Loginmanager` en `dbmanager` rollen in de hoofddatabase. Zie voor meer informatie over deze aanvullende rollen en verificatie bij een SQL-Database [databases beheren en Azure SQL-Database-aanmeldingen][].  Zie [verbinding maken met SQL gegevens magazijn met behulp van Azure Active Directory-verificatie][]voor meer informatie over Azure AD voor SQL Data Warehouse.


## <a name="authorization"></a>Autorisatie

Vergunning verwijst naar wat u in een Azure SQL Data Warehouse-database doen kunt, en dit wordt bepaald door de machtigingen en lidmaatschappen van uw gebruikersaccount rol. Beste, moet u gebruikers de minste bevoegdheden verlenen. Azure SQL Data Warehouse maakt dit eenvoudig te beheren met T-SQL-rollen:

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser to read data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser to write data
```

De u verbinding met maakt server admin-account is lid van db_owner, die bevoegd is te ondernemen binnen de database. Sla deze account voor het implementeren van schema-upgrades en andere beheertaken uit te voeren. "ApplicationUser"-account met beperkte machtigingen vanuit de toepassing naar de database met de minste bevoegdheden die nodig zijn voor uw toepassing verbinding te gebruiken.

Er zijn manieren om verder te beperken wat een gebruiker kan doen met Azure SQL-Database:

- Gedetailleerde [machtigingen][] kunt u bepalen welke bewerkingen kunt u doen in afzonderlijke kolommen, tabellen, weergaven, procedures en andere objecten in de database. Granulaire machtigingen gebruiken de meeste controle hebt en de minimaal vereiste machtigingen verlenen. Het systeem granulaire machtigingen enigszins gecompliceerd is en moet u sommige onderzoek effectief te gebruiken.
- [Databaserollen][] dan db_datareader en db_datawriter kunnen worden gebruikt voor het maken van krachtige toepassing gebruikersaccounts of minder krachtige beheer rekeningen. De ingebouwde vaste databaserollen bieden een gemakkelijke manier om machtigingen te verlenen, maar kunnen dit ertoe leiden dat meer machtigingen dan nodig zijn.
- [Opgeslagen procedures][] kunnen worden gebruikt om de acties beperken die kunnen worden genomen in de database.

Databases en logische servers beheren vanaf de Azure klassieke Portal of met behulp van de API Azure Resource Manager wordt beheerd door uw portal gebruikersaccount roltoewijzingen. Zie [op rollen gebaseerde toegangscontrole in Azure Portal][]voor meer informatie over dit onderwerp.

## <a name="encryption"></a>Codering

Azure SQL gegevens magazijn transparante gegevens codering (TDE) biedt beveiliging tegen het gevaar van schadelijke activiteiten door middel van real-time codering en decodering van gegevens in rust.  Wanneer u een database codeert, worden gekoppelde back-ups en transactielogboekbestanden gecodeerd zonder de wijzigingen in uw toepassingen. De opslag van een volledige database codeert TDE met behulp van een symmetrische sleutel die de database coderingssleutel genoemd. In de SQL-Database wordt de database coderingssleutel beschermd door een ingebouwde server-certificaat. De ingebouwde servercertificaat is uniek voor elke Database van SQL server. Microsoft draait deze certificaten automatisch ten minste elke 90 dagen. De coderingsalgoritme die wordt gebruikt door SQL Data Warehouse is AES-256. Zie voor een algemene beschrijving van TDE [Transparante gegevenscodering][].

U kunt uw database met behulp van de [Portal Azure] coderen[ Encryption with Portal] of [T-SQL][Encryption with TSQL].

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie en voorbeelden voor het verbinden met uw SQL Data Warehouse met verschillende protocollen, [verbinding maken met SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[Verbinding maken met SQL datawarehouse]: ./sql-data-warehouse-connect-overview.md
[Encryption with Portal]: ./sql-data-warehouse-encryption-tde.md
[Encryption with TSQL]: ./sql-data-warehouse-encryption-tde-tsql.md
[Verbinding maken met SQL datawarehouse met Azure Active Directory-verificatie]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[Azure SQL-Database-firewall]: https://msdn.microsoft.com/library/ee621782.aspx
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx
[Databaserollen]: https://msdn.microsoft.com/library/ms189121.aspx
[Beheer van databases en Azure SQL-Database-aanmeldingen]: https://msdn.microsoft.com/library/ee336235.aspx
[Machtigingen]: https://msdn.microsoft.com/library/ms191291.aspx
[Opgeslagen procedures]: https://msdn.microsoft.com/library/ms190782.aspx
[Transparante gegevenscodering]: https://msdn.microsoft.com/library/bb934049.aspx
[Azure portal]: https://portal.azure.com/

<!--Other Web references-->
[Toegangsbeheer op basis van rollen in Azure Portal]: https://azure.microsoft.com/documentation/articles/role-based-access-control-configure
