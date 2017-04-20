<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Mimecast Personal Portal | Microsoft Azure" 
    description="Meer informatie over het Mimecast persoonlijke Portal met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-mimecast-personal-portal"></a>Zelfstudie: Azure Active Directory-integratie met Mimecast Personal Portal
  
Het doel van deze zelfstudie is de integratie van de Azure en persoonlijke Portal Mimecast weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een persoonlijke Portal Mimecast eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Mimecast persoonlijke Portal kan worden met één teken in de toepassing aan uw persoonlijke Mimecast-Portal-site van het bedrijf (service gestart aanmelden op) of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor persoonlijke Portal Mimecast inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794991.png "Scenario")
##<a name="enabling-the-application-integration-for-mimecast-personal-portal"></a>De integratie van toepassingen voor persoonlijke Portal Mimecast inschakelen
  
Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor persoonlijke Portal Mimecast overzicht.

###<a name="to-enable-the-application-integration-for-mimecast-personal-portal-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor persoonlijke Portal Mimecast, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak**, **Persoonlijke Portal Mimecast**.

    ![Galerie van toepassing] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794992.png "Galerie van toepassing")

7.  **Persoonlijke Portal Mimecast**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Mimecast Personal Portal] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794993.png "Mimecast Personal Portal")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij persoonlijke Portal Mimecast met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat-bestand te maken.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, op de pagina **Mimecast persoonlijke Portal** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794994.png "Eenmalige aanmelding configureren")

2.  Op de pagina **Hoe wilt u dat gebruikers aanmelden bij persoonlijke Portal Mimecast** Selecteer **AD Azure Microsoft Single Sign-On**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794995.png "Eenmalige aanmelding configureren")

3.  Op de pagina **URL van App configureren** in de **Persoonlijke Portal-teken Mimecast op URL** textbox, typ de URL die wordt gebruikt door de gebruikers aan te melden op uw persoonlijke Portal Mimecast toepassing (bijvoorbeeld: "https://webmail-uk.mimecast.com" of "https://webmail-us.mimecast.com"), en klik op **volgende**.

    >[AZURE.NOTE] Het URL-teken is een specifieke regio.

    ![App-URL configureren] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794996.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op persoonlijke Portal Mimecast** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794997.png "Eenmalige aanmelding configureren")

5.  Log in op uw persoonlijke Portal Mimecast als beheerder in een ander web browser-venster.

6.  Ga naar **Services \> toepassing**.

    ![Toepassingen] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794998.png "Toepassingen")

7.  Klik op **verificatie profielen**.

    ![Verificatie-profielen] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794999.png "Verificatie-profielen")

8.  Klik op **nieuwe verificatie-profiel**.

    ![Nieuwe verificatie-profiel] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795000.png "Nieuwe verificatie-profiel")

9.  Voer de volgende stappen uit in de sectie **Verificatie-profiel** :

    ![Verificatie-profiel] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795001.png "Verificatie-profiel")

    1.  Typ in het tekstvak **Beschrijving** een naam voor uw configuratie.
    2.  Schakel **verificatie voor Mimecast Personal Portal SAML afdwingen**.
    3.  Selecteer als **Provider**, **Azure Active Directory**.
    4.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op persoonlijke Portal Mimecast** de **Uitgever URL** -waarde kopiëren en vervolgens plakken in het tekstvak **URL van de uitgevende instelling** .
    5.  De waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het tekstvak **Aanmeldings-URL** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op persoonlijke Portal Mimecast** .
    6.  De waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het tekstvak **URL Logout** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op persoonlijke Portal Mimecast** .  

        >[AZURE.NOTE] De aanmeldings-URL-waarde en de waarde van de URL Logout zijn voor het - op op persoonlijke Portal Mimecast hetzelfde.

    7.  Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.  

        >[AZURE.TIP]Zie [conversie van binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)voor meer informatie.

    8.  Open de base-64 gecodeerde certificaat in Kladblok de eerste regel verwijderen ("*--*") en de laatste regel ("*--*"), de resterende inhoud ervan kopiëren naar het Klembord en plak deze vervolgens naar de textbox **Identity Provider certificaat (metagegevens)** .
    9.  Selecteer **één teken op**.
    10. Klik op **Opslaan**.

10. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795002.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij de persoonlijke Portal Mimecast AD Azure, moeten zij worden ingericht in persoonlijke Portal Mimecast.  
Mimecast persoonlijke Portal is inrichten een handmatige taak.
  
U moet een domein registreren voordat u gebruikers kunt maken.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Meld u aan uw **Persoonlijke Portal Mimecast** aan als beheerder.

2.  Ga naar **mappen \> interne**.

    ![Mappen] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795003.png "Mappen")

3.  Klik op **Nieuw domein registreren**.

    ![Nieuw domein registreren] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795004.png "Nieuw domein registreren")

4.  Nadat u het nieuwe domein hebt gemaakt, klikt u op **Nieuw adres**.

    ![Nieuw adres] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795005.png "Nieuw adres")

5.  Voer de volgende stappen uit in het dialoogvenster Nieuw adres:

    ![Opslaan] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795006.png "Opslaan")

    1.  Typ de kenmerken van het **E-mailadres**, **Algemene naam**, **wachtwoord** en **Bevestig het wachtwoord** van een geldige AAD account te verrichten in de verwante tekstvakken.
    2.  Klik op **Opslaan**.

>[AZURE.NOTE]U kunt een andere persoonlijke Portal Mimecast gebruiker account maken van hulpprogramma's of API's die worden geleverd door persoonlijke Portal Mimecast bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-mimecast-personal-portal-perform-the-following-steps"></a>Gebruikers toewijzen aan persoonlijke Portal Mimecast, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Mimecast persoonlijke Portal **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795007.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.