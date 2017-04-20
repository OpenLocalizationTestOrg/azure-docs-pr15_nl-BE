<properties
    pageTitle="Pachters Resourcegebruik API | Microsoft Azure"
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

# <a name="tenant-resource-usage-api"></a>Pachters Resourcegebruik API

Een huurder kan de huurder-API gebruiken om de gegevens over het gebruik van huurder resource weer te geven. Deze API is consistent met de API Azure gebruik (momenteel in privé voorbeeld).

U kunt de Windows PowerShell-cmdlet **Get-UsageAggregates** om gebruiksgegevens zoals in Azure.

## <a name="api-call"></a>API-aanroep

### <a name="request"></a>Aanvraag

De aanvraag haalt verbruik details voor de aangevraagde abonnementen en voor de gevraagde periode. Er is geen hoofdgedeelte van de aanvraag.

| **Methode**  | **Aanvraag URI** |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Toevoegen         | https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/usageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&api-version=2015-06-01-preview&continuationToken={token-value} |

### <a name="arguments"></a>Argumenten

| **Argument**             | **Beschrijving** |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| *Armendpoint*             | Azure Resource Manager eindpunt van uw stapel Azure-omgeving. |
| *subId*                   | Abonnement-ID van de gebruiker die de aanroep. U kunt deze API alleen aan de query voor gebruik met één abonnement. Providers kunnen de Provider Resource gebruik API query-syntaxis gebruiken voor alle huurders. |
| *reportedStartTime*       | De begintijd van de query. De waarde voor *datum/tijd* moet zijn en aan het begin van het uur, zo 13:00 UTC. Voor dagelijkse samenvoegen, moet u deze waarde instelt op middernacht UTC. De indeling is *escaped* ISO 8601, bijvoorbeeld 2015-06-16T18% 3a53% 3a11% 2b00% 3a00Z, waar de dubbele punt is escaped naar % 3a en plus is voorafgegaan aan % 2b beschrijvende URI is. |
| *reportedEndTime*         | De eindtijd van de query. De beperkingen die voor *reportedStartTime gelden* gelden ook voor dit argument. De waarde voor *reportedEndTime* kan niet in de toekomst. |
| *aggregationGranularity*  | Een optionele parameter met twee aparte mogelijke waarden: per dag en per uur. Als de waarden voorstellen, met als de gegevens in de dagelijkse granulatie en de andere is een uur op te lossen. De dagelijkse optie is de standaardinstelling. |
| *subscriberId*            | Abonnement-ID. Als u gefilterde gegevens, is de abonnement-ID van een rechtstreekse huurder van de provider vereist. Als er geen abonnement-ID-parameter wordt opgegeven, retourneert de oproep gebruiksgegevens voor rechtstreekse huurder van de provider. |
| *API versie*             | De versie van het protocol dat wordt gebruikt voor het maken van dit verzoek. 2015-06-01-voorbeeld moet u gebruiken. |
| *continuationToken*       | Token ophalen uit de laatste aanroep van de provider gebruik API. Dit is nodig wanneer een antwoord groter dan 1000 regels is. Dit is de bladwijzer van de voortgang. Als deze niet aanwezig zijn, de gegevens worden opgehaald vanaf het begin van de dag of uur, op basis van de granulatie doorgegeven. |

### <a name="response"></a>Reactie

/Subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00 o & reportedEndTime = 2015-06-01T00% 3a00% 3a00% 2b00% 3a00 & aggregationGranularity = dagelijks & api versie 1.0 =

{

"waarde":\[

{

"id": "/ subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregate/sub1-meterID1",

"name": "Abb1 meterID1",

"type": "Microsoft.Commerce/UsageAggregate",

'Eigenschappen': {

"subscriptionId": "Abb1",

"usageStartTime": "2015-03-03T00:00:00 + 00:00",

"usageEndTime": "2015-03-04T00:00:00 + 00:00",

'instanceData': ' {\\"Microsoft.Resources\\": {\\"resourceUri\\":\\"resourceUri1\\",\\"locatie\\":\\"Alaska\\",\\"tags\\": null,\\"additionalInfo\\": null}} ",

"aantal"-:2.4000000000

"meterId": "meterID1"

}

},

…

### <a name="response-details"></a>Antwoord details

| **Argument**      | **Beschrijving** |
| ------------------ | ------------------------------------------------------------------------------------------------------------- |
| *ID*              | Unieke ID van de samengestelde gebruik |
| *naam*            | De naam van de samengestelde gebruik |
| *type*            | Bron, definitie |
| *subscriptionId*  | Abonnement-id van de gebruiker Azure |
| *usageStartTime*  | UTC-begintijd van het gebruik van bucket waartoe deze samenvoeging gebruik behoort |
| *usageEndTime*    | Duur van het gebruik van bucket waartoe deze samenvoeging gebruik behoort UTC |
| *instanceData*    | Sleutel / waarde-paren van gegevens over exemplaar (in een nieuwe indeling):<br>  *resourceUri*: volledige bron-ID, met inbegrip van bronnengroepen en exemplaarnaam <br>  *locatie*: regio waarin deze service wordt uitgevoerd. <br>  *tags*: bron codes waarmee de gebruiker <br>  *additionalInfo*: meer details over de resource wordt verbruikt bijvoorbeeld OS versie of image type |
| *hoeveelheid*        | Bedrag van het verbruik van de resource die zich hebben voorgedaan in deze periode |
| *meterId*         | De unieke ID voor de resource die is verbruikt (ook wel *ResourceID*) |

## <a name="next-steps"></a>Volgende stappen

[Veelgestelde vragen over het gebruik van gerelateerde](azure-stack-usage-related-faq.md)
