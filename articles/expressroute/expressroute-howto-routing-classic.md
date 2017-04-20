<properties
   pageTitle="Het configureren van routering voor een ExpressRoute circuit voor de klassieke implementatiemodel met PowerShell | Microsoft Azure"
   description="Dit artikel begeleidt u door de stappen voor het maken en het particuliere, openbare en Microsoft peering van een circuit ExpressRoute inrichten. In dit artikel ziet u ook het controleren van de status, bijwerken of verwijderen van peerings voor het circuit."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr"/>

# <a name="create-and-modify-routing-for-an-expressroute-circuit"></a>Maken en wijzigen voor een circuit ExpressRoute routering

> [AZURE.SELECTOR]
[Azure Portal - Resource Manager](expressroute-howto-routing-portal-resource-manager.md)
[PowerShell - Resource Manager](expressroute-howto-routing-arm.md)
[PowerShell - klassiek](expressroute-howto-routing-classic.md)



Dit artikel begeleidt u door de stappen voor het maken en beheren van de configuratie van de routering voor een ExpressRoute circuit met PowerShell en het klassieke implementatiemodel. De volgende stappen ziet u ook het controleren van de status, update of delete en deprovision peerings voor een ExpressRoute circuit.


**Over de Azure-implementatie**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>Vereisten voor configuratie

- U moet de meest recente versie van de Azure PowerShell-modules. De meest recente PowerShell-module kunt u downloaden van de PowerShell-sectie van de [pagina Downloads Azure](https://azure.microsoft.com/downloads/). Volg de instructies op de pagina voor [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor stapsgewijze instructies voor het configureren van de computer voor het gebruik van de Azure PowerShell-modules. 
- Zorg ervoor dat u de pagina [vereisten](expressroute-prerequisites.md) en de [vereisten voor routering](expressroute-routing.md) pagina de pagina [werkstromen](expressroute-workflows.md) hebt gecontroleerd voordat u configuratie.
- U hebt een actieve ExpressRoute circuit. Volg de instructies voor het [maken van een circuit ExpressRoute](expressroute-howto-circuit-classic.md) en hebben het circuit door uw provider verbinding ingeschakeld voordat u verdergaat. Het circuit ExpressRoute moet zijn ingericht en de ingeschakelde status voor te kunnen uitvoeren van de cmdlets die hieronder worden beschreven.

>[AZURE.IMPORTANT] Deze instructies zijn alleen van toepassing op circuits gemaakt met serviceproviders bieden services voor laag 2-connectiviteit. Als u een serviceprovider biedt managed Layer 3-services (doorgaans een IPVPN, zoals MPLS), uw provider verbinding configureren en beheren van routering voor u.

U kunt een, twee of alle drie peerings (Azure private, Azure publiek en Microsoft) voor een ExpressRoute circuit. U kunt de peerings in elke gewenste volgorde. Echter, moet u ervoor zorgen dat u de configuratie van elk peering tegelijk uitvoeren. 

## <a name="azure-private-peering"></a>Azure private peering

Deze sectie bevat instructies voor het maken, ophalen, bijwerken en verwijderen van de Azure private peering configuratie voor een ExpressRoute circuit. 

### <a name="to-create-azure-private-peering"></a>Azure private peering maken

1. **De PowerShell-module voor ExpressRoute importeren.**
    
    Om te kunnen starten met de cmdlets ExpressRoute, moet u de Azure en ExpressRoute modules importeren in de PowerShell-sessie. Voer de volgende opdrachten de Azure en ExpressRoute modules importeren in de PowerShell-sessie.  

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **ExpressRoute circuits maken.**
    
    Volg de instructies voor het maken van een [ExpressRoute circuit](expressroute-howto-circuit-classic.md) en deze door de provider verbinding hebt ingericht. Als uw provider verbinding managed Layer 3-services biedt, kunt u uw provider verbinding inschakelen Azure private peering voor u aanvragen. In dat geval hoeft u niet te volgen instructies vermeld in de volgende secties. Volg de onderstaande instructies echter als uw provider verbinding voor u, na het maken van uw circuit, routering niet beheerd. 

3. **Controleer het circuit ExpressRoute om ervoor te zorgen dat is ingericht.**

    U moet controleren of het circuit ExpressRoute is ingericht en ook ingeschakeld. Zie het voorbeeld hieronder.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Zorg ervoor dat het circuit Provisioned en ingeschakeld bevat. Als dat niet het geval is, samen met uw provider verbinding met uw circuit met de vereiste status en de status opvragen.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled


4. **Configureer Azure private peering voor het circuit.**

    Zorg ervoor dat u beschikt over het volgende voordat u met de volgende stappen verdergaat:

    - Een /30 subnet voor de primaire koppeling. Dit mag geen deel uitmaken van een adresruimte gereserveerd voor virtuele netwerken.
    - Een /30 subnet voor de tweede koppeling. Dit mag geen deel uitmaken van een adresruimte gereserveerd voor virtuele netwerken.
    - Een geldige VLAN ID vast te stellen deze peering op. Zorg ervoor dat er geen bij andere peering in het circuit dezelfde VLAN-ID gebruikt.
    - Als getal voor peering. U kunt zowel 2-byte en 4-byte als getallen. U kunt een particulier als nummer voor deze peering. Zorg ervoor dat u geen 65515 gebruikt.
    - Een MD5-hash als u kiest voor een. **Dit is optioneel**.
    
    U kunt de volgende cmdlet Azure private peering voor uw circuit configureren uitvoeren.

        New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100

    U kunt de cmdlet hieronder als u kiest voor een MD5-hash.

        New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100 -SharedKey "A1B2C3D4"

    >[AZURE.IMPORTANT] Zorg ervoor dat u uw AS-nummer als peering ASN, geen klant ASN opgeven.

### <a name="to-view-azure-private-peering-details"></a>Azure private peering details weergeven

U krijgt met de volgende cmdlet configuratiegegevens

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100


### <a name="to-update-azure-private-peering-configuration"></a>Azure private peering-configuratie bij te werken

U kunt een deel van de configuratie met de volgende cmdlet bijwerken. In het volgende voorbeeld wordt de VLAN-ID van het circuit wordt bijgewerkt van 100 tot 500.

    Set-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 500 -SharedKey "A1B2C3D4"

### <a name="to-delete-azure-private-peering"></a>Azure private peering verwijderen

De configuratie van de peering kunt u verwijderen door de volgende cmdlet wordt uitgevoerd.

>[AZURE.WARNING] U moet ervoor zorgen dat alle virtuele netwerken van het circuit ExpressRoute ontkoppeld voordat deze cmdlet wordt uitgevoerd. 

    Remove-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"


## <a name="azure-public-peering"></a>Azure openbare peering

Deze sectie bevat instructies voor het maken, ophalen, bijwerken en verwijderen de Azure openbare peering configuratie voor een ExpressRoute circuit.

### <a name="to-create-azure-public-peering"></a>Azure openbare peering maken

1. **De PowerShell-module voor ExpressRoute importeren.**
    
    Om te kunnen starten met de cmdlets ExpressRoute, moet u de Azure en ExpressRoute modules importeren in de PowerShell-sessie. Voer de volgende opdrachten de Azure en ExpressRoute modules importeren in de PowerShell-sessie. 

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **ExpressRoute circuits maken**
    
    Volg de instructies voor het maken van een [ExpressRoute circuit](expressroute-howto-circuit-classic.md) en deze door de provider verbinding hebt ingericht. Als uw provider verbinding managed Layer 3-services biedt, kunt u uw provider verbinding inschakelen Azure openbare peering voor u aanvragen. In dat geval hoeft u niet te volgen instructies vermeld in de volgende secties. Volg de onderstaande instructies echter als uw provider verbinding voor u, na het maken van uw circuit, routering niet beheerd.

3. **Controleer ExpressRoute circuit om ervoor te zorgen dat is ingericht.**

    U moet controleren of het circuit ExpressRoute is ingericht en ook ingeschakeld. Zie het voorbeeld hieronder.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Zorg ervoor dat het circuit Provisioned en ingeschakeld bevat. Als dat niet het geval is, samen met uw provider verbinding met uw circuit met de vereiste status en de status opvragen.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled

    

4. **Azure openbare peering voor het circuit configureren**

    Zorg ervoor dat de volgende informatie voordat u verder verder gaat.

    - Een /30 subnet voor de primaire koppeling. Dit moet een geldig voorvoegsel voor openbare IPv4.
    - Een /30 subnet voor de tweede koppeling. Dit moet een geldig voorvoegsel voor openbare IPv4.
    - Een geldige VLAN ID vast te stellen deze peering op. Zorg ervoor dat er geen bij andere peering in het circuit dezelfde VLAN-ID gebruikt.
    - Als getal voor peering. U kunt zowel 2-byte en 4-byte als getallen.
    - Een MD5-hash als u kiest voor een. **Dit is optioneel**.
    
    U kunt de volgende cmdlet Azure openbare peering voor uw circuit configureren uitvoeren

        New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200

    U kunt de cmdlet hieronder als u kiest voor een MD5-hash

        New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200 -SharedKey "A1B2C3D4"

    >[AZURE.IMPORTANT] Zorg ervoor dat u uw AS-nummer als peering ASN en niet de klant ASN opgeven.

### <a name="to-view-azure-public-peering-details"></a>Azure openbare peering details weergeven

U krijgt met de volgende cmdlet configuratiegegevens

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 131.107.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 131.107.0.4/30
    State                          : Enabled
    VlanId                         : 200


### <a name="to-update-azure-public-peering-configuration"></a>Azure openbare peering configuratie bij te werken

U kunt een deel van de configuratie met de volgende cmdlet bijwerken

    Set-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 600 -SharedKey "A1B2C3D4"

De VLAN-ID van het circuit wordt bijgewerkt van 200 tot 600 in het bovenstaande voorbeeld.

### <a name="to-delete-azure-public-peering"></a>Azure openbare peering verwijderen

U kunt de configuratie van uw peering verwijderen door de volgende cmdlet wordt uitgevoerd

    Remove-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

## <a name="microsoft-peering"></a>Microsoft peering

Deze sectie bevat instructies voor het maken, ophalen, bijwerken en verwijderen van de peering configuratie van Microsoft voor een ExpressRoute circuit. 

### <a name="to-create-microsoft-peering"></a>Microsoft peering maken

1. **De PowerShell-module voor ExpressRoute importeren.**
    
    Om te kunnen starten met de cmdlets ExpressRoute, moet u de Azure en ExpressRoute modules importeren in de PowerShell-sessie. Voer de volgende opdrachten de Azure en ExpressRoute modules importeren in de PowerShell-sessie.  

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **ExpressRoute circuits maken**
    
    Volg de instructies voor het maken van een [ExpressRoute circuit](expressroute-howto-circuit-classic.md) en deze door de provider verbinding hebt ingericht. Als uw provider verbinding managed Layer 3-services biedt, kunt u uw provider verbinding inschakelen Azure private peering voor u aanvragen. In dat geval hoeft u niet te volgen instructies vermeld in de volgende secties. Volg de onderstaande instructies echter als uw provider verbinding voor u, na het maken van uw circuit, routering niet beheerd.

3. **Controleer ExpressRoute circuit om ervoor te zorgen dat is ingericht.**

    U moet controleren om te zien of het circuit ExpressRoute Provisioned en ingeschakeld staat.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Zorg ervoor dat het circuit Provisioned en ingeschakeld bevat. Als dat niet het geval is, samen met uw provider verbinding met uw circuit met de vereiste status en de status opvragen.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled


4. **Configureren van Microsoft voor het circuit peering**

    Zorg ervoor dat u hebt de volgende informatie voordat u verdergaat.

    - Een /30 subnet voor de primaire koppeling. Dit moet een geldige openbare IPv4-voorvoegsel eigendom en geregistreerd zijn in een RIR / IR.
    - Een /30 subnet voor de tweede koppeling. Dit moet een geldige openbare IPv4-voorvoegsel eigendom en geregistreerd zijn in een RIR / IR.
    - Een geldige VLAN ID vast te stellen deze peering op. Zorg ervoor dat er geen bij andere peering in het circuit dezelfde VLAN-ID gebruikt.
    - Als getal voor peering. U kunt zowel 2-byte en 4-byte als getallen.
    - Aangekondigde voorvoegsels: U dient een lijst van alle nummers die u van plan bent om te adverteren via het BGP-sessie. Alleen openbare IP-adresprefixen worden geaccepteerd. U kunt een door komma's gescheiden lijst verzenden als u van plan bent voor het verzenden van een set van voorvoegsels. Deze voorvoegsels voor u moeten zijn geregistreerd in een RIR / IR.
    - ASN klant: Als u reclame voorvoegsels die niet zijn geregistreerd op de peering als getal, kunt u het nummer van de AS waaraan ze zijn geregistreerd. **Dit is optioneel**.
    - Routering registernaam: Kunt u de RIR / IR waartegen de AS en de voorvoegsels zijn geregistreerd.
    - Een MD5-hash, als u kiest voor een. **Dit is optioneel.**
    
    U kunt de volgende cmdlet Microsoft pering voor uw circuit configureren uitvoeren

        New-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"


### <a name="to-view-microsoft-peering-details"></a>Microsoft peering details weergeven

U kunt met de volgende cmdlet configuratiegegevens krijgen.

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 123.0.0.0/30
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 2245
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : ARIN
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 300


### <a name="to-update-microsoft-peering-configuration"></a>Microsoft peering-configuratie bij te werken

U kunt een deel van de configuratie met de volgende cmdlet bijwerken.

        Set-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"

### <a name="to-delete-microsoft-peering"></a>Microsoft peering verwijderen

De configuratie van de peering kunt u verwijderen door de volgende cmdlet wordt uitgevoerd.

    Remove-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

## <a name="next-steps"></a>Volgende stappen

Volgende, [een VNet op een circuit ExpressRoute koppeling](expressroute-howto-linkvnet-classic.md).


-  Zie voor meer informatie over workflows [ExpressRoute werkstromen](expressroute-workflows.md).
-  Zie voor meer informatie over peering circuit [ExpressRoute circuits en routering domeinen](expressroute-circuit-peerings.md).

