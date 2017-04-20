<properties
    pageTitle="Azure AD v2.0 AngularJS aan de slag | Microsoft Azure"
    description="Het bouwen van een app hoek JS één pagina die gebruikers met zowel persoonlijke Microsoft-account aanmelden en rekeningen voor werk of school."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="add-sign-in-to-an-angularjs-single-page-app---net"></a>Aanmelden toevoegen aan een AngularJS één pagina app - .NET

In dit artikel gaan we Meld u aan met aangedreven Microsoft-accounts toevoegen aan een AngularJS app met behulp van het eindpunt van de v2.0 Azure Active Directory.  Het eindpunt v2.0 kunt u het uitvoeren van een enkele integratie in uw app en verificatie van gebruikers met een account voor zowel persoonlijke als werk/school.

In dit voorbeeld wordt een eenvoudige takenlijst één pagina app waarmee taken worden opgeslagen in een back-end REST API, geschreven met .NET 4.5 MVC framework en beveiligd met OAuth aan toonder tokens uit Azure Active Directory.  De app AngularJS gebruikt onze open-source JavaScript verificatie bibliotheek [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) de gehele aanmelden proces verwerken en het verkrijgen van tokens voor de REST API aanroept.  Hetzelfde patroon kan worden toegepast voor het verifiëren van andere REST-API's, zoals de [Microsoft Graph](https://graph.microsoft.com).

> [AZURE.NOTE]
    Niet alle scenario's Azure Active Directory en functies worden ondersteund door het eindpunt v2.0.  Om te bepalen als u het eindpunt v2.0 moet gebruiken, Lees over [v2.0 beperkingen](active-directory-v2-limitations.md).

## <a name="download"></a>Downloaden

Om te beginnen, moet u downloaden en installeren van Visual Studio.  Vervolgens kunt u een kloon of [download](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) een skelet app:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet.git
```

De overgebleven app omvat de generieke code voor een eenvoudige AngularJS app, maar alle van de stukken betrekking hebben op identiteit ontbreekt.  Als u niet volgen wilt, kunt u in plaats daarvan klonen of [download](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/complete.zip) het voltooide monster.

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-DotNet.git
```

## <a name="register-an-app"></a>Registreren van een app

Eerst maakt u een app in de [App-Portal registratie](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)of Volg deze [gedetailleerde stappen](active-directory-v2-app-registration.md).  Zorg ervoor dat:

- Het **Web** platform voor uw app toevoegen.
- Voer de juiste **Redirect URI**. De standaardwaarde voor dit voorbeeld is `https://localhost:44326/`.
- Laat het selectievakje **Impliciete Flow toestaan** is ingeschakeld. 

Kopiëren naar beneden de **Toepassings-ID** die is toegewezen aan uw app, moet u dit binnenkort. 

## <a name="install-adaljs"></a>Adal.js installeren
Om te beginnen, Ga project u gedownload en adal.js installeren.  Als u [bower](http://bower.io/) geïnstalleerd hebt, kunt u deze opdracht alleen uitvoeren.  Voor eventuele problemen afhankelijkheid versie, kiest u een nieuwere versie.
```
bower install adal-angular#experimental
```

U kunt ook handmatig downloaden [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) en [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).  Beide bestanden toevoegt aan de `app/lib/adal-angular-experimental/dist` directory van de `TodoSPA` project.

Nu opent u het project in Visual Studio en adal.js aan het einde van de hoofdtekst van de hoofdpagina te laden:

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## <a name="set-up-the-rest-api"></a>De REST API instellen

Terwijl we nog dingen, krijgen we de REST API backend werken.  Open in de hoofdmap van het project `web.config` en vervangt u de `audience` waarde.  De REST API gebruikt deze waarde om tokens die worden ontvangen van de hoek app op AJAX-aanvragen te valideren.

```xml
<!--web.config-->

...

    <appSettings>
        <add key="ida:Audience" value="[Your-application-id]" />
    </appSettings>
    
...
```

Dat is altijd gaan we besteden aan het bespreken van de werking van de REST API.  Gerust rond in de code maak, maar als u weten wilt over de web-API's met Azure AD, controleert u [in dit artikel](active-directory-v2-devquickstarts-dotnet-api.md). 

## <a name="sign-users-in"></a>Aanmelding van gebruikers in
Tijd om sommige identiteitscode te schrijven.  Hebt misschien u al opgemerkt dat adal.js bevat een AngularJS-provider netjes met hoek routering mechanismen speelt.  Starten met de module adal aan de app toe te voegen:

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

U kunt nu initialiseren het `adalProvider` met de toepassings-ID:

```js
// app/scripts/app.js

...

adalProvider.init({
        
        // Use this value for the public instance of Azure AD
        instance: 'https://login.microsoftonline.com/', 
        
        // The 'common' endpoint is used for multi-tenant applications like this one
        tenant: 'common',
        
        // Your application id from the registration portal
        clientId: '<Your-application-id>',
        
        // If you're using IE, uncommment this line - the default HTML5 sessionStorage does not work for localhost.
        //cacheLocation: 'localStorage',
         
    }, $httpProvider);
```

Geweldig adal.js heeft nu alle informatie die nodig is voor het beveiligen van uw app en gebruikers aanmelden.  Als u voor een bepaalde route in de app inloggen, is een kwestie van één regel code:

```js
// app/scripts/app.js

...

}).when("/TodoList", {
    controller: "todoListCtrl",
    templateUrl: "/static/views/TodoList.html",
    requireADLogin: true, // Ensures that the user must be logged in to access the route
})

...
```

Nu wanneer een gebruiker klikt op de `TodoList` koppeling, adal.js worden automatisch omgeleid naar Azure AD voor aanmelding bij indien nodig.  U kunt ook expliciet aanmeldings- en afmeldingstijden aanvragen verzenden door het aanroepen van adal.js in uw domeincontrollers:

```js
// app/scripts/homeCtrl.js

angular.module('todoApp')
// Load adal.js the same way for use in controllers and views   
.controller('homeCtrl', ['$scope', 'adalAuthenticationService','$location', function ($scope, adalService, $location) {
    $scope.login = function () {
        
        // Redirect the user to sign in
        adalService.login();
        
    };
    $scope.logout = function () {
        
        // Redirect the user to log out    
        adalService.logOut();
    
    };
...
```

## <a name="display-user-info"></a>Gebruikersgegevens weergeven
Nu dat de gebruiker is aangemeld, moet u waarschijnlijk ondertekend in van de gebruiker verificatie gegevens in uw toepassing.  Adal.js stelt deze informatie voor u in de `userInfo` object.  Als u dit object in een weergave, moet u eerst adal.js toevoegen aan de scope van de hoofdmap van de bijbehorende controller:

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

En u kunt rechtstreeks verband met de `userInfo` object in uw weergave: 

```html
<!--app/views/UserData.html-->

...

    <!--Get the user's profile information from the ADAL userInfo object-->
    <tr ng-repeat="(key, value) in userInfo.profile">
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
...
```

U kunt ook de `userInfo` object om te bepalen als de gebruiker is aangemeld of niet.

```html
<!--index.html-->

...

    <!--Use the ADAL userInfo object to show the right login/logout button-->
    <ul class="nav navbar-nav navbar-right">
        <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
        <li><a class="btn btn-link" ng-hide="userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    </ul>
...
```

## <a name="call-the-rest-api"></a>De REST API aanroepen
Ten slotte is het tijd om sommige tokens krijgen en roept de REST API om te maken, lezen, bijwerken en verwijderen van taken.  Ook goed nieuws!  U hoeft te doen *een ding*.  Adal.js wordt automatisch gedaan krijgen, caching en vernieuwen van tokens.  Het zal ook zorgen voor deze tokens toevoegen aan uitgaande AJAX-aanvragen die u naar de REST API verzendt.  

Hoe werkt dit? Het is allemaal dankzij de magie van [AngularJS interceptors](https://docs.angularjs.org/api/ng/service/$http), waarmee de adal.js om te zetten uitgaande en binnenkomende HTTP-berichten.  Bovendien adal.js wordt ervan uitgegaan dat alle aanvragen tot hetzelfde domein verzenden als het venster tokens die bestemd zijn voor dezelfde toepassing-ID als de app AngularJS moet gebruiken.  Dit is de reden waarom we dezelfde toepassings-ID in de hoek app en in de REST NodeJS API gebruikt.  Natuurlijk, u kunt dit gedrag wijzigen en zien adal.js voor tokens voor andere REST API zo nodig - maar dit eenvoudige scenario doet de standaardwaarden.

Hier is een fragment waarin wordt aangegeven hoe makkelijk het is voor het verzenden van aanvragen met tokens aan toonder uit Azure Active Directory:

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

Gefeliciteerd!  De geïntegreerde enkele pagina Azure AD app is nu voltooid.  Ga verder, strik neemt.  U kunt gebruikers verifiëren, veilig roept de REST API OpenID verbinding maken met back-end en elementaire informatie over de gebruiker.  Het ondersteunt elke gebruiker met een persoonlijke Microsoft-Account of een account werk/school van Azure AD uit het vak.  De toepassing uitvoeren en in een browser, Ga naar `https://localhost:44326/`.  Log in met behulp van een persoonlijke Microsoft-account of een account voor werk/school.  Taken toevoegen aan de takenlijst van de gebruiker en afmelden.  Probeer aan te melden met een ander type account. Als u een huurder Azure AD werk/school-gebruikers kunnen maken op [informatie over het verkrijgen van een hier](active-directory-howto-tenant.md) (het is gratis).

Om door te gaan met meer informatie over het eindpunt v2.0 terug head naar onze [v2.0-handleiding voor ontwikkelaars](active-directory-appmodel-v2-overview.md).  Aanvullende bronnen voor uitchecken:

- [Azure-monsters op GitHub >>](https://github.com/Azure-Samples)
- [Azure AD op Stack Overflow >>](http://stackoverflow.com/questions/tagged/azure-active-directory)
- Azure AD documentatie op [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)

## <a name="get-security-updates-for-our-products"></a>Beveiligingsupdates voor onze producten

We raden u meldingen wanneer beveiligingsincidenten plaatsvinden door [deze pagina](https://technet.microsoft.com/security/dd252948) te bezoeken en abonneren op waarschuwingen Raadgevend ophalen.
