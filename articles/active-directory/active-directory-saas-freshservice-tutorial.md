<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met FreshService | Microsoft Azure" 
    description="Meer informatie over het FreshService met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-freshservice"></a>Zelfstudie: Azure Active Directory-integratie met FreshService
  
Het doel van deze zelfstudie is de integratie van de Azure en FreshService weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een FreshService eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan FreshService kan worden naar één teken in de toepassing met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor FreshService
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-freshservice-tutorial/IC790807.png "Scenario")
##<a name="enabling-the-application-integration-for-freshservice"></a>Inschakelen van de integratie van toepassingen voor FreshService
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor FreshService.

###<a name="to-enable-the-application-integration-for-freshservice-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor FreshService, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-freshservice-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-freshservice-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-freshservice-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-freshservice-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **FreshService**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-freshservice-tutorial/IC790808.png "Galerie van toepassing")

7.  **FreshService**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Freshservice] (./media/active-directory-saas-freshservice-tutorial/IC790809.png "Freshservice")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij FreshService met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Configureren van eenmalige aanmelding voor FreshService moet u een waarde uit een certificaat ophalen.  
Als u niet bekend met deze procedure bent, raadpleegt u [voor het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **FreshService** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-freshservice-tutorial/IC790810.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op FreshService** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-freshservice-tutorial/IC790811.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App URL configureren** , in het tekstvak **FreshService teken op URL** de URL die wordt gebruikt door de gebruikers aan te melden op uw toepassing Freshdesk (bijvoorbeeld: "*http://democompany.freshservice.com/*"), en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-freshservice-tutorial/IC790812.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op FreshService** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-freshservice-tutorial/IC790813.png "Eenmalige aanmelding configureren")

5.  Log in op uw site FreshService bedrijf als beheerder in een ander web browser-venster.

6.  In het menu aan de bovenkant, klik op de **beheerder**.

    ![Admin] (./media/active-directory-saas-freshservice-tutorial/IC790814.png "Admin")

7.  Klik op **beveiliging**in het **Portal voor klanten**.

    ![Beveiliging] (./media/active-directory-saas-freshservice-tutorial/IC790815.png "Beveiliging")

8.  In de sectie **beveiliging** voert u de volgende stappen uit:

    ![Eenmalige aanmelding] (./media/active-directory-saas-freshservice-tutorial/IC790816.png "Eenmalige aanmelding")

    1.  **Eenmalige OnON**overschakelen.
    2.  Selecteer **SAML SSO**.
    3.  De waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het tekstvak **SAML aanmeldings-URL** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op FreshService** .
    4.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij FreshService** de waarde van de **Externe Logout URL** kopiëren en vervolgens plakken in het tekstvak **URL Logout** .
    5.  **De waarde** van het geëxporteerde certificaat kopiëren en plak deze in de textbox **Beveiliging certificaat vingerafdruk** .
    
        >[AZURE.TIP]Zie voor meer informatie [hoe u kunt ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

9.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-freshservice-tutorial/IC790817.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij FreshService Azure AD, moeten zij worden ingericht in FreshService.  
Bij FreshService is het inrichten van een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw site **FreshService** bedrijf als beheerder.

2.  In het menu aan de bovenkant, klik op de **beheerder**.

    ![Admin] (./media/active-directory-saas-freshservice-tutorial/IC790814.png "Admin")

3.  Klik in de sectie **User Management** **aanvragers**.

    ![Aanvragers] (./media/active-directory-saas-freshservice-tutorial/IC790818.png "Aanvragers")

4.  Klik op **nieuwe aanvrager**.

    ![Nieuwe aanvragers] (./media/active-directory-saas-freshservice-tutorial/IC790819.png "Nieuwe aanvragers")

5.  Voer de volgende stappen uit in de sectie **Nieuwe aanvrager** :

    ![Nieuwe aanvrager] (./media/active-directory-saas-freshservice-tutorial/IC790820.png "Nieuwe aanvrager")

    1.  De kenmerken **Voornaam** en **e-mailadres** van een geldige Azure Active Directory-account die u wilt naar de voorziening in de verwante tekstvakken invoeren.
    2.  Klik op **Opslaan**.

    >[AZURE.NOTE] De accounthouder Azure Active Directory krijgt een e-mailbericht met een koppeling naar de account bevestigen voordat deze actief wordt

>[AZURE.NOTE] Kunt u een andere FreshService gebruiker account maken van hulpprogramma's of API's geleverd door FreshService bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-freshservice-perform-the-following-steps"></a>Gebruikers toewijzen aan FreshService, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **FreshService **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-freshservice-tutorial/IC790821.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-freshservice-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.