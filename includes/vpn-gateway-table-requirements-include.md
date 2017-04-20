De volgende tabel worden de vereisten voor PolicyBased en RouteBased VPN-gateways. Deze tabel geldt voor de resourcemanager en de van klassieke implementatiemodellen. Het klassieke model PolicyBased VPN-gateways zijn hetzelfde als statische gateways en gateways op basis van een Route zijn hetzelfde als dynamische gateways.


|   | **PolicyBased Basic VPN-Gateway** | **RouteBased Basic VPN-Gateway** | **RouteBased standaard VPN-Gateway**   | **RouteBased High Performance VPN-Gateway** |
|---|---------------------------------------|---------------------------------------|----------------------------|----------------------------------|
|    **Verbinding van site naar Site (S2S)**  | PolicyBased VPN-configuratie        | RouteBased VPN-configuratie  | RouteBased VPN-configuratie     | RouteBased VPN-configuratie    |
| **Punt-naar-Site verbinding (P2S**)      | Niet ondersteund   | Ondersteund (kunnen naast S2S)  | Ondersteund (kunnen naast S2S)  | Ondersteund (kunnen naast S2S) |
| **Verificatiemethode**                 |    Vooraf-gedeelde sleutel  | Vooraf-gedeelde sleutel voor S2S connectiviteit, certificaten voor P2S connectiviteit | Vooraf-gedeelde sleutel voor S2S connectiviteit, certificaten voor P2S connectiviteit | Vooraf-gedeelde sleutel voor S2S connectiviteit, certificaten voor P2S connectiviteit |
| **Maximum aantal verbindingen S2S**       | 1                              | 10                                                                    | 10                                | 30                               |
| **Maximum aantal verbindingen P2S**       | Niet ondersteund                  | 128                                                                   | 128                               | 128                              |
|**Actieve ondersteuning voor routering (BGP)**           | Niet ondersteund                  | Niet ondersteund                                                         | Ondersteund                     | Ondersteund                   |
 
