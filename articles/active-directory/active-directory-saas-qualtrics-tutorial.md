<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Qualtrics | Microsoft Azure" 
    description="Meer informatie over het Qualtrics met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-qualtrics"></a>Zelfstudie: Azure Active Directory-integratie met Qualtrics
  
Het doel van deze zelfstudie is de integratie van de Azure en Qualtrics weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een Qualtrics eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Qualtrics kan worden naar één teken in de toepassing van de Qualtrics bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Qualtrics
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-qualtrics-tutorial/IC789542.png "Scenario")
##<a name="enabling-the-application-integration-for-qualtrics"></a>Inschakelen van de integratie van toepassingen voor Qualtrics
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Qualtrics.

###<a name="to-enable-the-application-integration-for-qualtrics-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Qualtrics, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-qualtrics-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-qualtrics-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-qualtrics-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-qualtrics-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **Qualtrics**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-qualtrics-tutorial/IC789543.png "Galerie van toepassing")

7.  **Qualtrics**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Qualtrics] (./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Qualtrics met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Qualtrics** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-qualtrics-tutorial/IC789545.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Qualtrics** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-qualtrics-tutorial/IC789546.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App URL configureren** , in het tekstvak **Qualtrics teken op URL** de URL (bijvoorbeeld: "*https://ssotest2ut1.qualtrics.com*"), en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-qualtrics-tutorial/IC789547.png "App-URL configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Qualtrics** op **metagegevens downloaden**en sla het bestand op uw computer met metagegevens.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-qualtrics-tutorial/IC789548.png "Eenmalige aanmelding configureren")

5.  De MetadataFile toe aan het ondersteuningsteam Qualtrics verzenden.

    >[AZURE.NOTE]De configuratie voor eenmalige aanmelding heeft door het ondersteuningsteam van Qualtrics moet worden uitgevoerd. U krijgt een melding zodra de configuratie is voltooid.

6.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-qualtrics-tutorial/IC789549.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Er is geen actie-item aan de Qualtrics van gebruikers configureren.  
Wanneer een toegewezen gebruiker probeert aan te melden bij de Qualtrics met het deelvenster toegang, Qualtrics wordt gecontroleerd of de gebruiker bestaat.  
Als er nog geen gebruikersaccount beschikbaar, wordt deze automatisch gemaakt door de Qualtrics.
##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-qualtrics-perform-the-following-steps"></a>Gebruikers toewijzen aan Qualtrics, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Qualtrics **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-qualtrics-tutorial/IC789550.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-qualtrics-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.