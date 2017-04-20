<properties 
    pageTitle="Het maken en beheren van Azure bestand Vgx. Cache met de Azure opdrachtregelinterface (CLI Azure) | Microsoft Azure" 
    description="Informatie over de Azure CLI installeren op elk platform en het gebruik van deze verbinding maken met uw account Azure maken en beheren van een cache bestand Vgx. uit de CLI Azure." 
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
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="how-to-create-and-manage-azure-redis-cache-using-the-azure-command-line-interface-azure-cli"></a>Het maken en beheren van Azure bestand Vgx. Cache met de Azure opdrachtregelinterface (CLI Azure)

> [AZURE.SELECTOR]
- [PowerShell](cache-howto-manage-redis-cache-powershell.md)
- [Azure CLI](cache-manage-cli.md)

De CLI Azure is een uitstekende manier om uw Azure-infrastructuur beheren vanaf een willekeurig platform. In dit artikel wordt beschreven hoe u voor het maken en beheren van uw Azure bestand Vgx. Cache exemplaren met behulp van de CLI Azure.

## <a name="prerequisites"></a>Vereisten

Wilt maken en beheren van Azure bestand Vgx. Cache exemplaren met Azure CLI, moet u de volgende stappen uitvoeren.

-   U moet een Azure-account hebben. Als u er geen hebt, kunt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) maken in een paar seconden.
-   [De Azure CLI installeren](../xplat-cli-install.md).
-   Verbinding maken met uw Azure CLI-installatie met een persoonlijke account Azure, of met een werk of school Azure account en aanmelden via de Azure CLI de `azure login` opdracht. Als u wilt weten over de verschillen en kies, Zie [verbinding maken met een Azure-abonnement van de Azure-opdrachtregelinterface (CLI Azure)](../xplat-cli-connect.md).
-   Ga voordat u een van de volgende opdrachten, de CLI Azure Resource Manager modus door het uitvoeren van de `azure config mode arm` opdracht. Zie [de bronnenbeheerder Azure-modus instellen](../xplat-cli-azure-resource-manager.md#set-the-azure-resource-manager-mode)voor meer informatie.

## <a name="redis-cache-properties"></a>Eigenschappen in Cache bestand Vgx.

De volgende eigenschappen worden gebruikt bij het maken en bijwerken van Cache bestand Vgx. exemplaren.

| Eigenschap            | Switch                                  | Beschrijving                                                                                                                                                                                                                                          |
|---------------------|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| naam                | -n,--naam                              | De naam van de Cache voor het bestand Vgx.                                                                                                                                                                                                                             |
| resourcegroep      | -g,--resource-groep                    | De naam van de resourcegroep.                                                                                                                                                                                                                          |
| locatie            | -l,--locatie                          | Locatie voor het maken van de cache.                                                                                                                                                                                                                            |
| grootte                | -z,--grootte                              | De grootte van de Cache bestand Vgx.. Geldige waarden: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]                                                                                                                                                                  |
| SKU                 | -x,--sku                               | Bestand Vgx. SKU. Een van: [Basic, Standard en Premium]                                                                                                                                                                                             |
| EnableNonSslPort    | -e,--enable-niet-ssl-poort               | De eigenschap EnableNonSslPort van het bestand Vgx. Cache. Deze vlag toevoegen als u wilt dat de SSL-poort niet voor uw cache inschakelen                                                                                                                                    |
| Configuratie bestand Vgx. | -c,--configuratie bestand Vgx.               | Bestand Vgx. configuratie. Een reeks opgemaakte JSON configuratiesleutels en waarden hier invoeren. Indeling: ' {"": "","": ""} "                                                                                                                                     |
| Configuratie bestand Vgx. | -f,--bestand Vgx. configuratie bestand          | Bestand Vgx. configuratie. Typ het pad van een bestand met de van configuratiesleutels en waarden hier. Indeling voor de vermelding: {"": "","": ""}                                                                                                                |
| Shard Count         | -r,--shard count                       | Aantal Shards te maken op een Premium Cluster Cache met clustering.                                                                                                                                                                               |
| Virtueel netwerk     | -v,--virtueel netwerk                   | Wanneer de cache in een VNET die als host optreedt, geeft de exacte ARM resource-ID van het virtuele netwerk voor de implementatie van de cache bestand Vgx. in. Voorbeeld van de notatie: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| type sleutel            | -t,--type sleutel                          | Type sleutel te vernieuwen. Geldige waarden: [primaire, secundaire]                                                                                                                                                                                             |
| StaticIP            | -p,--statische ip-< statische ip->             | Wanneer de cache in een VNET die als host optreedt, geeft een uniek IP-adres in het subnet voor de cache. Als u niet opgeeft, wordt een voor u gekozen uit het subnet.                                                                                                |
| Subnet              | t,--subnet<subnet>                    | Wanneer de cache in een VNET die als host optreedt, geeft de naam van het subnet waarop de cache te implementeren.                                                                                                                                                    |
| VirtualNetwork      | -v,--virtueel netwerk < virtuele-netwerk > | Wanneer de cache in een VNET die als host optreedt, geeft de exacte ARM resource-ID van het virtuele netwerk voor de implementatie van de cache bestand Vgx. in. Voorbeeld van de notatie: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Abonnement        | -s, -abonnement                      | De abonnement-id.                                                                                                                                                                                                                         |

## <a name="see-all-redis-cache-commands"></a>Zie opdrachten voor alle Cache bestand Vgx.

Overzicht van alle Cache bestand Vgx. opdrachten en parameters, gebruikt u de `azure rediscache -h` opdracht.

    C:\>azure rediscache -h
    help:    Commands to manage your Azure Redis Cache(s)
    help:
    help:    Create a Redis Cache
    help:      rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Delete an existing Redis Cache
    help:      rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    List all Redis Caches within your Subscription or Resource Group
    help:      rediscache list [options]
    help:
    help:    Show properties of an existing Redis Cache
    help:      rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Change settings of an existing Redis Cache
    help:      rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Renew the authentication key for an existing Redis Cache
    help:      rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:      rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help  output usage information
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="create-a-redis-cache"></a>Maak een Cache bestand Vgx.

Als u wilt een bestand Vgx. Cache maakt, gebruik de volgende opdracht:

    azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]

Voor meer informatie over deze opdracht, voert u de `azure rediscache create -h` opdracht.

    C:\>azure rediscache create -h
    help:    Create a Redis Cache
    help:
    help:    Usage: rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -l, --location <location>                                Location to create cache.
    help:      -z, --size <size>                                        Size of the Redis Cache. Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]
    help:      -x, --sku <sku>                                          Redis SKU. Should be one of : [Basic, Standard, Premium]
    help:      -e, --enable-non-ssl-port                                EnableNonSslPort property of the Redis Cache. Add this flag if you want to enable the Non SSL Port for your cache
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here. Format:"{"<key1>":"<value1>","<key2>":"<value2>"}"
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here. Format for the file entry: {"<key1>":"<value1>","<key2>":"<value2>"}
    help:      -r, --shard-count <shard-count>                          Number of Shards to create on a Premium Cluster Cache
    help:      -v, --virtual-network <virtual-network>                  The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1
    help:      -t, --subnet <subnet>                                    Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -p, --static-ip <static-ip>                              Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -s, --subscription <id>                                  the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="delete-an-existing-redis-cache"></a>Een bestaand bestand Vgx. Cache verwijderen

Als u wilt een bestand Vgx. Cache verwijderen, gebruik de volgende opdracht:

    azure rediscache delete [--name <name> --resource-group <resource-group> ]

Voor meer informatie over deze opdracht, voert u de `azure rediscache delete -h` opdracht.

    C:\>azure rediscache delete -h
    help:    Delete an existing Redis Cache
    help:
    help:    Usage: rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which the cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-all-redis-caches-within-your-subscription-or-resource-group"></a>Lijst van alle Caches binnen uw abonnement of resourcegroep van bestand Vgx.

U kunt alle bestand Vgx. Caches binnen uw abonnement of resourcegroep gebruiken met de volgende opdracht:

    azure rediscache list [options]

Voor meer informatie over deze opdracht, voert u de `azure rediscache list -h` opdracht.

    C:\>azure rediscache list -h
    help:    List all Redis Caches within your Subscription or Resource Group
    help:
    help:    Usage: rediscache list [options]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="show-properties-of-an-existing-redis-cache"></a>Eigenschappen van een bestaand bestand Vgx. Cache weergeven

Als de eigenschappen van een bestaand bestand Vgx. Cache, gebruiken de volgende opdracht:

    azure rediscache show [--name <name> --resource-group <resource-group>]

Voor meer informatie over deze opdracht, voert u de `azure rediscache show -h` opdracht.

    C:\>azure rediscache show -h
    help:    Show properties of an existing Redis Cache
    help:
    help:    Usage: rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

<a name="scale"></a>
## <a name="change-settings-of-an-existing-redis-cache"></a>Instellingen van een bestaand bestand Vgx. Cache wijzigen

Gebruik de volgende opdracht als u wilt wijzigen van een bestaand bestand Vgx. Cache:

    azure rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]

Voor meer informatie over deze opdracht, voert u de `azure rediscache set -h` opdracht.

    C:\>azure rediscache set -h
    help:    Change settings of an existing Redis Cache
    help:
    help:    Usage: rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here.
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here.
    help:      -s, --subscription <subscription>                        the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="renew-the-authentication-key-for-an-existing-redis-cache"></a>De verificatiesleutel voor een bestaand bestand Vgx. Cache vernieuwen

Als de verificatiesleutel voor een bestaand bestand Vgx. Cache vernieuwen, gebruik de volgende opdracht:

    azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]

Geef `Primary` of `Secondary` voor `key-type`.

Voor meer informatie over deze opdracht, voert u de `azure rediscache renew-key -h` opdracht.

    C:\>azure rediscache renew-key -h
    help:    Renew the authentication key for an existing Redis Cache
    help:
    help:    Usage: rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which cache exists
    help:      -t, --key-type <key-type>              type of key to renew. Valid values are: 'Primary', 'Secondary'.
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-primary-and-secondary-keys-of-an-existing-redis-cache"></a>Lijst met primaire en secundaire sleutels van een bestaand bestand Vgx. Cache

Gebruik de volgende opdracht om de lijst met primaire en secundaire sleutels van een bestaand bestand Vgx. Cache:

    azure rediscache list-keys [--name <name> --resource-group <resource-group>]

Voor meer informatie over deze opdracht, voert u de `azure rediscache list-keys -h` opdracht.

    C:\>azure rediscache list-keys -h
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:
    help:    Usage: rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which Cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)
