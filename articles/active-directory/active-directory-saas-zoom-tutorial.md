<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Zoom | Microsoft Azure" 
    description="Informatie over het in-/ uitzoomen met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!." 
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

#<a name="tutorial-azure-active-directory-integration-with-zoom"></a>Zelfstudie: Azure Active Directory-integratie met in-/ uitzoomen
  
Het doel van deze zelfstudie is de integratie van de Azure en zoomen weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder in-/ uitzoomen
  
Na het voltooien van deze zelfstudie, zal de Azure AD-gebruikers die u hebt toegewezen aan de Zoom kunnen eenmalige in de toepassing van de in-/ uitzoomen bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor in-/ uitzoomen inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-zoom-tutorial/IC784693.png "Scenario")

##<a name="enabling-the-application-integration-for-zoom"></a>De integratie van toepassingen voor in-/ uitzoomen inschakelen
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor in-/ uitzoomen.

###<a name="to-enable-the-application-integration-for-zoom-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor in-/ uitzoomen, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zoom-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-zoom-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-zoom-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-zoom-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **in-en uitzoomen**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-zoom-tutorial/IC784694.png "Galerie van toepassing")

7.  Selecteer **Zoomen**in het resultatendeelvenster en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![In-en uitzoomen] (./media/active-directory-saas-zoom-tutorial/IC784695.png "In-en uitzoomen")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij zoomen met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat-bestand te maken.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen in de Azure klassieke portal op de pagina toepassing integratie **in-en uitzoomen** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zoom-tutorial/IC784696.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u gebruikers aan te melden op in-/ uitzoomen** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zoom-tutorial/IC784697.png "Eenmalige aanmelding configureren")

3.  Typ de URL met het volgende patroon '*http://company.zoom.us*' op de pagina **URL van App configureren** in de **Sign in-en uitzoomen In URL** textbox, en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-zoom-tutorial/IC784698.png "App-URL configureren")

4.  Klik op **certificaat downloaden**en sla het bestand op uw computer op de pagina **configureren eenmalige aanmelding bij in-/ uitzoomen** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zoom-tutorial/IC784699.png "Eenmalige aanmelding configureren")

5.  Log in op uw site in-/ uitzoomen bedrijf als beheerder in een ander web browser-venster.

6.  Klik op het tabblad **Single Sign-On** .

    ![Eenmalige aanmelding] (./media/active-directory-saas-zoom-tutorial/IC784700.png "Eenmalige aanmelding")

7.  Klik op het tabblad **Beveiliging** en gaat u naar de instellingen voor **Eenmalige aanmelding** .

8.  In de sectie op de volgende stappen uitvoeren:

    ![Eenmalige aanmelding] (./media/active-directory-saas-zoom-tutorial/IC784701.png "Eenmalige aanmelding")

    1.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op Zoom** **Single Sign-On Service URL** -waarde kopiëren en vervolgens plakken in het tekstvak **URL - in pagina** .
    2.  Kopieer de **URL van de Service Single Sign-Out** waarde in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op zoomen** en vervolgens plakken in het tekstvak **URL van afmelden** .
    3.  Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.  

        >[AZURE.TIP] Zie voor meer informatie, [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    4.  De base-64 gecodeerde certificaat in Kladblok te openen, de inhoud ervan kopiëren naar het Klembord en plak deze vervolgens naar de **provider van het certificaat identiteit** textbox
    5.  Kopieer de waarde van de **URL van de uitgevende instelling** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op zoomen** en vervolgens plakken in het tekstvak van de **uitgevende instelling** .
    6.  Klik op **Opslaan**.

9.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zoom-tutorial/IC784702.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij zoomen Azure AD, moeten ze worden ingericht in in-/ uitzoomen.  
Zoomen is inrichten een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder uw bedrijf **in-en uitzoomen** site.

2.  Klik op het tabblad **Account Management** en klik op **Gebruikersbeheer**.

3.  Klik op **gebruikers toevoegen**in de sectie beheer van de gebruiker.

    ![Gebruikersbeheer] (./media/active-directory-saas-zoom-tutorial/IC784703.png "Gebruikersbeheer")

4.  Voer de volgende stappen uit op de pagina **gebruikers toevoegen** :

    ![Gebruikers toevoegen] (./media/active-directory-saas-zoom-tutorial/IC784704.png "Gebruikers toevoegen")

    1.  Als **Het Type gebruiker**, selecteert u **Basic**.
    2.  Typ in het tekstvak **e-mailberichten** , het e-mailadres van een geldige AAD-account die u verrichten wilt.
    3.  Klik op **toevoegen**.

>[AZURE.NOTE] U kunt een andere gebruiker account maken zoomgereedschappen of API's die worden geleverd door in-/ uitzoomen naar AAD de voorziening gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-zoom-perform-the-following-steps"></a>Gebruikers om aan te wijzen in-/ uitzoomen, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina toepassing integratie **in-en uitzoomen **.

    ![Gebruikers toewijzen] (./media/active-directory-saas-zoom-tutorial/IC784705.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-zoom-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.