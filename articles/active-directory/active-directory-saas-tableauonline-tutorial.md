<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Tableau Online | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Tableau Online."
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
    ms.date="10/18/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-tableau-online"></a>Zelfstudie: Azure Active Directory-integratie met Online Tableau

In deze zelfstudie leert u hoe Online Tableau integreren met Azure Active Directory (AD Azure).

Tableau Online integreren met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang hebben tot Online Tableau
- U kunt gebruikers automatisch ophalen ondertekend voor toegang tot Online Tableau (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met Tableau Online, moet u de volgende items:

- Een abonnement op Azure AD
- Een **Online Tableau** eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Tableau Online toevoegen uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-tableau-online-from-the-gallery"></a>Tableau Online toevoegen uit de galerie
De integratie van Online Tableau in Azure AD configureren, moet u Online Tableau uit de galerie toevoegt aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen uit de galerie Tableau Online, moet u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. In het zoekvak typt u **Tableau Online**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_01.png)

7. In het resultatendeelvenster **Tableau Online**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD Tableau online op basis van een testgebruiker "Simon Britta" genoemd.

Voor eenmalige aanmelding wilt werken, moet AD Azure weet wat de gebruiker tegenhanger online Tableau Azure advertentie aan een gebruiker is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gebruiker in de on line Tableau worden vastgesteld.
Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in de on line Tableau toewijzen.

Als u wilt configureren en testen Azure AD single sign-on met Tableau Online, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Gebruikers maken een Online Tableau test](#creating-a-Tableau-Online-test-user)** - hebben een tegenhanger van Britta Simon Tableau online die is gekoppeld aan de weergave Azure AD van haar.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

Het doel van deze sectie is Azure AD eenmalige aanmelding in de klassieke Azure portal inschakelen en configureren op in uw toepassing Tableau Online.

**Configureren van eenmalige aanmelding Azure AD Tableau online, moet u de volgende stappen uitvoeren:**

1. In het menu aan de bovenkant, klikt u op de **Werkbalk Snel starten**.

    ![Eenmalige aanmelding configureren][6]
2. Klik in de klassieke portal op de pagina **On line Tableau** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren][7] 

3. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij de on line Tableau** **Azure AD Single Sign-On**selecteren en klik op **volgende**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_06.png)

4. Voer de volgende stappen uit op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** : 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_07.png)


    een. Typ in het tekstvak teken in de URL een URL met het volgende patroon:`https://sso.online.tableau.com`

    c. Klik op **volgende**.

5. Klik op de pagina **configureren eenmalige aanmelding op Online Tableau** **metagegevens worden gedownload**, en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_08.png)

6. Selecteer de bevestiging enkele aanmelding-configuratie en klik op **volgende**.
    
    ![Azure AD Single Sign-On][10]

7. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
    
    ![Azure AD Single Sign-On][11]
8. In een andere browser-venster aanmelding bij uw Online Tableau-toepassing. Ga naar **Instellingen** en klik vervolgens op **verificatie**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_09.png)

9. Onder de sectie **Verificatietypen** . Schakel het selectievakje **Single sign-on met SAML** zodat SAML.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_12.png)

10. Schuif naar beneden tot de sectie **importeren metagegevensbestand in Online Tableau** .  Klik op Bladeren en importeer het metagegevensbestand die u hebt gedownload van Azure AD. Klik vervolgens op **toepassen**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_13.png)

11. Invoegen in de sectie **assertions overeenkomen met** de bijbehorende naam identiteitsprovider bevestiging voor het e-mailadres, voornaam en achternaam. Deze informatie ophalen uit Azure Active Directory:

    een. Ga terug naar Azure AD. In de klassieke Azure portal, op het Tableau pagina **On line** toepassing integratie, in het menu aan de bovenkant, klikt u op **kenmerken**. De naam voor de waarden kopiëren: userprincipalname, givenname en achternaam.
     
    ![Azure AD Single Sign-On](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_10.png)

    b. Schakel over naar de toepassing Online Tableau en stel vervolgens de sectie **Tableau Online kenmerken** als volgt:
    
    -  E-mail: **mail** of **userprincipalname**
    -  Voornaam: **givenname**
    -  Achternaam: **Achternaam**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_14.png)

### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:
 
    ![Azure AD test gebruikers maken](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :

    ![Azure AD test gebruikers maken](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-a-tableau-online-test-user"></a>Maken van een Online Tableau testgebruiker

In dit gedeelte maakt u een gebruiker met de naam Britta Simon Tableau Online.

1. Klik op **Instellingen** en vervolgens de sectie **verificatie** op **Online Tableau**. Ga naar de sectie **Selecteer gebruikers** . Klik op **gebruikers toevoegen** en **e-mailadressen invoeren**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_15.png)
2. Selecteer **gebruikers toevoegen voor single sign-on (SSO)-verificatie**. Toevoegen in het tekstvak **E-mailadressen invoeren** .britta.simon@contoso.com

    ![Azure AD test gebruikers maken](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_11.png)

3.  Klik op **maken**.


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

In dit gedeelte vindt inschakelen u Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan Tableau Online.

![Gebruiker toewijzen][200] 

**Britta Simon Tableau online toewijzen, moet u de volgende stappen uitvoeren:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

3. Selecteer **Tableau Online**in de lijst toepassingen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_50.png) 

4. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

5. Selecteer in de lijst alle gebruikers **Britta Simon**.

6. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Wanneer u op de tegel Tableau Online in het Access-venster, u moet krijgen automatisch ondertekend voor toegang tot uw Online Tableau-toepassing.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_205.png
