
<properties
   pageTitle="Interne taakverdeling overzicht | Microsoft Azure"
   description="Overzicht van interne taakverdeling en de bijbehorende functies. De werking van een load balancer voor Azure en mogelijke scenario's voor het interne eindpunten configureren"
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


# <a name="internal-load-balancer-overview"></a>Interne load balancer-overzicht

In tegenstelling tot vervolgens Internet gerichte taakverdeling, de interne taakverdeling (ILB) zorgt ervoor dat alleen verkeer bronnen binnen de cloud-service of via VPN toegang te krijgen tot de infrastructuur van Azure. De infrastructuur beperkt toegang tot de taakverdeling virtuele IP-adressen (VIP's) van een Cloud-Service of een virtueel netwerk, zodat ze worden nooit rechtstreeks blootgesteld aan een Internet-eindpunt. Hierdoor kunnen de interne regel van toepassingen uit te voeren in Azure en in de cloud of resources in ruimten zijn toegankelijk vanuit business (LOB).

## <a name="why-you-may-need-an-internal-load-balancer"></a>Waarom moet u mogelijk een interne taakverdeling

Azure interne laden Balancing (ILB) biedt de taakverdeling tussen de virtuele machines die zich in een cloud-service of een virtueel netwerk met een regionale bereik bevinden. Zie voor meer informatie over het gebruik en de configuratie van de virtuele netwerken met een bereik van regionale [Regionale virtuele netwerken](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/) in de Azure blog. Bestaande virtuele netwerken die zijn geconfigureerd voor een groep affiniteit niet ILB gebruiken.

ILB kunnen de volgende typen taakverdeling:

- In een cloud-service uit een set van virtuele machines die zich in dezelfde cloud service bevinden voor virtuele machines (Zie figuur 1).
- In een virtueel netwerk van virtuele machines in het virtuele netwerk op een set van virtuele machines die zich in dezelfde cloud-service de virtuele bevinden netwerk (Zie afbeelding 2).
- Voor een virtueel netwerk meerdere gebouwen van computers voor ruimten met een set van virtuele machines die zich in dezelfde cloud-service de virtuele bevinden netwerk (Zie afbeelding 3).
- Verbonden met Internet toepassingen met meerdere lagen die voor de back-end-niveaus zijn niet verbonden met Internet maar taakverdeling voor verkeer afkomstig van de laag internetverbinding vereist.
- Taakverdeling voor LOB-toepassingen die worden gehost in Azure zonder extra load balancer hardware of software. Met inbegrip van servers op gebouwen in de verzameling computers waarvan u het verkeer is de belasting in evenwicht.

## <a name="internet-facing-multi-tier-applications"></a>Toepassingen met meerdere niveaus toegankelijk via Internet


De weblaag heeft Internet gerichte eindpunten voor internetclients en maakt deel uit van een reeks verdeeld. De taakverdeling worden binnenkomende verkeer van webclients voor TCP-poort 443 (HTTPS) naar de webservers.

De databaseservers zijn achter een eindpunt ILB waarvan de webservers worden gebruikt voor de opslag. Deze database service Netwerktaakverdeling eindpunt, welk verkeer gelijkmatig verdeeld over de database-servers in de ILB-set wordt.

De volgende afbeelding toont de meerlagige toepassing binnen dezelfde service cloud toegankelijk via Internet.

Figuur 1 - meerlagige toepassing toegankelijk via Internet

![Interne één cloud service voor taakverdeling](./media/load-balancer-internal-overview/IC736321.png)

Ook gebruiken voor een toepassing met meerdere niveaus is wanneer het ILB naar een andere cloud-service dan het gebruik van de service voor het ILB geïmplementeerd.

Cloud-services met behulp van hetzelfde virtuele netwerk hebben toegang tot het eindpunt van het ILB.

Figuur 2 ziet front-end web servers zich in een andere cloud-service uit de database back-end en het gebruik van het ILB eindpunt in hetzelfde virtuele netwerk.

![Interne taakverdeling tussen cloud services](./media/load-balancer-internal-overview/IC744147.png)

Figuur 2 - front-end-servers in een andere cloud-service

## <a name="intranet-line-of-business-applications"></a>Intranet aantal zakelijke toepassingen

Verkeer van clients op het netwerk op gebouwen krijgen verdeeld voor de set van LOB-servers met behulp van VPN-verbinding met het netwerk van Azure.

De clientcomputer hebben toegang tot een IP-adres van Azure VPN-service met de punt naar site VPN. Het gebruik kan de LOB-toepassing die achter het eindpunt van het ILB zijn ondergebracht.

![Interne taakverdeling met punt naar site VPN](./media/load-balancer-internal-overview/IC744148.png)

Figuur 3 - LOB-toepassingen die worden gehost achter het eindpunt LB

Een ander scenario voor de LOB is om een site naar site VPN naar het virtuele netwerk waar het ILB eindpunt is geconfigureerd. Dit kan netwerkverkeer moet worden doorgestuurd naar het eindpunt van het ILB op gebouwen.

![Interne taakverdeling met behulp van site naar site VPN](./media/load-balancer-internal-overview/IC744150.png)

Figuur 4 - On-premises verkeer doorgestuurd naar het eindpunt van het ILB


## <a name="next-steps"></a>Volgende stappen

[Azure Resource Manager ondersteuning voor taakverdeling Azure](load-balancer-arm.md)

[Een internetverbinding taakverdeling configureren aan de slag](load-balancer-get-started-internet-arm-ps.md)

[Een interne taakverdeling configureren aan de slag](load-balancer-get-started-ilb-arm-ps.md)

[Een Load balancer distributie-modus configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP-time-outinstellingen voor de taakverdeling configureren](load-balancer-tcp-idle-timeout.md)

