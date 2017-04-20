<properties
    pageTitle="Azure diagnostics (.NET) gebruiken met Cloud Services | Microsoft Azure"
    description="Azure diagnostische gegevens gebruiken om gegevens te verzamelen van Azure cloud Services voor foutopsporing, het meten van prestaties, controle, analyse van het netwerkverkeer en meer."
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="01/25/2016"
    ms.author="robb"/>



# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a>Azure Diagnostics in Azure Cloud Services inschakelen

Overzicht [Azure diagnostische gegevens](../azure-diagnostics.md) voor de achtergrond op Azure diagnostische gegevens.


## <a name="how-to-enable-diagnostics-in-a-worker-role"></a>Het inschakelen van diagnostische gegevens in een rol werknemer

In dit stapsgewijs wordt beschreven hoe u voor het implementeren van een werknemer Azure rol die telemetriegegevens met behulp van de klasse gebeurtenisbron .NET genereert. Azure diagnostische gegevens van wordt de telemetriegegevens verzamelen en opslaan in een account Azure opslag gebruikt. Bij het maken van de rol van een werknemer kunnen Visual Studio automatisch Diagnostics 1.0 als onderdeel van de oplossing in Azure SDK voor .NET 2.4 en eerder. De volgende instructies beschrijven het proces voor het maken van de functie van werknemer Diagnostics 1.0 van de oplossing en implementeren Diagnostics 1.2 of 1.3 uitschakelen aan de rol van de werknemer.

### <a name="pre-requisites"></a>Minimumvereisten
In dit artikel wordt ervan uitgegaan dat u hebt een abonnement op Azure en gebruikmaakt van Visual Studio 2013 met de SDK Azure. Als u niet een Azure-abonnement hebt, kunt u zich aanmelden voor de [Gratis proefperiode][]. Zorg ervoor dat [installeren en configureren van Azure PowerShell versie 0.8.7 of hoger][].

### <a name="step-1-create-a-worker-role"></a>Stap 1: Maak een rol werknemer
1.  Start **Visual Studio 2013**.
2.  Een nieuw project voor **Azure Cloud-Service** van de sjabloon **wolk** die .NET Framework 4.5-doelen maken.  Geef het project 'WadExample' en klik op Ok.
3.  **Rol van de werknemer** en klik op Ok. Het project wordt gemaakt.
4.  Dubbelklik op het bestand **WorkerRole1** -eigenschappen in de **Solution Explorer**.
5.  In de **configuratie** tab un-check **Diagnostics inschakelen** om te schakelen Diagnostics 1.0 (Azure SDK 2.4 en eariler).
6.  De oplossing om te controleren of er geen fouten maken.

### <a name="step-2-instrument-your-code"></a>Stap 2: Uw code Instrument
De inhoud van de WorkerRole.cs vervangen door de volgende code. De klasse SampleEventSourceWriter, die is overgeërfd van de [Klasse gebeurtenisbron][]logboekregistratie op vier manieren worden geïmplementeerd: **SendEnums**, **MessageMethod**, **SetOther** en **HighFreq**. De eerste parameter aan de methode **WriteEvent** wordt de ID voor de betreffende gebeurtenis. De methode Run implementeert een oneindige lus die elk van de logboekregistratiemethoden geïmplementeerd in de klasse **SampleEventSourceWriter** elke 10 seconden wordt aangeroepen.

    using Microsoft.WindowsAzure.ServiceRuntime;
    using System;
    using System.Diagnostics;
    using System.Diagnostics.Tracing;
    using System.Net;
    using System.Threading;

    namespace WorkerRole1
    {
    sealed class SampleEventSourceWriter : EventSource
    {
        public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
        public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); }// Cast enums to int for efficient logging.
        public void MessageMethod(string Message) { if (IsEnabled())  WriteEvent(2, Message); }
        public void SetOther(bool flag, int myInt) { if (IsEnabled())  WriteEvent(3, flag, myInt); }
        public void HighFreq(int value) { if (IsEnabled()) WriteEvent(4, value); }

    }

    enum MyColor
    {
        Red,
        Blue,
        Green
    }

    [Flags]
    enum MyFlags
    {
        Flag1 = 1,
        Flag2 = 2,
        Flag3 = 4
    }

    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
            // This is a sample worker implementation. Replace with your logic.
            Trace.TraceInformation("WorkerRole1 entry point called");

            int value = 0;

            while (true)
            {
                Thread.Sleep(10000);
                Trace.TraceInformation("Working");

                // Emit several events every time we go through the loop
                for (int i = 0; i < 6; i++)
                {
                    SampleEventSourceWriter.Log.SendEnums(MyColor.Blue, MyFlags.Flag2 | MyFlags.Flag3);
                }

                for (int i = 0; i < 3; i++)
                {
                    SampleEventSourceWriter.Log.MessageMethod("This is a message.");
                    SampleEventSourceWriter.Log.SetOther(true, 123456789);
                }

                if (value == int.MaxValue) value = 0;
                SampleEventSourceWriter.Log.HighFreq(value++);
            }
        }

        public override bool OnStart()
        {
            // Set the maximum number of concurrent connections
            ServicePointManager.DefaultConnectionLimit = 12;

            // For information on handling configuration changes
            // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.

            return base.OnStart();
        }
    }
    }


### <a name="step-3-deploy-your-worker-role"></a>Stap 3: Implementatie van uw rol werknemer
1.  De rol van de werknemer op Azure vanuit Visual Studio implementeren door het **WadExample** -project in de Solution Explorer en vervolgens **publiceren** in het menu **Build** te selecteren.
2.  Kies uw abonnement.
3.  Selecteer **Nieuw**in het dialoogvenster **Publicatie-instellingen van Microsoft Azure** .
4.  Voer in het dialoogvenster **Cloud-Service maken en opslag Account** een **naam** (bijvoorbeeld "WadExample") en selecteer een regio of een groep van affiniteit.
5.  **Staging** **omgeving** instellen.
6.  Andere **Instellingen** desgewenst wijzigen en klik op **publiceren**.
7.  Nadat de implementatie is voltooid controleren of in de klassieke Azure portal uw cloud-service in een staat **wordt uitgevoerd** .

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-the-extension"></a>Stap 4: Het bestand met de diagnostische gegevens maken en installeren van de uitbreiding
1.  De schemadefinitie voor openbare configuratie bestand door het uitvoeren van de volgende opdracht met PowerShell downloaden:
2.
        (Get-AzureServiceAvailableExtension - Extensienaam 'PaaSDiagnostics' - ProviderNamespace 'Microsoft.Azure.Diagnostics'). PublicConfigurationSchema | Uitgaande bestand-codering utf8 - FilePath 'WadConfig.xsd'

2.  Een XML-bestand toevoegen aan uw project **WorkerRole1** door met de rechtermuisknop op het **WorkerRole1** -project en selecteer **toevoegen** -> **Nieuw Item...**  ->  **Artikelen visual C#** -> **Data** -> **XML-bestand**. Noem het bestand "WadExample.xml".

    ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)

3.  De WadConfig.xsd koppelen aan het configuratiebestand. Zorg ervoor dat het WadExample.xml editor-venster het actieve venster is. Druk op **F4** om het venster **Eigenschappen** openen. Klik op de **schema** -eigenschap in het **eigenschappenvenster** . Klik op de **...** in de eigenschap **schema's** . Klik op de **toevoegen...** en navigeer naar de locatie waar u het XSD-bestand hebt opgeslagen en selecteer het bestand WadConfig.xsd. Klik op **OK**.
4.  De inhoud van het configuratiebestand WadExample.xml vervangen door het volgende XML-bestand en sla het bestand. Dit configuratiebestand wordt gedefinieerd voor een aantal prestatiemeteritems te verzamelen: één voor de CPU-gebruik en één voor geheugengebruik. Vervolgens wordt de configuratie van de vier gebeurtenissen die overeenkomen met de methoden in de klasse SampleEventSourceWriter.

```
        <?xml version="1.0" encoding="utf-8"?>
        <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
            <WadCfg>
                <DiagnosticMonitorConfiguration overallQuotaInMB="25000">
                <PerformanceCounters scheduledTransferPeriod="PT1M">
                    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />
                    <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT1M" unit="bytes"/>
                    </PerformanceCounters>
                    <EtwProviders>
                        <EtwEventSourceProviderConfiguration provider="SampleEventSourceWriter" scheduledTransferPeriod="PT5M">
                            <Event id="1" eventDestination="EnumsTable"/>
                            <Event id="2" eventDestination="MessageTable"/>
                            <Event id="3" eventDestination="SetOtherTable"/>
                            <Event id="4" eventDestination="HighFreqTable"/>
                            <DefaultEvents eventDestination="DefaultTable" />
                        </EtwEventSourceProviderConfiguration>
                    </EtwProviders>
                </DiagnosticMonitorConfiguration>
            </WadCfg>
        </PublicConfig>
```

### <a name="step-5-install-diagnostics-on-your-worker-role"></a>Stap 5: Diagnostische gegevens van het installeren van de rol van de werknemer
De PowerShell-cmdlets voor het beheren van diagnostische gegevens op een webpagina of werknemer rol zijn: Set AzureServiceDiagnosticsExtension, Get-AzureServiceDiagnosticsExtension en software-AzureServiceDiagnosticsExtension.

1.  Azure PowerShell openen.
2.  Diagnostische gegevens van het installeren van de rol van de werknemer (Vervang *StorageAccountKey* met de sleutel van de account opslag voor uw account wadexample opslag) het script uitvoeren:

```
    $storage_name = "wadexample"
    $key = "<StorageAccountKey>"
    $config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
    $service_name="wadexample"
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a>Stap 6: Uw telemetriegegevens bekijken
Navigeer naar de account wadexample opslag in Visual Studio **Server Explorer** . Na de cloud al service actief is ongeveer 5 minuten ziet u de tabellen, **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** en **WADSetOtherTable**. Dubbelklik op een van de tabellen weer te geven van de telemetrie die zijn verzameld.
    ![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)


## <a name="configuration-file-schema"></a>Configuration File Schema

Het configuratiebestand van de diagnostische gegevens definieert de waarden die worden gebruikt voor diagnostische configuratie-instellingen te initialiseren wanneer de agent Diagnostiek wordt gestart. Zie de [meest recente schema: referentie](https://msdn.microsoft.com/library/azure/mt634524.aspx) voor geldige waarden en voorbeelden.

## <a name="troubleshooting"></a>Het oplossen van problemen

Als u problemen ondervindt, Zie [Azure Diagnostics problemen oplossen](../azure-diagnostics-troubleshooting.md) voor hulp bij veelvoorkomende problemen.

## <a name="next-steps"></a>Volgende stappen
[Zie een lijst van de virtuele machine verwante Azure Diagnostics artikelen](azure-diagnostics.md#cloud-services) te wijzigen van de gegevens die u verzamelt, problemen of meer informatie over diagnostische gegevens in het algemeen.


[Gebeurtenisbron klasse]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Gratis proefversie]: http://azure.microsoft.com/pricing/free-trial/
[Installeer en configureer Azure PowerShell versie 0.8.7 of hoger]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
