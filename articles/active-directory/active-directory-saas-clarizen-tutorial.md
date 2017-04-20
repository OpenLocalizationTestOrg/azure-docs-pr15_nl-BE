<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Clarizen | Microsoft Azure" 
    description="Meer informatie over het Clarizen met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-clarizen"></a>Zelfstudie: Azure Active Directory-integratie met Clarizen

Het doel van deze zelfstudie is de integratie van de Azure en Clarizen weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een Clarizen eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Clarizen kan worden naar één teken in de toepassing van de Clarizen bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Clarizen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-clarizen-tutorial/IC784679.png "Scenario")
##<a name="enabling-the-application-integration-for-clarizen"></a>Inschakelen van de integratie van toepassingen voor Clarizen

Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Clarizen.

###<a name="to-enable-the-application-integration-for-clarizen-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Clarizen, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-clarizen-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-clarizen-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-clarizen-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-clarizen-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **Clarizen**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-clarizen-tutorial/IC784680.png "Galerie van toepassing")

7.  **Clarizen**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Clarizen] (./media/active-directory-saas-clarizen-tutorial/IC784681.png "Clarizen")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Clarizen met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Clarizen** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-clarizen-tutorial/IC784682.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Clarizen** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-clarizen-tutorial/IC784683.png "Eenmalige aanmelding configureren")

3.  Op de pagina **configureren eenmalige aanmelding op Clarizen** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-clarizen-tutorial/IC784684.png "Eenmalige aanmelding configureren")

4.  In een ander web browser-venster-log in op uw site **Clarizen** bedrijf als beheerder (bijvoorbeeld: *https://app2.clarizen.com/Clarizen/Pages/Service/Login.aspx*).

5.  Klik op uw gebruikersnaam en klik vervolgens op **Instellingen**.

    ![Instellingen] (./media/active-directory-saas-clarizen-tutorial/IC784685.png "Instellingen")

6.  Klik op het tabblad **Algemene instellingen** en klik vervolgens op **bewerken**naast de **Federatieve verificatie**.

    ![Globale instellingen] (./media/active-directory-saas-clarizen-tutorial/IC786906.png "Globale instellingen")

7.  In het dialoogvenster **Verificatie federatieve** , voert u de volgende stappen uit:

    ![Federatieve verificatie] (./media/active-directory-saas-clarizen-tutorial/IC785892.png "Federatieve verificatie")

    1.  Klik op **uploaden** om uw gedownloade certificaat uploaden.
    2.  In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding bij Clarizen** de **Single Sign-On Service URL** -waarde kopiëren en vervolgens plakken in het tekstvak **URL - in** .
    3.  In de Azure klassieke portal op de pagina van het dialoogvenster **configureren enkel afmelden bij Clarizen** de **URL van de Service Single Sign-Out** waarde kopiëren en vervolgens plakken in het tekstvak **URL Sign-out** .
    4.  Selecteer **boeken gebruiken**.
    5.  Klik op **Opslaan**.

8.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-clarizen-tutorial/IC784688.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Om gebruikers aan te melden bij Clarizen Azure AD, moeten zij worden ingericht in Clarizen.  
Bij Clarizen is het inrichten van een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw site **Clarizen** bedrijf als beheerder.

2.  Klik op **mensen**.

    ![Mensen] (./media/active-directory-saas-clarizen-tutorial/IC784689.png "Mensen")

3.  Klik op **gebruiker uitnodigen**.

    ![Gebruikers uitnodigen] (./media/active-directory-saas-clarizen-tutorial/IC784690.png "Gebruikers uitnodigen")

4.  Voer de volgende stappen uit op de pagina van het dialoogvenster personen uitnodigen:

    ![Mensen uitnodigen] (./media/active-directory-saas-clarizen-tutorial/IC784691.png "Mensen uitnodigen")

    1.  Typ in het tekstvak **e-mailadres** het e-mailadres van een geldige Azure Active Directory-account die u verrichten wilt.
    2.  Klik op **uitnodigen**.

    >[AZURE.NOTE] De accounthouder Azure Active Directory een e-mailbericht ontvangt en een link om te bevestigen hun account voordat deze actief wordt als volgt.

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-clarizen-perform-the-following-steps"></a>Gebruikers toewijzen aan Clarizen, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Clarizen **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-clarizen-tutorial/IC784692.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-clarizen-tutorial/IC767830.png "Ja")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
