<properties
   pageTitle="Azure bestand Vgx. Cache gebruiken met Java | Microsoft Azure"
    description="Aan de slag met Azure bestand Vgx. Cache met behulp van Java"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor=""/>

<tags
    ms.service="cache"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/24/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-java"></a>Azure bestand Vgx. Cache gebruiken met Java

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure Cache bestand Vgx. hebt die u toegang tot een specifiek bestand Vgx. cache, beheerd door Microsoft. De cache is toegankelijk vanuit elke toepassing binnen Microsoft Azure.

In dit onderwerp wordt beschreven hoe u aan de slag met Azure bestand Vgx. Cache met behulp van Java.

## <a name="prerequisites"></a>Vereisten

[Jedis](https://github.com/xetorthio/jedis) - Java client voor bestand Vgx.

Deze zelfstudie Jedis gebruikt, maar u kunt een Java-client die aan [http://redis.io/clients](http://redis.io/clients)worden vermeld.

## <a name="create-a-redis-cache-on-azure"></a>Maak een bestand Vgx. cache op Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>De host naam en de toegang tot de sleutels ophalen

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-the-non-ssl-endpoint"></a>Het eindpunt van de niet-SSL inschakelen

Sommige clients bestand Vgx. geen SSL ondersteunen en standaard de [niet-SSL-poort uitgeschakeld voor nieuwe exemplaren van het bestand Vgx. Azure-Cache](cache-configure.md#access-ports). De client [Jedis](https://github.com/xetorthio/jedis) ondersteunt SSL niet op het moment van schrijven van dit. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]




## <a name="add-something-to-the-cache-and-retrieve-it"></a>Iets toevoegen aan de cache en deze ophalen

    package com.mycompany.app;
    import redis.clients.jedis.Jedis;
    import redis.clients.jedis.JedisShardInfo;

    /* Make sure you turn on non-SSL port in Azure Redis using the Configuration section in the Azure Portal */
    public class App
    {
      public static void main( String[] args )
      {
        /* In this line, replace <name> with your cache name: */
        JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6379);
        shardInfo.setPassword("<key>"); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);
        jedis.set("foo", "bar");
        String value = jedis.get("foo");
      }
    }


## <a name="next-steps"></a>Volgende stappen

- [Diagnostische gegevens van cache inschakelen](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics) zodat kunt u de [monitor](https://msdn.microsoft.com/library/azure/dn763945.aspx) van de gezondheid van de cache.
- Lees de officiële [bestand Vgx. documentatie](http://redis.io/documentation).

