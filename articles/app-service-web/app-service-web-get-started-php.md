<properties 
    pageTitle="Implementeren van de eerste PHP web app op Azure in vijf minuten | Microsoft Azure" 
    description="Leer hoe makkelijk web apps in App-Service worden uitgevoerd door het implementeren van een monster app. Start doen reële ontwikkeling snel en direct de resultaten bekijken." 
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
    ms.date="10/13/2016" 
    ms.author="cephalin"
/>
    
# <a name="deploy-your-first-php-web-app-to-azure-in-five-minutes"></a>Implementeren van de eerste PHP web app op Azure in vijf minuten

Deze handleiding helpt u bij het implementeren van uw eerste PHP web app met [Azure App-Service](../app-service/app-service-value-prop-what-is.md).
App-Service kunt u web-apps, [mobiele app back-ends](/documentation/learning-paths/appservice-mobileapps/)en [API apps](../app-service-api/app-service-api-apps-why-best-platform.md)maken.

U wordt: 

- Maak een web app in Azure App-Service.
- Voorbeeld van PHP-code implementeren.
- Zie de code die live in de productie wordt uitgevoerd.
- Bijwerken van uw web app de dezelfde manier [Git voert push](https://git-scm.com/docs/git-push).

## <a name="prerequisites"></a>Vereisten

- [Git](http://www.git-scm.com/downloads).
- [Azure CLI](../xplat-cli-install.md).
- Een account van Microsoft Azure. Als u geen account hebt, kunt u [uw voordelen van Visual Studio abonnee activeren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)of [registreren voor een gratis proefversie](/pricing/free-trial/?WT.mc_id=A261C142F) .

>[AZURE.NOTE] [Probeer het App-Service](http://go.microsoft.com/fwlink/?LinkId=523751) kunt u zonder een Azure-account. Een starter app en te spelen met het voor een uur lang--geen creditcard vereist, geen verbintenissen.

## <a name="deploy-a-php-web-app"></a>Implementeren van een PHP web app.

1. Open een nieuwe opdrachtprompt van Windows PowerShell-venster, Linux shell of OS X terminal. Uitvoeren van `git --version` en `azure --version` om te controleren dat Git en Azure CLI op uw computer zijn geïnstalleerd.

    ![Test de installatie van hulpmiddelen voor uw eerste web app CLI in Azure](./media/app-service-web-get-started/1-test-tools.png)

    Als u de hulpprogramma's hebt geïnstalleerd, Zie [vereisten](#Prerequisites) voor downloadkoppelingen.

3. Meld u aan bij Azure als volgt:

        azure login

    Volg de help-bericht om te gaan met de aanmelding.

    ![Log in op Azure voor het maken van uw eerste web app](./media/app-service-web-get-started/3-azure-login.png)

4. Azure CLI ASM-modus wijzigen en stel vervolgens de gebruiker implementatie voor App-Service. Implementeert u code later met behulp van de referenties.

        azure config mode asm
        azure site deployment user set --username <username> --pass <password>

1. In een werkmap wijzigen (`CD`) en het klonen van het monster app als volgt:

        git clone https://github.com/Azure-Samples/app-service-web-php-get-started.git

2. Wijzig in de opslagplaats van uw app monster. Bijvoorbeeld:

        cd app-service-web-php-get-started

4. De App Service app resource in Azure maken met een unieke naam en de implementatie-gebruiker die u eerder hebt geconfigureerd. Wanneer u wordt gevraagd, geeft u het nummer van het gewenste gebied.

        azure site create <app_name> --git --gitusername <username>

    ![Maak de Azure bron voor uw eerste web app in Azure](./media/app-service-web-get-started-languages/php-site-create.png)

    De app is nu gemaakt in Azure. Ook is de huidige map Git geïnitialiseerd en verbonden met de nieuwe App Service app als een Git remote.
    U kunt bladeren naar de app-URL (http://&lt;Naam_toepassing >. azurewebsites.net) om te zien de prachtige standaard HTML-pagina, maar we werkelijk de code er nu ophalen.

4. De voorbeeldcode implementeren op uw Azure app, zoals zou u code met Git push. Wanneer dat wordt gevraagd, gebruikt u het wachtwoord dat u eerder hebt geconfigureerd.

        git push azure master

    ![Push-code naar uw eerste web app in Azure](./media/app-service-web-get-started-languages/php-git-push.png)

    `git push`niet alleen code geplaatst in Azure, maar activeert ook implementatietaken in de installatie-engine. U kunt ook  [de componist-extensie inschakelen](web-sites-php-mysql-deploy-use-git.md#composer) voor het verwerken van bestanden in uw PHP-app composer.json automatisch.

Gefeliciteerd, u hebt uw app naar Azure App-Service geïmplementeerd.

## <a name="see-your-app-running-live"></a>Zie uw app live uitvoeren

Overzicht van uw app in Azure live uitgevoerd, moet u deze opdracht uitvoert vanaf een willekeurige map in uw bibliotheek:

    azure site browse

## <a name="make-updates-to-your-app"></a>Breng wijzigingen aan uw app

Nu kunt u in Git push uit op elk gewenst moment uw hoofdmap van het project (bibliotheek) om een update naar de actieve site. Doet u dit net als wanneer u de code voor het eerst geïmplementeerd. Bijvoorbeeld elke keer dat u wilt een nieuwe wijziging die u hebt getest lokaal push, gewoon Voer de volgende opdrachten vanaf de hoofdmap van het project (bibliotheek):

    git add .
    git commit -m "<your_message>"
    git push azure master

## <a name="next-steps"></a>Volgende stappen

[Maken, configureren en implementeren van een Laravel web app met Azure](app-service-web-php-get-started.md). Door deze zelfstudie leert u de basisvaardigheden, zoals een PHP web app in Azure, het uitvoeren:

- Maken en configureren van apps in Azure van PowerShell/Bash.
- PHP versie.
- Een start-bestand dat zich niet in de hoofdmap van de toepassing gebruikt.
- Composer inschakelen.
- Toegang tot milieu-specifieke variabelen.
- Algemene fouten oplossen.

Of Doe meer met uw eerste web app. Bijvoorbeeld:

- Probeer [andere manieren om uw code naar Azure-implementatie](../app-service-web/web-sites-deploy.md). Bijvoorbeeld voor de implementatie van een van de opslagplaatsen van GitHub, selecteert **GitHub** in plaats van **Lokale Git Repository** in **implementatieopties**.
- Uw app Azure nemen naar het volgende niveau. De gebruikers worden geverifieerd. De schaal is die het gebaseerd op de vraag. Sommige prestatiewaarschuwingen instellen. Allemaal met slechts enkele muisklikken. Zie [functionaliteit toevoegen aan uw eerste web app](app-service-web-get-started-2.md).

