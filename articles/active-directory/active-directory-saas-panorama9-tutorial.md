<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Panorama9 | Microsoft Azure" 
    description="Meer informatie over het Panorama9 met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-panorama9"></a>Zelfstudie: Azure Active Directory-integratie met Panorama9
  
Het doel van deze zelfstudie is de integratie van de Azure en Panorama9 weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een Panorama9 eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Panorama9 kan worden naar één teken in de toepassing van de Panorama9 bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Panorama9
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-panorama9-tutorial/IC790016.png "Scenario")
##<a name="enabling-the-application-integration-for-panorama9"></a>Inschakelen van de integratie van toepassingen voor Panorama9
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Panorama9.

###<a name="to-enable-the-application-integration-for-panorama9-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Panorama9, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-panorama9-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-panorama9-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-panorama9-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-panorama9-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **Panorama9**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-panorama9-tutorial/IC790017.png "Galerie van toepassing")

7.  **Panorama9**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Panorama9] (./media/active-directory-saas-panorama9-tutorial/IC790018.png "Panorama9")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Panorama9 met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Configureren van eenmalige aanmelding voor Panorama9 moet u een waarde uit een certificaat ophalen.  
Als u niet bekend met deze procedure bent, raadpleegt u [voor het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Panorama9** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-panorama9-tutorial/IC790019.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Panorama9** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-panorama9-tutorial/IC790020.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App URL configureren** , in het tekstvak **Panorama9 teken op URL** de URL die wordt gebruikt door uw gebruikers aanmelden bij Panorama9 (bijvoorbeeld: "*https://dashboard.panorama9.com/saml/access/3262*"), en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-panorama9-tutorial/IC790021.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op Panorama9** om te downloaden van uw certificaat, klik op **certificaat downloaden**en lokaal opslaan op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-panorama9-tutorial/IC790022.png "Eenmalige aanmelding configureren")

5.  Log in op uw site Panorama9 bedrijf als beheerder in een ander web browser-venster.

6.  In de werkbalk op de bovenkant, klikt u op **beheren**en klik op **extensies**.

    ![Extensies] (./media/active-directory-saas-panorama9-tutorial/IC790023.png "Extensies")

7.  Klik in het dialoogvenster **extensies** **Single Sign-On**.

    ![Eenmalige aanmelding] (./media/active-directory-saas-panorama9-tutorial/IC790024.png "Eenmalige aanmelding")

8.  Voer de volgende stappen uit in de sectie **Instellingen** :

    ![Instellingen] (./media/active-directory-saas-panorama9-tutorial/IC790025.png "Instellingen")

    1.  In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding bij Panorama9** de **Single Sign-On Service URL** -waarde kopiëren en vervolgens plakken in het tekstvak **URL van de provider identiteit** .
    2.  **De waarde** van het geëxporteerde certificaat kopiëren en vervolgens plakken in het tekstvak **certificaat vingerafdruk** .  

        >[AZURE.TIP]Zie voor meer informatie [hoe u kunt ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

    3.  Klik op **Opslaan**.

9.  Op de klassieke portal Azure AD selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **Voltooien** om het **Configureren van eenmalige aanmelding** dialoogvenster te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-panorama9-tutorial/IC790026.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij Panorama9 Azure AD, moeten zij worden ingericht in Panorama9.  
Bij Panorama9 is het inrichten van een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Log in op uw site **Panorama9** bedrijf als beheerder.

2.  In het menu aan de bovenkant, klikt u op **beheren**en klik vervolgens op **gebruikers**.

    ![Gebruikers] (./media/active-directory-saas-panorama9-tutorial/IC790027.png "Gebruikers")

3.  Klik op **+**.

4.  Voer de volgende stappen uit in de sectie gebruiker gegevens:

    ![Gebruikers] (./media/active-directory-saas-panorama9-tutorial/IC790028.png "Gebruikers")

    1.  Typ in het tekstvak **e-mailadres** het e-mailadres van een geldige Azure Active Directory-gebruiker die u verrichten wilt.
    2.  Klik op **Opslaan**.

>[AZURE.NOTE]Kunt u een andere Panorama9 gebruiker account maken van hulpprogramma's of API's geleverd door Panorama9 bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-panorama9-perform-the-following-steps"></a>Gebruikers toewijzen aan Panorama9, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Panorama9** application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-panorama9-tutorial/IC790029.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-panorama9-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.