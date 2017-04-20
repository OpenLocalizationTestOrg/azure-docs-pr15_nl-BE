<properties 
    pageTitle="Uw eerste Java web app distribueren naar Azure in vijf minuten | Microsoft Azure" 
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
    
# <a name="deploy-your-first-java-web-app-to-azure-in-five-minutes"></a>Uw eerste Java web app distribueren naar Azure in vijf minuten

Deze zelfstudie helpt u bij het implementeren van een eenvoudige Java web app met [Azure App-Service](../app-service/app-service-value-prop-what-is.md).
App-Service kunt u web-apps, [mobiele app back-ends](/documentation/learning-paths/appservice-mobileapps/)en [API apps](../app-service-api/app-service-api-apps-why-best-platform.md)maken.

U wordt: 

- Maak een web app in Azure App-Service.
- Implementeer een monster Java-app.
- Zie de code die live in de productie wordt uitgevoerd.

## <a name="prerequisites"></a>Vereisten

- Krijg een client FTP/FTPS, zoals [FileZilla](https://filezilla-project.org/).
- Een Microsoft Azure-account aanvragen. Als u geen account hebt, kunt u [uw voordelen van Visual Studio abonnee activeren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)of [registreren voor een gratis proefversie](/pricing/free-trial/?WT.mc_id=A261C142F) .

>[AZURE.NOTE] [Probeer het App-Service](http://go.microsoft.com/fwlink/?LinkId=523751) kunt u zonder een Azure-account. Een starter app en te spelen met het voor een uur lang--geen creditcard vereist, geen verbintenissen.

<a name="create"></a>
## <a name="create-a-web-app"></a>Maken van een web app.

1. Log in om de [Azure portal](https://portal.azure.com) met uw account Azure.

2. Klik in het linkermenu op **Nieuw** > **Web + Mobile** > **Web App**.

    ![](./media/app-service-web-get-started-languages/create-web-app-portal.png)

3. Gebruik de volgende instellingen voor uw nieuwe app in de app maken-blade:

    - **Naam toep.**: Typ een unieke naam.
    - **Resourcegroep**: **Nieuw** selecteert en een naam geven aan de resourcegroep.
    - **App plan/locatie**: klikt u op configureren en klik vervolgens op **Nieuw** om de naam, locatie en prijzen laag van het abonnement App instellen. Gerust **gratis** prijzen laag.

    Wanneer u bent klaar, uw app maken blade ziet er zo uit:

    ![](./media/app-service-web-get-started-languages/create-web-app-settings.png)

3. Klik onderaan op **maken** . U kunt klikken op pictogram in het **systeemvak** boven de voortgang bekijken.

    ![](./media/app-service-web-get-started-languages/create-web-app-started.png)

4. Wanneer de implementatie is voltooid, kunt u deze melding ziet. Klik op het bericht van uw implementatie blade openen.

    ![](./media/app-service-web-get-started-languages/create-web-app-finished.png)

5. In het blad **implementatie is voltooid** , klikt u op de koppeling **bron** van uw nieuwe web app blade.

    ![](./media/app-service-web-get-started-languages/create-web-app-resource.png)

## <a name="deploy-a-java-app-to-your-web-app"></a>Een Java-toepassing implementeren op uw web app

Nu gaan we een Java-toepassing implementeren op FTPS met Azure.

5. In het blad web app, Ga naar de **instellingen van de toepassing** of zoekt u het en klik erop. 

    ![](./media/app-service-web-get-started-languages/set-java-application-settings.png)

6. **Java-versie**, selecteert u **Java 8** en klik op **Opslaan**.

    ![](./media/app-service-web-get-started-languages/set-java-application-settings.png)

    Wanneer u de melding **bijgewerkt web app instellingen**, Ga naar http://*&lt;appname >*. azurewebsites.net de standaard JSP servlet in actie zien.

7. Terug in de blade web app, Ga naar de **implementatie van referenties** of zoekt u het en klik erop.

8. Stel uw referenties voor de implementatie en klik op **Opslaan**.

7. Klik op **Overzicht**in het web app-blade. Naast **de gebruikersnaam FTP/implementatie** en **TRANSFEREERT hostname**, klikt u op de knop **kopiëren** als deze waarden wilt kopiëren.

    ![](./media/app-service-web-get-started-languages/get-ftp-url.png)

    U kunt nu uw Java-app met FTPS implementeren.

8. FTP/FTPS-client zich aanmelden met uw Azure web app in de FTP-server met behulp van de waarden die u hebt gekopieerd in de laatste stap. Gebruik het wachtwoord voor implementatie die u eerder hebt gemaakt.

    Het volgende screenshot toont aanmelden met behulp van FileZilla.

    ![](./media/app-service-web-get-started-languages/filezilla-login.png)

    Wordt er beveiligingswaarschuwingen voor het niet-herkende SSL-certificaat van Azure. Ga verder en verder.

9. Klik op [deze koppeling](https://github.com/Azure-Samples/app-service-web-java-get-started/raw/master/webapps/ROOT.war) om het WAR-bestand downloaden naar uw lokale computer.

9. Ga naar **/site/wwwroot/webapps** op de externe site in uw client FTP/FTPS en sleept u het WAR-bestand op uw lokale computer gedownload in een externe map.

    ![](./media/app-service-web-get-started-languages/transfer-war-file.png)

    Klik op **OK** om het bestand in Azure overschrijven.

    >[AZURE.NOTE] Volgens het standaardgedrag van Tomcat, bestandsnaam **ROOT.war** in /site/wwwroot/webapps kunt u de web-app root (http://*&lt;appname >*. azurewebsites.net), en de bestandsnaam ** * &lt;anyname >*.war** biedt u een benoemde web app (http://*&lt;appname >*.azurewebsites.net/*&lt;anyname >*).

Dat is alles. De Java-app is nu actief in Azure wonen. In uw browser, gaat u naar http://*&lt;appname >*. azurewebsites.net om het te zien in actie. 

## <a name="make-updates-to-your-app"></a>Breng wijzigingen aan uw app

Wanneer u een update, moet u alleen het nieuwe WAR bestand uploaden naar de externe map met uw FTP/FTPS client.

## <a name="next-steps"></a>Volgende stappen

[Een Java web app met een sjabloon in de markt Azure maken](web-sites-java-get-started.md#marketplace). U kunt uw eigen volledig aanpasbare Tomcat-container en de bekende beheer UI. 

Fouten opsporen in uw Azure web app, rechtstreeks in [IntelliJ](app-service-web-debug-java-web-app-in-intellij.md) of [Eclips](app-service-web-debug-java-web-app-in-eclipse.md).

Of Doe meer met uw eerste web app. Bijvoorbeeld:

- Probeer [andere manieren om uw code naar Azure-implementatie](../app-service-web/web-sites-deploy.md). 
- Uw app Azure nemen naar het volgende niveau. De gebruikers worden geverifieerd. De schaal is die het gebaseerd op de vraag. Sommige prestatiewaarschuwingen instellen. Allemaal met slechts enkele muisklikken. Zie [functionaliteit toevoegen aan uw eerste web app](app-service-web-get-started-2.md).

