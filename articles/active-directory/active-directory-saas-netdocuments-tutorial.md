<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met NetDocuments | Microsoft Azure" 
    description="Meer informatie over het NetDocuments met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-netdocuments"></a>Zelfstudie: Azure Active Directory-integratie met NetDocuments
  
Het doel van deze zelfstudie is de integratie van de Azure en NetDocuments weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder NetDocuments
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan NetDocuments kan worden naar één teken in de toepassing van de NetDocuments bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor NetDocuments
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-netdocuments-tutorial/IC795040.png "Scenario")
##<a name="enabling-the-application-integration-for-netdocuments"></a>Inschakelen van de integratie van toepassingen voor NetDocuments
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor NetDocuments.

###<a name="to-enable-the-application-integration-for-netdocuments-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor NetDocuments, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-netdocuments-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-netdocuments-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-netdocuments-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-netdocuments-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **NetDocuments**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-netdocuments-tutorial/IC795041.png "Galerie van toepassing")

7.  **NetDocuments**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![NetDocuments] (./media/active-directory-saas-netdocuments-tutorial/IC795042.png "NetDocuments")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij NetDocuments met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Configureren van eenmalige aanmelding voor NetDocuments moet u een waarde uit een certificaat ophalen.  
Als u niet bekend met deze procedure bent, raadpleegt u [voor het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **NetDocuments** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-netdocuments-tutorial/IC795043.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op NetDocuments** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-netdocuments-tutorial/IC795044.png "Eenmalige aanmelding configureren")

3.  Op de pagina **URL van App configureren** , moet u de volgende stappen uitvoeren:

    ![App-URL configureren] (./media/active-directory-saas-netdocuments-tutorial/IC795045.png "App-URL configureren")

    1.  Typ in het tekstvak **Teken op URL** de URL die wordt gebruikt door de gebruikers aan te melden op uw toepassing NetDocuments (bijvoorbeeld: "*https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=CA-JI1BG3H1*").
    2.  Typ in het tekstvak **URL van NetDocuments-antwoord** dezelfde waarde die u hebt getypt in het **Teken van URL** textbox.  

        >[AZURE.NOTE]U vindt de juiste waarde aan het einde van het dialoogvenster **Federatieve identiteit** (Zie het screenshot voor stap 9).

    3.  Klik op **volgende**

4.  Op de pagina **configureren eenmalige aanmelding op NetDocuments** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-netdocuments-tutorial/IC795046.png "Eenmalige aanmelding configureren")

5.  Log in op uw site NetDocuments bedrijf als beheerder in een ander web browser-venster.

6.  Ga naar **Admin**.

7.  Klik op **toevoegen en verwijderen, gebruikers en groepen**.

    ![Opslagplaats] (./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Opslagplaats")

8.  Klik op **configureren, geavanceerde verificatie-opties**.

    ![Configureren, geavanceerde verificatie-opties] (./media/active-directory-saas-netdocuments-tutorial/IC795048.png "Configureren, geavanceerde verificatie-opties")

9.  In het dialoogvenster **De federatieve identiteit** , kunt u de volgende stappen uitvoeren:

    ![Federatieve Identitty] (./media/active-directory-saas-netdocuments-tutorial/IC795049.png "Federatieve Identitty")

    1.  Selecteer als **type federatieve identiteit-server**, **Active Directory Federation Services**.
    2.  Klik op **bestand kiezen**om de van het gedownloade metagegevensbestand te uploaden.
    3.  Klik op **OK**.

10. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-netdocuments-tutorial/IC795050.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij NetDocuments Azure AD, moeten zij worden ingericht in NetDocuments.  
Bij NetDocuments is het inrichten van een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Zing mee op uw site **NetDocuments** bedrijf als beheerder.

2.  In het menu aan de bovenkant, klik op de **beheerder**.

    ![Admin] (./media/active-directory-saas-netdocuments-tutorial/IC795051.png "Admin")

3.  Klik op **toevoegen en verwijderen, gebruikers en groepen**.

    ![Opslagplaats] (./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Opslagplaats")

4.  Typ in het tekstvak **E-mailadres** het e-mailadres van een geldige Azure Active Directory-account dat u wilt verrichten, en klik vervolgens op **Gebruiker toevoegen**.

    ![E-mailadres] (./media/active-directory-saas-netdocuments-tutorial/IC795053.png "E-mailadres")

    >[AZURE.NOTE]De accounthouder Azure Active Directory krijgt een e-mail met daarin een link om te bevestigen de account voordat deze actief wordt.

>[AZURE.NOTE]Kunt u een andere NetDocuments gebruiker account maken van hulpprogramma's of API's geleverd door NetDocuments bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-netdocuments-perform-the-following-steps"></a>Gebruikers toewijzen aan NetDocuments, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **NetDocuments **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-netdocuments-tutorial/IC795054.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-netdocuments-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.