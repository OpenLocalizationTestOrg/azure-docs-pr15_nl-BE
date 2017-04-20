
<properties
   pageTitle="Azure virtueel netwerk peering | Microsoft Azure"
   description="Meer informatie over peering in Azure VNet."
   services="virtual-network"
   documentationCenter="na"
   authors="NarayanAnnamalai"
   manager="jefco"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="narayan" />

# <a name="vnet-peering"></a>Peering VNet

VNet peering is een mechanisme dat twee virtuele netwerken (VNets) in dezelfde regio via de Azure backbone-netwerk verbindt. Zodra peered, worden de twee virtuele netwerken weergegeven als een voor alle verbindingen. Ze nog steeds als afzonderlijke bronnen worden beheerd, maar de virtuele machines in deze virtuele netwerken met elkaar kunnen communiceren via particuliere IP-adressen.

Het verkeer tussen de virtuele machines in de peered virtuele netwerken wordt gerouteerd via de Azure infrastructuur als verkeer tussen VMs in hetzelfde virtuele netwerk wordt omgeleid. Enkele van de voordelen van het gebruik van VNet peering zijn:

- Een lage latentie en hoge bandbreedte verbinding tussen resources in verschillende virtuele netwerken.
- De mogelijkheid om bronnen zoals toestellen voor netwerken en gateways VPN gebruiken als doorvoer punten in een peered VNet.
- De mogelijkheid om een virtueel netwerk dat gebruikmaakt van het model Azure Resource Manager met een virtueel netwerk dat gebruikmaakt van het implementatiemodel klassiek en volledige verbindingen tussen bronnen in deze virtuele netwerken verbinden.

Vereisten en belangrijke aspecten van de peering van VNet:

- De twee virtuele netwerken die zijn peered moeten in dezelfde regio Azure.
- De virtuele netwerken die zijn peered moeten hebben niet-overlappende IP-adresruimten.
- VNet peering is tussen twee virtuele netwerken en er is geen afgeleide transitieve relatie. Bijvoorbeeld als virtueel netwerk A is peered met virtueel netwerk B en als virtueel netwerk B is peered met virtueel netwerk C, deze niet wordt omgezet naar virtuele netwerk een peered daarvan met een virtueel netwerk C.
- Peering kan worden gelegd tussen de virtuele netwerken in twee verschillende abonnementen lang een bevoegde gebruiker van beide abonnementen machtigt de peering en de abonnementen zijn gekoppeld aan de dezelfde huurder van Active Directory. 
- Peering tussen virtueel netwerk in een model van resource manager en klassieke implementatiemodel is vereist dat de VNets van het abonnement op hetzelfde moet zijn.
- Een virtueel netwerk dat gebruikmaakt van het implementatiemodel Resource Manager kan met een andere virtuele netwerk dat gebruikmaakt van dit model worden peered of een virtueel netwerk dat wordt gebruikt voor het klassieke implementatiemodel. Virtuele netwerken die gebruikmaken van het implementatiemodel klassiek niet kunnen echter worden peered aan elkaar.
- Hoewel de communicatie tussen virtuele machines in peered virtuele netwerken geen beperkingen meer bandbreedte heeft, bandbreedte cap op basis van de VM nog steeds van toepassing is.


![Basic VNet peering](./media/virtual-networks-peering-overview/figure01.png)

## <a name="connectivity"></a>Connectiviteit
Nadat de twee virtuele netwerken zijn peered, een virtuele machine (web/werknemer rol) in het virtuele netwerk rechtstreeks verbinding kunnen maken met andere virtuele machines in de peered virtueel netwerk. Deze twee netwerken hebben volledige IP-connectiviteit.

De netwerklatentie voor een retour tussen twee virtuele machines in peered virtuele netwerken is hetzelfde als voor een retour in een virtueel lokaal netwerk. Doorvoer van het netwerk is gebaseerd op de bandbreedte die toegestaan voor de virtuele machine die evenredig is aan het formaat. Er is geen extra beperkingen voor de bandbreedte.

Het verkeer tussen de virtuele machines in peered virtuele netwerken wordt gerouteerd via de Azure back-end infrastructuur en niet via een gateway.

Virtuele machines in een virtueel netwerk toegang tot de interne taakverdeling (ILB) eindpunten in de peered virtueel netwerk. Netwerk-beveiligingsgroepen (NSGs) kunnen worden toegepast in een virtueel netwerk toegang tot andere virtuele netwerken of subnetten blokkeren indien gewenst.

Wanneer gebruikers peering configureert, kunnen ze openen of sluiten van de NSG regels tussen de virtuele netwerken. Als de gebruiker wil openen volledige connectiviteit tussen peered virtuele netwerken (dit is de standaardoptie), kunt deze vervolgens NSGs op specifieke subnetten of virtuele machines te blokkeren of weigeren van toegang voor specifieke.

Azure geleverde interne DNS-naamomzetting voor virtuele machines werkt in peered virtuele netwerken niet. Virtuele machines hebben een interne DNS-namen die omgezet slechts binnen het lokale virtuele netwerk zijn. Gebruikers kunnen echter een virtuele machines die worden uitgevoerd in peered virtuele netwerken als DNS-servers voor een virtueel netwerk configureren.

## <a name="service-chaining"></a>Chaining-service
Kunnen gebruikers door de gebruiker gedefinieerde routetabellen die naar de virtuele machines in peered virtuele netwerken als de "volgende hop" IP-adres verwijzen, zoals wordt weergegeven in het diagram dat verderop in dit artikel. Hierdoor kunnen gebruikers bereiken chaining-service, waarmee ze verkeer van een virtueel netwerk naar een virtueel toestel met een peered virtueel netwerk via de door de gebruiker gedefinieerde routetabellen kunnen verwijzen.

Gebruikers kunnen ook effectief type hub en spoke-omgevingen waar de hub zijn onderdelen van de infrastructuur zoals een virtueel toestel netwerk host kan maken. Alle spoke virtuele netwerken kunnen vervolgens peer met, evenals een subset van verkeer naar toestellen die actief zijn in het virtuele netwerk hub. Kortom, de volgende hop IP-adres op de 'gedefinieerd door de gebruiker tabel"VNet peering kan worden het IP-adres van een virtuele machine in de peered virtueel netwerk.

## <a name="gateways-and-on-premises-connectivity"></a>Gateways en op gebouwen-connectiviteit
Elk virtueel netwerk, ongeacht of deze peered is met een ander virtueel netwerk, en kan nog steeds hebben een eigen gateway en met het verbinding maken met op gebouwen. Gebruikers kunnen ook [VNet-VNet - verbindingen](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) configureren via gateways, zelfs als de virtuele netwerken zijn peered.

Wanneer beide opties voor het virtuele netwerk interconnectiviteit zijn geconfigureerd, het verkeer tussen de virtuele netwerken loopt via de peering configuratie (dat wil zeggen via de Azure backbone).

Wanneer de virtuele netwerken zijn peered, kunnen gebruikers ook configureren de gateway in het peered virtuele netwerk als doorvoer tot op gebouwen. In dit geval het virtuele netwerk dat van een externe gateway gebruikmaakt geen eigen gateway. Een virtueel netwerk kan slechts één gateway hebben. Dit is de gateway van een lokale of een externe gateway (in de peered virtueel netwerk), zoals in de volgende afbeelding.

Gateway-doorvoer wordt niet ondersteund in de peering relatie tussen virtuele netwerken met behulp van het model Resource Manager en die met behulp van het implementatiemodel klassiek. Zowel virtuele netwerken in de peering relatie moeten het implementatiemodel Resource Manager voor de doorvoer van een gateway gebruiken om te werken.

Wanneer de virtuele netwerken die één verbinding Azure ExpressRoute delen zijn peered, het verkeer tussen de peering relatie doorloopt (dat wil zeggen via de Azure backbone-netwerk). Gebruikers kunnen nog steeds lokale gateways in elk virtueel netwerk gebruiken verbinding maken met het circuit op gebouwen. Ook kunnen een gedeelde gateway en de doorvoer voor connectiviteit voor bedrijfsruimten configureren.

![VNet peering doorvoer](./media/virtual-networks-peering-overview/figure02.png)

## <a name="provisioning"></a>Inrichten
VNet peering is een bevoegde bewerking. Het is een aparte functie in de naamruimte VirtualNetworks. Een gebruiker kan specifieke rechten toe te staan peering krijgen. Een gebruiker die alleen-lezen toegang tot het virtuele netwerk heeft neemt deze rechten automatisch.

Een gebruiker die ofwel een beheerder is of gebruiker met volledige rechten van de peering mogelijkheid tot stand kan brengen een peering-bewerking op een andere VNet. Als er een overeenkomende aanvraag voor peering aan de andere kant en overige voorwaarden is voldaan, wordt de peering vastgesteld.

Raadpleeg de artikelen in de sectie 'Volgende stappen' voor meer informatie over het tot stand brengen tussen twee virtuele netwerken peering VNet.

## <a name="limits"></a>Limieten
Er zijn beperkingen op het aantal peerings die zijn toegestaan voor een enkele virtuele netwerk. Verwijzen naar [Azure netwerken grenzen](../azure-subscription-service-limits.md#networking-limits) voor meer informatie.

## <a name="pricing"></a>Prijzen
VNet peering is gratis tijdens de controleperiode. Nadat deze is uitgebracht, zal er een nominale vergoeding in de ingress- en egress-verkeer dat gebruikmaakt van de peering. Raadpleeg voor meer informatie op de [pagina prijzen](https://azure.microsoft.com/pricing/details/virtual-network).


## <a name="next-steps"></a>Volgende stappen
- [Peering tussen virtuele netwerken instellen](virtual-networks-create-vnetpeering-arm-portal.md).
- Meer informatie over [NSGs](virtual-networks-nsg.md).
- Informatie over [aangepaste routes en doorsturen via IP](virtual-networks-udr-overview.md).
