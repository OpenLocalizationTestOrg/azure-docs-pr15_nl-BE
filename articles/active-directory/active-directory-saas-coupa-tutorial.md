<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Coupa | Microsoft Azure" 
    description="Meer informatie over het Coupa met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-coupa"></a>Zelfstudie: Azure Active Directory-integratie met Coupa

Het doel van deze zelfstudie is de integratie van de Azure en Coupa weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een Coupa eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan Coupa kan worden naar één teken in de toepassing met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de integratie van toepassingen voor Coupa
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-coupa-tutorial/IC791897.png "Scenario")
##<a name="enabling-the-application-integration-for-coupa"></a>Inschakelen van de integratie van toepassingen voor Coupa

Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Coupa.

###<a name="to-enable-the-application-integration-for-coupa-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Coupa, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-coupa-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-coupa-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-coupa-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-coupa-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ **Coupa**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-coupa-tutorial/IC791898.png "Galerie van toepassing")

7.  **Coupa**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Coupa] (./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Coupa met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Configureren van eenmalige aanmelding voor Coupa moet u een waarde uit een certificaat ophalen.  
Als u niet bekend met deze procedure bent, raadpleegt u [voor het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Meld u aan uw site Coupa bedrijf aan als beheerder.

2.  Ga naar **Setup \> beveiliging**.

    ![Besturingselementen voor beveiliging] (./media/active-directory-saas-coupa-tutorial/IC791900.png "Besturingselementen voor beveiliging")

3.  De Coupa metagegevens om bestand te downloaden naar uw computer, klikt u op **Download en SP-metagegevens importeren**.

    ![Coupa SP-metagegevens] (./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP-metagegevens")

4.  In een ander venster, moet u zich aanmelden bij de Azure klassieke portal.

5.  Klik op de pagina **Coupa** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-coupa-tutorial/IC791902.png "Eenmalige aanmelding configureren")

6.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aan te melden op Coupa** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-coupa-tutorial/IC791903.png "Eenmalige aanmelding configureren")

7.  Op de pagina **URL van App configureren** , moet u de volgende stappen uitvoeren:

    ![App-URL configureren] (./media/active-directory-saas-coupa-tutorial/IC791904.png "App-URL configureren")

    1.  Typ in het tekstvak **Teken op URL** URL die wordt gebruikt door de gebruikers aan te melden op uw toepassing Coupa (bijvoorbeeld: "*http://company.Coupa.com*").
    2.  Open het gedownloade bestand met metagegevens Coupa en kopieert u de **index AssertionConsumerService/URL**.
    3.  Plak de **AssertionConsumerService index/URL** -waarde in het tekstvak **URL van Coupa antwoord** .
    4.  Klik op **volgende**.

8.  Op de pagina **configureren eenmalige aanmelding bij Coupa** op **metagegevens downloaden**uw metagegevensbestand te downloaden en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-coupa-tutorial/IC791905.png "Eenmalige aanmelding configureren")

9.  Ga naar op de site van het bedrijf Coupa **Setup \> beveiliging**.

    ![Besturingselementen voor beveiliging] (./media/active-directory-saas-coupa-tutorial/IC791900.png "Besturingselementen voor beveiliging")

10. In de sectie **aanmelden met de referenties van de Coupa van** de volgende stappen uitvoeren:

    ![Meld u aan met de referenties van de Coupa] (./media/active-directory-saas-coupa-tutorial/IC791906.png "Meld u aan met de referenties van de Coupa")

    1.  Selecteer **aanmelden met SAML**.
    2.  Klik op **Bladeren** om de gedownloade Azure Active metagegevens-bestand te uploaden.
    3.  Klik op **Opslaan**.

11. Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-coupa-tutorial/IC791907.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren

Om gebruikers aan te melden bij Coupa Azure AD, moeten zij worden ingericht in Coupa.  
Bij Coupa is het inrichten van een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Configureren van gebruiker wordt ingericht, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder uw bedrijf **Coupa** site.

2.  In het menu aan de bovenkant, klikt u op **Instellingen**en klik vervolgens op **gebruikers**.

    ![Gebruikers] (./media/active-directory-saas-coupa-tutorial/IC791908.png "Gebruikers")

3.  Klik op **maken**.

    ![Gebruikers maken] (./media/active-directory-saas-coupa-tutorial/IC791909.png "Gebruikers maken")

4.  Voer de volgende stappen uit in de sectie **Gebruiker maken** :

    ![Gebruikerdetails] (./media/active-directory-saas-coupa-tutorial/IC791910.png "Gebruikerdetails")

    1.  Typ de **aanmelding**, **Voornaam**, **Achternaam**, **Single Sign-On-ID**, kenmerken van de **e-mailadres** van een geldige Azure Active Directory-account verrichten in de verwante tekstvakken te.
    2.  Klik op **maken**.

    >[AZURE.NOTE] De accounthouder Azure Active Directory krijgt een e-mailbericht met een koppeling naar de account bevestigen voordat deze actief wordt.

>[AZURE.NOTE] Kunt u een andere Coupa gebruiker account maken van hulpprogramma's of API's geleverd door Coupa bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-coupa-perform-the-following-steps"></a>Gebruikers toewijzen aan Coupa, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Coupa **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-coupa-tutorial/IC791911.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-coupa-tutorial/IC767830.png "Ja")

Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.
