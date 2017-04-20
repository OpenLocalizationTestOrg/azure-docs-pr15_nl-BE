<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met kruipen dicht | Microsoft Azure" 
    description="Leren kruipen dicht gebruiken met Azure Active Directory inschakelen voor eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-huddle"></a>Zelfstudie: Azure Active Directory-integratie met kruipen dicht
  
Het doel van deze zelfstudie is de integratie van de Azure en kruipen dicht weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een kruipen dicht eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan het kruipen dicht kan worden met één teken in de toepassing van de site in kruipen dicht bedrijf (service gestart aanmelden op) of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor kruipen dicht inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Eenmalige aanmelding configureren] (./media/active-directory-saas-huddle-tutorial/IC787830.png "Eenmalige aanmelding configureren")
##<a name="enabling-the-application-integration-for-huddle"></a>De integratie van toepassingen voor kruipen dicht inschakelen
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor kruipen dicht.

###<a name="to-enable-the-application-integration-for-huddle-perform-the-following-steps"></a>Schakel in de integratie van toepassingen voor kruipen dicht door de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-huddle-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-huddle-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-huddle-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-huddle-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak**, **kruipen dicht**.

    ![Galerie van toepassing] (./media/active-directory-saas-huddle-tutorial/IC787831.png "Galerie van toepassing")

7.  In het resultatendeelvenster **kruipen dicht**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Kruipen dicht] (./media/active-directory-saas-huddle-tutorial/IC787832.png "Kruipen dicht")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij het kruipen dicht bij hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina application integration **kruipen dicht** **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-huddle-tutorial/IC787833.png "Eenmalige aanmelding configureren")

2.  Op de pagina **Hoe wilt u gebruikers aan te melden op kruipen dicht** Selecteer **AD Azure Microsoft Single Sign-On**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-huddle-tutorial/IC787834.png "Eenmalige aanmelding configureren")

3.  Typ de URL van uw huurder kruipen dicht met het volgende patroon '*http://company.huddle.com*' op de pagina **Configureren App-URL** in het tekstvak **Teken kruipen dicht op URL** en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-huddle-tutorial/IC787835.png "App-URL configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding op kruipen dicht** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-huddle-tutorial/IC787836.png "Eenmalige aanmelding configureren")

    1.  Klik op **certificaat downloaden**en sla het bestand op uw computer.
    2.  Kopieer de waarde van de **URL van de uitgevende instelling** , de **SAML SSO-URL** -waarde en het gedownloade certificaat en verzend deze naar het ondersteuningsteam kruipen dicht.

    >[AZURE.NOTE] Eenmalige aanmelding moet worden ingeschakeld door het ondersteuningsteam kruipen dicht.
U krijgt een melding wanneer de configuratie is voltooid.

5.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-huddle-tutorial/IC787837.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij het kruipen dicht Azure AD, moeten zij worden ingericht in kruipen dicht.  
Kruipen dicht is inrichten een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder uw bedrijf **kruipen dicht** site.

2.  Klik op **werkruimte**.

3.  Klik op **mensen \> personen uitnodigen**.

    ![Mensen] (./media/active-directory-saas-huddle-tutorial/IC787838.png "Mensen")

4.  Voer de volgende stappen uit in de sectie **een nieuwe uitnodiging maken** :

    ![Nieuwe uitnodiging] (./media/active-directory-saas-huddle-tutorial/IC787839.png "Nieuwe uitnodiging")

    1.  Selecteer in de lijst **Kies een team mensen uitnodigen lid te** **team**.
    2.  Typ het **E-mailadres** van een geldige AAD account te verrichten in het bijbehorende tekstvak.
    3.  Klik op **uitnodigen**.

    >[AZURE.NOTE] De accounthouder Azure AD ontvangt een e-mail met een koppeling naar de account bevestigen voordat deze actief zijn.

>[AZURE.NOTE] U kunt een andere kruipen dicht gebruiker account hulpmiddelen voor het maken of API's die worden geleverd door kruipen dicht naar AAD de voorziening gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-huddle-perform-the-following-steps"></a>Gebruikers toewijzen aan kruipen dicht, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina toepassing integratie **kruipen dicht **.

    ![Gebruikers toewijzen] (./media/active-directory-saas-huddle-tutorial/IC787840.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-huddle-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.