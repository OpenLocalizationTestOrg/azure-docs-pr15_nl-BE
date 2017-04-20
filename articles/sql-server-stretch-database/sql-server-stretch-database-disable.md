<properties
    pageTitle="Uitrekken Database uitschakelen en externe gegevens terughalen | Microsoft Azure"
    description="Informatie over het uitrekken Database uitschakelen voor een tabel en eventueel terugbrengen externe gegevens."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="disable-stretch-database-and-bring-back-remote-data"></a>Uitrekken Database uitschakelen en externe gegevens terughalen

Als u wilt uitrekken Database uitschakelen voor een tabel, selecteert u **Uitrekken** voor een tabel in SQL Server Management Studio. Selecteer een van de volgende opties.

-   **Uitschakelen | Terughalen van gegevens van Azure**. Uitrekken Database kopiëren de externe gegevens voor de tabel in Azure terug naar SQL Server, vervolgens uitschakelen voor de tabel. Deze bewerking data transferkosten in rekening worden gebracht en kan niet worden geannuleerd.

-   **Uitschakelen | Gegevens in Azure laat**. Uitrekken Database uitschakelen voor de tabel.  De externe gegevens voor de tabel in Azure verlaten.

U kunt ook de Transact\-SQL Database uitrekken uitschakelen voor een tabel of voor een database.

Nadat u de Database uitrekken uitgeschakeld voor een tabel, gegevens migreren stopt en de resultaten van query niet meer voorzien van resultaten van de externe tabel.

Als u onderbreken, gegevensmigratie wilt, Zie [onderbreken en hervatten uitrekken Database](sql-server-stretch-database-pause.md).

>   [AZURE.NOTE] Database uitrekken uit te schakelen voor een tabel of voor een database verwijdert niet het externe object. Als u verwijderen van de externe tabel of de externe database wilt, hebt u via de portal management Azure neer. De externe objecten blijven Azure kosten totdat u ze verwijdert. Zie [SQL Server uitrekken Database prijzen](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/)voor meer info.

## <a name="disable-stretch-database-for-a-table"></a>Uitrekken Database uitschakelen voor een tabel

### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-table"></a>Gebruik SQL Server Management Studio uitrekken Database uitschakelen voor een tabel

1.  Selecteer de tabel die u wilt uitrekken Database uitgeschakeld in SQL Server Management Studio in Object Explorer.

2.  Rechts\-Klik op **Uitrekken**, en selecteer vervolgens een van de volgende opties selecteren.

    -   **Uitschakelen | Terughalen van gegevens van Azure**. Uitrekken Database kopiëren de externe gegevens voor de tabel in Azure terug naar SQL Server, vervolgens uitschakelen voor de tabel. Deze opdracht kan niet worden geannuleerd.

        >   [AZURE.NOTE] De externe gegevens kopiëren voor de tabel in Azure terug naar SQL Server data transferkosten maakt. Zie voor meer info, [Data overdrachten prijzen Details](https://azure.microsoft.com/pricing/details/data-transfers/).

        Nadat de externe gegevens zijn gekopieerd van Azure ongedaan met SQL Server maken, uitrekken uitgeschakeld voor de tabel.

    -   **Uitschakelen | Gegevens in Azure laat**. Uitrekken Database uitschakelen voor de tabel.  De externe gegevens voor de tabel in Azure verlaten.

    >   [AZURE.NOTE] Uitrekken Database uitschakelen voor een tabel, worden de externe gegevens of de externe tabel niet verwijderen. Als u wilt dat de externe tabel verwijdert, hebt u neer te zetten met behulp van de portal Azure management. De externe tabel blijft Azure kosten totdat u het verwijdert. Zie [SQL Server uitrekken Database prijzen](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/)voor meer info.

### <a name="use-transact-sql-to-disable-stretch-database-for-a-table"></a>Gebruik de Transact\-SQL Database uitrekken uitschakelen voor een tabel

-   Als u wilt uitrekken uitschakelen voor een tabel en de externe gegevens voor de tabel in Azure SQL Server back-up, moet u de volgende opdracht uitvoeren. Nadat de externe gegevens zijn gekopieerd van Azure ongedaan met SQL Server maken, uitrekken uitgeschakeld voor de tabel.

    Deze opdracht kan niet worden geannuleerd.

    ```tsql
    USE <Stretch-enabled database name>;
    GO
    ALTER TABLE <Stretch-enabled table name>  
       SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;
    GO
    ```
    >   [AZURE.NOTE] De externe gegevens kopiëren voor de tabel in Azure terug naar SQL Server data transferkosten maakt. Zie voor meer info, [Data overdrachten prijzen Details](https://azure.microsoft.com/pricing/details/data-transfers/).

-   Uitrekken uitschakelen voor een tabel en het verlaten van de externe gegevens, kunt u de volgende opdracht uitvoeren.

    ```tsql
    ALTER TABLE <table_name>
       SET ( REMOTE_DATA_ARCHIVE = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED ) ) ;
    ```

>   [AZURE.NOTE] Uitrekken Database uitschakelen voor een tabel, worden de externe gegevens of de externe tabel niet verwijderen. Als u wilt dat de externe tabel verwijdert, hebt u neer te zetten met behulp van de portal Azure management. De externe tabel blijft Azure kosten totdat u het verwijdert. Zie [SQL Server uitrekken Database prijzen](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/)voor meer info.

## <a name="disable-stretch-database-for-a-database"></a>Uitrekken Database voor een database uitschakelen
Voordat u uitrekken Database voor een database uitschakelen kunt, u moet uitschakelen uitrekken Database op de afzonderlijke uitrekken\-tabellen in de database wordt ingeschakeld.

### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-database"></a>Gebruik SQL Server Management Studio uitrekken Database uitschakelen voor een database

1.  Selecteer de database die u wilt uitrekken Database uitgeschakeld in SQL Server Management Studio in Object Explorer.

2.  Rechts\-op en selecteert u **taken**, en selecteer **Uitrekken**en selecteer **uitschakelen**.

>   [AZURE.NOTE] Uitrekken Database uitschakelen voor een database verwijdert u de externe database. Als u verwijderen van de externe database wilt, hebt u via de portal management Azure neer. De externe database blijft Azure kosten totdat u het verwijdert. Zie [SQL Server uitrekken Database prijzen](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/)voor meer info.

### <a name="use-transact-sql-to-disable-stretch-database-for-a-database"></a>Gebruik de Transact\-SQL Database uitrekken voor een database uitschakelen
Voer de volgende opdracht.

```tsql
ALTER DATABASE <database name>
    SET REMOTE_DATA_ARCHIVE = OFF ;
```

>   [AZURE.NOTE] Uitrekken Database uitschakelen voor een database verwijdert u de externe database. Als u verwijderen van de externe database wilt, hebt u via de portal management Azure neer. De externe database blijft Azure kosten totdat u het verwijdert. Zie [SQL Server uitrekken Database prijzen](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/)voor meer info.

## <a name="see-also"></a>Zie ook

[ALTER DATABASE SET opties (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx)

[Onderbreken en hervatten van de Database uitrekken](sql-server-stretch-database-pause.md)
