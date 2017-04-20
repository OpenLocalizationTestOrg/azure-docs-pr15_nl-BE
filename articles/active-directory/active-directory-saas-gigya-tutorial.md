<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Gigya | Microsoft Azure" 
    description="Meer informatie over het Gigya met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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
    ms.date="09/01/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-gigya"></a>Zelfstudie: Azure Active Directory-integratie met Gigya
  
Het doel van deze zelfstudie is de integratie van de Azure en Gigya weergeven.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een Gigya eenmalige aanmelding abonnement ingeschakeld
  
Na het voltooien van deze zelfstudie, is de Azure AD-gebruikers die u hebt toegewezen aan Gigya mogelijk voor één teken in de toepassing van de Gigya bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor Gigya inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Eenmalige aanmelding configureren] (./media/active-directory-saas-gigya-tutorial/IC789512.png "Eenmalige aanmelding configureren")
##<a name="enabling-the-application-integration-for-gigya"></a>De integratie van toepassingen voor Gigya inschakelen
  
Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor Gigya.

###<a name="to-enable-the-application-integration-for-gigya-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Gigya, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-gigya-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-gigya-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-gigya-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-gigya-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **Gigya**.

    ![Galerie van toepassing] (./media/active-directory-saas-gigya-tutorial/IC789513.png "Galerie van toepassing")

7.  In het resultatendeelvenster **Gigya**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Gigya] (./media/active-directory-saas-gigya-tutorial/IC789527.png "Gigya")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Gigya met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat-bestand te maken.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Gigya** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-gigya-tutorial/IC789528.png "Eenmalige aanmelding configureren")

2.  Op de pagina **Hoe wilt u dat gebruikers aanmelden bij Gigya** Selecteer **AD Azure Microsoft Single Sign-On**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-gigya-tutorial/IC789529.png "Eenmalige aanmelding configureren")

3.  Typ de URL met het volgende patroon '*http://company.gigya.com*' op de pagina **Configureren App-URL** in het tekstvak **Gigya teken in de URL** en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-gigya-tutorial/IC789530.png "App-URL configureren")

4.  Klik op **certificaat downloaden**en sla het bestand op uw computer op de pagina **configureren eenmalige aanmelding op Gigya** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-gigya-tutorial/IC789531.png "Eenmalige aanmelding configureren")

5.  Log in op uw site Gigya bedrijf als beheerder in een ander web browser-venster.

6.  Ga naar **instellingen \> SAML-aanmelding**, en klik vervolgens op de knop **toevoegen** .

    ![SAML-aanmelding] (./media/active-directory-saas-gigya-tutorial/IC789532.png "SAML-aanmelding")

7.  In de sectie **SAML-aanmelding** moet u de volgende stappen uitvoeren:

    ![SAML-configuratie] (./media/active-directory-saas-gigya-tutorial/IC789533.png "SAML-configuratie")

    1.  Typ in het tekstvak **naam** een naam voor uw configuratie.
    2.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Gigya** de **Uitgever URL** -waarde kopiëren en vervolgens plakken in het tekstvak van de **uitgevende instelling** .
    3.  In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding bij Gigya** de **Single Sign-On Service URL** -waarde kopiëren en vervolgens plakken in het tekstvak **Single Sign-On Service URL** .
    4.  In de klassieke Azure portal, op de pagina **configureren eenmalige aanmelding bij Gigya** de **Indeling van de id** -waarde kopiëren en vervolgens plakken in het tekstvak **Naam ID opmaken** .
    5.  Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.
        
        >[AZURE.TIP]Zie voor meer informatie, [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    6.  De base-64 gecodeerde certificaat openen in Kladblok, de inhoud ervan kopiëren naar het Klembord en plak deze vervolgens naar de textbox **X.509-certificaat** .
    7.  Klik op **Instellingen opslaan**.

8.  Op de klassieke Azure portal, selecteert u de van één aanmelding Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren van eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-gigya-tutorial/IC789534.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij Gigya Azure AD, moeten zij worden ingericht in Gigya.  
Gigya is inrichten een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder uw bedrijf **Gigya** site.

2.  Ga naar **Admin \> gebruikers beheren**, en klik vervolgens op **Gebruikers uitnodigen**.

    ![Gebruikers beheren] (./media/active-directory-saas-gigya-tutorial/IC789535.png "Gebruikers beheren")

3.  Voer de volgende stappen uit in het dialoogvenster gebruikers uitnodigen:

    ![Gebruikers uitnodigen] (./media/active-directory-saas-gigya-tutorial/IC789536.png "Gebruikers uitnodigen")

    1.  Typ in het tekstvak **e-mailadres** het e-mailalias van een geldige Azure Active Directory-account die u verrichten wilt.
    2.  Klik op **gebruiker uitnodigen**.
    
        >[AZURE.NOTE] De accounthouder Azure Active Directory ontvangt een e-mail met daarin een link om te bevestigen de account voordat deze actief wordt.

>[AZURE.NOTE] Kunt u een andere Gigya gebruiker account maken van hulpprogramma's of API's geleverd door Gigya bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-gigya-perform-the-following-steps"></a>Gebruikers toewijzen aan Gigya, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Gigya **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-gigya-tutorial/IC789537.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-gigya-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.