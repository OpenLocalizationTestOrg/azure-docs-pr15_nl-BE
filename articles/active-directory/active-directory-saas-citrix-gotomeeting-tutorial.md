<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Citrix GoToMeeting | Microsoft Azure" 
    description="Meer informatie over het Citrix GoToMeeting met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-citrix-gotomeeting"></a>Zelfstudie: Azure Active Directory-integratie met Citrix GoToMeeting  
Van toepassing op: Azure

>[AZURE.TIP]Voor feedback, klik [hier](http://go.microsoft.com/fwlink/?LinkId=522412).

Het doel van deze zelfstudie is de integratie van de Azure en Citrix GoToMeeting weergeven. Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder in Citrix GoToMeeting

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor Citrix GoToMeeting inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Configuratie] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768996.png "Configuratie")



##<a name="enabling-the-application-integration-for-citrix-gotomeeting"></a>De integratie van toepassingen voor Citrix GoToMeeting inschakelen

Het doel van deze sectie is een overzicht van de integratie van toepassingen voor Citrix GoToMeeting inschakelen.

###<a name="to-enable-the-application-integration-for-citrix-gotomeeting-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Citrix GoToMeeting, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de galerie toevoegen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749322.png "Toepassing van de galerie toevoegen")

6.  Typ in het **zoekvak**, **Citrix GoToMeeting**.

    ![Citrix GoToMeeting] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701006.png "Citrix GoToMeeting")

7.  **Citrix GoToMeeting**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Citrix GoToMeeting] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701012.png "Citrix GoToMeeting")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Citrix GoToMeeting met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat uploaden naar uw huurder Citrix GoToMeeting.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik op de pagina **Citrix GoToMeeting** application integration **eenmalige aanmelding configureren** om het dialoogvenster **SINGLE SIGN ON configureren** te openen.

    ![Eenmalige aanmelding inschakelen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768997.png "Eenmalige aanmelding inschakelen")

2.  Selecteer op de pagina **Hoe wilt u dat gebruikers aan te melden op Citrix GoToMeeting** **AD Azure Microsoft Single Sign-On**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768998.png "Eenmalige aanmelding configureren")


3. Klik op **volgende**op de pagina **Toepassingsinstellingen configureren** . 

    ![Eenmalige aanmelding inschakelen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC7689981.png "Eenmalige aanmelding inschakelen")

4.  Klik op **certificaat downloaden**en sla het bestand op uw computer op de pagina **configureren eenmalige aanmelding op Citrix GoToMeeting** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768999.png "Eenmalige aanmelding configureren")

5.  Log in op uw Citrix organisatie Center ([https://account.citrixonline.com/organization/administration/](https://account.citrixonline.com/organization/administration/)) in een ander browservenster.

6. Klik op het tabblad **Identity Provider** en voer de volgende stappen uit:  

    ![SAML-setup] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC6892321.png "SAML-setup")

    een. Selecteer **handmatig**

    
    b. In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding op Citrix GoToMeeting** de waarde van het **Teken In de URL** kopiëren en vervolgens plakken in het tekstvak **URL - in pagina** . 

    
    c. In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding op Citrix GoToMeeting** de waarde **Sign-Out de URL** kopiëren en vervolgens plakken in het tekstvak **URL van afmelden** .

    
    d. De **Entiteit-ID** -waarde kopiëren en vervolgens plakken in het tekstvak **Identity Provider entiteits-ID** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op Citrix GoToMeeting** .

   
    e. Als u wilt uw gedownloade certificaat uploaden, klikt u op **Certificaat uploaden**.

    
    f. Klik op **Opslaan**.

6.  Selecteer de bevestiging enkele aanmelding-configuratie op de Azure klassieke portal en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769000.png "Eenmalige aanmelding configureren")


7. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .

    ![SAML-setup] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC7689982.png "SAML-setup")





##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Het doel van deze sectie is het inschakelen van Active Directory-gebruikersaccounts met Citrix GoToMeeting ingericht overzicht.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Klik in de Azure klassieke portal op de pagina **Citrix GoToMeeting** toepassing integratie op **Gebruikersaanvragen configureren** om het dialoogvenster **Gebruikersaanvragen configureren** te openen.

    ![Gebruikersaanvragen configureren] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769001.png "Gebruikersaanvragen configureren")

2.  Op de pagina **instellingen en referenties van de beheerder** de volgende stappen uitvoeren:

    ![Gebruikersaanvragen configureren] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769002.png "Gebruikersaanvragen configureren")

    een. Typ in het tekstvak **Citrix GoToMeeting Admin gebruikersnaam** de gebruikersnaam van de beheerder.

    
    b. In het tekstvak **Citrix GoToMeeting Admin-wachtwoord** het wachtwoord van de beheerder.

    
    c. Klik op **volgende**.

3.  Klik op het selectievakje om de configuratie op te slaan op **de bevestigingspagina** .

4.  Klik op de knop **valideren** om te controleren of uw configuratie.


##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-citrix-gotomeeting-perform-the-following-steps"></a>Gebruikers toewijzen aan Citrix GoToMeeting, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Citrix GoToMeeting** application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769003.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC767830.png "Ja")

Nu wacht 10 minuten en u controleren of de account is gesynchroniseerd naar Dropbox voor bedrijven.

U kunt de Voorzieningsstatus controleren door te klikken op Dashboard in de D op de pagina **Citrix GoToMeeting** toepassing integratie op de klassieke Azure portal als een eerste controlestap.

![Dashboard] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769004.png "Dashboard")

Een cyclus van met succes zijn gebruikers wordt aangegeven door een bijbehorende status:

![Integratie-status] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769005.png "Integratie-status")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster.

Zie [Inleiding tot het Access-venster](https://msdn.microsoft.com/library/dn308586)voor meer informatie over het Access-venster.
