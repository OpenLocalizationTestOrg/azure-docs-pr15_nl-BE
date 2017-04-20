<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met DocuSign | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en DocuSign."
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
    ms.date="08/16/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Zelfstudie: Azure Active Directory-integratie met DocuSign

Het doel van deze zelfstudie is de integratie van de Azure en DocuSign weergeven.
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

- Een geldig abonnement Azure
- Een huurder in DocuSign



Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1. [Inschakelen van de integratie van toepassingen voor DocuSign](#enabling-the-application-integration-for-docusign) 


2. [Eenmalige aanmelding configureren](#configuring-single-sign-on) 


3. [Inrichten van het account configureren](#configuring-account-provisioning) 


4. [Gebruikers toewijzen](#assigning-users) 

    ![Eenmalige aanmelding configureren][0]
 

## <a name="enabling-the-application-integration-for-docusign"></a>Inschakelen van de integratie van toepassingen voor DocuSign

Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor DocuSign.

### <a name="to-enable-the-application-integration-for-docusign-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor DocuSign, kunt u de volgende stappen uitvoeren:

1. Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Eenmalige aanmelding configureren][1]

2. Selecteer in de lijst map de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Eenmalige aanmelding configureren][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Op de pagina Wat wilt u doen dialoogvenster, klikt u op **een toepassing uit de galerie toevoegen**.

    ![Eenmalige aanmelding configureren][4]


6. Typ **DocuSign**in het zoekvak.

    ![Eenmalige aanmelding configureren][5]

7. **DocuSign**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Eenmalige aanmelding configureren][6]


## <a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij DocuSign met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.


### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1. Klik in de klassieke Azure portal, op de pagina **DocuSign integratie van** **eenmalige aanmelding configureren** om het dialoogvenster configureren van eenmalige aanmelding te openen.

    ![Eenmalige aanmelding configureren][7]

2. Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op DocuSign** en klik op volgende.

    ![Eenmalige aanmelding configureren][8]

3. Voer de volgende stappen uit op de pagina **Toepassingsinstellingen configureren** :

    ![Eenmalige aanmelding configureren][61]

    een. Typ in het tekstvak **URL aanmelden** `https://account.docusign.com/*`.  

    b. Typ in het tekstvak **id** `https://account.docusign.com/*`.  
   
    c. Klik op **volgende**. 


    > [AZURE.TIP] Het aanmelden op de URL en de id-waarden zijn alleen tijdelijke aanduidingen. Instructies voor het ophalen van de werkelijke waarden voor uw omgeving worden verderop in dit onderwerp behandeld.
 

4. Klik op **certificaat downloaden**en sla het bestand lokaal op uw computer op de pagina **configureren eenmalige aanmelding op DocuSign** .

    ![Eenmalige aanmelding configureren][10]


5. Log in op uw **DocuSign admin portal** als beheerder in een ander web browser-venster.


6. Klik in het navigatiemenu aan de linkerkant op **domeinen**.

    ![Eenmalige aanmelding configureren][51]

7. Klik in het rechterdeelvenster op **Claim domein**.

    ![Eenmalige aanmelding configureren][52]

8. Typ het bedrijfsdomein van uw in het dialoogvenster **Claim een domein** in het tekstvak **Domeinnaam** en klik op **aanvraag**. Zorg ervoor dat u controleren of het domein en de status actief is.

    ![Eenmalige aanmelding configureren][53]

9. Klik op **Id-Providers** in het menu aan de linkerkant.  

    ![Eenmalige aanmelding configureren][54]

10. Klik in het rechterdeelvenster op **Identiteitsprovider toevoegen**. 
    
    ![Eenmalige aanmelding configureren][55]

11. Op de pagina **Instellingen voor de Provider identiteiten** , moet u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren][56]


    een. Typ in het tekstvak **naam** een unieke naam voor de configuratie. Gebruik geen spaties.

    b. Kopieer de URL van de uitgevende instelling in de klassieke Azure portal, en plak deze in de textbox **Identity Provider uitgever** .

    c. In de klassieke Azure portal, de **Externe aanmeldings-URL**kopiëren en plak deze in de textbox **Identity Provider aanmeldings-URL** .

    d. In de klassieke Azure portal, de **Externe Logout URL**kopiëren en plak deze in de **Identity Provider Logout URL** textbox.

    e. Selecteer **teken AuthN**.

    f. **Verzoek verzenden AuthN van** **POST**selecteren.

    g. Selecteer als **logout verzoek door te sturen**, **boeken**. 


12. Kies het veld dat u wilt toewijzen met Azure AD Claim in de sectie **Aangepast kenmerk toewijzen** . In dit voorbeeld wordt de claim **emailaddress** toegewezen met de waarde van **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**. Dit is de standaardnaam van de claim van Azure advertentie voor de e-mailclaim. 

    > [AZURE.NOTE] Gebruik de juiste **gebruikers-id** Azure AD Docusign gebruiker toewijzen aan gebruikers toewijzen. Selecteer het juiste veld en voer de gewenste waarde op basis van de instellingen van uw organisatie.

    ![Eenmalige aanmelding configureren][57]

13. In de sectie **Identiteit Provider certificaat** op **Certificaat toevoegen**en vervolgens het certificaat dat u hebt gedownload van de klassieke portal Azure AD uploaden.   

    ![Eenmalige aanmelding configureren][58]

14. Klik op **Opslaan**.

15. In de sectie **Id-Providers** op **Acties**en klik vervolgens op de **eindpunten**.   

    ![Eenmalige aanmelding configureren][59]



10. Ga terug naar de pagina **Toepassingsinstellingen configureren** op de Azure klassieke portal. 

16. Op **DocuSign admin portal**, in de sectie **Weergave SAML 2.0 eindpunten** , de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren][60]

    een. Kopieer de **URL van de Provider uitgever**en plak deze in het tekstvak **id** op de klassieke Azure portal.

    b. De **Service Provider aanmeldings-URL**kopiëren en plak in **Teken op URL** textbox op de klassieke Azure portal.

    c.  Klik op **sluiten**  


10. Klik op **volgende**op de Azure klassieke portal. 


15. Selecteer de **bevestiging van enkele aanmelding-configuratie**op de Azure klassieke portal en klik op **volgende**.

    ![Toepassingen][14]

10. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .

    ![Toepassingen][15]
 

## <a name="configuring-account-provisioning"></a>Inrichten van het account configureren

Het doel van deze sectie is aan de contour van het inschakelen van gebruikersaanvragen van gebruikersaccounts in Active Directory op DocuSign.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1. Klik in de **Azure klassieke portal**op de pagina **DocuSign integratie** op **inrichten van de account configureren** om het dialoogvenster gebruikersaanvragen configureren te openen.

    ![Inrichten van het account configureren][30]

2. Op de pagina **instellingen en referenties van de beheerder** als u wilt dat automatische gebruiker wordt ingericht, bieden de referenties van de account van een DocuSign met voldoende rechten en klik op **volgende**. 

    ![Inrichten van het account configureren][31]

3. In het dialoogvenster **verbinding testen** Klik op **test starten**en na een geslaagde test, klikt u op **volgende**.

    ![Inrichten van het account configureren][32]

3. Klik op de pagina **bevestiging** op **Voltooien**.

    ![Inrichten van het account configureren][33]
 

## <a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

### <a name="to-assign-users-to-docusign-perform-the-following-steps"></a>Gebruikers toewijzen aan DocuSign, voert u de volgende stappen uit:

1. Maak een testaccount in **Azure klassieke portal**.

2. Klik op de pagina **DocuSign toepassingsintegratie** **gebruikers toewijzen**.

    ![Gebruikers toewijzen][40]
 

3. Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Gebruikers toewijzen][41]


U moet nu wacht 10 minuten en controleren of de account is gesynchroniseerd met de DocuSign.

U kunt de Voorzieningsstatus controleren door te klikken op Dashboard in de D op de pagina DocuSign toepassing integratie op de klassieke Azure portal als een eerste controlestap.

![Gebruikers toewijzen][42]

Een cyclus van met succes zijn gebruikers wordt aangegeven door een bijbehorende status:

![Gebruikers toewijzen][43]


Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster.

Zie Inleiding tot het Access-venster voor meer informatie over het Access-venster.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_00.png
[1]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_01.png
[6]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_02.png
[7]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_03.png
[8]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_04.png
[9]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_05.png
[10]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_06.png

[14]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_10.png
[15]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_11.png

[30]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_400.png
[31]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_12.png
[32]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_13.png
[33]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_14.png



[40]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_15.png
[41]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_16.png
[42]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_17.png
[43]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_18.png

[51]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_29.png