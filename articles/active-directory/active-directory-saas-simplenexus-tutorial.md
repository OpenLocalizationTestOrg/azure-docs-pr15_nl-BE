<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met SimpleNexus | Microsoft Azure" 
    description="Meer informatie over het SimpleNexus met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-simplenexus"></a>Zelfstudie: Azure Active Directory-integratie met SimpleNexus
  
Het doel van deze zelfstudie is de integratie van de Azure en SimpleNexus weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een SimpleNexus eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan SimpleNexus kan worden naar één teken in de toepassing van de SimpleNexus bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor SimpleNexus
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-simplenexus-tutorial/IC785893.png "Scenario")
##<a name="enabling-the-application-integration-for-simplenexus"></a>Inschakelen van de integratie van toepassingen voor SimpleNexus
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor SimpleNexus.

###<a name="to-enable-the-application-integration-for-simplenexus-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor SimpleNexus, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-simplenexus-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-simplenexus-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-simplenexus-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-simplenexus-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak**, **eenvoudige nexus**.

    ![Galerie van toepassing] (./media/active-directory-saas-simplenexus-tutorial/IC785894.png "Galerie van toepassing")

7.  **SimpleNexus**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Eenvoudige Nexus] (./media/active-directory-saas-simplenexus-tutorial/IC809578.png "Eenvoudige Nexus")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij SimpleNexus met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **SimpleNexus** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-simplenexus-tutorial/IC785896.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op SimpleNexus** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-simplenexus-tutorial/IC785897.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App URL configureren** , in het tekstvak **SimpleNexus teken In de URL** de URL van uw gebruik van het volgende patroon "*https://simplenexus.com/CompanyName\_login*", en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-simplenexus-tutorial/IC786904.png "App-URL configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij SimpleNexus** op **metagegevens downloaden**en doorsturen naar het bestand met metagegevens aan het ondersteuningsteam van SimpleNexus.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-simplenexus-tutorial/IC785899.png "Eenmalige aanmelding configureren")

    >[AZURE.NOTE] Eenmalige aanmelding moet worden ingeschakeld door het ondersteuningsteam van SimpleNexus.

5.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-simplenexus-tutorial/IC785900.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij SimpleNexus Azure AD, moeten zij worden ingericht in SimpleNexus.  
Bij SimpleNexus is het inrichten van een handmatige taak uitgevoerd door de beheerder van de huurder.

>[AZURE.NOTE] Kunt u een andere SimpleNexus gebruiker account maken van hulpprogramma's of API's geleverd door SimpleNexus bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-simplenexus-perform-the-following-steps"></a>Gebruikers toewijzen aan SimpleNexus, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **SimpleNexus **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-simplenexus-tutorial/IC785901.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-simplenexus-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.