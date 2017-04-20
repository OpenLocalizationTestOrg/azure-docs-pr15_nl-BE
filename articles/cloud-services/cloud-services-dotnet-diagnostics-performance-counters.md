<properties
   pageTitle="Gebruik van prestatiemeteritems in Azure Diagnostics | Microsoft Azure"
   description="Gebruik van prestatiemeteritems in Azure cloud services of virtuele machine knelpunten in prestaties afstemmen."
   services="cloud-services"
   documentationCenter=".net"
   authors="rboucher"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/29/2016"
   ms.author="robb" />

# <a name="create-and-use-performance-counters-in-an-azure-application"></a>Maken en gebruiken van prestatiemeteritems in een toepassing Azure

In dit artikel wordt beschreven de voordelen van prestatiemeteritems in uw toepassing Azure zetten. U kunt deze gegevens verzamelen en afstemmen van de prestaties van systeem- en knelpunten te vinden.

Prestatiemeteritems die beschikbaar zijn voor Windows Server, IIS en ASP.NET kunnen ook worden verzameld en gebruikt voor het bepalen van de gezondheid van uw web Azure rollen, rollen van de werknemer en virtuele Machines. U kunt ook maken en gebruiken van aangepaste prestatiemeters.  

U kunt gegevens van prestatiemeteritems controleren
1. Direct op de toepassinghost met het hulpprogramma Prestatiemeter toegankelijk via Extern bureaublad
2. Met behulp van het Management Azure Pack met System Center Operations Manager
3. Met andere controleprogramma's die toegang de diagnostische gegevens tot overgebracht naar Azure opslag. Zie [winkel en diagnostische gegevens weergeven in Azure opslag](https://msdn.microsoft.com/library/azure/hh411534.aspx) voor meer informatie.  

Zie [How to Monitor Cloud Services](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/)voor meer informatie over het controleren van de prestaties van uw toepassing in [Azure klassieke portal](http://manage.azure.com/).

Zie [Aanbevolen procedures voor probleemoplossing voor Azure-toepassingen ontwikkelen](https://msdn.microsoft.com/library/azure/hh771389.aspx)voor meer gedetailleerde informatie over het maken van een logboekregistratie en tracering van de strategie en het gebruik van diagnostische gegevens en andere technieken problemen oplossen en optimaliseren van Azure toepassingen.


## <a name="enable-performance-counter-monitoring"></a>Performance counter-controle inschakelen

Prestatiemeteritems zijn niet standaard ingeschakeld. Uw toepassing of een taak is gestart, moet de diagnostische gegevens van agent standaardconfiguratie bevatten de specifieke prestatiemeteritems die u wilt controleren voor elke instantie van de rol wijzigen.

### <a name="performance-counters-available-for-microsoft-azure"></a>Prestatiemeteritems die beschikbaar zijn voor Microsoft Azure

Azure biedt een subset van de prestatiemeteritems beschikbaar voor Windows Server, IIS en de ASP.NET-stack. De volgende tabel worden enkele van de prestatiemeteritems van bijzonder belang voor Azure toepassingen.

|Prestatiemeteritem-categorie: Object (instantie)|Naam van het item      |Referentie|
|---|---|---|
|.NET CLR: uitzonderingen (_Global_)|# Uitzonderingen gegenereerd per seconde   |Prestatiemeteritems van de uitzondering|
|.NET CLR: geheugen (_Global_)    |Percentage tijd in ° c            |Prestatiemeteritems voor geheugen|
|ASP.NET                      |Toepassing wordt opnieuw opgestart    |Prestatiemeteritems voor ASP.NET|
|ASP.NET                      |Uitvoertijd van aanvraag  |Prestatiemeteritems voor ASP.NET|
|ASP.NET                      |Verbroken aanvragen   |Prestatiemeteritems voor ASP.NET|
|ASP.NET                      |Opnieuw gestarte werkprocessen |Prestatiemeteritems voor ASP.NET|
|ASP.NET-toepassingen (__Totaal__)|Totaal aantal aanvragen        |Prestatiemeteritems voor ASP.NET|
|ASP.NET-toepassingen (__Totaal__)|Aanvragen per seconde          |Prestatiemeteritems voor ASP.NET|
|ASP.NET-v4.0.30319           |Uitvoertijd van aanvraag  |Prestatiemeteritems voor ASP.NET|
|ASP.NET-v4.0.30319           |Wachttijd van aanvraag       |Prestatiemeteritems voor ASP.NET|
|ASP.NET-v4.0.30319           |Lopende aanvragen        |Prestatiemeteritems voor ASP.NET|
|ASP.NET-v4.0.30319           |Aanvragen in wachtrij         |Prestatiemeteritems voor ASP.NET|
|ASP.NET-v4.0.30319           |Geweigerde aanvragen       |Prestatiemeteritems voor ASP.NET|
|Geheugen                       |Beschikbare MBytes        |Prestatiemeteritems voor geheugen|
|Geheugen                       |Toegewezen Bytes         |Prestatiemeteritems voor geheugen|
|Processor(_Totaal)            |Percentage processortijd        |Prestatiemeteritems voor ASP.NET|
|TCPv4                        |Verbindingsfouten     |TCP-Object|
|TCPv4                        |Verbindingen |TCP-Object|
|TCPv4                        |Verbindingen opnieuw instellen       |TCP-Object|
|TCPv4                        |Segmenten verzonden per seconde       |TCP-Object|
|Netwerk Interface(*)         |Ontvangen bytes per seconde      |Netwerkinterface-Object|
|Netwerk Interface(*)         |Verzonden bytes per seconde          |Netwerkinterface-Object|
|Netwerk Interface (Microsoft Virtual Machine Bus Network Adapter-_2)|Ontvangen bytes per seconde|Netwerkinterface-Object|
|Netwerk Interface (Microsoft Virtual Machine Bus Network Adapter-_2)|Verzonden bytes per seconde|Netwerkinterface-Object|
|Netwerk Interface (Microsoft Virtual Machine Bus Network Adapter-_2)|Totaal aantal bytes per seconde|Netwerkinterface-Object|

## <a name="create-and-add-custom-performance-counters-to-your-application"></a>Maken en aangepaste prestatiemeters aan uw toepassing toevoegen

Azure biedt ondersteuning voor maken en aangepaste prestatiemeters voor web-rollen en functies van de werknemer wijzigen. De items kunnen bijhouden en controleren van toepassingsspecifieke gedrag worden gebruikt. U kunt maken en aangepaste categorieën voor prestatiemeteritems en specificaties verwijderen uit een taak starten, Webrol of functie van de werknemer met verhoogde bevoegdheden.

>[AZURE.NOTE] Code die wijzigingen in de aangepaste prestatiemeters aanbrengt moet veel rechten uit te voeren. Als de code in een web-functie of rol van de werknemer, moet de rol van de tag bevatten <Runtime executionContext="elevated" /> in het bestand ServiceDefinition.csdef voor de rol die juist worden geïnitialiseerd.

U kunt aangepaste prestatiemeteritemgegevens naar Azure opslag met behulp van de agent diagnostiek verzenden.

De standaard prestatiemeteritemgegevens wordt gegenereerd door de Azure processen. Aangepaste gegevens van prestatiemeteritems moet worden gemaakt door uw rol of werknemer rol webtoepassing. Zie [Performance Counter typen](https://msdn.microsoft.com/library/z573042h.aspx) voor meer informatie over de gegevenstypen die kunnen worden opgeslagen in aangepaste prestatiemeters. Zie [Voorbeeld PerformanceCounters](http://code.msdn.microsoft.com/azure/) voor een voorbeeld van wordt gemaakt en wordt de aangepaste gegevens van prestatiemeteritems in de Webrol van een.

## <a name="store-and-view-performance-counter-data"></a>Gegevens van prestatiemeteritems voor opslaan en weergeven

Azure in de cache opgeslagen gegevens met andere diagnostische gegevens van prestatiemeteritems. Deze gegevens zijn beschikbaar voor controle op afstand terwijl de functie wordt uitgevoerd toegang tot extern bureaublad gebruikt om hulpprogramma's, zoals Performance Monitor weer te geven. Als u wilt de gegevens buiten de instantie rol behouden, moet de agent diagnostiek de gegevens overbrengen naar Azure opslag. De maximale grootte van de gegevens in de cache van prestatiemeteritems kan worden geconfigureerd in de agent diagnostiek of kan worden geconfigureerd als onderdeel van een gedeelde limiet voor de diagnostische gegevens. Zie voor meer informatie over het instellen van de grootte van de [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) en [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx). Zie [winkel en diagnostische gegevens weergeven in Azure opslag](https://msdn.microsoft.com/library/azure/hh411534.aspx) voor een overzicht van de diagnostische gegevens van agent om gegevens te verzenden naar een opslag instellen.

Elk exemplaar van het geconfigureerde prestatiemeteritem wordt vastgelegd bij een opgegeven sampling-frequentie en de bemonsterde gegevens overgebracht naar de opslag-account door een aanvraag voor een geplande of een overdracht via bellen op verzoek. Automatische transfers kunnen zo vaak als eenmaal per minuut worden gepland. Prestatiemeteritemgegevens overgebracht door de agent Diagnostiek wordt opgeslagen in een tabel, WADPerformanceCountersTable, in de opslag. Deze tabel kan toegankelijk en doorzocht met Azure standaardopslag API methoden. Zie [Microsoft Azure PerformanceCounters voorbeeld](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) voor een voorbeeld van zoeken en weergeven van gegevens van prestatiemeteritems uit de tabel WADPerformanceCountersTable.

>[AZURE.NOTE] De meest recente prestatiemeteritemgegevens in de opslag mogelijk afhankelijk van de diagnose agent overdracht frequentie en de latentie van de wachtrij, enkele minuten verlopen.

## <a name="enable-performance-counters-using-diagnostics-configuration-file"></a>Met diagnostische gegevens van het configuratiebestand prestatiemeteritems inschakelen

Gebruik de volgende procedure om prestatiemeters in uw toepassing Azure.

## <a name="prerequisites"></a>Vereisten

In dit gedeelte wordt ervan uitgegaan dat u hebt geïmporteerd van de monitor diagnostische gegevens in uw toepassing en de diagnostische gegevens van het bestand toegevoegd aan Visual Studio-oplossing (diagnostics.wadcfg in punt 2.4 van SDK en lager of diagnostics.wadcfgx in SDK 2.5 en hoger). Zie stap 1 en 2 in [Diagnostics inschakelen in Azure Cloud Services en virtuele Machines](./cloud-services-dotnet-diagnostics.md)) voor meer informatie.

## <a name="step-1-collect-and-store-data-from-performance-counters"></a>Stap 1: Verzamelen en opslaan van gegevens van prestatiemeteritems

Nadat u de diagnostische gegevens van het bestand hebt toegevoegd aan de Visual Studio-oplossing kunt u de collectie en de opslag van gegevens van prestatiemeteritems in een Azure-toepassing. Dit wordt gedaan door items toe te voegen aan de diagnostische gegevens van het bestand. Diagnostische gegevens, met inbegrip van prestatiemeteritems, wordt eerst op de instantie verzameld. De gegevens vervolgens persistent gemaakt naar de tabel WADPerformanceCountersTable in de tabel Azure service, dus u ook moet de opslag account opgeven in uw toepassing. Als u uw toepassing lokaal in de Emulator berekenen testen bent, kunt u diagnostische gegevens ook lokaal opslaan in de opslag-Emulator. Voordat u diagnostische gegevens op te slaan moet u Ga naar de [Azure klassieke portal](http://manage.windowsazure.com/) en maak een account voor de opslag. Beste manier is om uw account opslag op de geo-locatie waar de Azure toepassing vinden om te voorkomen dat externe bandbreedtekosten betaalt en wachttijden te verminderen.

### <a name="add-performance-counters-to-the-diagnostics-file"></a>Prestatiemeteritems toevoegen aan het bestand diagnostische gegevens

Er zijn veel items die u kunt gebruiken. In het volgende voorbeeld ziet u verschillende prestatiemeteritems die worden aanbevolen voor het web en het controleren van de rol van werknemer.

Open het bestand diagnostics (diagnostics.wadcfg in punt 2.4 van SDK en lager of diagnostics.wadcfgx in SDK 2.5 en hoger) en voeg het volgende toe aan het element DiagnosticMonitorConfiguration:

```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
       <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT30S" />

    <!-- Use the Process(w3wp) category counters in a web role -->

       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\% Processor Time" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Private Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Thread Count" sampleRate="PT30S" />

    <!-- Use the Process(WaWorkerHost) category counters in a worker role.
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\% Processor Time" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Private Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Thread Count" sampleRate="PT30S" />
    -->

       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Interop(_Global_)\# of marshalling" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Loading(_Global_)\% Time Loading" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR LocksAndThreads(_Global_)\Contention Rate / sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Memory(_Global_)\# Bytes in all Heaps" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Networking(_Global_)\Connections Established" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Remoting(_Global_)\Remote Calls/sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Jit(_Global_)\% Time in Jit" sampleRate="PT30S" />
    </PerformanceCounters>
```

Het kenmerk bufferQuotaInMB geeft de maximale opslag-systeem dat beschikbaar is voor het gegevenstype-collectie (Azure Logboeken, logboeken van IIS, enz.). De standaardwaarde is 0. Wanneer het quotum is bereikt, worden de oudste gegevens worden verwijderd als nieuwe gegevens worden toegevoegd. De som van alle eigenschappen van de bufferQuotaInMB moet groter zijn dan de waarde van het kenmerk OverallQuotaInMB. Zie het gedeelte Setup af van [Het oplossen van Best Practices voor Azure-toepassingen ontwikkelen](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx)voor meer informatie over om te bepalen hoeveel opslagruimte is vereist voor het verzamelen van diagnostische gegevens.

Het kenmerk scheduledTransferPeriod Hiermee geeft u het interval tussen de geplande overdracht van gegevens, naar boven afgerond op de dichtstbijzijnde minuut. In de volgende voorbeelden wordt ingesteld op PT30M (30 minuten). Als u de transfer periode op een kleine waarde, bijvoorbeeld 1 minuut nadelig beïnvloedt de prestaties van de toepassing in de productie, maar is handig voor zien diagnostische gegevens snel te werken als u wilt testen. De geplande overdracht periode moet klein genoeg zijn om ervoor te zorgen dat diagnostische gegevens van het exemplaar, maar groot genoeg dat het is niet van invloed op de prestaties van uw toepassing niet wordt overschreven.

Het kenmerk counterSpecifier geeft de Prestatiemeter te verzamelen. Het kenmerk sampleRate geeft de snelheid aan waarmee de Prestatiemeter te worden bemonsterd, in dit geval 30 seconden.

Zodra u hebt toegevoegd de prestatiemeteritems die u wilt verzamelen, kunt u de wijzigingen opslaan naar het bestand diagnostische gegevens. Vervolgens moet u de opslag account opgeven die aan de diagnostische gegevens bewaard.

### <a name="specify-the-storage-account"></a>Geef de rekening op opslag

Persistent maken uw diagnostische informatie aan uw account Azure opslag, moet u een verbindingsreeks opgeven in uw service-configuratiebestand (ServiceConfiguration.cscfg).

Voor Azure SDK 2.5 kan de opslag Account worden opgegeven in het bestand diagnostics.wadcfgx.

>[AZURE.NOTE] Deze instructies zijn alleen van toepassing en onder Azure SDK 2.4. Voor Azure SDK 2.5 kan de opslag Account worden opgegeven in het bestand diagnostics.wadcfgx.

De verbindingsreeksen instellen:

1. Het ServiceConfiguration.Cloud.cscfg-bestand met uw favoriete teksteditor openen en stel de verbindingsreeks voor de opslag. De *accountnaam* en *AccountKey* -waarden worden gevonden in de Azure klassieke portal in het dashboard van de opslag account onder sleutels beheren.

    ```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>"/>
    </ConfigurationSettings>
    ```
2. Sla het bestand ServiceConfiguration.Cloud.cscfg.

3. Open het bestand ServiceConfiguration.Local.cscfg en controleer of UseDevelopmentStorage is ingesteld op true.

    ```
    <ConfigurationSettings>
      <Settingname="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true"/>
    </ConfigurationSettings>
    ```
Nu dat de verbindingsreeksen zijn ingesteld, behouden de toepassing diagnostische gegevens aan uw account opslag wanneer uw toepassing is geïmplementeerd.
4. Opslaan en het project bouwen en implementeren van uw toepassing.

## <a name="step-2-optional-create-custom-performance-counters"></a>Stap 2: (Optioneel) maken van aangepaste prestatiemeters

Naast de vooraf gedefinieerde items, kunt u uw eigen aangepaste prestatiemeters voor het controleren van web- of werknemer rollen toevoegen. Aangepaste prestatiemeters kunnen worden gebruikt voor het bijhouden en toepassingsspecifieke gedrag controleren en kunnen worden gemaakt of verwijderd in taak starten, Webrol of functie van de werknemer met verhoogde bevoegdheden.

De agent Azure diagnostische gegevens vernieuwt de performance counter configuratie uit het bestand .wadcfg een minuut na het starten.  Als u aangepaste prestatiemeters in de methode OnStart maken en uw opstarttaken duurt langer dan een minuut uit te voeren, zullen uw aangepaste prestatiemeters niet zijn gemaakt als de agent Azure diagnostische gegevens probeert te laden.  In dit scenario ziet u alle diagnostische gegevens, met uitzondering van uw aangepaste prestatiemeters Hiermee legt vast Azure diagnostische gegevens.  Dit probleem oplossen door de prestatiemeteritems maken in een taak is gestart of verplaats werken sommige van uw taak is gestart met de methode OnStart na het maken van de prestatiemeteritems.

Voer de volgende stappen voor het maken van een eenvoudige aangepaste Prestatiemeter '\MyCustomCounterCategory\MyButton1Counter':

1. Open het definitiebestand (CSDEF) voor uw toepassing.
2. Voeg de Runtime-element in de WebRole of WorkerRole element dat moet worden uitgevoerd met verhoogde bevoegdheden:

    ```
    <runtime executioncontext="elevated"/>
    ```
3. Sla het bestand.
4. Open het bestand diagnostics (diagnostics.wadcfg in punt 2.4 van SDK en lager of diagnostics.wadcfgx in SDK 2.5 en hoger) en voeg het volgende toe aan de DiagnosticMonitorConfiguration 

    ```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
     <PerformanceCounterConfiguration counterSpecifier="\MyCustomCounterCategory\MyButton1Counter" sampleRate="PT30S"/>
    </PerformanceCounters>
    ```
5. Sla het bestand.
6. Maak de aangepaste Prestatiemeteritem-categorie in de methode OnStart van uw rol, voordat base. OnStart. De volgende C#-voorbeeld wordt een aangepaste categorie gemaakt als deze nog niet bestaat:

    ```
    public override bool OnStart()
    {
    if (!PerformanceCounterCategory.Exists("MyCustomCounterCategory"))
    {
       CounterCreationDataCollection counterCollection = new CounterCreationDataCollection();

       // add a counter tracking user button1 clicks
       CounterCreationData operationTotal1 = new CounterCreationData();
       operationTotal1.CounterName = "MyButton1Counter";
       operationTotal1.CounterHelp = "My Custom Counter for Button1";
       operationTotal1.CounterType = PerformanceCounterType.NumberOfItems32;
       counterCollection.Add(operationTotal1);

       PerformanceCounterCategory.Create(
         "MyCustomCounterCategory",
         "My Custom Counter Category",
         PerformanceCounterCategoryType.SingleInstance, counterCollection);

       Trace.WriteLine("Custom counter category created.");
    }
    else{
       Trace.WriteLine("Custom counter category already exists.");
    }

    return base.OnStart();
    }
    ```
7. De items in de toepassing bijwerken. In het volgende voorbeeld updates een aangepaste Prestatiemeter op gebeurtenissen Button1_Click:

    ```
    protected void Button1_Click(object sender, EventArgs e)
        {
         PerformanceCounter button1Counter = new PerformanceCounter(
           "MyCustomCounterCategory",
           "MyButton1Counter",
           string.Empty,
           false);
         button1Counter.Increment();
        this.Button1.Text = "Button 1 count: " +
           button1Counter.RawValue.ToString();
        }
    ```
8. Sla het bestand.  

Aangepaste prestatiemeteritemgegevens worden nu door de monitor Azure diagnostische gegevens verzameld.

## <a name="step-3-query-performance-counter-data"></a>Stap 3: Querygegevens van prestatiemeteritems

Zodra uw toepassing is geïmplementeerd en de diagnostische gegevens ziet begint met het verzamelen van prestatiemeteritems en persistent maken die gegevens naar Azure opslag. Met hulpprogramma's zoals Server Explorer in Visual Studio of [Azure Opslagverkenner](http://azurestorageexplorer.codeplex.com/) [Azure diagnostische gegevens Manager](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) met Cerebrata kunt u de prestatiegegevens van de items weergeven in de tabel WADPerformanceCountersTable. U kunt de tabel service met behulp van [C#](../storage/storage-dotnet-how-to-use-tables.d), [Java](../storage/storage-java-how-to-use-table-storage.md), [Node.js](../storage/storage-nodejs-how-to-use-table-storage.md), [Python](../storage/storage-python-how-to-use-table-storage.md), [Ruby](../storage/storage-ruby-how-to-use-table-storage.md), of [PHP](../storage/storage-php-how-to-use-table-storage.md)ook programmatisch zoeken.

De volgende C#-voorbeeld ziet u een eenvoudige query bij de WADPerformanceCountersTable-tabel en de diagnostische gegevens naar een CSV-bestand opgeslagen. Zodra de items zijn opgeslagen in een CSV-bestand kunt u de grafische mogelijkheden in Microsoft Excel of een ander hulpprogramma om de gegevens anders. Zorg ervoor dat een verwijzing toevoegen aan Microsoft.WindowsAzure.Storage.dll, die is opgenomen in de SDK voor .NET oktober 2012 van Azure en hoger. De assembly wordt geïnstalleerd in de map % programma Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-num\ref\.

```
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ...

    // Get the connection string. When using Microsoft Azure Cloud Services, it is recommended
    // you store your connection string using the Microsoft Azure service configuration
    // system (*.csdef and *.cscfg files). You can you use the CloudConfigurationManager type
    // to retrieve your storage connection string.  If you're not using Cloud Services, it's
    // recommended that you store the connection string in your web.config or app.config file.
    // Use the ConfigurationManager type to retrieve your storage connection string.

    string connectionString = Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
    //string connectionString = System.Configuration.ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString;

    // Get a reference to the storage account using the connection string.  You can also use the development
    // storage account (Storage Emulator) for local debugging.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
    //CloudStorageAccount storageAccount = CloudStorageAccount.DevelopmentStorageAccount;

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "WADPerformanceCountersTable" table.
    CloudTable table = tableClient.GetTableReference("WADPerformanceCountersTable");

    // Create the table query, filter on a specific CounterName, DeploymentId and RoleInstance.
    TableQuery<PerformanceCountersEntity> query = new TableQuery<PerformanceCountersEntity>()
       .Where(
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("CounterName", QueryComparisons.Equal, @"\Processor(_Total)\% Processor Time"),
          TableOperators.And,
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("DeploymentId", QueryComparisons.Equal, "ec26b7a1720447e1bcdeefc41c4892a3"),
          TableOperators.And,
          TableQuery.GenerateFilterCondition("RoleInstance", QueryComparisons.Equal, "WebRole1_IN_0")
          )
       )
    );

    // Execute the table query.
    IEnumerable<PerformanceCountersEntity> result = table.ExecuteQuery(query);

    // Process the query results and build a CSV file.
    StringBuilder sb = new StringBuilder("TimeStamp,EventTickCount,DeploymentId,Role,RoleInstance,CounterName,CounterValue\n");

    foreach (PerformanceCountersEntity entity in result)
    {
       sb.Append(entity.Timestamp + "," + entity.EventTickCount + "," + entity.DeploymentId + ","
          + entity.Role + "," + entity.RoleInstance + "," + entity.CounterName + "," + entity.CounterValue+"\n");
    }

    StreamWriter sw = File.CreateText(@"C:\temp\PerfCounters.csv");
    sw.Write(sb.ToString());
    sw.Close();
```

Entiteiten toewijzen aan de C#-objecten met behulp van een aangepaste klasse die is afgeleid van **TableEntity**. De volgende code definieert een klasse entiteit die een Prestatiemeter in de **WADPerformanceCountersTable** -tabel vertegenwoordigt.


    public class PerformanceCountersEntity : TableEntity
    {
       public long EventTickCount { get; set; }
       public string DeploymentId { get; set; }
       public string Role { get; set; }
       public string RoleInstance { get; set; }
       public string CounterName { get; set; }
       public double CounterValue { get; set; }
    }


## <a name="next-steps"></a>Volgende stappen
[Aanvullende artikelen voor weergave op Azure Diagnostics] (.. / azure-diagnostics.md)
