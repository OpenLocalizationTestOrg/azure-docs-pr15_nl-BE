<properties 
    pageTitle="Actieve Geo-replicatie configureren voor Azure SQL-Database met PowerShell | Microsoft Azure" 
    description="Actieve Geo-replicatie configureren voor Azure PowerShell met SQL-Database" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
   ms.workload="NA"
    ms.date="07/14/2016"
    ms.author="sstein"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-powershell"></a>Geo-replicatie configureren voor Azure SQL-Database met PowerShell

> [AZURE.SELECTOR]
- [Overzicht](sql-database-geo-replication-overview.md)
- [Azure Portal](sql-database-geo-replication-portal.md)
- [PowerShell](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

In dit artikel wordt beschreven hoe u actieve Geo-replicatie configureren voor SQL-Database met PowerShell.

Om te beginnen met PowerShell failover, Zie [een failover- of niet gepland voor Azure SQL-Database met PowerShell](sql-database-geo-replication-failover-powershell.md).

>[AZURE.NOTE] Actieve Geo-replicatie (secundaire servers kan worden gelezen) is nu beschikbaar voor alle databases in alle Servicelagen. In April 2017 het secundaire niet-leesbare type wordt ingetrokken en de bestaande niet-leesbare databases automatisch worden bijgewerkt naar secundaire servers voor kan worden gelezen.



Actieve Geo-replicatie configureren via PowerShell, moet u het volgende:

- Een abonnement op Azure. 
- Azure SQL-database - de primaire database die u wilt repliceren.
- Azure PowerShell 1.0 of hoger. U kunt downloaden en installeren van de Azure PowerShell modules door volgende [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).


## <a name="configure-your-credentials-and-select-your-subscription"></a>Uw referenties te configureren en selecteer uw abonnement

Eerst moet u access maken met uw account Azure dus starten van PowerShell en voer vervolgens de volgende cmdlet. Typ in het aanmeldingsscherm hetzelfde e-mailadres en wachtwoord die u gebruikt voor aanmelding bij de Azure portal.


    Login-AzureRmAccount

Na het aanmelden is ziet u enkele gegevens op het scherm met de Id die u aangemeld met en u toegang tot hebt Azure abonnementen.


### <a name="select-your-azure-subscription"></a>Selecteer uw abonnement Azure

Selecteer het abonnement moet u uw abonnement-id. Kunt u de abonnements-Id die uit de vorige stap wordt weergegeven of als er meerdere abonnementen en meer informatie kunt u de cmdlet **Get-AzureRmSubscription** uitvoeren en kopieer de abonnementsgegevens van het gewenste van de resultaatset. De volgende cmdlet gebruikt de abonnements-Id in te stellen van het huidige abonnement:

    Select-AzureRmSubscription -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

**Selecteer AzureRmSubscription** met succes uitgevoerd u terug naar de PowerShell-prompt.


## <a name="add-secondary-database"></a>Secundaire database toevoegen


De volgende stappen wordt een nieuwe secundaire database maken in een partnerschap Geo-replicatie.  
  
Als u een secundair moet u het abonnement eigenaar of mede-eigenaar zijn. 

U kunt de cmdlet **New-AzureRmSqlDatabaseSecondary** een secundaire database op een partnerserver met een lokale database op de server waarmee u bent verbonden (de primaire database) toevoegen. 

Deze cmdlet vervangen **Start AzureSqlDatabaseCopy** door de parameter **-IsContinuous** .  Er wordt een **AzureRmSqlDatabaseSecondary** -object dat kan worden gebruikt door andere cmdlets duidelijk aangeven dat een bepaalde replicatiekoppeling uitvoer. Deze cmdlet wordt geretourneerd wanneer de secundaire database wordt gemaakt en volledig overgeënt. Afhankelijk van de grootte van de database kan duren van minuten tot uren.

De gerepliceerde database op de secundaire server heeft dezelfde naam als de database op de primaire server en, standaard, hebben hetzelfde. De secundaire database kan kan worden gelezen of niet-leesbare, en een enkele database of een database elastische. Zie voor meer informatie, [Nieuwe AzureRMSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603689.aspx) en [Service Tiers](sql-database-service-tiers.md).
Nadat u de secundaire gemaakt en overgeënt begint gegevens uit de primaire database repliceren naar de nieuwe secundaire database. De volgende stappen wordt beschreven hoe u hiervoor niet-leesbare en leesbaar secundaire servers, met een enkele database of een elastische database maken met behulp van PowerShell.

De opdracht mislukt als de database partner al bestaat (bijvoorbeeld - als gevolg van het beëindigen van een relatie met vorige Geo-replicatie).



### <a name="add-a-non-readable-secondary-single-database"></a>Een niet-leesbare secundair (één database) toevoegen

De volgende opdracht maakt u een niet-leesbare secundair van database 'mijndb' van server 'srv2' resource group "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" -AllowConnections "No"



### <a name="add-readable-secondary-single-database"></a>Leesbare secundaire (één database) toevoegen

De volgende opdracht maakt u een leesbare secundair van database 'mijndb' van server 'srv2' resource group "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" -AllowConnections "All"




### <a name="add-a-non-readable-secondary-elastic-database"></a>Een niet-leesbare secundair (elastische database) toevoegen

De volgende opdracht maakt een niet-leesbare secundair van database 'mijndb' in de groep met de naam 'ElasticPool1' van server 'srv2' resource group "rg2" met elastische database:

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" –SecondaryElasticPoolName "ElasticPool1" -AllowConnections "No"


### <a name="add-a-readable-secondary-elastic-database"></a>Een leesbare secundair (elastische database) toevoegen

De volgende opdracht maakt een leesbare secundair van database 'mijndb' in de groep met de naam 'ElasticPool1' van server 'srv2' resource group "rg2" met elastische database:

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" –SecondaryElasticPoolName "ElasticPool1" -AllowConnections "All"





## <a name="remove-secondary-database"></a>Secundaire database verwijderen

Gebruik de cmdlet **Verwijderen AzureRmSqlDatabaseSecondary** de replicatie partnerschap tussen een secundaire database en de bijbehorende primaire definitief te beëindigen. Na de beëindiging van de relatie wordt de tweede database een database voor lezen en schrijven. De opdracht is uitgevoerd als de secundaire database verbinding verbroken wordt, maar de secundaire worden alleen-lezen nadat de verbinding is hersteld. Zie voor meer informatie [Verwijderen AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603457.aspx) en [Service Tiers](sql-database-service-tiers.md).

Deze cmdlet vervangt Stop AzureSqlDatabaseCopy voor replicatie. 

Het verwijderen is het equivalent van een geforceerde afsluiting die wordt verwijderd van de replicatiekoppeling en de voormalige secundaire blijft als een afzonderlijke database die niet volledig is gerepliceerd voor beëindiging. Alle koppelingsgegevens worden opgeschoond op de voormalige primaire en secundaire, voormalige als of wanneer ze beschikbaar zijn. Deze cmdlet retourneert als de replicatiekoppeling wordt verwijderd. 


Gebruikers moeten beschikken over schrijfrechten voor primaire en secundaire databases volgens RBAC wilt verwijderen secundaire. Zie op rollen gebaseerd toegangsbeheer voor meer informatie.

De volgende verwijdert replicatiekoppeling van de database met de naam 'mijndb' naar de server 'srv2' van de resource-groep "rg2". 

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | Get-AzureRmSqlDatabaseReplicationLink –SecondaryResourceGroup "rg2" –PartnerServerName "srv2"
    $secondaryLink | Remove-AzureRmSqlDatabaseSecondary 


## <a name="monitor-geo-replication-configuration-and-health"></a>Configuratie van geo-replication monitor en de gezondheid

Controle taken omvatten het toezicht op de configuratie van de geo-replicatie en de controle van de gezondheid voor replicatie van gegevens.  

[Get-AzureRmSqlDatabaseReplicationLink](https://msdn.microsoft.com/library/mt619330.aspx) kan worden gebruikt voor het ophalen van de informatie over de forward replicatiekoppelingen zichtbaar in de weergave van de catalogus sys.geo_replication_links.

De volgende opdracht wordt de status van de replicatiekoppeling tussen de primaire database 'mijndb' en de secundaire op server 'srv2' van de resource-groep "rg2" opgehaald.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | Get-AzureRmSqlDatabaseReplicationLink –PartnerResourceGroup "rg2” –PartnerServerName "srv2”


## <a name="next-steps"></a>Volgende stappen

- Zie meer informatie over actieve Geo-replicatie, - [Actieve Geo-replicatie](sql-database-geo-replication-overview.md)
- Zie voor een overzicht van business continuity en scenario's [Business continuity-overzicht](sql-database-business-continuity.md)

