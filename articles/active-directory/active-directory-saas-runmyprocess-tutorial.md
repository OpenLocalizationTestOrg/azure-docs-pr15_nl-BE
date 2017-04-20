<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met RunMyProcess | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en RunMyProcess."
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
    ms.date="10/21/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-runmyprocess"></a>Zelfstudie: Azure Active Directory-integratie met RunMyProcess

Het doel van deze zelfstudie is u tonen hoe RunMyProcess integreren met Azure Active Directory (AD Azure).

RunMyProcess integratie met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot de RunMyProcess heeft
- U kunt gebruikers automatisch ophalen ondertekend bij RunMyProcess (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met RunMyProcess, moet u de volgende items:

- Een abonnement op Azure AD
- Een RunMyProcess eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD testen in een testomgeving.

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. RunMyProcess uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-runmyprocess-from-the-gallery"></a>RunMyProcess uit de galerie toe te voegen.
De integratie van RunMyProcess in Azure AD configureren, moet u RunMyProcess uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Om de RunMyProcess van de galerie wilt toevoegen, moet u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ **RunMyProcess**in het zoekvak.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_01.png)

7. **RunMyProcess**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is u tonen hoe te configureren en eenmalige aanmelding Azure AD test met RunMyProcess op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet u weten wat de gebruiker tegenhanger in de RunMyProcess aan een gebruiker in AD Azure Azure AD is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gebruiker in RunMyProcess worden vastgesteld.

Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** toewijzen in Azure AD als de waarde van de **gebruikersnaam** in het RunMyProcess.

Als u wilt configureren en testen Azure AD single sign-on met RunMyProcess, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
3. **[Maken van een RunMyProcess gebruiker testen](#creating-a-runmyprocess-test-user)** - een tegenhanger van Britta Simon in RunMyProcess die is gekoppeld aan de weergave Azure AD van haar hebben.
4. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.


### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie, Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren van eenmalige aanmelding in uw toepassing RunMyProcess.

**Configureren van eenmalige aanmelding Azure AD met RunMyProcess, voert u de volgende stappen uit:**

1. Klik in de klassieke portal op de pagina **RunMyProcess** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.
     
    ![Eenmalige aanmelding configureren][6] 

2. Selecteer **Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op RunMyProcess** en klik op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_03.png) 

3. Voer de volgende stappen uit op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_04.png) 

    een. Typ in het tekstvak **Teken in de URL** een URL met het volgende patroon: `https://live.runmyprocess.com/live/<tenant id>`.
        
    b. Klik op **volgende**

    > [AZURE.NOTE] Houd er rekening mee dat u de waarde bijwerken met de werkelijke teken moet in de URL. Als u deze waarde, neem contact op met RunMyProcess support team via <mailto:support@runmyprocess.com>.
 
4. Klik op **Certificaat downloaden** en sla het bestand op uw computer op de pagina **configureren eenmalige aanmelding op RunMyProcess** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_05.png)

5. In een ander web browser-venster aanmelding bij uw huurder RunMyProcess als beheerder.

6. Klik op **Account** in het linkernavigatievenster, en selecteer **configuratie**.

    ![App aan eenmalige aanmelding configureren](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_001.png)

7. Ga naar de sectie **verificatiemethode** en onderstaande stappen uitvoeren:

    ![App aan eenmalige aanmelding configureren](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_002.png)

    een. Als **methode**, selecteer **eenmalige aanmelding met Samlv2**.

    b. In de **SSO omleiden** plaatsen textbox de waarde van **SAML SSO-URL** vanuit Azure AD application configuratiewizard.

    c. In de **Logout omleiden** plaatsen textbox de waarde van de **URL van de Service Single Sign-Out** vanuit Azure AD application configuratiewizard.

    d. In de **Indeling van de Id** gebracht textbox de waarde van de **Indeling van de identificatie** van Azure AD wizard configureren.

    e. De inhoud van het gedownloade bestand kopiëren en vervolgens plakken in het tekstvak van het **certificaat** . 

    f. Klik op het pictogram **Opslaan** .
    
8. In de klassieke portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.
    
    ![Azure AD Single Sign-On][10]

9. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
 
    ![Azure AD Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
Het doel van deze sectie is een om testgebruiker te maken in de portal voor klassieke Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_01.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_02.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_03.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster als volgt:  ![Azure AD test gebruikers maken](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_04.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Klik op de pagina van het dialoogvenster **Gebruikersprofiel** als volgt: ![Azure AD test gebruikers maken](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_05.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_06.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_07.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   

### <a name="creating-a-runmyprocess-test-user"></a>Maken van een gebruiker RunMyProcess test
  
Om gebruikers aan te melden bij RunMyProcess Azure AD, moeten zij worden ingericht in RunMyProcess. Bij RunMyProcess is het inrichten van een handmatige taak.

#### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw site RunMyProcess bedrijf als beheerder.

2.  Klik op **rekening** en **gebruikers** selecteren in het navigatievenster links en klik op **Nieuwe gebruiker**.

    ![Nieuwe gebruiker] (./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_003.png "Nieuwe gebruiker")

3.  Voer de volgende stappen uit in de sectie **Instellingen voor gebruikers** :

    ![Profiel] (./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_004.png "Profiel")

    een. Typ de **naam** en **E-mail** van een geldige AAD account te verrichten in de verwante tekstvakken.

    b. Selecteer een **IDE-taal**, een **taal** en een **profiel**.

    c. Selecteer de **account maken van e-mail aan mij verzenden**.

    d. Klik op **Opslaan**.

    >[AZURE.NOTE] U kunt andere RunMyProcess gebruiker account hulpmiddelen voor het maken of API's geleverd door RunMyProcess bepaling Azure Active Directory-gebruikersaccounts.
    

### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

Het doel van deze sectie wordt aan het inschakelen van Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan RunMyProcess.

![Gebruiker toewijzen][200] 

**Britta Simon aan RunMyProcess toewijzen, moet u de volgende stappen uitvoeren:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **RunMyProcess**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_50.png) 

3. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. balk aan de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.
 
Wanneer u op de tegel RunMyProcess in het Access-venster, u moet krijgen automatisch ondertekend bij uw toepassing RunMyProcess.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_205.png
