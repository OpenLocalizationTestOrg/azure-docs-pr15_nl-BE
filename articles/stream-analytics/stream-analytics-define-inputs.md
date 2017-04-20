<properties
    pageTitle="Gegevensverbinding: Data stream ingangen van een gebeurtenis stroom | Microsoft Azure"
    description="Informatie over het instellen van een gegevensverbinding naar een Stream Analytics 'invoer' genoemd. "Inputs" zijn een gegevensstroom van gebeurtenissen en ook verwijzen naar gegevens."
    keywords="de gegevensstroom, gegevensverbinding, gebeurtenis-stream"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="data-connection-learn-about-data-stream-inputs-from-events-to-stream-analytics"></a>Gegevensverbinding: informatie over data stream invoer van gebeurtenissen voor Stream Analytics

De gegevensverbinding met Analytics Stream is een gegevensstroom van gebeurtenissen uit een gegevensbron. Dit heet een 'input'. Stream Analytics heeft eersteklas integratie met Azure stream bronnen gebeurtenis Hub, IoT Hub en Blob opslag van gegevens die van dezelfde of een andere Azure abonnement als uw analytics-taak worden kan.

## <a name="data-input-types-data-stream-and-reference-data"></a>Gegevens typen input: gegevens stroom en een verwijzing naar gegevens
Als u gegevens naar een gegevensbron wordt geduwd, is door de taak Analytics stroom verbruikt en in real-time verwerkt. Ingangen worden onderverdeeld in twee verschillende typen: data stream-ingangen en verwijst naar de invoer van gegevens.

### <a name="data-stream-inputs"></a>Gegevensstroom ingangen
Een gegevensstroom is onbegrensde reeks gebeurtenissen die na verloop van tijd. Stream Analytics taken moeten ten minste één stream invoer worden verbruikt en getransformeerd door de taak opnemen. BLOB-opslag en Hubs gebeurtenis IoT Hubs worden ondersteund als data stream invoerbronnen. Gebeurtenis Hubs worden gebruikt om de gebeurtenis streams verzameld via meerdere apparaten en services, zoals sociale media activiteitsfeeds, gegevens over aandelen handel of gegevens van sensoren. IoT Hubs zijn geoptimaliseerd voor het verzamelen van gegevens van de aangesloten apparaten in scenario's Internet dingen (IoT).  BLOB-opslag kan worden gebruikt als invoerbron voor ingesting grote hoeveelheden gegevens als een stroom.  

### <a name="reference-data"></a>Referentiegegevens
Stream Analytics ondersteunt een tweede type invoer bekend als verwijzingsgegevens. Dit is de aanvullende gegevens die statisch of langzaam veranderende na verloop van tijd en meestal gebruikt wordt voor het uitvoeren van de correlatie en op te zoeken. Azure Blob-opslag is momenteel de enige ondersteunde invoerbron voor referentiegegevens. Referentie data source BLOB's zijn beperkt tot 100MB in grootte.
Om verwijzing te maken gegevens ingangen, Zie [Referentie-gegevens gebruiken](stream-analytics-use-reference-data.md)  

## <a name="create-a-data-stream-input-with-an-event-hub"></a>De invoer van een stream met een Hub gebeurtenis maken

[Azure gebeurtenis Hubs](https://azure.microsoft.com/services/event-hubs/) zijn uiterst schaalbaar zijn publicatie abonnementen gebeurtenis ingestor. Deze kan miljoenen gebeurtenissen per seconde, zodat u kunt verwerken en analyseren van de enorme hoeveelheden gegevens die worden geproduceerd door de aangesloten apparaten en toepassingen worden verzameld. Het is een van de meest gebruikte inputs voor de Stream Analytics. Gebeurtenis Hubs en Stream Analytics bieden samen klanten een end-to-end oplossing voor real time analytics. Gebeurtenis Hubs kunnen klanten gebeurtenissen in Azure-feed in real-time en Stream Analytics taken kunnen verwerken in real-time. Klanten kunnen bijvoorbeeld web muisklikken, metingen van de sensor, on line gebeurtenissen gebeurtenis Hubs verzenden en taken gebeurtenis Hubs gebruikt als de invoer gegevensstromen voor real-time filteren en aggregeren en correlatie Stream Analytics maken.

Het is belangrijk te weten dat het standaard-tijdstempel van gebeurtenissen die afkomstig zijn van de gebeurtenis Hubs in Analytics Stream is de tijdstempel die de gebeurtenis in de gebeurtenis Hub EventEnqueuedUtcTime is ontvangen. Voor het verwerken van de gegevens als een stroom met behulp van een tijdstempel in de nettolading van de gebeurtenis, moet het [Tijdstempel door](https://msdn.microsoft.com/library/azure/dn834998.aspx) sleutelwoord worden gebruikt.

### <a name="consumer-groups"></a>Groepen consumenten

Elke input Stream Analytics gebeurtenis Hub moet worden geconfigureerd om een eigen groep van de consument. Wanneer een taak een self-join of meerdere ingangen bevat, kan bepaalde input worden gelezen door meer dan één reader downstream, die van invloed op het aantal lezers in een groep enkele consument. Om te voorkomen dat de limiet van 5 lezers per groep per partitie consument gebeurtenis Hub, is het beste om aan te wijzen voor elke taak Stream Analytics een groep consumenten. Opmerking Het is ook een limiet van 20 groepen consumenten per gebeurtenis Hub. Zie de [Gebeurtenis Hubs Programming Guide](../event-hubs/event-hubs-programming-guide.md)voor meer informatie.

### <a name="configure-event-hub-as-an-input-data-stream"></a>Gebeurtenis Hub configureren als een stroom van ingevoerde gegevens

In de onderstaande tabel wordt elke eigenschap uitgelegd in het geval van Hub invoer tab met de beschrijving ervan:

| NAAM VAN EIGENSCHAP | BESCHRIJVING |
|------|------|
| Invoer-Alias | Een beschrijvende naam die wordt gebruikt in de query voor de taak om te verwijzen naar deze invoer |
| Service Bus Namespace | Een naamruimte Service Bus is een container voor een aantal entiteiten messaging. Wanneer u een nieuwe gebeurtenis Hub hebt gemaakt, is ook een Service Bus-naamruimte gemaakt. |
| Gebeurtenis Hub | De naam van de gebeurtenis Hub invoer. |
| Gebeurtenis Hub-beleidsnaam | Het beleid gedeeld gebruik kan worden gemaakt op het tabblad gebeurtenis Hub configureren. Elk beleid voor gedeelde toegang wordt een naam hebben, machtigingen die u hebt ingesteld, en de toegangstoetsen. |
| Gebeurtenis Hub beleidssleutel | De toegang tot gedeelde sleutel gebruikt voor de verificatie van de naamruimte Service Bus. |
| Gebeurtenis Hub Consumer Group (optioneel) | De groep consumenten aan de consumptie van gegevens van de gebeurtenis Hub. Als niet wordt opgegeven, gebruikt stroom Analytics taken de groep standaard consument te nemen van de gegevens van de gebeurtenis Hub. Het verdient een afzonderlijke consument groep gebruiken voor elke taak Stream Analytics. |
| Gebeurtenis serialisatie-indeling | Om ervoor te zorgen dat uw query's werken zoals u verwacht, Stream Analytics moet weten welke indeling serialisatie (JSON, CSV of Avro) gebruikt u voor binnenkomende gegevensstromen. |
| Codering | UTF-8 is de enige ondersteunde coderingsindeling op dit moment. |

Als uw gegevens is afkomstig van een bron van gebeurtenis Hub, kunt u toegang tot enkele metagegevensvelden in uw query Stream Analytics. De onderstaande tabel worden de velden en de bijbehorende beschrijvingen.

| EIGENSCHAP | BESCHRIJVING |
|------|------|
| EventProcessedUtcTime | De datum en tijd waarop de gebeurtenis is verwerkt door Analytics Stream. |
| EventEnqueuedUtcTime | De datum en tijd waarop de gebeurtenis is ontvangen door de gebeurtenis Hubs. |
| Id van partitie | De op nul gebaseerde partitie-ID voor de invoer-adapter. |

U kan bijvoorbeeld een query als volgt schrijven:

````
SELECT
    EventProcessedUtcTime,
    EventEnqueuedUtcTime,
    PartitionId
FROM Input
````

## <a name="create-an-iot-hub-data-stream-input"></a>Maak een IoT Hub data stream input

Azure Iot Hub is een zeer schaalbare publicatie abonnementen gebeurtenis ingestor geoptimaliseerd voor IoT-scenario's.
Het is belangrijk te weten dat het standaard tijdstempel van gebeurtenissen die afkomstig zijn van IoT Hubs in Analytics Stream is de tijdstempel die de gebeurtenis in de IoT Hub EventEnqueuedUtcTime is ontvangen. Voor het verwerken van de gegevens als een stroom met behulp van een tijdstempel in de nettolading van de gebeurtenis, moet het [Tijdstempel door](https://msdn.microsoft.com/library/azure/dn834998.aspx) sleutelwoord worden gebruikt.

> [AZURE.NOTE] Alleen berichten met een DeviceClient-eigenschap kunnen worden verwerkt.

### <a name="consumer-groups"></a>Groepen consumenten

Elke input Stream Analytics IoT Hub moet worden geconfigureerd om een eigen groep van de consument. Wanneer een taak een self-join of meerdere ingangen bevat, kan bepaalde input worden gelezen door meer dan één reader downstream, die van invloed op het aantal lezers in een groep enkele consument. Om te voorkomen van 5 lezers per groep per partitie consument IoT-Hub is overschreden, is het beste aan te wijzen voor elke taak Stream Analytics een groep consumenten.

### <a name="configure-iot-hub-as-an-data-stream-input"></a>IoT Hub configureren als een gegevensstroom invoer

In de onderstaande tabel wordt elke eigenschap in de IoT Hub invoer tab met de beschrijving ervan uitgelegd:

| NAAM VAN EIGENSCHAP | BESCHRIJVING |
|------|------|
| Invoer-Alias | Een beschrijvende naam die wordt gebruikt in de query voor de taak om te verwijzen naar deze invoer. |
| IoT Hub | Een IoT Hub is een container voor een set van messaging entiteiten. |
| Eindpunt | De naam van uw eindpunt IoT Hub. |
| Gedeelde-beleidsnaam | Het beleid voor gedeelde toegang toegang te verlenen tot de IoT-Hub. Elk beleid voor gedeelde toegang wordt een naam hebben, machtigingen die u hebt ingesteld, en de toegangstoetsen. |
| Gedeelde toegang beleidssleutel | De toegang tot gedeelde sleutel die wordt gebruikt voor het verifiëren van de IoT Hub. |
| Consumer Group (optioneel) | De consument aan de consumptie van gegevens van de IoT Hub-groep. Als niet wordt opgegeven, gebruikt stroom Analytics taken de groep standaard consument te nemen van de gegevens van de IoT Hub. Het verdient een afzonderlijke consument groep gebruiken voor elke taak Stream Analytics. |
| Gebeurtenis serialisatie-indeling | Om ervoor te zorgen dat uw query's werken zoals u verwacht, Stream Analytics moet weten welke indeling serialisatie (JSON, CSV of Avro) gebruikt u voor binnenkomende gegevensstromen. |
| Codering | UTF-8 is de enige ondersteunde coderingsindeling op dit moment. |

Als uw gegevens is afkomstig van een bron IoT Hub, kunt u toegang tot enkele metagegevensvelden in uw query Stream Analytics. De onderstaande tabel worden de velden en de bijbehorende beschrijvingen.

| EIGENSCHAP | BESCHRIJVING |
|------|------|
| EventProcessedUtcTime | De datum en tijd waarop de gebeurtenis is verwerkt. |
| EventEnqueuedUtcTime | De datum en tijd waarop de gebeurtenis is ontvangen door de IoT Hub. |
| Id van partitie | De op nul gebaseerde partitie-ID voor de invoer-adapter. |
| IoTHub.MessageId | Gebruikt voor communicatie in twee richtingen in IoT Hub correleren. |
| IoTHub.CorrelationId | Gebruikt in antwoorden op berichten en feedback in IoT Hub. |
| IoTHub.ConnectionDeviceId | De geverifieerde id gebruikt voor het verzenden van dit bericht, op servicebound-berichten door IoT Hub gestempeld. |
| IoTHub.ConnectionDeviceGenerationId | De generationId van de geverifieerde apparaat dat wordt gebruikt voor het verzenden van dit bericht op servicebound berichten door IoT Hub gestempeld. |
| IoTHub.EnqueuedTime | De tijd waarop het bericht is ontvangen door de IoT Hub. |
| IoTHub.StreamId | Aangepaste eigenschap toegevoegd door het apparaat van de afzender. |

## <a name="create-a-blob-storage-data-stream-input"></a>Een Blob storage data stream input maken

Blob-opslag biedt een kosteneffectieve en schaalbare oplossing voor scenario's met grote hoeveelheden ongestructureerde gegevens op te slaan in de cloud. Gegevens in de [Blob-opslag](https://azure.microsoft.com/services/storage/blobs/) wordt over het algemeen beschouwd als de gegevens 'in rust', maar deze kan worden verwerkt als een gegevensstroom door Analytics Stream. Een gangbare scenario voor Blob storage ingangen met Analytics Stream is logboekverwerking waar telemetrie van een systeem is vastgelegd en moet worden geparseerd en om uit te pakken, betekenisvolle gegevens verwerkt.

Het is belangrijk te weten dat de tijdstempel standaard Blob storage gebeurtenissen in Analytics Stream het tijdstempel is dat de blob welke *isBlobLastModifiedUtcTime*laatst is gewijzigd. Voor het verwerken van de gegevens als een stroom met behulp van een tijdstempel in de nettolading van de gebeurtenis, moet het [Tijdstempel door](https://msdn.microsoft.com/library/azure/dn834998.aspx) sleutelwoord worden gebruikt.

Noteer ook CSV indeling inputs een veldnamenrij voor het definiëren van velden voor de gegevensset **vereist** . Verdere header-rijvelden moet alle **uniek**zijn.

> [AZURE.NOTE] Stream Analytics ondersteunt geen inhoud toevoegen aan een bestaande blob. Stream Analytics wordt alleen een blob weergeven eenmaal en eventuele wijzigingen die zijn gedaan na deze lezen wordt niet verwerkt. De beste manier is om alle gegevens één keer uploaden en geen aanvullende gebeurtenissen toevoegen aan de blob-opslag.

In de volgende tabel wordt uitgelegd dat elke eigenschap in het Blob storage tabblad invoer met de beschrijving:

<table>
<tbody>
<tr>
<td>NAAM VAN EIGENSCHAP</td>
<td>BESCHRIJVING</td>
</tr>
<tr>
<td>Invoer-Alias</td>
<td>Een beschrijvende naam die wordt gebruikt in de query voor de taak om te verwijzen naar deze invoer.</td>
</tr>
<tr>
<td>Opslag Account</td>
<td>De naam van de account van de opslag waar de blob-bestanden zich bevinden.</td>
</tr>
<tr>
<td>Opslag Account sleutel</td>
<td>De geheime sleutel die is gekoppeld aan de account van de opslag.</td>
</tr>
<tr>
<td>Opslag Container
</td>
<td>Containers bieden een logische groepering voor opgeslagen in de service Microsoft Azure Blob BLOB's. Wanneer u een blob naar de Blob-service uploaden, moet u een container voor die blob.</td>
</tr>
<tr>
<td>Pad Prefix patroon [optioneel]</td>
<td>Het pad voor het vinden van de BLOB's in de opgegeven container.
U kunt kiezen in het pad, geeft u een of meer exemplaren van de volgende 3 variabelen:<BR>{datum}, {tijd}<BR>{partitie}<BR>Voorbeeld 1: cluster1/logboeken / {datum} / {time} / {partitioneren}<BR>Voorbeeld 2: cluster1/logboeken / {datum}<P>Merk op dat ' * ' is geen toegestane waarde voor pathprefix. Alleen geldige <a HREF="https://msdn.microsoft.com/library/azure/dd135715.aspx">Azure blob tekens</a> zijn toegestaan.</td>
</tr>
<tr>
<td>Datumnotatie [optioneel]</td>
<td>Als het token van de datum in het pad van het voorvoegsel wordt gebruikt, kunt u de notatie waarin de bestanden worden geordend. Voorbeeld: Jjjj/MM/DD</td>
</tr>
<tr>
<td>Tijdnotatie [optioneel]</td>
<td>Als het token van de tijd in het pad van het voorvoegsel wordt gebruikt, geeft u de indeling waarin de bestanden worden geordend. Op dit moment is de enige ondersteunde waarde uu.</td>
</tr>
<tr>
<td>Gebeurtenis serialisatie-indeling</td>
<td>Om ervoor te zorgen dat uw query's werken zoals u verwacht, Stream Analytics moet weten welke indeling serialisatie (JSON, CSV of Avro) gebruikt u voor binnenkomende gegevensstromen.</td>
</tr>
<tr>
<td>Codering</td>
<td>CSV en JSON is UTF-8 de enige ondersteunde coderingsindeling op dit moment.</td>
</tr>
<tr>
<td>Scheidingsteken</td>
<td>Stream Analytics ondersteunt een aantal algemene scheidingstekens voor het serialiseren gegevens in CSV-indeling. Ondersteunde waarden zijn door komma's, puntkomma, spatie, tab en de verticale balk.</td>
</tr>
</tbody>
</table>

Wanneer uw gegevens is afkomstig van een bron Blob-opslag, kunt u een paar metagegevensvelden in uw query Stream Analytics openen. De onderstaande tabel worden de velden en de bijbehorende beschrijvingen.

| EIGENSCHAP | BESCHRIJVING |
|------|------|
| BlobName | De naam van de invoer blob die de gebeurtenis afkomstig is. |
| EventProcessedUtcTime | De datum en tijd waarop de gebeurtenis is verwerkt door Analytics Stream. |
| BlobLastModifiedUtcTime | De datum en tijd waarop de label voor het laatst is gewijzigd. |
| Id van partitie | De op nul gebaseerde partitie-ID voor de invoer-adapter. |

U kan bijvoorbeeld een query als volgt schrijven:

````
SELECT
    BlobName,
    EventProcessedUtcTime,
    BlobLastModifiedUtcTime
FROM Input
````


## <a name="get-help"></a>Help-informatie opvragen
Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) voor verdere ondersteuning

## <a name="next-steps"></a>Volgende stappen
U hebt geleerd over opties voor gegevensverbinding in Azure voor uw taken Stream Analytics. Meer informatie over de Stream Analytics, Zie:

- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
