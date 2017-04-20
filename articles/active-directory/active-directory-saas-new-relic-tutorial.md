<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met nieuwe Relic | Microsoft Azure" 
    description="Meer informatie over het nieuwe Relic met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-new-relic"></a>Zelfstudie: Azure Active Directory-integratie met nieuwe Relic
  
Het doel van deze zelfstudie is voor het instellen van eenmalige aanmelding tussen Azure Active Directory en nieuwe Relic weergeven.
  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een nieuwe Relic eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, worden de Azure Active Directory-gebruikers die u hebt toegewezen aan de nieuwe Relic kunnen eenmalige aanmelding via het deelvenster AAD toegang.

1.  De integratie van toepassingen voor nieuwe Relic inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-new-relic-tutorial/IC797030.png "Scenario")
##<a name="enabling-the-application-integration-for-new-relic"></a>De integratie van toepassingen voor nieuwe Relic inschakelen
  
Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor nieuwe Relic overzicht.

###<a name="to-enable-the-application-integration-for-new-relic-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor nieuwe Relic, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-new-relic-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-new-relic-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-new-relic-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-new-relic-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak**, **Nieuwe Relic**.

    ![Galerie van toepassing] (./media/active-directory-saas-new-relic-tutorial/IC797031.png "Galerie van toepassing")

7.  **Nieuwe Relic**selecteren in het resultatendeelvenster en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Nieuwe Relic] (./media/active-directory-saas-new-relic-tutorial/IC797032.png "Nieuwe Relic")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
In deze sectie vindt u een overzicht van gebruikers worden geverifieerd met hun account in Azure Active Directory, op basis van het protocol SAML federation met nieuwe Relic inschakelen.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Nieuwe Relic** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-new-relic-tutorial/IC769534.png "Eenmalige aanmelding configureren")

2.  Op de pagina **Hoe wilt u dat gebruikers aanmelden bij nieuwe Relic** **AD Azure Microsoft Single Sign-On**selecteren en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-new-relic-tutorial/IC797033.png "Eenmalige aanmelding configureren")

3.  Typ de URL die wordt gebruikt door de gebruikers aan te melden op de toepassing van nieuwe Relic op de pagina **Configureren App-URL** in het tekstvak **Nieuwe Relic teken op URL** en klik op **volgende**. 

    De app-URL is de URL van uw nieuwe Relic huurder (bijvoorbeeld: *https://rpm.newrelic.com*):

    ![App-URL configureren] (./media/active-directory-saas-new-relic-tutorial/IC797034.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op nieuwe Relic** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-new-relic-tutorial/IC797035.png "Eenmalige aanmelding configureren")

5.  In een ander web browser-venster aanmelden bij uw bedrijf **Relic nieuwe** site als beheerder.

6.  In het menu aan de bovenkant, klikt u op **Accountinstellingen**.

    ![Accountinstellingen] (./media/active-directory-saas-new-relic-tutorial/IC797036.png "Accountinstellingen")

7.  Klik op het tabblad **beveiliging en -verificatie** en klik vervolgens op het tabblad voor **eenmalige aanmelding** .

    ![Eenmalige aanmelding] (./media/active-directory-saas-new-relic-tutorial/IC797037.png "Eenmalige aanmelding")

8.  Op de pagina van het dialoogvenster SAML voert u de volgende stappen uit:

    ![SAML] (./media/active-directory-saas-new-relic-tutorial/IC797038.png "SAML")

    1.  Klik op **Bestand kiezen** om te uploaden van uw gedownloade Azure Active Directory-certificaat.
    2.  De waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het tekstvak **URL voor externe aanmelding** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op nieuwe Relic** .
    3.  In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding op nieuwe Relic** de waarde van de **Externe Logout URL** kopiëren en vervolgens plakken in het tekstvak **URL landingspagina Logout** .
    4.  Klik op **wijzigingen opslaan**.

9.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-new-relic-tutorial/IC797039.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij een nieuwe Relic Azure Active Directory, moeten zij worden ingericht in nieuwe Relic.  
Nieuwe Relic is inrichten een handmatige taak.

###<a name="to-provision-a-user-account-to-new-relic-perform-the-following-steps"></a>Om een gebruikersaccount aan nieuwe Relic inrichten, kunt u de volgende stappen uitvoeren:

1.  Meld u als beheerder uw bedrijf **Relic nieuwe** site.

2.  In het menu aan de bovenkant, klikt u op **Accountinstellingen**.

    ![Accountinstellingen] (./media/active-directory-saas-new-relic-tutorial/IC797040.png "Accountinstellingen")

3.  Klik op **Overzicht**in het venster **Account** aan de linkerkant en klik op **gebruiker toevoegen**.

    ![Accountinstellingen] (./media/active-directory-saas-new-relic-tutorial/IC797041.png "Accountinstellingen")

4.  Voer de volgende stappen uit in het dialoogvenster **actieve gebruikers** :

    ![Actieve gebruikers] (./media/active-directory-saas-new-relic-tutorial/IC797042.png "Actieve gebruikers")

    1.  Typ in het tekstvak **e-mailadres** het e-mailadres van een geldige Azure Active Directory-gebruiker die u verrichten wilt.
    2.  Als de **rol van** de **gebruiker**te selecteren.
    3.  Klik op **deze gebruiker toevoegen**.

>[AZURE.NOTE]Kunt u een andere nieuwe Relic gebruiker account hulpmiddelen voor het maken of API's die nieuwe Relic bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-new-relic-perform-the-following-steps"></a>Gebruikers toewijzen aan nieuwe Relic, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Nieuwe Relic** application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-new-relic-tutorial/IC797043.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-new-relic-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.




