<properties
    pageTitle="Azure SQL-Databases met Azure automatisering beheren | Microsoft Azure"
    description="Meer informatie over hoe de automatisering Azure-service kan worden gebruikt voor het beheren van Azure SQL-databases op een schaal."
    services="sql-database, automation"
    documentationCenter=""
    authors="jodoglevy"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/26/2016"
    ms.author="jolevy"/>



#<a name="managing-azure-sql-databases-using-azure-automation"></a>Azure Azure automatisering met SQL-Databases beheren

Deze gids laat u kennismaken met de automatisering van Azure-service en hoe deze kan worden gebruikt om het beheer van uw databases SQL Azure te vereenvoudigen.


## <a name="what-is-azure-automation"></a>Wat is Azure automatisering?

[Azure automatisering](https://azure.microsoft.com/services/automation/) is een Azure voor cloud beheer via automatisering van bedrijfsprocessen te vereenvoudigen. Met Azure automatisering, worden langdurige, handmatige lastige en vaak herhaalde taken geautomatiseerd om betrouwbaarheid, efficiëntie en tijd waarde voor uw organisatie te vergroten.

Azure automatisering biedt een zeer betrouwbare en uiterst beschikbare uitvoering workflowengine die wordt aangepast aan uw behoeften als uw organisatie groeit. In Azure automatisering, processen volledig uit handmatig, door 3de partij systemen of regelmatig zodat taken gebeurt precies wanneer dit nodig is.

Lagere operationele overhead en vrij IT / DevOps medewerkers zich richten op werk dat business toegevoegd door het verplaatsen van uw taken automatisch worden uitgevoerd door de automatisering van Azure cloud waarde.


## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a>Hoe kan automatisering Azure helpen Azure SQL-databases beheren

Azure SQL-Database kunnen worden beheerd in Azure automatisering met behulp van de [Azure SQL Database PowerShell-cmdlets](https://msdn.microsoft.com/library/dn546723.aspx) die beschikbaar zijn in de [Azure PowerShell tools](https://msdn.microsoft.com/library/azure/jj156055.aspx). Azure automatisering heeft deze Azure SQL Database PowerShell cmdlets beschikbaar uit de verpakking, zodat u kunt alle taken binnen de service SQL DB beheer uitvoeren. U kunt ook deze cmdlets in Azure automatisering met de cmdlets voor andere Azure diensten, complexe taken automatiseren via Azure services en 3de partij systemen combineren.

Azure automatisering heeft ook de mogelijkheid om te communiceren met SQL-servers rechtstreeks, via de uitgifte van SQL-opdrachten met PowerShell.

De [Azure automatisering runbook galerie](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) bevat tal van team en Gemeenschap runbooks product aan de slag automatiseren beheer van Azure SQL-Databases, andere Azure diensten en systemen met 3de partij. Galerie runbooks omvatten:

 * [SQL-query's uitvoeren op een SQL Server-database](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
 * [Verticaal schalen (omhoog of omlaag) Azure SQL-Database op een schema](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
 * [Een SQL-tabel wordt afgekapt als de database de maximale omvang nadert](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
 * [Tabellen in een Azure SQL-Database indexeren als ze sterk gefragmenteerd zijn](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd dat de basisbeginselen van Azure automatisering en hoe deze kan worden gebruikt voor het beheren van Azure SQL-databases, klik op deze koppelingen voor meer informatie over Azure automatisering.

- [Overzicht automatisering Azure](../automation/automation-intro.md)
- [Mijn eerste runbook](../automation/automation-first-runbook-graphical.md)
- [Azure leren voor automatisering toewijzen](https://azure.microsoft.com/documentation/learning-paths/automation/)
- [Azure automatisering: Uw SQL Agent in de Cloud](https://azure.microsoft.com/blog/2014/06/26/azure-automation-your-sql-agent-in-the-cloud/) 
 
