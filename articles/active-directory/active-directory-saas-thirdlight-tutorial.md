<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Thirdlight | Microsoft Azure" 
    description="Meer informatie over het Thirdlight met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-thirdlight"></a>Zelfstudie: Azure Active Directory-integratie met Thirdlight
  
Het doel van deze zelfstudie is de integratie van de Azure en Thirdlight weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een Thirdlight eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Thirdlight kan worden naar één teken in de toepassing van de Thirdlight bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Thirdlight
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-thirdlight-tutorial/IC805836.png "Scenario")

##<a name="enabling-the-application-integration-for-thirdlight"></a>Inschakelen van de integratie van toepassingen voor Thirdlight
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Thirdlight.

###<a name="to-enable-the-application-integration-for-thirdlight-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Thirdlight, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-thirdlight-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-thirdlight-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-thirdlight-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-thirdlight-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **Thirdlight**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-thirdlight-tutorial/IC805837.png "Galerie van toepassing")

7.  **Thirdlight**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![ThirdLight] (./media/active-directory-saas-thirdlight-tutorial/IC805838.png "ThirdLight")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Thirdlight met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Configureren van eenmalige aanmelding voor Thirdlight moet u een waarde uit een certificaat ophalen.  
Als u niet bekend met deze procedure bent, raadpleegt u [voor het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Thirdlight** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-thirdlight-tutorial/IC805839.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Thirdlight** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-thirdlight-tutorial/IC805840.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **URL van App configureren** , in het tekstvak **Thirdlight teken In de URL** de URL die wordt gebruikt door de gebruikers aan te melden op uw toepassing Thirdlight (bijvoorbeeld: "*http://azuresso2.thirdlight.com/*"), en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-thirdlight-tutorial/IC805841.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding bij Thirdlight** downloaden van de metagegevens op **metagegevens downloaden**en sla het bestand lokaal op uw computer met metagegevens.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-thirdlight-tutorial/IC805842.png "Eenmalige aanmelding configureren")

5.  Log in op uw site Thirdlight bedrijf als beheerder in een ander web browser-venster.

6.  Ga naar **configuratie \> System Administration**, en klik vervolgens op **SAML2**.

    ![System Administration] (./media/active-directory-saas-thirdlight-tutorial/IC805843.png "System Administration")

7.  Voer de volgende stappen uit in de configuratiesectie van SAML2:

    ![SAML Single Sign-On] (./media/active-directory-saas-thirdlight-tutorial/IC805844.png "SAML Single Sign-On")

    1.  Selecteer **eenmalige aanmelding SAML2 inschakelen**.
    2.  Selecteer als **bron voor IdP-metagegevens**, **IdP-metagegevens uit XML laden**.
    3.  De van het gedownloade metagegevensbestand te openen, de inhoud kopiëren en vervolgens plakken in het tekstvak **IdP Metadata XML** .
    4.  Klik op **Instellingen opslaan SAML2**.

8.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-thirdlight-tutorial/IC805845.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij Thirdlight Azure AD, moeten zij worden ingericht in Thirdlight.  
Bij Thirdlight is het inrichten van een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Log in op uw site **Thirdlight** bedrijf als beheerder.

2.  Ga naar het tabblad **gebruikers** .

3.  Selecteer **gebruikers en groepen**.

4.  Klik op de knop **nieuwe gebruiker toevoegen** .

5.  Voer **gebruikersnaam, naam of omschrijving, E-mail, kies een voorinstelling of van nieuwe leden van de groep** van een geldige AAD-account die u wilt verrichten.

6.  Klik op **maken**.

>[AZURE.NOTE] Kunt u een andere Thirdlight gebruiker account maken van hulpprogramma's of API's geleverd door Thirdlight bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-thirdlight-perform-the-following-steps"></a>Gebruikers toewijzen aan Thirdlight, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Thirdlight **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-thirdlight-tutorial/IC805846.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-thirdlight-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.