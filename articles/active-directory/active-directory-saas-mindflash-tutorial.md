<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Mindflash | Microsoft Azure" 
    description="Meer informatie over het Mindflash met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-mindflash"></a>Zelfstudie: Azure Active Directory-integratie met Mindflash
  
Het doel van deze zelfstudie is de integratie van de Azure en Mindflash weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een Mindflash eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Mindflash kan worden naar één teken in de toepassing van de Mindflash bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Mindflash
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-mindflash-tutorial/IC787132.png "Scenario")
##<a name="enabling-the-application-integration-for-mindflash"></a>Inschakelen van de integratie van toepassingen voor Mindflash
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Mindflash.

###<a name="to-enable-the-application-integration-for-mindflash-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Mindflash, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-mindflash-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-mindflash-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-mindflash-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-mindflash-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **Mindflash**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-mindflash-tutorial/IC787133.png "Galerie van toepassing")

7.  **Mindflash**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Mindflash] (./media/active-directory-saas-mindflash-tutorial/IC787134.png "Mindflash")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Mindflash met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Mindflash** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mindflash-tutorial/IC787135.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Mindflash** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mindflash-tutorial/IC787136.png "Eenmalige aanmelding configureren")

3.  Typ de URL met het volgende patroon '*http://company.mindflash.com*' op de pagina **Configureren App-URL** in het tekstvak **Teken op een URL** en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-mindflash-tutorial/IC787137.png "App-URL configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Mindflash** op **metagegevens downloaden**en sla het bestand op uw computer met metagegevens.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mindflash-tutorial/IC787138.png "Eenmalige aanmelding configureren")

5.  De MetadataFile toe aan het ondersteuningsteam Mindflash verzenden.

    >[AZURE.NOTE] De configuratie voor eenmalige aanmelding heeft door het ondersteuningsteam van Mindflash moet worden uitgevoerd. U krijgt een melding zodra de configuratie is voltooid.

6.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mindflash-tutorial/IC787139.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij Mindflash Azure AD, moeten zij worden ingericht in Mindflash.  
Bij Mindflash is het inrichten van een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw site **Mindflash** bedrijf als beheerder.

2.  Het gaat om gebruikers te **beheren**.

    ![Gebruikers beheren] (./media/active-directory-saas-mindflash-tutorial/IC787140.png "Gebruikers beheren")

3.  Klik op **Gebruikers toevoegen**en klik vervolgens op **Nieuw**.

4.  In de sectie **Nieuwe gebruikers toevoegen** , voert u de volgende stappen uit:

    ![Nieuwe gebruikers toevoegen] (./media/active-directory-saas-mindflash-tutorial/IC787141.png "Nieuwe gebruikers toevoegen")

    1.  Typ de **Voornaam**, **Achternaam** en **e-mailadres** van een geldige AAD account te verrichten in de verwante tekstvakken.
    2.  Klik op **toevoegen**.

>[AZURE.NOTE]Kunt u een andere Mindflash gebruiker account maken van hulpprogramma's of API's geleverd door Mindflash bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-mindflash-perform-the-following-steps"></a>Gebruikers toewijzen aan Mindflash, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Mindflash **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-mindflash-tutorial/IC787142.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-mindflash-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.