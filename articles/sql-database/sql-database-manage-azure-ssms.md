<properties 
    pageTitle="Beheren van een SQL-Database met SSMS | Microsoft Azure" 
    description="Informatie over het beheren van de SQL-Database servers en databases met SQL Server Management Studio." 
    services="sql-database" 
    documentationCenter=".net" 
    authors="stevestein" 
    manager="jhubbard" 
    editor="tysonn"/>

<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="sstein"/>


# <a name="managing-azure-sql-database-using-sql-server-management-studio"></a>Azure SQL-Database met behulp van SQL Server Management Studio beheren 


> [AZURE.SELECTOR]
- [Azure portal](sql-database-manage-portal.md)
- [SSMS](sql-database-manage-azure-ssms.md)
- [PowerShell](sql-database-manage-powershell.md)

U kunt SQL Server Management Studio (SSMS) Azure SQL Database servers en databases te beheren. In dit onderwerp helpt u bij veelvoorkomende taken uitvoeren met SSMS. U moet al een server en de database die is gemaakt in Azure SQL-Database voordat u begint. Zie [de eerste Azure SQL-Database maken](sql-database-get-started.md) en [verbinding maken en een Query op basis van SSMS](sql-database-connect-query-ssms.md) voor meer informatie.

Het wordt aanbevolen dat u de nieuwste versie van SSMS gebruiken wanneer u met Azure SQL-Database werkt. 

> [AZURE.IMPORTANT] Gebruik altijd de meest recente versie van SSMS, omdat deze wordt voortdurend verbeterd om te werken met de recentste updates voor Azure en SQL-Database. Zie als u de meest recente versie, [Download SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).



## <a name="create-and-manage-azure-sql-databases"></a>Maken en beheren van Azure SQL-databases

Terwijl u verbinding hebt met de **master** -database, kunt u databases op de server maken en wijzigen of bestaande databases weghalen. De volgende stappen wordt beschreven hoe u taken voor verschillende algemene database management via Management Studio. Zorg dat u bent verbonden met de **master** -database met het niveau van de server principal-aanmelding die u hebt gemaakt bij het instellen van uw server voor het uitvoeren van deze taken.

Een queryvenster te openen in Management Studio, open de map Databases, vouw de map **Databases systeem** met de rechtermuisknop op de **basispagina**en klik vervolgens op **Nieuw**.

-   Gebruik de instructie **CREATE DATABASE** om een database te maken. Zie [CREATE DATABASE (SQL-Database)](https://msdn.microsoft.com/library/dn268335.aspx)voor meer informatie. De volgende instructie wordt een database met de naam **myTestDB** gemaakt en wordt aangegeven dat dit een database Standard Edition met S0 met standaard een maximale grootte van 250 GB.

        CREATE DATABASE myTestDB
        (EDITION='Standard',
         SERVICE_OBJECTIVE='S0');

Klik op **uitvoeren** als de query wilt uitvoeren.

-   Gebruik de instructie **ALTER DATABASE** voor het wijzigen van een bestaande database, bijvoorbeeld als u wilt wijzigen van de naam en versie van de database. Zie voor meer informatie, [ALTER DATABASE (SQL-Database)](https://msdn.microsoft.com/library/ms174269.aspx). De volgende instructie wijzigt u de database die u hebt gemaakt in de vorige stap wijzigen S1 Standard edition.

        ALTER DATABASE myTestDB
        MODIFY
        (SERVICE_OBJECTIVE='S1');

-   Gebruik **De DROP DATABASE** -instructie aan een bestaande database verwijderen. Zie voor meer informatie, [DROP DATABASE (SQL-Database)](https://msdn.microsoft.com/library/ms178613.aspx). De volgende instructie wordt de **myTestDB** -database verwijderd, maar niet neer nu omdat u deze gebruikt om aanmeldingen te maken in de volgende stap.

        DROP DATABASE myTestBase;

-   De master-database is de weergave van **sys.databases** kunt u details weergeven over alle databases. Als u wilt weergeven van alle bestaande databases, voer de volgende instructie:

        SELECT * FROM sys.databases;

-   In de SQL-Database, wordt de instructie **Gebruik** niet ondersteund voor het schakelen tussen databases. In plaats daarvan moet u een verbinding rechtstreeks naar de doeldatabase.

>[AZURE.NOTE] Veel van de Transact-SQL-instructies maken of wijzigen van een database in hun eigen batch moeten worden uitgevoerd en kunnen niet worden gegroepeerd met andere Transact-SQL-instructies. Zie de instructie-specifieke informatie voor meer informatie.

## <a name="create-and-manage-logins"></a>Maken en beheren van aanmeldingen

De **master** -database bevat aanmeldingen en welke aanmeldingen zijn gemachtigd voor het maken van databases of andere aanmeldingen. Aanmeldingen met de **master** -database met het niveau van de server principal-aanmelding die u hebt gemaakt bij het instellen van uw server te beheren. Voor het uitvoeren van query's op de master-database die aanmeldingen worden beheerd via de gehele server kunt u de **Aanmelding maken**, **Aanmelding wijzigen**of **Aanmelding DROP** -instructies. Voor meer informatie, Zie [beheren van Databases en SQL-Database-aanmeldingen](http://msdn.microsoft.com/library/azure/ee336235.aspx). 


-   Gebruik de instructie **Aanmelding maken** voor het maken van een niveau van de server aanmelding. Zie voor meer informatie, [Aanmelding maken (SQL-Database)](https://msdn.microsoft.com/library/ms189751.aspx). De volgende instructie maakt u een aanmelding **login1**genoemd. **Wachtwoord1** vervangen door het wachtwoord van uw keuze.

        CREATE LOGIN login1 WITH password='password1';

-   Gebruik de instructie **CREATE USER** databaseniveau machtigingen te verlenen. Alle aanmeldingen moeten worden gemaakt in **de hoofddatabase** . Voor een aanmelding verbinding maken met een andere database, moet u deze database niveau machtigingen verlenen met de instructie **CREATE USER** in die database. Zie [CREATE USER (SQL-Database)](https://msdn.microsoft.com/library/ms173463.aspx)voor meer informatie. 

-   Login1 als machtigingen wilt geven aan een database met de naam **myTestDB**, voert u de volgende stappen uit:

 1.  Klik met de rechtermuisknop op de naam van de server in Object Explorer Explorer-Object om de door u gemaakte database **myTestDB** weer te vernieuwen, en klik vervolgens op **vernieuwen**.  

     Als u de verbinding gesloten, kunt u herstellen door het **Object Explorer verbinding** selecteren in het menu bestand.

 2. Met de rechtermuisknop op de **myTestDB** database en selecteer **Nieuwe Query**.

    3.  Voer de volgende instructie voor de database myTestDB voor het maken van een databasegebruiker met de naam **login1User** komt met het niveau van de server aanmelding **login1 overeen**.

            CREATE USER login1User FROM LOGIN login1;

-   Gebruik de **sp\_addrolemember** opgeslagen procedure geven de account van de gebruiker het juiste niveau van machtigingen voor de database. Zie [sp_addrolemember (Transact-SQL)](http://msdn.microsoft.com/library/ms187750.aspx)voor meer informatie. De volgende instructie geeft **login1User** alleen-lezen machtigingen voor de database door toevoeging van **login1User** aan de **db\_datareader** rol.

        exec sp_addrolemember 'db_datareader', 'login1User';    

-   Gebruik de instructie **ALTER aanmelding** te wijzigen van een bestaande aanmelding, bijvoorbeeld als u wilt wijzigen van het wachtwoord voor de aanmelding. Zie voor meer informatie, [Aanmelding wijzigen (SQL-Database)](https://msdn.microsoft.com/library/ms189828.aspx). De instructie **ALTER aanmelding** moet worden uitgevoerd ten opzichte van de **master** -database. Ga terug naar het venster van de query die is verbonden met een database. De volgende instructie wijzigt de **login1** aanmelden om het wachtwoord opnieuw instellen. Vervangen **newPassword** met het wachtwoord van uw keuze en **OudWachtwoord** met het huidige wachtwoord voor de aanmelding.

        ALTER LOGIN login1
        WITH PASSWORD = 'newPassword'
        OLD_PASSWORD = 'oldPassword';

-   **De aanmelding DROP** -instructie te verwijderen van een bestaande aanmelding gebruiken. Een aanmelding op het serverniveau van de ook verwijdert alle gebruikersaccounts van de gekoppelde database. Zie voor meer informatie, [DROP DATABASE (SQL-Database)](https://msdn.microsoft.com/library/ms178613.aspx). De instructie **DROP aanmelding** moet worden uitgevoerd ten opzichte van de **master** -database. De instructie verwijdert u de aanmelding **login1** .

        DROP LOGIN login1;

-   De master-database is de **sys.sql\_aanmeldingen** waarmee u kunt bekijken van aanmeldingen bekijken. Als u wilt weergeven van alle bestaande aanmeldingen, voer de volgende instructie:

        SELECT * FROM sys.sql_logins;

## <a name="monitor-sql-database-using-dynamic-management-views"></a>SQL-Database met behulp van dynamische weergaven van beheer controleren

SQL-Database ondersteunt verschillende weergaven van dynamische-beheer kunt u een afzonderlijke database controleren. Volgen een paar voorbeelden van het type monitor gegevens die door middel van deze weergaven kunt u ophalen. Zie voor volledige details en meer voorbeelden van het gebruik, [Monitoring SQL-Database via weergaven van dynamische Management](https://msdn.microsoft.com/library/azure/ff394114.aspx).

-   Bij het controleren van een weergave van het beheer van dynamische **DATABASE WEERGAVESTATUS** machtigingen vereist. Om de **WEERGAVESTATUS DATABASE** machtiging aan een specifieke databasegebruiker, verbinding met de database en voer de volgende instructie voor de database:

        GRANT VIEW DATABASE STATE TO login1User;

-   Berekenen van de database-formaat met de **sys.dm\_db\_partitie\_stats** weergeven. De **sys.dm\_db\_partitie\_stats** weergave retourneert informatie pagina en het aantal rijen voor elke partitie in de database die u gebruiken kunt voor het berekenen van de grootte van de database. De volgende query geeft als resultaat de grootte van de database in MB:

        SELECT SUM(reserved_page_count)*8.0/1024
        FROM sys.dm_db_partition_stats;   

-   Gebruik de **sys.dm\_exec\_verbindingen** en **sys.dm\_exec\_sessies** weergaven voor het ophalen van informatie over de huidige verbindingen van gebruikers en interne taken die zijn gekoppeld aan de database. De volgende query retourneert informatie over de huidige verbinding:

        SELECT
            e.connection_id,
            s.session_id,
            s.login_name,
            s.last_request_end_time,
            s.cpu_time
        FROM
            sys.dm_exec_sessions s
            INNER JOIN sys.dm_exec_connections e
              ON s.session_id = e.session_id;

-   Gebruik de **sys.dm\_exec\_query\_stats** weergave statistische prestatiestatistieken voor ophalen in de cache opgeslagen query-plannen. De volgende query retourneert informatie over de bovenste vijf query's gerangschikt door de gemiddelde CPU-tijd.

        SELECT TOP 5 query_stats.query_hash AS "Query Hash",
            SUM(query_stats.total_worker_time), SUM(query_stats.execution_count) AS "Avg CPU Time",
            MIN(query_stats.statement_text) AS "Statement Text"
        FROM
            (SELECT QS.*,
            SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
            ((CASE statement_end_offset
                WHEN -1 THEN DATALENGTH(ST.text)
                ELSE QS.statement_end_offset END
                    - QS.statement_start_offset)/2) + 1) AS statement_text
             FROM sys.dm_exec_query_stats AS QS
             CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
        GROUP BY query_stats.query_hash
        ORDER BY 2 DESC;
 
 
