<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met broeikasgassen | Microsoft Azure" 
    description="Meer informatie over het Greenhouse met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-greenhouse"></a>Zelfstudie: Azure Active Directory-integratie met de uitstoot van broeikasgassen
  
Het doel van deze zelfstudie is weer te geven van de integratie van de Azure en de uitstoot van broeikasgassen.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een broeikasgassen één aanmelding abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan de uitstoot van broeikasgassen kan worden met één teken in de toepassing van de uitstoot van broeikasgassen bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor de uitstoot van broeikasgassen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-greenhouse-tutorial/IC790783.png "Scenario")
##<a name="enabling-the-application-integration-for-greenhouse"></a>Inschakelen van de integratie van toepassingen voor de uitstoot van broeikasgassen
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor de uitstoot van broeikasgassen.

###<a name="to-enable-the-application-integration-for-greenhouse-perform-the-following-steps"></a>Schakel in de integratie van toepassingen voor de uitstoot van broeikasgassen door de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-greenhouse-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-greenhouse-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-greenhouse-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-greenhouse-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak**, **uitstoot van broeikasgassen**.

    ![Galerie van toepassing] (./media/active-directory-saas-greenhouse-tutorial/IC790784.png "Galerie van toepassing")

7.  **Broeikasgassen**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Uitstoot van broeikasgassen] (./media/active-directory-saas-greenhouse-tutorial/IC790785.png "Uitstoot van broeikasgassen")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij de uitstoot van broeikasgassen met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, op de pagina **broeikasgassen** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-greenhouse-tutorial/IC790786.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**, en klik op **volgende**op de pagina **Hoe wilt u dat gebruikers aanmelden bij de uitstoot van broeikasgassen** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-greenhouse-tutorial/IC790787.png "Eenmalige aanmelding configureren")

3.  Typ de URL met het volgende patroon '*https://company.greenhouse.io*' op de pagina **Configureren App-URL** in het tekstvak **Teken op een URL** en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-greenhouse-tutorial/IC790788.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op de uitstoot van broeikasgassen** op **metagegevens downloaden**en vervolgens metagegevensbestand lokaal op uw computer opslaan.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-greenhouse-tutorial/IC790789.png "Eenmalige aanmelding configureren")

5.  Stuur dat bestand metagegevens aan broeikasgassen Support team.

    >[AZURE.NOTE] Eenmalige aanmelding heeft worden ingeschakeld door de uitstoot van broeikasgassen support team.

6.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-greenhouse-tutorial/IC790790.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij de uitstoot van broeikasgassen Azure AD, moeten ze worden ingericht in de uitstoot van broeikasgassen.  
Voor de uitstoot van broeikasgassen is inrichting een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder uw bedrijf **broeikasgassen** site.

2.  In het menu aan de bovenkant, klikt u op **configureren**en klik vervolgens op **gebruikers**.

    ![Gebruikers] (./media/active-directory-saas-greenhouse-tutorial/IC790791.png "Gebruikers")

3.  Klik op **nieuwe gebruikers**.

    ![Nieuwe gebruiker] (./media/active-directory-saas-greenhouse-tutorial/IC790792.png "Nieuwe gebruiker")

4.  In de sectie **Gebruiker toevoegen** kunt u de volgende stappen uitvoeren:

    ![Nieuwe gebruiker toevoegen] (./media/active-directory-saas-greenhouse-tutorial/IC790793.png "Nieuwe gebruiker toevoegen")

    1.  Typ in het tekstvak **gebruiker e-mails met Enter** het e-mailadres van een geldige Azure Active Directory-account die u verrichten wilt.
    2.  Klik op **Opslaan**.
        
        >[AZURE.NOTE] De houders van Azure Active Directory-account ontvangt een e-mailbericht met een koppeling naar de account bevestigen voordat deze actief wordt.

>[AZURE.NOTE] Kunt u een andere broeikasgassen gebruiker account hulpmiddelen voor het maken of API's die door de uitstoot van broeikasgassen bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-greenhouse-perform-the-following-steps"></a>Gebruikers toewijzen aan de uitstoot van broeikasgassen, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  De **uitstoot van broeikasgassen **Integratiepagina, klik op **gebruikers toewijzen**.

    ![Gebruikers toewijzen] (./media/active-directory-saas-greenhouse-tutorial/IC790794.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-greenhouse-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.