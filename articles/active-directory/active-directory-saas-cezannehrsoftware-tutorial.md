<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Cezanne HR Software | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Cezanne HR Software."
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
    ms.date="10/26/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-cezanne-hr-software"></a>Zelfstudie: Azure Active Directory-integratie met Cezanne HR-Software

Het doel van deze zelfstudie is u tonen hoe Cezanne HR-Software integreren met Azure Active Directory (AD Azure).

Cezanne HR-Software integreren met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot Cezanne HR-Software heeft
- U kunt gebruikers automatisch ophalen ondertekend bij Cezanne HR Software (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met Cezanne HR-Software, moet u de volgende items:

- Een abonnement op Azure AD
- Een HR-Software Cezanne eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD testen in een testomgeving.

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Cezanne HR Software uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-cezanne-hr-software-from-the-gallery"></a>Cezanne HR Software uit de galerie toe te voegen.
De integratie van Cezanne HR Software in Azure AD configureren, moet u Cezanne HR Software uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Cezanne HR Software toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke Portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.
    
    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.
    
    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Cezanne HR Software**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_01.png)

7. In het resultatenpaneel **Cezanne HR Software**te selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![De app kiezen in de galerie](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is u tonen hoe te configureren en eenmalige aanmelding Azure AD test met Cezanne HR Software op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in Cezanne HR Software aan een gebruiker in AD Azure. Met andere woorden, moet een relatie van de koppeling tussen een Azure AD-gebruiker en de gebruiker in Cezanne HR Software worden vastgesteld.

Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in het Cezanne HR Software toewijzen.

Als u wilt configureren en testen Azure AD eenmalige aanmelding met Cezanne HR-Software, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
3. **[Maken van een HR-Software van Cezanne gebruiker testen](#creating-a-cezanne-hr-software-test-user)** - een tegenhanger van Britta Simon in Cezanne HR-Software die is gekoppeld aan de weergave Azure AD van haar hebben.
4. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie, Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Cezanne HR.

**Configureren van eenmalige aanmelding Azure AD met Cezanne HR-Software, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal op de pagina **Cezanne HR Software** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.
     
    ![Eenmalige aanmelding configureren][6] 

2. Op de pagina **Hoe wilt u dat gebruikers aanmelden bij Cezanne HR Software** Selecteer **Azure AD Single Sign-On**en klik vervolgens op **volgende**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_03.png)

3. Voer de volgende stappen uit en klik op **volgende**op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_04.png)

    een. Typ in het tekstvak **Teken in de URL** een URL met het volgende patroon: `https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`.

    b. Typ in het tekstvak **URL antwoord op** een URL met het volgende patroon: `https://w3.cezanneondemand.com:443/CezanneOnDemand/-/<tenant id>/Saml/samlp`.

    c. Klik op **volgende**

    > [AZURE.NOTE] Houd er rekening mee dat u deze waarden bijwerken met de werkelijke teken op URL en URL antwoord hebben. Deze waarden ophalen, neem contact op met het ondersteuningsteam Cezanne HR Software via <mailto:info@cezannehr.com>.

4. Voer de volgende stappen uit en klik op **volgende**op de pagina **configureren eenmalige aanmelding bij Cezanne HR Software** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_05.png)

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.

5. In een ander web browser-venster aanmelding bij uw huurder Cezanne HR-Software als een beheerder.

6. Klik op het linkernavigatievenster op **Systeeminstellingen**. Ga naar de **Beveiligingsinstellingen**. Ga naar de **Single Sign-On-configuratie**.

    ![Single Sign-On op App kant configureren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

7. In de **externe gebruikers zich aanmelden met de volgende Single Sign On (SSO) Service** deelvenster **SAML 2.0** het selectievakje en selecteert u de optie **Geavanceerde configuratie** ernaast.

    ![Single Sign-On op App kant configureren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

8. Klik op **Nieuwe toevoegen** .

    ![Single Sign-On op App kant configureren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

9. De volgende stappen uitvoeren in de sectie **Identiteit aanbieders van SAML 2.0** .

    ![Single Sign-On op App kant configureren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)

    een. Voer de naam van de Provider van uw identiteit als de **Weergavenaam**.

    b. In de **Entiteit-id** gebracht textbox de waarde van de **Entiteit-ID** van Azure AD wizard configureren.

    c. Wijzig de **SAML-Binding** 'Boeken'.

    d. In het **Security Token Service-eindpunt** geplaatst textbox de waarde van **Single Sign-on Service URL** vanuit Azure AD application configuratiewizard.

    e. 'Http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name' in het **id-kenmerk van gebruikersnaam**invoeren.

    f. Klik op pictogram **uploaden** als u wilt uploaden van de gedownloade certificaat van Azure AD.

    g. Klik op **Ok** . 

10. Klik op de knop **Opslaan** .

    ![Single Sign-On op App kant configureren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

11. In de klassieke portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.
    
    ![Azure AD Single Sign-On][10]

12. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
    
    ![Azure AD Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
Het doel van deze sectie is een om testgebruiker te maken in de portal voor klassieke Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke Portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_09.png)

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png)

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png)

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_05.png)

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_06.png)

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Typ in het tekstvak **Achternaam** **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_07.png)

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_08.png)

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-a-cezanne-hr-software-test-user"></a>Maken van een gebruiker Cezanne HR Software testen

Om gebruikers aan te melden bij Cezanne HR Software AD Azure, moeten zij worden ingericht in Cezanne HR Software. Bij Cezanne HR Software is het inrichten van een handmatige taak.

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw site Cezanne HR Software bedrijf als beheerder.

2.  Klik op het linkernavigatievenster op **Systeeminstellingen**. Het gaat om gebruikers te **beheren**. Ga naar de **Nieuwe gebruiker toevoegen**.

    ![Nieuwe gebruiker] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "Nieuwe gebruiker")

3.  In de sectie **Informatie over de persoon** , voert u onderstaande stappen te volgen:

    ![Nieuwe gebruiker] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "Nieuwe gebruiker")

    een. **Interne gebruiker** instellen als OFF.

    b. Typ in het tekstvak **Voornaam** **Britta**.  

    c. Typ in het tekstvak **Achternaam** **Simon**.

    d. Typ in het tekstvak **E-mail** het e-mailadres van de account Britta Simon.

4.  In de sectie **Accountgegevens** voert u onderstaande stappen te volgen:

    ![Nieuwe gebruiker] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "Nieuwe gebruiker")

    een. Typ in het tekstvak **gebruikersnaam** het e-mailadres van Britta Simon.

    b. Typ in het tekstvak **wachtwoord** het wachtwoord van de account Britta Simon.

    c. **HR-Professional** als de **beveiligingsrol**selecteren.

    d. Klik op **OK**.

5. Ga naar **- Op** het tabblad en selecteer **Nieuwe toevoegen** in het gebied **-Id's van SAML 2.0** .

    ![Gebruiker] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "Gebruiker")

6. Kies uw Provider identiteit voor de **Id-Provider** en in het tekstvak van de **Gebruikers-id**, het e-mailadres van de account Britta Simon.

    ![Gebruiker] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "Gebruiker")
    
7. Klik op de knop **Opslaan** .

    ![Gebruiker] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "Gebruiker")


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

Het doel van deze sectie wordt aan het inschakelen van Britta Simon voor de Azure eenmalige aanmelding toegang verlenen tot Cezanne HR Software.
    
![Gebruiker toewijzen][200]

**Britta Simon Cezanne HR software toewijzen, moet u de volgende stappen uitvoeren:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.
    
    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen **Cezanne HR Software**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_50.png)

3. In het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. Klik op **toewijzen**op de werkbalk aan de onderkant.
    
    ![Gebruiker toewijzen][205]

### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.
 
Wanneer u op de tegel Cezanne HR-Software in het Configuratiescherm toegang, u moet krijgen automatisch ondertekend bij uw toepassing Cezanne HR Software.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_205.png
