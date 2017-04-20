<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met herkennen | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en herkennen."
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
    ms.date="10/27/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-recognize"></a>Zelfstudie: Azure Active Directory-integratie met herkennen

Het doel van deze zelfstudie is u tonen hoe te herkennen met Azure Active Directory (AD Azure) integreren.

Herkennen integreren met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot herkennen heeft
- U kunt gebruikers automatisch ophalen ondertekend bij herkennen (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met herkennen, moet u de volgende items:

- Een abonnement op Azure AD
- Een herkennen eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD testen in een testomgeving.

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Herkennen uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-recognize-from-the-gallery"></a>Herkennen uit de galerie toe te voegen.
De integratie van herkennen in Azure AD configureren, moet u herkennen in de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Om te herkennen in de galerie hebt toegevoegd, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke Portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.
    
    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.
    
    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak, **herkennen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_01.png)

7. Selecteer **herkennen**in het resultatenpaneel en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![De app kiezen in de galerie](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is u tonen hoe te configureren en eenmalige aanmelding Azure AD test met herkennen op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in herkennen aan een gebruiker in AD Azure. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de bijbehorende gebruiker in herkennen worden vastgesteld.

Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** toewijzen in Azure AD als de waarde van de **gebruikersnaam** in het herkennen.

Als u wilt configureren en testen Azure AD single sign-on met herkennen, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
3. **[Gebruikers maken een herkennen test](#creating-a-recognize-test-user)** - hebben een tegenhanger van Britta Simon in herkennen die is gekoppeld aan de weergave Azure AD van haar.
4. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie, Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren van eenmalige aanmelding in uw toepassing herkennen.

**Configureren van eenmalige aanmelding Azure AD met herkennen, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal op de pagina **herkennen** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.
     
    ![Eenmalige aanmelding configureren][6] 

2. Op de pagina **Hoe wilt u dat gebruikers aanmelden bij herkennen** **Azure AD Single Sign-On**selecteren en klik op **volgende**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_03.png)

3. Voer de volgende stappen uit en klik op **volgende**op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_04.png)

    een. Typ in het tekstvak **Teken in de URL** een URL met het volgende patroon: `https://recognizeapp.com/<your-domain>/saml/sso`.

    b. Typ in het tekstvak **id** een URL met het volgende patroon: `https://recognizeapp.com/<your-domain>/saml/metadata`.

    c. Klik op **volgende**

    > [AZURE.NOTE] Als u niet over deze URL's weet, typt u de voorbeeld-URL's met voorbeeld patroon. Als u deze waarden, kunt u stap 9 raadplegen voor meer informatie of neem contact op met het ondersteuningsteam herkennen via <mailto:support@recognizeapp.com>.

4. Klik op **certificaat downloaden** en sla het bestand op uw computer op de pagina **configureren eenmalige aanmelding te herkennen** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_05.png)

5. In een ander web browser-venster aanmelding bij uw huurder herkennen als een beheerder.

6. Klik op **Menu**in de rechterbovenhoek. Ga naar de **systeembeheerder van het bedrijf**.

    ![Single Sign-On op App kant configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_000.png)

7. Klik op **Instellingen**in het linkernavigatievenster.

    ![Single Sign-On op App kant configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_001.png)

8. De volgende stappen uitvoeren in de sectie **Instellingen voor eenmalige aanmelding** .

    ![Single Sign-On op App kant configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_002.png)

    een. **Eenmalige aanmelding inschakelen** **op**selecteren.

    b. In de **Entiteit-ID IDP** gebracht textbox de waarde van de **URL van de uitgevende instelling** van Azure AD wizard configureren.

    c. In de **SSO-doel-url** plaatsen textbox de waarde van **Single Sign-On Service URL** vanuit Azure AD application configuratiewizard.

    d. In de **doel-url Slo** plaatsen textbox de waarde van de **URL van de Service Single Sign-Out** vanuit Azure AD application configuratiewizard.

    e. Open het gedownloade bestand in Kladblok en plak deze vervolgens naar het tekstvak van het **certificaat** de inhoud ervan kopiëren naar het Klembord. 

    f. Klik op de knop **Instellingen opslaan** . 

9. Kopieer de URL onder **Service Provider metagegevens url**naast de sectie **Instellingen voor eenmalige aanmelding** .
    
    ![Single Sign-On op App kant configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_003.png)

10. Open de **metagegevens URL-koppeling** onder een browser leeg om de metagegevensdocument te downloaden. Gebruik vervolgens de waarde van de EntityDescriptor herkennen die u voor de **id** in het dialoogvenster **Toepassingsinstellingen configureren** .

    ![Single Sign-On op App kant configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_004.png)

11. In de klassieke portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.
    
    ![Azure AD Single Sign-On][10]

12. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
    
    ![Azure AD Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
Het doel van deze sectie is een om testgebruiker te maken in de portal voor klassieke Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke Portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-recognize-tutorial/create_aaduser_09.png)

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-recognize-tutorial/create_aaduser_03.png)

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-recognize-tutorial/create_aaduser_04.png)

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-recognize-tutorial/create_aaduser_05.png)

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-recognize-tutorial/create_aaduser_06.png)

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Typ in het tekstvak **Achternaam** **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-recognize-tutorial/create_aaduser_07.png)

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-recognize-tutorial/create_aaduser_08.png)

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-a-recognize-test-user"></a>Maken van een gebruiker herkennen test

Om gebruikers aan te melden bij herkennen Azure AD, moeten zij worden ingericht in herkennen. Herkennen is inrichten een handmatige taak.

Deze toepassing ondersteunt geen SCIM ingericht maar heeft een andere gebruiker synchronisatie die gebruikers bepalingen. 

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw site herkennen bedrijf als beheerder.

2.  Klik op **Menu**in de rechterbovenhoek. Ga naar de **systeembeheerder van het bedrijf**.

3.  Klik op **Instellingen**in het linkernavigatievenster.

4.  De volgende stappen uitvoeren in de sectie **Gebruiker synchroniseren** .
    
    ![Nieuwe gebruiker] (./media/active-directory-saas-recognize-tutorial/tutorial_recognize_005.png "Nieuwe gebruiker")

    een. Als de **Synchronisatie is ingeschakeld**, selecteert u **op**.

    b. Als **synchronisatie-provider kiezen**, selecteert u **Microsoft / Office 365**.

    c. Klik op **gebruiker synchronisatie uitvoeren**

### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

Het doel van deze sectie wordt aan het inschakelen van Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan herkennen.
    
![Gebruiker toewijzen][200]

**Britta Simon toewijzen om te herkennen, kunt u de volgende stappen uitvoeren:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.
    
    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen **herkennen**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_50.png)

3. In het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. Klik op **toewijzen**op de werkbalk aan de onderkant.
    
    ![Gebruiker toewijzen][205]

### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.
 
Wanneer u op de tegel herkennen in het Access-venster, u moet krijgen automatisch ondertekend bij uw toepassing herkennen.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_205.png
