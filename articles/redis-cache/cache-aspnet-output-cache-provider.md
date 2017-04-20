<properties
    pageTitle="Provider van cache ASP.NET uitvoer-Cache"
    description="Meer informatie over het ASP.NET-uitvoer met Azure bestand Vgx. Cache cache"
    services="redis-cache"
    documentationCenter="na"
    authors="steved0x"
    manager="douge"
    editor="tysonn" />
<tags
    ms.service="cache"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="09/27/2016"
    ms.author="sdanie" />

# <a name="aspnet-output-cache-provider-for-azure-redis-cache"></a>Uitvoer van de ASP.NET-Cache Provider voor Azure bestand Vgx. Cache

De Provider bestand Vgx. uitvoer-Cache is een out-of-process-opslagmechanisme voor uitvoergegevens cache. Deze gegevens zijn specifiek voor volledige HTTP-antwoorden (pagina uitvoercaching). De provider in het uitvoer-cache provider uitbreidbaarheid punt dat werd geïntroduceerd in ASP.NET 4 wordt geplaatst.

Eerst de cache configureren voor het gebruik van de Provider bestand Vgx. uitvoer-Cache en configureert u de ASP.NET-toepassing met behulp van het bestand Vgx. uitvoer-Cache Provider NuGet pakket. Dit onderwerp bevat richtlijnen voor het configureren van uw toepassing het bestand Vgx. uitvoer-Cache Provider te gebruiken. Zie voor meer informatie over het maken en configureren van een exemplaar van het bestand Vgx. Azure-Cache [maken een cache](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

## <a name="store-aspnet-page-output-in-the-cache"></a>De uitvoer van de ASP.NET-pagina in de cache opslaan

Een clienttoepassing configureren in Visual Studio met behulp van het bestand Vgx. uitvoer-Cache Provider NuGet pakket, het project in de **Solution Explorer** met de rechtermuisknop en kies **NuGet pakketten beheren**.

![Azure bestand Vgx. Cache NuGet pakketten beheren](./media/cache-aspnet-output-cache-provider/redis-cache-manage-nuget-menu.png)

**RedisOutputCacheProvider** in het zoekvak typt, selecteert u deze in de resultaten en klikt u op **installeren**.

![Azure bestand Vgx. Cache uitvoer-Cache Provider](./media/cache-aspnet-output-cache-provider/redis-cache-page-output-provider.png)

Het bestand Vgx. uitvoer-Cache Provider NuGet pakket heeft een afhankelijkheid van het pakket StackExchange.Redis.StrongName. Als het pakket StackExchange.Redis.StrongName niet aanwezig in uw project die wordt geïnstalleerd is. U ziet dat naast het pakket StackExchange.Redis.StrongName sterke naam ook de StackExchange.Redis niet-sterke naam versie is. Als uw project is de StackExchange.Redis niet-sterke naam versie dat u moet verwijderen, vóór of na de installatie van het pakket bestand Vgx. uitvoer-Cache Provider NuGet, zal anders u krijgen naamconflicten in uw project. Zie voor meer informatie over deze pakketten [configureren .NET cache-clients](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

Het pakket NuGet downloads en de vereiste assembly-verwijzingen worden toegevoegd en wordt toegevoegd in de volgende sectie in het bestand web.config waarin de vereiste configuratie voor uw ASP.NET-toepassing het bestand Vgx. uitvoer-Cache Provider te gebruiken.

    <caching>
      <outputCachedefault Provider="MyRedisOutputCache">
        <providers>
          <!--
          <add name="MyRedisOutputCache"
            host = "127.0.0.1" [String]
            port = "" [number]
            accessKey = "" [String]
            ssl = "false" [true|false]
            databaseId = "0" [number]
            applicationName = "" [String]
            connectionTimeoutInMilliseconds = "5000" [number]
            operationTimeoutInMilliseconds = "5000" [number]
          />
          -->
          <add name="MyRedisOutputCache" type="Microsoft.Web.Redis.RedisOutputCacheProvider" host="127.0.0.1" accessKey="" ssl="false"/>
        </providers>
      </outputCache>
    </caching>

De commentaar sectie vindt u een voorbeeld van de kenmerken en voorbeelden van instellingen voor elk kenmerk.

De kenmerken configureren met de waarden uit de cache blade in de portal voor Microsoft Azure en de andere waarden naar wens configureren. Zie voor meer informatie over de toegang tot de eigenschappen van uw cache [cache-instellingen configureren bestand Vgx.](cache-configure.md#configure-redis-cache-settings).

-   **host** : Geef uw cache-eindpunt.
-   **poort** – gebruik de niet-SSL-poort of de SSL-poort, afhankelijk van de ssl-instellingen.
-   **accessKey** : de primaire of secundaire sleutel gebruiken voor de cache.
-   **ssl** : true als u wilt beveiligen, cache/client communicaties met ssl; anders false. Zorg ervoor dat de juiste poort opgeven.
    -   De niet-SSL-poort is standaard uitgeschakeld voor nieuwe caches. True opgeeft voor deze instelling om de SSL-poort. Zie de sectie [Toegang-poorten](cache-configure.md#access-ports) in het onderwerp [een cache configureren](cache-configure.md) voor meer informatie over het inschakelen van de niet-SSL-poort.
-   **databaseId** – welke database moet worden gebruikt voor de uitvoer-cache-gegevens opgegeven. Als u niet opgeeft, wordt de standaardwaarde 0 gebruikt.
-   **applicationName** -sleutels worden opgeslagen in het bestand als Vgx. <AppName>_<SessionId>_Data. Hierdoor kunnen meerdere toepassingen voor het delen van dezelfde sleutel. Deze parameter is optioneel en als u niet beschikken over een standaardwaarde gebruikt.
-   **connectionTimeoutInMilliseconds** : met deze instelling kunt u de instelling connectTimeout in de StackExchange.Redis-client te negeren. Als u niet opgeeft, wordt de standaardinstelling van de connectTimeout van 5000 gebruikt. Zie voor meer informatie [StackExchange.Redis configuratiemodel](http://go.microsoft.com/fwlink/?LinkId=398705).
-   **operationTimeoutInMilliseconds** : met deze instelling kunt u de instelling syncTimeout in de StackExchange.Redis-client te negeren. Als u niet opgeeft, wordt de standaardinstelling van de syncTimeout van 1000 gebruikt. Zie voor meer informatie [StackExchange.Redis configuratiemodel](http://go.microsoft.com/fwlink/?LinkId=398705).

Een instructie OutputCache aan elke pagina waarvoor u de uitvoer in de cache wilt toevoegen.

    <%@ OutputCache Duration="60" VaryByParam="*" %>

In dit voorbeeld pagina in de cache blijven de gegevens in het cachegeheugen gedurende 60 seconden en een andere versie van de pagina wordt opgeslagen voor elke combinatie van parameters. Zie voor meer informatie over de instructie OutputCache [@OutputCache](http://go.microsoft.com/fwlink/?linkid=320837).

Nadat u deze stappen zijn uitgevoerd, wordt uw toepassing geconfigureerd voor het gebruik van de Provider bestand Vgx. uitvoer-Cache.

## <a name="next-steps"></a>Volgende stappen

De [Provider voor ASP.NET sessie staat voor Azure bestand Vgx. Cache](cache-aspnet-session-state-provider.md)uitchecken.
