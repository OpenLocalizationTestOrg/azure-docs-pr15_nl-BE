<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Amazon Web Service (AWS) | Microsoft Azure"
    description="Leren hoe Amazon Web Services (AWS) met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!"
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


# <a name="tutorial-azure-active-directory-integration-with-amazon-web-service-aws"></a>Zelfstudie: Azure Active Directory-integratie met Amazon Web Service (AWS)

Het doel van deze zelfstudie is u tonen hoe Amazon Web Service (AWS) integreren met Azure Active Directory (AD Azure).  
Amazon Web Service (AWS) integreren met AD Azure biedt de volgende voordelen: 

- U kunt bepalen in Azure AD met toegang tot Amazon Web Service (AWS) 
- U kunt gebruikers automatisch ophalen ondertekend op naar Amazon Web Service (AWS) (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten 

Azure AD-integratie met Amazon Web Service (AWS) configureren, moet u de volgende items:

- Een abonnement op Azure AD
- Een Amazon Web Service (AWS) eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen. 

 
## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD testen in een testomgeving.  
Het scenario dat is beschreven in deze zelfstudie bestaat uit drie belangrijkste bouwstenen:

1. Amazon Web Service (AWS) uit de galerie toe te voegen. 
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-amazon-web-service-aws-from-the-gallery"></a>Amazon Web Service (AWS) uit de galerie toe te voegen.
De integratie van Amazon Web Service (AWS) in Azure AD configureren, moet u Amazon Web Service (AWS) uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

### <a name="to-add-amazon-web-service-aws-from-the-gallery-perform-the-following-steps"></a>Amazon Web Service (AWS) uit de galerie toevoegen, voert u de volgende stappen uit:

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1] 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu. 
   
    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina. 
   
    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**. 
   
    ![Toepassingen][4]

6. Typ in het zoekvak, **Amazon Web Service (AWS)**.
   
    ![Toepassingen][5]

7. **Amazon Web Service (AWS)**selecteert in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Toepassingen][6]



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is u tonen hoe te configureren en testen Azure AD eenmalige aanmelding met Amazon Web Service (AWS) op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in Amazon Web Service (AWS) aan een gebruiker in AD Azure. Met andere woorden, moet een relatie van de koppeling tussen een Azure AD-gebruiker en de gebruiker in Amazon Web Service (AWS) worden vastgesteld.  
Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** toewijzen in Azure AD als de waarde van de **gebruikersnaam** in Amazon Web Service (AWS).
 
Als u wilt configureren en testen Azure AD eenmalige aanmelding met Amazon Web Service (AWS), moet u voor het voltooien van de volgende elementen:

1. **[Configureren AD Azure één Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Gebruikers maken een Amazon Web Service (AWS) testen](#creating-a-halogen-software-test-user)** - hebben een tegenhanger van Britta Simon in Amazon Web Service (AWS) die is gekoppeld aan de weergave Azure AD van haar.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Azure AD enkele eenmalige aanmelding configureren

Het doel van deze sectie is Azure AD eenmalige aanmelding in de klassieke Azure portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Amazon Web Service (AWS).  
Uw toepassing Amazon Web Service (AWS) verwacht dat de SAML-bevestigingen in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de **saml-token kenmerken** . De volgende schermafbeelding ziet u een voorbeeld voor dit.


![Eenmalige aanmelding configureren][27]

**Configureren van eenmalige aanmelding Azure AD met Amazon Web Service (AWS), voert u de volgende stappen uit:**

1. Klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen in de Azure klassieke portal op de Integratiepagina van **Amazon Web Service (AWS)** toepassing.

    ![Eenmalige aanmelding configureren][7]

2. Op de pagina **Hoe wilt u dat gebruikers aanmelden op Amazon Web Service (AWS)** Selecteer **Azure AD Single Sign-On**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren][8]

3. Klik op volgende op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** . 

    ![App-instellingen configureren][9]
 
4. Klik op de pagina **configureren eenmalige aanmelding bij Amazon Web Service (AWS)** op **metagegevens downloaden**en sla het bestand lokaal op uw computer met metagegevens.

    ![Eenmalige aanmelding configureren][10]

5. In een andere browser-venster aanmelding bij uw bedrijf Amazon Web Service (AWS) site als beheerder.

6. Klik op de **Console-startpagina**.

    ![Eenmalige aanmelding configureren][11]

7. Klik op **Identity and Access Management**. 

    ![Eenmalige aanmelding configureren][12]

8. Klik op **Id-Providers**en klik op **Provider maken**. 

    ![Eenmalige aanmelding configureren][13]

9. Voer de volgende stappen uit op de pagina van het dialoogvenster **Provider configureren** : 

    ![Eenmalige aanmelding configureren][14]

     een. **ProviderType**, **SAML**selecteren.

     b. Typ een naam in het tekstvak **Naam van de Provider** (bv.: *WAAD*).

     c. Uw gedownloade om metagegevensbestand te uploaden, klikt u op **Bestand kiezen**.

     d. Klik op de **volgende stap**.


10. Klik op de pagina van het dialoogvenster **Controleren of de gegevens** **maken**. 

    ![Eenmalige aanmelding configureren][15]

11. Klik op **functies**en klik op **Nieuwe rol maken**. 

    ![Eenmalige aanmelding configureren][16]

12. Voer de volgende stappen uit in het dialoogvenster **Rolnaam instellen** : 

    ![Eenmalige aanmelding configureren][17]

    een. Typ in het tekstvak **Naam** een rolnaam (bv.: *' testuser '*).

    b. Klik op de **volgende stap**.

13. Voer de volgende stappen uit in het dialoogvenster **Roltype selecteren** : 

    ![Eenmalige aanmelding configureren][18]

    een. Selecteer **de rol voor Identity Provider toegang**.

    b. Klik op **selecteren**in de sectie **subsidie Web Single Sign-On (WebSSO) de toegang tot aanbieders van SAML** .


14. Voer de volgende stappen uit in het dialoogvenster **Vertrouwensrelaties tot stand brengen** :  

    ![Eenmalige aanmelding configureren][19]
     
     een. Als het SAML-provider, selecteert u de SAML-provider u previousley hebt gemaakt (bijvoorbeeld: *WAAD*) 

     b. Klik op de **volgende stap**.


15. Klik op de **Volgende stap**in het dialoogvenster **Rol vertrouwen controleren** . 

    ![Eenmalige aanmelding configureren][32]


16. Klik op de **Volgende stap**in het dialoogvenster **Koppelen beleid** .  

    ![Eenmalige aanmelding configureren][33]


17. Voer de volgende stappen uit in het dialoogvenster **revisie** :   

    ![Eenmalige aanmelding configureren][34]

     een. Kopieer de waarde van de **Rol van ARN** .

     b. Kopieer de waarde van de ARN **Vertrouwde entiteiten** .

     c. Klik op **rol maken**. 

18. Selecteer de bevestiging enkele aanmelding-configuratie op de Azure klassieke portal en klik op **volgende**.

    ![Wat is Azure AD verbinden][20]

19. Klik op **Voltooien** om het dialoogvenster voor **eenmalige aanmelding configureren** te sluiten op de pagina **bevestiging van één aanmelding** .

    ![Wat is Azure AD verbinden][22]


20. In het menu aan de bovenkant, klikt u op **kenmerken** om de **SAML-Token kenmerken** -dialoogvenster te openen. 

    ![Eenmalige aanmelding configureren][21]

21. Klik op **gebruikerskenmerk toevoegen**. 

    ![Eenmalige aanmelding configureren][23]

22. In het dialoogvenster gebruikerskenmerk toevoegen de volgende stappen uitvoeren. 

    ![Eenmalige aanmelding configureren][24] 

    een. Typ in het tekstvak **Naam van kenmerk** **https://aws.amazon.com/SAML/Attributes/Role**.

    b. Typ in het tekstvak **Waarde van het kenmerk** **[de rol ARN waarde], [de waarde van de vertrouwde entiteit ARN]**.

    >[AZURE.TIP] Dit zijn de waarden die u hebt gekopieerd uit het dialoogvenster controleren bij het maken van uw rol. 

    c. Klik op **Voltooien** om het dialoogvenster **Gebruikerskenmerk toevoegen** te sluiten.

23. Klik op **gebruikerskenmerk toevoegen**. 

    ![Eenmalige aanmelding configureren][23]


24. In het dialoogvenster gebruikerskenmerk toevoegen de volgende stappen uitvoeren. 

    ![Eenmalige aanmelding configureren][25]


     een. Typ in het tekstvak **Naam van kenmerk** **https://aws.amazon.com/SAML/Attributes/RoleSessionName**.

     b. Typ in het tekstvak **Waarde van het kenmerk** of **user.userprincipalname** selecteren in de vervolgkeuzelijst.
     
    ![Eenmalige aanmelding configureren][35]
    

     c. Klik op **Voltooien** om het dialoogvenster **Gebruikerskenmerk toevoegen** te sluiten.


25. Klik op **wijzigingen toepassen**. 

    ![Eenmalige aanmelding configureren][26]





### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken

Het doel van deze sectie is voor het maken van een testgebruiker in Azure klassieke portal Britta Simon genoemd.

![Azure AD test gebruikers maken](./media/active-directory-saas-amazon-web-service/create_aaduser_01.png)

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-amazon-web-service/create_aaduser_02.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-amazon-web-service/create_aaduser_03.png) 
 
4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**. 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-amazon-web-service/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren: 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-amazon-web-service/create_aaduser_05.png) 

  1. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.
  2. Typ **BrittaSimon**in de naam van het **tekstvak**.
  3. Klik op volgende.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** : 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-amazon-web-service/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Achternaam** txtbox, type, **Simon**.
  
    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .
  
    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-amazon-web-service/create_aaduser_07.png) 
 
8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-amazon-web-service/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.
  
    b. Klik op **Voltooien**.   
  
 
### <a name="creating-a-amazon-web-service-aws-test-user"></a>Maken van een testgebruiker Amazon Web Service (AWS)

Het doel van deze sectie is voor het maken van een gebruiker met de naam Britta Simon in Amazon Web Service (AWS).

### <a name="to-create-a-user-called-britta-simon-in-amazon-web-service-aws-perform-the-following-steps"></a>Als een gebruiker met de naam Britta Simon in Amazon Web Service (AWS) maakt, moet u de volgende stappen uitvoeren:

1. Meld u als beheerder uw bedrijf **Amazon Web Service (AWS)** site.

2. Klik op het pictogram voor de **Console-startpagina** . 

    ![Eenmalige aanmelding configureren][11]

3. Klik op identiteit en Management. 

    ![Eenmalige aanmelding configureren][28]

4. In het Dashboard, klikt u op gebruikers en klik vervolgens op nieuwe gebruikers maken. 

    ![Eenmalige aanmelding configureren][29]

5. Voer de volgende stappen uit in het dialoogvenster gebruiker maken: 

    ![Eenmalige aanmelding configureren][30]

     een. Typ de gebruikersnaam van Brita Simon (userprincipalname) in Azure AD in de tekstvakken **Gebruikersnaam invoeren** .

     b. Klik op **maken**.




### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

Het doel van deze sectie wordt aan het inschakelen van Britta Simon voor de Azure eenmalige aanmelding toegang verlenen tot Amazon Web Service (AWS).

![Gebruiker toewijzen][31]

**Britta Simon aan CloudPassage toewijzen, moet u de volgende stappen uitvoeren:**

1. Op de Azure klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][26]

2. Selecteer in de lijst met toepassingen, **Amazon Web Service (AWS)**.

    ![Gebruiker toewijzen][27]

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][25]

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][29]

### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.  
Wanneer u op de tegel Amazon Web Service (AWS) in het Access-venster, u moet krijgen automatisch ondertekend bij uw toepassing Amazon Web Service (AWS).


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-amazon-web-service/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service/tutorial_general_04.png
[5]: ./media/active-directory-saas-amazon-web-service/ic795019.png
[6]: ./media/active-directory-saas-amazon-web-service/ic795020.png
[7]: ./media/active-directory-saas-amazon-web-service/ic795027.png
[8]: ./media/active-directory-saas-amazon-web-service/ic795028.png
[9]: ./media/active-directory-saas-amazon-web-service/capture23.png
[10]: ./media/active-directory-saas-amazon-web-service/capture24.png
[11]: ./media/active-directory-saas-amazon-web-service/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service/tutorial_general_15.png
[26]: ./media/active-directory-saas-amazon-web-service/tutorial_general_18.png
[27]: ./media/active-directory-saas-amazon-web-service/ic7950357.png
[28]: ./media/active-directory-saas-amazon-web-service/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service/tutorial_general_16.png
[30]: ./media/active-directory-saas-amazon-web-service/ic795038.png
[31]: ./media/active-directory-saas-amazon-web-service/tutorial_general_17.png
[32]: ./media/active-directory-saas-amazon-web-service/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service/user_attributes_01.png






















