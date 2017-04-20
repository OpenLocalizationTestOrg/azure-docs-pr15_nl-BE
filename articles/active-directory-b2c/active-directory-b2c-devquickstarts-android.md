<properties
    pageTitle="Azure Active Directory B2C: Een web API aanroepen vanuit een toepassing Android | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe u voor het maken van een taak-overzicht van Android app die een Node.js web API aanroept met behulp van tokens voor OAuth 2.0 aan toonder. Zowel de Android app en het web API gebruiken Azure Active Directory B2C identiteit van gebruikers beheren en gebruikers verifiëren."
    services="active-directory-b2c"
    documentationCenter="android"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c-call-a-web-api-from-an-android-application"></a>Azure AD B2C: Een web API aanroepen vanuit een toepassing voor Android

> [AZURE.WARNING] Deze zelfstudie is vereist voor sommige belangrijke updates beschikbaar, op het gebruik van Android ADAL voor B2C verwijderen.  We publiceren vers instructies voor het gebruik van Azure AD B2C in Android apps in de volgende week gaan en het beste bedrijf uit tot die tijd.  Maar als u wilt proberen dingen uit, gerust doorgaan met het onderstaande artikel.



Met behulp van B2C Azure Active Directory (AD Azure) kunt u krachtige Self-service identity management-functies toevoegen aan uw Android apps en web-API's in een paar korte stappen. In dit artikel wordt beschreven hoe u voor het maken van een Android 'takenlijst' app die een Node.js web API aanroept met behulp van tokens voor OAuth 2.0 aan toonder. Zowel de Android app en het web API gebruiken Azure AD B2C identiteit van gebruikers beheren en gebruikers verifiëren.

Deze quickstart hebt u een web API die wordt beveiligd door Azure AD met B2C volledig werken. Wij hebben gebouwd voor zowel .NET en Node.js kunt gebruiken. Dit overzicht wordt verondersteld dat het monster Node.js web API is geconfigureerd. Zie voor meer informatie de [Azure AD B2C web API voor zelfstudie Node.js](active-directory-b2c-devquickstarts-api-node.md).

Voor Android-clients die toegang moeten krijgen tot beveiligde bronnen biedt AD Azure Active Directory verificatie bibliotheek (ADAL). Het enige doel van het ADAL is kunt u gemakkelijk uw app toegangstokens ophalen. Om aan te tonen hoe gemakkelijk het is, in deze handleiding we bouw een toepassing Android to-do lijst die:

- Toegangstokens die een takenlijst API aanroepen met het [verificatieprotocol OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx)haalt.
- Deze eigenschap haalt een takenlijsten van gebruikers.
- Symptomen van gebruikers.

> [AZURE.NOTE] Dit artikel dekt niet aanmelden, aanmelden bij implementatie en beheer van gebruikersprofielen via Azure AD B2C. Het richt zich op het web API's aanroepen nadat de gebruiker is geverifieerd. Als u nog niet gedaan hebt, moet u beginnen met de [zelfstudie .NET web app aan de slag](active-directory-b2c-devquickstarts-web-dotnet.md) voor meer informatie over de basisbeginselen van Azure AD B2C.

## <a name="get-an-azure-ad-b2c-directory"></a>Een AD-B2C Azure map ophalen

Voordat u AD-B2C Azure gebruiken kunt, moet u een map maken of pachters. Een directory is een container voor alle gebruikers, toepassingen en groepen. Als u niet al een hebt, [een B2C-map maken](active-directory-b2c-get-started.md) voordat u verder in deze handleiding.

## <a name="create-an-application"></a>Een toepassing maken

Vervolgens moet u voor het maken van een app in de B2C-map. Dit geeft Azure AD-informatie die nodig is om veilig te communiceren met uw app. De app en de web-API worden vertegenwoordigd door een enkele **Aanvraag-ID** in dit geval, omdat ze bestaan uit één logische app. Als u wilt een app maken, volg [de instructies](active-directory-b2c-app-registration.md). Zorg ervoor dat:

- Een **web app**zijn/**web API** in de toepassing.
- Voer `urn:ietf:wg:oauth:2.0:oob` als een **antwoord op URL**. Het is de standaard-URL voor deze voorbeeldcode.
- Een **geheim van toepassing** voor uw toepassing te maken en te kopiëren. U zal het later nodig hebt. Houd er rekening mee dat deze waarde worden [XML-escaped moet](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) voordat u deze kunt gebruiken.
- Kopieer de **Toepassings-ID** die is toegewezen aan uw app. Ook moet u dit later.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Uw beleid maken

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

In Azure AD B2C, wordt elke gebruikerservaring gedefinieerd door een [beleid](active-directory-b2c-reference-policies.md). Deze app bevat drie identiteit ervaringen: aanmelden, log in en meld u aan met Facebook.  Moet u voor het maken van een beleid voor elk type, zoals beschreven in het [artikel beleid](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Als u de drie beleidsregels maakt, moet u:

- Kies de **weergegeven naam** en andere kenmerken van de aanmelding in uw aanmelding beleid.
- Kies de **weergavenaam** en de **Object-ID** application claims in elk beleid. U kunt ook andere vorderingen.
- De **naam** van elk beleid kopiëren nadat u deze hebt gemaakt. Er is het voorvoegsel `b2c_1_`.  Moet u deze namen beleid later.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nadat u de drie beleidsregels hebt gemaakt, bent u klaar om uw app te bouwen.

Opmerking in dit artikel dekt niet het gebruik van het beleid dat u zojuist hebt gemaakt. Meer informatie over de werking van beleid in Azure AD B2C, start met de [zelfstudie .NET web app aan de slag](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>De code downloaden

De code voor deze zelfstudie [wordt onderhouden op GitHub](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android). Voor het opbouwen van het monster als u gaat, kunt u [de overgebleven project als een ZIP-bestand downloaden](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/skeleton.zip). U kunt ook het skelet klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-Android.git
```

> [AZURE.NOTE] **U moet het skelet als u deze zelfstudie wilt downloaden.** Gezien de complexiteit van de implementatie van een volledig functionele toepassing Android, heeft het skelet UX-code die wordt uitgevoerd nadat u deze zelfstudie hebt voltooid. Dit is een tijdbesparende maatregel voor ontwikkelaars. De UX-code is niet naar het onderwerp van B2C toevoegen aan een Android toepassing belangrijkste.

De voltooide app is ook [beschikbaar als een ZIP-bestand](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip) of op de `complete` tak van de dezelfde opslagplaats.

Bouwen met Maven, kunt u `pom.xml` op het hoogste niveau.

  1. Volg de stappen in [de sectie vereisten voor het instellen van Maven voor Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android).
  2. Stel een emulator met 21 SDK.
  3. Ga naar de map waar u de repo gekloond.
  4. Voer de opdracht `mvn clean install`.
  5. Wijzig de map in het monster Quickstart `cd samples\hello`.
  6. Voer de opdracht `mvn android:deploy android:run`.

U ziet de app starten. Test de referenties van de gebruiker om te proberen het invoeren.

Java-archief (JAR) pakketten wordt naast het pakket Android archief (AAR) ook worden ingediend.

## <a name="download-the-android-adal-and-add-it-to-your-android-studio-workspace"></a>Android ADAL downloaden en toevoegen aan uw werkruimte Android Studio

U hebt de opties voor het gebruik van deze bibliotheek in uw Android project:

* U kunt de broncode importeren in de bibliotheek Eclips en koppelen aan uw toepassing.
* Als u Android Studio, kunt u de indeling van het pakket AAR gebruikt en verwijst naar de binaire bestanden.

### <a name="option-1-binaries-via-gradle-recommended"></a>Optie 1: Binaire bestanden via Gradle (aanbevolen)

U krijgt de binaire bestanden van de centrale Maven-repo. Het pakket AAR kan worden opgenomen in uw project in Android Studio (bijvoorbeeld, in `build.gradle`) op deze manier:

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
    compile('com.microsoft.aad:adal:2.0.1-alpha') {
        exclude group: 'com.android.support'
    } // Recent version is 2.0.1-alpha
}
```

### <a name="option-2-aar-via-maven"></a>Optie 2: AAR via Maven

Als u de `m2e` invoegtoepassing in Eclips, kunt u de afhankelijkheid in de `pom.xml` bestand:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>2.0.1-alpha</version>
    <type>aar</type>
</dependency>
```

### <a name="option-3-source-via-git-last-resort"></a>Optie 3: De gegevensbron via Git (laatste redmiddel)

Als u de broncode van de SDK via Git, typ:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

Gebruik de tak **convergentie.**

## <a name="set-up-your-configuration-file"></a>Instellen van uw configuratiebestand

Gebruikt u de configuratie die u eerder in de B2C-portal voor het configureren van de Android-project.

Open `helpes/Constants.java` en vul de waarden voor de volgende:

```

package com.microsoft.aad.taskapplication.helpers;

import com.microsoft.aad.adal.AuthenticationResult;

public class Constants {

    public static final String SDK_VERSION = "1.0";
    public static final String UTF8_ENCODING = "UTF-8";
    public static final String HEADER_AUTHORIZATION = "Authorization";
    public static final String HEADER_AUTHORIZATION_VALUE_PREFIX = "Bearer ";

    // -------------------------------AAD
    // PARAMETERS----------------------------------
    public static String AUTHORITY_URL = "https://login.microsoftonline.com/hypercubeb2c.onmicrosoft.com/";
    public static String CLIENT_ID = "<your application id>";
    public static String[] SCOPES = {"<your application id>"};
    public static String[] ADDITIONAL_SCOPES = {""};
    public static String REDIRECT_URL = "<redirect uri>";
    public static String CORRELATION_ID = "";
    public static String USER_HINT = "";
    public static String EXTRA_QP = "";
    public static String FB_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNIN_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNUP_POLICY = "B2C_1_<your policy>";
    public static boolean FULL_SCREEN = true;
    public static AuthenticationResult CURRENT_RESULT = null;
    // Endpoint we are targeting for the deployed WebAPI service
    public static String SERVICE_URL = "http://localhost:3000/tasks";

    // ------------------------------------------------------------------------------------------

    static final String TABLE_WORKITEM = "WorkItem";
    public static final String SHARED_PREFERENCE_NAME = "com.example.com.test.settings";


}


```
- `SCOPES`: De scopes die u doorgeven aan de server die u wilt aanvragen opnieuw van de server wanneer een gebruiker zich aanmeldt. De voorvertoning B2C geeft u `client_id`. Dit moet echter wijzigen in `read scopes` in de toekomst. Dit document wordt bijgewerkt wanneer die plaatsvindt.
- `ADDITIONAL_SCOPES`: Extra scopes die u wilt gebruiken voor uw toepassing. Naar verwachting zal worden gebruikt in de toekomst.
- `CLIENT_ID`: De toepassings-ID u hebt gekregen van de portal.
- `REDIRECT_URL`: De omleiding waarin u verwacht dat de token teruggestuurd.
- `EXTRA_QP`: Geen extra hulpmiddelen u wilt doorgeven aan de server in een URL-gecodeerde indeling.
- `FB_POLICY`: Het beleid dat u aanroept. Dit is het belangrijkste onderdeel van dit overzicht.
- `EMAIL_SIGNIN_POLICY`: Het beleid dat u aanroept. Dit is het belangrijkste onderdeel van dit overzicht.
- `EMAIL_SIGNUP_POLICY`: Het beleid dat u aanroept. Dit is het belangrijkste onderdeel van dit overzicht.

## <a name="add-references-to-android-adal-to-your-project"></a>Verwijzingen naar Android ADAL aan uw project toevoegen

> [AZURE.NOTE]  ADAL voor Android maakt gebruik van een model op basis van het doel aan te roepen van verificatie. Intenties vast"over' de app om te werken. Dit hele sample en alle ADAL voor Android, gericht op het beheren van intents en doorgeven van informatie tussen deze.

Vertel eerst Android over de lay-out van uw toepassing, met inbegrip van de die u wilt gebruiken. Deze intenties worden verderop in deze zelfstudie in detail beschreven.

Bijwerken van uw project `AndroidManifest.xml` bestand alle uw intenties worden opgenomen:

```
   <?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.microsoft.aad.taskapplication"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk
        android:minSdkVersion="11"
        android:targetSdkVersion="19" />

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <activity
            android:name="com.microsoft.aad.adal.AuthenticationActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:exported="true"
            android:label="@string/title_login_hello_app" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.LoginActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.UsersListActivity"
            android:label="@string/title_activity_feed" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.SettingsActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.AddTaskActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.ToDoActivity"
            android:label="@string/app_name" >
        </activity>
    </application>

</manifest>    
```

Zoals u zien kunt, kunt u vijf activiteiten definiëren. U gebruikt deze.

- `AuthenticationActivity`: Dit wordt overgenomen uit het ADAL en biedt de webweergave aanmelden.
- `LoginActivity`: Hier worden de knoppen voor elk beleid en het beleid-in.
- `SettingsActivity`: U kunt dit app wijzigen tijdens runtime.
- `AddTaskActivity`: U deze taken toevoegen aan de REST API die zijn beveiligd met AD Azure.
- `ToDoActivity`: Dit is de hoofdactiviteit die taken worden weergegeven.

## <a name="create-the-sign-in-activity"></a>Maak de activiteit aanmelden

Een hoofdactiviteit en wordt het `LoginActivity`.

Maak een bestand genaamd `LoginActivity.java`.

U moet de activiteit initialiseren en enkele knoppen waarmee u uw UI toevoegen. Dit is u vertrouwd voorkomen als u Android code voordat u hebt geschreven.

```
import android.app.Activity;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;

/**
 * Created by brwerner on 9/17/15.
 */
public class LoginActivity extends Activity {

    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_signin);
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        Button button = (Button) findViewById(R.id.signin_facebook);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.FB_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signin_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNIN_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signup_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNUP_POLICY);
                startActivity(intent);

            }
        });

    }

}


```
U hebt nu gemaakt met knoppen die uw `ToDoActivity` intent (die ADAL wordt aangeroepen wanneer u een token). Dit doen ze met uw activiteit als een verwijzing en een extra parameter. Deze extra parameter wordt doorgegeven door de `intent.putExtra()` methode. U definieert `"thePolicy"` met behulp van opgegeven in `Constants.java`. Zo weet de bedoeling welk beleid moet worden aangeroepen tijdens de verificatie.

## <a name="create-the-settings-activity"></a>De instellingen voor activiteit maken

Dit is een activiteit die de settings-gebruikersinterface wordt gevuld.

Maak een bestand met de naam `SettingsActivity.java` voor eenvoudige maken, lezen, bijwerken en verwijderen (CRUD).

```
 package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.content.SharedPreferences;
import android.content.SharedPreferences.Editor;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.Switch;
import android.widget.TextView;

import com.microsoft.aad.taskapplication.helpers.Constants;

/**
 * Settings page to try broker by options
 */
public class SettingsActivity extends Activity {

    //private CheckBox checkboxAskBroker, checkboxCheckBroker;
    private Switch fullScreenSwitch;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_settings);

        loadSettings();
//      checkboxAskBroker = (CheckBox) findViewById(R.id.askInstall);
//      checkboxCheckBroker = (CheckBox) findViewById(R.id.useBroker);

        Button save = (Button) findViewById(R.id.settingsSave);

        save.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                TextView textView = (TextView)findViewById(R.id.authority);
                Constants.AUTHORITY_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.resource);
            //    Constants.SCOPES = textView.getText().toString();

                textView = (TextView)findViewById(R.id.clientId);
                Constants.CLIENT_ID = textView.getText().toString();

                textView = (TextView)findViewById(R.id.extraQueryParameters);
                Constants.EXTRA_QP = textView.getText().toString();

                textView = (TextView)findViewById(R.id.redirectUri);
                Constants.REDIRECT_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                Constants.SERVICE_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                textView.setText(Constants.SERVICE_URL);

                textView = (TextView)findViewById(R.id.fb_signin);
                textView.setText(Constants.FB_POLICY);

                textView = (TextView)findViewById(R.id.email_signin);
                textView.setText(Constants.EMAIL_SIGNIN_POLICY);

                textView = (TextView)findViewById(R.id.email_signup);
                textView.setText(Constants.EMAIL_SIGNUP_POLICY);

                textView = (TextView)findViewById(R.id.correlationId);
                textView.setText(Constants.CORRELATION_ID);

                fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
                Constants.FULL_SCREEN = fullScreenSwitch.isChecked();

                finish();
            }
        });


    }

    private void loadSettings() {
        TextView textView = (TextView)findViewById(R.id.authority);
        textView.setText(Constants.AUTHORITY_URL);
        textView = (TextView)findViewById(R.id.resource);
        textView.setText(Constants.SCOPES[0]);
        textView = (TextView)findViewById(R.id.clientId);
        textView.setText(Constants.CLIENT_ID);
        textView = (TextView)findViewById(R.id.extraQueryParameters);
        textView.setText(Constants.EXTRA_QP);
        textView = (TextView)findViewById(R.id.redirectUri);
        textView.setText(Constants.REDIRECT_URL);
        textView = (TextView)findViewById(R.id.serviceUrl);
        textView.setText(Constants.SERVICE_URL);
        textView = (TextView)findViewById(R.id.fb_signin);
        textView.setText(Constants.FB_POLICY);
        textView = (TextView)findViewById(R.id.email_signin);
        textView.setText(Constants.EMAIL_SIGNIN_POLICY);
        textView = (TextView)findViewById(R.id.email_signup);
        textView.setText(Constants.EMAIL_SIGNUP_POLICY);
        textView = (TextView)findViewById(R.id.correlationId);
        textView.setText(Constants.CORRELATION_ID);

        fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
        fullScreenSwitch.setChecked(Constants.FULL_SCREEN);
    }

    private void saveSettings(String key, boolean value) {
        SharedPreferences prefs = SettingsActivity.this.getSharedPreferences(
                Constants.SHARED_PREFERENCE_NAME, Activity.MODE_PRIVATE);
        Editor prefsEditor = prefs.edit();
        prefsEditor.putBoolean(key, value);
        prefsEditor.commit();
    }
}
```

## <a name="create-the-add-task-activity"></a>De activiteit toevoegen-taak maken

U kunt deze activiteit een taak toevoegen aan uw eindpunt REST API.

Maak een bestand met de naam `AddTaskActivity.java` en de volgende te schrijven.

```
package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;

public class AddTaskActivity extends Activity {

    EditText textField;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_add_task);
        textField = (EditText) findViewById(R.id.taskToAdd);
        Button button = (Button) findViewById(R.id.postTaskbutton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (textField.getText().toString() != null
                        && !textField.getText().toString().trim().isEmpty()
                        && Constants.CURRENT_RESULT != null) {

                    TodoListHttpService service = new TodoListHttpService();
                    try {
                        service.addItem(textField.getText().toString(), Constants.CURRENT_RESULT.getAccessToken());
                    } catch (Exception e) {
                        SimpleAlertDialog.showAlertDialog(getApplicationContext(), "Exception caught", e.getMessage());
                    }
                    finish();
                }
            }
        });
    }

}

```

## <a name="create-the-to-do-list-activity"></a>De lijst taakactiviteit maken

Dit is de belangrijkste activiteit. U kunt gebruikmaken van het verkrijgen van een token van Azure AD voor een beleid, en deze token naar de server taak REST API aanroepen.

Maak een bestand met de naam `ToDoActivity.java` en de volgende te schrijven. (De oproepen worden beschreven verderop.)

```

package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.app.AlertDialog;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Build;
import android.os.Bundle;
import android.view.View;
import android.view.Window;
import android.webkit.CookieManager;
import android.webkit.CookieSyncManager;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.ListView;
import android.widget.TextView;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationCallback;
import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.adal.AuthenticationResult;
import com.microsoft.aad.adal.AuthenticationSettings;
import com.microsoft.aad.adal.UserIdentifier;
import com.microsoft.aad.adal.PromptBehavior;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.InMemoryCacheStore;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;
import com.microsoft.aad.taskapplication.helpers.Utils;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;


import java.io.UnsupportedEncodingException;
import java.net.MalformedURLException;
import java.net.URL;
import java.security.NoSuchAlgorithmException;
import java.security.spec.InvalidKeySpecException;
import java.util.ArrayList;
import java.util.List;
import java.util.UUID;

public class ToDoActivity extends Activity {



    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;
    private static AuthenticationResult sResult;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;


    /**
     * Initializes the activity
     */
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_list_todo_items);
        CookieSyncManager.createInstance(getApplicationContext());
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        // Clear previous sessions
        clearSessionCookie();
        try {
            // Provide key info for Encryption
            if (Build.VERSION.SDK_INT < 18) {
                Utils.setupKeyForSample();
            }

        Button button = (Button) findViewById(R.id.addTaskButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, AddTaskActivity.class);
                startActivity(intent);
            }
        });

        button = (Button) findViewById(R.id.appSettingsButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, SettingsActivity.class);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.switchUserButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, LoginActivity.class);
                startActivity(intent);

            }
        });


        final TextView name = (TextView)findViewById(R.id.userLoggedIn);


        mLoginProgressDialog = new ProgressDialog(this);
        mLoginProgressDialog.requestWindowFeature(Window.FEATURE_NO_TITLE);
        mLoginProgressDialog.setMessage("Login in progress...");
        mLoginProgressDialog.show();
        // Ask for token and provide callback
        try {
            mAuthContext = new AuthenticationContext(ToDoActivity.this, Constants.AUTHORITY_URL,
                    false);
            String policy = getIntent().getStringExtra("thePolicy");

            if(Constants.CORRELATION_ID != null &&
                    Constants.CORRELATION_ID.trim().length() !=0){
                mAuthContext.setRequestCorrelationId(UUID.fromString(Constants.CORRELATION_ID));
            }

            AuthenticationSettings.INSTANCE.setSkipBroker(true);

            mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES, policy, Constants.CLIENT_ID,
                    Constants.REDIRECT_URL, getUserInfo(), PromptBehavior.Always,
                    "nux=1&" + Constants.EXTRA_QP,
                    new AuthenticationCallback<AuthenticationResult>() {

                        @Override
                        public void onError(Exception exc) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }
                            SimpleAlertDialog.showAlertDialog(ToDoActivity.this,
                                    "Failed to get token", exc.getMessage());
                        }

                        @Override
                        public void onSuccess(AuthenticationResult result) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }

                            if (result != null && !result.getToken().isEmpty()) {
                                setLocalToken(result);
                                updateLoggedInUser();
                                getTasks();
                                ToDoActivity.sResult = result;
                                Toast.makeText(getApplicationContext(), "Token is returned", Toast.LENGTH_SHORT)
                                        .show();

                                if (sResult.getUserInfo() != null) {
                                    name.setText(result.getUserInfo().getDisplayableId());
                                    Toast.makeText(getApplicationContext(),
                                            "User:" + sResult.getUserInfo().getDisplayableId(), Toast.LENGTH_SHORT)
                                            .show();
                                }
                            } else {
                                //TODO: popup error alert
                            }
                        }
                    });
        } catch (Exception e) {
            SimpleAlertDialog.showAlertDialog(ToDoActivity.this, "Exception caught", e.getMessage());
        }
        Toast.makeText(ToDoActivity.this, TAG + "done", Toast.LENGTH_SHORT).show();
    } catch (InvalidKeySpecException e) {
            e.printStackTrace();
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }}

```


 Hebt misschien u opgemerkt dat dit afhankelijk is van de methoden die nog niet zijn geschreven. Zij omvatten `updateLoggedInUser()`, `clearSessionCookie()`, en `getTasks()`. U kunt schrijven die verderop in deze handleiding. U kunt de fouten in Android Studio nu negeren.

Beschrijvingen van de parameters:

  - `SCOPES`: Vereist, de scopes die u probeert op te vragen voor toegang. Voor de voorvertoning B2C, dit is hetzelfde als `client_id`, maar dit is te verwachten in de toekomst te wijzigen.
  - `POLICY`: Het beleid voor de gewenste om de gebruiker te verifiëren.
  - `CLIENT_ID`: Vereist, is afkomstig van de portal Azure AD.
  - `redirectUri`: Kan worden ingesteld als de pakketnaam van uw. Dit is niet vereist voor de `acquireToken` aanroepen.
  - `getUserInfo()`: De manier om te zoeken of een gebruiker zich al in de cache. De parameter wordt ook beschreven hoe een gebruiker wordt gevraagd als die gebruiker is niet gevonden of als het toegangstoken van de gebruiker ongeldig is. Met deze methode wordt verderop in deze handleiding worden geschreven.
  - `PromptBehavior.always`: Kunt u vragen om referenties in cache en cookies overslaan.
  - `Callback`: Wordt aangeroepen nadat een autorisatiecode voor een token wordt uitgewisseld. Is er een object `AuthenticationResult`, dat toegangstoken, vervaldatum en token ID-informatie bevat.

> [AZURE.NOTE]  De Microsoft Intune bedrijf portal-app biedt de broker-component en het kan worden geïnstalleerd op een apparaat van de gebruiker. Single sign-on (SSO) toegang tot biedt de app voor alle toepassingen op het apparaat. Ontwikkelaars moeten worden voorbereid voor Intune. ADAL voor Android zal de broker-account gebruiken als er één gebruikersaccount worden gemaakt in de verificator. Voor het gebruik van de makelaar de ontwikkelaar nodig heeft voor het registreren van een speciale `redirectUri` voor de broker te gebruiken. `redirectUri`in de indeling van msauth://packagename/Base64UrlencodedSignature is. U krijgt `redirectUri` voor uw app met het script `brokerRedirectPrint.ps1` of met behulp van de API-aanroep `mContext.getBrokerRedirectUri()`. De handtekening is gerelateerd aan de handtekeningcertificaten uit de Google Play store.

 U kunt de gebruiker broker overslaan met behulp van:

    ```java
     AuthenticationSettings.Instance.setSkipBroker(true);
    ```
> [AZURE.NOTE] Ter vermindering van de complexiteit van deze B2C-Quickstart we ervoor hebt gekozen om over te slaan, de makelaar in ons voorbeeld.

Maak vervolgens de methoden die alleen de token tijdens verificatie aanroepen naar de API van de taak ophalen.

In dezelfde `ToDoActivity.java` het bestand, schrijft het volgende.

```
    private void getToken(final AuthenticationCallback callback) {

        String policy = getIntent().getStringExtra("thePolicy");

        // one of the acquireToken overloads
        mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES,
                policy, Constants.CLIENT_ID, Constants.REDIRECT_URL, getUserInfo(),
                PromptBehavior.Always, "nux=1&" + Constants.EXTRA_QP, callback);
    }
```

Methoden die 'set' en 'get' ook toevoegen `AuthenticationResult` (heeft uw token) naar de globale `Constants`. Hoewel `ToDoActivity.java` maakt gebruik van `sResult` in de stromen, moet u deze methoden toevoegen. Als u niet uw andere activiteiten geen toegang hebt tot het token voor werken (zoals het toevoegen van een taak in `AddTaskActivity.java`).

```

    private AuthenticationResult getLocalToken() {
        return Constants.CURRENT_RESULT;
    }

    private void setLocalToken(AuthenticationResult newToken) {


        Constants.CURRENT_RESULT = newToken;
    }


```
## <a name="create-a-method-to-return-a-user-identifier"></a>Een methode om terug te keren een gebruikers-id maken

ADAL voor Android de gebruiker in de vorm van vertegenwoordigt een `UserIdentifier` object. Hiermee beheert de gebruiker. U kunt het object te bepalen of de gebruiker wordt gebruikt in uw gesprekken. Met behulp van deze informatie, kunt u vertrouwen op de cache, plaats een nieuwe oproep tot de server. Wij gemaakt te vergemakkelijken, de `getUserInfo()` de methode geeft als resultaat `UserIdentifier`. U kunt deze met `acquireToken()`. Ook wij een `getUniqueId()` methode geeft als resultaat de ID van `UserIdentifier` in de cache.

```
  private String getUniqueId() {
        if (sResult != null && sResult.getUserInfo() != null
                && sResult.getUserInfo().getUniqueId() != null) {
            return sResult.getUserInfo().getUniqueId();
        }

        return null;
    }

    private UserIdentifier getUserInfo() {

        final TextView names = (TextView)findViewById(R.id.userLoggedIn);
        String name = names.getText().toString();
        return new UserIdentifier(name, UserIdentifier.UserIdentifierType.OptionalDisplayableId);
    }

```

### <a name="write-helper-methods"></a>Schrijven-methoden

Sommige methoden waarmee u cookies wissen en bieden vervolgens schrijven `AuthenticationCallback`. Deze methoden worden alleen gebruikt voor het monster om ervoor te zorgen dat je in een schone staat bij het aanroepen van de `ToDo` activiteit.

In hetzelfde bestand genaamd `ToDoActivity.java`, de volgende te schrijven.

```

    private void clearSessionCookie() {

        CookieManager cookieManager = CookieManager.getInstance();
        cookieManager.removeSessionCookie();
        CookieSyncManager.getInstance().sync();
    }
```

```
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        mAuthContext.onActivityResult(requestCode, resultCode, data);
    }

```   

## <a name="call-the-task-api"></a>De taak API aanroepen

Nadat u uw activiteiten kunt vastpakken tokens hebt, kunt u de API voor toegang tot de server van de taak.

`getTasks`bevat een matrix die de taken in uw server aangeeft.

Beginnen met `getTasks`.

In hetzelfde bestand genaamd `ToDoActivity.java`, de volgende te schrijven.

```
    private void getTasks() {
        if (sResult == null || sResult.getToken().isEmpty())
            return;

        List<String> items = new ArrayList<>();
        try {
            items = new TodoListHttpService().getAllItems(sResult.getToken());
        } catch (Exception e) {
            items = new ArrayList<>();
        }

        ListView listview = (ListView) findViewById(R.id.listViewToDo);
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,
                android.R.layout.simple_list_item_1, android.R.id.text1, items);
        listview.setAdapter(adapter);
    }

```

Schrijf ook een methode die de tabellen op de eerste uitvoering wordt geïnitialiseerd.

In hetzelfde bestand genaamd `ToDoActivity.java`, de volgende te schrijven.

```
    private void initAppTables() {
        try {
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);

        } catch (Exception e) {
            createAndShowDialog(new Exception(
                    "There was an error creating the Mobile Service. Verify the URL"), "Error");
        }
    }

```

U kunt zien dat deze code moet enkele aanvullende methoden zijn werk doen. Schrijf de volgende.

### <a name="create-an-endpoint-url-generator"></a>De generator van een eindpunt-URL maken

U moet de URL van het eindpunt dat u gaat aansluiten op genereren. Dit doen in het klassebestand met dezelfde.

**In hetzelfde bestand** genaamd `ToDoActivity.java`, de volgende te schrijven.

 ```
    private URL getEndpointUrl() {
        URL endpoint = null;
        try {
            endpoint = new URL(Constants.SERVICE_URL);
        } catch (MalformedURLException e) {
            e.printStackTrace();
        }
        return endpoint;
    }

 ```


Houd er rekening mee dat u het toegangstoken aan de aanvraag in de code in de volgende sectie besproken toevoegen.

## <a name="write-some-ux-methods"></a>Sommige methoden UX

Android, moet u voor het verwerken van sommige retouraanroepen voor het gebruik van de app. Dit zijn `createAndShowDialog` en `onResume()`. Dit is u vertrouwd voorkomen als u Android code voordat u hebt geschreven.

In hetzelfde bestand genaamd `ToDoActivity.java`, de volgende te schrijven.

```
    @Override
    public void onResume() {
        super.onResume(); // Always call the superclass method first

        updateLoggedInUser();
        // User can click logout, it will come back here
        // It should refresh list again
        getTasks();
    }


```

Vervolgens wordt het dialoogvenster terugbellen beheren.

In hetzelfde bestand genaamd `ToDoActivity.java`, de volgende te schrijven.

```
    /**
     * Creates a dialog and shows it
     *
     * @param exception The exception to show in the dialog
     * @param title     The dialog title
     */
    private void createAndShowDialog(Exception exception, String title) {
        createAndShowDialog(exception.toString(), title);
    }

    /**
     * Creates a dialog and shows it
     *
     * @param message The dialog message
     * @param title   The dialog title
     */
    private void createAndShowDialog(String message, String title) {
        AlertDialog.Builder builder = new AlertDialog.Builder(this);

        builder.setMessage(message);
        builder.setTitle(title);
        builder.create().show();
    }

```

U hebt nu een `ToDoActivity.java` -bestand dat wordt gecompileerd. Het hele project moet ook op dit punt compileren.

## <a name="run-the-sample-app"></a>De steekproef toepassing uitvoeren

Ten slotte bouwen en uitvoeren van de app in Android Studio of Eclips. Meld u aan of de app inloggen. Taken maken voor de gebruiker ingelogd. Afmelden en opnieuw aanmelden als een andere gebruiker en vervolgens taken maken voor die gebruiker.

U ziet dat de taken opgeslagen per gebruiker op de API, omdat de API haalt de identiteit van de gebruiker uit het toegangstoken dat wordt ontvangen.

Voor een verwijzing naar de voltooide monster [wordt geleverd als een ZIP-bestand](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip). U kunt ook het klonen van GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-Android```


## <a name="important-information"></a>Belangrijke informatie


### <a name="encryption"></a>Codering

ADAL codeert de tokens en winkel in `SharedPreferences` standaard. U kunt zoeken op de `StorageHelper` klasse om de details te bekijken. Android geïntroduceerd voor veilige opslag van persoonlijke sleutels **AndroidKeyStore voor 4.3(API18)** . ADAL gebruikt voor API18 en hoger. Als u ADAL gebruiken voor lagere versies van de SDK wilt, moet u een geheime sleutel op te geven `AuthenticationSettings.INSTANCE.setSecretKey`.

### <a name="session-cookies-in-web-view"></a>Cookies per sessie in de weergave

Android webweergave wordt cookies per sessie niet gewist nadat de app is gesloten. Met de volgende voorbeeldcode kunt u dit verwerken.
```
CookieSyncManager.createInstance(getApplicationContext());
CookieManager cookieManager = CookieManager.getInstance();
cookieManager.removeSessionCookie();
CookieSyncManager.getInstance().sync();
```
[Meer informatie over cookies](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).
