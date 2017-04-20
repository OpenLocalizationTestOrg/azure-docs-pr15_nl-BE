<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Projectplace | Microsoft Azure" 
    description="Meer informatie over het Projectplace met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-projectplace"></a>Zelfstudie: Azure Active Directory-integratie met Projectplace
  
Het doel van deze zelfstudie is de integratie van de Azure en Projectplace weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een Projectplace eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Projectplace kan worden naar één teken in de toepassing van de Projectplace bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Projectplace
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-projectplace-tutorial/IC790217.png "Scenario")
##<a name="enabling-the-application-integration-for-projectplace"></a>Inschakelen van de integratie van toepassingen voor Projectplace
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Projectplace.

###<a name="to-enable-the-application-integration-for-projectplace-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Projectplace, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-projectplace-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-projectplace-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-projectplace-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-projectplace-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **Projectplace**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-projectplace-tutorial/IC790218.png "Galerie van toepassing")

7.  **Projectplace**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![ProjectPlace] (./media/active-directory-saas-projectplace-tutorial/IC790219.png "ProjectPlace")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Projectplace met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Projectplace** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Enkele aanmelding configureren] (./media/active-directory-saas-projectplace-tutorial/IC790220.png "Enkele aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Projectplace** en klik op **volgende**.

    ![Enkele aanmelding configureren] (./media/active-directory-saas-projectplace-tutorial/IC790221.png "Enkele aanmelding configureren")

3.  Typ op de pagina **App URL configureren** , in het tekstvak **Projectplace teken op URL** de URL van uw ProjectPlace huurder (bijvoorbeeld: "*http://company.projectplace.com*"), en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-projectplace-tutorial/IC790222.png "App-URL configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Projectplace** op **metagegevens downloaden**en sla het bestand op uw computer met metagegevens.

    ![Enkele aanmelding configureren] (./media/active-directory-saas-projectplace-tutorial/IC790223.png "Enkele aanmelding configureren")

5.  De MetadataFile toe aan het ondersteuningsteam Projectplace verzenden.

    >[AZURE.NOTE] De configuratie voor eenmalige aanmelding heeft door het ondersteuningsteam van Projectplace moet worden uitgevoerd. U krijgt een melding zodra de configuratie is voltooid.

6.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Enkele aanmelding configureren] (./media/active-directory-saas-projectplace-tutorial/IC790227.png "Enkele aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij Projectplace Azure AD, moeten zij worden ingericht in Projectplace.  
Bij Projectplace is het inrichten van een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw site **Projectplace** bedrijf als beheerder.

2.  Ga naar **mensen**en klik vervolgens op **leden**.

    ![Mensen] (./media/active-directory-saas-projectplace-tutorial/IC790228.png "Mensen")

3.  Klik op **lid toevoegen**.

    ![Leden toevoegen] (./media/active-directory-saas-projectplace-tutorial/IC790232.png "Leden toevoegen")

4.  Voer de volgende stappen uit in de sectie **Lid toevoegen** :

    ![Nieuwe leden] (./media/active-directory-saas-projectplace-tutorial/IC790233.png "Nieuwe leden")

    1.  Typ het e-mailadres van een geldige AAD account te verrichten in de verwante tekstvakken in het tekstvak **Nieuwe leden** .
    2.  Klik op **verzenden**

        >[AZURE.NOTE] Een e-mailbericht met een koppeling naar de account bevestigen voordat deze actief wordt verzonden naar de rekeninghouder Azure Active Directory.
    
>[AZURE.NOTE]Kunt u een andere Projectplace gebruiker account maken van hulpprogramma's of API's geleverd door Projectplace bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-projectplace-perform-the-following-steps"></a>Gebruikers toewijzen aan Projectplace, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Projectplace **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-projectplace-tutorial/IC790234.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-projectplace-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.