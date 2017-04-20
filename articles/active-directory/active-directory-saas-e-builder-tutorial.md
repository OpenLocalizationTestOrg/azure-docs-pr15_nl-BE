<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met e-Builder | Microsoft Azure" 
    description="Meer informatie over het e-Builder met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-e-builder"></a>Zelfstudie: Azure Active Directory-integratie met e-Builder
  
Het doel van deze zelfstudie is de integratie van de Azure en e-Builder weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder e Builder
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan een e-Builder kan worden met één teken in de toepassing van de e-Builder bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor e-Builder inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-e-builder-tutorial/IC777378.png "Scenario")
##<a name="enabling-the-application-integration-for-e-builder"></a>De integratie van toepassingen voor e-Builder inschakelen
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor e-Builder.

###<a name="to-enable-the-application-integration-for-e-builder-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor e-Builder, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-e-builder-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-e-builder-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-e-builder-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-e-builder-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **e Builder**.

    ![Galerie van toepassing] (./media/active-directory-saas-e-builder-tutorial/IC777379.png "Galerie van toepassing")

7.  Selecteer **e-Builder**in het resultatenvenster en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![e-Builder] (./media/active-directory-saas-e-builder-tutorial/IC777380.png "e-Builder")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij e-Builder met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, op de pagina **e-Builder** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-e-builder-tutorial/IC777381.png "Eenmalige aanmelding configureren")

2.  Op de pagina **Hoe wilt u dat gebruikers aanmelden bij e-Builder** Selecteer **AD Azure Microsoft Single Sign-On**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-e-builder-tutorial/IC777382.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App URL configureren** , in het tekstvak **e-Builder aanmelden URL** de URL van uw gebruik van het volgende patroon "*https://\<-naam van de huurder\>CF,o.e Builder.com*", en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-e-builder-tutorial/IC777383.png "App-URL configureren")

4.  Op de **Configure eenmalige aanmelding op e-Builder** pagina voor het downloaden van de metagegevens **metagegevens worden gedownload**en vervolgens de gegevens lokaal opslaan als **c:\\e BuilderMetaData.xml**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-e-builder-tutorial/IC777384.png "Eenmalige aanmelding configureren")

5.  Doorsturen naar e-Builder metagegevensbestand ondersteuningsteam. Het team ondersteuning wordt geconfigureerd voor eenmalige aanmelding voor u.

6.  Selecteer de Configuratiebevestiging van één aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-e-builder-tutorial/IC777385.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Er is geen actie-item naar e-Builder van gebruikers configureren.  
Wanneer een toegewezen gebruiker probeert aan te melden bij de e-Builder met behulp van het paneel toegang, e-Builder wordt gecontroleerd of de gebruiker bestaat.  
Als er nog geen gebruikersaccount beschikbaar, wordt deze automatisch gemaakt door e-Builder.
##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-e-builder-perform-the-following-steps"></a>Gebruikers toewijzen aan e-Builder, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **e-Builder **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-e-builder-tutorial/IC777386.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-e-builder-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
