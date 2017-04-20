<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Freshdesk | Microsoft Azure" 
    description="Meer informatie over het Freshdesk met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Zelfstudie: Azure Active Directory-integratie met Freshdesk
  
Het doel van deze zelfstudie is de integratie van de Azure en Freshdesk weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder Freshdesk
  
Na het voltooien van deze zelfstudie, is de Azure AD-gebruikers die u hebt toegewezen aan Freshdesk mogelijk voor één teken in de toepassing van de Freshdesk bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor Freshdesk inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-freshdesk-tutorial/IC776761.png "Scenario")
##<a name="enabling-the-application-integration-for-freshdesk"></a>De integratie van toepassingen voor Freshdesk inschakelen
  
Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor Freshdesk overzicht.

###<a name="to-enable-the-application-integration-for-freshdesk-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Freshdesk, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-freshdesk-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-freshdesk-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-freshdesk-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-freshdesk-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **Freshdesk**.

    ![Galerie van toepassing] (./media/active-directory-saas-freshdesk-tutorial/IC776762.png "Galerie van toepassing")

7.  **Freshdesk**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Freshdesk] (./media/active-directory-saas-freshdesk-tutorial/IC776763.png "Freshdesk")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Freshdesk met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Eenmalige aanmelding voor Freshdesk configureren, moet u een waarde uit een certificaat ophalen.  
Als u niet bekend met deze procedure bent, raadpleegt u [voor het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Freshdesk** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-freshdesk-tutorial/IC776764.png "Eenmalige aanmelding configureren")

2.  Op de pagina **Hoe wilt u dat gebruikers aanmelden bij Freshdesk** Selecteer **AD Azure Microsoft Single Sign-On**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-freshdesk-tutorial/IC776765.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App URL configureren** , in het tekstvak **Freshdesk teken In de URL** de URL van uw volgende patroon met ' https://*\<-naam van de huurder\>. Freshdesk.com*", en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-freshdesk-tutorial/IC776766.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op Freshdesk** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal als **c:\\Freshdesk.cer**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-freshdesk-tutorial/IC776767.png "Eenmalige aanmelding configureren")

5.  Log in op uw site Freshdesk bedrijf als beheerder in een ander web browser-venster.

6.  In het menu aan de bovenkant, klik op de **beheerder**.

    ![Admin] (./media/active-directory-saas-freshdesk-tutorial/IC776768.png "Admin")

7.  Klik op het tabblad **Algemene instellingen** op **beveiliging**.

    ![Beveiliging] (./media/active-directory-saas-freshdesk-tutorial/IC776769.png "Beveiliging")

8.  In de sectie **beveiliging** voert u de volgende stappen uit:

    ![Eenmalige aanmelding] (./media/active-directory-saas-freshdesk-tutorial/IC776770.png "Eenmalige aanmelding")

    1.  Selecteer **op**voor **Single Sign On (SSO)**.
    2.  Selecteer **SAML SSO**.
    3.  De waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in het tekstvak **SAML aanmeldings-URL** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op Freshdesk** .
    4.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Freshdesk** de waarde van de **Externe Logout URL** kopiëren en vervolgens plakken in het tekstvak **URL Logout** .
    5.  **De waarde** van het geëxporteerde certificaat kopiëren en plak deze in de textbox **Beveiliging certificaat vingerafdruk** .  

        >[AZURE.TIP]Zie voor meer informatie [hoe u kunt ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

    6.  Klik op **Opslaan**.

9.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-freshdesk-tutorial/IC776771.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij Freshdesk Azure AD, moeten zij worden ingericht in Freshdesk.  
Freshdesk is inrichten een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw huurder **Freshdesk** .

2.  In het menu aan de bovenkant, klik op de **beheerder**.

    ![Admin] (./media/active-directory-saas-freshdesk-tutorial/IC776772.png "Admin")

3.  Klik op het tabblad **Algemeen instellingen** **agenten**.

    ![Agenten] (./media/active-directory-saas-freshdesk-tutorial/IC776773.png "Agenten")

4.  Klik op **nieuwe Agent**.

    ![Nieuwe Agent] (./media/active-directory-saas-freshdesk-tutorial/IC776774.png "Nieuwe Agent")

5.  In het dialoogvenster met informatie over de volgende stappen uitvoeren:

    ![Informatie over] (./media/active-directory-saas-freshdesk-tutorial/IC776775.png "Informatie over")

    1.  Typ in het tekstvak **Volledige naam** de naam van de gewenste voorziening Azure AD-account.
    2.  Typ in het tekstvak **e-mailadres** het e-mailadres van Azure AD van de Azure AD-account die u verrichten wilt.
    3.  Typ in het tekstvak **titel** de titel van de Azure AD-account die u verrichten wilt.
    4.  **Agenten rol**te selecteren en klik vervolgens op **toewijzen**.
    5.  Klik op **Opslaan**.
    
        >[AZURE.NOTE] De accounthouder Azure AD krijgt een e-mail met daarin een link om te bevestigen de account voordat het wordt geactiveerd.

>[AZURE.NOTE] Kunt u een andere Freshdesk gebruiker account maken van hulpprogramma's of API's geleverd door Freshdesk bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-freshdesk-perform-the-following-steps"></a>Gebruikers toewijzen aan Freshdesk, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Freshdesk **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-freshdesk-tutorial/IC776776.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-freshdesk-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.