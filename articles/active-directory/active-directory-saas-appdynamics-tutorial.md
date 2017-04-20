<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met AppDynamics | Microsoft Azure" 
    description="Meer informatie over het AppDynamics met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-appdynamics"></a>Zelfstudie: Azure Active Directory-integratie met AppDynamics

Het doel van deze zelfstudie is de integratie van de Azure en AppDynamics weergeven. Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een AppDynamics eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan AppDynamics kan worden naar één teken in de toepassing van de AppDynamics bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor AppDynamics
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-appdynamics-tutorial/IC790209.png "Scenario")
##<a name="enabling-the-application-integration-for-appdynamics"></a>Inschakelen van de integratie van toepassingen voor AppDynamics

Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor AppDynamics.

###<a name="to-enable-the-application-integration-for-appdynamics-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor AppDynamics, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-appdynamics-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-appdynamics-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-appdynamics-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-appdynamics-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **AppDynamics**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-appdynamics-tutorial/IC790210.png "Galerie van toepassing")

7.  **AppDynamics**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![AppDynamics] (./media/active-directory-saas-appdynamics-tutorial/IC790211.png "AppDynamics")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij AppDynamics met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat-bestand te maken.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **AppDynamics** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Enkele aanmelding configureren] (./media/active-directory-saas-appdynamics-tutorial/IC790212.png "Enkele aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op AppDynamics** en klik op **volgende**.

    ![Enkele aanmelding configureren] (./media/active-directory-saas-appdynamics-tutorial/IC790213.png "Enkele aanmelding configureren")

3.  Typ de URL die wordt gebruikt door uw gebruikers om aanmelding bij AppDynamics ("*https://companyname.saas.appdynamics.com*") op de pagina **Configureren App-URL** in het tekstvak **AppDynamics aanmelden op de URL** en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-appdynamics-tutorial/IC790214.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op AppDynamics** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand op uw computer.

    ![Enkele aanmelding configureren] (./media/active-directory-saas-appdynamics-tutorial/IC790215.png "Enkele aanmelding configureren")

5.  Log in op uw site AppDynamics bedrijf als beheerder in een ander web browser-venster.

6.  In de werkbalk op de bovenkant, klikt u op **Instellingen**en klik vervolgens op **beheer**.

    ![Beheer] (./media/active-directory-saas-appdynamics-tutorial/IC790216.png "Beheer")

7.  Klik op het tabblad **Verificatie** .

    ![Verificatieprovider] (./media/active-directory-saas-appdynamics-tutorial/IC790224.png "Verificatieprovider")

8.  Voer de volgende stappen uit in de sectie **Verificatie** :

    ![SAML-configuratie] (./media/active-directory-saas-appdynamics-tutorial/IC790225.png "SAML-configuratie")

    1.  Selecteer **SAML**als **Verificatieprovider**.
    2.  De waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het tekstvak **Aanmeldings-URL** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op AppDynamics** .
    3.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij AppDynamics** de waarde van de **Externe Logout URL** kopiëren en vervolgens plakken in het tekstvak **URL Logout** .
    4.  Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.  

        >[AZURE.TIP] Zie voor meer informatie, [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    5.  De base-64 gecodeerde certificaat openen in Kladblok en plak deze vervolgens naar het tekstvak van het **certificaat** de inhoud ervan kopiëren naar het Klembord
    6.  Klik op **Opslaan**.
        ![Opslaan] (./media/active-directory-saas-appdynamics-tutorial/IC777673.png "Opslaan")

9.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Enkele aanmelding configureren] (./media/active-directory-saas-appdynamics-tutorial/IC790226.png "Enkele aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Om gebruikers aan te melden bij AppDynamics Azure AD, moeten zij worden ingericht in AppDynamics.  
Bij AppDynamics is het inrichten van een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Log in op uw site AppDynamics bedrijf als beheerder.

2.  Ga naar **gebruikers**en klik vervolgens op **+** voor het openen van het dialoogvenster voor de **Gebruiker maken** .

    ![Gebruikers] (./media/active-directory-saas-appdynamics-tutorial/IC790229.png "Gebruikers")

3.  Voer de volgende stappen uit in de sectie **Gebruiker maken** :

    ![Gebruiker maken] (./media/active-directory-saas-appdynamics-tutorial/IC790230.png "Gebruiker maken")

    1.  Typ de **gebruikersnaam**, **naam**, **e-mailadres**, **Wachtwoord**, **Nieuw wachtwoord herhalen** van een geldige AAD account te verrichten in de verwante tekstvakken.
    2.  Klik op **Opslaan**.

>[AZURE.NOTE] Kunt u een andere AppDynamics gebruiker account maken van hulpprogramma's of API's die door AppDynamics AD Azure gebruikersaccounts inrichten.

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-appdynamics-perform-the-following-steps"></a>Gebruikers toewijzen aan AppDynamics, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **AppDynamics **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-appdynamics-tutorial/IC790231.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-appdynamics-tutorial/IC767830.png "Ja")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
