<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Sprinklr | Microsoft Azure" 
    description="Meer informatie over het Sprinklr met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-sprinklr"></a>Zelfstudie: Azure Active Directory-integratie met Sprinklr
  
Het doel van deze zelfstudie is de integratie van de Azure en Sprinklr weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder Sprinklr
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Sprinklr kan worden naar één teken in de toepassing van de Sprinklr bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Sprinklr
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-sprinklr-tutorial/IC782900.png "Scenario")

##<a name="enabling-the-application-integration-for-sprinklr"></a>Inschakelen van de integratie van toepassingen voor Sprinklr
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Sprinklr.

###<a name="to-enable-the-application-integration-for-sprinklr-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Sprinklr, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-sprinklr-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-sprinklr-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-sprinklr-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-sprinklr-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **Sprinklr**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-sprinklr-tutorial/IC782901.png "Galerie van toepassing")

7.  **Sprinklr**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Sprinklr] (./media/active-directory-saas-sprinklr-tutorial/IC782902.png "Sprinklr")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Sprinklr met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat-bestand te maken.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Sprinklr** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sprinklr-tutorial/IC782903.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Sprinklr** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sprinklr-tutorial/IC782904.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App URL configureren** , in het tekstvak **Sprinklr teken In de URL** de URL van uw gebruik van het volgende patroon "*https://\<huurder naam\>. sprinklr.com*", en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-sprinklr-tutorial/IC782905.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op Sprinklr** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sprinklr-tutorial/IC782906.png "Eenmalige aanmelding configureren")

5.  Log in op uw site Sprinklr bedrijf als beheerder in een ander web browser-venster.

6.  Ga naar **beheer \> instellingen**.

    ![Beheer] (./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Beheer")

7.  Ga naar **beheren Partner \> eenmalige** op in het linkerdeelvenster.

    ![Partner beheren] (./media/active-directory-saas-sprinklr-tutorial/IC782908.png "Partner beheren")

8.  Klik op **+ eenmalige invoegtoepassingen**.

    ![Single Sign-ins] (./media/active-directory-saas-sprinklr-tutorial/IC782909.png "Single Sign-ins")

9.  Op de pagina met **Eenmalige aanmelding** kunt u de volgende stappen uitvoeren:

    ![Single Sign-ins] (./media/active-directory-saas-sprinklr-tutorial/IC782910.png "Single Sign-ins")

    1.  Typ in het tekstvak **naam** een naam voor de configuratie (bijvoorbeeld: *WAADSSOTest*).
    2.  Selecteer **ingeschakeld**.
    3.  Selecteer **Nieuwe SSO-certificaat gebruiken**.
    4.  Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.  

        >[AZURE.TIP] Zie voor meer informatie, [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    5.  De base-64 gecodeerde certificaat in Kladblok te openen, de inhoud ervan kopiëren naar het Klembord en plak deze vervolgens naar de textbox **Identity Provider certificaat**
    6.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Sprinklr** de waarde **Identity Provider-ID** kopiëren en vervolgens plakken in het tekstvak **Entiteits-Id** .
    7.  De waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in de textbox **Identity Provider aanmeldings-URL** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op Sprinklr** .
    8.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Sprinklr** de waarde van de **Externe Logout URL** kopiëren en vervolgens plakken in **Identity Provider Logout URL** textbox.
    9.  Selecteer als **Type SAML gebruiker-ID**, **bevestiging gebruiker bevat ' s sprinklr.com gebruikersnaam**.
    10. Selecteer **gebruikers-ID in het element-id de naam van het onderwerp overzicht is**als **Locatie voor de SAML-ID**.
    11. Sluit **Opslaan**.

        ![SAML] (./media/active-directory-saas-sprinklr-tutorial/IC782911.png "SAML")

10. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sprinklr-tutorial/IC782912.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
AAD gebruikers kunnen inloggen, als ze worden ingericht om toegang te krijgen in de toepassing Sprinklr.  
In deze sectie wordt beschreven hoe gebruikersaccounts AAD in Sprinklr te maken.

###<a name="to-provision-a-user-account-in-sprinklr-perform-the-following-steps"></a>Voor het inrichten van een gebruikersaccount in Sprinklr, moet u de volgende stappen uitvoeren:

1.  Log in op uw site Sprinklr bedrijf als beheerder.

2.  Ga naar **beheer \> instellingen**.

    ![Beheer] (./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Beheer")

3.  Ga naar **Client beheren \> gebruikers** in het linkerdeelvenster.

    ![Instellingen] (./media/active-directory-saas-sprinklr-tutorial/IC782914.png "Instellingen")

4.  Klik op **gebruiker toevoegen**.

    ![Instellingen] (./media/active-directory-saas-sprinklr-tutorial/IC782915.png "Instellingen")

5.  Voer de volgende stappen uit in het dialoogvenster **gebruiker bewerken** :

    ![Gebruiker bewerken] (./media/active-directory-saas-sprinklr-tutorial/IC782916.png "Gebruiker bewerken")

    1.  In het **e-mailadres**, **Voornaam** en **Achternaam** tekstvakken, type de gegevens van een Azure AD-account u wilt verrichten.
    2.  Selecteer **wachtwoord uitgeschakeld**.
    3.  Selecteer een **taal**.
    4.  Selecteer een **gebruikerstype**.
    5.  Klik op **bijwerken**.

    >[AZURE.IMPORTANT] **Wachtwoord uitgeschakeld** moet zijn ingeschakeld zodat de gebruiker zich aanmeldt via een id-provider.

6.  Ga naar de **rol**en voer de volgende stappen uit:

    ![Partner-rollen] (./media/active-directory-saas-sprinklr-tutorial/IC782917.png "Partner-rollen")

    1.  Selecteer in de lijst met **algemene** **alle\_machtigingen**.
    2.  Klik op **bijwerken**.

>[AZURE.NOTE] Kunt u een andere Sprinklr gebruiker account maken van hulpprogramma's of API's die door Sprinklr AD Azure gebruikersaccounts inrichten.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-sprinklr-perform-the-following-steps"></a>Gebruikers toewijzen aan Sprinklr, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Sprinklr **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-sprinklr-tutorial/IC782918.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-sprinklr-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.