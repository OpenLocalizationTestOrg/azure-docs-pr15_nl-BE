<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Veracode | Microsoft Azure" 
    description="Meer informatie over het Veracode met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-veracode"></a>Zelfstudie: Azure Active Directory-integratie met Veracode
  
Het doel van deze zelfstudie is de integratie van de Azure en Veracode weergeven. Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een Veracode eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Veracode kan worden naar één teken in de toepassing met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Veracode
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-veracode-tutorial/IC802903.png "Scenario")

##<a name="enabling-the-application-integration-for-veracode"></a>Inschakelen van de integratie van toepassingen voor Veracode
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Veracode.

###<a name="to-enable-the-application-integration-for-veracode-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Veracode, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-veracode-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-veracode-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-veracode-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-veracode-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **Veracode**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-veracode-tutorial/IC802904.png "Galerie van toepassing")

7.  **Veracode**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Veracode] (./media/active-directory-saas-veracode-tutorial/IC802905.png "Veracode")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Veracode met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Uw toepassing Veracode verwacht de SAML-bevestigingen in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de **saml-token kenmerken** .  
De volgende schermafbeelding ziet u een voorbeeld voor dit.

![Kenmerken] (./media/active-directory-saas-veracode-tutorial/IC802906.png "Kenmerken")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Veracode** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-veracode-tutorial/IC802907.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Veracode** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-veracode-tutorial/IC802908.png "Eenmalige aanmelding configureren")

3.  Klik op **volgende**op de pagina **Toepassingsinstellingen configureren** .

    ![App-instellingen configureren] (./media/active-directory-saas-veracode-tutorial/IC802909.png "App-instellingen configureren")

4.  Op de pagina **configureren eenmalige aanmelding op Veracode** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-veracode-tutorial/IC802910.png "Eenmalige aanmelding configureren")

5.  Log in op uw site Veracode bedrijf als beheerder in een ander web browser-venster.

6.  In het menu aan de bovenkant, klikt u op **Instellingen**en klik vervolgens op **beheer**.

    ![Beheer] (./media/active-directory-saas-veracode-tutorial/IC802911.png "Beheer")

7.  Klik op het tabblad **SAML** .

8.  Voer de volgende stappen uit in de sectie **Organisatie SAML-instellingen** :

    ![Beheer] (./media/active-directory-saas-veracode-tutorial/IC802912.png "Beheer")

    1.  In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding bij Veracode** de **Uitgever URL** -waarde kopiëren en vervolgens plakken in het tekstvak van de **uitgevende instelling**
    2.  Als u wilt uw gedownloade certificaat uploaden, klikt u op **Bestand kiezen**.
    3.  Selecteer **de zelf-registratie inschakelen**.

9.  Voer de volgende stappen uit in de sectie **Zelf registratie-instellingen** en klik op **Opslaan**:

    ![Beheer] (./media/active-directory-saas-veracode-tutorial/IC802913.png "Beheer")

    1.  Als **Nieuwe gebruiker activeren**, selecteert u **Geen activering vereist**.
    2.  Als een **Gebruiker Gegevensupdates**, **Voorkeur voor Veracode gebruikersgegevens**te selecteren.
    3.  Voor **Details van de SAML-kenmerk**, selecteer het volgende:
        -   **Gebruikersrollen**
        -   **Policy Administrator**
        -   **Revisor**
        -   **Beveiliging, Lead**
        -   **Executive**
        -   **Indiener**
        -   **De maker**
        -   **Alle Scan typen**
        -   **Team lidmaatschappen**
        -   **Standaardteam**

10. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-veracode-tutorial/IC802914.png "Eenmalige aanmelding configureren")

11. In het menu aan de bovenkant, klikt u op **kenmerken** om de **SAML-Token kenmerken** -dialoogvenster te openen.

    ![Kenmerken] (./media/active-directory-saas-veracode-tutorial/IC795920.png "Kenmerken")

12. Als u wilt de toewijzingen vereist kenmerk toevoegen, moet u de volgende stappen uitvoeren:

    ![Kenmerken] (./media/active-directory-saas-veracode-tutorial/IC802906.png "Kenmerken")

  	| Naam van kenmerk | Waarde van het kenmerk |
  	|:---------------|:----------------|
  	| Voornaam      | User.givenName  |
  	| Achternaam       | User.surname    |
  	| E-mail          | User.mail       |

    1.  Voor elke rij met gegevens in de bovenstaande tabel, klikt u op **gebruikerskenmerk toevoegen**.
    
    2.  Typ de naam van het kenmerk voor de rij weergegeven in het tekstvak **Naam van kenmerk** .

    3.  Selecteer in het tekstvak **Waarde van het kenmerk** de kenmerkwaarde voor die rij weergegeven.

    4.  Klik op **Voltooien**.

13. Klik op **wijzigingen toepassen**.

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij Veracode Azure AD, moeten zij worden ingericht in Veracode.  
Bij Veracode is het inrichten van een geautomatiseerde taak.  
Er is geen actie-item voor u...
  
Gebruikers worden automatisch gemaakt als nodig tijdens de eerste single sign-on.

>[AZURE.NOTE] Kunt u een andere Veracode gebruiker account maken van hulpprogramma's of API's geleverd door Veracode bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-veracode-perform-the-following-steps"></a>Gebruikers toewijzen aan Veracode, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Veracode **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-veracode-tutorial/IC802915.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-veracode-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.