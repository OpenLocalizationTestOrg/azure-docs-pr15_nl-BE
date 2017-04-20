<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met een toegestane vertraging | Microsoft Azure" 
    description="Meer informatie over het toegestane vertraging met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-slack"></a>Zelfstudie: Azure Active Directory-integratie met een toegestane vertraging
  
Het doel van deze zelfstudie is de integratie van de Azure en toegestane vertraging weergegeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een toegestane eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan de toegestane vertraging kan worden met één teken in de toepassing van de toegestane bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor toegestane vertraging inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-slack-tutorial/IC794980.png "Scenario")

##<a name="enabling-the-application-integration-for-slack"></a>De integratie van toepassingen voor toegestane vertraging inschakelen
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor toegestane vertraging.

###<a name="to-enable-the-application-integration-for-slack-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor toegestane vertraging, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-slack-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-slack-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-slack-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-slack-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak**, **toegestane vertraging**.

    ![Galerie van toepassing] (./media/active-directory-saas-slack-tutorial/IC794981.png "Galerie van toepassing")

7.  **Vertraging**selecteert in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Scenario] (./media/active-directory-saas-slack-tutorial/IC796925.png "Scenario")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij vertraging met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat-bestand te maken.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, op de pagina **vertraging** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-slack-tutorial/IC794982.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u gebruikers aan te melden op vertraging** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-slack-tutorial/IC794983.png "Eenmalige aanmelding configureren")

3.  Op de pagina **URL van toepassing configureren** in het tekstvak **Vertraging teken In de URL** , typ de URL van uw huurder tekort (bv.: "*https://azuread.slack.com*"), en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-slack-tutorial/IC794984.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding bij vertraging** voor het downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-slack-tutorial/IC794985.png "Eenmalige aanmelding configureren")

5.  Log in op uw site toegestane bedrijf als beheerder in een ander web browser-venster.

6.  Ga naar **naar Microsoft Azure AD \> Team van instellingen voor**.

    ![Instellingen voor team] (./media/active-directory-saas-slack-tutorial/IC794986.png "Instellingen voor team")

7.  Klik op het tabblad **verificatie** in de sectie **Instellingen voor Team** en klik vervolgens op **Instellingen wijzigen**.

    ![Instellingen voor team] (./media/active-directory-saas-slack-tutorial/IC794987.png "Instellingen voor team")

8.  In het dialoogvenster **Instellingen voor SAML-verificatie** , moet u de volgende stappen uitvoeren:

    ![SAML-instellingen] (./media/active-directory-saas-slack-tutorial/IC794988.png "SAML-instellingen")

    1.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op de toegestane vertraging van** de waarde van **SAML SSO-URL** kopiëren en vervolgens plakken in het tekstvak **SAML 2.0 eindpunt (HTTP)** .
    2.  Kopieer de waarde van de **URL van de uitgevende instelling** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij vertraging** en plak deze in de textbox **Identity Provider uitgever** .
    3.  Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.
    
        >[AZURE.TIP] Zie voor meer informatie, [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    4.  De base-64 gecodeerde certificaat in Kladblok te openen, de inhoud ervan kopiëren naar het Klembord en vervolgens plakken naar **Een certificaat met openbare** textbox.
    5.  Hef de selectie van **de gebruikers toestaan om hun e-mailadres wijzigen**.
    6.  Selecteer **gebruikers mogen hun eigen gebruikersnaam kiezen**.
    7.  Schakel **verificatie voor uw team moet worden gebruikt door**, **is optioneel**.
    8.  Klik op **configuratie op te slaan**.

9.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-slack-tutorial/IC794989.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij vertraging Azure AD, moeten zij worden ingericht in de toegestane vertraging.
  
Er is geen actie-item voor u gebruikersaanvragen om vertraging te configureren.  
Wanneer een toegewezen gebruiker zich probeert aan te melden bij vertraging, wordt automatisch een toegestane account gemaakt indien nodig.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-slack-perform-the-following-steps"></a>Gebruikers toewijzen aan de toegestane vertraging, kunt u de volgende stappen uitvoeren:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  De **toegestane vertraging **Integratiepagina, klik op **gebruikers toewijzen**.

    ![Gebruikers toewijzen] (./media/active-directory-saas-slack-tutorial/IC794990.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-slack-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.