<properties
   pageTitle="SQL Server-databasemigratie naar de SQL-Database | Microsoft Azure"
   description="Informatie over hoe op ruimten SQL Server-databasemigratie naar Azure SQL-Database in de cloud. Hulpprogramma's voor migratie gebruiken voor het testen van compatibiliteit voorafgaand aan de databasemigratie."
   keywords="database migratie, migratie van sql server-database, hulpprogramma's voor migratie, migreren, database, sql-database migreren"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-migrate"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="sql-server-database-migration-to-sql-database-in-the-cloud"></a>SQL Server-databasemigratie naar de SQL-Database in de cloud

In dit artikel leert u hoe een op ruimten SQL Server 2005 database of hoger met Azure SQL-Database migreren. In dit databasemigratieproces migreren u uw schema en de gegevens uit de SQL Server-database in uw huidige omgeving naar een SQL-Database. Om te slagen, moet de bestaande database eerst een test compatibele doorgeven. Met [SQL Database V12](sql-database-v12-whats-new.md)zijn er enkele resterende problemen met de compatibiliteit, dan het probleem betrekking op het niveau van de server en meerdere databases. Databases en toepassingen die afhankelijk van zijn [gedeeltelijk of niet-ondersteunde functies](sql-database-transact-sql-information.md) moeten sommige herstructureren om deze compatibiliteitsproblemen oplossen voordat de SQL Server-database kan worden gemigreerd.

Als u wilt migreren, de volgende zijn de stappen u moet ondernemen:

- **Testen op compatibiliteit**: databasecompatibiliteit met [SQL Database V12](sql-database-v12-whats-new.md)valideren. 
- **Oplossen problemen met de compatibiliteit, indien van toepassing**: als validatie mislukt, moet u de validatiefouten corrigeren.  
- **De migratie uitvoeren** Als uw database compatibel is, kunt u een of meer methoden voor het uitvoeren van de migratie. 

SQL Server biedt verschillende methoden beschreven voor het uitvoeren van deze taken. Dit artikel bevat een overzicht van de beschikbare methoden voor elke taak. In het volgende diagram ziet u de stappen en de methoden.

  ![Diagram van VSSSDT migratie](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)
  
 > [AZURE.NOTE] Zie [SQL Server Migration Assistant](http://blogs.msdn.com/b/ssma/)om te migreren van een SQL Server-database, Microsoft Access, Sybase, MySQL, Oracle en DB2 Azure SQL-database, inclusief.

## <a name="database-migration-tools-test-sql-server-database-compatibility-with-sql-database"></a>Hulpprogramma's voor migratie testen van SQL Server-databasecompatibiliteit met SQL-Database

U kunt testen op compatibiliteitsproblemen SQL-Database voordat u begint met het databasemigratieproces met een van de volgende methoden:

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Upgrade Advisor](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

- [SQL Server Data Tools voor Visual Studio ("SSDT")](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): SSDT gebruikt de meest recente compatibele regels voor het detecteren van SQL-Database V12 incompatibiliteiten. Als er incompatibiliteiten worden aangetroffen, kunt u de gedetecteerde problemen oplossen rechtstreeks in dit hulpprogramma. Deze methode is de aanbevolen methode voor het testen en oplossen van compatibiliteitsproblemen SQL Database V12. 
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md): SqlPackage is een opdrachtregelprogramma dat wordt getest voor compatibiliteit problemen en genereert een rapport met compatibiliteitsproblemen aangetroffen. Als u dit hulpprogramma gebruikt, moet u de meest recente versie te gebruiken van de meest recente regels voor compatibiliteit. Als er fouten zijn gevonden, wordt moet u een ander hulpprogramma voor het oplossen van eventuele gevonden compatibiliteitsproblemen - SSDT aanbevolen.  
- [De gegevenslaag exporteren application wizard in SQL Server Management Studio](sql-database-cloud-migrate-determine-compatibility-ssms.md): deze wizard detecteert en fouten rapporteert aan het scherm. Als fouten worden aangetroffen, kunt u doorgaan en de migratie naar een SQL-Database. Als er fouten zijn gevonden, wordt moet u een ander hulpprogramma voor het oplossen van eventuele gevonden compatibiliteitsproblemen - SSDT aanbevolen.
- [De Microsoft SQL Server 2016 Upgrade Advisor Preview](http://www.microsoft.com/download/details.aspx?id=48119): Dit zelfstandige programma heet, die zich in het voorbeeld, detecteert en genereert een rapport met de SQL-Database V12 compatibiliteitsproblemen. Dit hulpprogramma nog niet de meest recente regels voor compatibiliteit. Als er geen fouten zijn gevonden, kunt u doorgaan en de migratie naar een SQL-Database. Als er fouten zijn gevonden, wordt moet u een ander hulpprogramma voor het oplossen van eventuele gevonden compatibiliteitsproblemen - SSDT aanbevolen. 
- [SQL Azure Migration Wizard ("SAMW")](sql-database-cloud-migrate-fix-compatibility-issues.md): SAMW is een codeplex dat de regels van de compatibiliteit van Azure SQL Database V11: gebruikt Azure SQL Database V12 incompatibiliteiten detecteren. Als er incompatibiliteiten worden gedetecteerd, kunnen sommige problemen rechtstreeks in dit hulpprogramma worden vastgesteld. Dit hulpprogramma is wellicht onverenigbaarheden die niet hoeven te worden vastgesteld. Het is de eerste Azure SQL-Database migreren hulpprogramma hulp beschikbaar en actief wordt ondersteund door de Gemeenschap van SQL Server. Dit hulpprogramma kan ook uitvoeren van de migratie uit in het hulpprogramma zelf. 

## <a name="fix-database-migration-compatibility-issues"></a>De databasemigratie compatibiliteitsproblemen oplossen

Als er compatibiliteitsproblemen worden gedetecteerd, moet u deze oplossen voordat u verdergaat met de migratie van SQL Server-database. Er zijn tal van compatibiliteitsproblemen die u tegenkomen kunt, beide, afhankelijk van de versie van SQL Server in de brondatabase en de complexiteit van de database die u wilt migreren. Oudere versies van SQL Server hebben meer problemen met de compatibiliteit. Gebruik de volgende bronnen, naast een gerichte zoeken op Internet met behulp van de zoekmachine van keuze:

- [Onderdelen van SQL Server-database niet worden ondersteund in Azure SQL-Database](sql-database-transact-sql-information.md)
- [Niet meer leverbaar-Database-Engine functionaliteit in SQL Server 2016](https://msdn.microsoft.com/library/ms144262%28v=sql.130%29)
- [Niet meer leverbaar-Database-Engine functionaliteit in SQL Server 2014](https://msdn.microsoft.com/library/ms144262%28v=sql.120%29)
- [Niet meer leverbaar-Database-Engine functionaliteit in SQL Server 2012](https://msdn.microsoft.com/library/ms144262%28v=sql.110%29)
- [Niet meer leverbaar-Database-Engine functionaliteit in SQL Server 2008 R2](https://msdn.microsoft.com/library/ms144262%28v=sql.105%29)
- [Niet meer leverbaar-Database-Engine functionaliteit in SQL Server 2005](https://msdn.microsoft.com/library/ms144262%28v=sql.90%29)

Naast zoeken op het Internet en deze bronnen gebruikt, [StackOverflow](http://stackoverflow.com/)of [gemeenschapsforums MSDN SQL Server](https://social.msdn.microsoft.com/Forums/sqlserver/home?category=sqlserver) te gebruiken.

Gebruik een van de volgende database Migratietools om de gevonden problemen opgelost:

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

- Gebruik [SQL Server Data Tools voor Visual Studio ("SSDT")](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): voor het gebruik van SSDT u importeert het schema van uw database in SQL Server Data Tools voor Visual Studio "SSDT") en bouwt het project voor de implementatie van een SQL-Database V12. U kunt vervolgens alle gevonden compatibiliteitsproblemen oplossen in SSDT. Als alles klaar is, synchroniseert u de wijzigingen naar de brondatabase (of een kopie van de brondatabase. SSDT is de aanbevolen methode voor het testen en oplossen van compatibiliteitsproblemen SQL Database V12. Volg de link voor een [overzicht met behulp van SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md).
- Gebruik [SQL Server Management Studio ("SSMS")](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md): voor het gebruik van SSMS u fouten opgespoord met behulp van een ander gereedschap Transact-SQL-opdrachten uitvoeren. Deze methode is voornamelijk bedoeld voor ervaren gebruikers voor het wijzigen van het schema van de database rechtstreeks in de brondatabase. 
- Gebruik [SQL Azure Migration Wizard ("SAMW")](sql-database-cloud-migrate-fix-compatibility-issues.md): voor het gebruik van SAMW u een Transact-SQL-script genereren uit de brondatabase. De wizard zet het script, indien mogelijk, het schema om compatibel te maken met de SQL-Database V12. Volledige, SAMW verbinding kunnen maken met SQL-Database V12 het script wilt uitvoeren. Deze tool analyseert ook traceringsbestanden om compatibiliteitsproblemen vast te stellen. Het script met schema alleen kan worden gegenereerd of gegevens kunt opnemen in de BCP-indeling.

## <a name="migrate-a-compatible-sql-server-database-to-sql-database"></a>SQL Server-databases compatibel met SQL-Database migreren

De migratie van een compatibele SQL Server-database, biedt Microsoft verschillende migratiemethoden voor verschillende scenario's. Welke methode die u kiest, hangt af van de tolerantie voor downtime, de grootte en complexiteit van uw SQL Server-database en de verbinding met de cloud met Microsoft Azure.  

> [AZURE.SELECTOR]
- [SSMS migratiewizard](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [Exporteren naar bestand Bacpac-](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Importeren uit bestand Bacpac-](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Transactionele replicatie](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

Als u de migratiemethode, is de eerste vraag als u zich veroorloven kunt te nemen van de database productie tijdens de migratie. Een database migreren tijdens actieve transacties zijn kan leiden tot inconsistenties database en de database is mogelijk beschadigd. Er zijn veel methoden stilleggen, een database van clients verbinding maken met het maken van een [momentopname van de database](https://msdn.microsoft.com/library/ms175876.aspx)uitschakelen.

Met een minimale uitval migreren, [transactie replicatie van SQL Server](sql-database-cloud-migrate-compatible-using-transactional-replication.md) te gebruiken als uw database voldoet aan de vereisten voor transactionele replicatie. Als u zich enkele uitvaltijd kunt veroorloven, of u een testmigratie van een productiedatabase voor latere migratie uitvoert, kunt u overwegen een van de volgende drie methoden:

- [De migratiewizard SSMS](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md): voor kleine tot middelgrote databases, het migreren van een compatibele SQL Server 2005 database of hoger is net zo eenvoudig als het uitvoeren van de [Wizard Microsoft Azure Database Database implementeren](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md) in SQL Server Management Studio.
- [Bacpac-bestand exporteren](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) en [importeren uit bestand Bacpac-](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md): als uw uitdagingen connectiviteit (geen connectiviteit, lage bandbreedte of time-out problemen) en voor middelgrote tot grote databases, een [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) -bestand. Met deze methode wordt exporteren u de SQL Server-schema en de gegevens naar een Bacpac-bestand. U importeren het Bacpac-bestand vervolgens in een SQL-Database met behulp van de Data Tier toepassing Wizard exporteren in SQL Server Management Studio of het opdrachtregelprogramma [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) .
- Bacpac- en BCP samen gebruiken: een [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) -bestand en [BCP](https://msdn.microsoft.com/library/ms162802.aspx) voor veel grotere databases gebruiken voor een groter parallelization voor prestaties verhoogt, weliswaar met grotere complexiteit. Met deze methode kunnen migreren van het schema en de gegevens afzonderlijk.
 - [Het schema alleen naar een Bacpac-bestand exporteren](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md).
 - [Het schema alleen uit het Bacpac-bestand importeren](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) in de SQL-Database.
 - [BCP](https://msdn.microsoft.com/library/ms162802.aspx) uitpakken de gegevens in bestanden en vervolgens [parallel laden](https://technet.microsoft.com/library/dd425070.aspx) deze bestanden in Azure SQL-Database gebruiken.

     ![SQL Server-database migratie - SQL-database migreren naar de cloud.](./media/sql-database-cloud-migrate/01SSMSDiagram_new.png)

## <a name="next-steps"></a>Volgende stappen

- [De Microsoft SQL Server 2016 Upgrade Advisor Preview](http://www.microsoft.com/download/details.aspx?id=48119)
- [Nieuwste versie van SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nieuwste versie van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

##<a name="additional-resources"></a>Aanvullende bronnen

- [SQL Database V12](sql-database-v12-whats-new.md)
[Transact-SQL gedeeltelijk of niet-ondersteunde functies](sql-database-transact-sql-information.md)
- [Migreren van SQL Server databases SQL Server migratie-assistent](http://blogs.msdn.com/b/ssma/)
