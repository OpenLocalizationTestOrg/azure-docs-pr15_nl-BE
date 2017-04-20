<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met de levenscyclus van SCC | Microsoft Azure" 
    description="Meer informatie over het SCC levenscyclus met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a>Zelfstudie: Azure Active Directory-integratie met SCC LifeCycle
  
Het doel van deze zelfstudie is de integratie van de Azure en SCC LifeCycle weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een SCC LifeCycle eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, is de Azure AD-gebruikers die u hebt toegewezen aan de levenscyclus van SCC mogelijk voor één teken in de toepassing van de levenscyclus van SCC bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor SCC levenscyclus inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "Scenario")
##<a name="enabling-the-application-integration-for-scc-lifecycle"></a>De integratie van toepassingen voor SCC levenscyclus inschakelen
  
Het doel van deze sectie wordt aan de contour van het inschakelen van de integratie van toepassingen voor SCC-levenscyclus.

###<a name="to-enable-the-application-integration-for-scc-lifecycle-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor SCC levenscyclus, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak**, **SCC LifeCycle**.

    ![Galerie van toepassing] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "Galerie van toepassing")

7.  **SCC LifeCycle**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![SCC LifeCycle] (./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC LifeCycle")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij SCC levenscyclus met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, op de pagina **SCC LifeCycle** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "Eenmalige aanmelding configureren")

2.  Op de pagina **Hoe wilt u dat gebruikers aanmelden bij SCC LifeCycle** **AD Azure Microsoft Single Sign-On**selecteren en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "Eenmalige aanmelding configureren")

3.  Typ de URL die wordt gebruikt door de gebruikers aan te melden op de levenscyclus van SCC toepassing met behulp van het volgende patroon '*https://bs1.scc.com/lc7/welcome/customer/PICTtest.aspx*' op de pagina **Configureren App-URL** in het tekstvak **Teken op een URL** en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding bij SCC LifeCycle** op **metagegevens downloaden**en vervolgens metagegevensbestand lokaal op uw computer opslaan.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "Eenmalige aanmelding configureren")

5.  Stuur dat bestand metagegevens SCC LifeCycle Support Team.

    >[AZURE.NOTE]Eenmalige aanmelding moet worden ingeschakeld door het SCC LifeCycle support team.

6.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij de levenscyclus van SCC Azure AD, moeten zij worden ingericht in de levenscyclus van SCC.
  
Er is geen actie-item voor u gebruikersaanvragen voor SCC LifeCycle configureren.  
Wanneer een toegewezen gebruiker zich probeert aan te melden bij de levenscyclus van SCC, wordt automatisch een levenscyclus van SCC-account gemaakt indien nodig.

>[AZURE.NOTE]Kunt u een andere SCC LifeCycle gebruiker account hulpmiddelen voor het maken of API's geleverd door SCC LifeCycle bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-scc-lifecycle-perform-the-following-steps"></a>Gebruikers toewijzen aan de levenscyclus van SCC, de volgende stappen uitvoeren:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **SCC LifeCycle **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.