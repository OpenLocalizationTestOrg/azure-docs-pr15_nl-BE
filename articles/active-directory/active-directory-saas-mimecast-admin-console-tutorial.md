<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Mimecast-beheerconsole | Microsoft Azure" 
    description="Meer informatie over het Mimecast-beheerconsole met Azure Active Directory gebruiken om te schakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a>Zelfstudie: Azure Active Directory-integratie met Mimecast-beheerconsole
  
Het doel van deze zelfstudie is de integratie van de Azure en Mimecast-beheerconsole weergegeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een Mimecast-beheerconsole eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Mimecast-beheerconsole kan worden met één teken in de toepassing van de Mimecast-beheerconsole bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor Mimecast Admin Console inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795008.png "Scenario")
##<a name="enabling-the-application-integration-for-mimecast-admin-console"></a>De integratie van toepassingen voor Mimecast Admin Console inschakelen
  
Het doel van deze sectie is een overzicht van de integratie van toepassingen voor Mimecast Admin Console inschakelen.

###<a name="to-enable-the-application-integration-for-mimecast-admin-console-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Mimecast Admin Console, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **Mimecast-beheerconsole**.

    ![Galerie van toepassing] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795009.png "Galerie van toepassing")

7.  In het resultatendeelvenster **Mimecast-beheerconsole**te selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Mimecast-beheerconsole] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795010.png "Mimecast-beheerconsole")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Mimecast-beheerconsole met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat-bestand te maken.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, op de pagina **Mimecast Admin Console** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795011.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Mimecast-beheerconsole** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795012.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **URL van App configureren** , in het tekstvak **Mimecast Admin Console aanmelden op URL** de URL die wordt gebruikt door de gebruikers aan te melden op uw toepassing Mimecast-beheerconsole (bijvoorbeeld: "https://webmail-uk.mimecast.com" of "https://webmail-us.mimecast.com"), en klik op **volgende**.

    >[AZURE.NOTE] Het URL-teken is een specifieke regio.

    ![App-URL configureren] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795013.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op de beheerconsole van Mimecast** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795014.png "Eenmalige aanmelding configureren")

5.  Log in op uw Mimecast-beheerconsole als beheerder in een ander web browser-venster.

6.  Ga naar **Services \> toepassing**.

    ![Services] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC794998.png "Services")

7.  Klik op **verificatie profielen**.

    ![Verificatie-profielen] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC794999.png "Verificatie-profielen")

8.  Klik op **nieuwe verificatie-profiel**.

    ![Nieuwe profielen voor verificatie] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795000.png "Nieuwe profielen voor verificatie")

9.  Voer de volgende stappen uit in de sectie **Verificatie-profiel** :

    ![Verificatie-profiel] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795015.png "Verificatie-profiel")

    1.  Typ in het tekstvak **Beschrijving** een naam voor uw configuratie.
    2.  Selecteer de **SAML-verificatie voor Mimecast-beheerconsole afdwingen**.
    3.  Selecteer als **Provider**, **Azure Active Directory**.
    4.  Kopieer de waarde van de **URL van de uitgevende instelling** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Mimecast Admin Console** en vervolgens plakken in het tekstvak **URL van de uitgevende instelling** .
    5.  De waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het tekstvak **Aanmeldings-URL** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Mimecast-beheerconsole** .
    6.  De waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het tekstvak **URL Logout** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Mimecast-beheerconsole** .  

        >[AZURE.NOTE]De aanmeldings-URL-waarde en de Logout URL-waarde zijn voor de Mimecast-beheerconsole hetzelfde.

    7.  Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.  

        >[AZURE.TIP]Zie [conversie van binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)voor meer informatie.

    8.  Open de base-64 gecodeerde certificaat in Kladblok de eerste regel verwijderen ("*--*") en de laatste regel ("*--*"), de resterende inhoud ervan kopiëren naar het Klembord en plak deze vervolgens naar de textbox **Identity Provider certificaat (metagegevens)** .
    9.  Selecteer **één teken op**.
    10. Klik op **Opslaan**.

10. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795016.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij Mimecast-beheerconsole Azure AD, moeten zij worden ingericht in Mimecast-beheerconsole.  
Mimecast-beheerconsole is inrichten een handmatige taak.
  
U moet een domein registreren voordat u gebruikers kunt maken.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Meld u aan uw **Mimecast Admin Console** aan als beheerder.

2.  Ga naar **mappen \> interne**.

    ![Mappen] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795003.png "Mappen")

3.  Klik op **Nieuw domein registreren**.

    ![Nieuw domein registreren] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795004.png "Nieuw domein registreren")

4.  Nadat u het nieuwe domein hebt gemaakt, klikt u op **Nieuw adres**.

    ![Nieuw adres] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795005.png "Nieuw adres")

5.  Voer de volgende stappen uit in het dialoogvenster Nieuw adres:

    ![Opslaan] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795006.png "Opslaan")

    1.  Typ de kenmerken van het **E-mailadres**, **Algemene naam**, **wachtwoord** en **Bevestig het wachtwoord** van een geldige AAD account te verrichten in de verwante tekstvakken.
    2.  Klik op **Opslaan**.

>[AZURE.NOTE]U kunt een andere beheerconsole Mimecast gebruiker account maken van hulpprogramma's of API's die worden geleverd door Mimecast-beheerconsole bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-mimecast-admin-console-perform-the-following-steps"></a>Gebruikers toewijzen aan Mimecast Admin Console, kunt u de volgende stappen uitvoeren:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Mimecast Admin Console **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795017.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.