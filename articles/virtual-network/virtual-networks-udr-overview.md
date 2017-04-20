<properties 
   pageTitle="Wat zijn de gebruiker gedefinieerde Routes en IP-doorsturen?"
   description="Informatie over gebruiker gedefinieerd Routes (UDR) en het doorsturen van IP-verkeer wilt doorsturen gebruiken om virtuele toestellen in Azure netwerk."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="what-are-user-defined-routes-and-ip-forwarding"></a>Wat zijn de gebruiker gedefinieerde Routes en IP-doorsturen?
Als u virtuele machines (VMs) aan een virtueel netwerk (VNet) in Azure toevoegt, ziet u dat de VMs kunnen met elkaar communiceren via het netwerk, automatisch. U hoeft niet op te geven van een gateway, hoewel het VMs in verschillende subnetten. Hetzelfde geldt voor de communicatie via de VMs met het openbare Internet en zelfs op uw locatie op netwerk wanneer een verbinding hybride van Azure naar uw eigen datacenter wordt weergegeven.

Deze stroom van communicatie is mogelijk omdat Azure een aantal routes systeem gebruikt om te bepalen hoe de IP-verkeer loopt. Routes systeem bepalen de communicatiestroom verloopt in de volgende scenario's:

- Uit in hetzelfde subnet.
- In een subnet binnen een VNet.
- Vanaf VMs met het Internet.
- Van een VNet naar een andere VNet via een VPN-gateway.
- Uit een VNet met uw netwerk op locatie via een VPN-gateway.

In de volgende afbeelding ziet u een eenvoudige installatie met een VNet, twee subnetten en een paar VMs, samen met de systeem-routes die IP-verkeer toestaan.

![Routes in Azure systeem](./media/virtual-networks-udr-overview/Figure1.png)

Hoewel het gebruik van routes systeem verkeer automatisch voor de implementatie vergemakkelijkt, zijn er gevallen waarin u wilt besturen met de routering van pakketten via een virtueel toestel. U kunt dus door de gebruiker gedefinieerde routes maken die opgeeft de volgende hop voor pakketten die naar een specifiek subnet te gaan in plaats daarvan op uw toestel virtuele doorloopt en doorsturen via IP voor VM wordt uitgevoerd als het virtuele toestel inschakelen.

In de volgende afbeelding ziet u een voorbeeld van de gebruiker gedefinieerde routes en doorsturen via IP tot pakketten verzonden naar één subnet van de andere te gaan via een virtueel toestel in een derde subnet.

![Routes in Azure systeem](./media/virtual-networks-udr-overview/Figure2.png)

>[AZURE.IMPORTANT] Door gebruiker gedefinieerde routes worden alleen toegepast op verkeer dat vanaf een subnet. u kunt geen routes als u wilt opgeven hoe verkeer wordt geleverd in een subnet van het Internet, bijvoorbeeld. Ook kan het toestel dat u bij het doorsturen van verkeer naar niet in hetzelfde subnet waar het verkeer vandaan komt. Maak altijd een afzonderlijk subnet bestemd voor de toestellen. 

## <a name="route-resource"></a>Route-resource
Pakketten worden doorgestuurd via een TCP/IP-netwerk op basis van een routetabel op elk knooppunt in het fysieke netwerk gedefinieerd. Een tabel is een verzameling afzonderlijke routes die worden gebruikt om te bepalen waar u voor het doorsturen van pakketten op basis van het IP-doeladres. Een route bestaat uit de volgende opties:

|Eigenschap|Beschrijving|Beperkingen|Overwegingen met betrekking tot|
|---|---|---|---|
| Adresprefix | De bestemming CIDR waarop de route van toepassing, zoals 10.1.0.0/16 is.|Moet worden geldig bereik CIDR die adressen op het openbare Internet, Azure virtueel netwerk of datacenter op gebouwen.|Zorg ervoor dat het **adresprefix** bevat het adres van het **Next-hopadres**, anders uw pakketten wordt ingevoerd in een lus die van de bron naar de volgende hop zonder ooit bereikt de bestemming niet. |
| Volgende-hop-type | Het type van Azure hop het pakket moet worden verzonden naar. | Moet een van de volgende waarden: <br/> **Virtueel netwerk**. Hiermee geeft u het lokale virtuele netwerk. Bijvoorbeeld, als er twee subnetten, 10.1.0.0/16 en 10.2.0.0/16 in hetzelfde virtuele netwerk, heeft de route voor elk subnet in de routetabel een volgende hop-waarde van een *Virtueel netwerk*. <br/> **Virtueel netwerkgateway**. Dit object vertegenwoordigt een Azure S2S VPN-Gateway. <br/> **Internet**. Hiermee geeft u de standaard Internet-gateway geleverd door de infrastructuur Azure. <br/> **Virtueel toestel**. Dit object vertegenwoordigt een virtueel toestel dat u hebt toegevoegd aan uw Azure virtueel netwerk. <br/> **Geen**. Dit object vertegenwoordigt een zwart gat. Pakketten die worden doorgestuurd naar een zwart gat wordt helemaal niet worden doorgestuurd.| Kunt u met een type **geen** pakketten door naar een bepaalde bestemming te stoppen. | 
| Next-hopadres | Het next-hopadres bevat het IP-adres pakketten moeten worden doorgestuurd naar. De waarden voor de volgende hop zijn alleen toegestaan in de routes waar de volgende hop *Virtueel toestel is*.| Moet u een IP-adres kan worden bereikt binnen het virtuele netwerk waar de Route door de gebruiker gedefinieerde wordt toegepast. | Als het IP-adres een VM vertegenwoordigt, moet dat u [doorsturen via IP](#IP-forwarding) in Azure inschakelen voor de VM. |

In Azure PowerShell hebben enkele waarden voor 'NextHopType' verschillende namen:
- Virtueel netwerk is VnetLocal
- Virtueel netwerkgateway is VirtualNetworkGateway
- Virtuele toestel is VirtualAppliance
- Internet is Internet
- Is geen geen

### <a name="system-routes"></a>Systeem-Routes
Elk subnet dat is gemaakt in een virtueel netwerk wordt automatisch gekoppeld aan een tabel met de volgende regels voor systeem-route:

- **Lokale Vnet regel**: deze regel wordt automatisch gemaakt voor elk subnet van een virtueel netwerk. Dit geeft aan dat er een directe koppeling tussen de VMs in de VNet en er geen tussenliggende volgende hop is.
- **On-premises regel**: deze regel is van toepassing op alle verkeer dat bestemd is voor het adresbereik van lokalen en VPN-gateway gebruikt als de bestemming van de volgende hop.
- **Regels voor Internet**: deze regel verwerkt al het verkeer dat bestemd is voor het openbare Internet (adres voorvoegsel 0.0.0.0/0) en maakt gebruik van de infrastructuur voor internet-gateway als de volgende hop voor al het verkeer dat bestemd is voor het Internet.

### <a name="user-defined-routes"></a>Door de gebruiker gedefinieerde Routes
Voor de meeste omgevingen moet u alleen de routes systeem is al gedefinieerd door Azure. U moet echter een tabel maken en toevoegen van een of meer routes in specifieke gevallen, zoals:

- Force tunneling op het Internet via het netwerk op gebouwen.
- Het gebruik van virtuele apparaten in uw Azure-omgeving.

In de scenario's wordt er een tabel maken en door gebruiker gedefinieerde routes aan toe te voegen. U kunt meerdere routetabellen hebben en dezelfde routetabel kan worden gekoppeld aan een of meer subnetten. En elk subnet kan alleen worden gekoppeld aan een tabel met één route. Alle VMs en cloud services in een subnet-gebruik de tabel die is gekoppeld aan dat subnet.

Subnetten zijn afhankelijk van de routes systeem tot een tabel gekoppeld aan het subnet is. Zodra er een koppeling bestaat, routering gebeurt op basis van langste voorvoegsel Match (LPM) onder zowel door gebruiker gedefinieerde routes en routes systeem. Als er meer dan één route met de overeenkomst in dezelfde LPM is vervolgens een route geselecteerd op basis van hun oorsprong vinden in de volgende volgorde:

1. Gebruiker gedefinieerde route
1. BGP route (wanneer ExpressRoute wordt gebruikt)
1. Route systeem

Meer informatie over het maken van routes door de gebruiker gedefinieerd, Zie [Routes maken en doorsturen via IP in Azure](virtual-network-create-udr-arm-template.md).

>[AZURE.IMPORTANT] Door gebruiker gedefinieerde routes worden alleen toegepast op Azure VMs en cloud services. Bijvoorbeeld, als u toevoegen een virtueel toestel firewall tussen uw netwerk op gebouwen en Azure wilt, hebt u voor het maken van een route door de gebruiker gedefinieerd voor uw routetabellen Azure die alle verkeer naar de adresruimte van gebouwen op het virtuele toestel wordt doorgestuurd. U kunt ook een route door de gebruiker gedefinieerd (UDR) toevoegen aan de GatewaySubnet voor het doorsturen van alle verkeer op ruimten naar Azure via het virtuele toestel. Dit is een recente toevoeging.

### <a name="bgp-routes"></a>BGP Routes
Als u een ExpressRoute-verbinding tussen uw netwerk op gebouwen en Azure hebt, kunt u BGP routes van het netwerk op ruimten naar Azure doorgeven. Deze routes BGP worden op dezelfde manier als het systeem routes en routes door de gebruiker gedefinieerd in elk subnet Azure gebruikt. Zie [ExpressRoute-inleiding](../expressroute/expressroute-introduction.md)voor meer informatie.

>[AZURE.IMPORTANT] U kunt uw Azure-omgeving voor het gebruik van de kracht van tunneling via uw netwerk op gebouwen door een gebruiker gedefinieerde route voor het subnet 0.0.0.0/0 die gebruikmaakt van de VPN-gateway als de volgende hop. Echter, dit werkt alleen als u een VPN-gateway, niet ExpressRoute. ExpressRoute, wordt geforceerde tunneling geconfigureerd via BGP.

## <a name="ip-forwarding"></a>Doorsturen via IP
Zoals hierboven wordt beschreven, is een van de belangrijkste redenen voor het maken van een gebruiker gedefinieerde route voor het doorsturen van verkeer naar een virtueel toestel. Een virtuele toestel is niets meer dan een VM met een toepassing die wordt gebruikt voor het verwerken van netwerkverkeer op een bepaalde manier, zoals een firewall of een NAT-apparaat.

Deze virtuele VM moet zich kunnen ontvangen van binnenkomend verkeer dat niet aan zichzelf wordt behandeld. Als u wilt toestaan dat een VM naar andere bestemmingen worden geadresseerd verkeer wordt ontvangen, moet u doorsturen via IP inschakelen voor VM. Dit is een instelling, niet een instelling in het gastbesturingssysteem Azure.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [maken van routes in het implementatiemodel Resource Manager](virtual-network-create-udr-arm-template.md) en deze koppelen aan subnetten. 
- Meer informatie over het [maken van routes in de klassieke implementatiemodel](virtual-network-create-udr-classic-ps.md) en deze koppelen aan subnetten.
