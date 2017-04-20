<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met bestanden houden | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en houden van bestanden."
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


# <a name="tutorial-azure-active-directory-integration-with-flatter-files"></a>Zelfstudie: Azure Active Directory-integratie met houden van bestanden

Het doel van deze zelfstudie is u tonen hoe te houden bestanden integreren met Azure Active Directory (AD Azure).  
Houden bestanden integreren met AD Azure biedt de volgende voordelen: 

- U kunt bepalen in Azure AD die toegang tot bestanden houden heeft 
- U kunt gebruikers automatisch ophalen ondertekend bij houden bestanden (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke portal Azure Active Directory beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten 

Azure AD-integratie configureren met houden van bestanden, moet u de volgende items:

- Een abonnement op Azure AD
- Een bestanden houden eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen. 

 
## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD testen in een testomgeving.  
Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Houden bestanden toevoegen uit de galerie 
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-flatter-files-from-the-gallery"></a>Houden bestanden toevoegen uit de galerie
De integratie van het houden van bestanden in Azure AD configureren, moet u houden bestanden uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt houden, bestanden toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Bestanden houden**.


    ![Azure AD test gebruikers maken](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_01.png)

7. In het resultatendeelvenster **Houden bestanden**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_500.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is u tonen hoe te configureren en eenmalige aanmelding Azure AD test met houden bestanden op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in houden van bestanden aan een gebruiker in AD Azure. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gebruiker in het houden van bestanden worden vastgesteld.  
Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in het houden van bestanden toewijzen.
 
Als u wilt configureren en testen Azure AD eenmalige aanmelding met houden van bestanden, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Gebruikers maken een houden bestanden testen](#creating-a-halogen-software-test-user)** - een tegenhanger van Britta Simon in houden van bestanden die is gekoppeld aan de weergave Azure AD van haar hebben.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

Het doel van deze sectie is Azure AD eenmalige aanmelding in de klassieke portal Azure AD inschakelen en configureren van eenmalige aanmelding in uw toepassing bestanden houden. Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat-bestand te maken. Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

U moet een geregistreerde domeinnaam configureren van eenmalige aanmelding voor het houden van bestanden. Als u niet over een geregistreerde domeinnaam nog contact houden bestanden support team via [support@flatterfiles.com](mailto:support@flatterfiles.com).  



**Configureren van eenmalige aanmelding Azure AD met houden van bestanden, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal van Azure AD op de pagina **Bestanden houden** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren][6] 

2. Selecteer **Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aanmelden bij het houden van bestanden** en klik op **volgende**.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_02.png) 

3. Klik op **volgende**op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_03.png) 

    > [AZURE.NOTE] Houden van bestanden gebruikt dezelfde SSO login-URL voor alle klanten: [https://www.flatterfiles.com/site/login/sso/](https://www.flatterfiles.com/site/login/sso/).
.
 
 
4. Op de pagina **configureren eenmalige aanmelding bij het houden van bestanden** , moet u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_04.png)  

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


1. Aanmelding bij uw toepassing bestanden houden als beheerder.

2. Klik op het Dashboard. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_05.png)  



2. Klik op **Instellingen**en voert u de volgende stappen uit op het tabblad **bedrijf** : 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_06.png)  

    een. Selecteer **SAML 2.0 voor verificatie gebruiken**.

    b. Klik op **SAML configureren**.



2. In het dialoogvenster **SAML-configuratie** , moet u de volgende stappen uitvoeren: 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_08.png)  

    een. Typ in het tekstvak domein uw geregistreerde domein.

    > [AZURE.NOTE] Als u niet over een geregistreerde domeinnaam nog contact houden bestanden support team via [support@flatterfiles.com](mailto:support@flatterfiles.com).
    
    b. In de Azure klassieke portal, voor het configureren van één aanmelding in het dialoogvenster bestanden houden, copt de Single Sign-On Service URL en plak deze in het tekstvak URL van de Provider identiteit.

    c.  Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.  

    >[AZURE.TIP] Zie voor meer informatie, [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    d.  De base-64 gecodeerde certificaat in Kladblok te openen, de inhoud ervan kopiëren naar het Klembord en plak deze op **Het certificaat FlatterFiles identiteit Provider** textbox.

    e. Klik op **bijwerken**.

6. In de klassieke Azure AD-portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**. 

    ![Azure AD Single Sign-On][10]

7. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  

    ![Azure AD Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
Het doel van deze sectie is voor het maken van een testgebruiker in Azure klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_03.png) 
 
4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**. 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren: 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_05.png)  

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** : 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_06.png) 
 
    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .
    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_07.png) 
 
8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_08.png) 
  
    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   

  
 
### <a name="creating-a-flatter-files-test-user"></a>Een gebruiker bestanden houden test maken

Het doel van deze sectie is voor het maken van een gebruiker met de naam Britta Simon in bestanden houden.

**Als een gebruiker met de naam Britta Simon in houden van bestanden maakt, moet u de volgende stappen uitvoeren:**

1. Meld u aan uw site **Bestanden houden** bedrijf aan als beheerder.

2. In het navigatiedeelvenster aan de linkerkant, klikt u op **Instellingen**en klik vervolgens op het **tabblad**van gebruikers.

    ![Cfreate gebruiker bestanden houden](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_09.png)

3. Klik op **gebruiker toevoegen**. 

4. In het dialoogvenster **Gebruiker toevoegen** kunt u de volgende stappen uitvoeren:

    ![Cfreate gebruiker bestanden houden](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_10.png)

    een. Typ in het tekstvak **Voornaam** **Britta**.

    b. Typ in het tekstvak **Achternaam** **Simon**. 

    c. Typ in het tekstvak **E-mailadres** van Britta e-mailadres in de klassieke Azure portal.

    d. Klik op **indienen**.   


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

Het doel van deze sectie is Britta Simon gebruik van door haar toegang verlenen tot bestanden houden Azure eenmalige aanmelding inschakelen.

![Gebruiker toewijzen][200] 

**Britta Simon toewijzen aan bestanden houden, kunt u de volgende stappen uitvoeren:**

1. Op de Azure klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Bestanden houden**.

    ![Gebruiker toewijzen](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_11.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.  
Wanneer u op de tegel houden van bestanden in het Access-venster, u moet krijgen automatisch ondertekend voor toegang tot uw toepassing bestanden houden.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_205.png






