<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met TeamSeer | Microsoft Azure" 
    description="Meer informatie over het TeamSeer met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-teamseer"></a>Zelfstudie: Azure Active Directory-integratie met TeamSeer
  
Het doel van deze zelfstudie is de integratie van de Azure en TeamSeer weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder TeamSeer
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan TeamSeer kan worden naar één teken in de toepassing van de TeamSeer bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor TeamSeer
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-teamseer-tutorial/IC789618.png "Scenario")

##<a name="enabling-the-application-integration-for-teamseer"></a>Inschakelen van de integratie van toepassingen voor TeamSeer
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor TeamSeer.

###<a name="to-enable-the-application-integration-for-teamseer-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor TeamSeer, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-teamseer-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-teamseer-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-teamseer-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-teamseer-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **TeamSeer**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-teamseer-tutorial/IC789619.png "Galerie van toepassing")

7.  **TeamSeer**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![TeamSeer] (./media/active-directory-saas-teamseer-tutorial/IC789620.png "TeamSeer")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij TeamSeer met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat-bestand te maken.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **TeamSeer** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-teamseer-tutorial/IC789621.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op TeamSeer** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-teamseer-tutorial/IC789628.png "Eenmalige aanmelding configureren")

3.  Typ de URL met het volgende patroon '*http://www.teamseer.com/companyid*' op de pagina **Configureren App-URL** in het tekstvak **TeamSeer teken In de URL** en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-teamseer-tutorial/IC789629.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op TeamSeer** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-teamseer-tutorial/IC789630.png "Eenmalige aanmelding configureren")

5.  Log in op uw site TeamSeer bedrijf als beheerder in een ander web browser-venster.

6.  Ga naar **de Admin HR**.

    ![HR-Admin] (./media/active-directory-saas-teamseer-tutorial/IC789634.png "HR-Admin")

7.  Klik op **Instellingen**.

    ![Setup] (./media/active-directory-saas-teamseer-tutorial/IC789635.png "Setup")

8.  Klik op **details van SAML-provider instellen**.

    ![SAML-instellingen] (./media/active-directory-saas-teamseer-tutorial/IC789636.png "SAML-instellingen")

9.  Voer de volgende stappen uit in de detailsectie van SAML-provider:

    ![SAML-instellingen] (./media/active-directory-saas-teamseer-tutorial/IC789637.png "SAML-instellingen")

    1.  In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding bij TeamSeer** de **Single Sign-On Service URL** -waarde kopiëren en vervolgens plakken in het tekstvak **URL** .
    2.  Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.  

        >[AZURE.TIP] Zie voor meer informatie, [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    3.  De base-64 gecodeerde certificaat in Kladblok te openen, de inhoud ervan kopiëren naar het Klembord en vervolgens plakken naar **Een certificaat met openbare IdP** textbox.

10. Het SAML-provider om configuratie te voltooien, voert u de volgende stappen uit:

    ![SAML-instellingen] (./media/active-directory-saas-teamseer-tutorial/IC789638.png "SAML-instellingen")

    1.  Typ het testgebruiker e-mailadres in de **E-mailadressen testen**.
    2.  Typ in het tekstvak **de uitgever van** de URL van de uitgevende instelling van de serviceprovider.
    3.  Klik op **Opslaan**.

11. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-teamseer-tutorial/IC789639.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij TeamSeer Azure AD, moeten zij worden ingericht in ShiftPlanning.  
Bij TeamSeer is het inrichten van een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw site **TeamSeer** bedrijf als beheerder.

2.  Voer de volgende stappen uit:

    ![HR-Admin] (./media/active-directory-saas-teamseer-tutorial/IC789640.png "HR-Admin")

    1.  Ga naar **Admin HR \> gebruikers**.
    2.  Klik op **de wizard nieuwe gebruiker**.

3.  In de sectie **Informatie over de gebruiker** , voert u de volgende stappen uit:

    ![Gebruikerdetails] (./media/active-directory-saas-teamseer-tutorial/IC789641.png "Gebruikerdetails")

    1.  Typ de **Voornaam**, **Achternaam**, **gebruikersnaam (e-mailadres)** van een geldige AAD account te verrichten in de verwante tekstvakken.
    2.  Klik op **volgende**.

4.  Volg de instructies op het scherm voor het toevoegen van een nieuwe gebruiker en klik op **Voltooien**.

>[AZURE.NOTE] Kunt u een andere TeamSeer gebruiker account maken van hulpprogramma's of API's die door TeamSeer AD Azure gebruikersaccounts inrichten.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-teamseer-perform-the-following-steps"></a>Gebruikers toewijzen aan TeamSeer, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **TeamSeer **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-teamseer-tutorial/IC789642.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-teamseer-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.