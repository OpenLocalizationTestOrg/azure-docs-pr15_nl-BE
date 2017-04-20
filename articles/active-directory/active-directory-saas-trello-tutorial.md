<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Trello | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Trello."
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
    ms.date="09/26/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-trello"></a>Zelfstudie: Azure Active Directory-integratie met Trello

In deze zelfstudie leert u hoe Trello integreren met Azure Active Directory (AD Azure).

Trello integreren met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot Trello heeft
- U kunt gebruikers automatisch ophalen ondertekend bij Trello (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met Trello, moet u de volgende items:

- Een abonnement op Azure AD
- Een **Trello** eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Trello uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-trello-from-the-gallery"></a>Trello uit de galerie toe te voegen.
De integratie van Trello in Azure AD configureren, moet u Trello uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Trello toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Trello**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-trello-tutorial/tutorial_trello_01.png)

7. **Trello**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-trello-tutorial/tutorial_trello_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Trello op basis van een testgebruiker "Simon Britta" genoemd.

Voor eenmalige aanmelding wilt werken, moet AD Azure weet wat de gebruiker tegenhanger in Trello Azure advertentie aan een gebruiker is. Met andere woorden, moet een relatie van de koppeling tussen een Azure AD-gebruiker en de bijbehorende gebruiker in Trello worden vastgesteld.
Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in het Trello toewijzen. Als u wilt configureren en testen Azure AD single sign-on met Trello, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Gebruikers maken een Trello testen](#creating-a-the-funding-portal-test-user)** - hebben een tegenhanger van Britta Simon in Trello die is gekoppeld aan de weergave Azure AD van haar.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

Het doel van deze sectie is Azure AD eenmalige aanmelding in de klassieke Azure portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Trello.

Trello toepassing verwacht de SAML-assertions specifieke kenmerken bevatten. Configureer de volgende kenmerken voor deze toepassing. De waarden van deze kenmerken kunt u beheren via het tabblad **'Atrributes'** van de toepassing. De volgende schermafbeelding ziet u een voorbeeld voor dit.

![Eenmalige aanmelding configureren](./media/active-directory-saas-trello-tutorial/tutorial_trello_03.png) 

**Configureren van eenmalige aanmelding Azure AD met Trello, voert u de volgende stappen uit:**

1. In de klassieke Azure portal, op de pagina **Trello** toepassing integratie, in het menu aan de bovenkant, klikt u op **kenmerken**.
     
    ![Eenmalige aanmelding configureren][5]


2. In het dialoogvenster **SAML-token kenmerken** voor elke rij die wordt weergegeven in de onderstaande tabel, de volgende stappen uitvoeren:
    

  	| Naam van kenmerk | Waarde van het kenmerk |
  	| --- | --- |    
  	| User.Email | User.mail |
  	| User.FirstName | User.givenName |
  	| User.LastName | User.surname |

    een. Klik op **gebruikerskenmerk toevoegen** om te openen, het dialoogvenster **Gebruiker-Attribure toevoegen** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-trello-tutorial/tutorial_trello_05.png)
    
    b. Typ de naam van het kenmerk voor de rij weergegeven in het tekstvak **Naam van kenmerk** .
    
    c. Selecteer in de lijst **Waarde van het kenmerk** de waarde voor die rij.
    
    d. Klik op **Voltooien**. Vervolgens **Wijzigingen toepassen** onder aan de pagina.

3. In het menu aan de bovenkant, klikt u op de **Werkbalk Snel starten**.

    ![Eenmalige aanmelding configureren][6]

4. Klik in de klassieke portal op de pagina **Trello** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren][7] 

5. Klik op de pagina **Hoe wilt u dat gebruikers aan te melden op Trello** **Azure AD Single Sign-On**selecteren en klik op **volgende**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-trello-tutorial/tutorial_trello_06.png)

6. Klik op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** als u instellen dat de toepassing in **IDP modus gestart wilt**, als volgt:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-trello-tutorial/tutorial_trello_07.png)


    een. Typ in het tekstvak URL antwoord op een URL met het volgende patroon: `https://trello.com/auth/saml/consume/<enterprise>`.

    b. Klik op **volgende**.

> [AZURE.NOTE] U moet de ** \<onderneming\> ** slug van Trello. Als u niet de waarde van de witruimte rond pagina's hebt, neem contact op met het ondersteuningsteam Trello op <support@trello.com> voor de witruimte voor u onderneming.

6. Als u instellen dat de toepassing in **SP modus gestart** op de pagina van het dialoogvenster **App-instellingen configureren wilt** , klikt u op de **"Show geavanceerde instellingen (optioneel)"** en voer vervolgens het **Teken op URL**:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-trello-tutorial/tutorial_trello_08.png)

    een. Typ in het tekstvak **Teken in de URL** een URL met het volgende patroon:`https://trello.com/auth/saml/consume/<enterprise>`

    b. Klik op **volgende**

7. Klik op de pagina **configureren eenmalige aanmelding op Trello** op **certificaat downloaden**en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-trello-tutorial/tutorial_trello_09.png)

6. Als u eenmalige aanmelding configureren voor uw toepassing, gaat u naar de pagina [Trello enterprise SSO-configuratie](https://trello.com/sso-configuration) voor het verzenden van Trello team teken op URL en gedownloade certificaat koppelen.

7. In de klassieke portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.
    
    ![Azure AD Single Sign-On][10]

8. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
    
    ![Azure AD Single Sign-On][11]

### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-trello-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-trello-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-trello-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:
 
    ![Azure AD test gebruikers maken](./media/active-directory-saas-trello-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :

    ![Azure AD test gebruikers maken](./media/active-directory-saas-trello-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-trello-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-trello-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-a-trello-test-user"></a>Een gebruiker Trello test maken

In dit gedeelte maakt u een gebruiker met de naam Britta Simon in Trello. In dit gedeelte maakt u een gebruiker met de naam Britta Simon in Trello. Trello ondersteunt just in time-aanbod en een nieuwe account wordt gemaakt de eerste keer dat u zich via Azure AD aanmelden.

### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

In dit gedeelte vindt inschakelen u Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan Trello.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Trello, voert u de volgende stappen uit:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Trello**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-trello-tutorial/tutorial_trello_10.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst alle gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Wanneer u op de tegel Trello in het Access-venster, u moet krijgen automatisch ondertekend bij uw toepassing Trello.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-trello-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-trello-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-trello-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-trello-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-trello-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-trello-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-trello-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-trello-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-trello-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-trello-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-trello-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-trello-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-trello-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-trello-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-trello-tutorial/tutorial_general_205.png
