<properties
    pageTitle="Beheren en oplossen van problemen met Database uitrekken | Microsoft Azure"
    description="Informatie over het beheren en oplossen van problemen met Database uitrekken."
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
    ms.date="06/27/2016"
    ms.author="douglasl"/>

# <a name="manage-and-troubleshoot-stretch-database"></a>Beheren en oplossen van problemen met Database uitrekken

Gebruiken voor het beheren en oplossen van problemen met Database uitrekken, hulpmiddelen en methoden die worden beschreven in dit onderwerp.

## <a name="manage-local-data"></a>Lokale gegevens beheren

### <a name="LocalInfo"></a>Info over lokale databases en tabellen ingeschakeld voor uitrekken Database ophalen
Open de catalogus weergaven **sys.databases** en **systeemtabellen** info over uitrekken zien\-SQL Server-databases en tabellen ingeschakeld. Zie voor meer info [vinden (Transact-SQL)](https://msdn.microsoft.com/library/ms178534.aspx) en [systeemtabellen (Transact-SQL)](https://msdn.microsoft.com/library/ms187406.aspx).

Om te zien hoeveel ruimte een Stretch\-tabel ingeschakeld in SQL Server, voer de volgende instructie wordt gebruikt.

 ```tsql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'LOCAL_ONLY';
GO
 ```
## <a name="manage-data-migration"></a>Gegevensmigratie beheren

### <a name="check-the-filter-function-applied-to-a-table"></a>De filterfunctie is toegepast op een tabel controleren
Open de catalogus **sys.remote\_gegevens\_archief\_tabellen** en controleert u de waarde van de **filter\_predicaat** kolom om de functie die uitrekken Database gebruikt om rijen te migreren selecteren. Als de waarde null is, wordt de hele tabel in aanmerking moeten worden gemigreerd. Zie [sys.remote_data_archive_tables (Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx) en [rijen te migreren met behulp van de filterfunctie van een selecteren](sql-server-stretch-database-predicate-function.md)voor meer informatie.

### <a name="Migration"></a>Controleer de status van de gegevensmigratie
Selecteer taken **| Uitrekken | Monitor** voor een database in SQL Server Management Studio gegevensmigratie in uitrekken Databasemonitor controleren. Zie voor meer info, [controleren en oplossen van problemen met gegevensmigratie (uitrekken Database)](sql-server-stretch-database-monitor.md).

Of opent u de weergave dynamic management **sys.dm\_db\_rda\_migratie\_status** voor een overzicht van het aantal batches en rijen met gegevens zijn gemigreerd.

### <a name="Firewall"></a>Gegevensmigratie oplossen
Raadpleeg voor meer suggesties [controleren en oplossen van problemen met gegevensmigratie (uitrekken Database)](sql-server-stretch-database-monitor.md).

## <a name="manage-remote-data"></a>Beheer van externe gegevens

### <a name="RemoteInfo"></a>Info over externe databases en tabellen die worden gebruikt door uitrekken Database ophalen
Open de catalogusweergaven **sys.remote\_gegevens\_archief\_databases** en **sys.remote\_gegevens\_archief\_tabellen** voor een overzicht van de informatie over de externe databases en tabellen waarin de gemigreerde gegevens worden opgeslagen. Zie voor meer info, [sys.remote_data_archive_databases (Transact-SQL)](https://msdn.microsoft.com/library/dn934995.aspx) en [sys.remote_data_archive_tables (Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx).

Als u wilt zien hoeveel ruimte een uitrekken ingeschakeld tabel gebruikt in Azure, voer de volgende instructie.

 ```tsql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'REMOTE_ONLY';
GO
 ```

### <a name="delete-migrated-data"></a>Gemigreerde gegevens verwijderen  
Als u gegevens verwijderen die al zijn gemigreerd naar Azure wilt, de stappen die worden beschreven in [sys.sp_rda_reconcile_batch](https://msdn.microsoft.com/library/mt707768.aspx).

## <a name="manage-table-schema"></a>Tabelschema beheren

### <a name="dont-change-the-schema-of-the-remote-table"></a>Het schema van de externe tabel niet wijzigen
Het schema van een externe Azure tabel die is gekoppeld aan een SQL Server-tabel die is geconfigureerd voor uitrekken Database worden niet gewijzigd. In het bijzonder niet wijzigen de naam of het gegevenstype van een kolom. De functie Database uitrekken kunt verschillende veronderstellingen over het schema van de externe tabel ten opzichte van het schema van de SQL Server-tabel. Als u het schema van de externe Database uitrekken werkt niet voor de tabel gewijzigd.

### <a name="reconcile-table-columns"></a>Tabelkolommen afstemmen  
Als u kolommen van de tabel met externe per ongeluk hebt verwijderd, voert u **sp_rda_reconcile_columns** als kolommen wilt toevoegen aan de externe tabel die bestaan in het uitrekken\-SQL Server-tabel ingeschakeld, maar niet in de externe tabel. Zie [sys.sp_rda_reconcile_columns](https://msdn.microsoft.com/library/mt707765.aspx)voor meer info.  

  > [!IMPORTANT] Wanneer **sp_rda_reconcile_columns** wordt opnieuw gemaakt met kolommen die u per ongeluk hebt verwijderd uit de externe tabel, worden de gegevens die eerder in de verwijderde kolommen niet hersteld.

**sp_rda_reconcile_columns** verwijdert de kolommen niet van de externe tabel die voorkomt in de externe tabel maar niet in het uitrekken\-SQL Server-tabel ingeschakeld. Als u kolommen in de tabel met externe Azure die niet meer bestaan in het uitrekken\-SQL Server ingeschakeld tabel, deze extra kolommen niet belemmeren Database uitrekken uit de normale werking. Desgewenst kunt u extra kolommen handmatig verwijderen.  

## <a name="manage-performance-and-costs"></a>Beheer van prestaties en kosten  

### <a name="troubleshoot-query-performance"></a>Problemen met prestaties van query 's
Query's met Stretch\-ingeschakelde tabellen worden verwacht voor het uitvoeren van langzamer dan vóór de tabellen zijn ingeschakeld voor uitrekken. Als prestaties van query's worden aanzienlijk afnemen, raadpleegt u de volgende mogelijke problemen.

-   Is uw Azure-server in een andere geografische regio dan SQL Server? Worden in hetzelfde geografische gebied als de SQL Server te verminderen netwerkvertraging Azure server configureren.

-   De voorwaarden van uw netwerk kunnen hebben aangetast. Neem contact op met de netwerkbeheerder voor informatie over recente problemen of storingen.

### <a name="increase-azure-performance-level-for-resource-intensive-operations-such-as-indexing"></a>Azure prestaties verhogen voor resource\-intensieve bewerkingen, zoals het indexeren
Als bouwen, opnieuw maken of opnieuw indelen van een index op een grote tabel die geconfigureerd voor uitrekken Database en u verwacht zwaar bij het controleren van de gemigreerde gegevens in Azure gedurende deze tijd, kunt u het prestatieniveau van de bijbehorende externe Azure database verhogen voor de duur van de bewerking. Zie [SQL Server uitrekken Database prijzen](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/)voor meer informatie over prestaties en prijs.

### <a name="you-cant-pause-the-sql-server-stretch-database-service-on-azure"></a>U kunt de service SQL Server-Database uitrekken op Azure niet onderbreken  
 Zorg ervoor dat u de juiste prestaties en prijs niveau. Als u het prestatieniveau van de voor een resource tijdelijk verhogen\-intensieve bewerking, naar het vorige niveau herstellen nadat de bewerking is voltooid. Zie [SQL Server uitrekken Database prijzen](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/)voor meer informatie over prestaties en prijs.  

## <a name="change-the-scope-of-queries"></a>Het bereik van de query's wijzigen  
 Query's voor uitrekken\-ingeschakelde tabellen zowel lokale als externe gegevens standaard geretourneerd. U kunt het bereik van de query's voor alle query's voor alle gebruikers of alleen voor een enkele query die door een beheerder wijzigen.  

### <a name="change-the-scope-of-queries-for-all-queries-by-all-users"></a>Het bereik van de query's voor alle query's voor alle gebruikers wijzigen  
 Om het bereik van alle query's voor alle gebruikers wijzigen, voert u de opgeslagen procedure **sys.sp_rda_set_query_mode**. U kunt het bereik zodat alleen lokale gegevens opvragen, alle query's uitschakelen of de standaardinstelling herstellen verminderen. Zie [sys.sp_rda_set_query_mode](https://msdn.microsoft.com/library/mt703715.aspx)voor meer info.  

### <a name="queryHints"></a>Het bereik van de query's voor een enkele query die door een beheerder wijzigen  
 Toevoegen als u wilt wijzigen in het bereik van een enkele query door een lid van de rol db_owner, de * *met \( REMOTE_DATA_ARCHIVE_OVERRIDE = *waarde* \)** queryaanwijzing de SELECT-instructie. De queryaanwijzing REMOTE_DATA_ARCHIVE_OVERRIDE kan de volgende waarden hebben.  
 -   **LOCAL_ONLY**. Alleen lokale gegevens opvragen.  

 -   **REMOTE_ONLY**. Alleen externe gegevens opvragen.  

 -   **STAGE_ONLY**. Query alleen de gegevens in de tabel waar uitrekken Database stadia rijen die in aanmerking komen voor migratie en behoudt de gemigreerde rijen voor de opgegeven periode na de migratie. Deze geheugensteun query is de enige manier om de tijdelijke tabel.  

Bijvoorbeeld retourneert de volgende query alleen lokale resultaten.  

 ```tsql  
 USE <Stretch-enabled database name>;
 GO
 SELECT * FROM <Stretch_enabled table name> WITH (REMOTE_DATA_ARCHIVE_OVERRIDE = LOCAL_ONLY) WHERE ... ;
 GO
```  

## <a name="adminHints"></a>Beheerdersupdates en verwijderen  
 Standaard kunt u niet bijwerken of verwijderen van rijen die in aanmerking komen voor migratie of rijen die al zijn gemigreerd, in een stuk\-tabel ingeschakeld. Wanneer er een probleem op te lossen, een lid van de rol db_owner een UPDATE- of DELETE-bewerking kunt uitvoeren door het toevoegen van de * *met \( REMOTE_DATA_ARCHIVE_OVERRIDE = *waarde* \)** queryaanwijzing aan de instructie. De queryaanwijzing REMOTE_DATA_ARCHIVE_OVERRIDE kan de volgende waarden hebben.  
 -   **LOCAL_ONLY**. Bijwerken of verwijderen, alleen lokale gegevens.  

 -   **REMOTE_ONLY**. Bijwerken of verwijderen van externe gegevens.  

 -   **STAGE_ONLY**. Bijwerken of verwijderen van gegevens in de tabel waar uitrekken Database stadia rijen die in aanmerking komen voor migratie en behoudt de gemigreerde rijen voor de opgegeven periode na de migratie.  

## <a name="see-also"></a>Zie ook

[Monitor uitrekken Database](sql-server-stretch-database-monitor.md)

[Back-up uitrekken geschikte databases](sql-server-stretch-database-backup.md)

[Uitrekken geschikte databases terugzetten](sql-server-stretch-database-restore.md)
