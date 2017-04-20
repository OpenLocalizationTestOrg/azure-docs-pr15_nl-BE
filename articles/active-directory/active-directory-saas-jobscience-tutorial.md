<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Jobscience | Microsoft Azure" 
    description="Meer informatie over het Jobscience met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-jobscience"></a>Zelfstudie: Azure Active Directory-integratie met Jobscience
  
Het doel van deze zelfstudie is de integratie van de Azure en Jobscience weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een abonnement Jobscience Single Sign-On ingeschakeld
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Jobscience kan worden naar één teken in de toepassing van de Jobscience bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Jobscience
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-jobscience-tutorial/IC784341.png "Scenario")
##<a name="enabling-the-application-integration-for-jobscience"></a>Inschakelen van de integratie van toepassingen voor Jobscience
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Jobscience.

###<a name="to-enable-the-application-integration-for-jobscience-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Jobscience, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-jobscience-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-jobscience-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-jobscience-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-jobscience-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **jobscience**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-jobscience-tutorial/IC784342.png "Galerie van toepassing")

7.  **Jobscience**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Jobscience] (./media/active-directory-saas-jobscience-tutorial/IC784357.png "Jobscience")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Jobscience met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Configureren van eenmalige aanmelding voor Jobscience moet u een waarde uit een certificaat ophalen.  
Als u niet bekend met deze procedure bent, raadpleegt u [voor het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Log in op uw site Jobscience bedrijf als beheerder.

2.  Ga naar **Instellingen**.

    ![Setup] (./media/active-directory-saas-jobscience-tutorial/IC784358.png "Setup")

3.  In het linkernavigatievenster in de sectie **beheren** op **Domain Management** om de bijbehorende sectie weer te geven en klik vervolgens op **Mijn domein** Open de pagina **Mijn domein** . 

    ![Mijn domein] (./media/active-directory-saas-jobscience-tutorial/IC767825.png "Mijn domein")

4.  Zorg ervoor dat het in "**Stap 4 beschikbaar stelt aan gebruikers**" is om te controleren dat uw domein goed ingesteld zijn is, en bekijk uw '**Mijn domeininstellingen**'.

    ![Domein gebruiker geïmplementeerd] (./media/active-directory-saas-jobscience-tutorial/IC784377.png "Domein gebruiker geïmplementeerd")

5.  In een ander web browser-venster aanmelden bij uw klassieke Azure portal.

6.  Klik op de pagina **Jobscience** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-jobscience-tutorial/IC784360.png "Eenmalige aanmelding configureren")

7.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Jobscience** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-jobscience-tutorial/IC784361.png "Eenmalige aanmelding configureren")

8.  Typ de URL met het volgende patroon '*http://company.my.salesforce.com*' op de pagina **Configureren App-URL** in het tekstvak **Jobscience teken In de URL** en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-jobscience-tutorial/IC784362.png "App-URL configureren")

9.  Op de pagina **configureren eenmalige aanmelding op Jobscience** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-jobscience-tutorial/IC784363.png "Eenmalige aanmelding configureren")

10. Op de site van het bedrijf Jobscience, klikt u op de **Besturingselementen voor beveiliging**en klik vervolgens op **Instellingen voor eenmalige aanmelding**.

    ![Besturingselementen voor beveiliging] (./media/active-directory-saas-jobscience-tutorial/IC784364.png "Besturingselementen voor beveiliging")

11. Voer de volgende stappen uit in de sectie **Instellingen voor eenmalige aanmelding** :

    ![Instellingen voor eenmalige aanmelding] (./media/active-directory-saas-jobscience-tutorial/IC781026.png "Instellingen voor eenmalige aanmelding")

    1.  Selecteer **SAML ingeschakeld**.
    2.  Klik op **Nieuw**.

12. In het dialoogvenster **SAML Single Sign-On instelling bewerken** kunt u de volgende stappen uitvoeren:

    ![SAML Single Sign-On-instelling] (./media/active-directory-saas-jobscience-tutorial/IC784365.png "SAML Single Sign-On-instelling")

    1.  Typ in het tekstvak **naam** een naam voor uw configuratie.
    2.  In de Azure klassieke portal op de dialoog pagina **configureren eenmalige aanmelding bij Jobscience** de **Uitgever URL** -waarde kopiëren en vervolgens plakken in het tekstvak van de **uitgevende instelling**
    3.  Typ in het tekstvak **Entiteits-Id** **https://salesforce-jobscience.com**
    4.  Klik op **Bladeren** om te uploaden van uw certificaat Azure AD.
    5.  Selecteren **Identiteitstype SAML** **Assertion bevat de ID van de Federatie van het gebruikersobject**.
    6.  Als **Locatie van SAML-identiteit**, selecteer **identiteit is in het NameIdentfier-element van het onderwerp Overzicht**.
    7.  In de Azure klassieke portal op de dialoog pagina **configureren eenmalige aanmelding bij Jobscience** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in de textbox **Identity Provider aanmeldings-URL**
    8.  In de Azure klassieke portal op de dialoog pagina **configureren eenmalige aanmelding bij Jobscience** de **Externe Logout URL** -waarde kopiëren en vervolgens plakken in **Identity Provider Logout URL** textbox
    9.  Klik op **Opslaan**.

13. In het linkernavigatievenster in de sectie **beheren** op **Domain Management** om de bijbehorende sectie weer te geven en klik vervolgens op **Mijn domein** Open de pagina **Mijn domein** . 

    ![Mijn domein] (./media/active-directory-saas-jobscience-tutorial/IC767825.png "Mijn domein")

14. Klik op **bewerken**op de pagina **Mijn domein** in de sectie **Aanmelden pagina huisstijl** .

    ![Aanmeldingspagina huismerk] (./media/active-directory-saas-jobscience-tutorial/IC767826.png "Aanmeldingspagina huismerk")

15. De naam van de **SAML SSO-instellingen** op de pagina **Login pagina huismerk** in het gedeelte **Authentication Service** wordt weergegeven. Selecteer deze en klik vervolgens op **Opslaan**.

    ![Aanmeldingspagina huismerk] (./media/active-directory-saas-jobscience-tutorial/IC784366.png "Aanmeldingspagina huismerk")

16. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-jobscience-tutorial/IC784367.png "Eenmalige aanmelding configureren")
  
De SP Klik gestarte eenmalige aanmelding aanmeldings-URL op de **Instellingen voor eenmalige aanmelding** in de sectie **Besturingselementen voor beveiliging** -menu.

![Besturingselementen voor beveiliging] (./media/active-directory-saas-jobscience-tutorial/IC784368.png "Besturingselementen voor beveiliging")
  
Klik op de SSO-profiel dat u hebt gemaakt in de stap hierboven.  
Deze pagina bevat de eenmalige op URL voor uw bedrijf (bv. *https://companyname.my.salesforce.com?so=companyid*).
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij Jobscience Azure AD, moeten zij worden ingericht in Jobscience.  
Bij Jobscience is het inrichten van een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder uw bedrijf **Jobscience** site.

2.  Ga naar instellingen

    ![Setup] (./media/active-directory-saas-jobscience-tutorial/IC784358.png "Setup")

3.  Ga naar **voor het beheren van gebruikers \> gebruikers**.

    ![Gebruikers] (./media/active-directory-saas-jobscience-tutorial/IC784369.png "Gebruikers")

4.  Klik op **nieuwe gebruiker**.

    ![Alle gebruikers] (./media/active-directory-saas-jobscience-tutorial/IC784370.png "Alle gebruikers")

5.  Voer de volgende stappen uit in het dialoogvenster **Gebruiker bewerken** :

    ![Gebruiker bewerken] (./media/active-directory-saas-jobscience-tutorial/IC784371.png "Gebruiker bewerken")

    1.  Typ de voornaam, achternaam, alias, e-mailadres, naam en bijnaam eigenschappen voor gebruiker van de gewenste voorziening in de verwante tekstvakken Azure AD-gebruiker.
    2.  Klik op **Opslaan**.

    >[AZURE.NOTE] De accounthouder Azure AD krijgt een e-mail met daarin een link om te bevestigen de account voordat het wordt geactiveerd.

>[AZURE.NOTE] Kunt u een andere Jobscience gebruiker account maken van hulpprogramma's of API's geleverd door Jobscience bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-jobscience-perform-the-following-steps"></a>Gebruikers toewijzen aan Jobscience, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Jobscience **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-jobscience-tutorial/IC784372.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-jobscience-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.