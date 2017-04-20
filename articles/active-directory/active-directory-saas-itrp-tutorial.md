<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met ITRP | Microsoft Azure" 
    description="Meer informatie over het ITRP met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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
    ms.date="09/07/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-itrp"></a>Zelfstudie: Azure Active Directory-integratie met ITRP
  
Het doel van deze zelfstudie is de integratie van de Azure en ITRP weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder ITRP
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan ITRP kan worden naar één teken in de toepassing van de ITRP bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor ITRP
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-itrp-tutorial/IC775551.png "Scenario")
##<a name="enabling-the-application-integration-for-itrp"></a>Inschakelen van de integratie van toepassingen voor ITRP
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor ITRP.

###<a name="to-enable-the-application-integration-for-itrp-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor ITRP, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-itrp-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-itrp-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-itrp-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-itrp-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **ITRP**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-itrp-tutorial/IC775565.png "Galerie van toepassing")

7.  **ITRP**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![ITRP] (./media/active-directory-saas-itrp-tutorial/IC775566.png "ITRP")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij ITRP met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Configureren van eenmalige aanmelding voor ITRP moet u een waarde uit een certificaat ophalen.  
Als u niet bekend met deze procedure bent, raadpleegt u [voor het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **ITRP** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-itrp-tutorial/IC771709.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op ITRP** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-itrp-tutorial/IC775567.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App URL configureren** , in het tekstvak **ITRP teken In de URL** de URL van uw volgende patroon met ' https://*\<-naam van de huurder\>. ITRP.com*", en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-itrp-tutorial/IC775568.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op ITRP** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal als **c:\\ITRP.cer**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-itrp-tutorial/IC775569.png "Eenmalige aanmelding configureren")

5.  Log in op uw site ITRP bedrijf als beheerder in een ander web browser-venster.

6.  In de werkbalk op de bovenkant, klikt u op **Instellingen**.

    ![ITRP] (./media/active-directory-saas-itrp-tutorial/IC775570.png "ITRP")

7.  Selecteer in het linkernavigatievenster **Single Sign-On**.

    ![Eenmalige aanmelding] (./media/active-directory-saas-itrp-tutorial/IC775571.png "Eenmalige aanmelding")

8.  In de configuratiesectie van eenmalige aanmelding kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding] (./media/active-directory-saas-itrp-tutorial/IC775572.png "Eenmalige aanmelding")

    ![Eenmalige aanmelding] (./media/active-directory-saas-itrp-tutorial/IC775573.png "Eenmalige aanmelding")

    1.  Klik op **inschakelen**.
    2.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij ITRP** de waarde van de **Externe Logout URL** kopiëren en vervolgens plakken in het tekstvak **URL van externe Logout** .
    3.  In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding bij ITRP** de waarde van **SAML SSO-URL** kopiëren en vervolgens plakken in het tekstvak **URL van SAML SSO** .
    4.  **De waarde** van het geëxporteerde certificaat kopiëren en vervolgens plakken in het tekstvak **Certificaat vingerafdruk** .
        
        >[AZURE.TIP]Zie voor meer informatie [hoe u kunt ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

    5.  Klik op **Opslaan**.

9.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-itrp-tutorial/IC775574.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij ITRP Azure AD, moeten zij worden ingericht in ITRP.  
Bij ITRP is het inrichten van een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw huurder **ITRP** .

2.  Klik in de werkbalk op de bovenste **Records**.

    ![Admin] (./media/active-directory-saas-itrp-tutorial/IC775575.png "Admin")

3.  Selecteer in het pop-upmenu **mensen**.

    ![Mensen] (./media/active-directory-saas-itrp-tutorial/IC775587.png "Mensen")

4.  Klik op **toevoegen nieuwe persoon** ("+").

    ![Admin] (./media/active-directory-saas-itrp-tutorial/IC775576.png "Admin")

5.  Voer de volgende stappen uit in het dialoogvenster nieuwe persoon toevoegen:

    ![Gebruiker] (./media/active-directory-saas-itrp-tutorial/IC775577.png "Gebruiker")

    1.  Typ de **naam**, **e-mailadres** van een geldige AAD-account die u verrichten wilt.
    2.  Klik op **Opslaan**.

>[AZURE.NOTE] Kunt u een andere ITRP gebruiker account maken van hulpprogramma's of API's geleverd door ITRP bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-itrp-perform-the-following-steps"></a>Gebruikers toewijzen aan ITRP, voert u de volgende stappen uit:

1.  Maak een testaccount in de portal Azure AD.

2.  Klik op **gebruikers toewijzen**op de pagina **ITRP **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-itrp-tutorial/IC775588.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-itrp-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.