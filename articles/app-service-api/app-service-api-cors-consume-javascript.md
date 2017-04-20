<properties
    pageTitle="Ondersteuning van CORS in App Service | Microsoft Azure"
    description="Leren werken met ondersteuning van CORS in Azure Azure App-Service."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/27/2016"
    ms.author="rachelap"/>

# <a name="consume-an-api-app-from-javascript-using-cors"></a>Een API-app uit JavaScript aan met CORS verbruiken

App-Service biedt ingebouwde ondersteuning voor [Cross oorsprong Resource Sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), zodat clients JavaScript interdomein-aanroepen van API's die worden gehost in API apps te maken. App-Service kunt u configureren CORS toegang tot de API een-code te schrijven in de API.

Dit artikel bevat twee secties:

* De sectie [CORS configureren](#corsconfig) hoe in het algemeen CORS configureren voor API app, web app of mobiele app. Toepassing gelijkelijk op alle kaders die worden ondersteund door de App Service, met inbegrip van .NET, Node.js en Java. 

* Starten met de [voortgezette de zelfstudies .NET introductie](#tutorialstart) sectie, is het artikel een zelfstudie waarin wordt gedemonstreerd dat CORS ondersteunen door te bouwen op wat u hebt gedaan in de [handleiding de eerste API Apps aan de slag](app-service-api-dotnet-get-started.md). 

## <a id="corsconfig"></a>CORS in Azure App-Service configureren

U kunt CORS in Azure portal of met behulp van [Bronbeheer Azure](../azure-resource-manager/resource-group-overview.md) hulpmiddelen.

#### <a name="configure-cors-in-the-azure-portal"></a>CORS in Azure portal configureren

8. Ga naar de [Azure portal](https://portal.azure.com/)in een browser.

2. Op **App-Services**en klik vervolgens op de naam van uw app API.

    ![Selecteer API app in de portal](./media/app-service-api-cors-consume-javascript/browseapiapps.png)

10. In het blad dat wordt geopend aan de rechterkant van het blad **API app** **Instellingen** , de **API** -sectie vinden en klik vervolgens op **CORS**.

    ![Selecteer CORS in instellingen blade](./media/app-service-api-cors-consume-javascript/clicksettings.png)

11. In de tekst vak Voer de URL of URL's die u toestaan dat JavaScript aanroepen afkomstig zijn wilt uit.


    Als u uw JavaScript-toepassing naar een web app met de naam todolistangular geïmplementeerd, voert u bijvoorbeeld "https://todolistangular.azurewebsites.net". Als alternatief kunt u een sterretje (*) als u wilt opgeven dat alle domeinen van oorsprong worden geaccepteerd.


13. Klik op **Opslaan**.

    ![Klik op Opslaan](./media/app-service-api-cors-consume-javascript/corsinportal.png)

    Nadat u op **Opslaan**klikt, wordt de API-app JavaScript aanroepen vanuit de opgegeven URL's te accepteren.

#### <a name="configure-cors-by-using-azure-resource-manager-tools"></a>CORS configureren met behulp van bronbeheer Azure hulpmiddelen

U kunt ook CORS voor een API-app via [Azure Resource Manager-sjablonen](../resource-group-authoring-templates.md) in de opdrachtregelprogramma's zoals [Azure PowerShell](../powershell-install-configure.md) en de [CLI Azure](../xplat-cli-install.md). 

Open het [bestand in de opslagplaats voor de voorbeeldtoepassing van deze zelfstudie azuredeploy.json](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json)voor een voorbeeld van een sjabloon Azure Resource Manager die de eigenschap CORS. Zoek de sectie van de sjabloon die u in het volgende voorbeeld ziet:

        "cors": {
            "allowedOrigins": [
                "todolistangular.azurewebsites.net"
            ]
        }

## <a id="tutorialstart"></a>U doorgaat met de .NET introductie zelfstudie

Als u de Node.js of Java introductie reeks voor API apps, kunt u de reeks voor ophalen gestart hebt voltooid. Ga naar de sectie [volgende stappen uit](#next-steps) om te zoeken naar suggesties voor meer informatie over API Apps.

De rest van dit artikel is een voortzetting van de .NET introductie reeks en wordt ervan uitgegaan dat [de eerste zelfstudie](app-service-api-dotnet-get-started.md)is voltooid.

## <a name="deploy-the-todolistangular-project-to-a-new-web-app"></a>Het ToDoListAngular-project implementeren op een nieuwe web app.

U hebt gemaakt in [de eerste zelfstudie](app-service-api-dotnet-get-started.md)een middenlaag API-app en een gegevens reeks API-app. In deze zelfstudie maakt u een toepassing met één pagina (SPA) web app dat app de middenlaag API-aanroepen. Hebben voor de SPA werkt u CORS inschakelen op de middelste laag API app. 

Het project ToDoListAngular is in de [voorbeeldtoepassing takenlijst van](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list)een eenvoudige AngularJS-client die het project van de middelste laag ToDoListAPI Web-API aanroept. De JavaScript-code in het bestand *app/scripts/todoListSvc.js* roept de API met de AngularJS HTTP-provider. 

        angular.module('todoApp')
        .factory('todoListSvc', ['$http', function ($http) {

            $http.defaults.useXDomain = true;
            delete $http.defaults.headers.common['X-Requested-With']; 
        
            return {
                getItems : function(){
                    return $http.get(apiEndpoint + '/api/TodoList');
                },

                /* Get by ID, Put, and Delete methods not shown */

                postItem : function(item){
                    return $http.post(apiEndpoint + '/api/TodoList', item);
                }
            };
        }]);

### <a name="create-a-new-web-app-for-the-todolistangular-project"></a>Maak een nieuwe web app voor het project ToDoListAngular

De procedure een nieuwe App Service web app maken en implementeren van een project aan het is vergelijkbaar met wat u hebt gezien voor het [maken en implementeren van een API-app in de eerste zelfstudie in deze reeks](app-service-api-dotnet-get-started.md#createapiapp). Het enige verschil is dat het type app **Web App** in plaats van **API-App**.  Zie voor schermopnamen van dialoogvensters, 

1. In de **Solution Explorer**met de rechtermuisknop op het ToDoListAngular-project en klik vervolgens op **publiceren**.

3.  Op het tabblad **profiel** van de wizard **Publiceren** , klikt u op **Microsoft Azure App-Service**.

5. Klik op **Nieuw**in het dialoogvenster **App-Service** .

3. Voer in het tabblad **hosten** van het dialoogvenster **App-Service maken** en een **Web App naam** die uniek is in het domein *azurewebsites.net* . 

5. Kies de Azure **abonnement** u wilt werken.

6. Kies in de vervolgkeuzelijst **Groep** dezelfde resourcegroep als die u eerder hebt gemaakt.

4. Kies in de vervolgkeuzelijst **App serviceplan** hetzelfde plan dat u eerder hebt gemaakt. 

7. Klik op **maken**.

    Visual Studio de web app maakt, maakt een publicatieprofiel voor het en de **verbinding** stap van de wizard **Publiceren** wordt weergegeven.

    **Publiceren** Klik nog niet op. In de volgende sectie configureert u de nieuwe web app om te roepen de middenlaag API app die in App-Service wordt uitgevoerd. 

### <a name="set-the-middle-tier-url-in-web-app-settings"></a>De URL van de middelste laag ingesteld in web app instellingen

1. Ga naar de [Azure portal](https://portal.azure.com/)en navigeer vervolgens naar de blade **Web App** voor de web-app die als host voor het project TodoListAngular (front end).

2. Klik op **Instellingen > Toepassingsinstellingen**.

3. In de sectie **Toepassingsinstellingen** toevoegen de volgende sleutel en waarde:

  	|Sleutel|Waarde|Voorbeeld
  	|---|---|---|
  	|toDoListAPIURL|https://{Your van de middelste laag API app naam} .azurewebsites .net|https://todolistapi0121.azurewebsites.NET|

4. Klik op **Opslaan**.

    Wanneer de code wordt uitgevoerd in Azure, overschrijft deze waarde de localhost URL die in het bestand *Web.config* . 

    De code die met deze eigenschap de waarde van wordt is *index.cshtml*:

        <script type="text/javascript">
            var apiEndpoint = "@System.Configuration.ConfigurationManager.AppSettings["toDoListAPIURL"]";
        </script>
        <script src="app/scripts/todoListSvc.js"></script>

    De code in *todoListSvc.js* wordt de instelling:

        return {
            getItems : function(){
                return $http.get(apiEndpoint + '/api/TodoList');
            },
            getItem : function(id){
                return $http.get(apiEndpoint + '/api/TodoList/' + id);
            },
            postItem : function(item){
                return $http.post(apiEndpoint + '/api/TodoList', item);
            },
            putItem : function(item){
                return $http.put(apiEndpoint + '/api/TodoList/', item);
            },
            deleteItem : function(id){
                return $http({
                    method: 'DELETE',
                    url: apiEndpoint + '/api/TodoList/' + id
                });
            }
        };

### <a name="deploy-the-todolistangular-web-project-to-the-new-web-app"></a>Het webproject ToDoListAngular implementeren op de nieuwe web app.

*  Klik in Visual Studio in de **verbinding** stap van de wizard **Publiceren Web** **publiceren**.

    Visual Studio implementeert u het ToDoListAngular-project naar het nieuwe web app en opent u een browser naar de URL van de web app. 

### <a name="test-the-application-without-cors-enabled"></a>De toepassing testen zonder CORS ingeschakeld 

2. Open in uw browser ontwikkelhulpprogramma's van het consolevenster.

3. Klik op de koppeling van de **Takenlijst** in het browservenster waarin de UI AngularJS.

    De JavaScript-code aan te roepen, de middelste laag API app geprobeerd maar de aanroep mislukt omdat de front-endtoepassing wordt uitgevoerd in een ander domein dan de back-end. Developer Tools Console-venster van de browser ziet u een foutbericht van verschillende oorsprong.

    ![Foutbericht cross-oorsprong](./media/app-service-api-cors-consume-javascript/consoleaccessdenied.png)

## <a name="configure-cors-for-the-middle-tier-api-app"></a>CORS configureren voor de middelste laag API app

In deze sectie configureert u de instelling CORS in Azure voor de middelste laag ToDoListAPI API app. Deze instelling kan de middenlaag app API JavaScript aanroepen kan ontvangen van het web app die u hebt gemaakt voor het project ToDoListAngular.

8. In een browser, gaat u naar de [portal Azure](https://portal.azure.com/).

2. Op **App-Services**en klik vervolgens op de ToDoListAPI (middelste laag) API app.

    ![Selecteer API app in de portal](./media/app-service-api-cors-consume-javascript/browseapiapps.png)

10. In het blad dat wordt geopend aan de rechterkant van het blad **API app** **Instellingen** , de **API** -sectie vinden en klik vervolgens op **CORS**.

    ![Selecteer CORS in portal](./media/app-service-api-cors-consume-javascript/clicksettings.png)

12. Voer in het tekstvak de URL voor het web app van ToDoListAngular (front end). Bijvoorbeeld als u het ToDoListAngular-project naar een web app met de naam todolistangular0121 geïmplementeerd, kunnen oproepen via de URL `https://todolistangular0121.azurewebsites.net`.

    Als alternatief kunt u een sterretje (*) als u wilt opgeven dat alle domeinen van oorsprong worden geaccepteerd.

13. Klik op **Opslaan**.

    ![Klik op Opslaan](./media/app-service-api-cors-consume-javascript/corsinportal.png)

    Nadat u op **Opslaan**klikt, accepteert de app API JavaScript aanroepen vanuit de opgegeven URL. In deze schermafbeelding accepteert de app ToDoListAPI0223 API JavaScript client-aanroepen vanuit de ToDoListAngular web app.

### <a name="test-the-application-with-cors-enabled"></a>De toepassing testen met CORS ingeschakeld

* Open een browser het HTTPS-URL van de web app. 

    Nu u kunt weergeven, toevoegen, bewerken en verwijderen van taken. 

    ![Pagina van het monster app takenlijst](./media/app-service-api-cors-consume-javascript/corssuccess.png)

## <a name="app-service-cors-versus-web-api-cors"></a>Service App CORS versus Web API CORS

In project Web API, kunt u het pakket [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet in code opgeven welke domeinen de API accepteert-JavaScript aanroepen van installeren.
 
Ondersteuning voor web-API CORS is flexibeler dan App Service CORS ondersteuning. Bijvoorbeeld, in code kunt u verschillende geaccepteerde oorsprong voor andere actie methoden, terwijl voor CORS van App-Service u één set van geaccepteerde oorsprong voor alle methoden van een API-app geeft.

> [AZURE.NOTE] Probeer niet met zowel Web API CORS en App Service CORS in één API app. App Service CORS voorrang en Web API CORS heeft geen effect. Bijvoorbeeld, als u een domein van oorsprong in App-Service inschakelen en alle domeinen van de oorsprong in uw Web API-code inschakelen, aanvaardt uw app Azure API alleen oproepen van het domein dat u hebt opgegeven in Azure.

### <a name="how-to-enable-cors-in-web-api-code"></a>Het inschakelen van CORS in Web API-code

De volgende stappen samenvatten het proces voor het inschakelen van ondersteuning voor Web-API CORS. Zie voor meer informatie, [Waardoor Cross-oorsprong aanvragen in ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).

1. Installeer het pakket [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet in project Web API.

1. Inclusief een `config.EnableCors()` regel code in de methode **registreren** van de klasse **WebApiConfig** , zoals in het volgende voorbeeld. 

        public static class WebApiConfig
        {
            public static void Register(HttpConfiguration config)
            {
                // Web API configuration and services
                
                // The following line enables you to control CORS by using Web API code
                config.EnableCors();
    
                // Web API routes
                config.MapHttpAttributeRoutes();
    
                config.Routes.MapHttpRoute(
                    name: "DefaultApi",
                    routeTemplate: "api/{controller}/{id}",
                    defaults: new { id = RouteParameter.Optional }
                );
            }
        }

1. Toevoegen in uw Web API-controller, een `using` -instructie voor de `System.Web.Http.Cors` -naamruimte en voeg de `EnableCors` kenmerk aan de klasse controller of individuele actie methoden. In het volgende voorbeeld wordt CORS ondersteuning geldt voor de hele controller.

        namespace ToDoListAPI.Controllers 
        {
            [HttpOperationExceptionFilterAttribute]
            [EnableCors(origins:"https://todolistangular0121.azurewebsites.net", headers:"accept,content-type,origin,x-my-header", methods: "get,post")]
            public class ToDoListController : ApiController
 
## <a name="using-azure-api-management-with-api-apps"></a>Met behulp van beheer Azure API met API Apps

Als u met een API app Azure API Management, CORS in configureren Management API in plaats van in de API-app. Zie de volgende bronnen voor meer informatie:

* [Azure-API voor beheer-overzicht (video: CORS begint om 12:10)](https://azure.microsoft.com/documentation/videos/azure-api-management-overview/)
* [Beheer-API cross-domeinbeleid](https://msdn.microsoft.com/library/azure/dn894084.aspx#CORS)
 
## <a name="troubleshooting"></a>Het oplossen van problemen

In het geval u een probleem hebt uitgevoerd tijdens het doorlopen van deze zelfstudie, vindt hier u enkele ideeën voor het oplossen van problemen.

* Zorg ervoor dat u de meest recente versie van de [SDK voor .NET for Visual Studio 2015 Azure](http://go.microsoft.com/fwlink/?linkid=518003).

* Zorg ervoor dat u hebt ingevoerd `https` in de instelling CORS en zorg ervoor dat u `https` voor het uitvoeren van de front-end web app.

* Zorg ervoor dat u de instelling CORS ingevoerd in de middenlaag API app, niet in de front-end web app.

* Als u CORS in de toepassingscode en Azure App-Service configureren bent, houd er rekening mee dat de App Service CORS instelling overschrijft wat u doet in de toepassingscode. 

Zie voor meer informatie over de functies van Visual Studio die het oplossen van problemen vereenvoudigen, [Troubleshooting Azure App Service apps in Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## <a name="next-steps"></a>Volgende stappen 

In dit artikel hebt u gezien hoe Service CORS van App-ondersteuning inschakelen zodat de client JavaScript-code kan een API aanroepen in een ander domein. Lees de [Inleiding tot verificatie in App-Service](../app-service/app-service-authentication-overview.md)voor meer informatie over API apps, en gaat u naar de [Gebruikersverificatie voor API apps](app-service-api-dotnet-user-principal-auth.md) zelfstudie.
