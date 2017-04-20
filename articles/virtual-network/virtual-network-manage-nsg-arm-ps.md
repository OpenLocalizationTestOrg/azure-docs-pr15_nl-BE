<properties 
   pageTitle="Beheren met PowerShell in Resource Manager NSGs | Microsoft Azure"
   description="Informatie over het beheren van de huidige NSGs met PowerShell in Resource Manager"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jdial" />

# <a name="manage-nsgs-using-powershell"></a>NSGs met PowerShell beheren

[AZURE.INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel.

[AZURE.INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="retrieve-information"></a>Gegevens ophalen

U kunt uw bestaande NSGs weergeven, ophalen van regels voor een bestaande NSG en weten welke bronnen een NSG is gekoppeld aan.

### <a name="view-existing-nsgs"></a>Bestaande NSGs weergeven
Uitvoeren om alle bestaande NSGs in een abonnement bekijken, de `Get-AzureRmNetworkSecurityGroup` cmdlet zoals hieronder wordt weergegeven.

    Get-AzureRmNetworkSecurityGroup

Verwacht resultaat:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 :                         
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
    
    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
                            
    Name                 : WEB1
    ResourceGroupName    : RG101
    Location             : eastus2
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG101/providers/M
                           icrosoft.Network/networkSecurityGroups/WEB1
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]


Uitvoeren om de lijst met NSGs weergeven in een bepaalde resourcegroep, de `Get-AzureRmNetworkSecurityGroup` cmdlet zoals hieronder wordt weergegeven. 

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG

Verwachte output:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 :                         
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
    
    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
         
### <a name="list-all-rules-for-an-nsg"></a>Lijst van alle regels voor een NSG

Uitvoeren als u wilt weergeven op de regels van een NSG **NSG-FrontEnd**met de naam, de `Get-AzureRmNetworkSecurityGroup` cmdlet zoals hieronder wordt weergegeven. 

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd | Select SecurityRules -ExpandProperty SecurityRules

Verwachte output:
    
    Name                     : rdp-rule
    Id                       : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/                        Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule
    Etag                     : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState        : Succeeded
    Description              : Allow RDP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 100
    Direction                : Inbound
    
    Name                     : web-rule
    Id                       : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/                        Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule
    Etag                     : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState        : Succeeded
    Description              : Allow HTTP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 80
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 101
    Direction                : Inbound

>[AZURE.NOTE] U kunt ook `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` om de standaardregels van de **NSG-FrontEnd** NSG.

### <a name="view-nsgs-associations"></a>NSGs koppelingen weergeven

Uitvoeren om te bekijken de NSG **NSG-FrontEnd is** resources koppelen aan, de `Get-AzureRmNetworkSecurityGroup` cmdlet zoals hieronder wordt weergegeven.

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd

Zoek de eigenschappen **NetworkInterfaces** en **subnetten** , zoals hieronder wordt weergegeven:

    NetworkInterfaces    : []
    Subnets              : [
                             {
                               "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                               "IpConfigurations": []
                             }
                           ]

In het bovenstaande voorbeeld de NSG is niet gekoppeld aan alle netwerkinterfaces (NIC's) en is gekoppeld aan een subnet genaamd **FrontEnd**.

## <a name="manage-rules"></a>Regels beheren

U kunt regels toevoegen aan een bestaande NSG, de bestaande regels bewerken en verwijderen van regels.

### <a name="add-a-rule"></a>Een regel toevoegen

Een regel die **binnenkomend** verkeer op poort **443** waardoor vanaf een willekeurige computer op de **NSG-FrontEnd** NSG toe, de volgende stappen uit te voeren.

1. Voer het `Get-AzureRmNetworkSecurityGroup` cmdlet de bestaande NSG ophalen en opslaan in een variabele, zoals hieronder wordt weergegeven.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Voer het `Add-AzureRmNetworkSecurityRuleConfig` cmdlet, zoals hieronder wordt weergegeven.

        Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule `
            -Description "Allow HTTPS" `
            -Access Allow `
            -Protocol Tcp `
            -Direction Inbound `
            -Priority 102 `
            -SourceAddressPrefix * `
            -SourcePortRange * `
            -DestinationAddressPrefix * `
            -DestinationPortRange 443

3. Uitvoeren om de wijzigingen in de NSG opslaan, de `Set-AzureRmNetworkSecurityGroup` cmdlet zoals hieronder wordt weergegeven.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Verwachte output alleen de regels weergegeven:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "*",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="change-a-rule"></a>Een regel wijzigen

Als u wilt wijzigen in de regel voor het binnenkomende verkeer vanaf het **Internet** alleen toestaan dat hierboven is gemaakt, de volgende stappen uit te voeren.

1. Voer het `Get-AzureRmNetworkSecurityGroup` cmdlet de bestaande NSG ophalen en opslaan in een variabele, zoals hieronder wordt weergegeven.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Voer het `Set-AzureRmNetworkSecurityRuleConfig` cmdlet, zoals hieronder wordt weergegeven.

        Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule `
            -Description "Allow HTTPS" `
            -Access Allow `
            -Protocol Tcp `
            -Direction Inbound `
            -Priority 102 `
            -SourceAddressPrefix * `
            -SourcePortRange Internet `
            -DestinationAddressPrefix * `
            -DestinationPortRange 443

3. Uitvoeren om de wijzigingen in de NSG opslaan, de `Set-AzureRmNetworkSecurityGroup` cmdlet zoals hieronder wordt weergegeven.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Verwachte output alleen de regels weergegeven:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "Internet",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="delete-a-rule"></a>Een regel verwijderen

1. Voer het `Get-AzureRmNetworkSecurityGroup` cmdlet de bestaande NSG ophalen en opslaan in een variabele, zoals hieronder wordt weergegeven.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Voer het `Remove-AzureRmNetworkSecurityRuleConfig` cmdlet, zoals hieronder wordt weergegeven.

        Remove-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule

3. Uitvoeren om de wijzigingen in de NSG opslaan, de `Set-AzureRmNetworkSecurityGroup` cmdlet zoals hieronder wordt weergegeven.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Verwachte output met alleen de veiligheidsregels aankondiging de **https-regel** wordt niet meer vermeld:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 }
                               ]

## <a name="manage-associations"></a>Koppelingen beheren

U kunt een NSG met subnetten en NIC's koppelen. U kunt ook een NSG van alle bronnen die met bijbehorende koppeling.

### <a name="associate-an-nsg-to-a-nic"></a>Een NSG naar een NIC koppelen

Als u wilt koppelen de **NSG-FrontEnd** NSG naar **TestNICWeb1** NIC, volg de onderstaande stappen.

1. Voer het `Get-AzureRmNetworkSecurityGroup` cmdlet de bestaande NSG ophalen en opslaan in een variabele, zoals hieronder wordt weergegeven.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Voer het `Get-AzureRmNetworkInterface` cmdlet de bestaande NIC ophalen en opslaan in een variabele, zoals hieronder wordt weergegeven.

        $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG `
            -Name TestNICWeb1

3. De eigenschap **NetworkSecurityGroup** van het **NIC** -variabele ingesteld op de waarde van de variabele **NSG** zoals hieronder wordt weergegeven.

        $nic.NetworkSecurityGroup = $nsg

4. Uitvoeren om de wijzigingen in de NIC opslaan, de `Set-AzureRmNetworkInterface` cmdlet zoals hieronder wordt weergegeven.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Verwachte output met alleen de eigenschap **NetworkSecurityGroup** :

        NetworkSecurityGroup : {
                                 "SecurityRules": [],
                                 "DefaultSecurityRules": [],
                                 "NetworkInterfaces": [],
                                 "Subnets": [],
                                 "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                               }

### <a name="dissociate-an-nsg-from-a-nic"></a>Koppeling van een NSG van een NIC

Volg de onderstaande stappen om de koppeling **NSG FrontEnd** NSG uit **TestNICWeb1** NIC.

1. Voer het `Get-AzureRmNetworkSecurityGroup` cmdlet de bestaande NSG ophalen en opslaan in een variabele, zoals hieronder wordt weergegeven.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Voer het `Get-AzureRmNetworkInterface` cmdlet de bestaande NIC ophalen en opslaan in een variabele, zoals hieronder wordt weergegeven.

        $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG `
            -Name TestNICWeb1

3. Stel de eigenschap **NetworkSecurityGroup** van de variabele **NIC** op **$null**, zoals hieronder wordt weergegeven.

        $nic.NetworkSecurityGroup = $null

4. Uitvoeren om de wijzigingen in de NIC opslaan, de `Set-AzureRmNetworkInterface` cmdlet zoals hieronder wordt weergegeven.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Verwachte output met alleen de eigenschap **NetworkSecurityGroup** :

        NetworkSecurityGroup : null

### <a name="dissociate-an-nsg-from-a-subnet"></a>Koppeling van een NSG van een subnet

Volg de onderstaande stappen om de **NSG-FrontEnd** NSG koppeling van het subnet **FrontEnd** .

1. Voer het `Get-AzureRmVirtualNetwork` cmdlet voor het ophalen van de bestaande VNet en opslaan in een variabele, zoals hieronder wordt weergegeven.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG `
            -Name TestVNet

2. Voer het `Get-AzureRmVirtualNetworkSubnetConfig` cmdlet het subnet **FrontEnd** ophalen en opslaan in een variabele, zoals hieronder wordt weergegeven.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
            -Name FrontEnd 

3. Stel de eigenschap **NetworkSecurityGroup** van de variabele **subnet** op **$null**, zoals hieronder wordt weergegeven.

        $subnet.NetworkSecurityGroup = $null

4. Uitvoeren om de wijzigingen op het subnet wilt opslaan, de `Set-AzureRmVirtualNetwork` cmdlet zoals hieronder wordt weergegeven.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Verwachte output waarin alleen de eigenschappen van het subnet **FrontEnd** . Zoals u ziet, dat er is niet een eigenschap voor **NetworkSecurityGroup**:

            ...
            Subnets           : [
                                  {
                                    "Name": "FrontEnd",
                                    "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [
                                      {
                                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1"
                                      },
                                      {
                                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1"
                                      }
                                    ],
                                    "ProvisioningState": "Succeeded"
                                  },
                                    ...
                                ]

### <a name="associate-an-nsg-to-a-subnet"></a>Een NSG met een subnet koppelen

Als u wilt koppelen **NSG FrontEnd** NSG opnieuw naar het subnet **FronEnd** , volg de onderstaande stappen.

1. Voer het `Get-AzureRmVirtualNetwork` cmdlet voor het ophalen van de bestaande VNet en opslaan in een variabele, zoals hieronder wordt weergegeven.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG `
            -Name TestVNet

2. Voer het `Get-AzureRmVirtualNetworkSubnetConfig` cmdlet het subnet **FrontEnd** ophalen en opslaan in een variabele, zoals hieronder wordt weergegeven.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
            -Name FrontEnd 

1. Voer het `Get-AzureRmNetworkSecurityGroup` cmdlet de bestaande NSG ophalen en opslaan in een variabele, zoals hieronder wordt weergegeven.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

3. Stel de eigenschap **NetworkSecurityGroup** van de variabele **subnet** op **$null**, zoals hieronder wordt weergegeven.

        $subnet.NetworkSecurityGroup = $nsg

4. Uitvoeren om de wijzigingen op het subnet wilt opslaan, de `Set-AzureRmVirtualNetwork` cmdlet zoals hieronder wordt weergegeven.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Verwachte output met alleen de eigenschap **NetworkSecurityGroup** van het subnet **FrontEnd** :

        ...
        "NetworkSecurityGroup": {
                                  "SecurityRules": [],
                                  "DefaultSecurityRules": [],
                                  "NetworkInterfaces": [],
                                  "Subnets": [],
                                  "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                }
        ...

## <a name="delete-an-nsg"></a>Een NSG verwijderen

U kunt een NSG alleen verwijderen als deze niet aan een resource zijn gekoppeld. U verwijdert een NSG, volg de onderstaande stappen.

1. Uitvoeren om te controleren van de resources die zijn gekoppeld aan een NSG, de `azure network nsg show` zoals weergegeven in de [weergave NSGs verenigingen](#View-NSGs-associations).
2. Als de NSG gekoppeld aan een NIC's is, voert u de `azure network nic set` zoals in [Dissociate een NSG van een NIC](#Dissociate-an-NSG-from-a-NIC) voor elke NIC 
3. Als de NSG gekoppeld aan een subnet is, voert u de `azure network vnet subnet set` zoals in [Dissociate een NSG van een subnet](#Dissociate-an-NSG-from-a-subnet) voor elk subnet.
4. Uitvoeren als u wilt verwijderen in de NSG, de `Remove-AzureRmNetworkSecurityGroup` cmdlet zoals hieronder wordt weergegeven.

        Remove-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd -Force

    >[AZURE.NOTE] De **-Force** parameter zorgt dat u niet hoeft te bevestigen.

## <a name="next-steps"></a>Volgende stappen

- [Logboekregistratie inschakelen](virtual-network-nsg-manage-log.md) voor NSGs.