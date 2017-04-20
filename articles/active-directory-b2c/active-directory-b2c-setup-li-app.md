<properties
    pageTitle="Active Directory B2C Azure: Configuratie LinkedIn | Microsoft Azure"
    description="Aanmelden en inloggen voor consumenten met een LinkedIn-account in uw toepassingen die worden beveiligd door Azure Active Directory B2C bieden"
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-linkedin-accounts"></a>Azure Active Directory B2C: Bieden aanmelding en aanmelden voor consumenten met een LinkedIn-account

## <a name="create-a-linkedin-application"></a>Een LinkedIn-toepassing maken

Voor het gebruik van LinkedIn als een identiteitsprovider in B2C Azure Active Directory (AD Azure), moet u een toepassing voor LinkedIn en verstrekt haar de juiste parameters. U moet hiervoor een LinkedIn-account. Als u er geen hebt, vindt je het op [https://www.linkedin.com/](https://www.linkedin.com/).

1. Ga naar de [website van LinkedIn, ontwikkelaars](https://www.developer.linkedin.com/) en log in met uw LinkedIn accountreferenties.
2. **Mijn Apps** in de bovenste menubalk en klik op **Toepassing maken**.

    ![LinkedIn - nieuwe app](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)

3. Vul in het formulier **een nieuwe toepassing maken** , relevante informatie (**Bedrijfsnaam**, **naam**, **Beschrijving**, **URL van Logo**, **Gebruik van toepassing**, **Website-URL**, **Zakelijke e-mailadres** en **Telefoonnummer**).
4. Het **LinkedIn API gebruiksvoorwaarden** accepteren en klik op **verzenden**.

    ![LinkedIn - kassa app](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)

5. Kopieer de waarden van **De Client-ID** en het **Geheim van de Client**. (U vindt deze onder **Verificatiesleutels**.) U moet beide LinkedIn configureren als een identiteitsprovider in uw huurder.

    >[AZURE.NOTE] **Het geheim van de client** is een belangrijke.

6. Voer `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in het veld van het **Omleiden van URL's toegestaan** (onder **OAuth 2.0**). **{Huurder}** vervangen door de naam van de huurder (bijvoorbeeld contoso.onmicrosoft.com). Klik op **toevoegen**en klik op **bijwerken**. De waarde **{huurder}** is hoofdlettergevoelig.

    ![LinkedIn - app Setup](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a>LinkedIn configureren als een identiteitsprovider in uw huurder

1. Volg deze stappen om te [Navigeren naar de blade B2C-functies](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) op de Azure portal.
2. Klik op **id-providers**op de bladeserver B2C-functies.
3. Klik op **+ toevoegen** aan de bovenkant van het blad.
4. Een beschrijvende **naam** voor de configuratie van de provider identiteit bieden. Voer bijvoorbeeld "LI".
5. Klik op **identiteit providertype** **LinkedIn**selecteren en klik op **OK**.
6. Klik op **deze identiteitsprovider instellen** en voer de ID van de client en het geheim van de client van de LinkedIn-toepassing die u eerder hebt gemaakt.
7. Klik op **OK** en klik vervolgens op **maken** als u wilt uw LinkedIn-configuratie op te slaan.
