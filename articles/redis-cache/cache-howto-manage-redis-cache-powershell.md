<properties
    pageTitle="Beheer van Cache met Azure PowerShell Azure bestand Vgx. | Microsoft Azure"
    description="Informatie over het uitvoeren van beheertaken voor Azure bestand Vgx. Cache met Azure PowerShell."
    services="redis-cache"
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="manage-azure-redis-cache-with-azure-powershell"></a>Beheer van Cache met Azure PowerShell Azure bestand Vgx.

> [AZURE.SELECTOR]
- [PowerShell](cache-howto-manage-redis-cache-powershell.md)
- [Azure CLI](cache-manage-cli.md)

In dit onderwerp ziet u hoe het uitvoeren van veelvoorkomende taken, zoals maken, bijwerken en schalen van uw sessies Azure bestand Vgx. Cache, hoe toegangstoetsen genereren en het weergeven van informatie over uw caches. Zie voor een volledige lijst van Azure bestand Vgx. Cache PowerShell cmdlets [Azure bestand Vgx. Cache cmdlets](https://msdn.microsoft.com/library/azure/mt634513.aspx).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)][Klassiek model](../resource-manager-deployment-model.md#classic-deployment-characteristics).

## <a name="prerequisites"></a>Vereisten

Als u al Azure PowerShell hebt geïnstalleerd, hebt u Azure PowerShell versie 1.0.0 of hoger. U kunt controleren welke versie van Azure PowerShell die u met deze opdracht bij de opdrachtprompt Azure PowerShell hebt geïnstalleerd.

    Get-Module azure | format-table version

[AZURE.INCLUDE [powershell-preview](../../includes/powershell-preview-inline-include.md)]

Ten eerste moet u zich voor Azure met deze opdracht.

    Login-AzureRmAccount

Geef het e-mailadres van uw Azure-account en het wachtwoord in het dialoogvenster Microsoft Azure-in.

Als er meerdere abonnementen voor Azure, moet u vervolgens uw abonnement Azure instellen. Een lijst van uw huidige abonnementen wilt bekijken, moet u deze opdracht uitvoert.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Als u het abonnement, kunt u de volgende opdracht uitvoeren. In het volgende voorbeeld wordt de naam van het abonnement is `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

Voordat u Windows PowerShell Azure Resource Manager te gebruiken kunt, moet u het volgende:

- Windows PowerShell, versie 3.0 of 4.0. De versie van Windows PowerShell, typt:`$PSVersionTable` en controleer of de waarde van `PSVersion` 3.0 of 4.0. Zie [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) of [4.0 voor Windows Management Framework](http://www.microsoft.com/download/details.aspx?id=40855)installeren van een compatibele versie.

Voor gedetailleerde informatie over elke cmdlet u in deze zelfstudie ziet, gebruikt u de cmdlet Get-Help.

    Get-Help <cmdlet-name> -Detailed

Bijvoorbeeld Help-informatie voor de `New-AzureRmRedisCache` cmdlet, type:

    Get-Help New-AzureRmRedisCache -Detailed

### <a name="how-to-connect-to-azure-government-cloud-or-azure-china-cloud"></a>Verbinding maken met Azure overheid Cloud of Azure China Cloud

De Azure omgeving is standaard `AzureCloud`, die de globale wolk van Azure exemplaar vertegenwoordigt. Gebruiken om te verbinden met een andere instantie, de `Add-AzureRmAccount` opdracht met de `-Environment` of -`EnvironmentName` schakeloptie de opdrachtregel met de gewenste omgeving of het milieu.

Als u wilt zien in de lijst met beschikbare omgevingen, de `Get-AzureRmEnvironment` cmdlet.

### <a name="to-connect-to-the-azure-government-cloud"></a>Verbinding maken met de overheid Azure Cloud

De overheid Azure Cloud verbinding wordt een van de volgende opdrachten te gebruiken.

    Add-AzureRMAccount -EnvironmentName AzureUSGovernment

of

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureUSGovernment)

Gebruik wilt maken van een cache in de Cloud Azure regering, een van de volgende locaties.

-   USGov-Virginia
-   USGov-Iowa

Zie [Microsoft Azure overheid](https://azure.microsoft.com/features/gov/) en [Microsoft Azure overheid Developer Guide](../azure-government-developer-guide.md)voor meer informatie over de overheid Azure Cloud.

### <a name="to-connect-to-the-azure-china-cloud"></a>Verbinding maken met de China Azure Cloud

Voor verbinding met de Cloud Azure China, een van de volgende opdrachten te gebruiken.

    Add-AzureRMAccount -EnvironmentName AzureChinaCloud

of

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureChinaCloud)

Gebruik wilt maken van een cache in de Cloud Azure China, een van de volgende locaties.

-   East China
-   China-Noord

Zie voor meer informatie over de China Azure Cloud, [AzureChinaCloud voor Azure beheerd door 21Vianet in China](http://www.windowsazure.cn/).

### <a name="properties-used-for-azure-redis-cache-powershell"></a>Eigenschappen die worden gebruikt voor Azure PowerShell van Cache voor bestand Vgx.

De volgende tabel bevat de eigenschappen en omschrijvingen voor vaak gebruikte parameters bij het maken en beheren van uw Azure bestand Vgx. Cache sessies met Azure PowerShell.

| Parameter          | Beschrijving                                                                                                                                                                                                        | Standaard  |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| Naam               | De naam van de cache                                                                                                                                                                                                  |          |
| Locatie           | Locatie van de cache                                                                                                                                                                                              |          |
| ResourceGroupName  | Resourcegroepnaam waarop de cache maken                                                                                                                                                                   |          |
| Grootte               | De grootte van de cache. Geldige waarden zijn: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1GB, 2,5 GB, 6 GB, 13 GB, 26 GB, 53 GB                                                                     | 1GB      |
| ShardCount         | Het aantal shards te maken bij het maken van een cache premium terwijl clusteren is ingeschakeld. Geldige waarden zijn: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10                                                                                                      |          |
| SKU                | Hiermee geeft u de SKU van de cache. Geldige waarden zijn: Basic, Standard, Premium                                                                                                                                         | Standaard |
| RedisConfiguration | Hiermee geeft u een bestand Vgx. configuratie-instellingen. Zie voor meer informatie over de instellingen in de volgende tabel met [Eigenschappen van de RedisConfiguration](#redisconfiguration-properties) . |          |
| EnableNonSslPort   | Geeft aan of de niet-SSL-poort is ingeschakeld.                                                                                                                                                                     | False    |
| MaxMemoryPolicy    | Deze parameter is afgeschaft - RedisConfiguration gebruiken.                                                                                                                                              |          |
| StaticIP           | Wanneer de cache in een VNET die als host optreedt, geeft een uniek IP-adres in het subnet voor de cache. Als u niet opgeeft, wordt een voor u gekozen uit het subnet.                                                                                                                     |          |
| Subnet             | Wanneer de cache in een VNET die als host optreedt, geeft de naam van het subnet waarop de cache te implementeren.                                                                                                                  |          |
| VirtualNetwork     | Wanneer de cache in een VNET die als host optreedt, geeft u het bron-ID van de VNET voor het implementeren van de cache.                                                                                                             |          |
| KeyType            | Hiermee geeft u aan welke toegangssleutel te genereren tijdens het vernieuwen van de toegangstoetsen. Geldige waarden zijn: primaire, secundaire |  |                                                                                                                                                                                                              |          |


### <a name="redisconfiguration-properties"></a>RedisConfiguration eigenschappen

| Eigenschap                      | Beschrijving                                                                                                          | Prijzen lagen       |
|-------------------------------|----------------------------------------------------------------------------------------------------------------------|---------------------|
| RDB back-up is ingeschakeld            | Of het [bestand Vgx. persistentie van gegevens](cache-how-to-premium-persistence.md) is ingeschakeld                                     | Alleen Premium        |
| RDB-opslag-verbindingsreeks | De verbindingsreeks op de rekening van de opslag voor [persistentie van gegevens bestand Vgx.](cache-how-to-premium-persistence.md)       | Alleen Premium        |
| RDB-back-up-frequentie          | De back-up frequentie voor [persistentie van gegevens bestand Vgx.](cache-how-to-premium-persistence.md)                               | Alleen Premium        |
| maxmemory gereserveerd            | Hiermee configureert u het [geheugen die is gereserveerd](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) voor niet-cache processen | Standard en Premium |
| maxmemory-beleid              | Hiermee configureert u het [beleid voor verwijdering](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) voor de cache           | Alle prijzen lagen   |
| kennis keyspace gebeurtenissen        | [Keyspace meldingen](cache-configure.md#keyspace-notifications-advanced-settings) configureren                     | Standard en Premium |
| hash-max-ziplist-posten      | Hiermee configureert u [geheugen optimalisatie](http://redis.io/topics/memory-optimization) voor kleine samengestelde gegevenstypen          | Standard en Premium |
| hash-max-ziplist-waarde        | Hiermee configureert u [geheugen optimalisatie](http://redis.io/topics/memory-optimization) voor kleine samengestelde gegevenstypen          | Standard en Premium |
| set max-intset posten        | Hiermee configureert u [geheugen optimalisatie](http://redis.io/topics/memory-optimization) voor kleine samengestelde gegevenstypen          | Standard en Premium |
| zset max-ziplist posten      | Hiermee configureert u [geheugen optimalisatie](http://redis.io/topics/memory-optimization) voor kleine samengestelde gegevenstypen          | Standard en Premium |
| zset-max-ziplist-waarde        | Hiermee configureert u [geheugen optimalisatie](http://redis.io/topics/memory-optimization) voor kleine samengestelde gegevenstypen          | Standard en Premium |
| databases                     | Hiermee configureert u het aantal databases. Deze eigenschap kan worden geconfigureerd op cache maken.                          | Standard en Premium |

## <a name="to-create-a-redis-cache"></a>Voor het maken van een Cache bestand Vgx.

Nieuwe exemplaren van het bestand Vgx. Azure-Cache zijn gemaakt met de cmdlet [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) .

>[AZURE.IMPORTANT] De eerste keer dat u een cache bestand Vgx. maken van een abonnement op de Azure portal de portal registreert de `Microsoft.Cache` naamruimte voor dit abonnement. Als u de eerste bestand Vgx. cache in een abonnement met PowerShell maakt probeert, moet u eerst registreren die naamruimte met de volgende opdracht. anders cmdlets, zoals `New-AzureRmRedisCache` en `Get-AzureRmRedisCache` mislukt.
>
>`Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Cache"`

Voor een overzicht van de beschikbare parameters en hun beschrijvingen voor `New-AzureRmRedisCache`, de volgende opdracht uitvoeren.

    PS C:\> Get-Help New-AzureRmRedisCache -detailed
    
    NAME
        New-AzureRmRedisCache
    
    SYNOPSIS
        Creates a new redis cache.
    
    
    SYNTAX
        New-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]
    
    
    DESCRIPTION
        The New-AzureRmRedisCache cmdlet creates a new redis cache.
    
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to create.
    
        -ResourceGroupName <String>
            Name of resource group in which to create the redis cache.
    
        -Location <String>
            Location in which to create the redis cache.
    
        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.
    
        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.
    
        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.
    
        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}
    
        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.
    
        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. If no value is provided, the default value is false and the
            non-SSL port will be disabled. Possible values are true and false.
    
        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.
    
        -VirtualNetwork <String>
            The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}
    
        -Subnet <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.
    
        -StaticIP <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Voer de volgende opdracht om een cache te maken met de standaardparameters.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

`ResourceGroupName`, `Name`, en `Location` parameters vereist zijn, maar de overige zijn optioneel en standaardwaarden hebben. De vorige opdracht wordt uitgevoerd, wordt een instantie SKU Azure bestand Vgx. standaardcache gemaakt met de opgegeven naam, locatie en bronnengroep, 1 GB in grootte met de niet-SSL-poort uitgeschakeld.

Voor het maken van een premium-cache, Geef een grootte van P1 (6 GB tot 60 GB), P2 (13 GB - 130 GB), (26 GB - 260 GB), P3 of P4 (53 GB - 530 GB). Opgeven zodat clustering een shard tellen met de `ShardCount` parameter. In het volgende voorbeeld wordt een cache P1 premium met 3 shards gemaakt. Een cache van de premie P1 6 GB groot is en omdat we drie shards opgegeven totale grootte 18 GB (3 x 6 GB) is.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

Geeft de waarden voor de `RedisConfiguration` parameter, plaatst u de waarden in `{}` als sleutel/waarde-paren, zoals `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`. Het volgende voorbeeld wordt een standaard 1 GB cachegeheugen met `allkeys-random` maxmemory beleid en keyspace meldingen geconfigureerd met `KEA`. Zie voor meer informatie [Keyspace meldingen (Geavanceerd)](cache-configure.md#keyspace-notifications-advanced-settings) en [Maxmemory beleid en maxmemory-gereserveerde](cache-configure.md#maxmemory-policy-and-maxmemory-reserved).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>
## <a name="to-configure-the-databases-setting-during-cache-creation"></a>De databases instellen tijdens het maken van de cache configureren

De `databases` instelling alleen tijdens het maken van de cache kan worden geconfigureerd. Het volgende voorbeeld wordt een premie P3-cache (26 GB) met 48-databases met de cmdlet [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) .

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

Voor meer informatie over de `databases` (eigenschap), Zie [Serverconfiguratie standaard Azure bestand Vgx. Cache](cache-configure.md#default-redis-server-configuration). Zie de vorige sectie [voor het maken van een bestand Vgx. Cache](#to-create-a-redis-cache) voor meer informatie over het maken van een cache met de cmdlet [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) .

## <a name="to-update-a-redis-cache"></a>Voor het bijwerken van een cache bestand Vgx.

Azure Cache bestand Vgx. exemplaren worden bijgewerkt met de cmdlet [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) .

Voor een overzicht van de beschikbare parameters en hun beschrijvingen voor `Set-AzureRmRedisCache`, de volgende opdracht uitvoeren.

    PS C:\> Get-Help Set-AzureRmRedisCache -detailed
    
    NAME
        Set-AzureRmRedisCache
    
    SYNOPSIS
        Set redis cache updatable parameters.
    
    SYNTAX
        Set-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]
    
    DESCRIPTION
        The Set-AzureRmRedisCache cmdlet sets redis cache parameters.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to update.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.
    
        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.
    
        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}
    
        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.
    
        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. The default value is null and no change will be made to the
            currently configured value. Possible values are true and false.
    
        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

De `Set-AzureRmRedisCache` cmdlet kan worden gebruikt om eigenschappen te werken, zoals `Size`, `Sku`, `EnableNonSslPort`, en de `RedisConfiguration` waarden. 

De volgende opdracht werkt het maxmemory-beleid voor de Cache bestand Vgx. met de naam myCache.

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>
## <a name="to-scale-a-redis-cache"></a>Voor het schalen van een cache bestand Vgx.

`Set-AzureRmRedisCache`kan worden gebruikt voor het schalen van een cache Azure bestand Vgx. exemplaar wanneer de `Size`, `Sku`, of `ShardCount` eigenschappen zijn gewijzigd. 

>[AZURE.NOTE]Een cache met PowerShell schalen is onderworpen aan de grenzen en de richtsnoeren als een cache van het portal voor Azure schalen. U kunt schalen naar een andere prijzen laag met de volgende beperkingen.
>
>-  U kan niet uit een hogere prijzen laag schalen naar een lagere prijzen laag.
>    -    U kan niet vanuit een **Premium** cache naar een **standaard** of een **eenvoudige** cache schalen.
>    -    U kan niet uit de cache **standaard** naar een **eenvoudige** cache schalen.
>-  Kunt u de schaal van een **eenvoudige** cache naar cache **standaard** , maar u tegelijkertijd de grootte niet wijzigen. Als u een ander formaat nodig hebt, kunt u de volgende schaal bewerking op de gewenste grootte kunt doen.
>-  U kan niet uit de cache van een **eenvoudige** schaal rechtstreeks naar een **Premium** -cache. U moet de schaal van **Basic** **Standard** in één bewerking van de schaal, en vervolgens van **standaard** **Premium** in een latere bewerking schalen.
>-  Kan niet u de schaal van een groter formaat om de **C0 (250 MB)** formaat.
>
>Voor meer informatie Zie [schaal Azure bestand Vgx. Cache](cache-how-to-scale.md).

In het volgende voorbeeld ziet u hoe een cache met de naam schalen `myCache` naar een 2,5 GB cache. Houd er rekening mee dat deze opdracht voor zowel een Basic of een standaard-cache werkt.

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Nadat deze opdracht is gegeven, wordt de status van de cache geretourneerd (vergelijkbaar met bellen `Get-AzureRmRedisCache`). Houd er rekening mee dat de `ProvisioningState` is `Scaling`.

    PS C:\> Set-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup -Size 2.5GB
    
    
    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/mygroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Scaling
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 150], [notify-keyspace-events, KEA],
                         [maxmemory-delta, 150]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : mygroup
    PrimaryKey         : ....
    SecondaryKey       : ....
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

Als de schaal bewerking voltooid en is de `ProvisioningState` gewijzigd in `Succeeded`. Als u nodig hebt om volgende schaal bewerking, zoals het wijzigen van elementaire standaard en vervolgens de grootte te wijzigen moet u wachten tot de vorige bewerking voltooid is of wordt er een met de volgende strekking foutbericht.

    Set-AzureRmRedisCache : Conflict: The resource '...' is not in a stable state, and is currently unable to accept the update request.

## <a name="to-get-information-about-a-redis-cache"></a>Informatie over een cache bestand Vgx.

U kunt informatie ophalen over een cache met de cmdlet [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) .

Voor een overzicht van de beschikbare parameters en hun beschrijvingen voor `Get-AzureRmRedisCache`, de volgende opdracht uitvoeren.

    PS C:\> Get-Help Get-AzureRmRedisCache -detailed
    
    NAME
        Get-AzureRmRedisCache
    
    SYNOPSIS
        Gets details about a single cache or all caches in the specified resource group or all caches in the current
        subscription.
    
    SYNTAX
        Get-AzureRmRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]
    
    DESCRIPTION
        The Get-AzureRmRedisCache cmdlet gets the details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzureRmRedisCache will return details about the
        specific cache name provided.
    
        If only ResourceGroupName is provided than it will return details about all caches in the specified resource group.
    
        If no parameters are given than it will return details about all caches the current subscription.
    
    PARAMETERS
        -Name <String>
            The name of the cache. When this parameter is provided along with ResourceGroupName, Get-AzureRmRedisCache
            returns the details for the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache or caches. If ResourceGroupName is provided with Name
            then Get-AzureRmRedisCache returns the details of the cache specified by Name. If only the ResourceGroup
            parameter is provided, then details for all caches in the resource group are returned.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Uitvoeren als u wilt informatie over alle caches in het huidige abonnement, `Get-AzureRmRedisCache` zonder parameters.

    Get-AzureRmRedisCache

Uitvoeren als u wilt informatie over alle caches in een bepaalde resourcegroep, `Get-AzureRmRedisCache` met de `ResourceGroupName` parameter.

    Get-AzureRmRedisCache -ResourceGroupName myGroup

Uitvoeren als u informatie over een specifieke cache weer, `Get-AzureRmRedisCache` met de `Name` -parameter met de naam van de cache, en de `ResourceGroupName` parameter met de bronnengroep met die cache.

    PS C:\> Get-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup
    
    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/myGroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Succeeded
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 62], [notify-keyspace-events, KEA],
                         [maxclients, 1000]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : myGroup
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

## <a name="to-retrieve-the-access-keys-for-a-redis-cache"></a>Voor het ophalen van de toegangstoetsen voor een cache bestand Vgx.

U haalt de toegangstoetsen voor de cache, kunt u de cmdlet [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) .

Voor een overzicht van de beschikbare parameters en hun beschrijvingen voor `Get-AzureRmRedisCacheKey`, de volgende opdracht uitvoeren.

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed
    
    NAME
        Get-AzureRmRedisCacheKey
    
    SYNOPSIS
        Gets the accesskeys for the specified redis cache.
    
    
    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]
    
    DESCRIPTION
        The Get-AzureRmRedisCacheKey cmdlet gets the access keys for the specified cache.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Aanroepen voor het ophalen van de sleutels voor de cache, de `Get-AzureRmRedisCacheKey` cmdlet en de naam van de bronnengroep met de cache van de naam van de cache doorgeeft.

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup
    
    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="to-regenerate-access-keys-for-your-redis-cache"></a>Regenereren toegangstoetsen voor uw cache bestand Vgx.

Als u wilt de toegangstoetsen voor de cache genereren, kunt u de cmdlet [New-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) .

Voor een overzicht van de beschikbare parameters en hun beschrijvingen voor `New-AzureRmRedisCacheKey`, de volgende opdracht uitvoeren.

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed
    
    NAME
        New-AzureRmRedisCacheKey
    
    SYNOPSIS
        Regenerates the access key of a redis cache.
    
    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]
    
    DESCRIPTION
        The New-AzureRmRedisCacheKey cmdlet regenerate the access key of a redis cache.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        -KeyType <String>
            Specifies whether to regenerate the primary or secondary access key. Possible values are Primary or Secondary.
    
        -Force
            When the Force parameter is provided, the specified access key is regenerated without any confirmation prompts.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    
Als de primaire of secundaire sleutel voor de cache opnieuw te genereren, bel de `New-AzureRmRedisCacheKey` cmdlet en geeft u de naam van de resourcegroep, en geven een `Primary` of `Secondary` voor de `KeyType` parameter. In het volgende voorbeeld wordt de secundaire toegangstoets voor een cache opnieuw wordt gegenereerd.

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary
    
    Confirm
    Are you sure you want to regenerate Secondary key for redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
    
    
    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="to-delete-a-redis-cache"></a>Verwijderen van een cache bestand Vgx.

Als u wilt een bestand Vgx. cache verwijderen, gebruikt u de cmdlet [Verwijderen AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) .

Voor een overzicht van de beschikbare parameters en hun beschrijvingen voor `Remove-AzureRmRedisCache`, de volgende opdracht uitvoeren.

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed
    
    NAME
        Remove-AzureRmRedisCache
    
    SYNOPSIS
        Remove redis cache if exists.
    
    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>
    
    DESCRIPTION
        The Remove-AzureRmRedisCache cmdlet removes a redis cache if it exists.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to remove.
    
        -ResourceGroupName <String>
            Name of the resource group of the cache to remove.
    
        -Force
            When the Force parameter is provided, the cache is removed without any confirmation prompts.
    
        -PassThru
            By default Remove-AzureRmRedisCache removes the cache and does not return any value. If the PassThru par
            is provided then Remove-AzureRmRedisCache returns a boolean value indicating the success of the operatio
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

In het volgende voorbeeld wordt de cache met de naam `myCache` wordt verwijderd.

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup
    
    Confirm
    Are you sure you want to remove redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="to-import-a-redis-cache"></a>Importeren van een cache bestand Vgx.

U kunt gegevens importeren in een bestand Vgx. Azure-Cache exemplaar met behulp van de `Import-AzureRmRedisCache` cmdlet.

>[AZURE.IMPORTANT] Importeren/exporteren is alleen beschikbaar voor [premium-tier](cache-premium-tier-intro.md) -caches. Zie voor meer informatie over het importeren/exporteren, [importeren en exporteren van gegevens in het bestand Vgx. Azure-Cache](cache-how-to-import-export-data.md).

Voor een overzicht van de beschikbare parameters en hun beschrijvingen voor `Import-AzureRmRedisCache`, de volgende opdracht uitvoeren.

    PS C:\> Get-Help Import-AzureRmRedisCache -detailed
    
    NAME
        Import-AzureRmRedisCache
    
    SYNOPSIS
        Import data from blobs to Azure Redis Cache.
    
    
    SYNTAX
        Import-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Import-AzureRmRedisCache cmdlet imports data from the specified blobs into Azure Redis Cache.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -Files <String[]>
            SAS urls of blobs whose content should be imported into the cache.
    
        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.
    
        -Force
            When the Force parameter is provided, import will be performed without any confirmation prompts.
    
        -PassThru
            By default Import-AzureRmRedisCache imports data in cache and does not return any value. If the PassThru
            parameter is provided then Import-AzureRmRedisCache returns a boolean value indicating the success of the
            operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    

De volgende opdracht importeert gegevens uit de opgegeven door de uri SAS in Azure bestand Vgx. Cache blob.


    PS C:\>Import-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="to-export-a-redis-cache"></a>Voor het exporteren van een cache bestand Vgx.

U kunt gegevens exporteren uit een bestand Vgx. Azure-Cache exemplaar met behulp van de `Export-AzureRmRedisCache` cmdlet.

>[AZURE.IMPORTANT] Importeren/exporteren is alleen beschikbaar voor [premium-tier](cache-premium-tier-intro.md) -caches. Zie voor meer informatie over het importeren/exporteren, [importeren en exporteren van gegevens in het bestand Vgx. Azure-Cache](cache-how-to-import-export-data.md).

Voor een overzicht van de beschikbare parameters en hun beschrijvingen voor `Export-AzureRmRedisCache`, de volgende opdracht uitvoeren.

    PS C:\> Get-Help Export-AzureRmRedisCache -detailed
    
    NAME
        Export-AzureRmRedisCache
    
    SYNOPSIS
        Exports data from Azure Redis Cache to a specified container.
    
    
    SYNTAX
        Export-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Export-AzureRmRedisCache cmdlet exports data from Azure Redis Cache to a specified container.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -Prefix <String>
            Prefix to use for blob names.
    
        -Container <String>
            SAS url of container where data should be exported.
    
        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.
    
        -PassThru
            By default Export-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Export-AzureRmRedisCache returns a boolean value indicating the success of the operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


De volgende opdracht exporteert gegevens uit een instantie Azure bestand Vgx. Cache naar de container die is opgegeven door de uri van SAS.


        PS C:\>Export-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="to-reboot-a-redis-cache"></a>Opnieuw opstarten van een cache bestand Vgx.

U kunt opstarten uw Azure bestand Vgx. Cache exemplaar met behulp van de `Reset-AzureRmRedisCache` cmdlet.

>[AZURE.IMPORTANT] Opnieuw opstarten is alleen beschikbaar voor [premium-tier](cache-premium-tier-intro.md) -caches. Zie voor meer informatie over het opnieuw opstarten van de cache, [Cache-beheer - opnieuw opstarten](cache-administration.md#reboot).

Voor een overzicht van de beschikbare parameters en hun beschrijvingen voor `Reset-AzureRmRedisCache`, de volgende opdracht uitvoeren.

    PS C:\> Get-Help Reset-AzureRmRedisCache -detailed
    
    NAME
        Reset-AzureRmRedisCache
    
    SYNOPSIS
        Reboot specified node(s) of an Azure Redis Cache instance.
    
    
    SYNTAX
        Reset-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Reset-AzureRmRedisCache cmdlet reboots the specified node(s) of an Azure Redis Cache instance.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -RebootType <String>
            Which node to reboot. Possible values are "PrimaryNode", "SecondaryNode", "AllNodes".
    
        -ShardId <Integer>
            Which shard to reboot when rebooting a premium cache with clustering enabled.
    
        -Force
            When the Force parameter is provided, reset will be performed without any confirmation prompts.
    
        -PassThru
            By default Reset-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Reset-AzureRmRedisCache returns a boolean value indicating the success of the operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    

De volgende opdracht beide knooppunten van het opgegeven cache opnieuw is opgestart.

    
        PS C:\>Reset-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force
    

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over het gebruik van Windows PowerShell met Azure, Zie de volgende bronnen:

- [Azure Cache bestand Vgx. cmdlet documentatie op MSDN](https://msdn.microsoft.com/library/azure/mt634513.aspx)
- [Azure Resource Manager-Cmdlets](http://go.microsoft.com/fwlink/?LinkID=394765): informatie over de cmdlets gebruiken in de module AzureResourceManager.
- [Resourcegroepen gebruiken voor het beheren van uw resources Azure](../resource-group-template-deploy-portal.md): informatie over het maken en beheren van resourcegroepen in de portal Azure.
- [Azure blog](http://blogs.msdn.com/windowsazure): informatie over nieuwe functies in Azure.
- [Windows PowerShell-blog](http://blogs.msdn.com/powershell): informatie over nieuwe functies in Windows PowerShell.
- ["Hoi Scripting Guy!" Blog](http://blogs.technet.com/b/heyscriptingguy/): krijgt u praktische tips en trucs uit de Gemeenschap van Windows PowerShell.
