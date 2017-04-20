<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Kontiki | Microsoft Azure" 
    description="Meer informatie over het Kontiki met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-kontiki"></a>Zelfstudie: Azure Active Directory-integratie met Kontiki
  
Het doel van deze zelfstudie is de integratie van de Azure en Kontiki weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een Kontiki eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, is de Azure AD-gebruikers die u hebt toegewezen aan Kontiki mogelijk voor één teken in de toepassing van de Kontiki bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor Kontiki inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-kontiki-tutorial/IC790235.png "Scenario")
##<a name="enabling-the-application-integration-for-kontiki"></a>De integratie van toepassingen voor Kontiki inschakelen
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Kontiki.

###<a name="to-enable-the-application-integration-for-kontiki-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Kontiki, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-kontiki-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-kontiki-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-kontiki-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-kontiki-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **Kontiki**.

    ![Galerie van toepassing] (./media/active-directory-saas-kontiki-tutorial/IC790236.png "Galerie van toepassing")

7.  **Kontiki**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Kontiki] (./media/active-directory-saas-kontiki-tutorial/IC790237.png "Kontiki")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Kontiki met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Kontiki** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Enkele aanmelding configureren] (./media/active-directory-saas-kontiki-tutorial/IC790238.png "Enkele aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aanmelden bij Kontiki** en klik op **volgende**.

    ![Enkele aanmelding configureren] (./media/active-directory-saas-kontiki-tutorial/IC790239.png "Enkele aanmelding configureren")

3.  Typ op de pagina **URL van App configureren** , in het tekstvak **Kontiki teken op URL** de URL die wordt gebruikt door de gebruikers aan te melden op Kontiki (bijvoorbeeld: "*https://company.mc.eval.kontiki.com/*"), en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-kontiki-tutorial/IC790240.png "App-URL configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Kontiki** op **metagegevens downloaden**en sla het bestand op uw computer met metagegevens.

    ![Enkele aanmelding configureren] (./media/active-directory-saas-kontiki-tutorial/IC790241.png "Enkele aanmelding configureren")

5.  De MetadataFile toe aan het ondersteuningsteam Kontiki verzenden.

    >[AZURE.NOTE] De configuratie voor eenmalige aanmelding moet worden uitgevoerd door het ondersteuningsteam Kontiki. U krijgt een melding zodra de configuratie is voltooid.

6.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Enkele aanmelding configureren] (./media/active-directory-saas-kontiki-tutorial/IC790242.png "Enkele aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Er is geen actie-item voor u voor het configureren van gebruikers op Kontiki.  
Wanneer een toegewezen gebruiker probeert aan te melden bij Kontiki met behulp van het paneel toegang, Kontiki wordt gecontroleerd of de gebruiker bestaat.  
Als er nog geen gebruikersaccount beschikbaar, wordt deze automatisch gemaakt door Kontiki.
##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-kontiki-perform-the-following-steps"></a>Gebruikers toewijzen aan Kontiki, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Kontiki **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-kontiki-tutorial/IC790243.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-kontiki-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.