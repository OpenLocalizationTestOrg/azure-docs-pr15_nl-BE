<properties
    pageTitle="Gebruikersverificatie voor Apps in Azure App Service API | Microsoft Azure"
    description="Informatie over het beveiligen van een API-app in de App-Service Azure doordat access alleen aan geverifieerde gebruikers."
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
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="rachelap"/>

# <a name="user-authentication-for-api-apps-in-azure-app-service"></a>Gebruikersverificatie voor Apps in Azure App Service API

## <a name="overview"></a>Overzicht

In dit artikel ziet u hoe een Azure API app beveiligen zodat alleen geverifieerde gebruikers kunnen aanroepen. Het artikel wordt ervan uitgegaan dat u de [Azure App Service verificatie-overzicht](../app-service/app-service-authentication-overview.md)hebt gelezen.

U leert:

* Het configureren van een verificatieprovider met details voor Azure Active Directory (AD Azure).
* Hoe u een beveiligde API app verbruiken met behulp van de [Active Directory verificatie bibliotheek (ADAL) voor JavaScript](https://github.com/AzureAD/azure-activedirectory-library-for-js).

Het artikel bevat twee secties:

* De sectie voor [het configureren van gebruikersverificatie in Azure App-Service](#authconfig) in het algemeen wordt uitgelegd hoe u gebruikersverificatie configureren voor elke toepassing API en ook geldt voor alle kaders die worden ondersteund door de App Service, met inbegrip van .NET, Node.js en Java.

* Beginnen met de sectie [door de zelfstudies .NET API Apps te gaan](#tutorialstart) , de hulplijnen artikel bij het configureren van een voorbeeldtoepassing met .NET back-end en een AngularJS voorkant beëindigen zodat Azure Active Directory wordt gebruikt voor de verificatie. 

## <a id="authconfig"></a>Gebruikersverificatie configureren in Azure App Service

Deze sectie bevat algemene instructies die voor alle API-app gelden. Voor specifieke stappen om de voorbeeldtoepassing te doen lijst-.NET, gaat u naar [de zelfstudies .NET introductie door te gaan](#tutorialstart).

1. In de [Azure portal](https://portal.azure.com/), gaat u naar het blad van de **Instellingen** van de app API die u wilt beschermen, **onderdelen** zoeken en klik vervolgens op **Authentication / Authorization**.

    ![Azure portal verificatie](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. In de **Authentication / Authorization** blade, klikt u **op**.

4. Selecteer een van de opties in de lijst **actie moet worden ondernomen wanneer de aanvraag niet is geverifieerd** .

    * Als u wilt dat alleen geverifieerde oproepen tot uw app API, kies een van de opties **... meldt u zich aan met** . Met deze optie kunt u de API-app zonder het schrijven van code die wordt uitgevoerd in het beschermen.

    * Als u wilt dat alle aanroepen voor het bereiken van uw app API, kies **aanvraag toestaan (geen actie)**. Met deze optie kunt u direct een keuze uit verificatieproviders niet-geverifieerde bellers. Met deze optie hebt u voor het schrijven van code voor het verwerken van de vergunning.

    Zie [verificatie en machtiging van Apps in Azure App Service API](app-service-api-authentication.md#multiple-protection-options)voor meer informatie.

5. Selecteer een of meer van de **Verificatieproviders**.

    De afbeelding toont keuzes waarbij alle Bellers kunnen worden geverifieerd door AD Azure.
 
    ![Azure portal verificatie blade](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

    Als u een verificatieprovider kiest, wordt de portal een blade configuratie voor die provider. 

    Zie voor gedetailleerde instructies over het configureren van de verificatie-provider configuratie blades, [het configureren van uw App-servicetoepassing Azure Active Directory-aanmelding gebruiken](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). (De link gaat naar een artikel over Azure AD, maar het artikel zelf bevat tabbladen die zijn gekoppeld aan artikelen voor de verificatieproviders.)

7. Wanneer u met het blad configuratie van verificatie-provider bent klaar, klikt u op **OK**.

7. In de **Authentication / Authorization** blade, klikt u op **Opslaan**.

Wanneer dit gebeurt, verifieert App-Service alle API-aanroepen voordat ze de API app bereiken. De verificatieservices werken hetzelfde voor alle talen die App-service ondersteunt, met inbegrip van .NET, Node.js en Java. 

Als u geverifieerde API-aanroepen, bevat de beller de verificatieprovider OAuth 2.0 aan toonder token in de autorisatie-header van HTTP-aanvragen. Het token kan worden verkregen met behulp van de verificatieprovider SDK.

## <a id="tutorialstart"></a>De zelfstudies .NET API Apps u doorgaat

Als u de Node.js of Java-cursussen voor API apps, gaat u naar het volgende artikel in de [service principal verificatie voor API apps](app-service-api-dotnet-service-principal-auth.md). 

Als u reeksen .NET zelfstudie voor API apps volgen en al de voorbeeldtoepassing hebt geïmplementeerd als gericht in de [eerste](app-service-api-dotnet-get-started.md) en [tweede](app-service-api-cors-consume-javascript.md) zelfstudies, gaat u verder met de sectie [verificatie en Azure AD App instellen](#azureauth) .

Als u deze zelfstudie volgen zonder tussenkomst van de eerste en tweede zelfstudies, doen de volgende stappen waarin wordt uitgelegd hoe u aan de slag met behulp van een geautomatiseerd proces voor de implementatie van de voorbeeldtoepassing.

>[AZURE.NOTE] De volgende stappen kunnen u op hetzelfde beginpunt als u de eerste twee zelfstudies, met één uitzondering hebt: Visual Studio al weet niet welke web app of API-app die op elk project wordt geïmplementeerd. Dit betekent dat u geen precieze instructies in deze zelfstudie waarin wordt uitgelegd hoe u op de juiste doelen implementeren. Als u niet vertrouwd bent met hoe de implementatiestappen zelf uitvoeren, is het beter om de zelfstudie reeks van de [eerste zelfstudie](app-service-api-dotnet-get-started.md) dan om te beginnen met dit proces geautomatiseerd volgen.

1. Zorg dat u alle vereisten zoals vermeld in de [eerste zelfstudie](app-service-api-dotnet-get-started.md). Naast de vereisten die zijn vermeld, deze zelfstudies verificatie wordt ervan uitgegaan dat u hebt gewerkt met App Service web apps en apps in Visual Studio en de Azure portal API.

2. Klik op **Deploy naar Azure** in de [takenlijst monster opslagplaats Leesmij-bestand](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/readme.md) voor de implementatie van de API-toepassingen en de web app. Maak een notitie van de Azure resourcegroep die wordt gemaakt, zoals u deze kunt later web app en API app namen opzoeken.
 
3. Downloaden of kopiëren van de [takenlijst monster opslagplaats](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) als u de code waarin u met lokaal in Visual Studio werkt.

## <a id="azureauth"></a>Verificatie en Azure AD App instellen

U hebt nu de toepassing in Azure App-Service wordt uitgevoerd zonder dat de gebruikers worden geverifieerd. In deze sectie kunt u verificatie toevoegen door de volgende taken uitvoeren:

* Azure Active Directory (AD Azure) is verificatie vereist voor het aanroepen van de API van de middelste laag app App Service configureren.
* Azure AD-toepassing maken.
* De Azure AD toepassing configureren voor het token aan toonder na aanmelding sturen naar de front-end AngularJS. 

Als u op problemen stuit bij de zelfstudie aanwijzingen, Zie de sectie [problemen oplossen](#troubleshooting) aan het einde van de zelfstudie. 
 
### <a name="configure-authentication-for-the-middle-tier-api-app"></a>Verificatie configureren voor de middelste laag API app

1. In [Azure portal](https://portal.azure.com/), gaat u naar het blad van de **Instellingen** van de API-app die u hebt gemaakt voor het project ToDoListAPI, de **functies** sectie vinden en klik vervolgens op **Authentication / Authorization**.

    ![Azure portal verificatie](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. In de **Authentication / Authorization** blade, klikt u **op**.

4. Selecteer **aanmelden met Azure Active Directory**in de vervolgkeuzelijst **actie moet worden ondernomen wanneer de aanvraag niet is geverifieerd** .

    Deze optie zorgt ervoor dat er geen aanvragen voor unauathenticated de API app zullen bereiken. 

5. Klik onder **Verificatieproviders** **Azure Active Directory**.

    ![Azure portal verificatie blade](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. Klik in de blade **Azure Active Directory-instellingen** op **Express**

    ![Azure verificatie Express optie portal](./media/app-service-api-dotnet-user-principal-auth/aadsettings.png)

    Met de optie **Express** maken App Service automatisch een Azure AD-toepassing in uw AD Azure [huurder](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 

    U hoeft te maken van een huurder, omdat elke account Azure automatisch een heeft.

7. Onder **Management-modus**, klikt u op **Nieuwe AD App maken** als dit niet is gebeurd en Let op de waarde in het tekstvak **App maken** . u zult deze AAD toepassing hoger in de klassieke Azure portal opzoeken.

    ![Instellingen voor Azure portal Azure AD](./media/app-service-api-dotnet-user-principal-auth/aadsettings2.png)

    Azure maakt automatisch een Azure AD-toepassing met de aangegeven naam in uw huurder Azure AD. Standaard de Azure AD-toepassing met de naam hetzelfde als de API-app. Als u wilt, kunt u een andere naam invoeren.
 
7. Klik op **OK**.

7. In de **Authentication / Authorization** blade, klikt u op **Opslaan**.

    ![Klik op Opslaan](./media/app-service-api-dotnet-user-principal-auth/authsave.png)

Alleen gebruikers in uw huurder Azure AD kunnen nu de app API aanroepen.

### <a name="optional-test-the-api-app"></a>Optioneel: Het testen van de API-app

1. Ga naar de URL van de API-app in een browser: in de blade **API app** in de portal Azure, klikt u op de koppeling onder **URL**.  

    U wordt omgeleid naar een scherm voor aanmelding omdat niet-geverifieerde aanvragen zijn niet toegestaan de API app bereiken.

    Als uw browser gaat naar de pagina "is gemaakt", de browser mogelijk al aangemeld op--in dat geval een InPrivate of Incognito-venster open en Ga naar de URL van de API-app.

2. Meld u aan met de referenties voor een gebruiker in uw huurder Azure AD.

    Wanneer u bent aangemeld, verschijnt de pagina 'is gemaakt' in de browser.

9. Sluit de browser.

### <a name="configure-the-azure-ad-application"></a>De Azure AD-toepassing configureren

Tijdens het configureren van verificatie Azure AD App Service gemaakt een Azure AD-toepassing. Toepassing is standaard ingesteld op de nieuwe Azure AD geconfigureerd voor het token aan toonder aan de API-app URL. In deze sectie kunt u de Azure AD-toepassing waarmee het token aan toonder voor de AngularJS-front-end in plaats van rechtstreeks naar de middelste laag API app configureren. De front-end AngularJS stuurt de token API App wanneer de API-app roept.

>[AZURE.NOTE] Het proces bewaren als eenvoudig mogelijk, deze zelfstudie wordt een enkele Azure AD trapsgewijs toepassing voor de front-end en de middelste API app. Een andere optie is met twee Azure AD-toepassingen. U moet in dat geval machtigt de front-end Azure AD toepassing de middenlaag Azure AD toepassing aanroepen. Deze aanpak van meerdere toepassingen, krijgt u meer gedetailleerde controle over de machtigingen voor elke laag.

11. Ga in de [Azure klassieke portal](https://manage.windowsazure.com/)naar **Azure Active Directory**.

    U hebt de klassieke portal omdat bepaalde Azure Active Directory-instellingen die u toegang tot nog niet beschikbaar in de huidige Azure portal zijn.

12. Klik op het tabblad **map** op uw huurder AAD.

    ![Azure AD in klassieke portal](./media/app-service-api-dotnet-user-principal-auth/selecttenant.png)

14. Klik op **toepassingen > toepassingen mijn bedrijf eigenaar is van**, en klik vervolgens op het selectievakje is ingeschakeld.

    Mogelijk moet u ook Vernieuw de pagina om te zien de nieuwe toepassing.

15. Klik op de naam van de Azure voor u gemaakt wanneer u verificatie voor uw app API ingeschakeld in de lijst met toepassingen.

    ![Azure AD toepassingen-tabblad](./media/app-service-api-dotnet-user-principal-auth/appstab.png)

16. Klik op **configureren**.

    ![Azure configureren voor AD-tabblad](./media/app-service-api-dotnet-user-principal-auth/configure.png)

17. **Aanmelding URL** ingesteld op de URL voor uw AngularJS web app geen afsluitende slash.

    Bijvoorbeeld: https://todolistangular.azurewebsites.net

    ![URL aanmelding](./media/app-service-api-dotnet-user-principal-auth/signonurlazure.png)

17. **Antwoord URL** ingesteld op de URL voor uw web app, geen afsluitende slash.

    Bijvoorbeeld: https://todolistsangular.azurewebsites.net

16. Klik op **Opslaan**.

    ![Antwoord-URL](./media/app-service-api-dotnet-user-principal-auth/replyurlazure.png)

15. Klik onderaan de pagina op **manifest beheren > Download manifest**.

    ![Manifest downloaden](./media/app-service-api-dotnet-user-principal-auth/downloadmanifest.png)

17. Download het bestand naar een locatie waar u het kunt bewerken.

16. Zoeken in het manifestbestand gedownload naar de `oauth2AllowImplicitFlow` eigenschap. Wijzig de waarde van deze eigenschap van de `false` te `true`, en sla het bestand.

    Deze instelling is vereist voor toegang vanuit een JavaScript-toepassing voor één pagina. U kunt het token Oauth 2.0 aan toonder worden geretourneerd in de URL-fragment.

16. Klik op **manifest beheren > Upload manifest**, en het uploaden van het bestand dat u in de vorige stap hebt bijgewerkt.

    ![Manifest uploaden](./media/app-service-api-dotnet-user-principal-auth/uploadmanifest.png)

17. De **Client-ID** -waarde kopiëren en ergens kun je het later opslaan.

## <a name="configure-the-todolistangular-project-to-use-authentication"></a>Het project ToDoListAngular voor verificatie configureren

In deze sectie wijzigen de front-end AngularJS zodat Active Directory verificatie bibliotheek (ADAL) voor JS ter verwerving van een token aan toonder voor de aangemelde gebruiker uit Azure Active Directory wordt gebruikt. De code wordt het token opnemen in HTTP-aanvragen die zijn verzonden naar de middelste laag, zoals in het volgende diagram. 

![Diagram van de verificatie gebruiker](./media/app-service-api-dotnet-user-principal-auth/appdiagram.png)

De volgende wijzigingen aanbrengen in de bestanden in het project ToDoListAngular.

1. Open het bestand *index.html* .

2. Opmerkingen bij de regels die verwijzen naar de actieve Directory verificatie bibliotheek (ADAL) voor JS-scripts.

        <script src="app/scripts/adal.js"></script>
        <script src="app/scripts/adal-angular.js"></script>

1. Open het bestand *app/scripts/app.js* .

2. Reactie op het blok met code is gemarkeerd voor 'zonder verificatie' en verwijder de opmerkingen van het blok met code is gemarkeerd voor 'met authentication'.

    Deze wijziging verwijst naar de verificatieprovider ADAL JS en waarden aan deze configuratie. In de volgende stappen kunt u de configuratiewaarden voor uw app API en Azure AD-toepassing instellen.

8. In de code die de `endpoints` variabele, de API-URL instellen naar de URL van de API-app gemaakt voor het ToDoListAPI-project en de Azure AD toepassings-ID instellen op de client-ID die u hebt gekopieerd uit de klassieke Azure portal.

    De code is nu zoals in het volgende voorbeeld.

        var endpoints = {
            "https://todolistapi0121.azurewebsites.net/": "1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3"
        };

9. In de aanroep naar `adalProvider.init`stelt `tenant` op de naam van de huurder en `clientId` dezelfde waarde die u in de vorige stap hebt gebruikt.

    De code is nu zoals in het volgende voorbeeld.

        adalProvider.init(
            {
                instance: 'https://login.microsoftonline.com/', 
                tenant: 'contoso.onmicrosoft.com',
                clientId: '1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3',
                extraQueryParameter: 'nux=1',
                endpoints: endpoints
            },
            $httpProvider
            );

    Deze wijzigingen in `app.js` opgeven dat de aanroepende code en de API genoemd in dezelfde toepassing AD Azure zijn.

1. Open het bestand *app/scripts/homeCtrl.js* .

2. Reactie op het blok met code is gemarkeerd voor 'zonder verificatie' en verwijder de opmerkingen van het blok met code is gemarkeerd voor 'met authentication'.

1. Open het bestand *app/scripts/indexCtrl.js* .

2. Reactie op het blok met code is gemarkeerd voor 'zonder verificatie' en verwijder de opmerkingen van het blok met code is gemarkeerd voor 'met authentication'.

### <a name="deploy-the-todolistangular-project-to-azure"></a>Het ToDoListAngular-project op Azure implementeren

8. In de **Solution Explorer**met de rechtermuisknop op het ToDoListAngular-project en klik vervolgens op **publiceren**.

9. Klik op **publiceren**.

    Visual Studio implementeert u het project en opent u een browser de basis-URL van de web-app. Hier ziet een 403-fout-pagina een poging is om een basis-URL voor Web-API vanuit een browser naar normaal.

    U hebt nog een wijziging aanbrengt in de middenlaag API app voordat u de toepassing kunt testen.

10. Sluit de browser.

## <a name="configure-the-todolistapi-project-to-use-authentication"></a>Het project ToDoListAPI voor verificatie configureren

Op dit moment het ToDoListAPI-project wordt verzonden "*" Als de `owner` ToDoListDataAPI waarde. In dit gedeelte wijzigt u de code voor het verzenden van de ID van de gebruiker is aangemeld.

De volgende wijzigingen aanbrengen in het project ToDoListAPI.

1. Open het bestand *Controllers/ToDoListController.cs* en verwijder de opmerking van de regel in elke actie-methode die `owner` naar de Azure AD `NameIdentifier` waarde van claim. Bijvoorbeeld:

        owner = ((ClaimsIdentity)User.Identity).FindFirst(ClaimTypes.NameIdentifier).Value;

    **Belangrijk**: geen opmerkingen bij de code in de `ToDoListDataAPI` methode; u zult doen later voor de UPN-verificatie-zelfstudie.

### <a name="deploy-the-todolistapi-project-to-azure"></a>Het ToDoListAPI-project op Azure implementeren

8. In de **Solution Explorer**met de rechtermuisknop op het ToDoListAPI-project en klik vervolgens op **publiceren**.

9. Klik op **publiceren**.

    Visual Studio implementeert u het project en opent u een browser de basis-URL van de API-app.

10. Sluit de browser.

### <a name="test-the-application"></a>De toepassing testen

9. Ga naar de URL van de web-app **met behulp van HTTPS niet HTTP**.

8. Klik op de tab van de **Takenlijst** .

    U wordt gevraagd aan te melden.

9. Meld u aan met de referenties van een gebruiker in uw huurder AAD.

10. De pagina van de **Takenlijst** wordt weergegeven.

    ![Takenlijst pagina](./media/app-service-api-dotnet-user-principal-auth/webappindex.png)

    Geen taakitems worden weergegeven omdat tot nu alle eigenaar zijn "*". Nu vraagt de middenlaag artikelen voor de gebruiker is aangemeld en zijn geen nog gemaakt.

11. Voeg nieuwe taakitems om te controleren of de toepassing werkt.

12. Ga naar de UI Swagger URL voor de ToDoListDataAPI API app in een andere browservenster en klik op **ToDoList > Get**. Geef een sterretje voor de `owner` parameter en klik vervolgens op **uit te proberen**.

    Het antwoord ziet u dat de nieuwe taken de werkelijke hebben Azure AD gebruikers-ID in de eigenschap eigenaar.

    ![Eigenaar-ID JSON-reactie](./media/app-service-api-dotnet-user-principal-auth/todolistapiauth.png)


## <a name="building-the-projects-from-scratch"></a>De projecten zelf bouwen

De twee Web API-projecten zijn met behulp van de projectsjabloon **Azure API App** en de standaard waarden controller vervangen door een takenlijst van domeincontroller gemaakt. 

Zie voor meer informatie over het maken van een enkele pagina AngularJS toepassing met een Web API 2 back-end [handen op Lab: bouwen van een enkele pagina toepassing (SPA) met ASP.NET Web API en Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Zie de volgende bronnen voor meer informatie over het toevoegen van Azure AD verificatiecode:

* [AngularJS één pagina Apps met Azure Active Directory beveiligen](../active-directory/active-directory-devquickstarts-angular.md).
* [Introductie van ADAL JS v1](http://www.cloudidentity.com/blog/2015/02/19/introducing-adal-js-v1/)

## <a name="troubleshooting"></a>Het oplossen van problemen

[AZURE.INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Zorg ervoor dat u is niet als ToDoListAPI (middelste laag) en ToDoListDataAPI (data laag hetzelfde). Controleer bijvoorbeeld of u verificatie toegevoegd aan de middenlaag API app, niet de gegevenslaag. 
* Zorg ervoor dat de AngularJS code verwijst naar de middelste laag API app URL (ToDoListAPI, niet ToDoListDataAPI) en de juiste Azure AD client-ID. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe App Service om verificatie te gebruiken voor een API-app en hoe de app API aanroept met behulp van de bibliotheek ADAL JS. In de volgende zelfstudie leert u hoe u [beveiligde toegang tot uw app API voor Services scenario's](app-service-api-dotnet-service-principal-auth.md).

