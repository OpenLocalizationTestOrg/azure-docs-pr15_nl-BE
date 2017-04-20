<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Ceridian Dayforce HCM | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Ceridian Dayforce HCM."
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


# <a name="tutorial-azure-active-directory-integration-with-ceridian-dayforce-hcm"></a>Zelfstudie: Azure Active Directory-integratie met Ceridian Dayforce HCM

Het doel van deze zelfstudie is u tonen hoe Ceridian Dayforce HCM integreren met Azure Active Directory (AD Azure).  
Ceridian Dayforce HCM integreren met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot de Ceridian Dayforce HCM heeft
- U kunt gebruikers automatisch ophalen ondertekend bij Ceridian Dayforce HCM (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren


Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met Ceridian Dayforce HCM, moet u de volgende items:

- Een abonnement op Azure AD
- Een Ceridian Dayforce HCM eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD testen in een testomgeving.  
Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Ceridian Dayforce HCM toevoegen uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-ceridian-dayforce-hcm-from-the-gallery"></a>Ceridian Dayforce HCM toevoegen uit de galerie
De integratie van Ceridian Dayforce HCM in Azure AD configureren, moet u Ceridian Dayforce HCM uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Ceridian Dayforce HCM toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Ceridian Dayforce HCM**.

    ![Toepassingen](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_01.png)

7. In het resultatendeelvenster **Ceridian Dayforce HCM**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Toepassingen](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_07.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is u tonen hoe te configureren en eenmalige aanmelding Azure AD test met Ceridian Dayforce HCM op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in Ceridian Dayforce HCM aan een gebruiker in AD Azure. Met andere woorden, moet een relatie van de koppeling tussen een Azure AD-gebruiker en de bijbehorende gebruiker in Ceridian Dayforce HCM worden vastgesteld.

Als u wilt configureren en Azure AD single sign-on met Ceridian Dayforce HCM testen, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Gebruikers maken een Dayforce Ceridian HCM testen](#creating-a-ceridian-dayforce-hcm-test-user)** - hebben een tegenhanger van Britta Simon in Ceridian Dayforce HCM die is gekoppeld aan de weergave Azure AD van haar.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

Het doel van deze sectie is Azure AD eenmalige aanmelding in de klassieke Azure portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Ceridian Dayforce HCM.

Uw toepassing Ceridian Dayforce HCM verwacht de SAML-bevestigingen in een specifieke indeling. Neem samen met team Dayforce HCM eerst aan de juiste gebruikers-id te identificeren. Microsoft raadt het gebruik van het kenmerk **"naam"** als gebruikers-id. U kunt de waarde van dit kenmerk in het dialoogvenster **'Atrribute'** beheren. De volgende schermafbeelding ziet u een voorbeeld voor dit. 

![Eenmalige aanmelding configureren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_02.png) 

**Configureren van eenmalige aanmelding Azure AD met Ceridian Dayforce HCM, voert u de volgende stappen uit:**




1. In de klassieke Azure portal, op de pagina **Ceridian Dayforce HCM** toepassing integratie, in het menu aan de bovenkant, klikt u op **kenmerken**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_81.png) 


1. In de lijst kenmerken **saml-token kenmerken** , selecteert u het naamkenmerk en klik vervolgens op **bewerken**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_06.png) 


1. Voer de volgende stappen uit in het dialoogvenster **Gebruikerskenmerk bewerken** :
 
    een. Selecteer in de lijst **Waarde van het kenmerk** het gebruikerskenmerk dat u wilt gebruiken voor uw implementatie.  
    Bijvoorbeeld, als u wilt de werknemer-id gebruikt als de unieke gebruikers-id en u de waarde van het kenmerk in de ExtensionAttribute2 hebt opgeslagen, selecteert u **user.extensionattribute2**. 

    b. Klik op **Voltooien**.  
    



1. In het menu aan de bovenkant, klikt u op de **Werkbalk Snel starten**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hightail-tutorial/tutorial_general_83.png)  





1. Klik op **eenmalige aanmelding configureren**in de Azure klassieke portal op de pagina **Ceridian Dayforce HCM** application integration.

    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Ceridian Dayforce HCM** **Azure AD Single Sign-On**selecteren en klik op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_03.png) 

3. Klik op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** als volgt:.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_04.png) 


    een. Typ in het tekstvak **Teken op URL** de URL die wordt gebruikt door uw gebruikers om aanmelding bij uw toepassing Ceridian Dayforce HCM. Gebruik de volgende URL-notatie voor productieomgevingen:`https://sso.dayforcehcm.com/DayforcehcmNamespace`

    Voor de duidelijkheid, vervangt u DayforcehcmNamespace door de naamruimte van uw omgeving of uw bedrijfs-ID; bijvoorbeeld:`https://sso.dayforcehcm.com/contoso`
    
    Voor testomgevingen, gebruikt u de volgende URL-indeling:`https://ssotest.dayforcehcm.com/DayforcehcmNamespace` 

    b. Typ de URL die is gebruikt door AD Azure voor het boeken van het antwoord in het tekstvak **URL antwoord** .  
    Gebruik voor productieomgevingen:`https://ncpingfederate.dayforcehcm.com/sp/ACS.saml2`  
    Voor testomgevingen gebruikt:`https://fs-test.dayforcehcm.com/sp/ACS.saml2`  
   

4. Voer de volgende stappen uit op de pagina **configureren eenmalige aanmelding op Ceridian Dayforce HCM** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_05.png) 

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


5. Neem contact op met het ondersteuningsteam Ceridian Dayforce HCM via e-mail als u eenmalige aanmelding configureren voor uw toepassing, en geeft u het volgende:

    - Het gedownloade bestand
    - De **URL van de uitgevende instelling**
    - De **SAML SSO-URL** 
    - De **Single Sign Out Service URL** 


6. In de klassieke Azure portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.

    ![Azure AD Single Sign-On][10]

7. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  

    ![Azure AD Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
Het doel van deze sectie is voor het maken van een testgebruiker in Azure klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :

    ![Azure AD test gebruikers maken](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-a-ceridian-dayforce-hcm-test-user"></a>Een gebruiker Ceridian Dayforce HCM test maken

Het doel van deze sectie is voor het maken van een gebruiker met de naam Britta Simon in Ceridian Dayforce HCM. 

Neem werken met het ondersteuningsteam Ceridian Dayforce HCM voor gebruikers die zijn toegevoegd aan de in de toepassing Ceridian Dayforce HCM. 





### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

Het doel van deze sectie wordt aan het inschakelen van Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan Ceridian Dayforce HCM.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Ceridian Dayforce HCM, voert u de volgende stappen uit:**

1. Op de Azure klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Ceridian Dayforce HCM**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.  
Wanneer u op de tegel Ceridian Dayforce HCM in het Access-venster, u moet krijgen automatisch ondertekend bij uw toepassing Ceridian Dayforce HCM.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_205.png
