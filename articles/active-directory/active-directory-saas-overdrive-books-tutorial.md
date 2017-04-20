<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Overdrive boeken | Microsoft Azure" 
    description="Meer informatie over het Overdrive boeken met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-overdrive-books"></a>Zelfstudie: Azure Active Directory-integratie met Overdrive boeken
  
Het doel van deze zelfstudie is de integratie van de Azure en OverDrive weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een OverDrive eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, is de Azure AD-gebruikers die u hebt toegewezen aan de OverDrive mogelijk voor één teken in de toepassing van de OverDrive bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor OverDrive inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-overdrive-books-tutorial/IC784462.png "Scenario")
##<a name="enabling-the-application-integration-for-overdrive"></a>De integratie van toepassingen voor OverDrive inschakelen
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor OverDrive.

###<a name="to-enable-the-application-integration-for-overdrive-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor OverDrive, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-overdrive-books-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-overdrive-books-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-overdrive-books-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-overdrive-books-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak**, **OverDrive**.

    ![Galerie van toepassing] (./media/active-directory-saas-overdrive-books-tutorial/IC784463.png "Galerie van toepassing")

7.  In het resultatendeelvenster **OverDrive**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![OverDrive] (./media/active-directory-saas-overdrive-books-tutorial/IC799950.png "OverDrive")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij de OverDrive met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **OverDrive** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding inschakelen] (./media/active-directory-saas-overdrive-books-tutorial/IC784465.png "Eenmalige aanmelding inschakelen")

2.  Op de pagina **Hoe wilt u gebruikers aan te melden op OverDrive** Selecteer **AD Azure Microsoft Single Sign-On**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-overdrive-books-tutorial/IC784466.png "Eenmalige aanmelding configureren")

3.  Typ de URL met het volgende patroon '*http://mslibrarytest.libraryreserve.com*' op de pagina **Configureren App-URL** in het tekstvak **OverDrive teken In de URL** en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-overdrive-books-tutorial/IC784467.png "App-URL configureren")

4.  Op de **Configure eenmalige aanmelding op OverDrive** pagina te downloaden van het bestand met metagegevens en verzend het naar het ondersteuningsteam OverDrive.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-overdrive-books-tutorial/IC784468.png "Eenmalige aanmelding configureren")

    >[AZURE.NOTE]Het ondersteuningsteam OverDrive eenmalige aanmelding voor u configureert en stuurt u een melding wanneer de configuratie is voltooid.

5.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-overdrive-books-tutorial/IC784469.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Er is geen actie-item voor u gebruikersaanvragen op OverDrive configureren.  
Wanneer een toegewezen gebruiker zich probeert aan te melden bij de OverDrive, wordt automatisch een OverDrive-account gemaakt indien nodig.

>[AZURE.NOTE]Kunt u alle andere OverDrive gebruiker account maken van hulpprogramma's of API's geleverd door OverDrive bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-overdrive-perform-the-following-steps"></a>Gebruikers toewijzen aan OverDrive, kunt u de volgende stappen uitvoeren:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **OverDrive **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-overdrive-books-tutorial/IC784470.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-overdrive-books-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.