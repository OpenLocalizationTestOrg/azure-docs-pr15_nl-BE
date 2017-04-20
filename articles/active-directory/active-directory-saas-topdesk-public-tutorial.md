<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met TOPdesk - publiek | Microsoft Azure" 
    description="Leren werken met TOPdesk - publiek met Azure Active Directory inschakelen voor eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-directory-integration-with-topdesk---public"></a>Zelfstudie: Azure Directory-integratie met TOPdesk - publiek

Het doel van deze zelfstudie is de integratie van de Azure TOPdesk - publiek weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een TOPdesk - openbare eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan TOPdesk - publiek mag één teken in de toepassing op uw TOPdesk - naamloze vennootschap-site (service gestart aanmelden op) of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor TOPdesk - publiek inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-topdesk-public-tutorial/IC790613.png "Scenario")

##<a name="enabling-the-application-integration-for-topdesk---public"></a>De integratie van toepassingen voor TOPdesk - publiek inschakelen
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor TOPdesk - publiek.

###<a name="to-enable-the-application-integration-for-topdesk---public-perform-the-following-steps"></a>Om de toepassingsintegratie voor TOPdesk - openbare, voer de volgende stappen uit:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-topdesk-public-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-topdesk-public-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-topdesk-public-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-topdesk-public-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **TOPdesk - publiek**.

    ![Galerie van toepassing] (./media/active-directory-saas-topdesk-public-tutorial/IC790614.png "Galerie van toepassing")

7.  **TOPdesk - publiek**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![TOPdesk publiek] (./media/active-directory-saas-topdesk-public-tutorial/IC791317.png "TOPdesk publiek")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij de TOPdesk - publiek met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Moet u een pictogrambestand logo uploaden configureren eenmalige aanmelding voor TOPdesk - publiek. Als u het pictogrambestand, neem contact op met het ondersteuningsteam TOPdesk.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Meld u aan uw site **TOPdesk - publiek** bedrijf aan als beheerder.

2.  Klik op **Instellingen**in het menu **TOPdesk** .

    ![Instellingen] (./media/active-directory-saas-topdesk-public-tutorial/IC790598.png "Instellingen")

3.  Klik op **Instellingen voor aanmelding**.

    ![Instellingen voor aanmelding] (./media/active-directory-saas-topdesk-public-tutorial/IC790599.png "Instellingen voor aanmelding")

4.  Vouw het menu **Instellingen voor aanmelding** en klik vervolgens op **Algemeen**.

    ![Algemeen] (./media/active-directory-saas-topdesk-public-tutorial/IC790600.png "Algemeen")

5.  In het **openbare** gedeelte van de configuratiesectie **SAML-aanmelding** moet u de volgende stappen uitvoeren:

    ![Technische instellingen] (./media/active-directory-saas-topdesk-public-tutorial/IC790601.png "Technische instellingen")

    1.  Klik op **downloaden** om de van openbare-metagegevensbestand te downloaden en lokaal opslaan op uw computer.
    2.  Open het metagegevensbestand en zoek het knooppunt **AssertionConsumerService** .
        ![AssertionConsumerService] (./media/active-directory-saas-topdesk-public-tutorial/IC790619.png "AssertionConsumerService")
    3.  Kopieer de waarde **AssertionConsumerService** .  

        >[AZURE.NOTE] Verderop in deze handleiding moet u de waarde in de sectie **URL van App configureren** .

6.  Log in op uw **Azure klassieke portal** als beheerder in een ander web browser-venster.

7.  Klik op de pagina **TOPdesk - publiek** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-topdesk-public-tutorial/IC790620.png "Eenmalige aanmelding configureren")

8.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op TOPdesk - publiek** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-topdesk-public-tutorial/IC790621.png "Eenmalige aanmelding configureren")

9.  Op de pagina **URL van App configureren** , moet u de volgende stappen uitvoeren:

    ![App-URL configureren] (./media/active-directory-saas-topdesk-public-tutorial/IC790622.png "App-URL configureren")

    1.  Typ in het tekstvak **TOPdesk - openbare teken op URL** de URL die wordt gebruikt door de gebruikers wilt aanmelden bij de TOPdesk - toepassing van openbare (bijvoorbeeld: "*https://qssolutions.topdesk.net*").
    2.  Plakken de **TOPdesk - openbare AssertionConsumerService-URL** in het tekstvak **TOPdesk – openbare antwoord URL** (bijvoorbeeld: "*https://qssolutions.topdesk.net/tas/public/login/saml*")
    3.  Klik op **volgende**.

10. Op de pagina **configureren eenmalige aanmelding op TOPdesk - publiek** op **metagegevens downloaden**uw metagegevensbestand te downloaden en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-topdesk-public-tutorial/IC790623.png "Eenmalige aanmelding configureren")

11. Om een certificaat, voert u de volgende stappen uit:

    ![Certificaat] (./media/active-directory-saas-topdesk-public-tutorial/IC790606.png "Certificaat")

    1.  De van het gedownloade metagegevensbestand openen.
    2.  Vouw het knooppunt **RoleDescriptor** met een **xsi: type** van **ingevoerd: ApplicationServiceType**.
    3.  De waarde van het knooppunt **X509Certificate** kopiëren.
    4.  De gekopieerde **X509Certificate** waarde lokaal opslaan in een bestand op uw computer.

12. Klik op **Instellingen**op de TOPdesk - site van de naamloze vennootschap, in het menu **TOPdesk** .

    ![Instellingen] (./media/active-directory-saas-topdesk-public-tutorial/IC790598.png "Instellingen")

13. Klik op **Instellingen voor aanmelding**.

    ![Instellingen voor aanmelding] (./media/active-directory-saas-topdesk-public-tutorial/IC790599.png "Instellingen voor aanmelding")

14. Vouw het menu **Instellingen voor aanmelding** en klik vervolgens op **Algemeen**.

    ![Algemeen] (./media/active-directory-saas-topdesk-public-tutorial/IC790600.png "Algemeen")

15. Klik op **toevoegen**in het **openbare** gedeelte.

    ![SAML-aanmelding] (./media/active-directory-saas-topdesk-public-tutorial/IC790625.png "SAML-aanmelding")

16. Klik op de pagina van het dialoogvenster **SAML-configuratie-assistent** kunt u de volgende stappen uitvoeren:

    ![SAML-configuratie-assistent] (./media/active-directory-saas-topdesk-public-tutorial/IC790608.png "SAML-configuratie-assistent")

    1.  Klik op **Bladeren**om te uploaden van uw gedownloade metagegevensbestand onder **Federation-metagegevens**.
    2.  Als u wilt uploaden van uw certificaatbestand onder **Certificaat (RSA)**, klikt u op **Bladeren**.
    3.  Klik op **Bladeren**om te uploaden van het logobestand dat u van het ondersteuningsteam TOPdesk onder het **pictogram Logo**gekregen.
    4.  Typ in het tekstvak **kenmerk van gebruikersnaam** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    5.  Typ in het tekstvak **weergegeven naam** een naam voor uw configuratie.
    6.  Klik op **Opslaan**.

17. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-topdesk-public-tutorial/IC790627.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij de TOPdesk - Azure AD openbaar zijn, moeten zij worden ingericht in TOPdesk - publiek.  
In het geval van TOPdesk - openbare inrichting is een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Meld u aan uw site **TOPdesk - publiek** bedrijf aan als beheerder.

2.  Klik in het menu aan de bovenkant, **TOPdesk \> nieuwe \> -ondersteuningsbestanden \> persoon**.

    ![Persoon] (./media/active-directory-saas-topdesk-public-tutorial/IC790628.png "Persoon")

3.  Voer de volgende stappen uit in het dialoogvenster nieuwe persoon:

    ![Nieuwe persoon] (./media/active-directory-saas-topdesk-public-tutorial/IC790629.png "Nieuwe persoon")

    1.  Klik op het tabblad Algemeen.
    2.  Typ in het tekstvak Achternaam de achternaam van een geldige Azure Active Directory-account die u verrichten wilt.
    3.  Selecteer een **Site** voor de account.
    4.  Klik op **Opslaan**.

>[AZURE.NOTE] Kunt u een andere TOPdesk - hulpmiddelen voor het openbare gebruiker account maken of API's die door TOPdesk - openbare voorziening AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-topdesk---public-perform-the-following-steps"></a>Gebruikers toewijzen aan TOPdesk - openbare, voer de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **TOPdesk - publiek **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-topdesk-public-tutorial/IC790630.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-topdesk-public-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.