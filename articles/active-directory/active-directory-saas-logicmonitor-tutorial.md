<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met LogicMonitor | Microsoft Azure" 
    description="Meer informatie over het LogicMonitor met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-logicmonitor"></a>Zelfstudie: Azure Active Directory-integratie met LogicMonitor
  
Het doel van deze zelfstudie is de integratie van de Azure en LogicMonitor weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder LogicMonitor
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor LogicMonitor
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-logicmonitor-tutorial/IC790045.png "Scenario")
##<a name="enabling-the-application-integration-for-logicmonitor"></a>Inschakelen van de integratie van toepassingen voor LogicMonitor
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor LogicMonitor.

###<a name="to-enable-the-application-integration-for-logicmonitor-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor LogicMonitor, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-logicmonitor-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-logicmonitor-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-logicmonitor-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-logicmonitor-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **logicmonitor**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-logicmonitor-tutorial/IC790046.png "Galerie van toepassing")

7.  **LogicMonitor**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![LogicMonitor] (./media/active-directory-saas-logicmonitor-tutorial/IC790047.png "LogicMonitor")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij LogicMonitor met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **LogicMonitor **application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-logicmonitor-tutorial/IC790048.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op LogicMonitor** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-logicmonitor-tutorial/IC790049.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App URL configureren** , in het tekstvak **Teken op URL** de URL die wordt gebruikt door de gebruikers aan te melden op LogicMonitor \(e, g,: "*http://company.logicmonitor.com*"\), en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-logicmonitor-tutorial/IC790050.png "App-URL configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij LogicMonitor** op **metagegevens downloaden**en sla het op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-logicmonitor-tutorial/IC790051.png "Eenmalige aanmelding configureren")

5.  Log in op uw site **LogicMonitor** bedrijf als beheerder.

6.  In het menu aan de bovenkant, klikt u op **Instellingen**.

    ![Instellingen] (./media/active-directory-saas-logicmonitor-tutorial/IC790052.png "Instellingen")

7.  Klik in het bat navigatie aan de linkerkant, **Single Sign On**

    ![Eenmalige aanmelding] (./media/active-directory-saas-logicmonitor-tutorial/IC790053.png "Eenmalige aanmelding")

8.  Voer de volgende stappen uit in de sectie **Instellingen voor eenmalige aanmelding (SSO)** :

    ![Instellingen voor eenmalige aanmelding] (./media/active-directory-saas-logicmonitor-tutorial/IC790054.png "Instellingen voor eenmalige aanmelding")

    1.  Selecteer **eenmalige aanmelding inschakelen**.
    2.  Selecteer als **Standaard roltoewijzing**, **alleen-lezen**.
    3.  De van het gedownloade metagegevensbestand openen in Kladblok en plak de inhoud van het bestand in de textbox **Identity Provider metagegevens** .
    4.  Klik op **wijzigingen opslaan**.

9.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-logicmonitor-tutorial/IC790055.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
AAD gebruikers kunnen inloggen, als ze worden ingericht voor de toepassing van de LogicMonitor met hun naam Azure Active Directory.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Log in op uw site LogicMonitor bedrijf als beheerder.

2.  In het menu aan de bovenkant, klikt u op **Instellingen**en klik vervolgens op **rollen en gebruikers**.

    ![Rollen en gebruikers] (./media/active-directory-saas-logicmonitor-tutorial/IC790056.png "Rollen en gebruikers")

3.  Klik op **toevoegen**.

4.  Voer de volgende stappen uit in de sectie **een account toevoegen** :

    ![Een account toevoegen] (./media/active-directory-saas-logicmonitor-tutorial/IC790057.png "Een account toevoegen")

    1.  Typ de **gebruikersnaam**, **e-mailadres**, **wachtwoord** en **wachtwoord opnieuw** de waarden van de gewenste voorziening in de verwante tekstvakken Azure Active Directory-gebruiker.
    2.  Selecteer de **rollen**, **machtigingen weergeven** en de **Status**.
    3.  Klik op **indienen**.

>[AZURE.NOTE]U kunt andere LogicMonitor gebruiker account hulpmiddelen voor het maken of API's geleverd door LogicMonitor bepaling Azure Active Directory-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-logicmonitor-perform-the-following-steps"></a>Gebruikers toewijzen aan LogicMonitor, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **LogicMonitor** application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-logicmonitor-tutorial/IC790058.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-logicmonitor-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.




