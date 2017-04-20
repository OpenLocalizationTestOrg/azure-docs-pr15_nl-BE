<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Keylight | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Keylight."
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


# <a name="tutorial-azure-active-directory-integration-with-keylight"></a>Zelfstudie: Azure Active Directory-integratie met Keylight

In deze zelfstudie leert u hoe Keylight integreren met Azure Active Directory (AD Azure).

Keylight integratie met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot de Keylight heeft
- U kunt gebruikers automatisch ophalen ondertekend bij Keylight (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met Keylight, moet u de volgende items:

- Een abonnement op Azure
- Een Keylight eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. 

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Keylight uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-keylight-from-the-gallery"></a>Keylight uit de galerie toe te voegen.
De integratie van Keylight in Azure AD configureren, moet u Keylight uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Om de Keylight van de galerie wilt toevoegen, moet u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ **Keylight**in het zoekvak.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_01.png)

7. **Keylight**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Keylight op basis van een testgebruiker met de naam "Simon Britta".

Als u wilt configureren en testen Azure AD single sign-on met Keylight, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Maken van een Keylight gebruiker testen](#creating-a-keylight-test-user)** - een tegenhanger van Britta Simon in Keylight die is gekoppeld aan de weergave Azure AD van haar hebben.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

In deze sectie, Azure AD eenmalige aanmelding in de klassieke Azure portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Keylight.


**Configureren van eenmalige aanmelding Azure AD met Keylight, voert u de volgende stappen uit:**

1. Klik in de klassieke Azure portal, klik op de pagina **Keylight** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren][6] 


2. Selecteer **Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Keylight** en klik op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_03.png) 

3. Voer de volgende stappen uit op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** :
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_04.png) 


    een. In het teken op URL-tekstvak typt u de URL die wordt gebruikt door gebruikers voor aanmelding bij uw toepassing Keylight is met het volgende patroon: **"https://\<bedrijf\>.keylightgrc.com/Login.aspx?saml=1 '**.


4. Klik op de pagina **configureren eenmalige aanmelding op Keylight** de volgende stappen uitvoeren:
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_05.png) 

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


5. Als u eenmalige aanmelding in Keylight, moet u de volgende stappen uitvoeren:
 
    een. Aanmelding bij uw account Keylight als beheerder.

    b. In het menu aan de bovenkant, klikt u op de **persoon**en **Keylight de instellingen**selecteren.
       
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-keylight-tutorial/401.png) 

    c. Klik in de structuurweergave links **SAML**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-keylight-tutorial/402.png) 

    d. Klik op **bewerken**in het dialoogvenster **Instellingen van SAML** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-keylight-tutorial/404.png) 
  

5. Voer de volgende stappen uit op de pagina van het dialoogvenster **SAML-instellingen bewerken** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-keylight-tutorial/405.png) 

    een. **SAML-verificatie** ingesteld op **actief**.


    b. In Azure AD klassieke portal, de waarde van **SAML SSO-URL** kopiëren en plak deze in de textbox **Identity Provider aanmeldings-URL** .

    c. Kopieer de **URL van de Service Single Sign-Out** waarde in Azure AD klassieke portal en plak deze in de **Identity Provider Logout URL** textbox.

    d. Klik op **Bestand kiezen** om uw gedownloade Keylight certificaat te selecteren en klik op **openen** om het certificaat te uploaden.


    e. **Gebruikersnaam SAML locatie** instellen op **NameIdentifier-element van het onderwerp Overzicht**.
   
    f. Geef de **Keylight Service Provider met behulp van het volgende patroon: **https://&lt;bedrijfsnaam&gt;. keylightgrc.com**.

    g. **Automatische bepaling gebruikers** ingesteld op **actief**.

    h. **Automatische bepaling rekeningsoort** ingesteld op **Volledige gebruiker**.

    ik. **Standaardgebruiker met SAML** **beveiligingsrol automatische voorziening**selecteren.
   
    j. Als **automatische voorziening beveiliging config**, **Standaardconfiguratie van de gebruiker**te selecteren.
   
    k. Typ in het tekstvak e-mailadres-kenmerk **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    l. Typ in het tekstvak **Voornaam kenmerk** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    m. Typ in het tekstvak **laatste naamkenmerk** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.

    n. Klik op **Opslaan**.
   
  
   
  
6. In de klassieke Azure portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.

    ![Azure AD Single Sign-On][10]

7. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  

    ![Azure AD Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
In dit gedeelte maakt u een testgebruiker in Azure klassieke portal Britta Simon genoemd.

Selecteer in de lijst gebruikers **Britta Simon**.

![Azure AD-gebruiker maken][20]



**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke Portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-keylight-tutorial/create_aaduser_09.png) 


2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-keylight-tutorial/create_aaduser_03.png) 


4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-keylight-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-keylight-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :

    ![Azure AD test gebruikers maken](./media/active-directory-saas-keylight-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-keylight-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-keylight-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-a-keylight-test-user"></a>Maken van een gebruiker Keylight test

In dit gedeelte maakt u een gebruiker met de naam Britta Simon in Keylight. Keylight biedt ondersteuning voor just-in-time provisioning, die standaard is ingeschakeld.

Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker wordt gemaakt wanneer toegang tot Keylight als de gebruiker nog niet bestaat. 

> [AZURE.NOTE] Als u een gebruiker handmatig te maken, moet u contact opnemen met het ondersteuningsteam van Keylight.


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

In deze sectie kunt inschakelen u Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan Keylight.

![Gebruiker toewijzen][200] 

**Britta Simon aan Keylight toewijzen, moet u de volgende stappen uitvoeren:**

1. Op de Azure klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Keylight**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

In dit gedeelte vindt u Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Wanneer u op de tegel Keylight in het Access-venster, u moet krijgen automatisch ondertekend bij uw toepassing Keylight.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_205.png
