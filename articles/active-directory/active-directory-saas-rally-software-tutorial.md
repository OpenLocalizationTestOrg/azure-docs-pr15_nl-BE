<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Rally Software | Microsoft Azure" 
    description="Meer informatie over het Rally-Software met Azure Active Directory gebruiken om te schakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-rally-software"></a>Zelfstudie: Azure Active Directory-integratie met Rally Software
  
Het doel van deze zelfstudie is de integratie van de Azure en Rally Software weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder Rally Software
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Rally-Software
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-rally-software-tutorial/IC769525.png "Scenario")
##<a name="enabling-the-application-integration-for-rally-software"></a>Inschakelen van de integratie van toepassingen voor Rally-Software
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Rally-Software.

###<a name="to-enable-the-application-integration-for-rally-software-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Rally-Software, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-rally-software-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-rally-software-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-rally-software-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-rally-software-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak**, **rally**.

    ![Galerie van toepassing] (./media/active-directory-saas-rally-software-tutorial/IC769526.png "Galerie van toepassing")

7.  In het resultatendeelvenster **Rally Software**te selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Rally Software] (./media/active-directory-saas-rally-software-tutorial/IC769527.png "Rally Software")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Rally Software met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
U moet een certificaat uploaden naar Rally Software als onderdeel van deze procedure.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Rally Software **application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-rally-software-tutorial/IC749323.png "Eenmalige aanmelding configureren")

2.  Op de pagina **Hoe wilt u gebruikers aan te melden op Rally** Selecteer **AD Azure Microsoft Single Sign-On**en klik vervolgens op **volgende**.

    ![Microsoft Azure AD Single Sign-On] (./media/active-directory-saas-rally-software-tutorial/IC769528.png "Microsoft Azure AD Single Sign-On")

3.  Typ op de pagina **URL van App configureren** in het tekstvak **URL van Software huurder Rally** uw URL met behulp van het volgende patroon "*https://\<huurder naam\>. rally.com*", en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-rally-software-tutorial/IC769529.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding bij Rally** op metagegevens downloaden en sla het op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-rally-software-tutorial/IC769530.png "Eenmalige aanmelding configureren")

5.  Log in op uw huurder **Rally Software** .

6.  In de werkbalk op de bovenkant, klikt u op **Instellingen**en selecteer **abonnement**.

    ![Abonnement] (./media/active-directory-saas-rally-software-tutorial/IC769531.png "Abonnement")

7.  Klik op de knop **actie** In de werkbalk bovenaan rechts op en selecteer vervolgens **Bewerken-abonnement**.

8.  Voer de volgende stappen uit en klik vervolgens op **Opslaan en sluiten**op de pagina van het dialoogvenster **abonnement** :

    ![Verificatie] (./media/active-directory-saas-rally-software-tutorial/IC769542.png "Verificatie")

    1.  **Rally of een SSO-verificatie** selecteert in de vervolgkeuzelijst verificatie
    2.  In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding bij Rally Software** de **Identity Provider-ID** -waarde kopiëren en vervolgens plakken in het tekstvak **URL van de Provider identiteit**
    3.  Kopieer de **Externe Logout URL** -waarde in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Rally Software** .

9.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-rally-software-tutorial/IC769547.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
AAD gebruikers kunnen inloggen, als ze worden ingericht voor de Rally softwaretoepassing met hun naam Azure Active Directory.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Log in op uw huurder Rally Software.

2.  Ga naar **Setup \> gebruikers**, en klik vervolgens op **+ Nieuw toevoegen**.

    ![Gebruikers] (./media/active-directory-saas-rally-software-tutorial/IC781039.png "Gebruikers")

3.  Typ de naam in het tekstvak van de nieuwe gebruiker en klik op **toevoegen met Details**.

4.  Voer de volgende stappen uit in de sectie **Gebruiker maken** :

    ![Gebruiker maken] (./media/active-directory-saas-rally-software-tutorial/IC781040.png "Gebruiker maken")

    1.  Typ in het tekstvak **Gebruikersnaam** de naam van de gewenste voorziening Azure AD-gebruiker.
    2.  Typ in het tekstvak **E-mailadres** het e-mailadres van de Azure AD-gebruiker die u verrichten wilt.
    3.  Klik op **Opslaan en sluiten**.

>[AZURE.NOTE]Kunt u andere Software Rally gebruiker account maken tools of API's geleverd door Rally Software bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-rally-software-perform-the-following-steps"></a>Gebruikers toewijzen aan Rally Software, moet u de volgende stappen uitvoeren:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op de pagina **Rally Software** application integration **gebruikers toewijzen**.

    ![Gebruikers toewijzen] (./media/active-directory-saas-rally-software-tutorial/IC769548.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-rally-software-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.




