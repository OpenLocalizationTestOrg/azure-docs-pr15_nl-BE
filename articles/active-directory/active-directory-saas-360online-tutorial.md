<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met 360° Online | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en 360° Online."
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


# <a name="tutorial-azure-active-directory-integration-with-360-online"></a>Zelfstudie: Azure Active Directory-integratie met Online 360°


Het doel van deze zelfstudie is u tonen hoe Online 360° integreren met Azure Active Directory (AD Azure).

Integratie van 360° biedt Online met Azure Active Directory u de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot 360° Online-Server heeft
- U kunt gebruikers automatisch ophalen ondertekend voor toegang tot 360° Online (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met 360° Online, moet u de volgende items:

- Een abonnement op Azure AD
- Een Online huurder 360°


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD testen in een testomgeving. 

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Online 360° uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding



## <a name="adding-360-online-from-the-gallery"></a>Online 360° uit de galerie toe te voegen.
De integratie van Online 360° in Azure AD configureren, moet u Online 360° uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.


**Online 360° toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Online 360 °**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-360online-tutorial/tutorial_360online_01.png)

7. Selecteer **Online 360 °**in het resultatenvenster en klik op **Voltooien** als de toepassing wilt toevoegen.
 
    ![Toepassingen](./media/active-directory-saas-360online-tutorial/tutorial_360online_06.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is u tonen hoe te configureren en te testen Azure AD single sign-on met 360° Online op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure welke gebruiker tegenhanger in 360° Online aan een gebruiker in AD Azure is bekend. Met andere woorden, een koppeling relatie tussen een Azure AD-gebruiker en de bijbehorende gebruiker in 360° on line dient te worden voorzien.


Als u wilt configureren en testen Azure AD single sign-on met 360° Online, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Maken van een 360 ° Online gebruiker testen](#creating-a-360-online-test-user)** - een tegenhanger van Britta Simon in 360 ° Online die is gekoppeld aan de weergave Azure AD van haar hebben.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.


### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van deze sectie is Azure AD eenmalige aanmelding in de klassieke Azure portal inschakelen en configureren op in de 360° Online toepassing.

**Configureren van eenmalige aanmelding Azure AD met 360° Online, moet u de volgende stappen uitvoeren:**


1. De Azure klassieke portal op de pagina **on line 360 °** toepassing integratie Klik op **eenmalige aanmelding configureren** voor het openen van het dialoogvenster **Configureren van eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren][13] 

2. Op de pagina **Hoe wilt u dat gebruikers zich aanmelden bij de on line 360 °** Selecteer **Azure AD Single Sign-On**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-360online-tutorial/tutorial_360online_03.png) 

3. Voer de volgende stappen uit op de pagina van het dialoogvenster **App URL configureren** en klik op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-360online-tutorial/tutorial_360online_04.png)

    een. Typ in het tekstvak **Teken op URL** de URL die wordt gebruikt door uw gebruikers om aanmelding bij uw 360 ° Online-toepassing met behulp van het volgende patroon:`https://<company name>.public360online.com`

    b. Klik op **volgende**

4. Voer de volgende stappen uit op de pagina van het dialoogvenster **App URL configureren** en klik op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-360online-tutorial/tutorial_360online_05.png) 

    een. Klik op **metagegevens downloaden**en sla het op uw computer.

    b. Klik op **volgende**.


5. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met uw 360° Online support team via [360online@software-innovation.com](mailto:360online@software-innovation.com) en de van het gedownloade metagegevensbestand te koppelen aan uw e-mailberichten.

6. In de klassieke Azure portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.

    ![Azure AD Single Sign-On][10]

7. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  

    ![Azure AD Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
Het doel van deze sectie is voor het maken van een testgebruiker in Azure klassieke portal Britta Simon genoemd.


![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-360online-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
 
    ![Azure AD test gebruikers maken](./media/active-directory-saas-360online-tutorial/create_aaduser_03.png) 


4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-360online-tutorial/create_aaduser_04.png)

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-360online-tutorial/create_aaduser_05.png) 

    een. Selecteer **Type van gebruiker**, **nieuwe gebruiker in uw organisatie**.

    b. Typ in het tekstvak **Gebruikersnaam** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :

    ![Azure AD test gebruikers maken](./media/active-directory-saas-360online-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-360online-tutorial/create_aaduser_07.png) 


8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:
 
    ![Azure AD test gebruikers maken](./media/active-directory-saas-360online-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   




### <a name="creating-a-360-online-test-user"></a>Een Online test 360° gebruiker maken

Het doel van deze sectie is voor het maken van een gebruiker met de naam Britta Simon in Online 360°. 

Als u een gebruiker in 360° Online gemaakt, moet u contact opnemen met uw 360° Online support team via [360online@software-innovation.com](mailto:360online@software-innovation.com).


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

Het doel van deze sectie is Britta Simon gebruik van door haar toegang verlenen tot 360° Online Azure eenmalige aanmelding inschakelen.

![Gebruiker toewijzen][200] 

**Britta Simon en 360° Online toewijzen, moet u de volgende stappen uitvoeren:**

1. Op de Azure klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.
 
    ![Gebruiker toewijzen][201] 


2. Selecteer in de lijst toepassingen **Online 360 °**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-360online-tutorial/tutorial_360online_50.png) 


1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Wanneer u op de 360° Online tegel in het Access-venster, u moet krijgen automatisch ondertekend bij de 360° Online toepassing.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)







<!--Image references-->

[1]: ./media/active-directory-saas-360online-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-360online-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-360online-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-360online-tutorial/tutorial_general_04.png

[10]: ./media/active-directory-saas-360online-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-360online-tutorial/tutorial_general_07.png
[12]: ./media/active-directory-saas-360online-tutorial/tutorial_general_08.png
[13]: ./media/active-directory-saas-360online-tutorial/tutorial_general_09.png
[20]: ./media/active-directory-saas-360online-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-360online-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-360online-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-360online-tutorial/tutorial_general_203.png
[205]: ./media/active-directory-saas-360online-tutorial/tutorial_general_205.png
