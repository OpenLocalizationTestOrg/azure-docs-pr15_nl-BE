<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Lessonly | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Lessonly."
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


# <a name="tutorial-azure-active-directory-integration-with-lessonly"></a>Zelfstudie: Azure Active Directory-integratie met Lessonly

Het doel van deze zelfstudie is u tonen hoe Lessonly integreren met Azure Active Directory (AD Azure).

Lessonly integratie met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot de Lessonly heeft
- U kunt gebruikers automatisch ophalen ondertekend bij Lessonly (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met Lessonly, moet u de volgende items:

- Een abonnement op Azure AD
- Een Lessonly eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD testen in een testomgeving. 

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Lessonly uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-lessonly-from-the-gallery"></a>Lessonly uit de galerie toe te voegen.
De integratie van Lessonly in Azure AD configureren, moet u Lessonly uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Om de Lessonly van de galerie wilt toevoegen, moet u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.
 
    ![Toepassingen][4]

6. Typ **Lessonly**in het zoekvak.
 
    ![Azure AD test gebruikers maken](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_01.png)

7. **Lessonly**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.


![Azure AD test gebruikers maken](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is u tonen hoe te configureren en eenmalige aanmelding Azure AD test met Lessonly op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in de Lessonly aan een gebruiker in AD Azure. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gebruiker in Lessonly worden vastgesteld.

Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** toewijzen in Azure AD als de waarde van de **gebruikersnaam** in het Lessonly.

Als u wilt configureren en testen Azure AD single sign-on met Lessonly, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Maken van een Lessonly gebruiker testen](#creating-a-Lessonly-test-user)** - een tegenhanger van Britta Simon in Lessonly die is gekoppeld aan de weergave Azure AD van haar hebben.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Lessonly met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

Uw toepassing Lessonly verwacht de SAML-bevestigingen in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de **saml-token kenmerken** .
De volgende schermafbeelding ziet u een voorbeeld voor dit.

![Eenmalige aanmelding configureren](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_06.png) 

**Configureren van eenmalige aanmelding Azure AD met Lessonly, voert u de volgende stappen uit:**

1. In de klassieke Azure portal, klik op de pagina toepassing Lessonly integratie in het menu aan de bovenkant, klikt u op **kenmerken** om de **SAML-Token kenmerken** -dialoogvenster te openen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_07.png) 

2. Als u wilt de toewijzingen vereist kenmerk toevoegen, moet u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_08.png) 

    een. Voor elke rij met gegevens in de bovenstaande tabel, klikt u op **gebruikerskenmerk toevoegen**.

    b. Typ de naam van het kenmerk voor de rij weergegeven in het tekstvak **Naam van kenmerk** .

    c. Selecteer in de lijst **Waarde van het kenmerk** de kenmerkwaarde voor die rij weergegeven.

    d. Klik op **Voltooien**

3. Klik op **wijzigingen toepassen**.

4. In de browser, klikt u op **terug** om het dialoogvenster Quick Start opnieuw openen

5. Klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren][6] 

6. Selecteer **Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Lessonly** en klik op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_03.png) 

7. Voer de volgende stappen uit op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** :
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_04.png) 


    een. In het teken op URL-tekstvak typt u de URL die wordt gebruikt door uw gebruikers om aanmelding bij de Lessonly-toepassing met behulp van het volgende patroon: **'https://companyname.lesson.ly/signin'**. Wanneer wordt verwezen naar een algemene naam moet dat **bedrijf** worden vervangen door de feitelijke naam.


8. Klik op de pagina **configureren eenmalige aanmelding op Lessonly** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_05.png) 

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


9. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met uw Lessonly support team via dev@lessonly.com. Het gedownloade bestand toevoegen aan uw e-mailberichten en de metagegevens van URL's (Afdelings-ID, SSO-teken in de URL en Sign Out URL) delen met Lessonly team kunt u eenmalige aanmelding instellen op hun kant.


10. In de klassieke Azure portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.

    ![Azure AD Single Sign-On][10]

11. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
  
    ![Azure AD Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
Het doel van deze sectie is voor het maken van een testgebruiker in Azure klassieke portal Britta Simon genoemd.


![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-lessonly-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-lessonly-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-lessonly-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-lessonly-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :

    ![Azure AD test gebruikers maken](./media/active-directory-saas-lessonly-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-lessonly-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:
 
    ![Azure AD test gebruikers maken](./media/active-directory-saas-lessonly-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-a-lessonly-test-user"></a>Maken van een testgebruiker Lessonly

Het doel van deze sectie is voor het maken van een gebruiker met de naam Britta Simon in Lessonly. Lessonly ondersteunt just in time-aanbod is standaard ingeschakeld.

Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker wordt gemaakt tijdens een poging toegang te krijgen tot Lessonly als deze nog niet bestaat. [Azure AD voor eenmalige aanmelding configureren](#configuring-azure-ad-single-single-sign-on).

> [AZURE.NOTE] Als u een gebruiker handmatig te maken, moet u contact opnemen met het ondersteuningsteam van Lessonly.


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

Het doel van deze sectie wordt aan het inschakelen van Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan Lessonly.

![Gebruiker toewijzen][200] 

**Britta Simon aan Lessonly toewijzen, moet u de volgende stappen uitvoeren:**

1. Op de Azure klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Lessonly**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Wanneer u klikt op het Lessonly naast elkaar in het Configuratiescherm toegang u moet krijgen automatisch ondertekend bij uw toepassing Lessonly.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_205.png
