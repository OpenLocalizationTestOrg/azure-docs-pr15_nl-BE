<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Tableau Server | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en de Server Tableau."
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


# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Zelfstudie: Azure Active Directory-integratie met Tableau-Server

Het doel van deze zelfstudie is u tonen hoe Tableau Server integreren met Azure Active Directory (AD Azure).

Tableau Server integreren met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot de Server Tableau heeft
- U kunt gebruikers automatisch ophalen ondertekend voor toegang tot Server Tableau (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met Tableau-Server, moet u de volgende items:

- Een abonnement op Azure AD
- Een Tableau Server eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD testen in een testomgeving. 

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Tableau-Server toe te voegen in de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-tableau-server-from-the-gallery"></a>Tableau-Server toe te voegen in de galerie
De integratie van Tableau Server in Azure AD configureren, moet u Tableau-Server uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Tableau als Server wilt toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 
 
    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Tableau-Server**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_01.png)

7. **Tableau Server**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![De app kiezen in de galerie](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is u tonen hoe te configureren en eenmalige aanmelding Azure AD test met Tableau-Server op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in Tableau-Server aan een gebruiker in AD Azure. Met andere woorden, moet een relatie koppeling tussen een Azure AD-gebruiker en de gebruiker in Tableau-Server tot stand worden gebracht.

Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in het Tableau-Server toewijzen.

Als u wilt configureren en testen Azure AD single sign-on met Tableau-Server, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Gebruikers maken een Tableau Server testen](#creating-a-tableauserver-test-user)** - hebben een tegenhanger van Britta Simon in Tableau-Server die is gekoppeld aan de weergave Azure AD van haar.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van deze sectie is Azure AD eenmalige aanmelding in de klassieke Azure portal inschakelen en configureren op in uw toepassing Tableau Server.

Tableau-servertoepassing verwacht de SAML-bevestigingen in een specifieke indeling. De volgende schermafbeelding ziet u een voorbeeld voor dit. 

![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_51.png) 

**Configureren van eenmalige aanmelding Azure AD Tableau-server, moet u de volgende stappen uitvoeren:**


1. In de klassieke Azure portal, op de pagina **Server Tableau** application integration, in het menu aan de bovenkant, klikt u op **kenmerken**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_81.png) 


1. Voer de volgende stappen uit in het dialoogvenster **SAML-token kenmerken** :

    

    een. Klik op **gebruikerskenmerk toevoegen** om te openen, het dialoogvenster **Gebruiker-Attribure toevoegen** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_82.png) 


    b. Typ de **gebruikersnaam**in het tekstvak **Naam Attrubute** .

    c. In de lijst **Waarde van het kenmerk** selsect **user.displayname**.

    d. Klik op **Voltooien**.  
    



1. In het menu aan de bovenkant, klikt u op de **Werkbalk Snel starten**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_83.png)  








1. Klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren][6] 



2. Klik op de pagina **Hoe wilt u dat gebruikers aan te melden op de Server Tableau** **Azure AD Single Sign-On**selecteren en klik op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_03.png) 


3. Voer de volgende stappen uit en klik op **volgende**op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_04.png) 



    een. Typ in het tekstvak **Teken In de URL** de URL van uw server Tableau. 

    b. Vak kopie in de id voor de 

    c. Klik op **volgende**


4. Voer de volgende stappen uit en klik op **volgende**op de pagina **configureren eenmalige aanmelding op de Server Tableau** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_05.png) 


    een. Klik op **metagegevens downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


6. Als u eenmalige aanmelding configureren voor uw toepassing, moet u eenmalige aanmelding aan de huurder Tableau Server als beheerder.

    een. Klik op het tabblad **SAML** in de configuratie van de Server Tableau.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_001.png) 


    b. Schakel het selectievakje van **SAML voor eenmalige aanmelding gebruiken**.

    c. Zoek uw metagegevens Federation-bestand gedownload van Azure klassieke portal en upload deze in het **bestand met metagegevens SAML Idp**.

    d. Tableau Server retour-URL, de URL die Tableau Server-gebruikers toegang, zoals http://tableau_server tot. Gebruikmaking van http://localhost wordt niet aanbevolen. Via een URL met een slash (bijvoorbeeld http://tableau_server/) wordt niet ondersteund. **Tableau Server retour-URL** kopiëren en plakken naar Azure AD **Sign in URL** textbox, zoals in de stap 3

    e. SAML entiteits-ID, de entiteit-ID is uniek voor de serverinstallatie van Tableau naar de IdP. Hier geeft u de URL van de Server Tableau opnieuw, als u dat wilt, maar het hoeft niet te worden van het Tableau Server-URL. **SAML entiteits-ID** kopiëren en plakken naar Azure AD **id** textbox, zoals in de stap 3.

    f. Klik op het **Bestand met metagegevens exporteren** en open het in de toepassing tekst editor. Ga naar URL van de consument bevestiging met Http Post en Index 0 en kopieer de URL. Nu plakken naar Azure AD **Antwoord URL** textbox zoals in stap 3. 

    g. Klik op de knop **OK** op de pagina Tableau Server Configiuration.

    > [AZURE.NOTE] Hulp SAML op Tableau Server configureren vervolgens raadpleegt u dit artikel [SAML configureren](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm) 

6. In de klassieke Azure portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.

    ![Azure AD Single Sign-On][10]

7. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** . 
 
    ![Azure AD Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
Het doel van deze sectie is voor het maken van een testgebruiker in Azure klassieke portal Britta Simon genoemd.

Selecteer in de lijst gebruikers **Britta Simon**.

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
 
    ![Azure AD test gebruikers maken](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_03.png) 


4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_04.png)

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_05.png) 

    een. Selecteer **Type van gebruiker**, **nieuwe gebruiker in uw organisatie**.

    b. Typ in het tekstvak **Gebruikersnaam** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :

    ![Azure AD test gebruikers maken](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_07.png) 


8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:
 
    ![Azure AD test gebruikers maken](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-a-tableau-server-test-user"></a>Maken van een gebruiker Tableau Server testen

Het doel van deze sectie is voor het maken van een gebruiker met de naam Britta Simon Tableau server. U moet alle gebruikers op de server Tableau. Vergeet niet dat gebruikersnaam van de gebruiker moet overeenkomen met de waarde die u hebt ingesteld in het aangepaste Azure AD-kenmerk van **gebruikersnaam**. Met de juiste toewijzing moet de integratie van [Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)werken.

> [AZURE.NOTE] Als u een gebruiker handmatig te maken, moet u contact op met de serverbeheerder Tableau in uw organisatie.


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

Het doel van deze sectie is Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan Tableau-Server inschakelen.

![Gebruiker toewijzen][200] 

**Britta Simon Tableau-server toewijzen, voert u de volgende stappen uit:**

1. Op de Azure klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.
 
    ![Gebruiker toewijzen][201] 

2. Selecteer **Tableau Server**in de lijst toepassingen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_50.png) 


1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Wanneer u op de tegel Tableau Server in het Configuratiescherm toegang, u moet krijgen automatisch ondertekend voor toegang tot de servertoepassing Tableau.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_205.png
