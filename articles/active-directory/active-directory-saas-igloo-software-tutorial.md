<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Igloo Software | Microsoft Azure" 
    description="Meer informatie over het Igloo-Software met Azure Active Directory gebruiken om te schakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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
    ms.date="10/20/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-igloo-software"></a>Zelfstudie: Azure Active Directory-integratie met Igloo-Software
  
Het doel van deze zelfstudie is de integratie van de Azure en Igloo Software weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een [Igloo Software](http://www.igloosoftware.com/) -eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Igloo Software kan worden met één teken in de toepassing van de Igloo Software bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor Software Igloo inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-igloo-software-tutorial/IC783961.png "Scenario")
##<a name="enabling-the-application-integration-for-igloo-software"></a>De integratie van toepassingen voor Software Igloo inschakelen
  
Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor Igloo Software overzicht.

###<a name="to-enable-the-application-integration-for-igloo-software-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Igloo Software, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-igloo-software-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-igloo-software-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-igloo-software-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-igloo-software-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **Igloo Software**.

    ![Galerie van toepassing] (./media/active-directory-saas-igloo-software-tutorial/IC783962.png "Galerie van toepassing")

7.  In het resultatendeelvenster **Igloo Software**te selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Igloo] (./media/active-directory-saas-igloo-software-tutorial/IC783963.png "Igloo")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Igloo Software met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat uploaden naar uw bureaublad centrale huurder.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Igloo Software** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-igloo-software-tutorial/IC783964.png "Eenmalige aanmelding configureren")

2.  Op de pagina **Hoe wilt u dat gebruikers aanmelden bij Igloo Software** , selecteer **Microsoft Azure AD Single Sign-On**en klik op **volgende**.

    ![Microsoft Azure AD Single Sign-On] (./media/active-directory-saas-igloo-software-tutorial/IC783965.png "Microsoft Azure AD Single Sign-On")

3.  Typ de URL met het volgende patroon '*https://company.igloocommunities.com/?signin*' op de pagina **URL van App configureren** in de **Igloo Software Sign In URL** textbox, en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-igloo-software-tutorial/IC773625.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding bij Igloo Software** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-igloo-software-tutorial/IC783966.png "Eenmalige aanmelding configureren")

5.  Log in op uw site Igloo Software bedrijf als beheerder in een ander web browser-venster.

6.  Ga naar het **Configuratiescherm**.

    ![Het Configuratiescherm] (./media/active-directory-saas-igloo-software-tutorial/IC799949.png "Het Configuratiescherm")

7.  Klik op **Sign In instellingen**op het tabblad **lidmaatschap** .

    ![Aanmelden instellingen] (./media/active-directory-saas-igloo-software-tutorial/IC783968.png "Aanmelden instellingen")

8.  Klik in de sectie configuratie van SAML **SAML-verificatie configureren**.

    ![SAML-configuratie] (./media/active-directory-saas-igloo-software-tutorial/IC783969.png "SAML-configuratie")

9.  Voer de volgende stappen uit in de sectie **Algemene configuratie** :

    ![Algemene configuratie] (./media/active-directory-saas-igloo-software-tutorial/IC783970.png "Algemene configuratie")

    1.  Typ in het tekstvak **Naam van de verbinding** een aangepaste naam voor uw configuratie.
    2.  De waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het tekstvak **IdP aanmeldings-URL** in de Azure klassieke portal op de dialoog pagina **configureren eenmalige aanmelding bij Igloo Software** .
    3.  In de Azure klassieke portal op de dialoog pagina **configureren eenmalige aanmelding bij Igloo Software** de **Externe Logout URL** -waarde kopiëren en vervolgens plakken in het tekstvak **URL van IdP Logout** .
    4.  Selecteer **boeken**als **antwoord Logout en Type HTTP-aanvragen**.
    5.  Maak een tekstbestand van de gedownloade certificaat.
        
        >[AZURE.TIP]Zie voor meer informatie, [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    6.  Verwijderen van de eerste regel en de laatste regel van de tekstversie van het certificaat, de resterende certificaat tekst kopiëren en vervolgens plakken in het tekstvak **Een certificaat met openbare** .

10. In de **respons en de configuratie van de verificatie**, moet u de volgende stappen uitvoeren:

    ![Reactie en verificatie configureren] (./media/active-directory-saas-igloo-software-tutorial/IC783971.png "Reactie en verificatie configureren")

    1.  Als **Identiteitsprovider**, selecteer **Microsoft AD FS**.
    2.  Selecteer als **Type id**, **E-mailadres**.
    3.  Typ in het tekstvak **E-kenmerk** **emailaddress**.
    4.  Typ in het tekstvak **Voornaam kenmerk** **givenname**.
    5.  Typ in het tekstvak **Kenmerk laatste naam** **Achternaam**.

11. Uitvoeren van de volgende stappen uit om de configuratie te voltooien:

    ![Maken van een gebruikersaccount op inloggen] (./media/active-directory-saas-igloo-software-tutorial/IC783972.png "Maken van een gebruikersaccount op inloggen")

    1.  Selecteer **een nieuwe gebruiker op uw site wanneer ze zich aanmelden**als van het **maken van een gebruikersaccount op inloggen**.
    2.  **Aanmelden instellingen**, schakel **Gebruik SAML-knop op het scherm 'Aanmelden'**.
    3.  Klik op **Opslaan**.

12. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-igloo-software-tutorial/IC783973.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Er is geen actie-item voor u gebruikersaanvragen Igloo software configureren.  
Wanneer een toegewezen gebruiker probeert aan te melden bij Igloo-Software met behulp van het paneel toegang, Igloo Software wordt gecontroleerd of de gebruiker bestaat.  
Als er nog geen gebruikersaccount beschikbaar, wordt deze automatisch gemaakt door Igloo Software.
##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-igloo-software-perform-the-following-steps"></a>Gebruikers toewijzen aan Igloo Software, moet u de volgende stappen uitvoeren:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op de pagina **Igloo Software **application integration **gebruikers toewijzen**.

    ![Gebruikers toewijzen] (./media/active-directory-saas-igloo-software-tutorial/IC783974.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-igloo-software-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.