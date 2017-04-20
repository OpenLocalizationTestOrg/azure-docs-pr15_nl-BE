<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met ADP-eTime | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en ADP-eTime."
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
    ms.date="10/17/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-adp-etime"></a>Zelfstudie: Azure Active Directory-integratie met ADP-eTime

Het doel van deze zelfstudie is u tonen hoe ADP eTime integreren met Azure Active Directory (AD Azure).  
ADP eTime integreren met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot het ADP-eTime heeft
- U kunt gebruikers automatisch ophalen ondertekend bij ADP eTime (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren


Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met ADP-eTime, moet u de volgende items:

- Een abonnement op Azure AD
- Een ADP eTime eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD testen in een testomgeving.  
Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. ADP-eTime uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-adp-etime-from-the-gallery"></a>ADP-eTime uit de galerie toe te voegen.
De integratie van ADP eTime in Azure AD configureren, moet u ADP-eTime uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**ETime ADP toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **ADP-eTime**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_01.png)

7. **ADP eTime**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is u tonen hoe te configureren en eenmalige aanmelding Azure AD test met ADP-eTime op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in ADP-eTime aan een gebruiker in AD Azure. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gebruiker in een ADP-eTime worden vastgesteld.  
Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in ADP eTime toewijzen.

Als u wilt configureren en testen Azure AD single sign-on met ADP-eTime, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Gebruikers maken een ADP-eTime test](#creating-a-adpetime-test-user)** - hebben een tegenhanger van Britta Simon in ADP-eTime die is gekoppeld aan de weergave Azure AD van haar.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van deze sectie is Azure AD eenmalige aanmelding in de klassieke Azure portal inschakelen en configureren van eenmalige aanmelding in uw toepassing ADP-eTime.

De SAML beweringen omtrent verwacht uw toepassing ADP eTime in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. De volgende schermafbeelding ziet u een voorbeeld voor dit. De claimnaam van de is **'PersonImmutableID'** en de waarde die we hebben toegewezen aan ExtensionAttribute2 die de werknemer-id van de gebruiker bevat. Hier gebruiker toewijzing teks Azure AD om ADP-eTime op de werknemer-id wordt uitgevoerd, maar kunt u deze toewijzen aan een andere waarde ook op basis van de instellingen van de toepassing. Dus u moet werken met ADP eTime team eerst gebruikt u de juiste id van een gebruiker en die waarde met de **'PersonImmutableID'** claim toewijzen.  

![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_02.png) 

Voordat u de SAML-bevestiging configureren kunt, moet u contact opnemen met het ondersteuningsteam van ADP eTime en de waarde van het kenmerk de unieke id voor de huurder aanvragen. U moet deze waarde voor het configureren van de aangepaste claim voor uw toepassing.


**Configureren van eenmalige aanmelding Azure AD met ADP-eTime, voert u de volgende stappen uit:**

1. Klik in de klassieke Azure portal, op de pagina **eTime ADP** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren][6] 

2. Op de pagina **Hoe wilt u dat gebruikers aanmelden bij ADP eTime** Selecteer **Azure AD Single Sign-On**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_03.png) 

3. Klik op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** als volgt:.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_04.png) 


    een. Typ in het tekstvak **URL antwoord** de URL die door uw gebruikers wordt gebruikt voor aanmelding bij uw ADP-eTime-toepassing met behulp van het volgende patroon: `https://<server name>.adp.com/affwebservices/public/saml2assertionconsumer`.

    b. Klik op **volgende**.

4. Op de pagina **configureren eenmalige aanmelding bij ADP eTime** voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_05.png) 

    een. Klik op **metagegevens downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


5. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met het ondersteuningsteam van ADP eTime en e-mail het metagegevensbestand gedownload koppelen zodat ze kunnen worden geconfigureerd voor integratie van eenmalige aanmelding.

    > [AZURE.NOTE] Nadat het **ADP-eTime** team het exemplaar is geconfigureerd, haal de **RelayState** uit deze. Volg de hieronder vermelde stappen om deze te configureren. Na deze configuratie kunt u de integratie testen. Dus houd er rekening mee dat dit belangrijke configuratie voor de integratie van deze toepassingen werken is.

6. Als de waarde RelayState in Azure AD configureert, moet u de volgende stappen uitvoeren: 
    
    een. Meld u aan als beheerder [Azure Management Portal](https://portal.azure.com) .

    b. Klik op **Meer Services**in het linkernavigatievenster. 
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_07.png)

    c. Typ **Azure Active Directory**in het tekstvak **Zoeken** en klik vervolgens op de bijbehorende koppeling.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_08.png)

    d. Klik op **zakelijke toepassingen**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_09.png)

    e. Klik op **Alle toepassingen**in de sectie **beheren** .
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_10.png)

    f. **ADP eTime**typt in het tekstvak **Zoeken** en klik vervolgens op de bijbehorende koppeling. 
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_11.png)

    g. Klik in de sectie **Manage** **- on**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_12.png)

    h. Selecteer **weergeven-geavanceerde instellingen voor URL**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_13.png)
    
    ik. Typ in het tekstvak **Relay staat** een waarde met de volgende patronen:
    
    - Productie-omgeving:`https://fed.adp.com/saml/fedlanding.html?<id>` 
    - Staging omgeving:`https://fed-stag.adp.com/saml/fedlanding.html?PORTAL`

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_14.png)

    j. De instellingen opslaan.

7. In de klassieke Azure portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.

    ![Azure AD Single Sign-On][10]

8. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  

    ![Azure AD Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
Het doel van deze sectie is voor het maken van een testgebruiker in Azure klassieke portal Britta Simon genoemd.  
Selecteer in de lijst gebruikers **Britta Simon**.

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-adpetime-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-adpetime-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-adpetime-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-adpetime-tutorial/create_aaduser_05.png) 

    een. Selecteer **Type van gebruiker**, **nieuwe gebruiker in uw organisatie**.

    b. Typ in het tekstvak **Gebruikersnaam** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :

    ![Azure AD test gebruikers maken](./media/active-directory-saas-adpetime-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-adpetime-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-adpetime-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-a-adp-etime-test-user"></a>Maken van een gebruiker ADP eTime test

Het doel van deze sectie is voor het maken van een gebruiker met de naam Britta Simon in ADP-eTime. Neem samen met ADP eTime support team aan de gebruikers in de ADP eTime account toevoegen. 


> [AZURE.NOTE]Als u een gebruiker handmatig te maken, moet u contact opnemen met het ondersteuningsteam van ADP eTime.


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

Het doel van deze sectie is Britta Simon gebruik van door haar toegang verlenen tot eTime ADP Azure eenmalige aanmelding inschakelen.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen eTime ADP, moet u de volgende stappen uitvoeren:**

1. Op de Azure klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst toepassingen **ADP-eTime**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.  
Wanneer u op de tegel ADP eTime in het Access-venster, u moet krijgen automatisch ondertekend voor toegang tot uw toepassing ADP-eTime.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_205.png
