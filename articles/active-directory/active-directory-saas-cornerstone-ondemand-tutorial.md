<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met hoeksteen OnDemand | Microsoft Azure" 
    description="Meer informatie over het hoeksteen OnDemand met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-cornerstone-ondemand"></a>Zelfstudie: Azure Active Directory-integratie met hoeksteen OnDemand

Het doel van deze zelfstudie is de integratie van de Azure hoeksteen OnDemand weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder hoeksteen OnDemand

Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan de hoeksteen OnDemand kan worden met één teken in de toepassing van de hoeksteen OnDemand bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor de hoeksteen OnDemand inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781593.png "Scenario")
##<a name="enabling-the-application-integration-for-cornerstone-ondemand"></a>De integratie van toepassingen voor de hoeksteen OnDemand inschakelen

Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor de hoeksteen OnDemand overzicht.

###<a name="to-enable-the-application-integration-for-cornerstone-ondemand-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor de hoeksteen OnDemand, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **hoeksteen ondemand**.

    ![Galerie van toepassing] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781594.png "Galerie van toepassing")

7.  In het resultatendeelvenster **Hoeksteen OnDemand**te selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Hoeksteen OnDemand] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781595.png "Hoeksteen OnDemand")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij de hoeksteen OnDemand met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Hoeksteen OnDemand** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding inschakelen] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781596.png "Eenmalige aanmelding inschakelen")

2.  Op de pagina **Hoe wilt u dat gebruikers aan te melden op de hoekstenen OnDemand** **AD Azure Microsoft Single Sign-On**selecteren en klik op **volgende**.

    ![Microsoft Azure AD Single Sign-On] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781597.png "Microsoft Azure AD Single Sign-On")

3.  Typ de URL met het volgende patroon '*http://company.csod.com*' op de pagina **URL van App configureren** in de **Hoeksteen OnDemand Sign In URL** textbox, en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781598.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op de hoeksteen OnDemand** downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781599.png "Eenmalige aanmelding configureren")

5.  Stuur dat de volgende items om de hoeksteen OnDemand te support team:

    1.  Het gedownloade certificaat
    2.  De waarde van de **Externe aanmeldings-URL**
    3.  De waarde van de **Externe Logout URL** .

    >[AZURE.NOTE] Moet worden geconfigureerd met het ondersteuningsteam hoeksteen OnDemand Single Sign-On.
Wanneer de configuratie is voltooid, ontvangt u een melding van het ondersteuningsteam.

6.  Selecteer de Configuratiebevestiging van één aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781600.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Om gebruikers aan te melden bij de hoeksteen OnDemand Azure AD, moeten zij worden ingericht in de hoeksteen OnDemand.  
Hoeksteen-OnDemand is inrichten een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  De gegevens verzenden (bijvoorbeeld: naam, Emial) over de Azure AD-gebruiker die u verlenen aan de hoeksteen OnDemand wilt ondersteuningsteam.

>[AZURE.NOTE] U kunt een andere hoeksteen OnDemand gebruiker account hulpmiddelen voor het maken of API's die hoeksteen OnDemand bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-cornerstone-ondemand-perform-the-following-steps"></a>Gebruikers toewijzen aan de hoeksteen OnDemand, kunt u de volgende stappen uitvoeren:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Hoeksteen OnDemand **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC775564.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Gebruikers toewijzen] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781601.png "Gebruikers toewijzen")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
