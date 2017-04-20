<properties
    pageTitle="Active Directory B2C Azure: Configuratie Microsoft account | Microsoft Azure"
    description="Aanmelden en inloggen voor consumenten met een Microsoft-account in uw toepassingen die worden beveiligd door Azure Active Directory B2C bieden."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-microsoft-accounts"></a>Azure Active Directory B2C: Bieden aanmelding en aanmelden voor consumenten met een Microsoft-account

## <a name="create-a-microsoft-account-application"></a>De toepassing van een Microsoft-account maken

Voor het gebruik van Microsoft-account als een identiteitsprovider in B2C Azure Active Directory (AD Azure), moet u een Microsoft-account-toepassing maken en leveren met de juiste parameters. U moet een Microsoft-account om dit te doen. Als u er geen hebt, vindt je het op [https://www.live.com/](https://www.live.com/).

1. Ga naar de [Microsoft-Portal registratie van toepassing](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) en meld u aan met uw referenties van Microsoft.
2. Klik op **een app toevoegen**.

    ![Microsoft account - een nieuwe toepassing toevoegen](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)

3. Geef een **naam** voor de toepassing en klikt u op **toepassing maken**.

    ![Microsoft-account - Naam toep.](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)

4. Kopieer de waarde van de **Toepassings-Id**. U moet het Microsoft-account configureren als een identiteitsprovider in uw huurder.

    ![Microsoft-account - Id van de toepassing](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)

5. Klik op **Add platform** en kiest u **Web**.

    ![Microsoft account - platform toevoegen](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)

    ![Microsoft-account - Web](./media/active-directory-b2c-setup-msa-app/msa-web.png)

6. Voer `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in het veld **Aantal URI's omleiden** . **{Huurder}** vervangen door de naam van de huurder (bijvoorbeeld contosob2c.onmicrosoft.com).

    ![Microsoft-account - Omleidings-URL](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)

7. Klik op **Nieuw wachtwoord genereren** onder de sectie **Toepassing geheimen** . Kopieer het nieuwe wachtwoord op het scherm weergegeven. U moet het Microsoft-account configureren als een identiteitsprovider in uw huurder. Dit wachtwoord is een belangrijke.

    ![Microsoft rekening - nieuw wachtwoord genereren](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)

    ![Microsoft-account - nieuw wachtwoord](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)

8. Schakel het selectievakje in dat **ondersteuning voor Live SDK** onder de sectie **Geavanceerde opties** . Klik op **Opslaan**.

    ![Microsoft-account - SDK Live ondersteuning](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a>Microsoft-account configureren als een identiteitsprovider in uw huurder

1. Volg deze stappen om te [Navigeren naar de blade B2C-functies](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) op de Azure portal.
2. Klik op **id-providers**op de bladeserver B2C-functies.
3. Klik op **+ toevoegen** aan de bovenkant van het blad.
4. Een beschrijvende **naam** voor de configuratie van de provider identiteit bieden. Voer bijvoorbeeld "MSA".
5. Klik op **identiteitstype provider** **Microsoft-account**te selecteren en klik op **OK**.
6. Klik op **deze identiteitsprovider instellen** en voer de toepassings-Id en het wachtwoord van de account-toepassing van Microsoft die u eerder hebt gemaakt.
7. Klik op **OK** en klik vervolgens op **maken** als u wilt opslaan van de configuratie van de Microsoft-account.
