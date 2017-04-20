<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met TOPdesk - beveiligde | Microsoft Azure"
    description="Informatie over het gebruik van TOPdesk - Secure met Azure Active Directory inschakelen voor eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Zelfstudie: Azure Active Directory-integratie met TOPdesk - veilig
  
Het doel van deze zelfstudie is weer te geven van de integratie van de Azure TOPdesk - veilig.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een TOPdesk - beveiligde eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan TOPdesk - veilig kan worden met één teken in de toepassing op uw TOPdesk - bedrijf beveiligde site (service gestart aanmelden op) of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor TOPdesk - veilig schakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "Scenario")

##<a name="enabling-the-application-integration-for-topdesk---secure"></a>De integratie van toepassingen voor TOPdesk - veilig schakelen
  
Het doel van deze sectie wordt aan de contour van het inschakelen van de integratie van toepassingen voor TOPdesk - veilig.

###<a name="to-enable-the-application-integration-for-topdesk---secure-perform-the-following-steps"></a>Als u de integratie van toepassingen voor TOPdesk - beveiligen, voer de volgende stappen uit:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **TOPdesk - veilig**.

    ![Galerie van toepassing] (./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "Galerie van toepassing")

7.  **TOPdesk - beveiligde**selecteren in het resultatendeelvenster en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![TOPdesk - veilig] (./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk - veilig")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie is het inschakelen van gebruikers worden geverifieerd bij de TOPdesk - overzicht met hun account beveiligen in Azure AD federation op basis van de SAML-protocol gebruiken.  
Moet u een pictogrambestand logo uploaden configureren eenmalige aanmelding voor TOPdesk - veilig. Als u het pictogrambestand, neem contact op met het ondersteuningsteam TOPdesk.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Meld u aan uw bedrijf **TOPdesk - beveiligde** site aan als beheerder.

2.  Klik op **Instellingen**in het menu **TOPdesk** .

    ![Instellingen] (./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Instellingen")

3.  Klik op **Instellingen voor aanmelding**.

    ![Instellingen voor aanmelding] (./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Instellingen voor aanmelding")

4.  Vouw het menu **Instellingen voor aanmelding** en klik vervolgens op **Algemeen**.

    ![Algemeen] (./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "Algemeen")

5.  In de sectie **beveiligen** van de configuratiesectie **SAML-aanmelding** moet u de volgende stappen uitvoeren:

    ![Technische instellingen] (./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "Technische instellingen")

    1.  Klik op **downloaden** om de van openbare-metagegevensbestand te downloaden en lokaal opslaan op uw computer.
    2.  Open het metagegevensbestand en zoek het knooppunt **AssertionConsumerService** .
        ![Dienstverlening aan consumenten bevestiging] (./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "Dienstverlening aan consumenten bevestiging")
    3.  Kopieer de waarde **AssertionConsumerService** .  

        >[AZURE.NOTE] Verderop in deze handleiding moet u de waarde in de sectie **URL van App configureren** .

6.  Log in op uw **Azure klassieke portal** als beheerder in een ander web browser-venster.

7.  Klik op de pagina **TOPdesk - beveiligde** toepassing integratie **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "Eenmalige aanmelding configureren")

8.  Op de pagina **Hoe wilt u dat gebruikers aan te melden op TOPdesk - beveiligde** **Microsoft Azure AD Single Sign-On**selecteren en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "Eenmalige aanmelding configureren")

9.  Op de pagina **URL van App configureren** , moet u de volgende stappen uitvoeren:

    ![App-URL configureren] (./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "App-URL configureren")

    1.  Typ in het tekstvak **TOPdesk - beveiligde aanmelding op URL** de URL die wordt gebruikt door de gebruikers wilt aanmelden bij de TOPdesk - beveiligde toepassingen (bijv.: "*https://qssolutions.topdesk.net*").
    2.  Plak in het tekstvak **TOPdesk – openbare URL voor antwoord** de **TOPdesk - Secure AssertionConsumerService URL** (bijvoorbeeld: "*https://qssolutions.topdesk.net/tas/public/login/saml*")
    3.  Klik op **volgende**.

10. Op de pagina **configureren eenmalige aanmelding op TOPdesk - veilig** op **metagegevens downloaden**uw metagegevensbestand te downloaden en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "Eenmalige aanmelding configureren")

11. Om een certificaat, voert u de volgende stappen uit:

    ![Certificaat] (./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "Certificaat")

    1.  De van het gedownloade metagegevensbestand openen.
    2.  Vouw het knooppunt **RoleDescriptor** met een **xsi: type** van **ingevoerd: ApplicationServiceType**.
    3.  De waarde van het knooppunt **X509Certificate** kopiëren.
    4.  De gekopieerde **X509Certificate** waarde lokaal opslaan in een bestand op uw computer.

12. Klik op **Instellingen**op de TOPdesk - bedrijf beveiligde site in het menu **TOPdesk** .

    ![Instellingen] (./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Instellingen")

13. Klik op **Instellingen voor aanmelding**.

    ![Instellingen voor aanmelding] (./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Instellingen voor aanmelding")

14. Vouw het menu **Instellingen voor aanmelding** en klik vervolgens op **Algemeen**.

    ![Algemeen] (./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "Algemeen")

15. Klik op **toevoegen**in het **openbare** gedeelte.

    ![Toevoegen] (./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "Toevoegen")

16. Klik op de pagina van het dialoogvenster **SAML-configuratie-assistent** kunt u de volgende stappen uitvoeren:

    ![SAML-configuratie-assistent] (./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "SAML-configuratie-assistent")

    1.  Klik op **Bladeren**om te uploaden van uw gedownloade metagegevensbestand onder **Federation-metagegevens**.
    2.  Als u wilt uploaden van uw certificaatbestand onder **Certificaat (RSA)**, klikt u op **Bladeren**.
    3.  Klik op **Bladeren**om te uploaden van het logobestand dat u van het ondersteuningsteam TOPdesk onder het **pictogram Logo**gekregen.
    4.  Typ in het tekstvak **kenmerk van gebruikersnaam** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    5.  Typ in het tekstvak **weergegeven naam** een naam voor uw configuratie.
    6.  Klik op **Opslaan**.

17. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij de TOPdesk - Azure AD veilig, ze moeten worden ingericht in TOPdesk - veilig.  
TOPdesk - veilig inrichten is een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Meld u aan uw bedrijf **TOPdesk - beveiligde** site aan als beheerder.

2.  Klik in het menu aan de bovenkant, **TOPdesk \> nieuwe \> -ondersteuningsbestanden \> Operator**.

    ![Operator] (./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "Operator")

3.  Voer de volgende stappen uit in het dialoogvenster **Nieuwe Operator** :

    ![Nieuwe Operator] (./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "Nieuwe Operator")

    1.  Klik op het tabblad Algemeen.
    2.  Typ in het tekstvak **naam** van de sectie **Algemeen** , de achternaam van een geldige Azure Active Directory-account die u verrichten wilt.
    3.  Selecteer een **Site** voor de account in de sectie **locatie** .
    4.  In het tekstvak van de **Aanmeldingsnaam** van de sectie **TOPdesk aanmelden** , typt u een aanmeldingsnaam voor de gebruiker.
    5.  Klik op **Opslaan**.

>[AZURE.NOTE] U kunt een andere TOPdesk - beveiligde gebruiker rekening maken of API's die worden geleverd door TOPdesk - beveiligde gebruikersaccounts AAD inrichten.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-topdesk---secure-perform-the-following-steps"></a>Gebruikers toewijzen aan TOPdesk - Secure, voer de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op de pagina **TOPdesk - beveiligde **toepassing integratie op **gebruikers toewijzen**.

    ![Gebruikers toewijzen] (./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.