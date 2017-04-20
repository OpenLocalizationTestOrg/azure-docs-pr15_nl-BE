<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Lucidchart | Microsoft Azure" 
    description="Meer informatie over het Lucidchart met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-lucidchart"></a>Zelfstudie: Azure Active Directory-integratie met Lucidchart
  
Het doel van deze zelfstudie is de integratie van de Azure en Lucidchart weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een Lucidchart eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Lucidchart kan worden naar één teken in de toepassing van de Lucidchart bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Lucidchart
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-lucidchart-tutorial/IC791183.png "Scenario")
##<a name="enabling-the-application-integration-for-lucidchart"></a>Inschakelen van de integratie van toepassingen voor Lucidchart
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Lucidchart.

###<a name="to-enable-the-application-integration-for-lucidchart-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Lucidchart, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-lucidchart-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-lucidchart-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-lucidchart-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-lucidchart-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **Lucidchart**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-lucidchart-tutorial/IC791184.png "Galerie van toepassing")

7.  **Lucidchart**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Lucidchart] (./media/active-directory-saas-lucidchart-tutorial/IC791185.png "Lucidchart")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Lucidchart met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Lucidchart** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-lucidchart-tutorial/IC791186.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Lucidchart** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-lucidchart-tutorial/IC791187.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **URL van App configureren** , in het tekstvak **Lucidchart teken op URL** de URL die wordt gebruikt door de gebruikers aan te melden op uw toepassing Lucidchart (bijvoorbeeld: "*https://chart2.office.lucidchart.com/saml/sso/azure*"), en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-lucidchart-tutorial/IC791188.png "App-URL configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Lucidchart** downloaden van de metagegevens op **metagegevens downloaden**en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-lucidchart-tutorial/IC791189.png "Eenmalige aanmelding configureren")

5.  Log in op uw site Lucidchart bedrijf als beheerder in een ander web browser-venster.

6.  Klik in het menu aan de bovenkant, **Team**.

    ![Team] (./media/active-directory-saas-lucidchart-tutorial/IC791190.png "Team")

7.  Klik op **toepassing \> SAML beheren**.

    ![SAML beheren] (./media/active-directory-saas-lucidchart-tutorial/IC791191.png "SAML beheren")

8.  Klik op de pagina van het dialoogvenster **SAML-verificatie-instellingen** kunt u de volgende stappen uitvoeren:

    1.  **SAML-verificatie inschakelen**selecteert en vervolgens klikt u op **optioneel**.
        ![SAML-verificatie-instellingen] (./media/active-directory-saas-lucidchart-tutorial/IC791192.png "SAML-verificatie-instellingen")
    2.  In het tekstvak **domein** , typt u uw domein en klik op **Certificaat wijzigen**.
        ![Certificaat wijzigen] (./media/active-directory-saas-lucidchart-tutorial/IC791193.png "Certificaat wijzigen")
    3.  Uw gedownloade metagegevensbestand openen, de inhoud kopiëren en vervolgens plakken in de textbox **Metagegevens uploaden** .
        ![Metagegevens uploaden] (./media/active-directory-saas-lucidchart-tutorial/IC791194.png "Metagegevens uploaden")
    4.  Selecteer **nieuwe gebruiker automatisch toegevoegd aan het team**en klik vervolgens op **wijzigingen opslaan**.
        ![Wijzigingen opslaan] (./media/active-directory-saas-lucidchart-tutorial/IC791195.png "Wijzigingen opslaan")

9.  Selecteer de Configuratiebevestiging van één aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-lucidchart-tutorial/IC791196.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Er is geen actie-item aan de Lucidchart van gebruikers configureren.  
Wanneer een toegewezen gebruiker probeert aan te melden bij de Lucidchart met het deelvenster toegang, Lucidchart wordt gecontroleerd of de gebruiker bestaat.  
Als er nog geen gebruikersaccount beschikbaar, wordt deze automatisch gemaakt door de Lucidchart.
##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-lucidchart-perform-the-following-steps"></a>Gebruikers toewijzen aan Lucidchart, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Lucidchart **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-lucidchart-tutorial/IC791197.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-lucidchart-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.