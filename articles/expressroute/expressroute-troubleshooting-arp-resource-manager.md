<properties 
   pageTitle="ExpressRoute Troubleshooting Guide - ARP tabellen ophalen | Microsoft Azure"
   description="Deze pagina biedt instructies voor het ophalen van het ARP tabellen voor een ExpressRoute circuit"
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

#<a name="expressroute-troubleshooting-guide---getting-arp-tables-in-the-resource-manager-deployment-model"></a>ExpressRoute Troubleshooting guide - Getting ARP-tabellen in het implementatiemodel Resource Manager

> [AZURE.SELECTOR]
[PowerShell - Resource Manager](expressroute-troubleshooting-arp-resource-manager.md)
[PowerShell - klassiek](expressroute-troubleshooting-arp-classic.md)

Dit artikel begeleidt u door de stappen voor meer informatie over de ARP-tabellen voor uw ExpressRoute circuit. 

>[AZURE.IMPORTANT] Dit document is bedoeld om u te helpen u bij het vaststellen en oplossen van eenvoudige problemen. Het is niet bedoeld ter vervanging voor de ondersteuning van Microsoft. Als u niet het probleem met behulp van de hieronder beschreven richtlijnen op te lossen, moet u een support ticket openen met [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>ARP-Resolution Protocol (ARP) en adres
Address Resolution Protocol (ARP) is een layer 2-protocol dat is gedefinieerd in [RFC 826](https://tools.ietf.org/html/rfc826). ARP wordt gebruikt om de Ethernet-adres (MAC-adres) met een IP-adres toewijzen.

De ARP-tabel biedt een toewijzing van een IPv4-adres en MAC-adres voor een bepaalde peering. De ARP-tabel voor een peering ExpressRoute circuit biedt de volgende informatie voor elke interface (primair en secundair)

1. Toewijzing van voor gebouwen router interface IP-adres aan het MAC-adres
2. Toewijzing van IP-adres van ExpressRoute router-interface voor het MAC-adres
3. Leeftijd van de toewijzing

ARP-tabellen kunnen laag 2 configuratie valideren en het oplossen van elementaire layer 2 verbindingsproblemen. 

Voorbeeld van de ARP-tabel: 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


De volgende sectie bevat informatie over hoe u de ARP-tabellen gezien door de routers ExpressRoute rand kunt bekijken. 

## <a name="prerequisites-for-learning-arp-tables"></a>Vereisten voor het leren van de ARP-tabellen

Hebt u het volgende voordat u verdere vooruitgang

 - Een geldige ExpressRoute circuit geconfigureerd met ten minste één peering. Het circuit moet volledig worden geconfigureerd door de provider connectiviteit. U (of uw provider verbinding) moet zijn geconfigureerd ten minste één van de peerings (Azure private, Azure publiek en Microsoft) op dit circuit.
 - IP-adresbereiken die wordt gebruikt voor het configureren van de peerings (Azure private, Azure publiek en Microsoft). Het IP-adres toewijzing voorbeelden in de [ExpressRoute routing pagina met systeemvereisten](expressroute-routing.md) voor een goed begrip van hoe IP-adressen zijn toegewezen aan interfaces aan uw zijde en aan de kant van de ExpressRoute bekijken. U krijgt informatie over de configuratie van de peering aan de hand van de [configuratiepagina van ExpressRoute peering](expressroute-howto-routing-arm.md).
 - Gegevens van uw netwerk team / provider connectiviteit op de MAC-adressen van de interfaces met deze IP-adressen gebruikt.
 - U hebt de nieuwste PowerShell module voor Azure (1,50 of nieuwere versie).

## <a name="getting-the-arp-tables-for-your-expressroute-circuit"></a>De ARP-tabellen ophalen voor uw ExpressRoute circuit
Deze sectie bevat instructies over hoe u de ARP-tabellen per peering met PowerShell kunt bekijken. U of uw provider verbinding moet hebt geconfigureerd voor de peering voordat u zich verder ontwikkelen. Elk circuit heeft twee paden (primaire en secundaire). U kunt de ARP-tabel voor elk pad afzonderlijk controleren.

### <a name="arp-tables-for-azure-private-peering"></a>ARP-tabellen voor Azure private peering
De volgende cmdlet voorziet het ARP tabellen Azure private peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary
        
        # ARP table for Azure private peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

Voorbeeld van de uitvoer hieronder vindt u een van de paden

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>ARP-tabellen voor Azure openbare peering
De volgende cmdlet voorziet het ARP tabellen Azure openbare peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary
        
        # ARP table for Azure public peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


Voorbeeld van de uitvoer hieronder vindt u een van de paden

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>ARP-tabellen voor Microsoft peering
De volgende cmdlet voorziet het ARP tabellen Microsoft peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary
        
        # ARP table for Microsoft peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


Voorbeeld van de uitvoer hieronder vindt u een van de paden

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Het gebruik van deze informatie
De ARP-tabel van een peering kan worden gebruikt om te bepalen layer 2-configuratie en de verbindingen te valideren. Deze sectie bevat een overzicht van hoe ARP tabellen onder verschillende omstandigheden uitziet.

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a>ARP-tabel als een circuit in de operationele status (verwachte status)

 - De ARP-tabel heeft een vermelding voor de kant op bedrijven met een geldig IP-adres en MAC-adres en een vergelijkbare vermelding voor de kant van Microsoft. 
 - Het laatste octet van het IP-adres van op het bedrijf is een oneven getal.
 - Het laatste octet van het IP-adres van Microsoft worden altijd een even getal.
 - Hetzelfde MAC-adres wordt weergegeven voor alle 3 peerings (primaire / secundaire) bij Microsoft. 


        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a>ARP-tabel wanneer in ruimten / connectiviteit provider heeft problemen

 - Slechts één vermelding wordt in de ARP-tabel weergegeven. Hier ziet de koppeling tussen het MAC-adres en het IP-adres in van de kant van Microsoft. 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

>[AZURE.NOTE] Open een verzoek voor ondersteuning van uw provider verbinding met dergelijke problemen opsporen. 


### <a name="arp-table-when-microsoft-side-has-problems"></a>ARP-tabel wanneer Microsoft problemen heeft

 - Een ARP-tabel weergegeven voor een peering als er problemen bij Microsoft zijn wordt niet weergegeven. 
 -  Een support ticket openen met [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Opgeven dat u een probleem met laag 2-connectiviteit. 

## <a name="next-steps"></a>Volgende stappen

 - Layer 3-configuraties voor uw ExpressRoute circuit valideren
     - Route overzicht om te bepalen wat de status van sessies BGP ophalen 
     - Routetabel om te bepalen welke prefixen aangegeven via ExpressRoute ophalen
 - Overdracht van gegevens valideren aan de hand van bytes in / out
 - Een support ticket openen met [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) als u nog steeds problemen ondervindt.
