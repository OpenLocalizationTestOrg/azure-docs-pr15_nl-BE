<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Intacct | Microsoft Azure" 
    description="Meer informatie over het Intacct met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-intacct"></a>Zelfstudie: Azure Active Directory-integratie met Intacct
  
Het doel van deze zelfstudie is de integratie van de Azure en Intacct weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder Intacct
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Intacct kan worden naar één teken in de toepassing van de Intacct bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Intacct
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-intacct-tutorial/IC790030.png "Scenario")
##<a name="enabling-the-application-integration-for-intacct"></a>Inschakelen van de integratie van toepassingen voor Intacct
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Intacct.

###<a name="to-enable-the-application-integration-for-intacct-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Intacct, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-intacct-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-intacct-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-intacct-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-intacct-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **Intacct**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-intacct-tutorial/IC790031.png "Galerie van toepassing")

7.  **Intacct**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Intacct] (./media/active-directory-saas-intacct-tutorial/IC790032.png "Intacct")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Intacct met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat-bestand te maken.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Intacct** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-intacct-tutorial/IC790033.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Intacct** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-intacct-tutorial/IC790034.png "Eenmalige aanmelding configureren")

3.  Typ de URL met het volgende patroon '*https://Intacct.com/company*' op de pagina **Configureren App-URL** in het tekstvak **Intacct aanmelden op de URL** en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-intacct-tutorial/IC790035.png "App-URL configureren")

4.  Klik op **certificaat downloaden**en sla het bestand op uw computer op de pagina **configureren eenmalige aanmelding op Intacct** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-intacct-tutorial/IC790036.png "Eenmalige aanmelding configureren")

5.  Log in op uw site Intacct bedrijf als beheerder in een ander web browser-venster.

6.  Klik op het tabblad van het **bedrijf** en klikt u vervolgens op **Bedrijfsgegevens**.

    ![Bedrijf] (./media/active-directory-saas-intacct-tutorial/IC790037.png "Bedrijf")

7.  Klik op het tabblad **beveiliging** en klik vervolgens op **bewerken**.

    ![Beveiliging] (./media/active-directory-saas-intacct-tutorial/IC790038.png "Beveiliging")

8.  In de sectie voor **eenmalige aanmelding (SSO)** , voert u de volgende stappen uit:

    ![Eenmalige aanmelding] (./media/active-directory-saas-intacct-tutorial/IC790039.png "Eenmalige aanmelding")

    1.  Selecteer **eenmalige op inschakelen**.
    2.  Selecteer als **identiteit providertype**, **SAML 2.0**.
    3.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Intacct** de **Uitgever URL** -waarde kopiëren en vervolgens plakken in het tekstvak **URL van de uitgevende instelling** .
    4.  De waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het tekstvak **Aanmeldings-URL** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op Intacct** .
    5.  Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.
        
        >[AZURE.TIP]Zie voor meer informatie, [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    6.  De base-64 gecodeerde certificaat openen in Kladblok en plak deze vervolgens naar het tekstvak van het **certificaat** de inhoud ervan kopiëren naar het Klembord
    7.  Klik op **Opslaan**.

9.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-intacct-tutorial/IC790040.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij Intacct Azure AD, moeten zij worden ingericht in Intacct.  
Bij Intacct is het inrichten van een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw huurder **Intacct** .

2.  Klik op het tabblad van het **bedrijf** en klik vervolgens op **gebruikers**.

    ![Gebruikers] (./media/active-directory-saas-intacct-tutorial/IC790041.png "Gebruikers")

3.  Klik op het tabblad **toevoegen**

    ![Toevoegen] (./media/active-directory-saas-intacct-tutorial/IC790042.png "Toevoegen")

4.  Voer de volgende stappen uit in de sectie **Gebruikersgegevens** :

    ![Gebruikersgegevens] (./media/active-directory-saas-intacct-tutorial/IC790043.png "Gebruikersgegevens")

    1.  Typ de **Gebruikers-ID**, de **Achternaam**, **Voornaam**, het **e-mailadres**, de **titel** en de **Telefoon** van een bepaling in de verwante tekstvakken de gewenste Azure AD-account.
    2.  Selecteer de **Admin bevoegdheden** van een Azure AD-account die u verrichten wilt.
    3.  Klik op **Opslaan**.
        
        >[AZURE.NOTE] De accounthouder AAD ontvangt een e-mail en een link om te bevestigen hun account voordat deze actief wordt als volgt.

>[AZURE.NOTE] Kunt u een andere Intacct gebruiker account maken van hulpprogramma's of API's geleverd door Intacct bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-intacct-perform-the-following-steps"></a>Gebruikers toewijzen aan Intacct, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Intacct **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-intacct-tutorial/IC790044.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-intacct-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.