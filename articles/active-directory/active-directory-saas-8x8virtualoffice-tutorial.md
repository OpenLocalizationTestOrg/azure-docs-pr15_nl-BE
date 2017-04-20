<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met 8 x 8 Virtual Office | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en 8 x 8 Virtual Office."
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
    ms.date="10/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a>Zelfstudie: Azure Active Directory-integratie met 8 x 8 Virtual Office

Het doel van deze zelfstudie is u tonen hoe 8 x 8 Virtual Office integratie met Azure Active Directory (AD Azure).

Integratie van 8 x 8 biedt virtuele kantoor met Azure AD de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot 8 x 8 Virtual Office heeft
- U kunt gebruikers automatisch ophalen ondertekend bij 8 x 8 Virtual Office (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met 8 x 8 Virtual Office, moet u de volgende items:

- Een abonnement op Azure AD
- Een 8 x 8 Virtual Office eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Microsoft Azure AD testen in een testomgeving.

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. 8 x 8 Virtual Office toevoegen uit de galerie
2. Configureren en testen van Microsoft Azure AD Single Sign-On


## <a name="adding-8x8-virtual-office-from-the-gallery"></a>8 x 8 Virtual Office toevoegen uit de galerie
De integratie van 8 x 8 Virtual Office in Azure AD configureren, moet u 8 x 8 Virtual Office uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**8 x 8 Virtual Office toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke Portal**in het linkernavigatievenster op **Active Directory**.

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.
    
    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak, **8 x 8 Virtual Office**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_01.png)
7. **8 x 8 Virtual Office**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![De app kiezen in de galerie](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_0001.png)


##  <a name="configuring-and-testing-microsoft-azure-ad-single-sign-on"></a>Configureren en testen van Microsoft Azure AD Single Sign-On
Het doel van deze sectie is u tonen hoe te configureren en te testen Microsoft Azure AD Single Sign-On met 8 x 8 die Virtual Office op basis van een testgebruiker met de naam "Britta Simon".

Voor eenmalige aanmelding wilt werken, moet Azure AD weten wat de tegenhanger van gebruiker in 8 x 8 virtuele kantoor aan een gebruiker in AD Azure is. Met andere woorden, een relatie van de koppeling tussen een Azure AD-gebruiker en de bijbehorende gebruiker in 8 x 8 Virtual Office moet worden vastgesteld.

Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in 8 x 8 Virtual Office toewijzen.

Als u wilt configureren en testen AD Azure Microsoft Single Sign-On met 8 x 8 Virtual Office, moet u voor het voltooien van de volgende elementen:

1. **[Configureren AD Azure Microsoft Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - AD Azure Microsoft Single Sign-On met Britta Simon testen.
3. **[Een 8 x 8 Virtual Office testgebruiker maken](#creating-a-8x8-virtual-office-test-user)** - een tegenhanger van Britta Simon in 8 x 8 Virtual Office die is gekoppeld aan de weergave Azure AD van haar hebben.
4. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon AD Azure Microsoft Single Sign-On gebruiken inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-microsoft-azure-ad-single-sign-on"></a>Microsoft Azure AD voor eenmalige aanmelding configureren

In dit gedeelte AD Azure Microsoft Single Sign-On in de klassieke portal inschakelen en eenmalige aanmelding configureren in de 8 x 8 Virtual Office-toepassing.

**Configureren van Microsoft Azure AD Single Sign-On met 8 x 8 Virtual Office, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal op de pagina **8 x 8 Virtual Office** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.
     
    ![Eenmalige aanmelding configureren][6] 

2. Op de pagina **Hoe wilt u gebruikers aan te melden bij 8 x 8 Virtual Office** Selecteer **AD Azure Microsoft Single Sign-On**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_03.png) 

3. Voer de volgende stappen uit en klik op **volgende**op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_04.png)

    een. Typ in het tekstvak **URL antwoord** :`https://sso.8x8.com/saml2`

    b. Klik op **volgende**

4. Voer de volgende stappen uit en klik op **volgende**op de pagina **configureren van eenmalige aanmelding bij 8 x 8 virtueel kantoor** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_05.png)

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.

5. Aanmelding bij de huurder 8 x 8 Virtual Office als beheerder.
6. **Virtual Office Account Mgr** op toepassing deelvenster selecteren.

    ![Aan de kant App configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

7. Selecteer **Business** -account beheren en klik op de knop **Aanmelden** .

    ![Aan de kant App configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

8. Klik op het tabblad **Accounts** in de menulijst.

    ![Aan de kant App configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

9. Klik op **Eenmalige aanmelding** in de lijst met Accounts.

    ![Aan de kant App configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

10. **Signle aanmelding** onder verificatiemethode en klik op **SAML**.

    ![Aan de kant App configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

11. **SAML SSO-URL**, **één weerklinken uit de URL** en **URL van de uitgevende instelling** van Azure AD **URL aanmelden**, **Afmelden URL** en **URL van de uitgevende instelling** in 8 x 8 Virtual Office kopiëren. 

    ![Aan de kant App configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)

    ![Aan de kant App configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_007.png)

12. Klik op de knop **Browser** om te uploaden, het certificaat dat u hebt gedownload van Azure AD.

13. Klik op de knop **Opslaan** .

14. In de klassieke portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.

    ![Azure AD Single Sign-On][10]

15. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  

    ![Azure AD Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
Het doel van deze sectie is een om testgebruiker te maken in de portal voor klassieke Britta Simon genoemd.
    
![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke Portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_09.png)

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_03.png)

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_04.png)

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_05.png)

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_06.png)

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_07.png)

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_08.png)

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-a-8x8-virtual-office-test-user"></a>Een 8 x 8 Virtual Office testgebruiker maken

Het doel van deze sectie is voor het maken van een gebruiker met de naam Britta Simon in 8 x 8 virtueel kantoor. 8 x 8 Virtual Office ondersteunt het inrichten van just-in-time is standaard ingeschakeld.

Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker wordt gemaakt tijdens een poging toegang te krijgen tot 8 x 8 Virtual Office als deze nog niet bestaat. 

> [AZURE.NOTE] Als u een gebruiker handmatig te maken, moet u contact opnemen met het ondersteuningsteam van 8 x 8 Virtual Office.


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

Het doel van deze sectie wordt aan het inschakelen van Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan 8 x 8 virtueel kantoor.
    
![Gebruiker toewijzen][200]

**Britta Simon toewijzen aan 8 x 8 virtueel kantoor, kunt u de volgende stappen uitvoeren:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **8 x 8 Virtual Office**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_50.png)

3. In het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie wordt naar Microsoft Azure AD-On configuratie testen met behulp van het Configuratiescherm toegang.

Wanneer u op de tegel 8 x 8 Virtual Office in het Access-venster, u moet krijgen automatisch ondertekend bij de 8 x 8 Virtual Office-toepassing.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_205.png
