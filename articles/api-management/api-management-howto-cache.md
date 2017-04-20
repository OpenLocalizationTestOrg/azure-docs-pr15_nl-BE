<properties
    pageTitle="Voor betere prestaties in Azure API beheer caching toevoegen | Microsoft Azure"
    description="Informatie over het verbeteren van de latentie, bandbreedte en web-service voor Management API serviceoproepen laden."
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
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="add-caching-to-improve-performance-in-azure-api-management"></a>Caching voor betere prestaties in Azure API-beheer toevoegen

Bewerkingen in de beheer-API kunnen worden geconfigureerd voor het antwoord in de cache opslaan. Antwoord caching aanzienlijk vermindert de API latentie, bandbreedteverbruik, en web-service laden voor gegevens die niet vaak worden gewijzigd.

Deze handleiding wordt beschreven hoe u antwoord in de cache opslaan voor uw API toevoegen en beleidsregels configureren voor de steekproef Echo API-bewerkingen. Vervolgens kunt u de bewerking aanroepen vanuit de portal voor ontwikkelaars om te controleren of de cache in actie.

>[AZURE.NOTE] Voor meer informatie over cache artikelen per sleutel met beleid expressies, Zie [aangepaste caching in Azure API beheer](api-management-sample-cache-by-key.md).

## <a name="prerequisites"></a>Vereisten

Voordat u de stappen in deze handleiding hebt u een exemplaar van de beheer-API service met een API en een product is geconfigureerd. Als u een exemplaar van de service Management API nog niet hebt gemaakt, ziet u [een API Management service-exemplaar maken][] in de zelfstudie [aan de slag met Azure API beheer][] .

## <a name="configure-caching"> </a>Een bewerking voor caching configureren

In deze stap wordt de cache-instellingen van de bewerking **Ophalen Resource (in cache)** van het monster Echo API herzien.

>[AZURE.NOTE] Elk exemplaar van de service Management API is vooraf geconfigureerd met een Echo-API die kan worden gebruikt om te experimenteren met en informatie over het beheer van de API. Zie [aan de slag met Azure API-beheer][]voor meer informatie.

Klik op **beheren** in de klassieke Azure-Portal voor uw service Management API om te beginnen. Hiermee gaat u naar de portal Management API publisher.

![Publisher-portal][api-management-management-console]

Klik op **API's** in het menu **API beheer** aan de linkerkant en klik vervolgens op **Echo API**.

![Echo-API][api-management-echo-api]

Klik op het tabblad **bewerkingen** en klik op de bewerking **Ophalen Resource (in cache)** uit de lijst van **bewerkingen** .

![Echo API operations][api-management-echo-api-operations]

Klik op het tabblad **cache** om de cache-instellingen voor deze bewerking.

![Tabblad opslaan in cache][api-management-caching-tab]

Als u in cache opslaan voor een bewerking, selecteert u het selectievakje **inschakelen** . In dit voorbeeld wordt is in de cache opslaan ingeschakeld.

Elk antwoord bewerking is gekoppeld, op basis van de waarden in de velden **variëren op queryreeksparameters** en **variëren op kop** . Als u meerdere antwoorden op basis van de parameters van query-tekenreeks of koppen in de cache wilt, kunt u ze in deze twee velden.

**Duur** geeft het interval voor het verlopen van de antwoorden in de cache. In dit voorbeeld wordt is het interval **3600** seconden, die gelijk is aan één uur.

De eerste aanvraag aan de bewerking **Ophalen Resource (in cache)** retourneert met behulp van de configuratie opslaan in cache in het volgende voorbeeld een antwoord van de backend-service. Dit antwoord wordt in de cache, door de opgegeven kop- en queryreeksparameters sleutelvelden. Volgende aanroepen voor de bewerking, met overeenkomstige parameters hebben de cache antwoord, totdat de cache duur-interval is verstreken.

## <a name="caching-policies"> </a>Het cache-beleid bekijken

In deze stap maakt controleren u de cache-instellingen voor de bewerking **Ophalen Resource (in cache)** van het monster Echo API.

Wanneer de cache-instellingen worden geconfigureerd voor een bewerking op het tabblad **cache** , cache-beleid toegevoegd voor de bewerking. Dit beleid kunnen worden bekeken en bewerkt in de Groepsbeleid-editor.

Klik op **beleid** van het **Management API** menu aan de linkerkant en selecteer **Echo API / bron (in cache)** uit de vervolgkeuzelijst **bewerking** .

![Beleid scope bewerking][api-management-operation-dropdown]

Hiermee geeft het beleid voor deze bewerking in de Groepsbeleid-editor.

![Beheer-API Systeembeleid-editor][api-management-policy-editor]

De definitie van het beleid voor deze bewerking bevat het beleid dat de configuratie opslaan in cache definiëren die werden gecontroleerd op het tabblad **cache** in de vorige stap.

    <policies>
        <inbound>
            <base />
            <cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
                <vary-by-header>Accept</vary-by-header>
                <vary-by-header>Accept-Charset</vary-by-header>
            </cache-lookup>
            <rewrite-uri template="/resource" />
        </inbound>
        <outbound>
            <base />
            <cache-store caching-mode="cache-on" duration="3600" />
        </outbound>
    </policies>

>[AZURE.NOTE] Wijzigingen aan het beleid in het cachegeheugen in de Systeembeleid-editor worden, doorgevoerd op het tabblad **cache** van een bewerking, en vice versa.

## <a name="test-operation"> </a>Aanroepen van een bewerking en test het in cache opslaan

Als u wilt zien voor caching in actie, noemen we de bewerking vanuit de developer-portal. Klik **Developer-portal** in het bovenste menu rechts.

![Developer-portal][api-management-developer-portal-menu]

**API's** op in het bovenste menu en selecteer **Echo-API**.

![Echo-API][api-management-apis-echo-api]

>Als er slechts één API geconfigureerde of zichtbaar is voor uw account, gaat API's op te klikken u rechtstreeks naar de bewerkingen voor die API.

Selecteer de bewerking **Ophalen Resource (in cache)** en klik op **Open Console**.

![Open console][api-management-open-console]

De console kunt u bewerkingen rechtstreeks vanuit de developer-portal aanroepen.

![Console][api-management-console]

De standaardwaarden voor **param1** en **param2**houden.

Selecteer de gewenste toets uit de vervolgkeuzelijst **abonnement toets** . Als uw account slechts één abonnement heeft, wordt deze al geselecteerd.

**Sampleheader:value1** invoeren in het tekstvak voor **kop aanvragen** .

Klik op **HTTP-Get** en maak een notitie van de reactie headers.

**Sampleheader:value2** invoeren in het tekstvak voor **kop aanvragen** en klik vervolgens op **HTTP Get**.

Houd er rekening mee dat de waarde van **sampleheader** nog steeds **waarde1** in het antwoord. Probeer enkele andere waarden en houd er rekening mee dat het antwoord in de cache van de eerste aanroep is geretourneerd.

**25** in het veld **param2** invoeren en klik op **HTTP Get**.

Opmerking: de waarde van **sampleheader** in het antwoord is nu **waarde2**. Omdat het resultaat van de bewerking zijn gekoppeld door een query-tekenreeks, wordt het vorige antwoord in de cache is niet geretourneerd.

## <a name="next-steps"> </a>Volgende stappen

-   Zie [Caching beleid][] in de [API Management beleidsverwijzing][]voor meer informatie over het beleid voor cache.
-   Voor meer informatie over cache artikelen per sleutel met beleid expressies, Zie [aangepaste caching in Azure API beheer](api-management-sample-cache-by-key.md).

[api-management-management-console]: ./media/api-management-howto-cache/api-management-management-console.png
[api-management-echo-api]: ./media/api-management-howto-cache/api-management-echo-api.png
[api-management-echo-api-operations]: ./media/api-management-howto-cache/api-management-echo-api-operations.png
[api-management-caching-tab]: ./media/api-management-howto-cache/api-management-caching-tab.png
[api-management-operation-dropdown]: ./media/api-management-howto-cache/api-management-operation-dropdown.png
[api-management-policy-editor]: ./media/api-management-howto-cache/api-management-policy-editor.png
[api-management-developer-portal-menu]: ./media/api-management-howto-cache/api-management-developer-portal-menu.png
[api-management-apis-echo-api]: ./media/api-management-howto-cache/api-management-apis-echo-api.png
[api-management-open-console]: ./media/api-management-howto-cache/api-management-open-console.png
[api-management-console]: ./media/api-management-howto-cache/api-management-console.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Aan de slag met Azure API beheer]: api-management-get-started.md

[Referentie voor API Management beleid]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Beleid voor caching]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[Een API Management service-exemplaar maken]: api-management-get-started.md#create-service-instance

[Configure an operation for caching]: #configure-caching
[Review the caching policies]: #caching-policies
[Call an operation and test the caching]: #test-operation
[Next steps]: #next-steps
