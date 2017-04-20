<properties
    pageTitle="Beheren van uw eerste API in Azure API Management | Microsoft Azure"
    description="Informatie over API's maken, activiteiten toevoegen en aan de slag met de beheer-API."
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
    ms.topic="hero-article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="manage-your-first-api-in-azure-api-management"></a>Uw eerste API in Azure API beheer beheren

## <a name="overview"> </a>Overzicht

Deze handleiding wordt beschreven hoe u snel aan de slag in met Azure API beheer en maakt uw eerste API-aanroep.

## <a name="concepts"> </a>Wat is Azure API Management?

Azure API beheer kunt u een back-end nemen en start een volwaardige API-programma op basis van deze.

Gebruikelijke scenario's omvatten:

* **Mobiele infrastructuur beveiligen** door toegang met API-sleutels gating voorkomen van DOS aanvallen met behulp van bandbreedteregeling of met een geavanceerde beveiligingsbeleid zoals JWT token validatie.
* **Inschakelen van ISV-partner ecosystemen** door bieden snelle partner onboarding via de portal voor ontwikkelaars en het bouwen van een gevel API loskoppelen van interne implementaties die niet rijp voor consumptie partner.
* **Een interne API-programma** biedt een centrale locatie voor de organisatie om te communiceren over de beschikbaarheid en de meest recente wijzigingen in de API's, gating toegang op basis van organisatie-accounts, alle gebaseerd op een beveiligd kanaal tussen de gateway API en de back-end.


Het systeem bestaat uit de volgende onderdelen:

* De **gateway API** is het eindpunt dat:
  * API aanroept en stuurt ze naar de backends accepteert.
  * Controleert of de API sleutels JWT tokens, certificaten en andere referenties.
  * Hiermee wordt het gebruik van quota's en beperkingen te beoordelen.
  * Zet uw API op elk gewenst moment zonder code wijzigingen.
  * Back-end antwoorden waar instellen in de cache opgeslagen.
  * Logboeken belt metagegevens voor analytics.

* De **uitgever portal** is de administratieve interface waar u de API-programma instellen. Met InfoPath kunt u:
    * Definiëren of API-schema importeren.
    * Pakket API's in de producten.
    * Instellen van het beleid zoals quota of transformaties van de API's.
    * Krijg inzicht van analytics.
    * Gebruikers beheren.

* De **developer-portal** fungeert als de belangrijkste website voor ontwikkelaars, waar ze kunnen:
    * Lees API-documentatie.
    * Probeer een API via de interactieve console.
    * Maak een account en zich inschrijven voor het opvragen van API-sleutels.
    * Toegang analytics op hun eigen gebruik.


## <a name="create-service-instance"> </a>Maakt een exemplaar van de beheer-API

>[AZURE.NOTE] Als u deze zelfstudie hebt voltooid, moet u een account Azure. Als u geen account hebt, kunt u een gratis account maken in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure][].

De eerste stap bij het werken met beheer-API is een service-exemplaar te maken. Aanmelden bij de [Klassieke Azure-Portal][] en klik op **Nieuw**, **App-Services**, **Management API**, **maken**.

![Nieuw exemplaar van de beheer-API][api-management-create-instance-menu]

Geef een naam voor de URL van de unieke subdomein voor **URL**.

Kies het gewenste **abonnement** en **de regio** voor uw service-exemplaar. Nadat u uw selecties, klikt u op de knop **volgende** .

![Nieuwe API-Management-service][api-management-create-instance-step1]

**Contoso Ltd.** invoeren voor de **Naam van de organisatie**, en voer uw e-mailadres in het veld **E-Mail systeembeheerder** .

>[AZURE.NOTE] Dit e-mailadres wordt voor meldingen van API-beheersysteem gebruikt. Zie [berichten en e-mailsjablonen in Azure API beheer configureren][]voor meer informatie.

![Nieuwe API-Management-service][api-management-create-instance-step2]

Exemplaren van de service Management API zijn beschikbaar in drie lagen: ontwikkelaar, Standard en Premium. Nieuwe API Management service-exemplaren worden standaard gemaakt in de Developer-laag. De Standard of Premium laag selecteren, het selectievakje **Geavanceerde instellingen** in en selecteer de gewenste laag in het volgende scherm.

>[AZURE.NOTE] De ontwikkelaar laag is voor het ontwikkelen, testen en testprogramma API's waar hoge beschikbaarheid niet van belang is. U kunt uw aantal gereserveerde eenheden meer verkeer verwerken schalen in de Standard- en Premium-lagen. De Standard- en Premium-niveaus bieden uw service Management API met de meeste verwerkingskracht en prestaties. U kunt deze zelfstudie met behulp van elke laag. Voor meer informatie over lagen Management API Zie [API Beheer prijsstelling][].

Schakel het selectievakje in om de service-exemplaar te maken.

![Nieuwe API-Management-service][api-management-instance-created]

Zodra de service-exemplaar is gemaakt, is de volgende stap te maken of importeren van een API.

## <a name="create-api"> </a>Een API importeren

Een API bestaat uit een reeks bewerkingen die kunnen worden aangeroepen vanuit een clienttoepassing. API-bewerkingen zijn via proxy met bestaande webservices.

API's kunnen worden gemaakt (en bewerkingen kunnen worden toegevoegd) handmatig, of kan worden geïmporteerd. In deze zelfstudie, zullen we de API voor een steekproef calculator webservice door Microsoft geleverd en die worden gehost op Azure importeren.

>[AZURE.NOTE] Zie voor informatie over het maken van een API en het handmatig toevoegen van bewerkingen, [API's maken](api-management-howto-create-apis.md) en [het toevoegen van bewerkingen voor een API](api-management-howto-add-operations.md).

API's worden van de portal voor publisher wordt geopend met de klassieke Azure-Portal geconfigureerd. Klik op **beheren** in de klassieke Azure-Portal voor uw service Management API voor het bereiken van de publisher-portal.

![Publisher-portal][api-management-management-console]

Voor het importeren van de Rekenmachine API **API's** klikt u op in het menu **API beheer** aan de linkerkant en klik vervolgens op **Importeren API**.

![Knop importeren API][api-management-import-api]

Voer de volgende stappen voor het configureren van de Rekenmachine API:

1. Klik op **De URL van** **http://calcapi.cloudapp.net/calcapi.json** invoeren in het tekstvak **URL van specificatie** en klikt u op het keuzerondje **Swagger** .
2. **Calc** typt in het vak **Web API-URL suffix** .
3. Klik in het vak **producten (optioneel)** en kies **Starter**.
4. Klik op **Opslaan** om de API te importeren.

![Nieuwe API toevoegen][api-management-import-new-api]

>[AZURE.NOTE] **Management API** ondersteunt momenteel versie 1.2 en 2.0 van Swagger-document importeren. Zorg ervoor dat, hoewel [Swagger 2.0-specificatie](http://swagger.io/specification) dat verklaart `host`, `basePath`, en `schemes` eigenschappen zijn optioneel, uw Swagger 2.0 document **moet** bevatten die eigenschappen; anders won't het ophalen geïmporteerd. 

Zodra de API wordt geïmporteerd, wordt de overzichtspagina voor de API in de portal van publisher weergegeven.

![API-overzicht][api-management-imported-api-summary]

De sectie API bevat verschillende tabbladen. Het tabblad **Overzicht** worden fundamentele maatstaven en informatie over de API. Het tabblad [Instellingen](api-management-howto-create-apis.md#configure-api-settings) wordt gebruikt voor het weergeven en bewerken van de configuratie voor een API. Het tabblad [bewerkingen](api-management-howto-add-operations.md) wordt gebruikt voor het beheren van de API's. Het tabblad **beveiliging** kan worden gebruikt, gateway-verificatie configureren voor de back endserver met behulp van basisverificatie of [wederzijdse verificatie](api-management-howto-mutual-certificates.md)en [machtiging van gebruikers met OAuth 2.0](api-management-howto-oauth2.md)te configureren.  Het tabblad **problemen** is waarmee problemen gerapporteerd door de ontwikkelaars die gebruik van de API's maken. Het tabblad **producten** wordt gebruikt voor het configureren van de producten die deze API bevatten.

Elk exemplaar van de beheer-API wordt standaard geleverd met twee voorbeeld-producten:

-   **Starter**
-   **Onbeperkt**

In deze zelfstudie is de eenvoudige rekenmachine API toegevoegd aan het product Starter wanneer de API is geïmporteerd.

Zodat het aanroepen van een API ontwikkelaars moeten zich eerst abonneren op een die ze toegang tot het geeft product. Ontwikkelaars kunnen zich abonneren op producten in de portal voor ontwikkelaars of beheerders kunnen ontwikkelaars op de producten van de uitgever portal abonneren. Nadat u de API beheer exemplaar in de vorige stappen in de handleiding, gemaakt dus u bent al geabonneerd op elk product standaard bent u een beheerder.

## <a name="call-operation"> </a>Aanroepen van een bewerking vanuit de developer-portal

Activiteiten kunnen rechtstreeks vanuit de developer-portal die is een handige manier om te bekijken en testen van de activiteiten van een API worden aangeroepen. In deze zelfstudie stap zal u **twee gehele getallen toevoegen** -bewerking de eenvoudige rekenmachine API's aanroepen. Klik op **Developer-portal** in het menu aan de bovenkant van de publisher-portal.

![Developer-portal][api-management-developer-portal-menu]

**API's** op in het bovenste menu, en klik op **Rekenmachine Basic** overzicht van de beschikbare bewerkingen.

![Developer-portal][api-management-developer-portal-calc-api]

Noteer de beschrijvingen van de steekproef en parameters die samen met de API en bewerkingen, documentatie voor de ontwikkelaars die deze bewerking wilt gebruiken die zijn geïmporteerd. Deze beschrijvingen kunnen ook worden toegevoegd als bewerkingen handmatig worden toegevoegd.

Aanroepen van de bewerking van **twee gehele getallen toevoegen** , klikt u op **dit proberen**.

![Probeer het nu][api-management-developer-portal-calc-api-console]

U kunt bepaalde waarden opgeven voor de parameters of de standaardwaarden behouden en klik op **verzenden**.

![HTTP-Get][api-management-invoke-get]

Nadat een bewerking wordt gestart, de developer-portal de **Response-status**, de **antwoordheaders**en **inhoud van de reactie**wordt weergegeven.

![Reactie][api-management-invoke-get-response]

## <a name="view-analytics"> </a>Analytics weergeven

Een overzichtskaart van analytics voor eenvoudige rekenmachine, Ga terug naar de uitgever portal door **beheren** selecteren in het menu aan de bovenkant van de developer-portal.

![Beheren][api-management-manage-menu]

De standaardweergave voor de portal van publisher is het **Dashboard**, waarin u een overzicht van uw exemplaar van de beheer-API.

![Dashboard][api-management-dashboard]

Plaats de muis op de grafiek voor een **Eenvoudige rekenmachine** voor een overzicht van de specifieke criteria voor het gebruik van de API voor een bepaalde periode.

>[AZURE.NOTE] Als er geen regels in de grafiek, gaat u terug naar de portal voor ontwikkelaars en sommige aanroepen naar de API, wacht even en teruggaan naar het dashboard.

Klik op **Details weergeven** om de overzichtspagina voor de API, met inbegrip van een grotere versie van de weergegeven parameters weer te geven.

![Analytics][api-management-mouse-over]

![Samenvatting][api-management-api-summary-metrics]

Klik op **Analytics** in het menu **API beheer** aan de linkerkant voor gedetailleerde statistieken en rapporten.

![Overzicht][api-management-analytics-overview]

De **Analytics** -sectie bevat de volgende vier tabbladen:

-   Biedt algemene maatstaven voor en de gezondheid alsmede de bovenste ontwikkelaars, top producten top API's en bovenste bewerkingen **in één oogopslag** .
-   **Gebruik** biedt een diepgaande blik op de API-aanroepen en bandbreedte, met inbegrip van een geografische aanduiding.
-   **Gezondheid** richt zich op statuscodes, succespercentages, responstijden en API in de cache en responstijden service.
-   **Activiteit** biedt rapporten die Inzoomen op de specifieke activiteit door de ontwikkelaar, product, API en werking.

## <a name="next-steps"> </a>Volgende stappen

- Meer informatie over het [beveiligen van uw API met limieten voor de snelheid](api-management-howto-product-with-rules.md).

[Azure gratis proefversie]: http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=api_management_hero_a

[Create an API Management instance]: #create-service-instance
[Create an API]: #create-api
[Add an operation]: #add-operation
[Add the new API to a product]: #add-api-to-product
[Subscribe to the product that contains the API]: #subscribe
[Call an operation from the Developer Portal]: #call-operation
[View analytics]: #view-analytics
[Next steps]: #next-steps


[How to manage developer accounts in Azure API Management]: api-management-howto-create-or-invite-developers.md
[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Meldingen en e-mailsjablonen in Azure API beheer configureren]: api-management-howto-configure-notifications.md
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md
[Prijzen van de beheer-API]: http://azure.microsoft.com/pricing/details/api-management/

[Azure klassieke Portal]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-get-started/api-management-management-console.png
[api-management-create-instance-menu]: ./media/api-management-get-started/api-management-create-instance-menu.png
[api-management-create-instance-step1]: ./media/api-management-get-started/api-management-create-instance-step1.png
[api-management-create-instance-step2]: ./media/api-management-get-started/api-management-create-instance-step2.png
[api-management-instance-created]: ./media/api-management-get-started/api-management-instance-created.png
[api-management-import-api]: ./media/api-management-get-started/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-get-started/api-management-import-new-api.png
[api-management-imported-api-summary]: ./media/api-management-get-started/api-management-imported-api-summary.png
[api-management-calc-operations]: ./media/api-management-get-started/api-management-calc-operations.png
[api-management-list-products]: ./media/api-management-get-started/api-management-list-products.png
[api-management-add-api-to-product]: ./media/api-management-get-started/api-management-add-api-to-product.png
[api-management-add-myechoapi-to-product]: ./media/api-management-get-started/api-management-add-myechoapi-to-product.png
[api-management-api-added-to-product]: ./media/api-management-get-started/api-management-api-added-to-product.png
[api-management-developers]: ./media/api-management-get-started/api-management-developers.png
[api-management-add-subscription]: ./media/api-management-get-started/api-management-add-subscription.png
[api-management-add-subscription-window]: ./media/api-management-get-started/api-management-add-subscription-window.png
[api-management-subscription-added]: ./media/api-management-get-started/api-management-subscription-added.png
[api-management-developer-portal-menu]: ./media/api-management-get-started/api-management-developer-portal-menu.png
[api-management-developer-portal-calc-api]: ./media/api-management-get-started/api-management-developer-portal-calc-api.png
[api-management-developer-portal-calc-api-console]: ./media/api-management-get-started/api-management-developer-portal-calc-api-console.png
[api-management-invoke-get]: ./media/api-management-get-started/api-management-invoke-get.png
[api-management-invoke-get-response]: ./media/api-management-get-started/api-management-invoke-get-response.png
[api-management-manage-menu]: ./media/api-management-get-started/api-management-manage-menu.png
[api-management-dashboard]: ./media/api-management-get-started/api-management-dashboard.png

[api-management-add-response]: ./media/api-management-get-started/api-management-add-response.png
[api-management-add-response-window]: ./media/api-management-get-started/api-management-add-response-window.png
[api-management-developer-key]: ./media/api-management-get-started/api-management-developer-key.png
[api-management-mouse-over]: ./media/api-management-get-started/api-management-mouse-over.png
[api-management-api-summary-metrics]: ./media/api-management-get-started/api-management-api-summary-metrics.png
[api-management-analytics-overview]: ./media/api-management-get-started/api-management-analytics-overview.png
[api-management-analytics-usage]: ./media/api-management-get-started/api-management-analytics-usage.png
[api-management-]: ./media/api-management-get-started/api-management-.png
[api-management-]: ./media/api-management-get-started/api-management-.png
