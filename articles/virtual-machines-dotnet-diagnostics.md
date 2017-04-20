<properties
    pageTitle="Azure diagnostische gegevens gebruiken in virtuele Machines | Microsoft Azure"
    description="Azure diagnostische gegevens gebruiken om gegevens te verzamelen van Azure Virtual Machines voor het opsporen van fouten, het meten van prestaties, controle, analyse van het netwerkverkeer en meer."
    services="virtual-machines"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/20/2016"
    ms.author="robb"/>



# <a name="enabling-diagnostics-in-azure-virtual-machines"></a>Diagnostische gegevens in Azure Virtual Machines schakelen

Overzicht [Azure diagnostische gegevens](azure-diagnostics.md) voor de achtergrond op Azure diagnostische gegevens.

## <a name="how-to-enable-diagnostics-in-a-virtual-machine"></a>Het inschakelen van diagnostische gegevens in een virtuele Machine

In dit stapsgewijs wordt beschreven hoe u extern diagnostische gegevens een Azure virtual machine installeert vanaf een ontwikkelcomputer. U leert ook hoe een toepassing die wordt uitgevoerd op die Azure virtuele machine en telemetriegegevens met behulp van de .NET- [Klasse gebeurtenisbron][]zendt. Azure diagnostische gegevens van wordt de telemetrie verzamelen en opslaan in een account Azure opslag gebruikt.

### <a name="pre-requisites"></a>Minimumvereisten
In dit stapsgewijs wordt ervan uitgegaan dat u hebt een abonnement op Azure en gebruikmaakt van Visual Studio 2013 met de SDK Azure. Als u niet een Azure-abonnement hebt, kunt u zich aanmelden voor de [Gratis proefperiode][]. Zorg ervoor dat [installeren en configureren van Azure PowerShell versie 0.8.7 of hoger][].

### <a name="step-1-create-a-virtual-machine"></a>Stap 1: Een virtuele Machine maken
1.  Op de computer, start u Visual Studio 2013.
2.  In Visual Studio **Server Explorer** **Azure**vouwen, klik met de rechtermuisknop op **virtuele Machines** en selecteer **virtuele Machine maken**.
3.  Uw abonnement op Azure selecteren in het dialoogvenster **Kies een abonnement** en klik op **volgende**.
4.  **Windows Server 2012 R2 Datacenter, November 2014** in het dialoogvenster **selecteert u de afbeelding voor een virtuele Machine** en klik op **volgende**.
5.  Stel name in virtuele machine op 'wadexample' in de **Basisinstellingen van de virtuele Machine**. Stel uw gebruikersnaam en wachtwoord en klik op **volgende**.
6.  Maak een nieuwe wolk service met de naam 'wadexampleVM' in het dialoogvenster **Instellingen voor Cloud-Service** . Maak een nieuwe opslag-account met de naam 'wadexample' en klik op **volgende**.
7.  Klik op **maken**.

### <a name="step-2-create-your-application"></a>Stap 2: Uw toepassing maken
1.  Op de computer, start u Visual Studio 2013.
2.  Maak een nieuw Visual C# consoletoepassing die gericht is op .NET Framework 4.5. De naam van het project 'WadExampleVM'.
    ![CloudServices_diag_new_project](./media/virtual-machines-dotnet-diagnostics/NewProject.png)
3.  De inhoud van de Program.cs vervangen door de volgende code. De klasse **SampleEventSourceWriter** logboekregistratie op vier manieren worden geïmplementeerd: **SendEnums**, **MessageMethod**, **SetOther** en **HighFreq**. De eerste parameter aan de methode WriteEvent wordt de ID voor de betreffende gebeurtenis. De methode Run implementeert een oneindige lus die elk van de logboekregistratiemethoden geïmplementeerd in de klasse **SampleEventSourceWriter** elke 10 seconden wordt aangeroepen.

        using System;
        using System.Diagnostics;
        using System.Diagnostics.Tracing;
        using System.Threading;

        namespace WadExampleVM
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

        class Program
        {
        static void Main(string[] args)
        {
            Trace.TraceInformation("My application entry point called");

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
        }
        }


4.  Sla het bestand en selecteer **Oplossing bouwen** in het menu **Build** om te bouwen van uw code.


### <a name="step-3-deploy-your-application"></a>Stap 3: Uw toepassing implementeren
1.  Klik met de rechtermuisknop op het **WadExampleVM** -project in de **Solution Explorer** en kies **Map openen in Verkenner**.
2.  Ga naar de map *bin\Debug* en kopieer alle bestanden (WadExampleVM.*)
3.  In **Server Explorer** met de rechtermuisknop op de virtuele machine en kies **verbinding maken via Extern bureaublad**.
4.  Maak een map met de naam WadExampleVM en uw toepassing in de map bestanden plakken wanneer een verbinding met de VM.
5.  Start de toepassing WadExampleVM.exe. Hier ziet u een lege consolevenster.

### <a name="step-4-create-your-diagnostics-configuration-and-install-the-extension"></a>Stap 4: De configuratie van de diagnostische gegevens maken en installeren van de uitbreiding
1.  De schemadefinitie voor openbare configuratie bestand downloaden naar de computer door het uitvoeren van de volgende PowerShell-opdracht:

        (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'

2.  Een nieuw XML-bestand openen in Visual Studio, hetzij in een project met geen projecten openen die u al hebt geopend of in een Visual Studio-exemplaar. Selecteer **toevoegen**in Visual Studio -> **Nieuw Item...**  ->  **Artikelen visual C#** -> **Data** -> **XML-bestand**. Noem het bestand "WadExample.xml"
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

### <a name="step-5-remotely-install-diagnostics-on-your-azure-virtual-machine"></a>Stap 5: Op afstand installeren diagnostische gegevens over uw Azure Virtual Machine
De PowerShell-cmdlets voor het beheren van diagnostische gegevens op een VM zijn: Set AzureVMDiagnosticsExtension, Get-AzureVMDiagnosticsExtension en software-AzureVMDiagnosticsExtension.

1.  Open op uw computer developer Azure PowerShell.
2.  Diagnostiek op afstand installeren op uw VM (Vervang *StorageAccountKey* met de sleutel van de account opslag voor uw account wadexamplevm opslag) het script uitvoeren:

        $storage_name = "wadexamplevm"
        $key = "<StorageAccountKey>"
        $config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExampleVM\WadExampleVM\WadExample.xml"
        $service_name="wadexamplevm"
        $vm_name="WadExample"
        $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
        $VM1 = Get-AzureVM -ServiceName $service_name -Name $vm_name
        $VM2 = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $config_path -Version "1.*" -VM $VM1 -StorageContext $storageContext
        $VM3 = Update-AzureVM -ServiceName $service_name -Name $vm_name -VM $VM2.VM


### <a name="step-6-look-at-your-telemetry-data"></a>Stap 6: Uw telemetriegegevens bekijken
Navigeer naar de account wadexample opslag in Visual Studio **Server Explorer** . Nadat de VM ongeveer 5 minuten wordt uitgevoerd, moet u de tabellen, **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** en **WADSetOtherTable**zien. Dubbelklik op een van de tabellen weer te geven van de telemetrie die zijn verzameld.

![CloudServices_diag_wadexamplevm_tables](./media/virtual-machines-dotnet-diagnostics/WadExampleVMTables.png)

## <a name="configuration-file-schema"></a>Configuratie bestand schema

Het configuratiebestand van de diagnostische gegevens definieert de waarden die worden gebruikt voor diagnostische configuratie-instellingen te initialiseren wanneer de agent Diagnostiek wordt gestart. Zie de [meest recente schema: referentie](https://msdn.microsoft.com/library/azure/mt634524.aspx) voor geldige waarden en voorbeelden.

## <a name="troubleshooting"></a>Het oplossen van problemen

Zie [Probleemoplossing Azure diagnostische gegevens](azure-diagnostics-troubleshooting.md) voor meer informatie.


## <a name="next-steps"></a>Volgende stappen
[Zie een lijst van de virtuele machine verwante Azure Diagnostics artikelen](azure-diagnostics.md#virtual-machines-using-azure-diagnostics) te wijzigen van de gegevens die u verzamelt, problemen of meer informatie over diagnostische gegevens in het algemeen.


[Gebeurtenisbron klasse]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Gratis proefversie]: http://azure.microsoft.com/pricing/free-trial/
[Installeer en configureer Azure PowerShell versie 0.8.7 of hoger]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
