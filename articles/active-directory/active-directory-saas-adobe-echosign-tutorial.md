<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Adobe-EchoSign | Microsoft Azure" 
    description="Meer informatie over het EchoSign van Adobe met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-adobe-echosign"></a>Zelfstudie: Azure Active Directory-integratie met Adobe-EchoSign

Het doel van deze zelfstudie is de integratie van de Azure en Adobe EchoSign weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een enkel Adobe EchoSign aanmelden ingeschakeld abonnement

Na het voltooien van deze zelfstudie, is de Azure AD-gebruikers die u hebt toegewezen aan de EchoSign van Adobe mogelijk voor één teken in de toepassing van de Adobe EchoSign bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor Adobe EchoSign inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-adobe-echosign-tutorial/IC789511.png "Scenario")
##<a name="enabling-the-application-integration-for-adobe-echosign"></a>De integratie van toepassingen voor Adobe EchoSign inschakelen

Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor Adobe EchoSign overzicht.

###<a name="to-enable-the-application-integration-for-adobe-echosign-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor EchoSign van Adobe, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-adobe-echosign-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-adobe-echosign-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-adobe-echosign-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-adobe-echosign-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak**, **Adobe EchoSign**.

    ![Galerie van toepassing] (./media/active-directory-saas-adobe-echosign-tutorial/IC789514.png "Galerie van toepassing")

7.  Selecteer **Adobe EchoSign**in het resultatenvenster en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Adobe EchoSign] (./media/active-directory-saas-adobe-echosign-tutorial/IC789515.png "Adobe EchoSign")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Adobe EchoSign met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat-bestand te maken.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Adobe EchoSign** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-adobe-echosign-tutorial/IC789516.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aanmelden bij Adobe EchoSign** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-adobe-echosign-tutorial/IC789517.png "Eenmalige aanmelding configureren")

3.  Typ de URL met het volgende patroon '*https://company.echosign.com/*' op de pagina **URL van App configureren** in de **Adobe EchoSign teken op URL** textbox, en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-adobe-echosign-tutorial/IC789518.png "App-URL configureren")

4.  Klik op **certificaat downloaden**en sla het bestand op uw computer op de pagina **configureren eenmalige aanmelding bij Adobe EchoSign** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-adobe-echosign-tutorial/IC789519.png "Eenmalige aanmelding configureren")

5.  Log in op de site van Adobe EchoSign bedrijf als beheerder in een ander web browser-venster.

6.  Klik op **Account**in het menu aan de bovenkant, en klik in het navigatiedeelvenster op de linker chip **SAML-instellingen** onder **Instellingen**.

    ![Account] (./media/active-directory-saas-adobe-echosign-tutorial/IC789520.png "Account")

7.  In de sectie SAML-instellingen kunt u de volgende stappen uitvoeren:

    ![SAML-instellingen] (./media/active-directory-saas-adobe-echosign-tutorial/IC789521.png "SAML-instellingen")

    1.  Als het **SAML-modus**, selecteer **SAML verplicht**.
    2.  Selecteer **EchoSign Account beheerders zich aanmelden met hun referenties EchoSign toestaan**.
    3.  Als het **Maken van een gebruikersaccount**, selecteer **automatisch toevoegen van gebruikers via de SAML geverifieerd**.

8.  Verplaatsen, de volgende stappen uit te voeren:

    ![SAML-instellingen] (./media/active-directory-saas-adobe-echosign-tutorial/IC789522.png "SAML-instellingen")

    1.  De **Entiteit-ID** -waarde kopiëren en vervolgens plakken in het tekstvak **IdP entiteits-ID** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Adobe EchoSign** .
    2.  In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding bij Adobe EchoSign** de waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het tekstvak **IdP aanmeldings-URL** .
    3.  In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding bij Adobe EchoSign** de waarde van de **Externe Logout URL** kopiëren en vervolgens plakken in het tekstvak **URL van IdP Logout** .
    4.  Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.  

        >[AZURE.TIP] Zie voor meer informatie, [het converteren van een binaire certificaat in een bestand](http://youtu.be/PlgrzUZ-Y1o) 
 5.  De base-64 gecodeerde certificaat openen in Kladblok, de inhoud ervan kopiëren naar het Klembord en plak deze vervolgens naar de textbox **IdP certificaat** 6.  Klik op **wijzigingen opslaan**.

9.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-adobe-echosign-tutorial/IC789523.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Om gebruikers aan te melden bij de Adobe EchoSign Azure AD, moeten zij worden ingericht in Adobe EchoSign.  
Adobe-EchoSign is inrichten een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder uw bedrijf **Adobe EchoSign** site.

2.  In het menu aan de bovenkant, klik op **rekening**en en klik vervolgens op **gebruikers en groepen**in het navigatiedeelvenster op de linker-chip, en klik vervolgens op **een nieuwe gebruiker**.

    ![Account] (./media/active-directory-saas-adobe-echosign-tutorial/IC789524.png "Account")

3.  Voer de volgende stappen uit in de sectie **Nieuwe gebruiker maken** :

    ![Gebruiker maken] (./media/active-directory-saas-adobe-echosign-tutorial/IC789525.png "Gebruiker maken")

    1.  Typ het **E-mailadres**, **Voornaam** en **Achternaam** van een geldige AAD-account die u wilt inrichten in de verwante tekstvakken.
    2.  Klik op **gebruiker maken**.

        >[AZURE.NOTE] De accounthouder Azure Active Directory ontvangt een e-mail met daarin een link om te bevestigen de account voordat deze actief wordt.

>[AZURE.NOTE] U kunt andere Adobe EchoSign gebruiker account maken tools of API's die door Adobe EchoSign bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-adobe-echosign-perform-the-following-steps"></a>Gebruikers toewijzen aan EchoSign van Adobe, kunt u de volgende stappen uitvoeren:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Adobe EchoSign **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-adobe-echosign-tutorial/IC789526.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-adobe-echosign-tutorial/IC767830.png "Ja")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
