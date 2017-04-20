<properties
    pageTitle="Active Directory B2C Azure: Configuratie Facebook | Microsoft Azure"
    description="Aanmelden en inloggen voor consumenten met een Facebook-account in uw toepassingen die worden beveiligd door Azure Active Directory B2C bieden."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-facebook-accounts"></a>Azure Active Directory B2C: Bieden aanmelding en aanmelden voor consumenten met een Facebook-account

## <a name="create-a-facebook-application"></a>Een Facebook-toepassing maken

Gebruik Facebook als een identiteitsprovider in B2C Azure Active Directory (AD Azure), moet u een Facebook-toepassing maken en deze met de juiste parameters opgeven. U moet een Facebook-account om dit te doen. Als u er geen hebt, krijgt u het op [https://www.facebook.com/](https://www.facebook.com/).

1. Ga naar de website van [Facebook voor ontwikkelaars](https://developers.facebook.com/) en meld u aan met de referenties van uw Facebook-account.
2. Als u dat nog niet hebt gedaan, moet u registreren als ontwikkelaar Facebook. Hiertoe klikt u op **registreren** (in de rechterbovenhoek van de pagina) de registratie stappen en accepteren van Facebook beleid.
3. **Mijn Apps** op en klik vervolgens op **nieuwe toepassing toevoegen**. **Website** als het platform kiest en klikt u vervolgens op **overslaan en App-ID maken**.

    ![Facebook - een nieuwe toepassing toevoegen](./media/active-directory-b2c-setup-fb-app/fb-add-new-app.png)

    ![Facebook - een nieuwe App - Website toevoegen](./media/active-directory-b2c-setup-fb-app/fb-add-new-app-website.png)

    ![Facebook - App-ID maken](./media/active-directory-b2c-setup-fb-app/fb-new-app-skip.png)

4. Op het formulier, een **Weergavenaam**, een geldig **E-mailadres van contactpersoon**, een **categorie**, en klik op **App-ID maken**. Hiervoor moet u Facebook platform beleid accepteren en uitvoeren van een online controle.

    ![Facebook - een nieuwe App-ID maken](./media/active-directory-b2c-setup-fb-app/fb-create-app-id.png)

5. Klik op **Instellingen** op de navigatiebalk aan de linkerkant.
6. Klik op **+ Platform toevoegen** en selecteer vervolgens de **Website**.

    ![Facebook - instellingen](./media/active-directory-b2c-setup-fb-app/fb-settings.png)

    ![Facebook - instellingen - Website](./media/active-directory-b2c-setup-fb-app/fb-website.png)

7. [Https://login.microsoftonline.com/](https://login.microsoftonline.com/) invoeren in het veld **URL van de Site** en klik vervolgens op **Wijzigingen opslaan**.

    ![Facebook - URL van de Site](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)

8. Kopieer de waarde van **App-ID**. Klik op **weergeven** en de waarde van **App geheim**. U moet beide Facebook configureren als een identiteitsprovider in uw huurder. **App Secret** is een belangrijke.

    ![Facebook - App ID & geheim App](./media/active-directory-b2c-setup-fb-app/fb-app-id-app-secret.png)

9. Klik op **+ Product toevoegen** op de navigatiebalk aan de linkerkant en klik vervolgens op de knop **Aan de slag** naast **Facebook aanmelden**.

    ![Facebook - Facebook Login](./media/active-directory-b2c-setup-fb-app/fb-login.png)

10. Voer `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in het veld **geldig OAuth omleiden van URI's** in de sectie **Instellingen voor Client OAuth** . **{Huurder}** vervangen door de naam van de huurder (bijvoorbeeld contosob2c.onmicrosoft.com). Klik op **Wijzigingen opslaan** onder aan de pagina.

    ![Facebook - OAuth Redirect URI](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)

11. Als u wilt uw Facebook-toepassing kan worden gebruikt door AD-B2C Azure, moet u het openbaar maken. U kunt dit doen door de **App bekijken** op de navigatiebalk aan de linkerkant te klikken en in te schakelen van de schakelaar aan de bovenkant van de pagina op **Ja** en op **bevestigen**te klikken.

    ![Facebook - App openbaar](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## <a name="configure-facebook-as-an-identity-provider-in-your-tenant"></a>Facebook configureren als een identiteitsprovider in uw huurder

1. Volg deze stappen om te [Navigeren naar de blade B2C-functies](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) op de Azure portal.
2. Klik op **id-providers**op de bladeserver B2C-functies.
3. Klik op **+ toevoegen** aan de bovenkant van het blad.
4. Een beschrijvende **naam** voor de configuratie van de provider identiteit bieden. Voer bijvoorbeeld "FB".
5. Klik op **identiteit providertype** **Facebook**selecteren en klik op **OK**.
6. Klik op **deze identiteitsprovider instellen** en voer de ID van de app en de app secret (van de Facebook-toepassing die u eerder hebt gemaakt) in de **Client-ID** en het **geheim van de Client** de velden respectievelijk.
7. Klik op **OK**en klik vervolgens op **maken** als u wilt uw Facebook-configuratie op te slaan.
