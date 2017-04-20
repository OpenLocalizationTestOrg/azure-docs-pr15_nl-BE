<properties
   pageTitle="ExpressRoute probleemoplossingsgids: ARP tabellen ophalen | Microsoft Azure"
   description="Deze pagina bevat instructies voor het ophalen van het ARP tabellen voor een ExpressRoute circuit."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carolz"
   editor="tysonn"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr"/>

# <a name="expressroute-troubleshooting-guide-getting-arp-tables-in-the-classic-deployment-model"></a>ExpressRoute probleemoplossingsgids: ARP tabellen ophalen in het implementatiemodel klassiek

> [AZURE.SELECTOR]
[PowerShell - Resource Manager](expressroute-troubleshooting-arp-resource-manager.md)
[PowerShell - klassiek](expressroute-troubleshooting-arp-classic.md)

Dit artikel begeleidt u door de stappen voor het verkrijgen van de tabellen Address Resolution Protocol (ARP) voor uw Azure ExpressRoute circuit.

>[AZURE.IMPORTANT] Dit document is bedoeld om u te helpen u bij het vaststellen en oplossen van eenvoudige problemen. Het is niet bedoeld ter vervanging voor de ondersteuning van Microsoft. Als u het probleem niet kunt oplossen met behulp van de volgende richtlijnen, open een verzoek voor ondersteuning door [Microsoft Azure Help + ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>ARP-Resolution Protocol (ARP) en adres
ARP is een Layer 2-protocol dat wordt gedefinieerd in [RFC 826](https://tools.ietf.org/html/rfc826). ARP wordt gebruikt om een Ethernet-adres (MAC-adres) toegewezen aan een IP-adres.

Een ARP-tabel bevat een toewijzing van een IPv4-adres en MAC-adres voor een bepaalde peering. De ARP-tabel voor een peering ExpressRoute circuit biedt de volgende informatie voor elke interface (primaire en secundaire):

1. Toewijzing van een op ruimten interface IP-routeradres een MAC-adres
2. Toewijzing van een ExpressRoute interface IP-routeradres een MAC-adres
3. De leeftijd van de toewijzing

ARP-tabellen kunnen helpen met valideren van Layer 2-configuratie en het oplossen van elementaire problemen met Layer 2.

Hier volgt een voorbeeld van een ARP-tabel:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


De volgende sectie bevat informatie over het weergeven van de ARP-tabellen die door de routers ExpressRoute rand zichtbaar zijn.

## <a name="prerequisites-for-using-arp-tables"></a>Vereisten voor het gebruik van tabellen met ARP

Zorg ervoor dat het volgende voordat u verdergaat:

 - Een geldige ExpressRoute circuit dat geconfigureerd met ten minste één peering. Het circuit moet volledig worden geconfigureerd door de provider connectiviteit. U (of uw provider verbinding) moet ten minste één van de peerings (Azure private, Azure publiek of Microsoft) op configureren dit circuit.

 - IP-adresbereiken die worden gebruikt voor het configureren van de peerings (Azure private, Azure publiek en Microsoft). Bekijk de voorbeelden IP adres toewijzing in de [ExpressRoute routing pagina met systeemvereisten](expressroute-routing.md) voor een goed begrip van hoe IP-adressen zijn toegewezen aan interfaces op de aise en aan de kant van de ExpressRoute. U krijgt informatie over de configuratie van de peering aan de hand van de [configuratiepagina van ExpressRoute peering](expressroute-howto-routing-classic.md).

 - Informatie van uw netwerk team of connectiviteit provider over de MAC-adressen van de interfaces die met deze IP-adressen worden gebruikt.

 - De meest recente Windows PowerShell-module voor Azure (1,50 of een latere versie).

## <a name="arp-tables-for-your-expressroute-circuit"></a>ARP-tabellen voor uw ExpressRoute circuit
Deze sectie bevat instructies over het weergeven van de ARP-tabellen voor elk type peering met PowerShell. Voordat u verdergaat, moet u of uw provider verbinding met de peering configureren. Elk circuit heeft twee paden (primaire en secundaire). U kunt de ARP-tabel voor elk pad afzonderlijk controleren.

### <a name="arp-tables-for-azure-private-peering"></a>ARP-tabellen voor Azure private peering
De volgende cmdlet voorziet het ARP tabellen Azure private peering:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure private peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Primary

        # ARP table for Azure private peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Secondary

Dit is een voorbeeld van uitvoer voor een van de paden:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>ARP-tabellen voor openbare peering Azure:
De volgende cmdlet biedt het ARP tabellen voor openbare peering Azure:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure public peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Primary

        # ARP table for Azure public peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Secondary

Dit is een voorbeeld van uitvoer voor een van de paden:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Dit is een voorbeeld van uitvoer voor een van de paden:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>ARP-tabellen voor Microsoft peering
De volgende cmdlet biedt het ARP tabellen voor peering van Microsoft:

    # ARP table for Microsoft peering--primary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Primary

    # ARP table for Microsoft peering--secondary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Secondary


Voorbeeld van de uitvoer hieronder vindt u een van de paden:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Het gebruik van deze informatie
De ARP-tabel van een peering kan worden gebruikt voor het valideren van Layer 2-configuratie en de verbindingen. Deze sectie bevat een overzicht van de ARP-tabellen als volgt in verschillende scenario's uitzien.

### <a name="arp-table-when-a-circuit-is-in-an-operational-expected-state"></a>ARP-tabel als een circuit een operationele is (verwacht)

 - De ARP-tabel heeft een vermelding voor de kant op bedrijven met een geldig IP- en MAC-adres en een vergelijkbare vermelding voor de kant van Microsoft.
 - Het laatste octet van het IP-adres van op het bedrijf is altijd een oneven getal.
 - Het laatste octet van het Microsoft-IP-adres is altijd een even getal.
 - Hetzelfde MAC-adres wordt weergegeven voor alle drie peerings (primaire en secundaire) bij Microsoft.


        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-its-on-premises-or-when-the-connectivity-provider-side-has-problems"></a>ARP-tabel wanneer in ruimten of wanneer de zijkant connectiviteit-provider problemen heeft

 Slechts één vermelding in de ARP-tabel wordt weergegeven. Het bevat de toewijzing van het MAC-adres het IP-adres dat wordt gebruikt bij Microsoft.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

>[AZURE.NOTE] Als er een probleem is als volgt, open een verzoek voor ondersteuning bij uw provider verbinding oplossen.


### <a name="arp-table-when-the-microsoft-side-has-problems"></a>ARP-tabel wanneer u de Microsoft kant heeft problemen

 - Een ARP-tabel weergegeven voor een peering als er problemen bij Microsoft zijn wordt niet weergegeven.
 -  Open een verzoek om ondersteuning door [Microsoft Azure Help + ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Opgeven dat u een probleem met laag 2-connectiviteit.

## <a name="next-steps"></a>Volgende stappen

 - Layer 3-configuraties voor uw ExpressRoute circuit valideren:
     - Krijg een route overzicht om te bepalen wat de status van BGP sessies.
     - Een tabel om te bepalen welke prefixen aangegeven via ExpressRoute ophalen.
 - Valideren van gegevens aan de hand van bytes in-en uitchecken.
 - Open een verzoek om ondersteuning door [Microsoft Azure Help + ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) als u nog steeds problemen ondervindt.
