<properties
   pageTitle="Maken en wijzigen van een ExpressRoute circuit met behulp van bronbeheer en PowerShell | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe maken, inrichten, controleren, bijwerken, verwijderen en een circuit ExpressRoute deprovision."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr"/>


# <a name="create-and-modify-an-expressroute-circuit"></a>Maken en wijzigen van een circuit ExpressRoute

> [AZURE.SELECTOR]
[Azure Portal - Resource Manager](expressroute-howto-circuit-portal-resource-manager.md)
[PowerShell - Resource Manager](expressroute-howto-circuit-arm.md)
[PowerShell - klassiek](expressroute-howto-circuit-classic.md)


In dit artikel wordt beschreven hoe een circuit Azure ExpressRoute maken met behulp van Windows PowerShell-cmdlets en het implementatiemodel Azure Resource Manager. In dit artikel leert u ook hoe u de status van het circuit, bijwerken of verwijderen en het deprovision.

**Over de Azure-implementatie**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>Voordat u begint


- De meest recente versie van de modules voor Azure PowerShell (minstens versie 1.0). Volg de instructies in [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md)voor stapsgewijze instructies voor het configureren van de computer voor het gebruik van de PowerShell-modules.

- De [vereisten](expressroute-prerequisites.md) en [werkstromen](expressroute-workflows.md) bekijken voordat u configuratie.

## <a name="create-and-provision-an-expressroute-circuit"></a>Maken en inrichten van een circuit ExpressRoute

### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. log in op uw account Azure en selecteer uw abonnement

Om te beginnen met de configuratie, het aanmelden bij uw account Azure. Zie voor meer informatie over PowerShell [Met behulp van Windows PowerShell met Resource Manager](../powershell-azure-resource-manager.md). Met de volgende voorbeelden kunt u verbinding kunt maken:

    Login-AzureRmAccount

De abonnementen voor de account controleren:

    Get-AzureRmSubscription

Selecteer het abonnement dat u wilt maken van een circuit ExpressRoute voor:

    Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. de lijst met ondersteunde providers, locaties en bandbreedten ophalen

Voordat u een circuit ExpressRoute maakt, moet u de lijst met ondersteunde connectiviteit providers, locaties en bandbreedte-opties.

De PowerShell-cmdlet `Get-AzureRmExpressRouteServiceProvider` geeft deze informatie waarmee u in de volgende stappen:

    Get-AzureRmExpressRouteServiceProvider

Controleer of uw provider verbinding er is vermeld. Noteer de volgende informatie omdat u het later nodig hebt zult bij het maken van een circuit:

- Naam

- PeeringLocations

- BandwidthsOffered

U kunt nu een circuit ExpressRoute maken.   

### <a name="3-create-an-expressroute-circuit"></a>3. ExpressRoute circuits maken

Als u nog niet een groep, moet u een voordat u uw ExpressRoute circuit maken. U kunt dit doen door de volgende opdracht uit te voeren:


    New-AzureRmResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"


In het volgende voorbeeld ziet u hoe een 200 Mbps ExpressRoute circuit via Equinix maken in Silicon Valley. Als u een andere provider en andere instellingen gebruikt, vervangen door die informatie bij het maken van uw aanvraag. Hier volgt een voorbeeld van de aanvraag voor een nieuwe sleutel:

    New-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200

Zorg ervoor dat u de juiste SKU-niveau en SKU-familie:

- SKU-niveau bepaalt of een standaard ExpressRoute of een ExpressRoute premium-invoegtoepassing is ingeschakeld. U kunt *standaard* als u de standaard SKU of de *premie* voor de premium-invoegtoepassing opgeven.

- SKU-familie bepaalt het factuuradres. Kunt u *Metereddata* voor een plan gemeten gegevens en *Unlimiteddata* voor onbeperkt. Houd er rekening mee dat kunt u het type van de facturering van *Metereddata* naar *Unlimiteddata*, maar u niet het type van *Unlimiteddata* in *Metereddata wijzigen*.


>[AZURE.IMPORTANT] Uw ExpressRoute circuit wordt afgeschreven vanaf het moment dat een sleutel wordt uitgegeven. Zorg ervoor dat u deze bewerking niet uitvoeren wanneer de provider connectiviteit gereed is voor het inrichten van het circuit.

Het antwoord bevat de sleutel. Gedetailleerde beschrijvingen van alle parameters kunt u opvragen door de volgende uit te voeren:


    get-help New-AzureRmExpressRouteCircuit -detailed


### <a name="4-list-all-expressroute-circuits"></a>4. lijst van alle ExpressRoute circuits

Uitvoeren als u een lijst met alle ExpressRoute circuits die u hebt gemaakt, de `Get-AzureRmExpressRouteCircuit` opdracht:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Het antwoord ziet er ongeveer als volgt uitzien:


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
    CircuitProvisioningState          : Enabled
    ServiceProviderProvisioningState  : NotProvisioned
    ServiceProviderNotes              :
    ServiceProviderProperties         : {
                                          "ServiceProviderName": "Equinix",
                                          "PeeringLocation": "Silicon Valley",
                                          "BandwidthInMbps": 200
                                        }
    ServiceKey                        : **************************************
    Peerings                          : []

U kunt deze gegevens op elk gewenst moment ophalen met behulp van de `Get-AzureRmExpressRouteCircuit` cmdlet. De oproep zonder parameters geeft een overzicht van alle circuits. De sleutel wordt weergegeven in het veld *ServiceKey* :


    Get-AzureRmExpressRouteCircuit


Het antwoord ziet er ongeveer als volgt uitzien:


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
    ServiceProviderProvisioningState : NotProvisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []


Gedetailleerde beschrijvingen van alle parameters kunt u opvragen door de volgende uit te voeren:


    get-help Get-AzureRmExpressRouteCircuit -detailed

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. de sleutel naar uw provider verbinding verzenden voor de inrichting

*ServiceProviderProvisioningState* geeft informatie over de huidige status van het aanbod aan de kant van de service provider. Status geeft de status aan de kant van Microsoft. Zie het artikel [werkstromen](expressroute-workflows.md#expressroute-circuit-provisioning-states) voor meer informatie over het inrichten van Staten circuit.

Wanneer u een nieuw ExpressRoute circuit maakt, is het circuit in de volgende status:


    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



Wanneer de provider verbinding wordt ingeschakeld voor u verandert het circuit in het volgende staat:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

U kunt een ExpressRoute circuit gebruiken, zich in de volgende status:

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. regelmatig controleren van de status en de status van de toets circuit

Controleren van de status en de status van de toets circuit, laat u weten wanneer uw provider uw circuit is ingeschakeld. Na het circuit is geconfigureerd, *ServiceProviderProvisioningState* wordt weergegeven als *Provisioned*, zoals in het volgende voorbeeld wordt getoond:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


Het antwoord ziet er ongeveer als volgt uitzien:


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

### <a name="7-create-your-routing-configuration"></a>7. Maak de routeringsconfiguratie

Zie het artikel [ExpressRoute circuit routeringsconfiguratie](expressroute-howto-routing-arm.md) maken en wijzigen van peerings circuit voor stapsgewijze instructies.


>[AZURE.IMPORTANT] Deze instructies zijn alleen van toepassing op circuits die zijn gemaakt met serviceproviders die laag 2-connectiviteit diensten aanbieden. Als u een serviceprovider die beheerde layer 3-diensten (meestal een IP VPN, zoals MPLS), uw provider verbinding configureren en beheren met routering voor u.

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. een virtueel netwerk koppelen aan een ExpressRoute circuit

Vervolgens wordt een virtueel netwerk een koppeling naar uw ExpressRoute circuit. Het artikel [virtuele netwerken koppelen aan ExpressRoute circuits](expressroute-howto-linkvnet-arm.md) gebruiken wanneer u met het implementatiemodel Resource Manager werkt.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Opvragen van de status van een circuit ExpressRoute

U kunt deze gegevens op elk gewenst moment ophalen met behulp van de `Get-AzureRmExpressRouteCircuit` cmdlet. De oproep zonder parameters geeft een overzicht van alle circuits.

    Get-AzureRmExpressRouteCircuit


De respons is vergelijkbaar met het volgende voorbeeld:


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


U kunt informatie over een specifieke ExpressRoute circuit opvragen door de Resourcegroepnaam en het circuit als een parameter doorgegeven aan de oproep:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


Het antwoord ziet er ongeveer als volgt uitzien:


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


Gedetailleerde beschrijvingen van alle parameters kunt u opvragen door de volgende uit te voeren:

    get-help get-azurededicatedcircuit -detailed


## <a name="modify"></a>Een circuit ExpressRoute wijzigen

U kunt bepaalde eigenschappen van een circuit ExpressRoute zonder invloed op verbindingen wijzigen.

U kunt het volgende doen met geen uitvaltijd:

- In- of uitschakelen van een invoegtoepassing ExpressRoute premie voor uw ExpressRoute circuit.
- Vergroot de bandbreedte van uw ExpressRoute circuit. Houd er rekening mee dat u uw abonnement beperkt de bandbreedte van een circuit wordt niet ondersteund.
- De meetmethode plan onbeperkt gegevens van de gemeten gegevens wijzigen. Houd er rekening mee dat wijzigt de meetmethode plan gegevens worden gemeten met behulp van onbeperkte gegevens wordt niet ondersteund.
-  U kunt inschakelen en uitschakelen van de *Klassieke bewerkingen toestaan*.

Raadpleeg de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md)voor meer informatie over de grenzen en beperkingen.

### <a name="to-enable-the-expressroute-premium-add-on"></a>De premium ExpressRoute invoegtoepassing inschakelen

U kunt de invoegtoepassing ExpressRoute premie voor uw bestaande circuit inschakelen met behulp van de volgende PowerShell fragment:

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Premium"
    $ckt.sku.Name = "Premium_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


Het circuit wordt nu de ExpressRoute premium invoegtoepassing functies hebt geactiveerd. Houd er rekening mee dat we facturering voor de invoegtoepassing premium mogelijkheid begint als de opdracht met succes is uitgevoerd.

### <a name="to-disable-the-expressroute-premium-add-on"></a>Uitschakelen van de invoegtoepassing ExpressRoute premium

>[AZURE.IMPORTANT] Deze bewerking kan mislukken als u gebruikmaakt van resources die groter zijn dan wat voor de standaard-circuit is toegestaan.

Let op het volgende:

- Voordat u gebruik maken van premium standard, moet u ervoor zorgen dat het aantal virtuele netwerken die zijn gekoppeld aan het circuit minder dan 10 is. Als u dit niet doet, mislukt de aanvraag voor een update en we zullen u tegen premies rekening.

- U moet alle virtuele netwerken in andere gebieden van de geopolitieke ontkoppelen. Als u dit niet doet, mislukt de aanvraag voor de update en we zullen u tegen premies rekening.

- De routetabel moet minder dan 4.000 routes voor private peering. Uw route tabel groter is dan 4000 routes, daalt en weer won't ingeschakeld totdat het aantal aangekondigde voorvoegsels 4.000 daaronder het BGP-sessie.

U kunt de invoegtoepassing ExpressRoute premie voor de bestaande circuit met behulp van de volgende PowerShell-cmdlet uitschakelen:


    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Standard"
    $ckt.sku.Name = "Standard_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-update-the-expressroute-circuit-bandwidth"></a>Voor het bijwerken van de bandbreedte ExpressRoute circuit

Controleer de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md)voor ondersteunde bandbreedte-opties voor uw provider. Kunt u een groter dan de grootte van uw bestaande circuit.

>[AZURE.IMPORTANT] U kunt de bandbreedte van een circuit ExpressRoute zonder verstoring niet reduceren. Downgraden van bandbreedte, moet u het circuit ExpressRoute deprovision en vervolgens een nieuw ExpressRoute circuit opnieuw wilt inrichten.

Nadat u hebt besloten welk formaat u nodig hebt, gebruikt u de volgende opdracht om het formaat van het circuit te:


    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.ServiceProviderProperties.BandwidthInMbps = 1000

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


Uw circuit formaat aangepast om aan de kant van Microsoft. U kunt vervolgens moet contact opnemen met uw provider verbinding om te werken-configuraties op hun kant aan deze wijziging. Nadat u deze melding, beginnen wij u voor de optie bijgewerkte bandbreedte facturering.


### <a name="to-move-the-sku-from-metered-to-unlimited"></a>Als u wilt verplaatsen van de SKU van gemeten onbeperkt

U kunt de SKU van een circuit ExpressRoute wijzigen met behulp van de volgende PowerShell fragment:

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Family = "UnlimitedData"
    $ckt.sku.Name = "Premium_UnlimitedData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>Toegang tot het klassieke en Resource Manager-omgevingen  

Lees de instructies in de [circuits van de klassiek aan de bronnenbeheerder implementatiemodel ExpressRoute verplaatsen](expressroute-howto-move-arm.md).  


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Deprovisioning en een circuit ExpressRoute verwijderen

Let op het volgende:

- U moet alle virtuele netwerken van het circuit ExpressRoute ontkoppelen. Als deze bewerking mislukt, controleert u of een willekeurige virtuele netwerken aan het circuit zijn gekoppeld.

- Als de ExpressRoute circuit serviceprovider provisioning staat **creëren** of **Provisioned** moet u werken met uw serviceprovider naar het circuit op hun kant deprovision. We blijven resources reserveren en u in rekening brengen totdat de serviceprovider is voltooid het circuit deprovisioning en ons meldt.

- U kunt de provider heeft het circuit (de status van serviceprovider inrichten is ingesteld op **niet ingericht**) deprovisioned dan het circuit te verwijderen. Hiermee stopt u de factuur voor het circuit

U kunt uw ExpressRoute circuit verwijderen door de volgende opdracht uit te voeren:

    Remove-AzureRmExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"



## <a name="next-steps"></a>Volgende stappen

Nadat u uw circuit hebt gemaakt, moet u het volgende doen:

- [Maken en wijzigen voor uw ExpressRoute circuit routering](expressroute-howto-routing-arm.md)
- [Het virtuele netwerk koppelen aan uw ExpressRoute circuit](expressroute-howto-linkvnet-arm.md)
