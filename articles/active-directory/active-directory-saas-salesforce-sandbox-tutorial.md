<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met sandbox-televergaderingen | Microsoft Azure"
    description="Informatie over sandbox-televergaderingen met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!." 
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
    ms.date="10/28/2016" 
    ms.author="jeedes" />


#<a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Zelfstudie: Azure Active Directory-integratie met sandbox-televergaderingen
>[AZURE.TIP]Voor feedback, klik [hier](http://go.microsoft.com/fwlink/?LinkId=521878).
  
Het doel van deze zelfstudie is de integratie van de Azure sandbox-televergaderingen weergeven.  
Sandboxen kunnen u meerdere kopieën van uw organisatie in aparte omgevingen voor allerlei doeleinden, zoals de ontwikkeling, testen, training, zonder verlies van gegevens en toepassingen in uw organisatie televergaderingen productie maken.  
Voor meer informatie Zie [Sandbox-overzicht](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)
  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een sandbox in Salesforce.com
  
Als u nog een geldige sandbox Salesforce.com nog, moet u contact opnemen met televergaderingen.
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor televergaderingen Sandbox inschakelen
2.  Eenmalige aanmelding configureren
3.  Uw domein inschakelen
4.  Gebruikersaanvragen configureren
5.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "Scenario")
##<a name="enabling-the-application-integration-for-salesforce-sandbox"></a>De integratie van toepassingen voor televergaderingen Sandbox inschakelen
  
Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor televergaderingen sandbox-overzicht.

###<a name="to-enable-the-application-integration-for-salesforce-sandbox-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor sandbox-televergaderingen, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "Toepassingen")

4.  Om te openen in de **Galerie van toepassing**, klikt u op **Een App toevoegen**en klik op **toevoegen een toepassing voor mijn organisatie te gebruiken**.

    ![Wat wilt u doen?] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "Wat wilt u doen?")

5.  Typ in het **zoekvak** **Sandbox-televergaderingen**.

    ![Galerie van toepassing] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "Galerie van toepassing")

6.  **Sandbox-televergaderingen**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Sandbox-televergaderingen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Sandbox-televergaderingen")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij televergaderingen met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, op de pagina **Televergaderingen sandbox-** toepassing integratie **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op sandbox-televergaderingen** en klik op **volgende**.

    ![Sandbox-televergaderingen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Sandbox-televergaderingen")

3.  Typ op de pagina **App URL configureren** , in het tekstvak **Teken op URL** de URL van uw gebruik van het volgende patroon `http://company.my.salesforce.com`, en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "App-URL configureren")

4. Als u al eenmalige aanmelding voor een ander exemplaar van de sandbox-televergaderingen in de map hebt geconfigureerd, moet u ook de **id** als u wilt dat dezelfde waarde als de **URL aanmelden**configureren. Door het selectievakje **weergeven geavanceerde instellingen** op de pagina **URL van toepassing configureren** in het dialoogvenster vindt u het **id** -veld.

4.  Klik op **certificaat downloaden**en sla het bestand op uw computer op de pagina **configureren eenmalige aanmelding op sandbox-televergaderingen** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "Eenmalige aanmelding configureren")

5.  Log in op uw sandbox televergaderingen als beheerder in een ander web browser-venster.

6.  In het menu aan de bovenkant, klikt u op **Instellingen**.

    ![Setup] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "Setup")

7.  In het navigatiedeelvenster aan de linkerkant, klikt u op de **Besturingselementen voor beveiliging**en klik vervolgens op **Instellingen voor eenmalige aanmelding**.

    ![Instellingen voor eenmalige aanmelding] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "Instellingen voor eenmalige aanmelding")

8.  Voer de volgende stappen uit in de sectie instellingen voor eenmalige aanmelding:

    ![Instellingen voor eenmalige aanmelding] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "Instellingen voor eenmalige aanmelding")

    een.  Selecteer **SAML ingeschakeld**.
    
    b.  Klik op **Nieuw**.

9.  In de sectie SAML Single Sign-On-instellingen kunt u de volgende stappen uitvoeren:

    ![SAML Single Sign-On-instellingen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML Single Sign-On-instellingen")

    een.  Typ in het tekstvak Naam de naam van de configuratie (bijvoorbeeld: *SPSSOWAAD\_Test*).
    
    b.  In de Azure klassieke portal op de dialoog pagina **configureren eenmalige aanmelding op sandbox-televergaderingen** de **Uitgever URL** -waarde kopiëren en vervolgens plakken in het tekstvak van de **uitgevende instelling** .
    
    c.  Typ in het tekstvak **Entiteits-Id** **https://test.salesforce.com** als dit de eerste televergaderingen sandbox-instantie die u aan de map toevoegt. Als u al een exemplaar van de sandbox-televergaderingen en vervolgens voor het type **Entiteit-ID** in het **Teken op de URL**die in deze indeling moet worden toegevoegd:`http://company.my.salesforce.com`
    
    d.  Klik op **Bladeren** om het gedownloade certificaat uploaden.
    
    e.  Selecteren **Identiteitstype SAML** **Assertion bevat de ID van de Federatie van het gebruikersobject**.
    
    f.  Als **Locatie van SAML-identiteit**, selecteer **identiteit is in het NameIdentifier-element van het onderwerp Overzicht**.
    
    g.  De waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in de textbox **Identity Provider aanmeldings-URL** in de Azure klassieke portal op de dialoog pagina **configureren eenmalige aanmelding op sandbox-televergaderingen** .
    
    h.  SFDC ondersteunt SAML logout niet.  Plakken als tijdelijke oplossing 'https://login.windows.net/common/wsfederation?wa=wsignout1.0' in de **Identity Provider Logout URL** textbox.
    
    ik.  Schakel **HTTP POST** **Service Provider gestart aanvragen Binding**.
    
    j. Klik op **Opslaan**.

10. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "Eenmalige aanmelding configureren")

##<a name="enabling-your-domain"></a>Uw domein inschakelen
  
In dit gedeelte wordt ervan uitgegaan dat u al hebt gemaakt een domein.  
Zie [Uw domeinnaam definiëren](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US)voor meer informatie.

###<a name="to-enable-your-domain-perform-the-following-steps"></a>Als u wilt dat uw domein, kunt u de volgende stappen uitvoeren:

1.  In het linkernavigatievenster op **Domain Management**en klikt u vervolgens op **Mijn domein.**

    ![Mijn domein] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "Mijn domein")

    >[AZURE.NOTE]Controleer of uw domein correct is geconfigureerd.

2.  In de sectie **Aanmelden pagina-instellingen** klikt u op **bewerken**, vervolgens als **Authentication Service**, selecteert u de naam van de SAML Single Sign-On instelling van de vorige sectie en ten slotte klikt u op **Opslaan**.

    ![Mijn domein] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "Mijn domein")
  
Zodra u een domein geconfigureerd hebt, moeten de gebruikers de URL van het domein om aan te melden aan de sandbox televergaderingen gebruiken.  
Als u de waarde van de URL, klikt u op de SSO-profiel dat u in de vorige sectie hebt gemaakt.
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Het doel van deze sectie is het inschakelen van gebruikersaanvragen van gebruikersaccounts in Active Directory op televergaderingen sandbox-overzicht.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Selecteer uw naam om de gebruiker een menu weer te geven in het portal voor televergaderingen in de bovenste navigatiebalk:

    ![Mijn instellingen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "Mijn instellingen")

2.  Selecteer **Mijn instellingen** voor het openen van de pagina **Mijn instellingen** in het gebruikersmenu.

3.  Klik op **persoonlijke** als de persoonlijke sectie wilt uitvouwen in het linkerdeelvenster en klik op **Opnieuw mijn beveiligingssleutel**:

    ![Mijn instellingen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "Mijn instellingen")

4.  Klik op de pagina **Opnieuw mijn beveiligingssleutel** op **Beveiligingssleutel opnieuw instellen** voor het aanvragen van een e-mailbericht met uw beveiligingstoken Salesforce.com.

    ![Nieuwe Token] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "Nieuwe Token")

5.  Controleer uw postvak voor e-mail voor een e-mailbericht van Salesforce.com met '**salesforce.com.com beveiligingsbevestiging**' als onderwerp.

6.  Bekijk dit e-mailadres en de waarde voor token kopiëren.

7.  Klik in de klassieke Azure portal, klik op de pagina **televergaderingen Sandbox** application integration op **Gebruikersaanvragen configureren** om het dialoogvenster **Gebruikersaanvragen configureren** te openen.

    ![Gebruikersaanvragen configureren] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "Gebruikersaanvragen configureren")

8.  Klik op de pagina **Geef uw referenties op sandbox-televergaderingen als u Automatische gebruikersaanvragen** bieden de volgende configuratie-instellingen:

    ![Sandbox-televergaderingen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Sandbox-televergaderingen")

    een.  Typ in het tekstvak **Televergaderingen Sandbox Admin gebruikersnaam** televergaderingen sandbox-naam van een met het profiel van de **Systeembeheerder** in Salesforce.com toegewezen.

    b.  Typ in het tekstvak **Televergaderingen Sandbox Admin-wachtwoord** het wachtwoord voor deze account.

    c.  In het tekstvak **Gebruiker Security Token** , plak de waarde voor token.

    d.  Klik op **valideren** om te controleren of uw configuratie.

    e.  Klik op de knop **volgende** om de pagina **bevestiging** te openen.

9.  Klik op **Voltooien** om de configuratie op te slaan op **de bevestigingspagina** .
##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-salesforce-sandbox-perform-the-following-steps"></a>Gebruikers toewijzen aan Sandbox televergaderingen, moet u de volgende stappen uitvoeren:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Televergaderingen Sandbox **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Ja")
  
U moet nu wacht 10 minuten en controleren of de account is gesynchroniseerd met sandbox-televergaderingen.
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](https://msdn.microsoft.com/library/dn308586)voor meer informatie over het Access-venster.
