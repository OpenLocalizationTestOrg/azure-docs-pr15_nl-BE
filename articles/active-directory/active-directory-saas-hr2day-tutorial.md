<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met HR2day door Merces | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en HR2day door Merces."
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


# <a name="tutorial-azure-active-directory-integration-with-hr2day-by-merces"></a>Zelfstudie: Azure Active Directory-integratie met HR2day met Merces

Het doel van deze zelfstudie is u tonen hoe HR2day door Merces integreren met Azure Active Directory (AD Azure).  
HR2day door Merces integreren met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot HR2day met Merces heeft
- U kunt gebruikers automatisch ophalen ondertekend bij HR2day door Merces (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met HR2day door Merces, moet u de volgende items:

- Een abonnement op Azure AD
- Een HR2day door Merces eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD testen in een testomgeving.  
Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. HR2day door Merces uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-hr2day-by-merces-from-the-gallery"></a>HR2day door Merces uit de galerie toe te voegen.
De integratie van HR2day door Merces in Azure AD configureren, moet u HR2day door Merces uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**HR2day door Merces toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.
 
    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **HR2day door Merces**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_01.png)

7. In het resultatendeelvenster selecteert **HR2day door Merces**en klik op **Voltooien** als de toepassing wilt toevoegen.


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is u tonen hoe te configureren en te testen Azure AD single sign-on met HR2day door Merces op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in HR2day door Merces aan een gebruiker in AD Azure. Met andere woorden, dient een relatie van de koppeling tussen een Azure AD-gebruiker en de bijbehorende gebruiker in HR2day door Merces te worden voorzien.  
Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in het HR2day door Merces toewijzen.

Als u wilt configureren en testen Azure AD single sign-on met HR2day door Merces, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Maken van een HR2day door Merces gebruiker testen](#creating-a-hr2day-by-merces-test-user)** - een tegenhanger van Britta Simon in HR2day door Merces die is gekoppeld aan de weergave Azure AD van haar hebben.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij HR2day door Merces met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

Uw HR2day door toepassing van Merces verwacht de SAML-bevestigingen in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. De volgende schermafbeelding ziet u een voorbeeld voor dit. 

![Eenmalige aanmelding configureren](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_00.png) 

Voordat u de SAML-bevestiging configureren kunt, moet u contact opnemen met het ondersteuningsteam van HR2day via [servicedesk@merces.nl](mailto:servicedesk@merces.nl) en de waarde van het kenmerk de unieke id voor de huurder aanvragen. U moet deze waarde voor het voltooien van de stappen in de volgende sectie.


**Configureren van eenmalige aanmelding Azure AD met HR2day door Merces, kunt u de volgende stappen uitvoeren:**

1. In de Azure klassieke portal op de **HR2day door Merces** application Integratiepagina in het menu aan de bovenkant, klikt u op **kenmerken** om de **SAML-Token kenmerken** -dialoogvenster te openen. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_06.png) 

2. Voer de volgende stappen uit om de vereiste kenmerktoewijzingen toevoegen, voer de volgende stappen uit: 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_07.png) 


    een. Klik op **gebruikerskenmerk toevoegen**.

    b. Typ in het tekstvak **Naam van kenmerk** **'ATTR_LOGINCLAIM'**.

    c. Selecteer in de lijst **Waarde van het kenmerk** **Join()**. 

    d. Selecteer in de lijst **tekenreeks1** **User.mail**. 

    e. Typ in het tekstvak **tekenreeks2** de **unieke id** die uw team HR2day. 

    f. Typ in het tekstvak **voor** **@**.

    g. Klik op **Voltooien**.

  
3. Klik op **wijzigingen toepassen**.


1. In het menu aan de bovenkant, klikt u op **Snel starten** om het dialoogvenster **Quick Start** te openen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hr2day-tutorial/tutorial_general_08.png) 



1. Klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij HR2day door Merces** **Azure AD Single Sign-On**selecteren en klik op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_03.png) 

3. Voer de volgende stappen uit op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** : 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_04.png) 


    een. Typ in het tekstvak teken op URL de URL die door uw gebruikers om aanmelding bij uw HR2day wordt gebruikt door Merces-toepassing met behulp van het volgende patroon: **"https://\<Naam huurder\>.force.com/\<exemplaarnaam\>'**.

    b. Klik op **volgende**.

4. Voer de volgende stappen uit op de pagina **configureren eenmalige aanmelding bij HR2day met Merces** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_05.png) 

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


5. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met uw HR2day door Merces support team via [servicedesk@merces.nl](emailTo:servicedesk@merces.nl) en het gedownloade bestand te koppelen aan uw e-mailadres. Ook Geef de SAML SSO-URL, teken van URL's en URL van de uitgevende instelling zodat deze kunnen worden geconfigureerd voor integratie van eenmalige aanmelding.


> [AZURE.NOTE] Neem vermelden bij Merces team dat deze integratie moet entiteits-ID worden ingesteld met dit patroon **https://hr2day.force.com/INSTANCENAME**



6. In de klassieke Azure portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.

    ![Azure AD Single Sign-On][10]

7. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  

    ![Azure AD Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
Het doel van deze sectie is voor het maken van een testgebruiker in Azure klassieke portal Britta Simon genoemd.  

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-hr2day-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-hr2day-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-hr2day-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-hr2day-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :

    ![Azure AD test gebruikers maken](./media/active-directory-saas-hr2day-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-hr2day-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-hr2day-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-a-hr2day-by-merces-test-user"></a>Een HR2day door het testgebruiker Merces maken

Het doel van deze sectie is voor het maken van een gebruiker met de naam Britta Simon in HR2day door Merces. Neem werken met HR2day met Merces ondersteuningsteam gebruikers toevoegen in de account HR2day. 


> [AZURE.NOTE]Als u een gebruiker handmatig te maken, moet u contact opnemen met de HR2day door Merces support team.


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

Het doel van deze sectie wordt aan het inschakelen van Simon voor de Azure eenmalige aanmelding toegang verlenen aan HR2day door Merces Britta.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen HR2day door Merces, kunt u de volgende stappen uitvoeren:**

1. Op de Azure klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteert **HR2day door Merces**in de lijst toepassingen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.  
Wanneer u op de HR2day door Merces tegel in het Access-venster, u moet krijgen automatisch ondertekend voor toegang tot uw HR2day door toepassing van Merces.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_205.png
