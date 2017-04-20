<properties
   pageTitle="Openen van poorten voor een VM met de Azure portal | Microsoft Azure"
   description="Meer informatie over het openen van een poort of een eindpunt voor uw Windows-VM in Azure Portal met behulp van het implementatiemodel resource manager maken"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-to-a-vm-in-azure-using-the-azure-portal"></a>Openen van poorten voor een VM in Azure met de Azure portal
[AZURE.INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Snelle opdrachten
U kunt ook [deze stappen met Azure PowerShell uitvoeren](virtual-machines-windows-nsg-quickstart-powershell.md).

Maak eerst de groep netwerk. Een resourcegroep selecteren in de portal, klik op **toevoegen**en zoek en selecteer 'Network security group':

![Netwerk beveiligingsgroep toevoegen](./media/virtual-machines-windows-nsg-quickstart-portal/add-nsg.png)

Voer een naam voor uw netwerk-beveiligingsgroep of een resourcegroep maken en selecteer een locatie selecteren. Klik op **maken** als u klaar bent:

![Maak een beveiligingsgroep netwerk](./media/virtual-machines-windows-nsg-quickstart-portal/create-nsg.png)

Selecteer de nieuwe groep netwerk. Selecteer 'inkomende beveiligingsregels en klik vervolgens op de knop **toevoegen** om een regel te maken:

![Een binnenkomende regel toevoegen](./media/virtual-machines-windows-nsg-quickstart-portal/add-inbound-rule.png)

Geef een naam voor de nieuwe regel. Poort 80 wordt al standaard ingevoerd. Deze blade is waar u de bron, bestemming en protocol wijzigen zou wanneer u extra regels toevoegen aan de groep netwerk. Klik op **OK** om de regel te maken:

![Een binnenkomende regel maken](./media/virtual-machines-windows-nsg-quickstart-portal/create-inbound-rule.png)

De laatste stap is uw netwerk beveiligingsgroep koppelen aan een subnet of een bepaalde netwerkinterface. Laten we de beveiligingsgroep netwerk koppelen met een subnet. Selecteer 'Subnetten' en klik op **koppelen**:

![Een beveiligingsgroep netwerk koppelen aan een subnet](./media/virtual-machines-windows-nsg-quickstart-portal/associate-subnet.png)

Het virtuele netwerk selecteren en selecteer het juiste subnet:

![Een beveiligingsgroep netwerk koppelen aan een virtueel netwerk](./media/virtual-machines-windows-nsg-quickstart-portal/select-vnet-subnet.png)

U hebt een beveiligingsgroep netwerk een binnenkomende regel gemaakt die kan verkeer op poort 80, en wat voor een subnet gemaakt. Een VMs u dat subnet zijn bereikbaar op poort 80.


## <a name="more-information-on-network-security-groups"></a>Meer informatie over beveiligingsgroepen netwerk
De snelle opdrachten hier kunt u aan de slag te kunnen met het verkeer dat naar uw VM. Beveiligingsgroepen netwerk bieden veel geweldige functies en granulatie voor het beheren van uw resources. U kunt meer lezen over het [maken van een netwerk-beveiligingsgroep en ACL-regels hier](../virtual-network/virtual-networks-create-nsg-arm-ps.md).

Als onderdeel van Azure Resource Manager-sjablonen kunt u netwerk-beveiligingsgroepen en ACL-regels definiëren. Meer informatie over het [maken van beveiligingsgroepen netwerk met sjablonen](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Als u gebruik van poort-forwarding een unieke externe poort toewijzen aan een interne poort op uw VM, een load balancer en regels voor NAT (Network Address Translation) gebruiken. U wilt bijvoorbeeld extern TCP-poort 8080 en verkeer op TCP-poort 80 op een VM gestuurd hebben. U kunt informatie over het [maken van een internetverbinding taakverdeling](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="next-steps"></a>Volgende stappen
In dit voorbeeld kunt u een eenvoudige regel voor HTTP-verkeer gemaakt. Hier vindt u informatie over het maken van gedetailleerde omgevingen in de volgende artikelen:

- [Azure Resource Manager-overzicht](../azure-resource-manager/resource-group-overview.md)
- [Wat is een Network Security Group (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Azure Resource Manager overzicht voor netwerktaakverdeling](../load-balancer/load-balancer-arm.md)
