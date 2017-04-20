<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Questetra BPM Suite | Microsoft-Aure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Questetra BPM Suite."
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
    ms.date="10/28/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a>Zelfstudie: Azure Active Directory-integratie met Questetra BPM Suite

Het doel van deze zelfstudie is u tonen hoe Questetra BPM Suite integreren met Azure Active Directory (AD Azure).  
Questetra BPM Suite integreren met AD Azure biedt de volgende voordelen: 

- U kunt bepalen in Azure AD die toegang tot de Questetra BPM Suite heeft 
- U kunt gebruikers automatisch ophalen ondertekend bij Questetra BPM Suite (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten 

Integratie van de Azure AD Questetra BPM Suite configureren, moet u de volgende items:

- Een abonnement op Azure AD
- Een [Questetra BPM Suite](https://senbon-imadegawa-988.questetra.net/) eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen. 

 
## <a name="scenario-description"></a>Beschrijving van het scenario
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD testen in een testomgeving.  
Het scenario dat is beschreven in deze zelfstudie bestaat uit drie belangrijkste bouwstenen:

1. Questetra BPM Suite uit de galerie toe te voegen. 
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-questetra-bpm-suite-from-the-gallery"></a>Questetra BPM Suite uit de galerie toe te voegen.
De integratie van Questetra BPM Suite in Azure AD configureren, moet u Questetra BPM Suite uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt Questetra BPM Suite toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Questetra BPM Suite**.

    ![Toepassingen][5]

7. Selecteer **Questetra BPM Suite**in het resultatenvenster en klik op **Voltooien** als de toepassing wilt toevoegen.



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is u tonen hoe te configureren en testen eenmalige aanmelding Azure AD Questetra BPM Suite op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in Questetra BPM Suite aan een gebruiker in AD Azure. Met andere woorden, moet een relatie van de koppeling tussen een Azure AD-gebruiker en de gebruiker in Questetra BPM Suite worden vastgesteld.  
Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Questetra BPM Suite toewijzen.
 
Als u wilt configureren en testen Azure AD eenmalige aanmelding met Questetra BPM Suite, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Maken van een Questetra BPM Suite gebruiker testen](#creating-a-questetra-bpm-suite-test-user)** - een tegenhanger van Britta Simon in Questetra BPM Suite die is gekoppeld aan de weergave Azure AD van haar hebben.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van deze sectie is Azure AD eenmalige aanmelding in de klassieke Azure portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Questetra BPM Suite.

**Configureren van eenmalige aanmelding Azure AD Questetra BPM Suite, voert u de volgende stappen uit:**

1. Klik in de klassieke Azure portal, klik op de pagina **Questetra BPM Suite** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren][8]

2. Klik op de pagina **Hoe wilt u dat gebruikers aan te melden op Questetra BPM Suite** **Azure AD Single Sign-On**selecteren en klik op **volgende**.

    ![Azure AD Single Sign-On][9]


3. Log in op uw site **Questetra BPM Suite** bedrijf als beheerder in een ander web browser-venster.

4. In het menu aan de bovenkant, klikt u op **Instellingen**. 

    ![Azure AD Single Sign-On][10]

5. Open de pagina **SingleSignOnSAML** , klik op **Eenmalige aanmelding (SAML)**. 

    ![Azure AD Single Sign-On][11]


6. In de klassieke Azure portal op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** als volgt: 

    ![App-instellingen configureren][13]
 
    een. Op u **Questetra BPM Suite** bedrijf site in de sectie informatie over de SP, kopieert u de **URL van de ACS**en plak deze in het **Teken op URL** textbox.

    b. Op u **Questetra BPM Suite** bedrijf site in de sectie informatie over de SP, kopieert u de **ID van de entiteit**, en vervolgens plakken in het tekstvak **URL van de uitgevende instelling** .

    c. Op u **Questetra BPM Suite** bedrijf site in de sectie informatie over de SP, kopieert u de **URL van de ACS**en plak deze in het tekstvak **URL van het antwoord** .

    d. Klik op **volgende**.

 
7. Klik op **certificaat downloaden**en sla het bestand lokaal op uw computer op de pagina **configureren eenmalige aanmelding bij Questetra BPM Suite** .

    ![Eenmalige aanmelding configureren][14]


8. Op uw bedrijf-site **Questetra BPM Suite** , voert u de volgende stappen uit: 

    ![Eenmalige aanmelding configureren][15]

    een. Selecteer **eenmalige aanmelding inschakelen**.
     
    b. Op de Azure klassieke portal de **Uitgever URL** -waarde kopiëren en vervolgens plakken in het tekstvak **Afdelings-ID** .

    c. Op de Azure klassieke portal de **Single Sign-On Service URL** -waarde kopiëren en vervolgens plakken in het tekstvak **URL - in pagina** .

    d. Kopieer de **URL van de Service Single Sign-Out** waarde op de Azure klassieke portal en plak deze in het tekstvak **URL van afmelden** .

    e. Typ in het tekstvak **NameID indeling** **urn: oasis: namen: tc: SAML:1.1:nameid-indeling: EmailAdres**.


    f. Een Base64-gecodeerd bestand maken van uw gedownloade certificaat. 

    >[AZURE.TIP] Zie [conversie van binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)voor meer informatie.

    g. De base-64 gecodeerde certificaat in Kladblok te openen, de inhoud ervan kopiëren naar het Klembord en plak deze in de textbox **certificaat validatie** . 

    h. Klik op **Opslaan**.


9. Selecteer de bevestiging enkele aanmelding-configuratie op de Azure klassieke portal en klik op **volgende**. 

    ![Wat is Azure AD verbinden][17]


10. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  

    ![Wat is Azure AD verbinden][18]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
Het doel van deze sectie is voor het maken van een testgebruiker in Azure klassieke portal Britta Simon genoemd.

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Het testgebruiker Azure advertentie maken][100] 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Het testgebruiker Azure advertentie maken][101] 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**. 

    ![Het testgebruiker Azure advertentie maken][102] 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Het testgebruiker Azure advertentie maken][103]
 
    een. Selecteer **Type van gebruiker**, **nieuwe gebruiker in uw organisatie**.
  
    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op volgende.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** : 

    ![Het testgebruiker Azure advertentie maken][104] 
  
    een. Typ in het tekstvak **Voornaam** **Britta**. 
 
    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Het testgebruiker Azure advertentie maken][105]  

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Het testgebruiker Azure advertentie maken][106]   
  
    een. Noteer de waarde van het **Nieuwe wachtwoord**.
  
    b. Klik op **Voltooien**.   
  
 
### <a name="creating-a-questetra-bpm-suite-test-user"></a>Een gebruiker Questetra BPM Suite test maken

Het doel van deze sectie is voor het maken van een gebruiker met de naam Britta Simon in Questetra BPM Suite.

**Als een gebruiker met de naam Britta Simon in Questetra BPM Suite maakt, moet u de volgende stappen uitvoeren:**

1.  Aanmelding bij uw bedrijf Questetra BPM Suite site als beheerder.
2.  Ga naar **Instellingen > lijst > nieuwe gebruiker**. 
3.  In het dialoogvenster Nieuwe gebruiker de volgende stappen uitvoeren: 

    ![Testgebruiker maken][300] 

    een. Typ in het tekstvak **naam** van Britta gebruikersnaam in Azure AD.

    b. Typ in het tekstvak **e-mailadres** van Britta gebruikersnaam in Azure AD.

    c. Typ een wachtwoord in het tekstvak **wachtwoord** .

4.  Klik op **nieuwe gebruiker toevoegen**.



### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

Het doel van deze sectie wordt aan het inschakelen van Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan Questetra BPM Suite.

![Wat is Azure AD verbinden][200]

**Britta Simon Questetra BPM Suite toewijzen, moet u de volgende stappen uitvoeren:**

1. Op de Azure klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Wat is Azure AD verbinden][201]

2. Selecteer in de lijst met toepassingen **Questetra BPM Suite**.

    ![Wat is Azure AD verbinden][205]

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Wat is Azure AD verbinden][202]

1. Selecteer in de lijst gebruikers **Britta Simon**.

    ![Wat is Azure AD verbinden][203]

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Wat is Azure AD verbinden][204]



### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.  
Wanneer u op de tegel Questetra BPM Suite in het Access-venster, u moet krijgen automatisch ondertekend voor toegang tot uw Questetra BPM Suite-toepassing.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_01.png
[2]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_02.png
[3]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_03.png
[4]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_04.png
[5]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_01.png


[8]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_06.png
[9]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_02.png
[10]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_03.png
[11]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_04.png
[12]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_05.png
[13]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_06.png
[14]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_07.png
[15]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_08.png
[16]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_09.png
[17]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_10.png
[18]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_08.png


[100]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_09.png 
[101]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_10.png 
[102]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_11.png 
[103]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_12.png 
[104]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_13.png 
[105]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_14.png 
[106]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_15.png 


[200]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_16.png 
[201]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_17.png 
[202]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_18.png
[203]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_19.png
[204]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_20.png
[205]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_12.png

[300]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_11.png 