<properties
    pageTitle="Het beheren van certificaten Federation in Azure AD | Microsoft Azure"
    description="Informatie over het aanpassen van de vervaldatum voor de federation-certificaten en het vernieuwen van certificaten die binnenkort verloopt."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="managing-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>Certificaten voor federatieve eenmalige aanmelding in Azure Active Directory beheren

Dit artikel heeft betrekking op veelgestelde vragen met betrekking tot de certificaten die Azure Active Directory wordt gemaakt om vast te stellen federatieve eenmalige aanmelding (SSO) voor uw SaaS-toepassingen.

Dit artikel is alleen relevant voor toepassingen die zijn geconfigureerd voor **Azure AD Single Sign-On**, zoals in het volgende voorbeeld wordt getoond:

![Azure AD Single Sign-On](./media/active-directory-sso-certs/fed-sso.PNG)

##<a name="how-to-customize-the-expiration-date-for-your-federation-certificate"></a>Het aanpassen van de vervaldatum voor de Federation-certificaat

Certificaten worden standaard ingesteld op verlopen na twee jaar. Door de volgende stappen kunt u een andere vervaldatum voor uw certificaat. De screenshots opgenomen televergaderingen voorbeeld gebruiken, maar deze stappen kunnen toepassen op elke federatieve SaaS-toepassing.

1. Azure Active Directory, klik op de pagina Quick Start voor uw toepassing, op op **eenmalige aanmelding configureren**.

    ![Open de wizard Eenmalige aanmelding configureren.](./media/active-directory-sso-certs/config-sso.png)

2. Selecteer **Azure AD Single Sign-On**en klik vervolgens op **volgende**.

3. Typ de **URL voor aanmelding** van uw toepassing en schakel het selectievakje in voor het **configureren van het certificaat voor de federatieve eenmalige aanmelding gebruikt**. Klik vervolgens op **volgende**.

    ![Azure AD Single Sign-On](./media/active-directory-sso-certs/new-app-config-sso.PNG)

4. Op de volgende pagina selecteert u **een nieuw certificaat genereren**en selecteer hoe lang u wilt dat het certificaat geldig is voor. Klik vervolgens op **volgende**.

    ![Een nieuw certificaat te genereren](./media/active-directory-sso-certs/new-app-config-cert.PNG)

5. Klik op **certificaat downloaden**. Als u wilt weten hoe u het certificaat uploaden naar uw specifieke SaaS-app, klikt u op **Weergave configuratie-instructies**.

    ![Download vervolgens het certificaat uploaden](./media/active-directory-sso-certs/new-app-config-app.PNG)

6. Het certificaat is niet beschikbaar totdat u het selectievakje bevestiging onder aan het dialoogvenster en druk op verzenden.

##<a name="how-to-renew-a-certificate-that-will-soon-expire"></a>Het vernieuwen van een certificaat die binnenkort verlopen

De vernieuwing van de stappen hieronder moeten bij voorkeur leiden tot geen aanzienlijke uitvaltijd voor gebruikers. De schermafbeeldingen in deze sectie functie televergaderingen gebruikt als een voorbeeld, maar deze stappen kunt toepassen op elke federatieve SaaS-toepassing.

1. Azure Active Directory, op de pagina Quick Start voor uw toepassing, klik op het **Configureren van eenmalige aanmelding**.

    ![Open de wizard Eenmalige aanmelding configureren](./media/active-directory-sso-certs/renew-sso-button.PNG)

2. Op de eerste pagina van het dialoogvenster **Azure AD Single Sign-On** moet al zijn geselecteerd, dus klikt u op **volgende**.

3. Schakel het selectievakje voor **het certificaat voor de federatieve eenmalige aanmelding configureren**op de tweede pagina. Klik vervolgens op **volgende**.

    ![Azure AD Single Sign-On](./media/active-directory-sso-certs/renew-config-sso.PNG)

4. Selecteer **een nieuw certificaat genereren**op de volgende pagina en selecteer hoe lang u wilt dat het nieuwe certificaat geldig zijn voor. Klik vervolgens op **volgende**.

    ![Een nieuw certificaat te genereren](./media/active-directory-sso-certs/new-app-config-cert.PNG)

5. Klik op **certificaat downloaden**. Om met succes rewnew uw certificaat, moet u uitvoeren de volgende twee stappen:

    - Het nieuwe certificaat uploaden naar de SaaS-app enkele aanmelding-Configuratiescherm. Als u wilt weten hoe u dit doet voor uw specifieke SaaS-app, klikt u op **Weergave configuratie-instructies**.

    - In Azure AD, schakelt u het selectievakje bevestiging onderaan in het dialoogvenster Nieuw certificaat inschakelen en klik vervolgens op **volgende** om te dienen.

    > [AZURE.IMPORTANT] Eenmalige aanmelding App wordt uitgeschakeld het moment dat u een van deze twee stappen is voltooid, maar het weer wordt ingeschakeld zodra de tweede stap is voltooid. Daarom om de uitvaltijd te minimaliseren, zorg ervoor dat beide stappen uitvoeren binnen een korte tijd uit elkaar.

    ![Download vervolgens het certificaat uploaden](./media/active-directory-sso-certs/renew-config-app.PNG)

## <a name="related-articles"></a>Verwante artikelen

- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md)
- [Het oplossen van problemen op basis van SAML Single Sign-On](active-directory-saml-debugging.md)
