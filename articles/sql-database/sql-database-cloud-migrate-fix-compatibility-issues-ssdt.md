<properties
   pageTitle="Corrigeren van compatibiliteitsproblemen van SQL Server-database voor de migratie naar een SQL-Database | Microsoft Azure"
   description="Microsoft Azure SQL-Database, databasemigratie, compatibiliteit, SQL Azure Migration Wizard, SSDT"
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

# <a name="migrate-a-sql-server-database-to-azure-sql-database-using-sql-server-data-tools-for-visual-studio"></a>Een SQL Server-Database migreren naar Azure SQL-Database met behulp van SQL Server Data Tools voor Visual Studio 

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Upgrade Advisor](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

In dit artikel leert u opsporen en oplossen van compatibiliteitsproblemen voor SQL Server-database met behulp van SQL Server Data Tools voor Visual Studio voor de migratie naar Azure SQL-Database.

## <a name="using-sql-server-data-tools-for-visual-studio"></a>Met behulp van SQL Server Data Tools voor Visual Studio

Gebruik SQL Server Data Tools voor Visual Studio ("SSDT") aan het schema van de database importeren in een Visual Studio-project-database voor analyse. Als u wilt analyseren, het doelplatform voor het project als een SQL-Database V12 opgeven en vervolgens het project samenstellen. Als het opbouwen voltooid is, wordt de database compatibel is. Als de build is mislukt, kunt u de fouten in de SSDT (of een van de andere hulpprogramma's die worden beschreven in dit onderwerp) kunt oplossen. Zodra het project is gebaseerd, kunt u deze publiceren als een kopie van de brondatabase terug. Vervolgens kunt u de vergelijkingsfunctie in SSDT de gegevens uit de brondatabase kopiëren naar de Azure SQL V12-compatibele database. Vervolgens kunt u deze bijgewerkte database migreren. Download de [nieuwste versie van SSDT](https://msdn.microsoft.com/library/mt204009.aspx)voor het gebruik van deze optie.

  ![Diagram van VSSSDT migratie](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)

  > [AZURE.NOTE] Als alleen-schema migratie vereist is, kan het schema direct naar Azure SQL Database rechtstreeks vanuit Visual Studio worden gepubliceerd. Gebruik deze methode als het databaseschema meer wijzigingen vereist dan door de migratiewizard alleen kunnen worden verwerkt.

## <a name="detecting-compatibility-issues-using-sql-server-data-tools-for-visual-studio"></a>Opsporen van problemen met de compatibiliteit met SQL Server Data Tools voor Visual Studio
   
1.  De **SQL Server-Object Explorer** openen in Visual Studio. Met **SQL Server toevoegen** verbinding maken met de SQL Server-sessie met de database die wordt gemigreerd. Zoek de database in Object Explorer, klik met de rechtermuisknop op de database en selecteer **Maak nieuw Project...**     
    
    ![Nieuw Project](./media/sql-database-migrate-visualstudio-ssdt/02MigrateSSDT.png)    
   
2.  De instellingen voor het **importeren van objecten alleen**voor importeren configureren. Schakel de opties voor het importeren van het volgende: aanmeldingen, machtigingen en instellingen.    

    ![ALT-tekst](./media/sql-database-migrate-visualstudio-ssdt/03MigrateSSDT.png)    

3.  Klik op **Start** om de database te importeren en het project met een T-SQL-scriptbestand voor elk object in de database te maken. De scriptbestanden zijn genest in mappen binnen het project.    

    ![ALT-tekst](./media/sql-database-migrate-visualstudio-ssdt/04MigrateSSDT.png)    

4.  In de Visual Studio Solution Explorer met de rechtermuisknop op het databaseproject en selecteer Eigenschappen. Configureer het doelplatform naar Microsoft Azure SQL Database V12 op de pagina **Instellingen voor het Project** .    
    
    ![ALT-tekst](./media/sql-database-migrate-visualstudio-ssdt/05MigrateSSDT.png)    
    
5.  Klik met de rechtermuisknop op het project en selecteer **Build** maken van het project.    
    
    ![ALT-tekst](./media/sql-database-migrate-visualstudio-ssdt/06MigrateSSDT.png)    
    
6.  Elke incompatibiliteit in de **Lijst met fouten** weergegeven. In dit geval is de naam NT AUTHORITY\NETWORK SERVICE niet compatibel. Omdat het niet compatibel is, kunt u het commentaar of verwijderen (en adres van de gevolgen van het verwijderen van deze aanmelding en de rol van de database-oplossing).     
    
    ![ALT-tekst](./media/sql-database-migrate-visualstudio-ssdt/07MigrateSSDT.png)    
    
## <a name="fixing-compatibility-issues-using-sql-server-data-tools-for-visual-studio"></a>Oplossen van problemen met de compatibiliteit met SQL Server Data Tools voor Visual Studio

1.  Dubbelklik op het eerste script om het script in een query-venster en een opmerking uit het script openen en vervolgens het script uitvoeren.     
    ![ALT-tekst](./media/sql-database-migrate-visualstudio-ssdt/08MigrateSSDT.png)

2.  Herhaal deze procedure voor elk script met incompatibiliteiten totdat er geen fout zijn.    
    ![ALT-tekst](./media/sql-database-migrate-visualstudio-ssdt/09MigrateSSDT.png)
    
3.  Wanneer de database vrij van fouten is, met de rechtermuisknop op het project en selecteer **publiceren**. Een kopie van de brondatabase is samengesteld en gepubliceerd (het is raadzaam een kopie, ten minste in eerste instantie gebruik).     
 - Voordat u publiceert, afhankelijk van de bron SQL Server (eerdere versie dan SQL Server 2014), wellicht opnieuw instellen van het project target platform voor implementatie mogelijk te maken.     
 - Als u een SQL Server-database ouder migreert, doen niet introduceren functies in het project die niet worden ondersteund in SQL Server van de bron tot de database te migreren naar een nieuwere versie van SQL Server.     

        ![alt text](./media/sql-database-migrate-visualstudio-ssdt/10MigrateSSDT.png)    
    
        ![alt text](./media/sql-database-migrate-visualstudio-ssdt/11MigrateSSDT.png)    
        
4.  SQL Server-Object Explorer met de rechtermuisknop op de brondatabase en klik op de **Vergelijking van de gegevens**. Het project met de oorspronkelijke database te vergelijken kunt u zien welke wijzigingen zijn aangebracht door de wizard. Selecteer uw Azure SQL V12-versie van de database en klik vervolgens op **Voltooien**.    
    
    ![ALT-tekst](./media/sql-database-migrate-visualstudio-ssdt/12MigrateSSDT.png)    
    
    ![ALT-tekst](./media/sql-database-migrate-visualstudio-ssdt/13MigrateSSDT.png)    

5.  Bekijk de gevonden verschillen en klik op **Update doel** om gegevens te migreren vanuit de brondatabase in de V12 van Azure SQL-database.     
    
    ![ALT-tekst](./media/sql-database-migrate-visualstudio-ssdt/14MigrateSSDT.png)    
    
6.  Kies een implementatiemethode. Zie [SQL Server-databases compatibel met SQL-Database migreren.](sql-database-cloud-migrate.md)  

## <a name="next-steps"></a>Volgende stappen

- [Nieuwste versie van SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nieuwste versie van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Aanvullende bronnen

- [SQL Database V12](sql-database-v12-whats-new.md)
- [Transact-SQL gedeeltelijk of niet-ondersteunde functies](sql-database-transact-sql-information.md)
- [Migreren van SQL Server databases SQL Server migratie-assistent](http://blogs.msdn.com/b/ssma/)
