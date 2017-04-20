<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Novatus | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en LearnUpon."
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
    ms.date="08/15/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a>Zelfstudie: Azure Active Directory-integratie met LearnUpon

Het doel van deze zelfstudie is u tonen hoe LearnUpon integreren met Azure Active Directory (AD Azure).  
LearnUpon integratie met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot de LearnUpon heeft
- U kunt gebruikers automatisch ophalen ondertekend bij LearnUpon (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - Azure Active Directory klassieke beheren 


Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met LearnUpon, moet u de volgende items:

- Een abonnement op Azure AD
- Een LearnUpon eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD testen in een testomgeving.  
Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. LearnUpon uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-learnupon-from-the-gallery"></a>LearnUpon uit de galerie toe te voegen.
De integratie van LearnUpon in Azure AD configureren, moet u LearnUpon uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Om de LearnUpon van de galerie wilt toevoegen, moet u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ **LearnUpon**in het zoekvak.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_01.png)

7. **LearnUpon**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is u tonen hoe te configureren en eenmalige aanmelding Azure AD test met LearnUpon op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in de LearnUpon aan een gebruiker in AD Azure. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gebruiker in LearnUpon worden vastgesteld.  
Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** toewijzen in Azure AD als de waarde van de **gebruikersnaam** in het LearnUpon.

Als u wilt configureren en testen Azure AD single sign-on met LearnUpon, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Maken van een LearnUpon gebruiker testen](#creating-a-learnupon-test-user)** - een tegenhanger van Britta Simon in LearnUpon die is gekoppeld aan de weergave Azure AD van haar hebben.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van deze sectie is Azure AD eenmalige aanmelding in de klassieke Azure portal inschakelen en configureren van eenmalige aanmelding in uw toepassing LearnUpon.



**Configureren van eenmalige aanmelding Azure AD met LearnUpon, voert u de volgende stappen uit:**

1. Klik in de klassieke Azure portal, klik op de pagina **LearnUpon** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren][6] 

2. Selecteer **Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op LearnUpon** en klik op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_03.png) 

3. Klik op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** als volgt:.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_04.png) 


    een. Typ in het tekstvak **URL antwoord** de consument bevestiging URL met behulp van het volgende patroon:`https://\<companyname\>.learnupon.com/saml/consumer`

    b. Klik op **volgende**. 


4. Klik op de pagina **configureren eenmalige aanmelding op LearnUpon** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_05.png) 

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer. Moeten we dit certificaat en de metagegevens van URL's (entiteits-ID, SSO-teken In de URL en Sign Out URL) voor het instellen van eenmalige aanmelding op LearnUpon aan de zijkant.

    b. Klik op **volgende**.




1. Een ander browserexemplaar en inloggen in LearnUpon met een administrator-account openen. 

1. Klik op het tabblad **Instellingen** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_06.png) 


1. Klik op **Eenmalige aanmelding - SAML**en klik vervolgens op **Algemene instellingen** voor het SAML-instellingen configureren.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_07.png) 


5. In de sectie **Algemene instellingen van** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_08.png) 

    een. Selecteer **ingeschakeld**.

    b. Selecteer als **versie** **2.0**.

    c. Selecteer **Nee**als **voorwaarden overslaan**.

    d. Type de naam van de aanvraag na parameter naar de URL van de consument SAML vermeld boven bevat in het **SAML-Token boeken param** tekstvak, de SAML-bevestiging worden gecontroleerd en geverifieerd - bijvoorbeeld **SAMLResponse**.

    e. Typ de waarde die waar de SAML-bevestiging de gebruikers-id (e-mailadres) woont aangeeft-bijvoorbeeld in het tekstvak **Naam id indeling** **urn: oasis: namen: tc: SAML:1.1:nameid-indeling: EmailAdres**.

    f. Typ in het tekstvak **Provider locatie bepalen** de waarde die waar de gebruikers worden verzonden aangeeft als ze op het pictogram van uw geüploade van uw scherm Azure klassieke aanmeldings klikken.

    g.in de klassieke Azure portal, kopieert u de **URL van de Service Single Sign-Out**en plak deze in de textbos **URL afmelden** .

    h. Klik op **beheren vinger afdrukken**en vervolgens de vingerafdruk van uw gedownloade certificaat uploaden. 


1. Klik op **Instellingen**en voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_11.png) 

    een. Typ de waarde dat waar in het SAML-bevestiging de voornaam van de gebruikers zich bevindt aangeeft-bijvoorbeeld in het tekstvak **Id-indeling Voornaam** : **givenname http://schemas.xmlsoap.org/ws/2005/05/identity/claims/**.

    b. Typ de waarde die waar de SAML-bevestiging de achternaam gebruikers woont aangeeft-bijvoorbeeld in het tekstvak **Naam id Achternaam** : **Achternaam http://schemas.xmlsoap.org/ws/2005/05/identity/claims/**.


6. In de klassieke Azure portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.

    ![Azure AD Single Sign-On][10]

7. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  

    ![Azure AD Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
Het doel van deze sectie is voor het maken van een testgebruiker in Azure klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-learnupon-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-learnupon-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-learnupon-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-learnupon-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :

    ![Azure AD test gebruikers maken](./media/active-directory-saas-learnupon-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-learnupon-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-learnupon-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-a-learnupon-test-user"></a>Maken van een gebruiker LearnUpon test

Het doel van deze sectie is voor het maken van een gebruiker met de naam Britta Simon in LearnUpon. LearnUpon ondersteunt just in time-aanbod is standaard ingeschakeld.

Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker wordt gemaakt tijdens een poging toegang te krijgen tot LearnUpon als deze nog niet bestaat. [Azure AD voor eenmalige aanmelding configureren](#configuring-azure-ad-single-single-sign-on).

> [AZURE.NOTE] Als u een gebruiker handmatig te maken, moet u contact opnemen met het ondersteuningsteam van LearnUpon.


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

Het doel van deze sectie wordt aan het inschakelen van Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan LearnUpon.

![Gebruiker toewijzen][200] 

**Britta Simon aan LearnUpon toewijzen, moet u de volgende stappen uitvoeren:**

1. Op de Azure klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **LearnUpon**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.  
Wanneer u op de tegel LearnUpon in het Access-venster, u moet krijgen automatisch ondertekend bij uw toepassing LearnUpon.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_205.png
