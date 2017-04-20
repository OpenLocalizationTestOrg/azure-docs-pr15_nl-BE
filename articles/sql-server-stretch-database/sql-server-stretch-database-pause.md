<properties
    pageTitle="Onderbreken en hervatten gegevensmigratie (Database uitrekken) | Microsoft Azure"
    description="Informatie over het onderbreken of hervatten gegevensmigratie naar Azure."
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
    ms.date="06/14/2016"
    ms.author="douglasl"/>

# <a name="pause-and-resume-data-migration-stretch-database"></a>Onderbreken en hervatten gegevensmigratie (uitrekken Database)

Als u wilt onderbreken of hervatten gegevensmigratie naar Azure, **Uitrekken** selecteren voor een tabel in SQL Server Management Studio, en selecteer vervolgens **onderbreken** gegevensmigratie onderbreken of **hervatten** hervatten gegevensmigratie. U kunt ook de Transact\-SQL onderbreken of hervatten gegevensmigratie.

Gegevensmigratie onderbreken in afzonderlijke tabellen wanneer u wilt oplossen van problemen op de lokale server of de beschikbare bandbreedte te maximaliseren.

## <a name="pause-data-migration"></a>Gegevensmigratie onderbreken

### <a name="use-sql-server-management-studio-to-pause-data-migration"></a>Gebruik SQL Server Management Studio gegevensmigratie onderbreken

1.  Selecteer het uitrekken in SQL Server Management Studio in Object Explorer\-tabel die u wilt onderbreken, gegevensmigratie ingeschakeld.

2.  Rechts\-op en **Uitrekken**selecteren en selecteer vervolgens **onderbreken**.

### <a name="use-transact-sql-to-pause-data-migration"></a>Gebruik de Transact\-SQL gegevensmigratie onderbreken
Voer de volgende opdracht.

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>  
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = PAUSED ) ) ;  
GO
```

## <a name="resume-data-migration"></a>Cv-gegevens migreren

### <a name="use-sql-server-management-studio-to-resume-data-migration"></a>Gebruik SQL Server Management Studio gegevensmigratie hervatten

1.  Selecteer het uitrekken in SQL Server Management Studio in Object Explorer\-tabel die u wilt hervatten, gegevensmigratie ingeschakeld.

2.  Rechts\-op en selecteer **Uitrekken**en selecteert u **hervatten**.

### <a name="use-transact-sql-to-resume-data-migration"></a>Gebruik de Transact\-SQL gegevensmigratie hervatten
Voer de volgende opdracht.

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>   
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = OUTBOUND ) ) ;  
 GO
```

## <a name="check-whether-migration-is-active-or-paused"></a>Controleer of de migratie actief of onderbroken is

### <a name="use-sql-server-management-studio-to-check-whether-migration-is-active-or-paused"></a>SQL Server Management Studio te gebruiken om te controleren of de migratie actief of onderbroken is
In SQL Server Management Studio **Uitrekken Databasemonitor** openen en controleren van de waarde van de kolom **Status van de migratie** . Zie voor meer info, [controleren en oplossen van problemen met gegevensmigratie](sql-server-stretch-database-monitor.md).

### <a name="use-transact-sql-to-check-whether-migration-is-active-or-paused"></a>Transact-SQL gebruiken om te controleren of de migratie actief of onderbroken is
De catalogus weergeven **sys.remote_data_archive_tables** query en controleer de waarde van de kolom **is_migration_paused** . Zie [sys.remote_data_archive_tables](https://msdn.microsoft.com/library/dn935003.aspx)voor meer info.

## <a name="see-also"></a>Zie ook

[ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx)
[controleren en oplossen van problemen met gegevensmigratie](sql-server-stretch-database-monitor.md)
