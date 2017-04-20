<properties
    pageTitle="Azure Active Directory v2.0 Android app | Microsoft Azure"
    description="Het bouwen van een Android app die gebruikers met zowel persoonlijke Microsoft-account en werk of school rekeningen en de Graph API-aanroepen via bibliotheken van derden zich aanmeldt."
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

#  <a name="add-sign-in-to-an-android-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>Aanmelden toevoegen aan een Android app met behulp van een externe bibliotheek met Graph API met behulp van het eindpunt v2.0

Het platform Microsoft identity maakt gebruik van open standaarden zoals OAuth2 en OpenID verbinden. Ontwikkelaars kunnen de bibliotheek willen integreren in onze services gebruiken. We enkele scenario's zoals dit om aan te tonen van het configureren van externe bibliotheken verbinding maken met het platform van Microsoft identity geschreven zodat ontwikkelaars gebruiken ons platform met andere bibliotheken. De meeste bibliotheken die [de spec RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) implementeren kunnen verbinding maken met het platform van Microsoft identity.

Met de toepassing die in deze procedure wordt gemaakt, kunnen gebruikers zich aanmelden bij hun organisatie en zoekt zelf in hun organisatie met behulp van de API van de grafiek.

Als u nieuwe OAuth2 of OpenID, logisch veel van deze configuratie monster niet dat voor u. Wij raden u aan [2.0-protocollen - OAuth 2.0 vergunning Code stroom](active-directory-v2-protocols-oauth-code.md) te lezen voor de achtergrond.

> [AZURE.NOTE] Sommige functies van ons platform waarin een expressie in de OAuth2 of OpenID verbinding normen, zoals voorwaardelijke toegang en beheer van Intune, moeten u onze open-source Microsoft Azure identiteit bibliotheken gebruiken.

Het eindpunt v2.0 biedt geen ondersteuning voor alle functies en Azure Active Directory-scenario's.

> [AZURE.NOTE] Om te bepalen als u het eindpunt v2.0 moet gebruiken, Lees over [v2.0 beperkingen](active-directory-v2-limitations.md).


## <a name="download-the-code-from-github"></a>De code van GitHub downloaden
De code voor deze zelfstudie wordt onderhouden [op GitHub](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2).  Als u wilt volgen, kunt u [downloaden van de app skelet als een ZIP-](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) of het skelet klonen:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

U kunt ook gewoon het programmeervoorbeeld downloaden en meteen aan de slag:

```
git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

## <a name="register-an-app"></a>Registreren van een app
Een nieuwe toepassing maken op de [portal Registratie van toepassing](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), of de gedetailleerde stappen bij [het registreren van een app aan het eindpunt v2.0](active-directory-v2-app-registration.md).  Zorg ervoor dat:

- De **Toepassings-Id** die toegewezen aan uw app omdat het snel hebt je kopiëren.
- Het **mobiele** platform voor uw app toevoegen.

> Opmerking:-Portal Registratie van de toepassing biedt een **Redirect URI** -waarde. In dit voorbeeld moet u de standaardwaarde gebruiken `https://login.microsoftonline.com/common/oauth2/nativeclient`.


## <a name="download-the-nxoauth2-third-party-library-and-create-a-workspace"></a>Downloaden van de bibliotheek van de derde NXOAuth2 en een werkruimte maken

Voor dit scenario gebruikt u de OIDCAndroidLib van GitHub, dat een OAuth2 bibliotheek op basis van de code OpenID verbinding van Google is. Deze het profiel van de oorspronkelijke toepassing implementeert en ondersteunt het eindpunt van de vergunning van de gebruiker. Dit zijn de dingen die u wilt integreren met het Microsoft-platform identiteit.

De OIDCAndroidLib repo naar uw computer kopiëren.

```
git@github.com:kalemontes/OIDCAndroidLib.git
```

![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/emotes-url.png)

## <a name="set-up-your-android-studio-environment"></a>Uw Android Studio-omgeving instellen

1. Maak een nieuw project met Android Studio en accepteer de standaardinstellingen in de wizard.

    ![Nieuw project maken in Android Studio](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample1.PNG)

    ![Android apparaten](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample2.PNG)

    ![Een activiteit toevoegen aan mobiele](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample3.PNG)

2. De projectmodules stelt de gekloonde repo te verplaatsen naar de projectlocatie. U kunt ook het project te maken en te klonen rechtstreeks naar de projectlocatie.

    ![Projectmodules](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4_1.PNG)

3. Open de projectinstellingen modules met behulp van het contextmenu of met behulp van de sneltoets Ctrl + Alt + rimaire + S.

    ![Projectinstellingen modules](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4.PNG)

4. De standaard app module verwijderen omdat u wilt dat alleen de projectinstellingen container.

    ![De standaard app-module](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample5.PNG)

5. Importeer de modules van de gekloonde repo aan het huidige project.

    ![Project importeren in gradle](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG)
    ![de nieuwe module pagina maken](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)

6. Herhaal deze stappen voor de `oidlib-sample` module.

7. Controleer de afhankelijkheden van de oidclib op de `oidlib-sample` module.

    ![afhankelijkheden van de oidclib van de module oidlib monster](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8.PNG)

8. Klik op **OK** en wacht gradle synchroniseren.

    De settings.gradle moet er als volgt uitzien:

    ![Screenshot van settings.gradle](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8_1.PNG)

9. Bouwen van het monster app om ervoor te zorgen dat het monster niet correct.

    Niet mogelijk nog gebruiken met Azure Active Directory. Moeten we eerst enkele eindpunten configureren. Dit is om ervoor te zorgen er is een Android Studio problemen laten we beginnen met het aanpassen van het monster app.

10. Bouwen en uitvoeren van `oidlib-sample` als doel in Android Studio.

    ![Voortgang van build oidlib monster](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample9.png)

11. Verwijder de `app ` map die is gegeven wanneer u de module verwijderd uit het project omdat Android Studio niet verwijderd voor de veiligheid.

    ![Structuur van het bestand met de map van de toepassing](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample12.PNG)

12. Open het menu **Bewerken configuraties** om de configuratie uitvoeren ook gegeven is wanneer u de module verwijderd uit het project verwijderen.

    ![Configuraties menu Bewerken](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
    ![configuratie van app uitvoeren](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)

## <a name="configure-the-endpoints-of-the-sample"></a>De eindpunten van het monster configureren

Nu de `oidlib-sample` met succes wordt uitgevoerd, zullen we bepaalde eindpunten als u deze werken met Azure Active Directory bewerken.

### <a name="configure-your-client-by-editing-the-oidcclientconfxml-file"></a>De client geconfigureerd door het bestand oidc_clientconf.xml te bewerken

1. Omdat u alleen bij een token ophalen en de Graph API aanroepen OAuth2 stromen gebruikt, stelt de client alleen OAuth2 doen. OIDC komen in een latere voorbeeld.

    ```xml
        <bool name="oidc_oauth2only">true</bool>
    ```

2. Configureer uw client-ID die u hebt ontvangen van de portal registratie.

    ```xml
        <string name="oidc_clientId">86172f9d-a1ae-4348-aafa-7b3e5d1b36f5</string>
        <string name="oidc_clientSecret"></string>
    ```

3. Het omleiden van URI met de onderstaande configureren.

    ```xml
        <string name="oidc_redirectUrl">https://login.microsoftonline.com/common/oauth2/nativeclient</string>
    ```

4. Configureer de bereiken die u nodig hebt voor toegang tot de API van de grafiek.

    ```xml
        <string-array name="oidc_scopes">
            <item>openid</item>
            <item>https://graph.microsoft.com/User.Read</item>
            <item>offline_access</item>
        </string-array>
    ```

De `User.Read` waarde in de `oidc_scopes` kunt u lezen het basisprofiel het ondertekende gebruiker.
U kunt meer informatie over de beschikbare scopes op [Microsoft Graph machtiging bereiken](https://graph.microsoft.io/docs/authorization/permission_scopes).

Als u uitleg over de `openid` of `offline_access` als bereiken in verbinding OpenID, Zie [2.0-protocollen - OAuth 2.0 vergunning Code stromen](active-directory-v2-protocols-oauth-code.md).

### <a name="configure-your-client-endpoints-by-editing-the-oidcendpointsxml-file"></a>De eindpunten van de client te configureren door het bestand oidc_endpoints.xml te bewerken

- Open de `oidc_endpoints.xml` -bestand en de volgende wijzigingen aanbrengen:

    ```xml
    <!-- Stores OpenID Connect provider endpoints. -->
    <resources>
        <string name="op_authorizationEnpoint">https://login.microsoftonline.com/common/oauth2/v2.0/authorize</string>
        <string name="op_tokenEndpoint">https://login.microsoftonline.com/common/oauth2/v2.0/token</string>
        <string name="op_userInfoEndpoint">https://www.example.com/oauth2/userinfo</string>
        <string name="op_revocationEndpoint">https://www.example.com/oauth2/revoketoken</string>
    </resources>
    ```

Deze eindpunten moeten nooit wijzigen als u OAuth2 als protocol gebruikt.

> [AZURE.NOTE]
De eindpunten voor `userInfoEndpoint` en `revocationEndpoint` worden momenteel niet ondersteund door Azure Active Directory. Als u deze met de standaardwaarde voor voorbeeld.com laat, wordt u eraan herinnerd dat ze niet beschikbaar in het monster zijn :-)


## <a name="configure-a-graph-api-call"></a>Een Graph API-aanroep configureren

- Open de `HomeActivity.java` -bestand en de volgende wijzigingen aanbrengen:

    ```Java
       //TODO: set your protected resource url
        private static final String protectedResUrl = "https://graph.microsoft.com/v1.0/me/";
    ```

Een eenvoudige grafiek API-aanroep retourneert hier onze informatie.

Dit zijn alle wijzigingen die u moet doen. Voer de `oidlib-sample` -toepassing en klik op **aanmelden**.

Nadat u hebt geverifieerd, klik op **Een beveiligde bron aanvragen** voor het testen van de aanroep van de API van de grafiek.

## <a name="get-security-updates-for-our-product"></a>Beveiligingsupdates voor ons product

We raden u aan u meldingen over veiligheidsincidenten op het [Security TechCenter](https://technet.microsoft.com/security/dd252948) bezoeken en abonneren op waarschuwingen Advisory.
