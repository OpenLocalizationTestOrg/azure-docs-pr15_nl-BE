<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Bime | Microsoft Azure" 
    description="Meer informatie over het Bime met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bime"></a>Zelfstudie: Azure Active Directory-integratie met Bime

Het doel van deze zelfstudie is de integratie van de Azure en Bime weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder Bime

Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Bime kan worden naar één teken in de toepassing van de Bime bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Bime
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-bime-tutorial/IC775552.png "Scenario")
##<a name="enabling-the-application-integration-for-bime"></a>Inschakelen van de integratie van toepassingen voor Bime

Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Bime.

###<a name="to-enable-the-application-integration-for-bime-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Bime, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-bime-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-bime-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-bime-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-bime-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **Bime**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-bime-tutorial/IC775553.png "Galerie van toepassing")

7.  **Bime**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Bime] (./media/active-directory-saas-bime-tutorial/IC775554.png "Bime")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Bime met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Configureren van eenmalige aanmelding voor Bime moet u een waarde uit een certificaat ophalen.  
Als u niet bekend met deze procedure bent, raadpleegt u [voor het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Bime** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bime-tutorial/IC771709.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Bime** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bime-tutorial/IC775555.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App URL configureren** , in het tekstvak **Bime teken In de URL** de URL van uw volgende patroon met ' https://*\<-naam van de huurder\>. Bimeapp.com*", en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-bime-tutorial/IC775556.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op Bime** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal als **c:\\Bime.cer**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bime-tutorial/IC775557.png "Eenmalige aanmelding configureren")

5.  Log in op uw site Bime bedrijf als beheerder in een ander web browser-venster.

6.  Klik in de werkbalk op **Admin**en **Account**.

    ![Admin] (./media/active-directory-saas-bime-tutorial/IC775558.png "Admin")

7.  Op de pagina account configuratie voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bime-tutorial/IC775559.png "Eenmalige aanmelding configureren")

    1.  Selecteer **inschakelen SAML-verificatie**.
    2.  In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding bij Bime** de waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het tekstvak **Externe aanmeldings-URL** .
    3.  **De waarde** van het geëxporteerde certificaat kopiëren en vervolgens plakken in het tekstvak **Certificaat vingerafdruk** .  

        >[AZURE.TIP] Zie voor meer informatie [hoe u kunt ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

    4.  Klik op **Opslaan**.

8.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bime-tutorial/IC775560.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Om gebruikers aan te melden bij Bime Azure AD, moeten zij worden ingericht in Bime.  
Bij Bime is het inrichten van een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Log in op uw huurder **Bime** .

2.  Klik in de werkbalk op **Admin**en **gebruikers**.

    ![Admin] (./media/active-directory-saas-bime-tutorial/IC775561.png "Admin")

3.  Klik in de **Lijst met gebruikers**op **Nieuwe gebruiker toevoegen** ("+").

    ![Gebruikers] (./media/active-directory-saas-bime-tutorial/IC775562.png "Gebruikers")

4.  Klik op de pagina van het dialoogvenster **Details van de gebruiker** de volgende stappen uitvoeren:

    ![Gebruikerdetails] (./media/active-directory-saas-bime-tutorial/IC775563.png "Gebruikerdetails")

    1.  Voer de voornaam, achternaam, aanmelding, e-mailadres van een geldige AAD account te verstrekken.
    2.  Klik op opslaan.

>[AZURE.NOTE] Kunt u een andere Bime gebruiker account maken van hulpprogramma's of API's geleverd door Bime bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-bime-perform-the-following-steps"></a>Gebruikers toewijzen aan Bime, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Bime **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-bime-tutorial/IC775564.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-bime-tutorial/IC767830.png "Ja")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
