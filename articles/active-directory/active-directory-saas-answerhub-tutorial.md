<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met AnswerHub | Microsoft Azure" 
    description="Meer informatie over het AnswerHub met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-answerhub"></a>Zelfstudie: Azure Active Directory-integratie met AnswerHub

Het doel van deze zelfstudie is de integratie van de Azure en [AnswerHub](http://www.dzonesoftware.com/products/answerhub-question-answer-software)weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een [AnswerHub](http://www.dzonesoftware.com/products/answerhub-question-answer-software) eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan AnswerHub kan worden naar één teken in de toepassing van de AnswerHub bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor AnswerHub
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-answerhub-tutorial/IC785165.png "Scenario")

##<a name="enabling-the-application-integration-for-answerhub"></a>Inschakelen van de integratie van toepassingen voor AnswerHub

Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor AnswerHub.

###<a name="to-enable-the-application-integration-for-answerhub-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor AnswerHub, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-answerhub-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-answerhub-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-answerhub-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-answerhub-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **AnswerHub**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-answerhub-tutorial/IC785166.png "Galerie van toepassing")

7.  **AnswerHub**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![AnswerHub] (./media/active-directory-saas-answerhub-tutorial/IC785167.png "AnswerHub")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij AnswerHub met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat-bestand te maken.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **AnswerHub** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-answerhub-tutorial/IC785168.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op AnswerHub** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-answerhub-tutorial/IC785169.png "Eenmalige aanmelding configureren")

3.  Typ de URL met het volgende patroon '*https://company.answerhub.com*' op de pagina **Configureren App-URL** in het tekstvak **AnswerHub teken In de URL** en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-answerhub-tutorial/IC785170.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op AnswerHub** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-answerhub-tutorial/IC785171.png "Eenmalige aanmelding configureren")

5.  Log in op uw site AnswerHub bedrijf als beheerder in een ander web browser-venster.
    >[AZURE.NOTE] Als u ondersteuning bij het configureren van AnswerHub, neem dan contact op met [het ondersteuningsteam van AnswerHub](mailto:success@answerhub.com. ).








6.  Ga naar **beheer**.

7.  Klik op het tabblad **gebruikers en groepen** .

8.  Klik in het navigatiedeelvenster aan de linkerzijde, in de sectie **Instellingen voor sociale** **SAML-Setup**.

9.  Klik op de tab **IDP-Config** .

10. Voer de volgende stappen uit op het tabblad **IDP-Config** :

    ![SAML-Setup] (./media/active-directory-saas-answerhub-tutorial/IC785172.png "SAML-Setup")

    1.  De waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het tekstvak **IDP aanmeldings-URL** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op AnswerHub** .
    2.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij AnswerHub** de waarde van de **Externe Logout URL** kopiëren en vervolgens plakken in het tekstvak **URL van IDP Logout** .
    3.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij AnswerHub** de **Indeling van de id** -waarde kopiëren en vervolgens plakken in de textbox **IDP id indeling** .
    4.  Klik op **certificaten en sleutels**.

11. Klik op het tabblad certificaten en sleutels kunt u de volgende stappen uitvoeren:

    ![Sleutels en certificaten] (./media/active-directory-saas-answerhub-tutorial/IC785173.png "Sleutels en certificaten")

    1.  Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.  

        >[AZURE.TIP] Zie voor meer informatie, [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    2.  De base-64 gecodeerde certificaat in Kladblok te openen, de inhoud ervan kopiëren naar het Klembord en plak deze vervolgens naar de **Openbare sleutel (x 509-indeling) IDP** textbox.
    3.  Klik op **Opslaan**.

12. Klik op het tabblad **IDP Config** **Opslaan**.

13. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-answerhub-tutorial/IC785174.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Om gebruikers aan te melden bij AnswerHub Azure AD, moeten zij worden ingericht in AnswerHub.  
Bij AnswerHub is het inrichten van een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder uw bedrijf **AnswerHub** site.

2.  Ga naar **beheer**.

3.  Klik op het tabblad **gebruikers en groepen** .

4.  Klik in het navigatiedeelvenster aan de linkerzijde, in de sectie **Gebruikers beheren** op **gebruikers maken of importeren**.

    ![Gebruikers & groepen] (./media/active-directory-saas-answerhub-tutorial/IC785175.png "Gebruikers & groepen")

5.  Typ het **e-mailadres**, de **gebruikersnaam** en het **wachtwoord** van een geldige Azure Active Directory-account verrichten in de verwante tekstvakken te en klik op **Opslaan**.

>[AZURE.NOTE] Kunt u een andere AnswerHub gebruiker account maken van hulpprogramma's of API's geleverd door AnswerHub bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-answerhub-perform-the-following-steps"></a>Gebruikers toewijzen aan AnswerHub, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **AnswerHub **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-answerhub-tutorial/IC785176.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-answerhub-tutorial/IC767830.png "Ja")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
