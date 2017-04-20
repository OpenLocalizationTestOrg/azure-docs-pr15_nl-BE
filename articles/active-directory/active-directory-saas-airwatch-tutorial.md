<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met AirWatch | Microsoft Azure" 
    description="Meer informatie over het AirWatch met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-airwatch"></a>Zelfstudie: Azure Active Directory-integratie met AirWatch

Het doel van deze zelfstudie is de integratie van de Azure en AirWatch weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een AirWatch eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, is de Azure AD-gebruikers die u hebt toegewezen aan AirWatch mogelijk voor één teken in de toepassing van de AirWatch bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor AirWatch inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![AirWatch] (./media/active-directory-saas-airwatch-tutorial/IC791913.png "AirWatch")
##<a name="enabling-the-application-integration-for-airwatch"></a>De integratie van toepassingen voor AirWatch inschakelen

Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor AirWatch overzicht.

###<a name="to-enable-the-application-integration-for-airwatch-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor AirWatch, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-airwatch-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-airwatch-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-airwatch-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-airwatch-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **AirWatch**.

    ![Galerie van toepassing] (./media/active-directory-saas-airwatch-tutorial/IC791914.png "Galerie van toepassing")

7.  In het resultatendeelvenster **AirWatch**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![AirWatch] (./media/active-directory-saas-airwatch-tutorial/IC791915.png "AirWatch")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij AirWatch met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat-bestand te maken.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **AirWatch** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-airwatch-tutorial/IC791916.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aanmelden bij AirWatch** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-airwatch-tutorial/IC791917.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App URL configureren** , in het tekstvak **AirWatch teken op URL** de URL die wordt gebruikt door de gebruikers aan te melden uw toepassing AirWatch (bijv.: "*https:// companycode.awmdm.com/AirWatch/Login?gid=companycode*"), en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-airwatch-tutorial/IC791918.png "App-URL configureren")

4.  Klik op **certificaat downloaden**en sla het bestand op uw computer op de pagina **configureren eenmalige aanmelding op AirWatch** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-airwatch-tutorial/IC791919.png "Eenmalige aanmelding configureren")

5.  Log in op uw site AirWatch bedrijf als beheerder in een ander web browser-venster.

6.  Klik op **Accounts**in het linkernavigatievenster en klikt u op **beheerders**.

    ![Beheerders] (./media/active-directory-saas-airwatch-tutorial/IC791920.png "Beheerders")

7.  Vouw het menu **Instellingen** en klik vervolgens op **Directory Services**.

    ![Instellingen] (./media/active-directory-saas-airwatch-tutorial/IC791921.png "Instellingen")

8.  Klik op het tabblad **gebruiker** , in het tekstveld **Base DN** , typt u de domeinnaam van uw en klik op **Opslaan**.

    ![Gebruiker] (./media/active-directory-saas-airwatch-tutorial/IC791922.png "Gebruiker")

9.  Klik op het tabblad **Server** .

    ![Server] (./media/active-directory-saas-airwatch-tutorial/IC791923.png "Server")

10. Voer de volgende stappen uit:

    ![Uploaden] (./media/active-directory-saas-airwatch-tutorial/IC791924.png "Uploaden")

    1.  Als het **Type map**, selecteert u **geen**.
    2.  Selecteer **SAML voor verificatie gebruiken**.
    3.  Het gedownloade certificaat uploaden, klikt u op **uploaden**.

11. Voer de volgende stappen uit in de sectie **aanvragen** :

    ![Aanvragen] (./media/active-directory-saas-airwatch-tutorial/IC791925.png "Aanvragen")

    1.  Selecteer als **Type Binding aanvraag**, **boeken**.
    2.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Airwatch** de **Single Sign-On Service URL** -waarde kopiëren en vervolgens plakken in **Identity Provider één teken op URL** textbox.
    3.  Selecteer **Indeling NameID**, **E-mailadres**.
    4.  Klik op **Opslaan**.

12. Klik nogmaals op het tabblad **gebruiker** .

    ![Gebruiker] (./media/active-directory-saas-airwatch-tutorial/IC791926.png "Gebruiker")

13. Voer de volgende stappen uit in de sectie **kenmerk** :

    ![Kenmerk] (./media/active-directory-saas-airwatch-tutorial/IC791927.png "Kenmerk")

    1.  Typ in het tekstvak **Object-id** **http://schemas.microsoft.com/identity/claims/objectidentifier**.
    2.  Typ in het tekstvak **gebruikersnaam** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    3.  Typ in het tekstvak **Weergegeven naam** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    4.  Typ in het tekstvak **naam van de eerste** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    5.  Typ **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**in het tekstvak **Achternaam** .
    6.  Typ in het tekstvak **e-mailadres** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    7.  Klik op **Opslaan**.

14. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-airwatch-tutorial/IC791928.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Om gebruikers aan te melden bij AirWatch Azure AD, moeten zij worden ingericht in AirWatch.  
AirWatch is inrichten een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder uw bedrijf **AirWatch** site.

2.  Klik op **Accounts**in het navigatiedeelvenster aan de linkerkant en klik op **gebruikers**.

    ![Gebruikers] (./media/active-directory-saas-airwatch-tutorial/IC791929.png "Gebruikers")

3.  Klik op **Lijst weergeven**in het menu **gebruikers** en klik vervolgens op **Add \> gebruiker toevoegen**.

    ![Gebruiker toevoegen] (./media/active-directory-saas-airwatch-tutorial/IC791930.png "Gebruiker toevoegen")

4.  In het dialoogvenster **toevoegen / bewerken van de gebruiker** de volgende stappen uitvoeren:

    ![Gebruiker toevoegen] (./media/active-directory-saas-airwatch-tutorial/IC791931.png "Gebruiker toevoegen")

    1.  Typ de **gebruikersnaam**, **wachtwoord**, **Bevestig het wachtwoord**, **Voornaam**, **Achternaam**, **E-mailadres** van een geldige Azure Active Directory-account verrichten in de verwante tekstvakken te.
    2.  Klik op **Opslaan**.

>[AZURE.NOTE] Kunt u een andere AirWatch gebruiker account maken van hulpprogramma's of API's geleverd door AirWatch bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-airwatch-perform-the-following-steps"></a>Gebruikers toewijzen aan AirWatch, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **AirWatch **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-airwatch-tutorial/IC791932.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-airwatch-tutorial/IC767830.png "Ja")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
