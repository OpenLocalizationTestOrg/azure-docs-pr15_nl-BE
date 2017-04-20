<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met de Portal financiering | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en de financiering Portal."
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
    ms.date="09/02/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-the-funding-portal"></a>Zelfstudie: Azure Active Directory-integratie met de Portal voor financiering

In deze zelfstudie leert u hoe de financiering Portal integreren met Azure Active Directory (AD Azure).

De financiering Portal integreren met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot de financiering Portal heeft
- U kunt gebruikers automatisch ophalen ondertekend voor toegang tot de financiering Portal (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met de financiering Portal, moet u de volgende items:

- Een abonnement op Azure AD
- Een **Portal voor de financiering van** eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Portal voor de financiering uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-the-funding-portal-from-the-gallery"></a>Portal voor de financiering uit de galerie toe te voegen.
Configureren van de integratie van de Portal voor financiering in Azure AD, moet u de Portal voor de financiering uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Portal voor de financiering uit de galerie toevoegen, voert u de volgende stappen uit:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak, **Portal voor de financiering**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_01.png)

7. Selecteer **Portal voor de financiering**in het resultatenvenster en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met de financiering Portal op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in de Portal voor financiering aan een gebruiker in AD Azure. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gebruiker in de Portal financiering worden vastgesteld.
Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** toewijzen in Azure AD als de waarde van de **gebruikersnaam** in de Portal voor financiering.

Als u wilt configureren en testen Azure AD eenmalige aanmelding met de financiering Portal, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Maken van een de financiering Portal gebruiker testen](#creating-a-the-funding-portal-test-user)** - een tegenhanger van Britta Simon in de financiering Portal die is gekoppeld aan de weergave Azure AD van haar hebben.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

Het doel van deze sectie is Azure AD eenmalige aanmelding in de klassieke Azure portal inschakelen en configureren op in uw toepassing de financiering Portal.

De toepassing financiering Portal verwacht de SAML-assertions bevatten een kenmerk met de naam 'externalId1'. De waarde van "externalId1" moet een erkende studentID. Configureer de claim "externalId1" voor deze toepassing. De waarden van deze kenmerken kunt u beheren via het tabblad **'Atrributes'** van de toepassing. De volgende schermafbeelding ziet u een voorbeeld voor dit.

![Eenmalige aanmelding configureren](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_03.png) 

**Azure AD eenmalige aanmelding configureren met de Portal voor financiering, voert u de volgende stappen uit:**

1. In de klassieke Azure portal, op de pagina **Portal voor de financiering van** toepassing integratie, in het menu aan de bovenkant, klikt u op **kenmerken**.
     
    ![Eenmalige aanmelding configureren][5]

2. Toevoegen in het dialoogvenster SAML-token kenmerken het kenmerk 'externalId1'.

    een. Klik op **gebruikerskenmerk toevoegen** om de **Gebruikerskenmerk toevoegen** dialoogvenster te openen. 
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_05.png)

    b. Typ in het tekstvak **Naam van kenmerk** kenmerk de naam 'externalId1'.

    c. Uit de lijst met **Waarden van kenmerk** , selecteer het kenmerk dat u wilt gebruiken voor uw implementatie. Bijvoorbeeld, als u de waarde StudentID in de ExtensionAttribute1 hebt opgeslagen, selecteert u user.extensionattribute1.

    d. Klik op **Voltooien**. Klik vervolgens op **Wijzigingen toepassen**.

1. In het menu aan de bovenkant, klikt u op de **Werkbalk Snel starten**.

    ![Eenmalige aanmelding configureren][6]

2. Klik in de klassieke portal op de pagina **Portal voor de financiering van** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren][7] 

3. Klik op de pagina **Hoe wilt u dat gebruikers aan te melden op de Portal financiering** **Azure AD Single Sign-On**selecteren en klik op **volgende**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_06.png)

4. Voer de volgende stappen uit op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** : 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_07.png)


    een. Typ in het tekstvak teken in de URL een URL met het volgende patroon: `https://<subdomain>.regenteducation.net/`.

    b. Klik op **volgende**.

5. Klik op de pagina **configureren eenmalige aanmelding bij de financiering Portal** **metagegevens worden gedownload**, en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_08.png)

6. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met ondersteuning voor de financiering Portal. Ze staan dan met het juiste kanaal voor het configureren van eenmalige aanmelding. Neem notitie die u hebt voor het verzenden van e-mail en koppelen gedownload bestand met metagegevens op info@regenteducation.com.

7. In de klassieke portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.
    
    ![Azure AD Single Sign-On][10]

8. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
    
    ![Azure AD Single Sign-On][11]

### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:
 
    ![Azure AD test gebruikers maken](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :

    ![Azure AD test gebruikers maken](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-a-the-funding-portal-test-user"></a>Een gebruiker van de Portal voor financiering test maken

In deze sectie kunt u een gebruiker met de naam Britta Simon in de Portal voor financiering. Werken met de financiering Portal-ondersteuningsteam toe te voegen het testgebruiker en eenmalige aanmelding inschakelen als u niet hoe Britta Simon toevoegen in de Portal voor financiering weet, is. Neem contact op met deze op info@regenteducation.com.

### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

In dit gedeelte vindt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door haar toegang verlenen tot de Portal financiering.

![Gebruiker toewijzen][200] 

**Britta Simon toewijzen aan de Portal voor financiering, voert u de volgende stappen uit:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer **De financiering Portal**in de lijst toepassingen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_09.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst alle gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Wanneer u op de Portal van de financiering tegel in het Access-venster, u moet krijgen automatisch ondertekend voor toegang tot uw Portal voor de financiering van toepassing.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_205.png
