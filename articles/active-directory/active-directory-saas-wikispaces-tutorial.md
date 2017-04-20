<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Wikispaces | Microsoft Azure" 
    description="Meer informatie over het Wikispaces met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-wikispaces"></a>Zelfstudie: Azure Active Directory-integratie met Wikispaces
  
Het doel van deze zelfstudie is de integratie van de Azure en Wikispaces weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een Wikispaces eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, is de Azure AD-gebruikers die u hebt toegewezen aan Wikispaces mogelijk voor één teken in de toepassing van de Wikispaces bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor Wikispaces inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Sceanrio] (./media/active-directory-saas-wikispaces-tutorial/IC787182.png "Sceanrio")

##<a name="enabling-the-application-integration-for-wikispaces"></a>De integratie van toepassingen voor Wikispaces inschakelen
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Wikispaces.

###<a name="to-enable-the-application-integration-for-wikispaces-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Wikispaces, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-wikispaces-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-wikispaces-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-wikispaces-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-wikispaces-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **Wikispaces**.

    ![Galerie van toepassing] (./media/active-directory-saas-wikispaces-tutorial/IC787186.png "Galerie van toepassing")

7.  In het resultatendeelvenster **Wikispaces**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Wikispaces] (./media/active-directory-saas-wikispaces-tutorial/IC787187.png "Wikispaces")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Wikispaces met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Wikispaces** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-wikispaces-tutorial/IC787188.png "Eenmalige aanmelding configureren")

2.  Op de pagina **Hoe wilt u dat gebruikers aanmelden bij Wikispaces** Selecteer **AD Azure Microsoft Single Sign-On**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-wikispaces-tutorial/IC787189.png "Eenmalige aanmelding configureren")

3.  Typ de URL met het volgende patroon '*http://company.wikispaces.net*' op de pagina **Configureren App-URL** in het tekstvak **Wikispaces teken in de URL** en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-wikispaces-tutorial/IC787190.png "App-URL configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Wikispaces** op **metagegevens downloaden**en sla het bestand op uw computer met metagegevens.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-wikispaces-tutorial/IC787191.png "Eenmalige aanmelding configureren")

5.  De MetadataFile toe aan het ondersteuningsteam Wikispaces verzenden.

    >[AZURE.NOTE] De configuratie voor eenmalige aanmelding moet worden uitgevoerd door het ondersteuningsteam Wikispaces. U krijgt een melding zodra de configuratie is voltooid.

6.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-wikispaces-tutorial/IC787192.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij Wikispaces Azure AD, moeten zij worden ingericht in Wikispaces.  
Wikispaces is inrichten een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder uw bedrijf **Wikispaces** site.

2.  Ga naar **leden**.

    ![Leden] (./media/active-directory-saas-wikispaces-tutorial/IC787193.png "Leden")

3.  Klik op het **uitnodigen van mensen**.

    ![Mensen uitnodigen] (./media/active-directory-saas-wikispaces-tutorial/IC787194.png "Mensen uitnodigen")

4.  Voer de volgende stappen uit in de sectie **Personen uitnodigen** :

    ![Mensen uitnodigen] (./media/active-directory-saas-wikispaces-tutorial/IC787208.png "Mensen uitnodigen")

    1.  Typ de **gebruikersnamen of e-mailadres** van een geldige AAD account te verrichten in de verwante tekstvakken.
    2.  Klik op **verzenden**.  

        >[AZURE.NOTE] De accounthouder Azure Active Directory ontvangt een e-mailbericht met een koppeling naar de account bevestigen voordat deze actief wordt.

>[AZURE.NOTE] Kunt u een andere Wikispaces gebruiker account maken van hulpprogramma's of API's geleverd door Wikispaces bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-wikispaces-perform-the-following-steps"></a>Gebruikers toewijzen aan Wikispaces, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Wikispaces **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-wikispaces-tutorial/IC787195.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-wikispaces-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.