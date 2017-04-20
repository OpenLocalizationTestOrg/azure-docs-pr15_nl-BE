<properties
    pageTitle="Azure Active Directory B2C: Google + configuratie | Microsoft Azure"
    description="Aanmelden en aanmelden aan de consumenten met Google + accounts in uw toepassingen die worden beveiligd door Azure Active Directory B2C bieden."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-google-accounts"></a>Azure Active Directory B2C: Bieden aanmelding en aanmelden voor de consument met Google + accounts

## <a name="create-a-google-application"></a>Maak een Google +-toepassing

Voor het gebruik van Google + als een identiteitsprovider in B2C Azure Active Directory (AD Azure), moet u een Google +-toepassing maken en deze met de juiste parameters opgeven. U moet een Google +-account om dit te doen. Als u er geen hebt, vindt je het op [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).

1. Ga naar de [Google-ontwikkelaars Console](https://console.developers.google.com/) en aanmelden met uw referenties Google + account.
2. Klik op **project maken**, typ een **naam**en klik op **maken**.

    ![Google + - aan de slag](./media/active-directory-b2c-setup-goog-app/google-get-started.png)

    ![Google + - nieuw project](./media/active-directory-b2c-setup-goog-app/google-new-project.png)

3. Klik op **Beheer van de API** en klik op **referenties** in de linkernavigatiebalk.
4. Klik op het tabblad **OAuth scherm staan** boven.

    ![Google + - referenties](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)

5. Selecteren of geef een geldig **e-mailadres**, een **productnaam**en klik op **Opslaan**.

    ![Google + - OAuth toestemming scherm](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)

6. Klik op **nieuwe referenties** en kies vervolgens **OAuth client-ID**.

    ![Google + - OAuth toestemming scherm](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)

7. Selecteer onder **type toepassing** **webtoepassing**.

    ![Google + - OAuth toestemming scherm](./media/active-directory-b2c-setup-goog-app/google-web-app.png)

8. Geef een **naam** voor uw toepassing, voert u `https://login.microsoftonline.com` in het veld **oorsprong toegelaten JavaScript** en `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in het veld **geautoriseerde redirect URI's** . **{Huurder}** vervangen door de naam van de huurder (bijvoorbeeld contosob2c.onmicrosoft.com). De waarde **{huurder}** is hoofdlettergevoelig. Klik op **maken**.

    ![Google + - client-ID maken](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)

9. Kopieer de waarden van **De Client-ID** en het **geheim van de Client**. U moet beide configureren Google + als een identiteitsprovider in uw huurder. **Het geheim van de client** is een belangrijke.

    ![Google + - geheim van de Client](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a>Configureer Google + als een identiteitsprovider in uw huurder

1. Volg deze stappen om te [Navigeren naar de blade B2C-functies](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) op de Azure portal.
2. Klik op **id-providers**op de bladeserver B2C-functies.
3. Klik op **+ toevoegen** aan de bovenkant van het blad.
4. Een beschrijvende **naam** voor de configuratie van de provider identiteit bieden. Bijvoorbeeld, voer "G +".
5. Klik op **identiteit providertype** **Google**selecteren en klik op **OK**.
6. Klik op **deze identiteitsprovider instellen** en voer de ID van de client en het geheim van de client van de Google +-toepassing die u eerder hebt gemaakt.
7. Klik op **OK** en klik vervolgens op **maken** als u wilt uw Google + configuratie op te slaan.
