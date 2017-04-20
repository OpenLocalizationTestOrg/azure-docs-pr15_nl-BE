<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met adaptieve Suite | Microsoft Azure"
    description="Meer informatie over het adaptieve Suite met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-adaptive-suite"></a>Zelfstudie: Azure Active Directory-integratie met adaptieve Suite

Het doel van deze zelfstudie is de integratie van de Azure en adaptieve Suite weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder adaptieve Suite

Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan adaptieve Suite kan worden met één teken in de toepassing van de adaptieve Suite bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor adaptieve Suite inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-adaptive-suite-tutorial/IC805637.png "Scenario")
##<a name="enabling-the-application-integration-for-adaptive-suite"></a>De integratie van toepassingen voor adaptieve Suite inschakelen

Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor adaptieve Suite overzicht.

###<a name="to-enable-the-application-integration-for-adaptive-suite-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor adaptieve Suite, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-adaptive-suite-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-adaptive-suite-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-adaptive-suite-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-adaptive-suite-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak**, **Adaptieve Suite**.

    ![Galerie van toepassing] (./media/active-directory-saas-adaptive-suite-tutorial/IC805638.png "Galerie van toepassing")

7.  Selecteer **Adaptieve Suite**in het resultatenvenster en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Adaptieve Suite] (./media/active-directory-saas-adaptive-suite-tutorial/IC805639.png "Adaptieve Suite")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij adaptieve Suite met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Configureren van eenmalige aanmelding voor adaptieve Suite, moet u een waarde uit een certificaat ophalen.  
Als u niet bekend met deze procedure bent, raadpleegt u [voor het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, op de pagina **Geavanceerde Suite** toepassing integratie **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-adaptive-suite-tutorial/IC805640.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Adaptief Suite** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-adaptive-suite-tutorial/IC805641.png "Eenmalige aanmelding configureren")

3.  Typ in het tekstvak **URL van het antwoord** op de pagina **Toepassingsinstellingen configureren** uw URL met behulp van het volgende patroon "*samlsso-https://login.adaptiveinsights.com:443/RlJFRVRSSUFMMTI3MTE =*', en klik op **volgende**.

    >[AZURE.NOTE] U kunt deze waarde uit de adaptieve Suite **SAML SSO-** pagina.

    ![App-instellingen configureren] (./media/active-directory-saas-adaptive-suite-tutorial/IC805642.png "App-instellingen configureren")

4.  Op de pagina **configureren eenmalige aanmelding op Adaptief Suite** downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-adaptive-suite-tutorial/IC805643.png "Eenmalige aanmelding configureren")

5.  Log in op uw site Adaptive Suite bedrijf als beheerder in een ander web browser-venster.

6.  Ga naar **Admin**.

    ![Admin] (./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Admin")

7.  Klik in de sectie **gebruikers en rollen** **Beheren SAML SSO-instellingen**.

    ![SAML SSO-instellingen beheren] (./media/active-directory-saas-adaptive-suite-tutorial/IC805645.png "SAML SSO-instellingen beheren")

8.  Op de pagina **Instellingen van SAML SSO** , kunt u de volgende stappen uitvoeren:

    ![SAML SSO-instellingen] (./media/active-directory-saas-adaptive-suite-tutorial/IC805646.png "SAML SSO-instellingen")

    1.  Typ in het tekstvak **naam van identiteit** een naam voor uw configuratie.
    2.  In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding op Adaptief Suite** de **Entiteit-ID** -waarde kopiëren en vervolgens plakken in het tekstvak **identiteitsprovider entiteits-ID** .
    3.  De waarde van **SAML SSO-URL** kopiëren en vervolgens plakken in het tekstvak **identiteitsprovider SSO-URL** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op Adaptief Suite** .
    4.  De waarde van **SAML SSO-URL** kopiëren en vervolgens plakken in het tekstvak **aangepaste logout URL** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op Adaptief Suite** .
    5.  Als u wilt uw gedownloade certificaat uploaden, klikt u op **bestand kiezen**.
    6.  Als het **SAML-gebruikers-id**, **Adaptive Insights gebruikersnaam van gebruiker**te selecteren.
    7.  Als **de locatie-id van SAML**, **gebruikers-id in NameID van onderwerp**te selecteren.
    8.  **NameID SAML-indeling**, selecteer **e-mailadres**.
    9.  **SAML inschakelen**, schakel **toestaan SAML SSO en directe Adaptive Insights login**.
    10. Klik op **Opslaan**.

9.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-adaptive-suite-tutorial/IC805647.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Om gebruikers aan te melden bij de adaptieve Suite Azure AD, moeten zij worden ingericht in adaptieve Suite.  
Adaptieve Suite is ingericht een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder uw bedrijf **Adaptieve Suite** site.

2.  Ga naar **Admin**.

    ![Admin] (./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Admin")

3.  Klik op **Gebruiker toevoegen**in de sectie **gebruikers en rollen** .

    ![Gebruiker toevoegen] (./media/active-directory-saas-adaptive-suite-tutorial/IC805648.png "Gebruiker toevoegen")

4.  In de sectie **Nieuwe gebruiker** de volgende stappen uitvoeren:

    ![Indienen] (./media/active-directory-saas-adaptive-suite-tutorial/IC805649.png "Indienen")

    1.  Typ de **naam**, **Login**, **e-mailadres**, het **wachtwoord** van een geldige Azure Active Directory-gebruiker die u verrichten in de verwante tekstvakken wilt.
    2.  Selecteer een **rol**.
    3.  Klik op **indienen**.

>[AZURE.NOTE] Kunt u een andere adaptieve Suite gebruiker account hulpmiddelen voor het maken of API's geleverd door adaptieve Suite bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-adaptive-suite-perform-the-following-steps"></a>Gebruikers toewijzen aan adaptieve Suite, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op de pagina **Geavanceerde Suite **toepassing integratie op **gebruikers toewijzen**.

    ![Gebruikers toewijzen] (./media/active-directory-saas-adaptive-suite-tutorial/IC805650.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-adaptive-suite-tutorial/IC767830.png "Ja")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
