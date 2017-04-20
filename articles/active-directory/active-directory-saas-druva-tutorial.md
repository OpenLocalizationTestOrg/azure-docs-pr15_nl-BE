<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie integratie met Druva | Microsoft Azure" 
    description="Meer informatie over het Druva met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-integration-with-druva"></a>Zelfstudie: Azure Active Directory-integratie integratie met Druva

Het doel van deze zelfstudie is de integratie van de Azure en Druva weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een Druva eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Druva kan worden naar één teken in de toepassing van de Druva bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Druva
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-druva-tutorial/IC795084.png "Scenario")
##<a name="enabling-the-application-integration-for-druva"></a>Inschakelen van de integratie van toepassingen voor Druva

Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Druva.

###<a name="to-enable-the-application-integration-for-druva-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Druva, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-druva-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-druva-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-druva-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-druva-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **Druva**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-druva-tutorial/IC795085.png "Galerie van toepassing")

7.  **Druva**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Druva] (./media/active-directory-saas-druva-tutorial/IC795086.png "Druva")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Druva met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat-bestand te maken.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

Uw toepassing Druva verwacht de SAML-bevestigingen in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de **saml-token kenmerken** .  
De volgende schermafbeelding ziet u een voorbeeld voor dit.

![SAML-Token kenmerken] (./media/active-directory-saas-druva-tutorial/IC795087.png "SAML-Token kenmerken")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Druva** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-druva-tutorial/IC795027.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Druva** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-druva-tutorial/IC795088.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **URL van App configureren** , in het tekstvak **Druva teken op URL** de URL die wordt gebruikt door de gebruikers aan te melden op uw toepassing Druva (bijvoorbeeld: "*https://cloud.druva.com/home/*"), en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-druva-tutorial/IC795089.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op Druva** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-druva-tutorial/IC795090.png "Eenmalige aanmelding configureren")

5.  Log in op uw site Druva bedrijf als beheerder in een ander web browser-venster.

6.  Ga naar **beheren \> instellingen**.

    ![Instellingen] (./media/active-directory-saas-druva-tutorial/IC795091.png "Instellingen")

7.  Voer de volgende stappen uit in het dialoogvenster Instellingen voor eenmalige aanmelding:

    ![Instellingen voor eenmalige aanmelding kel] (./media/active-directory-saas-druva-tutorial/IC795092.png "Instellingen voor eenmalige aanmelding kel")

    1.  De waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het tekstvak **ID Provider aanmeldings-URL** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op Druva** .
    2.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Druva** de waarde van de **Externe Logout URL** kopiëren en vervolgens plakken in het tekstvak **ID Provider Logout URL** .
    3.  Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.  

        >[AZURE.TIP] Zie voor meer informatie, [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    4.  De base-64 gecodeerde certificaat in Kladblok te openen, de inhoud ervan kopiëren naar het Klembord en vervolgens plakken in het tekstvak **ID Provider certificaat**
    5.  De pagina **-Instellingen** , klikt u op **Opslaan**.

8.  Klik op de pagina **-Instellingen** op **SSO-Token genereren**.

    ![Instellingen] (./media/active-directory-saas-druva-tutorial/IC795093.png "Instellingen")

9.  In het dialoogvenster **Single Sign-on verificatie Token** , kunt u de volgende stappen uitvoeren:

    ![SSO-Token] (./media/active-directory-saas-druva-tutorial/IC795094.png "SSO-Token")

    1.  Klik op **kopiëren**.
    2.  Klik op **sluiten**.

10. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-druva-tutorial/IC795095.png "Eenmalige aanmelding configureren")

11. In het menu aan de bovenkant, klikt u op **kenmerken** om de **SAML-Token kenmerken** -dialoogvenster te openen.

    ![Kenmerken] (./media/active-directory-saas-druva-tutorial/IC795096.png "Kenmerken")

12. Als u wilt de toewijzingen vereist kenmerk toevoegen, moet u de volgende stappen uitvoeren:

  	|Naam van kenmerk|Waarde van het kenmerk|
  	|---|---|
  	|synchroon\_auth\_token|<*Klembord-waarde*>|

    1.  Voor elke rij met gegevens in de bovenstaande tabel, klikt u op **gebruikerskenmerk toevoegen**.
    2.  Typ de naam van het kenmerk voor de rij weergegeven in het tekstvak **Naam van kenmerk** .
    3.  Typ in het tekstvak **Waarde van het kenmerk** de kenmerkwaarde voor die rij weergegeven.
    4.  Klik op **Voltooien**.

13. Klik op **wijzigingen toepassen**.
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Om gebruikers aan te melden bij Druva Azure AD, moeten zij worden ingericht in Druva.  
Bij Druva is het inrichten van een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder uw bedrijf **Druva** site.

2.  Ga naar **beheren \> gebruikers**.

    ![Gebruikers beheren] (./media/active-directory-saas-druva-tutorial/IC795097.png "Gebruikers beheren")

3.  Klik op **Nieuw maken**.

    ![Gebruikers beheren] (./media/active-directory-saas-druva-tutorial/IC795098.png "Gebruikers beheren")

4.  Voer de volgende stappen uit in het dialoogvenster Nieuwe gebruiker maken:

    ![Nieuwegebruiker maken] (./media/active-directory-saas-druva-tutorial/IC795099.png "Nieuwegebruiker maken")

    1.  Typ het e-mailadres en de naam van een geldige Azure Active Directory-gebruikersaccount die u wilt inrichten in de verwante tekstvakken.
    2.  Klik op **gebruiker maken**.

>[AZURE.NOTE] Kunt u een andere Druva gebruiker account maken van hulpprogramma's of API's geleverd door Druva bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-druva-perform-the-following-steps"></a>Gebruikers toewijzen aan Druva, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Druva **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-druva-tutorial/IC795100.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-druva-tutorial/IC767830.png "Ja")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
