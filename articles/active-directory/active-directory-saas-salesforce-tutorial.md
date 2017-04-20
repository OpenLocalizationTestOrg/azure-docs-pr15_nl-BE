<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met televergaderingen | Microsoft Azure"
    description="Meer informatie over het televergaderingen met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!"
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

#<a name="tutorial-how-to-integrate-salesforce-with-azure-active-directory"></a>Zelfstudie: Het televergaderingen te integreren met Azure Active Directory

In deze zelfstudie wordt beschreven hoe u verbinding maken met uw televergaderingen omgeving Azure Active Directory. U leert het configureren van eenmalige aanmelding naar televergaderingen, het inschakelen van Automatische gebruikersaanvragen en hoe gebruikers toegang hebben tot televergaderingen toe te wijzen.

##<a name="prerequisites"></a>Vereisten

1. Voor toegang tot Azure Active Directory via het [portal voor Azure classic](https://manage.windowsazure.com), moet u een geldig abonnement Azure hebben.

2. U hebt een geldige huurder in [Salesforce.com](https://www.salesforce.com/).

> [AZURE.IMPORTANT] Als u een **evaluatieversie** Salesforce.com-account gebruikt, klikt zult u niet voor het configureren van Automatische gebruikersaanvragen. Proefaccounts hoeft niet de benodigde API toegang ingeschakeld totdat deze zijn gekocht.
> 
> Met een [gratis developer-account](https://developer.salesforce.com/signup) voor het voltooien van deze zelfstudie kunt u rond deze beperking krijgen.

Als u televergaderingen sandbox-omgeving gebruikt, raadpleegt u de [sandbox-televergaderingen integratie zelfstudie](https://go.microsoft.com/fwLink/?LinkID=521879).

##<a name="video-tutorials"></a>Zelfstudies op video

U kunt deze zelfstudie met behulp van de video's hieronder volgen.

**Video voor zelfstudie deel één: eenmalige aanmelding inschakelen**

> [AZURE.VIDEO integrating-salesforce-with-azure-ad-how-to-enable-single-sign-on]

**Video voor zelfstudie deel twee: Het automatiseren van gebruikersaanvragen**

> [AZURE.VIDEO integrating-salesforce-with-azure-ad-how-to-automate-user-provisioning]

##<a name="step-1-add-salesforce-to-your-directory"></a>Stap 1: Televergaderingen toevoegen aan de directory

1. Klik in de [Azure klassieke portal](https://manage.windowsazure.com)in het linkernavigatievenster op **Active Directory**.

    ![Selecteer Active Directory in het linkernavigatievenster.][0]

2. Selecteer in de lijst **map** de map die u wilt toevoegen van televergaderingen aan.

3. Klik op de **toepassingen** in het bovenste menu.

    ![Klik op toepassingen.][1]

4. Klik op **toevoegen** onder aan de pagina.

    ![Klik op toevoegen om een nieuwe toepassing.][2]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Klik op een toepassing uit de galerie toevoegen.][3]

6. Typ **televergaderingen**in het **zoekvak**. Selecteer **televergaderingen** in de resultaten, en klikt u op **Voltooien** om de toepassing toevoegen.

    ![Televergaderingen toevoegen.][4]

7. U ziet nu de pagina Quick Start voor televergaderingen:

    ![Televergaderingen de pagina Quick Start in Azure AD][5]

##<a name="step-2-enable-single-sign-on"></a>Stap 2: Eenmalige aanmelding inschakelen

1. Voordat u eenmalige aanmelding configureren kunt, moet u een aangepast domein te implementeren voor uw televergaderingen omgeving en instellen. Zie voor instructies over hoe u dat doet, [Stelt u een domeinnaam](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_setup.htm&language=en_US).

2. Televergaderingen de pagina Quick Start in Azure AD, klikt u op de knop voor **eenmalige aanmelding configureren** .

    ![De single sign-on knop configureren][6]

3. Er wordt een dialoogvenster geopend en u ziet een scherm met de vraag 'Hoe wilt u gebruikers aan te melden op televergaderingen dat?' Selecteer **Azure AD Single Sign-On**en klik vervolgens op **volgende**.

    ![Selecteer Azure AD Single Sign-On][7]

    > [AZURE.NOTE] Voor meer informatie over over de verschillende single sign-on opties, [Klik hier](../active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)

4. Vul op de pagina **Toepassingsinstellingen configureren** het **Teken op de URL** in door te typen in de URL van uw televergaderingen-domein met de volgende notatie:
 - Enterprise-account:`https://<domain>.my.salesforce.com`
 - Developer-account:`https://<domain>-dev-ed.my.salesforce.com` 

    ![Type in uw aanmelding op URL][8]

5. Klik op de pagina **configureren eenmalige aanmelding op televergaderingen** Klik op **certificaat downloaden**en sla het bestand lokaal op uw computer.

    ![Certificaat downloaden][9]

6. Open een nieuw tabblad in uw browser en meld u aan op uw televergaderingen administrator-account.

7. Klik op **Beveiliging besturingselementen** als de gerelateerde sectie wilt uitvouwen onder in het navigatiedeelvenster van de **beheerder** . Klik vervolgens op **Instellingen voor eenmalige aanmelding**.

    ![Klik op instellingen voor eenmalige aanmelding onder besturingselementen voor beveiliging][10]

8. Klik op de knop **bewerken** op de pagina **Instellingen voor eenmalige aanmelding** .

    ![Klik op de knop bewerken][11]

    > [AZURE.NOTE] Als u niet te schakelen in Single Sign-On voor uw account televergaderingen, wellicht moet u contact opnemen met ondersteuning van de televergaderingen om de functie voor u is ingeschakeld.

9. Selecteer **SAML ingeschakeld**en klik vervolgens op **Opslaan**.

    ![Selecteer de SAML-ingeschakeld][12]

10. De SAML single sign-on om instellingen te configureren, klikt u op **Nieuw**.

    ![Selecteer de SAML-ingeschakeld][13]

11. Op de pagina **SAML Single Sign-On instelling bewerken** en breng de volgende configuraties:

    ![Screenshot van de configuraties die u maken moet.][14]

 - Typ een beschrijvende naam voor deze configuratie voor het veld **naam** . Een waarde op te geven voor de **naam van** het tekstvak voor de **API** automatisch ingevuld.

 - In Azure AD, de **Uitgever URL** -waarde kopiëren en vervolgens plakken in het veld voor de **uitgever** in de televergaderingen.

 - Typ de domeinnaam van uw televergaderingen met behulp van het volgende patroon in de **textbox entiteits-Id**:
     - Enterprise-account:`https://<domain>.my.salesforce.com`
     - Developer-account:`https://<domain>-dev-ed.my.salesforce.com`

 - Klik op **Bladeren** of **Kies bestand** opent u het dialoogvenster **Kies bestand uploaden** , selecteer uw certificaat televergaderingen en klik vervolgens op **openen** om het certificaat te uploaden.

 - Selecteer **Identiteitstype SAML** **Assertion bevat de gebruikersnaam van salesforce.com**.

 - Selecteer **identiteit is in het NameIdentifier-element van het onderwerp Overzicht** voor **SAML identiteit locatie**,

 - In Azure AD, de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het veld **Aanmeldings-URL voor Identity Provider** in televergaderingen.

 - Selecteer **HTTP-omleiding**voor **Service Provider gestart aanvragen Binding**.

 - Ten slotte klikt u op **Opslaan** als u wilt uw SAML single sign-on-instellingen toepassen.

12. In het linkernavigatievenster van de televergaderingen, klikt u op **Domain Management** om de sectie Verwante te vouwen en klik vervolgens op **Mijn domein**.

    ![Klik op mijn domein][15]

13. Ga naar de sectie **Configuratie van verificatie** en klik op de knop **bewerken** .

    ![Klik op de knop bewerken][16]

14. De beschrijvende naam van de SAML SSO-configuratie te selecteren in het gedeelte **Authentication Service** en klik op **Opslaan**.

    ![Selecteer de SSO-configuratie][17]

    > [AZURE.NOTE] Als meer dan één authentication-service is ingeschakeld, wordt wanneer gebruikers proberen om eenmalige aanmelding voor uw omgeving televergaderingen ze gevraagd te selecteren welke ze aanmelden willen met authentication-service. Als u niet wilt dat dit gebeurt, vervolgens moet u **alle andere verificatieservices uitgeschakeld te laten**.

15. In Azure AD, schakel het selectievakje in de configuratie van één aanmelding bevestiging zodat het certificaat dat u geüpload naar televergaderingen. Klik vervolgens op **volgende**.

    ![Schakel het selectievakje bevestigen][18]

16. Typ in een e-mailadres als u wenst te ontvangen van e-mailberichten voor fouten en waarschuwingen die betrekking hebben op de instandhouding van deze configuratie voor aanmelding op de laatste pagina van het dialoogvenster. 

    ![Typ uw e-mailadres.][19]

17. Klik op **Voltooien** om het dialoogvenster te sluiten. Test de configuratie, Zie de sectie met de naam [Gebruikers toewijzen aan televergaderingen](#step-4-assign-users-to-salesforce).

##<a name="step-3-enable-automated-user-provisioning"></a>Stap 3: Schakel Automatische gebruikersaanvragen

1. Klik op de knop **configureren gebruikersaanvragen** Azure AD Quick Start pagina voor televergaderingen.

    ![Klik op de knop gebruikersaanvragen configureren][20]

2. Typ in het dialoogvenster **configureren gebruikersaanvragen** in uw televergaderingen admin gebruikersnaam en wachtwoord.

    ![Typ in uw admin gebruikersnaam of wachtwoord.][21]

    > [AZURE.NOTE] Als u een productie-omgeving configureren wilt, is de beste manier om een nieuwe account maken in televergaderingen specifiek voor deze stap. Deze account moet het profiel **Systeembeheerder** is toegewezen in een televergaderingen hebben.

3. Als u de beveiliging van uw televergaderingen token, open een nieuw tabblad en meld u aan dezelfde televergaderingen admin account. Klik op uw naam in de rechterbovenhoek van de pagina en klik op **Mijn instellingen**.

    ![Klik op uw naam en klik vervolgens op Mijn instellingen][22]

4. Klik op **persoonlijke** vouwt u het gedeelte Verwante in het linkernavigatievenster en klik vervolgens op **Opnieuw mijn beveiligingssleutel**.

    ![Klik op uw naam en klik vervolgens op Mijn instellingen][23]

5. Klik op de pagina **Opnieuw mijn beveiligingssleutel** op de **Beveiligingssleutel opnieuw instellen** .

    ![Lees de waarschuwingen.][24]

6. Controleer of het postvak is gekoppeld aan deze account admin. Zoek naar een e-mailbericht van Salesforce.com die het nieuwe beveiligingstoken bevat.

7. Kopieer de token, Ga naar het venster Azure AD en plak deze in het veld **Gebruiker beveiligingssleutel** . Klik vervolgens op **volgende**.

    ![Plak in het beveiligingstoken][25]

8. Op de bevestigingspagina kunt u e-mailmeldingen voor ontvangen als provisioning fouten optreden. Klik op **Voltooien** om het dialoogvenster te sluiten.

    ![Typ uw e-mailadres voor het ontvangen van meldingen][26]

##<a name="step-4-assign-users-to-salesforce"></a>Stap 4: Gebruikers toewijzen aan televergaderingen

1. Test uw configuratie, begint met het maken van een nieuwe testaccount in Active directory.

2. Klik op de pagina Quick Start televergaderingen op de **Gebruikers toewijzen** .

    ![Klik op gebruikers toewijzen][27]

3. Selecteer het testgebruiker en klik op de knop **toewijzen** aan de onderkant van het scherm:

 - Als u nog niet hebt inschakelen Automatische gebruikersaanvragen en vervolgens u de volgende vraag ziet te bevestigen:

        ![Confirm the assignment.][28]

 - Als u Automatische gebruikersaanvragen hebt ingeschakeld, ziet u een prompt om te bepalen welk type van het profiel van de televergaderingen die de gebruiker moet hebben. Nieuw ingerichte gebruikers moeten worden weergegeven in uw televergaderingen omgeving na een paar minuten.

        ![Confirm the assignment.][29]

        > [AZURE.IMPORTANT] Als u een **ontwikkelaar** televergaderingen omgeving worden ingericht, hebt u een zeer beperkt aantal licenties beschikbaar voor elk profiel. Daarom is het aanbevolen voor gebruikers van de bepaling voor de **babbelt gratis** gebruikersprofiel, dat 4,999-licenties die beschikbaar is.

4. Test uw instellingen voor eenmalige aanmelding, opent u het paneel toegang op [https://myapps.microsoft.com](https://myapps.microsoft.com/), en vervolgens de testaccount aanmelden en klik op **televergaderingen**.

##<a name="related-articles"></a>Verwante artikelen

- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Lijst met zelfstudies over het integreren van SaaS-toepassingen](active-directory-saas-tutorial-list.md)

[0]: ./media/active-directory-saas-salesforce-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-salesforce-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-salesforce-tutorial/add-app.png
[3]: ./media/active-directory-saas-salesforce-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-salesforce-tutorial/add-salesforce.png
[5]: ./media/active-directory-saas-salesforce-tutorial/salesforce-added.png
[6]: ./media/active-directory-saas-salesforce-tutorial/config-sso.png
[7]: ./media/active-directory-saas-salesforce-tutorial/select-azure-ad-sso.png
[8]: ./media/active-directory-saas-salesforce-tutorial/config-app-settings.png
[9]: ./media/active-directory-saas-salesforce-tutorial/download-certificate.png
[10]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png
[11]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png
[12]: ./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png
[13]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png
[14]: ./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png
[15]: ./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png
[16]: ./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png
[17]: ./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png
[18]: ./media/active-directory-saas-salesforce-tutorial/sso-confirm.png
[19]: ./media/active-directory-saas-salesforce-tutorial/sso-notification.png
[20]: ./media/active-directory-saas-salesforce-tutorial/config-prov.png
[21]: ./media/active-directory-saas-salesforce-tutorial/config-prov-dialog.png
[22]: ./media/active-directory-saas-salesforce-tutorial/sf-my-settings.png
[23]: ./media/active-directory-saas-salesforce-tutorial/sf-personal-reset.png
[24]: ./media/active-directory-saas-salesforce-tutorial/sf-reset-token.png
[25]: ./media/active-directory-saas-salesforce-tutorial/got-the-token.png
[26]: ./media/active-directory-saas-salesforce-tutorial/prov-confirm.png
[27]: ./media/active-directory-saas-salesforce-tutorial/assign-users.png
[28]: ./media/active-directory-saas-salesforce-tutorial/assign-confirm.png
[29]: ./media/active-directory-saas-salesforce-tutorial/assign-sf-profile.png
