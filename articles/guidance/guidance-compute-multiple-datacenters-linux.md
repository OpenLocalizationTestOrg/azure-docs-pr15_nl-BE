<properties
   pageTitle="Met Linux VMs in meerdere regio's voor hoge beschikbaarheid | Verwijzen naar architectuur | Microsoft Azure"
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

# <a name="running-linux-vms-in-multiple-regions-for-high-availability"></a>Linux VMs actief in meerdere regio's voor hoge beschikbaarheid

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

> [AZURE.SELECTOR]
- [Linux VMs actief in meerdere regio's voor hoge beschikbaarheid](guidance-compute-multiple-datacenters-linux.md)
- [Windows VMs actief in meerdere regio's voor hoge beschikbaarheid](guidance-compute-multiple-datacenters.md)

In dit artikel, wordt aangeraden een reeks procedures Linux virtuele machines (VMs) uitvoeren in meerdere landen Azure, beschikbaarheid en een robuuste disaster recovery-infrastructuur.

> [AZURE.NOTE] Azure heeft twee verschillende modellen: [Resource Manager] [ resource groups] en klassiek. In dit artikel wordt een resourcemanager, wordt aangeraden voor nieuwe implementaties.

Een architectuur met meerdere regio kan bieden hogere beschikbaarheid dan het implementeren van een bepaalde regio. Als een regionale storing van invloed is op de primaire regio, kunt u [Verkeer Manager] [ traffic-manager] voor failover uitvoeren naar de secundaire regio. Deze architectuur kan ook helpen als een afzonderlijke subsysteem van de toepassing mislukt.

Er zijn diverse algemene benaderingen voor het bereiken van hoge beschikbaarheid in datacenters:   
  
- Actief/passief met hot stand-by. Verkeer wordt één regio, terwijl de andere wacht op stand-by. VMs in de secundaire regio zijn toegewezen en wordt uitgevoerd op alle tijden.

- Actief/passief met koude stand-by. De zelfde, maar VMs in de secundaire regio worden niet toegewezen totdat die nodig zijn voor failover. Deze aanpak kost minder worden uitgevoerd, maar wordt over het algemeen meer storingstijd tijdens een storing.

- Actief/actief. Beide regio's actief zijn en gelijkmatig verdeeld tussen hen. Als één gegevenscentrum niet beschikbaar is, is uit de queryrotatie gehaald.

Deze architectuur is gericht op actieve/passieve met hot stand-by, met behulp van beheer van netwerkverkeer voor failover. Opmerking u kunt implementeren van een klein aantal VMs voor hot stand-by en schaalbaar uitbreiden wanneer dat nodig is.

## <a name="architecture-diagram"></a>Architectuurdiagram

In het volgende diagram is gebaseerd op de architectuur in [een N-lagige architectuur op Azure betrouwbaarheid toevoegen](guidance-compute-n-tier-vm-linux.md)wordt weergegeven. 

> Een Visio-document met dit Architectuurdiagram is beschikbaar op het [Microsoft download center][visio-download]. Dit diagram is op de "Compute - pagina voor multi-regio (Linux).

![[0]][0]

- **Primaire en secundaire gebieden**. Deze architectuur gebruikt twee regio's een hogere mate van beschikbaarheid. Een is de primaire regio. Tijdens normale bewerkingen wordt netwerkverkeer doorgestuurd naar de primaire regio. Maar als dat niet beschikbaar is, verkeer wordt doorgestuurd naar de secundaire regio.

- ** [Azure verkeer Manager] [ traffic-manager] ** stuurt binnenkomende aanvragen naar de primaire regio. Als dat gebied niet beschikbaar is, wordt beheer van netwerkverkeer overgenomen door de secundaire regio. Zie de sectie [Verkeer Manager configureren](#configuring-traffic-manager)voor meer informatie.

- **Resourcegroepen**. Aparte [bronnengroepen] te maken[ resource groups] voor de primaire regio, het gebied van de secundaire en Manager voor het verkeer. Hierdoor kunt u elk gebied beheren als één collectie van bronnen. U kan bijvoorbeeld één gebied implementeren zonder het nemen van een andere. [Koppeling van de resourcegroepen][resource-group-links], zodat u kunt een query om de middelen voor de toepassing uitvoeren.

- **VNets**. Maak een aparte VNet voor elke regio. Zorg ervoor dat de adresruimten elkaar niet overlappen.

- **Apache-Cassandra** geïmplementeerd in data centers Azure regio's. Cassandra datacenters worden geïmplementeerd in verschillende gebieden van Azure voor hoge beschikbaarheid. Binnen elke regio, knooppunten zijn geconfigureerd in rack-hoogte modus met fouttolerantie en upgraden van domeinen voor tolerantie in de regio.

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

### <a name="cassandra-deployment-across-multiple-regions"></a>Cassandra-distributie over meerdere regio 's

Cassandra datacenters zijn afdelingen van de werkbelasting: een groep verwante knooppunten die samen worden geconfigureerd in een cluster voor replicatie en werkbelasting scheiding.

Beste [Onderneming DataStax] [ datastax] voor de productie gebruiken. Zie voor meer informatie over het uitvoeren van DataStax in Azure, [DataStax Enterprise Deployment Guide voor Azure][cassandra-in-azure]. De volgende algemene aanbevelingen zijn van toepassing op elke editie van Cassandra.

- Een openbaar IP-adres toewijzen aan elk knooppunt. Hierdoor worden de clusters voor de communicatie tussen de regio's met de Azure backbone-infrastructuur bieden hoge doorvoer tegen een lage prijs.

- Beveiligen met de juiste firewall en NSG-configuraties, zodat verkeer alleen van bekende hosts, met inbegrip van clients en andere clusterknooppunten knooppunten. Houd er rekening mee dat Cassandra wordt gebruikt voor verschillende poorten voor communicatie, OpsCenter, motoren, enzovoort. Zie [configureren firewall poort toegang]voor Poortgebruik in Cassandra,[cassandra-ports].

- SSL-codering gebruiken voor alle [client-naar-node] [ ssl-client-node] en [knooppunt naar] [ ssl-node-node] communicatie.

- Volg de richtlijnen in [aanbevelingen van Cassandra](guidance-compute-n-tier-vm-linux.md#cassandra-recommendations)binnen een regio.

## <a name="availability-considerations"></a>Overwegingen met betrekking tot beschikbaarheid

Met een complexe N-tier-app wellicht niet voor het repliceren van de volledige toepassing in de secundaire regio. In plaats daarvan kan alleen repliceert u een essentieel subsysteem dat nodig is ter ondersteuning van de bedrijfscontinuïteit.

Beheer van netwerkverkeer is een mogelijke fout in het systeem. Als de service mislukt, hebben clients geen toegang tot uw toepassing tijdens de uitvaltijd. Bekijk de [SERVICEOVEREENKOMST verkeer Manager][tm-sla], en bepalen of met behulp van beheer van verkeer alleen voldoet aan uw zakelijke vereisten voor hoge beschikbaarheid. Als dit niet het geval is, kunt u een andere oplossing voor het beheer van verkeer toe te voegen als een failback-bewerking. Als de Azure verkeer Manager-service niet, wijzigt u uw CNAME-records in DNS om te verwijzen naar het andere verkeer management-service. (Deze stap handmatig moet worden uitgevoerd en de toepassing niet beschikbaar totdat u de wijzigingen in DNS worden doorgegeven).

Voor het cluster Cassandra afhankelijk de failover-scenario's rekening te houden met de consistentie niveaus gebruikt door de toepassing, alsmede het aantal replica's gebruikt. Zie de [consistentie van de gegevens configureren] voor niveaus samenhang en gebruik in Cassandra,[ cassandra-consistency] en [Cassandra: hoeveel knooppunten er zijn een onderhoud gehad met met Quorum?][cassandra-consistency-usage] Beschikbaarheid van gegevens in Cassandra wordt bepaald door het niveau van de consistentie wordt gebruikt door de toepassing en het replicatiemechanisme. Zie [Replicatie van de gegevens in NoSQL Databases uitgelegd]voor replicatie in Cassandra,[cassandra-replication].

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
[cassandra-in-azure]: https://academy.datastax.com/resources/deployment-guide-azure
[cassandra-consistency]: http://docs.datastax.com/en/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html
[cassandra-replication]: http://www.planetcassandra.org/data-replication-in-nosql-databases-explained/
[cassandra-consistency-usage]: https://medium.com/@foundev/cassandra-how-many-nodes-are-talked-to-with-quorum-also-should-i-use-it-98074e75d7d5#.b4pb4alb2
[cassandra-ports]: http://docs.datastax.com/en/latest-dse/datastax_enterprise/sec/secConfFirePort.html
[datastax]: https://www.datastax.com/products/datastax-enterprise
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[hybrid-vpn]: guidance-hybrid-network-vpn.md
[regional-pairs]: ../best-practices-availability-paired-regions.md
[resource groups]: ../azure-resource-manager/resource-group-overview.md
[resource-group-links]: ../resource-group-link-resources.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[ssl-client-node]: http://docs.datastax.com/en/cassandra/2.0/cassandra/security/secureSSLClientToNode_t.html
[ssl-node-node]: http://docs.datastax.com/en/cassandra/2.0/cassandra/security/secureSSLNodeToNode_t.html
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
[0]: ./media/blueprints/compute-multi-dc-linux.png "Uiterst beschikbaar netwerkarchitectuur voor Azure N-tier toepassingen"
