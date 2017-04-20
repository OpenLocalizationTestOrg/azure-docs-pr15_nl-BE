<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met ArcGIS | Microsoft Azure" 
    description="Meer informatie over het ArcGIS met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-arcgis"></a>Zelfstudie: Azure Active Directory-integratie met ArcGIS

Het doel van deze zelfstudie is de integratie van de Azure en ArcGIS weergeven. Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een ArcGIS eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, is de Azure AD-gebruikers die u hebt toegewezen aan ArcGIS mogelijk voor één teken in de toepassing van de ArcGIS bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor ArcGIS inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-arcgis-tutorial/IC784735.png "Scenario")
##<a name="enabling-the-application-integration-for-arcgis"></a>De integratie van toepassingen voor ArcGIS inschakelen

Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor ArcGIS.

###<a name="to-enable-the-application-integration-for-arcgis-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor ArcGIS, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-arcgis-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-arcgis-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-arcgis-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-arcgis-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **ArcGIS**.

    ![Galerie Applcation] (./media/active-directory-saas-arcgis-tutorial/IC784736.png "Galerie Applcation")

7.  **ArcGIS**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![ArcGIS] (./media/active-directory-saas-arcgis-tutorial/IC784737.png "ArcGIS")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij ArcGIS met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **ArcGIS** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-arcgis-tutorial/IC784738.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**, en klik op **volgende**op de pagina **Hoe wilt u dat gebruikers aanmelden bij ArcGIS** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-arcgis-tutorial/IC784739.png "Eenmalige aanmelding configureren")

3.  Typ de URL die wordt gebruikt door de gebruikers aan te melden met het volgende patroon '*https://company.maps.arcgis.com*' en klik op **volgende**op de pagina **Configureren App-URL** in het tekstvak **ArcGIS teken In de URL** .

    ![App-URL configureren] (./media/active-directory-saas-arcgis-tutorial/IC784740.png "App-URL configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij ArcGIS** op **metagegevens downloaden**en sla het bestand lokaal op uw computer met metagegevens.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-arcgis-tutorial/IC784741.png "Eenmalige aanmelding configureren")

5.  Log in op uw site ArcGIS bedrijf als beheerder in een ander web browser-venster.

6.  Klik op **Instellingen bewerken**.

    ![Instellingen bewerken] (./media/active-directory-saas-arcgis-tutorial/IC784742.png "Instellingen bewerken")

7.  Klik op **beveiliging**.

    ![Beveiliging] (./media/active-directory-saas-arcgis-tutorial/IC784743.png "Beveiliging")

8.  Klik op **Id-Provider instellen**onder **Enterprise aanmeldingen**.

    ![Enterprise-aanmeldingen] (./media/active-directory-saas-arcgis-tutorial/IC784744.png "Enterprise-aanmeldingen")

9.  Op de configuratiepagina van **Identiteitsprovider instellen** , moet u de volgende stappen uitvoeren:

    ![Id-Provider instellen] (./media/active-directory-saas-arcgis-tutorial/IC784745.png "Id-Provider instellen")

    1.  Typ in het tekstvak Naam de naam van uw organisatie.
    2.  Voor de **dat metagegevens voor de identiteitsprovider Enterprise worden geleverd met behulp van** **Een bestand**te selecteren.
    3.  Uw gedownloade om metagegevensbestand te uploaden, klikt u op **bestand kiezen**.
    4.  Klik op **id-Provider**.

10. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-arcgis-tutorial/IC784746.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Om gebruikers aan te melden bij ArcGIS Azure AD, moeten zij worden ingericht in ArcGIS.  
ArcGIS is inrichten een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Log in op uw huurder **ArcGIS** .

2.  Klik op **leden uitnodigen**.

    ![Leden uitnodigen] (./media/active-directory-saas-arcgis-tutorial/IC784747.png "Leden uitnodigen")

3.  Selecteer **leden toevoegen zonder automatisch een e-mail verzenden**en klik vervolgens op **volgende**.

    ![Leden automatisch toevoegen] (./media/active-directory-saas-arcgis-tutorial/IC784748.png "Leden automatisch toevoegen")

4.  Op de pagina **leden** dialoogvenster voert u de volgende stappen uit:

    ![Toevoegen en bekijken] (./media/active-directory-saas-arcgis-tutorial/IC784749.png "Toevoegen en bekijken")

    1.  Voer de **Voornaam**, **Achternaam** en **e-mailadres** van een geldige AAD-account die u wilt om in te richten.
    2.  Klik op **toevoegen en bekijken**.

5.  Bekijk de gegevens die u hebt ingevoerd en klik vervolgens op **Leden toevoegen**.

    ![Lid toevoegen] (./media/active-directory-saas-arcgis-tutorial/IC784750.png "Lid toevoegen")

>[AZURE.NOTE] Kunt u een andere ArcGIS gebruiker account maken van hulpprogramma's of API's geleverd door ArcGIS bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-arcgis-perform-the-following-steps"></a>Gebruikers toewijzen aan ArcGIS, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **ArcGIS **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-arcgis-tutorial/IC784751.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-arcgis-tutorial/IC767830.png "Ja")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
