<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Brightspace door Desire2Learn | Microsoft Azure" 
    description="Meer informatie over het Brightspace door Desire2Learn met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-brightspace-by-desire2learn"></a>Zelfstudie: Azure Active Directory-integratie met Brightspace door Desire2Learn

Het doel van deze zelfstudie is de integratie van de Azure en Brightspace door Desire2Learn weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een Brightspace door eenmalige aanmelding Desire2Learn abonnement ingeschakeld

Na het voltooien van deze zelfstudie, is de Azure AD-gebruikers die u hebt toegewezen aan Brightspace door Desire2Learn mogelijk voor één teken in de toepassing op uw Brightspace door Desire2Learn bedrijf site (service gestart aanmelden op) of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor Brightspace door Desire2Learn inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798957.png "Scenario")
##<a name="enabling-the-application-integration-for-brightspace-by-desire2learn"></a>De integratie van toepassingen voor Brightspace door Desire2Learn inschakelen

Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor Brightspace door Desire2Learn overzicht.

###<a name="to-enable-the-application-integration-for-brightspace-by-desire2learn-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Brightspace door Desire2Learn, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **Brightspace door Desire2Learn**.

    ![Galerie Apllication] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798958.png "Galerie Apllication")

7.  **Brightspace door Desire2Learn**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Brightspace door Desire2Learn] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC799321.png "Brightspace door Desire2Learn")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Brightspace door Desire2Learn met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Brightspace door Desire2Learn** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798959.png "Eenmalige aanmelding configureren")

2.  Op de pagina **Hoe wilt u dat gebruikers aan te melden op Brightspace door Desire2Learn** Selecteer **AD Azure Microsoft Single Sign-On**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798960.png "Eenmalige aanmelding configureren")

3.  Op de pagina **URL van App configureren** , moet u de volgende stappen uitvoeren:

    ![App-URL configureren] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798961.png "App-URL configureren")

    1.  Typ in het tekstvak **Teken op URL** de URL die wordt gebruikt door de gebruikers wilt aanmelden bij uw **Brightspace door Desire2Learn** (bijvoorbeeld: *https://partnershowcase.desire2learn.com/Shibboleth.sso/Login?entityID=https://sts.windows-ppe.net/5caf9349-fd93-4a74-b064-0070f65bfb49/&target=https%3A%2F%2Fpartnershowcase.desire2learn.com%2Fd2l%2FshibbolethSSO%2Faspinfo.asp*).
    2.  Klik op **volgende**

4.  Klik op de pagina **configureren eenmalige aanmelding op Brightspace door Desire2Learn** downloaden van de metagegevens op **metagegevens downloaden**en vervolgens de metagegevens opslaan op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798962.png "Eenmalige aanmelding configureren")

5.  De van het gedownloade metagegevensbestand verzenden naar uw Brightspace door Desire2Learn support team.

    >[AZURE.NOTE] Uw Brightspace door Desire2Learn support team heeft te maken van de werkelijke SSO-configuratie.
U krijgt een melding wanneer de eenmalige aanmelding is ingeschakeld voor uw abonnement.

6.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798963.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Om gebruikers aan te melden bij Brightspace door Desire2Learn AD Azure, moeten zij in Brightspace worden ingericht door Desire2Learn.  
Brightspace door Desire2Learn moeten de gebruikersaccounts worden gemaakt door de Brightspace door Desire2Learn support team.

>[AZURE.NOTE] Kunt u een andere Brightspace door Desire2Learn gebruiker account maken van hulpprogramma's of API's die Brightspace door Desire2Learn bepaling Azure Active Directory-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-brightspace-by-desire2learn-perform-the-following-steps"></a>Gebruikers toewijzen aan Brightspace door Desire2Learn, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Brightspace door Desire2Learn **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798964.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC767830.png "Ja")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
