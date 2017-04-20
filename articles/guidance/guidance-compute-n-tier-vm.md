<properties
   pageTitle="Windows VMs uitvoeren voor een N-lagige architectuur | Verwijzen naar architectuur | Microsoft Azure"
   description="Het implementeren van een architectuur met meerdere lagen op Azure, betalende bijzondere aandacht aan de beschikbaarheid, beveiliging, schaalbaarheid en beheerbaarheid beveiliging."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
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

# <a name="running-windows-vms-for-an-n-tier-architecture-on-azure"></a>Windows VMs uitvoeren voor een N-lagige architectuur op Azure

> [AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

> [AZURE.SELECTOR]
- [Linux VMs uitvoeren voor een N-lagige architectuur op Azure](guidance-compute-n-tier-vm-linux.md)
- [Windows VMs uitvoeren voor een N-lagige architectuur op Azure](guidance-compute-n-tier-vm.md)

In dit artikel worden een aantal beproefde procedures voor het uitvoeren van Windows virtuele machines (VMs) voor een toepassing met een N-lagige architectuur. Het is gebaseerd op het [meerdere VMs op Azure]-artikel[multi-vm].

> [AZURE.NOTE] Azure heeft twee verschillende modellen: [Resource Manager] [ resource-manager-overview] en klassiek. In dit artikel wordt een resourcemanager, wordt aangeraden voor nieuwe implementaties.

## <a name="architecture-diagram"></a>Architectuurdiagram

Er zijn variaties van N-lagige architectuur. De verschillen niet moeten voor het grootste deel van belang voor de toepassing van deze aanbevelingen. In dit artikel wordt ervan uitgegaan dat een typische 3-tier web app:

- **De weblaag.** Binnenkomende HTTP-verzoeken verwerkt. Antwoorden worden geretourneerd via deze laag.

- **Business-laag.** Implements, bedrijfsprocessen en andere functionele logica voor het systeem.

- **De databaselaag.** Biedt blijvende gegevensopslag met [SQL Server altijd op beschikbaarheidsgroepen] [ sql-alwayson] voor hoge beschikbaarheid.

> Een Visio-document met dit Architectuurdiagram is beschikbaar op het [Microsoft download center][visio-download]. Dit diagram is op de "Compute - meerdere pagina's (Windows) laag.

![[0]][0]

- **Beschikbaarheid Sets.** Maak een [Beschikbaar] [ azure-availability-sets] voor elk trapsgewijs en inrichten van ten minste twee VMs in elke laag. Deze aanpak is vereist voor het bereiken van de beschikbaarheid van [SLA] [ vm-sla] van VMs.

- **Subnetten.** Een apart subnet maken voor elke laag. Het adres bereik en subnetmasker opgeven met de [CIDR] -notatie. 

- **Netwerktaakverdelers.** Gebruik een [internetgerichte taakverdeling] [ load-balancer-external] binnenkomende Internet-verkeer op de weblaag en een [interne taakverdeling] verdelen[ load-balancer-internal] voor het distribueren van netwerkverkeer naar de business-laag van de weblaag.

- **Jumpbox**. Een _jumpbox_, ook wel een [bastion-host]is een VM op het netwerk waarmee beheerders verbinding kunnen maken met de andere VMs. De jumpbox heeft een NSG waarmee het externe verkeer alleen van whitelisted openbare IP-adressen. De NSG moet remote desktop (RDP)-verkeer toestaan.

- **Controle**. Controle software zoals [Nagios], [Zabbix]of [Icinga] krijgt u inzicht in de responstijd, VM uptime en de algehele gezondheid van uw systeem. De monitoring software installeren op een VM, dat in het subnet van een afzonderlijk beheer geplaatst.

- **NSGs**. [Netwerk beveiligingsgroepen] gebruikt[ nsg] (NSGs) voor het beperken van het netwerkverkeer binnen de VNet. Bijvoorbeeld, in de 3-tier architectuur die hier worden weergegeven, accepteert de databaselaag geen verkeer van het web-front-end, alleen in de business-laag en het beheer van subnet.

- **SQL Server altijd op groep beschikbaarheid.** Biedt een hoge beschikbaarheid op de gegevenslaag door replicatie en -failover.

- **Active Directory Domain Services (AD DS)-Servers**. Active Directory Domain Services (AD DS) worden directorygegevens opgeslagen en beheert de communicatie tussen gebruikers en domeinen, inclusief aanmeldingsprocessen van gebruikers, verificatie en zoekopdrachten in de directory. Een Active Directory-domeincontroller is een server die AD DS uitvoert. Vóór Windows Server 2016, altijd op beschikbaarheidsgroepen moeten deel uitmaken van een domein. Dit komt omdat de beschikbaarheidsgroepen afhankelijk zijn van Windows Server Failover-Cluster (WSFC)-technologie. Windows Server 2016 introduceert de mogelijkheid voor het maken van een failover-Cluster zonder Active Directory. Voor meer informatie, Zie [Wat is er nieuw in failover-clusters in Windows Server 2016][wsfc-whats-new]

## <a name="recommendations"></a>Aanbevelingen

Azure biedt veel verschillende bronnen en brontypen, zodat deze referentiearchitectuur kan worden ingericht veel verschillende manieren. We hebben een sjabloon Azure Resource Manager wilt installeren de referentiearchitectuur die volgt op deze aanbevelingen verstrekt. Als u kiest voor het maken van uw eigen referentiearchitectuur Volg deze aanbevelingen, tenzij u een specifieke vereiste is dat een aanbeveling niet past.

### <a name="vnet--subnets"></a>VNet / subnetten

Bij het maken van de VNet voor de subnetten u moet, niet voldoende adresruimte toewijzen. De VNet adres bereik en subnetmasker opgeven met de [CIDR] -notatie. Gebruik een adresruimte die binnen de standaard [blokken van particuliere IP-adres valt][private-ip-space], 10.0.0.0/8, 172.16.0.0/12 en 192.168.0.0/16 zijn.

Kies een adresbereik dat met het lokale netwerk overlapt, als u later een gateway tussen de VNet en het lokale netwerk instellen. Als u de VNet hebt gemaakt, kunt u het bereik niet wijzigen.

Ontwerp de subnetten die aan de eisen van functionaliteit en beveiliging in gedachten. Alle VMs binnen dezelfde laag of rol moeten gaan in hetzelfde subnet, die een beveiligingsgrens kan zijn. Zie voor meer informatie over het ontwerpen van VNets en subnetten [Plan en ontwerp Azure virtuele netwerken][plan-network].

Geef voor elk subnet de adresruimte voor het subnet in CIDR-notatie. '10.0.0.0/24' maakt u bijvoorbeeld een bereik van 256 adressen. (VMs 251 deze kunnen gebruiken en vijf zijn gereserveerd. Zie [virtuele netwerken Veelgestelde vragen over][vnet faq].) Controleer of dat de adresbereiken via subnetten niet overlappen.

### <a name="network-security-groups"></a>Netwerk-beveiligingsgroepen

NSG-regels gebruiken om te beperken het verkeer tussen de lagen. Bijvoorbeeld in de bovenstaande 3-tier architectuur communiceert de weblaag niet rechtstreeks met de databaselaag. Als u dit, moet de databaselaag binnenkomend verkeer van het web tier subnet blokkeren.  

  1. Een NSG maken en deze te koppelen aan de database laag subnet.

  2. Een regel toevoegen waarmee wordt al het binnenkomende verkeer van de VNet geweigerd. (Gebruik de `VIRTUAL_NETWORK` -tag in de regel.) 

  3. Een regel met een hogere prioriteit, waarmee het binnenkomende gegevensverkeer van de business tier subnet toevoegen. Deze regel heeft voorrang op de vorige regel en kunt de niveaus business contact opnemen met de databaselaag.

  4. Een regel waarmee het binnenkomende gegevensverkeer van binnen het subnet van database laag zelf toevoegen. Deze regel maakt communicatie mogelijk tussen VMs in de databaselaag, die nodig is voor replicatie van de licentiedatabase en overname bij storingen.

  5. Een regel waarmee RDP-verkeer van het subnet jumpbox kunt toevoegen. Deze regel kunt verbinding maken met de databaselaag van de jumpbox beheerders.

  > [AZURE.NOTE] Een NSG heeft [standaardregels] [ nsg-rules] waardoor elk binnenkomend verkeer van binnen de VNet. Deze regels kunnen niet worden verwijderd, maar u kunt deze overschrijven door hogere prioriteit regels maken.

### <a name="load-balancers"></a>Netwerktaakverdeling

De externe taakverdeling Internet-verkeer op de weblaag. Maak een openbaar IP-adres voor deze taakverdeling. Zie [een taakverdeling internetverbinding maken][lb-external-create].

De interne taakverdeling netwerkverkeer naar de business-laag van de weblaag. Als u deze taakverdeling een particulier IP-adres, een frontend IP-configuratie maken en koppelen aan het subnet voor de business-laag. Zie [aan de slag maken van een interne taakverdeling][lb-internal-create].

### <a name="sql-server-always-on-availability-groups"></a>SQL Server altijd op beschikbaarheidsgroepen

Wij raden aan [Altijd op beschikbaarheidsgroepen] [ sql-alwayson] voor maximale beschikbaarheid van SQL Server. Altijd op beschikbaarheidsgroepen een domeincontroller nodig is. Alle knooppunten in de groep beschikbaarheid moeten zich in hetzelfde domein AD.

Andere lagen verbinding maken met de-database via een [listener voor beschikbaarheid groep][sql-alwayson-listeners]. De listener kan een SQL-client verbinding maken met de naam van de fysieke installatie van SQL Server niet kent. VMs dat toegang tot de database moet worden toegevoegd aan het domein. De client (in dit geval een andere laag) Gebruik virtuele-netwerknaam van de listener omzetten in IP-adressen van DNS.

SQL Server altijd op als volgt configureren:

- Maak een cluster met Windows Server Failover Clustering (WSFC) en een groep van de beschikbaarheid van SQL Server altijd op. Zie voor meer informatie, [Aan de slag met altijd op beschikbaarheidsgroepen][sql-alwayson-getting-started].

- Maak een interne taakverdeling met een statische particulier IP-adres.

- Maak een listener beschikbaarheid groep en de listener van de DNS-naam toewijzen aan het IP-adres van een interne taakverdeling. 

- Maak een regel load balancer voor de luisterende poort van SQL Server (standaard TCP-poort 1433). De regel load balancer dient _zwevende IP_, ook wel directe Server terug te schakelen. Dit zorgt ervoor dat de VM te beantwoorden rechtstreeks naar de client, waardoor een directe verbinding met de primaire replica.

    > [AZURE.NOTE] Als zwevende IP is ingeschakeld, moet de front-poortnummer hetzelfde zijn als de back-end-poortnummer in de regel load balancer.

Wanneer een SQL-client probeert verbinding te maken, stuurt de taakverdeling de verbindingsaanvraag aan de replica die de huidige primaire. Als er een failover naar een andere replica, de taakverdeling volgende aanvragen automatisch omgeleid naar de nieuwe primaire replica. Voor meer informatie Zie [configureren de belasting voor documentconversies voor SQL altijd op][sql-alwayson-ilb].

Tijdens een failover, worden bestaande clientverbindingen gesloten. Nadat de failover is voltooid, worden nieuwe verbindingen doorgestuurd naar de nieuwe primaire replica.

Als uw app aanzienlijk meer leest, dan schrijft, kunt u het kenmerk alleen-lezen zoekopdrachten naar een secundaire replica-offload. Zie [met behulp van een Listener voor verbinding met een secundair alleen-lezen (alleen-lezen routering) Replica][sql-alwayson-read-only-routing].

De implementatie testen door het [forceren van een handmatige failover][sql-alwayson-force-failover].

### <a name="jumpbox"></a>Jumpbox

Toegang niet is toegestaan RDP via het openbare Internet naar het VMs die de werkbelasting van toepassingen worden uitgevoerd. In plaats daarvan moet alle RDP/SSH toegang tot deze VMs komen via de jumpbox. Een beheerder meldt zich aan bij de jumpbox en meldt zich vervolgens in VM uit de jumpbox. De jumpbox kunt RDP-verkeer vanaf het Internet, maar alleen uit de bekende, whitelisted IP-adressen.

Plaats de jumpbox in de dezelfde VNet als de andere VMs, maar in een afzonderlijk beheer subnet.

Maak een [openbaar IP-adres] voor de jumpbox.

Een klein VM gebruiken voor het jumpbox, zoals standaard A1.

De NSGs voor de weblaag, zakelijke laag en database laag subnetten voor administratieve (RDP) verkeer om door te geven via het beheer van subnet configureren.

De jumpbox secure, een NSG maken en toepassen op het subnet jumpbox. Een regel NSG waarmee RDP-verbindingen alleen uit een set whitelisted van openbare IP-adressen toevoegen.

De NSG kan worden gekoppeld aan het subnet of aan de jumpbox NIC. In dit geval het beste koppelen aan de Netwerkkaart, zodat het RDP-verkeer mag alleen de jumpbox, zelfs als u andere VMs tot hetzelfde subnet toevoegen.

## <a name="availability-considerations"></a>Overwegingen met betrekking tot beschikbaarheid

Elke laag of VM rol in een set afzonderlijke beschikbaarheid te plaatsen. Plaats geen VMs uit verschillende lagen in dezelfde set beschikbaarheid. 

Bij de databaselaag omgezet met meerdere VMs niet automatisch in een maximaal beschikbare database. Voor een relationele database moet u normaal gesproken kunt gebruiken voor replicatie en -failover hoge beschikbaarheid. Voor SQL Server, het is raadzaam [altijd op beschikbaarheid]groepen[sql-alwayson]. 

Als u hogere beschikbaarheid dan de [Azure SLA voor VMs] moet[ vm-sla] biedt, de toepassing worden gerepliceerd tussen de twee regio's en Azure verkeer Manager gebruiken voor failover. Zie [Windows VMs actief in meerdere regio's voor maximale beschikbaarheid]voor meer informatie,[multi-dc].   

## <a name="security-considerations"></a>Beveiligingsoverwegingen

Het coderen van gegevens in rust. [Azure sleutel kluis] gebruiken[ azure-key-vault] voor het beheren van de database coderingssleutels. Sleutel kluis kunt coderingssleutels opslaan in hardware security modules (HSM's). Zie voor meer informatie, [Azure sleutel kluis integratie configureren voor SQL Server op Azure VMs] [ sql-keyvault] het is ook raadzaam geheimen van toepassing, zoals tekenreeksen voor een databaseverbinding, opslaan in een kluis met sleutel.

Overweeg een virtueel netwerkapparaat (NVA) een DMZ tussen Internet en de Azure virtueel netwerk maken. NVA is een algemene term voor een virtuele toestel die netwerk-gerelateerde taken, zoals de firewall, packet inspectie, controle, aangepaste routering of allerlei andere bewerkingen kunt uitvoeren. Zie voor meer informatie, [ter uitvoering van een DMZ tussen Azure en Internet][dmz].

## <a name="scalability-considerations"></a>Overwegingen voor schaalbaarheid

De netwerktaakverdeling verdelen netwerkverkeer naar de web- en lagen. Horizontaal schalen door het toevoegen van nieuwe VM exemplaren. Houd er rekening mee dat u de web- en toepassingslagen onafhankelijk schalen kunt, op basis van de belasting. Om mogelijke complicaties veroorzaakt door de noodzaak om de clientaffiniteit te behouden, moet het VMs in de weblaag stateless. Ook moet de hosting van de bedrijfslogica VMs stateless.

## <a name="manageability-considerations"></a>Overwegingen bij het beheer

Vereenvoudig het beheer van het hele systeem met behulp van gecentraliseerde beheerprogramma's zoals [Azure automatisering][azure-administration], [Microsoft Operations Management Suite][operations-management-suite], [chef-kok][chef], of [Puppet][puppet]. Deze hulpprogramma's kunnen samenvoegen van diagnose- en informatie uit meerdere VMs voor een algemeen overzicht van het systeem vastgelegd.

## <a name="solution-deployment"></a>Implementatie van oplossingen

Een implementatie voor een referentiearchitectuur die deze aanbevelingen implementeert, is beschikbaar op [Github][github-folder]. Deze referentiearchitectuur bevat een web tier, tier business, een gegevenslaag, alsmede een jumpbox VM en Active Directory-domeincontrollers.

De referentiearchitectuur kan worden geïmplementeerd door de onderstaande aanwijzingen: 

1. Klik op de knop hieronder.  
[! ["Implementeren op Azure'] [1]][2]

2. Nadat de koppeling is geopend in de portal Azure, voert u de volgende waarden: 
  - De naam van de **Resource** is al gedefinieerd in de parameterbestand, dus **Nieuw** te selecteren en voer `ra-ntier-sql-network-rg` in het tekstvak.
  - Selecteer de regio in de vervolgkeuzelijst **locatie** .
  - Bewerk de **Sjabloon basis-Uri** of de **Parameter basis-Uri** tekstvakken niet.
  - Bekijk de voorwaarden en bepalingen en klik op het selectievakje **dat ik ga akkoord met bovenstaande voorwaarden** .
  - Klik op de button **aankoop** .

3. Controleer de Azure portal kennisgeving voor een bericht dat de installatie is voltooid.

4. Klik op de knop hieronder.  
[! ["Implementeren op Azure'] [1]][3]

5. Nadat de koppeling is geopend in de portal Azure, voert u de volgende waarden: 
  - De naam van de **Resource** is al gedefinieerd in de parameterbestand, dus selecteer **Bestaande gebruiken** en voer `ra-ntier-sql-workload-rg` in het tekstvak.
  - Selecteer de regio in de vervolgkeuzelijst **locatie** .
  - Bewerk de **Sjabloon basis-Uri** of de **Parameter basis-Uri** tekstvakken niet.
  - Bekijk de voorwaarden en bepalingen en klik op het selectievakje **dat ik ga akkoord met bovenstaande voorwaarden** .
  - Klik op de button **aankoop** .

6. Controleer de Azure portal kennisgeving voor een bericht dat de installatie is voltooid.

7. Klik op de knop hieronder.  
[! ["Implementeren op Azure'] [1]][4]

8. Nadat de koppeling is geopend in de portal Azure, voert u de volgende waarden: 
  - De naam van de **Resource** is al gedefinieerd in de parameterbestand, dus **Nieuw** te selecteren en voer `ra-ntier-sql-network-rg` in het tekstvak.
  - Selecteer de regio in de vervolgkeuzelijst **locatie** .
  - Bewerk de **Sjabloon basis-Uri** of de **Parameter basis-Uri** tekstvakken niet.
  - Bekijk de voorwaarden en bepalingen en klik op het selectievakje **dat ik ga akkoord met bovenstaande voorwaarden** .
  - Klik op de button **aankoop** .

9. Controleer de Azure portal kennisgeving voor een bericht dat de installatie is voltooid.

10. De parameterbestanden bevatten een hardcoded beheerder gebruikersnamen en wachtwoorden wordt aanbevolen dat u onmiddellijk wijzigen op alle VMs. Op elke VM in Azure Portal en vervolgens op **wachtwoord opnieuw instellen** in de bladeserver **ondersteuning + het oplossen van problemen** . Selecteer **wachtwoord opnieuw instellen** in het dropdown **-modus** en selecteer vervolgens een nieuwe **gebruikersnaam** en **wachtwoord**. Klik op de knop **bijwerken** om de nieuwe gebruikersnaam en wachtwoord te behouden. 

Zie voor informatie over manieren waarop u deze referentiearchitectuur implementeren, het Leesmij-bestand in de [richtlijnen voor-één-vm] [ github-folder] map Github. 

## <a name="next-steps"></a>Volgende stappen

Om te bereiken deze referentiearchitectuur voor maximale beschikbaarheid, het beste [implementeert voor meerdere regio's][multi-dc].

<!-- links -->

[arm-templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/
[azure-administration]: ../automation/automation-intro.md
[azure-audit-logs]: ../resource-group-audit.md
[azure-availability-sets]: ../virtual-machines/virtual-machines-windows-manage-availability.md#configure-each-application-tier-into-separate-availability-sets
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-key-vault]: https://azure.microsoft.com/services/key-vault.md
[azure-load-balancer]: ../load-balancer/load-balancer-overview.md
[bastion-host]: https://en.wikipedia.org/wiki/Bastion_host
[CIDR]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
[chef]: https://www.chef.io/solutions/azure/
[dmz]: guidance-iaas-ra-secure-vnet-dmz.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier-sql
[lb-external-create]: ../load-balancer/load-balancer-get-started-internet-portal.md
[lb-internal-create]: ../load-balancer/load-balancer-get-started-ilb-arm-portal.md
[load-balancer-external]: ../load-balancer/load-balancer-internet-overview.md
[load-balancer-internal]: ../load-balancer/load-balancer-internal-overview.md
[multi-dc]: guidance-compute-multiple-datacenters.md
[multi-vm]: guidance-compute-multi-vm.md
[n-tier]: guidance-compute-n-tier-vm.md
[naming conventions]: guidance-naming-conventions.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[nsg-rules]: ../best-practices-resource-manager-security.md#network-security-groups
[operations-management-suite]: https://www.microsoft.com/en-us/server-cloud/operations-management-suite/overview.aspx
[plan-network]: ../virtual-network/virtual-network-vnet-plan-design-arm.md
[private-ip-space]: https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces
[openbare IP-adres]: ../virtual-network/virtual-network-ip-addresses-overview-arm.md
[puppet]: https://puppetlabs.com/blog/managing-azure-virtual-machines-puppet
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[sql-alwayson]: https://msdn.microsoft.com/en-us/library/hh510230.aspx
[sql-alwayson-force-failover]: https://msdn.microsoft.com/en-us/library/ff877957.aspx
[sql-alwayson-getting-started]: https://msdn.microsoft.com/en-us/library/gg509118.aspx
[sql-alwayson-ilb]: https://blogs.msdn.microsoft.com/igorpag/2016/01/25/configure-an-ilb-listener-for-sql-server-alwayson-availability-groups-in-azure-arm/
[sql-alwayson-listeners]: https://msdn.microsoft.com/en-us/library/hh213417.aspx
[sql-alwayson-read-only-routing]: https://technet.microsoft.com/en-us/library/hh213417.aspx#ConnectToSecondary
[sql-keyvault]: ../virtual-machines/virtual-machines-windows-ps-sql-keyvault.md
[vm-planned-maintenance]: ../virtual-machines/virtual-machines-windows-planned-maintenance.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines
[vnet faq]: ../virtual-network/virtual-networks-faq.md
[wsfc-whats-new]: https://technet.microsoft.com/windows-server-docs/failover-clustering/whats-new-in-failover-clustering
[Nagios]: https://www.nagios.org/
[Zabbix]: http://www.zabbix.com/
[Icinga]: http://www.icinga.org/
[VM-sizes]: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/deploy-reference-architecture.sh
[vnet-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/virtualNetwork.parameters.json
[vnet-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/virtualNetwork.parameters.json
[nsg-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/networkSecurityGroups.parameters.json
[nsg-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/networkSecurityGroups.parameters.json
[webtier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/webTier.parameters.json
[webtier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/webTier.parameters.json
[businesstier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/businessTier.parameters.json
[businesstier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/businessTier.parameters.json
[datatier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/dataTier.parameters.json
[datatier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/dataTier.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[0]: ./media/blueprints/compute-n-tier.png "N-lagige architectuur met Microsoft Azure"
[1]: ./media/blueprints/deploybutton.png 
[2]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2FvirtualNetwork.azuredeploy.json
[3]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2Fworkload.azuredeploy.json
[4]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2Fsecurity.azuredeploy.json