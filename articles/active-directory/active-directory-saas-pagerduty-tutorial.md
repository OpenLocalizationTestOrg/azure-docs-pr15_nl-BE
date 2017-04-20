<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Pagerduty | Microsoft Azure" 
    description="Meer informatie over het Pagerduty met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-pagerduty"></a>Zelfstudie: Azure Active Directory-integratie met Pagerduty
  
Het doel van deze zelfstudie is de integratie van de Azure en Pagerduty weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder Pagerduty
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Pagerduty kan worden naar één teken in de toepassing van de Pagerduty bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Pagerduty
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-pagerduty-tutorial/IC778528.png "Scenario")
##<a name="enabling-the-application-integration-for-pagerduty"></a>Inschakelen van de integratie van toepassingen voor Pagerduty
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Pagerduty.

###<a name="to-enable-the-application-integration-for-pagerduty-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Pagerduty, kunt u de volgende stappen uitvoeren:

1.  Klik in de Portal Azure Management in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-pagerduty-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-pagerduty-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-pagerduty-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-pagerduty-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **Pagerduty**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-pagerduty-tutorial/IC778529.png "Galerie van toepassing")

7.  **Pagerduty**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![PagerDuty] (./media/active-directory-saas-pagerduty-tutorial/IC778530.png "PagerDuty")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Pagerduty met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat-bestand te maken.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Pagerduty** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-pagerduty-tutorial/IC778531.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Pagerduty** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-pagerduty-tutorial/IC778532.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App URL configureren** , in het tekstvak **Pagerduty teken In de URL** de URL van uw volgende patroon met ' https://*\<-naam van de huurder\>. Pagerduty.com*", en klik op **volgende**.

    ![App-url configureren] (./media/active-directory-saas-pagerduty-tutorial/IC778533.png "App-url configureren")

4.  Klik op **certificaat downloaden**en sla het bestand op uw computer op de pagina **configureren eenmalige aanmelding op Pagerduty** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-pagerduty-tutorial/IC778534.png "Eenmalige aanmelding configureren")

5.  Log in op uw site Pagerduty bedrijf als beheerder in een ander web browser-venster.

6.  In het menu aan de bovenkant, klikt u op **Accountinstellingen**.

    ![Accountinstellingen] (./media/active-directory-saas-pagerduty-tutorial/IC778535.png "Accountinstellingen")

7.  Klik op **eenmalige aanmelding**.

    ![Eenmalige aanmelding] (./media/active-directory-saas-pagerduty-tutorial/IC778536.png "Eenmalige aanmelding")

8.  Voer de volgende stappen uit op de pagina inschakelen Single Sign-on (SSO):

    ![Eenmalige aanmelding inschakelen] (./media/active-directory-saas-pagerduty-tutorial/IC778537.png "Eenmalige aanmelding inschakelen")

    1.  Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.  

        >[AZURE.TIP] Zie voor meer informatie, [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    2.  De base-64 gecodeerde certificaat in Kladblok te openen, de inhoud ervan kopiëren naar het Klembord en plak deze vervolgens naar de textbox **X.509-certificaat**
    3.  In de Azure klassieke portal op de dialoog pagina **configureren eenmalige aanmelding bij Pagerduty** de waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het tekstvak **Aanmeldings-URL** .
    4.  In de Azure klassieke portal op de dialoog pagina **configureren eenmalige aanmelding bij Pagerduty** de waarde van de **Externe Logout URL** kopiëren en vervolgens plakken in het tekstvak **URL Logout** .
    5.  Selecteer **eenmalige aanmelding inschakelen**.
    6.  Klik op **wijzigingen opslaan**.

9.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-pagerduty-tutorial/IC778538.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij Pagerduty Azure AD, moeten zij worden ingericht in Pagerduty.  
Bij Pagerduty is het inrichten van een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw huurder **Pagerduty** .

2.  In het menu aan de bovenkant, klikt u op **gebruikers**.

3.  Klik op **gebruikers toevoegen**.

    ![Gebruikers toevoegen] (./media/active-directory-saas-pagerduty-tutorial/IC778539.png "Gebruikers toevoegen")

4.  Typ in het dialoogvenster **uitnodigen uw team** de **eerste en laatste naam** en het **e-** mailadres van de gebruiker die u wilt inrichten, klikt u op **toevoegen**en klik vervolgens op **Verzenden uitnodigt**voor Azure AD.

    ![Uw team uitnodigen] (./media/active-directory-saas-pagerduty-tutorial/IC778540.png "Uw team uitnodigen")

    >[AZURE.NOTE] Alle toegevoegde gebruikers ontvangen een uitnodiging voor het maken van een account PagerDuty.

>[AZURE.NOTE] Kunt u een andere Pagerduty gebruiker account maken van hulpprogramma's of API's geleverd door Pagerduty bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-pagerduty-perform-the-following-steps"></a>Gebruikers toewijzen aan Pagerduty, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Pagerduty **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-pagerduty-tutorial/IC778541.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-pagerduty-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.