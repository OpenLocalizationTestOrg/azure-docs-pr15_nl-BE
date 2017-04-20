<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met name | Microsoft Azure"
    description="Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en namelijk."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="prasannas"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-namely"></a>Zelfstudie: Azure Active Directory-integratie met namelijk

Het doel van deze zelfstudie is aan hoe u namelijk integreren met Azure Active Directory (AD Azure).

Namelijk integreren met AD Azure biedt de volgende voordelen: 

- U kunt bepalen in Azure AD die toegang heeft tot namelijk 
- U kunt uw gebruikers automatisch aangemeld bij namelijk inschakelen (Single Sign-On) met hun accounts Azure AD
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten 

Configureren van Azure AD integratie, met name als u het volgende nodig:

- Een abonnement op Azure AD
- Een namelijk eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen. 

 
## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD testen in een testomgeving. 

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Namelijk uit de galerie toe te voegen 
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-namely-from-the-gallery"></a>Namelijk uit de galerie toe te voegen
De integratie van namelijk in Azure AD configureren, moet u namelijk uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt namelijk toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak, **namelijk**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-namely-tutorial/tutorial_namely_01.png)

7. In het resultatendeelvenster, **namelijk**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-namely-tutorial/tutorial_namely_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is aan hoe u configureren en testen Azure AD single sign-on met namelijk op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in namelijk aan een gebruiker in AD Azure. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de bijbehorende gebruiker in namelijk worden vastgesteld.

Deze relatie koppeling wordt vastgesteld door het toewijzen van de waarde van de **gebruikersnaam** namelijk in Azure AD als de waarde van de **gebruikersnaam** in.
 
Als u wilt configureren en testen van Azure AD moet single sign-on met wil zeggen, u uitvoeren de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Maken van een gebruiker namelijk testen](#creating-a-namely-test-user)** - hebben een tegenhanger van Britta Simon in namelijk die is gekoppeld aan de weergave Azure AD van haar.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van deze sectie is Azure AD eenmalige aanmelding in de klassieke Azure portal inschakelen en configureren van eenmalige aanmelding in uw toepassing namelijk. 




**Namelijk configureren Azure AD single sign-on met de volgende stappen uitvoeren:**

1. In de Azure klassieke portal op de **namelijk** application Integratiepagina, klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren][6] 

2. Selecteer **Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers namelijk aanmelden bij** en klik op **volgende**.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-namely-tutorial/tutorial_namely_03.png) 

3. Klik op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** als volgt:.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-namely-tutorial/tutorial_namely_04.png) 

    een. Typ in het tekstvak **Teken op URL** de URL die wordt gebruikt door de gebruikers aan te melden op uw toepassing namelijk (bijvoorbeeld: *https://fabrikam.Namely.com/*).

    b. Klik op **volgende**.
 
 
4. Voer de volgende stappen uit op de pagina **namelijk eenmalige aanmelding te configureren** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-namely-tutorial/tutorial_namely_05.png) 

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


1. In het venster van een andere webbrowser aanmelden bij uw namelijk bedrijf site als beheerder.

1. In de werkbalk op de bovenkant, klikt u op het **bedrijf**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-namely-tutorial/tutorial_namely_06.png) 

1. Klik op het tabblad **Instellingen** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-namely-tutorial/tutorial_namely_07.png) 


1. Klik op **SAML**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-namely-tutorial/tutorial_namely_08.png) 


1. Op de pagina **Instellingen van SAML** , de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-namely-tutorial/tutorial_namely_09.png) 

    een. Klik op **SAML inschakelen**. 

    b. De **Single Sign-On Service URL** -waarde kopiëren en vervolgens plakken in het tekstvak **url van Identity provider DDO** in de Azure klassieke portal op de pagina dialoogvenster **namelijk eenmalige aanmelding te configureren** . 

    c. Uw gedownloade certificaat openen in Kladblok, Kopieer de inhoud en plak deze in de textbox **Identity provider certificaat** .    

    d. Klik op **Opslaan**.


6. In de klassieke Azure portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**. 

    ![Azure AD Single Sign-On][10]

7. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  

    ![Azure AD Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
Het doel van deze sectie is voor het maken van een testgebruiker in Azure klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]


**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-namely-tutorial/create_aaduser_09.png)  

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-namely-tutorial/create_aaduser_03.png) 
 
4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**. 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-namely-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren: 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-namely-tutorial/create_aaduser_05.png)  

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** : 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-namely-tutorial/create_aaduser_06.png) 
 
    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .
    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-namely-tutorial/create_aaduser_07.png) 
 
8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-namely-tutorial/create_aaduser_08.png) 
  
    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   

  
 
### <a name="creating-a-namely-test-user"></a>Maken van een gebruiker namelijk testen

Het doel van deze sectie is een Britta Simon namelijk genoemd gebruiker maken.

**U maakt namelijk Britta Simon in de naam van een gebruiker, moet u de volgende stappen uitvoeren:**

1. Aanmelding bij uw namelijk bedrijf site als beheerder.

1. Klik in de werkbalk op de bovenkant, **mensen**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-namely-tutorial/tutorial_namely_10.png) 

1. Klik op het tabblad **map** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-namely-tutorial/tutorial_namely_11.png) 

1. Klik op **nieuwe persoon toevoegen**.



1. Voer de volgende stappen uit in het dialoogvenster **Nieuwe persoon toevoegen** :

    een. Typ in het tekstvak **Voornaam** **Britta**.

    b. Typ in het tekstvak **Achternaam** **Simon**.

    c. Typ in het tekstvak **e-mailadres** van Britta e-mailadres in de klassieke Azure portal.

    d. Klik op **Opslaan**.





### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

Het doel van deze sectie is Britta Simon door het verlenen van toegang tot de computer namelijk gebruik van Azure eenmalige aanmelding inschakelen.

![Gebruiker toewijzen][200] 

**Voor het toewijzen van Britta Simon namelijk de volgende stappen uitvoeren:**

1. Op de Azure klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **namelijk**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-namely-tutorial/tutorial_namely_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Wanneer u klikt op de namelijk naast elkaar in het Access-venster, u moet krijgen automatisch ondertekend bij uw toepassing namelijk.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-namely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-namely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-namely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-namely-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-namely-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-namely-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-namely-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-namely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-namely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-namely-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-namely-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-namely-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-namely-tutorial/tutorial_general_205.png






