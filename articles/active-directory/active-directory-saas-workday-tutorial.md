<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met werkdag | Microsoft Azure" 
    description="Meer informatie over het werkdag met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-workday"></a>Zelfstudie: Azure Active Directory-integratie met werkdag
  
Het doel van deze zelfstudie is de integratie van de Azure en werkdag weergeven. Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder in een werkdag
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor de werkdag inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikersaanvragen configureren

![Scenario] (./media/active-directory-saas-workday-tutorial/IC782919.png "Scenario")

##<a name="enabling-the-application-integration-for-workday"></a>De integratie van toepassingen voor de werkdag inschakelen
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor televergaderingen.

###<a name="to-enable-the-application-integration-for-workday-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor een werkdag, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-workday-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-workday-tutorial/IC700994.png "Toepassingen")

4.  Om te openen in de **Galerie van toepassing**, klikt u op **Een App toevoegen**en klik op **toevoegen een toepassing voor mijn organisatie te gebruiken**.

    ![Wat wilt u doen?] (./media/active-directory-saas-workday-tutorial/IC700995.png "Wat wilt u doen?")

5.  Typ in het **zoekvak**de **werkdag**.

    ![Werkdag] (./media/active-directory-saas-workday-tutorial/IC701021.png "Werkdag")

6.  In het resultatendeelvenster **werkdag**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Werkdag] (./media/active-directory-saas-workday-tutorial/IC701022.png "Werkdag")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij de werkdag met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u vereist voor het maken van een base-64 gecodeerde certificaat.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik op de pagina **werkdag** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-workday-tutorial/IC782920.png "Eenmalige aanmelding configureren")

2.  Op de pagina **Hoe wilt u dat gebruikers aan te melden op werkdag** Selecteer **AD Azure Microsoft Single Sign-On**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-workday-tutorial/IC782921.png "Eenmalige aanmelding configureren")

3.  Voer de volgende stappen uit op de pagina **URL van App configureren** en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-workday-tutorial/IC782957.png "App-URL configureren")

    een. Typ in het tekstvak **Teken op URL** de URL die wordt gebruikt door de gebruikers aan te melden het patroon van de volgende werkdag:`https://impl.workday.com/<tenant>/login-saml2.htmld`

    b.  Typ in het tekstvak **URL van werkdag antwoord op** de werkdag antwoord URL met behulp van het volgende patroon:`https://impl.workday.com/<tenant>/login-saml.htmld`

    >[AZURE.NOTE]De URL van uw antwoord moet hebben een subdomein (bijv.: www, wd2, wd3, wd3-impl, wd5, wd5-impl). 
    >Met behulp van iets als '*http://www.myworkday.com*' werkt, maar niet het '*http://myworkday.com*'. 
 
4.  Op de pagina **configureren eenmalige aanmelding op werkdag** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-workday-tutorial/IC782922.png "Eenmalige aanmelding configureren")

5.  Log in op uw site werkdag bedrijf als beheerder in een ander web browser-venster.

6.  Ga naar **Menu \> Workbench**.

    ![Workbench] (./media/active-directory-saas-workday-tutorial/IC782923.png "Workbench")

7.  Ga naar **beheer**.

    ![Account beheer] (./media/active-directory-saas-workday-tutorial/IC782924.png "Account beheer")

8.  Ga naar de **huurder instellingen – beveiliging bewerken**.

    ![Huurder beveiliging bewerken] (./media/active-directory-saas-workday-tutorial/IC782925.png "Huurder beveiliging bewerken")

9.  Voer de volgende stappen uit in de sectie **URL's omleiden** :

    ![URL's omleiden] (./media/active-directory-saas-workday-tutorial/IC7829581.png "URL's omleiden")

    een. Klik op de **rij toevoegen**.

    b. Typ in het tekstvak **Omleiden aanmeldings-URL** en **Mobile Omleidings-URL** textbox, de **Werkdag huurder URL** die u hebt opgegeven op de pagina **Configureren App-URL** van de portal voor Azure klassieke.
    
    c. Kopieer de **URL van de Service Single Sign-Out**in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op werkdag** en vervolgens plakken in het tekstvak **Logout Omleidings-URL** .

    d.  Typ de naam van de omgeving in textbox **omgeving** .  


    >[AZURE.NOTE] De waarde van het kenmerk omgeving is gebonden aan de waarde van de URL van de huurder:
    >
    >-   Als de domeinnaam van de pachter werkdag URL met impl begint (bv.: *https://impl.workday.com/\<huurder\>/login-saml2.htmld*), het **milieu** -kenmerk moet worden ingesteld op de uitvoering.
    >-   Als de naam met iets anders begint, moet u contact opnemen met de werkdag voor de overeenkomende waarde in de **omgeving** .

10. In de sectie **SAML Setup** kunt u de volgende stappen uitvoeren:

    ![SAML-Setup] (./media/active-directory-saas-workday-tutorial/IC782926.png "SAML-Setup")

    een.  Selecteer de **SAML-verificatie inschakelen**.

    b.  Klik op de **rij toevoegen**.

11. In de sectie SAML id-Providers de volgende stappen uitvoeren:

    ![SAML id-Providers] (./media/active-directory-saas-workday-tutorial/IC7829271.png "SAML id-Providers")

    een. Typ een naam in het tekstvak naam van identiteit (bijv.: *SPInitiatedSSO*).

    b. In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op werkdag** de **Identity Provider-ID** -waarde kopiëren en vervolgens plakken in het tekstvak van de **uitgevende instelling** .

    c. Selecteer **werkdag Initialted Logout inschakelen**.

    d. Kopieer de **URL van de Service Single Sign-Out** waarde in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op werkdag** en plak deze in de textbox **Logout verzoek-URL** .


    e. Klik op **Identity Provider een certificaat met openbare sleutel**, en klik vervolgens op **maken**. 

    ![Maken] (./media/active-directory-saas-workday-tutorial/IC782928.png "Maken")

    f. Klik op **maken x509 openbare sleutel**. 
        
    ![Maken] (./media/active-directory-saas-workday-tutorial/IC782929.png "Maken")


1. Voer de volgende stappen uit in de sectie **Weergave x509 openbare sleutel** : 

    ![Openbare sleutel x509 weergeven] (./media/active-directory-saas-workday-tutorial/IC782930.png "Openbare sleutel x509 weergeven") 

    een. Typ in het tekstvak **naam** een naam voor het certificaat (bijvoorbeeld: *beschermingsmiddelen\_SP*).
        
    b. Typ vanaf de waarde van het kenmerk van uw certificaat geldig in de textbox **Geldig vanaf** .
    
    c.  Typ de waarde van het kenmerk van uw certificaat geldig in de textbox **Geldig tot** .
        
    >[AZURE.NOTE] U krijgt de geldige en geldig tot de datum van het gedownloade certificaat door erop te dubbelklikken. De datums worden weergegeven onder het tabblad **Details** .

    d. Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.  

    >[AZURE.TIP] Zie voor meer informatie, [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    e.  De base-64 gecodeerde certificaat openen in Kladblok en kopieer de inhoud van deze.
    
    f.  Plak de inhoud van het Klembord in de textbox **certificaat** .
    
    g.  Klik op **OK**.

12.  Voer de volgende stappen uit: 

    ![SSO-configuratie] (./media/active-directory-saas-workday-tutorial/IC7829351111.png "SSO-configuratie")

    een.  Schakel de **x509 combinatie van een persoonlijke sleutel**.

    b.  Typ in het tekstvak **Service Provider-ID** **http://www.workday.com**.

    c.  Selecteer **SP geïnitieerd SAML-verificatie inschakelen**.

    d.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op werkdag** de **Single Sign-On Service URL** -waarde kopiëren en vervolgens plakken in het tekstvak **URL van de SSO-IdP** .
     
    e. Selecteer **doen SP geïnitieerde verificatieaanvraag niet verkleinen**.

    f. Als **Aanvragen handtekening verificatiemethode**, selecteer **SHA256**. 
        
    ![Verificatie handtekeningmethode aanvragen] (./media/active-directory-saas-workday-tutorial/IC782932.png "Verificatie handtekeningmethode aanvragen") 
 
    g. Klik op **OK**. 
        
    ![OK] (./media/active-directory-saas-workday-tutorial/IC782933.png "OK")

12. Klik op **volgende**in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op de werkdag** . 

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-workday-tutorial/IC782934.png "Eenmalige aanmelding configureren")

13. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** . 

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-workday-tutorial/IC782935111.png "Eenmalige aanmelding configureren")



##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Als u een testgebruiker ingericht in een werkdag, moet u contact opnemen met het ondersteuningsteam van de werkdag.  
Het ondersteuningsteam werkdag zal de gebruiker voor u maken.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-workday-perform-the-following-steps"></a>Gebruikers toewijzen aan een werkdag, moet u de volgende stappen uitvoeren:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **werkdag **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-workday-tutorial/IC782935.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-workday-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.