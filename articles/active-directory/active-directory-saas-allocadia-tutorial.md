<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Allocadia | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Allocadia."
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
    ms.date="09/19/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-allocadia"></a>Zelfstudie: Azure Active Directory-integratie met Allocadia

In deze zelfstudie leert u hoe Allocadia integreren met Azure Active Directory (AD Azure).

Allocadia integratie met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot de Allocadia heeft
- U kunt gebruikers automatisch ophalen ondertekend bij Allocadia (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met Allocadia, moet u de volgende items:

- Een abonnement op Azure AD
- Een Allocadia eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Allocadia uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-allocadia-from-the-gallery"></a>Allocadia uit de galerie toe te voegen.
De integratie van Allocadia in Azure AD configureren, moet u Allocadia uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Om de Allocadia van de galerie wilt toevoegen, moet u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ **Allocadia**in het zoekvak.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_01.png)

7. **Allocadia**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Allocadia op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in de Allocadia aan een gebruiker in AD Azure. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gebruiker in Allocadia worden vastgesteld.
Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** toewijzen in Azure AD als de waarde van de **gebruikersnaam** in het Allocadia.

Als u wilt configureren en testen Azure AD single sign-on met Allocadia, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Maken van een Allocadia gebruiker testen](#creating-an-allocadia-test-user)** - een tegenhanger van Britta Simon in Allocadia die is gekoppeld aan de weergave Azure AD van haar hebben.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

In deze sectie, Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Allocadia.


Het SAML-assertions verwacht toepassing Allocadia in een specifieke indeling. Configureer de volgende claims voor deze toepassing. De waarden van deze kenmerken kunt u beheren via het tabblad **'Atrribute'** van de toepassing. De volgende schermafbeelding ziet u een voorbeeld voor dit. 

![Eenmalige aanmelding configureren](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_07.png) 

**Configureren van eenmalige aanmelding Azure AD met Hightail, voert u de volgende stappen uit:**


1. In de klassieke Azure portal, op de pagina **Allocadia** toepassing integratie, in het menu aan de bovenkant, klikt u op **kenmerken**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-allocadia-tutorial/tutorial_general_80.png) 


2. In het dialoogvenster **SAML-token kenmerken** voor elke rij die wordt weergegeven in de onderstaande tabel, de volgende stappen uitvoeren:

  	| Naam van kenmerk | Waarde van het kenmerk |
  	| --- | --- |    
  	| Voornaam | User.givenName |
  	| Achternaam  | User.surname |
  	| E-mail | User.mail |
    

    een. Klik op **gebruikerskenmerk toevoegen** om te openen, het dialoogvenster **Gebruiker-Attribure toevoegen** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-allocadia-tutorial/tutorial_general_81.png) 


    b. Typ de naam van het kenmerk voor de rij weergegeven in het tekstvak **Naam Attrubute** .

    c. In de lijst **Waarde van het kenmerk** selsect de waarde van het kenmerk voor de rij weergegeven.

    d. Klik op **Voltooien**.  
    

3. In het menu aan de bovenkant, klikt u op de **Werkbalk Snel starten**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-allocadia-tutorial/tutorial_general_83.png)  


4. Selecteer **Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Allocadia** en klik op **volgende**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_03.png) 

5. Klik op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** als volgt:.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_04.png) 

    een. Typ in het vak id de URL in het volgende patroon: test omgeving gebruikt de URL als **"https://na2standby.allocadia.com"** en voor de productie-omgeving gebruiken **"https://na2.allocadia.com"**

    b. Typ in de antwoord-URL de URL in het volgende patroon: test omgeving gebruikt het URL-patroon als **"https://na2standby.allocadia.com/allocadia/saml/SSO"** en voor de productie-omgeving gebruiken **"https://na2.allocadia.com/allocadia/saml/SSO"**


6. Klik op de pagina **configureren eenmalige aanmelding op Allocadia** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_05.png) 

    een. Klik op **metagegevens downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


7.  Als u eenmalige aanmelding configureren voor uw toepassing, dan [Allocadia Support](mailTo:support@allocadia.com) team en ze staan voor het configureren van eenmalige aanmelding. Neem gedownload die u hebt voor het verzenden van e-mail en koppelen metagegevensbestand SSO configureren aan de kant van de Allocadia.
 
    > [AZURE.NOTE] Zorg ervoor dat de id-waarde Allocadia team instellen in de testomgeving als **"https://na2standby.allocadia.com"** en voor de productie-omgeving, moet worden: **"https://na2.allocadia.com"**


8. In de klassieke portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.
    
    ![Azure AD Single Sign-On][10]

9. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
    
    ![Azure AD Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken

In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.
Selecteer in de lijst gebruikers **Britta Simon**.

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-allocadia-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-allocadia-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-allocadia-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:
 
    ![Azure AD test gebruikers maken](./media/active-directory-saas-allocadia-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :

    ![Azure AD test gebruikers maken](./media/active-directory-saas-allocadia-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-allocadia-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-allocadia-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-an-allocadia-test-user"></a>Het testgebruiker van een Allocadia maken

In dit gedeelte maakt u een gebruiker met de naam Britta Simon in Allocadia. Ondersteuning voor Allocadia toepassingen in de tijd van gebruikersaanvragen. Als u de vorderingen hebt geconfigureerd zoals hierboven in punt voor **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** zullen deze gebruikers in de toepassing inrichten. 


> [AZURE.NOTE] Als u een gebruiker handmatig maken of batch gebruikers, moet u contact opnemen met het ondersteuningsteam van Allocadia.


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

In deze sectie kunt inschakelen u Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan Allocadia.

![Gebruiker toewijzen][200] 

**Britta Simon aan Allocadia toewijzen, moet u de volgende stappen uitvoeren:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Allocadia**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

In dit gedeelte vindt u Azure AD één aanmelding configuratie testen met behulp van het Access-venster.
Wanneer u op de tegel Allocadia in het Access-venster, u moet krijgen automatisch ondertekend bij uw toepassing Allocadia.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_205.png
