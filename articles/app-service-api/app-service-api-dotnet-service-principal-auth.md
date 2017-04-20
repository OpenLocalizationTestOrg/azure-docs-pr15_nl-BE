<properties
    pageTitle="Service principal verificatie voor Apps in Azure App Service API | Microsoft Azure"
    description="Informatie over het beveiligen van een API-app in Azure App Service voor Services scenario's."
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

# <a name="service-principal-authentication-for-api-apps-in-azure-app-service"></a>Service principal verificatie voor Apps in Azure App Service API

## <a name="overview"></a>Overzicht

In dit artikel wordt uitgelegd hoe u App Service om verificatie te gebruiken voor *interne* toegang tot API apps. Een interne scenario is wanneer u een API-app die u wilt worden alleen door uw eigen toepassingscode verbruikbare. De aanbevolen manier voor het implementeren van dit scenario in App Service is Azure AD ter bescherming van de gebelde API app gebruiken. U kunt de beveiligde API app met een token aan toonder die u via Azure AD ontvangt door middel van toepassings-ID-referenties (service principal) aanroepen. Zie de sectie **Services verificatie** van de [Azure App Service verificatie-overzicht](../app-service/app-service-authentication-overview.md#service-to-service-authentication)voor alternatieven voor het gebruik van AD Azure.

In dit artikel leert u hoe:

* Het gebruik van Azure Active Directory (AD Azure) een API app beschermen tegen niet-geverifieerde toegang.
* Hoe u een beveiligde API app van een API-app, web app of mobiele app verbruiken met behulp van referenties voor Azure AD service principal (identiteit). Zie voor meer informatie over het gebruiken van een app logica [met uw aangepaste API op App Service met logica apps gehost](../app-service-logic/app-service-logic-custom-hosted-api.md).
* Hoe u ervoor zorgt dat de beschermde API app kan niet worden aangeroepen vanuit een browser door aangemelde gebruikers.
* Hoe om te controleren of de beveiligde API app kan alleen worden aangeroepen door een specifieke Azure AD service principal.

Het artikel bevat twee secties:

* De sectie [service principal verificatie in Azure App-Service configureren](#authconfig) in het algemeen wordt uitgelegd hoe u verificatie configureren voor elke toepassing API en hoe de beschermde API app in beslag neemt. Deze sectie geldt eveneens voor alle kaders die worden ondersteund door de App Service, met inbegrip van .NET, Node.js en Java.

* Vanaf de [voortgezette de zelfstudies .NET introductie](#tutorialstart) sectie is leidt de zelfstudie u door een scenario "interne toegang" voor een .NET-voorbeeldtoepassing in App-Service wordt uitgevoerd configureren. 

## <a id="authconfig"></a>Service principal verificatie in Azure App-Service configureren

Deze sectie bevat algemene instructies die voor alle API-app gelden. Voor specifieke stappen om de voorbeeldtoepassing te doen lijst-.NET, gaat u naar [de .NET API Apps zelfstudie reeks door te gaan](#tutorialstart).

1. In [Azure portal](https://portal.azure.com/), gaat u naar het blad van de **Instellingen** van de API app die u wilt beveiligen, en **onderdelen** zoeken en klik op **Authentication / Authorization**.

    ![Authentication/Authorization in Azure portal](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. In de **Authentication / Authorization** blade, klikt u **op**.

4. Selecteer **aanmelden met Azure Active Directory** in de vervolgkeuzelijst **actie moet worden ondernomen wanneer de aanvraag niet is geverifieerd** .

5. Selecteer onder **Verificatieproviders** **Azure Active Directory**.

    ![Blade in Azure portal verificatie](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. Configureer de blade **Azure Active Directory-instellingen** om een nieuwe te maken Azure AD toepassing of gebruik een bestaande Azure AD toepassing als u al hebt die u wilt gebruiken.

    Interne scenario's omvatten meestal een API app aanroepen van een API-app. U kunt afzonderlijke Azure AD toepassingen voor elke app API of één Azure AD-toepassing.

    Zie voor gedetailleerde instructies voor deze blade, [het configureren van uw App-servicetoepassing Azure Active Directory-aanmelding gebruiken](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

7. Wanneer u met het blad configuratie van verificatie-provider bent klaar, klikt u op **OK**.

7. In de **Authentication / Authorization** blade, klikt u op **Opslaan**.

    ![Klik op Opslaan](./media/app-service-api-dotnet-service-principal-auth/authsave.png)

Wanneer dit gebeurt, App-Service staat alleen aanvragen van bellers in de geconfigureerde Azure AD huurder. Geen code verificatie of machtiging is vereist in de beschermde API app. Het token aan toonder wordt doorgegeven aan de app API en vaak gebruikte gegevens in de HTTP-headers en vindt u die informatie in de code te valideren dat aanvragen afkomstig van een bepaalde beller, zoals een service principal zijn.

Deze verificatie-functionaliteit werkt op dezelfde wijze voor alle talen die App-service ondersteunt, met inbegrip van .NET, Node.js en Java. 

#### <a name="how-to-consume-the-protected-api-app"></a>Het verbruiken van de beschermde API app

De aanvrager moet bieden een token Azure AD aan toonder API-aanroepen. Als u een service principal referenties aan toonder-token, wordt de beller gebruikt Active Directory-verificatie Library (ADAL voor [.NET](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory), [Node.js](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs)of [Java](https://github.com/AzureAD/azure-activedirectory-library-for-java)). Als u een token, biedt de code ADAL roept ADAL de volgende informatie:

* De naam van de huurder Azure AD.
* De client-ID en het geheim van client (app key) van de Azure AD app die is gekoppeld aan de aanroepende functie.
* De client-ID van de Azure AD-toepassing die is gekoppeld aan de beveiligde API app. (Als u slechts één AD Azure-toepassing wordt gebruikt, dit is dezelfde client-ID als die van de aanroeper.)

Deze waarden zijn beschikbaar in de [portal voor Azure klassieke](https://manage.windowsazure.com/)Azure AD's.

Nadat het token is aangeschaft, bevat de beller het met HTTP-aanvragen in de koptekst van de vergunning.  App-Service het token valideert en kunnen de aanvragen tot de beschermde API app.

#### <a name="how-to-protect-the-api-app-from-access-by-users-in-the-same-tenant"></a>De API-app te beveiligen tegen toegang door gebruikers in de dezelfde huurder

Tokens voor gebruikers in de dezelfde huurder aan toonder worden beschouwd als geldig voor de beschermde API app.  Als u wilt om ervoor te zorgen dat alleen een service principal de beschermde API app kunt aanroepen, moet u code toevoegen in de beschermde API app voor het valideren van de volgende claims uit het token:

* `appid`moet de client-ID van de toepassing van Azure AD die is gekoppeld aan de aanroepende functie. 
* `oid`(`objectidentifier`) moet de principal-ID van de beller. 

App-Service biedt ook de `objectidentifier` in de header X-MS-CLIENT-PRINCIPAL-ID claimen.

### <a name="how-to-protect-the-api-app-from-browser-access"></a>Het beschermen van de API-app browser toegang

Als u vorderingen in de programmacode in de beschermde API app niet valideren en als u een afzonderlijke toepassing voor de beschermde API-app Azure AD zorg van de toepassing AD Azure antwoord URL is niet hetzelfde als de basis-URL van de API-app. Als het antwoord URL rechtstreeks naar de beveiligde API app verwijst, kan een gebruiker in de dezelfde Azure AD huurder API App bladeren, aanmelden en is de API aanroepen.

## <a id="tutorialstart"></a>De zelfstudie reeks .NET API Apps u doorgaat

Als u de zelfstudie reeks voor API apps Node.js of Java, gaat u verder met de sectie [volgende stappen](#next-steps) . 

De rest van dit artikel blijft de zelfstudie .NET API Apps-reeks en wordt ervan uitgegaan dat u de [gebruiker verificatie zelfstudie](app-service-api-dotnet-user-principal-auth.md) hebt voltooid en de voorbeeldtoepassing in Azure met gebruikersverificatie ingeschakeld.

## <a name="set-up-authentication-in-azure"></a>Azure-verificatie instellen

In deze sectie u App-Service zodanig configureren dat het alleen HTTP-verzoeken kunnen bereiken van de data tier API app zijn die met geldige Azure AD aan toonder tokens. 

In de volgende sectie configureert u de middelste laag API app toepassing referenties verzendt naar Azure AD, terughalen van een token aan toonder en de token aan toonder verzenden naar de data tier API app. Dit proces wordt geïllustreerd in het diagram.

![Service verificatie diagram](./media/app-service-api-dotnet-service-principal-auth/appdiagram.png)

Als u op problemen stuit bij de zelfstudie aanwijzingen, Zie de sectie [problemen oplossen](#troubleshooting) aan het einde van de zelfstudie. 

1. Ga naar het blad van de **Instellingen** van de API-app die u hebt gemaakt voor de ToDoListDataAPI (data laag) API app in [Azure portal](https://portal.azure.com/)en klik op **Instellingen**.

2. Zoek de sectie van de **functies** in de blade **Instellingen** en klik vervolgens op **Authentication / Authorization**.

    ![Authentication/Authorization in Azure portal](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. In de **Authentication / Authorization** blade, klikt u **op**.

4. Selecteer **aanmelden met Azure Active Directory**in de vervolgkeuzelijst **actie moet worden ondernomen wanneer de aanvraag niet is geverifieerd** .

    Dit is de instelling die ervoor zorgt dat de App-Service om ervoor te zorgen dat alleen aanvragen bereiken de API-app geverifieerde. Voor aanvragen waarvoor een geldige aan toonder tokens, App-Service geeft de tokens langs aan de API-app worden en HTTP-headers met vaak gebruikte claims die informatie aan uw code gemakkelijker beschikbaar te stellen.

5. Klik onder **Verificatieproviders** **Azure Active Directory**.

    ![Blade in Azure portal verificatie](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. Klik op **snel**in de blade **Azure Active Directory-instellingen** .

    Met de **Express** maken optie Azure automatisch een toepassing AAD in uw AD Azure [huurder](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 

    U hoeft te maken van een huurder, omdat elke account Azure automatisch een heeft.

7. **Modus voor beheer**, klik op **Nieuwe AD App maken** als dit niet is gebeurd.

    De portal wordt het invoervak **App maken** met een standaardwaarde. Standaard de Azure AD-toepassing met de naam hetzelfde als de API-app. Als u wilt, kunt u een andere naam invoeren.
    
    ![Azure AD-instellingen](./media/app-service-api-dotnet-service-principal-auth/aadsettings.png)

    **Opmerking**: als alternatief kunt u een enkele Azure AD toepassing voor zowel de aanroepende API-app en de beschermde API-app. Als u deze alternatieve kiest, moet u niet de optie **Nieuwe AD App maakt** hier omdat u een toepassing Azure AD al eerder in de handleiding van de verificatie gebruiker gemaakt. Voor deze zelfstudie kunt u afzonderlijke Azure AD-toepassingen voor de aanroepende API-app en de beschermde API app.

8. Maak een notitie van de waarde in het invoervak **App maken** ; u zult deze AAD toepassing hoger in de klassieke Azure portal opzoeken.

7. Klik op **OK**.

10. In de **Authentication / Authorization** blade, klikt u op **Opslaan**.

    ![Klik op Opslaan](./media/app-service-api-dotnet-service-principal-auth/saveauth.png)

    App-Service maakt een Azure Active Directory-toepassing met **URL aanmelding** en **Antwoord URL** automatisch ingesteld op de URL van uw app API. De laatste waarde kan gebruikers in uw huurder AAD aanmelden en toegang tot de API-app.

### <a name="verify-that-the-api-app-is-protected"></a>Controleer of de API app beveiligd is

1. Ga naar de URL van de API-app in een browser: in de blade **API app** in de portal Azure, klikt u op de koppeling onder **URL**. 

    U wordt omgeleid naar een scherm voor aanmelding omdat niet-geverifieerde aanvragen zijn niet toegestaan de API app bereiken. 

    Als uw browser gaat verder met de gebruikersinterface van Swagger, de browser mogelijk al aangemeld op--in dat geval een InPrivate of Incognito-venster open en Ga naar de URL Swagger UI.

18. Meld u aan met de referenties van een gebruiker in uw huurder AAD.

    Wanneer u bent aangemeld, verschijnt de pagina 'is gemaakt' in de browser.

## <a name="configure-the-todolistapi-project-to-acquire-and-send-the-azure-ad-token"></a>Het project ToDoListAPI te verkrijgen en het verzenden van het token Azure AD configureren

In deze sectie kunt u de volgende taken uitvoeren:

* Voeg code toe in de middenlaag API app dat Azure AD toepassing referenties gebruikt bij het aanschaffen van een token en verzenden met HTTP-aanvragen naar de data tier API app.
* De referenties die u moet worden opgehaald uit AD Azure.
* Voer de referenties in Azure App runtime omgevingsinstellingen in de middenlaag API app. 

### <a name="configure-the-todolistapi-project-to-acquire-and-send-the-azure-ad-token"></a>Het project ToDoListAPI te verkrijgen en het verzenden van het token Azure AD configureren

De volgende wijzigingen aanbrengen in het ToDoListAPI-project in Visual Studio.

1. Opmerkingen bij alle code in het bestand *ServicePrincipal.cs* .

    Dit is de code die gebruikmaakt van ADAL voor .NET de Azure AD aan toonder token verkrijgen.  Verschillende waarden die u later in de Azure runtime-omgeving instellen kunt wordt gebruikt. Dit is de code: 

        public static class ServicePrincipal
        {
            static string authority = ConfigurationManager.AppSettings["ida:Authority"];
            static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
            static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
            static string resource = ConfigurationManager.AppSettings["ida:Resource"];
        
            public static AuthenticationResult GetS2SAccessTokenForProdMSA()
            {
                return GetS2SAccessToken(authority, resource, clientId, clientSecret);
            }
        
            static AuthenticationResult GetS2SAccessToken(string authority, string resource, string clientId, string clientSecret)
            {
                var clientCredential = new ClientCredential(clientId, clientSecret);
                AuthenticationContext context = new AuthenticationContext(authority, false);
                AuthenticationResult authenticationResult = context.AcquireToken(
                    resource,
                    clientCredential);
                return authenticationResult;
            }
        }

    **Opmerking:** Deze code moet het ADAL voor .NET NuGet package (Microsoft.IdentityModel.Clients.ActiveDirectory), die al in het project is geïnstalleerd. Als u dit project helemaal maakt, moet u dit pakket te installeren. Dit pakket wordt niet automatisch geïnstalleerd door de API app nieuw project-sjabloon.

2. Opmerkingen bij de code in in *Controllers/ToDoListController*, de `NewDataAPIClient` methode waarmee het token worden toegevoegd aan de HTTP-aanvragen in de koptekst van de vergunning.

        client.HttpClient.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", ServicePrincipal.GetS2SAccessTokenForProdMSA().AccessToken);

3. Implementatie van het project ToDoListAPI. (Klik met de rechtermuisknop op het project en klik vervolgens op **publiceren > publiceren**.)

    Visual Studio implementeert u het project en opent u een browser de basis-URL van de web-app. Hier ziet een 403-fout-pagina een poging is om een basis-URL voor Web-API vanuit een browser naar normaal.

4. Sluit de browser.

### <a name="get-azure-ad-configuration-values"></a>Configuratiewaarden Azure AD ophalen

11. Ga in de [Azure klassieke portal](https://manage.windowsazure.com/)naar **Azure Active Directory**.

12. Klik op het tabblad **map** op uw huurder AAD.

14. Klik op **toepassingen > toepassingen mijn bedrijf eigenaar is van**, en klik vervolgens op het selectievakje is ingeschakeld.

15. Klik op de naam van degene die Azure voor u gemaakt wanneer u verificatie voor de app ToDoListDataAPI (data laag) API ingeschakeld in de lijst met toepassingen.

16. Klik op het tabblad **configureren** .

5. De **Client-ID** -waarde kopiëren en ergens kun je het later opslaan. 

8. Ga terug naar de lijst met **toepassingen die mijn bedrijf eigenaar is**in de klassieke Azure portal en op de AAD-toepassing die u hebt gemaakt voor de middelste laag ToDoListAPI API app (die u hebt gemaakt in de vorige zelfstudie, niet de tekst die u in deze zelfstudie hebt gemaakt).

16. Klik op het tabblad **configureren** .

5. De **Client-ID** -waarde kopiëren en ergens kun je het later opslaan.

6. Selecteer onder **toetsen**, **1 jaar** uit de vervolgkeuzelijst **Selecteer duur** .

6. Klik op **Opslaan**.

    ![App sleutel genereren](./media/app-service-api-dotnet-service-principal-auth/genkey.png)

7. Waarde van de sleutel te kopiëren en ergens kun je het later opslaan.

    ![Nieuwe app sleutel kopiëren](./media/app-service-api-dotnet-service-principal-auth/genkeycopy.png)

### <a name="configure-azure-ad-settings-in-the-middle-tier-api-apps-runtime-environment"></a>Azure AD-instellingen configureren in de middenlaag API-app runtime-omgeving

1. Ga naar de [portal Azure](https://portal.azure.com/)en navigeer vervolgens naar de blade **API App** voor de app API die fungeert als host voor het project TodoListAPI (middelste laag).

2. Klik op **Instellingen > Toepassingsinstellingen**.

3. In de sectie **Toepassingsinstellingen** toevoegen de volgende sleutels en waarden:

  	| **Sleutel** | IDA autoriteit: |
  	|---|---|
  	| **Waarde** | https://login.microsoftonline.com/ {uw naam Azure AD huurder} |
  	| **Voorbeeld** | https://login.microsoftonline.com/contoso.onmicrosoft.com |

  	| **Sleutel** | IDA ClientId: |
  	|---|---|
  	| **Waarde** | Client-ID van de aanroepende toepassing (middelste laag - ToDoListAPI) |
  	| **Voorbeeld** | 960adec2-b74a-484a-960adec2-b74a-484a |

  	| **Sleutel** | IDA: ClientSecret |
  	|---|---|
  	| **Waarde** | App-sleutel van de oproepende toepassing (middelste laag - ToDoListAPI) |
  	| **Voorbeeld** | e65e8fc9-5f6b-48e8-e65e8fc9-5f6b-48e8 |

  	| **Sleutel** | IDA Resource: |
  	|---|---|
  	| **Waarde** | Client-ID van de aangeroepen toepassing (data laag - ToDoListDataAPI) |
  	| **Voorbeeld** | e65e8fc9-5f6b-48e8-e65e8fc9-5f6b-48e8 |

    **Opmerking**: voor `ida:Resource`, moet u de naam van de toepassing **de client-ID** en niet de **App-ID-URI**.

    `ida:ClientId`en `ida:Resource` zijn verschillende waarden voor deze zelfstudie omdat u afzonderlijke Azure AD-applicaations voor de middelste laag en een gegevenslaag. Als u een enkele toepassing Azure AD voor de aanroepende API-app en de beschermde API-app, gebruikt u dezelfde waarde in beide `ida:ClientId` en `ida:Resource`.

    De ConfigurationManager wordt gebruikt om deze waarden, zodat ze worden opgeslagen in het Web.config-bestand van het project of de Azure runtime-omgeving. Terwijl een ASP.NET-toepassing in Azure App-Service wordt uitgevoerd, worden de instellingen van Web.config omgevingsinstellingen automatisch opheffen. Omgevingsinstellingen zijn meestal een [veiligere manier voor het opslaan van gevoelige informatie ten opzichte van een Web.config-bestand](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).

6. Klik op **Opslaan**.

    ![Klik op Opslaan](./media/app-service-api-dotnet-service-principal-auth/appsettings.png)

### <a name="test-the-application"></a>De toepassing testen

1. Ga naar de HTTPS-URL van de AngularJS-front-end web app in een browser.

2. Klik op de tab van de **Takenlijst** en log in met de referenties voor een gebruiker in uw huurder Azure AD. 

4. Voeg taken om te controleren of de toepassing werkt.

    ![Takenlijst pagina](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)

    Als de toepassing niet werkt zoals verwacht, Controleer alle instellingen die u hebt ingevoerd in de portal Azure. Als alle instellingen correct wordt weergegeven, raadpleegt u de sectie [problemen oplossen](#troubleshooting) verderop in deze handleiding.

## <a name="protect-the-api-app-from-browser-access"></a>De API app beschermen tegen toegang via de browser

Voor deze zelfstudie gemaakt u een afzonderlijke toepassing voor de ToDoListDataAPI (data laag) API app Azure AD. Zoals u hebt gezien, wanneer een toepassing AAD App-Service worden gemaakt, wordt de toepassing AAD geconfigureerd op een manier waarmee een gebruiker gaat naar de API-app URL in een browser en meld u aan. Dit betekent dat het is mogelijk voor een gebruiker in uw AD Azure huurder, niet alleen een service principal, toegang tot de API. 

Als u voorkomen dat toegang via de browser wilt zonder het schrijven van code in de beveiligde API app, kunt u het **Antwoord URL** in de toepassing AAD wijzigen zodat deze van de basis-URL van de API-app verschilt. 

### <a name="disable-browser-access"></a>Toegang via de browser uitschakelen

1. In de klassieke portal tabblad **configureren** voor de AAD-toepassing die is gemaakt voor de TodoListService, de waarde in het veld **URL antwoord** te wijzigen zodat deze een geldige URL, maar niet de URL van de API-app.
 
2. Klik op **Opslaan**.

### <a name="verify-browser-access-no-longer-works"></a>Controleer of de browser toegang werkt niet meer

Eerder u gecontroleerd dat u naar de URL van de app API vanuit een browser gaat door aan te melden met de referenties van een individuele gebruiker. In deze sectie controleert u dat dit niet meer mogelijk is. 

1. Ga opnieuw naar de URL van de API-app in een nieuw browservenster.

2. Meld u aan als u daarom wordt gevraagd.

3. Aanmelding is gelukt, maar leidt tot een foutpagina.

    U hebt de AAD app zodanig geconfigureerd dat gebruikers in de huurder AAD niet aanmelden en toegang de API vanuit een browser tot. U kunt nog steeds toegang tot de API-app met behulp van een service principal token, die u controleren kunt door te gaan naar de URL van de web-app en meer taken toe te voegen.

## <a name="restrict-access-to-a-particular-service-principal"></a>Toegang beperken tot een bepaalde service principal  

Rechts nu oproepers die een token kunt ophalen voor een gebruiker of service principal in uw huurder Azure AD de API TodoListDataAPI (data laag) app kunt aanroepen. U kunt controleren of de data tier API app accepteert alleen aanroepen vanuit de app TodoListAPI (middelste laag) API en alleen van een bepaalde service principal. 

U kunt deze beperkingen toevoegen door het toevoegen van code voor het valideren van de `appid` en `objectidentifier` vorderingen op binnenkomende gesprekken.

Voor deze zelfstudie kunt u de code die app-ID en service principal-ID direct in uw controller acties valideert plaatsen.  Alternatieven zijn voor het gebruik van een aangepaste `Authorize` kenmerk of doet deze validatie in het opstarten sequences (bv. OWIN middleware). Zie voor een voorbeeld van de laatste, [Deze voorbeeldtoepassing](https://github.com/mohitsriv/EasyAuthMultiTierSample/blob/master/MyDashDataAPI/Startup.cs). 

De volgende wijzigingen aanbrengen in het project TodoListDataAPI.

2. Open het bestand *Controllers/TodoListController.cs* .

3. Opmerkingen bij de regels die `trustedCallerClientId` en `trustedCallerServicePrincipalId`.

        private static string trustedCallerClientId = ConfigurationManager.AppSettings["todo:TrustedCallerClientId"];
        private static string trustedCallerServicePrincipalId = ConfigurationManager.AppSettings["todo:TrustedCallerServicePrincipalId"];

4. Opmerkingen bij de code in de methode CheckCallerId. Deze methode wordt aangeroepen bij het begin van elke methode actie in de controller. 

        private static void CheckCallerId()
        {
            string currentCallerClientId = ClaimsPrincipal.Current.FindFirst("appid").Value;
            string currentCallerServicePrincipalId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
            if (currentCallerClientId != trustedCallerClientId || currentCallerServicePrincipalId != trustedCallerServicePrincipalId)
            {
                throw new HttpResponseException(new HttpResponseMessage { StatusCode = HttpStatusCode.Unauthorized, ReasonPhrase = "The appID or service principal ID is not the expected value." });
            }
        }

5. Het ToDoListDataAPI project Azure App-Service opnieuw.

6. Ga naar de AngularJS-front-end web app HTTPS-URL in uw browser en klik op het tabblad **Takenlijst** de introductiepagina.

    De toepassing werkt niet omdat aanroepen naar de back-end mislukken. De nieuwe code controleert werkelijke appid en objectidentifier, maar niet nog de juiste waarden op en controleer of ze tegen. De browser Developer Tools Console meldt dat de server een HTTP 401-fout als resultaat.

    ![Fout in de Developer Tools Console](./media/app-service-api-dotnet-service-principal-auth/webapperror.png)

    In de volgende procedure configureert u de verwachte waarden.

8. Met Azure AD PowerShell, de waarde van de hoofdsom service krijgen voor de Azure AD-toepassing die u hebt gemaakt voor het project TodoListWebApp.

    een. Zie voor instructies voor het installeren van Azure PowerShell en verbinding maken met uw abonnement, [Met behulp van Azure PowerShell Azure Resource Manager](../powershell-azure-resource-manager.md).

    b. Als u een lijst met beveiligings-principals service, voeren de `Login-AzureRmAccount` opdracht en vervolgens de `Get-AzureRmADServicePrincipal` opdracht.

    c. De object-id voor service van de hoofdsom van de TodoListAPI toepassing vinden en deze opslaan op een locatie die u later van kopiëren kunt.

7. Ga naar de blade API app voor de app API die u voor het project ToDoListDataAPI geïmplementeerd in de portal voor Azure.

9. Klik op **Instellingen > Toepassingsinstellingen**.

3. In de sectie **Toepassingsinstellingen** toevoegen de volgende sleutels en waarden:

  	| **Sleutel** | TODO:TrustedCallerServicePrincipalId |
  	|---|---|
  	| **Waarde** | Service principal-id van de toepassing aanroepen |
  	| **Voorbeeld** | 4f4a94a4-6f0d-4072-4f4a94a4-6f0d-4072 |

  	| **Sleutel** | TODO:TrustedCallerClientId |
  	|---|---|
  	| **Waarde** | Client-ID van het aanroepen van toepassing - gekopieerd vanuit de toepassing Azure AD TodoListAPI |
  	| **Voorbeeld** | 960adec2-b74a-484a-960adec2-b74a-484a |

6. Klik op **Opslaan**.

    ![Klik op Opslaan](./media/app-service-api-dotnet-service-principal-auth/trustedcaller.png)

6. Terug naar de URL van de web-app in de browser en klik op het tabblad **Takenlijst** de introductiepagina.

    Deze keer de toepassing correct werkt omdat de app vertrouwde beller-ID en service principal-ID is de verwachte waarden.

    ![Takenlijst pagina](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)

## <a name="building-the-projects-from-scratch"></a>De projecten zelf bouwen

De twee Web API-projecten zijn met behulp van de projectsjabloon **Azure API App** en de standaard waarden controller vervangen door een takenlijst van domeincontroller gemaakt. Voor het verkrijgen van Azure AD service principal tokens in het ToDoListAPI-project, is de [Active Directory verificatie bibliotheek (ADAL) voor .NET](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) NuGet-pakket geïnstalleerd.
 
Zie voor meer informatie over het maken van een enkele pagina AngularJS toepassing met een back-end Web API zoals ToDoListAngular [handen op Lab: bouwen van een enkele pagina toepassing (SPA) met ASP.NET Web API en Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Zie voor meer informatie over het toevoegen van Azure AD verificatiecode [Beveiligen AngularJS één pagina Apps met Azure Active Directory](../active-directory/active-directory-devquickstarts-angular.md).

## <a name="troubleshooting"></a>Het oplossen van problemen

[AZURE.INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Zorg ervoor dat u is niet als ToDoListAPI (middelste laag) en ToDoListDataAPI (data laag hetzelfde). Bijvoorbeeld: in deze zelfstudie u toevoegen verificatie gegevens laag API App **maar de app-sleutel moet afkomstig zijn uit de Azure AD-toepassing die u hebt gemaakt voor de middelste laag API app**.

## <a name="next-steps"></a>Volgende stappen

Dit is de laatste in de reeks API Apps-zelfstudie. 

Zie de volgende bronnen voor meer informatie over Azure Active Directory.

* [Azure AD ontwikkelaarshandleiding](http://aka.ms/aaddev)
* [Azure AD-scenario 's](http://aka.ms/aadscenarios)
* [Azure AD monsters](http://aka.ms/aadsamples)

    Het monster [WebApp-WebAPI-OAuth2-AppIdentity-DotNet](http://github.com/AzureADSamples/WebApp-WebAPI-OAuth2-AppIdentity-DotNet) is vergelijkbaar met wat wordt weergegeven in deze zelfstudie, maar zonder verificatie van App-Service.

Zie voor meer informatie over andere manieren om Visual Studio-projecten op API-apps implementeren met behulp van Visual Studio of door het [automatiseren van de implementatie](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) van een [bronbeheersysteem](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control), [het implementeren van een app Azure App-Service](../app-service-web/web-sites-deploy.md).
