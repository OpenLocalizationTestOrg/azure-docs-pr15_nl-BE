< eigenschappen
    
    pageTitle="Upgrade to the latest elastic database client library | Microsoft Azure" 
    description="Upgrade apps and libraries using Nuget" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="upgrade-an-app-to-use-the-latest-elastic-database-client-library"></a>Een app voor het gebruik van de meest recente client-bibliotheek van elastische database bijwerken

Nieuwe versies van de [client-bibliotheek van elastische Database](sql-database-elastic-database-client-library.md) zijn beschikbaar via NuGetand, de Manager van de NuGetPackage-interface in Visual Studio. Upgrades bevatten correcties en ondersteuning voor de nieuwe mogelijkheden van de clientbibliotheek.

**Voor de meest recente versie:** Ga naar [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

Opnieuw opbouwen van uw toepassing met de nieuwe bibliotheek, evenals uw bestaande Shard kaart Manager opgeslagen metagegevens in Azure SQL-Databases ter ondersteuning van nieuwe functies te wijzigen.

Deze stappen in volgorde uitvoert, zorgt u ervoor dat oude versies van de clientbibliotheek die niet meer aanwezig in uw omgeving als metagegevensobjecten zijn bijgewerkt, hetgeen betekent dat oude versie metagegevensobjecten na de upgrade niet gemaakt.   

## <a name="upgrade-steps"></a>Upgradestappen

**1. uw toepassingen bijwerken.** In Visual Studio downloaden en verwijst naar de meest recente versie van de client-bibliotheek in alle ontwikkelingsprojecten die gebruikmaken van de bibliotheek; vervolgens bouwen en implementeren. 

 * Selecteer in de Visual Studio-oplossing, **Extra** --> **NuGet Package Manager** -->  **NuGet pakketten beheren voor oplossing**. 
 * (Visual Studio 2013) Selecteer **Updates**en selecteer vervolgens de knop **bijwerken** op de verpakking **Azure SQL Database elastische schaal Client-bibliotheek** wordt weergegeven in het venster in het linkerpaneel.
 * (Visual Studio 2015) Stel het Filter op **Upgrade beschikbaar is**. Selecteer het pakket bij te werken en klik op de knop **bijwerken** .
    
 
 * Bouwen en implementeren. 

**2. uw scripts bijwerken.** Als u via **PowerShell** scripts kopiëren naar de map van waaruit u de scripts voor het uitvoeren en beheren van shards, [download de nieuwe versie van de bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) . 

**3. upgrade uw service gesplitste samenvoegen.** Als u het hulpprogramma elastische database splitsen samenvoegen een laptopgeheugen [downloaden en installeren van de meest recente versie van het hulpprogramma](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/)gegevens indelen. Nadere upgrade stappen voor de Service vindt u [hier](sql-database-elastic-scale-overview-split-and-merge.md). 

**4. upgrade uw databases Shard kaart Manager**. De metagegevens ondersteunen uw kaarten Shard in Azure SQL-Database bijwerken.  Er zijn twee manieren u kunt dit doen met PowerShell of C#. Beide opties worden hieronder weergegeven.

***Optie 1: Werken met PowerShell metagegevens***

1. De meest recente hulpprogramma voor de opdrachtregel voor NuGet van [hier](http://nuget.org/nuget.exe) downloaden en opslaan in een map. 

2. Open een opdrachtprompt en Ga naar de map met de opdracht:`nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`

3. Ga naar de submap met nieuwe dll-versie van de client die hebt u zojuist hebt gedownload, bijvoorbeeld:`cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`

4. De elastische database client upgrade scriptlet downloaden van het [Script Center](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9)en sla het bestand in dezelfde map waarin het DLL-bestand.

5. '.\upgrade.ps1 PowerShell' uitvoeren vanaf de opdrachtprompt van die map, en volg de aanwijzingen.
 
***Optie 2: Upgrade van metagegevens voor C#***

U kunt ook een Visual Studio-toepassing die uw ShardMapManager wordt geopend en de upgrade van metagegevens door het aanroepen van de methoden [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) en [UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) , zoals in dit voorbeeld voert itereert over alle shards maken: 

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy); 
    smm.UpgradeGlobalStore(); 
    
    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations()) 
    {   
       smm.UpgradeLocalStore(loc); 
    } 

Deze technieken voor upgrades van de metagegevens kunnen meerdere keren worden toegepast zonder schade. Bijvoorbeeld, als een oudere clientversie wordt een shard per ongeluk gemaakt nadat u al hebt bijgewerkt, kunt u uitvoeren upgrade opnieuw over alle shards om ervoor te zorgen dat de meest recente metagegevensversie in uw infrastructuur aanwezig is. 

**Opmerking:**  Nieuwe versies van de clientbibliotheek gepubliceerd tot heden blijven werken met eerdere versies van de metagegevens Shard kaart Manager op Azure SQL DB en vice versa.   Maar om te profiteren van een aantal nieuwe functies in de meest recente client, metagegevens moet worden bijgewerkt.   Houd er rekening mee dat metagegevens upgrades heeft geen invloed op alle gebruikersgegevens of toepassingsspecifieke gegevens, alleen objecten die zijn gemaakt en gebruikt door de Manager van de Map Shard.  En de toepassingen blijven functioneren door het upgradeproces beschreven. 

## <a name="elastic-database-client-version-history"></a>Elastische database client versiegeschiedenis 

Ga naar [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) voor de versie-historie


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]  


<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png
 