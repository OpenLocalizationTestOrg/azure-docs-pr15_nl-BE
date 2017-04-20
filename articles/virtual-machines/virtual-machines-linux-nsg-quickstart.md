<properties
   pageTitle="Open poorten en eindpunten voor een Linux VM | Microsoft Azure"
   description="Meer informatie over het openen van een poort of een eindpunt voor uw Linux VM met behulp van het implementatiemodel Azure resource manager en de CLI Azure maken"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-and-endpoints-to-a-linux-vm-in-azure"></a>Openen van poorten en eindpunten voor een Linux VM in Azure
U opent een poort of eindpunt, met een virtuele machine (VM) in Azure maken door het maken van een netwerk-filter op een subnet of VM netwerk-interface. U plaatst deze filters die binnenkomend en uitgaand verkeer regelen, op een netwerk-beveiligingsgroep die is gekoppeld aan de resource die het verkeer ontvangt. We gebruiken een algemeen voorbeeld van Internet-verkeer op poort 80.

## <a name="quick-commands"></a>Snelle opdrachten
U moet [De CLI Azure](../xplat-cli-install.md) geïnstalleerd en Resource Manager gebruikt om een beveiligingsgroep netwerk en regels te maken:

```bash
azure config mode arm
```

In de volgende voorbeelden kunt u voorbeeld parameternamen vervangen door uw eigen waarden. Voorbeeld van de parameternamen opgenomen `myResourceGroup`, `myNetworkSecurityGroup`, en `myVnet`.

Maak uw netwerk beveiligingsgroep, uw eigen naam en locatie op de juiste manier invoeren. Het volgende voorbeeld wordt een netwerk beveiligingsgroep met de naam `myNetworkSecurityGroup` in de `WestUS` locatie:

```
azure network nsg create --resource-group myResourceGroup --location westus \
    --name myNetworkSecurityGroup
```

Een regel toevoegen waarmee uw webserver HTTP-verkeer toestaan (of aanpassen voor uw eigen scenario, zoals SSH toegang of database connectivity). Het volgende voorbeeld wordt een regel met de naam `myNetworkSecurityGroupRule` dat TCP-verkeer op poort 80:

```
azure network nsg rule create --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup --name myNetworkSecurityGroupRule \
    --protocol tcp --direction inbound --priority 1000 \
    --destination-port-range 80 --access allow
```

De beveiligingsgroep netwerk koppelen aan de netwerkinterface van uw VM (NIC). Het volgende voorbeeld wordt een bestaande NIC met de naam `myNic` met de netwerk-beveiligingsgroep met de naam `myNetworkSecurityGroup`:

```
azure network nic set --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup --name myNic
```

U kunt ook uw netwerk beveiligingsgroep koppelen met een virtueel netwerk subnet, en niet alleen op de netwerkinterface op een enkele VM. Het volgende voorbeeld wordt een bestaand subnet genaamd `mySubnet` in de `myVnet` virtueel netwerk met het netwerk beveiligingsgroep met de naam `myNetworkSecurityGroup`:

```
azure network vnet subnet set --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --vnet-name myVnet --name mySubnet
```

## <a name="more-information-on-network-security-groups"></a>Meer informatie over beveiligingsgroepen netwerk
De snelle opdrachten hier kunt u aan de slag te kunnen met het verkeer dat naar uw VM. Beveiligingsgroepen netwerk bieden veel geweldige functies en granulatie voor het beheren van uw resources. U kunt meer lezen over het [maken van een netwerk-beveiligingsgroep en ACL-regels hier](../virtual-network/virtual-networks-create-nsg-arm-cli.md).

Als onderdeel van Azure Resource Manager-sjablonen kunt u netwerk-beveiligingsgroepen en ACL-regels definiëren. Meer informatie over het [maken van beveiligingsgroepen netwerk met sjablonen](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Als u gebruik van poort-forwarding een unieke externe poort toewijzen aan een interne poort op uw VM, een load balancer en regels voor NAT (Network Address Translation) gebruiken. U wilt bijvoorbeeld extern TCP-poort 8080 en verkeer op TCP-poort 80 op een VM gestuurd hebben. U kunt informatie over het [maken van een internetverbinding taakverdeling](../load-balancer/load-balancer-get-started-internet-arm-cli.md).

## <a name="next-steps"></a>Volgende stappen
In dit voorbeeld kunt u een eenvoudige regel voor HTTP-verkeer gemaakt. Hier vindt u informatie over het maken van gedetailleerde omgevingen in de volgende artikelen:

- [Azure Resource Manager-overzicht](../azure-resource-manager/resource-group-overview.md)
- [Wat is een Network Security Group (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Azure Resource Manager overzicht voor netwerktaakverdeling](../load-balancer2    /load-balancer-arm.md)