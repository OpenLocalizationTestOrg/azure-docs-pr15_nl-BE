<properties
    pageTitle="Het inschakelen van SSO cross-app op iOS met ADAL | Microsoft Azure"
    description="Het gebruik van de functies van de ADAL SDK voor eenmalige aanmelding inschakelen voor uw toepassingen. "
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="how-to-enable-cross-app-sso-on-ios-using-adal"></a>Het inschakelen van SSO cross-app op iOS met ADAL


Mits de Single Sign-On (SSO) zodat gebruikers alleen moeten één keer hun referenties invoeren en deze referenties automatisch laten werken via toepassingen nu verwacht door klanten. Het probleem bij het invoeren van hun gebruikersnaam en wachtwoord op een klein scherm, vaak tijden, gecombineerd met een bijkomende factor (2FA), zoals een telefoongesprek of een texted code resulteert in snelle ontevredenheid als een gebruiker heeft om dit te doen meer dan één keer voor uw product.

Bovendien, als u gebruikmaken van een platform voor identiteit die van andere toepassingen zoals Microsoft Accounts of een account van het werk van Office365 gebruikmaken mogelijk, verwachten klanten dat de referenties die moeten worden gebruikt in hun toepassingen, ongeacht de leverancier.

Het platform Microsoft Identity, samen met onze SDK's van Microsoft Identity al dit harde werk voor u doet en biedt u de mogelijkheid om uw klanten met eenmalige aanmelding aangenaam verrassen hetzij in uw eigen suite van toepassingen of, net als bij onze broker mogelijkheden en toepassingen van de verificator, over het hele apparaat.

In dit scenario wordt uitgelegd hoe u voor het configureren van onze SDK in de toepassing van deze vergoeding aan uw klanten levert.

Deze procedure is van toepassing op:

* Azure Active Directory
* B2C Azure Active Directory
* B2B Azure Active Directory
* Azure Active Directory voorwaardelijke toegang


Houd er rekening mee dat het onderstaande document wordt ervan uitgegaan dat u kennis hebt van toepassingen [in de oude portal voor Azure Active Directory dient](./develop/active-directory-how-to-integrate.md) als uw toepassing hebt geïntegreerd met de [Microsoft Identity iOS SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc).

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a>SSO-concepten in het Platform Microsoft Identity

### <a name="microsoft-identity-brokers"></a>Microsoft Identity makelaars

Microsoft biedt toepassingen voor elke mobiel platform voor het overbruggen van de referenties van alle toepassingen van andere leveranciers als kunt u speciale verbeterde functies waarvoor één veilige plaats waar voor het valideren van de referenties. We noemen deze **makelaars**. Op iOS en Android zijn deze opgenomen in de downloadbare toepassingen dat klanten afzonderlijk te installeren of door een vennootschap die geheel of gedeeltelijk van het apparaat voor de werknemer beheert kunnen worden geduwd aan het apparaat. Deze makelaars ondersteuning voor beheren van beveiliging voor bepaalde toepassingen of het hele apparaat op basis van wat de IT-beheerders wensen. In Windows wordt deze functionaliteit geleverd door een kiezer account is ingebouwd in het besturingssysteem, technisch als de Broker Web-verificatie genoemd.

Om te begrijpen hoe we deze makelaars wilt gebruiken en hoe uw klanten kunnen zien in hun stroom aanmelden voor het Microsoft Identity platform Lees verder voor meer informatie.

### <a name="patterns-for-logging-in-on-mobile-devices"></a>Patronen voor logboekregistratie op mobiele apparaten

Toegang tot referenties op apparaten als volgt twee elementaire patronen voor het Microsoft Identity-platform:

* Met behulp van telefonist(e) aanmeldingen niet broker
* Met behulp van telefonist(e) aanmeldingen Broker

#### <a name="non-broker-assisted-logins"></a>Met behulp van telefonist(e) aanmeldingen niet broker

Non-broker assisted aanmeldingen zijn aanmelding ervaringen die gebeuren in lijn met de toepassing en het gebruik van lokale opslag op het apparaat voor de toepassing. Deze opslag in toepassingen kan worden gedeeld, maar de referenties zijn nauw verbonden met de app of suite van toepassingen met behulp van de referenties die. Dit is de ervaring die waarschijnlijk opgetreden in veel mobiele toepassingen waarin u een gebruikersnaam en wachtwoord in de toepassing zelf invoeren.

Deze aanmeldingen hebben de volgende voordelen:

-  Ervaring van de gebruiker bestaat geheel binnen de toepassing.
-  Referenties kunnen worden gedeeld met toepassingen die zijn ondertekend door een enkele aanmelding-ervaring naar uw suite van toepassingen bieden hetzelfde certificaat.
-  Controle om de ervaring van de logboekfunctie wordt geleverd om de toepassing vóór en na het aanmelden.

Deze aanmeldingen hebben de volgende nadelen:

- Gebruiker kan geen eenmalige aanmelding optreden in alle toepassingen die gebruikmaken van een Microsoft Identity alleen via de Microsoft Identities die eigenaar is van de toepassing en de hebt geconfigureerd.
- Uw toepassing kan niet worden gebruikt met meer geavanceerde zakelijke functies zoals voorwaardelijke toegang of gebruik de InTune-productsuite.
- Uw toepassing kan op basis van certificaten voor zakelijke gebruikers die niet ondersteunen.

Dit is een voorstelling van hoe de identiteit van Microsoft SDK's in combinatie met de gedeelde opslag van uw toepassingen voor eenmalige aanmelding inschakelen:

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| Azure SDK  | | Azure SDK  |  | Azure SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### <a name="broker-assisted-logins"></a>Met behulp van telefonist(e) aanmeldingen Broker

Broker-gesteunde aanmeldingen zijn aanmelding ervaringen die optreden in de toepassing van de makelaar en de opslag en beveiliging van de broker gebruiken voor het delen van referenties voor alle toepassingen die gebruikmaken van het platform Microsoft Identity op het apparaat. Dit betekent dat uw toepassingen afhankelijk van de broker zijn zullen gebruikers voor aanmelding. Op iOS en Android zijn deze opgenomen in de downloadbare toepassingen dat klanten afzonderlijk installeren of naar het apparaat kunnen worden geduwd door een bedrijf die het apparaat voor de gebruiker beheert. Een voorbeeld van dit type toepassing is de toepassing van de verificator Azure op iOS. In Windows wordt deze functionaliteit geleverd door een kiezer account is ingebouwd in het besturingssysteem, technisch als de Broker Web-verificatie genoemd.
De ervaring verschilt per platform en kan soms storend voor gebruikers zijn niet goed wordt beheerd. U bent waarschijnlijk meest bekend met dit patroon als u de Facebook-toepassing geïnstalleerd en Facebook Login functionaliteit in een andere toepassing gebruikt. Het platform Microsoft Identity maakt gebruik van hetzelfde patroon.

Voor dit tot een 'overgang leidt' iOS komt animatie waarin uw toepassing wordt verzonden naar de achtergrond terwijl de verificator Azure toepassingen naar de voorgrond van de gebruiker te selecteren die ze aanmelden willen met account.  

Voor Android en Windows is op uw toepassing minder storend voor de gebruiker die de kiezer account weergegeven.

#### <a name="how-the-broker-gets-invoked"></a>Hoe de broker wordt aangeroepen

Als een compatibel broker is geïnstalleerd op het apparaat, zoals de toepassing van de verificator Azure, wordt de identiteit van Microsoft SDK's de werkzaamheden van de makelaar voor u aanroepen wanneer een gebruiker aangeeft dat zij wensen aan te melden met een account van het platform Microsoft Identity automatisch doen. Dit kan bijvoorbeeld een persoonlijke Microsoft-Account, een werk of school-account of een account die u opgeeft en host in Azure met behulp van onze B2B- en B2C-producten. Met behulp van zeer veilige algoritmen en codering die wij ervoor zorgen dat de referenties gevraagd en geleverd terug naar de toepassing op een veilige manier. De exacte technische details van deze mechanismen niet is gepubliceerd, maar met samenwerking zijn ontwikkeld door Apple en Google.

**De ontwikkelaar heeft de keuze van als de SDK van Microsoft Identity de makelaar roept of de niet-broker assisted stroom gebruikt.** Als de ontwikkelaar niet wil gebruiken de broker-gesteunde stroom verliezen ze echter het voordeel van het gebruik van de SSO-referenties die de gebruiker kan al op het apparaat hebt toegevoegd en wordt voorkomen dat de toepassing wordt gebruikt met Microsoft haar klanten zoals voorwaardelijke toegang, Intune, mogelijkheden biedt functies voor zakelijke en verificatie op basis van certificaten.

Deze aanmeldingen hebben de volgende voordelen:

-  Eenmalige aanmelding ervaringen gebruiker in hun toepassingen, ongeacht de leverancier.
-  Uw toepassing kan gebruikmaken van geavanceerde bedrijfsfuncties zoals voorwaardelijke toegang of het pakket InTune.
-  Uw toepassing kan op basis van verificatie ondersteunen voor zakelijke gebruikers.
- Veel veiliger-in-ervaring als de identiteit van de toepassing en de gebruiker worden geverifieerd door de toepassing van de broker met algoritmen voor extra beveiliging en codering.

Deze aanmeldingen hebben de volgende nadelen:

- In iOS de gebruiker uit de ervaring van uw toepassing overgestapt, terwijl de referenties worden gekozen.
- Verlies van de mogelijkheid om de aanmelding ervaring voor uw klanten binnen de toepassing te beheren.



Dit is een voorstelling van hoe de identiteit van Microsoft SDK's in combinatie met de broker-toepassingen voor eenmalige aanmelding inschakelen:

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
| Azure SDK  | | Azure SDK  |   | Azure SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+
```

Gewapend met deze achtergrondinformatie die moet mogelijk zijn om beter te begrijpen en SSO implementeren binnen uw toepassing met behulp van Microsoft Identity platform en SDK's.


## <a name="enabling-cross-app-sso-using-adal"></a>Cross-app SSO met ADAL inschakelen

Hier gebruiken we het ADAL iOS SDK naar:

- Assisted SSO niet broker voor uw suite van toepassingen inschakelen
- Ondersteuning voor broker via eenmalige aanmelding inschakelen

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Eenmalige aanmelding inschakelen voor niet-broker via eenmalige aanmelding

Assisted SSO niet broker alle toepassingen beheren de identiteit van Microsoft SDK's voor veel van de complexiteit van eenmalige aanmelding voor u. Dit omvat het zoeken naar de juiste gebruiker in de cache en onderhouden van een lijst met aangemelde gebruikers kunt opvragen.

Eenmalige aanmelding inschakelen in toepassingen die u bezit, moet u het volgende doen:

1. Alle gebruikers van uw toepassingen garanderen hetzelfde Client-ID of id
* Zorg ervoor dat alle toepassingen dezelfde handtekeningcertificaat van Apple delen zodat u kunt keychains delen
* Verzoekt de dezelfde sleutelketen recht voor elk van uw toepassingen.
* De identiteit van Microsoft SDK's vertellen over de sleutelketen gedeelde dat u wilt gebruiken.

#### <a name="using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a>Met behulp van dezelfde Client-ID / toepassing-ID voor de toepassingen in uw suite van apps

Om het platform Microsoft Identity te weten dat het is toegestaan om tokens delen tussen alle toepassingen, moet elk van uw toepassingen delen hetzelfde Client-ID of id Dit is de unieke id die aan u is geleverd bij het registreren van de eerste toepassing in de portal.

U vraagt zich misschien af hoe u geeft verschillende apps met de service Microsoft Identity als het dezelfde id gebruikt Het antwoord is met het **Omleiden van URI's**. Elke toepassing kan meerdere omleiden van URI's in de portal onboarding geregistreerd zijn. Elke app in uw suite heeft een andere URI-omleiding. Een voorbeeld van hoe dit er uitziet is hieronder:

1 omleiden van URI:`x-msauth-mytestiosapp://com.myapp.mytestapp`

2-Redirect URI`x-msauth-mytestiosapp://com.myapp.mytestapp2`

App3 omleiden van URI:`x-msauth-mytestiosapp://com.myapp.mytestapp3`

....

Deze zijn genest onder dezelfde client-ID / toepassings-ID en opgezocht op basis van het omleiden van URI die u aan ons terug in de configuratie van de SDK.

```
+-------------------+
|                   |
|  Client ID        |
+---------+---------+
          |
          |           +-----------------------------------+
          |           |  App 1 Redirect URI               |
          +----------^+                                   |
          |           +-----------------------------------+
          |
          |           +-----------------------------------+
          +----------^+  App 2 Redirect URI               |
          |           |                                   |
          |           +-----------------------------------+
          |
          +----------^+-----------------------------------+
                      |  App 3 Redirect URI               |
                      |                                   |
                      +-----------------------------------+

```


*Houd er rekening mee dat de indeling van deze Redirect URI hieronder worden beschreven. U kunt een Redirect URI tenzij u wilt ondersteunen de makelaar, in welk geval ze moeten er ongeveer zo uitzien het bovenstaande*



#### <a name="create-keychain-sharing-between-applications"></a>Sleutelketen delen met toepassingen maken

Inschakelen van het delen van de sleutelketen valt buiten het bestek van dit document en Apple vallen in hun document [Mogelijkheden toe te voegen](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html). Wat belangrijk is, is dat u bepalen wat de sleutelketen worden aangeroepen en die mogelijkheid over al uw toepassingen toe te voegen.

Wanneer u beschikt over rechten stellen juist u ziet een bestand in de projectmap recht `entitlements.plist` die iets dat op het volgende lijkt bevat:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>keychain-access-groups</key>
    <array>
        <string>$(AppIdentifierPrefix)com.myapp.mytestapp</string>
        <string>$(AppIdentifierPrefix)com.myapp.mycache</string>
    </array>
</dict>
</plist>
```

Zodra de sleutelketen recht in elk van uw toepassingen is ingeschakeld en u klaar bent voor gebruik van de SSO vertellen de SDK van Microsoft Identity de sleutelketen met behulp van de volgende instelling in de `ADAuthenticationSettings` met de volgende instellingen:

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [AZURE.WARNING]
Wanneer u uw toepassingen een sleutelketen delen kan elke toepassing of verwijderen gebruikers erger alle tokens in uw toepassing. Dit is met name rampzalige toepassingen die afhankelijk zijn van de tokens werk op de achtergrond. Delen van een sleutelketen verwijderen betekent dat u moet voorzichtig in alle activiteiten door middel van de identiteit van Microsoft SDK's.

Dat is alles. De Microsoft SDK voor identiteit wordt nu referenties over al uw toepassingen delen. De lijst met gebruikers wordt ook in de toepassingsexemplaren worden gedeeld.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Eenmalige aanmelding inschakelen voor broker bijgestaan SSO

De mogelijkheid voor een toepassing voor het gebruik van een makelaar die is geïnstalleerd op het apparaat is **standaard uitgeschakeld**. U kunt uw toepassing met de broker gebruiken moet u enkele aanvullende instellingen en code toevoegen aan uw toepassing.

Er zijn de volgende stappen uitvoeren:

1. Broker-modus inschakelen in de toepassingscode aanroepen van de MS SDK.
2. Tot stand brengen van een nieuwe redirect URI en bieden die aan de app en de registratie van uw app.
3. Registratie van URL-schema.
4. Ondersteuning van iOS9: een machtiging toevoegen aan het info.plist-bestand.


#### <a name="step-1-enable-broker-mode-in-your-application"></a>Stap 1: Broker-modus inschakelen in uw toepassing
De mogelijkheid voor uw toepassing de broker is ingeschakeld wanneer u de 'context' of de eerste installatie van uw verificatie-object. Dit doen u door uw type referenties in uw code:

```
/*! See the ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
De `AD_CREDENTIALS_AUTO` de instelling de identiteit Microsoft SDK om te proberen om zich aan de makelaar, kunnen `AD_CREDENTIALS_EMBEDDED` wordt voorkomen dat de SDK van Microsoft Identity bellen met de makelaar.

#### <a name="step-2-registering-a-url-scheme"></a>Stap 2: Registratie van URL-schema
Het platform Microsoft Identity URL's gebruikt voor het aanroepen van de broker en vervolgens terugkeren naar de toepassing. U moet een URL-schema geregistreerd voor de toepassing waarin het platform Microsoft Identity weet over om te voltooien die u retouren. Dit is naast alle andere app's die mogelijk al geregistreerd bij uw toepassing.

> [AZURE.WARNING]
Is het raadzaam om de URL-schema vrij uniek zijn voor het minimaliseren van de kans op een andere toepassing met gebruik van de dezelfde URL-schema. Apple heeft niet de uniciteit van URL-schema's die zijn geregistreerd in de app store.

Hier volgt een voorbeeld van hoe deze wordt weergegeven in de projectconfiguratie van uw. U kunt dit ook doen in XCode ook:

```
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.myapp.mytestapp</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>x-msauth-mytestiosapp</string>
        </array>
    </dict>
</array>
```

#### <a name="step-3-establish-a-new-redirect-uri-with-your-url-scheme"></a>Stap 3: Een nieuwe redirect URI met de URL-schema maken

We moeten ervoor zorgen dat we de referentie-tokens altijd naar de juiste toepassing terugkeren, zorg noemen we terug naar de toepassing op een manier die u kunt controleren of het besturingssysteem iOS. Het besturingssysteem iOS rapporteert aan de broker-toepassingen van Microsoft de bundel-ID van de toepassing de methode aanroept. Dit kan niet worden vervalst door een rogue-toepassing. Daarom gebruiken we dit samen met de URI van de toepassing van onze makelaar om ervoor te zorgen dat de tokens worden geretourneerd naar de juiste toepassing. U moet deze unieke redirect URI, zowel in uw toepassing vast te stellen en een Redirect URI in onze developer-portal is vereist.

Het omleiden van URI moet in de juiste vorm van:

`<app-scheme>://<your.bundle.id>`

ex: *x-msauth-mytestiosapp://com.myapp.mytestapp*

Deze URI omleiden moet worden opgegeven in de registratie van uw app met de [Azure klassieke portal](https://manage.windowsazure.com/). Zie voor meer informatie over de registratie van Azure AD app, [integratie met Azure Active Directory](./develop/active-directory-how-to-integrate.md).


##### <a name="step-3a-add-a-redirect-uri-in-your-app-and-dev-portal-to-support-certificate-based-authentication"></a>Stap 3a: een redirect URI toevoegen in uw app en dev portal voor ondersteuning van verificatie op basis van certificaten

Ter ondersteuning van cert gebaseerde verificatie die een tweede 'msauth' dient te worden geregistreerd in uw toepassing en de [Azure klassieke portal](https://manage.windowsazure.com/) voor het verwerken van verificatie als u wilt dat er geen ondersteuning toevoegen in uw toepassing.

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

ex: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*


#### <a name="step-4-ios9-add-a-configuration-parameter-to-your-app"></a>Stap 4: iOS9: een configuratieparameter toevoegen aan uw app

ADAL – canOpenURL gebruikt: om te controleren of de makelaar op het apparaat is geïnstalleerd. In iOS vergrendeld 9 Apple wat regelingen toepassingen kunnen opvragen. U moet 'msauth' toevoegen aan de sectie LSApplicationQueriesSchemes van de `info.plist file`.

<key>LSApplicationQueriesSchemes</key>
<array>
     <string>msauth</string>
</array>

### <a name="youve-configured-sso"></a>U kunt eenmalige aanmelding hebt geconfigureerd.

Nu de SDK van Microsoft Identity automatisch zowel delen van referenties in uw toepassingen en aanroepen van de broker als op het apparaat aanwezig is.
