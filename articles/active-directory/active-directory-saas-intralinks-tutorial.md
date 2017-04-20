<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Intralinks | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Intralinks."
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


# <a name="tutorial-azure-active-directory-integration-with-intralinks"></a>Zelfstudie: Azure Active Directory-integratie met Intralinks

In deze zelfstudie leert u hoe Intralinks integreren met Azure Active Directory (AD Azure).

Intralinks integratie met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot de Intralinks heeft
- U kunt gebruikers automatisch ophalen ondertekend bij Intralinks (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met Intralinks, moet u de volgende items:

- Een abonnement op Azure AD
- Een Intralinks eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving.

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Intralinks uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-intralinks-from-the-gallery"></a>Intralinks uit de galerie toe te voegen.
De integratie van Intralinks in Azure AD configureren, moet u Intralinks uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Om de Intralinks van de galerie wilt toevoegen, moet u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ **Intralinks**in het zoekvak.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_01.png)

7. **Intralinks**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Intralinks op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in de Intralinks aan een gebruiker in AD Azure. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gebruiker in Intralinks worden vastgesteld.

Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** toewijzen in Azure AD als de waarde van de **gebruikersnaam** in het Intralinks.

Als u wilt configureren en testen Azure AD single sign-on met Intralinks, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Maken van een Intralinks gebruiker testen](#creating-an-intralinks-test-user)** - een tegenhanger van Britta Simon in Intralinks die is gekoppeld aan de weergave Azure AD van haar hebben.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

In deze sectie, Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Intralinks.


**Configureren van eenmalige aanmelding Azure AD met Intralinks, voert u de volgende stappen uit:**

1. Klik in de klassieke portal op de pagina **Intralinks** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.
     
    ![Eenmalige aanmelding configureren][6] 

2. Selecteer **Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Intralinks** en klik op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_03.png) 

3. Klik op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** als volgt:.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_04.png) 

    een. Typt in het tekstvak **Teken op URL** de URL die wordt gebruikt door uw gebruikers om aanmelding bij de toepassing van de Intralinks met behulp van het volgende patroon: **https://\<naam van het bedrijf\>.Intralinks.com/?PartnerIdpId= https://sts.windows.net/\<-ID Azure AD huurder\>**.

    b. Klik op **volgende**.
    
4. Klik op de pagina **configureren eenmalige aanmelding op Intralinks** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_05.png)

    een. Klik op **metagegevens downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


5. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met het ondersteuningsteam van Intralinks en het bestand bijvoegen metagegevens gedownloade e-mail.


6. In de klassieke portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.
    
    ![Azure AD Single Sign-On][10]

7. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
 
    ![Azure AD Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.

Selecteer in de lijst gebruikers **Britta Simon**.

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-intralinks-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-intralinks-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-intralinks-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster als volgt:  ![Azure AD test gebruikers maken](./media/active-directory-saas-intralinks-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Klik op de pagina van het dialoogvenster **Gebruikersprofiel** als volgt: ![Azure AD test gebruikers maken](./media/active-directory-saas-intralinks-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-intralinks-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-intralinks-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-an-intralinks-test-user"></a>Het testgebruiker van een Intralinks maken

In dit gedeelte maakt u een gebruiker met de naam Britta Simon in Intralinks. Neem samen met Intralinks support team toe te voegen de gebruikers van het platform Intralinks.


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

In deze sectie kunt inschakelen u Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan Intralinks.

![Gebruiker toewijzen][200] 

**Britta Simon aan Intralinks toewijzen, moet u de volgende stappen uitvoeren:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Intralinks**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_50.png) 

3. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]

### <a name="adding-intralinks-via-or-elite-application"></a>VIA Intralinks of Elite toepassing toe te voegen
Intralinks maakt gebruik van één teken op identiteit hetzelfde platform voor alle andere Intralinks toepassingen met uitzondering van deals Nexus toepassing. Dus als u van plan bent te gebruiken van een andere toepassing van de Intralinks hebt eerst u aan de eenmalige aanmelding configureren voor een primaire Intralinks-toepassing met behulp van de hierboven beschreven procedure.

Daarna voert u de onderstaande procedure in een andere toepassing van Intralinks in uw huurder die gebruikmaken van deze primaire toepassing voor eenmalige aanmelding kunt toevoegen. 

> [AZURE.NOTE] Opmerking deze functie is alleen beschikbaar voor klanten van Azure AD Premium SKU en niet beschikbaar voor het vrije of Basic SKU klanten Neem.

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Active Directory][1]
2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Uit de links uitlijnende tab Klik op tabblad **aangepast**

    ![VIA Intralinks of Elite toepassing toe te voegen](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_51.png)

7. De toepassing bijvoorbeeld **Intralinks Elite** passende naam geven en klik op de knop Voltooien.

8. Klik op de knop **Configureren Single Sign**

9. Selecteer de optie **Bestaande Single Sign On**

    ![VIA Intralinks of Elite toepassing toe te voegen](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_52.png)

10. Krijgen de SP geïnitieerd SSO-URL van de Intralinks voor de andere toepassing van het Intralinks-team en voer de code zoals hieronder wordt weergegeven. 

    ![VIA Intralinks of Elite toepassing toe te voegen](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_53.png)

    een. In het teken op URL-tekstvak typt u de URL die wordt gebruikt door gebruikers voor aanmelding bij uw toepassing Intralinks is met het volgende patroon: **https://\<bedrijf\>.Intralinks.com/?PartnerIdpId= https://sts.windows.net/\<AzureADTenantID\> **


11. Klik op **volgende**.

12. De toepassing aan gebruikers of groepen toewijzen, zoals wordt weergegeven in de sectie ** [het testgebruiker Azure AD toewijzen](#assigning-the-azure-ad-test-user)**


### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

In dit gedeelte vindt u Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Wanneer u op de tegel Intralinks in het Access-venster, u moet krijgen automatisch ondertekend bij uw toepassing Intralinks.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_205.png
