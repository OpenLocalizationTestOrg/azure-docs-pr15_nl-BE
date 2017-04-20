<properties 
    pageTitle="Basisbegrippen voor beheer-API" 
    description="Meer informatie over API's, producten, functies, groepen en andere belangrijke concepten voor de beheer-API." 
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

# <a name="how-to-import-the-definition-of-an-api-with-operations-in-azure-api-management"></a>Het importeren van de definitie van een bewerkingen in Azure API beheer-API

Nieuwe API's kunnen worden gemaakt in beheer-API en de bewerkingen handmatig worden toegevoegd of de API kan worden geïmporteerd met de bewerkingen in één stap.

API's en hun activiteiten kunnen worden geïmporteerd met behulp van de volgende indelingen.

-   WADL
-   Swagger

Deze gids toont hoe maakt u een nieuwe API en haar activiteiten in één stap geïmporteerd. Zie [API's maken][] en [het toevoegen van bewerkingen voor een API][]voor meer informatie over het handmatig maken van een API en bewerkingen toe te voegen.

## <a name="import-api"> </a>Een API importeren

API's zijn gemaakt en geconfigureerd in de publisher-portal. Klik op **beheren** in de klassieke Azure-Portal voor uw service Management API voor toegang tot de portal publisher. Als u een exemplaar van de service Management API nog niet hebt gemaakt, ziet u [een API Management service-exemplaar maken][] in de zelfstudie [aan de slag met Azure API beheer][] .

![Publisher-portal][api-management-management-console]

Klik op **API's** in het menu **API beheer** aan de linkerkant en klik vervolgens op **importeren API**.

![Import-API][api-management-import-apis]

Het venster **Import-API** bevat drie tabbladen die met de drie manieren overeenkomen om de API-specificatie.

-   Hiermee kunt u de API-specificatie in het opgegeven tekstvak plakken **vanaf het Klembord** .
-   **Uit bestand** kunt u op Bladeren en selecteer het bestand waarin de API-specificatie.
-   **Van URL** , kunt u de URL in de specificatie voor de API.

![API-importindeling][api-management-import-api-clipboard]

Gebruik de keuzerondjes op het recht om aan te geven van de indeling van de specificatie na het opgeven van de specificatie voor de API. De volgende indelingen worden ondersteund.

-   WADL
-   Swagger

Geef vervolgens een **achtervoegsel Web API-URL**. Dit wordt toegevoegd aan de basis-URL voor uw management-service API. De base URL is gemeenschappelijk voor alle API's die worden gehost op elk exemplaar van een API-Management-service. Beheer-API API's worden onderscheiden door hun achtervoegsel en derhalve het achtervoegsel moet uniek zijn voor elke API in een specifieke API management service-exemplaar.

Nadat alle waarden zijn ingevoerd, klikt u op **Opslaan** om de API en de daarmee verband houdende bewerkingen te maken. 

>[AZURE.NOTE] Zie voor een zelfstudie van het importeren van een eenvoudige rekenmachine API in de indeling Swagger, [de eerste API in Azure API beheer beheren](api-management-get-started.md).

## <a name="export-api"></a> Een API exporteren

Naast de nieuwe API's importeert, kunt u de definities van de API's van het portal voor publisher exporteren. Klik hiertoe op **API exporteren** op het **tabblad Samenvatting** van de **API**.

![Export-API][api-management-export-api]

API's kunnen worden geëxporteerd met behulp van WADL of Swagger. Selecteer de gewenste indeling, klikt u op **Opslaan**en kies de locatie waar u het bestand wilt opslaan.

![API-indeling exporteren][api-management-export-api-format]

## <a name="next-steps"> </a>Volgende stappen

Wanneer een API is gemaakt en geïmporteerd bewerkingen die u kunt bekijken en configureren van eventuele aanvullende instellingen de API aan een Product toevoegen en publiceren, zodat deze beschikbaar is voor ontwikkelaars. Zie de volgende handleidingen voor meer informatie.

-   [API-instellingen configureren][]
-   [Het maken en publiceren van een product][]




[api-management-management-console]: ./media/api-management-howto-import-api/api-management-management-console.png
[api-management-import-apis]: ./media/api-management-howto-import-api/api-management-api-import-apis.png
[api-management-import-api-clipboard]: ./media/api-management-howto-import-api/api-management-import-api-wizard.png
[api-management-export-api]: ./media/api-management-howto-import-api/api-management-export-api.png
[api-management-export-api-format]: ./media/api-management-howto-import-api/api-management-export-api-format.png

[Import an API]: #import-api
[Export an API]: #export-api
[Configure API settings]: #configure-api-settings
[Next steps]: #next-steps

[Aan de slag met Azure API beheer]: api-management-get-started.md
[Een API Management service-exemplaar maken]: api-management-get-started.md#create-service-instance

[Bewerkingen toevoegen aan een API]: api-management-howto-add-operations.md
[Het maken en publiceren van een product]: api-management-howto-add-products.md
[Het maken van API 's]: api-management-howto-create-apis.md
[API-instellingen configureren]: api-management-howto-create-apis.md#configure-api-settings
