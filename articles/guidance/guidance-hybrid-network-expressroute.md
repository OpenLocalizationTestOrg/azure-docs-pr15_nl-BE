<properties
   pageTitle="Implementatie van een hybride netwerkarchitectuur met Azure ExpressRoute | Verwijzen naar architectuur | Microsoft Azure"
   description="Het implementeren van een veilig netwerk van site naar site architectuur die een Azure virtual network en een in de lokalen netwerk verbonden via Azure ExpressRoute omvat."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-hybrid-network-architecture-with-azure-expressroute"></a>Implementatie van een hybride netwerkarchitectuur met Azure ExpressRoute

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dit artikel bevat verschillende aanbevelingen voor de verbinding met een netwerk op ruimten met behulp van ExpressRoute virtuele netwerken op Azure. ExpressRoute verbindingen zijn gemaakt met persoonlijke gereserveerde verbinding via een derde verbinding provider. De particuliere verbinding breidt het netwerk op gebouwen in Azure toegang bieden tot uw eigen infrastructuur IaaS in Azure, openbare eindpunten in PaaS, en Office365 SaaS-diensten gebruikt. Dit document ligt de nadruk op een verbinding maakt met een enkele Azure virtual network (VNet) met behulp van zogenaamde private peering via ExpressRoute.

> [AZURE.NOTE] Azure heeft twee verschillende modellen: [Resource Manager] [ resource-manager-overview] en klassiek. Deze blauwdruk gebruikt resourcemanager, wordt aangeraden voor nieuwe implementaties.

Typische use-cases voor deze architectuur zijn:

- Hybride toepassingen waarbij werklasten worden verdeeld tussen een netwerk op gebouwen en Azure.

- Toepassingen van grootschalige, kritieke werklast die een hoge mate van schaalbaarheid vereisen.

- Grootschalige maken en terugzetten van back-up faciliteiten voor gegevens die op een externe locatie moet worden opgeslagen.

- Grote gegevens werklasten verwerken.

- Met behulp van Azure als een disaster recovery-site.

> [AZURE.NOTE] Het [technisch overzicht van ExpressRoute] [ expressroute-technical-overview] bevat een inleiding tot ExpressRoute.

## <a name="architecture-diagram"></a>Architectuurdiagram

In het volgende diagram worden de belangrijke onderdelen van deze architectuur:

> Een Visio-document met dit Architectuurdiagram is beschikbaar op het [Microsoft download center][visio-download]. Dit diagram is op de pagina 'Hybride network - ER'.

![[0]][0]

- **Azure virtuele netwerken (VNets).** Elk VNet bevindt zich in een bepaalde regio Azure en meerdere lagen kunt onderbrengen. Lagen kunnen worden gesegmenteerd subnetten met in elke VNet en/of netwerk-beveiligingsgroepen (NSGs). 

- **Azure openbare diensten.** Dit zijn de Azure services die kunnen worden gebruikt binnen een toepassing hybride. Deze services zijn ook beschikbaar via het openbare Internet, maar deze toegang kunt krijgen via een ExpressRoute circuit biedt lage latentie en voorspelbare prestaties omdat verkeer via het Internet gaat. Verbindingen via **openbare peering**, met adressen die eigendom is van uw organisatie of verstrekt door uw provider verbinding worden uitgevoerd. 

- **Services voor Office 365.** Dit zijn de openbaar beschikbare Office 365-toepassingen en services van Microsoft. Verbindingen worden uitgevoerd via **Microsoft peering**, met adressen die eigendom is van uw organisatie of door uw provider verbinding geleverd worden.

>[AZURE.NOTE] U kunt ook rechtstreeks naar Microsoft CRM Online via een Microsoft peering verbinden.

- **Het bedrijfsnetwerk op gebouwen.** Dit is een netwerk van computers en apparaten, verbonden via een particuliere LAN-netwerk binnen een organisatie worden uitgevoerd.

- **Rand van lokale routers.** Dit zijn routers die het netwerk op ruimten met het circuit dat wordt beheerd door de provider verbinden. Afhankelijk van hoe de verbinding wordt ingericht, moet u het openbare IP-adressen die door de routers worden gebruikt. 

- **Microsoft edge routers.** Deze zijn twee routers in de configuratie van een actieve maximaal beschikbaar. Deze routers inschakelen een verbinding provider hun circuits rechtstreeks verbinden met hun datacenter. Afhankelijk van hoe de verbinding wordt ingericht, moet u het openbare IP-adressen die door de routers worden gebruikt.

- **ExpressRoute circuit.** Dit is een laag 2 of layer 3-circuit geleverd door de provider verbinding met dat netwerk joins in ruimten met Azure via de routers van de rand. Het circuit wordt gebruikt voor de hardware infrastructuur beheerd door de provider connectiviteit.

- **Providers connectiviteit.** Dit zijn bedrijven die een verbinding via beide laag 2 of layer 3-verbinding tussen uw datacenter en een Azure datacenter.

## <a name="recommendations"></a>Aanbevelingen

Azure biedt veel verschillende bronnen en brontypen, zodat deze referentiearchitectuur kan worden ingericht veel verschillende manieren. We hebben een sjabloon Azure Resource Manager wilt installeren de referentiearchitectuur die volgt op deze aanbevelingen verstrekt. Als u kiest voor het maken van uw eigen referentiearchitectuur Volg deze aanbevelingen tenzij u een specifieke vereiste is dat een aanbeveling niet past.

### <a name="connectivity-providers"></a>Providers connectiviteit

Selecteer een geschikte ExpressRoute connectiviteit provider voor uw locatie. Gebruik de volgende Azure PowerShell-opdracht voor een lijst van verbindingen providers beschikbaar op uw locatie:

```powershell
Get-AzureRmExpressRouteServiceProvider
```

ExpressRoute connectiviteit providers verbinden uw datacenter met Microsoft op de volgende manieren:

- **Kan zich bevinden op een wolk exchange.** Als u kan zich in een ruimte met een exchange-wolk bevinden bent, kunt u virtuele cross-verbindingen naar de cloud met Microsoft via Ethernet-uitwisseling van de provider collocatie bestellen. Collocatie providers kunnen bieden voor Layer 2 cross-verbindingen of beheerde Layer 3 cross-verbindingen tussen uw infrastructuur in collocatie inrichting en de Microsoft cloud...

- **Point-to-Point via Ethernet-verbindingen.** Naar de cloud met Microsoft via point-to-point Ethernet-verbindingen kunt u uw datacenters op gebouwen/kantoren. Point-to-Point Ethernet-providers bieden voor Layer 2-verbindingen of managed Layer 3-verbindingen tussen uw site en de Microsoft cloud.

- **Any-to-any netwerken (IPVPN).** U kunt uw WAN integreren met de Microsoft-cloud. IPVPN's (meestal MPLS VPN) bieden any-to-any connectiviteit tussen de filialen en datacenters. De Microsoft cloud kan onderling verbonden aan uw WAN zodat deze net als andere filiaal. WAN-aanbieders bieden gewoonlijk beheerde Layer 3-connectiviteit.

Zie voor meer informatie over providers connectiviteit [ExpressRoute circuits en routering domeinen][connectivity-providers].

### <a name="expressroute-circuit"></a>ExpressRoute circuit

Zorg ervoor dat uw organisatie voldoet aan de [vereisten van ExpressRoute] [ expressroute-prereqs] voor de verbinding met Azure.

Als u nog niet hebt gedaan, een subnet genaamd toevoegen `GatewaySubnet` op de Azure-VNet en maak een ExpressRoute virtueel netwerkgateway met de Azure VPN-Gateway service. Zie voor meer informatie over dit proces [ExpressRoute workflows voor circuit inrichten en Staten circuit][ExpressRoute-provisioning].

Een circuit ExpressRoute als volgt maken:

1. De volgende PowerShell-opdracht uitvoeren:

    ```powershell
    New-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>> -Location <<location>> -SkuTier <<sku-tier>> -SkuFamily <<sku-family>> -ServiceProviderName <<service-provider-name>> -PeeringLocation <<peering-location>> -BandwidthInMbps <<bandwidth-in-mbps>>
    ```

2. Verzendt het `ServiceKey` voor het nieuwe circuit met de serviceprovider.

3. Wacht totdat de provider het circuit inrichten. De inrichting staat van een circuit kunt u controleren met behulp van de volgende PowerShell-opdracht:

    ```powershell
    Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
    ```

De `Provisioning state` veld in de `Service Provider` sectie van de uitvoer wordt gewijzigd van `NotProvisioned` naar `Provisioned` wanneer het circuit is klaar.

>[AZURE.NOTE]Als u een layer 3-verbinding, te de provider configureren en beheren van routering. u bieden de informatie die nodig is opdat de voorziening voor het implementeren van de juiste routes.

Als u een layer 2-verbinding gebruikt, voert u de volgende stappen uit:

1. Reserveren twee/30 subnetten bestaat uit de geldige openbare IP-adressen voor elk type van peering die u wilt implementeren. Deze/30 subnetten om IP-adressen voor routers gebruikt voor het circuit wordt gebruikt. Als u particuliere, openbare en Microsoft peering implementeert, moet u de subnetten 6 /30 met geldige openbare IP-adressen.     

2. Voor het circuit ExpressRoute routering configureren. U moet de opdracht hieronder voor elk type van peering die u wilt configureren (particuliere, openbare en Microsoft).

    >[AZURE.NOTE]Zie [maken en pas bewerkingsplan voor een circuit ExpressRoute] [ configure-expressroute-routing] voor meer informatie. Gebruik de volgende PowerShell-opdrachten toe te voegen een netwerk peering voor het routeren van verkeer:

    ```powershell
    Set-AzureRmExpressRouteCircuitPeeringConfig -Name <<peering-name>> -Circuit <<circuit-name>> -PeeringType <<peering-type>> -PeerASN <<peer-asn>> -PrimaryPeerAddressPrefix <<primary-peer-address-prefix>> -SecondaryPeerAddressPrefix <<secondary-peer-address-prefix>> -VlanId <<vlan-id>>

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit <<circuit-name>>
    ```

3. Een andere groep van geldige openbare IP-adressen wilt gebruiken voor NAT voor publiek en Microsoft peering reserveren. Het verdient aanbeveling om een andere groep van toepassingen voor elke peering. Geef de groep met uw provider verbinding zodat ze advertenties voor die bereiken BGP kunnen configureren.

[Uw persoonlijke VNet(s) in de cloud een koppeling naar het circuit ExpressRoute][link-vnet-to-expressroute]. De volgende PowerShell-opdrachten gebruiken:

```
$circuit = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
$gw = Get-AzureRmVirtualNetworkGateway -Name <<gateway-name>> -ResourceGroupName <<resource-group>>
New-AzureRmVirtualNetworkGatewayConnection -Name <<connection-name>> -ResourceGroupName <<resource-group>> -Location <<location> -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute
```

Houd rekening met de volgende punten:

- Border Gateway Protocol (BGP) ExpressRoute gebruikt voor het uitwisselen van routeringsgegevens tussen uw netwerk en Azure.

- U kunt meerdere VNets die zich in verschillende regio's op hetzelfde circuit ExpressRoute als alle VNets en het circuit ExpressRoute bevinden zich in dezelfde regio geopolitieke verbinden.

## <a name="scalability-considerations"></a>Overwegingen voor schaalbaarheid

ExpressRoute circuits bieden een hoge bandbreedte pad tussen netwerken. In het algemeen, hoe hoger de bandbreedte hoe hoger de kosten. Hoewel sommige providers u de bandbreedte wijzigt kunt, moet dat u een eerste bandbreedte die uw behoeften wordt overschreden, en biedt ruimte voor groei kiezen. In het geval u vergroten bandbreedte in de toekomst wilt, wordt u verlaten met twee opties.

Vergroot de bandbreedte. Houd er rekening mee dat niet alle providers kunt u dynamisch doen. En die te maken hebben dat zoveel mogelijk moet worden vermeden. Maar indien nodig na controle van uw provider, voert u de onderstaande opdrachten.

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
$ckt.ServiceProviderProperties.BandwidthInMbps = <<bandwidth-in-mbps>>
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

U kunt de bandbreedte zonder verlies van connectiviteit te verhogen. Downgraden van de bandbreedte leidt tot verstoringen in connectiviteit. U moet het circuit te verwijderen en opnieuw te maken met de nieuwe configuratie.

Als u de standaard SKU ExpressRoute en de noodzaak om te upgraden naar een Premium of wijzigen als u uw prijzen echt plan (gemeten of onbeperkt), voert u de onderstaande opdrachten. De `Sku.Tier` voor de eigenschap zijn `Standard` of `Premium`; de `Sku.Name` voor de eigenschap zijn `MeteredData` of `UnlimitedData`.

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>

$ckt.Sku.Tier = "Premium"
$ckt.Sku.Family = "MeteredData"
$ckt.Sku.Name = "Premium_MeteredData"

 Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

>[AZURE.IMPORTANT] Zorg ervoor dat de `Sku.Name` de eigenschap komt overeen met de `Sku.Tier` en `Sku.Family`. Als u de familie en laag, maar niet de naam wijzigt, wordt de verbinding uitgeschakeld.

U kunt de SKU zonder onderbrekingen bijwerken, maar u kunt niet overschakelen van de prijzen unlimited-plan te worden gemeten met behulp. Wanneer de SKU downgraden, moet uw bandbreedtegebruik blijven binnen de grenzen van de standaard van de standaard SKU.

> [AZURE.NOTE] ExpressRoute biedt twee prijzen plannen aan klanten, op basis van meting of onbeperkt. [ExpressRoute prijzen] Zie[ expressroute-pricing] voor meer informatie. Toeslagen variëren afhankelijk van de bandbreedte circuit. Beschikbare bandbreedte variëren provider provider waarschijnlijk. Gebruik de `Get-AzureRmExpressRouteServiceProvider` cmdlet voor een overzicht van de beschikbare providers in uw regio en de bandbreedten die ze aanbieden.

Een enkele ExpressRoute circuit kan een aantal peerings en VNet koppelingen ondersteunen. [ExpressRoute grenzen] Zie[ expressroute-limits] voor meer informatie.

Premium-invoegtoepassing ExpressRoute biedt voor een extra toeslag:

- Maximaal 10.000 routes per circuit. De limiet is zonder invoegtoepassing ExpressRoute Premium, 4000 routes per circuit.

- Globale connectiviteit, waardoor een ExpressRoute circuit zich overal ter wereld voor toegang tot bronnen in elke regio in plaats van alleen regio's in het hetzelfde continent.

- Een toename in het aantal koppelingen per circuit van 10 voor een grotere, afhankelijk van de bandbreedte van het circuit VNet.

ExpressRoute circuits zijn ontworpen om tijdelijk netwerk bursts tot tweemaal de bandbreedte die u zonder extra kosten hebt aangeschaft. Dit wordt bereikt met behulp van redundante verbindingen. Echter ondersteunen niet alle verbindingen-mailproviders deze functie; Controleer of dat uw provider verbinding schakelt u deze functie voordat u afhankelijk van het.

## <a name="availability-considerations"></a>Overwegingen met betrekking tot beschikbaarheid

U kunt voor uw verbinding Azure hoge beschikbaarheid op verschillende manieren, afhankelijk van het type van de provider die u gebruikt en het aantal ExpressRoute circuits en virtuele gateway netwerkverbindingen u bent configureren. De volgende punten samenvatten uw beschikbaarheidopties:

- ExpressRoute ondersteunt geen router redundancy protocollen zoals HSRP en VRRP hoge beschikbaarheid implementeren. In plaats daarvan wordt een redundante paar BGP sessies per peering gebruikt. Ter vergemakkelijking van de maximaal beschikbare verbindingen met uw netwerk bepalingen Microsoft Azure u met twee redundante poorten op de twee routers (onderdeel van de rand van Microsoft) in een actieve configuratie.

- Als u een layer 2-verbinding, implementatie van redundante routers in het netwerk op de ruimten in een actieve configuratie. Het primaire circuit verbinden met één router en het secundaire circuit naar het andere. Hierdoor krijgt u een uiterst beschikbare verbinding aan beide uiteinden van de verbinding. Dit is nodig als u de SLA ExpressRoute nodig. [SLA voor Azure ExpressRoute] Zie[ sla-for-expressroute] voor meer informatie.

Het volgende diagram wordt getoond in een configuratie met redundante op ruimten routers aangesloten op de primaire en secundaire circuits. Elk circuit het verkeer voor een peering openbare en private peering verwerkt (een paar /30 elke peering is aangewezen adres ruimten, zoals beschreven in de vorige sectie).

![[1]][1]

Als u een layer 3-verbinding, controleert u of hieraan een redundante BGP sessies die beschikbaar voor u verwerken.

Virtuele netwerken kunnen worden aangesloten op meerdere ExpressRoute circuits en elke circuits kunnen worden geleverd door de verschillende dienstverleners. Deze strategie biedt extra hoge beschikbaarheid en noodherstel herstelfuncties.

Een VPN website als een failover-pad voor ExpressRoute configureren. Dit is alleen van toepassing op particuliere peering. Het Internet is voor Office 365- en Azure, het pad alleen failover.

BGP sessies gebruiken standaard een waarde van time-out voor inactiviteit van 60 seconden. De route is gemarkeerd als niet beschikbaar wanneer een sessie een time-out van 3 keer is, en al het verkeer wordt omgeleid naar de resterende router. Deze 180 seconden time-out is mogelijk te lang voor kritieke toepassingen. In dat geval kunt u uw BGP time-outinstellingen van de router op locatie op een kleinere waarde.

## <a name="manageability-considerations"></a>Overwegingen bij het beheer

U kunt de [Azure connectiviteit Toolkit (AzureCT)] [ azurect] voor het controleren van verbindingen tussen uw datacenter op gebouwen en Azure.

## <a name="security-considerations"></a>Beveiligingsoverwegingen

U kunt beveiligingsopties voor uw verbinding Azure op verschillende manieren, afhankelijk van uw veiligheid en behoeften voldoen. De volgende punten vatten de beveiligingsopties.

ExpressRoute werkt op laag 3. Bedreigingen in de Application-laag kunnen worden voorkomen met behulp van een netwerk Security-toestel dat verkeer wordt beperkt tot betrouwbare bronnen. Bovendien kunnen ExpressRoute verbindingen met openbare peering alleen worden gestart op ruimten. Hiermee voorkomt u dat een rogue-service toegang krijgen tot en verlies van gegevens uit het openbare Internet op ruimten.

Voeg beveiliging netwerkapparaten tussen het netwerk van gebouwen en de rand provider routers voor optimale beveiliging. Dit helpt te voorkomen dat de instroom van ongeoorloofde de VNet:

![[2]][2]

Ter controle of conformiteit mogelijk directe toegang verbieden uit onderdelen die in de VNet met het Internet worden uitgevoerd en implementeren van [tunneling gedwongen][forced-tuneling]. In dit geval wordt Internet-verkeer omgeleid terug via een proxy die wordt uitgevoerd op de lokalen waar kan worden gecontroleerd. De proxy kan worden geconfigureerd om ongeoorloofde die voortvloeien uit blokkeren en filteren van inkomend verkeer mogelijk schadelijke.

![[3]][3]

Standaard worden blootgesteld Azure VMs eindpunten die wordt gebruikt voor de aanmeldingstoegang ten behoeve van - RDP en Remote Powershell voor Windows VMs en SSH voor Linux-gebaseerde VMs wanneer via de portal Azure geïmplementeerd. Voor optimale beveiliging niet inschakelen van een openbaar IP-adres voor uw VMs en NSGs gebruiken om ervoor te zorgen dat deze VMs zijn niet toegankelijk voor het publiek. VMs alleen zijn beschikbaar via het interne IP-adres. Deze adressen kunnen toegankelijk via het netwerk inschakelen op gebouwen DevOps personeel voor het uitvoeren van de benodigde configuratie- of onderhoudsdiensten ExpressRoute gemaakt.

Als u moet management eindpunten voor VMs met een extern netwerk, gebruikt u NSGs en/of toegangsbeheerlijsten de zichtbaarheid van deze poorten beperken tot een "witte" lijst van IP-adressen of netwerken.

## <a name="troubleshooting-considerations"></a>Overwegingen bij het oplossen van problemen

Als u een eerder werkende ExpressRoute circuit nu geen verbinding kan maken, bij gebreke van iedere configuratie wijzigingen op-ruimte of binnen uw persoonlijke VNet, moet u contact op met de provider van connectivity en ermee werken om het probleem te verhelpen. U kunt ook de volgende Azure PowerShell-opdrachten gebruiken om bepaalde beperkte controles uitvoeren en nagaan waar problemen worden veroorzaakt door:

- Controleer of het circuit is ingericht:

```powershell
Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
```

De uitvoer van deze opdracht ziet u verschillende eigenschappen voor uw circuit, met inbegrip van `ProvisioningState`, `CircuitProvisioningState`, en `ServiceProviderProvisioningState` zoals hieronder wordt weergegeven.

```
ProvisioningState                : Succeeded
Sku                              : {
                                     "Name": "Standard_MeteredData",
                                     "Tier": "Standard",
                                     "Family": "MeteredData"
                                   }
CircuitProvisioningState         : Enabled
ServiceProviderProvisioningState : NotProvisioned
```

Als de `ProvisioningState` niet is ingesteld op `Succeeded` nadat u probeerde te maken van een nieuw circuit, het circuit met de volgende opdracht te verwijderen en probeer het opnieuw maken.

```powershell
Remove-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
```

Als uw provider al van het circuit voorzien was, en de `ProvisioningState` is ingesteld op `Failed`, of de `CircuitProvisioningState` niet `Enabled`, neem contact op met uw provider voor hulp.

## <a name="solution-deployment"></a>Implementatie van oplossingen

Als u een bestaande op gebouwen infrastructuur reeds geconfigureerd met een netwerk geschikt toestel hebt, kunt u de referentiearchitectuur implementeren door de volgende stappen:

Als u een bestaande op gebouwen infrastructuur reeds geconfigureerd met een VPN-apparaat hebt, kunt u de referentiearchitectuur implementeren door de volgende stappen:

1. Rechts Klik op de onderstaande knop en selecteer een 'Open koppeling in nieuw tabblad' of 'Open koppeling in nieuw venster':  
[![Implementeren op Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-er%2Fazuredeploy.json)

2. Wachten op de koppeling in de Azure portal te openen en vervolgens als volgt te werk: 
  - De naam van de **Resource** is al gedefinieerd in de parameterbestand, dus **Nieuw** te selecteren en voer `ra-hybrid-er-rg` in het tekstvak.
  - Selecteer de regio in de vervolgkeuzelijst **locatie** .
  - Bewerk de **Sjabloon basis-Uri** of de **Parameter basis-Uri** tekstvakken niet.
  - Bekijk de voorwaarden en bepalingen en klik op het selectievakje **dat ik ga akkoord met bovenstaande voorwaarden** .
  - Klik op de button **aankoop** .

3. Wacht totdat de installatie te voltooien.

4. Rechts Klik op de onderstaande knop en selecteer een 'Open koppeling in nieuw tabblad' of 'Open koppeling in nieuw venster':  
[![Implementeren op Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-er%2Fazuredeploy-expressRouteCircuit.json)

5. Wachten op de koppeling in de Azure portal te openen en vervolgens als volgt te werk: 
  - Selecteer **bestaande gebruiken** in de sectie **groep van bronnen** en voer `ra-hybrid-er-rg` in het tekstvak.
  - Selecteer de regio in de vervolgkeuzelijst **locatie** .
  - Bewerk de **Sjabloon basis-Uri** of de **Parameter basis-Uri** tekstvakken niet.
  - Bekijk de voorwaarden en bepalingen en klik op het selectievakje **dat ik ga akkoord met bovenstaande voorwaarden** .
  - Klik op de button **aankoop** .

6. Wacht totdat de installatie te voltooien.

## <a name="next-steps"></a>Volgende stappen

- [Implementatie van een maximaal beschikbare hybride netwerkarchitectuur] Zie[ highly-available-network-architecture] voor meer informatie over het verhogen van de beschikbaarheid van een hybride netwerk op basis van ExpressRoute worden niet via een VPN-verbinding.

<!-- links -->
[expressroute-technical-overview]: ../expressroute/expressroute-introduction.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[azure-powershell]: ../powershell-azure-resource-manager.md
[expressroute-prereqs]: ../expressroute/expressroute-prerequisites.md
[configure-expressroute-routing]: ../expressroute/expressroute-howto-routing-arm.md
[sla-for-expressroute]: https://azure.microsoft.com/support/legal/sla/expressroute/v1_0/
[link-vnet-to-expressroute]: ../expressroute/expressroute-howto-linkvnet-arm.md
[ExpressRoute-provisioning]: ../expressroute/expressroute-workflows.md
[expressroute-pricing]: https://azure.microsoft.com/pricing/details/expressroute/
[expressroute-limits]: ../azure-subscription-service-limits.md#networking-limits
[sample-script]: #sample-solution-script
[connectivity-providers]: ../expressroute/expressroute-introduction.md#how-can-i-connect-my-network-to-microsoft-using-expressroute
[azurect]: https://github.com/Azure/NetworkMonitoring/tree/master/AzureCT
[forced-tuneling]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[highly-available-network-architecture]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[arm-templates]: ../resource-group-authoring-templates.md
[naming-conventions]: ./guidance-naming-conventions.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/deploy-reference-architecture.sh
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/virtualNetwork.parameters.json
[virtualnetworkgateway-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/virtualNetworkGateway.parameters.json
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[er-circuit-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/expressRouteCircuit.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[0]: ./media/guidance-hybrid-network-expressroute/figure1.png "Hybride netwerkarchitectuur met Azure ExpressRoute"
[1]: ./media/guidance-hybrid-network-expressroute/figure2.png "Met behulp van redundante routers met de primaire en secundaire circuits ExpressRoute"
[2]: ./media/guidance-hybrid-network-expressroute/figure3.png "Beveiligingsapparatuur toe te voegen aan het netwerk van gebouwen"
[3]: ./media/guidance-hybrid-network-expressroute/figure4.png "Met behulp van gedwongen tunneling voor het controleren van de Internet-verkeer"
[4]: ./media/guidance-hybrid-network-expressroute/figure5.png "Zoeken naar de ServiceKey van een circuit ExpressRoute"
