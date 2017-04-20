<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met ServiceNow en ServiceNow Express | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en ServiceNow en ServiceNow Express."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-servicenow-and-servicenow-express"></a>Zelfstudie: Azure Active Directory-integratie met ServiceNow en ServiceNow Express.


In deze zelfstudie leert u hoe ServiceNow en ServiceNow Express integreren met Azure Active Directory (AD Azure).

ServiceNow en ServiceNow Express integreren met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot ServiceNow en ServiceNow Express heeft
- U kunt gebruikers automatisch ophalen ondertekend voor toegang tot ServiceNow en ServiceNow Express (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met ServiceNow en ServiceNow Express, moet u de volgende items:

- Een abonnement op Azure AD
- ServiceNow, een instantie of pachter van ServiceNow, Calgary versie of hoger
- Voor ServiceNow Express, een exemplaar van ServiceNow Express, Helsinki versie of hoger
- ServiceNow huurder moet hebben [Meerdere Provider één teken op invoegtoepassing](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) ingeschakeld. Dit u kunt doen door het indienen van een aanvraag voor service op <https://hi.service-now.com/> 


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. ServiceNow uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding voor ServiceNow of ServiceNow Express


## <a name="adding-servicenow-from-the-gallery"></a>ServiceNow uit de galerie toe te voegen.
De integratie van ServiceNow of ServiceNow Express in Azure AD configureren, moet u ServiceNow uit de galerie toevoegen aan de lijst met beheerde SaaS-apps. 

**ServiceNow toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **ServiceNow**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_01.png)

7. **ServiceNow**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en test Azure AD eenmalige aanmelding met ServiceNow of ServiceNow Express op basis van een testgebruiker met de naam "Britta Simon".

Voor eenmalige aanmelding wilt werken, moet AD Azure weet wat de gebruiker tegenhanger in ServiceNow Azure advertentie aan een gebruiker is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de bijbehorende gebruiker in ServiceNow worden vastgesteld.
Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in het ServiceNow toewijzen. Als u wilt configureren en testen Azure AD single sign-on met ServiceNow, moet u voor het voltooien van de volgende elementen:

1. **[Configureren AD Azure Single Sign-On voor ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Configureren AD Azure Single Sign-On voor ServiceNow Express](#configuring-azure-ad-single-sign-on-for-servicenow-express)** - gebruikers kunnen deze functie inschakelen.
3. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Gebruikers maken een ServiceNow testen](#creating-a-servicenow-test-user)** - hebben een tegenhanger van Britta Simon in ServiceNow die is gekoppeld aan de weergave Azure AD van haar.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
6. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

> [AZURE.NOTE] Als u wilt configureren ServiceNow stap 2 overslaan. Evenzo als u wilt configureren ServiceNow Express stap 1 overslaan.

### <a name="configuring-azure-ad-single-sign-on-for-servicenow"></a>Azure AD voor eenmalige aanmelding configureren voor ServiceNow

1.  Klik in de klassieke portal van Azure AD op de pagina **ServiceNow** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC749323.png "Eenmalige aanmelding configureren")

2.  Op de pagina **Hoe wilt u dat gebruikers aanmelden bij ServiceNow** Selecteer **AD Azure Microsoft Single Sign-On**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC749324.png "Eenmalige aanmelding configureren")

3.  Voer de volgende stappen uit op de pagina **Toepassingsinstellingen configureren** :

    ![App-URL configureren] (./media/active-directory-saas-servicenow-tutorial/IC769497.png "App-URL configureren")

    een. Typ in het tekstvak **ServiceNow teken op URL** de URL die wordt gebruikt door gebruikers voor aanmelding bij uw toepassing ServiceNow is het patroon volgen: `https://<instance-name>.service-now.com`.

    b. Typ de URL die door uw gebruikers gebruikt voor aanmelding bij uw ServiceNow toepassing na het patroon in het tekstvak **id** : `https://<instance-name>.service-now.com`.

    c. Klik op **volgende**

4.  Voer de instantienaam ServiceNow, gebruikersnaam admin en admin-wachtwoord in het formulier **automatisch configureren van eenmalige aanmelding** als Azure AD ServiceNow automatisch worden geconfigureerd voor verificatie op basis van SAML, en klik op *configureren*. Opmerking dat de gebruikersnaam admin hebben de rol van **security_admin** in ServiceNow voor deze werken. Anders handmatig configureren van ServiceNow Azure AD gebruikt als een identiteitsprovider SAML, **handmatig de toepassing voor eenmalige aanmelding configureren**, klik op **volgende** en voer de volgende stappen uit.

    ![App-URL configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694971.png "App-URL configureren")



5.  Klik op de pagina **configureren eenmalige aanmelding bij ServiceNow** **certificaat downloaden**, sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC749325.png "Eenmalige aanmelding configureren")

1. Aanmelding bij uw toepassing ServiceNow als beheerder.
2. De invoegtoepassing voor _integratie - meerdere Provider Single Sign-On Installer_ activeren door de volgende stappen:

    een. Ga naar de sectie **System Definition** in het navigatiedeelvenster aan de linkerkant en klik op **Plug-ins**.

    ![App-URL configureren] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "Activeren plugin")
    
    b. _Integratie - meerdere Provider Single Sign-On Installer_zoeken.

    ![App-URL configureren] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "Activeren plugin")

    c. Selecteer de invoegtoepassing. Rigth op en selecteer **Activeren/bijwerken**.
    
    d. Klik op de knop **activeren** .

2. In het navigatiedeelvenster aan de linkerzijde, klik op **Eigenschappen**.  

    ![App-URL configureren] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "App-URL configureren")


3. Voer de volgende stappen uit in het dialoogvenster **Eigenschappen van meerdere eenmalige aanmelding** :

    ![App-URL configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694981.png "App-URL configureren")

    een. Als u **meerdere voorziening eenmalige aanmelding inschakelen**, selecteert u **Ja**.

    b. Als de **logboekregistratie voor foutopsporing hebt u de meerdere provider SSO-integratie inschakelen**, selecteert u **Ja**.

    c. Typ in **het veld op de gebruiker... die tabel** tekstvak **gebruikersnaam**.

    d. Klik op **Opslaan**.



1. Klik in het navigatiedeelvenster aan de linkerkant, op **x509-certificaten**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "Eenmalige aanmelding configureren")


1. Klik op **Nieuw**in het dialoogvenster **X.509-certificaten** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694974.png "Eenmalige aanmelding configureren")


1. In het dialoogvenster voor **X.509-certificaten** , moet u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Eenmalige aanmelding configureren")

    een. Klik op **Nieuw**.

    b. Typ in het tekstvak **naam** een naam voor de configuratie (bijvoorbeeld: **TestSAML2.0**).

    c. Selecteer **actief**.

    d. **PEM**- **indeling**selecteren.

    e. Selecteer als **Type**, **Winkel certificaat vertrouwt**.
    
    f. Uw Base64-gecodeerde certificaat openen in Kladblok, de inhoud ervan kopiëren naar het Klembord en plak deze vervolgens naar het tekstvak **PEM certificaat** .

    g. Klik op **bijwerken**.


1. Klik in het navigatiedeelvenster aan de linkerkant op **Id-Providers**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "Eenmalige aanmelding configureren")

1. Klik op **Nieuw**in het dialoogvenster **Identiteitsproviders** :

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694977.png "Eenmalige aanmelding configureren")


1. Klik in het dialoogvenster **Identiteit aanbieders** op **SAML2 Update1?**:

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694978.png "Eenmalige aanmelding configureren")


1. In het dialoogvenster Eigenschappen van SAML2 Update1 voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694982.png "Eenmalige aanmelding configureren")


    een. Typ in het tekstvak **naam** een naam voor de configuratieset (bv.: **SAML 2.0**).

    b. In het **Veld gebruiker** textbox, **e-mailadres** of **user_id**, afhankelijk van welke veld wordt gebruikt als unieke identificatie voor gebruikers in uw implementatie ServiceNow. 
    
    > [AZURE.NOTE] Kunt u configue Azure AD te stoten, de Azure AD-gebruikers-ID (UPN-naam) of het e-mailadres als de unieke id in het SAML-token door te gaan naar de **ServiceNow > kenmerken > Single Sign-On** gedeelte van de klassieke Azure portal en het gewenste veld toe te wijzen aan het kenmerk **nameidentifier** . De opgeslagen waarde voor het geselecteerde kenmerk in Azure AD (bv. UPN-naam) moet overeenkomen met de waarde van ServiceNow voor het opgegeven veld (bijvoorbeeld user_id)

    c. In de klassieke portal Azure AD de **Identity Provider-ID** -waarde kopiëren en vervolgens plakken in het tekstvak **URL van de Provider identiteit** .

    d. In de klassieke portal Azure AD de **Verificatie aanvragen URL** -waarde kopiëren en vervolgens plakken in het tekstvak **id-Provider AuthnRequest** .

    e. In de klassieke portal Azure AD de **URL van de Service Single Sign-Out** waarde kopiëren en vervolgens plakken in het tekstvak **id-Provider SingleLogoutRequest** .

    f. Typ de URL van uw startpagina ServiceNow exemplaar in het tekstvak **ServiceNow Homepage** .

    > [AZURE.NOTE] De homepage van ServiceNow exemplaar is een samenvoeging van de **huurder URL van ServieNow** en **/navpage.do** (bijv.:`https://fabrikam.service-now.com/navpage.do`).
 

    g. In de **entiteit-ID / uitgever** textbox, typ de URL van uw huurder ServiceNow.

    h. Typ de URL van uw huurder ServiceNow in het tekstvak **URL van de doelgroep** . 

    ik. Typ in het tekstvak **-Binding Protocol voor SingleLogoutRequest van de IDP** **urn: oasis: namen: tc: SAML:2.0:bindings:HTTP-leiden**.

    j. Typ in het tekstvak NameID beleid **urn: oasis: namen: tc: SAML:1.1:nameid-indeling: onbekende**.

    k. Hef de selectie van **een AuthnContextClass maken**.

    l. Typ in de **Methode AuthnContextClassRef** `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`. Dit is alleen nodig als u alleen cloud-organisatie. Als u op de lokale ADFS of MVR gesloten voor de verificatie moet u deze waarde niet configureren. 

    m. Typ in het tekstvak **Schuintrekken klok** **60**.

    n. Als **Één teken op Script**, selecteert u **MultiSSO_SAML2_Update1**.

    o. Als **x509 certificaat**, selecteert u het certificaat dat u in de vorige stap hebt gemaakt.

    p. Klik op **indienen**. 



6. Op de klassieke Azure AD-portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**. 

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Eenmalige aanmelding configureren")

7. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .
 
    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Eenmalige aanmelding configureren")

### <a name="configuring-azure-ad-single-sign-on-for-servicenow-express"></a>Azure AD voor eenmalige aanmelding configureren voor ServiceNow Express

1.  Klik in de klassieke portal van Azure AD op de pagina **ServiceNow** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC749323.png "Eenmalige aanmelding configureren")

2.  Op de pagina **Hoe wilt u dat gebruikers aanmelden bij ServiceNow** Selecteer **AD Azure Microsoft Single Sign-On**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC749324.png "Eenmalige aanmelding configureren")

3.  Voer de volgende stappen uit op de pagina **Toepassingsinstellingen configureren** :

    ![App-URL configureren] (./media/active-directory-saas-servicenow-tutorial/IC769497.png "App-URL configureren")

    een. Typ in het tekstvak **ServiceNow teken op URL** de URL die wordt gebruikt door gebruikers voor aanmelding bij uw toepassing ServiceNow is het patroon volgen: `https://<instance-name>.service-now.com`.

    b. Typ de URL die door uw gebruikers gebruikt voor aanmelding bij uw ServiceNow toepassing na het patroon in het tekstvak **URL van de uitgevende instelling** `https://<instance-name>.service-now.com`.

    c. Klik op **volgende**

4.  Klik op **handmatig configureren van de toepassing voor eenmalige aanmelding**, en vervolgens klikt u op **volgende** en voer de volgende stappen uit.

    ![App-URL configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694971.png "App-URL configureren")

5.  Klik op **certificaat downloaden**op de pagina **configureren eenmalige aanmelding bij ServiceNow** , sla het bestand lokaal op uw computer, en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC749325.png "Eenmalige aanmelding configureren")

6. Aanmelding bij uw toepassing ServiceNow Express als beheerder.

7. Klik in het navigatiedeelvenster aan de linkerkant, **Single Sign-On**.  

    ![App-URL configureren] (./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "App-URL configureren")


8. In het dialoogvenster voor **Eenmalige aanmelding** , klikt u op het configuratiepictogram op de rechterbovenhoek en stel de volgende eigenschappen:

    ![App-URL configureren] (./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "App-URL configureren")

    een. Schakelen tussen **meerdere voorziening eenmalige aanmelding inschakelen** naar rechts.

    b. In-of uitschakelen **voor de verschillende integratie van provider SSO-logboekregistratie voor foutopsporing inschakelen** naar rechts.

    c. Typ in **het veld op de gebruiker... die tabel** tekstvak **gebruikersnaam**.


9. Klik op **Nieuw certificaat toevoegen**in het dialoogvenster **Single Sign-On** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "Eenmalige aanmelding configureren")



10. In het dialoogvenster voor **X.509-certificaten** , moet u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Eenmalige aanmelding configureren")

    een. Typ in het tekstvak **naam** een naam voor de configuratie (bijvoorbeeld: **TestSAML2.0**).

    b. Selecteer **actief**.

    c. **PEM**- **indeling**selecteren.

    d. Selecteer als **Type**, **Winkel certificaat vertrouwt**.

    e. Maak een Base64-gecodeerde bestand uit uw gedownloade certificaat.
   
    > [AZURE.NOTE] Zie [conversie van binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)voor meer informatie.
    
    f. Uw Base64-gecodeerde certificaat openen in Kladblok, de inhoud ervan kopiëren naar het Klembord en plak deze vervolgens naar het tekstvak **PEM certificaat** .

    g. Klik op **bijwerken**.


11. Klik op **Nieuwe IdP toevoegen**in het dialoogvenster **Single Sign-On** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "Eenmalige aanmelding configureren")


12. In het dialoogvenster **Nieuwe identiteitsprovider toevoegen** onder **Identiteitsprovider configureren**, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "Eenmalige aanmelding configureren")


    een. Typ in het tekstvak **naam** een naam voor de configuratieset (bv.: **SAML 2.0**).

    b. In de klassieke portal Azure AD de **Identity Provider-ID** -waarde kopiëren en vervolgens plakken in het tekstvak **URL van de Provider identiteit** .

    c. In de klassieke portal Azure AD de **Verificatie aanvragen URL** -waarde kopiëren en vervolgens plakken in het tekstvak **id-Provider AuthnRequest** .

    d. In de klassieke portal Azure AD de **URL van de Service Single Sign-Out** waarde kopiëren en vervolgens plakken in het tekstvak **id-Provider SingleLogoutRequest** .

    e. **Identity Provider certificaat**selecteren het certificaat dat u in de vorige stap hebt gemaakt.


13. Klik op **Geavanceerde instellingen**en onder **Meer eigenschappen identiteit**, de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "Eenmalige aanmelding configureren")

    een. Typ in het tekstvak **-Binding Protocol voor SingleLogoutRequest van de IDP** **urn: oasis: namen: tc: SAML:2.0:bindings:HTTP-leiden**.

    b. Typ in het tekstvak **NameID beleid** **urn: oasis: namen: tc: SAML:1.1:nameid-indeling: onbekende**.    

    c. Typ **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**in de **AuthnContextClassRef-methode**.
    
    d. Hef de selectie van **een AuthnContextClass maken**.

14. Onder **Extra eigenschappen van de Service Provider**, kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "Eenmalige aanmelding configureren")

    een. Typ de URL van uw startpagina ServiceNow exemplaar in het tekstvak **ServiceNow Homepage** .

    > [AZURE.NOTE] De homepage van ServiceNow exemplaar is een samenvoeging van de **huurder URL van ServieNow** en **/navpage.do** (bijv.: `https://fabrikam.service-now.com/navpage.do`).

    b. In de **entiteit-ID / uitgever** textbox, typ de URL van uw huurder ServiceNow.

    c. Typ de URL van uw huurder ServiceNow in het tekstvak **Doelgroep URI** . 

    d. Typ in het tekstvak **Schuintrekken klok** **60**.

    e. In het **Veld gebruiker** textbox, **e-mailadres** of **user_id**, afhankelijk van welke veld wordt gebruikt als unieke identificatie voor gebruikers in uw implementatie ServiceNow.
    
    > [AZURE.NOTE] Kunt u configue Azure AD te stoten, de Azure AD-gebruikers-ID (UPN-naam) of het e-mailadres als de unieke id in het SAML-token door te gaan naar de **ServiceNow > kenmerken > Single Sign-On** gedeelte van de klassieke Azure portal en het gewenste veld toe te wijzen aan het kenmerk **nameidentifier** . De opgeslagen waarde voor het geselecteerde kenmerk in Azure AD (bv. UPN-naam) moet overeenkomen met de waarde van ServiceNow voor het opgegeven veld (bijvoorbeeld user_id)

    f. Klik op **Opslaan**. 


15. Op de klassieke Azure AD-portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**. 

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Eenmalige aanmelding configureren")

16. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .
 
    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Het doel van deze sectie is het inschakelen van gebruikersaanvragen van Active Directory-gebruikersaccounts met ServiceNow overzicht.


### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1. Klik op **configureren gebruiker wordt ingericht**in de klassieke portal van Azure Management op de pagina **ServiceNow** application integration. 

    ![Gebruikersaanvragen] (./media/active-directory-saas-servicenow-tutorial/IC769498.png "Gebruikersaanvragen")


2. Bieden de volgende configuratie-instellingen op de pagina **Geef uw referenties ServiceNow, zodat Automatische gebruikersaanvragen** : gebruikersaanvragen configureren 

     een. Typ in het tekstvak **Naam van de instantie ServiceNow** de instantienaam ServiceNow.

     b. Typ in het tekstvak **ServiceNow Admin gebruikersnaam** de naam van de beheerdersaccount ServiceNow.

     c. Typ in het tekstvak **ServiceNow Admin-wachtwoord** het wachtwoord voor deze account.

     d. Klik op **valideren** om te controleren of uw configuratie.

     e. Klik op de knop **volgende** om de **volgende stappen** pagina te openen.

     f. Als u wilt dat alle gebruikers op deze toepassing, selecteer "**automatisch alle gebruikersaccounts in Active directory op deze toepassing inrichten**". 

    ![Volgende stappen] (./media/active-directory-saas-servicenow-tutorial/IC698804.png "Volgende stappen")

     g. Klik op de pagina van de **volgende stappen** **Voltooien** om de configuratie op te slaan.

### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-servicenow-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-servicenow-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-servicenow-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:
 
    ![Azure AD test gebruikers maken](./media/active-directory-saas-servicenow-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :

    ![Azure AD test gebruikers maken](./media/active-directory-saas-servicenow-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-servicenow-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-servicenow-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   


### <a name="creating-a-servicenow-test-user"></a>Een gebruiker ServiceNow test maken

In dit gedeelte maakt u een gebruiker met de naam Britta Simon in ServiceNow. In dit gedeelte maakt u een gebruiker met de naam Britta Simon in ServiceNow. Als u niet hoe u een gebruiker toevoegen in uw account ServiceNow of ServiceNow Express weet, contact op met het ondersteuningsteam ServiceNow.

### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

In dit gedeelte vindt inschakelen u Britta Simon voor de Azure eenmalige aanmelding toegang verlenen tot ServiceNow.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen ServiceNow, voert u de volgende stappen uit:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **ServiceNow**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_10.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst alle gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Wanneer u op de tegel ServiceNow in het Access-venster, u moet krijgen automatisch ondertekend bij uw toepassing ServiceNow.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-servicenow-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_205.png
