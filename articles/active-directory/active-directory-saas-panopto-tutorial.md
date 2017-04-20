<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Panopto | Microsoft Azure" 
    description="Meer informatie over het Panopto met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-panopto"></a>Zelfstudie: Azure Active Directory-integratie met Panopto
  
Het doel van deze zelfstudie is de integratie van de Azure en Panopto weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder Panopto
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Panopto kan worden naar één teken in de toepassing van de Panopto bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Panopto
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-panopto-tutorial/IC777665.png "Scenario")
##<a name="enabling-the-application-integration-for-panopto"></a>Inschakelen van de integratie van toepassingen voor Panopto
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Panopto.

###<a name="to-enable-the-application-integration-for-panopto-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Panopto, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-panopto-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-panopto-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-panopto-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-panopto-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **Panopto**in het **zoekvak**.

    ![Galerie Appkication] (./media/active-directory-saas-panopto-tutorial/IC777666.png "Galerie Appkication")

7.  **Panopto**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Panopto] (./media/active-directory-saas-panopto-tutorial/IC782936.png "Panopto")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Panopto met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat-bestand te maken.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Panopto** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-panopto-tutorial/IC777667.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Panopto** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-panopto-tutorial/IC777668.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App URL configureren** , in het tekstvak **Panopto teken In de URL** de URL van uw volgende patroon met ' https://*\<-naam van de huurder\>. Panopto.com*", en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-panopto-tutorial/IC777528.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op Panopto** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-panopto-tutorial/IC777669.png "Eenmalige aanmelding configureren")

5.  Log in op uw site Panopto bedrijf als beheerder in een ander web browser-venster.

6.  Op de werkbalk aan de linkerkant, klik op **systeem**en klik op **Id-Providers**.

    ![Systeem] (./media/active-directory-saas-panopto-tutorial/IC777670.png "Systeem")

7.  Klik op de **Provider toevoegen**.

    ![Id-Providers] (./media/active-directory-saas-panopto-tutorial/IC777671.png "Id-Providers")

8.  In de sectie SAML-provider, moet u de volgende stappen uitvoeren:

    ![SaaS-configuratie] (./media/active-directory-saas-panopto-tutorial/IC777672.png "SaaS-configuratie")

    1.  Selecteer in de lijst **Type Provider** **SAML20**
    2.  Typ in het tekstvak **Naam** een naam voor het exemplaar.
    3.  Typ een beschrijving in het tekstvak **Beschrijving** .
    4.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Panopto** de **Uitgever URL** -waarde kopiëren en vervolgens plakken in het tekstvak van de **uitgevende instelling** .
    5.  De waarde van **SAML SSO-URL** kopiëren en vervolgens plakken in het tekstvak **Url van Stuiteren** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op Panopto** .
    6.  Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.  

        >[AZURE.TIP] Zie voor meer informatie, [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    7.  De base-64 gecodeerde certificaat in Kladblok te openen, de inhoud ervan kopiëren naar het Klembord en plak deze vervolgens naar de **Openbare sleutel** textbox
    8.  Klik op **Opslaan**.
        ![Opslaan] (./media/active-directory-saas-panopto-tutorial/IC777673.png "Opslaan")

9.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-panopto-tutorial/IC777674.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Er is geen actie-item aan de Panopto van gebruikers configureren.  
Wanneer een toegewezen gebruiker probeert aan te melden bij de Panopto met het deelvenster toegang, Panopto wordt gecontroleerd of de gebruiker bestaat.  
Als er nog geen gebruikersaccount beschikbaar, wordt deze automatisch gemaakt door de Panopto.

>[AZURE.NOTE]Kunt u een andere Panopto gebruiker account maken van hulpprogramma's of API's die door Panopto AD Azure gebruikersaccounts inrichten.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-panopto-perform-the-following-steps"></a>Gebruikers toewijzen aan Panopto, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Panopto **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-panopto-tutorial/IC777675.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-panopto-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.