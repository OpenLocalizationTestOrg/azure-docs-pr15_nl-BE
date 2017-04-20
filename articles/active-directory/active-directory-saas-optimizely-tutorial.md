<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Optimizely | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Optimizely."
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
    ms.date="09/11/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-optimizely"></a>Zelfstudie: Azure Active Directory-integratie met Optimizely

In deze zelfstudie leert u hoe Optimizely integreren met Azure Active Directory (AD Azure).

Optimizely integreren met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot Optimizely heeft
- U kunt gebruikers automatisch ophalen ondertekend bij Optimizely (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met Optimizely, moet u de volgende items:

- Een abonnement op Azure AD
- Een **Optimizely** eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Optimizely uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-optimizely-from-the-gallery"></a>Optimizely uit de galerie toe te voegen.
De integratie van Optimizely in Azure AD configureren, moet u Optimizely uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Optimizely toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Optimizely**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_01.png)

7. In het resultatendeelvenster **Optimizely**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Optimizely op basis van een testgebruiker "Simon Britta" genoemd.

Voor eenmalige aanmelding wilt werken, moet AD Azure weet wat de gebruiker tegenhanger in Optimizely Azure advertentie aan een gebruiker is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de bijbehorende gebruiker in Optimizely worden vastgesteld.
Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in het Optimizely toewijzen.

Als u wilt configureren en testen Azure AD single sign-on met Optimizely, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Gebruikers maken een Optimizely testen](#creating-an-optimizely-test-user)** - hebben een tegenhanger van Britta Simon in Optimizely die is gekoppeld aan de weergave Azure AD van haar.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van deze sectie is Azure AD eenmalige aanmelding in de klassieke Azure portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Optimizely.

Toepassing Optimizely verwacht de SAML-assertions bevatten een kenmerk met de naam "email". De waarde van "email" moet een e-mail Optimizely herkend die door AD Azure kan krijgen geverifieerd. Configureer de claim "email" voor deze toepassing. De waarden van deze kenmerken kunt u beheren via het tabblad **'Atrributes'** van de toepassing. De volgende schermafbeelding ziet u een voorbeeld voor dit. 


![Eenmalige aanmelding configureren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_03.png) 


**Configureren van eenmalige aanmelding Azure AD met Optimizely, voert u de volgende stappen uit:**

1. In de klassieke Azure portal, op de pagina **Optimizely** toepassing integratie, in het menu aan de bovenkant, klikt u op **kenmerken**.
     
    ![Eenmalige aanmelding configureren][5]

2. Toevoegen in het dialoogvenster SAML-token kenmerken het kenmerk "email".

    een. Klik op **gebruikerskenmerk toevoegen** om de **Gebruikerskenmerk toevoegen** dialoogvenster te openen. 
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_05.png)

    b. Typ in het tekstvak **Naam van kenmerk** kenmerk naam "email".

    c. Selecteer in de lijst **Waarde van het kenmerk** userprincipalname' attribuut waarde"of een waarde met een e-mailbericht dat wordt herkend door AD Azure en Optimizely.

    d. Klik op **Voltooien**.
3. In het menu aan de bovenkant, klikt u op de **Werkbalk Snel starten**.

    ![Eenmalige aanmelding configureren][6]
4. Klik in de klassieke portal op de pagina **Optimizely** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren][7] 

5. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Optimizely** **Azure AD Single Sign-On**selecteren en klik op **volgende**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_06.png)

6. Voer de volgende stappen uit op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** : 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_07.png)


    een. Typ in het tekstvak **Teken op URL** :`https://app.optimizely.net/contoso`

    b. Typ in het tekstvak **id** :`urn:auth0:optimizely:contoso`

    c. Klik op **volgende**. 


    > [AZURE.NOTE] De waarden voor het **Aanmelden op de URL** en de **id** zijn tijdelijke aanduidingen voor de werkelijke waarden. U vindt instructies voor aquiring de werkelijke waarden van Optimizely verderop in deze handleiding.

7. Op de pagina **configureren eenmalige aanmelding bij Optimizely** voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_08.png)

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer.

    b. Kopieer de **URL van de Service voor eenmalige aanmelding**.

8. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met uw accountmanager Optimizely en geef de volgende informatie:

    - Uw gedownloade certificaat 
    - De URL van de Service voor eenmalige aanmelding
 
    In reactie op uw e-mailadres biedt Optimizely u teken op URL (SSO SP gestart) en de waarden van de id (Service Provider entiteit-ID).

9. Ga terug naar de **App instellingen configureren** dialoogvenster pagina en voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_07.png)

    een. Typ in het tekstvak **Teken op URL** **SP geïnitieerde SSO-URL** die door Optimizely.

    b. Typ in het tekstvak **id** **Service Provider entiteits-ID** verstrekt door Optimizely.

    c. Klik op **volgende**.

10. Op de pagina **configureren eenmalige aanmelding bij Optimizely** voert u de volgende stappen uit:
    
    ![Azure AD Single Sign-On][10]

    een. Selecteer de Configuratiebevestiging van één aanmelding.

    b. Klik op **volgende**.

11. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
    
    ![Azure AD Single Sign-On][11]

12. In een andere browser-venster aanmelding bij uw toepassing Optimizely.
13. Klik op de accountnaam in de rechterbovenhoek en klik op **Accountinstellingen**.

    ![Azure AD Single Sign-On](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_09.png)

14. Schakel het selectievakje **Eenmalige aanmelding inschakelen** onder Single Sign On in de sectie **Overzicht** op het tabblad Account.

    ![Azure AD Single Sign-On](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_10.png)

### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.
Selecteer in de lijst gebruikers **Britta Simon**.

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-optimizely-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-optimizely-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-optimizely-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:
 
    ![Azure AD test gebruikers maken](./media/active-directory-saas-optimizely-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :

    ![Azure AD test gebruikers maken](./media/active-directory-saas-optimizely-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-optimizely-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-optimizely-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-an-optimizely-test-user"></a>Een gebruiker Optimizely test maken

In dit gedeelte maakt u een gebruiker met de naam Britta Simon in Optimizely.

1. Selecteer tabblad van **uw collega's** op de introductiepagina
2. Klik op **Nieuwe collega** om een nieuwe collega toevoegen aan het project.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-optimizely-tutorial/create_aaduser_10.png)

3.  Vul het e-mailadres en een rol toewijzen. Klik op **uitnodigen**.


    ![Azure AD test gebruikers maken](./media/active-directory-saas-optimizely-tutorial/create_aaduser_11.png)

4. Ze ontvangt een e-mailadres uitnodigen. Met behulp van het e-mailadres. zij moeten zich aanmelden bij Optimizely.


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

In deze sectie kunt inschakelen u Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan Optimizely.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Optimizely, voert u de volgende stappen uit:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Optimizely**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst alle gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Wanneer u op de tegel Optimizely in het Access-venster, u moet krijgen automatisch ondertekend bij uw toepassing Optimizely.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-optimizely-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_205.png
