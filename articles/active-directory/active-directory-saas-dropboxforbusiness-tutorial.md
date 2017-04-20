<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Dropbox voor Business | Microsoft Azure" 
    description="Meer informatie over het synchronisatielocatie voor bedrijven met Azure Active Directory gebruiken om te schakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Zelfstudie: Azure Active Directory-integratie met Dropbox voor bedrijven
  
Het doel van deze zelfstudie is weer te geven van de integratie van de Azure en Dropbox voor bedrijven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder test in Dropbox voor bedrijven
  
Na het voltooien van deze zelfstudie bedrijf de Azure AD-gebruikers die u hebt toegewezen aan de synchronisatielocatie voor Business kunnen eenmalige in de toepassing op de synchronisatielocatie voor Business-site (service gestart aanmelden op) of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor de synchronisatielocatie voor bedrijven
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769508.png "Scenario")



##<a name="enabling-the-application-integration-for-dropbox-for-business"></a>Inschakelen van de integratie van toepassingen voor de synchronisatielocatie voor bedrijven
  
Het doel van deze sectie wordt aan de contour van het inschakelen van de integratie van toepassingen voor de synchronisatielocatie voor bedrijven.

###<a name="to-enable-the-application-integration-for-dropbox-for-business-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor de synchronisatielocatie voor bedrijven, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **synchronisatielocatie voor bedrijven**.

    ![Galerie van toepassing] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC701010.png "Galerie van toepassing")

7.  In het resultatendeelvenster **synchronisatielocatie voor bedrijf**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Dropbox voor bedrijven] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC701011.png "Dropbox voor bedrijven")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Dropbox voor bedrijven met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.

Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat uploaden naar je Dropbox voor Business huurder. Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, op de pagina **synchronisatielocatie voor Business** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749323.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat melden bij Dropbox voor zakelijke gebruikers** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749327.png "Eenmalige aanmelding configureren")

3.  Op de pagina **URL van App configureren** , moet u de volgende stappen uitvoeren:

    een. Aanmelding voor de synchronisatielocatie voor business huurder. 

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769509.png "Eenmalige aanmelding configureren")

    b. Klik in het navigatiedeelvenster aan de linkerzijde op **Admin-Console**. 

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769510.png "Eenmalige aanmelding configureren")

    c. Klik op **verificatie** in het linkernavigatievenster op de **Beheerconsole**. 

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769511.png "Eenmalige aanmelding configureren")

    d. Selecteer **eenmalige aanmelding inschakelen**in de sectie **eenmalige aanmelding** en klik op **meer** om deze sectie te vouwen.  

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769512.png "Eenmalige aanmelding configureren")

    e. Kopieer de URL naast **gebruikers kunnen aanmelden door hun e-mailadres invoeren of ze rechtstreeks naar kunnen gaan**. 

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769513.png "Eenmalige aanmelding configureren")

    f. Plak de URL in het tekstvak URL **synchronisatielocatie voor bedrijf aanmelden** op de Azure klassieke portal. 

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769514.png "Eenmalige aanmelding configureren")  



4. Klik op **certificaat downloaden**en sla het bestand op uw computer op de pagina **configureren eenmalige aanmelding op Dropbox voor bedrijven** .  

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769515.png "Eenmalige aanmelding configureren")


5. Op de synchronisatielocatie voor Business huurder, in de sectie **op** de pagina **verificatie** , moet u de volgende stappen uitvoeren: 

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "Eenmalige aanmelding configureren")

    een. Klik op **vereist**.

    b. De waarde van het **teken in de URL** kopiëren en vervolgens plakken in het tekstvak **URL aanmelden** in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding op Dropbox voor Business** .


    c. Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat. 

    > [AZURE.TIP] Zie [conversie van binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)voor meer informatie.


    d. Klik op de knop **"Kies certificaat"** en Ga naar het **Base64 - gecodeerd bestand**.


    e. Klik op de knop **'Wijzigingen opslaan'** om de configuratie op de synchronisatielocatie voor Business huurder te voltooien.


6. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten. 

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749329.png "Eenmalige aanmelding configureren")



##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikersaanvragen van gebruikersaccounts in Active Directory op Dropbox voor bedrijven.


### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1. Klik in de Azure klassieke Portal op de pagina **synchronisatielocatie voor zakelijke** toepassing integratie op **Gebruikersaanvragen configureren** om het dialoogvenster **Gebruikersaanvragen configureren** te openen.

2. De inschakelen gebruikersaanvragen voor de synchronisatielocatie voor zakelijke pagina, klik op Enable gebruikersaanvragen voor het aanmelden bij Dropbox te koppelen met Azure AD-dialoogvenster openen.  

    ![Gebruikersaanvragen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769517.png "Gebruikersaanvragen")

3. Log in voor de synchronisatielocatie voor Business huurder in het dialoogvenster **aanmelden bij Dropbox te koppelen met Azure Active Directory** . 

    ![Gebruikersaanvragen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769518.png "Gebruikersaanvragen")



4. Klik op **toestaan** als u wilt verlenen Azure AD toegang tot Dropbox. 

    ![Gebruikersaanvragen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769519.png "Gebruikersaanvragen")



5. Klik op de knop **Voltooien** om de configuratie.  

    ![Gebruikersaanvragen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769520.png "Gebruikersaanvragen")




##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-dropbox-for-business-perform-the-following-steps"></a>Gebruikers toewijzen aan Dropbox voor bedrijven, kunt u de volgende stappen uitvoeren:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de Integratiepagina van **Dropbox voor zakelijke **toepassing.

    ![Gebruikers toewijzen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769521.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC767830.png "Ja")
  


Nu wacht 10 minuten en u controleren of de account is gesynchroniseerd naar Dropbox voor bedrijven.

U kunt de Voorzieningsstatus controleren door te klikken op **Dashboard** in de **synchronisatielocatie voor zakelijke** toepassing Integratiepagina op de klassieke Azure Portal als een eerste controlestap.

![Gebruikers toewijzen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769522.png "Gebruikers toewijzen")


Een cyclus van met succes zijn gebruikers wordt aangegeven door een bijbehorende status.

![Gebruikers toewijzen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769523.png "Gebruikers toewijzen")


Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster.
Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.




## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)