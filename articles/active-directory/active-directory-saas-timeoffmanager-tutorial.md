<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met TimeOffManager | Microsoft Azure" 
    description="Meer informatie over het TimeOffManager met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-timeoffmanager"></a>Zelfstudie: Azure Active Directory-integratie met TimeOffManager
  
Het doel van deze zelfstudie is de integratie van de Azure en TimeOffManager weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een TimeOffManager eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan TimeOffManager kan worden naar één teken in de toepassing van de TimeOffManager bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor TimeOffManager
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-timeoffmanager-tutorial/IC795909.png "Scenario")

##<a name="enabling-the-application-integration-for-timeoffmanager"></a>Inschakelen van de integratie van toepassingen voor TimeOffManager
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor TimeOffManager.

###<a name="to-enable-the-application-integration-for-timeoffmanager-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor TimeOffManager, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-timeoffmanager-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-timeoffmanager-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-timeoffmanager-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-timeoffmanager-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **TimeOffManager**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-timeoffmanager-tutorial/IC795910.png "Galerie van toepassing")

7.  **TimeOffManager**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![TimeOffManager] (./media/active-directory-saas-timeoffmanager-tutorial/IC795911.png "TimeOffManager")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij TimeOffManager met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat uploaden naar uw huurder TimeOffManager.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **TimeOffManager** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-timeoffmanager-tutorial/IC795912.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op TimeOffManager** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-timeoffmanager-tutorial/IC795913.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **URL van App configureren** in het tekstvak **URL van TimeOffManager antwoord** uw URL TimeOffManager AssertionConsumerService (bijv.: "*Voorbeeld: https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company\_id = IC34216*", en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-timeoffmanager-tutorial/IC795914.png "App-URL configureren")

    Van het TimeOffManager met één teken op pagina-instelling kunt u de URL antwoord krijgen.

    ![Instellingen voor eenmalige aanmelding] (./media/active-directory-saas-timeoffmanager-tutorial/IC795915.png "Instellingen voor eenmalige aanmelding")

4.  Op de pagina **configureren eenmalige aanmelding op TimeOffManager** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-timeoffmanager-tutorial/IC795916.png "Eenmalige aanmelding configureren")

5.  Log in op uw site TimeOffManager bedrijf als beheerder in een ander web browser-venster.

6.  Ga naar **Account \> opties rekening \> Single Sign-On-instellingen**.

    ![Instellingen voor eenmalige aanmelding] (./media/active-directory-saas-timeoffmanager-tutorial/IC795917.png "Instellingen voor eenmalige aanmelding")

7.  Voer de volgende stappen uit in de sectie **Instellingen voor eenmalige aanmelding** :

    ![Instellingen voor eenmalige aanmelding] (./media/active-directory-saas-timeoffmanager-tutorial/IC795918.png "Instellingen voor eenmalige aanmelding")

    een.  Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.  

        >[AZURE.TIP] Zie voor meer informatie, [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    b.  De base-64 gecodeerde certificaat in Kladblok te openen, de inhoud ervan kopiëren naar het Klembord en plak het gehele certificaat in textbox **X.509-certificaat** .
    
    c.  Kopieer de waarde van de **URL van de uitgevende instelling** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op TimeOffManager** en plak deze in de textbox **Idp uitgever** .
    
    d.  De waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het tekstvak **URL van IdP-eindpunt** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op TimeOffManager** .
    
    e.  Als het **SAML afdwingen**, selecteert u **Nee**.
    

    f.  Als **Gebruikers automatisch maken**, selecteert u **Ja**.
    
    g.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij TimeOffManager** de waarde van de **Externe Logout URL** kopiëren en vervolgens plakken in het tekstvak **URL Logout** .
    
    h.  Klik op **wijzigingen opslaan**.

8.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op TimeOffManager** selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **Voltooien**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-timeoffmanager-tutorial/IC795919.png "Eenmalige aanmelding configureren")

9.  In het menu aan de bovenkant, klikt u op **kenmerken** om de **SAML-Token kenmerken** -dialoogvenster te openen.

    ![Kenmerken] (./media/active-directory-saas-timeoffmanager-tutorial/IC795920.png "Kenmerken")

10. Als u wilt de toewijzingen vereist kenmerk toevoegen, moet u de volgende stappen uitvoeren:

    ![SAML-token kenmerken] (./media/active-directory-saas-timeoffmanager-tutorial/123.png "SAML-token kenmerken")

  	|Naam van kenmerk|Waarde van het kenmerk|
  	|---|---|
  	|E-mail|User.mail|
  	|Voornaam|User.givenName|
  	|Achternaam|User.surname|

    een.  Voor elke rij met gegevens in de bovenstaande tabel, klikt u op **gebruikerskenmerk toevoegen**.

    b.  Typ de naam van het kenmerk voor de rij weergegeven in het tekstvak **Naam van kenmerk** .

    c.  Selecteer in het tekstvak **Waarde van het kenmerk** de kenmerkwaarde voor die rij weergegeven.

    d.  Klik op **Voltooien**.

11. Klik op **wijzigingen toepassen**.

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij TimeOffManager Azure AD, moeten ze ingericht voor TimeOffManager.  
TimeOffManager ondersteunt alleen in tijd van gebruikersaanvragen. Er is geen actie-item voor u.  
De gebruikers worden automatisch toegevoegd tijdens de eerste aanmelding met één teken op.

>[AZURE.NOTE] Kunt u een andere TimeOffManager gebruiker account maken van hulpprogramma's of API's geleverd door TimeOffManager bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-timeoffmanager-perform-the-following-steps"></a>Gebruikers toewijzen aan TimeOffManager, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **TimeOffManager** application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-timeoffmanager-tutorial/IC795922.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-timeoffmanager-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
