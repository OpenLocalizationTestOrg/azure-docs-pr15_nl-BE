<properties
    pageTitle="Veelgestelde vragen met betrekking tot gebruik | Microsoft Azure"
    description="Lijst van Azure Stack meters vergelijking Azure gebruik API, gebruik van tijd en tijd vermeld foutcodes."
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

# <a name="azure-stack-usage-api-faqs"></a>Azure Stack gebruik API-Veelgestelde vragen
Dit artikel bevat antwoorden op enkele veelgestelde vragen over de API Azure Stack-gebruik.

## <a name="what-meter-ids-can-i-see"></a>Welke meter id's kan ik zien?

Op dit moment wordt gebruik voor de netwerk-, opslag- en resource-providers compute gerapporteerd.

| **Bronnen** | **Meter-ID** |**Naam meter** | **Eenheid** | **Extra info** |
| --------------------------- | --------------------------------------- | -------------------------- | ---------------------------- | ----------------------------------------- |
| **Netwerk** | f114cb19-ea64-40b5-bcd7-aee474b62853 | Gebruik van openbare IP-adres | IP-adres |                    
| **Opslag**  | B4438D5D-453B-4EE1-B42A-DC72E377F1E4 | TableCapacity | GB\*uur | Totale capaciteit verbruikt door tabellen |
|              | B5C15376-6C94-4FDD-B655-1A69D138ACA3 | PageBlobCapacity | GB\*uur | Totale capaciteit worden gebruikt door de pagina BLOB 's |
|              | B03C6AE7-B080-4BFA-84A3-22C800F315C6 | QueueCapacity  | GB\*uur  | Totale capaciteit worden gebruikt door de wachtrij |
| | 09F8879E-87E9-4305-A572-4B7BE209F857 | BlockBlobCapacity | GB\*uur  | Totale capaciteit verbruikt door BLOB's blokkeren |
| | B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90 | TableTransactions  | Het aantal aanvragen in 10,000s   | Aanvragen voor tabel (in 10,000s) |
| | 50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D | TableDataTransIn | Ingress-gegevens in GB | Tabel service gegevens ingress in GB |
| | 1B8C1DEC-EE42-414B-AA36-6229CF199370 | TableDataTransOut | Outgress in GB | Tabel service gegevens egress in GB |
| | 43DAF82B-4618-444A-B994-40C23F7CD438 | BlobTransactions | Aanvragen tellen in 10,000s | BLOB aanvragen (in 10,000s) |
| | 9764F92C-E44A-498E-8DC1-AAD66587A810   | BlobDataTransIn    | Ingress-gegevens in GB          | BLOB-service gegevens ingress in GB 
| | 3023FEF4-ECA5-4D7B-87B3-CFBC061931E8   | BlobDataTransOut   | Outgress in GB              | BLOB-service gegevens egress in GB 
| | EB43DD12-1AA6-4C4B-872C-FAF15A6785EA   | QueueTransactions  | Aanvragen tellen in 10,000s   | Aanvragen in de wachtrij (in 10,000s) 
| | E518E809-E369-4A45-9274-2017B29FFF25   | QueueDataTransIn          | Ingress-gegevens in GB         | Queue service gegevens ingress in GB 
| | DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2   | QueueDataTransOut         | Outgress in GB  | Queue service gegevens egress in GB 
| **Berekenen** | 6DAB500F-A4FD-49C4-956D-229BB9C8C793 | VM grootte uren | Grootte van de virtuele machine |



## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsmsdnmicrosoftcomlibraryazure1ea5b323-54bb-423d-916f-190de96c6a3c-currently-in-public-preview"></a>Hoe vergelijk de Azure Stack gebruik API's voor de [Azure gebruik API](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) (momenteel in openbare preview)

-   De huurder gebruik API in overeenstemming is met de API Azure, met één uitzondering: de vlag *showDetails* momenteel wordt niet ondersteund in Azure Stack.

-   De Provider gebruik API alleen van toepassing op Azure Stack.

-   [RateCard API](https://msdn.microsoft.com/en-us/library/azure/mt219004.aspx) die beschikbaar is in Azure is momenteel niet beschikbaar in Azure stapel.

## <a name="what-is-the-difference-between-usage-time-and-reported-time"></a>Wat is het verschil tussen het gebruik van tijd en gerapporteerd?

Rapporten over gebruik van gegevens hebben twee keer belangrijkste waarden:

-   De **gerapporteerde tijd**. De tijd waarop de gebeurtenis met gebruik van het systeem gebruik ingevoerd

-   **Van gebruikstijd**. De tijd waarop de resource Azure Stack is verbruikt

U ziet een verschil in de waarden voor het gebruik van tijd en gerapporteerd voor een gebeurtenis van het specifieke gebruik. De vertraging is zo lang als meerdere uren in elke omgeving.

U kunt zoeken op dit moment *alleen door de gerapporteerde tijd*.

## <a name="what-do-these-usage-api-error-codes-mean"></a>Wat betekenen de foutcodes van deze API gebruik?

| **HTTP-statuscode** | **Foutcode** | **Beschrijving** |
| ---------------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| 400/onjuiste aanvraag        | *NoApiVersion*     | De *api versie* query-parameter ontbreekt.
| 400/onjuiste aanvraag        | *InvalidProperty*  | Een eigenschap ontbreekt of heeft een ongeldige waarde. De ontbrekende eigenschap van het bericht in de foutcode in het hoofdgedeelte van de response wordt aangeduid.
| 400/onjuiste aanvraag        | *RequestEndTimeIsInFuture*  | De waarde voor *ReportedEndTime* is in de toekomst. Waarden worden in de toekomst niet toegestaan voor dit argument.
| 400/onjuiste aanvraag        | *SubscriberIdIsNotDirectTenant*    | Een provider API-aanroep gebruikt een abonnements-ID is niet een geldige huurder van de beller.
| 400/onjuiste aanvraag        | *SubscriptionIdMissingInRequest*   | De abonnement-ID van de beller ontbreekt.
| 400/onjuiste aanvraag        | *InvalidAggregationGranularity*   | Een ongeldige aggregatie granulatie is aangevraagd. Geldige waarden zijn per dag en per uur.
| 503                    | *ServiceUnavailable*   | Er is een herstelbare fout opgetreden omdat de service bezet is of snelheid van de oproep. |

## <a name="next-steps"></a>Volgende stappen
[Klant facturering en terugboeking in Azure stapel](azure-stack-billing-and-chargeback.md)

[Resourcegebruik provider API](azure-stack-provider-resource-api.md)

[Pachters Resourcegebruik API](azure-stack-tenant-resource-usage-api.md)
