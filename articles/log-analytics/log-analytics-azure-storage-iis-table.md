<properties
    pageTitle="Blob-opslag gebruiken voor IIS- en opslag voor gebeurtenissen | Microsoft Azure"
    description="Logboek Analytics kunt lezen de logboeken voor Azure services die schrijft diagnostische gegevens naar de tabelopslag of de IIS-logboeken geschreven naar een blob-opslag."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="banders"/>


# <a name="using-blob-storage-for-iis-and-table-storage-for-events"></a>Met behulp van blobopslag voor IIS en tabelopslag voor gebeurtenissen

Logboek Analytics kunt lezen de logboeken voor de volgende services die schrijft diagnostische gegevens naar de tabelopslag of de IIS-logboeken geschreven naar een blob-opslag:

+ Service Fabric-clusters (voorbeeld)
+ Virtuele Machines
+ Web/werknemer rollen

Voordat het logboek Analytics kunt verzamelen van gegevens voor deze bronnen, moet Azure diagnostische gegevens worden ingeschakeld.

Diagnostische gegevens zijn ingeschakeld, kunt u de Azure portal of PowerShell Analytics voor het verzamelen van de Logboeken logboek configureert.

Azure Diagnostics is een Azure extensie waarmee u kunt diagnostische gegevens verzamelen van een werknemer rol, Webrol of virtuele machine met Azure. De gegevens worden opgeslagen in een account Azure opslag en kan vervolgens worden verzameld door logboek Analytics.

Voor Log Analytics voor het verzamelen van deze logboeken Azure diagnostische gegevens, moeten de logboeken op de volgende locaties:

| Logboektype | Resourcetype | Locatie |
| -------- | ------------- | -------- |
| IIS-logboeken | Virtuele Machines <br> Web-rollen <br> Rollen van de werknemer | af-iis-logboekbestanden (Blob Storage) |
| Syslog | Virtuele Machines | LinuxsyslogVer2v0 (tabel opslag) |
| Operationele gebeurtenissen service Fabric | Service Fabric-knooppunten | WADServiceFabricSystemEventTable |
| Betrouwbare Actor-servicegebeurtenissen Fabric | Service Fabric-knooppunten | WADServiceFabricReliableActorEventTable |
| Betrouwbare Service-servicegebeurtenissen Fabric | Service Fabric-knooppunten | WADServiceFabricReliableServiceEventTable |
| Windows-gebeurtenislogboeken | Service Fabric-knooppunten <br> Virtuele Machines <br> Web-rollen <br> Rollen van de werknemer | WADWindowsEventLogsTable (tabelopslag) |
| ETW Windows Logboeken | Service Fabric-knooppunten <br> Virtuele Machines <br> Web-rollen <br> Rollen van de werknemer | WADETWEventTable (tabelopslag) |

>[AZURE.NOTE] IIS-logboeken van Azure Websites worden momenteel niet ondersteund.

Voor virtuele machines hebt u de mogelijkheid van het installeren van de [agent Analytics logboek](log-analytics-azure-vm-extension.md) in uw virtuele machine om meer inzicht te krijgen. Niet alleen om IIS-logboeken en gebeurtenislogboeken te analyseren, kunt u aanvullende analyse inclusief configuratie wijzigingen bijhouden, SQL-beoordeling en evaluatie van de update uitvoeren.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a>Azure diagnostische gegevens in een virtuele machine inschakelen voor het logboek met systeemgebeurtenissen en IIS logboek collectie

Gebruik de volgende procedure om Azure diagnostische gegevens in een virtuele machine voor het logboek met systeemgebeurtenissen en IIS verzamelen met behulp van de portal voor Microsoft Azure.

### <a name="to-enable-azure-diagnostics-in-a-virtual-machine-with-the-azure-portal"></a>Azure diagnostische gegevens in een virtuele machine met Azure portal inschakelen

1. De Agent VM installeren wanneer u een virtuele machine maakt. Als de virtuele machine al bestaat, moet u controleren dat de Agent VM al is geïnstalleerd.
    - In de portal Azure, navigeer naar de virtuele machine **Optionele configuratie**en klik op **Diagnostische gegevens** en selecteer **Status** ingesteld **op**.

    Na voltooiing heeft de VM de Azure Diagnostics extensie geïnstalleerd en uitgevoerd. Deze uitbreiding is verantwoordelijk voor het verzamelen van diagnostische gegevens.

2. Controle inschakelen en configureren op een bestaande VM voor logboekregistratie van gebeurtenissen. U kunt diagnostische gegevens op het niveau van de VM. Als u wilt een diagnoseprogramma inschakelen en vervolgens de registratie van gebeurtenissen configureren, moet u de volgende stappen uitvoeren:
    1. Selecteer de VM.
    2. Klik op **controleren**.
    3. Klik op **Diagnostische gegevens**.
    4. De **Status** wordt ingesteld op **ON**.
    5. Selecteer elk diagnostische gegevens van het logboek dat u wilt verzamelen.
    7. Klik op **OK**.

Met Azure PowerShell kunt u nauwkeuriger opgeven de gebeurtenissen die zijn vastgelegd in Azure opslag. Raadpleeg met Azure diagnostische gegevens weggeschreven naar de tabelopslag of de IIS-logboeken geschreven naar een blob-gegevens te [verzamelen](log-analytics-azure-storage-json.md).


## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a>Azure diagnostische gegevens in een Web-functie voor IIS-logboekbestanden en gebeurtenis collectie inschakelen

Zie [Hoe te schakelen diagnostische gegevens in een Cloud Service](../cloud-services/cloud-services-dotnet-diagnostics.md) voor algemene stappen voor het inschakelen van Azure diagnostische gegevens. De onderstaande instructies deze informatie gebruiken en aanpassen voor gebruik met Analytics logboek.

Met diagnostische gegevens van Azure ingeschakeld:

- IIS-logboeken worden standaard opgeslagen met logboekgegevens overgedragen op de gewenste interval scheduledTransferPeriod.
- Windows-gebeurtenislogboeken worden standaard niet overgedragen.

### <a name="to-enable-diagnostics"></a>Diagnose inschakelen

Windows-Logboeken inschakelen of wijzigen van de scheduledTransferPeriod configureren Azure diagnostische gegevens van het XML-configuratiebestand (diagnostics.wadcfg), zoals in [stap 4: de diagnostische gegevens van het configuratiebestand maken en installeren van de extensie](../cloud-services/cloud-services-dotnet-diagnostics.md)

Het volgende voorbeeld configuratiebestand verzamelt de logboeken van IIS en alle gebeurtenissen van de toepassings- en systeemlogboeken:

```
    <?xml version="1.0" encoding="utf-8" ?>
    <DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"
          configurationChangePollInterval="PT1M"
          overallQuotaInMB="4096">

      <Directories bufferQuotaInMB="0"
         scheduledTransferPeriod="PT10M">  
        <!-- IISLogs are only relevant to Web roles -->
        <IISLogs container="wad-iis" directoryQuotaInMB="0" />
      </Directories>

      <WindowsEventLog bufferQuotaInMB="0"
         scheduledTransferLogLevelFilter="Verbose"
         scheduledTransferPeriod="PT10M">
        <DataSource name="Application!*" />
        <DataSource name="System!*" />
      </WindowsEventLog>

    </DiagnosticMonitorConfiguration>
```

Zorg ervoor dat uw ConfigurationSettings een opslag-account geeft:

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

De **accountnaam** en **AccountKey** -waarden worden gevonden in Azure portal in het dashboard opslag account onder toegangstoetsen beheren. Het protocol voor de verbindingsreeks moet **https**.

Nadat de bijgewerkte diagnostische wordt toegepast op uw cloud-service en het diagnostische gegevens naar Azure opslag wegschrijven, hebt u Analytics logboekbestand configureren.


## <a name="use-the-azure-portal-to-collect-logs-from-azure-storage"></a>De Azure portal gebruiken voor het verzamelen van Logboeken van Azure opslag

U kunt de Azure portal logboek Analytics voor het verzamelen van de logboeken voor de volgende Azure services configureren:

+ Service Fabric-clusters
+ Virtuele Machines
+ Web/werknemer rollen

Ga naar uw werkruimte logboek Analytics in Azure portal, en de volgende taken uitvoeren:

1. Klik op *opslag rekeningen logboeken*
2. Klik op de taak *toevoegen*
3. Selecteer de opslag rekening met de diagnostische logboeken
  - Deze account is een klassieke opslag-account of een account Azure Resource Manager opslag
4. Selecteer het gegevenstype dat u wilt verzamelen van Logboeken voor
  - De opties zijn de logboeken van IIS; Gebeurtenissen; Syslog (Linux); ETW-Logboeken; Service Fabric-gebeurtenissen
5. De waarde voor bron op basis van het gegevenstype wordt automatisch ingevuld en kan niet worden gewijzigd
6. Klik op OK om de configuratie opslaan

Herhaal stap 2-6 voor gegevenstypen gewenste logboek Analytics voor het verzamelen en extra opslagruimte accounts.

In ongeveer 30 minuten bent u gegevens uit de opslag-account in het logboek voor Analytics. U ziet alleen de gegevens die naar de opslag geschreven nadat de configuratie is toegepast. Logboek Analytics leest niet de bestaande gegevens van de opslag-account.

>[AZURE.NOTE] De portal wordt niet gevalideerd of de bron in de opslag bestaat of als nieuwe gegevens worden geschreven.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a>Azure diagnostische gegevens in een virtuele machine inschakelen voor het logboek met systeemgebeurtenissen en IIS aanmelden met PowerShell collectie

Volg de stappen in het [Logboek-Analytics indexeren Azure diagnostische gegevens configureren](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) PowerShell gebruiken om te lezen van Azure diagnostische gegevens die worden vastgelegd in de tabelopslag.

Met Azure PowerShell kunt u nauwkeuriger opgeven de gebeurtenissen die zijn vastgelegd in Azure opslag.
Voor meer informatie Zie [Diagnostics inschakelen in Azure virtuele Machines](../virtual-machines-dotnet-diagnostics.md).

U kunt inschakelen en bijwerken van Azure diagnostics de volgende PowerShell-script gebruiken.
U kunt dit script ook gebruiken met een aangepaste configuratie voor logboekregistratie.
Wijzig het script voor de opslag account, servicenaam en naam van de virtuele machine.
Het script maakt gebruik van cmdlets voor klassieke virtuele machines.

Bekijk het volgende script monster, kopieert u het naar wens aanpassen, het monster als een PowerShell script-bestand opslaan en vervolgens het script uitvoeren.

```
    #Connect to Azure
    Add-AzureAccount

    # settings to change:
    $wad_storage_account_name = "myStorageAccount"
    $service_name = "myService"
    $vm_name = "myVM"

    #Construct Azure Diagnostics public config and convert to config format

    # Collect just system error events:
    $wad_xml_config = "<WadCfg><DiagnosticMonitorConfiguration><WindowsEventLog scheduledTransferPeriod=""PT1M""><DataSource name=""System!* "" /></WindowsEventLog></DiagnosticMonitorConfiguration></WadCfg>"

    $wad_b64_config = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($wad_xml_config))
    $wad_public_config = [string]::Format("{{""xmlCfg"":""{0}""}}",$wad_b64_config)

    #Construct Azure diagnostics private config

    $wad_storage_account_key = (Get-AzureStorageKey $wad_storage_account_name).Primary
    $wad_private_config = [string]::Format("{{""storageAccountName"":""{0}"",""storageAccountKey"":""{1}""}}",$wad_storage_account_name,$wad_storage_account_key)

    #Enable Diagnostics Extension for Virtual Machine

    $wad_extension_name = "IaaSDiagnostics"
    $wad_publisher = "Microsoft.Azure.Diagnostics"
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of the extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```


## <a name="next-steps"></a>Volgende stappen

- [Gebruik JSON bestanden in blob-opslag](log-analytics-azure-storage-json.md) te lezen van de logboeken van Azure services dat diagnostische gegevens van het schrijven naar een blob-opslag in JSON-indeling.
- [Inschakelen oplossingen](log-analytics-add-solutions.md) bieden inzicht in de gegevens.
- [Zoekopdrachten gebruiken](log-analytics-log-searches.md) om de gegevens te analyseren.
