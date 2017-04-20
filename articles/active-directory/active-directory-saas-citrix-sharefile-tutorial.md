<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Citrix ShareFile | Microsoft Azure" 
    description="Meer informatie over het Citrix ShareFile met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a>Zelfstudie: Azure Active Directory-integratie met Citrix ShareFile

Het doel van deze zelfstudie is de integratie van de Azure en Citrix ShareFile weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder Citrix ShareFile

Na het voltooien van deze zelfstudie, is de Azure AD-gebruikers die u hebt toegewezen aan Citrix ShareFile mogelijk voor één teken in de toepassing van de Citrix ShareFile bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor Citrix ShareFile inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773620.png "Scenario")
##<a name="enabling-the-application-integration-for-citrix-sharefile"></a>De integratie van toepassingen voor Citrix ShareFile inschakelen

Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Citrix ShareFile.

###<a name="to-enable-the-application-integration-for-citrix-sharefile-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Citrix ShareFile, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-citrix-sharefile-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak**, **Citrix ShareFile**.

    ![Galerie van toepassing] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773621.png "Galerie van toepassing")

7.  **Citrix ShareFile**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Citrix ShareFile] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773622.png "Citrix ShareFile")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Citrix ShareFile met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Citrix ShareFile** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding inschakelen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773623.png "Eenmalige aanmelding inschakelen")

2.  Op de pagina **Hoe wilt u dat gebruikers aan te melden op Citrix ShareFile** Selecteer **AD Azure Microsoft Single Sign-On**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773624.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App URL configureren** , in het tekstvak **Citrix ShareFile teken op URL** de URL van uw volgende patroon met `https://<tenant-name>.shareFile.com`, en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773625.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op Citrix ShareFile** downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand op uw computer.

    ![ConfigureSingle aanmelding] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773626.png "ConfigureSingle aanmelding")

5.  Log in op uw site **Citrix ShareFile** bedrijf als beheerder in een ander web browser-venster.

6.  Klik in de werkbalk op de bovenkant, **Admin**.

7.  Selecteer in het linkernavigatievenster **Configureren van eenmalige aanmelding**.

    ![Account beheer] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773627.png "Account beheer")

8.  Op de **-On / configuratie van SAML 2.0** dialoogvenster pagina onder **Algemene instellingen**de volgende stappen uitvoeren:

    ![Eenmalige aanmelding] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773628.png "Eenmalige aanmelding")

    1.  Klik op **SAML inschakelen**.
    2.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op Citrix ShareFile** de **Entiteit-ID** -waarde kopiëren en plakken in de **uw IDP uitgever / entiteit-ID** textbox.
    3.  De waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het tekstvak **Aanmeldings-URL** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op Citrix ShareFile** .
    4.  De waarde van de **Externe Logout URL** kopiëren en vervolgens plakken in het tekstvak **URL Logout** in de Azure klassieke portal op **de Configure eenmalige aanmelding op Citrix ShareFile** dialoogvenster pagina.
    5.  Klik op **wijzigen** naast het veld **X.509-certificaat** en het certificaat dat u hebt gedownload van de klassieke portal Azure AD vervolgens uploaden.
        ![Basisinstellingen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773629.png "Basisinstellingen")

9.  Klik op **Opslaan** op de Citrix ShareFile management portal.

10. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773630.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Om gebruikers aan te melden bij Citrix ShareFile Azure AD, moeten zij worden ingericht in Citrix ShareFile.  
Citrix ShareFile is inrichten een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw huurder **Citrix ShareFile** .

2.  Klik op **gebruikers beheren \> thuis gebruikers beheren \> + werknemer**.

    ![Werknemer maken] (./media/active-directory-saas-citrix-sharefile-tutorial/IC781050.png "Werknemer maken")

3.  Voer het **e-mailadres**, **Voornaam** en **Achternaam** van een geldige Azure advertentie account u wilt verrichten.

    ![Basisinformatie] (./media/active-directory-saas-citrix-sharefile-tutorial/IC799951.png "Basisinformatie")

4.  Klik op **gebruiker toevoegen**.

    >[AZURE.NOTE] De accounthouder AAD ontvangt een e-mail en een link om te bevestigen hun account voordat deze actief wordt als volgt.

>[AZURE.NOTE] Kunt u alle andere Citrix ShareFile gebruiker account maken van hulpprogramma's of API's die door Citrix ShareFile bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-citrix-sharefile-perform-the-following-steps"></a>Gebruikers toewijzen aan Citrix ShareFile, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Citrix ShareFile **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773631.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-citrix-sharefile-tutorial/IC767830.png "Ja")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
