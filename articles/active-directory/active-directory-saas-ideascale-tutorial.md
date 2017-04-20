<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met IdeaScale | Microsoft Azure" 
    description="Meer informatie over het IdeaScale met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-ideascale"></a>Zelfstudie: Azure Active Directory-integratie met IdeaScale
  
Het doel van deze zelfstudie is de integratie van de Azure en IdeaScale weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een IdeaScale eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan IdeaScale kan worden naar één teken in de toepassing met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor IdeaScale
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-ideascale-tutorial/IC790838.png "Scenario")
##<a name="enabling-the-application-integration-for-ideascale"></a>Inschakelen van de integratie van toepassingen voor IdeaScale
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor IdeaScale.

###<a name="to-enable-the-application-integration-for-ideascale-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor IdeaScale, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-ideascale-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-ideascale-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-ideascale-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-ideascale-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **IdeaScale**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-ideascale-tutorial/IC790841.png "Galerie van toepassing")

7.  **IdeaScale**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![IdeaScale] (./media/active-directory-saas-ideascale-tutorial/IC790842.png "IdeaScale")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij IdeaScale met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Configureren van eenmalige aanmelding voor IdeaScale moet u een waarde uit een certificaat ophalen.  
Als u niet bekend met deze procedure bent, raadpleegt u [voor het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **IdeaScale** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-ideascale-tutorial/IC790843.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op IdeaScale** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-ideascale-tutorial/IC790844.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **URL van App configureren** , in het tekstvak **IdeaScale teken op URL** de URL die wordt gebruikt door de gebruikers aan te melden op uw toepassing IdeaScale (bijvoorbeeld: "*https://company.IdeaScale.com*"), en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-ideascale-tutorial/IC790845.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding bij IdeaScale** downloaden van de metagegevens op **metagegevens downloaden**en sla het bestand lokaal op uw computer met metagegevens.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-ideascale-tutorial/IC790846.png "Eenmalige aanmelding configureren")

5.  Log in op uw site IdeaScale bedrijf als beheerder in een ander web browser-venster.

6.  Ga naar de **instellingen van de Gemeenschap**.

    ![Instellingen van de Gemeenschap] (./media/active-directory-saas-ideascale-tutorial/IC790847.png "Instellingen van de Gemeenschap")

7.  Ga naar **Security \> aanmelden instellingen enkele**.

    ![Instellingen voor één aanmelding.] (./media/active-directory-saas-ideascale-tutorial/IC790848.png "Instellingen voor één aanmelding.")

8.  Selecteer als **Type eenmaal aanmelden**, **SAML 2.0**.

    ![Één Type van aanmelding] (./media/active-directory-saas-ideascale-tutorial/IC790849.png "Één Type van aanmelding")

9.  Voer de volgende stappen uit in het dialoogvenster **Instellingen voor eenmalige aanmelding** :

    ![Instellingen voor één aanmelding.] (./media/active-directory-saas-ideascale-tutorial/IC790850.png "Instellingen voor één aanmelding.")

    1.  In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding bij IdeaScale** de **Entiteit-ID** -waarde kopiëren en vervolgens plakken in het tekstvak **SAML IdP entiteits-ID** .
    2.  Kopieer de inhoud van het metagegevensbestand gedownload en plak deze in de textbox **SAML IdP-metagegevens** .
    3.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij IdeaScale** de waarde van de **Externe Logout URL** kopiëren en vervolgens plakken in de textbox **Logout succes URL** .
    4.  Klik op **wijzigingen opslaan**.

10. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-ideascale-tutorial/IC790851.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij IdeaScale Azure AD, moeten zij worden ingericht in IdeaScale.  
Bij IdeaScale is het inrichten van een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder uw bedrijf **IdeaScale** site.

2.  Ga naar de **instellingen van de Gemeenschap**.

    ![Instellingen van de Gemeenschap] (./media/active-directory-saas-ideascale-tutorial/IC790847.png "Instellingen van de Gemeenschap")

3.  Ga naar **basisinstellingen \> lid Management**.

4.  Klik op **lid toevoegen**.

    ![Lid Management] (./media/active-directory-saas-ideascale-tutorial/IC790852.png "Lid Management")

5.  Voer de volgende stappen uit in de sectie Nieuw lid toevoegen:

    ![Nieuw lid toevoegen] (./media/active-directory-saas-ideascale-tutorial/IC790853.png "Nieuw lid toevoegen")

    1.  Typ in het tekstvak **E-mailadres** het e-mailadres van een geldige AAD-account die u verrichten wilt.
    2.  Klik op **wijzigingen opslaan**.

    >[AZURE.NOTE] De accounthouder Azure Active Directory krijgt een e-mailbericht met een koppeling naar de account bevestigen voordat deze actief wordt.

>[AZURE.NOTE] Kunt u een andere IdeaScale gebruiker account maken van hulpprogramma's of API's geleverd door IdeaScale bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-ideascale-perform-the-following-steps"></a>Gebruikers toewijzen aan IdeaScale, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **IdeaScale **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-ideascale-tutorial/IC790854.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-ideascale-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.