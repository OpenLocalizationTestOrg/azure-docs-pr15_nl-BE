<properties
    pageTitle="Azure Active Directory B2C: Extensible beleidskader | Microsoft Azure"
    description="Een onderwerp in het beleidskader van de extensible van Azure Active Directory B2C en voor het maken van verschillende soorten van het beleid"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-extensible-policy-framework"></a>Azure Active Directory B2C: Extensible beleidskader

## <a name="the-basics"></a>De grondbeginselen

De extensible beleidskader van B2C Azure Active Directory (AD Azure) is de sterkte van de kern van de service. Beleid volledig beschreven consument identiteit ervaringen zoals aanmelding aanmelden of profiel bewerken. Bijvoorbeeld, kunt een beleid aanmelding u gedrag beheren door de volgende instellingen configureren:

- Typen (sociale accounts zoals Facebook of lokale accounts, zoals e-mailadres) die consumenten zich aanmelden voor de toepassing kunnen gebruiken.
- Kenmerken (bijvoorbeeld naam, postcode en schoen grootte) te verzamelen van de consument tijdens het aanmelden.
- Het gebruik van een meerledige verificatie.
- De look-and-feel van alle aanmeldingspagina's.
- (Die zich voordoet als de claims in een token) die de aanvraag ontvangt wanneer het beleid uitvoeren is voltooid.

U kunt meerdere beleidsregels van verschillende typen in uw huurder maken en gebruiken in uw toepassingen naar wens. Beleid kunnen worden hergebruikt in toepassingen. Hierdoor kunnen ontwikkelaars definiëren en consument identiteit ervaringen met minimale of geen wijzigingen in de code wijzigen.

Beleidsregels zijn beschikbaar voor gebruik via een eenvoudige developer-interface. De toepassing activeert een beleid met behulp van een standaard HTTP-verificatie-aanvraag (geven een parameter van het beleid in de aanvraag) en een aangepaste token als reactie ontvangt. Bijvoorbeeld, het enige verschil tussen vraagt aanroepen van een beleid aanmelding en die een beleid-in te roepen is de naam van het beleid in de "p" queryreeksparameter gebruikt:

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up policy

```

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in policy

```

Zie voor meer informatie over het beleidskader, deze [blog posten](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx).

## <a name="create-a-sign-up-policy"></a>Maak een beleid voor aanmelding

Zodat de aanmelding van uw toepassing, moet u een beleid aanmelding maken. Dit beleid beschrijft de ervaringen die consumenten tijdens het aanmelden doorloopt en de inhoud van de tokens die de toepassing op een succesvol sign-ups zal ontvangen.

1. [Volg deze stappen om te navigeren naar de B2C functies blade van de Azure portal](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klik op **aanmelden bij beleid**.
3. Klik op **+ toevoegen** aan de bovenkant van het blad.
4. De **naam** bepaalt de beleidsnaam voor aanmelding gebruikt door de toepassing. Voer bijvoorbeeld "SiUp".
5. Klik op **id-providers** en selecteer "Email aanmelden". U kunt eventueel ook sociale identiteitsaanbieders, selecteren als al geconfigureerd. Klik op **OK**.
6. Klik op **aanmelden bij kenmerken**. Hier kunt u de kenmerken die u wilt verzamelen van de consument tijdens het aanmelden kiezen. Selecteer bijvoorbeeld 'Land', 'Weergavenaam' en 'Postcode'. Klik op **OK**.
7. Klik op **claims van toepassing**. Hier kunt u claims die u als resultaat in de tokens die zijn verzonden naar de toepassing na een geslaagde aanmelding ervaring wilt kiezen. Selecteer bijvoorbeeld 'Weergavenaam', 'Identity Provider', 'Postcode', 'Gebruiker is de nieuwe' en 'Object-ID van de gebruiker'.
8. Klik op **maken**. Merk op dat het zojuist gemaakte beleid wordt weergegeven als '**B2C_1_SiUp**' (de **B2C\_1\_ ** fragment wordt automatisch toegevoegd.) in de blade **-aanmelding beleid** .
9. Open het beleid door te klikken op '**B2C_1_SiUp**'.
10. Selecteer 'Contoso B2C app' in de vervolgkeuzelijst **toepassingen** en `https://localhost:44321/` in het **antwoord URL / Redirect URI** -omlaag.
11. Klik op **nu uitvoeren**. Een nieuwe browser tab geopend en kunt u uitvoeren via de ervaring van de consument van het aanmelden voor uw toepassing.

    > [AZURE.NOTE]
    Het neemt een minuut voor het maken van beleid en updates worden pas van kracht.

## <a name="create-a-sign-in-policy"></a>Maak een beleid voor aanmelden

Als u wilt aanmelden bij uw toepassing inschakelen, moet u een beleid-in. Dit beleid beschrijft de ervaringen die consumenten doorloopt tijdens het aanmelden en de inhoud van de tokens die de aanvraag ontvangt op succesvolle aanmeldingen.

1. [Volg deze stappen om te navigeren naar de B2C functies blade van de Azure portal](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klik op **beleid voor aanmelden**.
3. Klik op **+ toevoegen** aan de bovenkant van het blad.
4. De **naam** bepaalt de naam van het teken in beleid door de toepassing gebruikt. Voer bijvoorbeeld "SiIn".
5. Klik op **id-providers** en selecteer 'Lokale Account inloggen'. U kunt eventueel ook sociale identiteitsaanbieders, selecteren als al geconfigureerd. Klik op **OK**.
6. Klik op **claims van toepassing**. Hier kunt u claims die u als resultaat in de tokens die worden verzonden naar uw toepassing na een succesvolle ervaring-in wilt kiezen. Selecteer bijvoorbeeld 'Weergavenaam', 'Identity Provider', 'Postcode' en 'Object-ID van de gebruiker'. Klik op **OK**.
7. Klik op **maken**. Merk op dat het zojuist gemaakte beleid wordt weergegeven als '**B2C_1_SiIn**' (de **B2C\_1\_ ** fragment wordt automatisch toegevoegd.) in de blade **- in beleid** .
8. Open het beleid door te klikken op '**B2C_1_SiIn**'.
9. Selecteer 'Contoso B2C app' in de vervolgkeuzelijst **toepassingen** en `https://localhost:44321/` in het **antwoord URL / Redirect URI** -omlaag.
10. Klik op **nu uitvoeren**. Een nieuwe browser tab geopend en kunt u uitvoeren via de ervaring van de consument van ondertekening in uw toepassing.

    > [AZURE.NOTE]
    Het neemt een minuut voor het maken van beleid en updates worden pas van kracht.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Maak een beleid voor aanmelden of aanmelden

Dit beleid zorgt voor beide ervaringen consument aanmelding & aanmelden met een enkele configuratie. Consumenten worden geleid naar het juiste pad (aanmelden of aanmelden) zijn afhankelijk van de context. Hierin worden ook de inhoud van de tokens die de aanvraag bij succesvolle sign-ups of aanmeldingen ontvangt.  Een voorbeeld van code voor het aanmelden of aanmelden beleid is [hier beschikbaar](active-directory-b2c-devquickstarts-web-dotnet-susi.md).

1. [Volg deze stappen om te navigeren naar de B2C functies blade van de Azure portal](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klik op **aanmelden of aanmelden beleid**.
3. Klik op **+ toevoegen** aan de bovenkant van het blad.
4. De **naam** bepaalt de beleidsnaam voor aanmelding gebruikt door de toepassing. Voer bijvoorbeeld "SiUpIn".
5. Klik op **id-providers** en selecteer "Email aanmelden". U kunt eventueel ook sociale identiteitsaanbieders, selecteren als al geconfigureerd. Klik op **OK**.
6. Klik op **aanmelden bij kenmerken**. Hier kunt u de kenmerken die u wilt verzamelen van de consument tijdens het aanmelden kiezen. Selecteer bijvoorbeeld 'Land', 'Weergavenaam' en 'Postcode'. Klik op **OK**.
7. Klik op **claims van toepassing**. Hier kunt u claims die u als resultaat in de tokens die zijn verzonden naar de toepassing na een geslaagde aanmelding of -in-ervaring wilt kiezen. Selecteer bijvoorbeeld 'Weergavenaam', 'Identity Provider', 'Postcode', 'Gebruiker is de nieuwe' en 'Object-ID van de gebruiker'.
8. Klik op **maken**. Merk op dat het zojuist gemaakte beleid wordt weergegeven als '**B2C_1_SiUpIn**' (de **B2C\_1\_ ** fragment wordt automatisch toegevoegd.) in de blade **beleid aanmelden of aanmelden** .
9. Open het beleid door te klikken op '**B2C_1_SiUpIn**'.
10. Selecteer 'Contoso B2C app' in de vervolgkeuzelijst **toepassingen** en `https://localhost:44321/` in het **antwoord URL / Redirect URI** -omlaag.
11. Klik op **nu uitvoeren**. Een nieuw tabblad in de browser wordt geopend en u kunt uitvoeren via het inschrijfformulier of aanmelden consument ervaring zoals deze is geconfigureerd.

    > [AZURE.NOTE]
    Het neemt een minuut voor het maken van beleid en updates worden pas van kracht.

## <a name="create-a-profile-editing-policy"></a>Maak een profiel beleid bewerken

Zodat het profiel bewerken op uw toepassing moet u een profiel bewerkt beleid te maken. Dit beleid beschrijft de ervaringen die consumenten doorloopt tijdens het bewerken van het profiel en de inhoud van de tokens die de toepassing voor voltooiing krijgt.

1. [Volg deze stappen om te navigeren naar de B2C functies blade van de Azure portal](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klik op **profiel bewerken van beleid**.
3. Klik op **+ toevoegen** aan de bovenkant van het blad.
4. De **naam van** bepaalt het profiel bewerken de naam gebruikt door de toepassing van beleid. Voer bijvoorbeeld "SiPe".
5. Klik op **id-providers** en selecteer 'E-mailadres'. U kunt eventueel ook sociale identiteitsaanbieders, selecteren als al geconfigureerd. Klik op **OK**.
6. Klik op **profiel kenmerken**. Hier kunt u kenmerken die de consument te kiezen. Selecteer bijvoorbeeld 'Land', 'Weergavenaam' en 'Postcode'. Klik op **OK**.
7. Klik op **claims van toepassing**. Hier kunt u claims die u wilt opvragen in de tokens die zijn verzonden naar de toepassing na een geslaagde profiel bewerken kiezen. Selecteer bijvoorbeeld 'Weergavenaam' en 'Postcode'.
8. Klik op **maken**. Merk op dat het zojuist gemaakte beleid wordt weergegeven als '**B2C_1_SiPe**' (de **B2C\_1\_ ** fragment wordt automatisch toegevoegd.) in het **profiel bewerken beleid** blade.
9. Open het beleid door te klikken op '**B2C_1_SiPe**'.
10. Selecteer 'Contoso B2C app' in de vervolgkeuzelijst **toepassingen** en `https://localhost:44321/` in het **antwoord URL / Redirect URI** -omlaag.
11. Klik op **nu uitvoeren**. Een nieuwe browser tab geopend en kunt u uitvoeren via het profiel bewerken consument ervaring in uw toepassing.

    > [AZURE.NOTE]
    Het neemt een minuut voor het maken van beleid en updates worden pas van kracht.
    
## <a name="create-a-password-reset-policy"></a>Maak een beleid voor wachtwoord opnieuw instellen

Om fijnmazig wachtwoord voor uw toepassing opnieuw inschakelen, moet u een wachtwoord opnieuw instellen van beleid maken. Dat de huurder het hele wachtwoord opgegeven optie Opmerking [hier](active-directory-b2c-reference-sspr.md) nog steeds van toepassing is op de beleid-in. Dit beleid beschrijft de ervaringen die de consumenten doorloopt tijdens het opnieuw instellen van wachtwoorden en de inhoud van de tokens die de toepassing voor voltooiing krijgt.

1. [Volg deze stappen om te navigeren naar de B2C functies blade van de Azure portal](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klik op **wachtwoord opnieuw instellen van beleid**.
3. Klik op **+ toevoegen** aan de bovenkant van het blad.
4. De **naam** bepaalt dat het wachtwoord opnieuw instellen de naam gebruikt door de toepassing van beleid. Voer bijvoorbeeld "SSPR".
5. Klik op **id-providers** en selecteer 'Reset wachtwoord met behulp van e-mailadres'. Klik op **OK**.
6. Klik op **claims van toepassing**. Hier kiest u claims die u opvragen in de tokens wilt na een geslaagde wachtwoord opnieuw ervaring terug naar de toepassing verzonden. Selecteer bijvoorbeeld "Object-ID van de gebruiker".
7. Klik op **maken**. Merk op dat het zojuist gemaakte beleid wordt weergegeven als '**B2C_1_SSPR**' (de **B2C\_1\_ ** fragment wordt automatisch toegevoegd.) in het **beleid voor wachtwoord** -blade.
8. Open het beleid door te klikken op '**B2C_1_SSPR**'.
9. Selecteer 'Contoso B2C app' in de vervolgkeuzelijst **toepassingen** en `https://localhost:44321/` in het **antwoord URL / Redirect URI** -omlaag.
10. Klik op **nu uitvoeren**. Een nieuwe browser tab geopend en kunt u uitvoeren via het wachtwoord opnieuw instellen gebruiken in uw toepassing.

    > [AZURE.NOTE]
    Het neemt een minuut voor het maken van beleid en updates worden pas van kracht.

## <a name="additional-resources"></a>Aanvullende bronnen

- [Token, sessie en enkele aanmelding-configuratie](active-directory-b2c-token-session-sso.md).
