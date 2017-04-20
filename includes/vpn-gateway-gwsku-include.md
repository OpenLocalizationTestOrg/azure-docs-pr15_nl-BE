Wanneer u een gateway virtueel netwerk maakt, moet u opgeven van de gateway SKU die u wilt gebruiken. Wanneer u een hogere SKU-gateway selecteert, meer CPU's en bandbreedte van het netwerk worden toegewezen aan de gateway en daardoor de gateway hogere netwerkdoorvoer met het virtuele netwerk kan ondersteunen.

VPN-Gateway kunt gebruiken de volgende SKU's:

- Basic
- Standaard
- HighPerformance

Bij het selecteren van een SKU, rekening met het volgende:

- Als u wilt een PolicyBased VPN-type gebruiken, moet u de elementaire SKU. PolicyBased VPN's (voorheen statische routering) worden niet ondersteund op een andere SKU.
- BGP is niet ondersteund op de elementaire SKU.
- Naast ExpressRoute VPN-Gateway configuraties worden niet ondersteund op de elementaire SKU.
- Actieve S2S VPN-Gateway-verbindingen kunnen worden geconfigureerd op de SKU HighPerformance alleen.
