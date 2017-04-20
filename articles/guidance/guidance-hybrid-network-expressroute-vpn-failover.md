<properties
   pageTitle="Implementatie van een maximaal beschikbare hybride netwerkarchitectuur | Microsoft Azure"
   description="Het implementeren van een beveiligde site naar site netwerkarchitectuur die omvat een Azure virtual network en een in de lokalen netwerk verbonden via ExpressRoute met VPN-gateway failover."
   services="guidance,virtual-network,vpn-gateway,expressroute"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-highly-available-hybrid-network-architecture"></a>Implementatie van een maximaal beschikbare hybride netwerkarchitectuur

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dit artikel bevat verschillende aanbevelingen voor de verbinding met een netwerk op ruimten met behulp van ExpressRoute, met een website virtueel particulier netwerk (VPN) als de verbinding van een failover-virtuele netwerken op Azure. De verkeersstromen tussen het netwerk van lokalen en een Azure virtueel netwerk (VNet) via een ExpressRoute.  Als er een verlies van connectiviteit in het circuit ExpressRoute, wordt verkeer doorgestuurd via een IPSec VPN-tunnel.

> [AZURE.NOTE] Azure heeft twee verschillende modellen: [Resource Manager] [ resource-manager-overview] en klassiek. Deze blauwdruk gebruikt resourcemanager, wordt aangeraden voor nieuwe implementaties.

Typische use-cases voor deze architectuur zijn:

- Hybride toepassingen waarbij werklasten worden verdeeld tussen een netwerk op gebouwen en Azure.

- Toepassingen van grootschalige, kritieke werklast die een hoge mate van schaalbaarheid vereisen.

- Grootschalige maken en terugzetten van back-up faciliteiten voor gegevens die op een externe locatie moet worden opgeslagen.

- Grote gegevens werklasten verwerken.

- Met behulp van Azure als een disaster recovery-site.

Houd er rekening mee dat als de ExpressRoute circuit niet beschikbaar is, de route voor de VPN-wordt alleen private peering verbindingen verwerken. Openbare Microsoft peering verbindingen en peering worden doorgegeven via het Internet.

## <a name="architecture-diagram"></a>Architectuurdiagram

>[AZURE.NOTE] De [Azure VPN-Gateway service] [ azure-vpn-gateway] implementeert twee soorten virtueel netwerkgateways; VPN virtueel netwerkgateways en ExpressRoute virtueel netwerkgateways. In dit document, worden de term *VPN-Gateway* verwijst naar de Azure service, terwijl de zinnen *VPN virtueel netwerkgateway* en *ExpressRoute virtueel netwerkgateway* gebruikt om te verwijzen naar de VPN- en ExpressRoute implementaties van de gateway, respectievelijk.

In het volgende diagram worden de belangrijke onderdelen van deze architectuur:

> Een Visio-document met dit Architectuurdiagram is beschikbaar op het [Microsoft download center][visio-download]. Dit diagram is op de 'hybride netwerk - Emergency Recovery VPN' pagina.

![[0]][0]

- **Azure virtuele netwerken (VNets).** Elk VNet bevindt zich in een bepaalde regio Azure en meerdere lagen kunt onderbrengen. Lagen kunnen worden gesegmenteerd subnetten met in elke VNet en/of netwerk-beveiligingsgroepen (NSGs).

- **Het bedrijfsnetwerk op gebouwen.** Dit is een netwerk van computers en apparaten, verbonden via een particuliere LAN-netwerk binnen een organisatie worden uitgevoerd.

- **VPN-toestel.** Dit is een apparaat of een service voor externe verbinding met het netwerk op gebouwen. Het VPN-apparaat is mogelijk een hardware-apparaat of kan een softwareoplossing zoals de Routing and Remote Access Service (RRAS) in Windows Server 2012.

> [AZURE.NOTE] Zie de instructies van het betreffende apparaat in de [lijst met VPN-apparaten worden ondersteund door Azure]voor een lijst met ondersteunde VPN-apparaten en informatie over het configureren van de geselecteerde VPN-apparaten voor een verbinding met een VPN-Gateway van Azure,[vpn-appliance].

- **VPN virtueel netwerkgateway.** Het virtuele netwerk VPN-gateway kan de VNet verbinding maken met de VPN-toestel in het netwerk op gebouwen. Het virtuele netwerk VPN-gateway is geconfigureerd om aanvragen te accepteren vanaf het netwerk op ruimten alleen via het VPN-apparaat. Zie voor meer informatie [verbinding maken met een netwerk van gebouwen met een virtueel netwerk Microsoft Azure][connect-to-an-Azure-vnet].

- **ExpressRoute virtueel netwerkgateway.** De gateway ExpressRoute virtueel netwerk kan de VNet verbinding maken met het ExpressRoute circuit gebruikt voor verbinding met het netwerk op gebouwen.

- **Subnetmasker, gateway.** Virtueel netwerkgateways worden vastgehouden in hetzelfde subnet bevinden.

- **VPN-verbinding.** De verbinding heeft eigenschappen die het verbindingstype (IPSec) en de gedeeld met het toestel op ruimten VPN-sleutel voor het coderen van verkeer.

- **ExpressRoute circuit.** Dit is een laag 2 of layer 3-circuit geleverd door de provider verbinding met dat netwerk joins in ruimten met Azure via de routers van de rand. Het circuit wordt gebruikt voor de hardware infrastructuur beheerd door de provider connectiviteit.

- **Wolk van N-tier-toepassing.** Dit is de toepassing die wordt gehost in Azure. Het kan meerdere lagen, met meerdere subnetten zijn verbonden via Azure Netwerktaakverdeling bevatten. Het verkeer in elk subnet kan worden onderworpen aan de regels die zijn gedefinieerd met behulp van [Beveiligingsgroepen Azure-netwerk][azure-network-security-group](NSGs). Zie voor meer informatie, [aan de slag met Azure Microsoft security][getting-started-with-azure-security].

## <a name="recommendations"></a>Aanbevelingen

Azure biedt veel verschillende bronnen en brontypen, zodat deze referentiearchitectuur kan worden ingericht veel verschillende manieren. We hebben een sjabloon Azure Resource Manager wilt installeren de referentiearchitectuur die volgt op deze aanbevelingen verstrekt. Als u kiest voor het maken van uw eigen referentiearchitectuur Volg deze aanbevelingen tenzij u een specifieke vereiste is dat een aanbeveling niet past.

### <a name="vnet-and-gatewaysubnet"></a>VNet en GatewaySubnet

De gateway ExpressRoute virtueel netwerk en het virtuele netwerk VPN-gateway in de dezelfde VNet maken. Dit betekent dat ze hetzelfde subnet genaamd **GatewaySubnet** moeten delen

Als de VNet een subnet genaamd **GatewaySubnet**bevat, zorg ervoor dat er een /27 of grotere adresruimte. Als het bestaande subnet te klein is, deze als volgt verwijderen en een nieuwe maken zoals in het volgende opsommingsteken:

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
```

Als de VNet niet een subnet genaamd **GatewaySubnet**bevat, maakt u een nieuw als volgt:

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.224/27"
$vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="vpn-and-expressroute-gateways"></a>VPN- en ExpressRoute gateways

Controleer of uw organisatie voldoet aan de [vereisten voor ExpressRoute] [ expressroute-prereq] voor de verbinding met Azure.

Als u al een virtueel netwerk VPN-gateway in uw VNet Azure, verwijderen, zoals hieronder wordt weergegeven.

```powershell
Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
```

Volg de instructies bij de [uitvoering van een hybride netwerkarchitectuur met Azure ExpressRoute] [ implementing-expressroute] de ExpressRoute verbinding tot stand brengen.

Volg de instructies bij de [uitvoering van een hybride netwerkarchitectuur met Azure en VPN-On-premises] [ implementing-vpn] uw VPN virtueel netwerk gateway verbinding tot stand brengen.

Nadat u de virtuele gateway netwerkverbindingen tot stand hebt gebracht, het milieu als volgt testen:

1. Zorg ervoor dat kunt u in uw netwerk op ruimten aan uw VNet Azure.

2. Neem contact op met uw provider om te stoppen ExpressRoute connectiviteit testen.

3. Controleer of de kunt u nog steeds koppelen vanaf het netwerk in de lokalen voor uw gebruik van de VPN-netwerk van virtuele gateway verbinding Azure-VNet.

4. Neem contact op met uw provider reestabilish ExpressRoute connectiviteit.

## <a name="considerations"></a>Overwegingen met betrekking tot

Zie voor ExpressRoute overwegingen met betrekking tot de [uitvoering van een hybride netwerkarchitectuur met Azure ExpressRoute] [ guidance-expressroute] richtlijnen.

Zie de [implementatie van een hybride netwerkarchitectuur met Azure en VPN-On-premises] omwille van Site naar Site VPN-[ guidance-vpn] richtlijnen.

Zie [Microsoft cloud services en netwerkbeveiliging]voor algemene Azure beveiligingsoverwegingen[best-practices-security].

## <a name="solution-deployment"></a>Implementatie van oplossingen

Als u een bestaande op gebouwen infrastructuur reeds geconfigureerd met een netwerk geschikt toestel hebt, kunt u de referentiearchitectuur implementeren door de volgende stappen:

1. Rechts Klik op de onderstaande knop en selecteer een 'Open koppeling in nieuw tabblad' of 'Open koppeling in nieuw venster':  
[![Implementeren op Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn-er%2Fazuredeploy.json)

2. Wachten op de koppeling in de Azure portal te openen en vervolgens als volgt te werk: 
  - De naam van de **Resource** is al gedefinieerd in de parameterbestand, dus **Nieuw** te selecteren en voer `ra-hybrid-vpn-er-rg` in het tekstvak.
  - Selecteer de regio in de vervolgkeuzelijst **locatie** .
  - Bewerk de **Sjabloon basis-Uri** of de **Parameter basis-Uri** tekstvakken niet.
  - Bekijk de voorwaarden en bepalingen en klik op het selectievakje **dat ik ga akkoord met bovenstaande voorwaarden** .
  - Klik op de button **aankoop** .

3. Wacht totdat de installatie te voltooien.

4. Rechts Klik op de onderstaande knop en selecteer een 'Open koppeling in nieuw tabblad' of 'Open koppeling in nieuw venster':  
[![Implementeren op Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn-er%2Fazuredeploy-expressRouteCircuit.json)

5. Wachten op de link te openen in de portal voor Azure Voer vervolgens als volgt te werk: 
  - Selecteer **bestaande gebruiken** in de sectie **groep van bronnen** en voer `ra-hybrid-vpn-er-rg` in het tekstvak.
  - Selecteer de regio in de vervolgkeuzelijst **locatie** .
  - Bewerk de **Sjabloon basis-Uri** of de **Parameter basis-Uri** tekstvakken niet.
  - Bekijk de voorwaarden en bepalingen en klik op het selectievakje **dat ik ga akkoord met bovenstaande voorwaarden** .
  - Klik op de button **aankoop** .

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[vpn-appliance]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[azure-vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[azure-network-security-group]: ../virtual-network/virtual-networks-nsg.md
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[expressroute-prereq]: ../expressroute/expressroute-prerequisites.md
[implementing-expressroute]: ./guidance-hybrid-network-expressroute.md#implementing-this-architecture
[implementing-vpn]: ./guidance-hybrid-network-vpn.md#implementing-this-architecture
[guidance-expressroute]: ./guidance-hybrid-network-expressroute.md
[guidance-vpn]: ./guidance-hybrid-network-vpn.md
[best-practices-security]: ../best-practices-network-security.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/deploy-reference-architecture.sh
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetwork.parameters.json
[virtualnetworkgateway-vpn-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetworkGateway-vpn.parameters.json
[virtualnetworkgateway-expressroute-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetworkGateway-expressRoute.parameters.json
[er-circuit-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/expressRouteCircuit.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[naming conventions]: ./guidance-naming-conventions.md
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[0]: ./media/blueprints/hybrid-network-expressroute-vpn-failover.png "Architectuur van een maximaal beschikbare hybride netwerkarchitectuur met ExpressRoute en VPN-gateway"
[ARM-Templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/
