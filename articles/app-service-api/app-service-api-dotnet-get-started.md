<properties
    pageTitle="Aan de slag met API Apps en ASP.NET in App Service | Microsoft Azure"
    description="Informatie over het maken, distribueren en verbruiken een ASP.NET-API app in Azure App-Service met behulp van Visual Studio 2015."
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
    ms.topic="hero-article"
    ms.date="09/20/2016"
    ms.author="rachelap"/>

# <a name="get-started-with-api-apps-aspnet-and-swagger-in-azure-app-service"></a>Aan de slag met API Apps, ASP.NET en Swagger in Azure App-Service

[AZURE.INCLUDE [selector](../../includes/app-service-api-get-started-selector.md)]

Dit is de eerste in een reeks cursussen die laten hoe zien u mogelijkheden van Azure App Service die handig zijn voor het ontwikkelen en hosten van RESTful API's.  Deze zelfstudie omvat ondersteuning voor metagegevens in de indeling Swagger API.

U leert:

* Informatie over het maken en implementeren van [API apps](app-service-api-apps-why-best-platform.md) in Azure App-Service met behulp van hulpprogramma's in Visual Studio 2015 ingebouwd.
* Hoe discovery API automatiseren met behulp van het pakket Swashbuckle NuGet Swagger API metagegevens dynamisch genereren.
* Het gebruik van metagegevens Swagger API automatisch om clientcode te genereren voor een API-app.

## <a name="sample-application-overview"></a>Voorbeeld: overzicht

U werkt met een eenvoudige taak lijst-voorbeeldtoepassing in deze zelfstudie. De toepassing heeft een front-end-toepassing één pagina (SPA), een ASP.NET Web API van de middelste laag en een gegevenslaag van ASP.NET Web API.

![De toepassing voorbeelddiagram API Apps](./media/app-service-api-dotnet-get-started/noauthdiagram.png)

Hier is een schermafdruk van de front-end [AngularJS](https://angularjs.org/) .

![API Apps voorbeeldtoepassing takenlijst](./media/app-service-api-dotnet-get-started/todospa.png)

De Visual Studio-oplossing bevat drie projecten:

![](./media/app-service-api-dotnet-get-started/projectsinse.png)

* **ToDoListAngular** - de front-endtoepassing: een AngularJS SPA die de middelste laag roept.

* **ToDoListAPI** - de middenlaag: een project voor ASP.NET Web API die de gegevenslaag roept om CRUD bewerkingen uitvoeren op taakitems.

* **ToDoListDataAPI** - de gegevenslaag: een project voor ASP.NET Web API die bewerkingen worden uitgevoerd CRUD op taakitems.

De drielagige architectuur is een van de vele architecturen die u kunt implementeren met behulp van de API Apps en wordt hier alleen gebruikt voor demonstratiedoeleinden. De code in elke laag is zo eenvoudig mogelijk om aan te tonen van Apps API-functies. Zo gebruikt de gegevenslaag geheugen van de server in plaats van een database als de persistentie mechanisme.

Voor het voltooien van deze zelfstudie hebt u twee projecten van Web-API en uitgevoerd in de cloud in App Service API apps.

De volgende zelfstudie in de reeks implementeert de front-end SPA naar de cloud.

## <a name="prerequisites"></a>Vereisten

* ASP.NET Web API - de zelfstudie instructies wordt ervan uitgegaan dat u een basiskennis hebben van het werken met ASP.NET [Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) in Visual Studio.

* Azure account - kunt u [gratis een Azure account openen](/pricing/free-trial/?WT.mc_id=A261C142F) of [activeren met Visual Studio abonnee vergoedingen](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

    Als u aan de slag met Azure App-Service wilt voordat u zich voor een account met Azure aanmeldt, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751). Daar kunt u direct maken een tijdelijk starter app in App-Service — **geen creditcard vereist**en geen verplichtingen.

* Visual Studio 2015 met de [SDK voor .NET Azure](https://azure.microsoft.com/downloads/archive-net-downloads/) - SDK de Visual Studio 2015 automatisch geïnstalleerd als u nog geen hebt.

    * In Visual Studio, klikt u op Help -> over Microsoft Visual Studio en controleer of "Azure App Service extra v2.9.1" of hoger is geïnstalleerd.

    ![Azure extra App versie](./media/app-service-api-dotnet-get-started/apiversion.png)

    >[AZURE.NOTE] Afhankelijk van hoeveel van de SDK-afhankelijkheden er al op uw computer, kan installeert de SDK lang duren, van enkele minuten tot een half uur of meer.

## <a name="download-the-sample-application"></a>De voorbeeldtoepassing downloaden

1. Download de bibliotheek [Azure-Samples/app-service-api-dotnet-to-do-list](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) .

    Als u klikt u op de knop **Downloaden ZIP-** of het klonen van de bibliotheek op uw lokale computer.

2. Open de takenlijst van oplossing in Visual Studio 2015 of 2013.
   1. U moet elke oplossing vertrouwt.
        ![Beveiligingswaarschuwing](./media/app-service-api-dotnet-get-started/securitywarning.png)

3. Maak de oplossing (CTRL + SHIFT + B) de NuGet pakketten te herstellen.

    Als u wilt dat de toepassing in werking zien voordat u het implementeert, kunt u deze lokaal uitvoeren. Zorg ervoor dat ToDoListDataAPI het project starten en uitvoeren van de oplossing. U moet verwachten een HTTP 403-fout in uw browser.

## <a name="use-swagger-api-metadata-and-ui"></a>Swagger API metagegevens en UI gebruiken

Ondersteuning voor metagegevens van [Swagger](http://swagger.io/) 2.0 API is ingebouwd in Azure App-Service. Elke app API kan een eindpunt URL geeft als resultaat de metagegevens voor de API Swagger JSON-indeling opgeven. De metagegevens die worden geretourneerd uit dat eindpunt kan worden gebruikt voor het genereren van clientcode.

Een project voor ASP.NET Web API kunt Swagger metagegevens dynamisch genereren met behulp van het pakket [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet. Het pakket Swashbuckle NuGet is al geïnstalleerd in de ToDoListDataAPI en ToDoListAPI projecten die u hebt gedownload.

U kijkt naar de gegenereerde Swagger 2.0 metagegevens in dit gedeelte van de zelfstudie en probeer vervolgens een proef gebruikersinterface die is gebaseerd op de metagegevens van Swagger.

1. Het ToDoListDataAPI-project (**niet** het ToDoListAPI-project) instellen als het project is gestart.

    ![ToDoDataAPI instellen als opstartproject](./media/app-service-api-dotnet-get-started/startupproject.png)

2. Druk op F5 of klik op **voor foutopsporing > Foutopsporing starten** het project uitvoeren in de foutopsporingsmodus.

    De browser wordt geopend en de pagina HTTP 403-fout wordt weergegeven.

3. Voeg in de adresbalk van uw browser, `swagger/docs/v1` aan het einde van de regel en druk vervolgens op Return. (De URL is `http://localhost:45914/swagger/docs/v1`.)

    Dit is de standaard-URL die door Swashbuckle wordt gebruikt om terug te keren Swagger 2.0 JSON-metagegevens voor de API.

    Als u Internet Explorer gebruikt, ontvangt u een *v1.json* -bestand te downloaden.

    ![Metagegevens in IE JSON downloaden](./media/app-service-api-dotnet-get-started/iev1json.png)

    Als u werkt met Chrome, Firefox of rand, weergegeven de browser de JSON in het browservenster. Verschillende browsers JSON anders verwerken en uw browservenster kan niet precies hetzelfde uitzien als het voorbeeld.

    ![JSON-metagegevens in chroom](./media/app-service-api-dotnet-get-started/chromev1json.png)

    Het volgende voorbeeld wordt de eerste sectie van de Swagger metagegevens voor de API, met de definitie voor de methode Get. Deze metagegevens worden wat de Swagger gebruikersinterface die u in de volgende stappen gebruikt stations en u deze gebruiken in een gedeelte van de zelfstudie voor het automatisch genereren van clientcode.

        {
          "swagger": "2.0",
          "info": {
            "version": "v1",
            "title": "ToDoListDataAPI"
          },
          "host": "localhost:45914",
          "schemes": [ "http" ],
          "paths": {
            "/api/ToDoList": {
              "get": {
                "tags": [ "ToDoList" ],
                "operationId": "ToDoList_GetByOwner",
                "consumes": [ ],
                "produces": [ "application/json", "text/json", "application/xml", "text/xml" ],
                "parameters": [
                  {
                    "name": "owner",
                    "in": "query",
                    "required": true,
                    "type": "string"
                  }
                ],
                "responses": {
                  "200": {
                    "description": "OK",
                    "schema": {
                      "type": "array",
                      "items": { "$ref": "#/definitions/ToDoItem" }
                    }
                  }
                },
                "deprecated": false
              },

4. Sluit de browser en stop de foutopsporing van Visual Studio.

5. Open het bestand *App_Start\SwaggerConfig.cs* in het ToDoListDataAPI-project in de **Solution Explorer**, en vervolgens Ga naar regel 174 en opmerkingen bij de volgende code.

        /*
            })
        .EnableSwaggerUi(c =>
            {
        */

    Het bestand *SwaggerConfig.cs* wordt gemaakt wanneer u het Swashbuckle-pakket in een project installeert. Het bestand bevat een aantal manieren om Swashbuckle te configureren.

    De code die u hebt zonder opmerkingen kunt de gebruikersinterface van Swagger die u gebruikt in de volgende stappen uit. Wanneer u een project Web API maakt met behulp van de projectsjabloon API app, is deze code toegelicht die standaard als veiligheidsmaatregel.

6. Het project opnieuw uitvoeren.

7. Voeg in de adresbalk van uw browser, `swagger` aan het einde van de regel en druk vervolgens op Return. (De URL is `http://localhost:45914/swagger`.)

8. Wanneer de pagina Swagger gebruikersinterface wordt weergegeven, klikt u op de **takenlijst van** overzicht van de beschikbare methoden.

    ![De beschikbare methoden UI swagger](./media/app-service-api-dotnet-get-started/methods.png)

9. Klik op de eerste knop **ophalen** in de lijst.

10. Voer in de sectie **Parameters** een sterretje als de waarde van de `owner` parameter en klik vervolgens op **uit te proberen**.

    Als u verificatie in latere cursussen toevoegt, krijgt de middenlaag de werkelijke gebruikers-ID om de gegevenslaag. Nu hebben alle taken sterretje als hun eigenaar-ID, terwijl de toepassing wordt uitgevoerd zonder verificatie is ingeschakeld.

    ![Swagger UI zelf proberen](./media/app-service-api-dotnet-get-started/gettryitout1.png)

    De gebruikersinterface Swagger roept de takenlijst van Get methode en geeft de antwoordcode en JSON.

    ![Swagger UI uitproberen resultaten](./media/app-service-api-dotnet-get-started/gettryitout.png)

11. Klik op **Post**en klik vervolgens op het vak onder **Schema Model**.

    Het modelschema te klikken op prefills het invoervak kunt u de waarde van de parameter voor de methode Post. (Als dit niet in Internet Explorer werkt, gebruik een andere browser of de waarde van de parameter handmatig invoeren in de volgende stap.)  

    ![Swagger UI uitproberen boeken](./media/app-service-api-dotnet-get-started/post.png)

12. Wijzig de JSON in de `todo` parameter invoer vervangen door uw eigen tekst of het vak zodat het er als in het volgende voorbeeld uitziet:

        {
          "ID": 2,
          "Description": "buy the dog a toy",
          "Owner": "*"
        }

13. Klik op **Probeer het uit**.

    De takenlijst van API geeft als resultaat een HTTP 204 reactiecode die succes aangeeft.

14. Klikt u op de eerste **ophalen** en klik vervolgens op de knop **uit te proberen** in die sectie van de pagina.

    De methode Get response bevat nu het nieuwe item.

15. Optioneel: Probeer ook de plaatsen, verwijderen en opvragen door ID methoden.

16. Sluit de browser en stop de foutopsporing van Visual Studio.

Swashbuckle werkt met een project voor ASP.NET Web API. Als u genereren van Swagger metagegevens toevoegen aan een bestaand project, installeert u het pakket Swashbuckle.

>[AZURE.NOTE] Swagger-metagegevens bevat een unieke ID voor elke bewerking API. Swashbuckle kunnen standaard dubbele Swagger bewerking id's voor uw Web API controller methoden worden gegenereerd. Dit gebeurt als de controller is overbelast HTTP-methoden, zoals `Get()` en `Get(id)`. Zie voor meer informatie over het afhandelen van overbelastingen [definities API aanpassen Swashbuckle gegenereerd](app-service-api-dotnet-swashbuckle-customize.md). Als u een Web API-project in Visual Studio met behulp van de sjabloon Azure API App maakt, wordt code die unieke bewerking van id's genereert automatisch toegevoegd aan het bestand *SwaggerConfig.cs* .  

## <a id="createapiapp"></a>Een API-app maken in Azure en code te implementeren

In deze sectie kunt u de Azure's die zijn geïntegreerd in de Visual Studio **Web publiceren** wizard voor het maken van een nieuwe API-app in Azure. Vervolgens het ToDoListDataAPI-project naar de toepassing van de nieuwe API implementeren en de API aanroepen door de gebruikersinterface Swagger uit te voeren.

1. In de **Solution Explorer**met de rechtermuisknop op het ToDoListDataAPI-project en klik vervolgens op **publiceren**.

    ![Klik op publiceren in Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu.png)

2.  In het **profiel** stap van de wizard **Publiceren** , klikt u op **Microsoft Azure App-Service**.

    ![Klik op Azure Service App in Web publiceren](./media/app-service-api-dotnet-get-started/selectappservice.png)

3. Aanmelden bij uw account Azure als u dat nog niet hebt gedaan of vernieuwen van uw referenties als ze zijn verlopen.

4. In het dialoogvenster App Service kiezen de Azure **abonnement** die u wilt gebruiken en klik vervolgens op **Nieuw**.

    ![Klik op Nieuw in het dialoogvenster App Service](./media/app-service-api-dotnet-get-started/clicknew.png)

    Het tabblad **hosten** van het dialoogvenster **App Service maken** wordt weergegeven.

    Omdat u een project Web API met Swashbuckle geïnstalleerd distribueren wilt, Visual Studio wordt ervan uitgegaan dat u wilt maken van een App-API. Dit wordt aangegeven door de titel **API App naam** en het feit dat de vervolgkeuzelijst **Type wijzigen** is ingesteld op **API App**.

    ![Type in het dialoogvenster App Service App](./media/app-service-api-dotnet-get-started/apptype.png)

5. Voer een **API App naam** die uniek is in het domein *azurewebsites.net* . U kunt de standaardnaam die Visual Studio voorstelt accepteren.

    Als u een naam die iemand anders al heeft gebruikt opgeeft, ziet u een rood uitroepteken rechts.

    De URL van de API-app zijn `{API app name}.azurewebsites.net`.

6. In de vervolgkeuzelijst **Resourcegroep** klikt u op **Nieuw**en voert u "ToDoListGroup" of een andere naam als u de voorkeur geeft.

    Een resourcegroep is een verzameling van Azure bronnen zoals API apps, databases, VMs, enzovoort. Voor deze zelfstudie is het beste een nieuwe resourcegroep maken omdat die gemakkelijk te verwijderen in één stap de Azure bronnen die u voor de zelfstudie maakt.

    In dit vak kunt u een bestaande [resourcegroep](../azure-resource-manager/resource-group-overview.md) selecteren of een nieuwe maken door te typen in een naam die afwijkt van een bestaande resourcegroep in uw abonnement.

7. Klik op de knop **Nieuw** naast de **App Service Plan** vervolgkeuzelijst.

    De schermopname bevat voorbeeldwaarden voor **API App**, **abonnement**en **Resourcegroep** --uw waarden zijn verschillend.

    ![Dialoogvenster App Service maken](./media/app-service-api-dotnet-get-started/createas.png)

    In de volgende stappen maakt u een App Service plan voor de nieuwe resourcegroep. Een serviceplan App geeft de compute-bronnen die compatibel is met uw app API. Bijvoorbeeld als u de vrij laag, de API-app wordt uitgevoerd op gedeelde VMs, terwijl voor sommige betaalde lagen deze wordt uitgevoerd op specifieke VMs. Voor meer informatie over App serviceplannen overzicht [App Service plannen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

8. Voer in het dialoogvenster **Configureren App Service Plan** "ToDoListPlan" of een andere naam als u liever.

9. Kies de locatie die zich het dichtst bij u in de vervolgkeuzelijst **locatie** .

    Deze instelling bepaalt welke Azure datacenter in uw toepassing wordt uitgevoerd. Kies een locatie dicht bij u [Latentie](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090)minimaliseren.

10. Klik in de vervolgkeuzelijst **grootte** **vrij**.

    Voor deze zelfstudie biedt de prijzen vrij laag voldoende prestaties.

11. Klik op **OK**in het dialoogvenster **Configureren App Service Plan** .

    ![Klik op OK in configureren App serviceplan](./media/app-service-api-dotnet-get-started/configasp.png)

12. Klik op **maken**in het dialoogvenster **App-Service maken** .

    ![Klik op maken in het dialoogvenster App-Service maken](./media/app-service-api-dotnet-get-started/clickcreate.png)

    Visual Studio maakt de API-app en een publicatieprofiel met alle vereiste instellingen voor de API-app. Vervolgens wordt de wizard **Web publiceren** , waarmee u de implementatie van het project.

    Hiermee opent u de wizard **Publiceren** op het tabblad **verbinding** (Zie hieronder).

    Op het tabblad **verbinding** wijst de instellingen van de **Server** en de **naam van** uw app API. De **gebruikersnaam** en het **wachtwoord** zijn implementatie-referenties die Azure voor u maakt. Na de implementatie opent Visual Studio u een browser naar de **Doel-URL** (dat is het enige doel voor de **Doel-URL**).  

13. Klik op **volgende**.

    ![Klik op volgende in het tabblad verbinding van het Web publiceren](./media/app-service-api-dotnet-get-started/connnext.png)

    Het volgende tabblad wordt het tabblad **Instellingen** (Zie hieronder). Hier kunt u het tabblad build configuratie voor de implementatie van een foutopsporingsversie voor [Foutopsporing op afstand](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug). Het tabblad biedt ook verschillende **Opties voor publiceren**:

    * Verwijderen van extra bestanden op bestemming
    * Voorcompileren tijdens publicatie
    * Sluit bestanden uit de map App_Data

    Voor deze zelfstudie hoeft u niet een van deze. Zie voor een uitgebreide beschrijving van wat ze doen [hoe: implementatie van een Web Project met één klik publiceren in Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx).

14. Klik op **volgende**.

    ![Klik op volgende op het tabblad instellingen van het Web publiceren](./media/app-service-api-dotnet-get-started/settingsnext.png)

    Vervolgens wordt het tabblad **voorbeeld** (Zie hieronder), die u te zien welke bestanden gaat het project worden gekopieerd naar de API app leidt. Wanneer u een project naar een API-toepassing die u al geïmplementeerd op eerder introduceert, worden alleen de gewijzigde bestanden worden gekopieerd. Als u een overzicht van wat wordt gekopieerd, kunt u de knop **Start** .

15. Klik op **publiceren**.

    ![Klik op publiceren in het tabblad Voorbeeld van Web publiceren](./media/app-service-api-dotnet-get-started/clickpublish.png)

    Visual Studio implementeert het ToDoListDataAPI-project naar de nieuwe API-app. Het venster **uitvoer** meldt een geslaagde implementatie en een pagina 'gemaakt' wordt weergegeven in een browservenster geopend aan de URL van de API-app.

    ![Succesvolle implementatie van output venster](./media/app-service-api-dotnet-get-started/deploymentoutput.png)

    ![Nieuwe API app gemaakt pagina](./media/app-service-api-dotnet-get-started/appcreated.png)

16. 'Swagger' toevoegen aan de URL in de adresbalk van de browser en druk op Enter. (De URL is `http://{apiappname}.azurewebsites.net/swagger`.)

    De browser geeft dezelfde Swagger gebruikersinterface die u eerder hebt gezien, maar nu wordt uitgevoerd in de cloud. De Get-methode uitproberen en ziet u dat u teruggekeerd naar de standaard 2 taakitems bent. De wijzigingen die u eerder hebt aangebracht zijn in het geheugen op de lokale computer opgeslagen.

17. Open de [Azure portal](https://portal.azure.com/).

    De Azure portal is een web-interface voor het beheren van Azure bronnen zoals API apps.

18. Klik op **meer Services > Services App**.

    ![App Services Bladeren](./media/app-service-api-dotnet-get-started/browseas.png)

19. De blade **App Services** zoeken en klik op de nieuwe API-app. (In de portal Azure, zijn vensters die worden geopend aan de rechterkant *blades*genoemd.)

    ![App Services blade](./media/app-service-api-dotnet-get-started/choosenewapiappinportal.png)

    Twee bladen openen. Eén blade is een overzicht van de API-app en een heeft een lange lijst met instellingen die u kunt weergeven en wijzigen.

20. De blade **Instellingen** de **API** -sectie vinden en klik op de **Definitie van API**.

    ![Definitie van API in instellingen blade](./media/app-service-api-dotnet-get-started/apidefinsettings.png)

    De **Definitie van API** -bladeserver kunt u de URL opgeven die resulteert in Swagger 2.0 metagegevens JSON-indeling. Als u Visual Studio maakt de API-app, wordt de URL van de definitie van API op de standaardwaarde voor Swashbuckle gegenereerd metagegevens die u eerder hebt gezien dat de API-app is de basis URL plus `/swagger/docs/v1`.

    ![URL van API-definitie](./media/app-service-api-dotnet-get-started/apidefurl.png)

    Als u een API-app om clientcode te genereren voor deze selecteert, haalt Visual Studio de metagegevens van deze URL.

## <a id="codegen"></a>Clientcode genereren voor de gegevenslaag

Een van de voordelen van integratie van Swagger in Azure API apps is automatische code genereren. Client gegenereerde klassen gemakkelijker code schrijven die een app API wordt aangeroepen.

Het ToDoListAPI-project is al de gegenereerde clientcode maar in de volgende stappen u deze verwijderen en opnieuw genereren om te zien hoe u het genereren van code.

1. Verwijder de map *ToDoListDataAPI* in Visual Studio **Solution Explorer**, in het ToDoListAPI-project. **Let op: Alleen de map, niet in het ToDoListDataAPI-project verwijderen.**

    ![Gegenereerde clientcode verwijderen](./media/app-service-api-dotnet-get-started/deletecodegen.png)

    Deze map is gemaakt met het proces voor het genereren van code die u wilt doorlopen.

2. Klik met de rechtermuisknop op het ToDoListAPI-project en klik vervolgens op **toevoegen > REST API-Client**.

    ![REST API-client toevoegen in Visual Studio](./media/app-service-api-dotnet-get-started/codegenmenu.png)

3. Klik in het dialoogvenster **REST API-Client toevoegen** klikt u op **URL Swagger**en klik op **Azure activum selecteren**.

    ![Azure activum selecteren](./media/app-service-api-dotnet-get-started/codegenbrowse.png)

4. Vouw de resourcegroep die u voor deze zelfstudie en selecteer uw API app in **App-Service** in het dialoogvenster en klik op **OK**.

    ![App API voor codegeneratie selecteren](./media/app-service-api-dotnet-get-started/codegenselect.png)

    Merk op dat wanneer u naar het dialoogvenster **REST API-Client toevoegen terugkeert** , in het tekstvak is ingevuld API de definitie van de URL-waarde die eerder in de portal.

    ![URL van API-definitie](./media/app-service-api-dotnet-get-started/codegenurlplugged.png)

    >[AZURE.TIP] Voer de URL rechtstreeks in plaats van via het bladerdialoogvenster is een alternatieve manier om metagegevens voor het genereren van code. Of als u wilt dat om clientcode te genereren voordat Azure, kan u het Web API project lokaal worden uitgevoerd, gaat u naar de URL waarmee de Swagger JSON-bestand, slaat u het bestand en gebruikt u de optie **selecteert u een bestaand bestand met metagegevens Swagger** .

5. Klik op **OK**in het dialoogvenster **REST API-Client toevoegen** .

    Visual Studio maakt een map met de naam van de API-app en client klassen gegenereerd.

    ![Codebestanden voor client gegenereerde](./media/app-service-api-dotnet-get-started/codegenfiles.png)

6. Open in het project ToDoListAPI *Controllers\ToDoListController.cs* als de code in regel 40 die de API aanroept met behulp van de gegenereerde client wilt weergeven.

    Het volgende fragment wordt getoond hoe de code wordt het clientobject en roept de methode Get.

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));
            return client;
        }

        public async Task<IEnumerable<ToDoItem>> Get()
        {
            using (var client = NewDataAPIClient())
            {
                var results = await client.ToDoList.GetByOwnerAsync(owner);
                return results.Select(m => new ToDoItem
                {
                    Description = m.Description,
                    ID = (int)m.ID,
                    Owner = m.Owner
                });
            }
        }

    De parameter van de constructor haalt de eindpunt-URL van de `toDoListDataAPIURL` app instelling. Die waarde is ingesteld op lokale IIS Express URL van de API-project in het bestand Web.config, zodat de toepassing lokaal kunnen worden uitgevoerd. Als u de constructor-parameter weglaat, wordt het standaardeindpunt de URL die u hebt gegenereerd met de code uit.

7. De clientklasse wordt gegenereerd met een andere naam op basis van de naam van de API-app; Wijzig de code in *Controllers\ToDoListController.cs* zodat de naam overeenkomt met wat er in uw project is gegenereerd. Als u de naam van uw App API ToDoListDataAPI071316, zou u bijvoorbeeld deze code wijzigen:

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));

in deze:

        private static ToDoListDataAPI071316 NewDataAPIClient()
        {
            var client = new ToDoListDataAPI071316(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));


## <a name="create-an-api-app-to-host-the-middle-tier"></a>Een API-app als host voor de middelste laag maken

Eerder u [de gegevens laag API app gemaakt en deze code geïmplementeerd](#createapiapp).  Volg nu de procedure voor de middelste laag API app.

1. In de **Solution Explorer**met de rechtermuisknop op de middelste laag ToDoListAPI (niet de gegevenslaag ToDoListDataAPI) project en klik vervolgens op **publiceren**.

    ![Klik op publiceren in Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu2.png)

2.  Op het tabblad **profiel** van de wizard **Publiceren** , klikt u op **Microsoft Azure App-Service**.

3. Klik op **Nieuw**in het dialoogvenster **App-Service** .

4. Accepteer de standaardinstelling **API App naam** op het tabblad **hosten** van het dialoogvenster **App Service maken** , of typ een naam die uniek is in het domein *azurewebsites.net* .

5. Kies de Azure **abonnement** die u hebt gebruikt.

6. Kies in de vervolgkeuzelijst **Groep** dezelfde resourcegroep als die u eerder hebt gemaakt.

7. Kies in de vervolgkeuzelijst **App serviceplan** hetzelfde plan dat u eerder hebt gemaakt. Deze waarde wordt standaard.

8. Klik op **maken**.

    Visual Studio maakt de API-app, maakt een publicatieprofiel voor het en de **verbinding** stap van de wizard **Publiceren** wordt weergegeven.

9.  In de **verbinding** stap van de wizard **Publiceren** , klikt u op **publiceren**.

    Visual Studio implementeert u het ToDoListAPI-project naar de nieuwe API-app en opent u een browser naar de URL van de API-app. "Met succes gemaakt" wordt de pagina weergegeven.

## <a name="configure-the-middle-tier-to-call-the-data-tier"></a>De middelste laag om aan te roepen de gegevenslaag configureren

Als u nu de middenlaag API app genoemd, zou het probeert aan te roepen van de gegevenslaag via de localhost URL die nog in het bestand Web.config. In deze sectie kunt u laag API app de URL van de invoeren in een milieu-instelling in de middenlaag API app. Wanneer de code in de middenlaag API app haalt de instelling van de data tier URL, overschrijft de omgevingsinstelling van de wat is in het bestand Web.config.

1. Ga naar de [portal Azure](https://portal.azure.com/)en navigeer vervolgens naar de blade **API App** voor de app API die u hebt gemaakt als host voor het project TodoListAPI (middelste laag).

2. In de API-App **Instellingen** blade, klikt u op **instellingen van de toepassing**.

3. Ga naar de sectie **Toepassingsinstellingen** in de API-App **Toepassingsinstellingen** blade, en voeg de volgende sleutel en waarde. De waarde is de URL van de eerste API App u in deze zelfstudie hebt gepubliceerd.

  	| **Sleutel** | toDoListDataAPIURL |
  	|---|---|
  	| **Waarde** | https://{your data tier API app naam} .azurewebsites .net |
  	| **Voorbeeld** | https://todolistdataapi.azurewebsites.NET |

4. Klik op **Opslaan**.

    ![Klik op Opslaan voor App instellingen](./media/app-service-api-dotnet-get-started/asinportal.png)

    Wanneer de code wordt uitgevoerd in Azure, overschrijft deze waarde de localhost URL die in het bestand Web.config is nu.

## <a name="test"></a>Test

1. Ga naar de URL van de nieuwe midden-tier API app die u zojuist hebt gemaakt voor ToDoListAPI in een browservenster. U kunt daar door te klikken op de URL in de belangrijkste blade de API-app in de portal.

2. 'Swagger' toevoegen aan de URL in de adresbalk van de browser en druk op Enter. (De URL is `http://{apiappname}.azurewebsites.net/swagger`.)

    Dezelfde Swagger gebruikersinterface die u eerder hebt gezien voor ToDoListDataAPI, maar nu wordt weergegeven in de browser `owner` , is niet vereist voor de bewerking ophalen uitgevoerd omdat de middenlaag API app die waarde gegevens laag API App voor u verzenden. (Wanneer u de verificatie zelfstudies, de middelste laag werkelijke gebruikers-id's voor verzendt het `owner` parameter; voor nu het is harde codering een sterretje.)

3. Probeer de Get-methode en de andere methoden voor het valideren van de middelste laag API app is de app data tier-API aanroept.

    ![Methode Get UI swagger](./media/app-service-api-dotnet-get-started/midtierget.png)

## <a name="troubleshooting"></a>Het oplossen van problemen

In het geval u een probleem uitvoert terwijl u deze zelfstudie hier zijn enkele ideeën voor het oplossen van problemen:

* Zorg ervoor dat u de meest recente versie van de [SDK voor .NET Azure](http://go.microsoft.com/fwlink/?linkid=518003).

* Twee van de projectnamen zijn vergelijkbaar (ToDoListAPI, ToDoListDataAPI). Als u niet tevreden bent zoals beschreven in de instructies als u met een project werkt, moet dat u het juiste project hebt geopend.

* Als u gebruikmaakt van een bedrijfsnetwerk wilt implementeren met Azure App-Service via een firewall, moet poort 443 en 8172 openstaan voor webonderdelen implementeren. Als u deze poorten niet kunt openen, kunt u andere implementatiemethoden.  Zie [uw app Azure App service implementeren](../app-service-web/web-sites-deploy.md).

* Fouten "route namen moeten uniek zijn"--kan u deze als u per ongeluk het verkeerde project naar een API-toepassing implementeren en vervolgens de juiste is om het later te implementeren. Om dit te corrigeren, selecteert u in dat geval het juiste project API App en op het tabblad **Instellingen** van de wizard **Publiceren** **Extra bestanden op bestemming verwijderen**.

Nadat u uw ASP.NET-API app in Azure App-Service wordt uitgevoerd hebt, kunt u voor meer informatie over de functies van Visual Studio die het oplossen van problemen vereenvoudigen. Zie [Service voor het oplossen van Azure-App apps in Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)voor meer informatie over logboekregistratie foutopsporing op afstand, en nog veel meer.

## <a name="next-steps"></a>Volgende stappen

U hebt gezien hoe bestaande projecten van het Web API implementeren op API apps, clientcode genereren voor API apps en apps van clients .NET API verbruiken. De volgende zelfstudie in deze reeks toont het [gebruik van CORS apps van clients JavaScript API in beslag neemt](app-service-api-cors-consume-javascript.md).

Zie voor meer informatie over het genereren van client-code, de opslagplaats [Azure/AutoRest](https://github.com/azure/autorest) op GitHub.com. Voor hulp bij problemen met de gegenereerde client, opent u een [probleem in de bibliotheek AutoRest](https://github.com/azure/autorest/issues).

Als u wilt dat nieuwe API app projecten helemaal zelf maakt, moet u de sjabloon **Azure API App** gebruiken.

![Sjabloon API App in Visual Studio](./media/app-service-api-dotnet-get-started/apiapptemplate.png)

De projectsjabloon **Azure API App** is equivalent aan het kiezen van de **lege** ASP.NET 4.5.2 sjabloon, klikt u op het selectievakje ondersteuning voor Web-API en het pakket Swashbuckle NuGet installeren. Bovendien wordt de sjabloon toegevoegd sommige configuratiecode Swashbuckle is ontworpen om te voorkomen dat dubbele Swagger bewerking id's worden gemaakt. Als u een project API App hebt gemaakt, kunt u deze op implementeren een API-app dezelfde manier als u deze zelfstudie hebt gezien.
