<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Zscaler Beta | Microsoft Azure" 
    description="Meer informatie over het Zscaler Bèta met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-zscaler-beta"></a>Zelfstudie: Azure Active Directory-integratie met Zscaler Beta
  
Het doel van deze zelfstudie is de integratie van de Azure en ZScaler bèta weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een ZScaler Beta eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan ZScaler Beta kan worden met één teken in de toepassing van de ZScaler bèta bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor ZScaler Beta inschakelen
2.  Eenmalige aanmelding configureren
3.  Proxy-instellingen configureren
4.  Gebruikersaanvragen configureren
5.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-zscaler-beta-tutorial/IC800223.png "Scenario")

##<a name="enabling-the-application-integration-for-zscaler-beta"></a>De integratie van toepassingen voor ZScaler Beta inschakelen
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor ZScaler Beta.

###<a name="to-enable-the-application-integration-for-zscaler-beta-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor ZScaler Beta, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zscaler-beta-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-zscaler-beta-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-zscaler-beta-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-zscaler-beta-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **ZScaler Beta**.

    ![Galerie van toepassing] (./media/active-directory-saas-zscaler-beta-tutorial/IC800224.png "Galerie van toepassing")

7.  **ZScaler Beta**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Een ZScaler] (./media/active-directory-saas-zscaler-beta-tutorial/IC800216.png "Een ZScaler")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij ZScaler Beta met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat uploaden naar uw huurder ZScaler Beta.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, op de pagina **ZScaler Beta** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zscaler-beta-tutorial/IC800225.png "Eenmalige aanmelding configureren")

2.  Op de pagina **Hoe wilt u dat gebruikers aan te melden op ZScaler Beta** **Microsoft Azure AD Single Sign-On**selecteren en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zscaler-beta-tutorial/IC800226.png "Eenmalige aanmelding configureren")

3.  Typ de URL die wordt gebruikt door gebruikers voor aanmelding bij uw toepassing ZScaler Beta op de pagina **Configureren App-URL** in het tekstvak **ZScaler bèta teken op URL** en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-zscaler-beta-tutorial/IC800227.png "App-URL configureren")

    >[AZURE.NOTE] Krijgt u de werkelijke waarde voor uw omgeving van uw team ZScaler Beta ondersteuning als u deze nodig hebt.

4.  Op de pagina **configureren eenmalige aanmelding op ZScaler Beta** downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zscaler-beta-tutorial/IC800228.png "Eenmalige aanmelding configureren")

5.  Log in op uw bedrijf ZScaler Beta site als beheerder in een ander web browser-venster.

6.  In het menu aan de bovenkant, klikt u op **beheer**.

    ![Beheer] (./media/active-directory-saas-zscaler-beta-tutorial/IC800206.png "Beheer")

7.  Klik op **gebruikers beheren en verificatie**onder **beheerders beheren en rollen**.

    ![Gebruikers & verificatie beheren] (./media/active-directory-saas-zscaler-beta-tutorial/IC800207.png "Gebruikers & verificatie beheren")

8.  Voer de volgende stappen uit in de sectie **Verificatie-opties kiezen voor uw organisatie** :

    ![Verificatie] (./media/active-directory-saas-zscaler-beta-tutorial/IC800208.png "Verificatie")

    1.  Selecteer **verifiëren met SAML Single Sign-On**.
    2.  Klik op **SAML Single Sign-On-Parameters configureren**.

9.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Configureren SAML Single Sign-On Parameters** en klik op **Gereed**:

    ![Eenmalige aanmelding] (./media/active-directory-saas-zscaler-beta-tutorial/IC800209.png "Eenmalige aanmelding")

    1.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij ZScaler Beta** de waarde **Verificatie verzoek-URL** kopiëren en vervolgens plakken in het tekstvak **URL van de SAML-Portal waarmee gebruikers worden verzonden voor verificatie** .
    2.  Typ in het tekstvak **kenmerk aanmeldingsnaam die** **NameID**.
    3.  Als u wilt uw gedownloade certificaat uploaden, klikt u op **Zscaler pem**.
    4.  Selecteer **Automatische SAML - ingericht**.

10. Voer de volgende stappen uit op de pagina van het dialoogvenster **Verificatie configureren** :

    ![Beheer] (./media/active-directory-saas-zscaler-beta-tutorial/IC800210.png "Beheer")

    1.  Klik op **Opslaan**.
    2.  Klik op **nu activeren**.

11. In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op ZScaler Beta** selecteert u de Configuratiebevestiging van één aanmelding en klik op **Voltooien**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zscaler-beta-tutorial/IC800229.png "Eenmalige aanmelding configureren")

##<a name="configuring-proxy-settings"></a>Proxy-instellingen configureren

###<a name="to-configure-the-proxy-settings-in-internet-explorer"></a>De proxy-instellingen configureren in Internet Explorer

1.  Start **Internet Explorer**.

2.  Selecteer **Internet-opties** in het menu **Extra** te openen van het dialoogvenster **Internet-opties** .

    ![Internet-opties] (./media/active-directory-saas-zscaler-beta-tutorial/IC769492.png "Internet-opties")

3.  Klik op het tabblad **verbindingen** .

    ![Verbindingen] (./media/active-directory-saas-zscaler-beta-tutorial/IC769493.png "Verbindingen")

4.  Klik op **LAN-instellingen** om het dialoogvenster **LAN-instellingen** te openen.

5.  Voer de volgende stappen uit in het gedeelte Proxy server:

    ![Proxyserver] (./media/active-directory-saas-zscaler-beta-tutorial/IC769494.png "Proxyserver")

    1.  Selecteer een proxyserver voor het LAN-netwerk gebruiken.
    2.  Typ **gateway.zscalerBeta.net**in het tekstvak adres.
    3.  Typ in het tekstvak poort **80**.
    4.  Selecteer **proxyserver niet gebruiken voor lokale adressen**.
    5.  Klik op **OK** om het dialoogvenster **Local Area Network (LAN)-instellingen** te sluiten.

6.  Klik op **OK** om het dialoogvenster **Internet-opties** te sluiten.

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij ZScaler Beta Azure AD, moeten zij worden ingericht naar ZScaler Beta.  
ZScaler Beta is inrichten een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Log in op uw huurder **Zscaler** .

2.  Klik op **beheer**.

    ![Beheer] (./media/active-directory-saas-zscaler-beta-tutorial/IC781035.png "Beheer")

3.  Klik op **Gebruikersbeheer**.

    ![Toevoegen] (./media/active-directory-saas-zscaler-beta-tutorial/IC781037.png "Toevoegen")

4.  Klik op **toevoegen**op het tabblad **gebruikers** .

    ![Toevoegen] (./media/active-directory-saas-zscaler-beta-tutorial/IC781037.png "Toevoegen")

5.  In de sectie gebruiker toevoegen kunt u de volgende stappen uitvoeren:

    ![Gebruiker toevoegen] (./media/active-directory-saas-zscaler-beta-tutorial/IC781038.png "Gebruiker toevoegen")

    1.  Typ de **gebruikers-id**, **Weergegeven naam**, **wachtwoord**, **Bevestig het wachtwoord**en selecteer **groepen** en de **afdeling** van een geldige AAD-account die u verrichten wilt.
    2.  Klik op **Opslaan**.

>[AZURE.NOTE] Kunt u een andere ZScaler bèta gebruiker account hulpmiddelen voor het maken of API's geleverd door ZScaler Beta bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-zscaler-beta-perform-the-following-steps"></a>Gebruikers toewijzen aan ZScaler Beta, moet u de volgende stappen uitvoeren:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **ZScaler Beta** application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-zscaler-beta-tutorial/IC800230.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-zscaler-beta-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.