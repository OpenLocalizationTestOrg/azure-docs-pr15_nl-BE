<properties
    pageTitle="Azure bestand Vgx. Cache gebruiken met Node.js | Microsoft Azure"
    description="Aan de slag met Azure bestand Vgx. Cache met Node.js en node_redis."
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="nodejs"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-nodejs"></a>Azure bestand Vgx. Cache gebruiken met Node.js

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure Cache bestand Vgx. hebt u toegang tot een beveiligde, speciale bestand Vgx. cache, beheerd door Microsoft. De cache is toegankelijk vanuit elke toepassing binnen Microsoft Azure.

In dit onderwerp wordt beschreven hoe u aan de slag met Azure bestand Vgx. Cache met Node.js. Zie [een toepassing Node.js Chat met Socket.IO op een Azure Website bouwen](../app-service-web/web-sites-nodejs-chat-app-socketio.md)voor een ander voorbeeld van het bestand Vgx. Azure-Cache met Node.js.


## <a name="prerequisites"></a>Vereisten

[Node_redis](https://github.com/mranney/node_redis)installeren:

    npm install redis

In deze zelfstudie wordt [node_redis](https://github.com/mranney/node_redis). Zie de afzonderlijke documentatie voor de Node.js clients weergegeven op [clients Node.js bestand Vgx.](http://redis.io/clients#nodejs)voor voorbeelden van het gebruik van andere clients Node.js.

## <a name="create-a-redis-cache-on-azure"></a>Maak een bestand Vgx. cache op Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>De host naam en de toegang tot de sleutels ophalen

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-to-the-cache-securely-using-ssl"></a>Verbinding maken met de veilig met behulp van SSL-cache

De meest recente versies van [node_redis](https://github.com/mranney/node_redis) bieden ondersteuning voor het verbinden met Azure bestand Vgx. Cache met behulp van SSL. In het volgende voorbeeld ziet u hoe verbinding maken met Azure bestand Vgx. Cache met behulp van het SSL-eindpunt van 6380. Vervangen `<name>` met de naam van de cache en `<key>` bij uw primaire of secundaire sleutel als beschreven in de vorige sectie van [de host naam en de toegang tot de sleutels worden opgehaald](#retrieve-the-host-name-and-access-keys) .

     var redis = require("redis");
    
      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});


## <a name="add-something-to-the-cache-and-retrieve-it"></a>Iets toevoegen aan de cache en deze ophalen

In het volgende voorbeeld ziet u hoe u kunt verbinding maken met een exemplaar Azure bestand Vgx. Cache en opslaan en ophalen van een artikel uit de cache. Zie voor meer voorbeelden van het bestand Vgx. met de client [node_redis](https://github.com/mranney/node_redis) [http://redis.js.org/](http://redis.js.org/).

     var redis = require("redis");
    
      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});
    
    client.set("key1", "value", function(err, reply) {
            console.log(reply);
        });
    
    client.get("key1",  function(err, reply) {
            console.log(reply);
        });

Uitvoer:

    OK
    value


## <a name="next-steps"></a>Volgende stappen

- [Diagnostische gegevens van cache inschakelen](cache-how-to-monitor.md#enable-cache-diagnostics) zodat kunt u de [monitor](cache-how-to-monitor.md) van de gezondheid van de cache.
- Lees de officiële [bestand Vgx. documentatie](http://redis.io/documentation).



