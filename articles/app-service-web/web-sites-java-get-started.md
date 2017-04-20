<properties
    pageTitle="Maak een Java web app in Azure App Service | Microsoft Azure"
    description="In deze zelfstudie wordt beschreven hoe u een Java web app met Azure App Service implementeren."
    services="app-service\web"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="get-started-article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-java-web-app-in-azure-app-service"></a>Een Java web app in Azure App Service maken

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Deze zelfstudie laat zien hoe een Java [web app in Azure App-Service] maken met behulp van de [Portal Azure]. De Portal Azure is een web-interface die u gebruiken kunt voor het beheren van uw resources Azure.

> [AZURE.NOTE] Als u deze zelfstudie hebt voltooid, moet u een account van Microsoft Azure. Als u geen account hebt, kunt u [uw voordelen van Visual Studio abonnee te activeren] of [Aanmelden voor een gratis proefversie].
>
> Als u aan de slag met Azure App-Service wilt voordat u zich voor een account met Azure aanmeldt, gaat u naar de [App-Service probeert]. Daar kunt u direct maken een tijdelijk starter web app in App-Service. geen creditcard vereist is, en geen verplichtingen.

## <a name="java-application-options"></a>Opties voor Java-toepassing

Er zijn verschillende manieren die kunt u een Java-toepassing in een web App Service app instellen. 

1. Een app maken en configureer vervolgens de **instellingen van de toepassing**.

    App-Service biedt verschillende versies van Tomcat en Jetty, standaard-configuratie. Als de toepassing die u host met een van de ingebouwde versies werkt, deze methode voor het instellen van een web-container is het eenvoudigst en is ideaal als u wilt doen, is een war-bestand uploaden naar een web-container. Voor deze methode maakt een app in de Portal Azure en gaat u naar de blade **Toepassingsinstellingen** voor uw app in uw versie van Java en de gewenste Java web container te kiezen. Als u deze methode gebruikt worden zowel Java als uw web-container uitgevoerd via programmabestanden. De andere methoden worden in de ruimte op de schijf de container van het web en eventueel de JVM geplaatst. Wanneer u dit model gebruikt, hebt u geen toegang om bestanden te bewerken in dit deel van het bestandssysteem. Dit betekent dat u geen zaken uitvoeren als het bestand *server.xml* configureren of dll-bestanden in de map */lib* te plaatsen. Zie voor meer informatie de [maken en configureren van een Java web app](#appsettings) sectie verderop in deze handleiding.
    
2. Een sjabloon gebruiken van de markt Azure.

    De markt Azure bevat sjablonen die automatisch maken en configureren van Java web apps met Tomcat of Jetty web containers. De web-containers die de sjablonen maakt kunnen worden geconfigureerd. Zie voor meer informatie het gedeelte [een sjabloon Java van Azure Marketplace gebruiken](#marketplace) van deze zelfstudie.
  
3. Een app maken en handmatig kopiëren en bewerken van configuratiebestanden 

    U kunt een aangepaste Java-toepassing geen in een van de web-containers die door de App implementeert Service te hosten. Bijvoorbeeld:
    
    * De Java-toepassing is een versie van Tomcat of Jetty die rechtstreeks wordt niet ondersteund door de App Service of die in de galerie.
    * De Java-toepassing wordt HTTP-aanvragen en wordt niet als een WAR implementeren in een bestaande webpagina.
    * Wilt u de container web helemaal zelf configureren. 
    * Wilt u een versie van Java die niet wordt ondersteund in App Service en willen zelf uploaden.

    U kunt voor zulke gevallen een app via de Portal Azure maken en vervolgens de juiste runtime-bestanden handmatig opgeven. In dit geval wordt de bestanden afgeboekt op de opslagquota ruimte voor uw serviceplan App. Zie [een aangepaste Java web app met Azure uploaden]voor meer informatie.

## <a name="portal"></a>Maken en configureren van een Java web app.

In dit gedeelte ziet u hoe een web app maken en configureren voor Java met behulp van de bladeserver **instellingen van de toepassing** van de portal.

1. Aanmelden bij de [Azure Portal].

2. Klik op **Nieuw > Web + Mobile > Web App**.

    ![Nieuwe Web App][newwebapp]

4. Voer een naam voor het web app in het **Web app** .

    Deze naam moet uniek zijn in het domein azurewebsites.net omdat de URL van de web app {naam}. azurewebsites.net. Als de naam die u invoert niet uniek is, wordt een rood uitroepteken weergegeven in het tekstvak.

5. Een **Resourcegroep** selecteren of een nieuwe maken.

    Zie [werken met de Portal Azure Azure resources beheren]voor meer informatie over resourcegroepen.

6. Een **App plan/locatie** selecteren of een nieuwe maken.

    Voor meer informatie over App serviceplannen overzicht [Azure App Service plannen].

7. Klik op **maken**.

    ![Web App maken][newwebapp2]
 
8. Wanneer de web app is gemaakt, klikt u op **Web Apps > {uw web app}**.
 
    ![Selecteer Web App][selectwebapp]

9. Klik op **Instellingen**in de blade **Web app** .

10. Klik op **instellingen van de toepassing**.

11. Kies de gewenste **versie van Java**. 

12. Kies de gewenste **secundaire versie van Java**. Als u **Nieuw**selecteert, wordt uw app de nieuwste secundaire versie die beschikbaar is in de App-Service voor die belangrijke versie van Java gebruikt. Het **Nieuw** item is uniek voor Java apps gemaakt op basis van de **instellingen van de toepassing**. Als u de Java-toepassing uit de galerie maakt, hebt u uw eigen container en JVM wijzigingen beheren. 

12. Kies de gewenste **webpagina container**. Als u de containernaam van een die met een **Nieuw begint**, zullen uw app in de meest recente versie van die webpagina container primaire versie die beschikbaar is in de App-Service worden bewaard. 

    ![Versies van web-Container][versions]

13. Klik op **Opslaan**.

    Binnen enkele minuten uw web app worden op basis van Java en is geconfigureerd voor het gebruik van de web-container die u hebt geselecteerd.

14. Klik op **Web apps > {uw nieuwe web app}**.

15. Klik op de **URL** om naar de nieuwe site te bladeren.

    De webpagina wordt bevestigd dat u een Java-gebaseerde web app hebt gemaakt.

## <a name="marketplace"></a>Een Java-sjabloon gebruiken van de markt Azure

In dit gedeelte ziet u hoe met behulp van de markt Azure een Java web app. De algemene stroom van hetzelfde kan ook worden gebruikt op Java gebaseerde mobiele of API app maken. 

1. Aanmelden bij de [Azure Portal]

2. Klik op **Nieuw > Marketplace**.

    ![Nieuwe marktplaats][newmarketplace]

3. Klik op **Web + Mobile**.

    Mogelijk moet de links Ga naar de blade **Marketplace** waar u **Web + Mobile**kunt selecteren.

4. Voer de naam van een Java-toepassingsserver, bijvoorbeeld **Apache Tomcat** of **Jetty**, in het zoekvak en druk op Enter.

5. Klik op de Java-toepassingsserver in de zoekresultaten.

    ![Mobiele web-Jetty][webmobilejetty]

6. Klik op **maken**in de eerste **Apache Tomcat** of **Jetty** blade.

    ![Jetty Portal Blade][jettyblade]

7. Voer een naam voor de web app in het **Web app** in de volgende **Apache Tomcat** of **Jetty** blade.

    Deze naam moet uniek zijn in het domein azurewebsites.net omdat de URL van de web app {naam}. azurewebsites.net. Als de naam die u invoert niet uniek is, wordt een rood uitroepteken weergegeven in het tekstvak.

8. Een **Resourcegroep** selecteren of een nieuwe maken.

    Zie [werken met de Portal Azure Azure resources beheren]voor meer informatie over resourcegroepen.

9. Een **App plan/locatie** selecteren of een nieuwe maken.

    Voor meer informatie over App serviceplannen overzicht [Azure App Service plannen].

10. Klik op **maken**.

    ![Jetty Portal maken][jettyportalcreate2]

    In een korte periode, meestal minder dan een minuut, Azure is voltooid de nieuwe web app maken.

11. Klik op **Web apps > {uw nieuwe web app}**.

12. Klik op de **URL** om naar de nieuwe site te bladeren.

    ![Jetty-URL][jettyurl]

    Tomcat wordt geleverd met een reeks pagina's. Als u Tomcat kiest, ziet u een pagina, zoals in het volgende voorbeeld.

    ![Met Tomcat Apache web app.][tomcat]

    Als u Jetty kiest, ziet u een pagina, zoals in het volgende voorbeeld. Jetty geen een reeks pagina, dus hier opnieuw de dezelfde JSP die wordt gebruikt voor een lege site voor Java wordt gebruikt.

    ![Web app met Jetty][jetty]

Nu dat u de web app hebt gemaakt met een app container, Zie de sectie [volgende stappen](#next-steps) voor meer informatie over het uploaden van uw toepassing naar de web app.

## <a name="next-steps"></a>Volgende stappen

U hebt nu een Java-toepassingsserver in uw web app in Azure App-Service wordt uitgevoerd. Zie uw eigen code op de web app installeren, [toevoegen van een toepassing of webpagina naar uw Java web app].

Zie voor meer informatie over het ontwikkelen van Java-toepassingen in Azure [Java Developer Center].

<!-- URL List -->

[Een toepassing of webpagina's toevoegen aan uw Java web app]: ./web-sites-java-add-app.md
[Azure serviceplannen App voor overzicht]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[Azure Portal]: https://portal.azure.com/
[uw voordelen van Visual Studio abonnement activeren]: http://go.microsoft.com/fwlink/?LinkId=623901
[aanmelden voor een gratis proefversie]: http://go.microsoft.com/fwlink/?LinkId=623901
[Probeer de Service App]: http://go.microsoft.com/fwlink/?LinkId=523751
[Web app in Azure App-Service.]: http://go.microsoft.com/fwlink/?LinkId=529714
[Java Developer Center]: /develop/java/
[Azure resources beheren met behulp van de Portal Azure]: ../azure-portal/resource-group-portal.md
[Een aangepaste Java web app uploaden naar Azure]: ./web-sites-java-custom-upload.md

<!-- IMG List -->

[newwebapp]: ./media/web-sites-java-get-started/newwebapp.png
[newwebapp2]: ./media/web-sites-java-get-started/newwebapp2.png
[selectwebapp]: ./media/web-sites-java-get-started/selectwebapp.png
[versions]: ./media/web-sites-java-get-started/versions.png
[newmarketplace]: ./media/web-sites-java-get-started/newmarketplace.png
[webmobilejetty]: ./media/web-sites-java-get-started/webmobilejetty.png
[jettyblade]: ./media/web-sites-java-get-started/jettyblade.png
[jettyportalcreate2]: ./media/web-sites-java-get-started/jettyportalcreate2.png
[jettyurl]: ./media/web-sites-java-get-started/jettyurl.png
[tomcat]: ./media/web-sites-java-get-started/tomcat.png
[jetty]: ./media/web-sites-java-get-started/jetty.png
