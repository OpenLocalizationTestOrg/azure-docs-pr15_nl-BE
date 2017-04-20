<properties
    pageTitle="Apps met Azure AD toepassingsproxy publiceren | Microsoft Azure"
    description="Publiceren op ruimten toepassingen naar de cloud met AD-toepassingsproxy Azure."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="kgremban"/>


# <a name="publish-applications-using-azure-ad-application-proxy"></a>Uitgeven van toepassingen met behulp van AD-toepassingsproxy Azure

Azure AD toepassingsproxy kunt u externe werknemers door het uitgeven van toepassingen in gebouwen toegankelijk via het internet ondersteunen. Door dit punt hebt u al [ingeschakeld in de klassieke Azure portal-toepassingsproxy](active-directory-application-proxy-enable.md). Dit artikel begeleidt u door de stappen voor het uitgeven van toepassingen die worden uitgevoerd op uw lokale netwerk en veilige externe toegang van buiten uw netwerk bieden. Nadat u dit artikel hebt uitgevoerd, zult u de toepassing configureren met persoonlijke informatie of beveiligingsvereisten.

> [AZURE.NOTE] Toepassingsproxy is een functie die is alleen beschikbaar als u een upgrade hebt uitgevoerd naar Premium of Basic edition van Azure Active Directory. Zie [edities Azure Active Directory](active-directory-editions.md)voor meer informatie.

## <a name="publish-an-app-using-the-wizard"></a>Een app met behulp van de wizard Publiceren

1. Log in als beheerder in [Azure klassieke portal](https://manage.windowsazure.com/).
2. Ga naar Active Directory en selecteer de map waarin u de toepassingsproxy ingeschakeld.

    ![Active Directory - pictogram](./media/active-directory-application-proxy-publish/ad_icon.png)

3. Klik op het tabblad **toepassingen** en klik vervolgens op de knop **toevoegen** onder aan het scherm

    ![Toepassing toevoegen](./media/active-directory-application-proxy-publish/aad_appproxy_selectdirectory.png)

4. Selecteer **een toepassing die toegankelijk van buiten uw netwerk is publiceren**.

    ![Een toepassing die toegankelijk van buiten uw netwerk is publiceren](./media/active-directory-application-proxy-publish/aad_appproxy_addapp.png)

5. Geef de volgende informatie over uw toepassing:

    - **Naam**: de beschrijvende naam voor uw toepassing. Deze moet uniek zijn binnen de map.
    - **Interne URL**: het adres dat de Connector-toepassing-Proxy wordt gebruikt voor toegang tot de toepassing van het particuliere netwerk. U kunt een specifiek pad opgeven op de back endserver te publiceren, terwijl de rest van de server niet gepubliceerd is. Op deze manier kunt u verschillende sites op dezelfde server publiceren en geven elk een eigen regels en de toegang.

        > [AZURE.TIP] Als u een pad, zorg bevat alle benodigde afbeeldingen, scripts en opmaakmodellen voor uw toepassing. Bijvoorbeeld, als uw app op https://yourapp/app is en afbeeldingen te vinden op https://yourapp/media, moet vervolgens u publiceren https://yourapp/ als het pad.

    - **Vooraf-verificatie methode**: de toepassingsproxy hoe gebruikers worden geverifieerd voordat ze toegang geven tot de toepassing. Kies een van de opties in de vervolgkeuzelijst.

        - Azure Active Directory: Toepassingsproxy leidt gebruikers zich aanmelden met Azure AD, die hun machtigingen voor de map en de toepassing wordt geverifieerd.
        - Doorvoer: Gebruikers niet hoeven te verifiëren om toegang te krijgen tot de toepassing.

    ![Eigenschappen van toepassingen](./media/active-directory-application-proxy-publish/aad_appproxy_appproperties.png)  

6. Klik op het selectievakje onder aan het scherm om de wizard. De toepassing is nu gedefinieerd in Azure AD.


## <a name="assign-users-and-groups-to-the-application"></a>Gebruikers en groepen toewijzen aan de toepassing

Om uw gebruikers toegang tot uw gepubliceerde toepassing, moet u ze afzonderlijk of in groepen toewijzen. (Vergeet niet om uzelf toegang toewijzen, te.) Hiervoor is vereist dat elke gebruiker een licentie voor Azure elementaire of hoger hebben. U kunt licenties voor individueel of aan groepen. Zie [gebruikers aan een toepassing toewijzen](active-directory-applications-guiding-developers-assigning-users.md) voor meer informatie. 

Voor toepassingen waarvoor verificatie vereist is, verleend deze machtigingen aan de app gebruiken. Voor toepassingen die geen verificatie is vereist, kunnen gebruikers nog steeds worden toegewezen aan de app zodat deze wordt weergegeven in de toepassingslijst van hun, zoals MyApps.

1. Nadat de wizard toepassing toevoegen is voltooid, ziet u de pagina Quick Start voor uw toepassing. Om te beheren wie toegang heeft tot de app, **gebruikers en groepen**te selecteren.

    ![Gebruikers - screenshot toepassing Proxy slag toewijzen](./media/active-directory-application-proxy-publish/aad_appproxy_usersgroups.png)

2. Alle gebruikers weergeven of zoeken naar specifieke groepen in de directory. De lijst met zoekresultaten, klikt u op het vinkje.

    ![Zoeken naar groepen of gebruikers - screenshot](./media/active-directory-application-proxy-publish/aad_appproxy_search.png)

2. Selecteer elke gebruiker of groep die u wilt toewijzen aan deze app en klik op **toewijzen**. U wordt gevraagd deze actie te bevestigen.

> [AZURE.NOTE] Voor geïntegreerde verificatie van Windows-toepassingen, kunt u alleen gebruikers en groepen die zijn gesynchroniseerd van uw Active Directory op ruimten toewijzen. Gebruikers die zich met een Microsoft-account en gasten aanmelden kunnen niet worden toegewezen voor apps gepubliceerd met Azure Active Directory Application Proxy. Zorg ervoor dat de gebruikers zich aanmelden met de referenties die deel uitmaken van hetzelfde domein als de toepassing die u wilt publiceren.

## <a name="test-your-published-application"></a>De uitgegeven toepassing testen

Wanneer u uw toepassing hebt gepubliceerd, kunt u dit testen door te gaan naar de URL die u hebt gepubliceerd. Zorg ervoor dat u toegang, dat deze correct wordt gerenderd en alles werkt zoals verwacht. Als u problemen ondervindt of een foutbericht krijgt, probeert u de [handleiding voor het oplossen van problemen](active-directory-application-proxy-troubleshoot.md).

## <a name="configure-your-application"></a>Uw toepassing configureren

U kunt gepubliceerde apps wijzigen of instellen van geavanceerde opties op de pagina configureren. Op deze pagina kunt u uw app aanpassen door het wijzigen van de naam of een logo uploaden. U kunt ook toegangsregels zoals de methode voor verificatie of meerledige verificatie beheren.

![Geavanceerde configuratie](./media/active-directory-application-proxy-publish/aad_appproxy_configure.png)


Nadat u toepassingen publiceren met Azure Active Directory Application Proxy, ze worden weergegeven in de lijst met toepassingen in Azure AD en u er kunt beheren.

Als u de Proxy-services uitschakelen nadat u toepassingen hebt gepubliceerd, zijn ze niet langer toegankelijk van buiten het particuliere netwerk. U verwijdert hiermee niet de toepassingen.

Als u wilt weergeven van een toepassing en zorg ervoor dat toegankelijk is, dubbelklikt u op de naam van de toepassing. Als de toepassingsproxy-service is uitgeschakeld en de toepassing niet beschikbaar is, wordt een waarschuwingsbericht weergegeven aan de bovenkant van het scherm.

Als u een toepassing verwijdert, selecteert u een toepassing in de lijst en klik op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

- [Uitgeven van toepassingen met behulp van uw eigen domeinnaam](active-directory-application-proxy-custom-domains.md)
- [Eenmalige inschakelen](active-directory-application-proxy-sso-using-kcd.md)
- [Voorwaardelijke toegang](active-directory-application-proxy-conditional-access.md)
- [Werken met vorderingen op toepassingen](active-directory-application-proxy-claims-aware-apps.md)

Bekijk de [toepassingsproxy blog](http://blogs.technet.com/b/applicationproxyblog/) voor het laatste nieuws en updates
