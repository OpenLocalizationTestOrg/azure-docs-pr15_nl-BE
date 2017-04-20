<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met InsideView | Microsoft Azure" 
    description="Meer informatie over het InsideView met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-insideview"></a>Zelfstudie: Azure Active Directory-integratie met InsideView
  
Het doel van deze zelfstudie is de integratie van de Azure en InsideView weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder InsideView
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan InsideView kan worden naar één teken in de toepassing van de InsideView bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor InsideView
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-insideview-tutorial/IC794128.png "Scenario")
##<a name="enabling-the-application-integration-for-insideview"></a>Inschakelen van de integratie van toepassingen voor InsideView
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor InsideView.

###<a name="to-enable-the-application-integration-for-insideview-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor InsideView, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-insideview-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-insideview-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-insideview-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-insideview-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **InsideView**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-insideview-tutorial/IC794129.png "Galerie van toepassing")

7.  **InsideView**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![InsideView] (./media/active-directory-saas-insideview-tutorial/IC794130.png "InsideView")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij InsideView met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat-bestand te maken.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **InsideView** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Enkele aanmelding configureren] (./media/active-directory-saas-insideview-tutorial/IC794131.png "Enkele aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op InsideView** en klik op **volgende**.

    ![Enkele aanmelding configureren] (./media/active-directory-saas-insideview-tutorial/IC794132.png "Enkele aanmelding configureren")

3.  Typ op de pagina **App URL configureren** , in het tekstvak **InsideView antwoord URL** de URL van de SSO-InsideView (bv.: `https://my.insideview.com/iv/<STS Name>/login.iv`), en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-insideview-tutorial/IC794133.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op InsideView** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand op uw computer.

    ![Enkele aanmelding configureren] (./media/active-directory-saas-insideview-tutorial/IC794134.png "Enkele aanmelding configureren")

5.  Log in op uw site InsideView bedrijf als beheerder in een ander web browser-venster.

6.  **Admin**, **SingleSignOn instellingen**, klikt u op op de werkbalk in de rechterbovenhoek en klik vervolgens op **SAML toevoegen**.

    ![SAML eenmalige op instellingen] (./media/active-directory-saas-insideview-tutorial/IC794135.png "SAML eenmalige op instellingen")

7.  Voer de volgende stappen uit in de sectie **een nieuwe SAML toevoegen** :

    ![Een nieuwe SAML toevoegen] (./media/active-directory-saas-insideview-tutorial/IC794136.png "Een nieuwe SAML toevoegen")

    1.  Typ een naam voor de configuratieset in het tekstvak **Naam STS** .
    2.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op InsideView** de **Service Provider (SP) geïnitieerd Endpoint** -waarde kopiëren en vervolgens plakken in het tekstvak **WS-SamlP-Fed Unsolicated eindpunt** .
    3.  Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.
        
        >[AZURE.TIP]Zie voor meer informatie, [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    4.  De base-64 gecodeerde certificaat openen in Kladblok, de inhoud ervan kopiëren naar het Klembord en plak deze vervolgens naar de textbox **STS-certificaat**
    5.  Typ in het tekstvak **Crm Id Gebruikerskoppeling** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    6.  Typ in het tekstvak **Koppelen van Crm-e-** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    7.  Typ in het tekstvak **Crm eerst Gebruikersnaamtoewijzing** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    8.  Typ **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**in het tekstvak **lastName Crm toewijzen** .
    9.  Klik op **Opslaan**.

8.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Enkele aanmelding configureren] (./media/active-directory-saas-insideview-tutorial/IC794137.png "Enkele aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij InsideView Azure AD, moeten zij worden ingericht in InsideView.  
Bij InsideView is het inrichten van een handmatige taak.
  
Als u gebruikers of contactpersonen die zijn gemaakt in InsideView, neem contact op met uw customer succes manager of e-mailbericht te verzenden**support@insideview.com**

>[AZURE.NOTE] Kunt u een andere InsideView gebruiker account maken van hulpprogramma's of API's die door InsideView AD Azure gebruikersaccounts inrichten.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-insideview-perform-the-following-steps"></a>Gebruikers toewijzen aan InsideView, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **InsideView **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-insideview-tutorial/IC794138.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-insideview-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.