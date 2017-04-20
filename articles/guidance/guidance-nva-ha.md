<properties
   pageTitle="Implementeren van virtuele apparaten in hoge mate van beschikbaarheid | Microsoft Azure"
   description="Het implementeren van virtuele netwerkapparaten in hoge mate van beschikbaarheid."
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
   ms.date="09/21/2016"
   ms.author="telmos"/>

# <a name="deploying-virtual-appliances-in-high-availability"></a>Implementeren van virtuele apparaten in hoge mate van beschikbaarheid

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

In dit artikel worden een aantal procedures voor de implementatie van netwerk virtuele toestellen (NVAs) op een wijze die maximaal beschikbaar. Voordat u verdergaat, moet u weten hoe [door de gebruiker gedefinieerde routes (UDR)] [ udr-overview] en [de belasting voor documentconversies] [ lb-overview] werken in Azure.

Kunt u verschillende NVAs beschikbaar in de Azure markt uit te breiden de functionaliteit van Azure op dezelfde manier als u toestellen in uw datacenter op gebouwen. De volgende afbeelding toont een voorbeeld-implementatie van een [enkele NVA] [ nva-scenario] als een toestel van de firewall. 

![[0]][0]

Maar de vorige installatie werkt, geeft het een potentieel risico. Als het virtuele toestel is mislukt, is er geen verkeer loopt. Als u wilt dat het probleem is opgelost, moet u meerdere NVAs gebruiken. Die ook moet echter andere instellingen en bronnen worden gebruikt, afhankelijk van uw vereisten.

U kunt een van de volgende oplossingen implementeren een uiterst beschikbare omgeving van de NVA.

|Oplossing|Voordelen|Overwegingen met betrekking tot|
|---|---|---|
|Ingress met layer 7 virtuele toestellen|Alle knooppunten zijn actief.|Vereist een NVA die SNAT kunt verbindingen beëindigen<br/>Vereist een aparte set NVAs voor verkeer afkomstig van het Internet en Azure<br/>Kan alleen worden gebruikt voor het verkeer van oorsprong zijn uit de Azure|
|Ingress-Egress met layer 7 virtuele toestellen|Alle knooppunten zijn actief.<br/>Verkeer dat afkomstig is van Azure aankunnen |Vereist een NVA die SNAT kunt verbindingen beëindigen<br/>Vereist een aparte set NVAs voor verkeer afkomstig van het Internet en Azure|
|PIP-UDR switch|Één set NVAs voor al het verkeer<br/>Alle verkeer (geen limiet poortregels) kunt verwerken|Actief-passief<br/>Een failover-procedure is vereist|

## <a name="ingress-with-layer-7-virtual-appliances"></a>Ingress met layer 7 virtuele toestellen
U kunt een reeks NVAs achter een Azure taakverdeling verzorgen naar Azure werklasten achter een kleine set van server-side-poorten (zoals HTTP en HTTPS). De onderstaande figuur benadrukt hoe beschikbaarheid in dit scenario op het niveau van de NVA.

![[1]][1]

In dit scenario moet het netwerk virtuele toestel gebruikt om alle verbindingen beëindigen en ze doorgeven aan het subnet van de werkbelasting. De werkbelasting van virtuele machines (VMs) reageren op de NVA die een verzoek van en verkeersstromen zonder problemen ontvangen. 

## <a name="ingress-egress-with-layer-7-virtual-appliances"></a>Ingress-egress met layer 7 virtuele toestellen
U kunt de voorgaande architectuur uitbreiden en een andere reeks NVAs voor het verwerken van verkeer dat afkomstig is van Azure kunnen worden verwerkt door NVAs, zoals in de volgende afbeelding:

![[2]][2]

In dit scenario wordt alle verkeer dat afkomstig is in Azure doorgestuurd naar een interne taakverdeling die belasting tussen verschillende NVAs levert. Deze NVAs rechtstreeks verkeer naar het Internet via hun afzonderlijke openbare IP-adressen. 

## <a name="pip-udr-switch"></a>PIP-UDR switch
U kunt voorkomen dat meerdere NVA stapels maken met behulp van twee NVAs in actieve-passieve modus. In dit scenario kunt u de door de gebruiker gedefinieerde routes (UDRs) en openbare IP-adres (PIP) schakelen wanneer het actieve knooppunt stopt.  

![[3]][3]

Dit scenario is vergelijkbaar met het scenario met één NVA. Het enige verschil is dat de ogen en de UDRs om over te schakelen van verkeer tussen de NVAs moet worden gewijzigd. Deze wijzigingen kunnen handmatig worden gedaan of kunt u ook automatiseren. Als u wilt automatiseren, kunt u een toepassing die controleert of de gezondheid van het actieve knooppunt voor beide NVAs implementeren. Als het actieve knooppunt niet actief is, kan uw toepassing PIP en UDRs te koppelen aan het passieve knooppunt wijzigen.

Een mogelijke implementatie van deze oplossing is het gebruik van een [ZooKeeper] [ zookeeper] daemon op de NVAs voor het verwerken van leader election (bepalen welk knooppunt is het actieve knooppunt). Zodra een leider is gekozen, roept deze de Azure REST API voor de ogen van het knooppunt verwijderen en koppel deze aan de leider. Het moet ook UDRs om te verwijzen naar de nieuwe leider van particuliere IP-adres wijzigen.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [implementeren van een DMZ tussen Azure en uw datacenter op gebouwen] [ dmz-on-prem] met layer 7 NVAs.
- Meer informatie over het [implementeren van een DMZ tussen Azure en Internet] [ dmz-internet] laag 7 NVAs met.

<!-- links -->
[udr-overview]: ../virtual-network/virtual-networks-udr-overview.md
[lb-overview]: ../load-balancer/load-balancer-overview.md
[zookeeper]: https://zookeeper.apache.org/
[nva-scenario]: ../virtual-network/virtual-network-scenario-udr-gw-nva.md
[dmz-on-prem]: guidance-iaas-ra-secure-vnet-hybrid.md
[dmz-internet]: guidance-iaas-ra-secure-vnet-dmz.md

<!-- images -->
[0]: ./media/guidance-nva-ha/single-nva.png "Enkele NVA-architectuur"
[1]: ./media/guidance-nva-ha/l7-ingress.png "Laag 7 ingress"
[2]: ./media/guidance-nva-ha/l7-ingress-egress.png "7 ingress- en egress-laag"
[3]: ./media/guidance-nva-ha/active-passive.png "Actief-passief cluster"