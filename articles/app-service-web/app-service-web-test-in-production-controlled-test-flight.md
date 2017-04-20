<properties
    pageTitle="Flighting implementatie (béta-testfase) in Azure App-Service"
    description="Informatie over nieuwe functies in uw app flight of bètaversie test uw updates in deze zelfstudie end-to-end. In dit venster App onderhouden voorzieningen zoals doorlopend publiceren, sleuven verkeersroutering en integratie van de inzichten van toepassing."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/02/2016"
    ms.author="cephalin"/>
# <a name="flighting-deployment-beta-testing-in-azure-app-service"></a>Flighting implementatie (béta-testfase) in Azure App-Service

In deze zelfstudie wordt beschreven hoe u *flighting implementaties* doen door de integratie van de verschillende mogelijkheden van [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) en [Azure toepassing inzichten](/services/application-insights/). 

*Flighting* is een implementatieproces een nieuwe functie of wijzigen met een beperkt aantal klanten echt evalueert en is een belangrijke test in het productieschema. Het lijkt op het testen van beta en wordt ook wel aangeduid als 'gecontroleerde test flight'. Deze aanpak veel grote ondernemingen met een website gebruiken om vroegtijdige validatie van hun app updates in hun praktijk van [flexibele ontwikkeling](https://en.wikipedia.org/wiki/Agile_software_development). Azure App-Service kunt u de test in de productie continu publiceren en inzichten voor het implementeren van DevOps hetzelfde scenario toepassing integreren. Voordelen van deze benadering zijn:

- **Winst real feedback _voordat_ updates worden vrijgegeven voor productie** - het enige wat beter dan feedback krijgen zodra u vrijgeven is feedback krijgen voordat u vrijgeven. U kunt updates met echte gebruikersverkeer en het gedrag vroeg u nodig hebt in de levenscyclus van producten testen.
- **Verbeteren [continu test driven development (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development) ** - test in productie integreren met continue integratie en instrumentatie met toepassing van inzichten, gebruikersvalidatie gebeurt vroeg en automatisch in de levenscyclus van uw product. Dit vermindert de investeringen in de handmatige test worden uitgevoerd.
- **Optimaliseren workflow test** - doordat de test in een continue controle-instrumenten, productie, kunt u mogelijk uitvoeren de doelstellingen van de verschillende soorten proeven in één proces, zoals [integratie](https://en.wikipedia.org/wiki/Integration_testing), [regressie](https://en.wikipedia.org/wiki/Regression_testing), [bruikbaarheid](https://en.wikipedia.org/wiki/Usability_testing), toegankelijkheid, lokalisatie, [prestaties](https://en.wikipedia.org/wiki/Software_performance_testing), [veiligheid](https://en.wikipedia.org/wiki/Security_testing)en [acceptatie](https://en.wikipedia.org/wiki/Acceptance_testing).

Een implementatie van flighting is niet alleen om live verkeer routing. In een dergelijk systeem die om inzicht te krijgen zo snel mogelijk de gewenste, of het zijn een onverwachte fout, afname van de prestaties, de gebruiker ervaring problemen. Denk eraan dat u werkt met echte klanten. Dus om het te doen rechts, moet u ervoor zorgen dat u hebt ingesteld uw implementatie van flighting voor het verzamelen van alle gegevens die u nodig hebt om een weloverwogen beslissing te nemen voor de volgende stap. In deze zelfstudie wordt beschreven hoe u gegevens verzamelen met toepassing inzichten u, maar u kunt nieuwe Relic of andere technologieën die aansluit bij uw scenario. 

## <a name="what-you-will-do"></a>Wat u doet

In deze zelfstudie leert u hoe u de volgende scenario's samen voor uw App Service app testen in productie brengen:

- [Route productie verkeer](app-service-web-test-in-production-get-start.md) naar uw app beta
- [Uw app instrument](../application-insights/app-insights-web-track-usage.md) handig metrics verkrijgen
- Voortdurend uw app beta implementeren en app live volgen
- Cijfers tussen de productie-app en de bèta-app om te zien hoe codewijzigingen vertalen naar resultaten vergelijken

## <a name="what-you-will-need"></a>Wat moet u

-   Een account Azure
-   Een [GitHub](https://github.com/) -account
- Visual Studio 2015 - download de [editie van de Gemeenschap](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx).
-   GIT-Shell (geïnstalleerd met [GitHub voor Windows](https://windows.github.com/)) - Hiermee kunt u de Git en de PowerShell-opdrachten uitvoeren in dezelfde sessie
-   Nieuwste [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) bits
-   Basiskennis van de volgende handelingen uit:
    -   Sjabloon voor implementatie van [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) (Zie [een complexe toepassing in Azure voorspelbare implementatie](app-service-deploy-complex-application-predictably.md))
    -   [GIT](http://git-scm.com/documentation)
    -   [PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [AZURE.NOTE] Een Azure-account voor het voltooien van deze zelfstudie hebt u nodig:
> + U kunt [gratis een Azure account openen](/pricing/free-trial/) - credits dat u kunt u uitproberen betaalde Azure services en zelfs nadat u ze gebruikt kunt u de account en gebruik gratis Azure services, zoals Web Apps.
> + U kunt [Visual Studio abonnee voordelen activeren](/pricing/member-offers/msdn-benefits-details/) - Visual Studio uw abonnement geeft u credits elke maand die u voor betaalde Azure services gebruiken kunt.
>
> Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een starter tijdelijk web app in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.

## <a name="set-up-your-production-web-app"></a>Instellen van uw web app in productie

>[AZURE.NOTE] Het script dat wordt gebruikt in deze zelfstudie configureert automatisch continue publicatie uit uw bibliotheek GitHub. Hiervoor is vereist dat uw referenties GitHub al zijn opgeslagen in Azure, anders mislukt de implementatie met de scripts tijdens het configureren van instellingen voor bronbeheer voor het web apps.
>
>Voor het opslaan van uw referenties GitHub in Azure, een web app te maken in de [Portal Azure](https://portal.azure.com/) en [implementatie van GitHub configureren](app-service-continuous-deployment.md#Step7). Alleen moet u dit één keer doen.

In een typisch voorbeeld van DevOps, hebt u een toepassing die wordt uitgevoerd in Azure wonen en u wilt wijzigen door middel van continue publiceren. In dit scenario implementeert u productie een sjabloon die u hebt ontwikkeld en getest.

1.  Maak uw eigen fork van de bibliotheek [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) . Zie voor meer informatie over het maken van uw vork [vork een Repo](https://help.github.com/articles/fork-a-repo/). Als uw vork is gemaakt, kunt u deze bekijken in uw browser.

    ![](./media/app-service-agile-software-development/production-1-private-repo.png)

2.  Open een Git-Shell-sessie. Als u nog geen Git-Shell, nu [GitHub voor Windows](https://windows.github.com/) installeren.
3.  Maak een lokale kopie van uw vork door het uitvoeren van de volgende opdracht:

        git clone https://github.com/<your_fork>/ToDoApp.git

4.  Als u uw lokale kopie hebt, Ga naar * &lt;repository_root >*\ARMTemplates, en installeer het deploy.ps1 script met een uniek achtervoegsel, zoals hieronder wordt weergegeven:

        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git -ResourceGroupSuffix <your_suffix>

4.  Wanneer dat wordt gevraagd, typt u de gewenste gebruikersnaam en wachtwoord voor toegang tot de database. Vergeet niet uw databasereferenties moet u ze opnieuw opgeven wanneer de resourcegroep bijwerken.

    U ziet de provisioning voortgang van verschillende bronnen van Azure. Wanneer de implementatie is voltooid, wordt het script start de toepassing in de browser en geeft u een beschrijvende pieptoon.
    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.1-app-in-browser.png)

6.  Terug in de Git-Shell-sessie uitvoeren:

        .\swap –Name ToDoApp<your_suffix>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.2-swap-to-production.png)

7.  Wanneer het script is voltooid, gaat u terug naar het adres van de frontend (http://ToDoApp*&lt;your_suffix >*.azurewebsites.net/) voor een overzicht van de toepassing wordt uitgevoerd in de productie.
5.  Log in op de [Portal Azure](https://portal.azure.com/) en kijk eens wat wordt gemaakt.

    U moet kunnen zien twee web apps in dezelfde bronnengroep, een met de `Api` achtervoegsel in de naam. Als u de weergave van de groep resources bekijken, ook ziet u de SQL-Database en server, het abonnement App en de staging-sleuven voor het web apps. Blader door de verschillende bronnen en vergelijk deze met * &lt;repository_root >*\ARMTemplates\ProdAndStage.json om te zien hoe deze zijn geconfigureerd in de sjabloon.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.3-resource-group-view.png)

U kunt de productie app hebt ingesteld.  Nu we Stel dat u feedback ontvangt dat gebruiksgemak slecht voor de app is. Dus u onderzoeken wilt. U gaat uw app zodat u feedback instrument.

## <a name="investigate-instrument-your-client-app-for-monitoringmetrics"></a>Onderzoeken: Uw client app voor bewaking/metrieken Instrument

5. Open * &lt;repository_root >*\src\MultiChannelToDo.sln in Visual Studio.
6. Alle Nuget pakketten herstellen door met de rechtermuisknop op oplossing > **NuGet pakketten beheren oplossing voor** > **herstellen**.
6. Klik met de rechtermuisknop op **MultiChannelToDo.Web** > **Toepassing inzichten telemetrie toevoegen** > **Instellingen configureren** > wijzigen bronnengroep te ToDoApp*&lt;your_suffix >* > **Toepassing inzichten toevoegen aan Project**.
7. Open het blad voor de resource **MultiChannelToDo.Web** toepassing inzicht in de Portal Azure. Klik in het gedeelte **toepassingsstatus** **browser pagina laden gegevens te verzamelen informatie** > code kopiëren.
7. De gekopieerde JS instrumentation code toevoegen aan * &lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml net vóór het afsluitende `<heading>` code. De unieke instrumentation sleutel van uw toepassing inzicht resource moet bevatten.

        <script type="text/javascript">
        var appInsights=window.appInsights||function(config){
            function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
        }({
            instrumentationKey:"<your_unique_instrumentation_key>"
        });

        window.appInsights=appInsights;
        appInsights.trackPageView();
        </script>

11. Aangepaste gebeurtenissen naar verzenden inzichten van toepassing voor de muis klikken door de volgende code toe te voegen aan de onderkant van het lichaam:

        <script>
            $(document.body).find("*").click(function(event) {

                appInsights.trackEvent(event.target.tagName + ": " + event.target.className);
            });
        </script>

    Telkens wanneer een gebruiker klikt op een willekeurige plaats in de web app, deze JavaScript-fragment een aangepaste gebeurtenis verzendt naar de inzichten van toepassing.

12. In de Git-Shell commit en push-uw wijzigingen naar de vork in de GitHub. Wacht tot clients vernieuwen van de browser.

        git add -A :/
        git commit -m "add AI configuration for client app"
        git push origin master

6.  Wisselen van de gedistribueerde toepassing wijzigingen in productie:

        .\swap –Name ToDoApp<your_suffix>

13. Blader naar de resource van toepassing inzichten die u hebt geconfigureerd. Klik op aangepaste gebeurtenissen.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/01-custom-events.png)

    Als er geen parameters voor aangepaste gebeurtenissen, wacht een paar minuten en klikt u op **vernieuwen**.

Stel dat u ziet als een grafiek hieronder:

![](./media/app-service-web-test-in-production-controlled-test-flight/02-custom-events-chart-view.png)

En het raster gebeurtenis eronder:

![](./media/app-service-web-test-in-production-controlled-test-flight/03-custom-event-grid-view.png)

De **knop** gebeurtenis komt overeen met de knop indienen volgens de code van de toepassing ToDoApp en de **INPUT** gebeurtenis komt overeen met het tekstvak. Tot nu toe zinvol dingen. Maar blijkbaar is er een goede hoeveelheid muisklikken en slechts enkele muisklikken op de taakitems (de gebeurtenissen **LI** ).

Op basis van dit formulier u de hypothese dat sommige gebruikers in de war welk deel van de gebruikersinterface is klikbaar en omdat de cursor voor de geselecteerde tekst is opgemaakt als de muisaanwijzer op de pictogrammen en de items in de lijst.

![](./media/app-service-web-test-in-production-controlled-test-flight/04-to-do-list-item-ui.png)

Dit is een voorbeeld van de gekunstelde mogelijk. U gaat toch een verbetering van uw app maken en vervolgens een flighting om bruikbaarheid feedback van klanten live-implementatie uit te voeren.

### <a name="instrument-your-server-app-for-monitoringmetrics"></a>Uw app server voor controle/metrieken instrument
Dit is een tangens aangezien de clienttoepassing het scenario dat is aangetoond in deze zelfstudie alleen behandelt. Voor de volledigheid wordt u echter ingesteld van de server-side-app.

6. Klik met de rechtermuisknop op **MultiChannelToDo** > **Toepassing inzichten telemetrie toevoegen** > **Instellingen configureren** > wijzigen bronnengroep te ToDoApp*&lt;your_suffix >* > **Toepassing inzichten toevoegen aan Project**.
12. In de Git-Shell commit en push-uw wijzigingen naar de vork in de GitHub. Wacht tot clients vernieuwen van de browser.

        git add -A :/
        git commit -m "add AI configuration for server app"
        git push origin master

6.  Wisselen van de gedistribueerde toepassing wijzigingen in productie:

        .\swap –Name ToDoApp<your_suffix>

Dat is alles.

## <a name="investigate-add-slot-specific-tags-to-your-client-app-metrics"></a>Onderzoeken: Sleuf-specifieke codes toevoegen aan uw client app metrics

In deze sectie configureert u de verschillende sleuven voor het verzenden van specifieke sleuf telemetrie aan dezelfde resource inzichten van toepassing. Op deze manier vergelijken telemetriegegevens tussen verkeer van andere sleuven (omgevingen) om het effect van uw wijzigingen app gemakkelijk zien. Op hetzelfde moment, kunt u het verkeer voor de productie van de rest scheiden, zodat u kunt doorgaan met het controleren van uw app productie indien nodig.

Nadat u van gegevens over de werking van de client verzamelen bent, wordt u [een telemetrie initialiseren met de JavaScript-code toevoegen](../application-insights/app-insights-api-custom-events-metrics.md#js-initializer) in de index.cshtml. Als u testen van de prestaties van de server-side wilt, bijvoorbeeld, kunt u ook doen op dezelfde manier in uw servercode (Zie [Application inzichten API voor aangepaste gebeurtenissen en statistieken](../application-insights/app-insights-api-custom-events-metrics.md).

1. Eerst voegen de code bewteen de twee `//` opmerkingen hieronder in de JavaScript blokkeren die u hebt toegevoegd aan de `<heading>` eerder code.

        window.appInsights = appInsights;

        // Begin new code
        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["Environment"] = "@System.Configuration.ConfigurationManager.AppSettings["environment"]";
            });
        });
        // End new code

        appInsights.trackPageView();

    Deze code initialiseren heeft tot gevolg dat de `appInsights` object toe te voegen het een aangepaste eigenschap genoemd `Environment` aan elk onderdeel van de telemetrie wordt verzonden.

2. Vervolgens voegt u deze aangepaste eigenschappen als een [instelling van de sleuf](web-sites-staged-publishing.md#AboutConfiguration) voor uw web app in Azure. Voer hiervoor de volgende opdrachten in de Git-Shell-sessie.

        $app = Get-AzureWebsite -Name todoapp<your_suffix> -Slot production
        $app.AppSettings.Add("environment", "Production")
        $app.SlotStickyAppSettingNames.Add("environment")
        $app | Set-AzureWebsite -Name todoapp<your_suffix> -Slot production

    Het bestand Web.config in het project al definieert de `environment` app instelling. Met deze instelling wanneer u de app lokaal, test uw statistieken zal worden gelabeld met `VS Debugger`. Echter, wanneer u uw wijzigingen naar Azure push, Azure zal vinden en gebruiken de `environment` in de web-app configuratie instelling app en uw statistieken worden gelabeld met `Production`.

3. Commit en push-codewijzigingen naar de vork op GitHub en wacht vervolgens gebruikers gebruiken de nieuwe app (nodig voor het vernieuwen van de browser). Het duurt ongeveer 15 minuten voor de nieuwe eigenschap in de inzichten van uw toepassing wilt laten `MultiChannelToDo.Web` bron.

        git add -A :/
        git commit -m "add environment property to AI events for client app"
        git push origin master

4. Nu gaat u opnieuw naar de blade **aangepaste gebeurtenissen** en de statistieken filteren op `Environment=Production`. Nu moet u kunnen zien van alle nieuwe aangepaste gebeurtenissen in de productie-sleuf met dit filter.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/05-filter-on-production-environment.png)

5. Klik op de knop **Favorieten** in het huidige maatstelsel van Explorer-instellingen om op te slaan als **aangepaste gebeurtenissen: productie**. U kunt gemakkelijk schakelen tussen deze weergaven en een implementatie sleuf later.

    > [AZURE.TIP] Voor nog krachtigere analytics, kunt u [uw toepassing inzichten resource met Power BI integreren](../application-insights/app-insights-export-power-bi.md).

### <a name="add-slot-specific-tags-to-your-server-app-metrics"></a>Sleuf-specifieke codes toevoegen aan uw app serverstatistieken
Nogmaals, voor de volledigheid u stelt de server-side-app. In tegenstelling tot de client-app die is geïmplementeerd in JavaScript, sleuf-specifieke tags voor de servertoepassing is geïmplementeerd met .NET-code.

1. Open * &lt;repository_root >*\src\MultiChannelToDo\Global.asax.cs. Toevoegen van het codeblok, net vóór de afsluiting accolade naamruimte.

        namespace MultiChannelToDo
        {
                ...

                // Begin new code
            public class ConfigInitializer
            : ITelemetryInitializer
            {
                void ITelemetryInitializer.Initialize(ITelemetry telemetry)
                {
                    telemetry.Context.Properties["Environment"] = System.Configuration.ConfigurationManager.AppSettings["environment"];
                }
            }
                // End new code
        }

2. Naam resolutie fouten verbeteren door de `using` instructies onder aan het begin van het bestand:

        using Microsoft.ApplicationInsights.Channel;
        using Microsoft.ApplicationInsights.Extensibility;

3. De volgende code toevoegen aan het begin van de `Application_Start()` methode:

        TelemetryConfiguration.Active.TelemetryInitializers.Add(new ConfigInitializer());

3. Commit en push-codewijzigingen naar de vork op GitHub en wacht vervolgens gebruikers gebruiken de nieuwe app (nodig voor het vernieuwen van de browser). Het duurt ongeveer 15 minuten voor de nieuwe eigenschap in de inzichten van uw toepassing wilt laten `MultiChannelToDo` bron.

        git add -A :/
        git commit -m "add environment property to AI events for server app"
        git push origin master

## <a name="update-set-up-your-beta-branch"></a>-Update: Stel uw filiaal beta

2. Open * &lt;repository_root >*\ARMTemplates\ProdAndStagetest.json en zoek de `appsettings` bronnen (zoeken naar `"name": "appsettings"`). Er zijn 4, één voor elke sleuf. 

2. Voor elke `appsettings` resource toevoegen een `"environment": "[parameters('slotName')]"` app instelling aan het einde van de `properties` array. Vergeet niet aan het einde van de vorige regel met een komma.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/06-arm-app-setting-with-slot-name.png)
    
    U hebt net toegevoegd de `environment` app instelling van de sleuven in de sjabloon.
    
2. In hetzelfde bestand, vinden de `slotconfignames` bronnen (zoeken naar `"name": "slotconfignames"`). Er zijn 2 één voor elke toepassing.

2. Voor elke `slotconfignames` resource toevoegen `"environment"` aan het einde van de `appSettingNames` array. Vergeet niet aan het einde van de vorige regel met een komma.

    U zojuist hebt gemaakt de `environment` app stick instellen op de respectieve implementatie sleuf voor beide apps.  

3. Voer de volgende opdrachten op dezelfde resource groep achtervoegsel die u eerder hebt gebruikt in Git-Shell-sessie.

        git checkout -b beta
        git push origin beta
        .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch beta

4. Wanneer dat wordt gevraagd, geeft u dezelfde SQL-databasereferenties als vóór. Als de resourcegroep bijwerken wordt gevraagd, typt u `Y`, vervolgens `ENTER`.

    Zodra het script is voltooid, worden alle resources in de resourcegroep van de oorspronkelijke behouden, maar een nieuwe sleuf met de naam "beta" is gemaakt in deze met dezelfde configuratie als de sleuf "Staging" die in het begin is gemaakt.

    >[AZURE.NOTE] Deze methode voor het maken van verschillende omgevingen verschilt van de methode in [flexibele softwareontwikkeling met Azure App-Service](app-service-agile-software-development.md). Maakt u omgevingen met sleuven voor implementatie, waarbij er zo u omgevingen met resourcegroepen maakt. Omgevingen met resourcegroepen beheren kunt u de productieomgeving off-limits voor ontwikkelaars, maar het is niet heel gemakkelijk testen in productie, die u gemakkelijk met sleuven doen kunt.

Als u wilt, kunt u ook een alpha-app maken met

    git checkout -b alpha
    git push origin alpha
    .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch alpha

Voor deze zelfstudie blijven u gewoon gebruiken uw app beta.

## <a name="update-push-your-updates-to-the-beta-app"></a>Update: Updates van uw App beta Push

Terug naar uw app die u wilt verbeteren.

1. Zorg ervoor dat u kunt nu uw filiaal beta

        git checkout beta

2. In * &lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml, vinden de `<li>` code en voeg de `style="cursor:pointer"` attribuut, zoals hieronder wordt weergegeven.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/07-change-cursor-style-on-li.png)

3. Commit en push naar Azure.

4. Controleer dat de wijziging wordt nu weergegeven in de sleuf beta door naar http://todoapp te gaan*&lt;your_suffix >*-beta.azurewebsites.net/. Als u de wijziging nog niet ziet, vernieuw uw browser als u de nieuwe javascript-code.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/08-verify-change-in-beta-site.png)

Nu u hebt uw wijziging uitgevoerd in de bèta-sleuf, bent u klaar voor het uitvoeren van de implementatie van een flighting.

## <a name="validate-route-traffic-to-the-beta-app"></a>Valideren: Routeren van netwerkverkeer beta App

In deze sectie wordt u verkeer doorstuurt naar de beta app. Ter demonstratie van de duidelijkheid gaat u een aanzienlijk deel van de gebruikersverkeer doorsturen naar het. In werkelijkheid hangt de hoeveelheid verkeer die u wilt doorsturen naar uw specifieke situatie. Bijvoorbeeld, als uw site zich op de schaal van microsoft.com, mogelijk moet u minder dan één procent van het totale verkeer krijgt u nuttige gegevens.

1. Voer de volgende opdrachten op de helft van de productie-verkeer doorsturen naar de beta-sleuf in Git-Shell-sessie:

        $siteName = "ToDoApp<your suffix>"
        $rule = New-Object Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.RampUpRule
        $rule.ActionHostName = "$siteName-beta.azurewebsites.net"
        $rule.ReroutePercentage = 50
        $rule.Name = "beta"
        Set-AzureWebsite $siteName -Slot Production -RoutingRules $rule

  De `ReroutePercentage=50` -eigenschap wordt aangegeven dat 50% van de productie-verkeer wordt doorgestuurd naar de URL van de bèta-app (aangegeven door de `ActionHostName` eigenschap).

2. Ga naar http://ToDoApp*&lt;your_suffix >*. azurewebsites.net. nu wordt 50% van het verkeer omgeleid naar de beta-sleuf.

3. Filteren in de bron toepassing inzichten cijfers over de omgeving = "beta".

    > [AZURE.NOTE] Als u deze gefilterde weergave als een andere favoriet opslaat, kunt u gemakkelijk de metrische explorer weergaven tussen productie en beta-weergaven spiegelen.

Stel dat in een toepassing inzichten ziet u iets met de volgende strekking:

![](./media/app-service-web-test-in-production-controlled-test-flight/09-test-beta-site-in-production.png)

Niet alleen wordt dit weergegeven er zijn veel meer klikken op de `<li>` -tags, maar er lijkt te zijn van een stijging van de klikken op `<li>` tags. U mag concluderen dat mensen hebben ontdekt de nieuwe `<li>` codes zijn klikbaar en worden alle taken eerder voltooid in de app nu wissen.

Op basis van de gegevens van uw implementatie van flighting, besluit u dat de nieuwe gebruikersinterface gereed voor productie is.

## <a name="go-live-move-your-new-code-into-production"></a>Go live: de nieuwe code naar een productie

U kunt nu de update voor de productie te verplaatsen. Wat is groot is dat nu u weet dat de update al gevalideerd _voordat is_ deze wordt geduwd naar productie. Nu kunt u vol vertrouwen implementeren. Nadat u een update naar de clienttoepassing AngularJS hebt gemaakt, worden alleen de client-side code gevalideerd. Als u wijzigingen aanbrengt in de back-end Web API app, kunt u uw wijzigingen kan valideren op dezelfde manier en eenvoudig.

1. Verwijder de regel voor het doorsturen van verkeer door de volgende opdracht uit te voeren in de Git-Shell:

        Set-AzureWebsite $siteName -Slot Production -RoutingRules @()

2. Voer het Git-opdrachten:

        git checkout master
        git pull origin master
        git merge beta
        git push origin master

2. Wacht enkele minuten duren voordat de nieuwe code op de staging-sleuf kunnen worden geïmplementeerd en vervolgens starten http://ToDoApp*&lt;your_suffix >*-staging.azurewebsites.net om te controleren of de nieuwe update is opgewarmd in de staging-sleuf. Vergeet niet dat de master branch van uw vork is gekoppeld aan de staging-sleuf van uw app.

3. Nu, wisselen de staging-sleuf in productie

        cd <ROOT>\ToDoApp\ARMTemplates
        .\swap.ps1 -Name todoapp<your_suffix>

## <a name="summary"></a>Samenvatting ##

Azure App Service eenvoudig voor kleine tot middelgrote bedrijven voor het testen van hun klantgerichte apps iets in de productie die traditioneel is uitgevoerd in grote ondernemingen. Hopelijk hebt deze zelfstudie gekregen de kennis moet u App Service en inzichten die toepassing om mogelijke implementatie van flighting, en zelfs andere test in productie scenario's in de wereld van DevOps samen te voegen. 

## <a name="more-resources"></a>Meer bronnen ##

-   [Flexibele softwareontwikkeling met Azure App-Service](app-service-agile-software-development.md)
-   [Staging-omgevingen voor web apps in Azure App-Service instellen](web-sites-staged-publishing.md)
-   [Een complexe toepassing in Azure voorspelbare implementeren](app-service-deploy-complex-application-predictably.md)
-   [Azure Resource Manager sjablonen ontwerpen](../resource-group-authoring-templates.md)
-   [JSONLint - de JSON Validator](http://jsonlint.com/)
-   [GIT vertakkingen: Basic vertakkingen en samenvoegen](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
-   [Azure PowerShell](../powershell-install-configure.md)
-   [Project Kudu Wiki](https://github.com/projectkudu/kudu/wiki)
