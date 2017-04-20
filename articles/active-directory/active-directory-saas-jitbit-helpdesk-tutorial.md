<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Jitbit Helpdesk | Microsoft Azure" 
    description="Meer informatie over het Jitbit Helpdesk met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a>Zelfstudie: Azure Active Directory-integratie met Jitbit Helpdesk
  
Het doel van deze zelfstudie is de integratie van de Azure en Jitbit Helpdesk weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder Jitbit Helpdesk
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan de Jitbit Helpdesk kan worden met één teken in de toepassing van de Jitbit Helpdesk bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor de Jitbit Helpdesk inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777676.png "Scenario")
##<a name="enabling-the-application-integration-for-jitbit-helpdesk"></a>De integratie van toepassingen voor de Jitbit Helpdesk inschakelen
  
Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor de Jitbit Helpdesk overzicht.

###<a name="to-enable-the-application-integration-for-jitbit-helpdesk-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Jitbit Helpdesk, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **Jitbit Helpdesk**.

    ![Galerie van toepassing] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777677.png "Galerie van toepassing")

7.  In het resultatendeelvenster **Jitbit Helpdesk**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![JitBit] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC781008.png "JitBit")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij de Jitbit Helpdesk met hun account in Azure AD federation op basis van de SAML-protocol gebruiken. .  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat-bestand te maken.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

>[AZURE.IMPORTANT] Voordat u eenmalige aanmelding configureren op uw huurder Jitbit Helpdesk, moet u eerst contact met de Jitbit Helpdesk technische ondersteuning als u deze functie is ingeschakeld.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Jitbit Helpdesk** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777678.png "Eenmalige aanmelding configureren")

2.  Op de pagina **Hoe wilt u dat gebruikers aan te melden op de Jitbit Helpdesk** **AD Azure Microsoft Single Sign-On**selecteren en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777679.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **URL van App configureren** in **Jitbit Helpdesk Sign In URL** textbox, de URL van uw gebruik van het volgende patroon "https://*\<-naam van de huurder\>. Jitbit.com*", en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777528.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding bij de Jitbit Helpdesk** downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal als **c:\\Jitbit Helpdesk.cer**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777680.png "Eenmalige aanmelding configureren")

5.  Log in op uw site Jitbit Helpdesk bedrijf als beheerder in een ander web browser-venster.

6.  Klik op **beheer**op de werkbalk op de voorgrond.

    ![Beheer] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Beheer")

7.  Klik op **algemene instellingen**.

    ![Gebruikers, bedrijven en machtigingen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Gebruikers, bedrijven en machtigingen")

8.  Voer de volgende stappen uit in de configuratiesectie van **verificatie-instellingen** :

    ![Verificatie-instellingen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777683.png "Verificatie-instellingen")

    1.  Selecteer **inschakelen-SAML enkel 2.0 aanmelden** inloggen met Single Sign-On (SSO) met **OneLogin**.
    2.  In de Azure klassieke portal op de dialoog pagina **configureren eenmalige aanmelding bij de Jitbit Helpdesk** de **Service Provider (SP) eindpunt geïnitieerd** waarde kopiëren en vervolgens plakken in het tekstvak **URL van het eindpunt** .
    3.  Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.
        
        >[AZURE.TIP]Zie voor meer informatie, [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    4.  De base-64 gecodeerde certificaat openen, de inhoud ervan kopiëren naar het Klembord en plak deze vervolgens naar de textbox **X.509-certificaat**
    5.  Klik op **wijzigingen opslaan**.

9.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777684.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij de Jitbit Helpdesk Azure AD, moeten zij worden ingericht in Jitbit Helpdesk.  
Jitbit Helpdesk is inrichten een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw huurder **Jitbit Helpdesk** .

2.  In het menu aan de bovenkant, klikt u op **beheer**.

    ![Beheer] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Beheer")

3.  Klik op **gebruikers, machtigingen en bedrijven**.

    ![Gebruikers, bedrijven en machtigingen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Gebruikers, bedrijven en machtigingen")

4.  Klik op **gebruiker toevoegen**.

    ![Gebruiker toevoegen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777685.png "Gebruiker toevoegen")

5.  Typ de gegevens van de Azure AD-account verstrekken in de volgende tekstvakken te in de sectie maken: **gebruikersnaam**, **e-mailadres**, **Voornaam**, **Achternaam**

    ![Maken] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777686.png "Maken")

6.  Klik op **maken**.

>[AZURE.NOTE] Kunt u een andere Jitbit Helpdesk gebruiker account hulpmiddelen voor het maken of API's geleverd door Jitbit Helpdesk bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-jitbit-helpdesk-perform-the-following-steps"></a>Gebruikers toewijzen aan de Jitbit Helpdesk, kunt u de volgende stappen uitvoeren:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Jitbit Helpdesk **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777687.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.