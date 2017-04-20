<properties
   pageTitle="Niet ondersteund in Azure SQL Database T-SQL | Microsoft Azure"
   description="Transact-SQL-instructies die minder dan volledig worden ondersteund in Azure SQL-Database"
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
   ms.date="08/30/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="azure-sql-database-transact-sql-differences"></a>Verschillen met Azure SQL Database Transact-SQL


De meeste van de toepassingen die afhankelijk van zijn Transact-SQL-functies worden ondersteund in Microsoft SQL Server en Azure SQL-Database. Er volgt een gedeeltelijke lijst van ondersteunde functies voor toepassingen:

- Gegevenstypen.
- Operatoren.
- String, meetkundige, logische, en cursor functies.

Azure SQL-Database is echter alleen bedoeld voor het isoleren van functies van alle afhankelijkheden van de **master** -database. Als gevolg hiervan veel activiteiten van het niveau van de server zijn niet geschikt voor SQL-Database en worden niet ondersteund. Functies die in SQL Server zijn afgeschaft worden over het algemeen niet ondersteund in SQL-Database.

> [AZURE.NOTE]
> Dit onderwerp wordt besproken van de functies die beschikbaar voor SQL-Database zijn bij upgrade naar de huidige versie; SQL Database V12. Zie voor meer informatie over V12 [SQL Database V12 wat de nieuwe](sql-database-v12-whats-new.md).

De volgende secties tonen de functies die gedeeltelijk worden ondersteund en welke functies worden volledig ondersteund.


## <a name="features-partially-supported-in-sql-database-v12"></a>Gedeeltelijk ondersteund in SQL Database V12 functies

SQL Database V12 ondersteunt sommige maar niet alle argumenten die bestaan in de overeenkomstige SQL Server 2016 Transact-SQL-instructies. De instructie CREATE PROCEDURE is bijvoorbeeld beschikbaar maar de opties van de CREATE PROCEDURE niet beschikbaar zijn. De syntaxis van de gekoppelde onderwerpen voor meer informatie over de ondersteunde gebieden van elke instructie verwijzen.

- Databases: [CREATE](https://msdn.microsoft.com/library/dn268335.aspx )/[wijzigen](https://msdn.microsoft.com/library/ms174269.aspx)
- DMVs zijn meestal beschikbaar voor functies die beschikbaar zijn.
- Functies: [CREATE](https://msdn.microsoft.com/library/ms186755.aspx)/[functie wijzigen](https://msdn.microsoft.com/library/ms186967.aspx)
- [KILL](https://msdn.microsoft.com/library/ms173730.aspx) 
- Aanmeldingen: [CREATE](https://msdn.microsoft.com/library/ms189751.aspx)/[aanmelding wijzigen](https://msdn.microsoft.com/library/ms189828.aspx)
- Opgeslagen procedures: [CREATE](https://msdn.microsoft.com/library/ms187926.aspx)/[ALTER PROCEDURE](https://msdn.microsoft.com/library/ms189762.aspx)
- Tabellen: [CREATE](https://msdn.microsoft.com/library/dn305849.aspx)/[wijzigen](https://msdn.microsoft.com/library/ms190273.aspx)
- Typen (aangepast): [TYPE maken](https://msdn.microsoft.com/library/ms175007.aspx)
- Gebruikers: [CREATE](https://msdn.microsoft.com/library/ms173463.aspx)/[gebruiker wijzigen](https://msdn.microsoft.com/library/ms176060.aspx)
- Weergaven: [CREATE](https://msdn.microsoft.com/library/ms187956.aspx)/[weergave wijzigen](https://msdn.microsoft.com/library/ms173846.aspx)

## <a name="features-not-supported-in-sql-database"></a>Functies die niet worden ondersteund in SQL-Database

- Sortering van systeemobjecten
- Verbinding gerelateerd: ORIGINAL_DB_NAME-eindpunt instructies. SQL-Database biedt geen ondersteuning voor Windows-verificatie, maar de vergelijkbare Azure Active Directory-verificatie ondersteunt. Sommige verificatietypen moeten de nieuwste versie van SSMS. Zie [verbinding maken met de SQL-Database of SQL gegevens magazijn met behulp van Azure Active Directory-verificatie](sql-database-aad-authentication.md)voor meer informatie.
- Cross-databasequery's met drie of vier deel namen. (Alleen-lezen cross-databasequery's worden ondersteund met [elastische databasequery](sql-database-elastic-query-overview.md).)
- Cross database chaining van eigendom, betrouwbare instelling
- Gegevens verzamelen
- Databasediagrammen
- Database Mail
- DATABASEPROPERTY (gebruik in plaats daarvan DATABASEPROPERTYEX)
- EXECUTE AS aanmeldingen
- Codering: extensible Sleutelbeheer
- Eventing: gebeurtenissen, meldingen, queryberichten
- Voorzieningen met betrekking tot de plaatsing van de database-bestanden, de grootte en -bestanden die automatisch worden beheerd door Microsoft Azure.
- Functies die betrekking hebben op de beschikbaarheid wordt beheerd via uw account Microsoft Azure: back-up maken, terugzetten, AlwaysOn, database-mirroring, logboekbestanden, herstelmodi. Voor meer informatie, Zie Azure SQL Database back-up en terugzetten.
- Functies die zijn gebaseerd op de logboekweergave die is uitgevoerd op een SQL-Database: Push-replicatie, wijziging gegevens vastleggen.
- Functies die zijn gebaseerd op de SQL Server Agent of de database MSDB: taken, waarschuwingen en operators, op een beleid gebaseerd beheer, database mail, centraal van beheerservers.
- FILESTREAM
- Functies: fn_get_sql, fn_virtualfilestats, fn_virtualservernodes
- Globale tijdelijke tabellen
- Hardware-gerelateerde instellingen: geheugen, werkthreads, CPU-affiniteit, trace, vlaggen, enz. Serviceniveaus te gebruiken.
- HAS_DBACCESS
- KILL STATISTIEKEN TAAK
- Gekoppelde servers, OPENQUERY OPENROWSET, OPENDATASOURCE, BULK INSERT en vierdelige namen
- Master/doelservers
- .NET framework [CLR-integratie met SQL Server](http://msdn.microsoft.com/library/ms254963.aspx)
- Resource-gouverneur
- Semantisch zoeken
- De referenties. Database gebruiken binnen het bereik van referenties in plaats daarvan.
- Items op server niveau: Server-rollen, IS_SRVROLEMEMBER, sys.login_token. Servermachtigingen op gebruikersniveau zijn niet beschikbaar, hoewel sommige worden vervangen door de machtigingen database op. Sommige DMVs niveau van de server zijn niet beschikbaar, hoewel sommige worden vervangen door de database-niveau DMVs.
- Als u kiest express: localdb, gebruikersexemplaren
- Service broker
- SET REMOTE_PROC_TRANSACTIONS
- AFSLUITEN
- sp_addmessage
- sp_configure opties en opnieuw te configureren. Sommige opties zijn beschikbaar met [ALTER DATABASE binnen het bereik van configuratie](https://msdn.microsoft.com/library/mt629158.aspx).
- sp_helpuser
- sp_migrate_user_to_contained
- Controle van SQL Server. Gebruik in plaats daarvan controleren de SQL-Database.
- SQL Server Profiler
- SQL Server-trace
- Hiermee traceert u vlaggen. Sommige artikelen traceren vlag zijn verplaatst naar de compatibiliteitsmodus.
- Foutopsporing in Transact-SQL
- Triggers: Binnen het bereik van Server of aanmelding triggers
- Instructie voor gebruik: om de databasecontext naar een andere database wilt wijzigen, moet u een nieuwe verbinding met de nieuwe database maken.


## <a name="full-transact-sql-reference"></a>Volledige Transact-SQL reference

Zie de [Transact-SQL Reference (Database-Engine)](https://msdn.microsoft.com/library/bb510741.aspx) in SQL Server Books Online voor meer informatie over Transact-SQL-grammatica, syntaxis en voorbeelden. 

### <a name="about-the-applies-to-tags"></a>De codes 'Van toepassing op'

De Transact-SQL reference bevat onderwerpen over versies van SQL Server 2008 bij de huidige. Onder de titel van het onderwerp is een pictogram staaf-, waarin de vier SQL Server-platforms en die toepassing aangeeft. Beschikbaarheidsgroepen zijn bijvoorbeeld geïntroduceerd in SQL Server 2012. Het onderwerp [Maken AVAILABILTY groep](https://msdn.microsoft.com/library/ff878399.aspx) geeft aan dat de instructie van toepassing is op ** SQL Server (vanaf 2012). De instructie is niet van toepassing op SQL Server 2008, SQL Server 2008 R2, Azure SQL-Database, Azure SQL Data Warehouse of Parallel datawarehouse.

In sommige gevallen wordt een algemeen onderwerp van een onderwerp in een product kan worden gebruikt, maar er zijn kleine verschillen tussen producten. De verschillen worden aangegeven bij de middelpunten in het onderwerp.

