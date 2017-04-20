<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Picturepark | Microsoft Azure" 
    description="Meer informatie over het Picturepark met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-picturepark"></a>Zelfstudie: Azure Active Directory-integratie met Picturepark
  
Het doel van deze zelfstudie is de integratie van de Azure en Picturepark weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder Picturepark
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Picturepark kan worden naar één teken in de toepassing van de Picturepark bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Picturepark
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-picturepark-tutorial/IC795055.png "Scenario")

##<a name="enabling-the-application-integration-for-picturepark"></a>Inschakelen van de integratie van toepassingen voor Picturepark
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Picturepark.

###<a name="to-enable-the-application-integration-for-picturepark-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Picturepark, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-picturepark-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-picturepark-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-picturepark-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-picturepark-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **Picturepark**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-picturepark-tutorial/IC795056.png "Galerie van toepassing")

7.  **Picturepark**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Picturepark] (./media/active-directory-saas-picturepark-tutorial/IC795057.png "Picturepark")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Picturepark met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Configureren van eenmalige aanmelding voor Picturepark moet u een waarde uit een certificaat ophalen.  
Als u niet bekend met deze procedure bent, raadpleegt u [waarde van een certificaat ophalen](http://youtu.be/YKQF266SAxI)...

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Picturepark** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-picturepark-tutorial/IC795058.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Picturepark** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-picturepark-tutorial/IC795059.png "Eenmalige aanmelding configureren")

3.  Typ de URL met het volgende patroon '*http://company.picturepark.com*' op de pagina **Configureren App-URL** in het tekstvak **Picturepark aanmelden op de URL** en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-picturepark-tutorial/IC795060.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op Picturepark** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-picturepark-tutorial/IC795061.png "Eenmalige aanmelding configureren")

5.  Log in op uw site Picturepark bedrijf als beheerder in een ander web browser-venster.

6.  In de werkbalk op de bovenkant, klik op **Systeembeheer**en klik op **Management-Console**.

    ![Management Console] (./media/active-directory-saas-picturepark-tutorial/IC795062.png "Management Console")

7.  Klik op **verificatie**en klik op **id-providers**.

    ![Verificatie] (./media/active-directory-saas-picturepark-tutorial/IC795063.png "Verificatie")

8.  In de sectie **identiteit providerconfiguratie** , kunt u de volgende stappen uitvoeren:

    ![Identity provider configureren] (./media/active-directory-saas-picturepark-tutorial/IC795064.png "Identity provider configureren")

    1.  Klik op **toevoegen**.
    2.  Typ een naam voor uw configuratie.
    3.  Selecteer **als standaard instellen**.
    4.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Picturepark** de waarde van **SAML SSO-URL** kopiëren en plak deze in de **URI van de uitgevende instelling** textbox.
    5.  **De waarde** van het geëxporteerde certificaat kopiëren en plak deze in de textbox **Vertrouwde uitgever Thumb afdrukken** .  

        >[AZURE.TIP]Zie voor meer informatie [hoe u kunt ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

    6.  Klik op **JoinDefaultUsersGroup**.
    7.  Als het kenmerk **Emailaddress** in de textbox **Claim** , typt u **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
        ![Configuratie] (./media/active-directory-saas-picturepark-tutorial/IC795065.png "Configuratie")
    8.  Klik op **Opslaan**.

9.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-picturepark-tutorial/IC795066.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij Picturepark Azure AD, moeten zij worden ingericht in Picturepark.  
Bij Picturepark is het inrichten van een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw huurder **Picturepark** .

2.  In de werkbalk op de bovenkant, klik op **Systeembeheer**en klik op **gebruikers**.

    ![Gebruikers] (./media/active-directory-saas-picturepark-tutorial/IC795067.png "Gebruikers")

3.  Klik op **Nieuw**op het tabblad **gebruikers-overzicht** .

    ![Gebruikersbeheer] (./media/active-directory-saas-picturepark-tutorial/IC795068.png "Gebruikersbeheer")

4.  Voer de volgende stappen uit in het dialoogvenster **Gebruiker maken** :

    ![Gebruiker maken] (./media/active-directory-saas-picturepark-tutorial/IC795069.png "Gebruiker maken")

    1.  Type de: **e-mailadres**, **wachtwoord**, **Bevestig het wachtwoord**, **Voornaam**, **Achternaam**, **bedrijf**, **land**, **ZIP**, **stad** van een geldige Azure Active Directory-gebruiker gewenste ot bepaling in de verwante tekstvakken.
    2.  Selecteer een **taal**.
    3.  Klik op **maken**.

>[AZURE.NOTE]Kunt u een andere Picturepark gebruiker account maken van hulpprogramma's of API's geleverd door Picturepark bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-picturepark-perform-the-following-steps"></a>Gebruikers toewijzen aan Picturepark, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Picturepark **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-picturepark-tutorial/IC795070.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-picturepark-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.