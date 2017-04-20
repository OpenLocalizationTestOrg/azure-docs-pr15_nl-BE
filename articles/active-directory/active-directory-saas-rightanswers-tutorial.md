<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met RightAnswers | Microsoft Azure" 
    description="Meer informatie over het RightAnswers met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-rightanswers"></a>Zelfstudie: Azure Active Directory-integratie met RightAnswers
  
Het doel van deze zelfstudie is de integratie van de Azure en RightAnswers weergeven. Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een RightAnswers eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan RightAnswers kan worden naar één teken in de toepassing met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor RightAnswers
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-rightanswers-tutorial/IC802925.png "Scenario")
##<a name="enabling-the-application-integration-for-rightanswers"></a>Inschakelen van de integratie van toepassingen voor RightAnswers
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor RightAnswers.

###<a name="to-enable-the-application-integration-for-rightanswers-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor RightAnswers, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-rightanswers-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-rightanswers-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-rightanswers-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-rightanswers-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **RightAnswers**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-rightanswers-tutorial/IC802926.png "Galerie van toepassing")

7.  **RightAnswers**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij RightAnswers met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **RightAnswers** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-rightanswers-tutorial/IC802927.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op RightAnswers** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-rightanswers-tutorial/IC802928.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **Toepassingsinstellingen configureren** , in het tekstvak **Teken op URL** de URL die wordt gebruikt door uw gebruikers om aanmelding bij uw toepassing RightAnswers (bv.: *https://fortify.rightanswers.com/portal/ss/*), en klik op **volgende**.

    ![App-instellingen configureren] (./media/active-directory-saas-rightanswers-tutorial/IC802929.png "App-instellingen configureren")

4.  Op de pagina **configureren eenmalige aanmelding bij RightAnswers** downloaden van de metagegevens op **metagegevens downloaden**en sla het bestand lokaal op uw computer met metagegevens.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-rightanswers-tutorial/IC802930.png "Eenmalige aanmelding configureren")

5.  De van het gedownloade metagegevensbestand verzenden naar uw RightAnswers support team.

    >[AZURE.NOTE] Uw RightAnswers support team heeft te maken van de werkelijke SSO-configuratie.
U krijgt een melding wanneer de eenmalige aanmelding is ingeschakeld voor uw abonnement.

6.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-rightanswers-tutorial/IC802931.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij RightAnswers Azure AD, moeten zij worden ingericht in RightAnswers.  
Bij RightAnswers is het inrichten van een geautomatiseerde taak.  
Er is geen actie-item voor u.
  
Gebruikers worden automatisch gemaakt als nodig tijdens de eerste single sign-on.

>[AZURE.NOTE]Kunt u een andere RightAnswers gebruiker account maken van hulpprogramma's of API's geleverd door RightAnswers bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-rightanswers-perform-the-following-steps"></a>Gebruikers toewijzen aan RightAnswers, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **RightAnswers **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-rightanswers-tutorial/IC802932.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-rightanswers-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.