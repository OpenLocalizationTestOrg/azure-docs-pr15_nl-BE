<properties
   pageTitle="Met Windows VMs in meerdere regio's voor hoge beschikbaarheid | Verwijzen naar architectuur | Microsoft Azure"
   description="Het implementeren van VMs in meerdere regio's op Azure voor hoge beschikbaarheid en tolerantie."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/21/2016"
   ms.author="mwasson"/>

# <a name="running-windows-vms-in-multiple-regions-for-high-availability"></a>Windows VMs actief in meerdere regio's voor hoge beschikbaarheid

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

> [AZURE.SELECTOR]
- [Linux VMs actief in meerdere regio's voor hoge beschikbaarheid](guidance-compute-multiple-datacenters-linux.md)
- [Windows VMs actief in meerdere regio's voor hoge beschikbaarheid](guidance-compute-multiple-datacenters.md)

In dit artikel, wordt aangeraden een reeks procedures uit te voeren Windows virtuele machines (VMs) in meerdere landen Azure, beschikbaarheid en een robuuste disaster recovery-infrastructuur.

> [AZURE.NOTE] Azure heeft twee verschillende modellen: [Resource Manager] [ resource groups] en klassiek. In dit artikel wordt een resourcemanager, wordt aangeraden voor nieuwe implementaties.

Een architectuur met meerdere regio kan bieden hogere beschikbaarheid dan het implementeren van een bepaalde regio. Als een regionale storing van invloed is op de primaire regio, kunt u [Verkeer Manager] [ traffic-manager] voor failover uitvoeren naar de secundaire regio. Deze architectuur kan ook helpen als een afzonderlijke subsysteem van de toepassing mislukt. 

Er zijn diverse algemene benaderingen voor het bereiken van hoge beschikbaarheid in datacenters:

- Actief/passief met hot stand-by. Verkeer wordt één regio, terwijl de andere wacht op stand-by. VMs in de secundaire regio zijn toegewezen en wordt uitgevoerd op alle tijden.

- Actief/passief met koude stand-by. De zelfde, maar VMs in de secundaire regio worden niet toegewezen totdat die nodig zijn voor failover. Deze aanpak kost minder worden uitgevoerd, maar wordt over het algemeen meer storingstijd tijdens een storing.

- Actief/actief. Beide regio's actief zijn en gelijkmatig verdeeld tussen hen. Als één gegevenscentrum niet beschikbaar is, is uit de queryrotatie gehaald. 

Deze architectuur is gericht op actieve/passieve met hot stand-by, met behulp van beheer van netwerkverkeer voor failover. Opmerking u kunt implementeren van een klein aantal VMs voor hot stand-by en schaalbaar uitbreiden wanneer dat nodig is.

## <a name="architecture-diagram"></a>Architectuurdiagram

In het volgende diagram is gebaseerd op de architectuur in [een N-lagige architectuur op Azure betrouwbaarheid toevoegen](guidance-compute-n-tier-vm.md)wordt weergegeven.

> Een Visio-document met dit Architectuurdiagram is beschikbaar op het [Microsoft download center][visio-download]. Dit diagram is op de "Compute - pagina voor multi-regio (Windows).

[! [0]][0]

- **Primaire en secundaire gebieden**. Deze architectuur gebruikt twee regio's een hogere mate van beschikbaarheid. Een is de primaire regio. Tijdens normale bewerkingen wordt netwerkverkeer doorgestuurd naar de primaire regio. Maar als dat niet beschikbaar is, verkeer wordt doorgestuurd naar de secundaire regio.

- ** [Azure verkeer Manager] [ traffic-manager] ** stuurt binnenkomende aanvragen naar de primaire regio. Als dat gebied niet beschikbaar is, wordt beheer van netwerkverkeer overgenomen door de secundaire regio. Zie de sectie [Verkeer Manager configureren](#configuring-traffic-manager)voor meer informatie.

- **Resourcegroepen**. Aparte [bronnengroepen] te maken[ resource groups] voor de primaire regio, het gebied van de secundaire en Manager voor het verkeer. Hierdoor kunt u elk gebied beheren als één collectie van bronnen. U kan bijvoorbeeld één gebied implementeren zonder het nemen van een andere. [Koppeling van de resourcegroepen][resource-group-links], zodat u kunt een query om de middelen voor de toepassing uitvoeren.

- **VNets**. Maak een aparte VNet voor elke regio. Zorg ervoor dat de adresruimten elkaar niet overlappen. 

- **SQL Server altijd op groep beschikbaarheid**. Als u SQL Server gebruikt, raden we aan [SQL altijd op Availabilty groepen] [ sql-always-on] voor hoge beschikbaarheid. Maak een van één beschikbaarheidsgroep met de SQL Server-exemplaren in de beide regio's. Zie voor meer informatie de sectie [configureren van de beschikbaarheid van SQL Server altijd op groep](#configuring-the-sql-server-alwayson-availability-group ).

> [AZURE.NOTE] Houd ook rekening met [Azure SQL-Database][azure-sql-db], waar u een relationele database als een cloud-service. Met SQL-Database hoeft u niet een groep beschikbaarheid configureren of beheren van failover.  

- **VPN-Gateways**: maken van een [VPN-gateway] [ vpn-gateway] in elk VNet, en het configureren van een [verbinding met VNet-VNet -][vnet-to-vnet], zodat het netwerkverkeer tussen de twee VNets. Dit is vereist voor de groep altijd SQL op beschikbaarheid.

## <a name="recommendations"></a>Aanbevelingen

Azure biedt veel verschillende bronnen en brontypen, zodat deze referentiearchitectuur kan worden ingericht veel verschillende manieren. We hebben een sjabloon Azure Resource Manager wilt installeren de referentiearchitectuur die volgt op deze aanbevelingen verstrekt. Als u kiest voor het maken van uw eigen referentiearchitectuur Volg deze aanbevelingen, tenzij u een specifieke vereiste is dat een aanbeveling niet past.

### <a name="regional-pairing"></a>Regionale koppelen

Elke regio Azure is gekoppeld aan een andere regio binnen de dezelfde Geografie. Kies in het algemeen gebieden in het regionale paar (bijvoorbeeld Oost-VS 2 en ons centraal). De voordelen hiervan zijn:

- Als er een groot onderhoud, krijgt herstel van ten minste één regio van elk paar voorrang.

- Geplande Azure systeemupdates worden uitgerold naar gepaarde regio's opeenvolgend, om mogelijke uitvaltijd tot een minimum.

- Paren zich bevinden binnen de dezelfde Geografie, om te voldoen aan de hand van vestigingsplaats gegevens.

Echter, ervoor zorgen dat beide regio's alle Azure services die nodig zijn voor de toepassing ondersteunen (Zie [Services per regio][services-by-region]). Zie voor meer informatie over regionale paren [Business continuïteit en disaster recovery (BCDR): Azure gekoppeld regio's][regional-pairs].

### <a name="traffic-manager-configuration"></a>Configuratie van beheer van netwerkverkeer

Houd rekening met de volgende punten bij het configureren van verkeer manager voor uw scenario:

- **Routering.** Beheer van verkeer ondersteunt verschillende [routeringsalgoritmen][tm-routing]. De in dit artikel beschreven scenario voor het bewerkingsplan wordt gebruikt _prioriteit_ (voorheen _failover_ routing). Met deze instelling wordt verzonden verkeer alle aanvragen op het gebied van de primaire tenzij de primaire regio onbereikbaar wordt. Op dat moment overgenomen deze automatisch door de secundaire regio. Zie [methode configureren van failover-][tm-configure-failover].

- **Sonde voor de gezondheid.** Beheer van netwerkverkeer gebruikt een HTTP (of HTTPS) [sonde] [ tm-monitoring] voor het controleren van de beschikbaarheid van elke regio. De sonde wordt gecontroleerd voor een antwoord HTTP 200 voor een opgegeven URL-pad. Beste, een eindpunt dat rapporten van de algehele gezondheid van de toepassing maken en dit eindpunt voor de gezondheid sonde te gebruiken. De sonde kan anders een eindpunt 'gezonde' rapporteren als essentiële onderdelen van de toepassing daadwerkelijk lukt niet. Zie voor meer informatie, [Gezondheid eindpunt Monitoring patroon][health-endpoint-monitoring-pattern].   

Bij het beheer van netwerkverkeer wordt overgenomen, is er een periode wanneer clients de toepassing, dit kan enkele minuten niet bereikbaar. Twee factoren zijn van invloed op de totale duur:

- De sonde gezondheid moet worden gedetecteerd dat het primaire gegevenscentrum niet bereikbaar is geworden.

- DNS-servers moeten de DNS-records in de cache voor het IP-adres, dat afhankelijk van de DNS-time-to-live (TTL is) bijwerken. De standaard-TTL is 300 seconden (5 minuten), maar u kunt deze waarde configureren wanneer u het verkeer Manager profiel maakt.

Zie voor meer informatie [Over het beheer van netwerkverkeer controleren][tm-monitoring]. 

Als het beheer van netwerkverkeer wordt overgenomen, wordt aangeraden een handmatig failback uitvoeren in plaats van automatisch terug bij gebreke. Controleer of alle subsystemen van toepassing zijn in orde eerst. Anders kunt u een situatie waarin de toepassing heen en weer tussen datacenters draait.

Standaard Manager verkeer wordt automatisch failback. Om dit te voorkomen, verlaagt u de prioriteit van de primaire regio handmatig na een failover-gebeurtenis. Stel dat de primaire regio prioriteit 1 is en de secundaire prioriteit 2. Nadat failover is uitgevoerd, zodanig dat de primaire regio prioriteit 3, om te voorkomen dat automatische failback. Wanneer u klaar om terug te schakelen bent, werken de prioriteit 1.

De volgende Azure CLI-opdracht werkt u de prioriteit:

```bat
azure network traffic-manager  endpoint set --resource-group <resource-group> --profile-name <profile>
    --name <traffic-manager-name> --type AzureEndpoints --priority 3
```    

Een andere manier om te voorkomen dat flip-flop is tijdelijk uitschakelen van het eindpunt:

```bat
azure network traffic-manager  endpoint set --resource-group <resource-group> --profile-name <profile>
    --name <traffic-manager-name> --type AzureEndpoints --status Disabled
```

Afhankelijk van de oorzaak van een failover, mogelijk moet u redploy de bronnen binnen een regio. Alvorens terug, moet u een operationele gereedheid-test uitvoeren. De test moet controleren of zaken als:

- VMs zijn correct geconfigureerd. (Alle benodigde software is geïnstalleerd, IIS wordt uitgevoerd, enz.)

- Subsystemen van toepassing zijn in orde. 

- Functionele testen. (Bijvoorbeeld de databaselaag is bereikbaar vanuit de weblaag.)

### <a name="sql-server-always-on-configuration"></a>Configuratie van SQL Server altijd op

Een domeincontroller nodig is in SQL Server altijd op beschikbaarheidsgroepen. Alle knooppunten in de beschikbaarheidsgroep moeten zich in hetzelfde domein AD. De volgende punten geven over het configureren van een groep van de beschikbaarheid van SQL Server altijd op:

- Plaats ten minste twee domeincontrollers in elk gebied. 

- Elke domeincontroller een statisch IP-adres geven.

- Maak een verbinding met VNet te VNet zodat de communicatie tussen de VNets.

- Toevoegen voor elke VNet, de IP-adressen van de domeincontrollers (van beide regio's) aan de lijst met DNS-server. Kunt u de volgende opdracht in de CLI. Meer informatie, Zie [beheren van DNS-servers die worden gebruikt door een virtueel netwerk (VNet)][vnet-dns].

```bat
azure network vnet set --resource-group dc01-rg --name dc01-vnet --dns-servers "10.0.0.4,10.0.0.6,172.16.0.4,172.16.0.6"
```

- Maak een [Windows Server Failover Clustering] [ wsfc] (WSFC)-cluster met de SQL Server-exemplaren in de beide regio's. 

- Een SQL-Server altijd op beschikbaarheidsgroep maken met de SQL Server-exemplaren in de primaire en secundaire gebieden. Zie [Uitbreiden altijd op beschikbaarheid groep externe Azure Datacenter (PowerShell)](https://blogs.msdn.microsoft.com/sqlcat/2014/09/22/extending-alwayson-availability-group-to-remote-azure-datacenter-powershell/) voor de stappen. 

- Plaats de primaire replica in de primaire regio.

- Plaats een of meer secundaire replica's in de primaire regio. Configureer deze synchrone commit met automatische failover gebruikt.

- Plaats een of meer secundaire replica's in de secundaire regio. Configureer deze voor het gebruik van *asynchrone* commit, omwille van de prestaties. (Anders alle SQL-transacties hebben te wachten op een retour via het netwerk met de secundaire regio). 

> [AZURE.NOTE] Asynchronous commit replica's bieden geen ondersteuning voor automatische failover. 

Zie voor meer informatie [Windows VMs uitvoeren voor een N-lagige architectuur op Azure](guidance-compute-n-tier-vm.md#SQL-AlwaysOn-Availability-Group).

## <a name="availability-considerations"></a>Overwegingen met betrekking tot beschikbaarheid

Met een complexe N-tier-app wellicht niet voor het repliceren van de volledige toepassing in de secundaire regio. In plaats daarvan kan alleen repliceert u een essentieel subsysteem dat nodig is ter ondersteuning van de bedrijfscontinuïteit.

Beheer van netwerkverkeer is een mogelijke fout in het systeem. Als de service mislukt, hebben clients geen toegang tot uw toepassing tijdens de uitvaltijd. Bekijk de [SERVICEOVEREENKOMST verkeer Manager][tm-sla], en bepalen of met behulp van beheer van verkeer alleen voldoet aan uw zakelijke vereisten voor hoge beschikbaarheid. Als dit niet het geval is, kunt u een andere oplossing voor het beheer van verkeer toe te voegen als een failback-bewerking. Als de Azure verkeer Manager-service niet, wijzigt u uw CNAME-records in DNS om te verwijzen naar het andere verkeer management-service. (Deze stap handmatig moet worden uitgevoerd en de toepassing niet beschikbaar totdat u de wijzigingen in DNS worden doorgegeven). 

Er zijn twee failover-scenario's te overwegen voor de SQL Server-cluster:

1. De SQL-replica's in de regio voor primaire mislukken. Dit kan bijvoorbeeld gebeuren tijdens een stroomstoring regionale. In dat geval u moet handmatig een failover de groep van de beschikbaarheid van SQL Hoewel verkeer Manager automatisch failover wordt uitgevoerd op de front-end. Volg de stappen in het [uitvoeren van een handmatige gedwongen Failover van een groep beschikbaarheid van SQL Server](https://msdn.microsoft.com/library/ff877957.aspx), waarin wordt beschreven hoe u een geforceerde failover uitvoeren met behulp van SQL Server Management Studio, Transact-SQL of PowerShell in SQL Server 2016. 

    > [AZURE.WARNING] Met geforceerde failover bestaat het risico van verlies van gegevens. Zodra de primaire regio weer on line is, wordt een momentopname van de database en [tablediff] gebruiken om te zoeken naar de verschillen.

2. Beheer van netwerkverkeer wordt overgenomen door de secundaire regio, maar de primaire SQL-replica is nog steeds beschikbaar. Bijvoorbeeld kan de front-laag mislukken, zonder dat de VMs SQL. In dat geval wordt Internet-verkeer wordt doorgestuurd naar de secundaire regio en die regio nog steeds verbinding kan maken met de primaire SQL-replica. Er wordt wel wachttijd, omdat de SQL-verbindingen tussen de regio's. In dit geval moet u als volgt een handmatige failover uitvoeren: 

    - Tijdelijk overschakelen van een SQL-replica in de secundaire regio *synchrone* doorgevoerd. Hierdoor worden er niet gegevens verloren gaan tijdens failover.

    - Failover uitvoeren naar deze SQL-replica. 
    
    - Wanneer u failback naar primaire regio, herstelt u de instelling asynchronous commit. 

## <a name="manageability-considerations"></a>Overwegingen bij het beheer

Tijdens het bijwerken van uw implementatie bijwerken één regio tegelijk te voorkomen dat een algemene risico van een onjuiste configuratie of een fout in de toepassing.

De tolerantie van het systeem op storingen testen. Hier volgen enkele algemene storing scenario's te testen:

- VM exemplaren afgesloten.

- Druk bronnen, zoals CPU en geheugen.

- Netwerk verbreken/vertraging.

- Processen crashen.

- Verlopen certificaten.

- Hardware-en softwarefouten simuleren.

- De DNS-service op de domeincontrollers afgesloten.

De tijden herstel meten en controleren of ze voldoen aan uw zakelijke behoeften. Combinaties van storingen op te sporen, ook testen.

## <a name="next-steps"></a>Volgende stappen

Deze reeks is gericht op pure cloud implementaties. Bedrijfsmatige scenario's vereisen vaak een hybride netwerk, verbinding maken met een netwerk op gebouwen met een virtueel netwerk van Azure. Als u wilt weten hoe u een dergelijke hybride netwerk, zie je [een hybride netwerkarchitectuur met Azure en On-premises VPN-implementatie van][hybrid-vpn].

<!-- Links -->

[azure-sla]: https://azure.microsoft.com/support/legal/sla/
[azure-sql-db]: https://azure.microsoft.com/en-us/documentation/services/sql-database/
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[hybrid-vpn]: guidance-hybrid-network-vpn.md
[regional-pairs]: ../best-practices-availability-paired-regions.md
[resource groups]: ../azure-resource-manager/resource-group-overview.md
[resource-group-links]: ../resource-group-link-resources.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[sql-always-on]: https://msdn.microsoft.com/en-us/library/hh510230.aspx
[tablediff]: https://msdn.microsoft.com/en-us/library/ms162843.aspx
[tm-configure-failover]: ../traffic-manager/traffic-manager-configure-failover-routing-method.md
[tm-monitoring]: ../traffic-manager/traffic-manager-monitoring.md
[tm-routing]: ../traffic-manager/traffic-manager-routing-methods.md
[tm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/traffic-manager/v1_0/
[traffic-manager]: https://azure.microsoft.com/en-us/services/traffic-manager/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vnet-dns]: ../virtual-network/virtual-networks-manage-dns-in-vnet.md
[vnet-to-vnet]: ../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[wsfc]: https://msdn.microsoft.com/en-us/library/hh270278.aspx
[0]: ./media/blueprints/compute-multi-dc.png "Uiterst beschikbaar netwerkarchitectuur voor Azure N-tier toepassingen"