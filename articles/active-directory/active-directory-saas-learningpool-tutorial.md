<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Learningpool | Microsoft Azure" 
    description="Meer informatie over het Learningpool met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-learningpool"></a>Zelfstudie: Azure Active Directory-integratie met Learningpool
  
Het doel van deze zelfstudie is de integratie van de Azure en Learningpool weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een Learningpool eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Learningpool kan worden naar één teken in de toepassing van de Learningpool bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Learningpool
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-learningpool-tutorial/IC791166.png "Scenario")
##<a name="enabling-the-application-integration-for-learningpool"></a>Inschakelen van de integratie van toepassingen voor Learningpool
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Learningpool.

###<a name="to-enable-the-application-integration-for-learningpool-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Learningpool, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-learningpool-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-learningpool-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-learningpool-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-learningpool-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **Learningpool**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-learningpool-tutorial/IC795073.png "Galerie van toepassing")

7.  **Learningpool**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Learningpool] (./media/active-directory-saas-learningpool-tutorial/IC809577.png "Learningpool")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Learningpool met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.
  
Uw toepassing Learningpool verwacht de SAML-bevestigingen in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de **saml-token kenmerken** .  
De volgende schermafbeelding ziet u een voorbeeld voor dit.

![SAML-Token kenmerken] (./media/active-directory-saas-learningpool-tutorial/IC795074.png "SAML-Token kenmerken")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  In de klassieke Azure portal, op de pagina **Learningpool** toepassing integratie, in het menu aan de bovenkant, klikt u op **kenmerken** om de **SAML-Token kenmerken** -dialoogvenster te openen.

    ![Kenmerken] (./media/active-directory-saas-learningpool-tutorial/IC795075.png "Kenmerken")

2.  Als u wilt de toewijzingen vereist kenmerk toevoegen, moet u de volgende stappen uitvoeren:

    ###  

  	|Naam van kenmerk                |Waarde van het kenmerk            |
  	|------------------------------|---------------------------|

     urn: oid:1.2.840.113556.1.4.221 | User.userPrincipalName
  	|-------------------------------|--------------------------|  
     urn: oid:2.5.4.42|User.givenName   
  	|urn: oid:0.9.2342.19200300.100.1.3|User.mail
  	|urn: oid:2.5.4.4|User.surname

    1.  Voor elke rij met gegevens in de bovenstaande tabel, klikt u op **gebruikerskenmerk toevoegen**.
    2.  Typ de naam van het kenmerk voor de rij weergegeven in het tekstvak **Naam van kenmerk** .
    3.  Selecteer in de lijst **Waarde van het kenmerk** de kenmerkwaarde voor die rij weergegeven.
    4.  Klik op **Voltooien**.

3.  Klik op **wijzigingen toepassen**.

4.  Klik op **terug** om het dialoogvenster **Quick Start** opnieuw openen in de browser.

5.  Klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Singel eenmalige aanmelding configureren] (./media/active-directory-saas-learningpool-tutorial/IC795076.png "Singel eenmalige aanmelding configureren")

6.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Learningpool** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-learningpool-tutorial/IC795077.png "Eenmalige aanmelding configureren")

7.  Typ op de pagina **URL van App configureren** , in het tekstvak **Learningpool teken op URL** de URL die wordt gebruikt door de gebruikers aan te melden op uw toepassing Learningpool (bv.: https://parliament.preview.learningpool.com/auth/shibboleth/index.php), en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-learningpool-tutorial/IC795078.png "App-URL configureren")

8.  Klik op de pagina **configureren eenmalige aanmelding bij Learningpool** downloaden van de metagegevens op **metagegevens downloaden**en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-learningpool-tutorial/IC795079.png "Eenmalige aanmelding configureren")

9.  Stuur dat bestand metagegevens voor uw Learningpool Support team.

    >[AZURE.NOTE]Eenmalige aanmelding moet worden ingeschakeld door het ondersteuningsteam van Learningpool.

10. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-learningpool-tutorial/IC795080.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij Learningpool Azure AD, moeten zij worden ingericht in Learningpool.
  
Er is geen actie-item aan de Learningpool van gebruikers configureren.  
Gebruikers moeten worden gemaakt door uw Learningpool support team.

>[AZURE.NOTE]Kunt u een andere Learningpool gebruiker account maken van hulpprogramma's of API's geleverd door Learningpool bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-learningpool-perform-the-following-steps"></a>Gebruikers toewijzen aan Learningpool, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Learningpool **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-learningpool-tutorial/IC795081.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-learningpool-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.