<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met de Bank actief | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en de Bank activa."
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


# <a name="tutorial-azure-active-directory-integration-with-asset-bank"></a>Zelfstudie: Azure Active Directory-integratie met de Bank actief

Het doel van deze zelfstudie is u tonen hoe activa Bank integreren met Azure Active Directory (AD Azure).

Activa Bank integreren met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot de Bank activa heeft
- U kunt gebruikers automatisch ophalen ondertekend bij Bank activa (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met de Bank actief, moet u de volgende items:

- Een abonnement op Azure AD
- Een Bank activa eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD testen in een testomgeving. 

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Activa-Bank toevoegen uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-asset-bank-from-the-gallery"></a>Activa-Bank toevoegen uit de galerie
De integratie van de Bank activa in Azure AD configureren, moet u activa Bank uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Activa Bank toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]


2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Bank activa**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_01.png)

7. In het resultatenvenster **Actief Bank**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is u tonen hoe te configureren en eenmalige aanmelding Azure AD test met activa Bank op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in Bank activa aan een gebruiker in AD Azure. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gebruiker in de Bank activa worden vastgesteld.

Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in de Bank activa toewijzen.

Als u wilt configureren en testen Azure AD eenmalige aanmelding met de Bank actief, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Maken van een Bank activa gebruiker testen](#creating-a-asset-bank-test-user)** - een tegenhanger van Britta Simon in activa Bank die is gekoppeld aan de weergave Azure AD van haar hebben.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van deze sectie is Azure AD eenmalige aanmelding in de klassieke Azure portal inschakelen en configureren op in uw toepassing Bank activa.



**Azure AD eenmalige aanmelding configureren met de Bank actief, voert u de volgende stappen uit:**

1. Klik in de klassieke Azure portal, op de pagina **Activa Bank** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

![Eenmalige aanmelding configureren][6] 


2. Klik op de pagina **Hoe wilt u dat gebruikers aan te melden op de Bank activa** **Azure AD Single Sign-On**selecteren en klik op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_03.png) 


3. Voer de volgende stappen uit op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_04.png) 


    een. In het teken op URL-tekstvak typt u de URL die wordt gebruikt door gebruikers voor aanmelding bij uw Bank activa-toepassing met behulp van het volgende patroon: **"https://\<bedrijf\>.assetbank server.com '**.

    b. Klik op **volgende**.

4. Klik op de pagina **configureren eenmalige aanmelding bij de Bank activa** de volgende stappen uitvoeren:
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_05.png) 

    een. Klik op **metagegevens downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


5. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met uw Bank activa support team via [support@assetbank.co.uk](mailto:support@assetbank.co.uk) en het bestand met metagegevens toevoegen aan uw e-mailadres.


6. In de klassieke Azure portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.

    ![Azure AD Single Sign-On][10]

7. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
  
    ![Azure AD Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
Het doel van deze sectie is voor het maken van een testgebruiker in Azure klassieke portal Britta Simon genoemd.

Selecteer in de lijst gebruikers **Britta Simon**.

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke Portal**in het linkernavigatievenster op **Active Directory**.
 
    ![Azure AD test gebruikers maken](./media/active-directory-saas-assetbank-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-assetbank-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-assetbank-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-assetbank-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :

    ![Azure AD test gebruikers maken](./media/active-directory-saas-assetbank-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.
    ![Azure AD test gebruikers maken](./media/active-directory-saas-assetbank-tutorial/create_aaduser_07.png) 


8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-assetbank-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-a-asset-bank-test-user"></a>Maken van een gebruiker actief Bank test

Het doel van deze sectie is voor het maken van een gebruiker met de naam Britta Simon in activa Bank. Activa Bank ondersteunt just in time-aanbod is standaard ingeschakeld.

Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker wordt gemaakt tijdens een poging toegang te krijgen tot de Bank actief als deze nog niet bestaat. 

> [AZURE.NOTE] Als u een gebruiker handmatig te maken, moet u contact opnemen met de Bank actief support team.


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

Het doel van deze sectie wordt aan het inschakelen van Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan activa Bank.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Bank activa, moet u de volgende stappen uitvoeren:**

1. Op de Azure klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer **Bank activa**in de lijst toepassingen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Wanneer u op de tegel Bank activa in het Access-venster, u moet krijgen automatisch ondertekend voor toegang tot uw toepassing Bank activa.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_205.png