<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Facebook op het werk | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Facebook op het werk."
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
    ms.date="04/26/2016"
    ms.author="asmalser"/>


# <a name="tutorial-azure-active-directory-integration-with-facebook-at-work"></a>Zelfstudie: Azure Active Directory-integratie met Facebook op het werk

Het doel van deze zelfstudie is u tonen hoe te integreren Facebook op het werk met Azure Active Directory (AD Azure).

Facebook op het werk te integreren met AD Azure biedt de volgende voordelen: 

- U kunt bepalen in Azure AD die toegang tot Facebook op het werk heeft 
- U kunt automatisch inrichten account voor gebruikers die toegang hebben gekregen tot Facebook op het werk
- U kunt gebruikers automatisch ophalen ondertekend bij Facebook op het werk (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie beheren 

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).


## <a name="prerequisites"></a>Vereisten 

Integratie van de Azure AD CS sterren configureren, moet u de volgende items:

- Een abonnement op Azure AD
- Facebook op het werk met eenmalige aanmelding is ingeschakeld

Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen. 


## <a name="adding-facebook-at-work-from-the-gallery"></a>Facebook toe te voegen aan het werk in de galerie
De integratie van Facebook op het werk in Azure AD configureren, moet u Facebook op het werk in de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Om toe te voegen Facebook op het werk in de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.
    
    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Facebook op het werk**.

7. Selecteer **Facebook op het werk**in het resultatenvenster en klik op **Voltooien** als de toepassing wilt toevoegen.


### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

In deze sectie vindt u een overzicht van het inschakelen van gebruikers worden geverifieerd met hun account in Azure Active Directory, op basis van het protocol SAML federation via Facebook op het werk.

**Configureren van eenmalige aanmelding Azure AD met Facebook op het werk, moet u de volgende stappen uitvoeren:**

1.  Na het toevoegen van Facebook op het werk in de portal voor Azure-klassiek, klikt u op **Configureren van eenmalige aanmelding**.

2.  In het scherm **Configureren App URL** de URL opgeven waar gebruikers in uw Facebook op werktoepassing zal ondertekenen. Dit is uw Facebook op werk huurder URL (voorbeeld: https://example.facebook.com/). Zodra u klaar bent, klikt u op **volgende**.

3.  Log in op uw Facebook op werk bedrijf site als beheerder in een ander web browser-venster.

4. Volg de instructies op de volgende URL Facebook configureren op uw werk Azure AD als een identiteitsprovider: [https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers)

5.  Zodra voltooid, Ga terug naar de browser windows met de klassieke Azure portal, het selectievakje in om te bevestigen dat u de procedure hebt voltooid, klik op **volgende** en **Voltooien**.


## <a name="automatically-provisioning-users-to-facebook-at-work"></a>Automatisch gebruikers aan Facebook op het werk

Azure AD ondersteunt de mogelijkheid om automatisch synchroniseren met details van de toegewezen gebruikers Facebook op het werk. Deze automatische synchronisatie kunt Facebook op het werk om de vereiste gegevens voor het machtigen van gebruikers om toegang te krijgen, voordat ze probeert aan te melden voor de eerste keer. Deze ook bepalingen opgeheven gebruikers van Facebook op het werk wanneer access in Azure AD is ingetrokken.

Automatische provisioning kunt instellen door te klikken op het **inrichten van de account configureren** in het Azure klassieke portal-venster.

Zie [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers) voor meer informatie over het configureren van automatische inrichting,


## <a name="assigning-users-to-facebook-at-work"></a>Gebruikers toewijzen aan Facebook op het werk

Ingerichte AAD gebruikers Facebook op het werk zien in hun Access-venster, als ze toegang binnen de klassieke Azure portal worden toegewezen.

**Gebruikers toewijzen aan Facebook op het werk:**

1.  Klik op de startpagina voor Facebook op het werk in de klassieke Azure portal **accounts toewijzen**.

2.  Selecteer in het menu **weergeven** , of u wilt dat een gebruiker of groep toewijzen aan Facebook op het werk en klik op de knop ingeschakeld.

3.  Selecteer de gebruikers of de groep die u wilt toewijzen, Facebook op het werk in de resulterende lijst.

4.  In de voettekst van de pagina, klikt u op de knop **toewijzen** .


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_04.png




