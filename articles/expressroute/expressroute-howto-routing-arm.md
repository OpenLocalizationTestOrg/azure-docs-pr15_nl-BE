<properties
   pageTitle="Het configureren van routering voor een circuit ExpressRoute | Microsoft Azure"
   description="Dit artikel begeleidt u door de stappen voor het maken en het particuliere, openbare en Microsoft peering van een circuit ExpressRoute inrichten. In dit artikel ziet u ook het controleren van de status, bijwerken of verwijderen van peerings voor het circuit."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="hero-article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/05/2016"
   ms.author="ganesr"/>

# <a name="create-and-modify-routing-for-an-expressroute-circuit"></a>Maken en wijzigen voor een circuit ExpressRoute routering


> [AZURE.SELECTOR]
[Azure Portal - Resource Manager](expressroute-howto-routing-portal-resource-manager.md)
[PowerShell - Resource Manager](expressroute-howto-routing-arm.md)
[PowerShell - klassiek](expressroute-howto-routing-classic.md)



Dit artikel begeleidt u door de stappen voor het maken en beheren van de configuratie van de routering voor een ExpressRoute circuit met PowerShell en Azure Resource Manager-implementatiemodel.  De volgende stappen ziet u ook het controleren van de status, update of delete en deprovision peerings voor een ExpressRoute circuit. 


**Over de Azure-implementatie**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>Vereisten voor configuratie

- U moet de meest recente versie van de modules Azure PowerShell versie 1.0 of hoger. 
- Zorg ervoor dat u de pagina [vereisten](expressroute-prerequisites.md) en de [vereisten voor routering](expressroute-routing.md) pagina de pagina [werkstromen](expressroute-workflows.md) hebt gecontroleerd voordat u configuratie.
- U hebt een actieve ExpressRoute circuit. Volg de instructies op [een circuit ExpressRoute maken](expressroute-howto-circuit-arm.md) en hebben het circuit door uw provider verbinding ingeschakeld voordat u verdergaat. Het circuit ExpressRoute moet zijn ingericht en de ingeschakelde status voor te kunnen uitvoeren van de cmdlets die hieronder worden beschreven.

Deze instructies zijn alleen van toepassing op circuits gemaakt met serviceproviders bieden services voor laag 2-connectiviteit. Als u een serviceprovider biedt managed Layer 3-services (doorgaans een IPVPN, zoals MPLS), uw provider verbinding configureren en beheren van routering voor u.

>[AZURE.IMPORTANT] Wij adverteren op dit moment geen peerings geconfigureerd door serviceproviders via de service management portal. Wij werken voor het inschakelen van deze mogelijkheid snel. Neem contact op met uw serviceprovider voor BGP peerings configureren.

U kunt een, twee of alle drie peerings (Azure private, Azure publiek en Microsoft) voor een ExpressRoute circuit. U kunt de peerings in elke gewenste volgorde. Echter, moet u ervoor zorgen dat u de configuratie van elk peering tegelijk uitvoeren. 

## <a name="azure-private-peering"></a>Azure private peering

Deze sectie bevat instructies voor het maken, ophalen, bijwerken en verwijderen van de Azure private peering configuratie voor een ExpressRoute circuit. 

### <a name="to-create-azure-private-peering"></a>Azure private peering maken

1. De PowerShell-module voor ExpressRoute importeren.
    
    U moet de nieuwste PowerShell installer installeren uit [De galerie PowerShell](http://www.powershellgallery.com/) en Azure Resource Manager modules importeren in de PowerShell-sessie gestart met de ExpressRoute-cmdlets. U moet de PowerShell uitvoeren als beheerder.

        Install-Module AzureRM

        Install-AzureRM

    Alle modules van de AzureRM.* binnen het bereik van de bekende semantische versie importeren

        Import-AzureRM

    U kunt een module selecteren in het bereik van de bekende semantische versie ook gewoon importeren 
        
        Import-Module AzureRM.Network 

    Aanmelden bij uw account

        Login-AzureRmAccount

    Selecteer het abonnement dat u wilt maken, ExpressRoute circuit
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. ExpressRoute circuits maken.
    
    Volg de instructies voor het maken van een [ExpressRoute circuit](expressroute-howto-circuit-arm.md) en deze door de provider verbinding hebt ingericht. 

    Als uw provider verbinding managed Layer 3-services biedt, kunt u uw provider verbinding inschakelen Azure private peering voor u aanvragen. In dat geval hoeft u niet te volgen instructies vermeld in de volgende secties. Volg de onderstaande instructies echter als uw provider verbinding voor u, na het maken van uw circuit, routering niet beheerd. 

3. Controleer het circuit ExpressRoute om ervoor te zorgen dat is ingericht.

    U moet controleren of het circuit ExpressRoute is ingericht en ook ingeschakeld. Zie het voorbeeld hieronder.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Het antwoord is zoals in het onderstaande voorbeeld:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []


4. Configureer Azure private peering voor het circuit.

    Zorg ervoor dat u beschikt over het volgende voordat u met de volgende stappen verdergaat:

    - Een /30 subnet voor de primaire koppeling. Dit mag geen deel uitmaken van een adresruimte gereserveerd voor virtuele netwerken.
    - Een /30 subnet voor de tweede koppeling. Dit mag geen deel uitmaken van een adresruimte gereserveerd voor virtuele netwerken.
    - Een geldige VLAN ID vast te stellen deze peering op. Zorg ervoor dat er geen bij andere peering in het circuit dezelfde VLAN-ID gebruikt.
    - Als getal voor peering. U kunt zowel 2-byte en 4-byte als getallen. U kunt een particulier als nummer voor deze peering. Zorg ervoor dat u geen 65515 gebruikt.
    - Een MD5-hash als u kiest voor een. **Dit is optioneel**.
    
    U kunt de volgende cmdlet Azure private peering voor uw circuit configureren uitvoeren.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    U kunt de cmdlet hieronder als u kiest voor een MD5-hash.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200  -SharedKey "A1B2C3D4"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    >[AZURE.IMPORTANT] Zorg ervoor dat u uw AS-nummer als peering ASN, geen klant ASN opgeven.

### <a name="to-view-azure-private-peering-details"></a>Azure private peering details weergeven

U krijgt met de volgende cmdlet configuratiegegevens

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt   


### <a name="to-update-azure-private-peering-configuration"></a>Azure private peering-configuratie bij te werken

U kunt een deel van de configuratie met de volgende cmdlet bijwerken. In het volgende voorbeeld wordt de VLAN-ID van het circuit wordt bijgewerkt van 100 tot 500.

    Set-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-delete-azure-private-peering"></a>Azure private peering verwijderen

De configuratie van de peering kunt u verwijderen door de volgende cmdlet wordt uitgevoerd.

>[AZURE.WARNING] U moet ervoor zorgen dat alle virtuele netwerken van het circuit ExpressRoute ontkoppeld voordat deze cmdlet wordt uitgevoerd. 

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt



## <a name="azure-public-peering"></a>Azure openbare peering

Deze sectie bevat instructies voor het maken, ophalen, bijwerken en verwijderen de Azure openbare peering configuratie voor een ExpressRoute circuit.

### <a name="to-create-azure-public-peering"></a>Azure openbare peering maken

1. De PowerShell-module voor ExpressRoute importeren.
    
    U moet de nieuwste PowerShell installer installeren uit [De galerie PowerShell](http://www.powershellgallery.com/) en Azure Resource Manager modules importeren in de PowerShell-sessie gestart met de ExpressRoute-cmdlets. U moet de PowerShell uitvoeren als beheerder.

        Install-Module AzureRM

        Install-AzureRM

    Alle modules van de AzureRM.* binnen het bereik van de bekende semantische versie importeren

        Import-AzureRM

    U kunt een module selecteren in het bereik van de bekende semantische versie ook gewoon importeren 
        
        Import-Module AzureRM.Network 

    Aanmelden bij uw account

        Login-AzureRmAccount

    Selecteer het abonnement dat u wilt maken, ExpressRoute circuit
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. ExpressRoute circuits maken.
    
    Volg de instructies voor het maken van een [ExpressRoute circuit](expressroute-howto-circuit-arm.md) en deze door de provider verbinding hebt ingericht. 

    Als uw provider verbinding managed Layer 3-services biedt, kunt u uw provider verbinding inschakelen Azure openbare peering voor u aanvragen. In dat geval hoeft u niet te volgen instructies vermeld in de volgende secties. Volg de onderstaande instructies echter als uw provider verbinding voor u, na het maken van uw circuit, routering niet beheerd.

3. Controleer ExpressRoute circuit om ervoor te zorgen dat is ingericht.

    U moet controleren of het circuit ExpressRoute is ingericht en ook ingeschakeld. Zie het voorbeeld hieronder.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Het antwoord is zoals in het onderstaande voorbeeld:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []   

4. Configureer Azure openbare peering voor het circuit.

    Zorg ervoor dat de volgende informatie voordat u verder verder gaat.

    - Een /30 subnet voor de primaire koppeling. Dit moet een geldig voorvoegsel voor openbare IPv4.
    - Een /30 subnet voor de tweede koppeling. Dit moet een geldig voorvoegsel voor openbare IPv4.
    - Een geldige VLAN ID vast te stellen deze peering op. Zorg ervoor dat er geen bij andere peering in het circuit dezelfde VLAN-ID gebruikt.
    - Als getal voor peering. U kunt zowel 2-byte en 4-byte als getallen.
    - Een MD5-hash als u kiest voor een. **Dit is optioneel**.
    
    U kunt de volgende cmdlet Azure openbare peering voor uw circuit configureren uitvoeren

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    U kunt de cmdlet hieronder als u kiest voor een MD5-hash

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100  -SharedKey "A1B2C3D4"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


    >[AZURE.IMPORTANT] Zorg ervoor dat u uw AS-nummer als peering ASN en niet de klant ASN opgeven.

### <a name="to-view-azure-public-peering-details"></a>Azure openbare peering details weergeven

U krijgt met de volgende cmdlet configuratiegegevens

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt


### <a name="to-update-azure-public-peering-configuration"></a>Azure openbare peering configuratie bij te werken

U kunt een deel van de configuratie met de volgende cmdlet bijwerken

    Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 600 

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

De VLAN-ID van het circuit wordt bijgewerkt van 200 tot 600 in het bovenstaande voorbeeld.

### <a name="to-delete-azure-public-peering"></a>Azure openbare peering verwijderen

U kunt de configuratie van uw peering verwijderen door de volgende cmdlet wordt uitgevoerd

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## <a name="microsoft-peering"></a>Microsoft peering

Deze sectie bevat instructies voor het maken, ophalen, bijwerken en verwijderen van de peering configuratie van Microsoft voor een ExpressRoute circuit. 

### <a name="to-create-microsoft-peering"></a>Microsoft peering maken

1. De PowerShell-module voor ExpressRoute importeren.
    
    U moet de nieuwste PowerShell installer installeren uit [De galerie PowerShell](http://www.powershellgallery.com/) en Azure Resource Manager modules importeren in de PowerShell-sessie gestart met de ExpressRoute-cmdlets. U moet de PowerShell uitvoeren als beheerder.

        Install-Module AzureRM

        Install-AzureRM

    Alle modules van de AzureRM.* binnen het bereik van de bekende semantische versie importeren

        Import-AzureRM

    U kunt een module selecteren in het bereik van de bekende semantische versie ook gewoon importeren 
        
        Import-Module AzureRM.Network 

    Aanmelden bij uw account

        Login-AzureRmAccount

    Selecteer het abonnement dat u wilt maken, ExpressRoute circuit
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. ExpressRoute circuits maken.
    
    Volg de instructies voor het maken van een [ExpressRoute circuit](expressroute-howto-circuit-arm.md) en deze door de provider verbinding hebt ingericht. 

    Als uw provider verbinding managed Layer 3-services biedt, kunt u uw provider verbinding inschakelen Azure private peering voor u aanvragen. In dat geval hoeft u niet te volgen instructies vermeld in de volgende secties. Volg de onderstaande instructies echter als uw provider verbinding voor u, na het maken van uw circuit, routering niet beheerd.

3. Controleer ExpressRoute circuit om ervoor te zorgen dat is ingericht.

    U moet controleren of het circuit ExpressRoute is ingericht en ook ingeschakeld. Zie het voorbeeld hieronder.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Het antwoord is zoals in het onderstaande voorbeeld:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []   
4. Configureer Microsoft peering voor het circuit.

    Zorg ervoor dat u hebt de volgende informatie voordat u verdergaat.

    - Een /30 subnet voor de primaire koppeling. Dit moet een geldige openbare IPv4-voorvoegsel eigendom en geregistreerd zijn in een RIR / IR.
    - Een /30 subnet voor de tweede koppeling. Dit moet een geldige openbare IPv4-voorvoegsel eigendom en geregistreerd zijn in een RIR / IR.
    - Een geldige VLAN ID vast te stellen deze peering op. Zorg ervoor dat er geen bij andere peering in het circuit dezelfde VLAN-ID gebruikt.
    - Als getal voor peering. U kunt zowel 2-byte en 4-byte als getallen.
    - Aangekondigde voorvoegsels: U dient een lijst van alle nummers die u van plan bent om te adverteren via het BGP-sessie. Alleen openbare IP-adresprefixen worden geaccepteerd. U kunt een door komma's gescheiden lijst verzenden als u van plan bent voor het verzenden van een set van voorvoegsels. Deze voorvoegsels voor u moeten zijn geregistreerd in een RIR / IR.
    - ASN klant: Als u reclame voorvoegsels die niet zijn geregistreerd op de peering als getal, kunt u het nummer van de AS waaraan ze zijn geregistreerd. **Dit is optioneel**.
    - Routering registernaam: Kunt u de RIR / IR waartegen de AS en de voorvoegsels zijn geregistreerd.
    - Een MD5-hash, als u kiest voor een. **Dit is optioneel.**
    
    U kunt de volgende cmdlet Microsoft peering voor uw circuit configureren uitvoeren

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "123.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-get-microsoft-peering-details"></a>Microsoft peering details ophalen

U kunt met de volgende cmdlet configuratiegegevens krijgen.

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt


### <a name="to-update-microsoft-peering-configuration"></a>Microsoft peering-configuratie bij te werken

U kunt een deel van de configuratie met de volgende cmdlet bijwerken.

        Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "124.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
        

### <a name="to-delete-microsoft-peering"></a>Microsoft peering verwijderen

De configuratie van de peering kunt u verwijderen door de volgende cmdlet wordt uitgevoerd.

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## <a name="next-steps"></a>Volgende stappen

Volgende stap, [een VNet op een circuit ExpressRoute koppeling](expressroute-howto-linkvnet-arm.md).

-  Zie voor meer informatie over workflows ExpressRoute [ExpressRoute werkstromen](expressroute-workflows.md).

-  Zie voor meer informatie over peering circuit [ExpressRoute circuits en routering domeinen](expressroute-circuit-peerings.md).

-  Zie [virtuele netwerk-overzicht](../virtual-network/virtual-networks-overview.md)voor meer informatie over het werken met virtuele netwerken.

