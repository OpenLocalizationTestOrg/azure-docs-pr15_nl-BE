<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met MCM | Microsoft Azure" 
    description="Meer informatie over het MCM met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="08/30/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-mcm"></a>Zelfstudie: Azure Active Directory-integratie met MCM
  
Het doel van deze zelfstudie is u tonen hoe MCM integreren met Azure Active Directory (AD Azure).

MCM integreren met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot het MCM heeft
- U kunt gebruikers automatisch ophalen ondertekend bij MCM (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met MCM, moet u de volgende items:

- Een geldig abonnement Azure
- Een MCM eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.

## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD testen in een testomgeving.

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. MCM uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-mcm-from-the-gallery"></a>MCM uit de galerie toe te voegen.
De integratie van MCM in Azure AD configureren, moet u MCM uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**MCM toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-mcm-tutorial/tutorial_general_01.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-mcm-tutorial/tutorial_general_02.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-mcm-tutorial/tutorial_general_03.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-mcm-tutorial/tutorial_general_04.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **MCM**.

    ![Galerie van toepassing] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_01.png "Galerie van toepassing")

7.  **MCM**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![MCM] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_001.png "MCM")

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is u tonen hoe te configureren en eenmalige aanmelding Azure AD test met MCM op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in het MCM aan een gebruiker in AD Azure. Met andere woorden, moet een relatie van de koppeling tussen een Azure AD-gebruiker en de gebruiker in het MCM worden vastgesteld.

Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in het MCM toewijzen.

Als u wilt configureren en testen Azure AD single sign-on met MCM, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
3. **[Gebruikers maken een MCM testen](#creating-a-mcm-test-user)** - hebben een tegenhanger van Britta Simon in MCM die is gekoppeld aan de weergave Azure AD van haar.
4. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren
  
In deze sectie, Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren van eenmalige aanmelding in uw toepassing MCM.

**Configureren van eenmalige aanmelding Azure AD met MCM, voert u de volgende stappen uit:**

1.  Klik in de klassieke Azure portal, klik op de pagina **MCM** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mcm-tutorial/tutorial_general_05.png "Eenmalige aanmelding configureren")

2.  Op de pagina **Hoe wilt u dat gebruikers aanmelden bij MCM** Selecteer **AD Azure Microsoft Single Sign-On**en klik vervolgens op **volgende**.

    ![Microsoft Azure AD Single Sign-On] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_03.png "Microsoft Azure AD Single Sign-On")

3.  Voer de volgende stappen uit op de pagina van het dialoogvenster Toepassingsinstellingen configureren:

    ![App-URL configureren] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_04.png "App-URL configureren")

    een. Typ in het tekstvak **Teken op URL** : `https://myaba.co.uk/client-access/<company name>/saml.php`.
    
    b. Klik op **volgende**

4.  Klik op de pagina **configureren eenmalige aanmelding bij MCM** op **metagegevens downloaden**en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_05.png "Eenmalige aanmelding configureren")

5. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met het ondersteuningsteam MCM. De van het gedownloade metagegevensbestand koppelen en delen met MCM-team voor het instellen van eenmalige aanmelding op hun kant.

6.  In de klassieke portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_06.png "Eenmalige aanmelding configureren")

7. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_07.png "Eenmalige aanmelding configureren")


### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken

Het doel van deze sectie is een om testgebruiker te maken in de portal voor klassieke Britta Simon genoemd.

![Azure AD test gebruikers maken](./media/active-directory-saas-mcm-tutorial/create_aaduser_00.png)

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke Portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-mcm-tutorial/create_aaduser_01.png)

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-mcm-tutorial/create_aaduser_02.png)

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-mcm-tutorial/create_aaduser_03.png)

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-mcm-tutorial/create_aaduser_04.png)

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-mcm-tutorial/create_aaduser_05.png)

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-mcm-tutorial/create_aaduser_06.png)

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-mcm-tutorial/create_aaduser_07.png)

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   

###<a name="creating-a-mcm-test-user"></a>Een gebruiker MCM test maken
  
In dit gedeelte maakt u een gebruiker met de naam Britta Simon in MCM. Neem samen met MCM support team toe te voegen van de gebruikers in het MCM-platform.

>[AZURE.NOTE]Kunt u een andere MCM gebruiker account maken van hulpprogramma's of API's geleverd door MCM bepaling AAD gebruikersaccounts.


###<a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen
  
Het doel van deze sectie wordt aan het inschakelen van Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan MCM.
    
![Gebruikers toewijzen] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_00.png "Gebruikers toewijzen")

**Britta Simon om aan te wijzen MCM, voert u de volgende stappen uit:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.
    
    ![Gebruikers toewijzen] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_01.png "Gebruikers toewijzen")

2. Selecteer in de lijst met toepassingen, **MCM**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_08.png)

1. In het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Gebruikers toewijzen] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_02.png "Gebruikers toewijzen")

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.
    
    ![Gebruikers toewijzen] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_03.png "Gebruikers toewijzen")


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.
 
Wanneer u op de tegel MCM in het Access-venster, u moet krijgen automatisch ondertekend bij uw toepassing MCM.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)