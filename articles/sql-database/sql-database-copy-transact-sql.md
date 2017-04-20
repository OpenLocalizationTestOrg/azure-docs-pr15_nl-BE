<properties 
    pageTitle="Kopiëren met behulp van Transact-SQL Azure SQL-database | Microsoft Azure" 
    description="Kopie maken van een Azure SQL-database met behulp van Transact-SQL" 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/19/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="copy-an-azure-sql-database-using-transact-sql"></a>Kopiëren met behulp van Transact-SQL Azure SQL-database


> [AZURE.SELECTOR]
- [Overzicht](sql-database-copy.md)
- [Azure portal](sql-database-copy-portal.md)
- [PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)


Deze volgende stappen laten zien hoe u een SQL-database met Transact-SQL naar dezelfde server of een andere server kopiëren. De kopieerbewerking database gebruikt de instructie [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) .

Als u de stappen in dit artikel wilt nodig u het volgende:

- Een abonnement op Azure. Als u een abonnement op Azure gewoon **Gratis** boven aan deze pagina op en teruggaan naar het voltooien van dit artikel.
- Azure SQL-Database. Als u niet een SQL-database, maken een volgens de stappen in dit artikel: [de eerste Azure SQL-Database maken](sql-database-get-started.md).
- [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/ms174173.aspx). Als u geen SSMS, of als u in dit artikel beschreven functies zijn niet beschikbaar, [de meest recente versie downloaden](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="copy-your-sql-database"></a>Kopieer de SQL-database

Meld u op de master-database met behulp van de belangrijkste login niveau van de server of de aanmelding die in de database die u wilt kopiëren. Aanmeldingen die niet het niveau van de server principal moeten leden van de rol van dbmanager kopiëren van databases. Zie voor meer informatie over de aanmeldingen en de verbinding met de server [beheren aanmeldingen](sql-database-manage-logins.md).

Kopiëren van de brondatabase met de instructie [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) starten. Deze instructie uitvoert, wordt de database kopiëren proces gestart. Omdat het kopiëren van een database een asynchroon proces is, retourneert de instructie CREATE DATABASE voordat de database is voltooid kopiëren.


### <a name="copy-a-sql-database-to-the-same-server"></a>Een SQL-database op dezelfde server kopiëren

Meld u op de master-database met behulp van de belangrijkste login niveau van de server of de aanmelding die in de database die u wilt kopiëren. Aanmeldingen die niet het niveau van de server principal moeten leden van de rol van dbmanager kopiëren van databases.

Deze opdracht kopieën Database1 bij een nieuwe database met de naam Database2 op dezelfde server. De kopieerbewerking kan enige tijd duren afhankelijk van de grootte van de database.

    -- Execute on the master database.
    -- Start copying.
    CREATE DATABASE Database1_copy AS COPY OF Database1;

### <a name="copy-a-sql-database-to-a-different-server"></a>Een SQL-database naar een andere server kopiëren

Meld u op de master-database van de server, de Azure SQL-databaseserver waar de nieuwe database wordt gemaakt. Gebruik een aanmelding met dezelfde naam en hetzelfde wachtwoord als de database-eigenaar (DBO) van de brondatabase op de bronserver Azure SQL-Database. De aanmelding op de server moet ook lid zijn van de rol van dbmanager of worden de belangrijkste login niveau van de server.

Met deze opdracht kopieert Database1 op server1 - naar een nieuwe database met de naam Database2 op server2. De kopieerbewerking kan enige tijd duren afhankelijk van de grootte van de database.


    -- Execute on the master database of the target server (server2)
    -- Start copying from Server1 to Server2
    CREATE DATABASE Database1_copy AS COPY OF server1.Database1;
    

## <a name="monitor-the-progress-of-the-copy-operation"></a>De voortgang van de kopieerbewerking

Het kopieerproces controleren bij het controleren van de weergaven sys.databases en sys.dm_database_copies. Tijdens het kopiëren uitgevoerd wordt, wordt de kolom state_desc van de weergave vinden voor de nieuwe database is ingesteld op kopiëren.


- Als het kopiëren niet werkt, wordt de kolom state_desc van de weergave vinden voor de nieuwe database is ingesteld op verdacht. In dit geval de instructie DROP uitvoeren op de nieuwe database en probeer het later opnieuw.
- Als het kopiëren is voltooid, wordt de kolom state_desc van de weergave vinden voor de nieuwe database is ingesteld op on line. In dit geval het kopiëren is voltooid en de nieuwe database is een normale database kunnen onafhankelijk van de brondatabase worden gewijzigd.

> [AZURE.NOTE] -Als u wilt kopiëren terwijl het bezig is, uitvoeren van de instructie [DROP DATABASE](https://msdn.microsoft.com/library/ms178613.aspx) in de nieuwe database. U kunt ook uitvoeren van de instructie DROP DATABASE op de brondatabase ook het kopiëren proces geannuleerd.


## <a name="resolve-logins-after-the-copy-operation-completes"></a>Aanmeldingen oplossen nadat de bewerking voor het kopiëren is voltooid

Nadat de nieuwe database op de doelserver online is, gebruikt u de instructie [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx) aan de gebruikers de nieuwe database-aanmeldingen op de doelserver opnieuw. Raadpleeg [Problemen met zwevende gebruikers](https://msdn.microsoft.com/library/ms175475.aspx)zwevende gebruikers oplossen. Zie ook [het beheren van Azure SQL databasebeveiliging na noodherstel](sql-database-geo-replication-security-config.md).

Alle gebruikers in de nieuwe database onderhouden de machtigingen die ze in de brondatabase hadden. De gebruiker die de databasekopie geïnitieerd eigenaar van de database van de nieuwe database en een nieuwe beveiligings-id (SID) wordt toegewezen. Nadat het kopiëren is voltooid en voordat u andere gebruikers opnieuw worden toegewezen, kan alleen de aanmelding die geïnitieerd met het kopiëren, de database-eigenaar (DBO), naar de nieuwe database aanmelden.


## <a name="next-steps"></a>Volgende stappen

- Zie [een Azure SQL-database kopiëren](sql-database-copy.md) voor een overzicht van het kopiëren van een SQL-Database Azure.
- Zie [met behulp van de portal Azure Azure SQL-database kopiëren](sql-database-copy-portal.md) voor het kopiëren van een database met behulp van de portal Azure.
- Zie [kopie van een database SQL Azure PowerShell gebruiken](sql-database-copy-powershell.md) voor het kopiëren van een database met PowerShell.
- Zie [het beheren van Azure SQL databasebeveiliging na noodherstel](sql-database-geo-replication-security-config.md) voor meer informatie over het beheren van gebruikers en aanmeldingen bij het kopiëren van een database naar een andere logische server.



## <a name="additional-resources"></a>Aanvullende bronnen

- [Aanmeldingen beheren](sql-database-manage-logins.md)
- [Verbinding maken met de SQL-Database met SQL Server Management Studio en een steekproef T-SQL-query uitvoeren](sql-database-connect-query-ssms.md)
- [De database exporteren naar een BACPAC](sql-database-export.md)
- [Business Continuity-overzicht](sql-database-business-continuity.md)
- [Documentatie voor SQL-Database](https://azure.microsoft.com/documentation/services/sql-database/)


