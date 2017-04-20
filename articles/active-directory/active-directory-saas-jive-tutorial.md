<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Jive | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Jive."
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-jive"></a>Zelfstudie: Azure Active Directory-integratie met Jive

In deze zelfstudie leert u hoe Jive integreren met Azure Active Directory (AD Azure).

Jive integreren met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot de Jive heeft
- U kunt gebruikers automatisch ophalen ondertekend bij Jive (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met Jive, moet u de volgende items:

- Een abonnement op Azure AD
- Een Jive eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving.

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Jive uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-jive-from-the-gallery"></a>Jive uit de galerie toe te voegen.
De integratie van de Jive in Azure AD configureren, moet u Jive uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Jive toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Active Directory][1]
2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Jive**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-jive-tutorial/tutorial_jive_01.png)
7. **Jive**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-jive-tutorial/tutorial_jive_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Jive op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weet wat de gebruiker tegenhanger in Jive is aan een gebruiker in AD Azure. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gebruiker in de Jive worden vastgesteld.

Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** toewijzen in Azure AD als de waarde van de **gebruikersnaam** in de Jive.

Als u wilt configureren en testen Azure AD single sign-on met Jive, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
3. **[Maken van een Jive gebruiker testen](#creating-a-jive-test-user)** - een tegenhanger van Britta Simon in Jive die is gekoppeld aan de weergave Azure AD van haar hebben.
4. **[Gebruikersaanvragen configureren](#configuring-user-provisioning)** - overzicht van het inschakelen van gebruikersaanvragen van Active Directory-gebruiker-accounts aan Jive.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
6. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van deze sectie is Azure AD eenmalige aanmelding in de klassieke Azure portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Jive.

**Configureren van eenmalige aanmelding Azure AD met Jive, voert u de volgende stappen uit:**

1. Klik in de klassieke portal op de pagina **Jive** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.
     
    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u gebruikers aan te melden op Jive** **Azure AD Single Sign-On**selecteren en klik op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-jive-tutorial/tutorial_jive_03.png) 

3. Voer de volgende stappen uit op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-jive-tutorial/tutorial_jive_04.png) 

    een. In het **Teken op URL** -tekstvak typt u de URL die wordt gebruikt door uw gebruikers om aanmelding bij de Jive-toepassing met behulp van het volgende patroon: **https://\<naam van de klant\>. jivecustom.com**.
    
    b. Klik op **volgende**
 
4. Op de pagina **configureren eenmalige aanmelding bij Jive** , kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-jive-tutorial/tutorial_jive_05.png)

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


5. Aanmelding bij de huurder Jive als beheerder.

6. Klik in het menu aan de bovenkant, '**Saml**'.

    ![App aan eenmalige aanmelding configureren](./media/active-directory-saas-jive-tutorial/tutorial_jive_002.png)

    een. Selecteer **ingeschakeld** op het tabblad **Genaral** .

    b. Klik op de knop '**alle saml-instellingen opslaan**'.

7. Ga naar het tabblad "**Idp Metadata**".

    ![App aan eenmalige aanmelding configureren](./media/active-directory-saas-jive-tutorial/tutorial_jive_003.png)

    een. De inhoud van de XML-metagegevens gedownloade bestand kopiëren en plak deze in de textbox **metagegevens Identity Provider (IDP)** .

    b. Klik op de knop '**alle saml-instellingen opslaan**'. 

8. Ga naar het tabblad "**Kenmerk Gebruikerskoppeling**".

    ![App aan eenmalige aanmelding configureren](./media/active-directory-saas-jive-tutorial/tutorial_jive_004.png)

    een. Kopiëren en plakken van de naam van het kenmerk van de **e** -waarde in het tekstvak **e-mailadres** .

    b. In het tekstvak **Voornaam** kopiëren en plakken van **givenname** waarde de naam van het kenmerk.

    c. In het tekstvak **Achternaam** , kopieer en plak de naam van het kenmerk van de waarde van de **Achternaam** .
    
9. In de portal Azure AD, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.
![Azure AD Single Sign-On][10]

10. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
  ![Azure AD Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.


![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-jive-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-jive-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-jive-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster als volgt:  ![Azure AD test gebruikers maken](./media/active-directory-saas-jive-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Klik op de pagina van het dialoogvenster **Gebruikersprofiel** als volgt: ![Azure AD test gebruikers maken](./media/active-directory-saas-jive-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-jive-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-jive-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



###<a name="creating-a-jive-test-user"></a>Een gebruiker Jive test maken

In dit gedeelte maakt u een gebruiker met de naam Britta Simon in Jive. Neem samen met Jive ondersteuningsteam gebruikers toevoegen in het platform Jive.


###<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikersaanvragen van Active Directory-gebruikersaccounts met Jive.  
Als onderdeel van deze procedure bent u verplicht om het beveiligingstoken van een gebruiker moet u aanvragen via Jive.com.
  
De volgende schermafdruk toont een voorbeeld van het bijbehorende dialoogvenster in Azure AD:

![Gebruikersaanvragen configureren] (./media/active-directory-saas-jive-tutorial/IC698794.png "Gebruikersaanvragen configureren")

####<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Klik in de Azure Management Portal op de pagina **Jive** application integration op **Gebruikersaanvragen configureren** om het dialoogvenster **Gebruikersaanvragen configureren** te openen.

2.  Klik op de pagina **Geef uw referenties Jive, zodat Automatische gebruikersaanvragen** bieden de volgende configuratie-instellingen:

    1.  Typ in het tekstvak **Jive Admin gebruikersnaam** de naam van een Jive met het profiel van de **Systeembeheerder** in Jive.com toegewezen.

    2.  Typ in het tekstvak **Jive Admin-wachtwoord** het wachtwoord voor deze account.

    3.  Typ de URL van de huurder Jive in het tekstvak **URL van de huurder Jive** .

        >[AZURE.NOTE]De huurder Jive URL is de URL die door uw organisatie wordt gebruikt om aan te melden bij de Jive.  
        De URL heeft meestal de volgende indeling: **www.\< organisatie\>. jive.com**.

    4.  Klik op **valideren** om te controleren of uw configuratie.

    5.  Klik op de knop **volgende** om de pagina **bevestiging** te openen.

3.  Klik op het selectievakje om de configuratie op te slaan op **de bevestigingspagina** .
  
U kunt nu een testaccount maken, wacht 10 minuten en controleer of de account is gesynchroniseerd met de Jive.com.




### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

In deze sectie kunt inschakelen u Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan Jive.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Jive, voert u de volgende stappen uit:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Jive**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-jive-tutorial/tutorial_jive_50.png) 

3. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van Single Sign-On

In dit gedeelte vindt u Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Wanneer u op de tegel Jive in het Access-venster, u moet krijgen automatisch ondertekend bij uw toepassing Jive.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-jive-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jive-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jive-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jive-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-jive-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-jive-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-jive-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-jive-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jive-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jive-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-jive-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-jive-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-jive-tutorial/tutorial_general_205.png
