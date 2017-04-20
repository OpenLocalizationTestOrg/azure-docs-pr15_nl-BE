<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met instructies voor Microsoft | Microsoft Azure" 
    description="Leren richtingen op Microsoft met Azure Active Directory gebruiken om eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-directions-on-microsoft"></a>Zelfstudie: Azure Active Directory-integratie met instructies voor Microsoft

Het doel van deze zelfstudie is de integratie van de Azure Active Directory en een routebeschrijving op Microsoft weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een routebeschrijving op Microsoft-abonnement

Als u een federatieve routebeschrijving op Microsoft abonnement nog geen, per e-mail een verzoek om "*service@DirectionsOnMicrosoft.com*".

Na het voltooien van deze zelfstudie, u hebt toegewezen aan de routebeschrijving op Microsoft Azure Active Directory-gebruikers kan worden met één teken in de toepassing gebruik van eenmalige aanmelding.

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie voor instructies over het Microsoft inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-directions-microsoft-tutorial/IC786877.png "Scenario")
##<a name="enabling-the-application-integration-for-directions-on-microsoft"></a>De toepassingsintegratie voor instructies over het Microsoft inschakelen

Het doel van deze sectie is aan de contour van het inschakelen van de toepassingsintegratie voor instructies over het Microsoft.

###<a name="to-enable-the-application-integration-for-directions-on-microsoft-perform-the-following-steps"></a>Als u wilt dat de toepassingsintegratie voor instructies over het Microsoft, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-directions-microsoft-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-directions-microsoft-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-directions-microsoft-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-directions-microsoft-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **richtingen op Microsoft**.

    ![Galerie van toepassing] (./media/active-directory-saas-directions-microsoft-tutorial/IC786878.png "Galerie van toepassing")

7.  Selecteer **routebeschrijving op Microsoft**in het resultatenvenster en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Scenario] (./media/active-directory-saas-directions-microsoft-tutorial/IC793922.png "Scenario")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij de routebeschrijving op Microsoft met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, op de pagina **routebeschrijving op Microsoft** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding inschakelen] (./media/active-directory-saas-directions-microsoft-tutorial/IC786879.png "Eenmalige aanmelding inschakelen")

2.  Op de pagina **Hoe wilt u dat gebruikers aan te melden op de routebeschrijving op Microsoft** **Azure AD Microsoft Single Sign-On**selecteren en klik op **volgende**.

    ![Microsoft Azure AD Singel aanmelding] (./media/active-directory-saas-directions-microsoft-tutorial/IC786880.png "Microsoft Azure AD Singel aanmelding")

3.  Typ **https://www.directionsonmicrosoft.com/user/login**op de pagina **Configureren App-URL** in het tekstvak teken op een URL en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-directions-microsoft-tutorial/IC786881.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op aanwijzingen van Microsoft** op **metagegevens downloaden**en sla het bestand lokaal op uw computer met metagegevens.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-directions-microsoft-tutorial/IC786882.png "Eenmalige aanmelding configureren")

5.  Het metagegevensbestand verzenden naar de instructies van Microsoft support team (*service@DirectionsOnMicrosoft.com*). Zodat de aanwijzingen op het ondersteuningsteam van Microsoft te vinden van uw sitelidmaatschap federatieve bevatten informatie over uw bedrijf in uw e-mailadres.

    >[AZURE.NOTE] Eenmalige aanmelding voor instructies over het Microsoft moet worden ingeschakeld door de aanwijzingen in Microsoft support team.
U ontvangt een melding wanneer eenmalige aanmelding is ingeschakeld.

6.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-directions-microsoft-tutorial/IC786883.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Er is geen actie-item voor u voor het configureren van gebruikers voor instructies over het Microsoft.  
Wanneer een toegewezen gebruiker zich probeert aan te melden bij de aanwijzingen op in het deelvenster voor toegang tot Microsoft, routebeschrijving op Microsoft wordt gecontroleerd of de gebruiker bestaat. Als er nog geen gebruikersaccount beschikbaar, wordt deze automatisch gemaakt door aanwijzingen op Microsoft.
##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-directions-on-microsoft-perform-the-following-steps"></a>Gebruikers toewijzen aan de routebeschrijving op Microsoft, kunt u de volgende stappen uitvoeren:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **routebeschrijving op Microsoft **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-directions-microsoft-tutorial/IC786884.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-directions-microsoft-tutorial/IC767830.png "Ja")
