<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Asana | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Asana."
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
    ms.date="10/24/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-asana"></a>Zelfstudie: Azure Active Directory-integratie met Asana

In deze zelfstudie leert u hoe Asana integreren met Azure Active Directory (AD Azure).

Asana integreren met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot de Asana heeft
- U kunt gebruikers automatisch ophalen ondertekend bij Asana (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met Asana, moet u de volgende items:

- Een abonnement op Azure AD
- Een **Asana** eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Asana uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-asana-from-the-gallery"></a>Asana uit de galerie toe te voegen.
De integratie van Asana in Azure AD configureren, moet u Asana uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Asana toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Asana**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-asana-tutorial/tutorial_asana_01.png)

7. **Asana**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-asana-tutorial/tutorial_asana_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Asana op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in de Asana aan een gebruiker in AD Azure. Met andere woorden, moet een relatie van de koppeling tussen een Azure AD-gebruiker en de gebruiker in de Asana worden vastgesteld.
Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in de Asana toewijzen.

Als u wilt configureren en testen Azure AD single sign-on met Asana, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Maken van een Asana gebruiker testen](#creating-an-Asana-test-user)** - een tegenhanger van Britta Simon in Asana die is gekoppeld aan de weergave Azure AD van haar hebben.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD configureren van eenmalige aanmelding

Het doel van deze sectie is Azure AD eenmalige aanmelding in de klassieke Azure portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Asana.

**Configureren van eenmalige aanmelding Azure AD met Asana, voert u de volgende stappen uit:**

1. In het menu aan de bovenkant, klikt u op de **Werkbalk Snel starten**.

    ![Eenmalige aanmelding configureren][6]
2. Klik in de klassieke portal op de pagina **Asana** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren][7] 

3. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Asana** **Azure AD Single Sign-On**selecteren en klik op **volgende**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-asana-tutorial/tutorial_asana_06.png)

4. Voer de volgende stappen uit op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** : 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-asana-tutorial/tutorial_asana_07.png)


    een. Typ in het tekstvak teken in de URL een URL met het volgende patroon:`https://app.asana.com`

    c. Klik op **volgende**.

5. Klik op de pagina **configureren eenmalige aanmelding op Asana** **-certificaat downloaden**en sla het bestand op uw computer. Ook de waarde van SAML SSO-URL kopiëren.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-asana-tutorial/tutorial_asana_08.png)

8. Klik met de rechtermuisknop op het certificaat en opent u het bestand in Kladblok of u voorkeur teksteditor. Kopieer de inhoud tussen de begin- en de titel eind-certificaat. Dit is het x.509-certificaat dat u wilt gebruiken in de Asana voor eenmalige aanmelding configureren.

6. In een andere browser-venster aanmelding bij uw toepassing Asana. Eenmalige aanmelding configureren in Asana, toegang krijgen tot de werkruimte-instellingen door te klikken op de naam van de werkruimte in de rechterbovenhoek van het scherm. Klik op ** \<de naam van uw werkruimte\> instellingen**. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-asana-tutorial/tutorial_asana_09.png)

7. Klik op de **organisatie-instellingen** venster **beheer**. Klik vervolgens op **leden moeten zich aanmelden via de SAML** als de SSO-configuratie wilt inschakelen. Het uitvoeren van de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-asana-tutorial/tutorial_asana_10.png)

    een. Plak in de texbox **- in de URL van** het SAML-teken op Azure AD-URL.

    b. Plak het x.509-certificaat dat u hebt gekopieerd van Azure AD in de textbox **X.509-certificaat** .

9. Klik op **Opslaan**. Naar de [Asana handleiding voor het instellen van eenmalige aanmelding](https://asana.com/guide/help/premium/authentication#gl-saml) als u meer hulp nodig hebt.

7. Ga naar **configureren eenmalige aanmelding op Asana** de pagina in Azure AD, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.
    
    ![Azure AD Single Sign-On][10]

8. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
    
    ![Azure AD Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-asana-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-asana-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-asana-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:
 
    ![Azure AD test gebruikers maken](./media/active-directory-saas-asana-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :

    ![Azure AD test gebruikers maken](./media/active-directory-saas-asana-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-asana-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-asana-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-an-asana-test-user"></a>Het testgebruiker van een Asana maken

In dit gedeelte maakt u een gebruiker met de naam Britta Simon in Asana.

1. Op **Asana**, gaat u naar de sectie **Teams** in het linkerpaneel. Klik op het plusteken (+) knop. 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-asana-tutorial/tutorial_asana_12.png) 

2. Typ het e-mailadres britta.simon@contoso.com in het tekstvak en klik vervolgens op **uitnodigen**.
3. Klik op **Uitnodiging verzenden**. De nieuwe gebruiker ontvangt een e-mailbericht in haar e-mailaccount. Zij moet maken en de account te valideren.


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

In deze sectie kunt inschakelen u Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan Asana.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Asana, kunt u de volgende stappen uitvoeren:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Asana**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-asana-tutorial/tutorial_asana_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst alle gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is voor het testen van uw Azure AD eenmalige aanmelding.

Ga naar de aanmeldingspagina van Asana. In de E-mail adres tekstvak het e-mailadres invoegen britta.simon@contoso.com. Laat het tekstvak wachtwoord leeg en klik op **Log In**. U wordt omgeleid naar de aanmeldingspagina Azure AD. Voer uw referenties Azure AD. U bent nu aangemeld op Asana.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-asana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-asana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-asana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-asana-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-asana-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-asana-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-asana-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-asana-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-asana-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-asana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-asana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-asana-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-asana-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-asana-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-asana-tutorial/tutorial_general_205.png
