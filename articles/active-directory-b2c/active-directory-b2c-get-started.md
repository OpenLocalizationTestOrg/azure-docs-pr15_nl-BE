<properties
    pageTitle="Azure Active Directory B2C: Een huurder Azure Active Directory B2C maken | Microsoft Azure"
    description="Een onderwerp voor het maken van een huurder Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.topic="article"
    ms.devlang="na"
    ms.date="08/30/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-create-an-azure-ad-b2c-tenant"></a>Azure Active Directory B2C: Maak een huurder Azure AD B2C

Om te starten met behulp van B2C Microsoft Azure Active Directory (AD Azure), de drie stappen in dit artikel beschreven.

## <a name="step-1-sign-up-for-an-azure-subscription"></a>Stap 1: Meld u aan voor een abonnement Azure

Als u al een Azure-abonnement hebt, kunt u deze stap overslaan. Zo niet, dan aanmelden voor een [abonnement op Azure](../active-directory/sign-up-organization.md) en toegang krijgen tot Azure AD B2C.

## <a name="step-2-create-an-azure-ad-b2c-tenant"></a>Stap 2: Maak een huurder Azure AD B2C

Gebruik de volgende stappen voor het maken van een nieuwe huurder voor Azure AD B2C. Op dit moment kunnen niet B2C-functies worden ingeschakeld in uw bestaande huurders.

1. Aanmelden bij de [Azure klassieke portal](https://manage.windowsazure.com/) als abonnement beheerder. Dit is het hetzelfde werk of school account of het Microsoft-account waarmee u zich aanmeldt voor Azure.
2. Klik op **nieuwe** > **App Services** > **Active Directory** > **map** > **aangepaste maken**.

    ![Schermafdruk van begint met het maken van een huurder](./media/active-directory-b2c-get-started/new-directory.png)

3. Kies de **naam**, de **Domeinnaam** en het **land of de regio** voor de huurder.
4. Schakel de optie die **Dit is een B2C-map zegt**.
5. Klik op het selectievakje om de bewerking te voltooien.

    ![Schermafdruk van het selectievakje een B2C-map maken](./media/active-directory-b2c-get-started/create-b2c-directory.png)

6. De huurder is nu gemaakt en wordt weergegeven in de Active Directory-extensie. U worden ook een globale beheerder van de huurder gedaan. Indien nodig kunt u andere globale beheerders toevoegen.

    > [AZURE.IMPORTANT]
    Als u van plan bent een huurder B2C voor een productie-app gebruiken, lees het artikel op [productie-schaal versus voorbeeld B2C huurders](active-directory-b2c-reference-tenant-type.md). Opmerking: Er zijn bekende problemen wanneer u een bestaande B2C huurder verwijderen en opnieuw met dezelfde domeinnaam maken. U moet een huurder B2C maken met een andere domeinnaam.

## <a name="step-3-navigate-to-the-b2c-features-blade-on-the-azure-portal"></a>Stap 3: Ga naar de B2C functies blade van de Azure portal

1. Ga naar de Active Directory-extensie op de navigatiebalk aan de linkerkant.
2. Zoek uw huurder onder het tabblad **map** en klik erop.
3. Klik op het tabblad **configureren** .
4. Klik op de koppeling **Instellingen voor B2C beheren** in de sectie **beheer van B2C** .

    ![Schermafdruk van de configuratie directory voor B2C](./media/active-directory-b2c-get-started/b2c-directory-configure-tab.png)

5. De Azure portal met de B2C functies blade weergegeven in een nieuw browser tab of venster geopend.

    > [AZURE.IMPORTANT]
    Het kan 2-3 minuten duren voordat uw huurder toegankelijk zijn op de Azure portal duren. Deze stappen opnieuw na enige tijd wordt dit opgelost. Als dit niet het geval is, neem contact op met Support.

6. PIN dit blade naar uw Startboard voor eenvoudige toegang. (Het hulpprogramma Pin is gemarkeerd in het rood in de rechterbovenhoek van de bladeserver functies.)

    ![Schermafdruk van de bladeserver B2C-functies](./media/active-directory-b2c-get-started/b2c-features-blade.png)

    > [AZURE.NOTE]
    U kunt gebruikers en groepen, self-service wachtwoord reset configuratie en huisstijl functies bedrijf van de huurder op de [Azure klassieke portal](https://manage.windowsazure.com/)beheren.

## <a name="easy-access-to-the-b2c-features-blade-on-the-azure-portal"></a>Eenvoudige toegang tot de B2C functies blade van de Azure portal

Ter verbetering van de onderwerpgebieden hebt hebben we een snelkoppeling toegevoegd aan de B2C functies blade van de Azure portal.

1. Inloggen op de portal Azure als globale beheerder van uw B2C-huurder. Als u al bent aangemeld bij een andere huurder, schakelen huurders (in de rechterbovenhoek).
2. Klik op **Bladeren** in de linker navigatie.
3. Klik op **Azure AD B2C** voor toegang tot de functies B2C blade.

    ![Schermafdruk van bladeren naar B2C functies blade](./media/active-directory-b2c-get-started/b2c-browse.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het registreren van een toepassing met Azure AD B2C en een Quick Start om toepassing te maken door te lezen [Azure Active Directory B2C: registreren van uw toepassing](active-directory-b2c-app-registration.md).
