<properties 
    pageTitle="Maatstaven voor opslag in de portal Azure inschakelen | Microsoft Azure" 
    description="Opslag maatstaven voor de Blob, wachtrij, tabel en bestand services inschakelen" 
    services="storage" 
    documentationCenter="" 
    authors="robinsh" 
    manager="carmonm" 
    editor="tysonn"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/03/2016" 
    ms.author="robinsh"/>

# <a name="enabling-storage-metrics-and-viewing-metrics-data"></a>Opslag metrics inschakelen en weergeven van metrische gegevens

[AZURE.INCLUDE [storage-selector-portal-enable-and-view-metrics](../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a>Overzicht

Opslag Metrics is standaard niet ingeschakeld voor uw opslagservices. U kunt controleren of de [Klassieke Portal Azure](https://manage.windowsazure.com), Windows PowerShell, met of via een programma via een API-opslag.

Als u opslag Metrics inschakelt, moet u een bewaarperiode voor de gegevens: dit wordt bepaald door de voor hoe lang de opslag service houdt de maatstaven en toeslagen dat u voor de ruimte moet opslaan. Meestal moet u een kortere bewaartermijn voor minuut metrics dan per uur metrics vanwege de aanzienlijke extra ruimte vereist voor minuten maatstaven. Kies een verloopperiode zodat u voldoende tijd hebt om te analyseren van de gegevens en parameters die u wilt behouden voor off line analyse en rapportage te downloaden. Vergeet niet dat u ook een rekening krijgt voor het downloaden van metrische gegevens van uw account voor opslag.

## <a name="how-to-enable-storage-metrics-using-the-azure-classic-portal"></a>Het inschakelen van de opslag statistieken met de klassieke Azure-Portal

In de [Klassieke Portal Azure](https://manage.windowsazure.com)u de pagina configureren voor een besturingselement Metrics opslag opslag. Voor de controle, kunt u een niveau en een periode in dagen voor elk van de BLOB's, tabellen en wachtrijen instellen. Het niveau is in elk geval een van de volgende opties:

- Uitgeschakeld — Geen metrische gegevens worden verzameld.

- Minimaal: Opslag Metrics verzamelt een basisverzameling Maatstelsel van de ingress/egress, beschikbaarheid, vertraging en succes percentages die worden samengevoegd voor de Blob-, tabel- en wachtrij-services.

- Uitgebreide — Opslag Metrics verzamelt maatstaven die dezelfde parameters voor elke opslag API-werking, naast de cijfers over de service level bevat een volledige set. Uitgebreide statistieken kunnen dichter bij analyse van de problemen die zich tijdens de toepassing voordoen.

Houd er rekening mee dat de klassieke Portal Azure momenteel laten minuut parameters configureren in uw account opslag; u moet inschakelen minuut metrics met PowerShell of via programmacode.


## <a name="how-to-enable-storage-metrics-using-powershell"></a>Het inschakelen van opslag metrics met PowerShell

U kunt PowerShell op uw lokale machine opslag statistieken in uw account opslag configureren met behulp van de Azure PowerShell-cmdlet Get-AzureStorageServiceMetricsProperty voor het ophalen van de huidige instellingen en de cmdlet Set-AzureStorageServiceMetricsProperty om de huidige instellingen te wijzigen.

De cmdlets die opslag parameters bepalen de volgende parameters gebruiken:

- Mogelijke waarden voor MetricsType zijn uren en minuten.

- ServiceType mogelijke waarden zijn Blob, wachtrij en tabel.

- MetricsLevel mogelijke waarden zijn None (equivalent aan Off in de klassieke Portal Azure), Service (equivalent aan minimaal in de klassieke Portal Azure) en ServiceAndApi (equivalent aan uitgebreid in de klassieke Portal Azure).

De volgende opdracht wordt bijvoorbeeld met de tijdsperiode die is ingesteld op vijf dagen bewaren op minuut maatstaven voor de blob-service in de standaard opslag-account:

`Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5`

De volgende opdracht haalt de huidige per uur metrics niveau en het vasthouden van dagen voor de blob-service in de standaard opslag-account:

`Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob`

Voor meer informatie over het configureren van de Azure PowerShell-cmdlets voor het werken met uw abonnement Azure en het selecteren van de standaard opslag-account te gebruiken, Zie: [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

## <a name="how-to-enable-storage-metrics-programmatically"></a>Opslag parameters programmatisch inschakelen

De volgende C#-fragment wordt getoond hoe maatstaven en logboekregistratie voor de Blob-service gebruikt de clientbibliotheek van opslag voor .NET inschakelen:

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

    // Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Enable Storage Analytics logging and set retention policy to 10 days. 
    ServiceProperties properties = new ServiceProperties();
    properties.Logging.LoggingOperations = LoggingOperations.All;
    properties.Logging.RetentionDays = 10;
    properties.Logging.Version = "1.0";

    // Configure service properties for metrics. Both metrics and logging must be set at the same time.
    properties.HourMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
    properties.HourMetrics.RetentionDays = 10;
    properties.HourMetrics.Version = "1.0";

    properties.MinuteMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
    properties.MinuteMetrics.RetentionDays = 10;
    properties.MinuteMetrics.Version = "1.0";

    // Set the default service version to be used for anonymous requests.
    properties.DefaultServiceVersion = "2015-04-05";

    // Set the service properties.
    blobClient.SetServiceProperties(properties);
    
## <a name="viewing-storage-metrics"></a>Opslag statistieken weergeven

Wanneer u statistieken voor het controleren van uw account opslag opslag hebt geconfigureerd, registreert de cijfers in een reeks bekende tabellen in uw account voor de opslag. U kunt de pagina voor uw account voor opslag in de klassieke Portal Azure het uurtarief statistieken bekijken die beschikbaar komen in een grafiek. Op deze pagina in de klassieke Azure-Portal, kunt u het volgende doen:

- Selecteer welke parameters worden uitgezet in de grafiek (de keuze van de beschikbare statistieken hangt gekozen uitgebreide of Minimale controle voor de service op de pagina configureren).


- Selecteer de periode voor de parameters in de grafiek weergegeven.


- Kies met behulp van een absolute of relatieve schaal de metrische gegevens uitzetten.


- E-mailwaarschuwingen om u te waarschuwen wanneer een bepaalde waarde wordt bereikt door een specifieke metric configureren.


Als u wilt downloaden, de metrics voor opslag op lange termijn of ze lokaal te analyseren, moet u een hulpprogramma gebruiken of sommige code schrijven om te lezen van de tabellen. U moet downloaden minuut metrics voor analyse. De tabellen worden niet weergegeven als u alle tabellen weergeven die in uw account voor de opslag, maar u ze met de naam openen kunt. Veel fabrikanten opslag surfen's zich bewust zijn van deze tabellen en kunt u ze rechtstreeks bekijken (Zie de blog [Microsoft Azure opslag Explorers](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) boeken voor een lijst met beschikbare hulpprogramma's).

### <a name="hourly-metrics"></a>Statistieken per uur
- $MetricsHourPrimaryTransactionsBlob
- $MetricsHourPrimaryTransactionsTable
- $MetricsHourPrimaryTransactionsQueue

### <a name="minute-metrics"></a>Minuut metrics
- $MetricsMinutePrimaryTransactionsBlob
- $MetricsMinutePrimaryTransactionsTable
- $MetricsMinutePrimaryTransactionsQueue

### <a name="capacity"></a>Capaciteit
- $MetricsCapacityBlob

Voor deze tabellen aan [Opslag Analytics Metrics tabelschema](https://msdn.microsoft.com/library/azure/hh343264.aspx)vindt u alle details van de schema's. Het monster rijen onder slechts een subset van de beschikbare kolommen weergeven, maar ziet u enkele belangrijke functies van de manier waarop die opslag Metrics deze gegevens worden opgeslagen:

| PartitionKey  |       RowKey       |                    Tijdstempel | TotalRequests | TotalBillableRequests | TotalIngress | TotalEgress | Beschikbaarheid | AverageE2ELatency | AverageServerLatency | PercentSuccess |
|---------------|:------------------:|-----------------------------:|---------------|-----------------------|--------------|-------------|--------------|-------------------|----------------------|----------------|
| 20140522T1100 |      gebruiker; Alle      | 2014-05-22T11:01:16.7650250Z | 7             | 7                     | 4003         | 46801       | 100          | 104.4286          | 6.857143             | 100            |
| 20140522T1100 | gebruiker; QueryEntities | 2014-05-22T11:01:16.7640250Z | 5             | 5                     | 2694         | 45951       | 100          | 143,8             | 7,8                  | 100            |
| 20140522T1100 |  gebruiker; QueryEntity  | 2014-05-22T11:01:16.7650250Z | 1             | 1                     | 538          | 633         | 100          | 3                 | 3                    | 100            |
| 20140522T1100 | gebruiker; UpdateEntity  | 2014-05-22T11:01:16.7650250Z | 1             | 1                     | 771          | 217         | 100          | 9                 | 6                    | 100               |

In dit voorbeeld minuut metrische gegevens wordt de partitie-toets de tijd minuut resolutie. De rijsleutel identificeert het type gegevens dat is opgeslagen in de rij en deze bestaat uit twee stukken van informatie, het toegangstype en het aanvraagtype:

- Het toegangstype is of de gebruiker of de gebruiker heeft betrekking op alle aanvragen van gebruikers met de storage-service waarbij systeem verwijst naar aanvragen die worden ingediend door Analytics opslag-systeem.

- Het aanvraagtype is in dat geval moeten zij een samenvatting-regel is, of de specifieke API zoals QueryEntity of UpdateEntity worden aangeduid.


De bovenstaande voorbeeldgegevens toont alle records voor één minuut (te beginnen bij 11:00 AM), zodat het aantal aanvragen voor QueryEntities plus het aantal QueryEntity-aanvragen plus het aantal UpdateEntity-aanvragen maximaal zeven, dat het totaal is toevoegen op de gebruiker: alle rij weergegeven. U kunt ook latentie van de gemiddelde end-to-end 104.4286 in de rij van de gebruiker: All afleiden door berekening van ((143.8 * 5) + 3 + 9) / 7.

Instellen van waarschuwingen in de klassieke Azure-Portal op de pagina van het beeldscherm moet u overwegen zodat opslag Metrics kan automatisch een melding van alle belangrijke wijzigingen in het gedrag van uw storage-services. Als u een opslag explorer tool voor het downloaden van deze metrische gegevens in een indeling met scheidingstekens, kunt u Microsoft Excel om de gegevens te analyseren. Zie de [Microsoft Azure opslag Explorers](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) boeken voor een lijst van beschikbare opslagruimte explorer's blog.



## <a name="accessing-metrics-data-programmatically"></a>Metrische gegevens via programmacode toegang tot

Voorbeeld C#-code die toegang heeft tot de minuut maatstaven voor een aantal minuten en de resultaten worden weergegeven in een console-venster ziet u de volgende aanbieding. De bibliotheek in Azure opslag versie 4 met de klasse CloudAnalyticsClient die vereenvoudigt de toegang tot de tabellen metrics in opslag wordt gebruikt.

    private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)
    {
    // Convert the dates to the format used in the PartitionKey
    var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    
    var services = Enum.GetValues(typeof(StorageService));
    foreach (StorageService service in services)
    {
    Console.WriteLine("Minute Metrics for Service {0} from {1} to {2} UTC", service, start, end);
    var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);
    var t = analyticsClient.GetMinuteMetricsTable(service);
    var opContext = new OperationContext();
    var query =
    from entity in metricsQuery
    // Note, you can't filter using the entity properties Time, AccessType, or TransactionType
    // because they are calculated fields in the MetricsEntity class.
    // The PartitionKey identifies the DataTime of the metrics.
    where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0 
    select entity;
    
    // Filter on "user" transactions after fetching the metrics from Table Storage.
    // (StartsWith is not supported using LINQ with Azure table storage)
    var results = query.ToList().Where(m => m.RowKey.StartsWith("user"));
    var resultString = results.Aggregate(new StringBuilder(), (builder, metrics) => builder.AppendLine(MetricsString(metrics, opContext))).ToString();
    Console.WriteLine(resultString);
    }
    }
    
    private static string MetricsString(MetricsEntity entity, OperationContext opContext)
    {
    var entityProperties = entity.WriteEntity(opContext);
    var entityString =
    string.Format("Time: {0}, ", entity.Time) +
    string.Format("AccessType: {0}, ", entity.AccessType) +
    string.Format("TransactionType: {0}, ", entity.TransactionType) +
    string.Join(",", entityProperties.Select(e => new KeyValuePair<string, string>(e.Key.ToString(), e.Value.PropertyAsObject.ToString())));
    return entityString;
    
    }




## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a>Welke kosten maken u wanneer u opslag metrics inschakelen?

Verzoeken voor tabel entiteiten voor metrieken worden aangerekend aan de standaardtarieven van toepassing op alle bewerkingen voor Azure opslag worden geschreven.

Lees- en delete-verzoeken door een client met metrische gegevens zijn ook te factureren aan de standaardtarieven. Als u een bewaarbeleid gegevens hebt geconfigureerd, u bent niet in rekening gebracht wanneer opslag Azure oude metrische gegevens worden verwijderd. Echter als u analytics-gegevens verwijdert, wordt uw rekening afgeschreven voor de verwijderbewerkingen.

De capaciteit die wordt gebruikt door de tabellen metrics is ook te factureren: u kunt de volgende schatting van de hoeveelheid capaciteit die wordt gebruikt voor het opslaan van metrische gegevens:

- Als elk uur elke API in elke service maakt gebruik van een service, wordt ongeveer 148KB aan gegevens elk uur in de transactie metrics tabellen opgeslagen als u de service- en API-niveau overzicht hebt ingeschakeld.

- Als u elk uur elke API in elke service maakt gebruik van een service, wordt ongeveer 12KB aan gegevens elk uur in de tabellen metrics transactie opgeslagen als u alleen serviceniveau overzicht hebt ingeschakeld.

- De tabel capaciteit voor BLOB's heeft twee rijen toegevoegd elke dag (mits de gebruiker heeft zich aangemeld voor Logboeken): dit betekent dat elke dag de grootte van deze tabel door ongeveer 300 bytes vergroot.

## <a name="next-steps"></a>Volgende stappen:
[Analytics voor opslag en het benaderen van logboekgegevens inschakelen](https://msdn.microsoft.com/library/dn782840.aspx)
 