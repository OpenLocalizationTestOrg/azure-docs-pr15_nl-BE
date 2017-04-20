<properties
   pageTitle="Openbare en particuliere IP-adressering in Azure Resource Manager | Microsoft Azure"
   description="Meer informatie over openbare en particuliere IP-adressering in Azure Resource Manager"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="ip-addresses-in-azure"></a>IP-adressen in Azure
U kunt IP-adressen toewijzen aan Azure middelen om te communiceren met andere bronnen Azure, het netwerk in de lokalen en het Internet. Er zijn twee typen IP-adressen die u in Azure gebruiken kunt:

- **Openbare IP-adressen**: gebruikt voor communicatie met het Internet, met inbegrip van Azure publieke diensten
- **Particuliere IP-adressen**: voor communicatie binnen een Azure virtual network (VNet) gebruikt en uw locatie op het netwerk wanneer u een VPN-gateway of ExpressRoute circuit met uw netwerk uitbreiden naar Azure.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassieke implementatiemodel](virtual-network-ip-addresses-overview-classic.md).

Als u bekend met het klassieke implementatiemodel bent, controleert u de [verschillen in IP-adressen tussen klassieke en Resource Manager](virtual-network-ip-addresses-overview-classic.md#Differences-between-Resource-Manager-and-classic-deployments).

## <a name="public-ip-addresses"></a>Openbare IP-adressen
Openbare IP-adressen toestaan Azure middelen om te communiceren met het Internet en Azure publieke diensten zoals [Azure bestand Vgx. Cache](https://azure.microsoft.com/services/cache/), [Azure gebeurtenis Hubs](https://azure.microsoft.com/services/event-hubs/), [SQL-databases](../sql-database/sql-database-technical-overview.md)en [Azure opslag](../storage/storage-introduction.md).

In Azure Resource Manager, een [openbaar IP-](resource-groups-networking.md#public-ip-address) adres is een bron die de eigen eigenschappen. U kunt een openbare bron van het IP-adres koppelen aan een van de volgende bronnen:

- Virtuele machines (VM)
- Netwerktaakverdeling internetverbinding
- VPN-gateways
- Toepassingsgateways

### <a name="allocation-method"></a>Toewijzingsmethode
Er zijn twee manieren waarop een IP-adres is toegewezen aan een *openbaar* IP-bron - *dynamische* of *statische*. De standaardmethode voor de toewijzing is *dynamisch*, wanneer een IP-adres **niet** is toegewezen op het moment van de creatie. In plaats daarvan wordt het openbare IP-adres toegewezen wanneer u start (of maken) de gekoppelde bron (zoals een VM of load balancer). Het IP-adres wordt vrijgegeven wanneer u stoppen (of verwijderen) de bron. Dit zorgt ervoor dat het IP-adres te wijzigen wanneer u stoppen en starten van een bron.

Als u dat het IP-adres voor de bijbehorende resource blijven hetzelfde, kunt u de toewijzingsmethode expliciet instellen op *statische*. In dit geval wordt een IP-adres direct toegewezen. Wanneer u de resource verwijderen of wijzigen in *dynamische*methode voor de toewijzing wordt vrijgegeven.

>[AZURE.NOTE] Zelfs als u de toewijzingsmethode voor *statische*, kunt u het IP-adres toegewezen aan de *openbare IP-bron*kan niet opgeven. In plaats daarvan wordt het toegewezen uit een groep van de beschikbare IP-adressen op de Azure-locatie die in de bron wordt gemaakt.

Openbare statische IP-adressen worden vaak gebruikt in de volgende scenario's:

- Eindgebruikers moeten firewallregels om te communiceren met uw Azure resources bijwerken.
- DNS-naamomzetting, waarbij een IP-adres in dat A-records bij te werken.
- Azure resources delen met andere toepassingen of services die gebruikmaken van een IP-adres gebaseerde beveiligingsmodel.
- Gebruik van SSL-certificaten die zijn gekoppeld aan een IP-adres.

>[AZURE.NOTE] De lijst met IP-adresbereiken waarvoor openbare IP-adressen (dynamic/statisch) worden toegewezen aan resources Azure is [Azure Datacenter IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653)gepubliceerd.

### <a name="dns-hostname-resolution"></a>DNS-hostnaamherleiding
U kunt opgeven dat een DNS-domein naamlabel voor een openbare IP-resource waaraan een toewijzing voor *domainnamelabel*wordt gemaakt. *locatie*. cloudapp.azure.com het openbare IP-adres in de Azure managed DNS-servers. Bijvoorbeeld, als u een openbaar IP-bron met **contoso** als een *domainnamelabel* in de **VS West maken** Azure *locatie*, de volledig gekwalificeerde domeinnaam (FQDN) van de naam **contoso.westus.cloudapp.azure.com** wordt omgezet in het openbare IP-adres van de resource. U kunt deze FQDN-naam voor het maken van een aangepaste domein CNAME-record verwijst naar het openbare IP-adres in Azure.

>[AZURE.IMPORTANT] Naam domeinlabel gemaakt moet uniek zijn binnen de Azure locatie.  

### <a name="virtual-machines"></a>Virtuele machines
U kunt een openbaar IP-adres koppelen aan een [Windows](../virtual-machines/virtual-machines-windows-about.md) - of [Linux](../virtual-machines/virtual-machines-linux-about.md) VM toewijzen aan de **netwerk-interface**. In het geval van een meervoudige netwerkinterface VM, kunt u deze toewijzen aan alleen de *primaire* netwerkinterface. U kunt een dynamische of een statisch, openbaar IP-adres toewijzen aan een VM.

### <a name="internet-facing-load-balancers"></a>Netwerktaakverdeling internetverbinding
U kunt een openbaar IP-adres koppelen aan een [Azure Load Balancer](../load-balancer/load-balancer-overview.md)toewijzen aan de load balancer **frontend** -configuratie. Deze openbare IP-adres, fungeert als een taakverdeling virtueel IP-adres (VIP). U kunt een dynamische of een statisch, openbaar IP-adres toewijzen aan een front-end-taakverdeling. U kunt ook meerdere openbare IP-adressen toewijzen aan een taakverdeling front-end, waarmee [Meerdere VIP](../load-balancer/load-balancer-multivip.md) scenario's zoals een omgeving met meerdere huurder met SSL-gebaseerde websites.

### <a name="vpn-gateways"></a>VPN-gateways
[Azure VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) wordt Azure virtueel netwerk (VNet) verbinding maken met een netwerk van lokalen of andere Azure-VNets gebruikt. U moet een openbaar IP-adres toewijzen aan de **IP-configuratie** te kunnen communiceren met het externe netwerk. Op dit moment kunt u alleen een *dynamische* openbare IP-adres toewijzen aan een VPN-gateway.

### <a name="application-gateways"></a>Toepassingsgateways
U kunt een openbaar IP-adres koppelen aan een [Toepassingsgateway](../application-gateway/application-gateway-introduction.md)Azure toewijzen aan de gatewayconfiguratie **frontend** . Deze openbare IP-adres, fungeert als een VIP verdeeld. Op dit moment kunt u alleen een *dynamische* openbare IP-adres toewijzen aan een toepassing gateway frontend-configuratie.

### <a name="at-a-glance"></a>Op-overzicht
In de onderstaande tabel toont de specifieke eigenschap waarmee een openbaar IP-adres kan worden gekoppeld aan een bron op het hoogste niveau en de mogelijke methoden voor de verdeelsleutel (dynamische of statische) die kunnen worden gebruikt.

|Resource op het hoogste niveau|IP-adres vereniging|Dynamische|Statische|
|---|---|---|---|
|Virtuele machine|Netwerk-interface|Ja|Ja|
|Taakverdeling|Front-end-configuratie|Ja|Ja|
|VPN-gateway|IP-configuratie van de standaardgateway|Ja|Nee|
|Toepassingsgateway|Front-end-configuratie|Ja|Nee|

## <a name="private-ip-addresses"></a>Particuliere IP-adressen
Particuliere IP-adressen toestaan Azure middelen om te communiceren met andere bronnen in een [virtueel netwerk](virtual-networks-overview.md) of op een netwerk op locatie via een VPN-gateway of ExpressRoute circuit, zonder een Internet bereikbaar IP-adres te gebruiken.

In het implementatiemodel Azure Resource Manager is een particulier IP-adres gekoppeld aan de volgende typen Azure bronnen:

- VMs
- Interne netwerktaakverdeling (ILBs)
- Toepassingsgateways

### <a name="allocation-method"></a>Toewijzingsmethode
Een particulier IP-adres wordt toegewezen uit het adresbereik van het subnet waaraan de resource is gekoppeld. Het adresbereik van het subnet zelf is een onderdeel van het adresbereik van de VNet.

Er zijn twee manieren waarop een particulier IP-adres wordt toegewezen: *dynamische* of *statische*. De standaardmethode voor de toewijzing is een *dynamische*, waarbij het IP-adres wordt toegewezen vanuit de bron van subnet (via DHCP). Dit IP-adres kunt wijzigen wanneer u stopt en start de resource.

Methode voor de toewijzing kunt u instellen op *statische* zodat het IP-adres blijft ongewijzigd. In dit geval moet u ook een geldig IP-adres die deel uitmaakt van het subnet van de resource opgeven.

Statische particuliere IP-adressen worden vaak gebruikt voor:

- VMs die als domeincontrollers of DNS-servers fungeren.
- Bronnen waarvoor de firewall-regels met IP-adressen.
- Bronnen die zijn geopend door andere apps/bronnen via een IP-adres.

### <a name="virtual-machines"></a>Virtuele machines
Een particulier IP-adres wordt toegewezen aan de **netwerkinterface** van een [Windows](../virtual-machines/virtual-machines-windows-about.md) - of [Linux](../virtual-machines/virtual-machines-linux-about.md) VM. In het geval van een interface met meerdere netwerk VM haalt elke interface een particuliere IP-adres toegewezen. U kunt de toewijzingsmethode opgeven als dynamische of statische voor een netwerkinterface.

#### <a name="internal-dns-hostname-resolution-for-vms"></a>Interne DNS-hostnaamherleiding (voor VMs)
Alle Azure VMs worden standaard zodanig geconfigureerd met [Azure managed DNS-servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) , tenzij u expliciet aangepaste DNS-servers configureren. Deze DNS-servers bieden interne naamomzetting voor VMs die zich binnen de dezelfde VNet bevinden.

Wanneer u een VM maakt, wordt een toewijzing voor de naam van de host op het particuliere IP-adres toegevoegd aan de Azure managed DNS-servers. In het geval van een meervoudige netwerkinterface VM, is de hostnaam toegewezen aan de particuliere IP-adres van de primaire netwerkinterface.

VMs met Azure managed DNS-servers zijn geconfigureerd om te zetten in de hostnamen van alle VMs binnen hun VNet hun particuliere IP-adressen kunnen worden.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Interne netwerktaakverdeling (ILB) & Toepassingsgateways
U kunt een persoonlijk IP-adres toewijzen aan de **front-end** -configuratie van een [Interne taakverdeling Azure](../load-balancer/load-balancer-internal-overview.md) (ILB) of een [Toepassingsgateway Azure](../application-gateway/application-gateway-introduction.md). Dit persoonlijke IP-adres fungeert als een interne eindpunt alleen toegankelijk voor de bronnen binnen het virtuele netwerk (VNet) en de externe netwerken verbonden met de VNet. De front-end-configuratie kunt u ofwel een dynamische of statische particulier IP-adres toewijzen.

### <a name="at-a-glance"></a>Op-overzicht
In de onderstaande tabel toont de specifieke eigenschap via een particulier IP-adres kan worden gekoppeld aan een bron op het hoogste niveau en de mogelijke methoden voor de verdeelsleutel (dynamische of statische) die kunnen worden gebruikt.

|Resource op het hoogste niveau|IP-adres vereniging|Dynamische|Statische|
|---|---|---|---|
|Virtuele machine|Netwerk-interface|Ja|Ja|
|Taakverdeling|Front-end-configuratie|Ja|Ja|
|Toepassingsgateway|Front-end-configuratie|Ja|Ja|

## <a name="limits"></a>Limieten

De beperkingen voor IP-adressen zijn opgenomen in de volledige lijst met [beperkingen voor netwerken](azure-subscription-service-limits.md#networking-limits) in Azure. Deze limieten zijn per regio en per abonnement. U kunt [contact opnemen met ondersteuning](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om het standaard verruimen tot de maximumgehalten op basis van uw bedrijfsbehoeften.

## <a name="pricing"></a>Prijzen

Openbare IP-adressen mogelijk een nominale vergoeding. Voor meer informatie over IP-adres prijzen in Azure, bekijk de pagina [prijzen van IP-adres](https://azure.microsoft.com/pricing/details/ip-addresses) .

## <a name="next-steps"></a>Volgende stappen
- [Een VM met een statische openbare IP-adres met behulp van de portal Azure implementeren](virtual-network-deploy-static-pip-arm-portal.md)
- [Implementeren van een VM met een statische openbare IP-adres met behulp van een sjabloon](virtual-network-deploy-static-pip-arm-template.md)
- [Een VM met een statisch particuliere IP-adres met behulp van de portal Azure implementeren](virtual-networks-static-private-ip-arm-pportal.md)
