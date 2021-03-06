<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Schoolbord leren | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Schoolbord leren."
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


# <a name="tutorial-azure-active-directory-integration-with-blackboard-learn"></a>Zelfstudie: Azure Active Directory-integratie met Schoolbord leren

In deze zelfstudie leert u hoe meer Schoolbord integreren met Azure Active Directory (AD Azure).

Schoolbord meer integreren met Azure Active Directory biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot informatie Schoolbord heeft
- U kunt gebruikers automatisch ophalen ondertekend bij Schoolbord leren (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met Schoolbord leren, moet u de volgende items:

- Een abonnement op Azure AD
- Een Schoolbord meer Cloud Platform eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving.

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Schoolbord informatie toevoegen uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-blackboard-learn-from-the-gallery"></a>Schoolbord informatie toevoegen uit de galerie
De integratie van meer Schoolbord in Azure AD configureren, moet u Schoolbord informatie toevoegen uit de galerie aan de lijst met beheerde SaaS-apps.

**Om toe te voegen Schoolbord informatie uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Active Directory][1]
2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. In het zoekvak typt **Schoolbord leren**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_01.png)

7. In het resultatendeelvenster **Schoolbord informatie**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_06.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Schoolbord leren op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weet wat de gebruiker tegenhanger in Schoolbord informatie Azure advertentie aan een gebruiker is. Met andere woorden, dient een koppeling relatie tussen een Azure AD-gebruiker en de gebruiker in het Schoolbord informatie te worden voorzien.

Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in het Schoolbord meer toewijzen.

Als u wilt configureren en testen Azure AD single sign-on met Schoolbord leren, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
3. **[Gebruikers maken een Schoolbord meer testen](#creating-a-blackboard-learn-test-user)** - een tegenhanger van Britta Simon in Schoolbord informatie die is gekoppeld aan de weergave Azure AD van haar hebben.
4. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

In deze sectie, Azure AD eenmalige aanmelding in de klassieke portal inschakelen en eenmalige aanmelding configureren in uw toepassing Schoolbord leren.

Het SAML-assertions verwacht Schoolbord meer toepassing in een specifieke indeling. Configureer de volgende claims voor deze toepassing. De waarden van deze kenmerken kunt u beheren via het tabblad **'Atrribute'** van de toepassing. De volgende schermafbeelding ziet u een voorbeeld voor dit. 

![Eenmalige aanmelding configureren](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_51.png) 

**Configureren van eenmalige aanmelding Azure AD met Schoolbord leren, kunt u de volgende stappen uitvoeren:**


1. In de klassieke Azure portal, op de pagina toepassing integratie **Schoolbord informatie** in het menu aan de bovenkant, klikt u op **kenmerken**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_80.png) 


1. In het dialoogvenster **SAML-token kenmerken** voor elke rij die wordt weergegeven in de onderstaande tabel, de volgende stappen uitvoeren: We hebben de Userprincipalname toewijzen als het unieke gebruiker-kenmerk, maar kunt u deze toewijzen aan de juiste waarde die de gebruiker in de organisatie een unieke onderscheiden en dat wordt toegewezen aan een Schoolbord veld username leren.

  	| Naam van kenmerk | Waarde van het kenmerk |
  	| --- | --- |    
  	| urn:oid:1.3.6.1.4.1.5923.1.1.1.6  | User.userPrincipalName |
   
 
    een. Klik op **gebruikerskenmerk toevoegen** om te openen, het dialoogvenster **Gebruiker-Attribure toevoegen** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_81.png) 


    b. Typ de naam van het kenmerk voor de rij weergegeven in het tekstvak **Naam Attrubute** .

    c. In de lijst **Waarde van het kenmerk** selsect de waarde van het kenmerk voor de rij weergegeven.

    d. Klik op **Voltooien**.  

2. Klik in de klassieke portal op de pagina toepassing integratie **Schoolbord informatie over** **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.
     
    ![Eenmalige aanmelding configureren][6] 

3. Op de pagina **Hoe wilt u dat gebruikers aanmelden bij Schoolbord informatie** **Azure AD Single Sign-On**selecteren en klik op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_03.png) 

4. Voer de volgende stappen uit op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_04.png) 

    een. In het **Teken op URL** -tekstvak typt u de URL die wordt gebruikt door gebruikers voor aanmelding bij uw toepassing Schoolbord leren met behulp van het volgende patroon: **https://\<bedrijf naam prijzen\>.blackboard.com/**
    
    b. Klik op **volgende**
 
5. Op de pagina **configureren eenmalige aanmelding op een Schoolbord informatie over** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_05.png)

    een. Klik op **metagegevens downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


6. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met het ondersteuningsteam Schoolbord leren en geeft u het volgende:

    • De gedownloade metagegevens


7. In de klassieke portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.
    
    ![Azure AD Single Sign-On][10]

8. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
 
    ![Azure AD Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.


![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster als volgt:  ![Azure AD test gebruikers maken](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Klik op de pagina van het dialoogvenster **Gebruikersprofiel** als volgt: ![Azure AD test gebruikers maken](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-an-blackboard-learn-test-user"></a>Een testgebruiker Schoolbord leren maken

In deze sectie kunt maken u een gebruiker met de naam Britta Simon in Schoolbord leren. 

Schoolbord meer toepassing ondersteunt alleen in de tijd van gebruikersaanvragen. Zorg ervoor dat u de vorderingen hebt geconfigureerd zoals beschreven in de sectie ** [Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)**

### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

In dit gedeelte vindt inschakelen u Britta Simon voor de Azure eenmalige aanmelding toegang verlenen voor Schoolbord meer.

![Gebruiker toewijzen][200] 

**Britta Simon Schoolbord informatie toewijzen, moet u de volgende stappen uitvoeren:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer **Meer Schoolbord**in de lijst toepassingen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_50.png) 

3. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

In dit gedeelte vindt u Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Schoolbord meer ondersteuning voor toepassingen wanneer u klikt op de tegel Schoolbord informatie in het Access-venster, ontvangt u automatisch ondertekend op voor uw toepassing meer Schoolbord.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_205.png
