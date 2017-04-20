<properties
    pageTitle="Azure SQL-database naar een Bacpac-bestand met behulp van de Portal Azure archiveren"
    description="Azure SQL-database naar een Bacpac-bestand met behulp van de Portal Azure archiveren"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/15/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="archive-an-azure-sql-database-to-a-bacpac-file-using-the-azure-portal"></a>Azure SQL-database naar een Bacpac-bestand met behulp van de Portal Azure archiveren

> [AZURE.SELECTOR]
- [Azure portal](sql-database-export.md)
- [PowerShell](sql-database-export-powershell.md)

Dit artikel bevat instructies voor het archiveren van uw Azure SQL-database naar een Bacpac-bestand (opgeslagen in Azure blob-opslag) met behulp van de [portal Azure](https://portal.azure.com).

Wanneer u een Azure SQL-database te archiveren, kunt u het databaseschema en de gegevens exporteren naar een bestand Bacpac. Een Bacpac-bestand is gewoon een ZIP-bestand met de extensie BACPAC. Een Bacpac-bestand later kan worden opgeslagen in lokale opslag op een locatie in de bedrijfsruimten of in Azure blob-opslag en later geïmporteerde terug naar Azure SQL-Database of een SQL-Server op-bedrijfsruimten installatie. 

***Overwegingen met betrekking tot***

- Een archief transactioneel consistent is, moet u ervoor zorgen die er activiteit plaatsvindt tijdens het exporteren of exporteren van een [Transactioneel consistente kopie](sql-database-copy.md) van uw Azure SQL-database.
- De maximale grootte van een Bacpac-bestand opgeslagen in Azure Blob-opslag is 200 GB. Als u wilt archiveren een groter Bacpac-bestand naar de lokale opslag, gebruik het opdrachtregelprogramma [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) . Dit hulpprogramma wordt geleverd met zowel Visual Studio en SQL Server. U kunt ook [download](https://msdn.microsoft.com/library/mt204009.aspx) de nieuwste versie van SQL Server Data Tools om met dit hulpprogramma.
- Archiveren naar Azure premium opslag met behulp van een Bacpac-bestand wordt niet ondersteund.
- Als de exportbewerking 20 uur overschrijdt, kan deze worden geannuleerd. U verhoogt de prestaties tijdens het exporteren, kunt u het volgende doen:
 - Tijdelijk verhogen uw serviceniveau.
 - Niet meer alle lees- en schrijfbewerkingen tijdens het exporteren.
 - Een [gegroepeerde index](https://msdn.microsoft.com/library/ms190457.aspx) gebruiken met niet-nulwaarden voor alle grote tabellen. Zonder gegroepeerde indexen worden exporteren kan mislukken als het duurt langer dan 6 tot 12 uur. Dit komt omdat de export service scannen van de tabel om te proberen moet om de hele tabel exporteren te voltooien. Er is een goede manier om te bepalen als uw tabellen zijn geoptimaliseerd voor uitvoer **DBCC SHOW_STATISTICS** uitvoeren en zorg ervoor dat de *RANGE_HI_KEY* niet null is en de waarde ervan is een goede verdeling. Zie [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx)voor details.


> [AZURE.NOTE] BACPACs zijn niet bedoeld om te worden gebruikt voor back-up- en terugzetbewerkingen. Azure SQL-Database automatisch back-ups voor elke gebruiker-database gemaakt. Zie [Business Continuity-overzicht](sql-database-business-continuity.md)voor meer informatie.

Voor het voltooien van dit artikel hebt u het volgende nodig:

- Een abonnement op Azure.
- Azure SQL-Database. 
- Een [Azure standaardopslag rekening](../storage/storage-create-storage-account.md) met een blob-container voor het opslaan van de BACPAC in standaard opslag.

## <a name="export-your-database"></a>Uw database exporteren

Open het SQL-Database-blade voor de database die u wilt exporteren.

> [AZURE.IMPORTANT] Garanderen van een transactioneel consistent Bacpac-bestand moet eerst [een kopie van de database maken](sql-database-copy.md) en vervolgens de databasekopie te exporteren. 

1.  Ga naar de [portal Azure](https://portal.azure.com).
2.  Klik op **SQL-databases**.
3.  Klik op de database te archiveren.
4.  In het blad SQL-Database, klikt u op **exporteren** de blade **database exporteren** :

    ![knop exporteren][1]

5.  Klik op **opslag** en selecteer uw opslag als de blob-container waarin de BACPAC worden opgeslagen:

    ![database exporteren][2]

6. Selecteer het verificatietype. 
7.  Voer de juiste verificatiereferenties voor de Azure SQL server met de database die u wilt exporteren.
8.  Klik op **OK** om de database te archiveren. Op **OK** te klikken maakt u een database exporteren aanvraag en wordt verzonden naar de service. De lengte van de tijd die de uitvoer is afhankelijk van de grootte en complexiteit van de database en uw serviceniveau. U ontvangt een melding.

    ![kennisgeving van uitvoer][3]

## <a name="monitor-the-progress-of-the-export-operation"></a>De voortgang van de exportbewerking

1.  Klik op **SQL-servers**.
2.  Klik op de server met de oorspronkelijke (bron)-database die u zojuist hebt gearchiveerd.
3.  Ga naar bewerkingen.
4.  Klik op **Geschiedenis importeren/exporteren**in de SQL server-blade:

    ![Import export geschiedenis][4]

## <a name="verify-the-bacpac-is-in-your-storage-container"></a>Controleer of de BACPAC in uw opslag container

1.  Klik op **opslag rekeningen**.
2.  Klik op de account van de opslag waar de Bacpac-archief is opgeslagen.
3.  Klik op de **Containers** en vervolgens de container die u de database in voor meer informatie (u kunt downloaden en opslaan van de BACPAC hier) geëxporteerd.

    ![details van .bacpac][5]  

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het importeren van een BACPAC een Azure SQL-database [importeren, een BACPCAC met een Azure SQL-database](sql-database-import.md)
- Zie voor meer informatie over het importeren van een BACPAC naar een SQL Server-database [importeren, een BACPCAC met een SQL Server-database](https://msdn.microsoft.com/library/hh710052.aspx)



<!--Image references-->
[1]: ./media/sql-database-export/export.png
[2]: ./media/sql-database-export/export-blade.png
[3]: ./media/sql-database-export/export-notification.png
[4]: ./media/sql-database-export/export-history.png
[5]: ./media/sql-database-export/bacpac-archive.png

