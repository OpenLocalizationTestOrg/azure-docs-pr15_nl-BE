<properties
    pageTitle="Uitrekken Database inschakelen voor een database | Microsoft Azure"
    description="Informatie over het configureren van een database voor de Database uitrekken."
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

# <a name="enable-stretch-database-for-a-database"></a>Uitrekken Database voor een database inschakelen

Voor meer informatie over het configureren van een bestaande database voor uitrekken Database **taken selecteren | Uitrekken | Inschakelen** voor een database in SQL Server Management Studio te openen van de wizard **Database inschakelen voor uitrekken** . U kunt ook de Transact\-SQL Database uitrekken voor een database inschakelen.

Als u taken **| Uitrekken | Inschakelen** voor een afzonderlijke tabel en u nog niet is ingeschakeld de database voor uitrekken Database, de wizard configureert u de database voor uitrekken Database en kunt u tabellen selecteren als onderdeel van het proces. Volg de stappen in dit onderwerp in plaats van de stappen in het [Uitrekken Database inschakelen voor een tabel](sql-server-stretch-database-enable-database.md).

Db uitrekken Database inschakelen op een database of een tabel vereist\_die de machtiging eigenaar. Uitrekken Database inschakelen op een database vereist DATABASE beheer machtigingen ook.

 >   [AZURE.NOTE] Later, als uitrekken Database is uitgeschakeld, moet u die Database uitrekken uit te schakelen voor een tabel of voor een database niet het externe object verwijdert. Als u verwijderen van de externe tabel of de externe database wilt, hebt u via de portal management Azure neer. De externe objecten blijven Azure kosten totdat u deze handmatig verwijderen.

## <a name="before-you-get-started"></a>Voordat u begint

-   Voordat u een database voor uitrekken configureren, we raden aan de adviseur uitrekken Database voor databases en tabellen die in aanmerking voor uitrekken komen. De Database uitrekken Advisor geeft ook de problematische kwesties. Zie voor meer info [identificeren databases en tabellen voor de Database uitrekken](sql-server-stretch-database-identify-databases.md).

-   Bekijk de [beperkingen voor de Database vergroten](sql-server-stretch-database-limitations.md).

-   Uitrekken Database overzet gegevens naar Azure. U moet daarom een Azure-account en een abonnement voor facturering. Om een Azure-account, [Klik hier](http://azure.microsoft.com/pricing/free-trial/).

-   De gegevens van de verbinding en aanmelding moet u een nieuwe server Azure maken of Selecteer een bestaande Azure server hebben.

## <a name="EnableTSQLServer"></a>Let op: Uitrekken Database op de server inschakelen
Voordat u uitrekken Database op een database of een tabel inschakelen kunt, moet u inschakelen op de lokale server. Deze bewerking moet de machtiging sysadmin of pas.

-   Als u de vereiste machtigingen hebt, wordt de server geconfigureerd voor uitrekken van de wizard **Database inschakelen voor uitrekken** .

-   Als u niet de vereiste machtigingen hebt, is de optie handmatig inschakelen door het uitvoeren van **sp\_configureren** voordat u de wizard uitvoert, of een beheerder heeft de wizard uit te voeren.

Als u handmatig uitrekken Database op de server, voert u **sp\_configureren** en schakel de optie **externe gegevens archiveren** . Het volgende voorbeeld wordt de optie **externe gegevens archiveren** door de waarde 1.

```
EXEC sp_configure 'remote data archive' , '1';
GO

RECONFIGURE;
GO
```
Voor meer info Zie [configureren de externe gegevens archiveren Server configuratieoptie](https://msdn.microsoft.com/library/mt143175.aspx) en [sp_configure (Transact-SQL)](https://msdn.microsoft.com/library/ms188787.aspx).

## <a name="Wizard"></a>Gebruik de wizard Database uitrekken inschakelen voor een database
Voor informatie over de Database inschakelen voor uitrekken Wizard, met inbegrip van de info die u moet invoeren en de keuzes die u moet doen, Zie [aan de slag door de Database inschakelen voor uitrekken Wizard uit te voeren](sql-server-stretch-database-wizard.md).

## <a name="EnableTSQLDatabase"></a>Gebruik de Transact\-SQL Database uitrekken inschakelen voor een database
Voordat u uitrekken Database voor afzonderlijke tabellen inschakelen kunt, hebt u inschakelt in de database.

Db uitrekken Database inschakelen op een database of een tabel vereist\_die de machtiging eigenaar. Uitrekken Database inschakelen op een database vereist DATABASE beheer machtigingen ook.

1.  Kies een bestaande Azure-server voor de gegevens die uitrekken Database worden gemigreerd voordat u begint, of maakt u een nieuwe server Azure.

2.  Op de Azure-server, moet u een firewallregel maken met het bereik van IP-adres van de SQL Server waarmee SQL Server communiceren met de externe server.

    De waarden die u nodig hebt en de firewallregel door de server Azure in Object Explorer in SQL Server Management Studio (SSMS) verbinding probeert te maken, kunt u gemakkelijk vinden. SSMS kunt u de regel maken door de volgende dialoogvenster waarin al de vereiste waarden voor IP-adres te openen.

    ![Een firewallregel maken in SSMS][FirewallRule]

3.  Voor meer informatie over het configureren van een SQL Server-database voor uitrekken Database heeft de database een databasehoofdsleutel hebben. De hoofdsleutel van de database worden de referenties die uitrekken Database verbinding maakt met de externe database. Hier volgt een voorbeeld van een nieuwe databasehoofdsleutel wordt gemaakt.

    ```tsql
    USE <database>;
    GO

    CREATE MASTER KEY ENCRYPTION BY PASSWORD ='<password>';
    GO
    ```

    Zie voor meer informatie over de databasehoofdsleutel [hoofdsleutel maken (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) en [maken een hoofdsleutel Database](https://msdn.microsoft.com/library/aa337551.aspx).

4.  Wanneer u een database voor uitrekken Database configureert, moet u een referentie voor de Database moet worden gebruikt voor communicatie tussen de op installaties van SQL Server en de externe server Azure uitrekken. U hebt twee opties.

    -   U kunt de referenties van een beheerder opgeven.

        -   Als u uitrekken Database inschakelen door de wizard wordt uitgevoerd, kunt u de referenties op dat moment.

        -   Als u uitrekken Database inschakelen wilt door het uitvoeren van de **DATABASE wijzigen**, hebt u de referenties voordat u **DATABASE wijzigen** zodat uitrekken Database handmatig maken.

        Hier volgt een voorbeeld van een nieuwe referentie wordt gemaakt.

        ```tsql
        CREATE DATABASE SCOPED CREDENTIAL <db_scoped_credential_name>
            WITH IDENTITY = '<identity>' , SECRET = '<secret>';
        GO
        ```

        Zie voor meer info over de referentie [Maken DATABASE binnen het bereik van referentie (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx). Maken van de referenties zijn vereist voor elke referentie wijzigen.

    -   U kunt een federatieve serviceaccount voor de SQL Server te communiceren met de externe server Azure wanneer de volgende omstandigheden zich tegelijk voordoen.

        -   De service waarmee het exemplaar van SQL Server wordt uitgevoerd een domeinaccount zijn.

        -   Een met Active Directory is federatieve met Azure Active Directory-domein behoort de domeinaccount.

        -   De externe server Azure is geconfigureerd voor ondersteuning van verificatie van Azure Active Directory.

        -   De serviceaccount waaronder het exemplaar van SQL Server wordt uitgevoerd, moet worden geconfigureerd als een dbmanager of sysadmin-account op de externe server Azure.

5.  Voor meer informatie over het configureren van een database voor uitrekken Database voert u de opdracht ALTER DATABASE.

    1.  Opgeven voor het argument van de SERVER, de naam van een bestaande server Azure, met inbegrip van de `.database.windows.net` gedeelte van de naam \- , `MyStretchDatabaseServer.database.windows.net`.

    2.  Voorzien van een bestaande referentie voor de beheerder het argument referentie of geef federatieve\_SERVICE\_ACCOUNT = ON. Het volgende voorbeeld wordt een bestaande referentie.

    ```tsql
    ALTER DATABASE <database name>
        SET REMOTE_DATA_ARCHIVE = ON
            (
                SERVER = '<server_name>',
                CREDENTIAL = <db_scoped_credential_name>
            ) ;
    GO
    ```

## <a name="next-steps"></a>Volgende stappen
-   [Uitrekken Database inschakelen voor een tabel](sql-server-stretch-database-enable-table.md) inschakelen extra tabellen.

-   [Monitor uitrekken Database](sql-server-stretch-database-monitor.md) voor de status van de gegevensmigratie.

-   [Onderbreken en hervatten van de Database uitrekken](sql-server-stretch-database-pause.md)

-   [Beheren en oplossen van problemen met Database uitrekken](sql-server-stretch-database-manage.md)

-   [Back-up uitrekken geschikte databases](sql-server-stretch-database-backup.md)

## <a name="see-also"></a>Zie ook

[Databases en tabellen voor uitrekken Database bepalen](sql-server-stretch-database-identify-databases.md)

[ALTER DATABASE SET opties (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx)

[FirewallRule]: ./media/sql-server-stretch-database-enable-database/firewall.png
