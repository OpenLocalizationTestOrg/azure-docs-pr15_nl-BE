<properties
    pageTitle="Active Directory B2C Azure: Configuratie Amazon | Microsoft Azure"
    description="Aanmelden en aanmelden aan de consumenten met Amazon accounts in uw toepassingen die worden beveiligd door Azure Active Directory B2C bieden."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-amazon-accounts"></a>Azure Active Directory B2C: Aanmelding en -in bieden aan de consument met Amazon accounts

## <a name="create-an-amazon-application"></a>Maak een Amazon-toepassing

Voor het gebruik van Amazon als een identiteitsprovider in B2C Azure Active Directory (AD Azure), moet u een Amazon-toepassing maken en deze met de juiste parameters opgeven. U moet een Amazon account om dit te doen. Als u er geen hebt, vindt je het op [http://www.amazon.com/](http://www.amazon.com/).

1. Ga naar de [Amazon Developer Center](https://login.amazon.com/) en meld u aan met uw referenties van Amazon.
2. Als u dit nog niet hebt gedaan, klikt u op **Aanmelden**de ontwikkelaar registratie stappen en het beleid te accepteren.
3. Klik op **registreren nieuwe toepassing**.

    ![Een nieuwe toepassing op de Amazon-website registreren](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)

4. Toepassingsinformatie (**naam**, **Beschrijving**en **Privacy kennisgeving URL**) en klik op **Opslaan**.

    ![Verstrekken van toepassingsinformatie voor het registreren van een nieuwe toepassing op Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)

5. Kopieer de waarden van **De Client-ID** en het **Geheim van de Client**in de sectie **Webinstellingen aanhouden** . (U moet op de knop **Geheim weergeven** klikken om dit te zien.) U moet beide Amazon configureren als een identiteitsprovider in uw huurder. Klik op **bewerken** onder aan de sectie. **Het geheim van de client** is een belangrijke.

    ![Verschaffen van de Client-ID en het geheim van de Client voor de nieuwe toepassing op Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)

6. Voer `https://login.microsoftonline.com` in het veld **Toegestane JavaScript-oorsprong** en `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in het veld **URL's voor retourneren toegestaan** . **{Huurder}** vervangen door de naam van de huurder (bijvoorbeeld contoso.onmicrosoft.com). Klik op **Opslaan**. De waarde **{huurder}** is hoofdlettergevoelig.

    ![JavaScript oorsprongen en URL's terug die voor de nieuwe toepassing op Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## <a name="configure-amazon-as-an-identity-provider-in-your-tenant"></a>Amazon configureren als een identiteitsprovider in uw huurder

1. Volg deze stappen om te [Navigeren naar de blade B2C-functies](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) op de Azure portal.
2. Klik op **id-providers**op de bladeserver B2C-functies.
3. Klik op **+ toevoegen** aan de bovenkant van het blad.
4. Een beschrijvende **naam** voor de configuratie van de provider identiteit bieden. Voer bijvoorbeeld "Amzn".
5. Klik op **identiteit providertype** **Amazon**selecteren en klik op **OK**.
6. Klik op **deze identiteitsprovider instellen** en voer de ID van de client en het geheim van de client van de Amazon-toepassing die u eerder hebt gemaakt.
7. Klik op **OK** en klik vervolgens op **maken** als u wilt uw Amazon-configuratie op te slaan.
