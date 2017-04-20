<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Origami | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en origami-werken."
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
    ms.date="10/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-origami"></a>Zelfstudie: Azure Active Directory-integratie met Origami

In deze zelfstudie leert u hoe Origami integreren met Azure Active Directory (AD Azure).

Origami integreren met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot Origami heeft
- U kunt gebruikers automatisch ophalen ondertekend bij Origami (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met Origami, moet u de volgende items:

- Een abonnement op Azure AD
- Een Origami eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving.

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Origami uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-origami-from-the-gallery"></a>Origami uit de galerie toe te voegen.
De integratie van Origami in Azure AD configureren, moet u Origami uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Origami toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Active Directory][1]
2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Origami**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-origami-tutorial/tutorial_origami_01.png)
7. **Origami**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-origami-tutorial/tutorial_origami_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Origami op basis van een testgebruiker "Simon Britta" genoemd.

Voor eenmalige aanmelding wilt werken, moet AD Azure weet wat de gebruiker tegenhanger in Origami Azure advertentie aan een gebruiker is. Met andere woorden, moet een relatie van de koppeling tussen een Azure AD-gebruiker en de bijbehorende gebruiker in Origami worden vastgesteld.

Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Origami toewijzen.

Als u wilt configureren en testen Azure AD single sign-on met Origami, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
3. **[Gebruikers maken een Origami test](#creating-a-origami-test-user)** - hebben een tegenhanger van Britta Simon in Origami die is gekoppeld aan de weergave Azure AD van haar.
4. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

In deze sectie, Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Origami.


**Configureren van eenmalige aanmelding Azure AD met Origami, voert u de volgende stappen uit:**

1. Klik in de klassieke portal op de pagina **Origami** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.
     
    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Origami** **Azure AD Single Sign-On**selecteren en klik op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/tutorial_origami_03.png) 

3. Voer de volgende stappen uit op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/tutorial_origami_04.png) 

    een. In het **Teken op URL** -tekstvak typt u de URL die wordt gebruikt door uw gebruikers om aanmelding bij de Origami-toepassing met behulp van het volgende patroon: **https://live.origamirisk.com/origami/account/login?account=\<bedrijf\> **
    
    b. Klik op **volgende**
 
4. Klik op de pagina **configureren eenmalige aanmelding op Origami** kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/tutorial_origami_05.png)

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.



1. Log in op de Origami-account met Administrator rechten.

1. In het menu aan de bovenkant, klik op de **beheerder**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/tutorial_origami_51.png)
  

1. Op de pagina één teken op Setup-dialoogvenster voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/123.png)

    een. Selecteer **eenmalige op inschakelen**.

    b. In de klassieke Azure portal de **SAML SSO-URL**kopiëren en vervolgens plakken in het tekstvak **Id-Provider-in de URL** .

    c. Kopieert **Één teken uit de URL**van de in de klassieke Azure portal, en vervolgens plakken in het tekstvak **URL van Sign-out id-Provider** .

    d. Klik op **Bladeren** om te uploaden, het certificaat dat u hebt gedownload van de klassieke Azure portal.

    e. Klik op **wijzigingen opslaan**.


6. In de klassieke portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.
    
    ![Azure AD Single Sign-On][10]

7. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
 
    ![Azure AD Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.


![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-origami-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-origami-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-origami-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster als volgt:  ![Azure AD test gebruikers maken](./media/active-directory-saas-origami-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Klik op de pagina van het dialoogvenster **Gebruikersprofiel** als volgt: ![Azure AD test gebruikers maken](./media/active-directory-saas-origami-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-origami-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-origami-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-an-origami-test-user"></a>Maken van een testgebruiker Origami

In dit gedeelte maakt u een gebruiker met de naam Britta Simon in Origami. 

1. Log in op de Origami-account met Administrator rechten.

2. In het menu aan de bovenkant, klik op de **beheerder**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/tutorial_origami_51.png)

3. Klik op **gebruikers**in het dialoogvenster **gebruikers en beveiliging** .
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/tutorial_origami_54.png)

4. Klik op **nieuwe gebruiker toevoegen**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/tutorial_origami_55.png)

5. In het dialoogvenster gebruiker toevoegen kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/tutorial_origami_56.png)

    een. Typ in het tekstvak **Gebruikersnaam** gebruiker naam Britta Simon in de klassieke Azure portal.

    b. Typ een passwotd in het tekstvak **wachtwoord** .

    c. Typ het wachtwoord nogmaals in het tekstvak **Wachtwoord bevestigen** .

    d. Typ in het tekstvak **Voornaam** **Britta**.

    e. Typ in het tekstvak **Achternaam** **Simon**.

    f. Klik op **Opslaan**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/tutorial_origami_57.png)

1. **Gebruikersrollen** en **Toegang van clients** toewijzen aan de gebruiker. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/tutorial_origami_58.png)

### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

In dit gedeelte vindt inschakelen u Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan Origami.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Origami, kunt u de volgende stappen uitvoeren:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Origami**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-origami-tutorial/tutorial_origami_50.png) 

3. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

In dit gedeelte vindt u Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Wanneer u op de tegel Origami in het Access-venster, u moet krijgen automatisch ondertekend bij uw toepassing Origami.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-origami-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-origami-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-origami-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-origami-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-origami-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-origami-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-origami-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-origami-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-origami-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-origami-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-origami-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-origami-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-origami-tutorial/tutorial_general_205.png
