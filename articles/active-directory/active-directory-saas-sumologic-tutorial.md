<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met SumoLogic | Microsoft Azure" 
    description="Meer informatie over het SumoLogic met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-sumologic"></a>Zelfstudie: Azure Active Directory-integratie met SumoLogic
  
Het doel van deze zelfstudie is de integratie van de Azure en SumoLogic weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder SumoLogic
  
Na het voltooien van deze zelfstudie bedrijf de Azure AD-gebruikers die u hebt toegewezen aan SumoLogicwill kunnen één teken worden in de toepassing op uw SumoLogic-site (service gestart aanmelden op) of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor SumoLogic
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-sumologic-tutorial/IC778549.png "Scenario")

##<a name="enabling-the-application-integration-for-sumologic"></a>Inschakelen van de integratie van toepassingen voor SumoLogic
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor SumoLogic.

###<a name="to-enable-the-application-integration-for-sumologic-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor SumoLogic, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-sumologic-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-sumologic-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-sumologic-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-sumologic-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **sumologic**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-sumologic-tutorial/IC778550.png "Galerie van toepassing")

7.  **SumoLogic**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![SumoLogic] (./media/active-directory-saas-sumologic-tutorial/IC778551.png "SumoLogic")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij SumoLogic met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat uploaden naar uw SumoLogictenant.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **SumoLogic** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sumologic-tutorial/IC778552.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op SumoLogic** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sumologic-tutorial/IC778553.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App URL configureren** , in het tekstvak **SumoLogic teken In de URL** de URL van uw volgende patroon met ' https://*\<-naam van de huurder\>. SumoLogic.com*", en klik op **volgende**.

    ![Aoo-URL configureren] (./media/active-directory-saas-sumologic-tutorial/IC778554.png "Aoo-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op SumoLogic** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sumologic-tutorial/IC778555.png "Eenmalige aanmelding configureren")

5.  Log in op uw site SumoLogic bedrijf als beheerder in een ander web browser-venster.

6.  Ga naar **beheren \> beveiliging**.

    ![Beheren] (./media/active-directory-saas-sumologic-tutorial/IC778556.png "Beheren")

7.  Klik op **SAML**.

    ![Globale instellingen] (./media/active-directory-saas-sumologic-tutorial/IC778557.png "Globale instellingen")

8.  In de lijst **Selecteer een configuratie of maak een nieuwe** **Azure AD**selecteren en klik vervolgens op **configureren**.

    ![Configureren van SAML 2.0] (./media/active-directory-saas-sumologic-tutorial/IC778558.png "Configureren van SAML 2.0")

9.  Voer de volgende stappen uit in het dialoogvenster **configureren SAML 2.0** :

    ![Configureren van SAML 2.0] (./media/active-directory-saas-sumologic-tutorial/IC778559.png "Configureren van SAML 2.0")

    1.  Typ in het tekstvak **Naam configuratie** **AD Azure**.
    2.  Selecteer de **Debug Mode**.
    3.  In de Azure klassieke portal op de dialoog pagina **configureren eenmalige aanmelding bij SumoLogic** de **Uitgever URL** -waarde kopiëren en vervolgens plakken in het tekstvak van de **uitgevende instelling** .
    4.  In de Azure klassieke portal op de dialoog pagina **configureren eenmalige aanmelding bij SumoLogic** de waarde **Verificatie verzoek-URL** kopiëren en vervolgens plakken in het tekstvak **Authn verzoek-URL** .
    5.  Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.  

        >[AZURE.TIP] Zie voor meer informatie, [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    6.  De base-64 gecodeerde certificaat in Kladblok te openen, de inhoud ervan kopiëren naar het Klembord en plak het gehele certificaat in textbox **X.509-certificaat** .
    7.  Als **E-kenmerk**, selecteer **Gebruik SAML-onderwerp**.
    8.  Selecteer **SP aanmelding-configuratie gestart**.
    9.  Typ in het tekstvak **Pad aanmelding** **Azure**.
    10. Klik op **Opslaan**.

10. In de Azure klassieke portal op de dialoog **configureren eenmalige aanmelding op SumoLogic** pagina selecteert u de Configuratiebevestiging van één aanmelding en klik op **Voltooien**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sumologic-tutorial/IC778560.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij SumoLogic Azure AD, moeten ze ingericht voor SumoLogic.  
Bij SumoLogic is het inrichten van een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw huurder **SumoLogic** .

2.  Ga naar **beheren \> gebruikers**.

    ![Gebruikers] (./media/active-directory-saas-sumologic-tutorial/IC778561.png "Gebruikers")

3.  Klik op **toevoegen**.

    ![Gebruikers] (./media/active-directory-saas-sumologic-tutorial/IC778562.png "Gebruikers")

4.  In het dialoogvenster **Nieuwe gebruiker** de volgende stappen uitvoeren:

    ![Nieuwe gebruiker] (./media/active-directory-saas-sumologic-tutorial/IC778563.png "Nieuwe gebruiker")

    1.  Typ de verwante gegevens van de gewenste voorziening in de tekstvakken voor **Voornaam**, **Achternaam** en **e-mailadres** Azure AD-account.
    2.  Selecteer een rol.
    3.  **Status**, selecteer **actief**.
    4.  Klik op **Opslaan**.

>[AZURE.NOTE] Kunt u een andere SumoLogic gebruiker account maken van hulpprogramma's of API's geleverd door SumoLogic bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-sumologic-perform-the-following-steps"></a>Gebruikers toewijzen aan SumoLogic, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **SumoLogic** application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-sumologic-tutorial/IC778564.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-sumologic-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.