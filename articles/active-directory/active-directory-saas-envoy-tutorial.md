<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Envoy | Microsoft Azure" 
    description="Meer informatie over het Envoy met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-envoy"></a>Zelfstudie: Azure Active Directory-integratie met Envoy
  
Het doel van deze zelfstudie is de integratie van de Azure en Envoy weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder Envoy
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Envoy kan worden met één teken in de toepassing van de Envoy bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor Envoy inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-envoy-tutorial/IC776759.png "Scenario")
##<a name="enabling-the-application-integration-for-envoy"></a>De integratie van toepassingen voor Envoy inschakelen
  
Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor Envoy overzicht.

###<a name="to-enable-the-application-integration-for-envoy-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Envoy, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-envoy-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-envoy-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-envoy-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-envoy-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **Envoy**.

    ![Galerie van toepassing] (./media/active-directory-saas-envoy-tutorial/IC776760.png "Galerie van toepassing")

7.  In het resultatendeelvenster **Envoy**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Envoy] (./media/active-directory-saas-envoy-tutorial/IC776777.png "Envoy")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Envoy met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Configureren van eenmalige aanmelding voor Envoy, moet u een waarde uit een certificaat ophalen.  
Als u niet bekend met deze procedure bent, raadpleegt u [waarde van een certificaat ophalen](http://youtu.be/YKQF266SAxI)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Envoy** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding inschakelen] (./media/active-directory-saas-envoy-tutorial/IC776778.png "Eenmalige aanmelding inschakelen")

2.  Op de pagina **Hoe wilt u dat gebruikers aanmelden bij Envoy** **AD Azure Microsoft Single Sign-On**selecteren en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-envoy-tutorial/IC776779.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App URL configureren** , in het tekstvak **Envoy teken In de URL** de URL van uw volgende patroon met ' https://*\<-naam van de huurder\>. Envoy.com*", en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-envoy-tutorial/IC776780.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding bij Envoy** downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal als **c:\\Envoy.cer**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-envoy-tutorial/IC776781.png "Eenmalige aanmelding configureren")

5.  Log in op uw bedrijf Envoy site als beheerder in een ander web browser-venster.

6.  In de werkbalk op de bovenkant, klikt u op **Instellingen**.

    ![Envoy] (./media/active-directory-saas-envoy-tutorial/IC776782.png "Envoy")

7.  Klik op **bedrijf**.

    ![Bedrijf] (./media/active-directory-saas-envoy-tutorial/IC776783.png "Bedrijf")

8.  Klik op **SAML**.

    ![SAML] (./media/active-directory-saas-envoy-tutorial/IC776784.png "SAML")

9.  Voer de volgende stappen uit in de configuratiesectie van **SAML-verificatie** :

    ![SAML-verificatie] (./media/active-directory-saas-envoy-tutorial/IC776785.png "SAML-verificatie")

    >[AZURE.NOTE] De waarde voor de HQ locatie-ID wordt automatisch gegenereerd door de toepassing.

    1.  **De waarde** van het geëxporteerde certificaat kopiëren en vervolgens plakken in het tekstvak **vingerafdruk** .  

        >[AZURE.TIP] Zie voor meer informatie [hoe u kunt ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

    2.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Envoy** de waarde van **SAML SSO-URL** kopiëren en plak deze in de textbox **Identity Provider HTTP SAML-URL** .
    3.  Klik op **wijzigingen opslaan**.

10. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-envoy-tutorial/IC776786.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Er is geen actie-item aan Envoy van gebruikers configureren.  
Wanneer een toegewezen gebruiker probeert aan te melden bij Envoy met behulp van het paneel toegang, Envoy wordt gecontroleerd of de gebruiker bestaat.  
Als er nog geen gebruikersaccount beschikbaar, wordt deze automatisch gemaakt door Envoy.
##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-envoy-perform-the-following-steps"></a>Gebruikers toewijzen aan Envoy, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Envoy **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-envoy-tutorial/IC776787.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-envoy-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.