<properties
    pageTitle="Azure SQL-Database via de Portal Azure beheren | Microsoft Azure"
    description="Informatie over de Azure Portal gebruiken voor het beheren van een relationele database in de cloud via de Portal Azure."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.date="09/19/2016"
    ms.author="sstein"/>


# <a name="managing-azure-sql-databases-using-the-azure-portal"></a>Azure SQL-Databases via de Azure portal beheren


> [AZURE.SELECTOR]
- [Azure portal](sql-database-manage-portal.md)
- [SSMS](sql-database-manage-azure-ssms.md)
- [PowerShell](sql-database-manage-powershell.md)

De [Azure portal](https://portal.azure.com/) kunt u maken, controleren en beheren van Azure SQL-databases en servers. Dit artikel bevat een beschrijving van snelle en koppelingen naar de details van de meer algemene taken.

## <a name="view-your-azure-sql-databases-servers-and-pools"></a>Uw Azure SQL databases, servers en toepassingen weergeven

Om de beschikbare services voor SQL-Database bekijken, klikt u op **meer services**en **SQL** te typen in het zoekvak:

![SQL-Database](./media/sql-database-manage-portal/sql-services.png)


## <a name="how-do-i-create-or-view-azure-sql-databases"></a>Hoe ik maken of Azure SQL-databases bekijken?

**SQL-databases**, klikt u op de bladeserver **SQL-databases** openen, en klikt u op de database die u werken wilt met, of klik op **+ toevoegen** als u wilt maken van een SQL-database. Zie [een SQL-database in minuten met behulp van de portal Azure maken](sql-database-get-started.md)voor meer informatie.


![SQL-databases](./media/sql-database-manage-portal/sql-databases.png)


## <a name="how-do-i-create-or-view-azure-sql-servers"></a>Hoe ik maken of Azure SQL-servers weergeven?

Wilt openen de bladeserver met **SQL-servers** , **SQL-servers**, klikt u op en klikt u op de server die u werken wilt met of klik op **+ toevoegen** als u wilt maken van een SQL-server. Zie [een SQL-database in minuten met behulp van de portal Azure maken](sql-database-get-started.md)voor meer informatie.

![SQL-servers](./media/sql-database-manage-portal/sql-servers.png)


## <a name="how-do-i-create-or-view-sql-elastic-pools"></a>Hoe ik maken of elastische SQL-toepassingen weergeven?

**Elastische SQL-toepassingen**, klikt u op openen de blade **elastische SQL-toepassingen** , en klikt u op de groep die u werken wilt met, of klik op **+ toevoegen** om een groep te maken. Zie voor meer informatie, [een elastische database toepassingen met Azure portal maken](sql-database-elastic-pool-create-portal.md).

![Elastische SQL-toepassingen](./media/sql-database-manage-portal/elastic-pools.png)



## <a name="how-do-i-update-or-view-sql-database-settings"></a>Hoe ik bijwerken of SQL-database-instellingen weergeven?

Als u wilt weergeven of bijwerken van de database-instellingen, klikt u op de gewenste instelling op het blad van SQL-database:


![SQL-database-instellingen](./media/sql-database-manage-portal/settings.png)


## <a name="how-do-i-find-a-sql-databases-fully-qualified-server-name"></a>Hoe vind ik een SQL-databases volledig gekwalificeerde naam

Als u de naam van uw databases, klikt u op **Overzicht** in de **SQL-database** -blade en noteer de servernaam van de:


![SQL-database-instellingen](./media/sql-database-manage-portal/server-name.png)


## <a name="how-do-i-manage-firewall-rules-to-control-access-to-my-sql-server-and-database"></a>Hoe beheer ik toegang tot mijn database en SQL server firewall-regels

Als u wilt weergeven, maken of bijwerken van de firewall-regels, klikt u op **een firewall** op de **SQL-database** -blade. Zie [een Azure SQL Database server niveau firewallregel met behulp van de portal Azure configureren](sql-database-configure-firewall-settings.md)voor meer informatie.


![firewall-regels](./media/sql-database-manage-portal/sql-database-firewall.png)


## <a name="how-do-i-change-my-sql-database-service-tier-or-performance-level"></a>Hoe wijzig ik mijn SQL database laag of het niveau van de service


Klik op **prijzen laag (schaal DTUs)** om het serviceniveau voor laag of de prestaties van een SQL-database op de **SQL-database** -blade. Zie [de lagen en prestaties serviceniveau (prijzen laag) van een SQL-database wijzigen](sql-database-scale-up.md)voor meer informatie.


![prijzen lagen](./media/sql-database-manage-portal/pricing-tier.png)


## <a name="how-do-i-configure-auditing-and-threat-detection-for-a-sql-database"></a>Hoe configureer ik controle en bedreiging-detectie van een SQL-database

Configureren van controle- en risico-detectie van een SQL-database, klikt u op **controle en bedreiging detectie** in de **SQL-database** -blade. Zie voor meer informatie, [aan de slag met SQL-database controleren](sql-database-auditing-get-started.md)en [aan de slag met SQL Database dreiging detectie](sql-database-threat-detection-get-started.md).


## <a name="how-do-i-configure-dynamic-data-masking-for-a-sql-database"></a>Hoe configureer ik dynamische gegevens maskeren voor een SQL-database

Configureren van dynamische gegevens maskeren voor een SQL-database, klikt u op **dynamische gegevens maskeren** in het blad van de **SQL-database** . Zie [aan de slag met SQL Database dynamische gegevens-maskeren](sql-database-dynamic-data-masking-get-started.md)voor meer informatie.


## <a name="how-do-i-configure-transparent-data-encryption-tde-for-a-sql-database"></a>Hoe configureer ik transparante gegevenscodering (TDE) voor een SQL-database

Transparante gegevenscodering voor een SQL-database configureren, klikt u op **transparante gegevenscodering** op de **SQL-database** -blade. Voor meer informatie, Zie [TDE inschakelen op een database met behulp van de portal](https://msdn.microsoft.com/library/dn948096#Anchor_1).

## <a name="how-do-i-view-or-change-the-max-size-of-a-sql-database"></a>Hoe ik weergeven of wijzigen van de maximale grootte van een SQL-database?

Als u wilt weergeven of wijzigen van de grootte van een SQL-database, klikt u op **grootte van de Database** op de **SQL-database** -blade. De maximale grootte van een database bijwerken door de laag of het serviceniveau wijzigen. Zie [de lagen en prestaties serviceniveau (prijzen laag) van een SQL-database wijzigen](sql-database-scale-up.md)voor meer informatie.

## <a name="how-do-i-monitor-and-improve-the-performance-of-a-sql-database"></a>Hoe ik controleren en verbeteren de prestaties van een SQL-database?

Om te controleren en verbeteren van prestatie-eigenschappen van een SQL-database, klikt u op **Prestaties-overzicht** in de **SQL-database** -blade. Zie [SQL Database prestaties inzicht](sql-database-performance.md).


## <a name="how-do-i-configure-geo-replication"></a>Hoe configureer ik Geo-replicatie

Geo-replicatie voor een SQL-database, klik op om **Geo-replicatie** op de **SQL-database** -blade. Zie voor meer informatie, [Geo-replicatie configureren voor Azure SQL-Database met Azure portal](sql-database-geo-replication-portal.md).


## <a name="how-do-i-failover-to-a-geo-replicated-sql-database"></a>Hoe maak ik een failover naar een SQL-geo-gerepliceerde database?

Voor failover naar een secundaire geo gerepliceerd, **Geo-replicatie** op de **SQL-database** -blade, klik op **Failover**. Zie [een failover- of niet gepland voor Azure SQL-Database met Azure portal](sql-database-geo-replication-failover-portal.md)voor meer informatie.


## <a name="how-do-i-copy-a-sql-database"></a>Hoe kopieer ik een SQL-database

Als u wilt kopiëren van een SQL-database, klikt u op **kopiëren** op de **SQL-database** -blade. Zie voor details [de Azure portal met Azure SQL-database kopiëren](sql-database-copy-portal.md).


![SQL-database-instellingen](./media/sql-database-manage-portal/sql-database-copy.png)

## <a name="how-do-i-archive-an-azure-sql-database-to-a-bacpac-file"></a>Hoe moet ik een Azure SQL-database naar een bestand Bacpac-archiveren?

Klik op **exporteren** in het blad van de **SQL-database** om een BACPAC van een SQL-database. Zie [een Azure SQL-database naar een Bacpac-bestand met behulp van de portal Azure archief](sql-database-export.md)voor meer informatie.


![SQL-database exporteren](./media/sql-database-manage-portal/sql-database-export.png)



## <a name="how-do-i-restore-a-sql-database-to-a-previous-point-in-time"></a>Hoe kan ik een SQL-database herstellen naar een vorig punt in de tijd?

Als u een SQL-database herstellen, klikt u op **terugzetten** op de **SQL-database** -blade. Zie [herstellen van een Azure SQL-Database naar een vorig punt in de tijd met Azure portal](sql-database-point-in-time-restore-portal.md)voor meer informatie.


![SQL-database-instellingen](./media/sql-database-manage-portal/sql-database-restore.png)


## <a name="how-do-i-create-an-azure-sql-database-from-a-bacpac-file"></a>Hoe maak ik een Azure SQL-database vanuit een bestand Bacpac-

Een SQL-database maken vanuit een Bacpac-bestand, klikt u op **database importeren** in het **SQL server** -blade. Voor meer informatie, Zie [Bacpac-bestand maken van een Azure SQL-database importeren](sql-database-import.md).


![SQL server](./media/sql-database-manage-portal/server-commands.png)


## <a name="how-do-i-restore-a-deleted-sql-database"></a>Hoe herstel ik een verwijderde SQL-database

Als u een verwijderde SQL-database herstellen, klikt u op **databases verwijderd** in de **SQL server** -blade (de SQL server die deel uitmaakt van de database die is verwijderd). Zie [een verwijderde Azure SQL-database via de portal Azure terugzetten](sql-database-restore-deleted-database-portal.md)voor meer informatie.

## <a name="how-do-i-delete-a-sql-database"></a>Hoe verwijder ik een SQL-database

Als u wilt verwijderen een SQL-database, klikt u op **verwijderen** op de **SQL-database** -blade. 

![SQL-database-instellingen](./media/sql-database-manage-portal/sql-database-delete.png)



## <a name="additional-resources"></a>Aanvullende bronnen

- [SQL-Database](sql-database-technical-overview.md)
- [Bewaken en beheren van een elastische database toepassingen met Azure portal](sql-database-elastic-pool-manage-portal.md)
