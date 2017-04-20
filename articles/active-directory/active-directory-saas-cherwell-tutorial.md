<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Cherwell | Microsoft Azure" 
    description="Meer informatie over het Cherwell met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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
    ms.date="10/14/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-cherwell"></a>Zelfstudie: Azure Active Directory-integratie met Cherwell

Het doel van deze zelfstudie is de integratie van de Azure en Cherwell weergeven. Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een Cherwell eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, is de Azure AD-gebruikers die u hebt toegewezen aan de Cherwell mogelijk voor één teken in de toepassing van de Cherwell bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor Cherwell inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-cherwell-tutorial/IC798988.png "Scenario")
##<a name="enabling-the-application-integration-for-cherwell"></a>De integratie van toepassingen voor Cherwell inschakelen

Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor Cherwell overzicht.

###<a name="to-enable-the-application-integration-for-cherwell-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Cherwell, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-cherwell-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-cherwell-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-cherwell-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-cherwell-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **Cherwell**.

    ![Cherwell] (./media/active-directory-saas-cherwell-tutorial/IC798989.png "Cherwell")

7.  **Cherwell**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

    ![Cherwell](./media/active-directory-saas-cherwell-tutorial/IC798996.png "Cherwell")

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Cherwell met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Cherwell** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-cherwell-tutorial/IC798990.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Cherwell** **AD Azure Microsoft Single Sign-On**selecteren en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-cherwell-tutorial/IC798991.png "Eenmalige aanmelding configureren")

3.  Op de pagina **URL van App configureren** , moet u de volgende stappen uitvoeren:

    ![App-URL configureren] (./media/active-directory-saas-cherwell-tutorial/IC798992.png "App-URL configureren")

    een.  In het **Teken op URL** -tekstvak typt u de URL die wordt gebruikt door de gebruikers wilt aanmelden bij uw **Cherwell** (bv.: *https://\<bedrijf\>.cherwellondemand.com/cherwellclient*).

    b.  Klik op **volgende**

4.  Op de pagina **configureren eenmalige aanmelding bij Cherwell** voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-cherwell-tutorial/IC798993.png "Eenmalige aanmelding configureren")

    een.  Klik op **certificaat downloaden**en sla het certificaat lokaal op uw computer.

    b.  Kopieer de **URL van de Provider identiteit**.

    c.  Kopieer de **URL van de Service voor eenmalige aanmelding**.

    d.  Klik op **volgende**.

5.  Het gedownloade certificaat, de **URL van de Provider identiteit** en de **Single Sign-On Service-URL** van uw ondersteuningsteam Cherwell indienen.

    >[AZURE.NOTE] Het ondersteuningsteam Cherwell is de werkelijke SSO-configuratie uitvoeren.
U krijgt een melding wanneer de eenmalige aanmelding is ingeschakeld voor uw abonnement.

6.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-cherwell-tutorial/IC798994.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Om gebruikers aan te melden bij Cherwell Azure AD, moeten zij worden ingericht in Cherwell.  
Cherwell moeten de gebruikersaccounts worden gemaakt door het ondersteuningsteam Cherwell.

>[AZURE.NOTE] U kunt een andere Cherwell gebruikers account hulpmiddelen voor het maken of API's die Cherwell bepaling Azure Active Directory-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-cherwell-perform-the-following-steps"></a>Gebruikers toewijzen aan Cherwell, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Cherwell** application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-cherwell-tutorial/IC798995.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-cherwell-tutorial/IC767830.png "Ja")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
