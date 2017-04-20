<properties 
    pageTitle="Status van de sessie met cache in Azure App Service Azure bestand Vgx." 
    description="Informatie over het gebruik van de Cache Azure Service voor de ondersteuning van ASP.NET session state caching." 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="Rick-Anderson" 
    manager="wpickett" 
    editor="none"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="get-started-article" 
    ms.date="06/27/2016" 
    ms.author="riande"/>


# <a name="session-state-with-azure-redis-cache-in-azure-app-service"></a>Status van de sessie met cache in Azure App Service Azure bestand Vgx.


In dit onderwerp wordt uitgelegd hoe u de Service wilt gebruiken Azure bestand Vgx. Cache voor sessiestatus.

Als uw ASP.NET-webtoepassing sessiestatus wordt gebruikt, moet u voor het configureren van een externe sessie staat provider (de Cache bestand Vgx. Service of een SQL Server-sessie staat provider). Als u session-status en geen gebruik van een externe provider, kunt u zich beperkt tot één exemplaar van uw web app. De Cache bestand Vgx. Service is de snelste en eenvoudigste om te schakelen.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

##<a id="createcache"></a>Maak de Cache
Volg [deze aanwijzingen](../cache-dotnet-how-to-use-azure-redis-cache.md#create-cache) om de cache te maken.

##<a id="configureproject"></a>Het pakket RedisSessionStateProvider NuGet toevoegen aan uw web app
Installeer de NuGet `RedisSessionStateProvider` pakket.  Gebruik de volgende opdracht installeren vanuit de package manager-console (**Extra** > **NuGet Package Manager** > **Package Manager-Console**):

  `PM> Install-Package Microsoft.Web.RedisSessionStateProvider`
  
Installeren van **Extra** > **NuGet Package Manager** > **NugGet-pakketten voor oplossing beheren**, zoeken naar `RedisSessionStateProvider`.

Zie de [pagina NuGet RedisSessionStateProvider](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/ ) en [de cache-client configureren](../cache-dotnet-how-to-use-azure-redis-cache.md#NuGet)voor meer informatie.

##<a id="configurewebconfig"></a>Het bestand Web.Config aanpassen
Assembly-verwijzingen maken voor Cache, wordt het pakket NuGet stub vermeldingen in het bestand *web.config* . 

1. Open *web.config* en zoek de het **sessionState** element.

1. Voer de waarden voor `host`, `accessKey`, `port` (de SSL-poort is 6380), en `SSL` op `true`. Deze waarden kunnen worden verkregen uit de blade [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) voor uw exemplaar van de cache. Zie [verbinding maken met de cache](../cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache)voor meer informatie. Houd er rekening mee dat de niet-SSL-poort is standaard uitgeschakeld voor nieuwe caches. Zie de sectie [Toegang-poorten](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) in het onderwerp [een cache in Azure bestand Vgx. Cache configureren](https://msdn.microsoft.com/library/azure/dn793612.aspx) voor meer informatie over het inschakelen van de niet-SSL-poort. De volgende code ziet u de wijzigingen in het bestand *web.config* , met name de wijzigingen in de *poort*, *host*, accessKey*, en *ssl *.

          <system.web>;
            <customErrors mode="Off" />;
            <authentication mode="None" />;
            <compilation debug="true" targetFramework="4.5" />;
            <httpRuntime targetFramework="4.5" />;
            <sessionState mode="Custom" customProvider="RedisSessionProvider">;
              <providers>;  
                  <!--<add name="RedisSessionProvider" 
                    host = "127.0.0.1" [String]
                    port = "" [number]
                    accessKey = "" [String]
                    ssl = "false" [true|false]
                    throwOnError = "true" [true|false]
                    retryTimeoutInMilliseconds = "0" [number]
                    databaseId = "0" [number]
                    applicationName = "" [String]
                  />;-->;
                 <add name="RedisSessionProvider" 
                      type="Microsoft.Web.Redis.RedisSessionStateProvider" 
                      port="6380"
                      host="movie2.redis.cache.windows.net" 
                      accessKey="m7PNV60CrvKpLqMUxosC3dSe6kx9nQ6jP5del8TmADk=" 
                      ssl="true" />;
              <!--<add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false" />;-->;
              </providers>;
            </sessionState>;
          </system.web>;


##<a id="usesessionobject"></a>Gebruik van het Session-Object in Code
De laatste stap is om te beginnen met het Session-object in uw ASP.NET-code. U voegt objecten aan sessiestatus met behulp van de methode **Session.Add** . Deze methode wordt een sleutel / waarde-paren om items te slaan in het cachegeheugen session state.

    string strValue = "yourvalue";
    Session.Add("yourkey", strValue);

De volgende code haalt deze waarde van de sessiestatus.

    object objValue = Session["yourkey"];
    if (objValue != null)
       strValue = (string)objValue; 

U kunt ook een bestand Vgx. Cache van de cacheobjecten gebruiken in uw web app. Zie voor meer info, [MVC film app met Azure bestand Vgx. Cache in 15 minuten](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/).
Zie voor meer informatie over het gebruik van de ASP.NET-sessiestatus [ASP.NET Session State overzicht][].

>[AZURE.NOTE] Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een starter tijdelijk web app in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.

## <a name="whats-changed"></a>Wat er veranderd
* Zie voor een handleiding voor het wijzigen van Websites met App-Service: [Azure App Service en de Impact op de bestaande Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

  *Door [Rick Anderson](https://twitter.com/RickAndMSFT)*
  
  [installed the latest]: http://www.windowsazure.com/downloads/?sdk=net  
  [Overzicht van ASP.NET-sessiestatus]: http://msdn.microsoft.com/library/ms178581.aspx

  [NewIcon]: ./media/web-sites-dotnet-session-state-caching/CacheScreenshot_NewButton.png
  [NewCacheDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CreateOptions.png
  [CacheIcon]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheIcon.png
  [NuGetDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_NuGet.png
  [OutputConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_OC_WebConfig.png
  [CacheConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheConfig.png
  [EndpointURL]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_EndpointURL.png
  [ManageKeys]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_ManageAccessKeys.png
 
