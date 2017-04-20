<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Bonus.ly | Microsoft Azure" 
    description="Meer informatie over het Bonus.ly met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bonusly"></a>Zelfstudie: Azure Active Directory-integratie met Bonus.ly

Het doel van deze zelfstudie is de integratie van de Azure en Bonus.ly weergeven. Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder test in Bonus.ly

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Bonus.ly
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-bonus-tutorial/IC773679.png "Scenario")
##<a name="enabling-the-application-integration-for-bonusly"></a>Inschakelen van de integratie van toepassingen voor Bonus.ly

Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Bonus.ly.

###<a name="to-enable-the-application-integration-for-bonusly-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Bonus.ly, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Eenmalige aanmelding inschakelen] (./media/active-directory-saas-bonus-tutorial/IC773680.png "Eenmalige aanmelding inschakelen")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-bonus-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-bonus-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-bonus-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **Bonus.ly**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-bonus-tutorial/IC773681.png "Galerie van toepassing")

7.  **Bonus.ly**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773682.png "Bonusly")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Bonus.ly met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Configureren van eenmalige aanmelding voor Bonus.ly moet u een waarde uit een certificaat ophalen.  
Als u niet bekend met deze procedure bent, raadpleegt u [voor het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Bonus.ly** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bonus-tutorial/IC749323.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Bonus.ly** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bonus-tutorial/IC773683.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **URL van toepassing configureren** in het tekstvak **URL van Bonus.ly huurder** uw URL met behulp van het volgende patroon "https://*\<-naam van de huurder\>. Bonus.LY*", en klik op **volgende**: 

    ![App-URL configureren] (./media/active-directory-saas-bonus-tutorial/IC773684.png "App-URL configureren")

4.  Klik op **certificaat downloaden**en sla het bestand lokaal als op de pagina **configureren eenmalige aanmelding op Bonus.ly** **c:\\Bonusly.cer**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bonus-tutorial/IC773685.png "Eenmalige aanmelding configureren")

5.  In een ander browservenster en log in op uw huurder **Bonus.ly** .

6.  In de werkbalk op de bovenkant, klikt u op **Instellingen**en selecteer **integraties en apps**.

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773686.png "Bonusly")

7.  Selecteer onder **Single Sign-On** **SAML**.

8.  Op de pagina van het dialoogvenster **SAML** voert u de volgende stappen uit:

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773687.png "Bonusly")

    1.  In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding bij Bonus.ly** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in de textbox **IdP SSO-doel-URL** .
    2.  In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding op Bonus.ly** de **Uitgever-ID** -waarde kopiëren en vervolgens plakken in de textbox **IdP uitgever** .
    3.  De waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het tekstvak **IdP aanmeldings-URL** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op Bonus.ly** .
    4.  **De waarde** van het geëxporteerde certificaat kopiëren en vervolgens plakken in de textbox **Cert vingerafdruk** .

        >[AZURE.TIP] Zie voor meer informatie [hoe u kunt ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

9.  Klik op **Opslaan**.

10. Op de klassieke portal van Microsoft Azure, selecteert u de Configuratiebevestiging en klik op **Voltooien** om het **Configureren van eenmalige aanmelding** dialoogvenster te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bonus-tutorial/IC773689.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Om gebruikers aan te melden bij Bonus.ly Azure AD, moeten zij worden ingericht in Bonus.ly.  
Bij Bonus.ly is het inrichten van een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Log in op uw huurder Bonus.ly in een webbrowservenster.

2.  Klik op **Instellingen**

    ![Instellingen] (./media/active-directory-saas-bonus-tutorial/IC781041.png "Instellingen")

3.  Klik op het tabblad **gebruikers en bonussen** .

    ![Gebruikers en bonussen] (./media/active-directory-saas-bonus-tutorial/IC781042.png "Gebruikers en bonussen")

4.  Klik op **gebruikers beheren**.

    ![Gebruikers beheren] (./media/active-directory-saas-bonus-tutorial/IC781043.png "Gebruikers beheren")

5.  Klik op **gebruiker toevoegen**.

    ![Gebruiker toevoegen] (./media/active-directory-saas-bonus-tutorial/IC781044.png "Gebruiker toevoegen")

6.  In het dialoogvenster **Gebruiker toevoegen** kunt u de volgende stappen uitvoeren:

    ![Gebruiker toevoegen] (./media/active-directory-saas-bonus-tutorial/IC781045.png "Gebruiker toevoegen")

    1.  Typ de "**e-mailadres**, **Voornaam**, **Achternaam**' van een geldige AAD account te verrichten in de verwante tekstvakken.
    2.  Klik op **Opslaan**.

    >[AZURE.NOTE] De accounthouder AAD ontvangt een e-mail met daarin een link om te bevestigen de account voordat deze actief wordt.

>[AZURE.NOTE] Kunt u een andere Bonus.ly gebruiker account maken van hulpprogramma's of API's geleverd door Bonus.ly bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-bonusly-perform-the-following-steps"></a>Gebruikers toewijzen aan Bonus.ly, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina Bonus.ly application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-bonus-tutorial/IC773690.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-bonus-tutorial/IC767830.png "Ja")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
