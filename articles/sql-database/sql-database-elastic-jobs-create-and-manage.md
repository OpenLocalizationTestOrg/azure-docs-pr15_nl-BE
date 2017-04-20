<properties
    pageTitle="Maken en beheren van Azure SQL-Databases met behulp van elastische taken geschaalde | Gepland in Microsoft Azure"
    description="Maken en beheren van een taak elastische database doorlopen."
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="ddove"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/27/2016"
    ms.author="ddove"/>

# <a name="create-and-manage-scaled-out-azure-sql-databases-using-elastic-jobs-preview"></a>Maken en beheren van geschaalde van Azure SQL-Databases met behulp van elastische taken (voorbeeld)

> [AZURE.SELECTOR]
- [Azure portal](sql-database-elastic-jobs-create-and-manage.md)
- [PowerShell](sql-database-elastic-jobs-powershell.md)


Beheer van groepen van databases vereenvoudigen **elastische Database taken** door het uitvoeren van administratieve bewerkingen zoals schemawijzigingen, referenties management, verwijzing bijwerkt, prestatiegegevens verzamelen of huurder (klant) telemetrie-collectie. Elastische Database taken is beschikbaar via de portal Azure en PowerShell-cmdlets. De portal Azure oppervlakken verminderd echter functionaliteit beperkt tot uitvoering op alle databases in een [groep met elastische Database (voorbeeld)](sql-database-elastic-pool.md). Zie toegang tot extra functies en scripts worden uitgevoerd binnen een groep van databases, met inbegrip van een aangepaste verzameling of een shard (gemaakt met [elastische Database client library](sql-database-elastic-scale-introduction.md)), [maken en beheren van taken met PowerShell](sql-database-elastic-jobs-powershell.md). Zie [overzicht van elastische Database](sql-database-elastic-jobs-overview.md)voor meer informatie over taken. 

## <a name="prerequisites"></a>Vereisten

* Een abonnement op Azure. Zie voor een gratis evaluatieversie, [gratis proefperiode van een maand](https://azure.microsoft.com/pricing/free-trial/).
* Een elastische database-toepassingen. Zie [over elastische database toepassingen](sql-database-elastic-pool.md)
* Installatie van elastische database taak service-onderdelen. Zie [de taak elastische databaseservice installeren](sql-database-elastic-jobs-service-installation.md).

## <a name="creating-jobs"></a>Taken maken

1. Met behulp van de [portal Azure](https://portal.azure.com), uit een bestaande database elastische taak van toepassingen, klikt u op **taak maken**.
2. Typ in de gebruikersnaam en het wachtwoord van de beheerder van de database (gemaakt tijdens de installatie van taken) voor de database taken (metagegevensopslag voor taken).

    ![De taak een naam, typ of plak in de code en klik op uitvoeren][1]
2. Typ een naam voor de taak in de blade **Taak maken** .
3. Typ de gebruikersnaam en het wachtwoord verbinding maken met de doeldatabases met voldoende machtigingen voor de uitvoering van het script te kunnen uitvoeren.
4. Plak of typ in de T-SQL-script.
5. Klik op **Opslaan** en klik vervolgens op **uitvoeren**.

    ![Taken maken en uitvoeren][5]

## <a name="run-idempotent-jobs"></a>Idempotency is ingeschakeld, taken uitvoeren

Wanneer u een script op een set van databases uitvoert, moet u ervoor dat het script idempotency is ingeschakeld zijn. Dat wil zeggen, moet het script zijn meerdere keren uitvoeren zelfs als het is mislukt vóór instabiel. Bijvoorbeeld wanneer een script is mislukt, de taak wordt automatisch opnieuw geprobeerd totdat de poging slaagt (binnen de grenzen, als de poging logica zal uiteindelijk stoppen het opnieuw proberen). De manier om dit te doen is met de een component 'Als bestaat' en elk gevonden exemplaar verwijderen voordat u een nieuw object maakt. Hier ziet u een voorbeeld:

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

U kunt ook "Bestaat niet als" component gebruiken voordat u een nieuw exemplaar:

    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
     CREATE TABLE TestTable(
      TestTableId INT PRIMARY KEY IDENTITY,
      InsertionTime DATETIME2
     );
    END
    GO

    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO

Dit script werkt vervolgens de tabel die u eerder hebt gemaakt.

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN

    ALTER TABLE TestTable

    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO


## <a name="checking-job-status"></a>De status controleren

Nadat een taak is begonnen, kunt u de voortgang controleren.

1. **Taken beheren**Klik op de databasepagina met elastische pool.

    ![Klik op "Projecten beheren"][2]

2. Klik op de naam van de onder a, van een taak. De **STATUS** kan zijn 'Voltooid' of 'Mislukt'. (B) weergegeven de details van de taak met de datum en tijd van maken en uitvoeren. De onderstaande (c) de die wordt de voortgang van het script voor elke database in de groep, waardoor de details van de datum en tijd.

    ![Een voltooide taak controleren][3]


## <a name="checking-failed-jobs"></a>Taken controle mislukt

Als een taak niet kan worden gevonden door een logboek van de uitvoering ervan. Klik op de naam van de mislukte taak om de details te bekijken.

![Een mislukte taak controleren][4]


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png

 
