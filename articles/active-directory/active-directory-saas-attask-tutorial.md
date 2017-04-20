<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met @Task| Microsoft Azure"
    description="Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en @Task."
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-task"></a>Zelfstudie: Azure Active Directory-integratie met@Task

Het doel van deze zelfstudie is u tonen hoe te integreren @Task met Azure Active Directory (AD Azure).  
Integratie van @Task met Azure Active Directory biedt u de volgende voordelen: 

- U kunt bepalen in Azure AD die toegang heeft tot@Task
- Kunt u uw gebruikers automatisch aangemeld bij @Task (Single Sign-On) met hun accounts Azure AD
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure Portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten 

Integratie met AD Azure configureren @Task, moet u de volgende items:

- Een abonnement op Azure AD
- Een @Task eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen. 

 
## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD testen in een testomgeving.  
Het scenario dat is beschreven in deze zelfstudie bestaat uit drie belangrijkste bouwstenen:

1. Toevoegen van @Task uit de galerie 
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-task-from-the-gallery"></a>Toevoegen van @Task uit de galerie
Voor het configureren van de integratie van de @Task in Azure AD, moet u toevoegen @Task uit de galerie aan de lijst met beheerde SaaS-apps.

**Toevoegen van @Task uit de galerie, de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1] 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2] 

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3] 

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4] 

6. Typ in het zoekvak **@Task**.

    ![Toepassingen][5] 

7. Selecteer in het deelvenster met resultaten, **@Task**, en klik vervolgens op **Voltooien** als de toepassing wilt toevoegen.

    ![Toepassingen][30] 



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding

Het doel van deze sectie is u tonen hoe te configureren en te testen Azure AD single sign-on met @Task op basis van een testgebruiker "Simon Britta" genoemd.

Voor eenmalige aanmelding te werken, Azure AD moet weten wat de tegenhanger van gebruiker in @Task voor een gebruiker in AD Azure is. Met andere woorden, een koppeling relatie tussen een Azure AD-gebruiker en de gebruiker in @Task moet worden vastgesteld.   
Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** toewijzen in Azure AD als de waarde van de **gebruikersnaam** in het @Task.
 
Configureren en testen Azure AD single sign-on met @Task, moet u uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Maken van een @Tasktest gebruiker](#creating-a-halogen-software-test-user) ** - hebben een tegenhanger van Britta Simon in @Taskthat is gekoppeld aan de weergave Azure AD van haar.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van deze sectie is Azure AD eenmalige aanmelding in de klassieke Azure portal inschakelen en configureren van eenmalige aanmelding in uw @Task toepassing.

**Voor het configureren van AD Azure single sign-on met @Task, de volgende stappen uitvoeren:**

1. In de klassieke Azure portal op de **@Task** toepassingsintegratie pagina en klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren][6] 

2. Op de **Hoe wilt u dat gebruikers zich aanmelden bij @Task ** **pagina en selecteer **Azure AD Single Sign-On****.

    ![Azure AD Single Sign-On][7] 

3. Voer de volgende stappen uit op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** :

    ![App-instellingen configureren][8] 
 
     een. In het **Teken op URL** -tekstvak typt u de URL die wordt gebruikt door gebruikers voor aanmelding bij uw @Task toepassing (bijvoorbeeld:*https://<Tenant name>.attask ondemand.com*).

     b. Klik op **volgende**.

4. Op de **configureren van eenmalige aanmelding op @Task ** pagina, klik op **Download metagegevens**metagegevens opslaan lokaal op uw computer en klik vervolgens op **volgende**.

    ![Wat is Azure AD verbinden][9] 



1. Aanmelding bij de @Task bedrijf site als beheerder.

2. Ga naar de **eenmalige aanmelding configureren**.


1. De volgende stappen uitvoeren in het dialoogvenster **Single Sign-On**

    ![Eenmalige aanmelding configureren][23]

    een. Selecteer als **Type** **SAML 2.0**.

    b. **Serviceprovider-ID**selecteren.

    c. De **Externe aanmeldings-URL**kopiëren en plak deze in de textbox **Aanmeldings-URL voor Portal** op de Azure klassieke portal.

    d. Kopieer de **URL van de Service Single Sign-Out**op de Azure klassieke portal en plak deze in het tekstvak **URL Sign-Out** .

    e. Kopieer de **URL van de wachtwoord wijzigen**op de Azure klassieke portal en plak deze in het tekstvak **URL van wachtwoord wijzigen** .

    f. Klik op **Opslaan**.

6. Selecteer de bevestiging enkele aanmelding-configuratie op de Azure klassieke portal en klik op **volgende**. 

    ![Wat is Azure AD verbinden][10]

7. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  

    ![Wat is Azure AD verbinden][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
Het doel van deze sectie is voor het maken van een testgebruiker in Azure klassieke portal Britta Simon genoemd.  

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-attask-tutorial/create_aaduser_02.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-attask-tutorial/create_aaduser_03.png) 
 
4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**. 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-attask-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren: 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-attask-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** : 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-attask-tutorial/create_aaduser_06.png) 
 
    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .
    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-attask-tutorial/create_aaduser_07.png) 
 
8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-attask-tutorial/create_aaduser_08.png) 
  
    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   

  
 
### <a name="creating-an-task-test-user"></a>Maken van een @Task testgebruiker

Het doel van deze sectie is voor het maken van een gebruiker met de naam Britta Simon in @Task.


**Voor het maken van een gebruiker met de naam Britta Simon in @Task, de volgende stappen uitvoeren:**

1. Aanmelden bij uw @Task bedrijf site als beheerder.

2. Klik in het menu aan de bovenkant, **mensen**.

3. Klik op **nieuwe persoon**. 

4. Voer de volgende stappen uit in het dialoogvenster nieuwe persoon:

    ![Maak een @Task testgebruiker][21] 

    een. Typ in het tekstvak **Voornaam** "Britta".

    b. Typ in het tekstvak **Achternaam** 'Simon'.

    c. Typ in het tekstvak **E-mailadres** Britta Simon van e-mailadres in Azure Active Directory.

    d. Klik op de **persoon toevoegen**.




### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

Het doel van deze sectie wordt aan het inschakelen van Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot @Task.

![Gebruiker toewijzen][200] 

**Toewijzen Britta Simon aan @Task, de volgende stappen uitvoeren:**

1. Op de Azure klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **@Task**.

    ![Gebruiker toewijzen][202] 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.  
Wanneer u klikt op de @Task naast elkaar in het Configuratiescherm toegang, ontvangt u automatisch aangemeld bij de @Task toepassing.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-attask-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attask-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attask-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attask-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_01.png
[30]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_02.png


[6]: ./media/active-directory-saas-attask-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_03.png
[8]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_04.png
[9]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_05.png
[10]: ./media/active-directory-saas-attask-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-attask-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-attask-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_08.png


[23]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_06.png

[200]: ./media/active-directory-saas-attask-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attask-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_09.png
[203]: ./media/active-directory-saas-attask-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-attask-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-attask-tutorial/tutorial_general_205.png






