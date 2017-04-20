<properties
    pageTitle="Tabellen met gegevens en het opzoeken in de Stream Analytics gebruiken | Microsoft Azure"
    description="Verwijzing naar gegevens in een Stream Analytics query gebruiken"
    keywords="referentiegegevens opzoektabel"
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

# <a name="using-reference-data-or-lookup-tables-in-a-stream-analytics-input-stream"></a>Met behulp van tabellen met gegevens of opzoeken in een Stream Analytics-invoerstroom

Referentiegegevens (ook bekend als een opzoektabel) is een eindige verzameling van gegevens die statisch is of vertragen wijzigen in de natuur, gebruikt een zoekopdracht uitvoeren of om te correleren met de gegevensstroom. Gebruik van verwijzingsgegevens in uw project Azure Stream Analytics wordt meestal een [Verwijzing naar lid worden van gegevens](https://msdn.microsoft.com/library/azure/dn949258.aspx) in uw Query gebruiken. Stream Analytics gebruikt Azure Blob-opslag als de opslaglaag van referentiegegevens en met verwijzing naar Azure Data Factory gegevens kunnen worden getransformeerd en/of gekopieerd naar Azure Blob-opslag, voor gebruik als referentie-gegevens uit [een willekeurig aantal wolk gebaseerd en opgeslagen gegevens lokalen](../data-factory/data-factory-data-movement-activities.md). Referentiegegevens wordt gemodelleerd als een reeks van BLOB's (zoals gedefinieerd in de configuratie van de input) in oplopende volgorde van de datum en tijd in de blob-naam opgegeven. Het ondersteunt **alleen** toe te voegen aan het einde van de reeks met behulp van een datum/tijd **groter is** dan het opgegeven door de laatste blob in de reeks.

## <a name="configuring-reference-data"></a>Referentiegegevens configureren

Als u wilt uw referentiegegevens configureren, moet u eerst maken een invoer van type **Verwijzingsgegevens**. In de onderstaande tabel wordt elke eigenschap die u opgeven moet tijdens het maken van de referentiegegevens van invoer met de beschrijving ervan uitgelegd:

<table>
<tbody>
<tr>
<td>Naam van eigenschap</td>
<td>Beschrijving</td>
</tr>
<tr>
<td>Invoer-Alias</td>
<td>Een beschrijvende naam die wordt gebruikt in de query voor de taak om te verwijzen naar deze invoer.</td>
</tr>
<tr>
<td>Opslag Account</td>
<td>De naam van de account van de opslag waar de blob-bestanden zich bevinden. Als het in het abonnement hetzelfde als de taak van de Stream Analytics, kunt u het selecteren in de vervolgkeuzelijst naar beneden.</td>
</tr>
<tr>
<td>Opslag Account sleutel</td>
<td>De geheime sleutel die is gekoppeld aan de account van de opslag. Dit wordt automatisch ingevuld als de rekening van de opslag in het abonnement hetzelfde als de taak van de Stream Analytics.</td>
</tr>
<tr>
<td>Opslag Container</td>
<td>Containers bieden een logische groepering voor opgeslagen in de service Microsoft Azure Blob BLOB's. Wanneer u een blob naar de Blob-service uploaden, moet u een container voor die blob.</td>
</tr>
<tr>
<td>Pad patroon</td>
<td>Het pad voor het vinden van de BLOB's in de opgegeven container. U kunt kiezen in het pad, geeft u een of meer exemplaren van de volgende 2 variabelen:<BR>{datum}, {time}<BR>Voorbeeld 1: products/{date}/{time}/product-list.csv<BR>Voorbeeld 2: products/{date}/product-list.csv
</tr>
<tr>
<td>Datumnotatie [optioneel]</td>
<td>Als u {datum} binnen het pad dat u hebt opgegeven gebruikt hebt, kunt u de notatie waarin de bestanden worden geordend selecteren uit de vervolgkeuzelijst van ondersteunde indelingen. Voorbeeld: Jjjj/MM/DD</td>
</tr>
<tr>
<td>Tijdnotatie [optioneel]</td>
<td>Als u {time} binnen het pad dat u hebt opgegeven gebruikt, kunt u de indeling waarin de bestanden worden geordend selecteren uit de vervolgkeuzelijst van ondersteunde indelingen. Voorbeeld: HH</td>
</tr>
<tr>
<td>Gebeurtenis serialisatie-indeling</td>
<td>Om ervoor te zorgen dat uw query's werken zoals u verwacht, Stream Analytics moet weten welke indeling serialisatie gebruikt u voor binnenkomende gegevensstromen. De ondersteunde indelingen zijn voor referentiegegevens, CSV- en JSON.</td>
</tr>
<tr>
<td>Codering</td>
<td>UTF-8 is de enige ondersteunde coderingsindeling op dit moment</td>
</tr>
</tbody>
</table>

## <a name="generating-reference-data-on-a-schedule"></a>Genereren van verwijzingsgegevens van een schema

Als uw referentiegegevens een langzaam veranderende gegevensverzameling is, vervolgens ondersteuning voor het vernieuwen van gegevens is ingeschakeld door het patroon van een pad op te geven in de input-configuratie met de {datum} referentie en vervanging tokens {time}. De verwijzing bijgewerkte definities op basis van dit patroon pad komt stream Analytics ophalen. Bijvoorbeeld, een patroon van `sample/{date}/{time}/products.csv` met een datumnotatie **"jjjj-MM-DD"** en een tijdstip van indeling van **'Uu'** Stream Analytics om op te halen de bijgewerkte blob geïnstrueerd `sample/2015-04-16/17:30/products.csv` op 5:30 PM April 16de 2015 UTC tijdzone.

> [AZURE.NOTE] Momenteel zoeken Stream Analytics taken voor de vernieuwing van de blob alleen als de systeemtijd wordt bestuurd en de tijd die is gecodeerd in de naam van de blob. De taak wordt bijvoorbeeld gezocht naar `sample/2015-04-16/17:30/products.csv` zo spoedig mogelijk, maar niet eerder dan 5:30 PM April 16de 2015 UTC tijdzone. Het zal *nooit* zoeken naar een bestand met een gecodeerde tijd eerder dan de laatste versie die is ontdekt.
> 
> Bv. Zodra de taak de blob zoekt `sample/2015-04-16/17:30/products.csv` negeert deze bestanden met een gecodeerde een eerdere datum dan 5:30 PM April 16de 2015 dus als een late aankomst `sample/2015-04-16/17:25/products.csv` blob wordt gemaakt in dezelfde container de taak wordt niet gebruikt.
> 
> Ook als `sample/2015-04-16/17:30/products.csv` 10:03 PM April 16de 2015 alleen is geproduceerd, maar geen blob met een eerdere datum is aanwezig in de container, de taak wordt dit bestand vanaf April 16de 2015 10:03 PM en de referentiegegevens van vorige gebruiken tot die tijd.
> 
> Een uitzondering hierop is wanneer de taak opnieuw verwerken van gegevens in de tijd moet of wanneer de taak eerst gestart. Start tijd die is opgegeven bij die de taak is op zoek naar de meest recente blob geproduceerd voordat de taak start. Dit is gedaan om ervoor te zorgen dat er een **niet-lege** verwijzing gegevensset is wanneer de taak wordt gestart. Als er een wordt gevonden, verschijnt het project de volgende diagnostische: `Initializing input without a valid reference data blob for UTC time <start time>`.


[Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) kan goedkeuringen van de taak voor het maken van de bijgewerkte blobs gegevensdefinities referentie bijwerken door Analytics Stream moet worden gebruikt. Data Factory is een cloud-gebaseerde data integratieservice orchestrates en automatiseert het verkeer en de transformatie van gegevens. Data Factory ondersteunt [verbinding maakt met een groot aantal cloud-gebaseerd en gegevensopslag op gebouwen](../data-factory/data-factory-data-movement-activities.md) en bewegende gegevens gemakkelijk op gezette tijden die u opgeeft. Bekijk voor meer informatie en stap voor stap instructies voor het instellen van een pijpleiding Data Factory referentiegegevens voor Stream Analytics die worden vernieuwd op een vooraf gedefinieerd schema genereren dit [monster GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs).

## <a name="tips-on-refreshing-your-reference-data"></a>Tips voor het vernieuwen van de verwijzingsgegevens van uw ##

1. BLOB's reference-gegevens worden overschreven, wordt stroom Analytics opnieuw laden van de blob en in sommige gevallen kan het gebeuren dat de taak niet meer functioneert. De aanbevolen manier om verwijzingen te wijzigen is het toevoegen van een nieuwe blob met dezelfde container en patroon pad gedefinieerd in de functie input en gebruikt u een datum/tijd **groter is** dan het opgegeven door de laatste blob in de reeks.
2.  BLOB's worden niet gesorteerd door van de blob "Laatst gewijzigd" tijd, maar alleen door de tijd en de datum die is opgegeven in de blob referentiegegevens naam met de {datum} en {time} vervangingen.
3.  In enkele gevallen die een taak moet terug in de tijd kan moeten daarom verwijzing gegevens BLOB's niet worden gewijzigd of verwijderd.

## <a name="get-help"></a>Help-informatie opvragen
Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) voor verdere ondersteuning

## <a name="next-steps"></a>Volgende stappen
U hebt nu enkele gegevensstroom Analytics, een beheerde service voor het streamen van analytics op gegevens uit de Internet zaken. Voor meer informatie over deze service, Zie:

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
