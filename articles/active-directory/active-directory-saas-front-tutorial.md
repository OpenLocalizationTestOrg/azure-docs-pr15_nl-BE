<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met voorzijde | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en de voorzijde."
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
    ms.date="10/24/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-front"></a>Zelfstudie: Azure Active Directory-integratie met de voorzijde

Het doel van deze zelfstudie is u tonen hoe voorzijde integreren met Azure Active Directory (AD Azure).

Voorzijde integreren met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot de voorzijde heeft
- U kunt gebruikers automatisch ophalen ondertekend-op naar voorgrond (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met de voorzijde, moet u de volgende items:

- Een abonnement op Azure AD
- Een voor eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD testen in een testomgeving.

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Vooraan in de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-front-from-the-gallery"></a>Vooraan in de galerie toe te voegen
De integratie van de voorzijde in Azure AD configureren, moet u vooraan in de galerie toevoegen aan uw lijst van beheerde SaaS-apps.

**Vooraan in de galerie toevoegen, voert u de volgende stappen uit:**

1. Klik in de **Azure klassieke Portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.
    
    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.
    
    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. In het zoekvak typt **Front**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-front-tutorial/tutorial_front_01.png)

7. **Front**selecteren in het resultatenpaneel en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![De app kiezen in de galerie](./media/active-directory-saas-front-tutorial/tutorial_front_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is aan hoe u configureren en testen Azure AD eenmalige aanmelding met de voorzijde op basis van een testgebruiker met de naam "Britta Simon".

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger op de voorgrond aan een gebruiker in AD Azure. Met andere woorden, moet een relatie van de koppeling tussen een Azure AD-gebruiker en de gebruiker op de voorgrond worden vastgesteld.

Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** toewijzen in Azure AD als de waarde van de **gebruikersnaam** op de voorgrond.

Als u wilt configureren en testen Azure AD single sign-on met vooraan, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
3. **[Gebruiker maken een voorzijde testen](#creating-a-front-test-user)** - een tegenhanger van Britta Simon voorop dat is gekoppeld aan de weergave Azure AD van haar hebben.
4. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie, Azure AD eenmalige aanmelding in de klassieke portal inschakelen en in uw toepassing voor eenmalige aanmelding configureren.

**Configureren van eenmalige aanmelding Azure AD aan de voorzijde, kunt u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal op **de voorpagina van toepassing-integratie** op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.
     
    ![Eenmalige aanmelding configureren][6] 

2. Op de pagina **Hoe wilt u gebruikers aan te melden op voorzijde** **Azure AD Single Sign-On**selecteren en klik op **volgende**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-front-tutorial/tutorial_front_03.png)

3. Op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** als u instellen dat de toepassing in **IDP modus gestart wilt**, voert de volgende stappen uit en klik op **volgende**:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-front-tutorial/tutorial_front_04.png)

    een. Typ in het tekstvak **id** een URL met het volgende patroon:`https://<company name>.frontapp.com`

    b. Typ in het tekstvak **URL antwoord op** een URL met het volgende patroon:`https://<company name>.frontapp.com/sso/saml/callback`

    c. Klik op **volgende**

4. Als u instellen dat de toepassing in **SP modus gestart** op de pagina van het dialoogvenster **App-instellingen configureren wilt** , klikt u op in de **'Show geavanceerde instellingen (optioneel)'** en voer vervolgens het **Teken in de URL** en klikt u op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-front-tutorial/tutorial_front_05.png)

    een. Typ in het tekstvak **Teken in de URL** een URL met het volgende patroon:`https://<company name>.frontapp.com`

    b. Klik op **volgende**

    > [AZURE.NOTE] Opmerking: dit zijn niet de werkelijke waarden. U moet deze waarden bijwerken met de werkelijke teken in de URL, id en URL antwoord. Als u deze waarden, kunt u **stap 12** raadplegen voor meer informatie of neem contact op met de voorzijde via [support@frontapp.com](emailTo:support@frontapp.com).

5. Voer de volgende stappen uit op de pagina **configureren eenmalige aanmelding vooraan** en klikt u op **volgende**:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-front-tutorial/tutorial_front_06.png)

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.

6. Aanmelding bij de huurder voorzijde als beheerder.

7. Ga naar **instellingen (tandwiel pictogram onderaan in de linker zijbalk) > Voorkeuren voor**.

    ![Single Sign-On op App kant configureren](./media/active-directory-saas-front-tutorial/tutorial_front_000.png)

8. Klik op de koppeling voor **Eenmalige aanmelding** .

    ![Single Sign-On op App kant configureren](./media/active-directory-saas-front-tutorial/tutorial_front_001.png)

9. **SAML** selecteren in de vervolgkeuzelijst van **Eenmalige aanmelding**.

    ![Single Sign-On op App kant configureren](./media/active-directory-saas-front-tutorial/tutorial_front_002.png)

10. In het **Ingangspunt** plaatsen textbox de waarde van **Single Sign-on Service URL** vanuit Azure AD application configuratiewizard.

    ![Single Sign-On op App kant configureren](./media/active-directory-saas-front-tutorial/tutorial_front_003.png)

11. De inhoud van het gedownloade bestand kopiëren en vervolgens plakken in het tekstvak van het **handtekeningcertificaat** .

    ![Single Sign-On op App kant configureren](./media/active-directory-saas-front-tutorial/tutorial_front_004.png)

12. Controleer of deze URL overeenkomt met de configuratie die u in stap 3.

    ![Single Sign-On op App kant configureren](./media/active-directory-saas-front-tutorial/tutorial_front_005.png)

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

    ![Azure AD test gebruikers maken](./media/active-directory-saas-front-tutorial/create_aaduser_09.png)

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-front-tutorial/create_aaduser_03.png)

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-front-tutorial/create_aaduser_04.png)

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-front-tutorial/create_aaduser_05.png)

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-front-tutorial/create_aaduser_06.png)

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-front-tutorial/create_aaduser_07.png)

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-front-tutorial/create_aaduser_08.png)

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-a-front-test-user"></a>Maken van een test voor gebruiker

Het doel van deze sectie is voor het maken van een gebruiker met de naam Britta Simon in Front.Please werken met het ondersteuningsteam voor de gebruikers in de Front-account toevoegen.

### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

Het doel van deze sectie wordt aan het inschakelen van Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan voorzijde.
    
![Gebruiker toewijzen][200]

**Britta Simon toewijzen aan de voorzijde, kunt u de volgende stappen uitvoeren:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.
    
    ![Gebruiker toewijzen][201]

2. Selecteer **Front**in de lijst toepassingen.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-front-tutorial/tutorial_front_50.png)

1. In het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Gebruiker toewijzen][203]

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.
    
    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.
 
Wanneer u op de tegel vooraan in het Access-venster, u moet krijgen automatisch ondertekend bij uw toepassing voor.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-front-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-front-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-front-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-front-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-front-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-front-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-front-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-front-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-front-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-front-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-front-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-front-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-front-tutorial/tutorial_general_205.png
