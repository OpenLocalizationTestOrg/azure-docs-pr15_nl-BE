<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met halogeen-Software"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en halogeen-Software."
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


# <a name="tutorial-azure-active-directory-integration-with-halogen-software"></a>Zelfstudie: Azure Active Directory-integratie met halogeen-Software

Het doel van deze zelfstudie is u tonen hoe halogeen-Software integreren met Azure Active Directory (AD Azure).

Halogeen-Software integreren met AD Azure biedt de volgende voordelen: 

- U kunt bepalen in Azure AD die toegang tot de Software halogeen heeft 
- U kunt gebruikers automatisch ophalen ondertekend bij halogeen-Software (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten 

Azure AD-integratie configureren met halogeen-Software, moet u de volgende items:

- Een abonnement op Azure AD
- Een halogeen Software eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen. 

 
## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD testen in een testomgeving. 

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Halogeen-Software uit de galerie toe te voegen. 
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-halogen-software-from-the-gallery"></a>Halogeen-Software uit de galerie toe te voegen.
De integratie van Software halogeen in Azure AD configureren, moet u halogeen Software uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Halogeen Software toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina. 

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **halogeen-software**.

    ![Toepassingen][5]

7. In het resultatendeelvenster **Halogeen Software**te selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is u tonen hoe te configureren en eenmalige aanmelding Azure AD test met halogeen-Software op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in halogeen Software aan een gebruiker in AD Azure. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gebruiker in halogeen-Software worden vastgesteld.

Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in halogeen-Software toe te wijzen.
 
Als u wilt configureren en testen Azure AD eenmalige aanmelding met halogeen-Software, moet u voor het voltooien van de volgende elementen:

1. **[Configureren AD Azure één Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Gebruikers maken een halogeen Software testen](#creating-a-halogen-software-test-user)** - hebben een tegenhanger van Britta Simon in halogeen-Software die is gekoppeld aan de weergave Azure AD van haar.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Azure AD enkele eenmalige aanmelding configureren

Het doel van deze sectie is Azure AD eenmalige aanmelding in de klassieke Azure portal inschakelen en configureren van eenmalige aanmelding in uw toepassing halogeen.


**Configureren van eenmalige aanmelding Azure AD met halogeen-Software, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, klik op de pagina **Halogeen Software** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren][8]

2. Op de pagina **Hoe wilt u dat gebruikers aanmelden bij halogeen Software** Selecteer **Azure AD Single Sign-On**en klik vervolgens op **volgende**.

    ![Azure AD Single Sign-On][9]

3. Klik op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** als volgt:  ![App-instellingen configureren][10]
 
     een. Typ in het tekstvak **Teken op URL** de URL die wordt gebruikt door de gebruikers aan te melden op uw halogeen toepassing met behulp van het volgende patroon: *https://global.hgncloud.com/fabrikam/welcome.jsp*

     b. Klik op **volgende**.
 
4. Klik op de pagina **configureren eenmalige aanmelding op halogeen-Software** op **metagegevens downloaden**en sla het bestand lokaal op uw computer met metagegevens.
    
    ![Wat is Azure AD verbinden][11]

5. In een andere browser-venster aanmelding bij uw toepassing **Halogeen** als beheerder.

6. Klik op het tabblad **Opties** . 

    ![Wat is Azure AD verbinden][12]


7. Klik in het linkernavigatievenster op **SAML-configuratie**. 

    ![Wat is Azure AD verbinden][13]

8. De volgende stappen uitvoeren op de pagina **Configuratie van SAML** :  ![wat is Azure AD verbinden][14]

    een. **De unieke id**, **NameID**selecteren.

    b. Selecteer de **gebruikersnaam**als **Unieke id gerelateerd aan**.

    c. Uw gedownloade om metagegevensbestand te uploaden, klikt u op **Bladeren** om het bestand te selecteren en vervolgens op **Bestand uploaden**.

    d. Als u wilt testen van de configuratie, klikt u op **Test uitvoeren**. 

    > [AZURE.NOTE] U hoeft te wachten voor het bericht "*de SAML-test is voltooid. Sluit dit venster*". Sluit het browservenster geopend. Het selectievakje **SAML inschakelen** is alleen beschikbaar als de test is voltooid.

    e. Selecteer **SAML inschakelen**.
    
    f. Klik op **wijzigingen opslaan**. 


9. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten. 

    ![Wat is Azure AD verbinden][15]

10. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  

    ![Wat is Azure AD verbinden][16]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
Het doel van deze sectie is voor het maken van een testgebruiker in Azure klassieke portal Britta Simon genoemd.

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Wat is Azure AD verbinden][100] 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Wat is Azure AD verbinden][101] 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**. 

    ![Wat is Azure AD verbinden][102] 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Wat is Azure AD verbinden][103] 
 
    een. Selecteer **Type van gebruiker**, **nieuwe gebruiker in uw organisatie**.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op volgende.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** : 

    ![Wat is Azure AD verbinden][104] 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Achternaam** txtbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Wat is Azure AD verbinden][105]  

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Wat is Azure AD verbinden][106]   

    een. Noteer de waarde van het **Nieuwe wachtwoord**.
    b. Klik op **Voltooien**.   
  
 
### <a name="creating-a-halogen-software-test-user"></a>Maken van een gebruiker halogeen Software testen

Het doel van deze sectie is voor het maken van een gebruiker met de naam Britta Simon in halogeen-Software.

**Als een gebruiker met de naam Britta Simon in halogeen-Software maakt, moet u de volgende stappen uitvoeren:**

1. Aanmelden met uw toepassing **Halogeen** als beheerder.

2. Klik op het tabblad **Gebruiker Center** en klik op **Gebruiker maken**.

    ![Wat is Azure AD verbinden][300]  

3. Klik op de pagina van het dialoogvenster **Nieuwe gebruiker** de volgende stappen uitvoeren:

    ![Wat is Azure AD verbinden][301]

    een. Typ in het tekstvak **Voornaam** **Britta**. 
  
    b. Typ in het tekstvak **Achternaam** **Simon**.
  
    c. Typ in het tekstvak **gebruikersnaam** **Brita Simon de gebruikersnaam in de klassieke Azure portal**.
  
    d. Typ een wachtwoord in het tekstvak **wachtwoord** voor Britta.
  
    e. Klik op **Opslaan**.


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

Het doel van deze sectie wordt aan het inschakelen van Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan halogeen-Software.

![Wat is Azure AD verbinden][200]

**Britta Simon halogeen software toewijzen, moet u de volgende stappen uitvoeren:**

1. Op de Azure klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Wat is Azure AD verbinden][201]

2. Selecteer in de lijst met toepassingen, **Halogeen-Software**.

    ![Wat is Azure AD verbinden][202]

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Wat is Azure AD verbinden][203]

1. Selecteer in de lijst gebruikers **Britta Simon**.

    ![Wat is Azure AD verbinden][204]

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Wat is Azure AD verbinden][205]



### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Wanneer u op de tegel halogeen-Software in het Configuratiescherm toegang, u moet krijgen automatisch ondertekend bij uw toepassing halogeen.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_01.png
[2]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_02.png
[3]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_03.png
[4]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_04.png
[5]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_05.png
[6]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_06.png
[7]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_07.png
[8]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_08.png
[9]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_09.png
[10]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_10.png
[11]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_11.png
[12]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_12.png
[13]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_13.png
[14]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_14.png
[15]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_15.png
[16]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_16.png
[100]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_100.png 
[101]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_101.png 
[102]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_102.png 
[103]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_103.png 
[104]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_104.png 
[105]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_105.png 
[106]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_106.png 
[200]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_200.png 
[201]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_201.png 
[202]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_202.png
[203]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_203.png
[204]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_204.png
[205]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_205.png
[300]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_300.png
[301]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_301.png