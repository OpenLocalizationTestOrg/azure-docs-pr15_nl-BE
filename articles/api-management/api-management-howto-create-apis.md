<properties 
    pageTitle="API's maken in Azure API beheer" 
    description="Informatie over het maken en configureren van API's in Azure API beheer." 
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

# <a name="how-to-create-apis-in-azure-api-management"></a>API's maken in Azure API beheer

Een API API beheer vertegenwoordigt een reeks bewerkingen die door toepassingen kunnen worden aangeroepen. Nieuwe API's worden gemaakt in de portal voor publisher en vervolgens de gewenste bewerkingen toe te voegen. Nadat de bewerkingen zijn toegevoegd, wordt de API wordt toegevoegd aan een product en kan worden gepubliceerd. Als een API is gepubliceerd, kunt u het abonnement op en gebruikt door ontwikkelaars.

Deze handleiding bevat de eerste stap in het proces: het maken en configureren van een nieuwe API voor beheer-API. Zie voor meer informatie over bewerkingen toe te voegen en het publiceren van een product, [het toevoegen van bewerkingen met een API][] en [het maken en publiceren van een product][].

## <a name="create-new-api"> </a>Maakt een nieuwe API

API's zijn gemaakt en geconfigureerd in de publisher-portal. Klik op **beheren** in de klassieke Azure-Portal voor uw service Management API voor toegang tot de portal publisher.

![Publisher-portal][api-management-management-console]

>Als u een exemplaar van de service Management API nog niet hebt gemaakt, ziet u [een API Management service-exemplaar maken][] in de zelfstudie [aan de slag met Azure API beheer][] .

Klik op **API's** in het menu **API beheer** aan de linkerkant en klik vervolgens op **toevoegen API**.

![API maken][api-management-create-api]

Gebruik het venster **toevoegen nieuwe API** voor het configureren van de nieuwe API.

![Nieuwe API toevoegen][api-management-add-new-api]

De volgende velden worden gebruikt voor het configureren van de nieuwe API.

-   **Naam van de web-API** biedt een unieke en beschrijvende naam voor de API. In de portals ontwikkelaar en de uitgever wordt weergegeven.
-   **Webservice-URL** verwijst naar de HTTP-service implementeert de API. Management API stuurt verzoeken naar dit adres.
-   **Web API-URL-achtervoegsel** wordt toegevoegd aan de basis-URL voor de API management-service. De base URL is gemeenschappelijk voor alle API's die worden gehost door een exemplaar van de service Management API. Beheer-API API's worden onderscheiden door hun achtervoegsel en derhalve het achtervoegsel moet uniek zijn voor elke API voor een bepaalde uitgever.
-   **Web API-URL-schema** bepaalt welke protocollen kunnen worden gebruikt voor toegang tot de API. Standaard HTTPs is opgegeven.
-   Om deze nieuwe API eventueel aan een product toevoegen, klikt u op de **producten (optioneel)** -omlaag en kies een product. Deze stap kan meerdere malen worden herhaald de API aan meerdere producten toevoegen.

Nadat u de gewenste waarden zijn geconfigureerd, klikt u op **Opslaan**. Zodra de nieuwe API is gemaakt, wordt de overzichtspagina voor de API in de portal van publisher weergegeven.

![API-overzicht][api-management-api-summary]

## <a name="configure-api-settings"> </a>API configureren-instellingen

U kunt het tabblad **Instellingen** te controleren en bewerken van de configuratie voor een API. **Web API naam**en **URL webservice** **Web API-URL suffix** worden eerst ingesteld wanneer de API wordt gemaakt en kan worden gewijzigd hier. **Beschrijving** biedt een optionele beschrijving en **Web API-URL-schema** bepaalt welke protocollen kunnen worden gebruikt voor toegang tot de API.

![API-instellingen][api-management-api-settings]

Gateway als verificatie wilt configureren voor de implementatie van de API backend-service, klik op het tabblad **beveiliging** . De **referenties** vervolgkeuzelijst kan worden gebruikt **HTTP basic** of **clientcertificaten** verificatie configureren. HTTP-basisverificatie gebruikt, geeft u eenvoudigweg de gewenste referenties. Zie [back-end services met behulp van verificatie van clientcertificaten in Azure API beheer beveiligen][]voor meer informatie over het gebruik van verificatie van clientcertificaten.

Het tabblad **beveiliging** kunt configureren **gebruikersautorisatie** met OAuth 2.0 ook worden gebruikt. Zie [developer-accounts gebruikt OAuth 2.0 in Azure API beheer toestaan][]voor meer informatie.

![Basisverificatie-instellingen][api-management-api-settings-credentials]

Klik op **Opslaan** om de wijzigingen in de API-instellingen opslaan.

## <a name="next-steps"> </a>Volgende stappen

Zodra een API wordt gemaakt en de instellingen die zijn geconfigureerd, de volgende stappen worden de bewerkingen toevoegen aan de API, de API aan een product toevoegen en publiceren, zodat deze beschikbaar voor ontwikkelaars is. Zie de volgende artikelen voor meer informatie.

-   [Bewerkingen toevoegen aan een API][]
-   [Het maken en publiceren van een product][]





[api-management-create-api]: ./media/api-management-howto-create-apis/api-management-create-api.png
[api-management-management-console]: ./media/api-management-howto-create-apis/api-management-management-console.png
[api-management-add-new-api]: ./media/api-management-howto-create-apis/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-create-apis/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-create-apis/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-create-apis/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-create-apis/api-management-echo-operations.png

[What is an API?]: #what-is-api
[Create a new API]: #create-new-api
[Configure API settings]: #configure-api-settings
[Configure API operations]: #configure-api-operations
[Next steps]: #next-steps

[Bewerkingen toevoegen aan een API]: api-management-howto-add-operations.md
[Het maken en publiceren van een product]: api-management-howto-add-products.md

[Aan de slag met Azure API beheer]: api-management-get-started.md
[Een API Management service-exemplaar maken]: api-management-get-started.md#create-service-instance
[Het beveiligen van back-end services client met certificaatverificatie in Azure API beheer]: api-management-howto-mutual-certificates.md
[Hoe developer accounts met OAuth 2.0 in Azure API beheer toe te staan]: api-management-howto-oauth2.md