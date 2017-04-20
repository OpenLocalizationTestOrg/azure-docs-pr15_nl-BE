<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Onit | Microsoft Azure" 
    description="Meer informatie over het Onit met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-onit"></a>Zelfstudie: Azure Active Directory-integratie met Onit
  
Het doel van deze zelfstudie is de integratie van de Azure en Onit weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een Onit eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Onit kan worden naar één teken in de toepassing van de Onit bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Onit
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-onit-tutorial/IC791166.png "Scenario")
##<a name="enabling-the-application-integration-for-onit"></a>Inschakelen van de integratie van toepassingen voor Onit
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Onit.

###<a name="to-enable-the-application-integration-for-onit-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Onit, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-onit-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-onit-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-onit-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-onit-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **Onit**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-onit-tutorial/IC791167.png "Galerie van toepassing")

7.  **Onit**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Onit] (./media/active-directory-saas-onit-tutorial/IC795325.png "Onit")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Onit met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Configureren van eenmalige aanmelding voor Onit moet u een waarde uit een certificaat ophalen.  
Als u niet bekend met deze procedure bent, raadpleegt u [voor het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).
  
Uw toepassing Onit verwacht de SAML-bevestigingen in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de **saml-token kenmerken** .  
De volgende schermafbeelding ziet u een voorbeeld voor dit.

![Eenmalige aanmelding] (./media/active-directory-saas-onit-tutorial/IC791168.png "Eenmalige aanmelding")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  In de klassieke Azure portal, op de pagina **Onit** toepassing integratie, in het menu aan de bovenkant, klikt u op **kenmerken** om de **SAML-Token kenmerken** -dialoogvenster te openen.

    ![Kenmerken] (./media/active-directory-saas-onit-tutorial/IC791169.png "Kenmerken")

2.  Als u wilt de toewijzingen vereist kenmerk toevoegen, moet u de volgende stappen uitvoeren:

    
  	|Naam van kenmerk|Waarde van het kenmerk|
  	|---|---|
  	|naam|User.userPrincipalName|
  	|E-mail|User.mail|

    1.  Voor elke rij met gegevens in de bovenstaande tabel, klikt u op **gebruikerskenmerk toevoegen**.
    2.  Typ de naam van het kenmerk voor de rij weergegeven in het tekstvak **Naam van kenmerk** .
    3.  Selecteer in de lijst **Waarde van het kenmerk** de kenmerkwaarde voor die rij weergegeven.
    4.  Klik op **Voltooien**.

3.  Klik op **wijzigingen toepassen**.

4.  Klik op **terug** om het dialoogvenster **Quick Start** opnieuw openen in de browser.

5.  Klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-onit-tutorial/IC791170.png "Eenmalige aanmelding configureren")

6.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Onit** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-onit-tutorial/IC791171.png "Eenmalige aanmelding configureren")

7.  Typ op de pagina **URL van App configureren** , in het tekstvak **Onit teken op URL** de URL die wordt gebruikt door de gebruikers aan te melden op uw toepassing Onit (bijvoorbeeld: "*https://ms-sso-test.onit.com*"), en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-onit-tutorial/IC791172.png "App-URL configureren")

8.  Op de pagina **configureren eenmalige aanmelding op Onit** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-onit-tutorial/IC791173.png "Eenmalige aanmelding configureren")

9.  Log in op uw site Onit bedrijf als beheerder in een ander web browser-venster.

10. In het menu aan de bovenkant, klikt u op **beheer**.

    ![Beheer] (./media/active-directory-saas-onit-tutorial/IC791174.png "Beheer")

11. Klik op **Bewerken Corporation**.

    ![Corporation bewerken] (./media/active-directory-saas-onit-tutorial/IC791175.png "Corporation bewerken")

12. Klik op het tabblad **beveiliging** .

    ![Bedrijfsgegevens bewerken] (./media/active-directory-saas-onit-tutorial/IC791176.png "Bedrijfsgegevens bewerken")

13. Op het tabblad **beveiliging** kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding] (./media/active-directory-saas-onit-tutorial/IC791177.png "Eenmalige aanmelding")

    1.  Als **Authentication Strategy**, **eenmalige aanmelding en wachtwoord**te selecteren.
    2.  In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding bij Onit** de waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het tekstvak **Idp de doel-URL** .
    3.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Onit** de waarde van de **Externe Logout URL** kopiëren en vervolgens plakken in het tekstvak **URL van Idp logout** .
    4.  **De waarde** van het geëxporteerde certificaat kopiëren en vervolgens plakken in het tekstvak **Idp Cert vingerafdruk (SHA1)** .  

        >[AZURE.TIP] Zie voor meer informatie [hoe u kunt ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

    5.  **SAML** **SSO-Type**selecteren.
    6.  Typ in het tekstvak **tekst van SSO login knop** een gewenste knoptekst.
    7.  Selecteer **Login met eenmalige aanmelding: vereist voor de volgende domeinen/gebruikers**, typt u het e-mailadres van de testgebruiker voor een in het bijbehorende tekstvak en klik vervolgens op **bijwerken**.
        ![Corporation bewerken] (./media/active-directory-saas-onit-tutorial/IC791178.png "Corporation bewerken")

14. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-onit-tutorial/IC791179.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij Onit Azure AD, moeten zij worden ingericht in Onit.  
Bij Onit is het inrichten van een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Meld u aan uw site **Onit** bedrijf aan als beheerder.

2.  Klik op **gebruiker toevoegen**.

    ![Beheer] (./media/active-directory-saas-onit-tutorial/IC791180.png "Beheer")

3.  Op de pagina van het dialoogvenster **Gebruiker toevoegen** kunt u de volgende stappen uitvoeren:

    ![Gebruiker toevoegen] (./media/active-directory-saas-onit-tutorial/IC791181.png "Gebruiker toevoegen")

    1.  Typ de **naam** en het **E-mailadres** van een geldige AAD-account die u wilt inrichten in de verwante tekstvakken.
    2.  Klik op **maken**.  

        >[AZURE.NOTE] De eigenaar van de rekening krijgt een e-mailbericht met een koppeling naar de account bevestigen voordat deze actief wordt.

>[AZURE.NOTE] Kunt u een andere Onit gebruiker account maken van hulpprogramma's of API's geleverd door Onit bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-onit-perform-the-following-steps"></a>Gebruikers toewijzen aan Onit, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Onit **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-onit-tutorial/IC791182.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-onit-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.