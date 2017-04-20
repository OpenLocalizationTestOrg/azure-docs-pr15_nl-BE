<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met O.C. Tan - AppreciateHub | Microsoft Azure"
    description="Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en O.C. Tan - AppreciateHub."
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
    ms.date="08/16/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-oc-tanner---appreciatehub"></a>Zelfstudie: Azure Active Directory-integratie met O.C. Tan - AppreciateHub

Het doel van deze zelfstudie is u tonen hoe te integreren, O.C. Tan - AppreciateHub met Azure Active Directory (AD Azure).  
Integratie van O.C. Tan - AppreciateHub met Azure Active Directory biedt de volgende voordelen: 

- U kunt bepalen in Azure AD die toegang tot de O.C. heeft Tan - AppreciateHub 
- U kunt gebruikers automatisch ophalen ondertekend bij O.C. inschakelen Tan - AppreciateHub (Single Sign-On) met hun accounts Azure AD
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten 

Azure AD-integratie configureren met O.C. Tan - AppreciateHub, moet u de volgende items:

- Een abonnement op Azure AD
- EEN O.C. Tan - AppreciateHub eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen. 

 
## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD testen in een testomgeving.  
Het scenario dat is beschreven in deze zelfstudie bestaat uit drie belangrijkste bouwstenen:

1. O.C. toevoegen Tan - AppreciateHub in de galerie 
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-oc-tanner---appreciatehub-from-the-gallery"></a>O.C. toevoegen Tan - AppreciateHub in de galerie
De integratie van O.C. configureren Tan - AppreciateHub in Azure AD, moet u O.C. toevoegen Tan - AppreciateHub uit de galerie aan de lijst met beheerde SaaS-apps.

**Om toe te voegen tan O.C. - AppreciateHub uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1] 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2] 

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3] 

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4] 

6. Typ in het zoekvak **O.C. tan - AppreciateHub**.

    ![Toepassingen][5] 

7. **O.C. tan - AppreciateHub**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Toepassingen][25] 




##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding

Het doel van deze sectie is u tonen hoe te configureren en te testen Azure AD single sign-on met O.C. Tan - AppreciateHub op basis van een testgebruiker "Simon Britta" genoemd.

Voor eenmalige aanmelding te werken, moet Azure AD weten wat de tegenhanger van gebruiker in O.C. Tan - AppreciateHub aan een gebruiker in AD Azure is. Met andere woorden, een relatie tussen een Azure AD-gebruiker en de bijbehorende gebruiker in O.C. koppeling Tan - AppreciateHub moet worden vastgesteld.  
Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in het O.C. toewijzen Tan - AppreciateHub.
 
Configureren en testen Azure AD single sign-on met O.C. Tan - AppreciateHub, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Maken van een tan O.C. - het testgebruiker AppreciateHub](#creating-a-halogen-software-test-user)** - hebben een tegenhanger van Britta Simon in O.C. Tan - AppreciateHub die is gekoppeld aan de weergave Azure AD van haar.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van deze sectie is Azure AD eenmalige aanmelding in de klassieke Azure portal inschakelen en configureren van eenmalige aanmelding in uw O.C. Tan - AppreciateHub toepassing.


**Configureren van eenmalige aanmelding Azure AD met tan O.C. - AppreciateHub, voert u de volgende stappen uit:**

1. Klik in de klassieke Azure portal, klik op de pagina **O.C. tan - AppreciateHub** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren][6]

2. Selecteer **Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aanmelden bij O.C. tan - AppreciateHub** en klik op **volgende**.

    ![Azure AD Single Sign-On][7]

3. Voer de volgende stappen uit op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** :

    ![App-instellingen configureren][8]
 
     een. Open het bestand met metagegevens met de volgende koppeling: [https://fed.appreciatehub.com/fed/sp/metadata](https://fed.appreciatehub.com/fed/sp/metadata).

     b. Zoek het knooppunt **md:AssertionConsumerService** . 

     c. Kopieer de waarde van het kenmerk **Location** . 

     ![App-instellingen configureren][12]
     
     d. In het **Teken op URL** -tekstvak voorbij de waarde hebt u verkregen in de vorige stap.

     > [AZURE.NOTE] Als u problemen met expiriencing ophalen van de URL van het antwoord van het metagegevensbestand, neem dan contact op met de O.C. Tan - ondersteuningsteam van AppreciateHub via [sso@octanner.com](mailto:sso@octanner.com).

     e. Klik op **volgende**.
 
4. Klik op de pagina **configureren eenmalige aanmelding bij O.C. tan - AppreciateHub** op **metagegevens downloaden**en sla het bestand lokaal op uw computer met metagegevens.

    ![Wat is Azure AD verbinden][9]

5. Neem contact op met de O.C. Tan - AppreciateHub support team via xyz, geeft u het bestand met metagegevens en ze laten weten dat deze eenmalige aanmelding voor u inschakelen mag.


6. Selecteer de bevestiging enkele aanmelding-configuratie op de Azure klassieke portal en klik op **volgende**. 

    ![Wat is Azure AD verbinden][10]

7. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  

    ![Wat is Azure AD verbinden][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
Het doel van deze sectie is voor het maken van een testgebruiker in Azure klassieke portal Britta Simon genoemd.  

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_02.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_03.png) 
 
4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**. 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren: 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** : 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_06.png)
 
    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .
    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_07.png) 
 
8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_08.png) 
  
    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   

  
 
### <a name="creating-a-oc-tanner---appreciatehub-test-user"></a>Een O.C. maken Tan - het testgebruiker AppreciateHub

Het doel van deze sectie is voor het maken van een gebruiker met de naam Britta Simon in O.C. Tan - AppreciateHub.

**Een gebruiker met de naam Britta Simon in O.C. tan - AppreciateHub, maken de volgende stappen uitvoeren:**

1. Vraag uw ondersteuningsteam OC tan een gebruiker met als nameID dezelfde waarde als de gebruikersnaam van Britta Simon in Azure AD kenmerk te maken.


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

Het doel van deze sectie wordt aan het inschakelen van Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan O.C. Tan - AppreciateHub.

![Gebruiker toewijzen][200]

**Britta Simon om aan te wijzen O.C. tan - AppreciateHub, voert u de volgende stappen uit:**

1. Op de Azure klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **O.C. tan - AppreciateHub**.

    ![Gebruiker toewijzen][202]

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.  
Wanneer u klikt op de O.C. Tan - AppreciateHub naast elkaar in het Configuratiescherm toegang u moet krijgen automatisch aangemeld voor toegang tot uw O.C. Tan - AppreciateHub toepassing.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->
[1]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_01.png
[25]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_25.png


[6]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_02.png
[8]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_03.png
[9]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_04.png
[10]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_05.png
[11]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_06.png
[12]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_08.png
[20]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_07.png
[203]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_205.png






