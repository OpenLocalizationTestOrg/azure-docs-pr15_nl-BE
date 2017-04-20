<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met ScreenSteps | Microsoft Azure" 
    description="Meer informatie over het ScreenSteps met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-screensteps"></a>Zelfstudie: Azure Active Directory-integratie met ScreenSteps
  
Het doel van deze zelfstudie is de integratie van de Azure en ScreenSteps weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder ScreenSteps
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan ScreenSteps kan worden naar één teken in de toepassing van de ScreenSteps bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor ScreenSteps
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-screensteps-tutorial/IC778516.png "Scenario")
##<a name="enabling-the-application-integration-for-screensteps"></a>Inschakelen van de integratie van toepassingen voor ScreenSteps
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor ScreenSteps.

###<a name="to-enable-the-application-integration-for-screensteps-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor ScreenSteps, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-screensteps-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-screensteps-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-screensteps-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-screensteps-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **ScreenSteps**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-screensteps-tutorial/IC778517.png "Galerie van toepassing")

7.  **ScreenSteps**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![ScreenSteps] (./media/active-directory-saas-screensteps-tutorial/IC778518.png "ScreenSteps")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij ScreenSteps met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **ScreenSteps** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-screensteps-tutorial/IC778519.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op ScreenSteps** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-screensteps-tutorial/IC778520.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App URL configureren** , in het tekstvak **ScreenSteps teken In de URL** de URL van uw volgende patroon met ' https://*\<-naam van de huurder\>. ScreenSteps.com*", en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-screensteps-tutorial/IC778521.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op ScreenSteps** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-screensteps-tutorial/IC778522.png "Eenmalige aanmelding configureren")

5.  Log in op uw site ScreenSteps bedrijf als beheerder in een ander web browser-venster.

6.  Klik op **accountmanagement**.

    ![Accountmanagement] (./media/active-directory-saas-screensteps-tutorial/IC778523.png "Accountmanagement")

7.  Klik op **externe verificatie**.

    ![Externe verificatie] (./media/active-directory-saas-screensteps-tutorial/IC778524.png "Externe verificatie")

8.  Klik op **Create verificatie eindpunt**.

    ![Externe verificatie] (./media/active-directory-saas-screensteps-tutorial/IC778525.png "Externe verificatie")

9.  In de sectie **maken een eindpunt voor verificatie** , moet u de volgende stappen uitvoeren:

    ![Een eindpunt verificatie maken] (./media/active-directory-saas-screensteps-tutorial/IC778526.png "Een eindpunt verificatie maken")

    1.  Typ een titel in het tekstvak **titel** .
    2.  Selecteer in de lijst **modus** **SAML**.
    3.  Klik op **maken**.

10. Voer de volgende stappen uit in de sectie **Externe verificatie eindpunt** :

    ![Externe verificatie eindpunt] (./media/active-directory-saas-screensteps-tutorial/IC778527.png "Externe verificatie eindpunt")

    1.  In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding bij ScreenSteps** de waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het tekstvak **Externe aanmeldings-URL** .
    2.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij ScreenSteps** de waarde van de **Externe Logout URL** kopiëren en vervolgens plakken in het tekstvak **URL afmelden** .
    3.  **Kies het bestand**op en upload het gedownloade certificaat.
    4.  Klik op **bijwerken**.

11. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-screensteps-tutorial/IC778542.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij **ScreenSteps**Azure AD, moeten zij worden ingericht in **ScreenSteps**.  
Bij **ScreenSteps**is het inrichten van een handmatige taak.

###<a name="to-provision-a-user-account-to-screensteps-perform-the-following-steps"></a>Als een gebruikersaccount om ScreenSteps te creëren, kunt u de volgende stappen uitvoeren:

1.  Log in op uw huurder **ScreenSteps** .

2.  Klik op **accountmanagement**.

    ![Accountmanagement] (./media/active-directory-saas-screensteps-tutorial/IC778523.png "Accountmanagement")

3.  Klik op **gebruikers**.

    ![Gebruikers] (./media/active-directory-saas-screensteps-tutorial/IC778544.png "Gebruikers")

4.  Klik op **een gebruiker maken**.

    ![Alle gebruikers] (./media/active-directory-saas-screensteps-tutorial/IC778545.png "Alle gebruikers")

5.  Selecteer in de lijst **Gebruikersrol** een rol voor de gebruiker.

6.  Typ de**"Voornaam**, **Achternaam**, **e-mailadres**, **Login**, **wachtwoord** en **Wachtwoord bevestigen'**van een geldige AAD account te verrichten in de verwante tekstvakken in de sectie gebruikersrol.

    ![Nieuwe gebruiker] (./media/active-directory-saas-screensteps-tutorial/IC778546.png "Nieuwe gebruiker")

7.  Selecteer **Verificatie-groep (SAML)**en klik op **Gebruiker maken**in de sectie groepen.

    ![Groepen] (./media/active-directory-saas-screensteps-tutorial/IC778547.png "Groepen")

>[AZURE.NOTE]Kunt u een andere ScreenSteps gebruiker account maken van hulpprogramma's of API's geleverd door ScreenSteps bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-screensteps-perform-the-following-steps"></a>Gebruikers toewijzen aan ScreenSteps, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **ScreenSteps **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-screensteps-tutorial/IC773094.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Gebruikers toewijzen] (./media/active-directory-saas-screensteps-tutorial/IC778548.png "Gebruikers toewijzen")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.