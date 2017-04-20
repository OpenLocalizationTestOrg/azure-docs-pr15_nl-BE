<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met kudo | Microsoft Azure" 
    description="Meer informatie over het Petje af met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-kudos"></a>Zelfstudie: Azure Active Directory-integratie met Petje af
  
Het doel van deze zelfstudie is weer te geven van de integratie van de Azure en Petje af.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder Petje af
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan kudo kan worden naar één teken in de toepassing van de site in Petje af bedrijf (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor kudo inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-kudos-tutorial/IC787799.png "Scenario")
##<a name="enabling-the-application-integration-for-kudos"></a>De integratie van toepassingen voor kudo inschakelen
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Petje af.

###<a name="to-enable-the-application-integration-for-kudos-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor kudo, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-kudos-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-kudos-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-kudos-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-kudos-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak**, **Petje af**.

    ![Galerie van toepassing] (./media/active-directory-saas-kudos-tutorial/IC787800.png "Galerie van toepassing")

7.  In het resultatendeelvenster **kudo**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Petje af] (./media/active-directory-saas-kudos-tutorial/IC787801.png "Petje af")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Petje af met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat-bestand te maken.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **kudo** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-kudos-tutorial/IC787802.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Petje af** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-kudos-tutorial/IC787803.png "Eenmalige aanmelding configureren")

3.  Typ de URL met het volgende patroon '*https://company.kudosnow.com*' op de pagina **Configureren App-URL** in het tekstvak **Kudo teken in de URL** en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-kudos-tutorial/IC787804.png "App-URL configureren")

4.  Klik op **certificaat downloaden**en sla het bestand op uw computer op de pagina **configureren eenmalige aanmelding in Petje af** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-kudos-tutorial/IC787805.png "Eenmalige aanmelding configureren")

5.  Log in op uw site kudo bedrijf als beheerder in een ander web browser-venster.

6.  In het menu aan de bovenkant, klikt u op **Instellingen**.

    ![Instellingen] (./media/active-directory-saas-kudos-tutorial/IC787806.png "Instellingen")

7.  Klik op **integraties \> SSO**.

8.  In de sectie met **eenmalige aanmelding** kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding] (./media/active-directory-saas-kudos-tutorial/IC787807.png "Eenmalige aanmelding")

    1.  De **Single Sign-On Service URL** -waarde kopiëren en vervolgens plakken in het tekstvak **URL aanmelden** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding in Petje af** .
    2.  Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.  

        >[AZURE.TIP]
        Zie voor meer informatie, [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    3.  De base-64 gecodeerde certificaat in Kladblok te openen, de inhoud ervan kopiëren naar het Klembord en plak deze vervolgens naar de textbox **x.509-certificaat**
    4.  Kopieer de **URL van de Service Single Sign-Out** waarde in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding in Petje af** en plak deze in de textbox **Logout-URL** .
    5.  Typ de naam van uw bedrijf in het tekstvak **URL van je Petje af** .
    6.  Klik op **Opslaan**.

9.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-kudos-tutorial/IC787808.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij kudo Azure AD, moeten zij worden ingericht in Petje af.  
Petje af is inrichten een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder uw bedrijf **kudo** site.

2.  In het menu aan de bovenkant, klikt u op **Instellingen**.

    ![Instellingen] (./media/active-directory-saas-kudos-tutorial/IC787806.png "Instellingen")

3.  Klik op de **Gebruikerbeheerder**.

4.  Klik op het tabblad **gebruikers** en klik vervolgens op **gebruiker toevoegen**.

    ![User Admin] (./media/active-directory-saas-kudos-tutorial/IC787809.png "User Admin")

5.  In de sectie **gebruiker toevoegen** kunt u de volgende stappen uitvoeren:

    ![Een gebruiker toevoegen] (./media/active-directory-saas-kudos-tutorial/IC787810.png "Een gebruiker toevoegen")

    1.  Typ de **Voornaam**, **Achternaam**, **e-mailadres** en andere details van een geldige Azure Active Directory-account verrichten in de verwante tekstvakken te.
    2.  Klik op **gebruiker maken**.

>[AZURE.NOTE]Kunt u een andere kudo gebruiker account hulpmiddelen voor het maken of API's geleverd door kudo bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-kudos-perform-the-following-steps"></a>Gebruikers toewijzen aan kudo, kunt u de volgende stappen uitvoeren:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **kudo **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-kudos-tutorial/IC787811.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-kudos-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.