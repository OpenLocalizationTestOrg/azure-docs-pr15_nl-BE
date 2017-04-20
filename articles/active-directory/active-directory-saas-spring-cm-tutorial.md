<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met veer CM | Microsoft Azure" 
    description="Meer informatie over het voorjaar CM met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-spring-cm"></a>Zelfstudie: Azure Active Directory-integratie met veer CM
  
Het doel van deze zelfstudie is het instellen van eenmalige aanmelding tussen Azure Active Directory en SpringCM weergeven.
  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een SpringCM eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, worden de Azure Active Directory-gebruikers die u hebt toegewezen aan SpringCM kunnen eenmalige aanmelding via het deelvenster AAD toegang.

1.  Inschakelen van de integratie van toepassingen voor SpringCM
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-spring-cm-tutorial/IC797044.png "Scenario")

##<a name="enabling-the-application-integration-for-springcm"></a>Inschakelen van de integratie van toepassingen voor SpringCM
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor SpringCM.

###<a name="to-enable-the-application-integration-for-springcm-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor SpringCM, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-spring-cm-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-spring-cm-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-spring-cm-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-spring-cm-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **SpringCM**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-spring-cm-tutorial/IC797045.png "Galerie van toepassing")

7.  **SpringCM**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![SpringCM] (./media/active-directory-saas-spring-cm-tutorial/IC797046.png "SpringCM")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Deze sectie behandelt het inschakelen van gebruikers worden geverifieerd met hun account in Azure Active Directory, op basis van het protocol SAML federation met SpringCM.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **SpringCM** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-spring-cm-tutorial/IC797047.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op SpringCM** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-spring-cm-tutorial/IC797048.png "Eenmalige aanmelding configureren")

3.  Typ de URL die wordt gebruikt door de gebruikers aan te melden op de toepassing van de SpringCM op de pagina **Configureren App-URL** in het tekstvak **SpringCM aanmelden op de URL** en klik op **volgende**. 

    De app-URL is de URL van uw SpringCM huurder (bijvoorbeeld: *https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=16826*):

    ![App-URL configureren] (./media/active-directory-saas-spring-cm-tutorial/IC797049.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op SpringCM** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal op uw computer.

    ![Enkele aanmelding configureren] (./media/active-directory-saas-spring-cm-tutorial/IC797050.png "Enkele aanmelding configureren")

5.  In een ander web browser-venster aanmelden op uw site **SpringCM** bedrijf als beheerder.

6.  Klik op **Ga naar**in het menu aan de bovenkant, klikt u op **Voorkeuren**en klik in de sectie **Accountvoorkeuren** **SAML SSO**.

    ![SAML SSO] (./media/active-directory-saas-spring-cm-tutorial/IC797051.png "SAML SSO")

7.  In de sectie identiteit providerconfiguratie, kunt u de volgende stappen uitvoeren:

    ![Identity Provider configureren] (./media/active-directory-saas-spring-cm-tutorial/IC797052.png "Identity Provider configureren")

    1.  Uw gedownloade Azure Active Directory certificaat uploaden, klikt u op **Selecteer uitgevende instelling** of **Wijziging uitgever certificaat**.
    2.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij SpringCM** de **Uitgever URL** -waarde kopiëren en vervolgens plakken in het tekstvak van de **uitgevende instelling** .
    3.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij SpringCM** de waarde **Singel Sign-On Service URL** kopiëren en vervolgens plakken in de textbox **Eindpunt van Service Provider (SP) gestart** .
    4.  Als het **SAML ingeschakeld**, selecteer **inschakelen**.
    5.  Klik op **Opslaan**.

8.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Enkele aanmelding configureren] (./media/active-directory-saas-spring-cm-tutorial/IC797053.png "Enkele aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om Azure Active Directory-gebruikers kunnen zich aanmelden bij SpringCM, moeten zij worden ingericht in SpringCM.  
Bij SpringCM is het inrichten van een handmatige taak.

>[AZURE.NOTE] Zie voor meer details, [maken en bewerken van een gebruiker SpringCM](http://knowledge.springcm.com/create-and-edit-a-springcm-user)

###<a name="to-provision-a-user-account-to-springcm-perform-the-following-steps"></a>Als een gebruikersaccount om SpringCM te creëren, kunt u de volgende stappen uitvoeren:

1.  Meld u als beheerder uw bedrijf **SpringCM** site.

2.  Klik op **Ga naar**en klik vervolgens op **Adresboek**.

    ![Gebruiker maken] (./media/active-directory-saas-spring-cm-tutorial/IC797054.png "Gebruiker maken")

3.  Klik op **gebruiker maken**.

4.  Selecteer een **rol van de gebruiker**.

5.  Selecteer **activeren per E-mail**.

6.  Typ de voornaam, achternaam en e-mailadres van een geldige Azure Active Directory-gebruikersaccount die u verrichten in de verwante tekstvakken wilt.

7.  Voeg de gebruiker toe aan een **beveiligingsgroep**.

8.  Klik op **Opslaan**.

>[AZURE.NOTE] Kunt u een andere SpringCM gebruiker account maken van hulpprogramma's of API's geleverd door SpringCM bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-springcm-perform-the-following-steps"></a>Gebruikers toewijzen aan SpringCM, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **SpringCM** application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-spring-cm-tutorial/IC797055.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-spring-cm-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.




