<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Benefitsolver | Microsoft Azure"
    description="Meer informatie over het Benefitsolver met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a>Zelfstudie: Azure Active Directory-integratie met Benefitsolver

Het doel van deze zelfstudie is de integratie van de Azure en Benefitsolver weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een Benefitsolver eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Benefitsolver kan worden naar één teken in de toepassing met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Benefitsolver
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "Scenario")
##<a name="enabling-the-application-integration-for-benefitsolver"></a>Inschakelen van de integratie van toepassingen voor Benefitsolver

Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Benefitsolver.

###<a name="to-enable-the-application-integration-for-benefitsolver-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Benefitsolver, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **Benefitsolver**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Galerie van toepassing")

7.  **Benefitsolver**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Benefitssolver] (./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Benefitsolver met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Uw toepassing Benefitsolver verwacht de SAML-bevestigingen in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de **saml-token kenmerken** .  
De volgende schermafbeelding ziet u een voorbeeld voor dit.

![Kenmerken] (./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Kenmerken")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Benefitsolver** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Benefitsolver** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "Eenmalige aanmelding configureren")

3.  Voer de volgende stappen uit op de pagina **Toepassingsinstellingen configureren** :

    ![App-instellingen configureren] (./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "App-instellingen configureren")

    1.  Typ in het tekstvak **Aanmelden op de URL** **http://azure.benefitsolver.com**.
    2.  Typ in het tekstvak **URL antwoord** **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.  


    3.  Klik op **volgende**.

4.  Op de pagina **configureren eenmalige aanmelding bij Benefitsolver** downloaden van de metagegevens op **metagegevens downloaden**en sla het bestand lokaal op uw computer met metagegevens.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "Eenmalige aanmelding configureren")

5.  De van het gedownloade metagegevensbestand verzenden naar uw Benefitsolver support team.

    >[AZURE.NOTE] Uw Benefitsolver support team heeft te maken van de werkelijke SSO-configuratie.
U krijgt een melding wanneer de eenmalige aanmelding is ingeschakeld voor uw abonnement.

6.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "Eenmalige aanmelding configureren")

7.  In het menu aan de bovenkant, klikt u op **kenmerken** om de **SAML-Token kenmerken** -dialoogvenster te openen.

    ![Kenmerken] (./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "Kenmerken")

8.  Als u wilt de toewijzingen vereist kenmerk toevoegen, moet u de volgende stappen uitvoeren:

    ![Kenmerken] (./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Kenmerken")

  	|Naam van kenmerk|Waarde van het kenmerk|
  	|---|---|
  	|ClientID|U moet deze waarde ophalen uit uw Benefitsolver support team.|
  	|ClientKey|U moet deze waarde ophalen uit uw Benefitsolver support team.|
  	|LogoutURL|U moet deze waarde ophalen uit uw Benefitsolver support team.|
  	|Werknemer-id|U moet deze waarde ophalen uit uw Benefitsolver support team.|

    1.  Voor elke rij met gegevens in de bovenstaande tabel, klikt u op **gebruikerskenmerk toevoegen**.
    2.  Typ de naam van het kenmerk voor de rij weergegeven in het tekstvak **Naam van kenmerk** .
    3.  Selecteer in het tekstvak **Waarde van het kenmerk** de kenmerkwaarde voor die rij weergegeven.
    4.  Klik op **Voltooien**.

9.  Klik op **wijzigingen toepassen**.

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Om gebruikers aan te melden bij Benefitsolver Azure AD, moeten zij worden ingericht in Benefitsolver.  
Benefitsolver is gegevens over werknemers in uw toepassing door middel van een bestand telling van uw HRIS-systeem (meestal elke nacht) gevuld.  

>[AZURE.NOTE] Kunt u een andere Benefitsolver gebruiker account maken van hulpprogramma's of API's geleverd door Benefitsolver bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-benefitsolver-perform-the-following-steps"></a>Gebruikers toewijzen aan Benefitsolver, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Benefitsolver **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Ja")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
