<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met OpsGenie | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en OpsGenie."
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
    ms.date="10/07/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-opsgenie"></a>Zelfstudie: Azure Active Directory-integratie met OpsGenie

Het doel van deze zelfstudie is u tonen hoe OpsGenie integreren met Azure Active Directory (AD Azure).

OpsGenie integratie met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot de OpsGenie heeft
- U kunt gebruikers automatisch ophalen ondertekend bij OpsGenie (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met OpsGenie, moet u de volgende items:

- Een abonnement op Azure AD
- Een OpsGenie eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD testen in een testomgeving. 

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. OpsGenie uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-opsgenie-from-the-gallery"></a>OpsGenie uit de galerie toe te voegen.
De integratie van OpsGenie in Azure AD configureren, moet u OpsGenie uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Om de OpsGenie van de galerie wilt toevoegen, moet u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ **OpsGenie**in het zoekvak.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_01.png)

7. **OpsGenie**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is u tonen hoe te configureren en eenmalige aanmelding Azure AD test met OpsGenie op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weet de gebruiker tegenhanger in de OpsGenie aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gebruiker in OpsGenie worden vastgesteld.

Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** toewijzen in Azure AD als de waarde van de **gebruikersnaam** in het OpsGenie.

Als u wilt configureren en testen Azure AD single sign-on met OpsGenie, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Maken van een OpsGenie gebruiker testen](#creating-a-opsgenie-test-user)** - een tegenhanger van Britta Simon in OpsGenie die is gekoppeld aan de weergave Azure AD van haar hebben.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

Het doel van deze sectie is Azure AD eenmalige aanmelding in de klassieke Azure portal inschakelen en configureren van eenmalige aanmelding in uw toepassing OpsGenie.



**Configureren van eenmalige aanmelding Azure AD met OpsGenie, voert u de volgende stappen uit:**

1. Klik in de klassieke Azure portal, klik op de pagina **OpsGenie** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren][6] 

2. Selecteer **Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op OpsGenie** en klik op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_03.png) 

3. Voer de volgende stappen uit op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_04.png) 


    een. In het teken op URL-tekstvak typt u de URL die wordt gebruikt door gebruikers voor aanmelding bij uw toepassing OpsGenie is met het volgende patroon: **'https://app.opsgenie.com/auth/login'**.

    > [AZURE.NOTE] Neem contact op met uw [OpsGenie support team](mailto:support@opsgenie.com) als u het teken op de URL van uw nodig.

    b. Klik op **volgende**.


4. Klik op de pagina **configureren eenmalige aanmelding op OpsGenie** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_05.png) 

    een. Klik op **Certificaat downloaden**en sla het bestand op uw computer. Moeten we dit certificaat en de metagegevens van URL's (entiteits-ID, SSO-teken In de URL en Sign Out URL) voor het instellen van eenmalige aanmelding op OpsGenie aan de zijkant.

    b. Klik op **volgende**.


5. Een ander browserexemplaar opent en vervolgens aanmelden op OpsGenie als beheerder.

6. Klik op **Instellingen**en klik vervolgens op het tabblad voor **Eenmalige aanmelding** .
 
    ![OpsGenie voor eenmalige aanmelding](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_06.png) 

7. Als u eenmalige aanmelding, schakelt u **ingeschakeld**.

    ![OpsGenie instellingen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_07.png) 
   
8. Klik op het tabblad **Azure Active Directory** in de sectie **Provider** .

    ![OpsGenie instellingen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_08.png) 
    
9. Op de pagina van het dialoogvenster Azure Active Directory kunt u de volgende stappen uitvoeren:
 
    ![OpsGenie instellingen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_09.png) 

    een. De waarde van **Één teken in de URL** kopiëren en vervolgens plakken in het **Eindpunt van SAML 2.0** textbox in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op OpsGenie** .

    b. Een Base64-gecodeerd bestand maken van uw gedownloade certificaat.      
    
    > [AZURE.NOTE] Zie [conversie van binair certificaat naar een tekstbestand](https://www.youtube.com/watch?v=PlgrzUZ-Y1o&feature=youtu.be)voor meer informatie.

    c. De base-64 gecodeerde certificaat in Kladblok te openen, de inhoud ervan kopiëren naar het Klembord en plak deze in het tekstvak **X.500-certificaat** .

    d. Klik op **wijzigingen opslaan**.


6. In de klassieke Azure portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.

    ![Azure AD Single Sign-On][10]

7. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  

    ![Azure AD Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
Het doel van deze sectie is voor het maken van een testgebruiker in Azure klassieke portal Britta Simon genoemd.



![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke Portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :

    ![Azure AD test gebruikers maken](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-a-opsgenie-test-user"></a>Maken van een gebruiker OpsGenie test

Het doel van deze sectie is voor het maken van een gebruiker met de naam Britta Simon in OpsGenie. 

1.  Log in op uw huurder OpsGenie als beheerder in een webbrowservenster.

2.  Ga naar de lijst met gebruikers door te klikken op de **gebruiker** in het linkerpaneel.
   
    ![OpsGenie instellingen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_10.png) 

3.  Klik op '**gebruiker toevoegen**'.

3.  In het dialoogvenster **Gebruiker toevoegen** kunt u de volgende stappen uitvoeren:

    ![OpsGenie instellingen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_11.png) 

    een. Typ in het tekstvak **e-mailadres** van Britta e-mailadres in Azure Active Directory.

    b. Typ in het tekstvak **Volledige naam** **Britta Simon**.

    c. Klik op **Opslaan**. 

Britta krijgt een e-mail met instructies voor het instellen van haar profiel.


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

Het doel van deze sectie wordt aan het inschakelen van Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan OpsGenie.

![Gebruiker toewijzen][200] 

**Britta Simon aan OpsGenie toewijzen, moet u de volgende stappen uitvoeren:**

1. Op de Azure klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.
 
    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **OpsGenie**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Wanneer u op de tegel OpsGenie in het Access-venster, u moet krijgen automatisch ondertekend bij uw toepassing OpsGenie.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_205.png
