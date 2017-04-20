<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Zscaler | Microsoft Azure" 
    description="Meer informatie over het Zscaler met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zscaler"></a>Zelfstudie: Azure Active Directory-integratie met Zscaler
  
Het doel van deze zelfstudie is de integratie van de Azure en Zscaler weergeven. Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder in Zscaler
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Zscaler
2.  Eenmalige aanmelding configureren
3.  Proxy-instellingen configureren
4.  Gebruikersaanvragen configureren
5.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-zscaler-tutorial/IC769226.png "Scenario")

##<a name="enabling-the-application-integration-for-zscaler"></a>Inschakelen van de integratie van toepassingen voor Zscaler
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Zscaler.

###<a name="to-enable-the-application-integration-for-zscaler-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Zscaler, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zscaler-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-zscaler-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-zscaler-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-zscaler-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **Zscaler**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-zscaler-tutorial/IC769227.png "Galerie van toepassing")

7.  **Zscaler**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Zscaler] (./media/active-directory-saas-zscaler-tutorial/IC769228.png "Zscaler")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Zscaler met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure moet u een certificaat uploaden naar Zscaler.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Zscaler** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding inschakelen] (./media/active-directory-saas-zscaler-tutorial/IC769229.png "Eenmalige aanmelding inschakelen")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Zscaler** en klik op **volgende**.

    ![Aanmelden voor één configureren] (./media/active-directory-saas-zscaler-tutorial/IC769230.png "Aanmelden voor één configureren")

3.  Typ uw teken in de URL die u hebt gekregen van de Zscaler op de pagina **Configureren App-URL** in het tekstvak **Zscaler teken In de URL** en klik op **volgende**: 

    >[AZURE.NOTE] Neem contact op met het ondersteuningsteam van Zscaler als u niet weet wat uw teken in de URL is.

    ![App-URL configureren] (./media/active-directory-saas-zscaler-tutorial/IC769231.png "App-URL configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding op Zscaler** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zscaler-tutorial/IC769232.png "Eenmalige aanmelding configureren")

    1.  Klik op **certificaat downloaden**en sla het bestand lokaal als **c:\\Zscaler.cer**.
    2.  De **aanvraag-URL voor verificatie** naar het Klembord kopiëren.

5.  Aanmelden bij uw huurder Zscaler.

6.  In het menu aan de bovenkant, klikt u op **beheer**.

    ![Beheer] (./media/active-directory-saas-zscaler-tutorial/IC769486.png "Beheer")

7.  Klik op **gebruikers beheren en verificatie**onder **beheerders beheren en rollen**.

    ![Beheerders & rollen beheren] (./media/active-directory-saas-zscaler-tutorial/IC769487.png "Beheerders & rollen beheren")

8.  Voer de volgende stappen uit in de sectie **Verificatie-optie kiezen voor uw organisatie** :

    ![Kies verificatieopties voor] (./media/active-directory-saas-zscaler-tutorial/IC769488.png "Kies verificatieopties voor")

    1.  Selecteer **verifiëren met SAML Single Sign-on**.
    2.  Klik op **SAML Single Sign-On-Parameters configureren**.

9.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Configureren SAML Single Sign-On Parameters** en klik op **Gereed**:

    ![Certificaat uploaden] (./media/active-directory-saas-zscaler-tutorial/IC769489.png "Certificaat uploaden")

    1.  Plak de waarde van het veld **verificatie aanvraag-URL** van de klassieke Azure portal in het tekstvak **URL van de SAML-Portal waarmee gebruikers worden verzonden voor verificatie** .
    2.  Typ in het tekstvak **kenmerk aanmeldingsnaam die** **NameID**.
    3.  In het veld **Uploaden openbare SSL-certificaat** , het certificaat dat u hebt gedownload van de klassieke Azure portal te uploaden.
    4.  Selecteer **Automatische SAML - ingericht**.

10. Voer de volgende stappen uit op de pagina van het dialoogvenster **Verificatie configureren** :

    ![Gebruikersverificatie configureren] (./media/active-directory-saas-zscaler-tutorial/IC769490.png "Gebruikersverificatie configureren")

    1.  Klik op **Opslaan**.
    2.  Klik op **nu activeren**.

11. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zscaler-tutorial/IC769491.png "Eenmalige aanmelding configureren")

##<a name="configuring-proxy-settings"></a>Proxy-instellingen configureren

###<a name="to-configure-the-proxy-settings-in-internet-explorer"></a>De proxy-instellingen configureren in Internet Explorer

1.  Start **Internet Explorer**.

2.  Selecteer **Internet-opties** in het menu **Extra** te openen van het dialoogvenster **Internet-opties** .

    ![Internet-opties] (./media/active-directory-saas-zscaler-tutorial/IC769492.png "Internet-opties")

3.  Klik op het tabblad **verbindingen** .

    ![Verbindingen] (./media/active-directory-saas-zscaler-tutorial/IC769493.png "Verbindingen")

4.  Klik op **LAN-instellingen** om het dialoogvenster **LAN-instellingen** te openen.

5.  Voer de volgende stappen uit in het gedeelte Proxy server:

    ![Proxyserver] (./media/active-directory-saas-zscaler-tutorial/IC769494.png "Proxyserver")

    1.  Selecteer een proxyserver voor het LAN-netwerk gebruiken.
    2.  Typ **gateway.zscalertwo.net**in het tekstvak adres.
    3.  Typ in het tekstvak poort **80**.
    4.  Selecteer **proxyserver niet gebruiken voor lokale adressen**.
    5.  Klik op **OK** om het dialoogvenster **Local Area Network (LAN)-instellingen** te sluiten.

6.  Klik op **OK** om het dialoogvenster **Internet-opties** te sluiten.

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij Zscaler Azure AD, moeten zij worden ingericht in Zscaler.  
Bij Zscaler is het inrichten van een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Log in op uw huurder **Zscaler** .

2.  Klik op **beheer**.

    ![Beheer] (./media/active-directory-saas-zscaler-tutorial/IC781035.png "Beheer")

3.  Klik op **Gebruikersbeheer**.

    ![Gebruikersbeheer] (./media/active-directory-saas-zscaler-tutorial/IC781036.png "Gebruikersbeheer")

4.  Klik op **toevoegen**op het tabblad **gebruikers** .

    ![Toevoegen] (./media/active-directory-saas-zscaler-tutorial/IC781037.png "Toevoegen")

5.  In de sectie gebruiker toevoegen kunt u de volgende stappen uitvoeren:

    ![Gebruiker toevoegen] (./media/active-directory-saas-zscaler-tutorial/IC781038.png "Gebruiker toevoegen")

    1.  Typ de **gebruikers-id**, **Weergegeven naam**, **wachtwoord**, **Bevestig het wachtwoord**en selecteer **groepen** en de **afdeling** van een geldige AAD-account die u verrichten wilt.
    2.  Klik op **Opslaan**.

>[AZURE.NOTE] Kunt u een andere Zscaler gebruiker account maken van gereedschap of API's geleverd door Zscaler bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-zscaler-perform-the-following-steps"></a>Gebruikers toewijzen aan Zscaler, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Zscaler** application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-zscaler-tutorial/IC769495.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-zscaler-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
