<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met SmarterU | Microsoft Azure" 
    description="Meer informatie over het SmarterU met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-smarteru"></a>Zelfstudie: Azure Active Directory-integratie met SmarterU
  
Het doel van deze zelfstudie is de integratie van de Azure en SmarterU weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder SmarterU
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan SmarterU kan worden naar één teken in de toepassing van de SmarterU bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor SmarterU
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-smarteru-tutorial/IC777320.png "Scenario")

##<a name="enabling-the-application-integration-for-smarteru"></a>Inschakelen van de integratie van toepassingen voor SmarterU
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor SmarterU.

###<a name="to-enable-the-application-integration-for-smarteru-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor SmarterU, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-smarteru-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-smarteru-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-smarteru-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-smarteru-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **SmarterU**in het **zoekvak**.

    ![Toepassing fallery] (./media/active-directory-saas-smarteru-tutorial/IC777321.png "Toepassing fallery")

7.  **SmarterU**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![SmarterU] (./media/active-directory-saas-smarteru-tutorial/IC777322.png "SmarterU")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij SmarterU met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **SmarterU** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-smarteru-tutorial/IC777323.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op SmarterU** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-smarteru-tutorial/IC777324.png "Eenmalige aanmelding configureren")

3.  Op de **Configure eenmalige aanmelding op SmarterU** pagina voor het downloaden van de metagegevens **metagegevens worden gedownload**en vervolgens de gegevens lokaal opslaan als **c:\\SmarterUMetaData.cer**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-smarteru-tutorial/IC777325.png "Eenmalige aanmelding configureren")

4.  Log in op uw site SmarterU bedrijf als beheerder in een ander web browser-venster.

5.  In de werkbalk op de bovenkant, klikt u op **Accountinstellingen**.

    ![Accountinstellingen] (./media/active-directory-saas-smarteru-tutorial/IC777326.png "Accountinstellingen")

6.  Op de pagina account configuratie voert u de volgende stappen uit:

    ![Externe autorisatie] (./media/active-directory-saas-smarteru-tutorial/IC777327.png "Externe autorisatie")

    1.  Selecteer **externe verificatie inschakelen**.
    2.  In de sectie **Model Login Control** , selecteer het tabblad **SmarterU** .
    3.  Selecteer het tabblad **SmarterU** in de sectie **Standaard gebruikersaanmelding** .
    4.  Selecteer **Okta inschakelen**.
    5.  De inhoud van de van het gedownloade metagegevensbestand kopiëren en vervolgens plakken in het tekstvak **Okta metagegevens** .
    6.  Klik op **Opslaan**.

7.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-smarteru-tutorial/IC777328.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij SmarterU Azure AD, moeten zij worden ingericht in SmarterU.  
Bij SmarterU is het inrichten van een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw huurder **SmarterU** .

2.  Ga naar **gebruikers**.

3.  In de sectie gebruiker, voert u de volgende stappen uit:

    ![Nieuwe gebruiker] (./media/active-directory-saas-smarteru-tutorial/IC777329.png "Nieuwe gebruiker")

    1.  Klik op **+ User**.
    2.  Typ in de volgende tekstvakken de waarden gerelateerde kenmerk van de gebruikersaccount Azure AD: **primaire E-mail**, **Werknemer-ID**, **wachtwoord**, **Bevestig het wachtwoord**, **naam**, **Achternaam**.
    3.  Klik op **actief**.
    4.  Klik op **Opslaan**.

>[AZURE.NOTE] Kunt u een andere SmarterU gebruiker account maken van hulpprogramma's of API's geleverd door SmarterU bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-smarteru-perform-the-following-steps"></a>Gebruikers toewijzen aan SmarterU, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **SmarterU **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-smarteru-tutorial/IC777330.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-smarteru-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.