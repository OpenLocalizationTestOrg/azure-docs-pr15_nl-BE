<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Zoho Mail | Microsoft Azure" 
    description="Meer informatie over het Zoho Mail met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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
    ms.date="09/09/2016" 
    ms.author="markvi" />

#<a name="tutorial-azure-active-directory-integration-with-zoho-mail"></a>Zelfstudie: Azure Active Directory-integratie met Zoho Mail
  
Het doel van deze zelfstudie is de integratie van de Azure en Zoho Mail weergegeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder Zoho Mail
  
Na het voltooien van deze zelfstudie, is de Azure AD-gebruikers die u hebt toegewezen aan de post van Zoho mogelijk voor één teken in de toepassing van de Zoho Mail bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor Zoho Mail inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-zoho-mail-tutorial/IC789600.png "Scenario")

##<a name="enabling-the-application-integration-for-zoho-mail"></a>De integratie van toepassingen voor Zoho Mail inschakelen
  
Het doel van deze sectie is een overzicht van de integratie van toepassingen voor Zoho Mail inschakelen.

###<a name="to-enable-the-application-integration-for-zoho-mail-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Zoho Mail, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zoho-mail-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-zoho-mail-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-zoho-mail-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-zoho-mail-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **Zoho Mail**.

    ![Galerie van toepassing] (./media/active-directory-saas-zoho-mail-tutorial/IC789601.png "Galerie van toepassing")

7.  **Zoho Mail**selecteert in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Zoho Mail] (./media/active-directory-saas-zoho-mail-tutorial/IC789602.png "Zoho Mail")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Zoho Mail met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat-bestand te maken.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Zoho Mail** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zoho-mail-tutorial/IC789603.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aanmelden bij Zoho Mail** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zoho-mail-tutorial/IC789604.png "Eenmalige aanmelding configureren")

3.  Op de pagina **URL van App configureren** , moet u de volgende stappen uitvoeren:

    ![App-URL configureren] (./media/active-directory-saas-zoho-mail-tutorial/IC789605.png "App-URL configureren")

    een. Typ in het tekstvak **Zoho Mail aanmelden op URL** de URL van uw gebruik van het volgende patroon:`http://<company name>.ZohoMail.com`

    b. Klik op **volgende**.


4.  Klik op **certificaat downloaden**en sla het bestand op uw computer op de pagina **configureren eenmalige aanmelding bij Zoho Mail** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zoho-mail-tutorial/IC789606.png "Eenmalige aanmelding configureren")

5.  Log in op uw site Zoho Mail bedrijf als beheerder in een ander web browser-venster.

6.  Ga naar het **Configuratiescherm**.

    ![Het Configuratiescherm] (./media/active-directory-saas-zoho-mail-tutorial/IC789607.png "Het Configuratiescherm")

7.  Klik op het tabblad **Verificatie SAML** .

    ![SAML-verificatie] (./media/active-directory-saas-zoho-mail-tutorial/IC789608.png "SAML-verificatie")

8.  In de sectie **Details van SAML-verificatie** , moet u de volgende stappen uitvoeren:

    ![Details over certificaatverificatie SAML] (./media/active-directory-saas-zoho-mail-tutorial/IC789609.png "Details over certificaatverificatie SAML")

    1.  De waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het tekstvak **Aanmeldings-URL** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Zoho Mail** .
    2.  In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding bij Zoho Mail** de waarde van de **Externe Logout URL** kopiëren en vervolgens plakken in het tekstvak **URL Logout** .
    3.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Zoho Mail** de waarde **Wijzigen wachtwoord URL** kopiëren en vervolgens plakken in het tekstvak **URL van wachtwoord wijzigen** .
    4.  Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.  

        >[AZURE.TIP] Zie voor meer informatie, [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    5.  De base-64 gecodeerde certificaat openen in Kladblok, de inhoud ervan kopiëren naar het Klembord en plak deze vervolgens naar het tekstvak **PublicKey** .
    6.  **RSA**- **algoritme**, selecteren.
    7.  Klik op **OK**.

9.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zoho-mail-tutorial/IC789610.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij Zoho Mail Azure AD, moeten zij worden ingericht in Zoho Mail.  
Zoho Mail is inrichten een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder uw bedrijf **Zoho Mail** site.

2.  Ga naar **het Configuratiescherm \> e & Docs**.

3.  Ga naar **Gebruikerdetails \> gebruiker toevoegen**.

    ![Gebruiker toevoegen] (./media/active-directory-saas-zoho-mail-tutorial/IC789611.png "Gebruiker toevoegen")

4.  In het dialoogvenster **gebruikers toevoegen** , voert u de volgende stappen uit:

    ![Gebruiker toevoegen] (./media/active-directory-saas-zoho-mail-tutorial/IC789612.png "Gebruiker toevoegen")

    1.  Typ de **Voornaam**, **Achternaam**, **E-ID**, het **wachtwoord** van een geldige Azure Active Directory-account verrichten in de verwante tekstvakken te.
    2.  Klik op **OK**.  

        >[AZURE.NOTE] De accounthouder Azure Active Directory ontvangt een e-mail met een koppeling naar de account bevestigen voordat deze actief wordt.

>[AZURE.NOTE] Kunt u een andere Zoho E-mail gebruiker account hulpmiddelen voor het maken of API's geleverd door Zoho Mail bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-zoho-mail-perform-the-following-steps"></a>Gebruikers toewijzen aan Zoho Mail, moet u de volgende stappen uitvoeren:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Zoho Mail **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-zoho-mail-tutorial/IC789613.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-zoho-mail-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.