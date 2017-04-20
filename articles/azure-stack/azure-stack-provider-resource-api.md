<properties
    pageTitle="Resourcegebruik provider API | Microsoft Azure"
    description="Verwijzing voor Resourcegebruik API, die informatie over het gebruik van de Stack Azure ophalen."
    services="azure-stack"
    documentationCenter=""
    authors="AlfredoPizzirani"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="alfredop"/>

# <a name="provider-resource-usage-api"></a>Resourcegebruik provider API

De term voorziening geldt voor de servicebeheerder en providers van eventuele overgedragen. Serviceadministrators en overgedragen providers de Provider gebruik API kunt weergeven met het gebruik van hun directe huurders. Bijvoorbeeld P0 de API-Provider voor informatie over het gebruik van de P1 en P2 van rechtstreekse gebruik kunt aanroepen en P1 kunt bellen voor informatie over het gebruik van P3 en P4.

![Conceptueel model van provider hiërarchie](media/azure-stack-provider-resource-api/image1.png)


## <a name="api-call-reference"></a>Referentie voor API-aanroep

### <a name="request"></a>Aanvraag

De aanvraag haalt verbruik details voor de aangevraagde abonnementen en voor de gevraagde periode. Er is geen hoofdgedeelte van de aanvraag.

Dit gebruik API is een API-Provider, zodat de beller een eigenaar, medewerker of Reader rol in het abonnement van de provider moet worden toegewezen.

| **Methode**  | **Aanvraag URI** |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
|  Toevoegen        | https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/subscriberUsageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&subscriberId={sub1.1}&api-version=2015-06-01-preview&continuationToken={token-value} |

### <a name="arguments"></a>Argumenten

| **Argument**              | **Beschrijving** |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| *armendpoint*             | Azure Resource Manager eindpunt van uw stapel Azure-omgeving. Het Verdrag Azure Stack is de naam van het eindpunt van de ARM in de indeling https://api. {domein naam}'. Bijvoorbeeld, als de domeinnaam azurestack.local is, vervolgens het eindpunt van de ARM niet https://api.azurestack.local. |
| *subId*                   | Abonnement-ID van de gebruiker die de aanroep. |
| *reportedStartTime*       | De begintijd van de query. De waarde voor *datum/tijd* moet zijn en aan het begin van het uur, zo 13:00 UTC. Voor dagelijkse samenvoegen, moet u deze waarde instelt op middernacht UTC. De indeling is *escaped* ISO 8601, bijvoorbeeld 2015-06-16T18% 3a53% 3a11% 2b00% 3a00Z, waar de dubbele punt is escaped naar % 3a en plus is voorafgegaan aan % 2b beschrijvende URI is. |
| *reportedEndTime*         | De eindtijd van de query. De beperkingen die voor *reportedStartTime gelden* gelden ook voor dit argument. De waarde voor *reportedEndTime* kan niet in de toekomst. |
| *aggregationGranularity*  | Een optionele parameter met twee aparte mogelijke waarden: per dag en per uur. Als de waarden voorstellen, met als de gegevens in de dagelijkse granulatie en de andere is een uur op te lossen. De dagelijkse optie is de standaardinstelling. |
| *subscriberId*            | Abonnement-ID. Als u gefilterde gegevens, is de abonnement-ID van een rechtstreekse huurder van de provider vereist. Als er geen abonnement-ID-parameter wordt opgegeven, retourneert de oproep gebruiksgegevens voor rechtstreekse huurder van de provider. |
| *API versie*             | De versie van het protocol dat wordt gebruikt voor het maken van dit verzoek. 2015-06-01-voorbeeld moet u gebruiken. |
| *continuationToken*       | Token ophalen uit de laatste aanroep van de provider gebruik API. Dit is nodig wanneer een antwoord groter dan 1000 regels is. Dit is de bladwijzer van de voortgang. Als deze niet aanwezig zijn, de gegevens worden opgehaald vanaf het begin van de dag of uur, op basis van de granulatie doorgegeven. |



### <a name="response"></a>Reactie

/Subscriptions/sub1/providers/Microsoft.Commerce/subscriberUsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00 o & reportedEndTime = 2015-06-01T00% 3a00% 3a00% 2b00% 3a00 & aggregationGranularity = dagelijks & subscriberId = sub1.1 & api versie 1.0 =

{

"waarde":\[

{

"id": "/subscriptions/sub1.1/providers/Microsoft.Commerce/UsageAggregate/sub1.1-

meterID1 ',

"name": "sub1.1 meterID1",

"type": "Microsoft.Commerce/UsageAggregate",

'Eigenschappen': {

"subscriptionId": "sub1.1",

"usageStartTime": "2015-03-03T00:00:00 + 00:00",

"usageEndTime": "2015-03-04T00:00:00 + 00:00",

'instanceData': ' {\\"Microsoft.Resources\\": {\\"resourceUri\\":\\"resourceUri1\\",\\'locatie\\

":\\" Alaska\\",\\" tags\\': null,\\"additionalInfo\\": null}} ",

"aantal"-:2.4000000000

"meterId": "meterID1"

}

},

…

### <a name="response-details"></a>Antwoord details


| **Argument**       | **Beschrijving**
| ------------------ | ------------------------------------------------------------------------------------------------------------- |
| *ID*               | Unieke ID van de samengestelde gebruik
| *naam*             | De naam van de samengestelde gebruik
| *type*             | Bron, definitie
| *subscriptionId*   | Abonnement-id van de gebruiker Azure Stack
| *usageStartTime*   | UTC-begintijd van het gebruik van bucket waartoe deze samenvoeging gebruik behoort
| *usageEndTime*     | Duur van het gebruik van bucket waartoe deze samenvoeging gebruik behoort UTC
| *instanceData*     | Sleutel / waarde-paren van gegevens over exemplaar (in een nieuwe indeling):<br> *resourceUri*: volledige bron-ID, waarin de resourcegroepen en naam van het exemplaar <br> *locatie*: regio waarin deze service wordt uitgevoerd. <br> *tags*: bron codes die zijn opgegeven door de gebruiker <br> *additionalInfo*: meer details over de resource wordt verbruikt bijvoorbeeld OS versie of image type |
| *hoeveelheid*         | Bedrag van het verbruik van de resource die zich hebben voorgedaan in deze periode |
| *meterId*          | De unieke ID voor de resource die is verbruikt (ook wel *ResourceID*) |

## <a name="next-steps"></a>Volgende stappen

[Resourcegebruik huurder API: naslag](azure-stack-tenant-resource-usage-api.md)

[Veelgestelde vragen over het gebruik van gerelateerde](azure-stack-usage-related-faq.md)
