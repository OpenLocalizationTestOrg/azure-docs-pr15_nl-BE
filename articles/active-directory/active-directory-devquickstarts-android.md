<properties
    pageTitle="Azure AD Android aan de slag | Microsoft Azure"
    description="Het bouwen van een Android-toepassing die is geïntegreerd met Azure AD voor aanmelden en roept Azure AD beschermd OAuth met API's."
    services="active-directory"
    documentationCenter="android"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="integrate-azure-ad-into-an-android-app"></a>Azure AD integreren in een Android App

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)] 

Als u een toepassing ontwikkelt, kunt Azure AD u eenvoudig en ongecompliceerd voor u gebruikers verifiëren met behulp van de Active Directory-accounts.  Bovendien kunt uw toepassing elk web API beschermd door Azure AD, zoals de Office 365-API of de API Azure veilig in beslag neemt.

Voor Android-clients die toegang moeten krijgen tot beveiligde bronnen biedt AD Azure de bibliotheek van Active Directory-verificatie of ADAL.  ADAL van enige doel in het leven is kunt u gemakkelijk uw app toegangstokens ophalen.  U kunt zien hoe gemakkelijk het is, bouw hier we een takenlijst Android toepassing die:

-   Haalt access tokens voor het aanroepen van een API van de To-Do lijst met het [verificatieprotocol OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
-   Met deze eigenschap wordt de takenlijst van een gebruiker
-   Symptomen gebruikers.

Om te beginnen moet u een huurder Azure AD kunt u gebruikers maken en registreren van een toepassing.  Als u een huurder, [Lees hoe u een](active-directory-howto-tenant.md)nog geen hebt.

> [AZURE.TIP] Probeer het voorbeeld van onze nieuwe [developer-portal](https://identity.microsoft.com/Docs/Android) waarmee u slag met Azure Active Directory in een paar minuten!  De developer-portal begeleidt u door het proces van het registreren van een app en Azure AD integreren in uw code.  Wanneer u klaar bent, hebt u een eenvoudige toepassing die gebruikers in uw huurder en een backend die kunnen tokens ontvangen en gevalideerd kan worden geverifieerd. 

## <a name="step-1-download-and-run-the-nodejs-rest-api-todo-sample-server"></a>Stap 1: Download en installeer Node.js REST API TODO Sample Server

In dit voorbeeld is geschreven ten opzichte van ons bestaande voorbeeld voor het bouwen van een enkele huurder taak REST API voor Microsoft Azure Active Directory werkt in. Dit is spelen voor een snelle Start.

Voor informatie over hoe u dit instellen, gaat u naar onze bestaande voorbeelden hier:

* [Microsoft Azure Active Directory monster REST API Service voor Node.js](active-directory-devquickstarts-webapi-nodejs.md)

## <a name="step-2-register-your-web-api-with-your-microsoft-azure-ad-tenant"></a>Stap 2: Uw Web API registreren met uw Microsoft Azure AD huurder

**Wat ben ik aan het doen?**

*Microsoft Active Directory ondersteunt twee typen toepassingen toe te voegen. Web API's die services bieden voor gebruikers en toepassingen (een op het web of een toepassing wordt uitgevoerd op een apparaat) die toegang hebben tot deze Web-API's. In deze stap schrijft u de Web-API die u lokaal worden uitgevoerd voor het testen van dit voorbeeld. Deze Web-API is normaal gesproken een REST-service die u wilt dat een app voor toegang tot functionaliteit biedt. Microsoft Azure Active Directory kan elk eindpunt beschermen!*

*Hier wordt uitgegaan van een u registreert de TODO REST API waarnaar hierboven wordt verwezen, maar dit werkt voor alle Web-API u Azure Active Directory kunt te beschermen.*

Stappen voor het registreren van een Web-API met Microsoft Azure AD

1. Log in om de [Azure management portal](https://manage.windowsazure.com).
2. Klik op Active Directory in de linkerhand nav.
3. Klik op de map huurder wanneer u wilt registreren de voorbeeldtoepassing.
4. Klik op het tabblad toepassingen.
5. In de lade, klikt u op toevoegen.
6. Klik op 'Mijn organisatie ontwikkelt toepassing toevoegen'.
7. Voer een beschrijvende naam voor de toepassing, bijvoorbeeld 'TodoListService', selecteer 'Web Application en/of Web API' en klik op volgende.
8. Voer voor de URL aanmelding de basis-URL voor het monster, die standaard is `https://localhost:8080`.
9. Voer voor de URI van App-ID, `https://<your_tenant_name>/TodoListService`, vervangt `<your_tenant_name>` met de naam van de huurder Azure AD.  Klik op OK om de inschrijving te voltooien.
10. Terwijl u werkt in de portal Azure, klikt u op het tabblad configureren van uw toepassing.
11. **De waarde van de Client-ID zoeken en reserveren te kopiëren**, moet u dit later bij het configureren van uw toepassing.

## <a name="step-3-register-the-sample-android-native-client-application"></a>Stap 3: De voorbeeldtoepassing Android Native Client registreren

Registreren van uw webtoepassing is de eerste stap. Vervolgens moet u te vertellen wat uw toepassing en Azure Active Directory. Hierdoor kan de toepassing communiceren met de Web-API alleen geregistreerde

**Wat ben ik aan het doen?**  

*Zoals hierboven aangegeven, ondersteunt Microsoft Azure Active Directory toe te voegen van de twee typen toepassingen. Web API's die services bieden voor gebruikers en toepassingen (een op het web of een toepassing wordt uitgevoerd op een apparaat) die toegang hebben tot deze Web-API's. In deze stap schrijft u de toepassing in dit voorbeeld. U moet doen om deze toepassing te kunnen op aanvraag voor toegang tot de Web-API die u zojuist hebt geregistreerd. Azure Active Directory weigert om zelfs uw toepassing om te vragen-in tenzij deze geregistreerd! Die uitmaakt deel van de beveiliging van het model.*

*Hier wordt uitgegaan van een registreert u deze voorbeeldtoepassing waarnaar hierboven wordt verwezen, maar dit werkt voor elke toepassing die u ontwikkelt.*

**Waarom heb ik een toepassing en een Web API in één huurder plaatsen?**

*Zoals u wellicht hebt geraden, kunt u een toepassing die toegang heeft tot een externe API die is geregistreerd in Azure Active Directory van een andere huurder kan samenstellen. Als u dat doet, wordt klanten gevraagd om toestemming voor het gebruik van de API in de toepassing. Het goede deel is Active Directory-verificatie Library voor iOS zorgt voor deze toestemming voor u! Als we in meer geavanceerde functies, ziet u dat dit is een belangrijk onderdeel van de werkzaamheden die nodig zijn voor toegang tot de suite van Microsoft APIs van Azure en Office, alsmede andere serviceprovider. Nu, omdat u uw Web-API en de toepassing onder de dezelfde huurder geregistreerd ziet u niet vragen om toestemming. Dit is meestal het geval als u bij het ontwikkelen van een toepassing voor uw eigen bedrijf te gebruiken.*

1. Log in om de [Azure management portal](https://manage.windowsazure.com).
2. Klik op Active Directory in de linkerhand nav.
3. Klik op de map huurder wanneer u wilt registreren de voorbeeldtoepassing.
4. Klik op het tabblad toepassingen.
5. In de lade, klikt u op toevoegen.
6. Klik op 'Mijn organisatie ontwikkelt toepassing toevoegen'.
7. Voer een beschrijvende naam voor de toepassing, bijvoorbeeld "TodoListClient-Android', selecteer 'Native Client Application', en klik op volgende.
8. Voer voor het omleiden van URI, `http://TodoListClient`.  Klik op voltooien.
9. Klik op het tabblad configureren van de toepassing.
10. De Client-ID vinden en kopiëren reserveren, moet u dit later bij het configureren van uw toepassing.
11. In 'Machtigingen voor andere toepassingen', klikt u op 'Toepassing toevoegen'.  "Andere" te selecteren in de vervolgkeuzelijst 'Show' en klik op het bovenste vinkje.  Klik op de TodoListService en klikt u op het selectievakje onder de toepassing toevoegen.  "TodoListService toegang" te selecteren in de vervolgkeuzelijst 'Overgedragen machtigingen' en de configuratie op te slaan.



Bouwen met Maven, kunt u de pom.xml op hoogste niveau

  * Klonen van deze repo in een map van uw keuze:

  `$ git clone git@github.com:AzureADSamples/NativeClient-Android.git`  

  * Volg de stappen in [de sectie Prerequests voor het instellen van de maven voor android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android)
  * Emulator met 19 SDK Setup
  * Ga naar de map waar u de repo gekloond
  * De opdracht uitvoeren: mvn schoon installeren
  * Wijzig de map in de Quick Start-voorbeeld: cd samples\hello
  * De opdracht uitvoeren: mvn android: android: run implementeren
  * U ziet nu app starten
  * U test de referenties van de gebruiker om te proberen!

JAR-pakketten worden ook naast het pakket aar voorgelegd.

### <a name="step-4-download-the-android-adal-and-add-it-to-your-eclipse-workspace"></a>Stap 4: Android ADAL downloaden en toevoegen aan uw werkruimte Eclips

We hebt gemaakt dat u deze bibliotheek in uw project Android gebruiken op verschillende manieren:

* U kunt de broncode importeren in deze bibliotheek Eclips en koppelen aan uw toepassing.
* U kunt als Android Studio, *aar* pakket indeling en verwijst naar de binaire bestanden.

####<a name="option-1-source-zip"></a>Optie 1: Bron Zip

Om een kopie van de broncode downloaden, klikt u op "Download ZIP" aan de rechterkant van de pagina of klik [hier](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).

####<a name="option-2-source-via-git"></a>Optie 2: Bron via Git

Als u de broncode van de SDK via git typt:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

####<a name="option-3-binaries-via-gradle"></a>Optie 3: Binaire bestanden via Gradle

U kunt de binaire bestanden van centrale Maven-repo krijgen. Pakket AAR kan als volgt worden opgenomen in uw project in AndroidStudio:

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
}
```

####<a name="option-4-aar-via-maven"></a>Optie 4: aar via Maven

Als u de invoegtoepassing m2e in Eclips gebruikt, kunt u de afhankelijkheid in uw pom.xml bestand:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>1.1.1</version>
    <type>aar</type>
</dependency>
```


####<a name="option-5-jar-package-inside-libs-folder"></a>Optie 5: jar-pakket in de map libs
U kunt ophalen van de jar-bestand van maven de repo en zet deze neer in de map *libs* in uw project. U moet de vereiste resources aan uw project ook kopiëren omdat de jar-pakketten niet opnemen.


### <a name="step-5-add-references-to-android-adal-to-your-project"></a>Stap 5: Verwijzingen naar Android ADAL aan uw project toevoegen


2. Een verwijzing naar het project toevoegen en opgeven als een bibliotheek voor Android. Als u niet zeker hoe u dit doet weet, [Klik hier voor meer informatie] (http://developer.android.com/tools/projects/projects-eclipse.html)

3. De afhankelijkheid van het project voor foutopsporing de instellingen van uw project toevoegen

4. Update AndroidManifest.xml-bestand van uw project op te nemen:

    ```Java
      <uses-permission android:name="android.permission.INTERNET" />
      <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
      <application
            android:allowBackup="true"
            android:debuggable="true"
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:theme="@style/AppTheme" >

            <activity
                android:name="com.microsoft.aad.adal.AuthenticationActivity"
                android:label="@string/title_login_hello_app" >
            </activity>
      ....
      <application/>
    ```

7. Een exemplaar maken van AuthenticationContext op uw hoofdactiviteit. De details van deze oproep vallen buiten het bestek van dit Leesmij-bestand, maar krijgt u een goede start door te kijken naar het [Voorbeeld van Android Native Client](https://github.com/AzureADSamples/NativeClient-Android). Hieronder volgt een voorbeeld:

    ```Java
    // Authority is in the form of https://login.windows.net/yourtenant.onmicrosoft.com
    mContext = new AuthenticationContext(MainActivity.this, authority, true); // This will use SharedPreferences as            default cache
    ```
  * Opmerking: mContext is een veld in uw activiteit

8. Kopieer dit codeblok voor het verwerken van het einde van de AuthenticationActivity nadat de gebruiker referenties invoert en autorisatiecode ontvangt:

    ```Java
     @Override
     protected void onActivityResult(int requestCode, int resultCode, Intent data) {
         super.onActivityResult(requestCode, resultCode, data);
         if (mContext != null) {
             mContext.onActivityResult(requestCode, resultCode, data);
         }
     }
    ```

9. Als u wilt vragen om een token, definieert u een retouraanroep

    ```Java
    private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {

            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    textViewStatus.setText("Cancelled");
                    Log.d(TAG, "Cancelled");
                } else {
                    textViewStatus.setText("Authentication error:" + exc.getMessage());
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                mResult = result;

                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    textViewStatus.setText("Token is empty");
                    Log.d(TAG, "Token is empty");
                } else {
                    // request is successful
                    Log.d(TAG, "Status:" + result.getStatus() + " Expired:"
                            + result.getExpiresOn().toString());
                    textViewStatus.setText(PASSED);
                }
            }
        };
    ```
10. Tot slot vragen om een token met behulp van terugbellen:

    ```Java
     mContext.acquireToken(MainActivity.this, resource, clientId, redirect, user_loginhint, PromptBehavior.Auto, "",
                    callback);
    ```

Beschrijvingen van de parameters:

  * Resource is vereist en de resource die u wilt openen.
  * Clientid is vereist en is afkomstig van de AzureAD Portal.
  * U kunt redirectUri instellen als de pakketnaam. Het is niet vereist voor het aanroepen van acquireToken.
  * PromptBehavior kunt u vragen om referenties in cache en cookies overslaan.
  * Terugbellen wordt aangeroepen nadat de code van de vergunning voor een token wordt uitgewisseld.

  De retouraanroep heeft een object van het AuthenticationResult met accesstoken, de datum is verstreken, en idtoken informatie.

Optioneel: **acquireTokenSilent**

U kunt bellen **acquireTokenSilent** voor het verwerken van caching en token vernieuwen. Het biedt ook sync versie. Gebruikers-id worden geaccepteerd als parameter.

    ```java
     mContext.acquireTokenSilent(resource, clientid, userId, callback );
    ```

11. **Makelaar**: portal van Microsoft Intune bedrijf-app biedt de broker-component. ADAL de broker-account, als er een gebruikersaccount is gemaakt op deze verificator en ontwikkelaar gebruik wilt niet overslaan. Ontwikkelaars kan de gebruiker broker met overslaan:

    ```java
     AuthenticationSettings.Instance.setSkipBroker(true);
    ```

 De ontwikkelaar moet registreren, speciale redirectUri voor het gebruik van de makelaar. RedirectUri is in de indeling van de msauth://packagename/Base64UrlencodedSignature. U kunt uw redirecturi op uw app met het script "brokerRedirectPrint.ps1" of gebruik van API-aanroep mContext.getBrokerRedirectUri. Handtekening is gerelateerd aan de handtekeningcertificaten.

 Huidige broker model is voor één gebruiker. AuthenticationContext biedt een API-methode als u de gebruiker broker.

 ```java
 String brokerAccount =  mContext.getBrokerUser();
 ```
 Broker gebruiker wordt geretourneerd als de account geldig is.

 Het manifest van de toepassing moet gemachtigd zijn om AccountManager accounts gebruiken: http://developer.android.com/reference/android/accounts/AccountManager.html

 * GET_ACCOUNTS
 * USE_CREDENTIALS
 * MANAGE_ACCOUNTS


In dit scenario gebruikt, beschikt u over wat u nodig hebt om te kunnen integreren met Azure Active Directory. Voor meer voorbeelden van deze werken, gaat u naar de AzureADSamples / bibliotheek op GitHub.

## <a name="important-information"></a>Belangrijke informatie

### <a name="customization"></a>Aanpassing

Bibliotheek projectresources kunnen worden overschreven door de toepassingsbronnen. Dit gebeurt wanneer het samenstellen van uw app. Daarom kunt u verificatie indelen zoals u dat wilt. U moet ervoor zorgen dat de id van de besturingselementen die ADAL uses(Webview).

### <a name="broker"></a>Makelaar

Broker-component wordt geleverd met Microsoft Intune van bedrijf portal app. Account wordt gemaakt in accountbeheer. Accounttype is 'com.microsoft.workaccount'. Het staat slechts één account voor eenmalige aanmelding. Na het voltooien van apparaat uitdaging voor een van de apps maakt het SSO-cookie voor deze gebruiker.

### <a name="authority-url-and-adfs"></a>Url van de autoriteit en AD FS

AD FS wordt niet herkend als productie STS, dus u hoeft te schakelen van discovery exemplaar en false doorgeeft aan de constructor AuthenticationContext.

Url van de autoriteit moet de STS-exemplaar en de naam van de huurder: https://login.windows.net/yourtenant.onmicrosoft.com

### <a name="querying-cache-items"></a>Bij het controleren van items in cache

ADAL biedt standaardcache in SharedPreferences met enkele eenvoudige cache queryfuncties. U kunt de huidige cache krijgen van AuthenticationContext met:
```Java
 ITokenCacheStore cache = mContext.getCache();
```
Als u wilt aanpassen, kunt u uw implementatie cache bieden.
```Java
mContext = new AuthenticationContext(MainActivity.this, authority, true, yourCache);
```

### <a name="promptbehavior"></a>PromptBehavior

ADAL biedt de optie voor het vraaggedrag opgeeft. PromptBehavior.Auto verschijnt de gebruikersinterface als vernieuwen token ongeldig is en gebruikersreferenties nodig zijn. PromptBehavior.Always wordt het gebruik van de cache overslaan en UI altijd weergeven.

### <a name="silent-token-request-from-cache-and-refresh"></a>Silent tokenaanvraag uit de cache en vernieuwen

Deze methode gebruikt geen UI pop up en vereist een activiteit. De eigenschap retourneert token uit de cache als beschikbaar. Als het token is verlopen, zal het proberen te vernieuwen. Als het token vernieuwen is verlopen of is mislukt, wordt AuthenticationException geretourneerd.

    ```Java
    Future<AuthenticationResult> result = mContext.acquireTokenSilent(resource, clientid, userId, callback );
    ```

U kunt ook synchroniseren met deze methode aanroepen maken. U kunt null voor terugbellen instellen of acquireTokenSilentSync gebruiken.

### <a name="diagnostics"></a>Diagnostische gegevens

Dit zijn de primaire bronnen van informatie voor het oplossen van problemen:

+ Uitzonderingen
+ Logboeken
+ Netwerk-traces

Let er ook op dat de correlatie-id's bepalend voor de diagnostische gegevens in de bibliotheek zijn. U kunt instellen dat de correlatie tussen id's op basis van per verzoek als u wilt een ADAL correleren met andere bewerkingen in uw code aanvragen. Als u niet een correlatie-id ADAL genereert een willekeurig een and alle worden berichten in het logboek en netwerk aanroepen met de correlatie-id gestempeld. De eigen gegenereerd bij elke aanvraag-id wordt gewijzigd.

#### <a name="exceptions"></a>Uitzonderingen

Dit is uiteraard de eerste diagnose. We proberen te bieden nuttige foutberichten worden weergegeven. Als u vindt dat niet handig Rapporteer een probleem en laat het ons weten. Geef ook gegevens van een apparaat zoals een model en SDK #.

#### <a name="logs"></a>Logboeken

U kunt de bibliotheek voor het genereren van berichten die u gebruiken kunt om vast te stellen die in het logboek. U kunt logboekregistratie configureren door de volgende oproep voor het configureren van een retouraanroep die ADAL wordt gebruikt aan de hand van elk bericht wordt gegenereerd.


 ```Java
 Logger.getInstance().setExternalLogger(new ILogger() {
     @Override
     public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) {
      ...
      // You can write this to logfile depending on level or errorcode.
      writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);
     }
 }
 ```
Berichten kunnen naar een aangepaste logboekbestand worden geschreven, zoals hieronder wordt weergegeven. Er is geen standaard manier om de logboeken van een apparaat. Er zijn bepaalde services die u met dit helpen kunnen. U kunt ook uw eigen, zoals het verzenden van het bestand naar een server voor voorraad.

```Java
private syncronized void writeToLogFile(Context ctx, String msg) {
       File directory = ctx.getDir(ctx.getPackageName(), Context.MODE_PRIVATE);
       File logFile = new File(directory, "logfile");
       FileOutputStream outputStream = new FileOutputStream(logFile, true);
       OutputStreamWriter osw = new OutputStreamWriter(outputStream);
       osw.write(msg);
       osw.flush();
       osw.close();
}
```

##### <a name="logging-levels"></a>Registratieniveaus

+ Error(Exceptions)
+ Warn(Warning)
+ Info (ter informatie)
+ Uitgebreid (meer details)

U instellen het niveau voor vastleggen als volgt:
```Java
Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);
 ```

 Alle berichten in het logboek worden verzonden naar logcat naast een aangepaste log-callbacks.
Log in om een formulier bestand logcat kunt u opvragen als belog weergegeven:

 ```
  adb logcat > "C:\logmsg\logfile.txt"
 ```
 Meer voorbeelden over adb opdrachten: https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat

#### <a name="network-traces"></a>Netwerk-Traces

Verschillende hulpprogramma's kunt u het HTTP-verkeer dat ADAL genereert vastleggen.  Dit is vooral handig als u bekend met het protocol OAuth bent of als u nodig hebt om Microsoft of andere kanalen ondersteuning diagnostische informatie te geven.

Fiddler is de eenvoudigste hulpmiddel voor het traceren van HTTP.  Gebruik de volgende koppelingen om tot goed record ADAL netwerkverkeer in te stellen.  Om nuttig is nodig voor het configureren van fiddler of een ander hulpmiddel zoals Charles voor het vastleggen van niet-gecodeerde SSL-verkeer.  Opmerking: Sporen gegenereerd op deze manier kunnen zeer beschermde informatie bevatten zoals toegangstokens, gebruikersnamen en wachtwoorden.  Als u accounts productie niet delen deze sporen met 3e partijen.  Als u traceren naar iemand wilt om ondersteuning te leveren, het probleem met een tijdelijke rekening met de gebruikersnamen en wachtwoorden die wel delen te reproduceren.

+ [Het instellen van Fiddler voor Android](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)
+ [Fiddler regels configureren voor ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)


### <a name="dialog-mode"></a>Dialoogvenster modus
de methode acquireToken zonder activiteit ondersteunt dialoogvenster.

### <a name="encryption"></a>Codering

ADAL codeert de tokens en winkel in SharedPreferences standaard. U kunt de klasse StorageHelper kunt u de details bekijken. Android AndroidKeyStore voor veilige opslag van persoonlijke sleutels 4.3(API18) ingevoerd. ADAL gebruikt voor API18 en hoger. Als u gebruik wilt maken ADAL voor lagere versies van de SDK, moet u opgeven, geheime sleutel op AuthenticationSettings.INSTANCE.setSecretKey

### <a name="oauth2-bearer-challenge"></a>Oauth2 aan toonder challenge

Klasse AuthenticationParameters biedt de functionaliteit om de authorization_uri van Oauth2 aan toonder challenge.

### <a name="session-cookies-in-webview"></a>Cookies per sessie in webweergave

Android webweergave wordt cookies per sessie niet gewist wanneer app wordt gesloten. Met de volgende voorbeeldcode kunt u dit verwerken:
```java
CookieSyncManager.createInstance(getApplicationContext());
CookieManager cookieManager = CookieManager.getInstance();
cookieManager.removeSessionCookie();
CookieSyncManager.getInstance().sync();
```
Meer informatie over cookies: http://developer.android.com/reference/android/webkit/CookieSyncManager.html

### <a name="resource-overrides"></a>Resource-overschrijvingen

De bibliotheek ADAL omvat Engelse tekenreeksen voor de volgende twee ProgressDialog weergegeven.

Uw toepassing moet overschrijven als gelokaliseerde tekenreeksen gewenst zijn.

```Java
<string name="app_loading">Loading...</string>
<string name="broker_processing">Broker is processing</string>
<string name="http_auth_dialog_username">Username</string>
<string name="http_auth_dialog_password">Password</string>
<string name="http_auth_dialog_title">Sign In</string>
<string name="http_auth_dialog_login">Login</string>
<string name="http_auth_dialog_cancel">Cancel</string>
```

=======

### <a name="ntlm-dialog"></a>NTLM-dialoogvenster
Adal versie 1.1.0 ondersteunt NTLM-dialoogvenster dat wordt verwerkt door de gebeurtenis onReceivedHttpAuthRequest van WebViewClient. Dialoogvenster lay-out en tekenreeksen kunnen worden aangepast.

### <a name="cross-app-sso"></a>SSO cross-app
Informatie over [het inschakelen van SSO cross-app op Android met ADAL](active-directory-sso-android.md)  


[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
