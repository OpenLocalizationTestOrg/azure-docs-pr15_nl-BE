<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met PolicyStat | Microsoft Azure" 
    description="Meer informatie over het PolicyStat met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-policystat"></a>Zelfstudie: Azure Active Directory-integratie met PolicyStat
  
Het doel van deze zelfstudie is de integratie van de Azure en PolicyStat weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder PolicyStat
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan PolicyStat kan worden naar één teken in de toepassing van de PolicyStat bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor PolicyStat
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-policystat-tutorial/IC808662.png "Scenario")
##<a name="enabling-the-application-integration-for-policystat"></a>Inschakelen van de integratie van toepassingen voor PolicyStat
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor PolicyStat.

###<a name="to-enable-the-application-integration-for-policystat-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor PolicyStat, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-policystat-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-policystat-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-policystat-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-policystat-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **PolicyStat**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-policystat-tutorial/IC808627.png "Galerie van toepassing")

7.  **PolicyStat**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![PolicyStat] (./media/active-directory-saas-policystat-tutorial/IC810430.png "PolicyStat")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij PolicyStat met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Uw toepassing PolicyStat verwacht de SAML-bevestigingen in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de **saml-token kenmerken** .  
De volgende schermafbeelding ziet u een voorbeeld voor dit.

![Kenmerken] (./media/active-directory-saas-policystat-tutorial/IC808628.png "Kenmerken")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **PolicyStat** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-policystat-tutorial/IC808629.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op PolicyStat** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-policystat-tutorial/IC808630.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **Toepassingsinstellingen configureren** , in het tekstvak **Teken op URL** de URL die door uw gebruikers om aanmelding bij de toepassing van de URL PolicyStat wordt gebruikt (bijvoorbeeld: *"https://demo-azure.policystat.com"*), en klik op **volgende**.

    ![App-instellingen configureren] (./media/active-directory-saas-policystat-tutorial/IC808631.png "App-instellingen configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij PolicyStat** op **metagegevens downloaden**en sla het bestand op uw computer met metagegevens.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-policystat-tutorial/IC808632.png "Eenmalige aanmelding configureren")

5.  Log in op uw site PolicyStat bedrijf als beheerder in een ander web browser-venster.

6.  Klik op het tabblad **beheer** en klik in het linkernavigatievenster op **Single Sign-On-configuratie** .

    ![Menu beheerder] (./media/active-directory-saas-policystat-tutorial/IC808633.png "Menu beheerder")

7.  Selecteer in de sectie **Instellingen** **inschakelen Single Sign-on integratie**.

    ![Single Sign-On-configuratie] (./media/active-directory-saas-policystat-tutorial/IC808634.png "Single Sign-On-configuratie")

8.  **Kenmerken configureren**op en voer de volgende stappen uit in de sectie **Kenmerken configureren** :

    ![Single Sign-On-configuratie] (./media/active-directory-saas-policystat-tutorial/IC808635.png "Single Sign-On-configuratie")

    1.  Typ in het tekstvak **Kenmerk Username** **uid**.
    2.  Typ in het tekstvak **Kenmerk voornaam** **Voornaam**.
    3.  Typ in het tekstvak **Laatste kenmerk met de naam** **lastname**.
    4.  Typ in het tekstvak **E-kenmerk** **emailaddress**.
    5.  Klik op **wijzigingen opslaan**.

9.  **Uw IDP metagegevens**op en voer de volgende stappen uit in de sectie **Metagegevens voor uw IDP** :

    ![Single Sign-On-configuratie] (./media/active-directory-saas-policystat-tutorial/IC808635.png "Single Sign-On-configuratie")

    1.  Uw gedownloade metagegevensbestand openen, de inhoud kopiëren en plak deze in de **Metagegevens van de Provider van uw identiteit** textbox
    2.  Klik op **wijzigingen opslaan**.

10. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-policystat-tutorial/IC771723.png "Eenmalige aanmelding configureren")

11. 12. In het menu aan de bovenkant, klikt u op **kenmerken** om de **SAML-Token kenmerken** -dialoogvenster te openen.

    ![Kenmerken] (./media/active-directory-saas-policystat-tutorial/IC795920.png "Kenmerken")

13. Als u wilt de toewijzingen vereist kenmerk toevoegen, moet u de volgende stappen uitvoeren:

    ![Kenmerken] (./media/active-directory-saas-policystat-tutorial/IC804823.png "Kenmerken")

    1.  Klik op **gebruikerskenmerk toevoegen**.
    2.  Typ in het tekstvak **Naam van kenmerk** **uid**.
    3.  Selecteer in het tekstvak **Waarde van het kenmerk** **ExtractMailPrefix()**.
    4.  Selecteer in de lijst **e-mailberichten** , **User.mail**.
    5.  Klik op **Voltooien**.
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij PolicyStat Azure AD, moeten zij worden ingericht in PolicyStat.  
PolicyStat ondersteunt alleen in tijd van gebruikersaanvragen. Dit betekent dat, u hoeft niet handmatig de gebruikers toevoegen aan PolicyStat.  
De gebruikers krijgen toegevoegd automatisch op hun eerste aanmelding via één teken op.

>[AZURE.NOTE]Kunt u een andere PolicyStat gebruiker account maken van hulpprogramma's of API's geleverd door PolicyStat bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-policystat-perform-the-following-steps"></a>Gebruikers toewijzen aan PolicyStat, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **PolicyStat **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-policystat-tutorial/IC808636.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-policystat-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.