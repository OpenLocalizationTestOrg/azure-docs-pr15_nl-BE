<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met UserVoice | Microsoft Azure" 
    description="Meer informatie over het UserVoice met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-uservoice"></a>Zelfstudie: Azure Active Directory-integratie met UserVoice
  
Het doel van deze zelfstudie is de integratie van de Azure en UserVoice weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder UserVoice
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan de UserVoice kan worden met één teken in de toepassing van de UserVoice bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor UserVoice inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-uservoice-tutorial/IC777514.png "Scenario")

##<a name="enabling-the-application-integration-for-uservoice"></a>De integratie van toepassingen voor UserVoice inschakelen
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor UserVoice.

###<a name="to-enable-the-application-integration-for-uservoice-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor UserVoice, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-uservoice-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-uservoice-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-uservoice-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-uservoice-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **UserVoice**.

    ![Galerie van toepassing] (./media/active-directory-saas-uservoice-tutorial/IC777513.png "Galerie van toepassing")

7.  **UserVoice**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![UserVoice] (./media/active-directory-saas-uservoice-tutorial/IC777810.png "UserVoice")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij UserVoice met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Configureren van eenmalige aanmelding voor UserVoice, moet u een waarde uit een certificaat ophalen.  
Als u niet bekend met deze procedure bent, raadpleegt u [voor het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **UserVoice** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-uservoice-tutorial/IC777515.png "Eenmalige aanmelding configureren")

2.  Op de pagina **Hoe wilt u dat gebruikers aanmelden bij UserVoice** Selecteer **AD Azure Microsoft Single Sign-On**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-uservoice-tutorial/IC777516.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App URL configureren** , in het tekstvak **UserVoice teken In de URL** de URL van uw volgende patroon met ' https://*\<-naam van de huurder\>. UserVoice.com*", en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-uservoice-tutorial/IC777517.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding op UserVoice** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal als **c:\\UserVoice.cer**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-uservoice-tutorial/IC777518.png "Eenmalige aanmelding configureren")

5.  Log in op uw site UserVoice bedrijf als beheerder in een ander web browser-venster.

6.  In de werkbalk op de bovenkant, klikt u op instellingen en webportal selecteert in het menu.

    ![Instellingen] (./media/active-directory-saas-uservoice-tutorial/IC777519.png "Instellingen")

7.  Klik op **bewerken** om het dialoogvenster **Verificatie bewerken** pagina te openen op het tabblad **webportal** in de sectie **gebruikersverificatie**

    ![Webportal] (./media/active-directory-saas-uservoice-tutorial/IC777520.png "Webportal")

8.  Klik op de pagina van het dialoogvenster **Verificatie bewerken** kunt u de volgende stappen uitvoeren:

    ![Verificatie van de gebruiker bewerken] (./media/active-directory-saas-uservoice-tutorial/IC777521.png "Verificatie van de gebruiker bewerken")

    1.  Klik op **Eenmalige aanmelding (SSO)**.
    2.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij UserVoice** de waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in de textbox **Extern aanmelden eenmalige aanmelding** .
    3.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij UserVoice** de waarde van de **Externe Logout URL** kopiëren en plak deze in de **SSO-externe Sign-Out textbox**.
    4.  **De waarde** van het geëxporteerde certificaat kopiëren en vervolgens plakken in het tekstvak **huidige certificaat SHA1-vingerafdruk** .  

        >[AZURE.TIP] Zie voor meer informatie [hoe u kunt ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

    5.  Klik op **verificatie-instellingen opslaan**.

9.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-uservoice-tutorial/IC777522.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij UserVoice Azure AD, moeten zij worden ingericht in UserVoice.  
UserVoice is inrichten een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw huurder **UserVoice** .

2.  Ga naar **Instellingen**.

    ![Instellingen] (./media/active-directory-saas-uservoice-tutorial/IC777811.png "Instellingen")

3.  Klik op **Algemeen**.

4.  Klik op **machtigingen en agenten**.

    ![Agenten en machtigingen] (./media/active-directory-saas-uservoice-tutorial/IC777812.png "Agenten en machtigingen")

5.  Klik op **beheerders toevoegen**.

    ![Beheerders toevoegen] (./media/active-directory-saas-uservoice-tutorial/IC777813.png "Beheerders toevoegen")

6.  In het dialoogvenster **uitnodigen, beheerders** , kunt u de volgende stappen uitvoeren:

    ![Beheerders uitnodigen] (./media/active-directory-saas-uservoice-tutorial/IC777814.png "Beheerders uitnodigen")

    1.  Typ het e-mailadres van de account die u wilt verrichten, en klik vervolgens op **toevoegen**in de texbox e-mailberichten.
    2.  Klik op **uitnodigen**.

>[AZURE.NOTE] Kunt u een andere UserVoice gebruiker account maken van hulpprogramma's of API's geleverd door UserVoice bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-uservoice-perform-the-following-steps"></a>Gebruikers toewijzen aan UserVoice, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **UserVoice **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-uservoice-tutorial/IC777523.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-uservoice-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.