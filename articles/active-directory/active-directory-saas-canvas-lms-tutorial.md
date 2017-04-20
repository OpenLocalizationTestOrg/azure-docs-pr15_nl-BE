<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Canvas LMS | Microsoft Azure" 
    description="Meer informatie over het Canvas LMS met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a>Zelfstudie: Azure Active Directory-integratie met Canvas LMS

Het doel van deze zelfstudie is weer te geven van de integratie van de Azure en Canvas.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder Canvas

Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan het Canvas kan worden met één teken in de toepassing op uw Canvas bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor Canvas inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-canvas-lms-tutorial/IC775984.png "Scenario")
##<a name="enabling-the-application-integration-for-canvas"></a>De integratie van toepassingen voor Canvas inschakelen

Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Canvas.

###<a name="to-enable-the-application-integration-for-canvas-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Canvas, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-canvas-lms-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-canvas-lms-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-canvas-lms-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-canvas-lms-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak**, **Canvas**.

    ![Galerie van toepassing] (./media/active-directory-saas-canvas-lms-tutorial/IC775985.png "Galerie van toepassing")

7.  In het resultatendeelvenster **tekenpapier**te selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Canvas] (./media/active-directory-saas-canvas-lms-tutorial/IC775986.png "Canvas")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Canvas met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Configureren van eenmalige aanmelding voor papier, moet u een waarde uit een certificaat ophalen.  
Als u niet bekend met deze procedure bent, raadpleegt u [waarde van een certificaat ophalen](http://youtu.be/YKQF266SAxI)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Canvas** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-canvas-lms-tutorial/IC771709.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**, en klik op **volgende**op de pagina **Hoe wilt u dat gebruikers aan te melden op Canvas** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-canvas-lms-tutorial/IC775987.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App URL configureren** , in het tekstvak **Canvas teken In de URL** de URL van uw volgende patroon met `https://<tenant-name>.instructure.com`, en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-canvas-lms-tutorial/IC775988.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op papier** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-canvas-lms-tutorial/IC775989.png "Eenmalige aanmelding configureren")

5.  Log in op uw site Canvas bedrijf als beheerder in een ander web browser-venster.

6.  Ga naar **cursussen \> beheerde Accounts \> Microsoft**.

    ![Canvas] (./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Canvas")

7.  In het navigatiedeelvenster aan de linkerkant, selecteer **verificatie**en klik op **Toevoegen nieuwe SAML Config**.

    ![Verificatie] (./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "Verificatie")

8.  Op de integratie van de huidige pagina, moet u de volgende stappen uitvoeren:

    ![Huidige integratie] (./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "Huidige integratie")

    1.  De **Entiteit-ID** -waarde kopiëren en vervolgens plakken in het tekstvak **IdP entiteits-ID** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op Canvas** .
    2.  De waarde van de **Externe aanmeldings-URL** kopiëren in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op papier** en plak dit in het **Logboek op URL** textbox.
    3.  De waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het **Logboek van URL** textbox in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op Canvas** .
    4.  Kopieer de waarde **Wachtwoord-URL wijzigen** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op papier** en vervolgens plakken in het tekstvak voor **De koppeling wachtwoord wijzigen** .
    5.  **De waarde** van het geëxporteerde certificaat kopiëren en vervolgens plakken in het tekstvak **Certificaat vingerafdruk** .  

        >[AZURE.TIP] Zie voor meer informatie [hoe u kunt ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

    6.  Selecteer in de lijst **Aanmelden kenmerk** **NameID**.
    7.  Selecteer in de lijst **Indeling id** **EmailAdres**.
    8.  Klik op **verificatie-instellingen opslaan**.

9.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-canvas-lms-tutorial/IC775993.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Om gebruikers aan te melden bij Canvas Azure AD, moeten zij worden ingericht in Canvas.  
Canvas is inrichten een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw **Canvas** huurder.

2.  Ga naar **cursussen \> beheerde Accounts \> Microsoft**.

    ![Canvas] (./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Canvas")

3.  Klik op **gebruikers**.

    ![Gebruikers] (./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "Gebruikers")

4.  Klik op **nieuwe gebruiker toevoegen**.

    ![Gebruikers] (./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "Gebruikers")

5.  Op een pagina van het dialoogvenster Nieuwe gebruiker toevoegen kunt u de volgende stappen uitvoeren:

    ![Gebruiker toevoegen] (./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "Gebruiker toevoegen")

    1.  Typ in het tekstvak **Naam van de volledige** naam van de gebruiker.
    2.  Typ in het tekstvak **e-mailadres** van de gebruiker e-mailadres.
    3.  Typ in het tekstvak **aanmelding** van de gebruiker Azure AD e-mailadres.
    4.  Selecteer **de gebruiker informatie over het maken van deze account E-mail**.
    5.  Klik op **gebruiker toevoegen**.

>[AZURE.NOTE] Kunt u een andere Canvas gebruiker account hulpmiddelen voor het maken of API's geleverd door Canvas bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-canvas-perform-the-following-steps"></a>Gebruikers toewijzen aan papier, kunt u de volgende stappen uitvoeren:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Canvas **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-canvas-lms-tutorial/IC775998.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-canvas-lms-tutorial/IC767830.png "Ja")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
