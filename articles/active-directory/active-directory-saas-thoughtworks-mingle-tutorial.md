<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Thoughtworks Singleplayer | Microsoft Azure" 
    description="Informatie over het Thoughtworks Singleplayer gebruik met Azure Active Directory inschakelen voor eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Zelfstudie: Azure Active Directory-integratie met Thoughtworks Singleplayer
  
Het doel van deze zelfstudie is de integratie van de Azure en Thoughtworks Singleplayer weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder Singleplayer Thoughtworks
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor Thoughtworks Singleplayer inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785150.png "Scenario")

##<a name="enabling-the-application-integration-for-thoughtworks-mingle"></a>De integratie van toepassingen voor Thoughtworks Singleplayer inschakelen
  
Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor Thoughtworks Singleplayer overzicht.

###<a name="to-enable-the-application-integration-for-thoughtworks-mingle-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Thoughtworks Singleplayer, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **thoughtworks Singleplayer**.

    ![Galerie van toepassing] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785151.png "Galerie van toepassing")

7.  In het resultatendeelvenster **Thoughtworks Singleplayer**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Thoughtworks Singleplayer] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785152.png "Thoughtworks Singleplayer")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij de Singleplayer Thoughtworks met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
U moet een certificaat uploaden naar Thoughtworks Singleplayer als onderdeel van deze procedure.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, op de pagina toepassing integratie **Thoughtworks Singleplayer ** **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785153.png "Eenmalige aanmelding configureren")

2.  Op de pagina **Hoe wilt u dat gebruikers aan te melden op Thoughtworks Singleplayer** **AD Azure Microsoft Single Sign-On**selecteren en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785154.png "Eenmalige aanmelding configureren")

3.  Typ de URL met het volgende patroon '*http://company.mingle.thoughtworks.com*' op de pagina in het tekstvak **URL van Thoughtworks huurder Singleplayer** **App URL configureren** en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785155.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding bij Thoughtworks Singleplayer** downloaden metagegevens op en sla het op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785156.png "Eenmalige aanmelding configureren")

5.  Meld u als beheerder uw bedrijf **Thoughtworks Singleplayer** site.

6.  Klik op het tabblad **beheer** en klik op **Eenmalige aanmelding-configuratie**.

    ![SSO-configuratie] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785157.png "SSO-configuratie")

7.  In de sectie **SSO-configuratie** kunt u de volgende stappen uitvoeren:

    ![SSO-configuratie] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785158.png "SSO-configuratie")

    1.  De als metagegevensbestand wilt uploaden, klikt u op **bestand kiezen**.
    2.  Klik op **wijzigingen opslaan**.

8.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785159.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
AAD gebruikers kunnen inloggen, als ze worden ingericht naar de toepassing Thoughtworks Singleplayer met hun naam Azure Active Directory.  
Thoughtworks Singleplayer is inrichten een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder uw bedrijf Thoughtworks Singleplayer site.

2.  Klik op **profiel**.

    ![Het eerste Project] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785160.png "Het eerste Project")

3.  Klik op het tabblad **beheer** en klik vervolgens op **gebruikers**.

    ![Gebruikers] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785161.png "Gebruikers")

4.  Klik op **nieuwe gebruiker**.

    ![Nieuwe gebruiker] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785162.png "Nieuwe gebruiker")

5.  Klik op de pagina van het dialoogvenster **Nieuwe gebruiker** de volgende stappen uitvoeren:

    ![Nieuwe gebruiker] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785163.png "Nieuwe gebruiker")

    1.  Typ de **aanmeldingsnaam**, **weergavenaam**, **Kies wachtwoord**, **Bevestig het wachtwoord** van een geldige AAD account te verrichten in de verwante tekstvakken.
    2.  Als **het type gebruiker**, **volledige gebruiker**te selecteren.
    3.  Klik op **Dit profiel maakt**.

>[AZURE.NOTE] Kunt u andere Thoughtworks Singleplayer gebruiker account maken van hulpprogramma's of API's geleverd door Thoughtworks Singleplayer bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-thoughtworks-mingle-perform-the-following-steps"></a>Gebruikers toewijzen aan Thoughtworks Singleplayer, kunt u de volgende stappen uitvoeren:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op de pagina application integration **Singleplayer Thoughtworks** **gebruikers toewijzen**.

    ![Gebruikers toewijzen] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785164.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
