<properties
    pageTitle="Maken van een web app van Azure Marketplace | Microsoft Azure"
    description="Informatie over het maken van een nieuwe WordPress web app van Azure Marketplace met behulp van de Portal Azure."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

<!-- Note: This article replaces web-sites-php-web-site-gallery.md -->

# <a name="create-a-web-app-from-the-azure-marketplace"></a>Maken van een web app van Azure Marketplace

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

De markt Azure beschikbaar voor tal van populaire web apps ontwikkeld door Microsoft, andere bedrijven en initiatieven van open source software. Bijvoorbeeld, WordPress, Umbraco CMS, Drupal, enz. Deze web-apps zijn gebaseerd op een breed scala van populaire frameworks, zoals [PHP] in deze WordPress bijvoorbeeld [.NET], [Node.js], [Java]en [Python], om een paar te noemen. Is de browser die u voor de [Azure Portal gebruikt]maken van een web app van Azure Marketplace alleen software die u nodig hebt.

In deze zelfstudie leert u hoe:

* Zoeken en web app in Azure App-Service die is gebaseerd op een sjabloon Azure Marketplace te maken.
* Azure App Service instellingen configureren voor de nieuwe web app.
* Opzetten en beheren van uw web app.

Voor de toepassing van deze zelfstudie implementeert u een WordPress blogsite van Azure Marketplace. Wanneer u de stappen in deze zelfstudie hebt voltooid, hebt u uw eigen WordPress site up en uitgevoerd in de cloud.

![Voorbeeld van de WordPress wep app dashboard][WordPressDashboard1]

De WordPress-site die u in deze zelfstudie implementeren gaat wordt MySQL voor de database. Als u gebruiken voor de database SQL-Database wilt, Zie [Nami Project], dat ook beschikbaar is via de markt Azure.

> [AZURE.NOTE]
> Als u deze zelfstudie hebt voltooid, moet u een account van Microsoft Azure. Als u geen account hebt, kunt u [uw voordelen van Visual Studio abonnee activeren] [ activate] of [zich aanmelden voor een gratis proefversie][free trial].
>
> Als u aan de slag met Azure App-Service wilt voordat u zich voor een account met Azure aanmeldt, gaat u naar de [App-Service probeert]. Van daaruit kunt u direct een starter tijdelijk web app in App-Service maken, geen creditcard is vereist en er zijn geen verplichtingen.

## <a name="find-and-create-a-web-app-in-azure-app-service"></a>Zoeken en maken van een Web App in Azure App-Service

1. Log in op de [Azure Portal].

1. Klik op **Nieuw**.
    
    ![Maak een nieuwe resource Azure][MarketplaceStart]
    
1. Zoeken naar **WordPress**en klik vervolgens op **WordPress**. (Als u wilt een SQL-Database gebruiken in plaats van MySQL, zoekt **Project Nami**.)

    ![Zoeken naar WordPress in de markt][MarketplaceSearch]
    
1. Na het lezen van de beschrijving van de WordPress app, klikt u op **maken**.

    ![WordPress web app maken][MarketplaceCreate]

## <a name="configure-azure-app-service-settings-for-your-new-web-app"></a>Azure App Service-instellingen configureren voor uw nieuwe Web App

1. Nadat u een nieuw web app hebt gemaakt, wordt de blade WordPress-instellingen weergegeven, waarin u de volgende stappen uitvoeren:

    ![WordPress web app-instellingen configureren][ConfigStart]

1. Voer een naam voor het web app in het **Web app** .

    Deze naam moet uniek zijn in het domein azurewebsites.net omdat de URL van de web app *{naam}*. azurewebsites.net. Als de naam die u invoert niet uniek is, wordt een rood uitroepteken weergegeven in het tekstvak.

    ![Configureer de naam van de WordPress web app][ConfigAppName]

1. Als u meer dan één abonnement hebt, kiest u de gewenste. 

    ![Het abonnement voor de web app configureren][ConfigSubscription]

1. Een **Resourcegroep** selecteren of een nieuwe maken.

    Zie voor meer informatie over resourcegroepen [Azure Resource Manager-overzicht][ResourceGroups].

    ![De resourcegroep voor het web app configureren][ConfigResourceGroup]

1. Een **App plan/locatie** selecteren of een nieuwe maken.

    Zie voor meer informatie over App serviceplannen [Azure App Service plannen-overzicht][AzureAppServicePlans]. 

    ![Het abonnement voor de web app configureren][ConfigServicePlan]

1. Klik op **Database**en in de **Nieuwe MySQL Database** blade geeft de vereiste waarden voor het configureren van de MySQL-database.

    een. Voer een nieuwe naam of laat de standaardnaam.

    b. Laat het **Type Database** ingesteld op **gedeeld**.

    c. Kies de locatie van een door die u voor het web app gekozen.

    d. Kies een prijzen laag. **Kwik** - die is gratis met minimale verbindingen en schijfruimte - is geen probleem voor deze zelfstudie.

    e. Accepteer de juridische voorwaarden in de blade **Nieuwe MySQL-Database** en klik op **OK**. 

    ![De database configureren voor het web app][ConfigDatabase]

1. In de blade **WordPress** akkoord met de juridische voorwaarden en klik vervolgens op **maken**. 

    ![De app instellingen voltooien en klik op OK][ConfigFinished]

    Azure App-Service maakt normaal gesproken de web-app in minder dan een minuut. U kunt de voortgang bekijken door te klikken op de bell-pictogram aan de bovenkant van de portal-pagina.

    ![Voortgangsindicator][ConfigProgress]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Opzetten en beheren van uw WordPress web app
    
1. Als het web app maken is voltooid, Ga in de Azure-Portal voor de bronnengroep waarin u de toepassing hebt gemaakt en u ziet de web app en de database.

    De extra bron met het gloeilampje [Inzichten van toepassing]is[ApplicationInsights], die verzorgt toezicht voor uw web app.

1. Klik in de blade **resourcegroep** op de regel web app.

    ![Selecteer uw WordPress web app][WordPressSelect]

1. Klik op **Bladeren**in het Web app-blade.

    ![Ga naar uw WordPress web app][WordPressBrowse]

1. Als u wordt gevraagd of u de taal voor uw WordPress blog te selecteren, selecteert u de gewenste taal en klik op **Doorgaan**.

    ![De taal voor uw WordPress web app configureren][WordPressLanguage]

1. Op de pagina **Welkom** WordPress, voert u de configuratiegegevens die zijn vereist voor WordPress en klik vervolgens op **WordPress installeren**.

    ![Configureer de instellingen van uw WordPress web app][WordPressConfigure]

1. Meld u aan met de referenties die u hebt gemaakt op de pagina **Welkom** .  

1. De pagina van de Dashboard-site en de informatie die u hebt opgegeven.    

    ![Uw WordPress dashboard weergeven][WordPressDashboard2]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u gezien hoe u kunt maken en implementeren van een voorbeeld van de web app van Azure Marketplace.

Zie de koppelingen aan de linkerkant van de pagina (voor windows browser breed) voor meer informatie over het werken met App Service Web Apps of aan de bovenkant van de pagina (voor smalle vensters).

Zie voor meer informatie over het ontwikkelen van WordPress web apps op Azure [WordPress op Azure App ontwikkelen][WordPressOnAzure]. 

<!-- URL List -->

[PHP]: https://azure.microsoft.com/develop/php/
[.NET]: https://azure.microsoft.com/develop/net/
[Node.js]: https://azure.microsoft.com/develop/nodejs/
[Java]: https://azure.microsoft.com/develop/java/
[Python]: https://azure.microsoft.com/develop/python/
[activate]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[free trial]: https://azure.microsoft.com/pricing/free-trial/
[Probeer de Service App]: http://go.microsoft.com/fwlink/?LinkId=523751
[ResourceGroups]: ../resource-group-overview.md
[AzureAppServicePlans]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[ApplicationInsights]: https://azure.microsoft.com/services/application-insights/
[Azure Portal]: https://portal.azure.com/
[Project Nami]: http://projectnami.org/
[WordPressOnAzure]: ./develop-wordpress-on-app-service-web-apps.md

<!-- IMG List -->

[MarketplaceStart]: ./media/app-service-web-create-web-app-from-marketplace/marketplacestart.png
[MarketplaceSearch]: ./media/app-service-web-create-web-app-from-marketplace/marketplacesearch.png
[MarketplaceCreate]: ./media/app-service-web-create-web-app-from-marketplace/marketplacecreate.png
[ConfigStart]: ./media/app-service-web-create-web-app-from-marketplace/configstart.png
[ConfigAppName]: ./media/app-service-web-create-web-app-from-marketplace/configappname.png
[ConfigSubscription]: ./media/app-service-web-create-web-app-from-marketplace/configsubscription.png
[ConfigResourceGroup]: ./media/app-service-web-create-web-app-from-marketplace/configresourcegroup.png
[ConfigServicePlan]: ./media/app-service-web-create-web-app-from-marketplace/configserviceplan.png
[ConfigDatabase]: ./media/app-service-web-create-web-app-from-marketplace/configdatabase.png
[ConfigFinished]: ./media/app-service-web-create-web-app-from-marketplace/configfinished.png
[ConfigProgress]: ./media/app-service-web-create-web-app-from-marketplace/configprogress.png
[WordPressSelect]: ./media/app-service-web-create-web-app-from-marketplace/wpselect.png
[WordPressBrowse]: ./media/app-service-web-create-web-app-from-marketplace/wpbrowse.png
[WordPressLanguage]: ./media/app-service-web-create-web-app-from-marketplace/wplanguage.png
[WordPressDashboard1]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard1.png
[WordPressDashboard2]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard2.png
[WordPressConfigure]: ./media/app-service-web-create-web-app-from-marketplace/wpconfigure.png
