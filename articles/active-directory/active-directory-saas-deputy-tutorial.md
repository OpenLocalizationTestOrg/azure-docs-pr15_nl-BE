<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Adjunct | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en adjunct."
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
    ms.date="09/28/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-deputy"></a>Zelfstudie: Azure Active Directory-integratie met Adjunct

Het doel van deze zelfstudie is u tonen hoe Adjunct integreren met Azure Active Directory (AD Azure).

Adjunct integreren met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot de Adjunct heeft
- U kunt gebruikers automatisch ophalen ondertekend bij Adjunct (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met Adjunct, moet u de volgende items:

- Een abonnement op Azure AD
- Een plaatsvervangend eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD testen in een testomgeving.

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Adjunct toevoegen uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-deputy-from-the-gallery"></a>Adjunct toevoegen uit de galerie
De integratie van adjunct in Azure AD configureren, moet u Adjunct uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Adjunct toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke Portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.
    
    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.
    
    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Adjunct**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_01.png)

7. **Adjunct**selecteren in het resultatenpaneel en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![De app kiezen in de galerie](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is u tonen hoe te configureren en eenmalige aanmelding Azure AD test met plaatsvervanger op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in Adjunct aan een gebruiker in AD Azure. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de bijbehorende gebruiker in Adjunct worden vastgesteld.

Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** toewijzen in Azure AD als de waarde van de **gebruikersnaam** in het Adjunct.

Als u wilt configureren en testen Azure AD single sign-on met Adjunct, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
3. **[Maken van een Adjunct-directeur gebruiker testen](#creating-a-deputy-test-user)** - een tegenhanger van Britta Simon in Adjunct die is gekoppeld aan de weergave Azure AD van haar hebben.
4. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie, Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Adjunct.

**Azure AD eenmalige aanmelding configureren met Adjunct, voert u de volgende stappen uit:**

1. Klik in de klassieke portal op de pagina **Adjunct** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.
     
    ![Eenmalige aanmelding configureren][6] 

2. Op de pagina **Hoe wilt u dat gebruikers aanmelden bij Adjunct** **Azure AD Single Sign-On**selecteren en klik op **volgende**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_03.png)

3. Op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** als u instellen dat de toepassing in **IDP modus gestart wilt**, voert de volgende stappen uit en klik op **volgende**:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_04.png)

    een. Typ in het tekstvak **id** een URL met het volgende patroon: `https://<your-subdomain>.<region>.deputy.com`.

    b. Typ in het tekstvak **URL antwoord op** een URL met het volgende patroon: `https://<your-subdomain>.<region>.deputy.com/exec/devapp/samlacs`.

    c. Klik op **volgende**.

4. Als u instellen dat de toepassing in **SP modus gestart** op de pagina van het dialoogvenster **App-instellingen configureren wilt** , klikt u op in de **'Show geavanceerde instellingen (optioneel)'** en voer vervolgens het **Teken in de URL** en klikt u op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_05.png)

    een. Typ in het tekstvak **Teken in de URL** een URL met het volgende patroon: `https://<your-subdomain>.<region>.deputy.com`.

    b. Klik op **volgende**.

    > [AZURE.NOTE] Adjunct regio achtervoegsel opitional is of zou moeten uitgaan van een van de volgende: au | NB | EU | als | la | af | een | ent au | ent na | ent eu | ent-als | ent-la | ent af | ent-een

5. Voer de volgende stappen uit en klikt u op **volgende**op de pagina **configureren eenmalige aanmelding bij de Adjunct** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_06.png)

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer.

    
6. Ga naar de volgende URL: https://(your-subdomain).deputy.com/exec/config/system_config. Ga naar **Instellingen** en klik op **bewerken**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_004.png)

7. Kopieer de SAML SSO-URL in de Azure klassieke portal op de Configure eenmalige aanmelding Adjunct-pagina. 

8. Op deze pagina **Beveiligingsinstellingen** uitvoeren van onderstaande stappen te volgen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_005.png)

    een. **Sociale aanmelding**inschakelen.

    b. Uw Base64-gecodeerde certificaat openen in Kladblok, de inhoud ervan kopiëren naar het Klembord en plak deze op het tekstvak **OpenSSL certificaat** .

    c. Typ in het tekstvak URL van de SSO-SAM`https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`
    
    d. Vervang in het tekstvak URL van de SSO-SAM `<your subdomain>` met een subdomein.

    e. Vervang in het tekstvak URL van de SSO-SAM `<saml sso url>` met de SAML SSO-URL die u hebt gekopieerd van de klassieke Azure portal.

    f. Klik op **Instellingen opslaan**.

9. In de klassieke portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.
    
    ![Azure AD Single Sign-On][10]

10. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
    
    ![Azure AD Single Sign-On][11]

### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
Het doel van deze sectie is een om testgebruiker te maken in de portal voor klassieke Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke Portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-deputy-tutorial/create_aaduser_09.png)

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-deputy-tutorial/create_aaduser_03.png)

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-deputy-tutorial/create_aaduser_04.png)

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-deputy-tutorial/create_aaduser_05.png)

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-deputy-tutorial/create_aaduser_06.png)

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-deputy-tutorial/create_aaduser_07.png)

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-deputy-tutorial/create_aaduser_08.png)

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-a-deputy-test-user"></a>Maken van een gebruiker Adjunct-test

Om gebruikers aan te melden bij Adjunct Azure AD, moeten ze worden ingericht in Adjunct. Plaatsvervanger is inrichten een handmatige taak.

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw site Adjunct bedrijf als beheerder.

2.  Klik in het bovenste navigatievenster **mensen**.

    ![Mensen] (./media/active-directory-saas-deputy-tutorial/tutorial_deputy_001.png "Mensen")

3.  Klik op de knop **Mensen toevoegen** en klikt u op **één persoon toevoegen**.

    ![Personen toevoegen] (./media/active-directory-saas-deputy-tutorial/tutorial_deputy_002.png "Personen toevoegen")

4.  Voer de volgende stappen uit en klik op **Opslaan en uitnodigen**.

    ![Nieuwe gebruiker] (./media/active-directory-saas-deputy-tutorial/tutorial_deputy_003.png "Nieuwe gebruiker")

    een. Typ in het tekstvak **naam** **Britta** en **Simon**.  

    b. Typ in het tekstvak **e-mailadres** het e-mailadres van een Azure AD-account die u verrichten wilt.

    c. Typ in het tekstvak **werk op basis van** de naam bussniess.

    d. Klik op de knop **Opslaan & uitnodigen** .

    >[AZURE.NOTE]De accounthouder AAD ontvangt een e-mail en een link om te bevestigen hun account voordat deze actief wordt als volgt. Kunt u een andere Adjunct gebruiker account hulpmiddelen voor het maken of API's geleverd door Adjunct bepaling AAD gebruikersaccounts.


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

Het doel van deze sectie wordt aan het inschakelen van Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan Adjunct.
    
![Gebruiker toewijzen][200]

**Britta Simon naar Adjunct toewijzen, moet u de volgende stappen uitvoeren:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.
    
    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen **Adjunct**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_50.png)

3. In het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. Klik op **toewijzen**op de werkbalk aan de onderkant.
    
    ![Gebruiker toewijzen][205]

### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.
 
Wanneer u op de tegel Adjunct in het Access-venster, u moet krijgen automatisch ondertekend bij de Adjunct-toepassing.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_205.png
