<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Concur | Microsoft Azure" 
    description="Meer informatie over het Concur met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-concur"></a>Zelfstudie: Azure Active Directory-integratie met Concur  


Het doel van deze zelfstudie is de integratie van de Azure Concur weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder in Concur

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor Concur inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-concur-tutorial/IC769766.png "Scenario")

>[AZURE.NOTE] De configuratie van uw abonnement Concur voor federatieve eenmalige aanmelding via SAML is een afzonderlijke taak, moet u contact opnemen met Concur uit te voeren.

##<a name="enabling-the-application-integration-for-concur"></a>De integratie van toepassingen voor Concur inschakelen

Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor Concur overzicht.

###<a name="to-enable-the-application-integration-for-concur-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Concur, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-concur-tutorial/IC700993.png "Active Directory")

2.  Vanuit de **Directory** lijst, selecteer de map wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-concur-tutorial/IC700994.png "Toepassingen")

4.  Om te openen in de **Galerie van toepassing**, klikt u op **Een App toevoegen**en klik op **toevoegen een toepassing voor mijn organisatie te gebruiken**.

    ![Wat wilt u doen?] (./media/active-directory-saas-concur-tutorial/IC700995.png "Wat wilt u doen?")

5.  Typ in het **zoekvak** **Concur**.

    ![Galerie van toepassing] (./media/active-directory-saas-concur-tutorial/IC721727.png "Galerie van toepassing")

6.  In het resultatendeelvenster **Concur**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Toegeven] (./media/active-directory-saas-concur-tutorial/IC721728.png "Toegeven")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Concur met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

>[AZURE.NOTE] De configuratie van uw abonnement Concur voor federatieve eenmalige aanmelding via SAML is een afzonderlijke taak, moet u contact opnemen met Concur uit te voeren.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Concur **application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-concur-tutorial/IC769767.png "Eenmalige aanmelding configureren")

2.  Op de pagina **Hoe wilt u dat gebruikers aan te melden op Concur** Selecteer **AD Azure Microsoft Single Sign-On**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-concur-tutorial/IC769768.png "Eenmalige aanmelding configureren")

3.  Op de pagina **App URL configureren** in de **Sign toegeven In URL** textbox, typt u uw concur huurder-in URL en klik op **volgende**: 

    ![URL aanmelden configureren] (./media/active-directory-saas-concur-tutorial/IC769769.png "URL aanmelden configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding op Concur** kunt u de volgende stappen uitvoeren.

    ![URL aanmelden configureren] (./media/active-directory-saas-concur-tutorial/IC769770.png "URL aanmelden configureren")

    1.  Klik op Download de metagegevens en vervolgens veilig het gegevensbestand op uw computer.
    2.  Neem contact op met het ondersteuningsteam Concur SSO configureren voor de huurder.
    3.  Selecteer de Configuratiebevestiging van één aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.  

    >[AZURE.NOTE] De configuratie van uw abonnement Concur voor federatieve eenmalige aanmelding via SAML is een afzonderlijke taak, moet u contact opnemen met Concur uit te voeren.

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Het doel van deze sectie is aan de contour van het inschakelen van Active Directory-gebruikersaccounts aan Concur-aanbod.

Als u toepassingen in de Service onkosten, er is installatie en het gebruik van een Web Service Admin-profiel. Niet gewoon de rol Admin WS toevoegen aan uw bestaande beheerdersprofiel die u voor T & E administratieve functies gebruikt.

Consultants toegeven of de beheerder van de client moet een afzonderlijke webservicebeheerder profiel maken en de beheerder van de Client moet dit profiel gebruiken voor het Web Services-beheerder functies (bijvoorbeeld toepassingen activeren). Deze profielen moeten gescheiden worden gehouden van de client dagelijkse T & E admin beheerdersprofiel (het admin-profiel T & E mogen geen rol WSAdmin).

Bij het maken van het profiel moet worden gebruikt voor het inschakelen van de toepassing de client van de beheerder naam invoeren in de velden gebruiker profiel Deze wordt eigenaar toegewezen aan het profiel. Zodra de profiel(en) is gemaakt, moet de client aanmelden met dit profiel klikt u op '*inschakelen*' voor de toepassing van een Partner in het menu Web Services.

Om de volgende redenen moet deze actie niet worden uitgevoerd met het profiel dat ze voor normale T & E-beheer gebruiken.

1.  De client is dat '*Ja*' klikt in het dialoog venster dat wordt weergegeven nadat een toepassing is ingeschakeld. U erkent klikt u op dat de client is bereid voor de toepassing van de Partner toegang te krijgen tot hun gegevens, zodat u of de Partner kan niet Klik op die knop Ja.
2.  Als beheerder van een client die een app is ingeschakeld met behulp van de T & E admin profiel het bedrijf verlaat (waardoor het profiel wordt geïnactiveerd), alle apps ingeschakeld met behulp van dat profiel niet werken totdat de app met een andere actieve profiel van WS-beheer is ingeschakeld. Dit is de reden waarom u moeten afzonderlijke WS Admin om profielen te maken.
3.  Als een beheerder het bedrijf verlaat, kan de naam die is gekoppeld aan het WS Admin-profiel worden gewijzigd in de beheerder van de vervangende desgewenst zonder invloed op dat de ingeschakelde app omdat dat profiel niet hoeft deactiveren

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Aanmelden bij uw huurder **Concur** .

2.  Selecteer **Web Services**in het menu **Administratie** .

    ![De huurder concur] (./media/active-directory-saas-concur-tutorial/IC721729.png "De huurder concur")

3.  Selecteer aan de linkerkant uit het deelvenster **Webservices** **Partnertoepassing inschakelen**.

    ![Partnertoepassing inschakelen] (./media/active-directory-saas-concur-tutorial/IC721730.png "Partnertoepassing inschakelen")

4.  **Azure Active Directory**selecteren in de lijst **Toepassing inschakelen** en klik vervolgens op **inschakelen**.

    ![Microsoft Azure Active Directory] (./media/active-directory-saas-concur-tutorial/IC721731.png "Microsoft Azure Active Directory")

5.  Klik op **Ja** om de **Actie bevestigen** -dialoogvenster te sluiten.

    ![Bevestig de actie] (./media/active-directory-saas-concur-tutorial/IC721732.png "Bevestig de actie")

6.  Selecteer in het portal voor Azure klassieke **Concur** uit de lijst met toepassingen op de pagina **Concur** -dialoogvenster openen.

7.  Het dialoogvenster **Configureren gebruikersaanvragen** pagina, klikt u op **configureren van gebruikersaanvragen**.

8.  Voer de gebruikersnaam en het wachtwoord van de beheerder Concur en klik op **volgende**.

9.  Klik op de knop **Voltooien** om de configuratie, klikt u op de pagina **bevestiging** .

Nu kunt u een testaccount maken, wacht 10 minuten en controleer of de account is gesynchroniseerd naar Concur.
##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-concur-perform-the-following-steps"></a>Gebruikers toewijzen aan Concur, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Concur **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-concur-tutorial/IC769771.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-concur-tutorial/IC767830.png "Ja")

U moet nu wacht 10 minuten en controleren of de account is gesynchroniseerd met de Concur.

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
