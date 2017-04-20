<properties
    pageTitle="Azure bestand Vgx. Cache gebruiken met Python | Microsoft Azure"
    description="Aan de slag met Azure bestand Vgx. Cache met Python"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/16/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-python"></a>Azure bestand Vgx. Cache gebruiken met Python

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

In dit onderwerp wordt beschreven hoe u aan de slag met Azure bestand Vgx. Cache met Python.


## <a name="prerequisites"></a>Vereisten

[Bestand Vgx. py](https://github.com/andymccurdy/redis-py)installeren.


## <a name="create-a-redis-cache-on-azure"></a>Maak een bestand Vgx. cache op Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>De host naam en de toegang tot de sleutels ophalen

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-the-non-ssl-endpoint"></a>Het eindpunt van de niet-SSL inschakelen

Sommige clients bestand Vgx. geen SSL ondersteunen en standaard de [niet-SSL-poort uitgeschakeld voor nieuwe exemplaren van het bestand Vgx. Azure-Cache](cache-configure.md#access-ports). Op het moment van schrijven van dit ondersteunt het [bestand Vgx. py](https://github.com/andymccurdy/redis-py) -client geen SSL. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]


## <a name="add-something-to-the-cache-and-retrieve-it"></a>Iets toevoegen aan de cache en deze ophalen


    >>> import redis
    >>> r = redis.StrictRedis(host='<name>.redis.cache.windows.net',
          port=6380, db=0, password='<key>', ssl=True)
    >>> r.set('foo', 'bar')
    True
    >>> r.get('foo')
    b'bar'


Vervangen `<name>` met de Cachenaam en `key` met de toegangstoets.


<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png
