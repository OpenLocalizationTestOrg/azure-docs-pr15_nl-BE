<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met centrale bureaublad | Microsoft Azure" 
    description="Meer informatie over het centrale bureaublad met Azure Active Directory gebruiken om te schakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Zelfstudie: Azure Active Directory-integratie met centrale bureaublad

Het doel van deze zelfstudie is de integratie van de Azure en centrale bureaublad weergeven. Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een centrale desktop eenmalige aanmelding ingeschakeld abonnement / centraal desktop pachters

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor centrale bureaublad inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-central-desktop-tutorial/IC769558.png "Scenario")
##<a name="enabling-the-application-integration-for-central-desktop"></a>De integratie van toepassingen voor centrale bureaublad inschakelen

Het doel van deze sectie is een overzicht van de integratie van toepassingen voor centrale bureaublad inschakelen.

###<a name="to-enable-the-application-integration-for-central-desktop-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Desktop centraal, de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-central-desktop-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-central-desktop-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-central-desktop-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak**, **Centrale bureaublad**.

    ![Galerie van toepassing] (./media/active-directory-saas-central-desktop-tutorial/IC769559.png "Galerie van toepassing")

7.  Selecteer **Bureaublad centraal**in het resultatenvenster en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Centrale bureaublad] (./media/active-directory-saas-central-desktop-tutorial/IC769560.png "Centrale bureaublad")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij de centrale bureaublad met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat uploaden naar uw bureaublad centrale huurder.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).



###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, op de pagina **Centraal Desktop** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-central-desktop-tutorial/IC749323.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aanmelden bij de centrale bureaublad** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-central-desktop-tutorial/IC777628.png "Eenmalige aanmelding configureren")

3.  Voer de volgende stappen uit op de pagina **URL van App configureren** en klik op **volgende**: 

    -   In de **Centrale bureaublad aanmelden In URL** textbox, typ de URL van uw bureaublad centrale huurder (bijvoorbeeld: *http://contoso.centraldesktop.com*).
    -   Typ in het tekstvak URL voor centraal Desktop antwoord centrale URL van uw Desktop AssertionConsumerService (bv.: https://contoso.centraldesktop.com/saml2-assertion.php).

    >[AZURE.NOTE] Kunt u de waarde ophalen van de centrale desktop metagegevens (bv.: *http://contoso.centraldesktop.com*).

    ![App-URL configureren] (./media/active-directory-saas-central-desktop-tutorial/IC769561.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding centrale bureaublad** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-central-desktop-tutorial/IC769562.png "Eenmalige aanmelding configureren")

5.  Log in op uw **Bureaublad centrale** huurder.

6.  Ga naar **Instellingen**, klik op **Geavanceerd**en klik vervolgens op **Eenmalige aanmelding**.

    ![Setup - Geavanceerd] (./media/active-directory-saas-central-desktop-tutorial/IC769563.png "Setup - Geavanceerd")

7.  Op de één teken op pagina **-Instellingen** kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding instellingen] (./media/active-directory-saas-central-desktop-tutorial/IC769564.png "Eenmalige aanmelding instellingen")

    1.  Selecteer **Enable SAML v2 eenmalige aanmelding**.
    2.  Kopieer de waarde van de **URL van de uitgevende instelling** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding centrale bureaublad** en vervolgens plakken in het tekstvak **URL van de SSO** .
    3.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding centrale bureaublad** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in de **SSO-aanmeldings-URL** textbox.
    4.  Kopieer de **URL van de Service Single Sign-Out** waarde in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding centrale bureaublad** en vervolgens plakken in het tekstvak **URL van SSO-Logout** .

8.  In de sectie **Message handtekening verificatiemethode** moet u de volgende stappen uitvoeren:

    ![Bericht handtekening verificatiemethode] (./media/active-directory-saas-central-desktop-tutorial/IC769565.png "Bericht handtekening verificatiemethode")

    1.  Selecteer het **certificaat**.
    2.  Selecteer in de lijst **SSO-certificaat** **RSH SHA256**.
    3.  Maak een tekstbestand van de gedownloade certificaat, Kopieer de inhoud van het tekstbestand en vervolgens plakken in het certificaatveld voor **Eenmalige aanmelding** .  

        >[AZURE.TIP] Zie voor meer informatie, [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    4.  Selecteer **een koppeling naar de aanmeldingspagina SAMLv2 weergeven**.

9.  Klik op **bijwerken**.

10. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-central-desktop-tutorial/IC769566.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

AAD gebruikers kunnen inloggen, als ze worden ingericht naar de centrale Desktop-toepassing. In deze sectie wordt beschreven hoe AAD om gebruikersaccounts te maken in Centraal Desktop.

###<a name="to-provision-user-accounts-to-central-desktop"></a>Gebruikersaccounts aan centrale bureaublad inrichten:

1.  Log in op uw bureaublad centrale huurder.

2.  Ga naar **mensen \> interne leden**.

3.  Klik op **interne leden toevoegen**.

    ![Mensen] (./media/active-directory-saas-central-desktop-tutorial/IC781051.png "Mensen")

4.  Typ in het tekstvak **E-mailadres van de nieuwe leden** een AAD account u wilt verrichten, en klik vervolgens op **volgende**.

    ![E-mailadressen van nieuwe leden] (./media/active-directory-saas-central-desktop-tutorial/IC781052.png "E-mailadressen van nieuwe leden")

5.  Klik op **toevoegen aan interne leden**.

    ![Interne lid toevoegen] (./media/active-directory-saas-central-desktop-tutorial/IC781053.png "Interne lid toevoegen")

    >[AZURE.NOTE] De gebruikers die u hebt toegevoegd, ontvangt een e-mail met een van bevestigingskoppeling moet worden geklikt om de account te activeren.

>[AZURE.NOTE] Kunt u andere centrale Desktop gebruiker account hulpmiddelen voor het maken of API's die door centrale bureaublad bepaling AAD gebruikersaccounts

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-central-desktop-perform-the-following-steps"></a>Gebruikers toewijzen aan centrale bureaublad, moet u de volgende stappen uitvoeren:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Centraal Desktop** application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-central-desktop-tutorial/IC769567.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Ja")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
