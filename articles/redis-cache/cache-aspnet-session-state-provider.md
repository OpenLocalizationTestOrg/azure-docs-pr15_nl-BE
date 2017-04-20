<properties
    pageTitle="ASP.NET-sessiestatus cache Provider | Microsoft Azure"
    description="Meer informatie over hoe u ASP.NET-sessiestatus met Azure bestand Vgx. Cache opslaat"
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
    ms.date="09/01/2016"
    ms.author="sdanie" />

# <a name="aspnet-session-state-provider-for-azure-redis-cache"></a>ASP.NET-sessiestatus Provider voor Azure bestand Vgx. Cache

Azure bestand Vgx. Cache levert een sessie staat provider kunt u de sessiestatus wordt opgeslagen in een cache in plaats van in het geheugen of in een SQL Server-database. Eerst de cache configureren voor het gebruik van de cache sessie staat provider en configureert u de ASP.NET-toepassing voor het bestand Vgx. Cache sessie staat NuGet pakket-cache.

Het praktisch vaak niet in een real-world cloud app om te voorkomen dat een vorm van staat voor een gebruikerssessie op te slaan, maar sommige manieren invloed hebben op prestaties en schaalbaarheid biedt meer dan andere. Als er voor het opslaan van de staat, is de beste oplossing het bedrag van de staat klein te houden en in cookies opslaan. Als dat niet haalbaar is, is de volgende beste oplossing voor de ASP.NET-sessiestatus gebruiken met een voorziening voor gedistribueerde, RAM-cache. De slechtste oplossing van prestaties en schaalbaarheid oogpunt is back session state-provider om een database te gebruiken. Dit onderwerp bevat richtlijnen over het gebruik van de ASP.NET-sessie staat Provider voor Azure bestand Vgx. Cache. Zie [ASP.NET-sessiestatus opties](#aspnet-session-state-options)voor informatie over andere opties van sessie staat.

## <a name="store-aspnet-session-state-in-the-cache"></a>ASP.NET-sessiestatus wordt opgeslagen in de cache

Een clienttoepassing configureren in Visual Studio met behulp van het bestand Vgx. Cache sessie staat NuGet pakket, het project in de **Solution Explorer** met de rechtermuisknop en kies **NuGet pakketten beheren**.

![Azure bestand Vgx. Cache NuGet pakketten beheren](./media/cache-aspnet-session-state-provider/redis-cache-manage-nuget-menu.png)

**RedisSessionStateProvider** in het zoekvak typt, selecteert u deze in de resultaten en klikt u op **installeren**.

>[AZURE.IMPORTANT] Als u de functie voor clustering van de premium-laag, moet u [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 of hoger of een uitzondering gegenereerd. Dit is een recente wijziging; Zie voor meer informatie [v2.0.0 wijziging Details te analyseren](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details).

![Sessiestatus Azure bestand Vgx. Cache Provider](./media/cache-aspnet-session-state-provider/redis-cache-session-state-provider.png)

Het bestand Vgx. Session State Provider NuGet pakket is afhankelijk van het pakket StackExchange.Redis.StrongName. Als het pakket StackExchange.Redis.StrongName niet aanwezig in uw project die wordt geïnstalleerd is. U ziet dat naast het pakket StackExchange.Redis.StrongName sterke naam ook de StackExchange.Redis niet-sterke naam versie is. Als uw project is de StackExchange.Redis niet-sterke naam versie dat u moet verwijderen, vóór of na de installatie van het pakket bestand Vgx. Session State Provider NuGet, zal anders u krijgen naamconflicten in uw project. Zie voor meer informatie over deze pakketten [configureren .NET cache-clients](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

Het pakket NuGet gedownload en worden de vereiste assembly verwijst naar en voegt dat de volgende worden toegevoegd in de volgende sectie in het bestand web.config waarin de vereiste configuratie voor uw ASP.NET-toepassing bestand Vgx. Cache Session State Provider te gebruiken.

    <sessionState mode="Custom" customProvider="MySessionStateStore">
        <providers>
        <!--
        <add name="MySessionStateStore"
            host = "127.0.0.1" [String]
            port = "" [number]
            accessKey = "" [String]
            ssl = "false" [true|false]
            throwOnError = "true" [true|false]
            retryTimeoutInMilliseconds = "0" [number]
            databaseId = "0" [number]
            applicationName = "" [String]
            connectionTimeoutInMilliseconds = "5000" [number]
            operationTimeoutInMilliseconds = "5000" [number]
        />
        -->
        <add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false"/>
        </providers>
    </sessionState>

De commentaar sectie vindt u een voorbeeld van de kenmerken en voorbeelden van instellingen voor elk kenmerk.

De kenmerken configureren met de waarden uit de cache blade in de Portal voor Microsoft Azure en de andere waarden naar wens configureren. Zie voor meer informatie over de toegang tot de eigenschappen van uw cache [cache-instellingen configureren bestand Vgx.](cache-configure.md#configure-redis-cache-settings).

-   **host** : Geef uw cache-eindpunt.
-   **poort** – gebruik de niet-SSL-poort of de SSL-poort, afhankelijk van de ssl-instellingen.
-   **accessKey** : de primaire of secundaire sleutel gebruiken voor de cache.
-   **ssl** : true als u wilt beveiligen, cache/client communicaties met ssl; anders false. Zorg ervoor dat de juiste poort opgeven.
    -   De niet-SSL-poort is standaard uitgeschakeld voor nieuwe caches. True opgeeft voor deze instelling om de SSL-poort. Zie de sectie [Toegang-poorten](cache-configure.md#access-ports) in het onderwerp [een cache configureren](cache-configure.md) voor meer informatie over het inschakelen van de niet-SSL-poort.
-   **throwOnError** : true als u wilt dat een uitzondering wordt gemaakt bij een storing of false als u wilt dat de bewerking mislukt zonder waarschuwing. Voor een storing kunt u controleren aan de hand van de statische eigenschap van de Microsoft.Web.Redis.RedisSessionStateProvider.LastException. De standaardwaarde is true.
-   **retryTimeoutInMilliseconds** – bewerkingen die niet zijn opnieuw geprobeerd tijdens dit interval wordt uitgedrukt in milliseconden. De eerste poging na 20 milliseconden optreedt en vervolgens pogingen tot het retryTimeoutInMilliseconds-interval is verstreken seconde optreden. Onmiddellijk na dit interval wordt de bewerking wordt opnieuw geprobeerd een laatste keer. Als het nog steeds mislukt, de uitzondering wordt gegenereerd terug naar de aanvrager, afhankelijk van de instelling van de throwOnError. De standaardwaarde is 0, wat betekent dat er geen nieuwe pogingen.
-   **databaseId** – geeft aan welke database u wilt gebruiken voor de cache gegevens uitvoeren. Als u niet opgeeft, wordt de standaardwaarde 0 gebruikt.
-   **applicationName** -sleutels worden opgeslagen in het bestand Vgx. Als `{<Application Name>_<Session ID>}_Data`. Hierdoor kunnen meerdere toepassingen voor het delen van dezelfde sleutel. Deze parameter is optioneel en als u niet beschikken over een standaardwaarde gebruikt.
-   **connectionTimeoutInMilliseconds** : met deze instelling kunt u de instelling connectTimeout in de StackExchange.Redis-client te negeren. Als u niet opgeeft, wordt de standaardinstelling van de connectTimeout van 5000 gebruikt. Zie voor meer informatie [StackExchange.Redis configuratiemodel](http://go.microsoft.com/fwlink/?LinkId=398705).
-   **operationTimeoutInMilliseconds** : met deze instelling kunt u de instelling syncTimeout in de StackExchange.Redis-client te negeren. Als u niet opgeeft, wordt de standaardinstelling van de syncTimeout van 1000 gebruikt. Zie voor meer informatie [StackExchange.Redis configuratiemodel](http://go.microsoft.com/fwlink/?LinkId=398705).

Zie de originele blog post aankondiging op [Aankondigen ASP.NET Session State-Provider voor bestand Vgx.](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)voor meer informatie over deze eigenschappen.

Vergeet niet om commentaar van de standaard InProc sessie staat provider gedeelte in uw web.config.

    <!-- <sessionState mode="InProc"
         customProvider="DefaultSessionProvider">
         <providers>
            <add name="DefaultSessionProvider"
                  type="System.Web.Providers.DefaultSessionStateProvider,
                        System.Web.Providers, Version=1.0.0.0, Culture=neutral,
                        PublicKeyToken=31bf3856ad364e35"
                  connectionStringName="DefaultConnection" />
          </providers>
    </sessionState> -->

Zodra deze stappen zijn uitgevoerd, wordt uw toepassing geconfigureerd voor het gebruik van het bestand Vgx. Cache sessie staat Provider. Wanneer u in uw toepassing de sessiestatus, wordt deze opgeslagen in een exemplaar van het bestand Vgx. Azure-Cache.

>[AZURE.NOTE] Houd er rekening mee dat gegevens die zijn opgeslagen in de cache moet worden geserialiseerd, in tegenstelling tot de gegevens die kunnen worden opgeslagen in de standaard ASP.NET-sessie in het geheugen staat Provider. Wanneer de Provider sessie staat voor het bestand Vgx. wordt gebruikt, zorg ervoor dat de gegevenstypen die worden opgeslagen in session state kunnen serialiseerbaar.

## <a name="aspnet-session-state-options"></a>Opties voor ASP.NET-sessiestatus

- Geheugen Session State-Provider - deze provider de sessiestatus in het geheugen opgeslagen. Het voordeel van deze provider is dat het eenvoudig en snel is. U kunt uw Web Apps echter kan niet schalen als u in het geheugen provider omdat deze niet wordt gedistribueerd.

- Provider voor SQL Server Session State - deze provider wordt de sessiestatus opgeslagen in Sql Server. Als u wilt dat de sessiestatus in een permanente opslag blijft bestaan, moet u deze provider gebruiken. U kunt de schaal van uw Web App maar hebben invloed op de prestaties met Sql Server voor de sessie op uw Web App.

- Verdeeld In geheugen sessie staat voorziening zoals bestand Vgx. Cache Session State-Provider - deze provider kunt u het beste van beide werelden. Uw Web App kan een eenvoudige, snelle en schaalbare Session State Provider hebben. U moet echter in overweging dat deze provider de sessiestatus wordt opgeslagen in een Cache en uw app te nemen alle kenmerken die horen bij het praten met een gedistribueerd In cachegeheugen zoals tijdelijke netwerkfouten rekening houden. Zie [richtlijnen voor Caching](../best-practices-caching.md) van Microsoft Patterns & Practices [Azure Cloud toepassingsontwerp en implementatie richtlijnen](https://github.com/mspnp/azure-guidance)voor aanbevolen procedures voor het gebruik van de Cache.

Zie voor meer informatie over de status van de sessie en andere aanbevelingen, [Web Development Best Practices (gebouw concrete wolk Apps met Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices).

## <a name="next-steps"></a>Volgende stappen

Bekijk de [ASP.NET uitvoer-Cache Provider voor Azure bestand Vgx. Cache](cache-aspnet-output-cache-provider.md).
