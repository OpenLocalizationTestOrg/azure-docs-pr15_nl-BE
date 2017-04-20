<properties
   pageTitle="Verzamelen van logboeken met Linux Azure Diagnostics | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe Azure diagnostische gegevens instellen voor het verzamelen van Logboeken van een Service Fabric Linux cluster uitgevoerd in Azure."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/28/2016"
   ms.author="subramar"/>


# <a name="collect-logs-by-using-azure-diagnostics"></a>Verzamelen van logboeken met Azure diagnostische gegevens

> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
- [Linux](service-fabric-diagnostics-how-to-setup-lad.md)

Wanneer u een Azure Service Fabric '-cluster uitvoert, is het een goed idee om de logboeken van de knooppunten op een centrale locatie verzamelen. De logboeken op een centrale locatie die gemakkelijk te analyseren en oplossen van problemen, of ze nu in de services, de toepassing of het cluster zelf. Een manier om te uploaden en verzamelen van Logboeken is de uitbreiding Azure diagnostische gegevens, die Logboeken worden geüpload naar Azure opslag gebruiken. U kunt lezen de gebeurtenissen uit de opslag en plaats deze in een product als [Elastische zoeken](service-fabric-diagnostic-how-to-use-elasticsearch.md) of een andere oplossing voor het parseren van het logboek.

## <a name="log-sources-that-you-might-want-to-collect"></a>Logboek-bronnen die u wilt verzamelen
- **Service Fabric logs**: uitlaatgassen van het platform via [LTTng](http://lttng.org) en geüpload naar uw account voor opslag. Logboeken zijn operationele gebeurtenissen of runtime-gebeurtenissen die het platform genereert. Deze logboeken worden opgeslagen op de locatie die het cluster manifest geeft. (Als u de details van de opslag, zoekt u de tag **AzureTableWinFabETWQueryable** en zoek naar **StoreConnectionString**.)
- **Application-gebeurtenissen**: uitlaatgassen van de code van de service. U kunt de logboekregistratie oplossingen die tekst gebaseerde logboekbestanden--bijvoorbeeld LTTng geschreven. Zie voor meer informatie de documentatie bij de LTTng op uw toepassing te traceren.  


## <a name="deploy-the-diagnostics-extension"></a>De uitbreiding van de diagnostische gegevens implementeren
De eerste stap bij het verzamelen van Logboeken wordt de extensie diagnostische gegevens op elk van de VMs in de cluster-Service Fabric geïmplementeerd. De uitbreiding van de diagnostische logboeken op elke VM verzamelt en ze uploaden naar de opslag-account die u opgeeft. De stappen zijn afhankelijk van of u de Azure portal of Azure Resource Manager gebruiken.

De uitbreiding van de diagnostische gegevens op de VMs in het cluster installeren als onderdeel van het maken van het cluster, ingesteld **Diagnostische gegevens** **op**. Nadat u het cluster hebt gemaakt, kunt u deze instelling niet wijzigen met behulp van de portal.

Configureer vervolgens Linux Azure Diagnostics (LAD) om de bestanden verzamelen en plaats deze in uw account voor de opslag. Dit proces wordt uitgelegd als scenario 3 ('uploaden uw eigen log files") in het artikel [LAD gebruiken om te controleren en diagnosticeren van Linux VMs](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension.md). Na dit proces krijgt u toegang tot de traces. U kunt de traces uploaden naar een visualizer van uw keuze.

U kunt ook de uitbreiding van de diagnostische gegevens implementeren met behulp van bronbeheer Azure. Het proces is vergelijkbaar voor Windows en Linux en voor Windows-clusters in [het verzamelen van logboeken met Azure diagnostische gegevens](service-fabric-diagnostics-how-to-setup-wad.md)wordt beschreven.

U kunt ook Operations Management Suite, zoals beschreven in het [Operations Management Suite logboek Analytics met Linux](https://blogs.technet.microsoft.com/hybridcloud/2016/01/28/operations-management-suite-log-analytics-with-linux/).

Nadat u deze configuratie, controleert de LAD agent de opgegeven logboekbestanden. Wanneer u een nieuwe regel wordt toegevoegd aan het bestand, maakt het een syslog-vermelding die wordt verzonden naar de opslag die u hebt opgegeven.


## <a name="next-steps"></a>Volgende stappen
Zie [LTTng documentatie](http://lttng.org/docs) en [LAD met behulp van](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension.md)inzicht in meer detail welke gebeurtenissen u onderzoekt tijdens het oplossen van problemen.
