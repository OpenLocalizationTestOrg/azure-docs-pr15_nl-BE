<properties
    pageTitle="Diagnostische gegevens opslaan en weergeven in Azure opslag | Microsoft Azure"
    description="Azure diagnostische gegevens op te slaan in Azure ophalen en weergeven"
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
    ms.date="08/01/2016"
    ms.author="robb" />

# <a name="store-and-view-diagnostic-data-in-azure-storage"></a>Diagnostische gegevens opslaan en weergeven in Azure opslag

Diagnostische gegevens wordt niet permanent opgeslagen tenzij u deze naar de Microsoft Azure opslag emulator of Azure opslag overbrengen. Eenmaal in de opslag, kunnen deze worden bekeken met een van de verschillende beschikbare hulpmiddelen.

## <a name="specify-a-storage-account"></a>Geef een opslag-account

U opgeven de opslag account die u wilt gebruiken in het bestand ServiceConfiguration.cscfg. De accountgegevens wordt gedefinieerd als een verbindingsreeks in een configuratie-instelling. In het volgende voorbeeld ziet u de standaard-verbindingsreeks voor een nieuwe wolk Service-project in Visual Studio zijn gemaakt:


```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

U kunt deze verbindingsreeks om accountgegevens voor een Azure opslag account wijzigen.

Afhankelijk van het type van de diagnostische gegevens die worden verzameld, gebruikt Azure diagnostische gegevens van de Blob-service of de service van de tabel. De volgende tabel ziet u de gegevensbronnen die worden doorgevoerd en hun notatie.

|Gegevensbron|Opslagindeling|
|---|---|
|Azure-Logboeken|Tabel|
|Logboeken van IIS 7.0|BLOB|
|Azure diagnostische infrastructuur Logboeken|Tabel|
|Traceerlogboeken aanvraag is mislukt|BLOB|
|Windows-gebeurtenislogboeken|Tabel|
|Prestatiemeteritems|Tabel|
|Crashdumps|BLOB|
|Aangepaste foutenlogboeken|BLOB|

## <a name="transfer-diagnostic-data"></a>Diagnostische gegevens overbrengen

SDK 2.5 en hoger, kan het verzoek om diagnostische gegevens overbrengen via het configuratiebestand optreden. U kunt diagnostische gegevens overbrengen op geplande tijden zoals opgegeven in de configuratie.

2.4 SDK en vorige kunt u de diagnostische gegevens als programmatisch overbrengen via het configuratiebestand aanvragen. De programmatische aanpak kunt u doen op aanvraag overdrachten.


>[AZURE.IMPORTANT] Wanneer u een diagnostische gegevens naar een account Azure opslag overbrengen, worden u kosten voor de opslagmiddelen die gebruikmaakt van de diagnostische gegevens.

## <a name="store-diagnostic-data"></a>Diagnostische gegevens op te slaan

Logboekgegevens worden opgeslagen in de opslag van Blob of tabel met de volgende namen:

**Tabellen**

- **WadLogsTable** - Logboeken geschreven in code met behulp van de traceringslistener.

- **WADDiagnosticInfrastructureLogsTable** - diagnostisch bewakingssysteem en de configuratie wordt gewijzigd.

- **WADDirectoriesTable** – mappen die de controle van de diagnostische monitor.  Dit geldt ook voor IIS-logboeken, IIS mislukt verzoek logboeken en aangepaste mappen.  De locatie van het logboekbestand blob is opgegeven in het veld voor de Container en de naam van de blob is in het veld RelativePath.  Het veld AbsolutePath geeft de locatie en de naam van het bestand zoals deze bestond op de Azure virtuele machine.

- **WADPerformanceCountersTable** – prestatiemeteritems.

- **WADWindowsEventLogsTable** – Windows-gebeurtenislogboeken.

**BLOB 's**

- **af-besturingselement-container** : bevat de XML-configuratiebestanden die regelt de Azure diagnostische gegevens (alleen voor 2.4 SDK en vorige).

- **af-iis-failedreqlogfiles** – bevat informatie van aanvragen van IIS is niet vastgelegd.

- **af-iis-logboekbestanden** – bevat informatie over IIS-logboeken.

- **'custom'** : een aangepaste container op basis van de mappen die worden gecontroleerd door een diagnostisch bewakingssysteem configureren.  De naam van deze container blob wordt opgegeven in WADDirectoriesTable.

## <a name="tools-to-view-diagnostic-data"></a>Extra diagnostische gegevens weergeven
Er zijn verschillende hulpprogramma's beschikbaar voor het weergeven van de gegevens na het overbrengen naar opslag. Bijvoorbeeld:

- Explorer in Visual Studio - Server als u de Azure's hebt geïnstalleerd voor Microsoft Visual Studio, kunt u het knooppunt Azure opslag in Server Explorer blob voor alleen-lezen en gegevens in een tabel weergeven van uw accounts Azure opslag. Kunt u de gegevens van uw lokale opslag emulator-account weergeven en ook van opslag accounts u hebt gemaakt voor Azure. Zie voor meer informatie, [surfen en opslagbronnen beheren met Server Explorer](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md).

- [Microsoft Azure Opslagverkenner](../vs-azure-tools-storage-manage-with-storage-explorer.md) is een zelfstandige toepassing waarmee u gemakkelijk werken met gegevens in Azure opslag op Windows, OSX en Linux.

- [Azure Management Studio](http://www.cerebrata.com/products/azure-management-studio/introduction) bevat Azure Diagnostics Manager kunt u bekijken, downloaden en beheren van de diagnostische gegevens die zijn verzameld door de toepassingen op Azure.


## <a name="next-steps"></a>Volgende stappen

[De stroom in een Cloud Services-toepassing met Azure diagnostische gegevens traceren](cloud-services-dotnet-diagnostics-trace-flow.md)
