<properties
   pageTitle="VNet Peering met behulp van bronbeheer sjablonen maken | Microsoft Azure"
   description="Informatie over het maken van een virtueel netwerk peering met de sjablonen in Resource Manager."
   services="virtual-network"
   documentationCenter=""
   authors="narayanannamalai"
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
   ms.author="narayanannamalai;annahar"/>

# <a name="create-vnet-peering-using-resource-manager-templates"></a>VNet Peering met behulp van bronbeheer sjablonen maken

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Als u wilt een VNet peering met behulp van bronbeheer sjablonen maken, neem de volgende stappen:

1. Als u Azure PowerShell nog nooit hebt gebruikt, Zie [installeren en configureren van Azure PowerShell](../powershell-install-configure.md) en volg de instructies helemaal naar het einde ondertekenen in Azure en selecteer uw abonnement.

    > [AZURE.NOTE] Voor het beheren van VNet peering de PowerShell-cmdlet wordt geleverd met [Azure PowerShell 1.6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. De onderstaande tekst toont de definitie van een peering VNet-koppeling voor VNet1 VNet2, op basis van het bovenstaande scenario. De onderstaande inhoud kopiëren en opslaan als een bestand met de naam VNetPeeringVNet1.json.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToVNet2",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet2')]"       
        }
            }
            }
        ]
        }

3. De onderstaande sectie bevat de definitie van een peering VNet-koppeling voor VNet2 aan de VNet1, op basis van het bovenstaande scenario.  De onderstaande inhoud kopiëren en opslaan als een bestand met de naam VNetPeeringVNet2.json.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet2/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
        ]
        }

    Zoals in de bovenstaande sjabloon, zijn er een aantal configureerbare eigenschappen voor peering VNet:

  	|Optie|Beschrijving|Standaard|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|Wel of niet de adresruimte van een peer-VNet maakt deel uit van de tag virtual_network.|Ja|
  	|AllowForwardedTraffic|Verkeer dat afkomstig is uit een peered VNet niet is geaccepteerd of verwijderd.|Nee|
  	|AllowGatewayTransit|Hiermee kunt u de peer VNet gebruik van de gateway VNet.|Nee|
  	|UseRemoteGateways|Van de peer VNet gateway gebruiken. De peer VNet moet een gateway geconfigureerd en AllowGatewayTransit geselecteerd. U kunt deze optie niet gebruiken als u een gateway geconfigureerd.|Nee|

    Elke koppeling in VNet peering heeft de bovenstaande eigenschappen. U kunt bijvoorbeeld AllowVirtualNetworkAccess ingesteld op True voor peering VNet-link VNet1 VNet2 en stel deze in op False voor de peering VNet-koppeling in de andere richting.


4. U kunt de cmdlet New-AzureRmResourceGroupDeployment maken of bijwerken van de implementatie uitvoeren voor de implementatie van het sjabloonbestand. Raadpleeg dit [artikel](../resource-group-template-deploy.md)voor meer informatie over het gebruik van Resource Manager-sjablonen.

        New-AzureRmResourceGroupDeployment -ResourceGroupName <resource group name> -TemplateFile <template file path> -DeploymentDebugLogLevel all

    > [AZURE.NOTE] Vervang de groep naam en de sjabloononderdeel bronbestand indien nodig.

    Hieronder is een voorbeeld op basis van het bovenstaande scenario:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet1.json -DeploymentDebugLogLevel all

    Uitvoer wordt weergegeven:

        DeploymentName      : VNetPeeringVNet1
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:05:03 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet2.json -DeploymentDebugLogLevel all

    Uitvoer wordt weergegeven:

        DeploymentName      : VNetPeeringVNet2
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:07:22 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

5. Nadat de installatie is voltooid, kunt u de cmdlet weer te geven van de peering staat hieronder uitvoeren:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNet1 -ResourceGroupName VNet101 -Name linktoVNet2

    Uitvoer wordt weergegeven:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : VNet101
        VirtualNetworkName  : VNet1
        ProvisioningState       : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    Nadat peering is gevestigd in dit scenario, wees de verbindingen van de virtuele machine met een virtuele machine in beide VNets te initiëren. Standaard AllowVirtualNetworkAccess True is en de juiste ACL's zodat de communicatie tussen de VNets VNet peering wordt ingericht. U kunt nog steeds toepassen network group (NSG) beveiligingsregels voor verbindingen tussen specifieke subnetten of virtuele machines toegang krijgt tot fijnmazige toegang tussen twee virtuele netwerken geblokkeerd.  Raadpleeg dit [artikel](virtual-networks-create-nsg-arm-ps.md)voor meer informatie voor het maken van regels voor NSG.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

U maakt een VNet peering over abonnementen, neem de volgende stappen:

1. Aanmelden Azure met bevoorrechte gebruiker A de rekening in het abonnement A en de volgende cmdlet uitvoert:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet5

    Dit is niet vereist, peering kan worden ingesteld zelfs als gebruikers peering aanvragen voor hun respectieve Vnets afzonderlijk verhogen als de aanvragen voldoen aan. Een bevoegde gebruiker van de andere VNet toe te voegen als gebruikers in de lokale VNet gemakkelijker te doen van de instellingen.

2. Voor aanmelden bij Azure met beschermde-van gebruiker B account abonnement-B en de volgende cmdlet uitvoert:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet3

3. Gebruiker A's in login sessie, deze cmdlet uitvoeren:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet3.json -DeploymentDebugLogLevel all

    Hier ziet u hoe de JSON-bestand is gedefinieerd.  

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet3/LinkToVNet5",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/<Subscription-B-ID>/resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet5"
                }
            }
            }
        ]
        }

4. In de aanmeldingssessie van de gebruiker-B, voeren de volgende cmdlet:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet5.json -DeploymentDebugLogLevel all

    Hier ziet u hoe de JSON-bestand is gedefinieerd:

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet5/LinkToVNet3",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/Subscription-A-ID /resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet3"
                }
            }
            }
        ]
        }

    Nadat de peering in dit scenario is gevestigd, wees de verbindingen vanaf een virtuele machine met een virtuele machine van beide VNets te initiëren over verschillende abonnementen.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. In dit scenario kunt u de onderstaande vast te stellen de VNet peering voorbeeldsjabloon implementeren.  U moet de eigenschap AllowForwardedTraffic op True, waardoor het virtuele netwerk-toestel in de peered VNet voor het verzenden en ontvangen van verkeer.

    Hier is de sjabloon voor het maken van een peering van HubVNet naar VNet1 VNet. Houd er rekening mee dat de AllowForwardedTraffic is ingesteld op false.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "HubVNet/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
            }
        ]
        }

2. Hier is de sjabloon voor het maken van een peering van VNet1 naar HubVnet VNet. Opmerking: AllowForwardedTraffic is ingesteld op true.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToHubVNet",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": true,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'HubVnet')]"       
                }
            }
            }
        ]
        }


3. Nadat peering is gemaakt, kunt u verwijzen naar dit [artikel](virtual-network-create-udr-arm-ps.md) om te definiëren door de gebruiker gedefinieerde routes(UDR) VNet1 verkeer omgeleid via een virtueel toestel gebruik van de mogelijkheden ervan. Als u de next-hopadres in de route opgeeft, stelt u deze naar het IP-adres van het virtuele toestel in de peer VNet HubVNet.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

U maakt een peering tussen virtuele netwerken van verschillende modellen, volg de onderstaande stappen:
1. De onderstaande tekst bevat de definitie van een peering VNet-koppeling voor VNET1 aan VNET2 in dit scenario. Slechts één koppeling is vereist voor een klassieke virtueel netwerk met een virtueel netwerk van Azure resource manager van peer.

    Plaats in uw abonnements-ID voor waar de klassieke virtueel netwerk- of VNET2 zich bevindt en de MyResouceGroup wijzigen in de naam van de desbetreffende bron.

    {"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#", "contentVersion": "1.0.0.0", "parameters": {}, "variabelen": {}, 'bronnen': [{'apiVersion': '2016-06-01","type":"Microsoft.Network/virtualNetworks/virtualNetworkPeerings","naam": ' VNET1/LinkToVNET2', 'locatie':"[.location resourceGroup-()]", 'Eigenschappen': {'allowVirtualNetworkAccess': true, 'allowForwardedTraffic': false,"allowGatewayTransit": false,"useRemoteGateways": false,"remoteVirtualNetwork": {"id":" [resourceId (' Microsoft.ClassicNetwork/virtualNetworks', 'VNET2')] "}}}]}

2. Voer de volgende cmdlet maken of bijwerken van de implementatie voor de implementatie van het sjabloonbestand.

        New-AzureRmResourceGroupDeployment -ResourceGroupName MyResourceGroup -TemplateFile .\VnetPeering.json -DeploymentDebugLogLevel all

        Output shows:

        DeploymentName          : VnetPeering
        ResourceGroupName       : MyResourceGroup
        ProvisioningState       : Succeeded
        Timestamp               : XX/XX/YYYY 5:42:33 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
        Outputs                 :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

3. Nadat de installatie is voltooid, kunt u de volgende cmdlet wilt weergeven van de peering staat uitvoeren:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNET1 -ResourceGroupName MyResourceGroup -Name LinkToVNET2

        Output shows:

        Name                             : LinkToVNET2
        Id                               : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResource
                                   Group/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeering
                                   s/LinkToVNET2
        Etag                             : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                     "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/M
                                   yResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                   }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

Nadat peering is gemaakt tussen een klassieke VNet en een resourcemanager VNet, wees initiëren van verbindingen vanaf een virtuele machine in VNET1 met een virtuele machine in VNET2 en vice versa.
