<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met 15Five | Microsoft Azure" 
    description="Meer informatie over het 15Five met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-15five"></a>Zelfstudie: Azure Active Directory-integratie met 15Five

Het doel van deze zelfstudie is de integratie van de Azure en 15Five weergeven. Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een 15Five eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan 15Five kan worden naar één teken in de toepassing van de 15Five bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor 15Five
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-15five-tutorial/IC784667.png "Scenario")
##<a name="enabling-the-application-integration-for-15five"></a>Inschakelen van de integratie van toepassingen voor 15Five

Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor 15Five.

###<a name="to-enable-the-application-integration-for-15five-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor 15Five, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-15five-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-15five-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-15five-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-15five-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **15Five**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-15five-tutorial/IC784668.png "Galerie van toepassing")

7.  **15Five**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![15Five] (./media/active-directory-saas-15five-tutorial/IC784669.png "15Five")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij 15Five met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **15Five** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-15five-tutorial/IC784670.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op 15Five** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-15five-tutorial/IC784671.png "Eenmalige aanmelding configureren")

3.  Typ de URL met het volgende patroon '*https://company.15Five.com*' op de pagina **Configureren App-URL** in het tekstvak **15Five teken In de URL** en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-15five-tutorial/IC784672.png "App-URL configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij 15Five** op **metagegevens downloaden**en doorsturen naar het bestand met metagegevens aan het ondersteuningsteam van 15Five.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-15five-tutorial/IC784673.png "Eenmalige aanmelding configureren")

    >[AZURE.NOTE] Eenmalige aanmelding moet worden ingeschakeld door het ondersteuningsteam van 15Five.

5.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-15five-tutorial/IC784674.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Om gebruikers aan te melden bij 15Five Azure AD, moeten zij worden ingericht in 15Five.  
Bij 15Five is het inrichten van een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder uw bedrijf **15Five** site.

2.  Ga naar het **bedrijf te beheren**.

    ![Bedrijf beheren] (./media/active-directory-saas-15five-tutorial/IC784675.png "Bedrijf beheren")

3.  Ga naar **mensen \> personen toevoegen**.

    ![Mensen] (./media/active-directory-saas-15five-tutorial/IC784676.png "Mensen")

4.  Voer de volgende stappen uit in de sectie nieuwe persoon toevoegen:

    ![Nieuwe persoon toevoegen] (./media/active-directory-saas-15five-tutorial/IC784677.png "Nieuwe persoon toevoegen")

    1.  Typ de **Voornaam**, **Achternaam**, **titel**, **e-mailadres** van een geldige Azure Active Directory-account verrichten in de verwante tekstvakken te.
    2.  Klik op **Gereed**.

    >[AZURE.NOTE] De accounthouder Azure AD ontvangt een e-mail met een koppeling naar de account bevestigen voordat deze actief zijn.

>[AZURE.NOTE] U kunt andere 15Five gebruiker account hulpmiddelen voor het maken of API's die door 15Five bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-15five-perform-the-following-steps"></a>Gebruikers toewijzen aan 15Five, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **15Five **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-15five-tutorial/IC784678.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-15five-tutorial/IC767830.png "Ja")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
