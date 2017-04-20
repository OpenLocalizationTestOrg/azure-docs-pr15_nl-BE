<properties
    pageTitle="Azure AD AngularJS aan de slag | Microsoft Azure"
    description="Het bouwen van een hoek JS één pagina toepassing die is geïntegreerd met Azure AD voor aanmelden en roept Azure AD beschermd OAuth met API's."
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


# <a name="securing-angularjs-single-page-apps-with-azure-ad"></a>AngularJS één pagina Apps met Azure Active Directory beveiligen

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD maakt het eenvoudig en ongecompliceerd invoegtoepassing voor aanmelden, afmelden en OAuth API-aanroepen naar de apps pagina beveiligen.  U kunt hiermee uw app voor de verificatie van gebruikers met hun Active Directory-accounts en elk web API beschermd door Azure AD, zoals de Office 365-API of de API Azure verbruiken.

AD Azure biedt voor javascript-toepassingen in een browser worden uitgevoerd, de Active Directory-verificatie Library of adal.js.  Enig doel in het leven van Adal.js is het eenvoudig uw app toegangstokens ophalen.  U kunt zien hoe gemakkelijk het is, bouw hier we een toepassing AngularJS takenlijst die:

- De gebruiker aangemeld bij de app met Azure AD als de id-provider.
- Sommige informatie over de gebruiker weergegeven.
- Veilig roept van de app te doen lijst API aan toonder tokens van AAD gebruiken.
- De gebruiker van de app ondertekent.

Als u de volledige toepassing van de werken, moet u naar:

2. Uw toepassing met Azure Active Directory registreren.
3. ADAL installeren en configureren van de SPA.
5. ADAL gebruiken ter beveiliging van pagina's in de SPA.

Ophalen is gestart, [het skelet app downloaden](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) of [het voltooide monster](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).  U moet ook een huurder Azure AD kunt u gebruikers maken en registreren van een toepassing.  Als u een huurder, [Lees hoe u een](active-directory-howto-tenant.md)nog geen hebt.

## <a name="1-register-the-directorysearcher-application"></a>*1. de toepassing van het DirectorySearcher registreren*
Als u wilt dat uw app voor de verificatie van gebruikers en tokens ophalen, moet u eerst registreren in uw huurder Azure AD:

-   Aanmelden bij de [Azure Management Portal](https://manage.windowsazure.com)
-   Klik in de nav links op in **Active Directory**
-   Selecteer een huurder waarin om de toepassing te registreren.
-   Klik op het tabblad **toepassingen** en klik op **toevoegen** in de onderste lade.
-   Volg de aanwijzingen op het scherm en maak een nieuwe **webtoepassing en/of WebAPI**.
    -   De **naam** van de toepassing wordt uw toepassing om eindgebruikers te beschrijven.
    -   Het **Omleiden van Uri** is een locatie waarop AAD tokens wordt geretourneerd.  De standaardlocatie voor dit voorbeeld is`https://localhost:44326/`
-   Als u de registratie hebt voltooid, zal AAD toewijzen uw app een unieke **Client-ID**.  Moet u deze waarde in de volgende secties, dus kopiëren vanaf het tabblad **configureren** .
- De impliciete OAuth-stroom Adal.js gebruikt om te communiceren met Azure Active Directory.  U moet de impliciete stroom inschakelen voor uw toepassing door:
    - Manifest van de toepassing downloaden door te klikken op **Het Manifest beheren**.
    - Het manifest Open en Ga naar de `oauth2AllowImplicitFlow` eigenschap. De waarde instellen op `true`.
    - Opslaan en uploaden manifest van de toepassing door nogmaals op **Manifest beheren** te klikken.

## <a name="2-install-adal--configure-the-spa"></a>*2. ADAL installeren en configureren van de SPA*
Nu dat u een toepassing in Azure AD hebt, kunt u adal.js installeren en uw identiteit gerelateerde code te schrijven.

-   Gestart door adal.js toe te voegen aan het TodoSPA-project met de Package Manager-Console:
  - [Adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) downloaden en toe te voegen aan de `App/Scripts/` project directory.
  - [Adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) downloaden en toe te voegen aan de `App/Scripts/` project directory.
  - Laden van elk script vóór het einde van de `</body>` in `index.html`:

```js
...
<script src="App/Scripts/adal.js"></script>
<script src="App/Scripts/adal-angular.js"></script>
...
```

-   Van de SPA backend te doen lijst API accepteren tokens van de browser, moet de back-end configuratie-informatie over de registratie app. Open in het project TodoSPA `web.config`.  Vervang de waarden van de elementen in de `<appSettings>` sectie aangepast aan de waarden die u in de Portal Azure ingevoerd.  De code verwijst naar deze waarden wanneer ADAL wordt gebruikt.
    -   De `ida:Tenant` is het domein van de huurder Azure AD, bv. contoso.onmicrosoft.com
    -   De `ida:Audience` moet de **Client-ID** van de toepassing die u hebt gekopieerd vanaf de portal.

## <a name="3--use-adal-to-secure-pages-in-the-spa"></a>*3. Gebruik ADAL voor het beveiligen van de pagina's in de SPA*
Adal.js is gebouwd om te integreren met AngularJS route- en HTTP-providers waarmee u beveiligde afzonderlijke weergaven in uw SPA.

- In `App/Scripts/app.js`, in de module adal.js brengen:

```js
angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {
...
```
- U kunt nu initialiseren het `adalProvider` met de configuratiewaarden van uw registratie van toepassing, ook in `App/Scripts/app.js`:

```js
adalProvider.init(
  {
      instance: 'https://login.microsoftonline.com/',
      tenant: 'Enter your tenant name here e.g. contoso.onmicrosoft.com',
      clientId: 'Enter your client ID here e.g. e9a5a8b6-8af7-4719-9821-0deef255f68e',
      extraQueryParameter: 'nux=1',
      //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
  },
  $httpProvider
);
```
- Nu voor de beveiliging van de `TodoList` bekijken in de app, slechts één regel code nodig is - `requireADLogin`.

```js
...
}).when("/TodoList", {
        controller: "todoListCtrl",
        templateUrl: "/App/Views/TodoList.html",
        requireADLogin: true,
...
```

U hebt nu een beveiligde pagina toepassing kunnen gebruikers inloggen en token beveiligde aanvragen aan toonder uitgeven aan de backend-API.  Wanneer een gebruiker klikt op de `TodoList` koppeling, adal.js worden automatisch omgeleid naar Azure AD voor teken in indien nodig.  Bovendien adal.js automatisch aan te koppelen een access_token een ajax-aanvragen die worden verzonden naar de backend van de toepassing.  Het bovenstaande is minimaal noodzakelijk om te bouwen van een SPA met adal.js - bare maar er zijn een aantal andere functies die handig in kuuroorden zijn:

- Expliciet probleem aanmelden en afmelden aanvragen kunt u functies definiëren in uw domeincontrollers die adal.js aanroepen.  In `App/Scripts/homeCtrl.js`:

```js
...
$scope.login = function () {
    adalService.login();
};
$scope.logout = function () {
    adalService.logOut();
};
...
```
- U kunt ook om gebruikersgegevens te presenteren in de gebruikersinterface van de toepassing.  Het adal service is al toegevoegd aan de `userDataCtrl` -controller, zodat u toegang hebt tot het `userInfo` -object in de gekoppelde weergave `App/Views/UserData.html`:

```js
<p>{{userInfo.userName}}</p>
<p>aud:{{userInfo.profile.aud}}</p>
<p>iss:{{userInfo.profile.iss}}</p>
...
```

- Er zijn ook veel scenario's waarin u weten wilt als de gebruiker is aangemeld of niet.  U kunt ook de `userInfo` object om deze informatie te verzamelen.  Bijvoorbeeld, in `index.html` kunt u de "Aanmelden" of "Logout" knop op basis van verificatiestatus weergeven:

```js
<li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
<li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
```

Gefeliciteerd! Uw advertentie Azure geïntegreerd in één pagina App is nu voltooid.  U kunt gebruikers verifiëren, veilig roept de backend met OAuth 2.0 en elementaire informatie over de gebruiker.  Als u nog niet gedaan hebt, is nu de tijd voor het vullen van de huurder met sommige gebruikers.  De te doen lijst SPA uitgevoerd en meld u aan met een van deze gebruikers.  Taken toevoegen aan de gebruikers lijst, afmelden en opnieuw aanmelden.

Adal.js kunt u al deze gemeenschappelijke identiteit-functies opnemen in uw toepassing.  Dit zorgt voor het vuile werk voor u - Cachebeheer, ondersteuning OAuth-protocol, dat de gebruiker een login UI, het vernieuwen van verlopen tokens en nog veel meer.

Voor een verwijzing naar het ingevulde monster (zonder de waarden van de systeemconfiguratie) vindt u [hier](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).  U kunt nu op verplaatsen naar nog meer scenario's.  U kunt proberen:

[Een CORS Web API aanroepen vanuit een SPA >>](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
