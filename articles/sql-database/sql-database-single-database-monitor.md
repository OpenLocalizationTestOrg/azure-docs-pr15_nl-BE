<properties
    pageTitle="Controleren van de databaseprestaties in Azure SQL-Database | Microsoft Azure"
    description="Meer informatie over de opties voor het controleren van uw database met Azure's en het beheer van dynamische weergaven."
    keywords="prestatiecontrole, cloud database database"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="09/27/2016"
    ms.author="carlrab"/>

# <a name="monitoring-database-performance-in-azure-sql-database"></a>Controleren van de databaseprestaties in Azure SQL-Database
Controleren van de prestaties van een SQL-database in Azure begint met het toezicht op het gebruik van bronnen ten opzichte van het niveau van de prestaties van de database die u kiest. Monitoring helpt die u bepalen of de database overtollige capaciteit is of problemen omdat resources maximum is zijn overschreden, en vervolgens beslissen of is het tijd om de prestaties en [service niveaus](sql-database-service-tiers.md) van uw database aanpassen. U kunt uw database met behulp van grafische hulpprogramma's in de [portal Azure](https://portal.azure.com) of met behulp van SQL- [weergaven van dynamische management](https://msdn.microsoft.com/library/ms188754.aspx)controleren.

## <a name="monitor-databases-using-the-azure-portal"></a>Databases met behulp van de portal Azure controleren

In de [Azure portal](https://portal.azure.com/)kunt u één database gebruik controleren door de database te selecteren en te klikken op de grafiek **controle** . Verschijnt een venster met **Metric** die u wijzigen kunt door te klikken op de knop **grafiek te bewerken** . Voeg de volgende parameters:

- CPU-percentage
- Percentage DTU
- Gegevens i/o-percentage
- Percentage van database-formaat

Zodra u deze gegevens hebt toegevoegd, kunt u bekijken in de grafiek **controle** met meer informatie over het venster **Metric** blijven. Alle zien vier het gemiddeld gebruik percentage ten opzichte van de **DTU** van uw database. Zie het artikel [Servicelagen](sql-database-service-tiers.md) voor meer informatie over DTUs.

![Service tier toezicht op de prestaties van de database.](./media/sql-database-service-tiers/sqldb_service_tier_monitoring.png)

U kunt ook waarschuwingen configureren op de prestatiegegevens. Klik op de knop **waarschuwing toevoegen** in het venster **Metric** . Volg de wizard om de waarschuwing te configureren. Als de parameters een bepaalde drempel overschrijden of als de metric onder een bepaalde drempel valt hebt u de mogelijkheid om een waarschuwing.

Bijvoorbeeld, als u verwacht de belasting op de database om te groeien dat, kunt u voor het configureren van een waarschuwingsbericht per e-mail zodra uw database 80% op een van de prestatiegegevens bereikt. U kunt dit gebruiken als een vroegtijdige waarschuwing om erachter te komen wanneer u mogelijk over te schakelen naar het eerstvolgende hogere prestatieniveau.

De prestatiegegevens kunt u nagaan of u kunt teruggaan naar een lager prestatieniveau. Stel dat u een database standaard S2 en alle prestatiegegevens weergeven dat de database gemiddeld niet meer dan 10% op elk gewenst moment gebruiken. Het is waarschijnlijk dat de database goed in standaard S1 werkt. Evenwel rekening houden met de werklast die oploopt of schommelen alvorens het besluit te verplaatsen naar een lager prestatieniveau.

## <a name="monitor-databases-using-dmvs"></a>Monitor databases met behulp van DMVs

De parameters die beschikbaar zijn in de portal zijn ook beschikbaar via systeemweergaven: [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) in het logische **model** -database van de server en de [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) in de database. Gebruik **sys.resource_stats** als u wilt controleren van een minder gedetailleerde gegevens over een langere periode. Gebruik **sys.dm_db_resource_stats** als u nodig hebt om meer gedetailleerde gegevens binnen een kleinere tijd te controleren. Zie [Richtlijnen voor Azure SQL Database prestaties](sql-database-performance-guidance.md#monitoring-resource-use-with-sysresourcestats)voor meer informatie.

>[AZURE.NOTE] **sys.dm_db_resource_stats** geeft als resultaat een lege resultaatset gebruikt in Web- en Business edition-databases die zijn ingetrokken.

Voor elastische database van toepassingen, kunt u afzonderlijke databases in de groep controleren met de technieken die in deze sectie worden beschreven. Maar u kunt ook controleren de groep als geheel. Zie voor informatie [Monitor en een elastische database toepassingen beheren](sql-database-elastic-pool-manage-portal.md).
