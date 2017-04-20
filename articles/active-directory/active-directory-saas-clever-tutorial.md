<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Clever | Microsoft Azure" 
    description="Meer informatie over het Clever met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-clever"></a>Zelfstudie: Azure Active Directory-integratie met Clever

Het doel van deze zelfstudie is de integratie van de Azure en Clever weergeven. Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een slimme huurder

Na het voltooien van deze zelfstudie, is de Azure AD-gebruikers die u hebt toegewezen aan Clever mogelijk voor één teken in de toepassing van de slimme bedrijf-site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor Clever inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-clever-tutorial/IC798977.png "Scenario")
##<a name="enabling-the-application-integration-for-clever"></a>De integratie van toepassingen voor Clever inschakelen

Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Clever.

###<a name="to-enable-the-application-integration-for-clever-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Clever, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-clever-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-clever-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-clever-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-clever-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **Clever**.

    ![Galerie van toepassing] (./media/active-directory-saas-clever-tutorial/IC798978.png "Galerie van toepassing")

7.  **Clever**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Slimme] (./media/active-directory-saas-clever-tutorial/IC798979.png "Slimme")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Clever met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
De slimme toepassing verwacht de SAML-bevestigingen in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de **saml-token kenmerken** .  
De volgende schermafbeelding ziet u een voorbeeld voor dit.

![Kenmerken] (./media/active-directory-saas-clever-tutorial/IC798980.png "Kenmerken")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Clever** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-clever-tutorial/IC784682.png "Eenmalige aanmelding configureren")

2.  Op de pagina **Hoe wilt u dat gebruikers aanmelden bij Clever** Selecteer **AD Azure Microsoft Single Sign-On**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-clever-tutorial/IC798981.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App URL configureren** , in het tekstvak **Slimme teken op URL** de URL die door uw gebruikers voor aanmelding bij uw slimme toepassing gebruikt (bijvoorbeeld: *https://clever.com/in/azsandbox*), en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-clever-tutorial/IC798982.png "App-URL configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Clever** downloaden van de metagegevens op **metagegevens downloaden**en sla het bestand lokaal op uw computer met metagegevens.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-clever-tutorial/IC798983.png "Eenmalige aanmelding configureren")

5.  Log in op uw site slimme bedrijf als beheerder in een ander web browser-venster.

6.  Klik in de werkbalk **Direct aanmelden**.

    ![Direct aanmelden] (./media/active-directory-saas-clever-tutorial/IC798984.png "Direct aanmelden")

7.  Klik op de pagina **Aanmelden voor expresberichten** kunt u de volgende stappen uitvoeren:

    ![Direct aanmelden] (./media/active-directory-saas-clever-tutorial/IC798985.png "Direct aanmelden")

    1.  Hier typt u de **aanmeldings-URL**.  

        >[AZURE.NOTE] De **Aanmeldings-URL** is een aangepaste waarde.
De werkelijke waarde krijgt u bij uw slimme support team.

    2.  Als de **Identiteit System**, selecteer **AD FS**.
    3.  Klik op **Opslaan**.

8.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-clever-tutorial/IC798986.png "Eenmalige aanmelding configureren")

9.  In het menu aan de bovenkant, klikt u op **kenmerken** om de **SAML-Token kenmerken** -dialoogvenster te openen.

    ![Kenmerken] (./media/active-directory-saas-clever-tutorial/IC795920.png "Kenmerken")

10. Als u wilt de toewijzingen vereist kenmerk toevoegen, moet u de volgende stappen uitvoeren:

    ![SAML-token kenmerken] (./media/active-directory-saas-clever-tutorial/IC795921.png "SAML-token kenmerken")

  	|Naam van kenmerk|Waarde van het kenmerk|
  	|---|---|
  	|clever.student.credentials.district\_gebruikersnaam|User.userPrincipalName|

    1.  Voor elke rij met gegevens in de bovenstaande tabel, klikt u op **gebruikerskenmerk toevoegen**.
    2.  Typ de naam van het kenmerk voor de rij weergegeven in het tekstvak **Naam van kenmerk** .
    3.  Selecteer in het tekstvak **Waarde van het kenmerk** de kenmerkwaarde voor die rij weergegeven.
    4.  Klik op **Voltooien**.

11. Klik op **wijzigingen toepassen**.

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Om gebruikers aan te melden bij Clever Azure AD, moeten zij worden ingericht in Clever.  
Clever is inrichten een handmatige taak die moet worden uitgevoerd door uw slimme support team.

>[AZURE.NOTE] Kunt u een andere slimme gebruiker account hulpmiddelen voor het maken of API's geleverd door Clever bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-clever-perform-the-following-steps"></a>Gebruikers toewijzen aan Clever, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Clever **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-clever-tutorial/IC798987.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-clever-tutorial/IC767830.png "Ja")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
