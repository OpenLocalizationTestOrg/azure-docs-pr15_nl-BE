<properties
   pageTitle="Openbare en particuliere IP-adressering (klassiek) in Azure | Microsoft Azure"
   description="Meer informatie over openbare en particuliere IP-adressering in Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/11/2016"
   ms.author="jdial" />

# <a name="ip-addresses-classic-in-azure"></a>IP-adressen (klassiek) in Azure
U kunt IP-adressen toewijzen aan Azure middelen om te communiceren met andere bronnen Azure, het netwerk in de lokalen en het Internet. Er zijn twee typen IP-adressen kunt u in Azure: openbare en particuliere.

Openbare IP-adressen worden gebruikt voor communicatie met het Internet, met inbegrip van Azure publieke diensten.

Particuliere IP-adressen worden gebruikt voor communicatie binnen Azure virtueel netwerk (VNet), een cloud-service en het netwerk op ruimten wanneer u een VPN-gateway of ExpressRoute circuit met uw netwerk uitbreiden naar Azure.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Meer informatie over het [uitvoeren van deze stappen het implementatiemodel Resource Manager](virtual-network-ip-addresses-overview-arm.md).

## <a name="public-ip-addresses"></a>Openbare IP-adressen
Openbare IP-adressen toestaan Azure middelen om te communiceren met het Internet en Azure publieke diensten zoals [Azure bestand Vgx. Cache](https://azure.microsoft.com/services/cache/), [Azure gebeurtenis Hubs](https://azure.microsoft.com/services/event-hubs/), [SQL-databases](../sql-database/sql-database-technical-overview.md)en [Azure opslag](../storage/storage-introduction.md).

Een openbaar IP-adres is gekoppeld aan de volgende resourcetypen:

- Cloud services
- IaaS virtuele Machines (VMs)
- PaaS rol exemplaren
- VPN-gateways
- Toepassingsgateways

### <a name="allocation-method"></a>Toewijzingsmethode
Wanneer een openbaar IP-adres worden toegewezen aan een resource Azure moet, is *dynamisch* toegewezen vanuit een groep met beschikbare openbare IP-adres binnen de locatie die de bron is gemaakt. Dit IP-adres wordt vrijgegeven wanneer de resource wordt gestopt. In het geval van een cloud-service, dit gebeurt wanneer de functie overal worden gestopt, die kunnen worden vermeden door een IP-adres *statisch* (gereserveerd) (Zie [Cloud Services](#Cloud-services)).

>[AZURE.NOTE] De lijst met IP-adresbereiken waarvoor openbare IP-adressen worden toegewezen aan resources Azure is [Azure Datacenter IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653)gepubliceerd.

### <a name="dns-hostname-resolution"></a>DNS-hostnaamherleiding
Als u een cloud-service of een VM IaaS maakt, moet u bieden een cloud service DNS-naam die uniek voor alle resources in Azure is. Hiermee maakt u een toewijzing in de Azure managed DNS-servers voor *DNS-naam*. cloudapp.net aan het openbare IP-adres van de resource. Als u een cloud service met een cloud service DNS-naam van **contoso**maakt, wordt de volledig gekwalificeerde domeinnaam (FQDN) van de naam **contoso.cloudapp.net** op een openbaar IP-adres (VIP) van de cloud-service opgelost. U kunt deze FQDN-naam voor het maken van een aangepaste domein CNAME-record verwijst naar het openbare IP-adres in Azure.

### <a name="cloud-services"></a>Cloud services
Een cloud-service heeft altijd een openbaar IP-adres is een virtueel IP-adres (VIP) genoemd. In een cloud service koppelen verschillende poorten in de VIP op interne poorten op VMs en exemplaren van de rol binnen de cloud-service kunt u eindpunten. 

Een cloud-service kan meerdere VMs IaaS en PaaS rol gevallen alle blootgestelde via de dezelfde cloud service VIP bevatten. U kunt ook [meerdere VIP's naar een cloud-service](../load-balancer/load-balancer-multivip.md), waarmee meerdere VIP scenario's zoals een omgeving met meerdere huurder met SSL-gebaseerde websites.

U kunt ervoor zorgen dat het openbare IP-adres van een cloud-service blijft hetzelfde, zelfs als u alle exemplaren van de rol zijn gestopt met behulp van een *statische* openbare IP-adres, [Gereserveerde IP](virtual-networks-reserved-public-ip.md)genoemd. U kunt een statisch IP-bron (gereserveerd) op een bepaalde locatie te maken en toe te wijzen aan een cloud-service op die locatie. U kunt het IP-adres opgeven voor de gereserveerde IP, deze wordt toegewezen uit de pool van beschikbare IP-adressen op de locatie die wordt gemaakt. Dit IP-adres wordt pas vrijgegeven wanneer het expliciet verwijderen.

Statische (gereserveerd) openbare IP-adressen worden vaak gebruikt in de gevallen waar een cloud-service:

- firewallregels worden ingesteld door de eindgebruikers vereist.
- afhankelijk van externe DNS-naamomzetting en een dynamisch IP-adres waarvoor een records bijwerken.
- externe webservices die gebruikmaken van IP-gebaseerde beveiligingsmodel verbruikt.
- SSL-certificaten die zijn gekoppeld aan een IP-adres gebruikt.

>[AZURE.NOTE] Wanneer u een klassieke VM maakt, wordt een container *cloud service* gemaakt door Azure is een virtueel IP-adres (VIP). Wanneer de oprichting vindt plaats via de portal, is een standaard RDP of SSH- *eindpunt* geconfigureerd door de portal zodat kunt u via de service cloud VIP VM. Deze VIP cloud-service kan worden gereserveerd, die effectief biedt een gereserveerd IP-adres verbinding maken met de VM. U kunt extra poorten openen door meer eindpunten configureren.

### <a name="iaas-vms-and-paas-role-instances"></a>Exemplaren van VMs IaaS en PaaS-rol
U kunt een openbare IP-adres rechtstreeks in een [VM](../virtual-machines/virtual-machines-linux-about.md) -IaaS of PaaS rol exemplaar in een cloud-service. Dit is een instantieniveau openbare IP-adres ([ILPIP](virtual-networks-instance-level-public-ip.md)) genoemd. Deze openbare IP-adres kan alleen dynamische zijn.

>[AZURE.NOTE] Dit verschilt van de VIP van de cloud-service een container voor VMs IaaS of PaaS rol instanties is, omdat een cloud-service kan meerdere IaaS VMs bevatten of PaaS rol instanties alle toegankelijk zijn via de dezelfde cloud service VIP.

### <a name="vpn-gateways"></a>VPN-gateways
Een [VPN-gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) kan verbinding maken met een VNet Azure andere Azure VNets of netwerken in gebouwen worden gebruikt. Een VPN-gateway is een openbaar IP-adres *dynamisch*, waardoor de communicatie met het externe netwerk worden toegewezen.

### <a name="application-gateways"></a>Toepassingsgateways
Een Azure [toepassingsgateway](../application-gateway/application-gateway-introduction.md) kan worden gebruikt voor Layer7-taakverdeling op route netwerkverkeer op basis van HTTP. Toepassingsgateway wordt een openbaar IP-adres *dynamisch*, die als de VIP verdeeld fungeert toegewezen.

### <a name="at-a-glance"></a>In één oogopslag
In de onderstaande tabel ziet u welke typen resources met de mogelijke toewijzingsmethoden (dynamisch/statisch) en de mogelijkheid om meerdere openbare IP-adressen toewijzen.

|Resource|Dynamische|Statische|Meerdere IP-adressen|
|---|---|---|---|
|Cloud-service|Ja|Ja|Ja|
|VM IaaS of PaaS rol exemplaar|Ja|Nee|Nee|
|VPN-gateway|Ja|Nee|Nee|
|Toepassingsgateway|Ja|Nee|Nee|

## <a name="private-ip-addresses"></a>Particuliere IP-adressen
Particuliere IP-adressen toestaan Azure middelen om te communiceren met andere bronnen in een cloud-service of een [virtueel netwerk](virtual-networks-overview.md)(VNet) of netwerk voor bedrijfsruimten (via een VPN-gateway of ExpressRoute circuit) zonder een Internet bereikbaar IP-adres te gebruiken.

In Azure klassieke implementatiemodel kan een persoonlijk IP-adres worden toegewezen aan de volgende Azure bronnen:

- Exemplaren van VMs IaaS en PaaS-rol
- Interne taakverdeling
- Toepassingsgateway

### <a name="iaas-vms-and-paas-role-instances"></a>Exemplaren van VMs IaaS en PaaS-rol
Virtuele machines (VMs) gemaakt met de klassieke implementatiemodel altijd worden geplaatst in een cloud-service, vergelijkbaar met PaaS rol exemplaren. Het gedrag van particuliere IP-adressen zijn dus identiek voor deze bronnen.

Het is belangrijk te weten dat een cloud-service kan worden geïmplementeerd op twee manieren:

- Als een *zelfstandige* cloud service, indien niet binnen een virtueel netwerk is.
- Als onderdeel van een virtueel netwerk.

#### <a name="allocation-method"></a>Toewijzingsmethode
In het geval van een *zelfstandige* cloud service u resources een particulier IP-adres toegewezen *dynamisch* vanuit Azure datacenter particuliere IP-adresbereik. Het kan alleen worden gebruikt voor communicatie met andere VMs binnen dezelfde cloud-service worden gebruikt. Dit IP-adres kunt wijzigen wanneer de resource wordt gestopt en gestart.

Bronnen ophalen bij een cloud service geïmplementeerd in een virtueel netwerk, particuliere IP-adres uit het adresbereik van de bijbehorende subnetten (zoals opgegeven in de netwerkconfiguratie) toegewezen. Dit persoonlijke IP-adres kan worden gebruikt voor communicatie tussen alle VMs binnen de VNet.

Bovendien in het geval van cloud diensten binnen een VNet, wordt een persoonlijk IP-adres toegewezen *dynamisch* (via DHCP) standaard. Deze kunt wijzigen wanneer de resource wordt gestopt en gestart. Het IP-adres hetzelfde blijft, zodat moet u de methode voor de toewijzing ingesteld op *statische*en geef een geldig IP-adres binnen het bijbehorende adresbereik.

Statische particuliere IP-adressen worden vaak gebruikt voor:

 - VMs die als domeincontrollers of DNS-servers fungeren.
 - VMs waarvoor firewallregels met IP-adressen.
 - VMs met services die worden benaderd door andere apps via een IP-adres.

#### <a name="internal-dns-hostname-resolution"></a>Interne DNS-hostnaamherleiding
Alle Azure VMs en PaaS rol exemplaren zijn geconfigureerd met [Azure managed DNS-servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) standaard, tenzij u expliciet aangepaste DNS-servers configureren. Deze DNS-servers bieden interne naamomzetting voor VMs en rol van instanties die zich in dezelfde VNet of cloud-service bevinden.

Wanneer u een VM maakt, wordt een toewijzing voor de naam van de host op het particuliere IP-adres toegevoegd aan de Azure managed DNS-servers. In het geval van een multi-NIC-VM is de hostnaam toegewezen aan de particuliere IP-adres van de primaire NIC. Deze toewijzingsgegevens is echter beperkt tot de bronnen binnen dezelfde cloud service- of VNet.

In het geval van een *zelfstandige* cloud service kun je hostnamen van alle exemplaren van de VMs/rol binnen dezelfde cloud service alleen oplossen. In het geval van een cloud-service binnen een VNet zult u kunnen herleiden van hostnamen van alle exemplaren van een VMs/rol binnen de VNet.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Interne netwerktaakverdeling (ILB) & Toepassingsgateways
U kunt een persoonlijk IP-adres toewijzen aan de **front-end** -configuratie van een [Interne taakverdeling Azure](../load-balancer/load-balancer-internal-overview.md) (ILB) of een [Toepassingsgateway Azure](../application-gateway/application-gateway-introduction.md). Dit persoonlijke IP-adres fungeert als een interne eindpunt alleen toegankelijk voor de bronnen binnen het virtuele netwerk (VNet) en de externe netwerken verbonden met de VNet. De front-end-configuratie kunt u ofwel een dynamische of statische particulier IP-adres toewijzen. U kunt ook meerdere particuliere IP-adressen aan meerdere vip scenario's mogelijk.

### <a name="at-a-glance"></a>In één oogopslag
In de onderstaande tabel ziet u welke typen resources met de mogelijke toewijzingsmethoden (dynamisch/statisch) en de mogelijkheid om meerdere particuliere IP-adressen toewijzen.

|Resource|Dynamische|Statische|Meerdere IP-adressen|
|---|---|---|---|
|VM (in een *zelfstandige* cloud service)|Ja|Ja|Ja|
|Exemplaar van PaaS-rol (in een *zelfstandige* cloud service)|Ja|Nee|Ja|
|VM- of PaaS rol exemplaar (in een VNet)|Ja|Ja|Ja|
|Interne load balancer-front-end|Ja|Ja|Ja|
|Application gateway-front-end|Ja|Ja|Ja|

## <a name="limits"></a>Limieten

De onderstaande tabel ziet u de beperkingen op IP-adressering in Azure per abonnement. U kunt [contact opnemen met ondersteuning](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om het standaard verruimen tot de maximumgehalten op basis van uw bedrijfsbehoeften.

||Standaardlimiet|Maximum aantal|
|---|---|---|
|Openbare IP-adressen (dynamisch)|5|Neem contact op met ondersteuning|
|Gereserveerde openbare IP-adressen|20|Neem contact op met ondersteuning|
|Openbare VIP per distributie (cloud service)|5|Neem contact op met ondersteuning|
|Particuliere VIP (ILB) per distributie (cloud service)|1|1|

Zorg ervoor dat u de volledige lijst met [beperkingen voor netwerken](azure-subscription-service-limits.md#networking-limits) in Azure lezen.

## <a name="pricing"></a>Prijzen

Openbare IP-adressen zijn in de meeste gevallen gratis. Er is een nominale vergoeding aanvullende en/of statische openbare IP-adressen gebruiken. Zorg ervoor dat u begrijpt de [prijsstructuur van openbare IP-adressen](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="differences-between-resource-manager-and-classic-deployments"></a>Verschillen tussen Resource Manager en klassieke implementaties
Hieronder vindt u een vergelijking van de IP-adressering functies in Resource Manager en de klassieke implementatiemodel.

||Resource|Klassiek|Resource Manager|
|---|---|---|---|
|**Openbare IP-adres**|VM|Bedoeld als een ILPIP (alleen dynamisch)|Bedoeld als een openbaar IP-adres (dynamische of statische)|
|||Toegewezen aan een VM IaaS of een exemplaar van de rol van PaaS|Dat is gekoppeld aan een van de VM-NIC|
||Internet-gerichte taakverdeling|VIP (dynamische) of een gereserveerd IP-(statisch) genoemd.|Bedoeld als een openbaar IP-adres (dynamische of statische)|
|||Toegewezen aan een cloud-service|Dat is gekoppeld aan de taakverdeling front-end-config|
||||
|**Particulier IP-adres**|VM|Bedoeld als een DIP|Een particulier IP-adres genoemd|
|||Toegewezen aan een VM IaaS of een exemplaar van de rol van PaaS|Toegewezen aan de Netwerkkaart van de VM|
||Interne taakverdeling (ILB)|Toegewezen aan het ILB (dynamische of statische)|Toegewezen aan van de ILB-front-end-configuratie (dynamische of statische)|

## <a name="next-steps"></a>Volgende stappen
- [Een VM met een statisch IP-adres persoonlijke implementeren](virtual-networks-static-private-ip-classic-pportal.md) met behulp van de klassieke portal.
