<properties 
    pageTitle="Azure API Management beleidsverwijzing" 
    description="Meer informatie over het beleid dat beschikbaar is voor het configureren van de beheer-API." 
    services="api-management" 
    documentationCenter="" 
    authors="vladvino" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="apimpm"/>

# <a name="azure-api-management-policy-reference"></a>Azure API Management beleidsverwijzing

Deze sectie bevat een index voor het beleid in de [referentie voor API Management beleid][]. Zie [beleid voor beheer van API][]voor meer informatie over het toevoegen en configureren van beleid.

Beleid-expressies kunnen worden gebruikt als het kenmerkwaarden of tekst in een van de API informatiebeheerbeleid tenzij u iets anders heeft het beleid. Sommige beleid zoals het beleid voor [Besturing][] en [variabele][] zijn gebaseerd op expressies van beleid. Zie [Geavanceerde beleid][] en [beleid voor expressies][] voor meer informatie

## <a name="policy-reference-index"></a>Beleid voor referentie-index

-   [Toegang tot softwarebeperkingsbeleid][]
    -   [Controleer HTTP-header][] - Hiermee wordt het bestaan en/of de waarde van een HTTP-Header.
    -   [Limiet call rate abonnement][] - API wordt voorkomen dat gebruik wordt bereikt door het beperken van de snelheid op basis van abonnement per oproep.
    -   [Limiet call rate via sleutel](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) - API wordt voorkomen dat gebruik wordt bereikt door het beperken van de snelheid op basis van sleutel per oproep.
    -   [Restrict beller IPs][] - Filters weigert (toestaat /) oproepen van bepaalde IP-adressen en/of de adresbereiken.
    -   [Instellen van de quota voor abonnement][] - kunt u om af te dwingen een hernieuwbare of levensduur oproep volume en/of bandbreedte contingent, op basis van per abonnement.
    -   [Instellen van computerquota voor gebruik door de sleutel](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) - kunt u een hernieuwbare of levensduur oproep volume en/of bandbreedte contingent, op basis van per toets afdwingen.
    -   [JWT valideren][] - Hiermee wordt het bestaan en de geldigheid van een JWT gewonnen uit een opgegeven HTTP-Header of een opgegeven query-parameter.
-   [Geavanceerde beleidsregels][]
    -   [Besturing][] - toepassing voorwaardelijk beleidsverklaringen op basis van de resultaten van de evaluatie van Boole- [expressies][].
    -   [Aanvragen doorsturen][] - de aanvraag doorgestuurd naar de backend-service.
    -   [Logboek op gebeurtenis Hub][] - verzonden berichten in de opgegeven indeling toe aan een bericht doel gedefinieerd door een entiteit [Logger](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) .
    -   [Opnieuw proberen](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) - pogingen worden uitgevoerd van de bijgevoegde beleidsverklaringen als en totdat de voorwaarde is voldaan. Uitvoering wordt met de opgegeven tussenpozen herhalen en het aantal herhaalpogingen tot de opgegeven.
    -   [Antwoord retourneren](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) - pipeline-uitvoering wordt afgebroken en retourneert de opgegeven reactie rechtstreeks naar de aanroepende functie.
    -   [Verzendt een manier](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) - verzonden een verzoek naar de opgegeven URL zonder te wachten op een antwoord.
    -   [Aanvraag versturen](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) - verzonden een verzoek naar de opgegeven URL.
    -   [Set aanvraagmethode](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) - kunt u de HTTP-methode voor een aanvraag te wijzigen.
    -   [Status instellen](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) - Hiermee wijzigt u de HTTP-statuscode op de opgegeven waarde.
    -   [Variabele][] - een waarde in een variabele met de naam [context][] voor later toegang blijft bestaan.
    -   [Trace](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) - wordt een tekenreeks toegevoegd in de uitvoer van de [API-inspecteur](../api-management/api-management-howto-api-inspector.md) .
    -   [Wacht](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) - wacht ingesloten verzenden verzoek waarde ophalen uit de cache of het beleid voor stroom te voltooien voordat u verdergaat.
-   [Verificatiebeleid voor][]
    -   [Verifiëren met de Basic][] - verifiëren met een back-end-service met behulp van basisverificatie.
    -   [Verifiëren met dit certificaat][] - verifiëren met een back-end-service met behulp van clientcertificaten.
-   [Beleid voor caching][] 
    -   [Ophalen van cache][] - cache uitvoeren opzoeken en geven een geldig antwoord in de cache wanneer beschikbaar.
    -   [Archief cache][] - Caches antwoord volgens de configuratie opgegeven cache.
    -   [Haal de waarde uit de cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) - een item in de cache opgeslagen door de sleutel ophalen.
    -   [De waarde opslaan in cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) - archief een item in de cache door de sleutel.
    -   [Verwijder de waarde uit de cache](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) - verwijderen op een item in de cache door de sleutel.
-   [Cross-domeinbeleid][] 
    -   [Toestaan interdomein - gesprekken][] - maakt de API toegankelijk van Adobe Flash en Microsoft Silverlight browser-clients.
    -   [CORS][] - cross oorsprong resource sharing ondersteuning om een bewerking of een API oproepen tussen domeinen van browser-clients toestaan (CORS) toegevoegd.
    -   [JSONP][] - JSON wordt toegevoegd met ondersteuning voor opvulling (JSONP) aan een bewerking of een API voor interdomein-aanroepen vanuit JavaScript browser-clients.
-   [Transformatie van beleid][] 
    -   [JSON converteren naar XML][] - converteert aanvraag of antwoord body van JSON met XML.
    -   [Converteren van XML naar JSON][] - converteert aanvraag of antwoord body van XML naar JSON.
    -   [Zoeken en vervangen string in tekst][] - een subtekenreeks aanvraag of antwoord worden gevonden en vervangen door een andere subtekenreeks.
    -   [Masker-URL's in de inhoud][] - koppelingen (maskers) opnieuw schrijft in het antwoord body zodat deze verwijzen naar de overeenkomstige koppeling via de gateway.
    -   [Backend-service instellen][] - Hiermee wijzigt u de backend-service voor de binnenkomende aanvraag.
    -   [Hoofdtekst ingesteld][] - wordt de berichttekst voor inkomende en uitgaande aanvragen.
    -   [Set HTTP-header][] - waarde wordt toegewezen aan een bestaande respons en/of de verzoek-header of een nieuwe reactie en/of verzoek-header toegevoegd.
    -   [Queryreeksparameter instellen][] - wordt toegevoegd, vervangt de waarde van, of verwijdert queryreeksparameter aanvragen.
    -   [Herschrijven van URL][] - converteert een aanvraag-URL van de openbare vorm aan het formulier door de webservice verwacht.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over expressies beleid, de volgende video.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[Toegang tot softwarebeperkingsbeleid]: https://msdn.microsoft.com/library/azure/dn894078.aspx
[Controleer de HTTP-header]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#CheckHTTPHeader
[Limiet oproep tarief per abonnement]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#LimitCallRate
[Beller IP-adressen beperken]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#RestrictCallerIPs
[Set quota voor abonnement]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#SetUsageQuota
[JWT valideren]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT

[Geavanceerde beleidsregels]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Controlestroom]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variabele]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[expressies]: https://msdn.microsoft.com/library/azure/dn910913.aspx
[context]: https://msdn.microsoft.com/library/azure/ea160028-fc04-4782-aa26-4b8329df3448#ContextVariables
[Aanvragen doorsturen]: https://msdn.microsoft.com/library/azure/dn894085.aspx#ForwardRequest
[Logboek met gebeurtenis-Hub]: https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub

[Verificatiebeleid voor]: https://msdn.microsoft.com/library/azure/dn894079.aspx
[Verificatie met Basic]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#Basic
[Met dit certificaat verifiëren]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#ClientCertificate
[Beleid voor caching]: https://msdn.microsoft.com/library/azure/dn894086.aspx
[Uit de cache ophalen]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#GetFromCache
[Opslaan in cache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#StoreToCache

[Cross-domeinbeleid]: https://msdn.microsoft.com/library/azure/dn894084.aspx
[Interdomein-contact]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#AllowCrossDomainCalls
[CORS]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#CORS
[JSONP]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#JSONP

[Transformatie van beleid]: https://msdn.microsoft.com/library/azure/dn894083.aspx
[JSON converteren naar XML]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertJSONtoXML
[Het converteren van XML naar JSON]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertXMLtoJSON
[Zoeken en vervangen string in tekst]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#Findandreplacestringinbody
[Masker-URL's in de inhoud]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#MaskURLSContent
[Back-end-service instellen]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetBackendService
[Body set]: https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBody
[HTTP-koptekst instellen]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetHTTPheader
[Set queryreeksparameter]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetQueryStringParameter
[Herschrijven van URL]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL



[Beleid in API-beheer]: api-management-howto-policies.md
[Referentie voor API Management beleid]: https://msdn.microsoft.com/library/azure/dn894081.aspx

[Beleid voor expressies]: https://msdn.microsoft.com/library/azure/dn910913.aspx

 
