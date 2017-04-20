<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Bamboe HR | Microsoft Azure" 
    description="Meer informatie over het HR Bamboe met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bamboo-hr"></a>Zelfstudie: Azure Active Directory-integratie met Bamboe HR

Het doel van deze zelfstudie is de integratie van de Azure en BambooHR weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een BambooHR eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan BambooHR kan worden naar één teken in de toepassing van de BambooHR bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor BambooHR
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-bamboo-hr-tutorial/IC796685.png "Scenario")
##<a name="enabling-the-application-integration-for-bamboohr"></a>Inschakelen van de integratie van toepassingen voor BambooHR

Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor BambooHR.

###<a name="to-enable-the-application-integration-for-bamboohr-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor BambooHR, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-bamboo-hr-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-bamboo-hr-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-bamboo-hr-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-bamboo-hr-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **BambooHR**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-bamboo-hr-tutorial/IC796686.png "Galerie van toepassing")

7.  **BambooHR**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![BambooHR] (./media/active-directory-saas-bamboo-hr-tutorial/IC796687.png "BambooHR")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij BambooHR met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat-bestand te maken.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **BambooHR** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Scenario] (./media/active-directory-saas-bamboo-hr-tutorial/IC796685.png "Scenario")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op BambooHR** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bamboo-hr-tutorial/IC796688.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App URL configureren** , in het tekstvak **BambooHR teken op URL** de URL die wordt gebruikt door de gebruikers aan te melden op uw toepassing BambooHR (bv.: https://company.bamboohr.com), en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-bamboo-hr-tutorial/IC796689.png "App-URL configureren")

4.  Klik op **certificaat downloaden**en sla het bestand op uw computer op de pagina **configureren eenmalige aanmelding op BambooHR** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bamboo-hr-tutorial/IC796690.png "Eenmalige aanmelding configureren")

5.  Log in op uw site BambooHR bedrijf als beheerder in een ander web browser-venster.

6.  Op de introductiepagina, moet u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding] (./media/active-directory-saas-bamboo-hr-tutorial/IC796691.png "Eenmalige aanmelding")

    1.  Klik op **Apps**.
    2.  Klik in het menu aan de linkerkant van apps, **Single Sign-On**.
    3.  Klik op **SAML Single Sign-On**.

7.  Voer de volgende stappen uit in de sectie **SAML Single Sign-On** :

    ![SAML Single Sign-On] (./media/active-directory-saas-bamboo-hr-tutorial/IC796692.png "SAML Single Sign-On")

    1.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij BambooHR** de **Single Sign-On Service URL** -waarde kopiëren en vervolgens plakken in de **SSO-aanmeldings-URL** textbox.
    2.  Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.  

        >[AZURE.TIP] Zie voor meer informatie, [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    3.  De base-64 gecodeerde certificaat in Kladblok te openen, de inhoud ervan kopiëren naar het Klembord en plak deze vervolgens naar de textbox **X.509-certificaat**
    4.  Klik op **Opslaan**.

8.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bamboo-hr-tutorial/IC796693.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Om gebruikers aan te melden bij BambooHR Azure AD, moeten zij worden ingericht in BambooHR.  
Bij BambooHR is het inrichten van een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw site **BambooHR** als beheerder.

2.  In de werkbalk op de bovenkant, klikt u op **Instellingen**.

    ![Instellen] (./media/active-directory-saas-bamboo-hr-tutorial/IC796694.png "Instellen")

3.  Klik op **Overzicht**.

4.  In het linkernavigatievenster, Ga naar **Security \> gebruikers**.

5.  Typ de gebruikersnaam naam, wachtwoord en e-mailadres van een geldige AAD account te verrichten in de verwante tekstvakken.

6.  Klik op **Opslaan**.

>[AZURE.NOTE] Kunt u een andere BambooHR gebruiker account maken van hulpprogramma's of API's geleverd door BambooHR bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-bamboohr-perform-the-following-steps"></a>Gebruikers toewijzen aan BambooHR, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **BambooHR **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-bamboo-hr-tutorial/IC796695.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-bamboo-hr-tutorial/IC767830.png "Ja")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
