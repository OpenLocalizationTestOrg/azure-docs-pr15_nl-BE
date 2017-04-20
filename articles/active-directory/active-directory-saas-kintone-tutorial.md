<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Kintone | Microsoft Azure" 
    description="Meer informatie over het Kintone met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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
    ms.date="09/01/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-kintone"></a>Zelfstudie: Azure Active Directory-integratie met Kintone
  
Het doel van deze zelfstudie is de integratie van de Azure en Kintone weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een Kintone eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Kintone kan worden naar één teken in de toepassing van de Kintone bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Kintone
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-kintone-tutorial/IC785859.png "Scenario")
##<a name="enabling-the-application-integration-for-kintone"></a>Inschakelen van de integratie van toepassingen voor Kintone
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Kintone.

###<a name="to-enable-the-application-integration-for-kintone-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Kintone, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-kintone-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-kintone-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-kintone-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-kintone-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **Kintone**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-kintone-tutorial/IC785867.png "Galerie van toepassing")

7.  **Kintone**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Kintone] (./media/active-directory-saas-kintone-tutorial/IC785871.png "Kintone")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Kintone met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Kintone** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-kintone-tutorial/IC785872.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Kintone** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-kintone-tutorial/IC785873.png "Eenmalige aanmelding configureren")

3.  Typ de URL met het volgende patroon '*https://company.kintone.com*' op de pagina **Configureren App-URL** in het tekstvak **Kintone aanmelden op de URL** en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-kintone-tutorial/IC785875.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op Kintone** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-kintone-tutorial/IC785878.png "Eenmalige aanmelding configureren")

5.  Log in op uw site **Kintone** bedrijf als beheerder in een ander web browser-venster.

6.  Klik op **Instellingen**.

    ![Instellingen] (./media/active-directory-saas-kintone-tutorial/IC785879.png "Instellingen")

7.  Klik op **gebruikers en System Administration**.

    ![Gebruikers & Systeembeheer] (./media/active-directory-saas-kintone-tutorial/IC785880.png "Gebruikers & Systeembeheer")

8.  Onder **System Administration \> Security** klikt u op **aanmelden**.

    ![Aanmelding] (./media/active-directory-saas-kintone-tutorial/IC785881.png "Aanmelding")

9.  Klik op **verificatie SAML inschakelen**.

    ![SAML-verificatie] (./media/active-directory-saas-kintone-tutorial/IC785882.png "SAML-verificatie")

10. In de sectie SAML-verificatie, moet u de volgende stappen uitvoeren:

    ![SAML-verificatie] (./media/active-directory-saas-kintone-tutorial/IC785883.png "SAML-verificatie")

    1.  De waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het tekstvak **Aanmeldings-URL** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op Kintone** .
    2.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Kintone** de waarde van de **Externe Logout URL** kopiëren en vervolgens plakken in het tekstvak **URL Logout** .
    3.  Klik op **Bladeren** om te uploaden van uw gedownloade certificaat.
    4.  Klik op **Opslaan**.

11. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-kintone-tutorial/IC785884.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij Kintone Azure AD, moeten zij worden ingericht in Kintone.  
Bij Kintone is het inrichten van een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw site **Kintone** bedrijf als beheerder.

2.  Klik op **instelling**.

    ![Instellingen] (./media/active-directory-saas-kintone-tutorial/IC785879.png "Instellingen")

3.  Klik op **gebruikers en System Administration**.

    ![Gebr. & Systeembeheer] (./media/active-directory-saas-kintone-tutorial/IC785880.png "Gebr. & Systeembeheer")

4.  Klik onder **Beheer van de gebruiker**, **afdelingen en gebruikers**.

    ![Afdeling & gebruikers] (./media/active-directory-saas-kintone-tutorial/IC785888.png "Afdeling & gebruikers")

5.  Klik op **nieuwe gebruiker**.

    ![Nieuwe gebruikers] (./media/active-directory-saas-kintone-tutorial/IC785889.png "Nieuwe gebruikers")

6.  In de sectie **Nieuwe gebruiker** de volgende stappen uitvoeren:

    ![Nieuwe gebruikers] (./media/active-directory-saas-kintone-tutorial/IC785890.png "Nieuwe gebruikers")

    1.  Typ een **Weergavenaam**, **Aanmeldingsnaam**, **Wachtwoord**, **Bevestig het wachtwoord**, **E-mailadres** en andere details van een geldige AAD account te verrichten in de bijbehorende texboxes.
    2.  Klik op **Opslaan**.

>[AZURE.NOTE] Kunt u een andere Kintone gebruiker account maken van hulpprogramma's of API's geleverd door Kintone bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-kintone-perform-the-following-steps"></a>Gebruikers toewijzen aan Kintone, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Kintone **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-kintone-tutorial/IC785891.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-kintone-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.