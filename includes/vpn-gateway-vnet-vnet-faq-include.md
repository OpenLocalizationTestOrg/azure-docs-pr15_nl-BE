- De virtuele netwerken kunnen worden in dezelfde of een andere Azure regio's (locatie).

- Een cloud-service of een eindpunt voor taakverdeling niet kan overspannen virtuele netwerken, zelfs als ze met elkaar zijn verbonden.

- Meerdere Azure virtuele netwerken aan elkaar koppelen nodig niet alle gateways op ruimten VPN tenzij cross-premises-connectiviteit vereist is.

- VNet-VNet-ondersteunt verbinden van virtuele netwerken. Het ondersteunt aansluitende virtuele machines of cloud services niet in een virtueel netwerk.

- VNet-VNet-gateways Azure VPN vereist met RouteBased (voorheen dynamische routering) VPN-typen. 

- Verbinding met het virtuele netwerk gelijktijdig kan worden gebruikt met meerdere site VPN-verbindingen, met een maximum van 10 (standaard/standaard-Gateways) of 30 (hoge prestaties Gateways) VPN tunnels voor een virtueel netwerk VPN-gateway verbinding maken met andere virtuele netwerken of sites ruimten.

- De adresruimte van de virtuele netwerken en op ruimten lokale netwerksites moeten niet overlappen. Overlappende adresruimten leidt ertoe dat het maken van VNet-VNet-verbindingen worden verbroken.

- Redundante tunnels tussen twee virtuele netwerken worden niet ondersteund.

- Alle VPN-tunnels van het virtuele netwerk delen de beschikbare bandbreedte op de Azure VPN-gateway en de dezelfde VPN-gateway uptime SLA in Azure.

- VNet-VNet-verkeer gaat via het Microsoft Network, niet op het Internet.

- VNet-VNet-verkeer binnen dezelfde regio is gratis voor beide richtingen; cross-regio VNet-VNet-egress is verkeer belast met de uitgaande inter-VNet-gegevensoverdrachtsnelheden op basis van de bron-regio's. Zie de [prijzen pagina](https://azure.microsoft.com/pricing/details/vpn-gateway/) voor meer informatie.