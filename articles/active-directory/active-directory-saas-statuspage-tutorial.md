<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met StatusPage | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en StatusPage."
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


# <a name="tutorial-azure-active-directory-integration-with-statuspage"></a>Zelfstudie: Azure Active Directory-integratie met StatusPage

Het doel van deze zelfstudie is u tonen hoe StatusPage integreren met Azure Active Directory (AD Azure).

StatusPage integratie met AD Azure biedt de volgende voordelen: 

- U kunt bepalen in Azure AD die toegang tot de StatusPage heeft 
- U kunt gebruikers automatisch ophalen ondertekend bij StatusPage (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten 

Azure AD-integratie configureren met StatusPage, moet u de volgende items:

- Een abonnement op Azure AD
- Een StatusPage eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen. 

 
## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD testen in een testomgeving. 

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. StatusPage uit de galerie toe te voegen. 
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-statuspage-from-the-gallery"></a>StatusPage uit de galerie toe te voegen.
De integratie van StatusPage in Azure AD configureren, moet u StatusPage uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Om de StatusPage van de galerie wilt toevoegen, moet u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.
 
    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ **StatusPage**in het zoekvak.
 
    ![Azure AD test gebruikers maken](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_01.png)

7. **StatusPage**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is u tonen hoe te configureren en eenmalige aanmelding Azure AD test met StatusPage op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in de StatusPage aan een gebruiker in AD Azure. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gebruiker in StatusPage worden vastgesteld.

Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** toewijzen in Azure AD als de waarde van de **gebruikersnaam** in het StatusPage.
 
Als u wilt configureren en testen Azure AD single sign-on met StatusPage, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Maken van een StatusPage gebruiker testen](#creating-a-statuspage-test-user)** - een tegenhanger van Britta Simon in StatusPage die is gekoppeld aan de weergave Azure AD van haar hebben.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van deze sectie is Azure AD eenmalige aanmelding in de klassieke Azure portal inschakelen en configureren van eenmalige aanmelding in uw toepassing StatusPage. 



**Configureren van eenmalige aanmelding Azure AD met StatusPage, voert u de volgende stappen uit:**

1. Klik in de klassieke Azure portal, klik op de pagina **StatusPage** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren][6] 

2. Selecteer **Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op StatusPage** en klik op **volgende**.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_03.png) 


3. Voer de volgende stappen uit op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** :
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_04.png) 

    > [AZURE.NOTE] Neem contact op met het ondersteuningsteam van StatusPage op [SupportTeam@statuspage.io](mailto:SupportTeam@statuspage.io)voor het aanvragen van metagegevens die nodig zijn voor het configureren van eenmalige aanmelding.


    een. Uit de metagegevens, de waarde van de uitgevende instelling kopiëren en vervolgens plakken in het tekstvak **id** .

    b. Kopieer de URL van het antwoord van de metagegevens, en plak deze in het tekstvak **URL antwoord** .

    c. Klik op **volgende**.
 
 
4. Klik op de pagina **configureren eenmalige aanmelding op StatusPage** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_05.png) 

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


1. In het venster van een andere webbrowser aanmelden bij uw site StatusPage bedrijf als beheerder.

1. Klik in de hoofdwerkbalk op **Account beheren**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_06.png) 


1. Klik op het tabblad **Single Sign-on** . 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_07.png) 


1. Voer de volgende stappen uit op de pagina instellingen voor eenmalige aanmelding:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_08.png) 

    een. In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding bij StatusPage** de **Single Sign-On Service URL** -waarde kopiëren en vervolgens plakken in het tekstvak **SSO-doel-URL** . 

    b. Uw gedownloade certificaat openen in Kladblok en kopieer de inhoud vervolgens plakken in het tekstvak van het **certificaat** . 

    c. Klik op **Opslaan**.


6. In de klassieke Azure portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**. 

    ![Azure AD Single Sign-On][10]

7. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
  
    ![Azure AD Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
Het doel van deze sectie is voor het maken van een testgebruiker in Azure klassieke portal Britta Simon genoemd.



![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-statuspage-tutorial/create_aaduser_09.png)  

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-statuspage-tutorial/create_aaduser_03.png) 
 
4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**. 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-statuspage-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren: 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-statuspage-tutorial/create_aaduser_05.png)  

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** : 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-statuspage-tutorial/create_aaduser_06.png) 
 
    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .
    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-statuspage-tutorial/create_aaduser_07.png) 
 
8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-statuspage-tutorial/create_aaduser_08.png) 
  
    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   

  
 
### <a name="creating-a-statuspage-test-user"></a>Maken van een gebruiker StatusPage test

Het doel van deze sectie is voor het maken van een gebruiker met de naam Britta Simon in StatusPage.
StatusPage ondersteunt just in time-aanbod. U hebt het al in [Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)ingeschakeld.


**Om een gebruiker met de naam Britta Simon in StatusPage maakt, moet u de volgende stappen uitvoeren:**

1. Aanmelding bij uw site StatusPage bedrijf als beheerder.

1. In het menu aan de bovenkant, klikt u op **Account beheren**.

1. Klik op het tabblad leden van het Team. 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_10.png) 

1. Klik op **toevoegen van teamleden**. 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_11.png) 

1. Typ het **E-mailadres**, **Voornaam** en **Achternaam** van een geldige gebruiker die u verrichten in de verwante tekstvakken wilt. 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_12.png) 

1. Kies **rol**, **Client Administrator**.

1. Klik op **Account maken**.

### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

Het doel van deze sectie wordt aan het inschakelen van Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan StatusPage.

![Gebruiker toewijzen][200] 

**Britta Simon aan StatusPage toewijzen, moet u de volgende stappen uitvoeren:**

1. Op de Azure klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **StatusPage**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Wanneer u op de tegel StatusPage in het Access-venster, u moet krijgen automatisch ondertekend bij uw toepassing StatusPage.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_205.png






