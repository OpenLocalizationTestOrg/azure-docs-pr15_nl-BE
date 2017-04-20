<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Litmos | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Litmos."
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


# <a name="tutorial-azure-active-directory-integration-with-litmos"></a>Zelfstudie: Azure Active Directory-integratie met Litmos

Het doel van deze zelfstudie is u tonen hoe Litmos integreren met Azure Active Directory (AD Azure).  
Litmos integratie met AD Azure biedt de volgende voordelen: 

- U kunt bepalen in Azure AD die toegang tot de Litmos heeft 
- U kunt gebruikers automatisch ophalen ondertekend bij Litmos (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - Azure Active Directory beheren 

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten 

Azure AD-integratie configureren met Litmos, moet u de volgende items:

- Een abonnement op Azure AD
- Een Litmos eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen. 

 
## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD testen in een testomgeving.  
Het scenario dat is beschreven in deze zelfstudie bestaat uit drie belangrijkste bouwstenen:

1. Litmos uit de galerie toe te voegen. 
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-litmos-from-the-gallery"></a>Litmos uit de galerie toe te voegen.
De integratie van Litmos in Azure AD configureren, moet u Litmos uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Om de Litmos van de galerie wilt toevoegen, moet u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ **Litmos**in het zoekvak.

    ![Toepassingen][5]

7. **Litmos**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Toepassingen][500]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is u tonen hoe te configureren en eenmalige aanmelding Azure AD test met Litmos op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in de Litmos aan een gebruiker in AD Azure. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gebruiker in Litmos worden vastgesteld.  
Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** toewijzen in Azure AD als de waarde van de **gebruikersnaam** in het Litmos.
 
Als u wilt configureren en testen Azure AD single sign-on met Litmos, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Maken van een Litmos gebruiker testen](#creating-a-halogen-software-test-user)** - een tegenhanger van Britta Simon in Litmos die is gekoppeld aan de weergave Azure AD van haar hebben.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van deze sectie is Azure AD eenmalige aanmelding in de klassieke portal Azure AD inschakelen en configureren van eenmalige aanmelding in uw toepassing Litmos.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat-bestand te maken.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

Als onderdeel van de configuratie moet u de **SAML-Token kenmerken** voor uw toepassing Litmos aanpassen.  

![Azure AD Single Sign-On][17] 

**Configureren van eenmalige aanmelding Azure AD met Litmos, voert u de volgende stappen uit:**

1. Klik in de klassieke portal van Azure AD op de pagina **Litmos** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren][6] 

2. Selecteer **Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Litmos** en klik op **volgende**.
 
    ![Azure AD Single Sign-On][7] 


1. Aanmelding bij uw site Litmos bedrijf (bv.: *https://azureapptest.litmos.com/account/Login*) als beheerder.

    ![Azure AD Single Sign-On][21] 


1. Klik op **Accounts**in de navigatiebalk aan de linkerkant.

    ![Azure AD Single Sign-On][22] 


1. Klik op het tabblad **integratie** .

    ![Azure AD Single Sign-On][23] 


1. Op het tabblad **integratie** Blader naar **3de partij integraties**en klik vervolgens op de tab **SAML 2.0** .

    ![Azure AD Single Sign-On][24] 

1. Kopieer de waarde onder **is het SAML-endoiint voor litmos:**.

    ![Azure AD Single Sign-On][26] 


3. In de klassieke Azure portal op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** als volgt:

    ![Azure AD Single Sign-On][8] 
 
    een. Typ de URL die door uw gebruikers gebruikt voor aanmelding bij uw toepassing Litmos in het tekstvak **id** (bijvoorbeeld: *https://azureapptest.litmos.com/account/Login*).
     
    b. Plak in het tekstvak **URL van antwoord op** de waarde die u hebt gekopieerd uit de toepassing van de Litmos in de vorige stap.

    c. Klik op **volgende**.
 
4. Klik op de pagina **configureren eenmalige aanmelding op Litmos** de volgende stappen uitvoeren:

    ![Azure AD Single Sign-On][2] 

    een. Klik op certificaat downloaden en sla het bestand op uw computer.


1. In uw toepassing **Litmos** voert u de volgende stappen uit:

    ![Azure AD Single Sign-On][25] 

    een. Klik op **SAML inschakelen**.

    b. Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.  

    >[AZURE.TIP] Zie voor meer informatie, [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    c. De base-64 gecodeerde certificaat openen in Kladblok, de inhoud ervan kopiëren naar het Klembord en plak deze vervolgens naar de textbox **SAML x.509-certificaat** .

    d. Klik op **wijzigingen opslaan**.


6. Op de klassieke Azure AD-portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**. 

    ![Azure AD Single Sign-On][10]

7. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
  
    ![Azure AD Single Sign-On][11]


20. In het menu aan de bovenkant, klikt u op **kenmerken** om de **SAML-Token kenmerken** -dialoogvenster te openen. 

    ![Eenmalige aanmelding configureren][12]


24. Voer de volgende stappen uit in het dialoogvenster **Gebruikerskenmerk toevoegen** : 

    ![Eenmalige aanmelding configureren][14]

  	| Naam van kenmerk | Waarde van het kenmerk |
  	| ---            | ---             |
  	| E-mail          | User.mail       |
  	| Voornaam      | User.givenName  |
  	| Achternaam       | User.surname    |

    Voor elke rij met gegevens in de bovenstaande tabel, moet u de volgende stappen uitvoeren:
   
    een. Klik op **gebruikerskenmerk toevoegen**. 

    ![Eenmalige aanmelding configureren][15]


    een. Typ in het tekstvak **Naam van kenmerk** de **Naam van een kenmerk** weergegeven voor die rij.

    b. Selecteer de **Waarde van het kenmerk** voor die rij.

    c. Klik op **Voltooien** om het dialoogvenster **Gebruikerskenmerk toevoegen** te sluiten.


25. Klik op **wijzigingen toepassen**. 

    ![Eenmalige aanmelding configureren][16]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
Het doel van deze sectie is voor het maken van een testgebruiker in Azure klassieke portal Britta Simon genoemd.  

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure clasic portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-litmos-tutorial/create_aaduser_09.png)  

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-litmos-tutorial/create_aaduser_03.png) 
 
4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**. 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-litmos-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren: 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-litmos-tutorial/create_aaduser_05.png)  

    een. Selecteer **Type van gebruiker**, **nieuwe gebruiker in uw organisatie**.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** : 

    ![Azure AD test gebruikers maken](./media/active-directory-saas-litmos-tutorial/create_aaduser_06.png) 
 
    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .
    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-litmos-tutorial/create_aaduser_07.png) 
 
8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-litmos-tutorial/create_aaduser_08.png) 
  
    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   

  
 
### <a name="creating-a-litmos-test-user"></a>Maken van een gebruiker Litmos test

Het doel van deze sectie is voor het maken van een gebruiker met de naam Britta Simon in Litmos.  
Just in Time biedt ondersteuning voor de toepassing van de Litmos wordt ingericht. Dit betekent, een account wordt automatisch gemaakt als nodig is tijdens een poging toegang te krijgen tot de toepassing met het Access-venster.

**Om een gebruiker met de naam Britta Simon in Litmos maakt, moet u de volgende stappen uitvoeren:**


1. Aanmelding bij uw site Litmos bedrijf (bv.: *https://azureapptest.litmos.com/account/Login*) als beheerder.

    ![Azure AD Single Sign-On][21] 


1. Klik op **Accounts**in de navigatiebalk aan de linkerkant.

    ![Azure AD Single Sign-On][22] 


1. Klik op het tabblad **integratie** .

    ![Azure AD Single Sign-On][23] 


1. Op het tabblad **integratie** Blader naar **3de partij integraties**en klik vervolgens op de tab **SAML 2.0** .

    ![Azure AD Single Sign-On][24] 

1. Selecteer **gebruikers automatisch genereren:**.

    ![Azure AD Single Sign-On][27] 


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

Het doel van deze sectie wordt aan het inschakelen van Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan Litmos.

![Gebruiker toewijzen][200] 

**Britta Simon aan Litmos toewijzen, moet u de volgende stappen uitvoeren:**

1. Op de Azure klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Litmos**.

    ![Gebruiker toewijzen][202] 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.  
Wanneer u op de tegel Litmos in het Access-venster, u moet krijgen automatisch ondertekend bij uw toepassing Litmos.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_01.png
[500]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_02.png

[6]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_03.png
[8]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_04.png
[9]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_05.png
[10]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_07.png
[12]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_80.png
[13]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_81.png
[14]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_82.png
[15]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_81.png
[16]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_19.png
[17]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_67.png


[20]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_60.png
[22]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_61.png
[23]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_62.png
[24]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_63.png
[25]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_64.png
[26]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_65.png
[27]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_66.png

[200]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_68.png
[203]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_205.png


[400]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_400.png
[401]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_401.png
[402]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_402.png





