<properties
    pageTitle="Functionaliteit toevoegen aan uw eerste web app"
    description="Handige functies toevoegen aan uw eerste web app in een paar minuten."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="05/12/2016"
    ms.author="cephalin"
/>

# <a name="add-functionality-to-your-first-web-app"></a>Functionaliteit toevoegen aan uw eerste web app

In [uw eerste web app met Azure in vijf minuten implementeren](app-service-web-get-started.md), kunt u een monster web app [Azure App](../app-service/app-service-value-prop-what-is.md)service geïmplementeerd. In dit artikel wordt zult u snel enkele geweldige functies toevoegen aan uw geïmplementeerde web app. U wordt in een paar minuten:

- verificatie voor gebruikers te dwingen
- uw app automatisch schalen
- meldingen ontvangen op de prestaties van uw app

Ongeacht welke monster app u in het vorige artikel hebt geïmplementeerd, kunt u volgen langs in de zelfstudie.

De drie activiteiten in deze zelfstudie worden slechts enkele voorbeelden van de vele handige voorzieningen die u krijgt wanneer u uw web app in de App-Service. Veel functies zijn beschikbaar in de **vrij** laag (die is wat uw eerste web app wordt uitgevoerd), en u kunt uw evaluatieversie credits functies waarvoor de prijzen hoger lagen uit te proberen. Dus zeker dat uw web app in **vrij** laag blijft tenzij u expliciet gewijzigd in een andere laag van de prijzen.

>[AZURE.NOTE] De web-app die u hebt gemaakt met Azure CLI in **vrij** laag, waardoor slechts één gedeelde VM exemplaar met quota's resource wordt uitgevoerd. Zie voor meer informatie over wat u met **vrij** laag, [limieten voor App-Service](../azure-subscription-service-limits.md#app-service-limits).

## <a name="authenticate-your-users"></a>Uw gebruikers verifiëren

Nu laten we zien hoe gemakkelijk het is verificatie toevoegen aan uw app (leeswijzer bij [App Service Authentication/Authorization](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/)).

1. In de portal blade voor uw app die u net hebt geopend, klikt u op **Instellingen voor** > **Authentication / Authorization**.  
    ![Verificatie - instellingen voor blade](./media/app-service-web-get-started/aad-login-settings.png)

2. Klik **op** verificatie inschakelen.  

4. Klik in **Verificatieproviders**op **Azure Active Directory**.  
    ![Verificatie - Selecteer Azure AD](./media/app-service-web-get-started/aad-login-config.png)

5. In het blad **Azure Active Directory-instellingen** **Express**, klik op **OK**. De standaardinstellingen maken een nieuwe toepassing in uw standaardmap Azure AD.  
 ![Verificatie - express-configuratie](./media/app-service-web-get-started/aad-login-express.png)

6. Klik op **Opslaan**.  
    ![Verificatie - configuratie opslaan](./media/app-service-web-get-started/aad-login-save.png)

    Nadat de wijziging voltooid is, ziet u de melding bell, groen en een vriendelijk bericht.

7. Klik in de portal blade van uw app op de **URL** -koppeling (of **Bladeren** in de menubalk). De koppeling is een HTTP-adres.  
    ![Verificatie - Ga naar URL](./media/app-service-web-get-started/aad-login-browse-click.png)  
    Maar zodra de toepassing wordt geopend in een nieuw tabblad, de URL vak omleidingen meermaals en eindigt op uw app met een HTTPS-adres. Wat u ziet is dat u al bent aangemeld bij uw abonnement Azure en bent u automatisch geverifieerd in de app.  
    ![Verificatie - ingelogd](./media/app-service-web-get-started/aad-login-browse-http-postclick.png)  
    Dus als u nu een niet-geverifieerde sessie in een andere browser opent, ziet u een scherm voor aanmelding als u naar de URL gaat.  
    <!-- ![Authenticate - login page](./media/app-service-web-get-started/aad-login-browse.png)  -->
   Als u iets met Azure Active Directory niet hebt gedaan, zijn de standaardmap kan geen Azure AD-gebruikers. In dat geval is waarschijnlijk de enige account die er het Microsoft-account bij uw abonnement Azure. Dat is de reden waarom u zijn automatisch aangemeld bij de app in dezelfde browser eerder.
   Die hetzelfde Microsoft-account kunt u op deze aanmeldingspagina en aanmelden.

Gefeliciteerd, u alle verkeer naar uw web app worden geverifieerd.

U hebt misschien al opgemerkt de **verificatie / vergunning** blade die u veel meer, zoals doen kunt:

- Sociale aanmelding inschakelen
- Meerdere opties voor aanmelden inschakelen
- Het standaardgedrag wijzigen wanneer mensen Ga eerst naar uw app

App-Service biedt dat een turn-key oplossing voor sommige van de gemeenschappelijke verificatie nodig zodat u niet hoeft te geven van de logica van de verificatie zelf.
Voor meer informatie Zie [App Service verificatie/vergunning](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

## <a name="scale-your-app-automatically-based-on-demand"></a>Schalen van uw app automatisch op basis van de vraag

Volgende we automatisch schalen uw app zodat deze automatisch deze capaciteit om te reageren op de behoeften van de gebruiker laten wilt (verder lezen op de [schaal van uw app in Azure](web-sites-scale.md) en [schalen exemplaar aantal handmatig of automatisch](../monitoring-and-diagnostics/insights-how-to-scale.md)).

Kortom, u de schaal van uw web app op twee manieren:

- [Schalen](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): meer CPU, geheugen, schijfruimte en extra functies zoals speciale VMs, aangepaste domeinen en certificaten, staging-sleuven en autoscaling ophalen. U laat de prijzen laag van het App-Service plan dat uw app behoort te wijzigen.
- [Geschaald uitbreiden](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): verhoging van het aantal VM exemplaren die uw toepassing uitvoeren.
Kunt u de schaal van maar liefst 50 exemplaren, afhankelijk van de prijzen laag.

Zonder verdere ado Stel autoscaling.

1. Eerst gaan we uitgebreid naar autoscaling inschakelen. Klik op **Instellingen**in de portal blade van uw app > **Schaal omhoog (App Service Plan)**.  
    ![Schaalbaar en - instellingen-blade](./media/app-service-web-get-started/scale-up-settings.png)

2. Blader en selecteer de **Standaard S1** laag, het laagste niveau dat ondersteunt autoscaling (omcirkeld in screenshot) en klik vervolgens op **selecteren**.  
    ![Schalen - laag kiezen](./media/app-service-web-get-started/scale-up-select.png)

    U bent klaar schaal omhoog.

    >[AZURE.IMPORTANT] Deze laag expends uw gratis proefperiode credits. Als u een pay-per-use-account hebt, maakt deze kosten voor je rekening.

3. Vervolgens gaan we autoscaling configureren. Klik op **Instellingen**in de portal blade van uw app > **Schaal uit (App Service Plan)**.  
    ![Geschaald uitbreiden - instellingen blade](./media/app-service-web-get-started/scale-out-settings.png)

4. **Schaal door** **CPU-Percentage**wijzigen. De schuifregelaars onder de vervolgkeuzelijst dienovereenkomstig bijwerken. Definieer vervolgens een **exemplaren** liggen tussen **1** en **2** en een **doelbereik** tussen **40** en **80**. Dit doen door in de vakken te typen of door de bijbehorende schuifregelaars.  
 ![Schaalbare - autoscaling configureren](./media/app-service-web-get-started/scale-out-configure.png)

    Op basis van deze configuratie, uw app automatisch aangepast om CPU-gebruik is meer dan 80% en schaalt wanneer CPU-gebruik van minder dan 40%.

5. Klik op **Opslaan** in de menubalk.

Gefeliciteerd, uw app autoscaling.

U hebt misschien al opgemerkt in de **Schaalinstellingen** bladeserver die u veel meer, zoals doen kunt:

- Handmatig schalen op een bepaald aantal exemplaren
- Schalen door andere prestatiegegevens, zoals geheugen of de schijf wachtrij
- Schaal gedrag aanpassen als een prestatie-regel wordt geactiveerd.
- Automatisch schalen op een schema
- Autoscaling gedrag voor een toekomstige gebeurtenis instellen

Zie voor meer informatie over het aanpassen van uw app, de [schalen van uw app in Azure](../app-service-web/web-sites-scale.md). Voor meer informatie over het schalen, Zie [schalen exemplaar aantal handmatig of automatisch](../monitoring-and-diagnostics/insights-how-to-scale.md).

## <a name="receive-alerts-for-your-app"></a>Meldingen voor uw app

Nu dat uw app autoscaling, wat er gebeurt als de maximale exemplaar telling (2) heeft bereikt en CPU hoger is dan de gewenste capaciteit (80%)?
U kunt een waarschuwing (leeswijzer bij [meldingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)) instellen zodat u deze situatie zodat u kunt verder omhoog of uit uw app, zoals schalen. Laten we snel een waarschuwing instellen voor dit scenario.

1. Klik op **Extra**in de portal blade van uw app > **waarschuwingen**.  
    ![Waarschuwingen - instellingen blade](./media/app-service-web-get-started/alert-settings.png)

2. Klik op **waarschuwing toevoegen**. Selecteer vervolgens de resource die op **(serverfarms eindigt)**in het vak **bron** . Dat is uw serviceplan App.  
    ![Waarschuwingen - toevoegen waarschuwing voor App-serviceplan](./media/app-service-web-get-started/alert-add.png)

3. Geef **de naam** als `CPU Maxed`, **Metric** als **Percentage van de CPU**en de **drempel** als `90` **e-eigenaren, medewerkers, en lezers**te selecteren en klik vervolgens op **OK**.   
 ![Waarschuwingen - configureren signaal](./media/app-service-web-get-started/alert-configure.png)

    Wanneer Azure gemaakt van de waarschuwing is, ziet u deze in de blade **waarschuwingen** .  
    ![Waarschuwingen - klaar weergeven](./media/app-service-web-get-started/alert-done.png)

Gefeliciteerd, u krijgt nu de waarschuwingen.

Deze waarschuwing instelling controleert elke vijf minuten voor het CPU-gebruik. Als dat nummer gaat boven de 90%, ontvangt u een waarschuwingsbericht per e-mail, samen met iedereen die gemachtigd is. Als u wilt zien aan iedereen die gemachtigd is om de waarschuwingen te ontvangen, terug te keren naar het blade portal van uw app en klik op de knop **toegang** .  
![Zie wie de waarschuwingen ontvangt](./media/app-service-web-get-started/alert-rbac.png)

U ziet zijn **beheerders abonnement** al **eigenaar** van de app. Deze groep zou u opnemen als u de accountbeheerder van uw abonnement op Azure (bv. uw proefabonnement). Voor meer informatie over Azure rollen gebaseerd toegangsbeheer Zie [Toegangsbeheer op Azure Role-Based](../active-directory/role-based-access-control-configure.md).

> [AZURE.NOTE] Waarschuwingsregels is een functie van Azure. Voor meer informatie, Zie [je de meldingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

## <a name="next-steps"></a>Volgende stappen

Op de weg voor het configureren van de waarschuwing misschien u opgevallen een uitgebreide verzameling hulpprogramma's in het **Extra** blad. Hier kunt u oplossen problemen, prestaties controleren, testen voor beveiligingsproblemen, resources beheren, communiceren met de console VM en handig extensies toevoegen. Wij nodigen u uit op een van deze hulpprogramma's de eenvoudige maar krachtige hulpmiddelen die uw vinger tips ontdekken.

Ontdek hoe u meer doen met uw app geïmplementeerd. Dit is slechts een gedeeltelijk overzicht:

- [Kopen en een aangepaste domeinnaam](custom-dns-web-site-buydomains-web-app.md) - koop een aantrekkelijke domein voor uw web app in plaats van de *. azurewebsites.net domein. Of gebruik een domein dat u al hebt.
- [Fasering omgevingen instellen](web-sites-staged-publishing.md) - uw app implementeren op een tijdelijke URL vóór de inbedrijfstelling productie. Bijwerken uw live web app met vertrouwen. Instellen van een uitgebreide oplossing voor DevOps met meerdere sleuven voor implementatie.
- [Continuous deployment instellen](app-service-continuous-deployment.md) - app implementatie integreren in uw bronbeheersysteem. Implementeren op Azure met elke commit.
- [Toegang op-premises bronnen](web-sites-hybrid-connection-get-started.md) - een bestaande Access op-premises-database of een CRM-systeem.
- Een [back-up van uw app](web-sites-backup.md) - instellen van back-up en terugzetten voor uw web app. Voorbereiden op onverwachte storingen en ze herstellen.
- De IIS-logboeken [inschakelen diagnostische logboeken](web-sites-enable-diagnostic-log.md) - van sporen Azure of toepassing worden gelezen. Lezen in een stroom, deze downloaden of deze poort in [Application inzichten](../application-insights/app-insights-overview.md) voor turn-key-analyse.
- [Scan uw app voor beveiligingslekken](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) -
uw web app tegen moderne gevaren met service van [Folie Security](https://www.tinfoilsecurity.com/)Scan.
- [Achtergrondtaken uitvoeren](../azure-functions/functions-overview.md) - taken uitvoeren voor gegevensverwerking, rapportage, enz.
- [Meer informatie over de werking van de App Service](../app-service/app-service-how-works-readme.md)
