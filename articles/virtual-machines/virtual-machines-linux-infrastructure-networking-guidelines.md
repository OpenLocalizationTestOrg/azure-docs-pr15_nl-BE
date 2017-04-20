<properties
    pageTitle="Netwerk infrastructuur richtlijnen | Microsoft Azure"
    description="Informatie over de belangrijkste ontwerp en de implementatie richtlijnen voor het implementeren van virtuele netwerken in Azure infrastructuurservices."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="networking-infrastructure-guidelines"></a>Richtlijnen voor netwerk infrastructuur

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

Dit artikel is gericht op het inzicht van de vereiste stappen van de planning voor virtuele netwerken in Azure en verbindingen tussen bestaande op prem omgevingen.


## <a name="implementation-guidelines-for-virtual-networks"></a>Implementatie van richtlijnen voor virtuele netwerken

Besluiten:

- Wat voor soort virtueel netwerk hebt u nodig voor het hosten van uw werkbelasting IT of infrastructuur (alleen cloud- of cross-ruimte)?
- Voor virtuele netwerken voor cross-ruimten, hoeveel ruimte moet u voor het hosten van de subnetten en VMs nu en in de toekomst redelijk uitbreiding?
- Gaat u voor het maken van gecentraliseerde virtuele netwerken of afzonderlijke virtuele netwerken voor elke bronnengroep maken?

Taken:

- Definieer de adresruimte voor de virtuele netwerken moet worden gemaakt.
- De verzameling subnetten en de adresruimte voor elk definiëren.
- Voor virtuele netwerken tussen bedrijven, definiëren de set van lokale netwerk adresruimten locaties van de op de ruimten die de VMs in het virtuele netwerk nodig om te bereiken.
- Werk in lokalen netwerken team om ervoor te zorgen de juiste routering wordt geconfigureerd bij het maken van cross-premises virtuele netwerken.
- Het virtuele netwerk met behulp van de naamgevingsconventie maken.


## <a name="virtual-networks"></a>Virtuele netwerken

Virtuele netwerken zijn nodig voor communicatie tussen virtuele machines (VMs). U kunt definiëren van subnetten, aangepaste IP-adres, DNS-instellingen, beveiliging, filteren en taakverdeling, net als bij de fysieke netwerken. Via een [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) of [Route Express circuit](../expressroute/expressroute-introduction.md), Azure virtuele netwerken verbinding kunt maken met uw netwerken op gebouwen. U kunt meer lezen over [virtuele netwerken en hun onderdelen](../virtual-network/virtual-networks-overview.md).

Met behulp van resourcegroepen, beschikt u over flexibiliteit in het ontwerp van uw virtuele netwerkonderdelen. VMs kunnen verbinding maken met virtuele netwerken buiten hun eigen bronnengroep. Een gemeenschappelijke aanpak van ontwerp zijn centrale resourcegroepen die bevatten uw hoofdnetwerkinfrastructuur die kan worden beheerd door een gemeenschappelijk team te maken. VMs en hun toepassingen die worden gebruikt om aparte bronnengroepen. Deze benadering kunt eigenaren-toepassing toegang tot de resourcegroep die hun VMs zonder toegang tot de configuratie van de bredere virtuele netwerken bronnen bevat.

## <a name="site-connectivity"></a>Site-connectiviteit

### <a name="cloud-only-virtual-networks"></a>Virtuele netwerken die uitsluitend cloud
Als continue connectiviteit met VMs in een virtueel netwerk van Azure op gebouwen-gebruikers en computers vereist, uw virtueel netwerkontwerp spreekt voor zich:

![Alleen cloud-virtuele Basisnetwerkdiagram](./media/virtual-machines-common-infrastructure-service-guidelines/vnet01.png)

Deze benadering is meestal voor de werkbelasting van de internetverbinding, zoals een webserver met Internet. U kunt deze VMs met SSH of point-to-site VPN-verbindingen kunt beheren.

Omdat ze geen verbinding met uw netwerk in de lokalen maken, kunnen alleen Azure virtuele netwerken een gedeelte van de particuliere IP-adresruimte gebruiken. De adresruimte kan dezelfde persoonlijke ruimte die in gebruik op ruimten zijn.


### <a name="cross-premises-virtual-networks"></a>Virtuele netwerken tussen ruimten
Als op gebouwen-gebruikers en computers continue connectiviteit met VMs in een virtueel netwerk in Azure vereisen, maakt u een virtueel netwerk meerdere ruimten. Het virtuele netwerk verbinding met het netwerk op gebouwen met een ExpressRoute of VPN-verbinding van site naar site.

![Cross-premises virtuele netwerkdiagram](./media/virtual-machines-common-infrastructure-service-guidelines/vnet02.png)

In deze configuratie is de Azure virtueel netwerk in wezen een cloud-gebaseerde uitbreiding van uw netwerk op gebouwen.

Omdat ze verbinding met uw netwerk op lokalen maken, cross-premises virtuele netwerken een gedeelte van de adresruimte gebruikt in uw organisatie unieke moeten gebruiken. Azure wordt een andere locatie op dezelfde manier die verschillende corporate locaties met een specifiek IP-subnet worden toegewezen als u van uw netwerk kunt uitbreiden.

Als pakketten worden getransporteerd van het virtuele netwerk meerdere ruimten aan uw netwerk op gebouwen, moet u de reeks adresprefixen betrokken op panden als onderdeel van de definitie van het lokale netwerk voor het virtuele netwerk. Afhankelijk van de adresruimte van het virtuele netwerk en de betrokken op panden, locaties, kunnen er veel adresprefixen in het lokale netwerk.

Kunt u een virtueel netwerk cloud alleen converteren naar een virtueel netwerk meerdere ruimten, maar het waarschijnlijk moet u op re-IP-adres de adresruimte virtueel netwerk en Azure bronnen. Daarom moet u zorgvuldig overwegen of een virtueel netwerk worden aangesloten op het netwerk op ruimten moet wanneer u een IP-subnet toewijst.

## <a name="subnets"></a>Subnetten
Subnetten kunnen u indelen van bronnen die verband houden, een logisch (bijvoorbeeld één subnet voor VMs die zijn gekoppeld aan dezelfde toepassing), of fysiek (bijvoorbeeld één subnet per resourcegroep). U kunt ook gebruikmaken van subnet isolatie technieken voor extra beveiliging.

Voor cross-premises virtuele netwerken, moet u de subnetten met dezelfde conventies die u voor de resources voor gebouwen gebruikt ontwerpen. **Azure altijd de eerste drie IP-adressen van de adresruimte voor elk subnet gebruikt**. Begin met het tellen van het aantal VMs die u nu nodig hebt om te bepalen van het aantal adressen, die nodig zijn voor het subnet. Ramen voor toekomstige groei en vervolgens in de volgende tabel gebruiken om te bepalen van de grootte van het subnet.

Aantal benodigde VMs | Aantal hostbits nodig | De grootte van het subnet
--- | --- | ---
1 – 3 | 3 | -29
4: 11     | 4 | / 28
12: 27 | 5 | / 27
28-59 | 6 | / 26
60 – 123 | 7 | 25

> [AZURE.NOTE] Voor normale op ruimten subnetten is het maximum aantal adressen voor subnet met n hostbits host 2<sup>n</sup> – 2. Het maximum aantal adressen voor subnet met n hostbits host is voor een Azure subnet 2<sup>n</sup> – 5 (2 plus 3 voor de adressen die Azure worden gebruikt in elk subnet).

Als u de grootte van een subnet dat te klein is, u moet re-IP- en implementeren van het VMs in het subnet.


## <a name="network-security-groups"></a>Netwerk-beveiligingsgroepen
U kunt filteren op het verkeer dat loopt via uw virtuele netwerken met behulp van beveiligingsgroepen netwerk toepassen. U kunt gedetailleerde filteren regels voor het beveiligen van uw virtuele netwerkomgeving maken beheren van binnenkomend en uitgaand verkeer, bron en doel IP-bereiken, toegestane poorten, enz. Beveiligingsgroepen netwerk kan worden toegepast op subnetten binnen een virtueel netwerk of rechtstreeks aan een bepaalde virtuele netwerkinterface. Het wordt aanbevolen om een redelijke mate van netwerk Security Group filteren van verkeer op uw virtuele netwerken. U kunt meer lezen over [Netwerk-beveiligingsgroepen](../virtual-network/virtual-networks-nsg.md).


## <a name="additional-network-components"></a>Extra netwerkonderdelen
Net als bij een in de lokalen fysieke netwerkinfrastructuur, bevatten Azure virtueel netwerk meer dan alleen subnetten en IP-adressen. Bij het ontwerpen van uw toepassing-infrastructuur, kunt u enkele van deze extra onderdelen opnemen:

- [Gateways VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) - verbinding Azure virtuele netwerken met andere Azure virtuele netwerken of verbinding maken met netwerken door middel van een Site naar Site VPN-verbinding voor gebouwen. Route Express-verbindingen voor speciale, veilige verbindingen implementeert. U kunt ook gebruikers rechtstreeks toegang geven met Point-to-Site VPN-verbindingen.
- [Load balancer](../load-balancer/load-balancer-overview.md) - biedt taakverdeling van verkeer voor zowel externe als interne verkeer naar wens.
- [Application Gateway](../application-gateway/application-gateway-introduction.md) - de belasting voor documentconversies HTTP-taakverdeling op de toepassingslaag, enkele extra voordelen bieden voor webtoepassingen in plaats van de Azure implementeren.
- [Beheer van netwerkverkeer](../traffic-manager/traffic-manager-overview.md) - verkeer op basis van de DNS distributie naar eindgebruikers het eindpunt van de dichtstbijzijnde beschikbare toepassing direct zodat u uw hosttoepassing van Azure datacenters in verschillende regio's.


## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 