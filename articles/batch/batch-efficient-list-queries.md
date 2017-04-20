<properties
    pageTitle="Efficiënte lijst met query's in Batch Azure | Microsoft Azure"
    description="De prestaties verbeteren door het filteren van uw query's bij het aanvragen van informatie over Batch bronnen, zoals toepassingen, projecten, taken en compute nodes."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/25/2016"
    ms.author="marsma" />

# <a name="query-the-azure-batch-service-efficiently"></a>De Batch Azure service efficiënt zoeken

Hier leert u hoe u uw Azure Batch-toepassing om prestaties te verhogen door vermindering van de hoeveelheid gegevens die door de service worden geretourneerd wanneer u projecten, taken, opvragen en met de [Batchverwerking .NET computerknooppunten] [ api_net] bibliotheek.

Bijna alle Batch toepassingen moeten sommige type controle of een andere bewerking die de Batch-service, vaak met regelmatige tussenpozen een query uitvoert. Om te bepalen of er taken in de wachtrij blijven in een taak, kunt u gegevens moet ophalen voor elke taak in het project. Om te bepalen van de status van de knooppunten in uw groep, moet u gegevens ophalen op elk knooppunt in de groep. In dit artikel wordt uitgelegd hoe u dergelijke query's uitvoeren in de meest efficiënte manier.

## <a name="meet-the-detaillevel"></a>Voldoen aan de DetailLevel

In een productie-Batch toepassing kunnen entiteiten zoals projecten, taken en computerknooppunten nummer in duizendtallen. Als u meer informatie over deze bronnen aanvraagt, moet een potentieel grote hoeveelheid gegevens "cross-the wire' uit de Batch-service aan uw toepassing voor elke query. U kunt de snelheid van query's en daarmee de prestaties van uw toepassing vergroten door het aantal items en het soort informatie dat wordt geretourneerd door een query te beperken.

Deze [Batch.NET] [ api_net] API codefragment geeft een overzicht van *alle* taken die aan een taak, samen met *alle* eigenschappen van elke taak is gekoppeld:

```csharp
// Get a collection of all of the tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

Door het toepassen van een 'detailniveau' aan uw query kunt u een lijstquery veel efficiënter echter uitvoeren. U doet dit door het leveren van een [ODATADetailLevel] [ odata] -object om de [JobOperations.ListTasks] te[ net_list_tasks] methode. In dit fragment retourneert alleen de ID, opdrachtregel en compute knooppunt eigenschappen van voltooide taken:

```csharp
// Configure an ODATADetailLevel specifying a subset of tasks and
// their properties to return
ODATADetailLevel detailLevel = new ODATADetailLevel();
detailLevel.FilterClause = "state eq 'completed'";
detailLevel.SelectClause = "id,commandLine,nodeInfo";

// Supply the ODATADetailLevel to the ListTasks method
IPagedEnumerable<CloudTask> completedTasks =
    batchClient.JobOperations.ListTasks("job-001", detailLevel);
```

In dit voorbeeldscenario, als er duizenden taken in het project, zijn de resultaten van de tweede query meestal geretourneerd veel sneller dan de eerste. Meer informatie over het gebruik van ODATADetailLevel wanneer u objecten met de batchverwerking .NET API is opgenomen [onder](#efficient-querying-in-batch-net).

> [AZURE.IMPORTANT]
> We raden opgeven die u *altijd* een ODATADetailLevel-object om te zorgen voor maximale efficiëntie en prestaties van uw toepassing de lijst .NET API aanroepen. Een detailniveau opgeeft, kunt u lagere Batch service responstijden, netwerkgebruik te verbeteren en geheugengebruik minimaliseren door clienttoepassingen.

## <a name="filter-select-and-expand"></a>Filteren, selecteren en vouwen

De [Batch.NET] [ api_net] en de [REST van de partij] [ api_rest] API's bieden de mogelijkheid om te beperken en het aantal van de artikelen die worden geretourneerd in een lijst, maar ook de hoeveelheid informatie die wordt geretourneerd voor elk. Hiervoor **filter**en **Selecteer** **tekenreeksen Vouw** opgeven tijdens het uitvoeren van query's lijst.

### <a name="filter"></a>Filter
De filtertekenreeks is een expressie die een vermindert het aantal items dat wordt geretourneerd. Bijvoorbeeld, lijst met actieve taken van een taak of lijst alleen computerknooppunten die taken uitvoeren.

- De filtertekenreeks bestaat uit een of meer expressies met een expressie die uit een eigenschapnaam, operator en waarde bestaat. De eigenschappen die kunnen worden opgegeven zijn specifiek voor elk entiteitstype dat u een query uitvoert, worden de operatoren die worden ondersteund voor elke eigenschap.
- Meerdere expressies kunnen worden gecombineerd met de logische operatoren `and` en `or`.
- In dit voorbeeld filterlijsten tekenreeks alleen de actieve "renderen" taken: `(state eq 'running') and startswith(id, 'renderTask')`.

### <a name="select"></a>Selecteer
De optie tekenreeks beperkt de waarden die worden geretourneerd voor elk artikel. U geeft een lijst met namen van eigenschappen en alleen die eigenschapswaarden voor de items in de resultaten van de query worden geretourneerd.

- De optie tekenreeks bestaat uit een door komma's gescheiden lijst met namen van eigenschappen. U kunt de eigenschappen voor het entiteitstype dat u wilt zoeken.
- In dit voorbeeld selecteren tekenreeks geeft aan dat slechts drie waarden voor elke taak moeten worden geretourneerd: `id, state, stateTransitionTime`.

### <a name="expand"></a>Vouw
Expand-reeks vermindert het aantal API-aanroepen die nodig zijn om bepaalde informatie te verkrijgen. Als u een tekenreeks uitvouwen, meer informatie over elk item kan worden verkregen met een enkele API-aanroep. In plaats van eerst het verkrijgen van de lijst met entiteiten, vraagt vervolgens gegevens voor elk item in de lijst met een tekenreeks uitvouwen kunt u dezelfde gegevens in een enkele API-aanroep te verkrijgen. Minder API-aanroepen betekent betere prestaties.

- Net als voor de geselecteerde tekenreeks, de expand-reeks bepaalt of bepaalde gegevens in de lijst met queryresultaten worden opgenomen.
- Expand-reeks wordt alleen ondersteund wanneer deze wordt gebruikt in de lijst taken, taakschema, taken en groepen. Op dit moment alleen ondersteunt statistische gegevens.
- Wanneer alle eigenschappen vereist zijn en geen Selecteer tekenreeks wordt opgegeven, worden het uitvouwen tekenreeks *moet* gebruikt om statistische gegevens. Als een select tekenreeks wordt gebruikt voor een subset van eigenschappen, vervolgens `stats` kan worden opgegeven in de reeks selecteren en de expand-reeks hoeft niet te worden opgegeven.
- In dit voorbeeld breiden tekenreeks geeft aan dat de statistische gegevens voor elk item in de lijst moet worden geretourneerd: `stats`.

> [AZURE.NOTE] Bij het maken van een van de drie query-tekenreekstypen (filteren en selecteer uitvouwen), moet u ervoor zorgen dat de namen van eigenschappen en de aanvraag overeenkomen met die van hun tegenhangers REST API-element. Bijvoorbeeld, als u werkt met de .NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask) -klasse, moet u **staat** **staat**, ook al is de eigenschap .NET [CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state). Zie de tabellen hieronder voor toewijzingen tussen de REST API's en .NET.

### <a name="rules-for-filter-select-and-expand-strings"></a>Regels voor het filteren, selecteren en vouw tekenreeksen

- Namen van eigenschappen in filters, selecteer en vouw tekenreeksen moeten worden weergegeven als in de [REST van de partij] [ api_rest] API--zelfs als u [Batch.NET] [ api_net] of een van de andere partij SDK's.
- De namen van alle eigenschappen zijn hoofdlettergevoelig, maar de waarden van eigenschappen zijn niet hoofdlettergevoelig.
- Datum/tijd tekenreeksen kunnen twee verschillende indelingen, en moet worden voorafgegaan door `DateTime`.

  - Voorbeeld van de W3C-DTF-indeling:`creationTime gt DateTime'2011-05-08T08:49:37Z'`
  - Voorbeeld van RFC 1123-indeling:`creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`
- Boolean tekenreeksen zijn `true` of `false`.
- Als een eigenschap is ongeldig of de operator is opgegeven, een `400 (Bad Request)` fout veroorzaakt.

## <a name="efficient-querying-in-batch-net"></a>Efficiënt zoeken in Batch .NET

Binnen de [Batch.NET] [ api_net] API, de [ODATADetailLevel] [ odata] klasse wordt gebruikt voor het verstrekken van filter, en selecteer tekenreeksen lijst activiteiten uitbreiden. De klasse ODataDetailLevel heeft drie openbare tekenreekseigenschappen die kunnen worden opgegeven in de constructor of rechtstreeks op het object zijn ingesteld. U vervolgens het ODataDetailLevel-object als parameter doorgeven aan de verschillende bewerkingen van de lijst zoals [ListPools][net_list_pools], [ListJobs][net_list_jobs], en [ListTasks][net_list_tasks].

- [ODATADetailLevel][odata]. [FilterClause] [ odata_filter]: Het aantal items dat wordt geretourneerd beperken.
- [ODATADetailLevel][odata]. [SelectClause] [ odata_select]: Opgeven welke eigenschapswaarden worden weergegeven bij elk item.
- [ODATADetailLevel][odata]. [ExpandClause] [ odata_expand]: Gegevens ophalen voor alle items in één API-aanroepen in plaats van afzonderlijke oproepen voor elk artikel.

Het volgende codefragment wordt de Batch .NET API efficiënt query uitvoeren op de Batch-service voor de statistieken van een bepaalde set groepen. In dit scenario heeft de gebruiker Batch test- en productie van toepassingen. De groep test ID's worden voorafgegaan door 'test' en de productie van toepassingen id's worden voorafgegaan door "Prod.-order". In het fragment is *myBatchClient* een goed geïnitialiseerd exemplaar van de klasse van de [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient) .

```csharp
// First we need an ODATADetailLevel instance on which to set the filter, select,
// and expand clause strings
ODATADetailLevel detailLevel = new ODATADetailLevel();

// We want to pull only the "test" pools, so we limit the number of items returned
// by using a FilterClause and specifying that the pool IDs must start with "test"
detailLevel.FilterClause = "startswith(id, 'test')";

// To further limit the data that crosses the wire, configure the SelectClause to
// limit the properties that are returned on each CloudPool object to only
// CloudPool.Id and CloudPool.Statistics
detailLevel.SelectClause = "id, stats";

// Specify the ExpandClause so that the .NET API pulls the statistics for the
// CloudPools in a single underlying REST API call. Note that we use the pool's
// REST API element name "stats" here as opposed to "Statistics" as it appears in
// the .NET API (CloudPool.Statistics)
detailLevel.ExpandClause = "stats";

// Now get our collection of pools, minimizing the amount of data that is returned
// by specifying the detail level that we configured above
List<CloudPool> testPools =
    await myBatchClient.PoolOperations.ListPools(detailLevel).ToListAsync();
```

> [AZURE.TIP] Een exemplaar van [ODATADetailLevel] [ odata] die is geconfigureerd met het selecteren en uitvouwen componenten kunnen ook worden doorgegeven aan passende Get methoden, zoals [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx), de hoeveelheid gegevens die wordt geretourneerd beperken.

## <a name="batch-rest-to-net-api-mappings"></a>Batch REST aan toewijzingen voor .NET API

De namen van eigenschappen in filters, selecteer en vouw tekenreeksen *moet* overeenkomen met hun tegenhangers REST API, zowel in de naam en het geval. De onderstaande tabellen bevatten toewijzingen tussen de tegenposten .NET en REST API.

### <a name="mappings-for-filter-strings"></a>Toewijzingen voor tekenreeksen

- **.NET lijst methoden**: elk van de .NET API methoden in deze kolom kan een [ODATADetailLevel] [ odata] -object als parameter.
- **REST-lijst aanvragen**: REST-API voor elke pagina is gekoppeld aan de in deze kolom bevat een tabel die de eigenschappen en bewerkingen die zijn toegestaan in *tekenreeksen* . U kunt deze namen van eigenschappen en bewerkingen wilt gebruiken bij het samenstellen van een [ODATADetailLevel.FilterClause] [ odata_filter] string.

| .NET lijst methoden | REST-lijst aanvragen |
|---|---|
| [CertificateOperations.ListCertificates][net_list_certs] | [Lijst met certificaten op een rekening][rest_list_certs]
| [CloudTask.ListNodeFiles][net_list_task_files] | [De bestanden die zijn gekoppeld aan een taak][rest_list_task_files]
| [JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status] | [De status van de voorbereiding van de taak en de release taken voor een project weergeven][rest_list_jobprep_status]
| [JobOperations.ListJobs][net_list_jobs] | [Overzicht van de taken in een account][rest_list_jobs]
| [JobOperations.ListNodeFiles][net_list_nodefiles] | [De bestanden op een knooppunt][rest_list_nodefiles]
| [JobOperations.ListTasks][net_list_tasks] | [Een lijst met de taken die zijn gekoppeld aan een taak][rest_list_tasks]
| [JobScheduleOperations.ListJobSchedules][net_list_job_schedules] | [Het taakschema in een account weergeven][rest_list_job_schedules]
| [JobScheduleOperations.ListJobs][net_list_schedule_jobs] | [Overzicht van de taken die zijn gekoppeld aan een projectplanning][rest_list_schedule_jobs]
| [PoolOperations.ListComputeNodes][net_list_compute_nodes] | [De compute nodes in een groep weergeven][rest_list_compute_nodes]
| [PoolOperations.ListPools][net_list_pools] | [Lijst van de toepassingen in een account][rest_list_pools]

### <a name="mappings-for-select-strings"></a>Toewijzingen voor tekenreeksen selecteren

- **Batch .NET typen**: Batch .NET API typen.
- **REST API entiteiten**: elke pagina in deze kolom bevat een of meer tabellen die de namen van de eigenschap REST API voor het type. Deze namen worden gebruikt bij het samenstellen van tekenreeksen *selecteren* . U gebruikt deze dezelfde namen als u een [ODATADetailLevel.SelectClause] [ odata_select] string.

| Batch .NET typen | REST API entiteiten |
|---|---|
| [Certificaat][net_cert] | [Informatie opvragen over een certificaat][rest_get_cert] |
| [CloudJob][net_job] | [Informatie opvragen over een taak][rest_get_job] |
| [CloudJobSchedule][net_schedule] | [Informatie opvragen over een taak plannen][rest_get_schedule] |
| [ComputeNode][net_node] | [Informatie opvragen over een knooppunt][rest_get_node] |
| [CloudPool][net_pool] | [Informatie over een groep][rest_get_pool] |
| [CloudTask][net_task] | [Informatie opvragen over een taak][rest_get_task] |

## <a name="example-construct-a-filter-string"></a>Voorbeeld: een filtertekenreeks maken

Als u een filtertekenreeks maken voor [ODATADetailLevel.FilterClause][odata_filter], raadpleegt u de tabel hierboven onder "Toewijzingen voor tekenreeksen" om te zoeken naar de REST API documentatiepagina die overeenkomt met de lijstbewerking die u wilt uitvoeren. Vindt u de Filterbaar eigenschappen en hun ondersteunde operators in de eerste multirow tabel op die pagina. Als u wilt dat alle taken waarvan u de code exit niet nul was, bijvoorbeeld deze rij in de [lijst met taken die zijn gekoppeld aan een taak] ophalen[ rest_list_tasks] geeft de tekenreeks voor de eigenschap van toepassing en de toegestane operators:

| Eigenschap | Toegestane bewerkingen | Type |
| :--- | :--- | :--- |
| `executionInfo/exitCode` | `eq, ge, gt, le , lt` | `Int` |

Dus zou de filterreeks voor het aanbieden van alle taken met een afsluitcode van nul zijn:

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a>Voorbeeld: een select-tekenreeks maken

Maken van [ODATADetailLevel.SelectClause][odata_select], neem contact op met de bovenstaande tabel onder 'Toewijzingen voor tekenreeksen selecteren' en navigeer naar de pagina met REST API die overeenkomt met het type entiteit dat u aanbiedt. Vindt u de eigenschappen te selecteren en de ondersteunde operators in de eerste multirow tabel op die pagina. Als u wilt dat alleen de ID en de opdrachtregel voor elke taak in een lijst, bijvoorbeeld, vindt u deze rijen in de tabel van toepassing op [informatie opvragen over een taak][rest_get_task]:

| Eigenschap | Type | Notities |
| :--- | :--- | :--- |
| `id` | `String` | `The ID of the task.` |
| `commandLine` | `String` | `The command line of the task.` |

Selecteer de string zou voor alleen de ID en de opdrachtregel bij elke taak weergegeven met inbegrip van vervolgens zijn:

`id, commandLine`

## <a name="code-samples"></a>Codevoorbeelden

### <a name="efficient-list-queries-code-sample"></a>Voorbeeldcode voor efficiënte lijst met query 's

Bekijk de [EfficientListQueries] [ efficient_query_sample] voorbeeldproject op GitHub om te zien hoe efficiënt lijst doorzoeken kan invloed hebben op prestaties in een toepassing. Deze C#-consoletoepassing wordt gemaakt en wordt een groot aantal taken toegevoegd aan een taak. Vervolgens het maakt meerdere aanroepen van de [JobOperations.ListTasks] [ net_list_tasks] methode en gangen [ODATADetailLevel] [ odata] -objecten die zijn geconfigureerd met verschillende waarden variëren van de hoeveelheid gegevens die moet worden geretourneerd. Produceert de volgende uitvoer:

```
Adding 5000 tasks to job jobEffQuery...
5000 tasks added in 00:00:47.3467587, hit ENTER to query tasks...

4943 tasks retrieved in 00:00:04.3408081 (ExpandClause:  | FilterClause: state eq 'active' | SelectClause: id,state)
0 tasks retrieved in 00:00:00.2662920 (ExpandClause:  | FilterClause: state eq 'running' | SelectClause: id,state)
59 tasks retrieved in 00:00:00.3337760 (ExpandClause:  | FilterClause: state eq 'completed' | SelectClause: id,state)
5000 tasks retrieved in 00:00:04.1429881 (ExpandClause:  | FilterClause:  | SelectClause: id,state)
5000 tasks retrieved in 00:00:15.1016127 (ExpandClause:  | FilterClause:  | SelectClause: id,state,environmentSettings)
5000 tasks retrieved in 00:00:17.0548145 (ExpandClause: stats | FilterClause:  | SelectClause: )

Sample complete, hit ENTER to continue...
```

Zoals in de verstreken tijd, kunt u query responstijden aanzienlijk verlagen door de eigenschappen en het aantal items dat wordt geretourneerd beperken. Vindt u deze en andere voorbeeldprojecten in [azure-batch-monsters] [ github_samples] bibliotheek op GitHub.

### <a name="batchmetrics-library-and-code-sample"></a>BatchMetrics-bibliotheek en de code monster

Naast het codevoorbeeld EfficientListQueries hierboven vindt u de [BatchMetrics] [ batch_metrics] -project in de [voorbeelden van azure batch] [ github_samples] GitHub opslagplaats. Het voorbeeldproject BatchMetrics laat zien hoe efficiënt Azure Batch job om voortgang te controleren met behulp van de Batch-API.

De [BatchMetrics] [ batch_metrics] steekproef omvat een .NET class library-project dat u kunt opnemen in uw eigen projecten en een eenvoudig opdrachtregelprogramma programma om te oefenen en voorbeelden van het gebruik van de bibliotheek.

De voorbeeldtoepassing binnen het project ziet u de volgende bewerkingen:

1. Specifieke kenmerken selecteren om alleen de eigenschappen die u moet downloaden
2. Filteren op status overgangstijden wilt downloaden alleen wijzigingen sinds de laatste query

De volgende methode wordt bijvoorbeeld weergegeven in de bibliotheek BatchMetrics. Het resultaat van een ODATADetailLevel die dat alleen aangeeft de `id` en `state` eigenschappen moeten worden verkregen voor de entiteiten die worden doorzocht. Ook bevat die alleen entiteiten waarvan de status is gewijzigd sinds u het opgegeven `DateTime` parameter moet worden geretourneerd.

```csharp
internal static ODATADetailLevel OnlyChangedAfter(DateTime time)
{
    return new ODATADetailLevel(
        selectClause: "id, state",
        filterClause: string.Format("stateTransitionTime gt DateTime'{0:o}'", time)
    );
}
```

## <a name="next-steps"></a>Volgende stappen

### <a name="parallel-node-tasks"></a>Knooppunt parallelle taken

[Azure Batch maximaliseren berekenen Resourcegebruik met gelijktijdige knooppunt taken](batch-parallel-node-tasks.md) is een ander artikel dat betrekking heeft op de prestaties van toepassingen. Sommige soorten werklasten kunnen profiteren van het uitvoeren van parallelle taken op grotere-- maar minder--computerknooppunten. Bekijk het [voorbeeldscenario](batch-parallel-node-tasks.md#example-scenario) in het artikel voor meer informatie over dit scenario.

### <a name="batch-forum"></a>Batch-Forum

Het [Forum van Azure Batch] [ forum] is een prima plek om te bespreken Batch en vragen stellen over de service op MSDN. Hoofd op over voor nuttige 'vastzetten' geposte berichten en vragen posten dat ze zich voordoen terwijl u uw Batch oplossingen bouwen.


[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_metrics]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchMetrics
[efficient_query_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_samples]: https://github.com/Azure/azure-batch-samples
[odata]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[odata_ctor]: https://msdn.microsoft.com/library/azure/dn866178.aspx
[odata_expand]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.expandclause.aspx
[odata_filter]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.filterclause.aspx
[odata_select]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.selectclause.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[rest_list_certs]: https://msdn.microsoft.com/library/azure/dn820154.aspx
[rest_list_compute_nodes]: https://msdn.microsoft.com/library/azure/dn820159.aspx
[rest_list_job_schedules]: https://msdn.microsoft.com/library/azure/mt282174.aspx
[rest_list_jobprep_status]: https://msdn.microsoft.com/library/azure/mt282170.aspx
[rest_list_jobs]: https://msdn.microsoft.com/library/azure/dn820117.aspx
[rest_list_nodefiles]: https://msdn.microsoft.com/library/azure/dn820151.aspx
[rest_list_pools]: https://msdn.microsoft.com/library/azure/dn820101.aspx
[rest_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/mt282169.aspx
[rest_list_task_files]: https://msdn.microsoft.com/library/azure/dn820142.aspx
[rest_list_tasks]: https://msdn.microsoft.com/library/azure/dn820187.aspx

[rest_get_cert]: https://msdn.microsoft.com/library/azure/dn820176.aspx
[rest_get_job]: https://msdn.microsoft.com/library/azure/dn820106.aspx
[rest_get_node]: https://msdn.microsoft.com/library/azure/dn820168.aspx
[rest_get_pool]: https://msdn.microsoft.com/library/azure/dn820165.aspx
[rest_get_schedule]: https://msdn.microsoft.com/library/azure/mt282171.aspx
[rest_get_task]: https://msdn.microsoft.com/library/azure/dn820133.aspx

[net_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificate.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_schedule]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjobschedule.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
