<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Schoolbord leren - Shibboleth | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Schoolbord leren - Shibboleth."
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
    ms.date="09/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-blackboard-learn---shibboleth"></a>Zelfstudie: Azure Active Directory-integratie met Schoolbord leren - Shibboleth

In deze zelfstudie leert u hoe te integreren Schoolbord leren - Shibboleth met Azure Active Directory (AD Azure).

Schoolbord meer - Shibboleth met Azure Active Directory integreren biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot het Schoolbord leren - Shibboleth heeft
- U kunt gebruikers automatisch ophalen ondertekend bij Schoolbord leren - Shibboleth (Single Sign-On) met hun Azure AD accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met Schoolbord leren - Shibboleth, moet u de volgende items:

- Een abonnement op Azure AD
- Een Schoolbord meer - Shibboleth eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving.

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Meer Schoolbord - Shibboleth uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-blackboard-learn---shibboleth-from-the-gallery"></a>Meer Schoolbord - Shibboleth uit de galerie toevoegen
Configureren van de integratie van het Schoolbord leren - Shibboleth in Azure AD, moet u toevoegen Schoolbord leren - Shibboleth uit de galerie aan de lijst met beheerde SaaS-apps.

**Om toe te voegen Schoolbord leren - Shibboleth uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Active Directory][1]
2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. In het zoekvak typt **Schoolbord leren - Shibboleth**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearnshibboleth_01.png)

7. In het resultatendeelvenster **Schoolbord leren - Shibboleth**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearnshibboleth_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD single sign-on met Schoolbord leren - Shibboleth op basis van een testgebruiker "Simon Britta" genoemd.

Voor eenmalige aanmelding te werken, Azure AD moet weten wat de tegenhanger van gebruiker in Schoolbord leren - Shibboleth is aan een gebruiker in AD Azure. Met andere woorden, een koppeling relatie tussen een Azure AD-gebruiker en de gebruiker in het Schoolbord leren - Shibboleth moet worden vastgesteld.

Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in het Schoolbord leren - Shibboleth toewijzen.

Als u wilt configureren en testen Azure AD single sign-on met Schoolbord leren - Shibboleth, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
3. **[Maken van een Schoolbord leren - het testgebruiker Shibboleth](#creating-a-blackboard-learn-shibboleth-test-user)** - een tegenhanger van Britta Simon in Schoolbord leren - Shibboleth die is gekoppeld aan de weergave Azure AD van haar hebben.
4. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie, Azure AD eenmalige aanmelding in de klassieke portal inschakelen en eenmalige aanmelding configureren in uw Schoolbord leren - Shibboleth-toepassing.


**Configureren van eenmalige aanmelding Azure AD met Schoolbord leren - Shibboleth, voert u de volgende stappen uit:**

1. Klik in de klassieke portal op de pagina toepassing integratie **Schoolbord leren - Shibboleth** **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.
     
    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Schoolbord leren - Shibboleth** **Azure AD Single Sign-On**selecteren en klik op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearnshibboleth_03.png) 

3. Voer de volgende stappen uit op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearnshibboleth_04.png) 

    een. In het **Teken op URL** -tekstvak typt u de URL die wordt gebruikt door uw gebruikers kunnen eenmalige aanmelding voor uw Schoolbord meer - Shibboleth-toepassing met behulp van het volgende patroon: **https://\<yourblackoardlearnserver\>.blackboardlearn.com/Shibboleth.sso/Login**
    
    b. Typ in het tekstvak **id** de URL met het volgende patroon: **https://\<yourblackoardlearnserver\>.blackboardlearn.com/shibboleth-sp**

    c. Typ in het tekstvak **URL antwoord** de URL met het volgende patroon: **https://\<yourblackoardlearnserver\>.blackboardlearn.com/Shibboleth.sso/SAML2/POST**

    > [AZURE.NOTE] U zal kunnen al deze waarden in de metagegevens van de Federatie doucument verstrekt door uw partner Schoolbord meer vinden.

    d. Klik op **volgende**
 
4. Klik op de pagina **configureren eenmalige aanmelding op een Schoolbord leren - Shibboleth** kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearnshibboleth_05.png)

    een. Klik op **metagegevens downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


5. Neem contact op met uw Schoolbord leren - Shibboleth-partner als u eenmalige aanmelding configureren voor uw toepassing, en geeft u het volgende:

    • De gedownloade **metagegevens**

    • De **URL van de uitgevende instelling**

    • De **SAML SSO-URL**

    • De **URL van de service voor eenmalige afmelden**

6. In de klassieke portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.
    
    ![Azure AD Single Sign-On][10]

7. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
 
    ![Azure AD Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.


![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster als volgt:  ![Azure AD test gebruikers maken](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Klik op de pagina van het dialoogvenster **Gebruikersprofiel** als volgt: ![Azure AD test gebruikers maken](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   


### <a name="creating-an-blackboard-learn---shibboleth-test-user"></a>Maken van een Schoolbord leren - het testgebruiker Shibboleth

In dit gedeelte maakt u een gebruiker met de naam Britta Simon in Schoolbord leren - Shibboleth. Neem samen met uw partner Schoolbord meer gebruikers toevoegen in het Schoolbord leren - platform Shibboleth.


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

In dit gedeelte vindt inschakelen u Britta Simon voor de Azure eenmalige aanmelding toegang verlenen voor Schoolbord meer - Shibboleth.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Schoolbord leren - Shibboleth, voert u de volgende stappen uit:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer **Schoolbord leren - Shibboleth**in de lijst toepassingen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearnshibboleth_50.png) 

3. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

In dit gedeelte vindt u Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Wanneer u klikt op het Schoolbord leren - Shibboleth tegel in het Configuratiescherm toegang u moet krijgen automatisch ondertekend voor toegang tot uw Schoolbord leren - Shibboleth-toepassing.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_205.png