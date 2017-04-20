<properties 
    pageTitle="Hoe developer accounts met OAuth 2.0 in Azure API beheer toe te staan" 
    description="Informatie over het gebruik OAuth 2.0 API beheer gebruikers machtigen." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-authorize-developer-accounts-using-oauth-20-in-azure-api-management"></a>Hoe developer accounts met OAuth 2.0 in Azure API beheer toe te staan

Veel API's ondersteunen [OAuth 2.0](http://oauth.net/2/) om de API te beveiligen en ervoor te zorgen dat alleen geldige gebruikers toegang hebben en dat zij alleen toegang krijgen tot bronnen waarvoor ze gerechtigd bent. Kunt Azure API beheer interactive Developer Console met deze API's gebruiken, kunt de service u voor het configureren van uw exemplaar van de service om te werken met uw OAuth 2.0 ingeschakelde API.

## <a name="prerequisites"> </a>Voorwaarden

Deze handleiding hoe u uw exemplaar van de service Management API gebruikt OAuth 2.0 vergunning voor ontwikkelaars accounts configureren, maar wordt niet uitgelegd hoe u een OAuth 2.0 provider configureren. De configuratie voor elke provider OAuth 2.0 is verschillend, maar de stappen vergelijkbaar zijn, en de vereiste stukken van informatie die wordt gebruikt bij het configureren van OAuth 2.0 in uw exemplaar van de service Management API hetzelfde zijn. Dit onderwerp bevat voorbeelden met Azure Active Directory als een provider OAuth 2.0.

>[AZURE.NOTE] Zie voor meer informatie over het configureren van OAuth 2.0 Azure Active Directory met de [WebApp-GraphAPI-DotNet][] monster.

## <a name="step1"> </a>Een vergunning OAuth 2.0 server configureren in beheer-API

Klik op **beheren** in de klassieke Azure-Portal voor uw service Management API om te beginnen. Hiermee gaat u naar de portal Management API publisher.

![Publisher-portal][api-management-management-console]

>[AZURE.NOTE] Als u een exemplaar van de service Management API nog niet hebt gemaakt, ziet u [een API Management service-exemplaar maken][] in de zelfstudie [aan de slag met Azure API beheer][] .

Klik op **beveiliging** in het menu **API beheer** aan de linkerkant **OAuth 2.0**en klik vervolgens op **Add vergunning server**.

![OAuth 2.0][api-management-oauth2]

Wanneer u op de **server van autorisatie toevoegen**, wordt het nieuwe formulier van de vergunning-server weergegeven.

![Nieuwe server][api-management-oauth2-server-1]

Voer een naam en een optionele beschrijving in de velden **naam** en **Beschrijving** . 

>[AZURE.NOTE] Deze velden worden gebruikt voor het identificeren van de server 2.0 OAuth vergunning binnen het huidige exemplaar van de beheer-API en hun waarden niet afkomstig zijn van de server 2.0 OAuth.

De **Client de registratie van URL**invoeren. Deze pagina is waar gebruikers kunnen maken en beheren van hun accounts, en is afhankelijk van de provider die OAuth 2.0 gebruikt. De **Client de registratie van URL** verwijst naar de pagina die gebruikers kunnen maken en hun eigen accounts configureren voor 2.0 OAuth-providers die ondersteuning bieden voor gebruikersbeheer van accounts gebruiken. Sommige organisaties niet configureren of deze functionaliteit te gebruiken, zelfs als de provider OAuth 2.0 ondersteunt. Als uw provider OAuth 2.0 geen Gebruikersbeheer van accounts die zijn geconfigureerd, voert u een tijdelijke aanduiding URL hier bijvoorbeeld de URL van uw bedrijf of een URL, zoals `https://placeholder.contoso.com`.

De volgende sectie van het formulier bevat de **autorisatiecode verlenen typen** **URL-autorisatie eindpunt**en **aanvraagmethode autorisatie** -instellingen.

![Nieuwe server][api-management-oauth2-server-2]

Geef de **code van de vergunning verlenen typen** door de gewenste typen controleren. **Validatiecode** is standaard opgegeven.

Voer de **URL-autorisatie eindpunt**. Voor Azure Active Directory, deze URL is vergelijkbaar met de volgende URL waar `<client_id>` wordt vervangen door de client-id die uw toepassing naar de server 2.0 OAuth identificeert.

    https://login.windows.net/<client_id>/oauth2/authorize

De **aanvraagmethode vergunning** geeft aan hoe de autorisatieaanvraag wordt verzonden naar de server 2.0 OAuth. **Ophalen** is standaard ingeschakeld.

De volgende sectie is waar het **Token eindpunt-URL**, **Client-verificatiemethoden** **toegangstoken methode verzenden**en **standaard scope** zijn ingesteld.

![Nieuwe server][api-management-oauth2-server-3]

Voor een server Azure Active Directory OAuth 2.0 de **eindpunt-URL Token** hebben de volgende indeling, waarbij `<APPID>` is de indeling van `yourapp.onmicrosoft.com`.

    https://login.windows.net/<APPID>/oauth2/token

De standaardinstelling voor **Client-verificatiemethoden** is **Basic**en **toegangstoken verzenden methode** **verificatieheader**is. Deze waarden worden in deze sectie van het formulier, samen met het **standaardbereik**geconfigureerd.

De **referenties van de Client** -sectie bevat de **Client-ID** en het **geheim van de Client**, die zijn verkregen tijdens het maken en de configuratie van uw server 2.0 OAuth. Zodra de **Client-ID** en het **geheim van de Client** zijn opgegeven, wordt de **redirect_uri** voor de **autorisatiecode** gegenereerd. Deze URI wordt gebruikt om de URL van het antwoord in de configuratie van uw server 2.0 OAuth configureren.

![Nieuwe server][api-management-oauth2-server-4]

Als **code vergunning verlenen typen** **Resource eigenaar**wachtwoord is ingesteld, wordt de sectie **bron eigenaar wachtwoordreferenties** gebruikt om op te geven die referenties; anders kunt u het leeg laten.

![Nieuwe server][api-management-oauth2-server-5]

Zodra het formulier voltooid is, klikt u op **Opslaan** om het opslaan van de configuratie van de API Management OAuth 2.0 vergunning. Zodra de configuratie van de server is opgeslagen, kunt u de API's voor het gebruik van deze configuratie configureren zoals wordt weergegeven in de volgende sectie.

## <a name="step2"> </a>Een API gebruikt OAuth 2.0 gebruikersautorisatie configureren

**API's** in het menu **API beheer** aan de linkerkant klikt u op, klikt u op de naam van de gewenste API, klikt u op **beveiliging**en vervolgens het selectievakje **OAuth 2.0**.

![Gebruikersautorisatie][api-management-user-authorization]

Selecteer de gewenste **server vergunning** uit de vervolgkeuzelijst en klik op **Opslaan**.

![Gebruikersautorisatie][api-management-user-authorization-save]

## <a name="step3"> </a>Test de gebruikersautorisatie OAuth 2.0 in de Developer-Portal

Zodra u hebt uw vergunning OAuth 2.0 server geconfigureerd en de API voor het gebruik van die server, kunt u deze testen door te gaan naar de Portal voor ontwikkelaars en een API aanroept.  Klik **Developer-portal** in het bovenste menu rechts.

![Developer-portal][api-management-developer-portal-menu]

**API's** op in het bovenste menu en selecteer **Echo-API**.

![Echo-API][api-management-apis-echo-api]

>[AZURE.NOTE] Als er slechts één API geconfigureerde of zichtbaar is voor uw account, gaat API's op te klikken u rechtstreeks naar de bewerkingen voor die API.

Bewerking van de **Resource die u** selecteert, klikt u op de **Geopende Console**en **autorisatiecode** selecteert in de vervolgkeuzelijst.

![Open console][api-management-open-console]

Wanneer **autorisatiecode** is geselecteerd, wordt een pop-upvenster weergegeven met het formulier aanmelden van de provider OAuth 2.0. In dit voorbeeld wordt het formulier geleverd door Azure Active Directory.

>[AZURE.NOTE] Als u pop-ups uitgeschakeld, moet u deze inschakelen door de browser. Nadat u deze hebt ingeschakeld, worden de select- **code van de vergunning** opnieuw en het formulier weergegeven.

![Aanmelden][api-management-oauth2-signin]

Zodra u bent aangemeld, de **aanvraagheaders** zijn gevuld met een `Authorization : Bearer` kop dat de aanvraag machtigt.

![Header-token aanvragen][api-management-request-header-token]

U kunt nu de gewenste waarden voor de overige parameters configureren en de aanvraag indienen. 

## <a name="next-steps"></a>Volgende stappen

Zie de volgende video en de bijbehorende [artikel](api-management-howto-protect-backend-with-aad.md)voor meer informatie over het gebruik van OAuth 2.0 en beheer-API.

> [AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

[api-management-management-console]: ./media/api-management-howto-oauth2/api-management-management-console.png
[api-management-oauth2]: ./media/api-management-howto-oauth2/api-management-oauth2.png
[api-management-user-authorization]: ./media/api-management-howto-oauth2/api-management-user-authorization.png
[api-management-user-authorization-save]: ./media/api-management-howto-oauth2/api-management-user-authorization-save.png
[api-management-oauth2-signin]: ./media/api-management-howto-oauth2/api-management-oauth2-signin.png
[api-management-request-header-token]: ./media/api-management-howto-oauth2/api-management-request-header-token.png
[api-management-developer-portal-menu]: ./media/api-management-howto-oauth2/api-management-developer-portal-menu.png
[api-management-open-console]: ./media/api-management-howto-oauth2/api-management-open-console.png
[api-management-oauth2-server-1]: ./media/api-management-howto-oauth2/api-management-oauth2-server-1.png
[api-management-oauth2-server-2]: ./media/api-management-howto-oauth2/api-management-oauth2-server-2.png
[api-management-oauth2-server-3]: ./media/api-management-howto-oauth2/api-management-oauth2-server-3.png
[api-management-oauth2-server-4]: ./media/api-management-howto-oauth2/api-management-oauth2-server-4.png
[api-management-oauth2-server-5]: ./media/api-management-howto-oauth2/api-management-oauth2-server-5.png
[api-management-apis-echo-api]: ./media/api-management-howto-oauth2/api-management-apis-echo-api.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Aan de slag met Azure API beheer]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Een API Management service-exemplaar maken]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

