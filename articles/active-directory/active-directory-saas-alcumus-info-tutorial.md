<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Exchange-Alcumus Info | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Exchange voor Alcumus Info."
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


# <a name="tutorial-azure-active-directory-integration-with-alcumus-info-exchange"></a>Zelfstudie: Azure Active Directory-integratie met Exchange-Alcumus Info

Het doel van deze zelfstudie is u tonen hoe Alcumus Info Exchange integreren met Azure Active Directory (AD Azure).  
Alcumus Info Exchange integreren met AD Azure biedt de volgende voordelen: 

- U kunt bepalen in Azure AD die toegang tot de informatie-uitwisseling Alcumus heeft 
- U kunt gebruikers automatisch ophalen ondertekend voor toegang tot Alcumus Info Exchange (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten 

Azure AD-integratie met Alcumus Info Exchange configureren, moet u de volgende items:

- Een abonnement op [Azure AD](https://azure.microsoft.com/)
- Een [Alcumus Info Exchange](http://www.alcumusgroup.com/) eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen. 

 
## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD testen in een testomgeving.  
Het scenario dat is beschreven in deze zelfstudie bestaat uit drie belangrijkste bouwstenen:

1. Alcumus Info Exchange toevoegen uit de galerie 
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-alcumus-info-exchange-from-the-gallery"></a>Alcumus Info Exchange toevoegen uit de galerie
De integratie van informatie-uitwisseling Alcumus in Azure AD configureren, moet u Alcumus Info Exchange uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Alcumus Info Exchange toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Alcumus Info Exchange**.

    ![Toepassingen][5]

7. **Alcumus Info Exchange**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Toepassingen][400]



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is u tonen hoe te configureren en eenmalige aanmelding Azure AD test met Alcumus informatie-uitwisseling op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in Alcumus Info Exchange aan een gebruiker in AD Azure. Met andere woorden, moet een relatie koppeling tussen een Azure AD-gebruiker en de gebruiker in Alcumus Info uitwisseling tot stand worden gebracht.  
Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in de Alcumus Info Exchange toewijzen.
 
Als u wilt configureren en testen Azure AD eenmalige aanmelding met Alcumus Info Exchange, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Maken van een informatie-uitwisseling Alcumus gebruiker testen](#creating-a-alcumus-info-exchange-test-user)** - een tegenhanger van Britta Simon in Alcumus Info Exchange die is gekoppeld aan de weergave Azure AD van haar hebben.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van deze sectie is Azure AD eenmalige aanmelding in de klassieke Azure portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Alcumus Info Exchange.

**Configureren van eenmalige aanmelding Azure AD met Alcumus Info Exchange, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, klik op de pagina **Alcumus Info Exchange** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren][6]

2. Selecteer **Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Alcumus Info Exchange** en klik op **volgende**.

    ![Azure AD Single Sign-On][7]

3. Voer de volgende stappen uit op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** : 

    ![Azure AD Single Sign-On][8]
 
    een. Typ in het tekstvak **URL antwoord** de consument URL setup voor u door uw ondersteuningsteam Alcumus Info Exchange is.

    > [AZURE.NOTE] Als u niet wat de juiste waarde is weet, neem dan contact op met het ondersteuningsteam Alcumus Info Exchange via [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com).

    b. Klik op **volgende**.
 
4. Klik op de pagina **configureren eenmalige aanmelding bij Alcumus Info Exchange** op **metagegevens downloaden**en sla het bestand lokaal op uw computer met metagegevens.

    ![Wat is Azure AD verbinden][9]

5. Neem contact op met het ondersteuningsteam Alcumus Info Exchange via [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com), het metagegevensbestand voorzien en hen laten weten dat deze eenmalige aanmelding voor u inschakelen mag.


6. Selecteer de bevestiging enkele aanmelding-configuratie op de Azure klassieke portal en klik op **volgende**. 

    ![Wat is Azure AD verbinden][10]

7. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  

    ![Wat is Azure AD verbinden][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
Het doel van deze sectie is voor het maken van een testgebruiker in Azure klassieke portal Britta Simon genoemd.  

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_02.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_03.png) 
 
4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**. 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren: 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.
  
    b. Typ **BrittaSimon**in de naam van het **tekstvak**.
  
    c. Klik op volgende.



6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** : 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_06.png) 
  

    een. Typ in het tekstvak **Voornaam** **Britta**.  
  
    b. In de **Achternaam** txtbox, type, **Simon**.
  
    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.
  
    d. Selecteer de **gebruiker**in de lijst **functie** .
  
    e. Klik op **volgende**.


7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_07.png) 
 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.
  
    b. Klik op **Voltooien**.   

  
 
### <a name="creating-a-alcumus-info-exchange-test-user"></a>Een gebruiker Alcumus Info Exchange test maken

Het doel van deze sectie is voor het maken van een gebruiker met de naam Britta Simon in Alcumus Info Exchange.

**Als een gebruiker met de naam Britta Simon in Alcumus Info Exchange hebt gemaakt, kunt u de volgende stappen uitvoeren:**

1. Neem contact op met het ondersteuningsteam Alcumus Info Exchange via [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com),


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

Het doel van deze sectie wordt aan het inschakelen van Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan Alcumus Info Exchange.

![Gebruiker toewijzen][200]

**Britta Simon met Alcumus Info Exchange toewijzen, moet u de volgende stappen uitvoeren:**

1. Op de portal Azure, het weergeven van toepassingen in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **Alcumus Info Exchange**.

    ![Gebruiker toewijzen][202]

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.  
Wanneer u op de tegel Alcumus Info Exchange in het Access-venster, u moet krijgen automatisch ondertekend bij uw toepassing Alcumus Info Exchange.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_01.png
[6]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_02.png
[7]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_03.png
[8]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_04.png
[9]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_05.png
[10]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_06.png
[11]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_07.png
[20]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_08.png
[203]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_205.png
[400]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_402.png