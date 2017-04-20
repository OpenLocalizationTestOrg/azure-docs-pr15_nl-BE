<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Qlik Sense onderneming | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Qlik Sense onderneming."
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
    ms.date="08/31/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a>Zelfstudie: Azure Active Directory-integratie met Qlik Sense onderneming

In deze zelfstudie leert u hoe Qlik Sense Enterprise integratie met Azure Active Directory (AD Azure).

Qlik Sense Enterprise integreren met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot de onderneming van Qlik Sense heeft
- U kunt gebruikers automatisch ophalen ondertekend bij Qlik Sense Enterprise (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met Qlik Sense onderneming, moet u de volgende items:

- Een abonnement op Azure AD
- Een onderneming van Qlik Sense eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving.

Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Qlik Sense onderneming uit de galerie toe te voegen.
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a>Qlik Sense onderneming uit de galerie toe te voegen.
De integratie van Qlik Sense onderneming in Azure AD configureren, moet u Qlik Sense onderneming uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Qlik Sense onderneming toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Active Directory][1]
2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Qlik Sense onderneming**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_01.png)

7. Selecteer **Qlik Sense Enterprise**en klik op **Voltooien** als de toepassing wilt toevoegen in het resultatendeelvenster.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Qlik Sense onderneming op basis van een testgebruiker "Simon Britta" genoemd.

Voor eenmalige aanmelding wilt werken, moet AD Azure weten wat de gebruiker tegenhanger van Qlik Sense onderneming aan een gebruiker in AD Azure. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gebruiker in Qlik Sense onderneming worden vastgesteld.

Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Qlik Sense onderneming toewijzen.

Als u wilt configureren en testen Azure AD eenmalige aanmelding met Qlik Sense onderneming, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
3. **[Gebruikers maken een onderneming Qlik Sense test](#creating-a-qliksense-enterprise-test-user)** - hebben een tegenhanger van Britta Simon in Qlik Sense onderneming die is gekoppeld aan de weergave Azure AD van haar.
4. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie, Azure AD eenmalige aanmelding in de klassieke portal inschakelen en eenmalige aanmelding configureren in uw toepassing Qlik Sense onderneming.


**Configureren van eenmalige aanmelding Azure AD met Qlik Sense onderneming, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal op de pagina **Qlik Sense Enterprise** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.
     
    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Qlik Sense Enterprise** **Azure AD Single Sign-On**selecteren en klik op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_03.png) 

3. Voer de volgende stappen uit op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_04.png) 

    een. In het **Teken op URL** -tekstvak typt u de URL die wordt gebruikt door gebruikers voor aanmelding bij uw onderneming van Qlik Sense-toepassing met behulp van het volgende patroon: **https://\<Qlik Sense volledig Qualifed hostnaam\>: 443 / < virtuele Proxy het voorvoegsel\>/samlauthn/**.
    
    > [AZURE.NOTE] Opmerking de afsluitende schuine streep aan het eind van deze URI.  Dit is vereist.

    b. Klik op **volgende**
 
4. Klik op de pagina **configureren eenmalige aanmelding bij Qlik Sense onderneming** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_05.png)

    een. Klik op **metagegevens downloaden**en sla het bestand op uw computer.  Dit metagegevensbestand bewerken voordat het uploaden naar de server Qlik Sense worden voorbereid.

    b. Klik op **volgende**.

5. De XML-metagegevens van Federation-bestand voorbereiden zodat u die naar de server Qlik Sense uploaden kunt.

    > [AZURE.NOTE] Voordat u de metagegevens IdP uploadt naar de server Qlik Sense, het bestand moet worden bewerkt om informatie voor een correcte werking tussen Azure AD verwijderen en Qlik Sense-server.

    ![QlikSense][qs24]

    een. Open het bestand FederationMetaData.xml is gedownload van Azure in een teksteditor.

    b. Zoekt de waarde **RoleDescriptor**.  Er zijn vier posten (twee paar begincodes en afsluitcodes element).

    c. Verwijderen van de codes RoleDescriptor en alle gegevens uit het bestand.

    d. Sla het bestand op en bewaar het in de buurt voor gebruik verderop in dit document.

6. Ga naar de Qlik Sense Qlik Management Console (QMC) als een gebruiker die virtuele proxyconfiguraties kunt maken.

7. Klik in de QMC, op het menu-item virtuele Proxy.

    ![QlikSense][qs6] 

8. Klik onderaan in het scherm op de nieuwe knop maken.

    ![QlikSense][qs7]

9. Het scherm virtuele proxy bewerken wordt weergegeven.  Aan de rechterkant van het scherm is een menu voor het zichtbaar maken van de configuratie-opties.

    ![QlikSense][qs9]

10. Met de optie identificatie gecontroleerd, voer de identificatiegegevens voor de Azure virtual proxyconfiguratie.

    ![QlikSense][qs8]  
    
    een. Het veld Omschrijving is een beschrijvende naam voor de virtuele proxyconfiguratie.  Voer een waarde voor een beschrijving.
    
    b. Het veld voorvoegsel identificeert het eindpunt van de virtuele proxy voor de verbinding met Qlik Sense met Azure AD Single Sign-On.  Voer de naam van een uniek voorvoegsel voor deze virtuele proxy.

    c. Sessie time-out bij inactiviteit (minuten) is de time-out voor verbindingen via deze virtuele proxy.

    d. De header-naam van de sessie-cookie is de cookienaam van de opslaan van de sessie-id voor de gebruiker na een geslaagde verificatie krijgt Qlik Sense-sessie.  Deze naam moet uniek zijn.

11. Klik op de optie verificatie zichtbaar te maken.  Verificatie wordt het scherm weergegeven.

    ![QlikSense][qs10]

    een. De vervolgkeuzelijst **anonieme toegangsmodus** bepaalt als anonieme gebruikers toegang krijgen Qlik Sense via de virtuele proxy tot kunnen.  De standaardoptie is geen anonieme gebruiker.

    b. De vervolgkeuzelijst **verificatiemethode** bepaalt dat het schema voor verificatie van de virtuele-proxy wordt gebruikt.  SAML selecteren in de vervolgkeuzelijst.  Als gevolg daarvan weergegeven meer opties.

    c. Invoer voeren de hostname-gebruikers toegang tot Qlik Sense via deze virtuele SAML-proxy in het **SAML-host URI veld**.  De hostnaam is de uri van de server Qlik Sense.

    d. Voer in de **SAML-entiteit-ID**ingevoerd voor het SAML host URI veld dezelfde waarde.

    e. De **metagegevens van SAML IdP** is het bestand dat eerder bewerkt in de sectie **Federation metagegevens bewerken van Azure AD-configuratie** .  Informatie voor een correcte werking tussen Azure AD verwijderen **voordat u de metagegevens IdP uploadt het bestand moet worden bewerkt** en Qlik Sense-server.  **Raadpleeg de instructies hierboven als het bestand nog wordt bewerkt.**  Klik op de knop Bladeren en selecteer de van bewerkte metagegevensbestand te uploaden naar de virtuele proxyconfiguratie als het bestand is bewerkt.

    f. Voer de naam van het kenmerk of schema: referentie voor het SAML-kenmerk voor de **gebruikers-id** Azure AD verstuurt naar de server Qlik Sense.  Schema-informatie is beschikbaar in de configuratie van Azure app schermen boeken.  Gebruik het kenmerk name, **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name invoeren**.

    g. Geef de waarde voor de **map** die wordt gekoppeld aan gebruikers wanneer ze via Azure AD Qlik Sense-server verifieert.  Vastgelegde waarden moeten tussen **vierkante haken []**.  Voer de naam van het kenmerk in deze tekst in **zonder** vierkante haken voor het gebruik van een kenmerk in de Azure AD SAML-bevestiging verzonden.

    h. Het **SAML-ondertekeningsalgoritme** stelt u de serviceprovider (in dit geval Qlik Sense server) certificaat ondertekenen voor de configuratie van de virtuele proxy.  Als Qlik Sense-server een vertrouwd certificaat gegenereerd met Microsoft Enhanced RSA en AES Cryptographic Provider gebruikt, wijzigen in de SAML-ondertekening algoritme **SHA-256**.

    ik. De sectie toewijzing van SAML kenmerk kan extra kenmerken zoals groepen worden verzonden naar Qlik Sense voor gebruik op de beveiligingsregels voor verbindingen.

12. Klik op de optie zichtbaar maken voor taakverdeling.  Load Balancing wordt het scherm weergegeven.

    ![QlikSense][qs11]

13. Klik op de nieuwe server knooppunt knop toevoegen, selecteer engine knooppunt of knooppunten Qlik Sense wordt sessies voor load balancing doeleinden verzenden en klik op de knop toevoegen.

    ![QlikSense][qs12]

14. Klik op de optie Geavanceerd menu zichtbaar te maken. Het geavanceerde scherm wordt weergegeven.

    ![QlikSense][qs13]

    een. De Host-lijst wit identificeert hostnamen die worden geaccepteerd wanneer u verbinding maakt met de server Qlik Sense.  **Voer de hostnaam die gebruikers opgeven wanneer u verbinding maakt met server Qlik Sense.** De hostnaam is dezelfde waarde als het SAML-uri die host zonder de https://.

15. Klik op de knop toepassen.

    ![QlikSense][qs14]

16. Klik op OK om de waarschuwing dat wordt gekoppeld aan de virtuele proxy proxy's worden gestart te accepteren.

    ![QlikSense][qs15]

17. Aan de rechterkant van het scherm, de gekoppelde items-menu wordt weergegeven.  Klik op de optie proxy's.

    ![QlikSense][qs16]

18. De proxy-scherm wordt weergegeven.  Klik op de knop koppeling onder aan het koppelen van een proxy naar de virtuele proxy.

    ![QlikSense][qs17]

19. Selecteer het knooppunt proxy die ondersteuning voor deze virtuele proxyverbinding en klik op de knop koppelen.  Na koppelen, worden de proxy weergegeven onder de bijbehorende proxy's.

    ![QlikSense][qs18]
    ![QlikSense][qs19]

20. Na ongeveer vijf tot tien seconden verschijnt het bericht QMC vernieuwen.  Klik op de knop Vernieuwen QMC.

    ![QlikSense][qs20]

21. Wanneer de QMC wordt vernieuwd, klik op de menuopdracht virtuele proxy's. Het nieuwe SAML virtuele proxy wordt vermeld in de tabel op het scherm.  Één klik op de virtuele proxy post.

    ![QlikSense][qs51]

22. Onderaan het scherm op wordt de knop downloaden SP metagegevens geactiveerd.  Klik op Download SP metagegevens op te slaan in de metagegevens van een bestand.

    ![QlikSense][qs52]

23. Open het bestand van sp-metagegevens.  Houd rekening met de **id van de entiteit** -post en de **AssertionConsumerService** post.  Deze waarden komen overeen met de **id** en de **URL aanmelden** in de configuratie van de toepassing AD Azure. Als ze niet overeenkomende vervolgens vervangt u deze in de configuratiewizard Azure AD App.

    ![QlikSense][qs53]

24. In de klassieke portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.
    
    ![Azure AD Single Sign-On][10]

25. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
 
    ![Azure AD Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.


![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster als volgt:  ![Azure AD test gebruikers maken](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Klik op de pagina van het dialoogvenster **Gebruikersprofiel** als volgt: ![Azure AD test gebruikers maken](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   


### <a name="creating-an-qlik-sense-enterprise-test-user"></a>Een testgebruiker Qlik Sense onderneming maken

In dit gedeelte maakt u een gebruiker met de naam Britta Simon in Qlik Sense onderneming. Neem samen met Qlik Sense Enterprise support team gebruikers toevoegen in het Qlik Sense Enterprise-platform.


### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

In dit gedeelte vindt inschakelen u Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan Qlik Sense Enterprise.

![Gebruiker toewijzen][200] 

**Britta Simon Qlik Sense onderneming toewijzen, moet u de volgende stappen uitvoeren:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst toepassingen **Qlik Sense onderneming**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_50.png) 

3. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]


## <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In dit gedeelte vindt u Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Wanneer u op de tegel Qlik Sense onderneming in het Access-venster, u moet krijgen automatisch ondertekend voor toegang tot de bedrijfstoepassing Qlik Sense.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_205.png

[qs6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs21]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_21.png
[qs22]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_22.png
[qs23]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_23.png
[qs24]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs25]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_25.png
[qs26]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_26.png
[qs51]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png