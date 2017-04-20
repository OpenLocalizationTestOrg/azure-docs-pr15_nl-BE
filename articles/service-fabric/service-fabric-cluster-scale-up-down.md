<properties
   pageTitle="Schaal van een Service Fabric '-cluster in- of uitzoomen | Microsoft Azure"
   description="Een Service Fabric '-cluster in- of uitzoomen vraag door het instellen van regels voor elke verzameling knooppunten type/VM schalen automatisch schalen overeen te schalen. Toevoegen of verwijderen van knooppunten aan een cluster Service Fabric"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="scale-a-service-fabric-cluster-in-or-out-using-auto-scale-rules"></a>Schaal van een Service Fabric '-cluster in- of uitzoomen met de regels voor automatisch schalen

Virtuele machine schaal sets zijn een Azure compute-bron die u gebruiken kunt om te implementeren en beheren van een collectie als een set virtuele machines. Elk knooppunttype dat is gedefinieerd in een cluster Service Fabric is ingesteld als een aparte VM schaal set. Elk knooppunttype kan vervolgens worden geschaald of uit onafhankelijk, hebben verschillende sets van poorten openen kan, en andere capaciteit metrics. Het document [Service Fabric-nodetypes](service-fabric-cluster-nodetypes.md) voor meer informatie. Omdat de Service Fabric knooppunttypen in het cluster worden gemaakt van de VM-schaal wordt ingesteld op de backend, moet u automatisch schalen regels instellen voor elke verzameling knooppunten type VM/schaal.

>[AZURE.NOTE] Uw abonnement hebt onvoldoende cores in de nieuwe VMs waaruit dit cluster toevoegen. Er is geen validatie, zodat u een keer fout bij implementatie, als een van de quotumlimieten worden geraakt.

## <a name="choose-the-node-typevm-scale-set-to-scale"></a>Het knooppunt type VM/schaal ingesteld op schaal kiezen

U bent momenteel niet kunnen opgeven van de regels automatisch schalen voor VM schaal wordt ingesteld met behulp van de portal, dus laat ons met Azure PowerShell (1.0 +) lijst van de knooppunttypen en vervolgens automatisch schalen regels toevoegen aan hen.

Als u de lijst met VM schaal instellen van het cluster, voert u de volgende cmdlets:

```powershell
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Compute/VirtualMachineScaleSets

Get-AzureRmVmss -ResourceGroupName <RGname> -VMScaleSetName <VM Scale Set name>
```

## <a name="set-auto-scale-rules-for-the-node-typevm-scale-set"></a>Automatisch schalen regels instellen voor de verzameling knooppunten type VM/schaal

Als het cluster meerdere knooppunttypen heeft, Herhaal dat dit voor elk knooppunt typen VM/schaal stelt dat u schalen wilt (in of uit). Rekening gehouden met het aantal knooppunten dat u hebt nodig voordat u een automatische schaling instellen. Het minimum aantal knooppunten die voor het primaire knooppunt zijn nodig wordt bepaald door het gekozen niveau van betrouwbaarheid. Lees meer over de [niveaus van betrouwbaarheid](service-fabric-cluster-capacity.md).

>[AZURE.NOTE]  Het verkleinen van het primaire knooppunt, typt u het cluster instabiel in minder dan het minimale aantal maken of omlaag brengen. Dit kan leiden tot verlies van gegevens voor uw toepassingen en de systeemservices.

De functie automatisch schalen is momenteel niet bepaald door de belastingen die uw toepassingen aan de Fabric-Service rapporteren kunnen. Op dit moment de auto-schaal u krijgt zuiver wordt aangedreven door de prestaties dus schaal items die worden uitgestoten door elk van de VM set exemplaren.  

Volg deze instructies [voor het instellen van Automatische-schaal schaal voor elke VM](../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md).

>[AZURE.NOTE] Tenzij het knooppunttype een niveau van de duurzaamheid van goud of zilver heeft in een schaal omlaag scenario moet u roept de [cmdlet verwijderen ServiceFabricNodeState](https://msdn.microsoft.com/library/azure/mt125993.aspx) met de naam van het desbetreffende knooppunt.

## <a name="manually-add-vms-to-a-node-typevm-scale-set"></a>VMs handmatig toevoegen aan een verzameling knooppunten type VM/schaal

De instructies monster/in de [galerie met sitesjablonen snel starten](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) wijzigen van het aantal VMs in elke Nodetype. 

>[AZURE.NOTE] Toevoegen van VMs duurt, zodat niet de toevoegingen aan de momentane worden verwacht. Dus plan om de capaciteit in tijd voor meer dan 10 minuten voordat de VM capaciteit beschikbaar voor de replica's is toevoegen / service exemplaren te worden geplaatst.

## <a name="manually-remove-vms-from-the-primary-node-typevm-scale-set"></a>VMs handmatig verwijderen uit het primaire knooppunt type VM/schaal

>[AZURE.NOTE] De service systeem configuratieservices in het type primaire knooppunt in het cluster worden uitgevoerd. Zodat u nooit moet afsluiten of schaal van het aantal exemplaren in dat knooppunttypen die kleiner zijn dan wat het niveau van betrouwbaarheid garandeert. Raadpleeg [de details van betrouwbaarheid lagen hier](service-fabric-cluster-capacity.md). 

U moet de volgende stappen een VM exemplaar tegelijk worden uitgevoerd. Hierdoor kunnen de systeemservices (en de stateful-services) worden afsluiten op de VM-sessie die u wilt verwijderen en nieuwe replica's zijn gemaakt op andere knooppunten.

1. [Disable ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) uitgevoerd met de bedoeling 'RemoveNode' uit te schakelen van het knooppunt dat u wilt verwijderen (de hoogste instantie in het desbetreffende knooppunttype).

2. [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) uitvoeren om ervoor te zorgen dat het knooppunt inderdaad is overgestapt op uitgeschakeld. Als dit niet het geval is, wacht totdat het knooppunt is uitgeschakeld. U kunt geen wacht niet langer in deze stap.

2. De instructies monster/in de [galerie met sitesjablonen snel starten](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) wijzigen van het aantal VMs door een in die Nodetype. De instantie die verwijderd is de hoogste instantie van de VM. 

3. Herhaal stap 1 tot en met 3 als nodig is, maar nooit het aantal exemplaren in de primaire knooppunttypen lager is dan wat de betrouwbaarheid laag garandeert schaalt. Raadpleeg [de details van betrouwbaarheid lagen hier](service-fabric-cluster-capacity.md). 

## <a name="manually-remove-vms-from-the-non-primary-node-typevm-scale-set"></a>VMs handmatig verwijderen uit de set niet-primaire knooppunt type VM/schaal

>[AZURE.NOTE] Een stateful-service moet u een bepaald aantal knooppunten die altijd tot beschikbaarheid worden onderhouden en behouden van de status van uw service. Aan zeer minimale moet u het aantal knooppunten gelijk is aan het aantal doel replica set van de partitie/service. 

Tegelijkertijd moet u het uitvoeren van de volgende stappen een VM-instantie. Hierdoor kunnen de systeemservices (en de stateful-services) worden afsluiten op de VM-exemplaar dat u wilt verwijderen en nieuwe replica's gemaakt waar u anders.

1. [Disable ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) uitgevoerd met de bedoeling 'RemoveNode' uit te schakelen van het knooppunt dat u wilt verwijderen (de hoogste instantie in het desbetreffende knooppunttype).

2. [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) uitvoeren om ervoor te zorgen dat het knooppunt inderdaad is overgestapt op uitgeschakeld. Niet wachten tot het knooppunt is uitgeschakeld. U kunt geen wacht niet langer in deze stap.

2. De instructies monster/in de [galerie met sitesjablonen snel starten](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) wijzigen van het aantal VMs door een in die Nodetype. Dit wordt nu het hoogste VM-exemplaar verwijderen. 

3. Herhaal stap 1 tot en met 3 als nodig is, maar nooit het aantal exemplaren in de primaire knooppunttypen lager is dan wat de betrouwbaarheid laag garandeert schaalt. Raadpleeg [de details van betrouwbaarheid lagen hier](service-fabric-cluster-capacity.md).

## <a name="behaviors-you-may-observe-in-service-fabric-explorer"></a>Gedrag kunnen zich in Service Fabric Explorer

Wanneer u de schaal van een cluster weerspiegelt de Service Fabric Explorer het aantal knooppunten (VM schaal ingesteld exemplaren) die deel van het cluster uitmaken.  Echter, wanneer u de schaal van een cluster beneden u ziet het verwijderde knooppunt/VM-exemplaar in een foutieve status wordt weergegeven, tenzij u [cmd verwijderen ServiceFabricNodeState](https://msdn.microsoft.com/library/mt125993.aspx) met de naam van het desbetreffende knooppunt aanroepen.   

Hier is de uitleg van dit probleem.

De knooppunten die zijn vermeld in de Service Fabric Explorer zijn een weerspiegeling van wat de Service systeem configuratieservices (FM speciaal) op de hoogte van het aantal knooppunten het cluster had/heeft. Wanneer u de schaal van de VM vastgestelde schalen, de VM is verwijderd maar FM-systeemservice denkt nog steeds dat het knooppunt (die is toegewezen aan de VM die is verwijderd) zal terugkomen. Service Fabric Explorer blijft dus dat knooppunt worden weergegeven (Hoewel de status is mogelijk fout of onbekend).

Om ervoor te zorgen dat een knooppunt wordt verwijderd wanneer een VM wordt verwijderd, hebt u twee mogelijkheden:

1) Kies een niveau van de duurzaamheid van goud of zilver (beschikbaar spoedig) voor de knooppunttypen in het cluster dat biedt u de integratie van de infrastructuur. Die vervolgens wordt automatisch verwijderd de knooppunten van onze systeemstatus services (FM) als u naar beneden schalen.
Verwijzen naar [de details over de niveaus van duurzaamheid hier](service-fabric-cluster-capacity.md)

2) Zodra de VM-sessie is verkleind, moet u de [cmdlet verwijderen ServiceFabricNodeState](https://msdn.microsoft.com/library/mt125993.aspx)aanroepen.

>[AZURE.NOTE] Service Fabric clusters vereisen een bepaald aantal knooppunten die tegelijkertijd de tijd om de beschikbaarheid te onderhouden en behouden van state - aangeduid als "het quorum onderhouden." Dan is het meestal veilig af te sluiten de machines in het cluster tenzij u eerst een [volledige back-up van uw status](service-fabric-reliable-services-backup-restore.md)hebt uitgevoerd.

## <a name="next-steps"></a>Volgende stappen
Lees ook meer over cluster capaciteit plannen, upgraden van een cluster en partitionering van de diensten van de volgende:

- [Plannen van de capaciteit van de cluster](service-fabric-cluster-capacity.md)
- [Cluster-upgrades](service-fabric-cluster-upgrade.md)
- [Partitie stateful services voor maximale schaal](service-fabric-concepts-partitioning.md)

<!--Image references-->
[BrowseServiceFabricClusterResource]: ./media/service-fabric-cluster-scale-up-down/BrowseServiceFabricClusterResource.png
[ClusterResources]: ./media/service-fabric-cluster-scale-up-down/ClusterResources.png
