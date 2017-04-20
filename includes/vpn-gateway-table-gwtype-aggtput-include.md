|    | **Gateway VPN-doorvoer (1)** | **Max IPsec VPN-Gateway tunnels (2)** | **ExpressRoute Gateway doorvoer** | **VPN-Gateway en ExpressRoute worden geïnstalleerd**|
|--- |----------------------------|-----------------------------------|-------------------------------------|-----------------------------------------|
| **Basic SKU (3)(5)**              |  100 Mbps | 10                         |  500 Mbps                           | Nee   |
| **Standaard SKU (4)(5)**           |  100 Mbps | 10                         | 1000 Mbps                           | Ja  |
| **Krachtige SKU (4)**   | 200 Mbps  | 30                         | 2000 Mbps                           | Ja  |

- (1) de VPN-doorvoer is een ruwe schatting op basis van de metingen tussen VNets in dezelfde regio Azure. Het is niet een gegarandeerde doorvoer voor cross-premises verbindingen via het Internet. De meting van de maximale doorvoer van mogelijk is.
- (2) het aantal tunnels Zie VPN's RouteBased. Een VPN PolicyBased ondersteunt slechts één Site naar Site VPN-tunnel.
- (3) BGP wordt niet ondersteund voor de elementaire SKU.
- (4) PolicyBased VPN's worden niet ondersteund voor deze SKU. Zij worden ondersteund voor alleen de elementaire SKU.
- (5) actieve S2S VPN-Gateway-verbindingen worden niet ondersteund voor deze SKU. Actieve wordt op de SKU HighPerformance alleen ondersteund.