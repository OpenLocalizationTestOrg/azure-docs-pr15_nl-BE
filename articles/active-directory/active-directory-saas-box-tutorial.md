<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met vak | Microsoft Azure" 
    description="Meer informatie over het vak met Azure Active Directory gebruiken om te schakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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




#<a name="tutorial-azure-active-directory-integration-with-box"></a>Zelfstudie: Azure Active Directory-integratie met vak


  
Het doel van deze zelfstudie is de integratie van de Azure en het vak weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder test in het vak
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan het vak kan worden met één teken in de toepassing in het vak bedrijf site (service gestart aanmelden op) of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor inschakelen
2.  Eenmalige aanmelding configureren
3.  Configureren van de gebruiker en groep inrichten
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-box-tutorial/IC769537.png "Scenario")



##<a name="enabling-the-application-integration-for-box"></a>De integratie van toepassingen voor inschakelen
  
Het doel van deze sectie is voor het inschakelen van de integratie van toepassingen voor een overzicht.

###<a name="to-enable-the-application-integration-for-box-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor het vak, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-box-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-box-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-box-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-box-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak**, **vak**.

    ![Galerie van toepassing] (./media/active-directory-saas-box-tutorial/IC701023.png "Galerie van toepassing")

7.  Schakel **in**en klik op **Voltooien** als de toepassing wilt toevoegen in het resultatendeelvenster.

    ![Vak] (./media/active-directory-saas-box-tutorial/IC701024.png "Vak")



##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij een vak met hun account in Azure AD federation op basis van de SAML-protocol gebruiken. Als onderdeel van deze procedure moet u metagegevens uploaden naar Box.com.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **vak** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-box-tutorial/IC769538.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aanmelden bij het vak** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-box-tutorial/IC769539.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **URL van App configureren** in het tekstvak **URL van de huurder in** uw URL vak huurder (bijvoorbeeld: https://<mydomainname>. box.com), en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-box-tutorial/IC669826.png "App-URL configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding in het vak** downloaden van uw metagegevens **metagegevens downloaden**en vervolgens het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-box-tutorial/IC669824.png "Eenmalige aanmelding configureren")

5.  Naar voren dat metagegevensbestand naar vak ondersteuningsteam. Het team ondersteuning wordt geconfigureerd voor eenmalige aanmelding voor u.

6.  Selecteer de Configuratiebevestiging van één aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-box-tutorial/IC769540.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Het doel van deze sectie is aan de contour van het inschakelen van Active Directory-gebruikersaccounts aan het inrichten.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1. Klik in de Azure klassieke portal op de pagina **vak** toepassing integratie op **Gebruikersaanvragen configureren** om het dialoogvenster **Gebruikersaanvragen configureren** te openen. 

    ![Inschakelen Automatische gebruikersaanvragen] (./media/active-directory-saas-box-tutorial/IC769541.png "Inschakelen Automatische gebruikersaanvragen")

2. Klik op de pagina **gebruiker ingericht naar vak** dialoogvenster **Gebruikersaanvragen inschakelen**. 

    ![Inschakelen Automatische gebruikersaanvragen] (./media/active-directory-saas-box-tutorial/IC769544.png "Inschakelen Automatische gebruikersaanvragen")

3. De vereiste referenties opgeven op de pagina **aanmelden toegang verlenen tot het vak** en klik op **machtigen**. 

    ![Inschakelen Automatische gebruikersaanvragen] (./media/active-directory-saas-box-tutorial/IC769546.png "Inschakelen Automatische gebruikersaanvragen")


4. Klik op **toegang verlenen tot het vak** toe te staan deze bewerking en terug te keren naar de klassieke Azure portal. 

    ![Inschakelen Automatische gebruikersaanvragen] (./media/active-directory-saas-box-tutorial/IC769549.png "Inschakelen Automatische gebruikersaanvragen")


5. Op de pagina **Configuratieopties** kunt de **Objecttypen bepaling** selectievakjes u aangeven of groepsobjecten naar vak naast gebruikersobjecten zijn ingericht.  Zie 'Toewijzen sectie gebruikers en groepen' hieronder voor meer informatie.


6. Als u klaar bent met de configuratie, klikt u op voltooien. 

    ![Inschakelen Automatische gebruikersaanvragen] (./media/active-directory-saas-box-tutorial/IC769551.png "Inschakelen Automatische gebruikersaanvragen")



##<a name="assigning-a-test-user"></a>Een testgebruiker toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-box-perform-the-following-steps"></a>Gebruikers toewijzen aan een vak, voert u de volgende stappen uit:

1. In de klassieke Azure portal, een testaccount te maken.

2. Klik op de pagina **vak **toepassing integratie op **gebruikers toewijzen**. 

    ![Gebruikers toewijzen] (./media/active-directory-saas-box-tutorial/IC769552.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing. 

    ![Ja] (./media/active-directory-saas-box-tutorial/IC767830.png "Ja")
  
U moet nu wacht 10 minuten en controleren of de account is gesynchroniseerd met het vak.

U kunt de Voorzieningsstatus controleren door te klikken op Dashboard in de D op de pagina vak toepassing integratie op de klassieke Azure portal als een eerste controlestap.

![Dashboard] (./media/active-directory-saas-box-tutorial/IC769553.png "Dashboard")

Een cyclus van met succes zijn gebruikers wordt aangegeven door een bijbehorende status:

![Integratie-status] (./media/active-directory-saas-box-tutorial/IC769555.png "Integratie-status")


Gesynchroniseerde gebruikers worden weergegeven in uw huurder vak onder **Gebruikers beheerd** in de **Beheerconsole**.

![Integratie-status] (./media/active-directory-saas-box-tutorial/IC769556.png "Integratie-status")


##<a name="assigning-users-and-groups"></a>Gebruikers en groepen toewijzen

De **vak > gebruikers en groepen** in Azure klassieke portal op het tabblad kunt u opgeven welke gebruikers en groepen toegang moeten worden verleend aan. Toewijzing van een gebruiker of groep heeft tot gevolg dat de volgende taken uitvoeren:

* Azure AD staat de toegewezen gebruiker (hetzij door rechtstreekse of groepslidmaatschap) om het vak te verifiëren. Als een gebruiker niet is toegewezen, Azure AD het niet mogelijk om een handtekening vak en retourneert een fout op de aanmeldingspagina van Azure AD.

* Een tegel app voor vak wordt toegevoegd aan de gebruiker [toepassingen starten](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

* Als automatisch inrichten is ingeschakeld, worden de toegewezen gebruikers en/of groepen toegevoegd aan de wachtrij inrichtingsproces automatisch worden ingericht.

    * Als alleen gebruikersobjecten zijn geconfigureerd om te worden ingericht, vervolgens alle gebruikers direct toegewezen in de provisioning wachtrij worden geplaatst en alle gebruikers die lid van de toegewezen groepen zijn in de provisioning wachtrij wordt geplaatst. 
    
    * Als een groepsobjecten zijn geconfigureerd om te worden ingericht, worden alle toegewezen groepsobjecten ingericht voor zowel in als alle gebruikers die lid zijn van deze groepen. De groeps- en lidmaatschappen blijven behouden bij het vak wordt geschreven.
    
U kunt de **kenmerken > Single Sign-On** tabblad configureren welke gebruikerskenmerken (of claims) worden gepresenteerd aan tijdens de verificatie op basis van SAML en de **kenmerken > Provisioning** tabblad configureren hoe gebruikers- en groepskenmerken laten doorlopen van Azure AD vak tijdens het inrichten van bewerkingen. Zie de onderstaande bronnen voor meer informatie.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Vorderingen in het SAML-token verleend aanpassen](active-directory-saml-claims-customization.md)
* [Aanbod: Kenmerktoewijzingen aanpassen](active-directory-saas-customizing-attribute-mappings.md)
* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)
