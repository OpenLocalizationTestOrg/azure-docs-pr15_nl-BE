<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Cisco Webex | Microsoft Azure" 
    description="Meer informatie over het Cisco Webex met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Zelfstudie: Azure Active Directory-integratie met Cisco Webex

Het doel van deze zelfstudie is de integratie van de Azure en Cisco Webex weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder Cisco Webex

Na het voltooien van deze zelfstudie, is de Azure AD-gebruikers die u hebt toegewezen aan de Cisco Webex mogelijk voor één teken in de toepassing van de Cisco Webex bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor Cisco Webex inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "Scenario")
##<a name="enabling-the-application-integration-for-cisco-webex"></a>De integratie van toepassingen voor Cisco Webex inschakelen

Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Cisco Webex.

###<a name="to-enable-the-application-integration-for-cisco-webex-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Cisco Webex, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak**de **Cisco Webex**.

    ![Galerie van toepassing] (./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "Galerie van toepassing")

7.  **Cisco Webex**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Cisco Webex] (./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Cisco Webex met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u vereist voor het maken van een base-64 gecodeerde certificaat.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Cisco Webex** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "Eenmalige aanmelding configureren")

2.  Op de pagina **Hoe wilt u dat gebruikers aanmelden bij Cisco Webex** Selecteer **AD Azure Microsoft Single Sign-On**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "Eenmalige aanmelding configureren")

3.  Voer de volgende stappen uit op de pagina **URL van App configureren** en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "App-URL configureren")

    1.  Typ in het tekstvak **URL Zing mee op** de URL van uw Cisco Webex huurder (bijvoorbeeld: *http://contoso.webex.com*).
    2.  Typ in het tekstvak **URL van Cisco Webex beantwoorden** uw **Cisco Webex AssertionConsumerService URL** (bijvoorbeeld: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company*).

4.  Op de pagina **configureren eenmalige aanmelding bij Cisco Webex** downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "Eenmalige aanmelding configureren")

5.  Log in op uw site Cisco Webex bedrijf als beheerder in een ander web browser-venster.

6.  Klik in het menu aan de bovenkant, **Sitebeheer**.

    ![Sitebeheer] (./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "Sitebeheer")

7.  Klik in de sectie **Site beheren** **SSO-configuratie**.

    ![SSO-configuratie] (./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "SSO-configuratie")

8.  In de sectie federatieve Web-SSO-configuratie kunt u de volgende stappen uitvoeren:

    ![Federatieve eenmalige aanmelding-configuratie] (./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "Federatieve eenmalige aanmelding-configuratie")

    1.  Selecteer in de lijst met **Federation-Protocol** **SAML 2.0**.
    2.  Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.  

        >[AZURE.TIP] Zie voor meer informatie, [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    3.  De base-64 gecodeerde certificaat openen in Kladblok en kopieer de inhoud van deze.
    4.  Klik op **SAML-metagegevens importeren**en plak de base-64 gecodeerde certificaat.
    5.  Kopieer de waarde van de **URL van de uitgevende instelling** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Cisco Webex** en plak deze in het tekstvak **verlener voor SAML (IdP-ID)** .
    6.  De waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het tekstvak **URL van klant SSO Login** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Cisco Webex** .
    7.  Selecteer in de lijst **Indeling NameID** , **e-mailadres**.
    8.  Typ in het tekstvak **AuthnContextClassRef** **Urn: oasis: namen: tc: SAML:2.0:ac:classes:Password**.
    9.  In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding bij Cisco Webex** de waarde van de **Externe Logout URL** kopiëren en vervolgens plakken in het tekstvak **URL van Logout SSO-klant** .
    10. Klik op **bijwerken**.

9.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Cisco Webex** selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Om gebruikers aan te melden bij Cisco Webex Azure AD, moeten zij worden ingericht in Cisco Webex.  
Cisco Webex is inrichten een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw huurder **Cisco Webex** .

2.  Ga naar **voor het beheren van gebruikers \> gebruiker toevoegen**.

    ![Gebruikers toevoegen] (./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "Gebruikers toevoegen")

3.  In de sectie gebruiker toevoegen kunt u de volgende stappen uitvoeren:

    ![Gebruiker toevoegen] (./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "Gebruiker toevoegen")

    1.  Selecteer als **Type Account**, **Host**.
    2.  Typ de gegevens van een bestaande Azure AD-gebruiker in de volgende tekstvakken: **Voornaam, achternaam**, **gebruikersnaam**, **e-mailadres**, **wachtwoord**, **Bevestig het wachtwoord**.
    3.  Klik op **toevoegen**.

>[AZURE.NOTE] U kunt een andere Cisco Webex gebruiker account hulpmiddelen voor het maken of API's geleverd door Cisco Webex aan bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-cisco-webex-perform-the-following-steps"></a>Gebruikers toewijzen aan Cisco Webex, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op de Integratiepagina van **Cisco Webex **toepassing op **gebruikers toewijzen**.

    ![Gebruikers toewijzen] (./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Ja")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
