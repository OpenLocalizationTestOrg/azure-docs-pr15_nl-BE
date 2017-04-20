<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Tangoe opdracht Premium Mobile | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Tangoe opdracht Premium Mobile."
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
    ms.date="09/29/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-tangoe-command-premium-mobile"></a>Zelfstudie: Azure Active Directory-integratie met Tangoe opdracht Premium Mobile

In deze zelfstudie leert u hoe te Tangoe opdracht Premium Mobile integreren met Azure Active Directory (AD Azure).

Tangoe opdracht Premium Mobile integreren met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot de Tangoe opdracht Premium Mobile heeft
- U kunt gebruikers automatisch ophalen ondertekend bij Tangoe opdracht Premium Mobile (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie met Tangoe opdracht Premium Mobile configureren, moet u de volgende items:

- Een abonnement op Azure
- Een Tangoe opdracht Premium Mobile eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. 

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Tangoe opdracht Premium Mobile uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-tangoe-command-premium-mobile-from-the-gallery"></a>Tangoe opdracht Premium Mobile uit de galerie toe te voegen.
De integratie van Tangoe opdracht Premium Mobile in Azure AD configureren, moet u Tangoe opdracht Premium Mobile uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Tangoe opdracht Premium Mobile toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Tangoe opdracht Premium Mobile**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_01.png)

7. **Tangoe opdracht Premium Mobile**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

![Azure AD test gebruikers maken](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_02.png)




##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Tangoe opdracht Premium Mobile op basis van een testgebruiker "Britta Simon" genoemd.

Voor eenmalige aanmelding wilt werken, moet AD Azure weet wat de gebruiker tegenhanger in Tangoe opdracht Premium Mobile Azure advertentie aan een gebruiker is. Met andere woorden, moet een relatie van de koppeling tussen een Azure AD-gebruiker en de gebruiker in Tangoe opdracht Premium Mobile worden vastgesteld.

Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Tangoe opdracht Premium Mobile toewijzen.

Als u wilt configureren en testen Azure AD eenmalige aanmelding met Tangoe opdracht Premium Mobile, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Maken van een Tangoe opdracht Premium mobiele gebruiker testen](#creating-an-tangoe-test-user)** - een tegenhanger van Britta Simon in Tangoe opdracht Premium Mobile die is gekoppeld aan de weergave Azure AD van haar hebben.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

In deze sectie, Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Tangoe opdracht Premium Mobile.


**Configureren van eenmalige aanmelding Azure AD met Tangoe opdracht Premium Mobile, voert u de volgende stappen uit:**

1. Klik in de klassieke portal op de pagina **Tangoe opdracht Premium Mobile** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren][6]


2. Klik op de pagina **Hoe wilt u dat gebruikers aan te melden op Tangoe opdracht Premium Mobile** **Azure AD Single Sign-On**selecteren en klik op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_03.png) 


3. Voer de volgende stappen uit op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** :
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_04.png) 


    een. **Sign in URL** textbox, typ de URL die wordt gebruikt door gebruikers voor aanmelding bij uw Tangoe opdracht Premium Mobile-toepassing met behulp van het volgende patroon: **"https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=\<huurder uitgever\>& Target =\<doel-URL van\>'**.

    b. Typ in het tekstvak **URL antwoord** de URL in het volgende patroon: **"https://sso.tangoe.com/sp/ACS.saml2"**

    > [AZURE.NOTE]  Als u niet de juiste waarden voor de URL kent, kunt u de bovenstaande waarden als tijdelijke aanduidingen en de aanvraag voor het koppelen van de juiste waarden van de klantenondersteuning voor uw Tangoe.


4. Voer de volgende stappen uit op de pagina **configureren eenmalige aanmelding bij Tangoe opdracht Premium Mobile** :
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_05.png) 

    een. Klik op **metagegevens downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


5.  Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met uw Tangoe klantenondersteuning koppelen en geef de volgende:


    - De van het gedownloade metagegevensbestand
    - De **URL van de uitgevende instelling**
    - De **SAML SSO-URL**
    - De **URL van de Service voor eenmalige afmelden**


  
6. In de klassieke portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.

    ![Azure AD Single Sign-On][10]

7. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
  
    ![Azure AD Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.

Selecteer in de lijst gebruikers **Britta Simon**.

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-tangoe-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-tangoe-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-tangoe-tutorial/create_aaduser_04.png)


5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-tangoe-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :

    ![Azure AD test gebruikers maken](./media/active-directory-saas-tangoe-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-tangoe-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:
 
    ![Azure AD test gebruikers maken](./media/active-directory-saas-tangoe-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-an-tangoe-command-premium-mobile-test-user"></a>Maken van een Tangoe opdracht Premium Mobile-testgebruiker

In dit gedeelte maakt u een gebruiker met de naam Britta Simon in Tangoe opdracht Premium Mobile. Tangoe opdracht Premium mobiele toepassing moeten alle gebruikers in de toepassing voordat u eenmalige aanmelding worden ingericht. Dus u moet werken met de Tangoe klant ondersteuning koppelen al deze gebruikers de toepassing inrichten. 


> [AZURE.NOTE] Als u een gebruiker handmatig maken of batch gebruikers, moet u contact opnemen met het ondersteuningsteam Tangoe opdracht Premium Mobile.


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

In dit gedeelte vindt inschakelen u Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan Tangoe opdracht Premium Mobile.

![Gebruiker toewijzen][200] 

**Britta Simon Tangoe opdracht Premium Mobile toewijzen, voert u de volgende stappen uit:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

![Gebruiker toewijzen][201] 

2. Selecteer in de lijst toepassingen **Tangoe opdracht Premium Mobile**.

![Eenmalige aanmelding configureren](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

In dit gedeelte vindt u Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Wanneer u op de tegel Tangoe opdracht Premium Mobile in het Access-venster, u moet krijgen automatisch ondertekend bij uw toepassing Tangoe opdracht Premium Mobile.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_205.png
