<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met FM: systemen | Microsoft Azure" 
    description="Informatie over het gebruik van FM: systemen met Azure Active Directory inschakelen voor eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-fm-systems"></a>Zelfstudie: Azure Active Directory-integratie met FM: systemen
  
Het doel van deze zelfstudie is de integratie van de Azure en FM:Systems weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een FM:Systems eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan FM:Systems kan worden naar één teken in de toepassing van de FM:Systems bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor FM:Systems
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-fm-systems-tutorial/IC795899.png "Scenario")
##<a name="enabling-the-application-integration-for-fmsystems"></a>Inschakelen van de integratie van toepassingen voor FM:Systems
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor FM:Systems.

###<a name="to-enable-the-application-integration-for-fmsystems-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor FM:Systems, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-fm-systems-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-fm-systems-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-fm-systems-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-fm-systems-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **FM:Systems**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-fm-systems-tutorial/IC795900.png "Galerie van toepassing")

7.  **FM:Systems**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![FM: systemen] (./media/active-directory-saas-fm-systems-tutorial/IC800213.png "FM: systemen")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij FM:Systems met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **FM:Systems** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-fm-systems-tutorial/IC790810.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op FM:Systems** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-fm-systems-tutorial/IC795901.png "Eenmalige aanmelding configureren")

3.  Op de pagina **URL van App configureren** , moet u de volgende stappen uitvoeren:

    ![App-URL configureren] (./media/active-directory-saas-fm-systems-tutorial/IC795902.png "App-URL configureren")

    1.  Typ in het tekstvak **FM:Systems teken op URL** uw FM:Systems **Antwoord URL** (bijvoorbeeld: *https://dpr.fmshosted.com/fminteract/ConsumerService2.aspx*).  

        >[AZURE.WARNING] U kunt deze waarde opvragen bij uw FM: ondersteuning voor team.

    2.  Klik op **volgende**

4.  Op de pagina **configureren eenmalige aanmelding bij FM:Systems** downloaden van de metagegevens op **metagegevens downloaden**en vervolgens de metagegevens opslaan op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-fm-systems-tutorial/IC795903.png "Eenmalige aanmelding configureren")

5.  Het gedownloade metagegevensbestand verzenden naar uw FM: ondersteuning voor team.

    >[AZURE.NOTE] Uw FM: Ondersteuningsteam systemen is de werkelijke SSO-configuratie uitvoeren.
U krijgt een melding wanneer de eenmalige aanmelding is ingeschakeld voor uw abonnement.

6.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-fm-systems-tutorial/IC795904.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij FM:Systems Azure AD, moeten zij worden ingericht in FM:Systems.  
Bij FM:Systems is het inrichten van een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Log in op uw site FM:Systems bedrijf als beheerder in een webbrowservenster.

2.  Ga naar **System Administration \> beveiliging beheren \> gebruikers \> lijst met gebruikers**.

    ![System Administration] (./media/active-directory-saas-fm-systems-tutorial/IC795905.png "System Administration")

3.  Klik op **nieuwe gebruiker maken**.

    ![Nieuwe gebruiker maken] (./media/active-directory-saas-fm-systems-tutorial/IC795906.png "Nieuwe gebruiker maken")

4.  Voer de volgende stappen uit in de sectie **Gebruiker maken** :

    ![Gebruiker maken] (./media/active-directory-saas-fm-systems-tutorial/IC795907.png "Gebruiker maken")

    1.  Typ de gebruikersnaam, het wachtwoord en de bevestiging, het e-mailadres en de werknemer-ID van een geldige Azure Active Directory-account verrichten in de verwante tekstvakken te.
    2.  Klik op **volgende**.

>[AZURE.NOTE] U kunt andere FM:Systems gebruiker account hulpmiddelen voor het maken of API's die door FM:Systems bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-fmsystems-perform-the-following-steps"></a>Gebruikers toewijzen aan FM:Systems, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **FM:Systems **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-fm-systems-tutorial/IC795908.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-fm-systems-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.