<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Boomi | Microsoft Azure" 
    description="Meer informatie over het Boomi met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-boomi"></a>Zelfstudie: Azure Active Directory-integratie met Boomi

Het doel van deze zelfstudie is de integratie van de Azure en Boomi weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een Boomi eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, is de Azure AD-gebruikers die u hebt toegewezen aan Boomi mogelijk voor één teken in de toepassing van de Boomi bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor Boomi inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-boomi-tutorial/IC791134.png "Scenario")
##<a name="enabling-the-application-integration-for-boomi"></a>De integratie van toepassingen voor Boomi inschakelen

Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor Boomi overzicht.

###<a name="to-enable-the-application-integration-for-boomi-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Boomi, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-boomi-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-boomi-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-boomi-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-boomi-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **Boomi**.

    ![Galerie van toepassing] (./media/active-directory-saas-boomi-tutorial/IC790822.png "Galerie van toepassing")

7.  In het resultatendeelvenster **Boomi**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Boomi] (./media/active-directory-saas-boomi-tutorial/IC790823.png "Boomi")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Boomi met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Boomi** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-boomi-tutorial/IC790824.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Boomi** **AD Azure Microsoft Single Sign-On**selecteren en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-boomi-tutorial/IC790825.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **URL van toepassing configureren** in het tekstvak **URL van Boomi antwoord** uw **Boomi AtomSphere aanmeldings-URL** (bijvoorbeeld: "*https://platform.boomi.com/sso/AccountName/saml*"), en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-boomi-tutorial/IC790826.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op Boomi** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-boomi-tutorial/IC790827.png "Eenmalige aanmelding configureren")

5.  Log in op uw site Boomi bedrijf als beheerder in een ander web browser-venster.

6.  Klik in de werkbalk bovenaan uw bedrijfsnaam en **Setup**.

    ![Setup] (./media/active-directory-saas-boomi-tutorial/IC790828.png "Setup")

7.  Klik op **Opties voor eenmalige aanmelding**.

    ![Opties voor eenmalige aanmelding] (./media/active-directory-saas-boomi-tutorial/IC790829.png "Opties voor eenmalige aanmelding")

8.  In de sectie **Opties voor Single Sign-On** , voert u de volgende stappen uit:

    ![Single Sign-On-opties] (./media/active-directory-saas-boomi-tutorial/IC790830.png "Single Sign-On-opties")

    1.  Selecteer **eenmalige aanmelding SAML inschakelen**.
    2.  Klik op **importeren**om het gedownloade certificaat uploaden.
    3.  De waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in de textbox **Identity Provider aanmeldings-URL** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Boomi** .
    4.  Als **Locatie voor Federation-Id**, selecteer **Federation-Id wordt NameID element van het onderwerp**.
    5.  Klik op **Opslaan**.

9.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-boomi-tutorial/IC775560.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Om gebruikers aan te melden bij Boomi Azure AD, moeten zij worden ingericht in Boomi.  
Boomi is inrichten een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder uw bedrijf **Boomi** site.

2.  Ga naar **User Management \> gebruikers**.

    ![Gebruikers] (./media/active-directory-saas-boomi-tutorial/IC790831.png "Gebruikers")

3.  Klik op **gebruiker toevoegen**.

    ![Gebruiker toevoegen] (./media/active-directory-saas-boomi-tutorial/IC790832.png "Gebruiker toevoegen")

4.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersrollen toevoegen** :

    ![Gebruiker toevoegen] (./media/active-directory-saas-boomi-tutorial/IC790833.png "Gebruiker toevoegen")

    1.  Typ de **Voornaam**, **Achternaam** en **e-mailadres** van een geldige AAD-account die u wilt inrichten in de verwante tekstvakken.
    2.  Klik op OK.

>[AZURE.NOTE] Kunt u een andere Boomi gebruiker account maken van hulpprogramma's of API's geleverd door Boomi bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-boomi-perform-the-following-steps"></a>Gebruikers toewijzen aan Boomi, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Boomi **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-boomi-tutorial/IC790834.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-boomi-tutorial/IC767830.png "Ja")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
