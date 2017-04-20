<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Work.com | Microsoft Azure" 
    description="Meer informatie over het Work.com met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-workcom"></a>Zelfstudie: Azure Active Directory-integratie met Work.com
  
Het doel van deze zelfstudie is de integratie van de Azure en Work.com weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een Work.com eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie toewijzen AAD gebruikers aan wie u hebt Work.com toegang zal worden tot één teken in de toepassing van de Work.com bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor Work.com inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-work-com-tutorial/IC794105.png "Scenario")

##<a name="enabling-the-application-integration-for-workcom"></a>De integratie van toepassingen voor Work.com inschakelen
  
Het doel van deze sectie is het inschakelen van de integratie van toepassingen voor Work.com overzicht.

###<a name="to-enable-the-application-integration-for-workcom-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Work.com, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-work-com-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-work-com-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-work-com-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-work-com-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **Work.com**.

    ![Galerie van toepassing] (./media/active-directory-saas-work-com-tutorial/IC794106.png "Galerie van toepassing")

7.  In het resultatendeelvenster **Work.com**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Work.com] (./media/active-directory-saas-work-com-tutorial/IC794107.png "Work.com")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Work.com met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure moet u een certificaat uploaden naar Work.com.com.

>[AZURE.NOTE] Als u eenmalige aanmelding configureert, moet u setup een aangepaste Work.com domeinnaam nog. U moet ten minste een domeinnaam opgeven, uw domeinnaam te testen en implementeren op uw gehele organisatie.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Log in op uw huurder Work.com als beheerder.

2.  Ga naar **Instellingen**.

    ![Setup] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")

3.  In het linkernavigatievenster in de sectie **beheren** op **Domain Management** om de bijbehorende sectie weer te geven en klik vervolgens op **Mijn domein** Open de pagina **Mijn domein** . 

    ![Mijn domein] (./media/active-directory-saas-work-com-tutorial/IC767825.png "Mijn domein")

4.  Zorg ervoor dat het in "**Stap 4 beschikbaar stelt aan gebruikers**" is om te controleren dat uw domein goed ingesteld zijn is, en bekijk uw '**Mijn domeininstellingen**'.

    ![Domein gebruiker geïmplementeerd] (./media/active-directory-saas-work-com-tutorial/IC784377.png "Domein gebruiker geïmplementeerd")

5.  In een ander web browser-venster aanmelden bij uw klassieke Azure portal.

6.  Klik op de pagina **Work.com **application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-work-com-tutorial/IC794109.png "Eenmalige aanmelding configureren")

7.  Op de pagina **Hoe wilt u dat gebruikers aanmelden bij Work.com** Selecteer **AD Azure Microsoft Single Sign-On**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-work-com-tutorial/IC794110.png "Eenmalige aanmelding configureren")

8.  Typ op de pagina **URL van App configureren** , in het tekstvak **Work.com teken op URL** de URL die wordt gebruikt door de gebruikers aan te melden op uw toepassing Work.com (bijvoorbeeld: " *http://company.my.salesforce.com*"), en klik op **volgende**: 

    ![App-URL configureren] (./media/active-directory-saas-work-com-tutorial/IC794111.png "App-URL configureren")

9.  Op de pagina **configureren eenmalige aanmelding op Work.com** om te downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-work-com-tutorial/IC794112.png "Eenmalige aanmelding configureren")

10. Log in op uw huurder Work.com.

11. Ga naar **Instellingen**.

    ![Setup] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")

12. Vouw het menu **Beveiliging besturingselementen** en klik vervolgens op **Instellingen voor eenmalige aanmelding**.

    ![Instellingen voor eenmalige aanmelding] (./media/active-directory-saas-work-com-tutorial/IC794113.png "Instellingen voor eenmalige aanmelding")

13. Voer de volgende stappen uit op de pagina van het dialoogvenster **Instellingen voor eenmalige aanmelding** :

    ![SAML ingeschakeld] (./media/active-directory-saas-work-com-tutorial/IC781026.png "SAML ingeschakeld")

    1.  Selecteer **SAML ingeschakeld**.
    2.  Klik op **Nieuw**.

14. Voer de volgende stappen uit in de sectie **SAML Single Sign-On-instellingen** :

    ![SAML Single Sign-On-instelling] (./media/active-directory-saas-work-com-tutorial/IC794114.png "SAML Single Sign-On-instelling")

    1.  Typ in het tekstvak **naam** een naam voor uw configuratie.  

        >[AZURE.NOTE] Die een waarde voor **naam** , het tekstvak voor de **API** automatisch ingevuld.

    2.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Work.com** de **Uitgever URL** -waarde kopiëren en vervolgens plakken in het tekstvak van de **uitgevende instelling** .
    3.  Het gedownloade certificaat uploaden, klikt u op **Bladeren**.
    4.  Typ in het tekstvak **Entiteits-Id** **https://salesforce-work.com**.
    5.  Selecteren **Identiteitstype SAML** **Assertion bevat de ID van de Federatie van het gebruikersobject**.
    6.  Als **Locatie van SAML-identiteit**, selecteer **identiteit is in het NameIdentfier-element van het onderwerp Overzicht**.
    7.  De waarde van de **Externe aanmeldings-URL** kopiëren en vervolgens plakken in de textbox **Identity Provider aanmeldings-URL** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op Work.com** .
    8.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Work.com** de waarde van de **Externe Logout URL** kopiëren en plak deze in de **Identity Provider Logout URL** textbox.
    9.  Schakel **HTTP Post** **Service Provider gestart aanvragen Binding**.
    10. Klik op **Opslaan**.

15. In de klassieke portal van Work.com in het linkernavigatievenster op **Domain Management** om de bijbehorende sectie weer te geven en klik vervolgens op **Mijn domein** Open de pagina **Mijn domein** . 

    ![Mijn domein] (./media/active-directory-saas-work-com-tutorial/IC794115.png "Mijn domein")

16. Klik op **bewerken**op de pagina **Mijn domein** in de sectie **Aanmelden pagina huisstijl** .

    ![Aanmeldingspagina huismerk] (./media/active-directory-saas-work-com-tutorial/IC767826.png "Aanmeldingspagina huismerk")

17. De naam van de **SAML SSO-instellingen** op de pagina **Login pagina huismerk** in het gedeelte **Authentication Service** wordt weergegeven. Selecteer deze en klik vervolgens op **Opslaan**.

    ![Aanmeldingspagina huismerk] (./media/active-directory-saas-work-com-tutorial/IC784366.png "Aanmeldingspagina huismerk")

18. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-work-com-tutorial/IC794116.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Azure Active Directory-gebruikers moeten zich aanmelden, als ze worden ingericht naar Work.com.  
Work.com is inrichten een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Meld u aan uw site Work.com bedrijf aan als beheerder.

2.  Ga naar **Instellingen**.

    ![Setup] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")

3.  Ga naar **voor het beheren van gebruikers \> gebruikers**.

    ![Gebruikers beheren] (./media/active-directory-saas-work-com-tutorial/IC784369.png "Gebruikers beheren")

4.  Klik op **nieuwe gebruiker**.

    ![Alle gebruikers] (./media/active-directory-saas-work-com-tutorial/IC794117.png "Alle gebruikers")

5.  Voer de volgende stappen uit in de sectie gebruiker bewerken:

    ![Gebruiker bewerken] (./media/active-directory-saas-work-com-tutorial/IC794118.png "Gebruiker bewerken")

    1.  Typ de **Achternaam**, **Alias**, **e-mailadres**, **gebruikersnaam** en kenmerken van de **bijnaam** van een geldige Azure Active Directory-account verrichten in de verwante tekstvakken te.
    2.  Selecteer de **rol**, **licentie** en **profiel**.
    3.  Klik op **Opslaan**.  

        >[AZURE.NOTE] De accounthouder Azure Active Directory krijgt een e-mailbericht met een koppeling naar de account bevestigen voordat deze actief wordt.

>[AZURE.NOTE] Kunt u een andere Work.com gebruiker account maken van hulpprogramma's of API's geleverd door Work.com bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-workcom-perform-the-following-steps"></a>Gebruikers toewijzen aan Work.com, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina Work.com application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-work-com-tutorial/IC794119.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-work-com-tutorial/IC767830.png "Ja")
  
U moet nu wacht 10 minuten en controleren of de account is gesynchroniseerd met de Work.com.com.
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.