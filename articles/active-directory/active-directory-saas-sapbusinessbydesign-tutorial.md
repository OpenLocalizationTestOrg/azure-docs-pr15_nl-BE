<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met SAP Business ByDesign | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SAP Business ByDesign."
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
    ms.date="09/09/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a>Zelfstudie: Azure Active Directory-integratie met SAP Business ByDesign

In deze zelfstudie leert u hoe SAP Business ByDesign integreren met Azure Active Directory (AD Azure).

SAP Business ByDesign integreren met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot SAP Business ByDesign heeft
- U kunt gebruikers automatisch ophalen ondertekend bij SAP Business ByDesign (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie met SAP Business ByDesign configureren, moet u de volgende items:

- Een abonnement op Azure AD
- Een SAP Business ByDesign eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving.

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. SAP Business ByDesign uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-sap-business-bydesign-from-the-gallery"></a>SAP Business ByDesign uit de galerie toe te voegen.
De integratie van SAP Business ByDesign in Azure AD configureren, moet u SAP Business ByDesign uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**SAP Business ByDesign toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.


3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **SAP Business ByDesign**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_01.png)

7. **SAP Business ByDesign**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Active Directory](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met SAP Business ByDesign op basis van een testgebruiker "Simon Britta" genoemd.

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in SAP Business ByDesign aan een gebruiker in AD Azure. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de bijbehorende gebruiker in SAP Business ByDesign worden vastgesteld.

Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in het SAP Business ByDesign toewijzen.

Als u wilt configureren en testen Azure AD eenmalige aanmelding met SAP Business ByDesign, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
3. **[Maken van een SAP Business ByDesign gebruiker testen](#creating-an-sap-business-bydesign-test-user)** - een tegenhanger van Britta Simon in SAP Business ByDesign die is gekoppeld aan de weergave Azure AD van haar hebben.
4. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie, Azure AD eenmalige aanmelding in de klassieke portal inschakelen en eenmalige aanmelding configureren in uw toepassing SAP Business ByDesign.

Het SAML-assertions verwacht SAP Business ByDesign toepassing in een specifieke indeling. Configureer de volgende claims voor deze toepassing. De waarden van deze kenmerken kunt u beheren via het tabblad **'Atrribute'** van de toepassing. De volgende schermafbeelding ziet u een voorbeeld voor dit. 


**Configureren van eenmalige aanmelding Azure AD met SAP Business ByDesign, voert u de volgende stappen uit:**


1. In de klassieke Azure portal, op de **ByDesign van SAP Business** application Integratiepagina in het menu aan de bovenkant, klikt u op **kenmerken**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_80.png) 


2. In de lijst kenmerken SAML-token kenmerken selecteert u het kenmerk nameidentifier en klik vervolgens op **bewerken**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_84.png) 

3. Voer de volgende stappen uit in het dialoogvenster gebruikerskenmerk bewerken:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_85.png) 

    een. Selecteer in de lijst waarde van het kenmerk, de **ExtractMailPrefix()** today

    b. Selecteer in de lijst e-mailberichten het gebruikerskenmerk dat u wilt gebruiken voor uw implementatie. 
    Bijvoorbeeld, als u wilt de werknemer-id gebruikt als de unieke gebruikers-id en u de waarde van het kenmerk in de ExtensionAttribute2 hebt opgeslagen, selecteert u **user.extensionattribute2**. 

    c. Klik op **Voltooien**. 
    

4. Klik in de klassieke portal op de pagina **ByDesign van SAP Business** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.
     
    ![Eenmalige aanmelding configureren][6] 

5. Op de pagina **Hoe wilt u dat gebruikers aanmelden bij SAP Business ByDesign** Selecteer **Azure AD Single Sign-On**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_03.png) 

6. Voer de volgende stappen uit op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_04.png) 

    een. Typ in het tekstvak **Teken op URL** de URL die wordt gebruikt door uw gebruikers om aanmelding bij uw SAP Business ByDesign-toepassing met behulp van het volgende patroon:`https://<servername>.sapbydesign.com`
    
    b. Klik op **volgende**
 
7. Voer de volgende stappen uit op de pagina **configureren eenmalige aanmelding op SAP Business ByDesign** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_05.png)

    een. Klik op **metagegevens downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


8. Als u eenmalige aanmelding configureren voor uw toepassing, kunt u de volgende stappen uitvoeren:

    een. Log in op uw portal SAP Business ByDesign met beheerdersrechten.

    b. Navigeer naar de **toepassing en de gebruiker Management taken** en klikt u op het tabblad **Identity Provider** .

    c. Klik op **Nieuwe identiteitsprovider** en selecteer de metagegevens XML-bestand dat u hebt gedownload van de klassieke Azure portal. U importeert de metagegevens, uploadt het systeem automatisch de vereiste handtekeningcertificaat en het versleutelingscertificaat.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)

    d. Als de **URL van de consument bevestiging** in de SAML-aanvraag, schakelt **Bevestiging consument Service URL opnemen**.

    e. Klik op **eenmalige aanmelding is geactiveerd**.

    f. Sla uw wijzigingen.

    g. Klik op het tabblad **Mijn systeem** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)

    h. De **SSO-URL** kopiëren en plakken in de textbox **Azure AD-teken in de URL** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)

    ik. Opgeven of de werknemer handmatig kunt aanmelden met een gebruikersnaam en wachtwoord of een SSO-door **Handmatige Identity Provider selectie**selecteren.

    j. Geef de URL die moet worden gebruikt door de werknemer voor aanmelding bij het systeem in de sectie **URL van eenmalige aanmelding** . 
    In de URL naar werknemer dropdown lijst verzonden, kunt u kiezen uit de volgende opties:
    
    **Niet-SSO-URL**
 
    Het systeem stuurt de URL van de normale regeling aan de werknemer. De werknemer kan zich aanmelden met behulp van SSO, en moet wachtwoord of certificaat in plaats daarvan.

    **SSO-URL** 

    Het systeem stuurt alleen de SSO-URL naar de werknemer. De werknemer kan aanmelden met behulp van SSO. Verificatieaanvraag wordt omgeleid via de IdP.

    **Automatische selectie**
 
    Als u eenmalige aanmelding niet is ingeschakeld, verzendt het systeem de URL van de normale regeling aan de werknemer. Als u eenmalige aanmelding is ingeschakeld, controleert het systeem of de werknemer een wachtwoord heeft. Als een wachtwoord beschikbaar is, worden zowel SSO-URL's en niet-SSO-URL verzonden aan de werknemer. Als de werknemer geen wachtwoord heeft, wordt echter alleen de SSO-URL naar de werknemer verzonden.

    k. Sla uw wijzigingen.

9. In de klassieke portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.
    
    ![Azure AD Single Sign-On][10]

10. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
 
    ![Azure AD Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.


![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-an-sap-business-bydesign-test-user"></a>Maken van een testgebruiker SAP Business ByDesign

In dit gedeelte maakt u een gebruiker met de naam Britta Simon in SAP Business ByDesign. Neem samen met SAP Business ByDesign support team de gebruikers toevoegen in het ByDesign van SAP Business-platform. 

> [AZURE.NOTE] Zorg ervoor dat NameID waarde met het veld gebruikersnaam in het ByDesign van SAP Business-platform overeenkomen moet.

### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

In dit gedeelte vindt inschakelen u Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan SAP Business ByDesign.

![Gebruiker toewijzen][200] 

**Britta Simon naar SAP Business ByDesign toewijzen, moet u de volgende stappen uitvoeren:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **SAP Business ByDesign**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_50.png) 

3. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In dit gedeelte vindt u Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Wanneer u op de tegel SAP Business ByDesign in het Access-venster, u moet krijgen automatisch ondertekend voor toegang tot uw toepassing SAP Business ByDesign.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_205.png
