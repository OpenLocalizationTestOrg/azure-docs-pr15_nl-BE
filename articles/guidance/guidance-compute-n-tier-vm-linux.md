<properties
   pageTitle="Linux VMs uitvoeren voor een N-lagige architectuur op Azure | Microsoft Azure"
   description="Het Linux VMs uitvoeren voor een N-lagige architectuur in Microsoft Azure."
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
   ms.date="10/20/2016"
   ms.author="mwasson"/>

# <a name="running-linux-vms-for-an-n-tier-architecture-on-azure"></a>Linux VMs uitvoeren voor een N-lagige architectuur op Azure

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]


> [AZURE.SELECTOR]
- [Linux VMs uitvoeren voor een N-lagige architectuur op Azure](guidance-compute-n-tier-vm-linux.md)
- [Windows VMs uitvoeren voor een N-lagige architectuur op Azure](guidance-compute-n-tier-vm.md)

In dit artikel worden een aantal beproefde procedures voor het uitvoeren van Linux virtuele machines (VMs) voor een toepassing met een N-lagige architectuur. Het is gebaseerd op het [meerdere VMs op Azure]-artikel[multi-vm].

> [AZURE.NOTE] Azure heeft twee verschillende modellen: [Resource Manager] [ resource-manager-overview] en klassiek. In dit artikel wordt een resourcemanager, wordt aangeraden voor nieuwe implementaties.

## <a name="architecture-diagram"></a>Architectuurdiagram

Er zijn variaties van N-lagige architectuur. De verschillen niet moeten voor het grootste deel van belang voor de toepassing van deze aanbevelingen. In dit artikel wordt ervan uitgegaan dat een typische 3-tier web app:

- **De weblaag.** Binnenkomende HTTP-verzoeken verwerkt. Antwoorden worden geretourneerd via deze laag.

- **Business-laag.** Implements, bedrijfsprocessen en andere functionele logica voor het systeem.

- **De databaselaag.** Biedt blijvende gegevensopslag met Apache Cassandra voor hoge beschikbaarheid.

> Een Visio-document met dit Architectuurdiagram is beschikbaar op het [Microsoft download center][visio-download]. Dit diagram is op de "Compute - multi laag (Linux) pagina.

![[0]][0]

- **Beschikbaarheid Sets.** Maak een [Beschikbaar] [ azure-availability-sets] voor elk trapsgewijs en inrichten van ten minste twee VMs in elke laag. Deze aanpak is vereist voor het bereiken van de beschikbaarheid van [SLA] [ vm-sla] van VMs.

- **Subnetten.** Een apart subnet maken voor elke laag. Het adres bereik en subnetmasker opgeven met de [CIDR] -notatie. 

- **Netwerktaakverdelers.** Gebruik een [internetgerichte taakverdeling] [ load-balancer-external] binnenkomende Internet-verkeer op de weblaag en een [interne taakverdeling] verdelen[ load-balancer-internal] voor het distribueren van netwerkverkeer naar de business-laag van de weblaag.

- **Jumpbox**. Een _jumpbox_, ook wel een [bastion-host]is een VM op het netwerk waarmee beheerders verbinding kunnen maken met de andere VMs. De jumpbox heeft een NSG waarmee het externe verkeer alleen van whitelisted openbare IP-adressen. De NSG moet secure shell (SSH) verkeer toestaan.

- **Controle**. Controle software zoals [Nagios], [Zabbix]of [Icinga] krijgt u inzicht in de responstijd, VM uptime en de algehele gezondheid van uw systeem. De monitoring software installeren op een VM, dat in het subnet van een afzonderlijk beheer geplaatst.

- **NSGs**. [Netwerk beveiligingsgroepen] gebruikt[ nsg] (NSGs) voor het beperken van het netwerkverkeer binnen de VNet. Bijvoorbeeld, in de 3-tier architectuur die hier worden weergegeven, accepteert de databaselaag geen verkeer van het web-front-end, alleen in de business-laag en het beheer van subnet.

- **Apache-Cassandra database**. Biedt een hoge beschikbaarheid op de gegevenslaag door replicatie en -failover.

## <a name="recommendations"></a>Aanbevelingen

Azure biedt veel verschillende bronnen en brontypen, zodat deze referentiearchitectuur kan worden ingericht veel verschillende manieren. We hebben een sjabloon Azure Resource Manager wilt installeren de referentiearchitectuur die volgt op deze aanbevelingen verstrekt. Als u kiest voor het maken van uw eigen referentiearchitectuur Volg deze aanbevelingen, tenzij u een specifieke vereiste is dat een aanbeveling niet past.

### <a name="vnet--subnets"></a>VNet / subnetten

Bij het maken van de VNet voor de subnetten u moet, niet voldoende adresruimte toewijzen. De VNet adres bereik en subnetmasker opgeven met de [CIDR] -notatie. Gebruik een adresruimte die binnen de standaard [blokken van particuliere IP-adres valt][private-ip-space], 10.0.0.0/8, 172.16.0.0/12 en 192.168.0.0/16 zijn.

Kies een adresbereik dat met het lokale netwerk overlapt, als u later een gateway tussen de VNet en het lokale netwerk instellen. Als u de VNet hebt gemaakt, kunt u het bereik niet wijzigen.

Ontwerp de subnetten die aan de eisen van functionaliteit en beveiliging in gedachten. Alle VMs binnen dezelfde laag of rol moeten gaan in hetzelfde subnet, die een beveiligingsgrens kan zijn. Zie voor meer informatie over het ontwerpen van VNets en subnetten [Plan en ontwerp Azure virtuele netwerken][plan-network].

Geef voor elk subnet de adresruimte voor het subnet in CIDR-notatie. '10.0.0.0/24' maakt u bijvoorbeeld een bereik van 256 adressen. (VMs 251 deze kunnen gebruiken en vijf zijn gereserveerd. Zie [virtuele netwerken Veelgestelde vragen over][vnet faq].) Controleer of dat de adresbereiken via subnetten niet overlappen.

### <a name="load-balancers"></a>Netwerktaakverdeling

De externe taakverdeling Internet-verkeer op de weblaag. Maak een openbaar IP-adres voor deze taakverdeling. Zie [een taakverdeling internetverbinding maken][lb-external-create].

De interne taakverdeling netwerkverkeer naar de business-laag van de weblaag. Als u deze taakverdeling een particulier IP-adres, een frontend IP-configuratie maken en koppelen aan het subnet voor de business-laag. Zie [aan de slag maken van een interne taakverdeling][lb-internal-create].

### <a name="cassandra"></a>Cassandra

Beste [Onderneming DataStax] [ datastax] voor productie gebruik, maar deze aanbevelingen gelden voor elke editie van Cassandra. Zie voor meer informatie over het uitvoeren van DataStax in Azure, [DataStax Enterprise Deployment Guide voor Azure][cassandra-in-azure]. 

Plaats het VMs voor een cluster Cassandra in een set van beschikbaarheid, om ervoor te zorgen dat de Cassandra replica's zijn verdeeld in meerdere domeinen voor fouttolerantie en domeinen upgraden. Zie [de beschikbaarheid van virtuele machines beheren]voor meer informatie over de fout met betrekking tot domeinen en domeinen upgraden[availability-sets-manage]. 

3 fout met betrekking tot domeinen (maximaal) per set beschikbaarheid configureren. 

18 upgraden domeinen per set beschikbaarheid configureren. Dit geeft u het maximum aantal domeinen upgraden dan kan nog steeds gelijkmatig verdeeld over de fout met betrekking tot domeinen.   

Knooppunten configureren in een rack-hoogte modus. Fout met betrekking tot domeinen worden toegewezen aan de rekken in de `cassandra-rackdc.properties` bestand.

U hoeft niet een load balancer voor het cluster. De client verbinding rechtstreeks met een knooppunt in het cluster.

### <a name="jumpbox"></a>Jumpbox

Plaats de jumpbox in de dezelfde VNet als de andere VMs, maar in een afzonderlijk beheer subnet.

Maak een [openbaar IP-adres] voor de jumpbox.

Een klein VM gebruiken voor het jumpbox, zoals standaard A1.

De NSGs voor de weblaag, zakelijke laag en database laag subnetten voor administratieve (SSH) verkeer om door te geven via het beheer van subnet configureren.

De jumpbox secure, een NSG maken en toepassen op het subnet jumpbox. Een NSG-regel die toestaat dat SSH alleen verbindingen met een set whitelisted openbare IP-adressen toevoegen.

De NSG kan worden gekoppeld aan het subnet of aan de jumpbox NIC. In dit geval het beste kunt koppelen aan de Netwerkkaart, zodat SSH verkeer mag alleen de jumpbox, zelfs als u andere VMs tot hetzelfde subnet toevoegen.

Niet kan SSH toegang vanaf het openbare Internet het VMs die de werkbelasting van toepassingen worden uitgevoerd. In plaats daarvan moet alle SSH toegang tot deze VMs komen via de jumpbox. Een beheerder meldt zich aan bij de jumpbox en meldt zich vervolgens in VM uit de jumpbox. De jumpbox kunt SSH verkeer vanaf het Internet, maar alleen uit de bekende, whitelisted IP-adressen.

## <a name="availability-considerations"></a>Overwegingen met betrekking tot beschikbaarheid

Elke laag of VM rol in een set afzonderlijke beschikbaarheid te plaatsen. Plaats geen VMs uit verschillende lagen in dezelfde set beschikbaarheid. 

Bij de databaselaag omgezet met meerdere VMs niet automatisch in een maximaal beschikbare database. Voor een relationele database moet u normaal gesproken kunt gebruiken voor replicatie en -failover hoge beschikbaarheid.  

Als u hogere beschikbaarheid dan de [Azure SLA voor VMs] moet[ vm-sla] biedt, de toepassing worden gerepliceerd tussen de twee regio's en Azure verkeer Manager gebruiken voor failover. Voor meer informatie, Zie [Linux VMs actief in meerdere regio's voor hoge beschikbaarheid][multi-dc].  

## <a name="security-considerations"></a>Beveiligingsoverwegingen

NSG-regels gebruiken om te beperken het verkeer tussen de lagen. Bijvoorbeeld in de bovenstaande 3-tier architectuur communiceert de weblaag niet rechtstreeks met de databaselaag. Als u dit, moet de databaselaag binnenkomend verkeer van het web tier subnet blokkeren.  

  1. Een NSG maken en deze te koppelen aan de database laag subnet.

  2. Een regel toevoegen waarmee wordt al het binnenkomende verkeer van de VNet geweigerd. (Gebruik de `VIRTUAL_NETWORK` -tag in de regel.) 

  3. Een regel met een hogere prioriteit, waarmee het binnenkomende gegevensverkeer van de business tier subnet toevoegen. Deze regel heeft voorrang op de vorige regel en kunt de niveaus business contact opnemen met de databaselaag.

  4. Een regel waarmee het binnenkomende gegevensverkeer van binnen het subnet van database laag zelf toevoegen. Deze regel maakt communicatie mogelijk tussen VMs in de databaselaag, die nodig is voor replicatie van de licentiedatabase en overname bij storingen.

  5. Een regel waarmee SSH verkeer van het subnet jumpbox kunt toevoegen. Deze regel kunt verbinding maken met de databaselaag van de jumpbox beheerders.

  > [AZURE.NOTE] Een NSG heeft [standaardregels] [ nsg-rules] waardoor elk binnenkomend verkeer van binnen de VNet. Deze regels kunnen niet worden verwijderd, maar u kunt deze overschrijven door hogere prioriteit regels maken.

Overweeg een virtueel netwerkapparaat (NVA) een DMZ tussen Internet en de Azure virtueel netwerk maken. NVA is een algemene term voor een virtuele toestel die netwerk-gerelateerde taken, zoals de firewall, packet inspectie, controle, aangepaste routering of allerlei andere bewerkingen kunt uitvoeren. Zie voor meer informatie, [ter uitvoering van een DMZ tussen Azure en Internet][dmz].

## <a name="scalability-considerations"></a>Overwegingen voor schaalbaarheid

De netwerktaakverdeling verdelen netwerkverkeer naar de web- en lagen. Horizontaal schalen door het toevoegen van nieuwe VM exemplaren. Houd er rekening mee dat u de web- en toepassingslagen onafhankelijk schalen kunt, op basis van de belasting. Om mogelijke complicaties veroorzaakt door de noodzaak om de clientaffiniteit te behouden, moet het VMs in de weblaag stateless. Ook moet de hosting van de bedrijfslogica VMs stateless.

## <a name="manageability-considerations"></a>Overwegingen bij het beheer

Vereenvoudig het beheer van het hele systeem met behulp van gecentraliseerde beheerprogramma's zoals [Azure automatisering][azure-administration], [Microsoft Operations Management Suite][operations-management-suite], [chef-kok][chef], of [Puppet][puppet]. Deze hulpprogramma's kunnen samenvoegen van diagnose- en informatie uit meerdere VMs voor een algemeen overzicht van het systeem vastgelegd.

## <a name="solution-deployment"></a>Implementatie van oplossingen

Een implementatie voor een referentiearchitectuur die deze aanbevelingen implementeert, is beschikbaar op [Github][github-folder]. Deze referentiearchitectuur bevat een weblaag, zakelijke laag en een gegevenslaag.

1. Klik op de knop hieronder.  
[! ["Implementeren op Azure'] [1]][2]

2. Nadat de koppeling is geopend in de portal Azure, voert u de volgende waarden: 
  - De naam van de **Resource** is al gedefinieerd in de parameterbestand, dus **Nieuw** te selecteren en voer `ra-ntier-sql-network-rg` in het tekstvak.
  - Selecteer de regio in de vervolgkeuzelijst **locatie** .
  - Bewerk de **Sjabloon basis-Uri** of de **Parameter basis-Uri** tekstvakken niet.
  - Bekijk de voorwaarden en bepalingen en klik op het selectievakje **dat ik ga akkoord met bovenstaande voorwaarden** .
  - Klik op de button **aankoop** .

3. Controleer de Azure portal kennisgeving voor een bericht dat de installatie is voltooid.

4. De parameterbestanden bevatten een hardcoded beheerder gebruikersnamen en wachtwoorden wordt aanbevolen dat u onmiddellijk wijzigen op alle VMs. Op elke VM in Azure Portal en vervolgens op **wachtwoord opnieuw instellen** in de bladeserver **ondersteuning + het oplossen van problemen** . Selecteer **wachtwoord opnieuw instellen** in het dropdown **-modus** en selecteer vervolgens een nieuwe **gebruikersnaam** en **wachtwoord**. Klik op de knop **bijwerken** om de nieuwe gebruikersnaam en wachtwoord te behouden.

## <a name="next-steps"></a>Volgende stappen

Om te bereiken deze referentiearchitectuur voor maximale beschikbaarheid, het beste [implementeert voor meerdere regio's][multi-dc].

<!-- links -->

[azure-administration]: ../automation/automation-intro.md
[azure-availability-sets]: ../virtual-machines/virtual-machines-windows-manage-availability.md#configure-each-application-tier-into-separate-availability-sets
[availability-sets-manage]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[bastion-host]: https://en.wikipedia.org/wiki/Bastion_host
[cassandra-consistency]: http://docs.datastax.com/en/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html
[cassandra-consistency-usage]: http://medium.com/@foundev/cassandra-how-many-nodes-are-talked-to-with-quorum-also-should-i-use-it-98074e75d7d5#.b4pb4alb2
[cassandra-in-azure]: https://docs.datastax.com/en/datastax_enterprise/4.5/datastax_enterprise/install/installAzure.html
[cassandra-replication]: http://www.planetcassandra.org/data-replication-in-nosql-databases-explained/
[CIDR]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
[chef]: https://www.chef.io/solutions/azure/
[datastax]: http://www.datastax.com/products/datastax-enterprise
[dmz]: guidance-iaas-ra-secure-vnet-dmz.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier
[lb-external-create]: ../load-balancer/load-balancer-get-started-internet-portal.md
[lb-internal-create]: ../load-balancer/load-balancer-get-started-ilb-arm-portal.md
[load-balancer-external]: ../load-balancer/load-balancer-internet-overview.md
[load-balancer-internal]: ../load-balancer/load-balancer-internal-overview.md
[multi-dc]: guidance-compute-multiple-datacenters-linux.md
[multi-vm]: guidance-compute-multi-vm.md
[naming conventions]: guidance-naming-conventions.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[nsg-rules]: ../best-practices-resource-manager-security.md#network-security-groups
[operations-management-suite]: https://www.microsoft.com/en-us/server-cloud/operations-management-suite/overview.aspx
[plan-network]: ../virtual-network/virtual-network-vnet-plan-design-arm.md
[private-ip-space]: https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces
[openbare IP-adres]: ../virtual-network/virtual-network-ip-addresses-overview-arm.md
[puppet]: https://puppetlabs.com/blog/managing-azure-virtual-machines-puppet
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines
[vnet faq]: ../virtual-network/virtual-networks-faq.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[Nagios]: https://www.nagios.org/
[Zabbix]: http://www.zabbix.com/
[Icinga]: http://www.icinga.org/
[0]: ./media/blueprints/compute-n-tier-linux.png "N-lagige architectuur met Microsoft Azure"
[1]: ./media/blueprints/deploybutton.png 
[2]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier%2Fazuredeploy.json


