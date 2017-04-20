<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met twee Zscaler | Microsoft Azure" 
    description="Informatie over het gebruik van twee Zscaler met Azure Active Directory inschakelen voor eenmalige aanmelding, geautomatiseerde provisioning en meer!." 
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

#<a name="tutorial-azure-active-directory-integration-with-zscaler-two"></a>Zelfstudie: Azure Active Directory-integratie met twee Zscaler

Het doel van deze zelfstudie is de integratie van de Azure en twee ZScaler weergegeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een ZScaler twee eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, zullen de Azure AD-gebruikers die u hebt toegewezen aan twee ZScaler kunnen eenmalige in de toepassing op de twee ZScaler bedrijf-site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor twee ZScaler inschakelen
2.  Eenmalige aanmelding configureren
3.  Proxy-instellingen configureren
4.  Gebruikersaanvragen configureren
5.  Gebruikers toewijzen

![Eenmalige aanmelding configureren] (./media/active-directory-saas-zscaler-two-tutorial/IC800199.png "Eenmalige aanmelding configureren")

##<a name="enabling-the-application-integration-for-zscaler-two"></a>De integratie van toepassingen voor twee ZScaler inschakelen
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor twee ZScaler.

###<a name="to-enable-the-application-integration-for-zscaler-two-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor twee ZScaler, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zscaler-two-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-zscaler-two-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-zscaler-two-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-zscaler-two-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Klik in het **zoekvak**typt u **Twee ZScaler**.

    ![Galerie van toepassing] (./media/active-directory-saas-zscaler-two-tutorial/IC800200.png "Galerie van toepassing")

7.  **Twee ZScaler**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Twee ZScaler] (./media/active-directory-saas-zscaler-two-tutorial/IC800201.png "Twee ZScaler")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij twee ZScaler met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat uploaden naar de huurder van twee ZScaler.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, op de pagina **ZScaler twee** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zscaler-two-tutorial/IC800202.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op twee ZScaler** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zscaler-two-tutorial/IC800203.png "Eenmalige aanmelding configureren")

3.  Typ de URL die wordt gebruikt door uw gebruikers om aanmelding bij de toepassing van de twee ZScaler op de pagina **Configureren App-URL** in het tekstvak **ZScaler twee teken op URL** en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-zscaler-two-tutorial/IC800204.png "App-URL configureren")

    >[AZURE.NOTE] Krijgt u de werkelijke waarde voor uw omgeving van het ondersteuningsteam van twee ZScaler als u deze nodig hebt.

4.  Op de pagina **configureren eenmalige aanmelding op twee ZScaler** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zscaler-two-tutorial/IC800205.png "Eenmalige aanmelding configureren")

5.  Log in op uw site met twee ZScaler bedrijf als beheerder in een ander web browser-venster.

6.  In het menu aan de bovenkant, klikt u op **beheer**.

    ![Beheer] (./media/active-directory-saas-zscaler-two-tutorial/IC800206.png "Beheer")

7.  Klik op **gebruikers beheren en verificatie**onder **beheerders beheren en rollen**.

    ![Gebruikers & verificatie beheren] (./media/active-directory-saas-zscaler-two-tutorial/IC800207.png "Gebruikers & verificatie beheren")

8.  Voer de volgende stappen uit in de sectie **Verificatie-opties kiezen voor uw organisatie** :

    ![Verificatie] (./media/active-directory-saas-zscaler-two-tutorial/IC800208.png "Verificatie")

    1.  Selecteer **verifiëren met SAML Single Sign-On**.
    2.  Klik op **SAML Single Sign-On-Parameters configureren**.

9.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Configureren SAML Single Sign-On Parameters** en klik op **Gereed**:

    ![Eenmalige aanmelding] (./media/active-directory-saas-zscaler-two-tutorial/IC800209.png "Eenmalige aanmelding")

    1.  In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding op twee ZScaler** de waarde **Verificatie verzoek-URL** kopiëren en vervolgens plakken in het tekstvak **URL van de SAML-Portal waarmee gebruikers worden verzonden voor verificatie** .
    2.  Typ in het tekstvak **kenmerk aanmeldingsnaam die** **NameID**.
    3.  Als u wilt uw gedownloade certificaat uploaden, klikt u op **Zscaler pem**.
    4.  Selecteer **Automatische SAML - ingericht**.

10. Voer de volgende stappen uit op de pagina van het dialoogvenster **Verificatie configureren** :

    ![Beheer] (./media/active-directory-saas-zscaler-two-tutorial/IC800210.png "Beheer")

    1.  Klik op **Opslaan**.
    2.  Klik op **nu activeren**.

11. In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op twee ZScaler** selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zscaler-two-tutorial/IC800211.png "Eenmalige aanmelding configureren")

##<a name="configuring-proxy-settings"></a>Proxy-instellingen configureren

###<a name="to-configure-the-proxy-settings-in-internet-explorer"></a>De proxy-instellingen configureren in Internet Explorer

1.  Start **Internet Explorer**.

2.  Selecteer **Internet-opties** in het menu **Extra** te openen van het dialoogvenster **Internet-opties** .

    ![Internet-opties] (./media/active-directory-saas-zscaler-two-tutorial/IC769492.png "Internet-opties")

3.  Klik op het tabblad **verbindingen** .

    ![Verbindingen] (./media/active-directory-saas-zscaler-two-tutorial/IC769493.png "Verbindingen")

4.  Klik op **LAN-instellingen** om het dialoogvenster **LAN-instellingen** te openen.

5.  Voer de volgende stappen uit in het gedeelte Proxy server:

    ![Proxyserver] (./media/active-directory-saas-zscaler-two-tutorial/IC769494.png "Proxyserver")

    1.  Selecteer een proxyserver voor het LAN-netwerk gebruiken.
    2.  Typ **gateway.zscalerone.net**in het tekstvak adres.
    3.  Typ in het tekstvak poort **80**.
    4.  Selecteer **proxyserver niet gebruiken voor lokale adressen**.
    5.  Klik op **OK** om het dialoogvenster **Local Area Network (LAN)-instellingen** te sluiten.

6.  Klik op **OK** om het dialoogvenster **Internet-opties** te sluiten.

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij ZScaler twee Azure AD, moeten zij worden ingericht met twee ZScaler.  
Twee ZScaler is het inrichten een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Log in op uw huurder **Zscaler** .

2.  Klik op **beheer**.

    ![Beheer] (./media/active-directory-saas-zscaler-two-tutorial/IC781035.png "Beheer")

3.  Klik op **Gebruikersbeheer**.

    ![Toevoegen] (./media/active-directory-saas-zscaler-two-tutorial/IC781037.png "Toevoegen")

4.  Klik op **toevoegen**op het tabblad **gebruikers** .

    ![Toevoegen] (./media/active-directory-saas-zscaler-two-tutorial/IC781037.png "Toevoegen")

5.  In de sectie gebruiker toevoegen kunt u de volgende stappen uitvoeren:

    ![Gebruiker toevoegen] (./media/active-directory-saas-zscaler-two-tutorial/IC781038.png "Gebruiker toevoegen")

    1.  Typ de **gebruikers-id**, **Weergegeven naam**, **wachtwoord**, **Bevestig het wachtwoord**en selecteer **groepen** en de **afdeling** van een geldige AAD-account die u verrichten wilt.
    2.  Klik op **Opslaan**.

>[AZURE.NOTE] Kunt u de andere twee ZScaler gebruiker account hulpmiddelen voor het maken of API's geleverd door twee ZScaler bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-zscaler-two-perform-the-following-steps"></a>Gebruikers toewijzen aan twee ZScaler, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **ZScaler twee** application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-zscaler-two-tutorial/IC800212.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-zscaler-two-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.