<properties
    pageTitle="Databases verplaatsen tussen servers, tussen abonnementen en in Azure."
    description="Snelle stappen te kopiëren, verplaatsen en gegevens en databases in Azure SQL-Database migreren."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="move-databases-between-servers-between-subscriptions-and-in-and-out-of-azure"></a>Databases verplaatsen tussen servers, tussen abonnementen en in Azure

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]
##<a name="to-move-a-database-to-a-different-server-in-the-same-subscription"></a>Een database verplaatsen naar een andere server in het abonnement op dezelfde
- Klik op **SQL-databases**in de [Portal Azure](https://portal.azure.com), selecteert u een database in de lijst en klik op **kopiëren**. Zie [een Azure SQL-database kopiëren](sql-database-copy.md) voor meer informatie.

## <a name="to-move-a-database-between-subscriptions"></a>Een database verplaatsen tussen abonnementen
- In de [Portal Azure](https://portal.azure.com), klikt u op **SQL-servers** en selecteer vervolgens de server die fungeert als host voor de database in de lijst. Klik op **verplaatsen**en kies vervolgens de bronnen te verplaatsen en het abonnement te verplaatsen.

## <a name="to-migrate-a-sql-database-into-azure"></a>Een SQL-database migreren naar Azure
- Databasecompatibiliteit bepalen en kies vervolgens de juiste migratiemethode op basis van uw behoeften. Volg de richtlijnen en de opties in [een SQL Server-database te migreren](sql-database-cloud-migrate.md).

## <a name="to-create-a-copy-of-a-database-for-use-outside-of-azure"></a>Voor het maken van een kopie van een database voor gebruik buiten Azure
- [Bacpac-bestand exporteren.](sql-database-export.md)
