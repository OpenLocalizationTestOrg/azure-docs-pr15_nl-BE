<properties
   pageTitle="Herstellen van een datawarehouse Azure SQL (Portal) | Microsoft Azure"
   description="Portal taken voor het terugzetten van een datawarehouse Azure SQL Azure."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/21/2016"
   ms.author="lakshmir;barbkess;sonyama"/>

# <a name="restore-an-azure-sql-data-warehouse-portal"></a>Herstellen van een datawarehouse Azure SQL (Portal)

> [AZURE.SELECTOR]
- [Overzicht][]
- [Portal][]
- [PowerShell][]
- [REST][]

In dit artikel leert u het herstellen van een Azure SQL Data Warehouse met behulp van de Portal Azure.

## <a name="before-you-begin"></a>Voordat u begint

**Controleer of de capaciteit van de DTU.** Elke SQL Data Warehouse wordt gehost door een SQL-server (bijv. myserver.database.windows.net) met een standaardquotum DTU.  Voordat u een SQL-datawarehouse terugzetten kunt, Controleer de SQL server heeft weinig DTU resterende voor de database wordt hersteld. Meer informatie over het berekenen van DTU die nodig zijn of dat er meer DTU, Zie [een quotum DTU wijziging aanvragen][].


## <a name="restore-an-active-or-paused-database"></a>Een actief of onderbroken database terugzetten

Een database terugzetten:

1. Log in op de [Azure portal][]
2. Aan de linkerkant van het scherm selecteren op **Bladeren** en selecteer vervolgens de **SQL-servers**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
    
3. Navigeer naar de server en selecteer het
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)

4. Het datawarehouse SQL die u wilt terugzetten en selecteer het zoeken
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. Aan de bovenkant van het datawarehouse blade, klikt u op **herstellen**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)

6. Geef een nieuwe **naam van de Database**
7. Het meest recente **Herstelpunt** selecteren
    1. Zorg ervoor dat u het meest recente herstelpunt kiezen.  Aangezien de herstelpunten worden weergegeven in UTC, is de standaardoptie weergegeven soms niet het meest recente herstelpunt.
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)

8. Klik op **OK**
9. De herstelprocedure voor de database wordt gestart en kan worden gecontroleerd met behulp van **meldingen**

>[AZURE.NOTE] Nadat het terugzetten is voltooid, kunt u de herstelde database met de volgende [configuratie van de database na herstel][].


## <a name="restore-a-deleted-database"></a>Een verwijderde database terugzetten

Een verwijderde database terugzetten:

1. Log in op de [Azure portal][]
2. Aan de linkerkant van het scherm selecteren op **Bladeren** en selecteer vervolgens de **SQL-servers**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)

3. Navigeer naar de server en selecteer het
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)

4. Ga naar de sectie bewerkingen op van de server-blade
5. Klik op de tegel **Databases verwijderen**
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)

6. Selecteer de verwijderde database die u wilt terugzetten
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)

7. Geef een nieuwe **naam van de Database**
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
    
8. Klik op **OK**
9. De herstelprocedure voor de database wordt gestart en kan worden gecontroleerd met behulp van **meldingen**

>[AZURE.NOTE] Zie de database configureren nadat het terugzetten is voltooid, [de database na herstel configureren][]. 

## <a name="next-steps"></a>Volgende stappen
Lees meer informatie over de functies van de business continuity van edities Azure SQL-Database, de [business continuity overzicht van Azure SQL-Database][].

<!--Image references-->

<!--Article references-->
[Azure SQL-Database business continuity-overzicht]: ./sql-database-business-continuity.md
[Overzicht]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[De database configureren na herstel]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Een wijziging van de quota DTU aanvragen]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change

<!--MSDN references-->

<!--Blog references-->

<!--Other Web references-->
[Azure portal]: https://portal.azure.com/
