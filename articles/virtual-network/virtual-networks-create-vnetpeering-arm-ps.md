<properties
   pageTitle="VNet Peering met Powershell-cmdlets maken | Microsoft Azure"
   description="Informatie over het maken van een virtueel netwerk met behulp van de portal Azure in Resource Manager."
   services="virtual-network"
   documentationCenter=""
   authors="NarayanAnnamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai; annahar"/>

# <a name="create-vnet-peering-using-powershell-cmdlets"></a>Maak VNet Peering met Powershell-cmdlets

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Wilt maken van een VNet peering met PowerShell, volgt u de volgende stappen uit:

1. Als u Azure PowerShell nog nooit hebt gebruikt, Zie [installeren en configureren van Azure PowerShell](../powershell-install-configure.md) en volg de instructies helemaal naar het einde ondertekenen in Azure en selecteer uw abonnement.

> [AZURE.NOTE] Voor het beheren van VNet peering PowerShell-cmdlet wordt geleverd met [Azure PowerShell 1.6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. Virtueel netwerkobjecten lezen:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
        $vnet2 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet2

3. Om te bepalen VNet peering, moet u twee koppelingen, één voor elke richting te maken. De volgende stap wordt eerst een VNet peering koppeling voor VNet1 om VNet2 te maken:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.Id

    Uitvoer wordt weergegeven:

        Name            : LinkToVNet2
        Id: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Initiated
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

4. Deze stap maakt een VNet peering koppeling voor VNet2 voor VNet1:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.Id

    Uitvoer wordt weergegeven:

        Name            : LinkToVNet1
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2/virtualNetworkPeerings/LinkToVNet1
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet2
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

5. Zodra de peering VNet-koppeling is gemaakt, ziet u de link staat hieronder:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name linktovnet2

    Uitvoer wordt weergegeven:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                             "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    Er zijn een aantal configureerbare eigenschappen voor peering VNet:

  	|Optie|Beschrijving|Standaard|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|Of de adresruimte van Peer-VNet om te worden opgenomen als onderdeel van de Tag Virtual_network|Ja|
  	|AllowForwardedTraffic|Verkeer dat afkomstig is uit een peered VNet niet is geaccepteerd of verwijderd|Nee|
  	|AllowGatewayTransit|Hiermee kunt u de peer VNet gebruik van de gateway VNet|Nee|
  	|UseRemoteGateways|Van de peer VNet gateway gebruiken. De peer VNet moet een gateway geconfigureerd en AllowGatewayTransit geselecteerd. U kunt deze optie niet gebruiken als u een gateway geconfigureerd|Nee|

    Elke koppeling in VNet peering heeft de bovenstaande eigenschappen. U kunt bijvoorbeeld AllowVirtualNetworkAccess ingesteld op True voor peering VNet-link VNet1 VNet2 en stel deze in op False voor de peering VNet-koppeling in de andere richting.

        $LinktoVNet2 = Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name LinkToVNet2
        $LinktoVNet2.AllowForwardedTraffic = $true
        Set-AzureRmVirtualNetworkPeering -VirtualNetworkPeering $LinktoVNet2

    U kunt uitvoeren Get-AzureRmVirtualNetworkPeering om te controleren op dubbele waarde van de eigenschap na de wijziging. U ziet van de uitvoer van de dat allowforwardedtraffic ingesteld op True wordt gewijzigd na het uitvoeren van de bovenstaande cmdlets.

        Name            : LinkToVNet2
        Id          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic       : True
        AllowGatewayTransit     : False
        UseRemoteGateways       : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

    Nadat de peering tot stand is gebracht in dit scenario, wees initiëren van verbindingen vanaf een virtuele machine met een virtuele machine van beide VNets. Standaard AllowVirtualNetworkAccess True is en de juiste ACL's zodat de communicatie tussen de VNets VNet peering wordt ingericht. U kunt nog steeds toepassen network group (NSG) beveiligingsregels voor verbindingen tussen specifieke subnetten of virtuele machines toegang krijgt tot fijne korrel toegang tussen twee virtuele netwerken geblokkeerd.  Raadpleeg dit [artikel](virtual-networks-create-nsg-arm-ps.md)voor meer informatie over het maken van regels voor NSG.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Peering over abonnementen met PowerShell VNet maken, voer de volgende stappen:

1. Aanmelden Azure met bevoorrechte gebruiker een account voor abonnement-A en voer de volgende cmdlet:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet5

    Dit is niet vereist, peering kan worden ingesteld zelfs als gebruikers peering aanvragen voor hun respectieve VNets afzonderlijk verhogen als de aanvragen voldoen aan. Een bevoegde gebruiker van de andere VNet toe te voegen als een gebruiker in de lokale VNet gemakkelijker te doen van de instellingen.

2. Voor aanmelden bij Azure met beschermde-van gebruiker B account abonnement-B en de volgende cmdlet uitvoert:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet3

3. Gebruiker A's in login sessie, de cmdlet hieronder uitvoeren:

        $vnet3 = Get-AzureRmVirtualNetwork -ResourceGroupName hr-vnets -Name vnet3

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet5 -VirtualNetwork $vnet3 -RemoteVirtualNetworkId "/subscriptions/<Subscription-B-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet5" -BlockVirtualNetworkAccess

4. Uitgevoerd in sessie-aanmelding gebruiker-B de cmdlet hieronder:

        $vnet5 = Get-AzureRmVirtualNetwork -ResourceGroupName vendor-vnets -Name vnet5

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet3 -VirtualNetwork $vnet5 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-A-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet3" -BlockVirtualNetworkAccess

5. Nadat peering is gevestigd, is een virtuele machine in VNet3 moeten kunnen communiceren met een virtuele machine in VNet5.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. In dit scenario kunt u de onderstaande vast te stellen de VNet peering PowerShell-cmdlets kunt uitvoeren.  U moet de eigenschap AllowForwardedTraffic ingesteld op True en VNET1 koppelen aan HubVNet, waarmee het binnenkomende verkeer van buiten de peering VNet-adresruimte.

        $hubVNet = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name HubVNet
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

        Add-AzureRmVirtualNetworkPeering -Name LinkToHub -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $HubVNet.Id -AllowForwardedTraffic

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $HubVNet -RemoteVirtualNetworkId $vnet1.Id

2. Nadat peering tot stand is gebracht, kunt u verwijzen naar dit [artikel](virtual-network-create-udr-arm-ps.md) en definieer een aangepaste route (UDR) VNet1 verkeer omleiden via een virtueel toestel gebruik van de mogelijkheden. Als u de next-hopadres in de route opgeeft, stelt u deze naar het IP-adres van het virtuele toestel in de peer VNet HubVNet. Hieronder volgt een voorbeeld:

        $route = New-AzureRmRouteConfig -Name TestNVA -AddressPrefix 10.3.0.0/16 -NextHopType VirtualAppliance -NextHopIpAddress 192.0.1.5

        $routeTable = New-AzureRmRouteTable -ResourceGroupName VNet101 -Location brazilsouth -Name TestRT -Route $route

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName VNet101 -Name VNet1

        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet1 -Name subnet-1 -AddressPrefix 10.1.1.0/24 -RouteTable $routeTable

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet1

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

U maakt een VNet tussen een klassieke virtueel netwerk en een virtueel netwerk Azure Resource Manager in PowerShell peering, volg de onderstaande stappen:

1. Virtueel netwerk-object voor de **VNET1**, het virtuele netwerk Azure Resource Manager als volgt gelezen:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

2. Om te bepalen VNet peering in dit scenario, slechts één koppeling nodig is, met name een koppeling van **VNET1** naar **VNET2**. Deze stap moet weten uw klassieke VNet resource-ID. De indeling van de resource groep-ID ziet:

        /subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.ClassicNetwork/virtualNetworks/{VirtualNetworkName}

    Zorg ervoor dat SubscriptionID, ResourceGroupName en VirtualNetworkName vervangen door de desbetreffende namen.

    Dit kunt u doen door het volgende:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2

3. Eenmaal de VNet peering koppeling is gemaakt, ziet u de status van de koppeling zoals in de volgende uitvoer:

        Name                             : LinkToVNet2
        Id                               : /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeerings/LinkToVNet2
        Etag                             : W/"acecbd0f-766c-46be-aa7e-d03e41c46b16"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                         "Id": "/subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                       }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

## <a name="remove-vnet-peering"></a>Peering VNet verwijderen

1.  U moet de peering VNet verwijderen, de volgende cmdlet uitvoert:

        Remove-AzureRmVirtualNetworkPeering  

        Remove both links, using the following commands:

        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2
        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2

2. Zodra u een koppeling in een VNET peering verwijdert, gaat de verbindingsstatus peer naar verbroken. In deze toestand niet kunt u de koppeling opnieuw maken totdat de status van de koppeling peer gestarte verandert. Het is raadzaam dat u beide koppelingen verwijderen voordat u opnieuw de VNet peering maken.
