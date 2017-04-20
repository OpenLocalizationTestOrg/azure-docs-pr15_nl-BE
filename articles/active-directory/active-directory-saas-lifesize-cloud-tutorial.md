<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Lifesize wolk | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Lifesize wolk."
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
    ms.date="10/04/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a>Zelfstudie: Azure Active Directory-integratie met Lifesize wolk

In deze zelfstudie leert u hoe Lifesize wolk integratie met Azure Active Directory (AD Azure).

Lifesize wolk integratie met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot de Lifesize wolk heeft
- U kunt gebruikers automatisch ophalen ondertekend bij Lifesize wolk (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met Lifesize wolk, moet u de volgende items:

- Een abonnement op Azure AD
- Een Lifesize wolk eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving.

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Lifesize wolk uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-lifesize-cloud-from-the-gallery"></a>Lifesize wolk uit de galerie toe te voegen.
De integratie van de Lifesize Cloud in Azure AD configureren, moet u Lifesize wolk uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Lifesize wolk toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Active Directory][1]
2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Lifesize wolk**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_01.png)

7. **Lifesize wolk**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en test Azure AD eenmalige aanmelding met Lifesize wolk op basis van een testgebruiker met de naam "Britta Simon".

Voor eenmalige aanmelding wilt werken, moet AD Azure weet wat de gebruiker tegenhanger van Lifesize wolk is aan een gebruiker in AD Azure. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gebruiker in de Lifesize Cloud worden vastgesteld.

Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in de Lifesize Cloud toewijzen.

Als u wilt configureren en testen Azure AD single sign-on met Lifesize wolk, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
3. **[Maken van een wolk Lifesize gebruiker testen](#creating-a-lifesize-cloud-test-user)** - een tegenhanger van Britta Simon in Lifesize wolk die is gekoppeld aan de weergave Azure AD van haar hebben.
4. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In dit gedeelte Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren op in uw toepassing Lifesize wolk.


**Configureren van eenmalige aanmelding Azure AD met Lifesize wolk, voert u de volgende stappen uit:**

1. Klik in de klassieke portal op de pagina **Lifesize wolk** toepassing integratie **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.
     
    ![Eenmalige aanmelding configureren][6] 

2. Op de pagina **Hoe wilt u dat gebruikers aanmelden bij Lifesize wolk** **Azure AD Single Sign-On**selecteren en klik op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_03.png) 

3. Voer de volgende stappen uit op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_04.png) 

    een. In het **Teken op URL** -tekstvak typt u de URL die wordt gebruikt door gebruikers voor aanmelding bij uw Lifesize wolk toepassing met behulp van het volgende patroon: **https://login.lifesizecloud.com/ls/?acs**.
    
    b. Klik op **volgende**
 
4. Op de pagina **configureren eenmalige aanmelding bij Lifesize wolk** , kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_05.png)

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


5. Voor eenmalige aanmelding configureren voor uw toepassing, aanmelding in de toepassing Lifesize wolk met beheerdersbevoegdheden.

6. Klik op uw naam in de rechterbovenhoek en klik op in de **Geavanceerde instellingen**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

7. In de geavanceerde instellingen nu Klik op de koppeling van de **SSO-configuratie** . Hiermee opent u de configuratiepagina van eenmalige aanmelding voor uw exemplaar.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

8. Nu de volgende waarden in de configuratie van de SSO-gebruikersinterface configureren.    

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)

    • De waarde van de URL van de uitgevende instelling uit Azure AD kopiëren en plakken die in een tekstvak **Identity Provider uitgever** .

    • De waarde van de externe aanmeldings-URL uit Azure AD kopiëren en plakken die in een tekstvak **Aanmeldings-URL** .

    • Het gedownloade certificaat openen in Kladblok en kopieer de inhoud van het certificaat, met uitzondering van de lijnen beginnen met het certificaat en het certificaat einde, plak deze in het tekstvak **X.509-certificaat** .

    • In de toewijzing van de SAML-kenmerk voor het **eerste** vak de waarde opgeeft als **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**

    • De toewijzing SAML-kenmerk voor het tekstvak **Achternaam** in de waarde opgeeft als **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**

    • De toewijzing SAML-kenmerk voor het tekstvak **e-mailadres** in de waarde opgeeft als **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**

9. Als u wilt controleren van de configuratie kunt u op de knop **testen** .

    > [AZURE.NOTE] Voor succesvolle testen moet u de configuratiewizard te voltooien in Azure AD en ook toegang bieden tot gebruikers of groepen die de test kunnen uitvoeren.
    
10. De eenmalige aanmelding inschakelen door te controleren op de knop voor **Eenmalige aanmelding inschakelen** .

11. Klik nu op de knop **bijwerken** zodat alle instellingen worden opgeslagen. Hierdoor wordt de waarde RelayState gegenereerd. De RelayState-waarde die wordt gegenereerd in het tekstvak kopiëren. Moeten we deze waarde in de volgende stappen.

12. In de klassieke portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.
    
    ![Azure AD Single Sign-On][10]

13. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
 
    ![Azure AD Single Sign-On][11]

14. Nu aanmelden in de Azure Management Portal **https://portal.azure.com** met de admin-referenties

15. Klik op de link **Meer Services** in het linkernavigatievenster
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_09.png)

16. Azure Active Directory en klik op de koppeling **Azure Active Directory** zoeken
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_10.png)

17. U vindt uw SaaS-toepassingen onder de knop **Bedrijfstoepassingen** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_11.png)

18. Klik nu op de koppeling van **Alle toepassingen** in het volgende blad
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_12.png)

19. Lifesize-toepassing die u wilt instellen van de RelayState zoeken. 
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_13.png)

20. Klik nu op **Single sign-on** koppeling in de blade

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_14.png)

21. Ziet u het selectievakje **weergeven geavanceerde instellingen voor URL** . Schakel het selectievakje in.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_15.png)
    
22. Nu de RelayState voor de toepassing die u kunt in de configuratiepagina van Lifesize toepassing SSO zien configureren. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_16.png)

23. De instellingen opslaan.

### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.


![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster als volgt:  ![Azure AD test gebruikers maken](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Klik op de pagina van het dialoogvenster **Gebruikersprofiel** als volgt: ![Azure AD test gebruikers maken](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-an-lifesize-cloud-test-user"></a>Een Lifesize wolk testgebruiker maken

In deze sectie kunt u een gebruiker met de naam Britta Simon in Lifesize wolk. Lifesize cloud biedt ondersteuning voor Automatische gebruikersaanvragen. Na een succesvolle verificatie op Azure AD de gebruiker automatisch ingericht in de toepassing. 


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

In dit gedeelte vindt inschakelen u Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan Lifesize wolk.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Lifesize wolk, kunt u de volgende stappen uitvoeren:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Lifesize wolk**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_50.png) 

3. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

In dit gedeelte vindt u Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Wanneer u op de tegel Lifesize wolk in het Access-venster, u moet krijgen automatisch ondertekend bij uw toepassing Lifesize wolk.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_205.png
