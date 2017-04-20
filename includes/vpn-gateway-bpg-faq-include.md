### <a name="is-bgp-supported-on-all-azure-vpn-gateway-skus"></a>BGP is wordt ondersteund op alle Azure VPN-Gateway SKU's?

Nee, BGP wordt ondersteund voor VPN-Azure gateways voor **standaard** - en **HighPerformance** . **Basic** SKU wordt niet ondersteund.

### <a name="can-i-use-bgp-with-azure-policy-based-vpn-gateways"></a>Kan ik met Azure Policy-Based VPN-gateways BGP gebruiken?

Nee, BGP op Route-gebaseerde VPN-gateways alleen wordt ondersteund.

### <a name="can-i-use-private-asns-autonomous-system-numbers"></a>Kan ik persoonlijke ASN's (autonoom systeem nummers) gebruiken?

Ja, kunt u uw eigen openbare ASN's of persoonlijke ASN's voor uw netwerken op gebouwen en de Azure virtuele netwerken.

#### <a name="are-there-asns-reserved-by-azure"></a>Zijn er gereserveerd door Azure ASN's?

Ja, de volgende ASN's zijn gereserveerd voor Azure voor interne en externe peerings:

- Openbare ASN's: 8075, 8076, 12076
- Particuliere ASN's: 65515, 65517, 65518, 65519, 65520

U kunt deze ASN's opgeven voor uw op locatie VPN-apparaten verbinding met de VPN-Azure gateways.

### <a name="can-i-use-the-same-asn-for-both-on-premises-vpn-networks-and-azure-vnets"></a>Kan ik de dezelfde ASN gebruiken voor VPN-netwerken op gebouwen en Azure-VNets

Nee, moet u andere ASN's tussen de netwerken op gebouwen en uw Azure VNets toewijzen als u deze verbinding met BGP. Azure VPN-Gateways zijn standaard ASN van 65515 is toegewezen, of BGP is ingeschakeld voor niet voor de verbindingen van de verschillende ruimten. U kunt deze standaardinstelling negeren door een andere ASN toewijzen bij het maken van de VPN-gateway of de ASN wijzigen nadat de gateway is gemaakt. U moet uw op ruimten ASN's toewijzen aan de bijbehorende Azure lokale netwerkgateways.

### <a name="what-address-prefixes-will-azure-vpn-gateways-advertise-to-me"></a>Welke adresprefixen wordt Azure VPN-gateways adverteren voor mij?

Azure VPN-gateway, wordt de volgende routes naar uw apparaten op ruimten BGP adverteren:

- De VNet-adresprefixen
- Adresprefixen voor elk lokaal netwerkgateways wordt verbonden met de Azure VPN-gateway
- Routes die van andere BGP peering sessies verbonden met Azure VPN-gateway, **behalve de standaardroute of routes die overlapt met een voorvoegsel VNet**.

#### <a name="can-i-advertise-default-route-00000-to-azure-vpn-gateways"></a>Kan ik een standaardroute (0.0.0.0/0), naar Azure VPN-gateways adverteren?

Niet op dit moment.

#### <a name="can-i-advertise-the-exact-prefixes-as-my-virtual-network-prefixes"></a>Kan ik de exacte voorvoegsels adverteren als mijn voorvoegsels virtueel netwerk

Nee, dezelfde prefixen als het virtuele netwerk reclame-adresprefixen worden geblokkeerd of gefilterd door de Azure platform. U kunt echter een voorvoegsel dat is een superset van wat u hebt het virtuele netwerk aangekondigd. Bijvoorbeeld, het virtuele netwerk het adres ruimte 10.10.0.0/16 kan gebruiken en u kan 10.0.0.0/8 adverteren.

### <a name="can-i-use-bgp-with-my-vnet-to-vnet-connections"></a>Kan ik mijn VNet-VNet-verbindingen met BGP gebruiken?

Ja, u kunt BGP gebruiken voor verbindingen met meerdere gebouwen en VNet-VNet-verbindingen.

### <a name="can-i-mix-bgp-with-non-bgp-connections-for-my-azure-vpn-gateways"></a>Kan ik BGP meng met niet-BGP verbindingen voor mijn Azure VPN-gateways?

Ja, kunt u zowel BGP niet BGP verbindingen voor de dezelfde Azure VPN-gateway mengen.

### <a name="does-azure-vpn-gateway-support-bgp-transit-routing"></a>Is Azure VPN-ondersteuning BGP transit gatewayrouting?

Ja, BGP transit routering wordt ondersteund, met de uitzondering dat Azure VPN-gateways wordt **niet** standaardroutes aan andere peers BGP adverteren. U moet BGP zodat doorvoer via meerdere gateways voor Azure VPN-routering inschakelen op alle tussenliggende VNet-VNet-verbindingen.

### <a name="can-i-have-more-than-one-tunnels-between-azure-vpn-gateway-and-my-on-premises-network"></a>Kan ik meerdere tunnels tussen Azure VPN-gateway en mijn netwerk op ruimten hebben?

Ja, kunt u meer dan één S2S VPN-tunnels tussen een Azure VPN-gateway en het netwerk op gebouwen maken. Houd er rekening mee dat alle deze tunnels wordt afgeboekt op het totale aantal tunnels voor uw VPN-Azure gateways. Als er twee redundante tunnels tussen de Azure VPN-gateway en een van uw netwerk op lokalen, verbruiken ze bijvoorbeeld 2 tunnels van het totale contingent voor Azure VPN-gateway (standaard 10) en 30 voor HighPerformance.

### <a name="can-i-have-multiple-tunnels-between-two-azure-vnets-with-bgp"></a>Kan ik meerdere tunnels tussen twee Azure VNets met BGP hebben?

Nee, redundante tunnels tussen twee virtuele netwerken worden niet ondersteund.

### <a name="can-i-use-bgp-for-s2s-vpn-in-an-expressroutes2s-vpn-co-existence-configuration"></a>Kan ik BGP gebruiken voor VPN-S2S in de configuratie van een VPN-ExpressRoute/S2S coëxistentie?

Niet op dit moment.

### <a name="what-address-does-azure-vpn-gateway-use-for-bgp-peer-ip"></a>Welk adres gebruikt Azure VPN-gateway voor BGP Peer IP?

De Azure VPN-gateway wordt één IP-adres toewijzen uit het bereik GatewaySubnet is gedefinieerd voor het virtuele netwerk. Standaard is de tweede laatste adres van het bereik. Bijvoorbeeld, als uw GatewaySubnet 10.12.255.0/27, variërend van 10.12.255.0 tot 10.12.255.31, vervolgens het BGP Peer-IP-adres op de Azure VPN-gateway worden 10.12.255.30. U kunt deze informatie vinden wanneer je de informatie Azure VPN-gateway.

### <a name="what-are-the-requirements-for-the-bgp-peer-ip-addresses-on-my-vpn-device"></a>Wat zijn de vereisten voor het BGP Peer-IP-adressen op Mijn VPN-apparaat?

Het BGP op ruimten adres van peer **Mogen niet** hetzelfde zijn als het openbare IP-adres van uw VPN-apparaat. Gebruik een ander IP-adres op het VPN-apparaat voor de Peer BGP IP. Het kan zijn dat een adres toegewezen aan de loopback-interface op het apparaat. Dit adres in de bijbehorende lokale netwerk-Gateway voor de locatie opgeven.

### <a name="what-should-i-specify-as-my-address-prefixes-for-the-local-network-gateway-when-i-use-bgp"></a>Wat moet ik opgeven als mijn adresprefixen voor het lokale netwerkgateway wanneer BGP?

Azure lokale netwerkgateway geeft de eerste adresprefixen voor het netwerk op gebouwen. Met BGP, moet u het voorvoegsel voor de host toewijzen (/ 32 voorvoegsel) van uw BGP Peer-IP-adres als de adresruimte voor het netwerk op gebouwen. Als de Peer BGP IP 10.52.255.254 is, moet u "10.52.255.254/32" Als de localNetworkAddressSpace van het lokale netwerkgateway die dit netwerk op gebouwen. Dit is om ervoor te zorgen dat de Azure VPN-gateway het BGP-sessie via de S2S VPN-tunnel tot stand brengt.

### <a name="what-should-i-add-to-my-on-premises-vpn-device-for-the-bgp-peering-session"></a>Wat moet ik toevoegen aan mijn op ruimten VPN-apparaat voor de peering BGP-sessie?

U moet een hostroute het Azure BGP Peer-IP-adres op het VPN-apparaat aan te wijzen de IPSec-S2S VPN-tunnel toevoegen. Als de IP Azure VPN-Peer '10.12.255.30' is, moet u een hostroute voor '10.12.255.30' met een nexthop-interface van de overeenkomende IPSec-tunnelinterface toevoegen op het VPN-apparaat.
