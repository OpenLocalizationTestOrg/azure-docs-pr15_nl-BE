<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met BlueJeans | Microsoft Azure" 
    description="Meer informatie over het BlueJeans met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-ad-integration-with-bluejeans"></a>Zelfstudie: Azure AD-integratie met BlueJeans

Het doel van deze zelfstudie is de integratie van de Azure en BlueJeans weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een BlueJeans eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan BlueJeans kan worden naar één teken in de toepassing van de BlueJeans bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor BlueJeans
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-bluejeans-tutorial/IC785860.png "Scenario")
##<a name="enabling-the-application-integration-for-bluejeans"></a>Inschakelen van de integratie van toepassingen voor BlueJeans

Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor BlueJeans.

###<a name="to-enable-the-application-integration-for-bluejeans-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor BlueJeans, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-bluejeans-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-bluejeans-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-bluejeans-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-bluejeans-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **BlueJeans**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-bluejeans-tutorial/IC785861.png "Galerie van toepassing")

7.  **BlueJeans**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![BlueJeans] (./media/active-directory-saas-bluejeans-tutorial/IC785862.png "BlueJeans")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij BlueJeans met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **BlueJeans** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bluejeans-tutorial/IC785863.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op BlueJeans** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bluejeans-tutorial/IC785864.png "Eenmalige aanmelding configureren")

3.  Typ de URL met het volgende patroon '*https://company.BlueJeans.com*' op de pagina **Configureren App-URL** in het tekstvak **BlueJeans aanmelden op de URL** en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-bluejeans-tutorial/IC785865.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op BlueJeans** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bluejeans-tutorial/IC785866.png "Eenmalige aanmelding configureren")

5.  Log in op uw site **BlueJeans** bedrijf als beheerder in een ander web browser-venster.

6.  Ga naar **ADMIN \> instellingen \> beveiliging**.

    ![Admin] (./media/active-directory-saas-bluejeans-tutorial/IC785868.png "Admin")

7.  In de sectie **beveiliging** voert u de volgende stappen uit:

    ![SAML eenmalige aanmelding] (./media/active-directory-saas-bluejeans-tutorial/IC785869.png "SAML eenmalige aanmelding")

    1.  Selecteer **SAML eenmalige aanmelding**.
    2.  Selecteer **Automatisch inrichten**.

8.  Gaan met de volgende stappen uit:

    ![Certificeringspad] (./media/active-directory-saas-bluejeans-tutorial/IC785870.png "Certificeringspad")

    1.  Klik op **Bestand kiezen**en vervolgens het gedownloade certificaat uploaden.
    2.  De waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het tekstvak **Aanmeldings-URL** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op BlueJeans** .
    3.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij BlueJeans** de waarde **Wijzigen wachtwoord URL** kopiëren en vervolgens plakken in het tekstvak **URL van wachtwoord wijzigen** .
    4.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij BlueJeans** de waarde van de **Externe Logout URL** kopiëren en vervolgens plakken in het tekstvak **URL Logout** .

9.  Gaan met de volgende stappen uit:

    ![Wijzigingen opslaan] (./media/active-directory-saas-bluejeans-tutorial/IC785874.png "Wijzigingen opslaan")

    1.  Typ in het tekstvak **gebruikersnaam** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**.
    2.  Typ in het tekstvak **e-mailadres** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**.
    3.  Klik op **wijzigingen opslaan**.

10. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bluejeans-tutorial/IC785876.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Om gebruikers aan te melden bij BlueJeans Azure AD, moeten zij worden ingericht in BlueJeans.  
Bij BlueJeans is het inrichten van een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw site **BlueJeans** bedrijf als beheerder.

2.  Ga naar **ADMIN \> gebruikers beheren \> gebruiker toevoegen**.

    ![Admin] (./media/active-directory-saas-bluejeans-tutorial/IC785877.png "Admin")

    >[AZURE.IMPORTANT] Het tabblad **Gebruiker toevoegen** is alleen beschikbaar als u op het **tabblad Beveiliging** **inschakelen automatisch inrichten** is uitgeschakeld.

3.  In de sectie **Gebruiker toevoegen** kunt u de volgende stappen uitvoeren:

    ![Gebruiker toevoegen] (./media/active-directory-saas-bluejeans-tutorial/IC785886.png "Gebruiker toevoegen")

    1.  Typ een **Gebruikersnaam BlueJeans**, een **e-mailadres**, een **BlueJeans vergadering-ID**, een **Toezichthouder code**, een **Volledige naam**, het **bedrijf** van een geldige AAD-account die u wilt inrichten in de verwante tekstvakken.
    2.  Klik op **gebruiker toevoegen**.

>[AZURE.NOTE] Kunt u een andere BlueJeans gebruiker account maken van hulpprogramma's of API's geleverd door BlueJeans bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-bluejeans-perform-the-following-steps"></a>Gebruikers toewijzen aan BlueJeans, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **BlueJeans **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-bluejeans-tutorial/IC785887.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-bluejeans-tutorial/IC767830.png "Ja")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
