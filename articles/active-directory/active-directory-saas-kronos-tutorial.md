<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Kronos | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Kronos."
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
    ms.date="09/19/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-kronos"></a>Zelfstudie: Azure Active Directory-integratie met Kronos

In deze zelfstudie leert u hoe Kronos integreren met Azure Active Directory (AD Azure).

Kronos integreren met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot de Kronos heeft
- U kunt gebruikers automatisch ophalen ondertekend bij Kronos (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met Kronos, moet u de volgende items:

- Een abonnement op Azure AD
- Een **Kronos personeel centraal** eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Kronos uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-kronos-from-the-gallery"></a>Kronos uit de galerie toe te voegen.
De integratie van Kronos in Azure AD configureren, moet u Kronos uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Kronos toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Kronos**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_01.png)

7. **Kronos**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Kronos op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weet wat de gebruiker tegenhanger in Kronos is aan een gebruiker in AD Azure. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gebruiker in de Kronos worden vastgesteld.
Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in het Kronos toewijzen.

Als u wilt configureren en testen Azure AD single sign-on met Kronos, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Gebruikers maken een Kronos testen](#creating-an-kronos-test-user)** - hebben een tegenhanger van Britta Simon in Kronos die is gekoppeld aan de weergave Azure AD van haar.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie, Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Kronos.

Uw toepassing Kronos verwacht de SAML-bevestigingen in een specifieke indeling. Neem samen met Kronos team eerst bij het identificeren van de juiste gebruikers-id die wordt toegewezen in de toepassing. Neem ook de richtlijnen van Kronos-team over het kenmerk dat moet worden gebruikt voor deze toewijzing. Het kenmerk **'NameIdentifier'** als de gebruikers-id gebruiken door Microsoft aanbevolen. De waarde van dit kenmerk kunt u beheren via het tabblad **'Atrribute'** van de toepassing. De volgende schermafbeelding ziet u een voorbeeld voor dit. Hier hebben we de claim nameidentifier met het kenmerk **userprincipalname** samen met de functie **ExtractMailPrefix** , waarmee een unieke gebruikers-ID, die wordt verzonden naar de Kronos-toepassing in de elke geslaagde SAML-reactie toegewezen.

![Eenmalige aanmelding configureren](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_07.png) 


**Configureren van eenmalige aanmelding Azure AD met Kronos, voert u de volgende stappen uit:**

1. Klik in de klassieke portal op de pagina **Kronos** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

     ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Kronos** **Azure AD Single Sign-On**selecteren en klik op **volgende**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_03.png) 

3. Klik op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** als volgt:.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_04.png) 


    een. Typ in het tekstvak id de URL die door uw gebruikers wordt gebruikt voor aanmelding bij de Kronos-toepassing met behulp van het volgende patroon:`https://<company name>.kronos.net/`

    b. Typ in de antwoord-URL de URL in het volgende patroon:`https://<company name>.kronos.net/wfc/navigator/logonWithUID`


4. Klik op de pagina **configureren eenmalige aanmelding bij Kronos** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_05.png) 

    een. Klik op **metagegevens downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


5.  Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met uw accountmanager van Kronos en hij zal helpen bij het juiste kanaal voor het configureren van eenmalige aanmelding. Houd er rekening mee dat er voor het verzenden van e-mail en gedownloade metagegevensbestand bijvoegen.
  
6. In de klassieke portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.
    
    ![Azure AD Single Sign-On][10]

7. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
    
    ![Azure AD Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-kronos-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-kronos-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-kronos-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:
 
    ![Azure AD test gebruikers maken](./media/active-directory-saas-kronos-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :

    ![Azure AD test gebruikers maken](./media/active-directory-saas-kronos-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-kronos-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-kronos-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-an-kronos-test-user"></a>Een gebruiker Kronos test maken

In dit gedeelte maakt u een gebruiker met de naam Britta Simon in Kronos. Kronos toepassing moeten alle gebruikers in de toepassing voordat u eenmalige aanmelding worden ingericht. Dus u moet werken met de klant Kronos ondersteuning koppelen al deze gebruikers de toepassing inrichten. 


> [AZURE.NOTE] Als u een gebruiker handmatig maken of batch gebruikers, moet u contact opnemen met het ondersteuningsteam van Kronos.


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

In deze sectie kunt inschakelen u Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan Kronos.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Kronos, voert u de volgende stappen uit:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Kronos**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In dit gedeelte vindt u Azure AD één aanmelding configuratie testen met behulp van het Access-venster.
Wanneer u op de tegel Kronos in het Access-venster, u moet krijgen automatisch ondertekend bij uw toepassing Kronos.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_205.png
