<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met SAP HANA Cloud Platform | Microsoft Azure" 
    description="Meer informatie over het Cloud Platform van SAP HANA met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform"></a>Zelfstudie: Azure Active Directory-integratie met SAP HANA Cloud Platform
  
Het doel van deze zelfstudie is de integratie van de Azure Cloud Platform van SAP HANA weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een account voor SAP HANA Cloud Platform
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan SAP HANA Cloud Platform kan worden met één teken in de toepassing met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).

>[AZURE.IMPORTANT]U moet uw eigen toepassing implementeren of zich abonneren op een toepassing op uw account SAP HANA Cloud Platform voor het testen van één teken op. In deze zelfstudie wordt in de account een toepassing geïmplementeerd.
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor SAP HANA Cloud Platform
2.  Eenmalige aanmelding configureren
3.  Een rol toewijzen aan een gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "Scenario")
##<a name="enabling-the-application-integration-for-sap-hana-cloud-platform"></a>Inschakelen van de integratie van toepassingen voor SAP HANA Cloud Platform
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor SAP HANA Cloud Platform.

###<a name="to-enable-the-application-integration-for-sap-hana-cloud-platform-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor SAP HANA Cloud Platform, moet u de volgende stappen uitvoeren:

1.  Klik in de Portal Azure Management in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak**, **SAP HANA Cloud Platform**.

    ![Galerie van toepassing] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Galerie van toepassing")

7.  In het resultatendeelvenster **SAP HANA Cloud Platform**te selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![SAP Hana] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij SAP HANA Cloud Platform met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat uploaden naar de Cloud Platform van SAP HANA huurder.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, op de pagina **Cloud Platform van SAP HANA** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij SAP HANA Cloud Platform** Selecteer **AD Azure Microsoft Single Sign-On**en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "Eenmalige aanmelding configureren")

3.  In een ander web browser-venster moet u zich aanmelden bij de Cockpit SAP HANA Cloud Platform op https://account. \<host liggend\>.ondemand.com/cockpit (bv.: *https://account.hanatrial.ondemand.com/cockpit*).

4.  Klik op het tabblad **vertrouwensrelaties** .

    ![Vertrouwen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "Vertrouwen")

5.  In de beheersectie van vertrouwensrelaties kunt u de volgende stappen uitvoeren:

    ![Metagegevens ophalen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "Metagegevens ophalen")

    1.  Klik op het tabblad **Lokale Service Provider** .
    2.  Het Cloud Platform van SAP HANA metagegevens om bestand te downloaden, klikt u op **Metagegevens ophalen**.

6.  Voer de volgende stappen uit in de Azure Active klassieke portal op de pagina **URL van App configureren** en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "App-URL configureren")

    1.  Typ de URL die wordt gebruikt door de gebruikers wilt aanmelden bij de toepassing van **SAP HANA Cloud Platform** in het tekstvak **Teken op URL** . Dit is de URL van de specifieke account van een beveiligde bron in uw toepassing SAP HANA Cloud Platform. De URL is gebaseerd op het volgende patroon: *https://\<applicationName\>\<accountnaam\>.\< Liggend host\>.ondemand.com/\<pad\_te\_beveiligd\_resource\> * (bv.: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)

        >[AZURE.NOTE]Dit is de URL in uw toepassing van SAP HANA Cloud Platform waarmee de gebruiker te verifiëren.

    2.  Open het gedownloade bestand met metagegevens SAP HANA Cloud Platform, en Ga naar de tag **ns3:AssertionConsumerService** .
    3.  De waarde van het kenmerk **Location** Kopieer en plak deze in het tekstvak **URL van SAP HANA Cloud Platform antwoord** .

7.  Op de pagina **configureren eenmalige aanmelding bij SAP HANA Cloud Platform** voor het downloaden van de metagegevens op **metagegevens downloaden**en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "Eenmalige aanmelding configureren")

8.  In de Cockpit van SAP HANA Cloud Platform in de sectie **Lokale serviceprovider** de volgende stappen uitvoeren:

    ![Vertrouwensrelaties beheren] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "Vertrouwensrelaties beheren")

    1.  Klik op **bewerken**.
    2.  Selecteer **aangepast**als **Type configuratie**.
    3.  Als de **Lokale naam van de Provider**, accepteer de standaardwaarde.
    4.  Klik op **Sleutelpaar genereren**voor het genereren van een **Sleutel voor ondertekening** en het sleutelpaar van een **Certificaat voor ondertekening** .
    5.  Selecteer **uitgeschakeld**als **Het doorgeven van de Principal**.
    6.  Selecteer **uitgeschakeld**als **Verificatie van kracht**.
    7.  Klik op **Opslaan**.

9.  Klik op het tabblad **Vertrouwde id-Provider** en klik vervolgens op **Vertrouwde identiteitsprovider toevoegen**.

    ![Vertrouwensrelaties beheren] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "Vertrouwensrelaties beheren")

    >[AZURE.NOTE]Om de lijst met vertrouwde id-providers beheren, moet u de aangepaste configuratietype hebt gekozen in de sectie lokale serviceprovider. Voor standaard configuratie hebt u een niet-bewerkbare en impliciete vertrouwensrelatie met de SAP-ID Service. Voor geen hoeft u niet alle vertrouwensinstellingen.

10. Klik op het tabblad **Algemeen** en klik vervolgens op **Bladeren** om de van het gedownloade metagegevensbestand te uploaden.

    ![Vertrouwensrelaties beheren] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "Vertrouwensrelaties beheren")

    >[AZURE.NOTE] Na het uploaden van het bestand met metagegevens, worden de waarden voor de **URL van Single Sign-on**, **Één Logout URL** en **Certificaat ondertekenen** automatisch ingevuld.

11. Klik op het tabblad **kenmerken** .

12. Voer de volgende stappen uit op het tabblad **kenmerken** :

    ![Kenmerken] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "Kenmerken")

    1.  Via **Add Assertion-Based-kenmerk**toevoegen de volgende kenmerken op basis van bevestiging:

        |Bevestiging, kenmerk| Kenmerk opdrachtgever|
        |-------------------|--------------------|
        |http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/givenName|   Voornaam|--------------------|--------------------|
        |http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/surname|        Achternaam|-----------|
        |http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/EmailAddress|E-mail|

    >[AZURE.NOTE]De configuratie van de kenmerken is afhankelijk van hoe de toepassing(en) van HCP worden ontwikkeld, dat wil zeggen welke kenmerken ze verwachten in de SAML-reactie en onder welke naam (Principal kenmerk) toegang tot dit kenmerk in de code.
    >  
    >een.  Het **Standaard kenmerk** in het screenshot is alleen ter illustratie. Het scenario werken is niet verplicht.  
    >
    >b.  De namen en waarden voor **Principal kenmerk** weergegeven in de schermafbeelding is afhankelijk van de manier waarop de toepassing wordt ontwikkeld. Het is mogelijk dat verschillende toewijzingen voor uw toepassing vereist is.

13. In de Azure klassieke portal op de dialoog pagina **configureren eenmalige aanmelding bij SAP HANA Cloud Platform** selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **Voltooien**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "Eenmalige aanmelding configureren")
  
Als een optionele stap kunt u groepen op basis van een verklaring voor de identiteitsprovider Azure Active Directory configureren

>[AZURE.NOTE]Groepen kunt op SAP HANA Cloud-Platform u een of meer gebruikers dynamisch toewijzen aan een of meer rollen in toepassingen van SAP HANA Cloud Platform, bepaald door waarden van kenmerken in de bewering van SAML 2.0. Bijvoorbeeld, als de bevestiging bevat het kenmerk '*contract = tijdelijke*", kunt u alle betrokken gebruikers worden toegevoegd aan de groep '*tijdelijk*'. De groep "*tijdelijk*" bevat mogelijk een of meer rollen uit een of meer toepassingen die worden gebruikt in uw account SAP HANA Cloud Platform.
>  
>Groepen op basis van een verklaring te gebruiken als u veel gebruikers een of meer rollen van toepassingen in uw account Cloud Platform van SAP HANA massa toewijzen. Als u alleen een enkele of een klein aantal gebruikers toewijzen aan (een) specifieke rol(len) aangeraden wijzen ze rechtstreeks in het tabblad "**vergunningen**" van de cockpit SAP HANA Cloud Platform.

##<a name="assigning-a-role-to-a-user"></a>Een rol toewijzen aan een gebruiker
  
Om gebruikers aan te melden bij SAP HANA Cloud Platform Azure AD, moet u de rollen in het SAP HANA Cloud Platform aan hen toewijzen.

###<a name="to-assign-a-role-to-a-user-perform-the-following-steps"></a>Als u een rol aan een gebruiker toewijst, kunt u de volgende stappen uitvoeren:

1.  Log in op uw **Cloud Platform van SAP HANA** cockpit.

2.  Voer de volgende stappen uit:

    ![Vergunningen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "Vergunningen")

    1.  Klik op **verificatie**.
    2.  Klik op het tabblad **gebruikers** .
    3.  Typ in het tekstvak **gebruiker** e-mailadres van de gebruiker.
    4.  Klik op **toewijzen** om de gebruiker aan een rol toewijzen.
    5.  Klik op **Opslaan**.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-sap-hana-cloud-platform-perform-the-following-steps"></a>Gebruikers toewijzen aan SAP HANA Cloud Platform, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Cloud Platform van SAP HANA** application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.