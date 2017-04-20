<properties
    pageTitle="Aan de slag met de test in de productie voor Web Apps"
    description="Meer informatie over de proef in de productie (TiP)-functie in Azure App Service Web Apps."
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
    ms.date="01/13/2016"
    ms.author="cephalin"/>

# <a name="get-started-with-test-in-production-for-web-apps"></a>Aan de slag met de test in de productie voor Web Apps

Testen in de productie of live-testen van uw web app met live klantverkeer, is een test-strategie die app-ontwikkelaars meer en meer in hun [ontwikkeling flexibele](https://en.wikipedia.org/wiki/Agile_software_development) werkwijze integreren. Hiermee kunt u de kwaliteit van uw toepassingen met live-gebruikersverkeer in uw productieomgeving in plaats van kunstmatige gegevens in een testomgeving te testen. Door uw nieuwe app voor echte gebruikers weergeeft, u kunt in kennis gesteld op de echte problemen die uw app kan geconfronteerd zodra deze is geïmplementeerd. U kunt controleren of de functionaliteit, prestaties en waarde van uw app updates tegen het volume, snelheid en diverse praktische verkeer, waardoor u nooit in een testomgeving benaderen kunt.

## <a name="traffic-routing-in-app-service-web-apps"></a>Verkeer Routing in App Service Web Apps

U kunt met de functie Routering van verkeer in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)direct een deel van de actieve gebruikersverkeer naar een of meer [sleuven voor implementatie](web-sites-staged-publishing.md)en vervolgens analyseren uw app met [Azure toepassing inzichten](/services/application-insights/) of [Azure HDInsight](/services/hdinsight/)of een hulpprogramma van derden zoals [Nieuwe Relic](/marketplace/partners/newrelic/newrelic/) voor het valideren van de wijziging. U kunt bijvoorbeeld de volgende scenario's implementeren met App-Service:

- Functionele bugs detecteren en lokaliseren van knelpunten in de updates voor de hele site-implementatie
- "Gecontroleerde test vluchten" van uw wijzigingen door het meten van cijfers op de bèta-app usibility uitvoeren
- Geleidelijk tot een nieuwe update aangeeft hoeveel en netjes terug naar beneden naar de huidige versie als een fout optreedt 
- Optimaliseren van uw app bedrijfsresultaten door [A / B-test](https://en.wikipedia.org/wiki/A/B_testing) of [multidimensionale tests](https://en.wikipedia.org/wiki/Multivariate_testing_in_marketing) in meerdere sleuven voor implementatie

### <a name="requirements-for-using-traffic-routing-in-web-apps"></a>Vereisten voor het gebruik van routering van verkeer in Web Apps

- Uw web app moet worden uitgevoerd in de laag van de **Standard** of **Premium** , zoals vereist voor meerdere sleuven voor implementatie is.
- De goede werking moet routering van verkeer cookies zijn ingeschakeld in de browser van gebruikers. Routering van verkeer worden cookies gebruikt om de browser van de client naar een sleuf implementatie voor de levensduur van de clientsessie vastmaken.
- Routering van verkeer ondersteunt geavanceerde TiP-scenario's via Azure PowerShell-cmdlets.

## <a name="route-traffic-segment-to-a-deployment-slot"></a>Route-verkeer segment in een sleuf voor de implementatie

Op basisniveau in elke TiP-scenario kunt u een vooraf gedefinieerde percentage van uw live verkeer doorsturen naar een niet-productie-implementatie-sleuf. Volg hiervoor de onderstaande stappen:

>[AZURE.NOTE] Hier de stappen wordt ervan uitgegaan dat u al een [niet-productie-implementatie-sleuf](web-sites-staged-publishing.md) en dat de gewenste app webinhoud is al [geïmplementeerd](web-sites-deploy.md) op deze.

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/).
2. Klik op **Instellingen**in uw webtoepassing blade, > **Verkeer routeren**.
  ![](./media/app-service-web-test-in-production/01-traffic-routing.png)
3. Selecteer de sleuf die u wilt routeren van verkeer en het percentage van het totale verkeer dat u wenst en klik op **Opslaan**.

    ![](./media/app-service-web-test-in-production/02-select-slot.png)

4. Ga naar de implementatie van de sleuf blade. U ziet nu live verkeer wordt doorgestuurd naar het.

    ![](./media/app-service-web-test-in-production/03-traffic-routed.png)

Wanneer Routering van verkeer is geconfigureerd, wordt het opgegeven percentage van clients willekeurig gerouteerd naar uw niet-productieve sleuf. Het is echter belangrijk te weten dat zodra een client wordt automatisch doorgestuurd naar een bepaalde sleuf, deze worden "" aan die sleuf voor de duur van de clientsessie vastgemaakt zal. Dit gebeurt met behulp van een cookie met de gebruikerssessie pin. Als u de HTTP-aanvragen, zult u een `TipMix` cookie in elke volgende aanvraag.

![](./media/app-service-web-test-in-production/04-tip-cookie.png)

## <a name="force-client-requests-to-a-specific-slot"></a>Werking van clientaanvragen naar een bepaalde sleuf

Naast verkeersroutering automatisch kan App Service route aanvragen in een bepaalde sleuf. Dit is handig als u wilt dat gebruikers kunnen opt-in of opt-out van uw app in beta. Hiertoe gebruikt u de `x-ms-routing-name` queryparameter.

Voor het omleiden van gebruikers naar een specifieke sleuf met `x-ms-routing-name`, moet u ervoor zorgen dat de sleuf is al toegevoegd aan de lijst Routering van verkeer. Aangezien u doorsturen naar een sleuf expliciet wilt, het werkelijke percentage dat u ingesteld voor routering maakt niet uit. Als u wilt, kunt u een "bètalink" waarop gebruikers klikken kunnen om de toegang tot de bèta-app opgesteld.

![](./media/app-service-web-test-in-production/06-enable-x-ms-routing-name.png)

### <a name="opt-users-out-of-beta-app"></a>Gebruikers van beta app kiezen

Om gebruikers te laten kiezen uit uw beta app, kunt u bijvoorbeeld deze link in uw webpagina plaatsen:

    <a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back to production app</a>

De tekenreeks `x-ms-routing-name=self` Hiermee geeft u de sleuf van de productie. Zodra de browser van de client toegang tot de koppeling, niet alleen wordt deze omgeleid naar de productie-sleuf, maar elke volgende aanvraag bevat de `x-ms-routing-name=self` cookie die de sessie met de sleuf Productie kledingwinkelketen.

![](./media/app-service-web-test-in-production/05-access-production-slot.png)

### <a name="opt-users-in-to-beta-app"></a>Gebruikers in beta app kiezen

Om gebruikers te laten deelnemen aan uw app beta, dezelfde queryparameter instellen op de naam van de sleuf niet voor productie, bijvoorbeeld:

        <webappname>.azurewebsites.net/?x-ms-routing-name=staging

## <a name="more-resources"></a>Meer bronnen ##

-   [Staging-omgevingen voor web apps in Azure App-Service instellen](web-sites-staged-publishing.md)
-   [Een complexe toepassing in Azure voorspelbare implementeren](app-service-deploy-complex-application-predictably.md)
-   [Flexibele softwareontwikkeling met Azure App-Service](app-service-agile-software-development.md)
-   [Efficiënt gebruik van DevOps omgevingen voor uw web-apps](app-service-web-staged-publishing-realworld-scenarios.md)