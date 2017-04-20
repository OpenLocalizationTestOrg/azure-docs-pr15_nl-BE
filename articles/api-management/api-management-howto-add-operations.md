<properties 
    pageTitle="Bewerkingen toevoegen aan een Azure API beheer-API | Microsoft Azure" 
    description="Informatie over bewerkingen toevoegen aan een Azure API beheer-API." 
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

# <a name="how-to-add-operations-to-an-api-in-azure-api-management"></a>Bewerkingen toevoegen aan een Azure API beheer-API

Voordat een API voor beheer-API kan worden gebruikt, kunnen bewerkingen moeten worden toegevoegd. Deze handleiding wordt beschreven hoe toevoegen en configureren van verschillende soorten operaties met een API voor beheer-API.

## <a name="add-operation"> </a>Een bewerking toevoegen

Bewerkingen worden toegevoegd en geconfigureerd voor een API in de publisher-portal. Klik op **beheren** in de klassieke Azure-Portal voor uw service Management API voor toegang tot de portal publisher.

![Publisher-portal][api-management-management-console]

>Als u een exemplaar van de service Management API nog niet hebt gemaakt, ziet u [een API Management service-exemplaar maken][] in de zelfstudie [aan de slag met Azure API beheer][] .

Selecteer de gewenste API in de publisher-portal en selecteer vervolgens het tabblad **bewerkingen** . 

![Bewerkingen][api-management-operations]

Klik op **Bewerking toevoegen** als een nieuwe bewerking wilt toevoegen. De **nieuwe bewerking** wordt weergegeven en het tabblad **handtekeningen** wordt standaard geselecteerd.

![Bewerking toevoegen][api-management-add-operation]

Het **HTTP-woord** opgeven door te kiezen uit de vervolgkeuzelijst.

![HTTP-methode][api-management-http-method]

<a name="url-template"></a>

Definieer de URL van sjabloon door te typen in een URL-fragment dat bestaat uit een of meer URL-padsegmenten en nul of meer queryreeksparameters. De URL van sjabloon toegevoegd aan de basis-URL van de API, identificeert een enkele HTTP-bewerking. Bevat mogelijk een of meer benoemde variabele delen, die worden aangegeven met accolades. Deze variabele delen Sjabloonparameters worden genoemd en worden de waarden van de aanvraag-URL is onttrokken wanneer de aanvraag wordt verwerkt door het platform Management API dynamisch toegewezen.

![URL van sjabloon][api-management-url-template]

<a name="rewrite-url-template"></a>

Indien gewenst, kunt u de **URL herschrijven sjabloon**opgeven. Hiermee kunt u de standaard URL-sjabloon gebruiken voor het verwerken van inkomende aanvragen op de front-end tijdens het aanroepen van de back-end via een geconverteerde URL op basis van de sjabloon opnieuw schrijven. Parameters van de URL van sjabloon sjabloon moeten worden gebruikt in de sjabloon opnieuw schrijven. In het volgende voorbeeld ziet u hoe inhoudstype als padsegment in de webservice uit het vorige voorbeeld kan worden opgegeven als een queryparameter in de API gepubliceerd via de API Management platform met behulp van de sjablonen voor URL gecodeerd.

![Herschrijven van URL van sjabloon][api-management-url-template-rewrite]

Bellers naar de bewerking gebruikt u de notatie `/customers?customerid=ALFKI` en deze worden toegewezen aan `/Customers('ALFKI')` wanneer de back-end-service is gestart.


**Weergegeven** naam en **Beschrijving** een beschrijving van de werking en worden gebruikt voor documentatie te verschaffen aan de ontwikkelaars met behulp van deze API in de developer-portal.

![Beschrijving][api-management-description]

De beschrijving van de bewerking kan worden opgegeven als tekst zonder opmaak of HTML-code in het vak **Beschrijving** .

## <a name="operation-caching"> </a>Bewerking opslaan in cache

Antwoord caching wordt latentie verlaagd, waargenomen door de consumenten API, lagere bandbreedteverbruik en vermindert de belasting op het web met HTTP-service implementeert de API. 

Schakel snel en eenvoudig opslaan in de cache voor de bewerking, klik op het tabblad **cache** en schakel het selectievakje **inschakelen** in.

![In cache plaatsen][api-management-caching-tab]

**Duur** geeft aan dat de periode gedurende welke de reactie van de bewerking in de cache blijft. De standaardwaarde is 3600 seconden of 1 uur.

Cache-sleutels worden gebruikt om onderscheid te maken tussen antwoorden zodat het antwoord dat overeenkomt met elke andere Cachesleutel een eigen afzonderlijk in de cache opgeslagen waarde krijgt. U kunt desgewenst bepaalde queryreeksparameters en/of de HTTP-headers moet worden gebruikt bij het berekenen van waarden in de tekstvakken **variëren op queryreeksparameters** en **variëren op headers** cache respectievelijk. Als er geen is opgegeven, volledige aanvraag-URL en de volgende HTTP-headerwaarden worden gebruikt in de cache genereren van sleutels: **accepteren** en **Accept-Charset**.

>Zie voor meer informatie over caching en caching beleid, [het resultaat van de bewerking in Azure API beheer cache][].


## <a name="request-parameters"> </a>Parameters aanvragen

Bewerkingsparameters worden beheerd op het tabblad Parameters. Parameters die zijn opgegeven in de **URL van sjabloon** op het tabblad **handtekening** worden automatisch toegevoegd en kunnen alleen worden gewijzigd met de URL-sjabloon bewerken. Aanvullende parameters kunnen handmatig worden ingevoerd.

Een nieuwe queryparameter toevoegen, klikt u op **Query-Parameter toevoegen** en voer de volgende gegevens:

-   **Name** - de naam van de parameter.
-   **Beschrijving** - een korte beschrijving van de parameter (optioneel).
-   **Type** - het parametertype, geselecteerd in de vervolgkeuzelijst naar beneden.
-   **Waarden** - waarden die kunnen worden toegewezen aan deze parameter. Een van de waarden kan worden gemarkeerd als standaard (optioneel).
-   **Vereist** - de parameter verplicht maken door het selectievakje controleren. 

![Parameters van de aanvraag][api-management-request-parameters]

## <a name="request-body"> </a>Instantie aanvragen

Als de bewerking is toegestaan (bijvoorbeeld PUT, POST) en een instantie kunt u desgewenst een voorbeeld van het in alle notaties van de ondersteunde weergave (bijvoorbeeld json, XML) vereist. 

>Het hoofdgedeelte van de aanvraag dient alleen voor documentatie en is niet gevalideerd.

Ga naar het tabblad **hoofdtekst** wilt invoeren in het hoofdgedeelte van een aanvraag.

Klik op **Weergave toevoegen**, begint te typen (bijvoorbeeld application/json) de naam van het gewenste inhoudstype en selecteren in de vervolgkeuzelijst het gewenste aanvraag instantie voorbeeld in de geselecteerde indeling plakken in het tekstvak. 

![Hoofdgedeelte van de aanvraag][api-management-request-body]

In aanvulling op de opmerkingen, kunt u ook opgeven in het vak **Beschrijving** een optionele beschrijving.

## <a name="responses"> </a>Reacties

Het is een goede gewoonte om voorbeelden van antwoorden voor alle statuscodes die de bewerking kan produceren. Elke statuscode mogelijk meer dan één antwoord instantie bijvoorbeeld één voor elk van de ondersteunde typen inhoud. 

Een antwoord, klikt u op **toevoegen** en typ de gewenste statuscode. In dit voorbeeld de statuscode is **200 OK**. Zodra de code wordt weergegeven in de vervolgkeuzelijst, selecteert u deze en de antwoordcode wordt gemaakt en toegevoegd aan de bewerking.

![Antwoordcode][api-management-response-code]

Klik op **Weergave toevoegen**, typ de naam van het gewenste inhoudstype (bijvoorbeeld application/json) en vervolgens omlaag in de vervolgkeuzelijst selecteren.

![Type inhoud van instantie][api-management-response-body-content-type]

In het voorbeeld van de instantie reactie in de geselecteerde indeling in het tekstvak plakken. 

![Hoofdgedeelte van de Response][api-management-response-body]

Indien gewenst, een optionele beschrijving in het vak **Beschrijving** toevoegen

Zodra de bewerking is geconfigureerd, klikt u op **Opslaan**.


## <a name="next-steps"> </a>Volgende stappen

Nadat de bewerkingen zijn toegevoegd aan een API, is de volgende stap de API koppelt aan een product en publiceren, zodat ontwikkelaars op haar activiteiten aanroepen kunnen.

-   [Het maken en publiceren van een product][]

[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Aan de slag met Azure API beheer]: api-management-get-started.md
[Een API Management service-exemplaar maken]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[Het maken en publiceren van een product]: api-management-howto-add-products.md
[Het resultaat van de bewerking in Azure API beheer cache]: api-management-howto-cache.md