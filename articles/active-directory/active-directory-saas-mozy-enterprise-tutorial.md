<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Mozy-onderneming | Microsoft Azure" 
    description="Meer informatie over het Mozy Enterprise met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-mozy-enterprise"></a>Zelfstudie: Azure Active Directory-integratie met Mozy onderneming
  
Het doel van deze zelfstudie is weer te geven van de integratie van de Azure en Mozy Enterprise.  
Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:

-   Een geldig abonnement Azure
-   Een huurder Mozy onderneming
  
Na het voltooien van deze zelfstudie, is de Azure AD-gebruikers die u hebt toegewezen aan de Mozy Enterprise mogelijk voor één teken in de toepassing van de onderneming Mozy bedrijf site (service gestart aanmelden op), of met behulp van de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md).
  
Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De integratie van toepassingen voor de onderneming Mozy inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruikersaanvragen configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777308.png "Scenario")
##<a name="enabling-the-application-integration-for-mozy-enterprise"></a>De integratie van toepassingen voor de onderneming Mozy inschakelen
  
Het doel van deze sectie wordt aan de contour van het inschakelen van de integratie van toepassingen voor Mozy-onderneming.

###<a name="to-enable-the-application-integration-for-mozy-enterprise-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor Mozy onderneming, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, in het linkernavigatievenster op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-mozy-enterprise-tutorial/IC700993.png "Active Directory")

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen] (./media/active-directory-saas-mozy-enterprise-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** onder aan de pagina.

    ![Toepassing toevoegen] (./media/active-directory-saas-mozy-enterprise-tutorial/IC749321.png "Toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassing van de gallerry toevoegen] (./media/active-directory-saas-mozy-enterprise-tutorial/IC749322.png "Toepassing van de gallerry toevoegen")

6.  Typ in het **zoekvak** **mozy enterprise**.

    ![Galerie van toepassing] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777309.png "Galerie van toepassing")

7.  Selecteer **Mozy Enterprise**en klik op **Voltooien** als de toepassing wilt toevoegen in het resultatendeelvenster.

    ![Mozy onderneming] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777310.png "Mozy onderneming")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van deze sectie wordt aan de contour van het inschakelen van gebruikers worden geverifieerd bij Mozy Enterprise met hun account in Azure AD federation op basis van de SAML-protocol gebruiken.  
Als onderdeel van deze procedure bent u verplicht een base-64 gecodeerde certificaat uploaden naar uw huurder Mozy Enterprise.  
Als u niet bekend met deze procedure bent, Zie [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:

1.  Klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen in de Azure klassieke portal op de pagina **Mozy Enterprise** application integration.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mozy-enterprise-tutorial/IC771709.png "Eenmalige aanmelding configureren")

2.  Selecteer **Microsoft Azure AD Single Sign-On**op de pagina **Hoe wilt u dat gebruikers aanmelden bij Mozy Enterprise** en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777311.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **URL van App configureren** in **Mozy Enterprise Sign In URL** textbox, de URL van uw gebruik van het volgende patroon "https://*\<-naam van de huurder\>. Mozyenterprise.com*", en klik op **volgende**.

    ![App-URL configureren] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777312.png "App-URL configureren")

4.  Op de pagina **configureren eenmalige aanmelding bij de onderneming Mozy** downloaden van uw certificaat, klik op **certificaat downloaden**en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777313.png "Eenmalige aanmelding configureren")

5.  Log in op uw site Mozy Enterprise bedrijf als beheerder in een ander web browser-venster.

6.  Klik in de sectie **configuratie van** **Beleid voor verificatie**.

    ![Beleid voor verificatie] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777314.png "Beleid voor verificatie")

7.  In de sectie **Beleid voor verificatie** , moet u de volgende stappen uitvoeren:

    ![Beleid voor verificatie] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777315.png "Beleid voor verificatie")

    1.  Selecteer **Active Directory** als **Provider**.
    2.  Selecteer **Push LDAP gebruiken**.
    3.  Klik op het tabblad **Verificatie SAML** .
    4.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Mozy onderneming** de waarde **Verificatie verzoek-URL** kopiëren en vervolgens plakken in het tekstvak **URL van de verificatie** .
    5.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij de onderneming Mozy** de **Identity Provider-ID** -waarde kopiëren en vervolgens plakken in het **SAML-eindpunt** textbox.
    6.  Een **Base64 - gecodeerd** bestand maken van uw gedownloade certificaat.  

        >[AZURE.TIP]Zie voor meer informatie, [het converteren van een binaire certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    7.  De base-64 gecodeerde certificaat in Kladblok te openen, de inhoud ervan kopiëren naar het Klembord en plak het gehele certificaat in textbox **SAML-certificaat** .
    8.  Selecteer **Eenmalige aanmelding inschakelen voor beheerders zich aanmelden met hun netwerkreferenties**.
    9.  Klik op **wijzigingen opslaan**.

8.  In de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij de onderneming Mozy** selecteert u de Configuratiebevestiging van één aanmelding en klik op **Voltooien**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777316.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Gebruikersaanvragen configureren
  
Om gebruikers aan te melden bij Mozy Enterprise AD Azure, moeten zij worden ingericht in Mozy Enterprise.  
Mozy-onderneming is inrichten een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Om het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1.  Log in op uw huurder **Mozy Enterprise** .

2.  Klik op **gebruikers**en klik vervolgens op **Nieuwe gebruiker toevoegen**.

    ![Gebruikers] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777317.png "Gebruikers")

    >[AZURE.NOTE]De optie **Nieuwe gebruiker toevoegen** wordt alleen weergegeven als **Mozy** is geselecteerd als de provider onder **beleid voor verificatie**. Als het SAML-verificatie is geconfigureerd worden vervolgens de gebruikers automatisch toegevoegd aan hun eerste aanmelding via Single sign op.

3.  In het gebruikersdialoogvenster nieuwe, voert u de volgende stappen uit:

    ![Gebruikers toevoegen] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777318.png "Gebruikers toevoegen")

    1.  Selecteer een groep in de lijst **Selecteer een groep** .
    2.  Selecteer een type in de lijst **type gebruiker** .
    3.  Typ in het tekstvak **gebruikersnaam** de naam van de gebruiker Azure AD.
    4.  Typ in het tekstvak **e-mailadres** het e-mailadres van de gebruiker Azure AD.
    5.  Selecteer de **gebruiker instructies per E-mail**.
    6.  Klik op **gebruikers toevoegen**.

    >[AZURE.NOTE]Nadat de gebruiker is gemaakt, een e-mailbericht ontvangt de Azure AD-gebruiker die een koppeling naar de account bevestigen voordat deze actief wordt.

>[AZURE.NOTE]Kunt u een andere onderneming Mozy gebruiker account hulpmiddelen voor het maken of API's die door Mozy onderneming bepaling AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
 
Test uw configuratie, moet u de Azure AD gebruikers verlenen dat u wilt toestaan via de toepassing toegang tot het door toe te wijzen.

###<a name="to-assign-users-to-mozy-enterprise-perform-the-following-steps"></a>Gebruikers toewijzen aan onderneming Mozy, voert u de volgende stappen uit:

1.  In de klassieke Azure portal, een testaccount te maken.

2.  Klik op **gebruikers toewijzen**op de pagina **Mozy Enterprise **application integration.

    ![Gebruikers toewijzen] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777319.png "Gebruikers toewijzen")

3.  Selecteer het testgebruiker, klik op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-mozy-enterprise-tutorial/IC767830.png "Ja")
  
Als u testen, uw instellingen voor eenmalige aanmelding wilt, open het Access-venster. Zie [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-venster.