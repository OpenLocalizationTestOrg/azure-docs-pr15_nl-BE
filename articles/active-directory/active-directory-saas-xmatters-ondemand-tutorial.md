<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met xMatters OnDemand | Microsoft Azure"
    description="Meer informatie over het xMatters OnDemand met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a>Zelfstudie: Azure Active Directory-integratie met xMatters OnDemand
  
Het doel van deze zelfstudie is de integratie van de Azure OnDemand xMatters weergeven. Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder xMatters OnDemand
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan xMatters OnDemand kan worden met één teken in de toepassing op uw xMatters OnDemand bedrijf site (service gestart aanmelden op) of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor xMatters OnDemand inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776788.png "Scenario")

##<a name="enabling-the-application-integration-for-xmatters-ondemand"></a>De integratie van toepassingen voor xMatters OnDemand inschakelen
  
Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor xMatters OnDemand overzicht.

###<a name="to-enable-the-application-integration-for-xmatters-ondemand-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor xMatters OnDemand, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **xMatters OnDemand**.

    ![Galerie van toepassing] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776789.png "Galerie van toepassing")

7.  In het resultatendeelvenster **XMatters OnDemand**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![xMatters OnDemand] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776790.png "xMatters OnDemand")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij XMatters OnDemand met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, op de pagina **XMatters OnDemand** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776791.png "Eenmalige aanmelding configureren")

2.  Op de pagina **Hoe wilt u dat gebruikers aan te melden op XMatters OnDemand** **AD Azure Microsoft Single Sign-On**selecteren en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776792.png "Eenmalige aanmelding configureren")

3.  Op de pagina **URL van App configureren** , moet u de volgende stappen uitvoeren:

    ![App-URL configureren] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776793.png "App-URL configureren")

    een. Typ in het tekstvak **XMatters OnDemand Sign In URL** uw URL met behulp van het volgende patroon:`https://<tenant-name>.XMattersOnDemandapp.com`

    b. Klik op **volgende**.


4.  Op de pagina **configureren eenmalige aanmelding bij XMatters OnDemand** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal als **c:\\XMatters OnDemand.cer**.

    >[AZURE.IMPORTANT] U moet het certificaat naar het ondersteuningsteam xMatters doorsturen. Het certificaat moet worden geüpload door het ondersteuningsteam xMatters voordat u de configuratie voor één kunt voltooien.

    ![Aanmelden voor één configureren] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776794.png "Aanmelden voor één configureren")

5.  Log in op uw site XMatters OnDemand bedrijf als beheerder in een ander web browser-venster.

6.  In de werkbalk op de bovenkant, klikt u op **beheer**en klik op **Bedrijfsgegevens** in de navigatiebalk aan de linkerkant.

    ![Admin] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "Admin")

7.  Op de pagina **Configuratie van SAML** , de volgende stappen uitvoeren:

    ![SAML-configuratie] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "SAML-configuratie")

    1.  Selecteer **SAML inschakelen**.
    2.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij XMatters OnDemand** de waarde **Identity Provider-ID** kopiëren en vervolgens plakken in het tekstvak **Id Provider-ID** .
    3.  In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding bij XMatters OnDemand** de **Single Sign-On Service URL** -waarde kopiëren en vervolgens plakken in het tekstvak met **Één teken op URL** .
    4.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij XMatters OnDemand** de **URL van de Service Single Sign-Out** waarde kopiëren en vervolgens plakken in het tekstvak **URL van één Logout** .
    5.  Klik op de pagina Bedrijfsgegevens aan de bovenkant, klikt u op **Wijzigingen opslaan**.
        ![Bedrijfsdetails] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "Bedrijfsdetails")

8.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Aanmelden voor één configureren] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776798.png "Aanmelden voor één configureren")

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij XMatters OnDemand Azure AD, moeten zij worden ingericht in XMatters OnDemand.  
Bij XMatters OnDemand is het inrichten van een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw huurder **XMatters OnDemand** .

2.  Klik op het tabblad **gebruikers** .

3.  Klik op **gebruiker toevoegen**.

    ![Gebruikers] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "Gebruikers")

4.  Selecteer **actief**.

5.  In de sectie **gebruiker toevoegen** kunt u de volgende stappen uitvoeren:

    ![Een gebruiker toevoegen] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "Een gebruiker toevoegen")

    1.  Voer de **gebruikersnaam**, **Voornaam**, **Achternaam**, **Site** van een geldige AAD-account die u verrichten wilt.
    2.  Klik op **Opslaan**.

>[AZURE.NOTE] Kunt u een andere XMatters OnDemand gebruiker account hulpmiddelen voor het maken of API's geleverd door XMatters OnDemand bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-xmatters-ondemand-perform-the-following-steps"></a>Gebruikers toewijzen aan XMatters OnDemand, moet u de volgende stappen uitvoeren:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **XMatters OnDemand **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776799.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.