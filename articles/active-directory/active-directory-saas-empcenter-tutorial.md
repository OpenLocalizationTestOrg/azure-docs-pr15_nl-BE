<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met EmpCenter | Microsoft Azure" 
    description="Meer informatie over het EmpCenter met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-empcenter"></a>Zelfstudie: Azure Active Directory-integratie met EmpCenter
  
Het doel van deze zelfstudie is de integratie van de Azure en EmpCenter weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een EmpCenter eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan EmpCenter kan worden naar één teken in de toepassing van de EmpCenter bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor EmpCenter
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-empcenter-tutorial/IC802916.png "Scenario")
##<a name="enabling-the-application-integration-for-empcenter"></a>Inschakelen van de integratie van toepassingen voor EmpCenter
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor EmpCenter.

###<a name="to-enable-the-application-integration-for-empcenter-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor EmpCenter, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-empcenter-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-empcenter-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-empcenter-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-empcenter-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **EmpCenter**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-empcenter-tutorial/IC802917.png "Galerie van toepassing")

7.  **EmpCenter**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![EmpCentral] (./media/active-directory-saas-empcenter-tutorial/IC802918.png "EmpCentral")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij EmpCenter met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **EmpCenter** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-empcenter-tutorial/IC802919.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op EmpCenter** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-empcenter-tutorial/IC802920.png "Eenmalige aanmelding configureren")

3.  Voer de volgende stappen uit op de pagina **Toepassingsinstellingen configureren** :

    ![App-instellingen configureren] (./media/active-directory-saas-empcenter-tutorial/IC802921.png "App-instellingen configureren")

    1.  In het **Teken op URL** -tekstvak typt u de URL die wordt gebruikt door gebruikers voor aanmelding bij uw toepassing EmpCenter (bv.: *https://partner-authenticati.empcenter.com/workforce/SSO.do*).
    2.  Klik op **volgende**

4.  Op de pagina **configureren eenmalige aanmelding bij EmpCenter** downloaden van de metagegevens op **metagegevens downloaden**en sla het bestand op uw computer met metagegevens.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-empcenter-tutorial/IC802922.png "Eenmalige aanmelding configureren")

5.  De van het gedownloade metagegevensbestand verzenden naar uw EmpCenter support team.

    >[AZURE.NOTE] Uw EmpCenter support team heeft te maken van de werkelijke SSO-configuratie.
U krijgt een melding wanneer de eenmalige aanmelding is ingeschakeld voor uw abonnement.

6.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-empcenter-tutorial/IC802923.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij EmpCenter Azure AD, moeten zij worden ingericht in EmpCenter.  
EmpCenter moeten de gebruikersaccounts worden gemaakt door uw EmpCenter support team.

>[AZURE.NOTE] U kunt andere EmpCenter gebruiker account hulpmiddelen voor het maken of API's geleverd door EmpCenter bepaling Azure Active Directory-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-empcenter-perform-the-following-steps"></a>Gebruikers toewijzen aan EmpCenter, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **EmpCenter **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-empcenter-tutorial/IC802924.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-empcenter-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.