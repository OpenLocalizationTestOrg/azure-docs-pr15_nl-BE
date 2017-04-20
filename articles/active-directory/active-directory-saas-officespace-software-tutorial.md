<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met OfficeSpace Software | Microsoft Azure" 
    description="Meer informatie over het OfficeSpace Software met Azure Active Directory gebruiken om te schakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-officespace-software"></a>Zelfstudie: Azure Active Directory-integratie met OfficeSpace Software
  
Het doel van deze zelfstudie is de integratie van de Azure en OfficeSpace Software weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een OfficeSpace Software eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan OfficeSpace-Software kan worden met één teken in de toepassing van de OfficeSpace Software bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor OfficeSpace Software
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-officespace-software-tutorial/IC777764.png "Scenario")
##<a name="enabling-the-application-integration-for-officespace-software"></a>Inschakelen van de integratie van toepassingen voor OfficeSpace Software
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor OfficeSpace Software.

###<a name="to-enable-the-application-integration-for-officespace-software-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor OfficeSpace Software, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-officespace-software-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-officespace-software-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-officespace-software-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-officespace-software-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **OfficeSpace Software**.

    ![Galerie van toepassing] (./media/active-directory-saas-officespace-software-tutorial/IC777765.png "Galerie van toepassing")

7.  In het resultatendeelvenster **OfficeSpace Software**te selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![OfficeSpace Software] (./media/active-directory-saas-officespace-software-tutorial/IC781007.png "OfficeSpace Software")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij de OfficeSpace Software met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Configureren van eenmalige aanmelding voor OfficeSpace Software, moet u een waarde uit een certificaat ophalen.  
Als u niet bekend met deze procedure bent, raadpleegt u [voor het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **OfficeSpace Software** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige configureren op =] (./media/active-directory-saas-officespace-software-tutorial/IC777766.png "Eenmalige configureren op =")

2.  Op de pagina **Hoe wilt u dat gebruikers aan te melden op OfficeSpace Software** , selecteer **Microsoft Azure AD Single Sign-On**en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-officespace-software-tutorial/IC777767.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **URL van toepassing configureren** in het tekstvak **OfficeSpace Software teken op URL** de URL die wordt gebruikt door de gebruikers aan te melden op uw toepassing OfficeSpace (bijvoorbeeld: "*https://company.officespacesoftware.com*"), en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-officespace-software-tutorial/IC775556.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op OfficeSpace Software** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-officespace-software-tutorial/IC793769.png "Eenmalige aanmelding configureren")

5.  Log in op uw site OfficeSpace Software bedrijf als beheerder in een ander web browser-venster.

6.  Ga naar **Admin \> verbindingslijnen**.

    ![Admin] (./media/active-directory-saas-officespace-software-tutorial/IC777769.png "Admin")

7.  Klik op de **SAML-vergunning**.

    ![Verbindingslijnen] (./media/active-directory-saas-officespace-software-tutorial/IC777770.png "Verbindingslijnen")

8.  In de sectie **SAML-vergunning** , kunt u de volgende stappen uitvoeren:

    ![SAML-configuratie] (./media/active-directory-saas-officespace-software-tutorial/IC777771.png "SAML-configuratie")

    1.  De waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het tekstvak **url van de provider Logout** in de Azure klassieke portal op de dialoog pagina **configureren eenmalige aanmelding op OfficeSpace Software** .
    2.  In de Azure klassieke portal op de dialoog pagina **configureren eenmalige aanmelding op OfficeSpace Software** de **Externe Logout URL** -waarde kopiëren en vervolgens plakken in het tekstvak **Client idp de doel-url** .
    3.  **De waarde** van het geëxporteerde certificaat kopiëren en vervolgens plakken in de textbox **idp Client cert vingerafdruk** .  

        >[AZURE.TIP]
        Zie voor meer informatie [hoe u kunt ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

    4.  Klik op **Instellingen opslaan**.

9.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-officespace-software-tutorial/IC777772.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om Azure AD-gebruikers aan te melden bij de OfficeSpace-Software, moeten zij worden ingericht in OfficeSpace Software. OfficeSpace-Software is-aanbod een geautomatiseerde taak.  
Er is geen actie-item voor u.  
Gebruikers worden automatisch gemaakt als nodig tijdens de eerste single sign-on.

>[AZURE.NOTE]Kunt u een andere OfficeSpace Software gebruiker account hulpmiddelen voor het maken of API's die door OfficeSpace Software bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-officespace-software-perform-the-following-steps"></a>Gebruikers toewijzen aan OfficeSpace Software, moet u de volgende stappen uitvoeren:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op de pagina **OfficeSpace Software **application integration **gebruikers toewijzen**.

    ![Gebruikers toewijzen] (./media/active-directory-saas-officespace-software-tutorial/IC777773.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-officespace-software-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.