<properties
    pageTitle="Overzicht: hulpprogramma's voor SQL-Database | Microsoft Azure"
    description="Vergelijkt de hulpprogramma's en opties voor het beheren van Azure SQL-Database"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="sstein"/>

# <a name="overview-management-tools-for-sql-database"></a>Overzicht: hulpprogramma's voor SQL-Database

In dit onderwerp wordt verkend en vergelijkt gereedschappen en opties voor het beheren van Azure SQL-databases, zodat u het juiste gereedschap voor de taak, uw bedrijf en u kunt kiezen. Kiezen van het juiste gereedschap is afhankelijk van hoeveel databases beheert u, de taak en hoe vaak een taak wordt uitgevoerd.

## <a name="azure-portal"></a>Azure portal

De [Azure portal](https://portal.azure.com) is een webtoepassing waar u kunt maken, bijwerken, en databases en logische servers verwijderen en controleren van activiteit in een database. Dit hulpprogramma is handig als u zich gewoon aan de slag met Azure, beheer van enkele databases, of om iets te doen snel.

Zie [de Azure portal met behulp van SQL-Databases beheren](sql-database-manage-portal.md)voor meer informatie over het gebruik van de portal.

## <a name="sql-server-management-studio-and-sql-server-data-tools-in-visual-studio"></a>SQL Server Management Studio en SQL Server Data Tools in Visual Studio

SQL Server Management Studio (SSMS) en SQL Server Data Tools (SSDT) zijn de clientprogramma's die worden uitgevoerd op uw computer voor het beheren en ontwikkelen van uw database in de cloud. Als u bekend bent met Visual Studio of andere geïntegreerde ontwikkelomgevingen (IDEs), [gebruik van SSDT in Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)toepassingen ontwikkelt. Veel databasebeheerders bent bekend met SSMS, die kan worden gebruikt met Azure SQL-databases. [Download de nieuwste versie van SSMS](https://msdn.microsoft.com/library/mt238290) en gebruik altijd de meest recente versie als u werkt met Azure SQL-Database. Zie voor meer informatie over het beheren van uw Azure SQL-Databases met SSMS [SQL-Databases beheren met behulp van SSMS](sql-database-manage-azure-ssms.md).

> [AZURE.IMPORTANT] Gebruik altijd de meest recente versie van [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290) en [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) om met updates voor Microsoft Azure en SQL-Database gesynchroniseerd blijven.


## <a name="powershell"></a>PowerShell

Kunt u PowerShell databases en elastische database toepassingen te beheren en automatiseren Azure resource-implementaties. Microsoft raadt u aan dit hulpprogramma voor het beheren van een groot aantal databases en deployment en resource wijzigingen in een productieomgeving te automatiseren.

Zie [SQL Database beheren met PowerShell](sql-database-manage-powershell.md) voor meer informatie.

## <a name="elastic-database-tools"></a>Elastische hulpmiddelen voor databases
De elastische database tools gebruiken om acties te voeren, zoals 

* T-SQL script gebruikt met een set van databases met behulp van een [elastische taak](sql-database-elastic-jobs-overview.md) uitvoeren
* Model met meerdere huurder databases verplaatsen naar een model met één huurder met het [hulpprogramma gesplitste samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md)
* Beheer van databases in een model met één huurder of pachter met meerdere model met behulp van de [elastische schaal client library](sql-database-elastic-database-client-library.md).
 

## <a name="additional-resources"></a>Aanvullende bronnen

- [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)
- [Azure automatisering](https://azure.microsoft.com/documentation/services/automation/)
- [Azure Scheduler](https://azure.microsoft.com/documentation/services/scheduler/)