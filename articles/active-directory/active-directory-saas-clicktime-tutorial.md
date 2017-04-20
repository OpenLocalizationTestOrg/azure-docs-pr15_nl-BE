<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met ClickTime | Microsoft Azure" 
    description="Meer informatie over het ClickTime met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
    services="active-directory" 
    authors="jeevansd"
    documentationCenter="na" 
    manager="femila" />
<tags
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-clicktime"></a>Zelfstudie: Azure Active Directory-integratie met ClickTime

In deze zelfstudie leert u hoe ClickTime integreren met Azure Active Directory (AD Azure).

ClickTime integratie met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot de ClickTime heeft
- U kunt gebruikers automatisch ophalen ondertekend bij ClickTime (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met ClickTime, moet u de volgende items:

- Een abonnement op Azure AD
- Een ClickTime eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving.

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. ClickTime uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding

##<a name="adding-clicktime-from-the-gallery"></a>ClickTime uit de galerie toe te voegen.

Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor ClickTime.

###<a name="to-enable-the-application-integration-for-clicktime-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor ClickTime, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-clicktime-tutorial/tic700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-clicktime-tutorial/tic700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-clicktime-tutorial/tic749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-clicktime-tutorial/tic749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **ClickTime**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-clicktime-tutorial/tic777275.png "Galerie van toepassing")

7.  **ClickTime**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![ClickTime] (./media/active-directory-saas-clicktime-tutorial/tic777276.png "ClickTime")

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met ClickTime op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger in de ClickTime aan een gebruiker in AD Azure. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gebruiker in ClickTime worden vastgesteld.

Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** toewijzen in Azure AD als de waarde van de **gebruikersnaam** in het ClickTime.

Als u wilt configureren en testen Azure AD single sign-on met ClickTime, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
3. **[Maken van een ClickTime gebruiker testen](#creating-a-clicktime-test-user)** - een tegenhanger van Britta Simon in ClickTime die is gekoppeld aan de weergave Azure AD van haar hebben.
4. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.


### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij ClickTime met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  


>[AZURE.IMPORTANT] Voordat u eenmalige aanmelding configureren op uw huurder ClickTime, moet u eerst contact met de technische ondersteuning van ClickTime als u deze functie is ingeschakeld.

**Configureren van eenmalige aanmelding Azure AD met ClickTime, voert u de volgende stappen uit:**

1.  Klik in de klassieke Azure portal, klik op de pagina **ClickTime** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding inschakelen] (./media/active-directory-saas-clicktime-tutorial/tic777277.png "Eenmalige aanmelding inschakelen")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op ClickTime** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-clicktime-tutorial/tic777278.png "Eenmalige aanmelding configureren")

3. Voer de volgende stappen uit op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-clicktime-tutorial/tic777286.png) 

    een. Typ in het tekstvak **IdentifierL** de URL met het volgende patroon: **https://app.clicktime.com/sp/**
    
    b. Typ in het tekstvak **URL antwoord** de URL met het volgende patroon: **https://app.clicktime.com/Login/**

    c. Klik op **volgende**

4.  Op de pagina **configureren eenmalige aanmelding op ClickTime** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-clicktime-tutorial/tic777279.png "Eenmalige aanmelding configureren")

4.  Log in op uw site ClickTime bedrijf als beheerder in een ander web browser-venster.

5.  In de werkbalk op de bovenkant, klikt u op **Voorkeuren**en klik op **Beveiligingsinstellingen**.

6.  In de sectie **Voorkeuren voor Single Sign-On** -configuratie kunt u de volgende stappen uitvoeren:

    ![Beveiligingsinstellingen] (./media/active-directory-saas-clicktime-tutorial/tic777280.png "Beveiligingsinstellingen")

    een.  Selecteer **toestaan** aanmelden via Single Sign-On (SSO) met **Azure Active Directory**.
    
    b.  In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding op ClickTime** de **Single Sign-On Service URL** -waarde kopiëren en vervolgens plakken in de textbox **Identity Provider-eindpunt** .

    c.  De base-64 gecodeerde certificaat openen in **Kladblok**en kopieer de inhoud vervolgens plakken in het tekstvak **X.509-certificaat** .
    
    d.  Klik op **Opslaan**.

7.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-clicktime-tutorial/tic777281.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Om gebruikers aan te melden bij ClickTime Azure AD, moeten zij worden ingericht in ClickTime.  
Bij ClickTime is het inrichten van een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw huurder **ClickTime** .

2.  Op **bedrijf**in de werkbalk op de bovenkant, en klik vervolgens op **personen**.

    ![Mensen] (./media/active-directory-saas-clicktime-tutorial/tic777282.png "Mensen")

3.  Klik op de **persoon toevoegen**.

    ![Persoon toevoegen] (./media/active-directory-saas-clicktime-tutorial/tic777283.png "Persoon toevoegen")

4.  Voer de volgende stappen uit in de sectie nieuwe persoon:

    ![Mensen] (./media/active-directory-saas-clicktime-tutorial/tic777284.png "Mensen")

    een.  Typ in het tekstvak **e-mailadres** het e-mailadres van uw account Azure AD.
    
    b.  Typ in het tekstvak **volledige naam** de naam van uw account Azure AD.  

    >[AZURE.NOTE] Als u wilt, kunt u extra eigenschappen van de nieuwe persoon object kunt instellen.

    c.  Klik op **Opslaan**.

>[AZURE.NOTE] Kunt u een andere ClickTime gebruiker account maken van hulpprogramma's of API's die door ClickTime AD Azure gebruikersaccounts inrichten.

### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

In deze sectie kunt inschakelen u Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan ClickTime.

![Gebruiker toewijzen][200]

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

**De volgende stappen uitvoeren om Britta Simon toewijzen aan ClickTime,**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **ClickTime**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_50.png) 

3. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]

## <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding
In dit gedeelte vindt u Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Wanneer u op de tegel ClickTime in het Access-venster, u moet krijgen automatisch ondertekend bij uw toepassing ClickTime.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[200]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_203.png
[205]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_205.png