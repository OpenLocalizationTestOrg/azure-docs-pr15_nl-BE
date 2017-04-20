<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met NetSuite | Microsoft Azure"
    description="Meer informatie over het NetSuite met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="05/16/2016"
    ms.author="asmalser-msft"/>

#<a name="tutorial-how-to-integrate-netsuite-with-azure-active-directory"></a>Zelfstudie: Het integreren van NetSuite met Azure Active Directory

In deze zelfstudie wordt beschreven hoe u verbinding maken met uw omgeving NetSuite Azure Active Directory (AD Azure). U leert het configureren van eenmalige aanmelding aan NetSuite, het inschakelen van Automatische gebruikersaanvragen en het toewijzen van gebruikers die toegang hebben tot NetSuite. 

##<a name="prerequisites"></a>Vereisten

1. Voor toegang tot Azure Active Directory via het [portal voor Azure classic](https://manage.windowsazure.com), moet u een geldig abonnement Azure hebben.

2. U moet beheerder toegang hebben tot een abonnement [NetSuite](http://www.netsuite.com/portal/home.shtml) . U kunt een gratis evaluatieversie account voor de service.

##<a name="step-1-add-netsuite-to-your-directory"></a>Stap 1: NetSuite toevoegen aan de Directory

1. Klik in de [Azure klassieke portal](https://manage.windowsazure.com)in het linkernavigatievenster op **Active Directory**.

    ![Selecteer Active Directory in het linkernavigatievenster.][0]

2. Selecteer in de lijst **map** de map die u wilt toevoegen van NetSuite aan.

3. Klik op de **toepassingen** in het bovenste menu.

    ![Klik op toepassingen.][1]

4. Klik op **toevoegen** onder aan de pagina.

    ![Klik op toevoegen om een nieuwe toepassing.][2]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Klik op een toepassing uit de galerie toevoegen.][3]

6. Typ in het **zoekvak**, **NetSuite**. Selecteer vervolgens **NetSuite** van de resultaten, en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![NetSuite toevoegen.][4]

7. U ziet nu de pagina Quick Start voor NetSuite:

    ![NetSuite de pagina Quick Start in Azure AD][5]

##<a name="step-2-enable-single-sign-on"></a>Stap 2: Eenmalige aanmelding inschakelen

1. In Azure AD, klikt u op de pagina Quick Start voor NetSuite, klikt u op de knop voor **eenmalige aanmelding configureren** .

    ![De single sign-on knop configureren][6]

2. Er wordt een dialoogvenster geopend en u ziet een scherm met de vraag 'Hoe wilt u gebruikers aan te melden op NetSuite dat?' Selecteer **Azure AD Single Sign-On**en klik vervolgens op **volgende**.

    ![Selecteer Azure AD Single Sign-On][7]

    > [AZURE.NOTE] Voor meer informatie over over de verschillende single sign-on opties, [Klik hier](../active-directory-appssoaccess-whatis/#how-does-single-sign-on-with-azure-active-directory-work)

3. Typ op de pagina **Toepassingsinstellingen configureren** voor het veld **Antwoord URL** in de URL van uw NetSuite huurder met behulp van een van de volgende indelingen:
    - `https://<tenant-name>.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na1.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na2.netsuite.com/saml2/acs`
    - `https://<tenant-name>.sandbox.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`

    ![Typ de URL van de huurder][8]

4. Klik op de pagina **configureren eenmalige aanmelding op NetSuite** Klik op **metagegevens downloaden**en sla het bestand lokaal op uw computer.

    ![Download de metagegevens.][9]

5. Een nieuw tabblad in uw browser openen en inloggen op uw site Netsuite bedrijf als beheerder.

6. De werkbalk boven aan de pagina **Setup**, klik op **Beheer van Setup**.

    ![Ga naar beheer van Setup][10]

7. Schakel **integratie**uit de lijst met **Taken** .

    ![Ga naar integratie][11]

8. Klik in de sectie **Verificatie beheren** **SAML Single Sign-on**.

    ![Ga naar de SAML Single Sign-on][12]

9. Op de pagina de **SAML-installatie** kunt u de volgende stappen uitvoeren:

    - De waarde van de **Externe aanmeldings-URL** kopiëren en plakken in het veld **Identity Provider aanmeldingspagina** in NetSuite in Azure Active Directory.

        ![De pagina Setup SAML.][13]

    - Selecteer in NetSuite, **Primaire methode voor verificatie**.

    - Voor het veld met de naam **SAMLV2 Identity Provider metagegevens**, selecteer **IDP metagegevens-bestand uploaden**. Klik vervolgens op **Bladeren** om het uploaden van het bestand met metagegevens dat u hebt gedownload in stap #4.

        ![De metagegevens uploaden][16]

    - Klik op **indienen**.

9. Schakel het selectievakje in de configuratie van één aanmelding bevestiging zodat het certificaat dat u hebt geüpload naar NetSuite in Azure AD. Klik vervolgens op **volgende**.

    ![Schakel het selectievakje bevestigen][14]

10. Typ in een e-mailadres als u wenst te ontvangen van e-mailberichten voor fouten en waarschuwingen die betrekking hebben op de instandhouding van deze configuratie voor aanmelding op de laatste pagina van het dialoogvenster. 

    ![Typ uw e-mailadres.][15]

11. Klik op **Voltooien** om het dialoogvenster te sluiten. Klik op **kenmerken** aan de bovenkant van de pagina.

    ![Klik op kenmerken.][17]

12. Klik op **gebruikerskenmerk toevoegen**.

    ![Klik op gebruikerskenmerk toevoegen.][18]

13. Typ in voor het veld **Attribute Name** `account`. Voor het veld **Waarde van het kenmerk** , typt u in uw account NetSuite. Instructies voor het zoeken naar uw account-ID vindt u hieronder:

    ![Toevoegen van uw Account NetSuite.][19]

    - In NetSuite, klik op **Instellingen** in het menu van de bovenste navigatiebalk.
    - Klik in de sectie **Taken** van het linkernavigatiemenu, selecteert u de sectie **integratie** en klikt u op het **Web Services-voorkeuren**.
    - Uw Account-ID van NetSuite kopiëren en plakken in het veld **Waarde van het kenmerk** in Azure AD.

        ![Uw account-ID ophalen][20]

14. In Azure AD, klikt u op **Voltooien** om het SAML-kenmerk toe te voegen. Klik vervolgens op **Wijzigingen toepassen** in het menu onder.

    ![Sla uw wijzigingen.][21]

15. Voordat gebruikers op in NetSuite uitvoeren kunnen, moeten zij eerst zijn toegewezen de juiste machtigingen in NetSuite. Volg de onderstaande instructies om deze machtigingen toe te wijzen.

    - Op de bovenste navigatiemenu **Setup**Klik op **Beheer van Setup**.

        ![Ga naar beheer van Setup][10]

    - In het linkernavigatiemenu **Gebruikers/rollen**selecteren, klik op **Rollen beheren**.

        ![Ga naar rollen beheren][22]

    - Klik op **nieuwe rol**.

    - Typ een **naam** voor de nieuwe rol en schakel het selectievakje **Single Sign-On alleen** .

        ![Naam van de nieuwe rol.][23]

    - Klik op **Opslaan**.

    - In het menu aan de bovenkant, klikt u op **machtigingen**. Klik vervolgens op **Setup**.

        ![Ga naar machtigingen][24]

    - Selecteer **Set Up SAM Single Sign-on**en klik vervolgens op **toevoegen**.

    - Klik op **Opslaan**.

    - Op de bovenste navigatiemenu **Setup**Klik op **Beheer van Setup**.

        ![Ga naar beheer van Setup][10]

    - In het linkernavigatiemenu **Gebruikers/rollen**selecteren, klik op **Gebruikers beheren**.

        ![Ga naar gebruikers beheren][25]

    - Selecteer een testgebruiker. Klik vervolgens op **bewerken**.

        ![Ga naar gebruikers beheren][26]

    - Selecteer de rol die u hebt gemaakt en klik op **toevoegen**in het dialoogvenster rollen.

        ![Ga naar gebruikers beheren][27]

    - Klik op **Opslaan**.

19. Test de configuratie, Zie de sectie met de naam [Gebruikers toewijzen aan NetSuite](#step-4-assign-users-to-netsuite).

##<a name="step-3-enable-automated-user-provisioning"></a>Stap 3: Schakel Automatische gebruikersaanvragen

> [AZURE.NOTE] Standaard wordt uw ingerichte gebruikers worden toegevoegd aan een dochteronderneming van de hoofdmap van uw omgeving NetSuite.

1. Azure Active Directory, op de pagina Quick Start voor NetSuite, klik op **configureren gebruikersaanvragen**.

    ![Gebruikersaanvragen configureren][28]

2. In het dialoogvenster dat wordt geopend, typt u in uw Administrator-referenties voor NetSuite en klik op **volgende**.

    ![Typ in uw NetSuite Administrator-referenties.][29]

3. Typ op de bevestigingspagina in uw e-mailadres voor het ontvangen van meldingen van fouten bij het inrichten.

    ![Bevestigen.][30]

4. Klik op **Voltooien** om het dialoogvenster te sluiten.

##<a name="step-4-assign-users-to-netsuite"></a>Stap 4: Gebruikers toewijzen aan NetSuite

1. Test uw configuratie, beginnen met het maken van een nieuwe testaccount in Active directory.

2. Klik op de knop **Gebruikers toewijzen** op de pagina Quick Start NetSuite.

    ![Klik op gebruikers toewijzen][31]

3. Selecteer het testgebruiker en klik op de knop **toewijzen** aan de onderkant van het scherm:

 - Als u nog niet hebt inschakelen Automatische gebruikersaanvragen en vervolgens u de volgende vraag ziet te bevestigen:

        ![Confirm the assignment.][32]

 - Als u Automatische gebruikersaanvragen hebt ingeschakeld, ziet u een prompt om te bepalen welk type van de rol van de gebruiker in NetSuite moet hebben. Nieuw ingerichte gebruikers moeten worden weergegeven in uw omgeving NetSuite na een paar minuten.

4. Test uw instellingen voor eenmalige aanmelding, opent u het paneel toegang op [https://myapps.microsoft.com](https://myapps.microsoft.com/), de testaccount aanmelden en klik op **NetSuite**.

##<a name="related-articles"></a>Verwante artikelen

- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Lijst met zelfstudies over het integreren van SaaS-toepassingen](active-directory-saas-tutorial-list.md)

[0]: ./media/active-directory-saas-netsuite-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-netsuite-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-netsuite-tutorial/add-app.png
[3]: ./media/active-directory-saas-netsuite-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-netsuite-tutorial/add-netsuite.png
[5]: ./media/active-directory-saas-netsuite-tutorial/quick-start-netsuite.png
[6]: ./media/active-directory-saas-netsuite-tutorial/config-sso.png
[7]: ./media/active-directory-saas-netsuite-tutorial/sso-netsuite.png
[8]: ./media/active-directory-saas-netsuite-tutorial/sso-config-netsuite.png
[9]: ./media/active-directory-saas-netsuite-tutorial/config-sso-netsuite.png
[10]: ./media/active-directory-saas-netsuite-tutorial/ns-setup.png
[11]: ./media/active-directory-saas-netsuite-tutorial/ns-integration.png
[12]: ./media/active-directory-saas-netsuite-tutorial/ns-saml.png
[13]: ./media/active-directory-saas-netsuite-tutorial/ns-saml-setup.png
[14]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-confirm.png
[15]: ./media/active-directory-saas-netsuite-tutorial/sso-email.png
[16]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-setup.png
[17]: ./media/active-directory-saas-netsuite-tutorial/ns-attributes.png
[18]: ./media/active-directory-saas-netsuite-tutorial/ns-add-attribute.png
[19]: ./media/active-directory-saas-netsuite-tutorial/ns-add-account.png
[20]: ./media/active-directory-saas-netsuite-tutorial/ns-account-id.png
[21]: ./media/active-directory-saas-netsuite-tutorial/ns-save-saml.png
[22]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-roles.png
[23]: ./media/active-directory-saas-netsuite-tutorial/ns-new-role.png
[24]: ./media/active-directory-saas-netsuite-tutorial/ns-sso.png
[25]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-users.png
[26]: ./media/active-directory-saas-netsuite-tutorial/ns-edit-user.png
[27]: ./media/active-directory-saas-netsuite-tutorial/ns-add-role.png
[28]: ./media/active-directory-saas-netsuite-tutorial/netsuite-provisioning.png
[29]: ./media/active-directory-saas-netsuite-tutorial/ns-creds.png
[30]: ./media/active-directory-saas-netsuite-tutorial/ns-confirm.png
[31]: ./media/active-directory-saas-netsuite-tutorial/assign-users.png
[32]: ./media/active-directory-saas-netsuite-tutorial/assign-confirm.png
