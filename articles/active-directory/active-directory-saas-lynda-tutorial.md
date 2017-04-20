<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Lynda.com | Microsoft Azure" 
    description="Meer informatie over het Lynda.com met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-lyndacom"></a>Zelfstudie: Azure Active Directory-integratie met Lynda.com
  
Het doel van deze zelfstudie is de integratie van de Azure en Lynda.com weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder Lynda.com
  
Na het voltooien van deze zelfstudie, is de Azure AD-gebruikers die u hebt toegewezen aan Lynda.com mogelijk voor één teken in de toepassing van de Lynda.com bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor Lynda.com inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-lynda-tutorial/IC781046.png "Scenario")
##<a name="enabling-the-application-integration-for-lyndacom"></a>De integratie van toepassingen voor Lynda.com inschakelen
  
Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor Lynda.com overzicht.

###<a name="to-enable-the-application-integration-for-lyndacom-perform-the-following-steps"></a>Schakel in de integratie van toepassingen voor Lynda.com door de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-lynda-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-lynda-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-lynda-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-lynda-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **Lynda.com**.

    ![Galerie van toepassing] (./media/active-directory-saas-lynda-tutorial/IC777524.png "Galerie van toepassing")

7.  In het resultatendeelvenster **Lynda.com**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Lynda.com] (./media/active-directory-saas-lynda-tutorial/IC777525.png "Lynda.com")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Lynda.com met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

>[AZURE.IMPORTANT]Voordat u eenmalige aanmelding configureren op uw huurder Lynda.com, moet u eerst contact met de technische ondersteuning van Lynda.com als u deze functie is ingeschakeld.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Lynda.com** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-lynda-tutorial/IC777526.png "Eenmalige aanmelding configureren")

2.  Op de pagina **Hoe wilt u dat gebruikers aanmelden bij Lynda.com** Selecteer **AD Azure Microsoft Single Sign-On**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-lynda-tutorial/IC777527.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App URL configureren** , in het tekstvak **Lynda.com teken In de URL** de URL van de huurder Lynda.com (bv.: *https://shib.lynda.com/Shibboleth.sso/InCommon?providerId=https://sts.windows-ppe.net/6247032d-9415-403c-b72b-277e3fb6f2c8/&target= https://shib.lynda.com/InCommon*), en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-lynda-tutorial/IC781047.png "App-URL configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Lynda.com** downloaden van de metagegevens op **metagegevens downloaden**en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-lynda-tutorial/IC777529.png "Eenmalige aanmelding configureren")

5.  De van het gedownloade metagegevensbestand naar het ondersteuningsteam Lynda.com verzenden. Het ondersteuningsteam Lynda.com biedt de eenmalige aanmelding-configuratie voor u.

6.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-lynda-tutorial/IC777530.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Er is geen actie-item aan Lynda.com van gebruikers configureren.  
Wanneer een toegewezen gebruiker probeert aan te melden bij Lynda.com met behulp van het paneel toegang, Lynda.com wordt gecontroleerd of de gebruiker bestaat.  
Als er nog geen gebruikersaccount beschikbaar, wordt deze automatisch gemaakt door Lynda.com.

>[AZURE.NOTE]Kunt u een andere Lynda.com gebruiker account maken van hulpprogramma's of API's geleverd door Lynda.com bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-lyndacom-perform-the-following-steps"></a>Gebruikers toewijzen aan Lynda.com, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Lynda.com **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-lynda-tutorial/IC777531.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-lynda-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.