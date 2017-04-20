<properties
    pageTitle="Azure diagnostische gegevens in het hete pad gebeurtenis Hubs met streaming | Microsoft Azure"
    description="Ziet u hoe Azure diagnostische gegevens configureren met Hubs gebeurtenis van begin tot eind, met inbegrip van richtsnoeren voor veelvoorkomende scenario's."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="event-hubs"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/14/2016"
    ms.author="sethm" />

# <a name="streaming-azure-diagnostics-data-in-the-hot-path-by-using-event-hubs"></a>Azure diagnostische gegevens in het pad hot streaming via Hubs gebeurtenis

Diagnostische gegevens van Azure biedt flexibele manieren om maatstaven en logboeken van cloud services, virtuele machines (VMs) te verkrijgen resultaten overbrengen naar Azure opslag. Starten in de periode van maart 2016 (2,9 SDK), kunt u diagnostische gegevens volledig aangepaste gegevensbronnen opvangen en gegevensoverdracht hot pad in seconden via [Azure gebeurtenis Hubs](https://azure.microsoft.com/services/event-hubs/).

Ondersteunde gegevenstypen zijn onder andere:

- Gebeurtenis Tracing voor Windows (ETW), gebeurtenissen
- Prestatiemeteritems
- Windows-gebeurtenislogboeken
- Toepassingslogboeken
- Azure diagnostische infrastructuur Logboeken

In dit artikel wordt beschreven hoe u Azure diagnostische gegevens configureren met Hubs gebeurtenis van begin tot eind. Ook richtsnoeren voor de volgende gevallen:

- Het aanpassen van de logboeken en het maatstelsel dat sinked met Hubs gebeurtenis ophalen
- Het wijzigen van configuraties in elke omgeving
- Gebeurtenis Hubs stream-gegevens weergeven
- Problemen met de verbinding oplossen  

## <a name="prerequisites"></a>Vereisten

Gebeurtenis Hubs zinken in Azure diagnostische gegevens wordt ondersteund in de Cloud-Services, VMs, virtuele Machine schaal Sets en Service Fabric starten in de 2,9 Azure SDK en de bijbehorende Azure-hulpprogramma's voor Visual Studio.

- Azure Diagnostics extensie 1.6 ([Azure SDK voor .NET 2,9 of hoger](https://azure.microsoft.com/downloads/) als doel deze standaard)
- [Visual Studio 2013 of later](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
- Bestaande configuraties van Azure diagnostische gegevens in een toepassing met behulp van een *.wadcfgx* en een van de volgende methoden:
    - Visual Studio: [Diagnostische gegevens voor Azure Cloud Services en virtuele Machines configureren](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)
    - Windows PowerShell: [diagnostiek in Azure Cloud Services met PowerShell inschakelen](../cloud-services/cloud-services-diagnostics-powershell.md)
- Gebeurtenis Hubs naamruimte ingericht per artikel [aan de slag met gebeurtenis-Hubs](./event-hubs-csharp-ephcs-getstarted.md)

## <a name="connect-azure-diagnostics-to-event-hubs-sink"></a>Verbinding maken met Azure Diagnostics Hubs gebeurtenis-sink

Azure Diagnostics sinks altijd logs en statistieken, standaard aan een account Azure opslag. Een toepassing kan ook met Hubs gebeurtenis opvangen door een nieuwe **put** sectie toe te voegen aan het element **WadCfg** in de sectie **PublicConfig** van het bestand *.wadcfgx* . In Visual Studio, het *.wadcfgx* -bestand is opgeslagen in het volgende pad: **Cloud Service Project** > **rollen** >  **(functienaam)** > **diagnostics.wadcfgx** -bestand.

```
<SinksConfig>
  <Sink name="HotPath">
    <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" />
  </Sink>
</SinksConfig>
```

In dit voorbeeld wordt de URL van de gebeurtenis Hub is ingesteld op de volledige naamruimte van de gebeurtenis Hub: gebeurtenis Hubs naamruimte + "/" + naam van de gebeurtenis Hub.  

De gebeurtenis Hub URL wordt weergegeven in de [Azure portal](http://go.microsoft.com/fwlink/?LinkID=213885) op het dashboard gebeurtenis Hubs.  

De naam van de **wastafel** kan worden ingesteld op een willekeurige geldige tekenreeks als dezelfde waarde in het config-bestand wordt gebruikt.

> [AZURE.NOTE]  Mogelijk zijn er extra putten, zoals *applicationInsights* in deze sectie worden geconfigureerd. Azure diagnostische gegevens kunt een of meer putten worden gedefinieerd als elke sink ook in de sectie **PrivateConfig aangegeven wordt** .  

De Hubs gebeurtenis-sink moet ook worden gedeclareerd en gedefinieerd in de sectie **PrivateConfig** van het *.wadcfgx* -configuratiebestand.

```
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <StorageAccount name="" key="" endpoint="" />
  <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="9B3SwghJOGEUvXigc6zHPInl2iYxrgsKHZoy4nm9CUI=" />
</PrivateConfig>
```

De `SharedAccessKeyName` waarde moet overeenkomen met een gedeelde Access handtekening (SAS) sleutel en een beleid dat is gedefinieerd in de naamruimte **Gebeurtenis Hubs** . Ga naar het dashboard gebeurtenis Hubs in [Azure portal](https://manage.windowsazure.com), klikt u op het tabblad **configureren** en instellen van een beleid met de naam (bijvoorbeeld "SendRule") met machtigingen *verzenden* . De **StorageAccount** wordt ook aangegeven in **PrivateConfig**. Er is niet nodig om hier waarden wijzigen als ze werken. In dit voorbeeld laat wij de waarden leeg, is een teken dat een downstream actief de waarden worden ingesteld. Bijvoorbeeld wordt, het configuratiebestand *ServiceConfiguration.Cloud.cscfg* omgeving de omgeving nodig namen en sleutels.  

> [AZURE.WARNING] De gebeurtenis Hubs SAS-sleutel wordt opgeslagen als tekst zonder opmaak in het bestand *.wadcfgx* . Vaak, deze sleutel aan broncodebeheer is ingecheckt of beschikbaar is als een actief op de server samenstellen zodat u deze zo nodig te beschermen. Wij raden u aan een SAS-toets te gebruiken met machtigingen *alleen verzenden* zodat een kwaadwillende gebruiker kan schrijven naar de Hub gebeurtenis maar niet luisteren naar deze beheren.

## <a name="configure-azure-diagnostics-logs-and-metrics-to-sink-with-event-hubs"></a>Azure diagnostische logboeken en metrics opvangen met Hubs gebeurtenis configureren

Zoals eerder is besproken, alle standaard- en aangepaste diagnostische gegevens, is statistieken en zich aanmeldt, automatisch sinked naar Azure opslag in de ingestelde tijdsintervallen. Met de gebeurtenis Hubs en eventuele extra zakken, kunt u een toegangspunt of een leaf-knooppunt in de hiërarchie te zijn met de Hub van de gebeurtenis sinked. Dit omvat de ETW-gebeurtenissen, prestatiemeters gebeurtenislogboeken van Windows en toepassingen.   

Het is belangrijk rekening te houden met hoeveel gegevenspunten moeten daadwerkelijk worden overgebracht naar de gebeurtenis Hubs. Normaal gesproken ontwikkelaars gegevensoverdracht lage latentie hot pad die worden verbruikt en snel geïnterpreteerd. Systemen die of de regels automatisch schalen waarschuwingen controleren zijn voorbeelden. Een ontwikkelaar kan ook een alternatieve analyse-archief configureren of archief--bijvoorbeeld Azure Stream Analytics, Elasticsearch, een aangepast systeem van toezicht of een favoriete controlesysteem van anderen te zoeken.

Hier volgen enkele voorbeeldconfiguraties.

```
<PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
</PerformanceCounters>
```

In het volgende voorbeeld wordt wordt de sink toegepast op het hoofdknooppunt van de **PerformanceCounters** in de hiërarchie, wat betekent dat alle onderliggende **PerformanceCounters** wordt verzonden naar de gebeurtenis Hubs.  

```
<PerformanceCounters scheduledTransferPeriod="PT1M">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" sinks="HotPath" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" sinks="HotPath"/>
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" sinks="HotPath"/>
</PerformanceCounters>
```

In het vorige voorbeeld de sink wordt toegepast op slechts drie items: **Aanvragen in wachtrij** **Aanvragen afgewezen**en **percentage processortijd**.  

In het volgende voorbeeld ziet u hoe een ontwikkelaar de hoeveelheid verzonden gegevens zijn de essentiële criteria die worden gebruikt voor de gezondheid van de service kunt beperken.  

```
<Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
```

In dit voorbeeld wordt de sink wordt toegepast op Logboeken en alleen voor fout niveau trace is gefilterd.

## <a name="deploy-and-update-a-cloud-services-application-and-diagnostics-config"></a>Implementeren en bijwerken van een toepassing en diagnose config voor Cloud Services

Visual Studio biedt de eenvoudigste pad voor de implementatie van de toepassing en Hubs gebeurtenis-sink-configuratie. Als u wilt bekijken en bewerken van het bestand, open het bestand *.wadcfgx* in Visual Studio, bewerken en opslaan. Het pad is een **Cloud Service Project** > **rollen** > **(functienaam)** > **diagnostics.wadcfgx**.  

Op dit moment alle distributie en implementatie bijwerken acties in Visual Studio, Visual Studio Team System en alle opdrachten of scripts die zijn gebaseerd op MSBuild en gebruik de **/t: publiceren** doel nemen de *.wadcfgx* in het proces van de verpakking. Bovendien implementaties en updates implementeren het bestand naar Azure met behulp van de juiste extensie voor Azure Diagnostics agent op uw VMs.

Nadat u de toepassing en de configuratie van diagnostische Azure geïmplementeerd, ziet u onmiddellijk de activiteit in het dashboard van de Hub-gebeurtenis. Dit geeft aan dat u verder wilt gaan naar de hot pad gegevens weergeven in de listener client of analysis tool van uw keuze.  

In de onderstaande figuur is bevat het dashboard gebeurtenis Hubs gezonde verzenden van diagnostische gegevens op de Hub gebeurtenis ergens na 23 uur starten. Als de toepassing is gedistribueerd met een bijgewerkte *.wadcfgx* -bestand en de sink correct is geconfigureerd.

![][0]  

> [AZURE.NOTE] Wanneer u updates voor de diagnose van Azure config-bestand (.wadcfgx), het raadzaam om de updates te pushen voor de hele toepassing als de configuratie met behulp van Visual Studio publiceren of een Windows PowerShell-script.  

## <a name="view-hot-path-data"></a>Hot-path-gegevens weergeven

Zoals eerder besproken, zijn er vaak gebruikt voor het luisteren naar en verwerken van gegevens van de gebeurtenis Hubs.

Een eenvoudige aanpak is een kleine test consoletoepassing te luisteren naar de gebeurtenis Hub en de uitvoerstroom afdrukken maken. U kunt de volgende code wordt uitvoeriger in [aan de slag met de gebeurtenis Hubs](./event-hubs-csharp-ephcs-getstarted.md)worden plaatsen), in een console-programma.  

Rekening mee dat de consoletoepassing moet de [gebeurtenis Processor Host Nuget package](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).  

Vergeet niet de waarden tussen punthaken in de **Main** -functie met de waarden voor uw resources vervangen.   

```
//Console application code for EventHub test client
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace EventHubListener
{
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                    Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                        context.Lease.PartitionId, data));

                foreach (var x in eventData.Properties)
                {
                    Console.WriteLine(string.Format("    {0} = {1}", x.Key, x.Value));
                }
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string eventHubConnectionString = "Endpoint= <your connection string>”
            string eventHubName = "<Event Hub name>";
            string storageAccountName = "<Storage account name>";
            string storageAccountKey = "<Storage account key>”;
            string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

            string eventProcessorHostName = Guid.NewGuid().ToString();
            EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
            Console.WriteLine("Registering EventProcessor...");
            var options = new EventProcessorOptions();
            options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
            eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

            Console.WriteLine("Receiving. Press enter key to stop worker.");
            Console.ReadLine();
            eventProcessorHost.UnregisterEventProcessorAsync().Wait();
        }
    }
}
```

## <a name="troubleshoot-event-hubs-sink"></a>Problemen met Hubs gebeurtenis-sink

- De gebeurtenis Hub weergegeven geen binnenkomende of uitgaande evenement activiteit zoals verwacht.

    Controleer dat uw Hub-gebeurtenis met succes is ingericht. Alle verbindingsgegevens in de sectie **PrivateConfig** van de *.wadcfgx* moet overeenkomen met de waarden van de bron in de portal. Zorg ervoor dat er een SAS-beleid gedefinieerd ("SendRule" in het voorbeeld) in de portal en dat de machtiging *verzenden* wordt verleend.  

- Nadat deze is bijgewerkt wordt de Hub gebeurtenis niet langer evenement voor binnenkomende of uitgaande activiteit.

    Controleer eerst of de gegevens van de gebeurtenis Hub en configuratie correct zoals eerder beschreven. Soms wordt de **PrivateConfig** opnieuw ingesteld in een update van de implementatie. De aanbevolen oplossing is om alle wijzigingen aanbrengen in de *.wadcfgx* in het project en vervolgens een volledige aanvraag update push. Als dit niet mogelijk is, moet u dat de update van de diagnostische gegevens van een volledige **PrivateConfig** waarin de SAS-toets duwt.  

- Ik heb geprobeerd de suggesties en de gebeurtenis Hub nog steeds niet werkt.

    Kijk in de tabel Azure opslag met Logboeken en fouten voor Azure Diagnostics zelf: **WADDiagnosticInfrastructureLogsTable**. Eén mogelijkheid is met een hulpprogramma zoals [Azure Opslagverkenner](http://www.storageexplorer.com) verbinding met deze account opslag, deze tabel weergeven en toevoegen van een query voor tijdstempel in de afgelopen 24 uur. Het hulpprogramma kunt u een CSV-bestand exporteren en open het in een toepassing zoals Microsoft Excel. Excel kunt u zoeken naar tekenreeksen met telefoonkaart, zoals **EventHubs**, om te zien welke fout gemeld.  

## <a name="next-steps"></a>Volgende stappen

• [Meer informatie over de gebeurtenis Hubs](https://azure.microsoft.com/services/event-hubs/)

## <a name="appendix-complete-azure-diagnostics-configuration-file-wadcfgx-example"></a>Aanhangsel: Azure Diagnostics (.wadcfgx) voorbeeldconfiguratiebestand voltooien

```
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
        <Directories scheduledTransferPeriod="PT1M">
          <IISLogs containerName="wad-iis-logfiles" />
          <FailedRequestLogs containerName="wad-failedrequestlogs" />
        </Directories>
        <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
        </PerformanceCounters>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*" />
        </WindowsEventLog>
        <CrashDumps>
          <CrashDumpConfiguration processName="WaIISHost.exe" />
          <CrashDumpConfiguration processName="WaWorkerHost.exe" />
          <CrashDumpConfiguration processName="w3wp.exe" />
        </CrashDumps>
        <Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
      </DiagnosticMonitorConfiguration>
      <SinksConfig>
        <Sink name="HotPath">
          <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" />
        </Sink>
        <Sink name="applicationInsights">
          <ApplicationInsights />
          <Channels>
            <Channel logLevel="Error" name="errors" />
          </Channels>
        </Sink>
      </SinksConfig>
    </WadCfg>
    <StorageAccount />
  </PublicConfig>
  <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="" key="" endpoint="" />
    <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" SharedAccessKey="YOUR_KEY_HERE" />
  </PrivateConfig>
  <IsEnabled>true</IsEnabled>
</DiagnosticsConfiguration>
```

De aanvullende *ServiceConfiguration.Cloud.cscfg* voor dit voorbeeld ziet er als volgt uit.

```
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="MyFixItCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="MyFixIt.WorkerRole">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="YOUR_CONNECTION_STRING" />
    </ConfigurationSettings>
  </Role>
</ServiceConfiguration>
```

<!-- Images. -->
[0]: ./media/event-hubs-streaming-azure-diags-data/dashboard.png
