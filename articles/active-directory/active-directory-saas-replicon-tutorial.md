<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Replicon | Microsoft Azure" 
    description="Meer informatie over het Replicon met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-replicon"></a>Zelfstudie: Azure Active Directory-integratie met Replicon
  
Het doel van deze zelfstudie is de integratie van de Azure en Replicon weergeven. Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder Replicon
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Replicon kan worden naar één teken in de toepassing van de Replicon bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Replicon
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-replicon-tutorial/IC777798.png "Scenario")
##<a name="enabling-the-application-integration-for-replicon"></a>Inschakelen van de integratie van toepassingen voor Replicon
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Replicon.

###<a name="to-enable-the-application-integration-for-replicon-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Replicon, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-replicon-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-replicon-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-replicon-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-replicon-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **Replicon**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-replicon-tutorial/IC777799.png "Galerie van toepassing")

7.  **Replicon**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Replicon] (./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Replicon met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Replicon** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-replicon-tutorial/IC777801.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Replicon** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-replicon-tutorial/IC777802.png "Eenmalige aanmelding configureren")

3.  Op de pagina **URL van App configureren** , moet u de volgende stappen uitvoeren:

    ![App-URL configureren] (./media/active-directory-saas-replicon-tutorial/IC777803.png "App-URL configureren")

    1.  Typ in het tekstvak **Replicon teken op URL** uw URL Replicon huurder (bijvoorbeeld: *https://na2.replicon.com/company/saml2/sp-sso/post*).
    2.  Typ in het tekstvak **URL van Replicon antwoord op** uw URL Replicon **AssertionConsumerService** (bv.: *https://global.replicon.com/! / saml2/bedrijf/sso/post*).  

        >[AZURE.NOTE] Kun je de URL van de metagegevens van de Replicon op:         **https://global.replicon.com/! /saml2/\<YourCompanyKey\>**.

    3.  Klik op **volgende**

4.  Op de pagina **configureren eenmalige aanmelding bij Replicon** downloaden van de metagegevens op **metagegevens downloaden**en vervolgens de metagegevens opslaan op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-replicon-tutorial/IC777804.png "Eenmalige aanmelding configureren")

5.  Log in op uw site Replicon bedrijf als beheerder in een ander web browser-venster.

6.  Configureren van SAML 2.0, moet u de volgende stappen uitvoeren:

    ![Verificatie van SAML inschakelen] (./media/active-directory-saas-replicon-tutorial/IC777805.png "Verificatie van SAML inschakelen")

    1.  Toevoegen als u wilt weergeven in het dialoogvenster **EnableSAML Authentication2** , de volgende aan uw URL, na de sleutel van uw bedrijf:  
        **/Services/SecurityService1.svc/Help/test/EnableSAMLAuthentication2**  
        Hier ziet u het schema van de volledige URL:  
        **https://Na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**
    2.  Klik op de **+** de **v20Configuration** sectie uitvouwen.
    3.  Klik op de **+** de **metaDataConfiguration** sectie uitvouwen.
    4.  Klik op **Bestand kiezen**om uw identiteit provider metagegevens XML-bestand te selecteren en klik op **verzenden**.

7.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-replicon-tutorial/IC778418.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij Replicon Azure AD, moeten zij worden ingericht in Replicon.  
Bij Replicon is het inrichten van een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Log in op uw site Replicon bedrijf als beheerder in een webbrowservenster.

2.  Ga naar **beheer \> gebruikers**.

    ![Gebruikers] (./media/active-directory-saas-replicon-tutorial/IC777806.png "Gebruikers")

3.  Klik op **+ Voeg gebruiker toe**.

    ![Gebruiker toevoegen] (./media/active-directory-saas-replicon-tutorial/IC777807.png "Gebruiker toevoegen")

4.  In de sectie **Gebruikersprofiel** , voert u de volgende stappen uit:

    ![Gebruikersprofiel] (./media/active-directory-saas-replicon-tutorial/IC777808.png "Gebruikersprofiel")

    1.  Typ in het tekstvak **Naam van aanmelding** het Azure AD e-mailadres van de Azure AD-gebruiker die u verrichten wilt.
    2.  Selecteer als **Type verificatie**, **eenmalige aanmelding**.
    3.  Typ in het tekstvak **afdeling** afdeling van de gebruiker.
    4.  Selecteer als **Type werknemer**, **beheerder**.
    5.  Klik op **profiel opslaan**.

>[AZURE.NOTE]Kunt u een andere Replicon gebruiker account maken van hulpprogramma's of API's geleverd door Replicon bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-replicon-perform-the-following-steps"></a>Gebruikers toewijzen aan Replicon, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Replicon **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-replicon-tutorial/IC777809.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-replicon-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.