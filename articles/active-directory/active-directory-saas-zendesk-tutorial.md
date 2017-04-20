<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Zendesk | Microsoft Azure" 
    description="Meer informatie over het Zendesk met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Zelfstudie: Azure Active Directory-integratie met Zendesk
  
Het doel van deze zelfstudie is de integratie van de Azure en Zendesk weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder Zendesk
  
Na het voltooien van deze zelfstudie, is de Azure AD-gebruikers die u hebt toegewezen aan Zendesk mogelijk voor één teken in de toepassing van de Zendesk bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor Zendesk inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-zendesk-tutorial/IC773083.png "Scenario")

##<a name="enabling-the-application-integration-for-zendesk"></a>De integratie van toepassingen voor Zendesk inschakelen
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Zendesk.

###<a name="to-enable-the-application-integration-for-zendesk-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Zendesk, kunt u de volgende stappen uitvoeren:

1.  Klik in de Portal Azure Management in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zendesk-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-zendesk-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-zendesk-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-zendesk-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **Zendesk**.

    ![Galerie van toepassing] (./media/active-directory-saas-zendesk-tutorial/IC773084.png "Galerie van toepassing")

7.  In het resultatendeelvenster **Zendesk**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Zendesk] (./media/active-directory-saas-zendesk-tutorial/IC773085.png "Zendesk")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Zendesk met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Eenmalige aanmelding voor Zendesk configureren, moet u een waarde uit een certificaat ophalen.  
Als u niet bekend met deze procedure bent, raadpleegt u [voor het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen in de portal Azure AD op de pagina **Zendesk** application integration.

    ![Eenmalige aanmelding] (./media/active-directory-saas-zendesk-tutorial/IC773086.png "Eenmalige aanmelding")

2.  Op de pagina **Hoe wilt u dat gebruikers aanmelden bij Zendesk** Selecteer **AD Azure Microsoft Single Sign-On**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zendesk-tutorial/IC773087.png "Eenmalige aanmelding configureren")

3.  Op de pagina **URL van App configureren** , moet u de volgende stappen uitvoeren:

    ![App-URL configureren] (./media/active-directory-saas-zendesk-tutorial/IC773088.png "App-URL configureren")
  
    een. Typ in het tekstvak **Zendesk teken In de URL** de URL van uw gebruik van het volgende patroon:`https://<tenant-name>.zendesk.com`

    b. Klik op **volgende**.



4.  Klik op **certificaat downloaden**en sla het bestand lokaal op uw compiter op de pagina **configureren eenmalige aanmelding op Zendesk** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zendesk-tutorial/IC777534.png "Eenmalige aanmelding configureren")

5.  Log in op uw site Zendesk bedrijf als beheerder in een ander web browser-venster.

6.  Klik op de **beheerder**.

7.  Klik op **Instellingen**in het linkernavigatievenster en klik vervolgens op **beveiliging**.

    ![Beveiliging] (./media/active-directory-saas-zendesk-tutorial/IC773089.png "Beveiliging")

8.  Klik op het tabblad **beheer en agenten** op de pagina **beveiliging** .

9.  Selecteer **eenmalige aanmelding (SSO) en SAML**, en selecteer vervolgens **SAML**.

10. De waarde van **SAML SSO-URL** kopiëren en vervolgens plakken in het tekstvak **URL van SAML SSO** in Azure AD-portal op de pagina **configureren eenmalige aanmelding op Zendesk** .

11. De waarde van de **Externe Logout URL** kopiëren en vervolgens plakken in het tekstvak **URL van externe Logout** in Azure AD-portal op de pagina **configureren eenmalige aanmelding op Zendesk** .

    ![Eenmalige aanmelding] (./media/active-directory-saas-zendesk-tutorial/IC773090.png "Eenmalige aanmelding")

12. **De waarde** van het geëxporteerde certificaat kopiëren en vervolgens plakken in het tekstvak **Certificaat vingerafdruk** .

    >[AZURE.TIP] Zie voor meer informatie [hoe u kunt ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

13. Klik op **Opslaan**.

14. Op de portal Azure AD selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **Voltooien** om het **Configureren van eenmalige aanmelding** dialoogvenster te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zendesk-tutorial/IC773093.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij **Zendesk**Azure AD, moeten zij worden ingericht in **Zendesk**.  
**Zendesk**is inrichten een handmatige taak.

###<a name="to-provision-a-user-account-to-zendesk-perform-the-following-steps"></a>Om een gebruikersaccount aan Zendesk inrichten, kunt u de volgende stappen uitvoeren:

1.  Log in op uw huurder **Zendesk** .

2.  Selecteer het tabblad **Klantenlijst** .

3.  Selecteer het tabblad **gebruiker** en klik op **toevoegen**.

    ![Gebruiker toevoegen] (./media/active-directory-saas-zendesk-tutorial/IC773632.png "Gebruiker toevoegen")

4.  Typ het e-mailadres van een bestaande Azure AD-account u wilt verrichten, en klik vervolgens op **Opslaan**.

    ![Nieuwe gebruiker] (./media/active-directory-saas-zendesk-tutorial/IC773633.png "Nieuwe gebruiker")

>[AZURE.NOTE] Kunt u een andere Zendesk gebruiker account maken van hulpprogramma's of API's geleverd door Zendesk bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-zendesk-perform-the-following-steps"></a>Gebruikers toewijzen aan Zendesk, voert u de volgende stappen uit:

1.  Maak een testaccount in de portal Azure AD.

2.  Klik op **gebruikers toewijzen**op de pagina **Zendesk **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-zendesk-tutorial/IC773094.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-zendesk-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
