<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Aha! | Microsoft Azure" 
    description="Meer informatie over het gebruik van Aha! met Azure Active Directory inschakelen voor eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-aha"></a>Zelfstudie: Azure Active Directory-integratie met Aha!

Het doel van deze zelfstudie is weer te geven van de integratie van de Azure en Aha!  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een Aha! eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, de gebruikers Azure AD u hebt toegewezen aan Aha! is het mogelijk om één teken in de toepassing op de Aha! bedrijf-site (service gestart aanmelden op) of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Aha!
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-aha-tutorial/IC798944.png "Scenario")
##<a name="enabling-the-application-integration-for-aha"></a>Inschakelen van de integratie van toepassingen voor Aha!

Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Aha!.

###<a name="to-enable-the-application-integration-for-aha-perform-the-following-steps"></a>Om de toepassingsintegratie voor Aha!, voer de volgende stappen uit:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-aha-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-aha-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-aha-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-aha-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **Aha!**.

    ![Galerie van toepassing] (./media/active-directory-saas-aha-tutorial/IC798945.png "Galerie van toepassing")

7.  Selecteer in het resultatendeelvenster **Aha!**, en klik vervolgens op **Voltooien** als de toepassing wilt toevoegen.

    ![Aha!] (./media/active-directory-saas-aha-tutorial/IC802746.png "Aha!")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie is aan de contour van het inschakelen van gebruikers worden geverifieerd bij Aha! met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  In de klassieke Azure portal op de **Aha!** integratie van toepassingen pagina en klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-aha-tutorial/IC798946.png "Eenmalige aanmelding configureren")

2.  Op de **Hoe wilt u dat gebruikers aan te melden op Aha!** pagina, selecteer **Microsoft Azure AD Single Sign-On**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-aha-tutorial/IC798947.png "Eenmalige aanmelding configureren")

3.  Op de pagina **URL van App configureren** in de **Aha! Aanmelden op de URL** textbox, type de URL door gebruikers gebruikt voor aanmelding bij uw Aha! Toepassing (bijvoorbeeld: "*https://company.aha.io/session/new*"), en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-aha-tutorial/IC798948.png "App-URL configureren")

4.  Op de **op Aha eenmalige aanmelding configureren!** de pagina te downloaden bestand met metagegevens op **metagegevens downloaden**en sla het bestand lokaal op uw computer met metagegevens.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-aha-tutorial/IC798949.png "Eenmalige aanmelding configureren")

5.  In een ander browservenster zich aanmelden bij uw Aha! website bedrijf als beheerder.

6.  In het menu aan de bovenkant, klikt u op **Instellingen**.

    ![Instellingen] (./media/active-directory-saas-aha-tutorial/IC798950.png "Instellingen")

7.  Klik op **Account**.

    ![Profiel] (./media/active-directory-saas-aha-tutorial/IC798951.png "Profiel")

8.  Klik op **beveiliging en eenmalige aanmelding**.

    ![Beveiliging en eenmalige aanmelding] (./media/active-directory-saas-aha-tutorial/IC798952.png "Beveiliging en eenmalige aanmelding")

9.  Selecteer in **Single Sign-On** sectie als **Identiteitsprovider**, **SAML2.0**.

    ![Beveiliging en eenmalige aanmelding] (./media/active-directory-saas-aha-tutorial/IC798953.png "Beveiliging en eenmalige aanmelding")

10. Op de configuratiepagina van **Eenmalige aanmelding** kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding] (./media/active-directory-saas-aha-tutorial/IC798954.png "Eenmalige aanmelding")

    1.  Typ in het tekstvak **naam** een naam voor uw configuratie.
    2.  Voor het **configureren met behulp van** **Metagegevens-bestand**te selecteren.
    3.  Uw gedownloade om metagegevensbestand te uploaden, klikt u op **Bladeren**.
    4.  Klik op **bijwerken**.

11. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-aha-tutorial/IC798955.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Om gebruikers aan te melden bij Aha Azure AD!, moeten zij worden ingericht in Aha!.  
In het geval van Aha!, inrichten is een geautomatiseerde taak.  
Er is geen actie-item voor u.
  
Gebruikers worden automatisch gemaakt als nodig tijdens de eerste single sign-on.

>[AZURE.NOTE] U kunt een andere Aha! hulpmiddelen voor het maken van account of API's die worden geleverd door Aha! AAD gebruikersaccounts inrichten.

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-aha-perform-the-following-steps"></a>Gebruikers toewijzen aan Aha!, voer de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Op de **Aha!** integratie van toepassingen pagina, klikt u op **gebruikers toewijzen**.

    ![Gebruikers toewijzen] (./media/active-directory-saas-aha-tutorial/IC798956.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-aha-tutorial/IC767830.png "Ja")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
