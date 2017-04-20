<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met IBM Kenexa enquête onderneming | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory- en IBM Kenexa enquête onderneming."
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
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-ibm-kenexa-survey-enterprise"></a>Zelfstudie: Azure Active Directory-integratie met IBM Kenexa enquête onderneming

In deze zelfstudie leert u hoe IBM Kenexa enquête Enterprise integratie met Azure Active Directory (AD Azure).

IBM Kenexa enquête Enterprise integreren met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot de IBM Kenexa enquête Enterprise heeft
- U kunt gebruikers automatisch ophalen ondertekend bij IBM Kenexa enquête Enterprise (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met IBM Kenexa onderzoek onderneming, moet u de volgende items:

- Een abonnement op Azure AD
- Een IBM Kenexa enquête Enterprise eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. IBM Kenexa enquête onderneming uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-ibm-kenexa-survey-enterprise-from-the-gallery"></a>IBM Kenexa enquête onderneming uit de galerie toe te voegen
De integratie van IBM Kenexa enquête onderneming in Azure AD configureren, moet u IBM Kenexa enquête onderneming uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**IBM Kenexa enquête onderneming toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **IBM Kenexa enquête onderneming**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_01.png)

7. Selecteer **IBM Kenexa enquête Enterprise**en klik op **Voltooien** als de toepassing wilt toevoegen in het resultatendeelvenster.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met IBM Kenexa enquête onderneming op basis van een testgebruiker "Britta Simon" genoemd.

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in IBM Kenexa enquête onderneming aan een gebruiker in AD Azure. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gebruiker in IBM Kenexa enquête onderneming worden vastgesteld.

Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in IBM Kenexa enquête onderneming toewijzen.

Als u wilt configureren en testen Azure AD eenmalige aanmelding met IBM Kenexa onderzoek onderneming, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Maken van een onderneming IBM Kenexa enquête gebruiker testen](#creating-an-kenexasurvey-test-user)** - een tegenhanger van Britta Simon in IBM Kenexa enquête onderneming die is gekoppeld aan de weergave Azure AD van haar hebben.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
6. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie, Azure AD eenmalige aanmelding in de klassieke portal inschakelen en eenmalige aanmelding configureren in uw bedrijfstoepassing IBM Kenexa enquête.


**Configureren van eenmalige aanmelding Azure AD met IBM Kenexa onderzoek onderneming, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal op de pagina **IBM Kenexa enquête Enterprise** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij IBM Kenexa enquête Enterprise** **Azure AD Single Sign-On**selecteren en klik op **volgende**.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_03.png)

3. Voer de volgende stappen uit op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_04.png)

    een. Typ in het tekstvak **id** een URL met het volgende patroon:`https://surveys.kenexa.com/<company code>`

    b. Typ in het tekstvak **URL antwoord op** een URL met het volgende patroon:`https://surveys.kenexa.com/<company code>/tools/sso.asp`

    c. Klik op **volgende**.

    > [AZURE.NOTE] Opmerking: dit zijn niet de werkelijke waarden. U moet deze waarden bijwerken met de werkelijke id en URL antwoord. Neem contact op met IBM Kenexa enquête Enterprise support team om deze waarden.

4. Klik op **certificaat downloaden** en sla het bestand op uw computer op de pagina **configureren eenmalige aanmelding bij IBM Kenexa enquête onderneming** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_05.png) 

5. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met het ondersteuningsteam van IBM-Kenexa en geeft u het volgende:

    • Het gedownloade bestand

    • De **URL van de uitgevende instelling**

    • De **SAML SSO-URL**

    • De **URL van de Service voor eenmalige afmelden**

    > [AZURE.NOTE] Opmerking dat NameID aanspraak maken op de waarde in het antwoord moet Neem overeenkomt met de ID voor eenmalige aanmelding in het Kenexa systeem is geconfigureerd. Dus u moet werken met Kenexa support team toewijzen van de juiste gebruikers-id in uw organisatie als eenmalige aanmelding. Azure AD wordt standaard het NameIdentifier als UPN-waarde. U kunt dit tabblad kenmerk zoals in het screenshot hieronder wijzigen. De integratie werkt alleen na het voltooien van de juiste toewijzing. 
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_51.png)

6. In de klassieke portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.

    ![Azure AD Single Sign-On][10]

7. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
  
    ![Azure AD Single Sign-On][11]

8. In de Azure klassieke portal op de **IBM Kenexa enquête Enterprise** application Integratiepagina in het menu aan de bovenkant, klikt u op **kenmerken**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_06.png)

9. Voer de volgende stappen uit in het dialoogvenster **SAML-token kenmerken** :

    een. Selecteer het kenmerk van **NameIdentifier** en klik op het pictogram **bewerken** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_07.png)
    
    b. Typ in de lijst **Waarde van het kenmerk** de kenmerkwaarde van SSO-ID die is geconfigureerd in het Kenexa systeem.
    
    c. Klik op **Voltooien**

### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
Het doel van deze sectie is een om testgebruiker te maken in de portal voor klassieke Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.
 
    ![Azure AD test gebruikers maken](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:
 
    ![Azure AD test gebruikers maken](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :

    ![Azure AD test gebruikers maken](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-an-ibm-kenexa-survey-enterprise-test-user"></a>Het testgebruiker van een onderneming van IBM Kenexa enquête maken

In dit gedeelte maakt u een gebruiker met de naam Britta Simon in IBM Kenexa enquête onderneming. Neem samen met IBM Kenexa support team toe te wijzen, de ID voor eenmalige aanmelding voor alle gebruikers. Deze waarde ID voor eenmalige aanmelding moet ook worden toegewezen aan de waarde van NameIdentifier uit Azure Active Directory. U kunt deze standaardinstellingen op het tabblad kenmerk wijzigen.


> [AZURE.NOTE] Als u een gebruiker handmatig te maken, moet u contact opnemen met de IBM Kenexa enquête Enterprise support team.


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

In dit gedeelte vindt inschakelen u Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan IBM Kenexa enquête Enterprise.

![Gebruiker toewijzen][200] 

**Britta Simon toewijzen aan IBM Kenexa onderzoek onderneming, moet u de volgende stappen uitvoeren:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **IBM Kenexa enquête Enterprise**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.
    
    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In dit gedeelte vindt u Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Wanneer u klikt op de IBM Kenexa enquête Enterprise tegel in het Access-venster, u moet krijgen automatisch ondertekend voor toegang tot de bedrijfstoepassing IBM Kenexa enquête.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_205.png
