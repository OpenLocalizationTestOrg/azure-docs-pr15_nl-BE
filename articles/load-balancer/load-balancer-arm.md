<properties
   pageTitle="Azure Resource Manager ondersteuning voor taakverdeling | Microsoft Azure "
   description="Met behulp van powershell voor taakverdeling met Azure Resource Manager. Met behulp van sjablonen voor taakverdeling"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="using-azure-resource-manager-support-with-azure-load-balancer"></a>Ondersteuning van Azure bronbeheer gebruiken met Azure Load Balancer

Azure Resource Manager is de beste raamwerk voor services in Azure. Azure taakverdeling kan worden beheerd met behulp van bronbeheer Azure-API's en hulpprogramma's.

## <a name="concepts"></a>Concepten

Met bronbeheer Azure Load Balancer bevat de volgende onderliggende bronnen:

- Front-IP-configuratie – een Load balancer kan een of meer front-end IP-adressen, ook bekend als een virtuele IP-adressen (VIP's) bevatten. Deze IP-adressen dienen als ingress voor het verkeer.

- Back-end-adresgroep – dit zijn IP-adressen die zijn gekoppeld aan de virtuele machine Network Interface Card (NIC) waarop belasting wordt verdeeld.

- Laden van netwerktaakverdeling regels – een regeleigenschap wijst een front-end voor bepaalde IP-poortcombinatie voor een reeks back-end-IP-adressen en poortcombinatie. Een single load balancer kan meerdere regels voor taakverdeling hebben. Elke regel is een combinatie van een front-IP-poort en back-end IP en poort geassocieerd met VMs.

- Sondes – sondes kunnen u bijhouden van de gezondheid van de VM-exemplaren. Als een sonde gezondheid mislukt, wordt de VM-sessie geleid van rotatie automatisch.

- Binnenkomende NAT regels – NAT regels definiëren het binnenkomende verkeer loopt via de voorkant beëindigen IP en gedistribueerd naar de back-end-IP.

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a>QuickStart-sjablonen

Azure Resource Manager kunt u uw toepassingen met behulp van een declaratieve sjabloon inrichten. U kunt meerdere services samen met hun afhankelijkheden implementeren in een bepaalde sjabloon. Met dezelfde sjabloon kunt u herhaaldelijk uw toepassing tijdens elke fase van de levenscyclus van de toepassing te implementeren.

Sjablonen kunnen bevatten definities voor virtuele Machines, virtuele netwerken beschikbaar stelt, netwerkinterfaces (NIC's), opslag rekeningen, Netwerktaakverdeling, netwerk-beveiligingsgroepen en openbare IP-adressen. Met sjablonen kunt u alles wat die u nodig hebt voor een complexe toepassing. Het sjabloonbestand kan worden gecontroleerd in content managementsysteem voor versiebeheer en samenwerking.

[Meer informatie over sjablonen](http://go.microsoft.com/fwlink/?LinkId=544798)

[Meer informatie over netwerkbronnen](../virtual-network/resource-groups-networking.md)

QuickStart sjablonen met Azure Load Balancer vindt u in een [opslagplaats van GitHub](https://github.com/Azure/azure-quickstart-templates) die als host fungeert voor een reeks sjablonen voor de gebruikersgemeenschap gegenereerd.

Voorbeelden van sjablonen:

- [2 VMs in een Load Balancer en de regels voor taakverdeling](http://go.microsoft.com/fwlink/?LinkId=544799)
- [2 VMs in een VNET met een interne taakverdeling en Load Balancer-regels](http://go.microsoft.com/fwlink/?LinkId=544800)
- [2 VMs in een Load Balancer en NAT-regels configureren op de LB](http://go.microsoft.com/fwlink/?LinkId=544801)


## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a>Azure taakverdeling met een PowerShell of CLI instellen

Aan de slag met Azure Resource Manager-cmdlets, opdrachtregelprogramma's en REST API 's

- [Azure Networking-Cmdlets](https://msdn.microsoft.com/library/azure/mt163510.aspx) kunnen worden gebruikt voor het maken van een taakverdeling.
- [Het maken van een load balancer Azure Resource Manager gebruiken](load-balancer-get-started-ilb-arm-ps.md)
- [Met behulp van de CLI Azure met Azure Resourcemanagement](../xplat-cli-azure-resource-manager.md)
- [De belasting voor documentconversies REST API 's](https://msdn.microsoft.com/library/azure/mt163651.aspx)


## <a name="next-steps"></a>Volgende stappen

U kunt ook [de taakverdeling van een internetverbinding maken](load-balancer-get-started-internet-arm-ps.md) en wat voor soort [distributie-modus](load-balancer-distribution-mode.md) voor een specifieke load balancer netwerkverkeer gedrag configureren.

Informatie over het beheren van [niet-actieve TCP-time-outinstellingen voor een taakverdeling](load-balancer-tcp-idle-timeout.md). Dit is belangrijk als uw toepassing moet verbindingen voor servers achter een load balancer in leven te houden.
