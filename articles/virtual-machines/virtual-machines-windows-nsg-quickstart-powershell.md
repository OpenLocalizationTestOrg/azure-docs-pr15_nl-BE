<properties
   pageTitle="Openen van poorten voor een VM met PowerShell | Microsoft Azure"
   description="Meer informatie over het openen een poort of een eindpunt voor uw Windows-VM met Azure PowerShell en Azure resource manager distributie modus maken"
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

# <a name="opening-ports-and-endpoints-to-a-vm-in-azure-using-powershell"></a>Openen van poorten en eindpunten voor een VM in Azure PowerShell gebruiken
[AZURE.INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Snelle opdrachten
Als een netwerk-beveiligingsgroep en ACL-regels wilt maken, moet u [de nieuwste versie van Azure PowerShell is geïnstalleerd](../powershell-install-configure.md). U kunt ook [u deze stappen uitvoert met behulp van de portal Azure](virtual-machines-windows-nsg-quickstart-portal.md).

Log in op uw Azure account:

```powershell
Login-AzureRmAccount
```

In de volgende voorbeelden kunt u voorbeeld parameternamen vervangen door uw eigen waarden. Voorbeeld van de parameternamen opgenomen `myResourceGroup`, `myNetworkSecurityGroup`, en `myVnet`.

Een regel maken. Het volgende voorbeeld wordt een regel met de naam `myNetworkSecurityGroupRule` dat TCP-verkeer op poort 80:

```powershell
$httprule = New-AzureRmNetworkSecurityRuleConfig -Name "myNetworkSecurityGroupRule" `
    -Description "Allow HTTP" -Access "Allow" -Protocol "Tcp" -Direction "Inbound" `
    -Priority "100" -SourceAddressPrefix "Internet" -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 80
```

Vervolgens wordt uw netwerk beveiligingsgroep maken en toewijzen van de HTTP-regel die u zojuist hebt gemaakt, als volgt. Het volgende voorbeeld wordt een netwerk beveiligingsgroep met de naam `myNetworkSecurityGroup`:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNetworkSecurityGroup" -SecurityRules $httprule
```

Nu gaan we uw netwerk groep toewijzen aan een subnet. Het volgende voorbeeld wordt een virtueel netwerk met de naam `myVnet` aan de variabele `$vnet`:

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

Uw netwerk beveiligingsgroep koppelen aan uw subnet. Het volgende voorbeeld wordt het subnet genaamd `mySubnet` met een groep met uw netwerk:

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

Ten slotte, update het virtuele netwerk in om uw wijzigingen te activeren:

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a>Meer informatie over beveiligingsgroepen netwerk
De snelle opdrachten hier kunt u aan de slag te kunnen met het verkeer dat naar uw VM. Beveiligingsgroepen netwerk bieden veel geweldige functies en granulatie voor het beheren van uw resources. U kunt meer lezen over het [maken van een netwerk-beveiligingsgroep en ACL-regels hier](../virtual-network/virtual-networks-create-nsg-arm-ps.md).

Als onderdeel van Azure Resource Manager-sjablonen kunt u netwerk-beveiligingsgroepen en ACL-regels definiëren. Meer informatie over het [maken van beveiligingsgroepen netwerk met sjablonen](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Als u gebruik van poort-forwarding een unieke externe poort toewijzen aan een interne poort op uw VM, een load balancer en regels voor NAT (Network Address Translation) gebruiken. U wilt bijvoorbeeld extern TCP-poort 8080 en verkeer op TCP-poort 80 op een VM gestuurd hebben. U kunt informatie over het [maken van een internetverbinding taakverdeling](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="next-steps"></a>Volgende stappen
In dit voorbeeld kunt u een eenvoudige regel voor HTTP-verkeer gemaakt. Hier vindt u informatie over het maken van gedetailleerde omgevingen in de volgende artikelen:

- [Azure Resource Manager-overzicht](../azure-resource-manager/resource-group-overview.md)
- [Wat is een Network Security Group (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Azure Resource Manager overzicht voor netwerktaakverdeling](../load-balancer/load-balancer-arm.md)