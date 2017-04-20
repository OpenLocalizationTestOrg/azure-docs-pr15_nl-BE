<properties 
    pageTitle="Geo-replicatie configureren voor Azure SQL-Database met Azure portal | Microsoft Azure" 
    description="Geo-replicatie configureren voor Azure SQL-Database via de portal Azure" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="NA"
    ms.date="10/18/2016"
    ms.author="sstein"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-the-azure-portal"></a>Geo-replicatie configureren voor Azure SQL-Database met Azure portal


> [AZURE.SELECTOR]
- [Overzicht](sql-database-geo-replication-overview.md)
- [Azure portal](sql-database-geo-replication-portal.md)
- [PowerShell](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

In dit artikel wordt beschreven hoe u actieve Geo-replicatie configureren voor SQL-Database met [Azure portal](http://portal.azure.com).

Zie [een failover- of niet gepland voor Azure SQL-Database met Azure portal](sql-database-geo-replication-failover-portal.md)voor het initiëren van failover met Azure portal.

>[AZURE.NOTE] Actieve Geo-replicatie (secundaire servers kan worden gelezen) is nu beschikbaar voor alle databases in alle Servicelagen. In April 2017, wordt het niet-leesbare secundaire type ingetrokken en bestaande niet-leesbare databases automatisch worden bijgewerkt naar secundaire servers voor leesbaar.

Met behulp van de portal Azure Geo-replicatie configureren, moet u de volgende bronnen:

- Azure SQL-database - de primaire database die u wilt repliceren op een andere geografische regio.

## <a name="add-secondary-database"></a>Secundaire database toevoegen

De volgende stappen wordt een nieuwe secundaire database maken in een partnerschap Geo-replicatie.  

Als u wilt toevoegen een secundair, moet u het abonnement eigenaar of mede-eigenaar zijn. 

De secundaire database heeft dezelfde naam als de primaire database en zal beschikken standaard over hetzelfde. De secundaire database kan zowel een enkele database of een database elastisch. Zie voor meer informatie, [Service Tiers](sql-database-service-tiers.md).
Nadat u de secundaire gemaakt en overgeënt begint gegevens uit de primaire database repliceren naar de nieuwe secundaire database. 

> [AZURE.NOTE] Als de database partner al bestaat (bijvoorbeeld - als gevolg van het beëindigen van een relatie met vorige Geo-replicatie), mislukt de opdracht.

### <a name="add-secondary"></a>Secundaire toevoegen

1. Blader naar de database die u wilt instellen voor Geo-replicatie in [Azure portal](http://portal.azure.com).
2. **Geo-replicatie**selecteert op de pagina SQL-database en selecteer vervolgens de regio de secundaire database maken. Kunt u elk gebied dan het gebied waarop de primaire database, maar de [gepaarde regio](../best-practices-availability-paired-regions.md) wordt aanbevolen.

    ![Geo-replicatie configureren](./media/sql-database-geo-replication-portal/configure-geo-replication.png)


4. Selecteer of de server en de prijzen laag voor de secundaire database configureren.

    ![secundaire configureren](./media/sql-database-geo-replication-portal/create-secondary.png)

5. U kunt desgewenst een secundaire database toevoegen aan een elastische database-toepassingen:

 De secundaire database maken in een groep, klikt u op **elastische database groep** en selecteert u een groep op de doelserver. Een groep moet al bestaan op de doelserver als deze werkstroom een groep maakt.

6. Klik op **maken** om de secundaire toevoegen.
 
6. De secundaire database gemaakt en wordt de seeding proces begint. 
 
    ![secundaire configureren](./media/sql-database-geo-replication-portal/seeding0.png)

7. Wanneer de seeding voltooid is, wordt de secundaire database de status ervan.

    ![seeding voltooid](./media/sql-database-geo-replication-portal/seeding-complete.png)


## <a name="remove-secondary-database"></a>Secundaire database verwijderen

De bewerking wordt beëindigd de replicatie naar de secundaire database permanent en de rol van de secundaire verandert in een normale alleen-lezen database. Als de verbinding met de secundaire database verbroken wordt, de opdracht is uitgevoerd maar wordt de secundaire wordt niet alleen-lezen nadat verbinding wordt hersteld.  

1. Ga naar de primaire database in het partnerschap Geo-replicatie in [Azure portal](http://portal.azure.com).
2. Selecteer op de pagina SQL-Database, **Geo-replicatie**.
3. Selecteer in de lijst met **secundaire servers voor** de database die u wilt verwijderen uit het partnerschap Geo-replicatie.
4. Klik op **Replicatie stoppen**.

    ![secundaire verwijderen](./media/sql-database-geo-replication-portal/remove-secondary.png)

5. Op **Replicatie stoppen** klikt, verschijnt een bevestigingsvenster dus klikt u op **Ja** om de database te verwijderen uit het partnerschap Geo-replicatie (ingesteld met een alleen-lezen database die geen deel uitmaakt van de replicatie).


## <a name="next-steps"></a>Volgende stappen

- Zie meer informatie over actieve Geo-replicatie, - [Actieve Geo-replicatie](sql-database-geo-replication-overview.md)
- Zie voor een overzicht van business continuity en scenario's [Business continuity-overzicht](sql-database-business-continuity.md)

