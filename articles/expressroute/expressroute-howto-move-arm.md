<properties
   pageTitle="ExpressRoute circuits verplaatsen van klassiek tot Resource Manager | Microsoft Azure"
   description="Deze pagina wordt beschreven hoe u een klassieke circuit naar het implementatiemodel Resource Manager."
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


# <a name="move-expressroute-circuits-from-the-classic-to-the-resource-manager-deployment-model"></a>ExpressRoute circuits van het klassieke naar het implementatiemodel Resource Manager verplaatsen

## <a name="configuration-prerequisites"></a>Vereisten voor configuratie

- U moet de meest recente versie van de modules voor Azure PowerShell (minstens versie 1.0).
- Zorg ervoor dat u [vereisten](expressroute-prerequisites.md), [vereisten voor routering](expressroute-routing.md)en [werkstromen](expressroute-workflows.md) hebt gecontroleerd voordat u configuratie.
- Lees voordat verdere voorafgaande informatie die wordt geleverd bij [een ExpressRoute circuit van klassieke Resource Manager te verplaatsen](expressroute-move.md). Zorg ervoor dat u hebt volledig worden begrepen de grenzen en beperkingen van de mogelijkheden.
- Als u wilt een Azure ExpressRoute circuit van het klassieke implementatiemodel naar het implementatiemodel Azure Resource Manager verplaatsen, moet u het circuit volledig geconfigureerd en operationeel zijn in de klassieke implementatiemodel hebben.
- Zorg ervoor dat er een resourcegroep die is gemaakt in het implementatiemodel Resource Manager.

## <a name="move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Het circuit ExpressRoute verplaatsen naar het implementatiemodel Resource Manager

U moet een circuit ExpressRoute verplaatsen naar het implementatiemodel Resource Manager zodat u deze in zowel de klassieke als de bronnenbeheerder implementatiemodellen gebruiken kunt. U kunt dit doen door de volgende PowerShell-opdrachten uit te voeren.

### <a name="step-1-gather-circuit-details-from-the-classic-deployment-model"></a>Stap 1: Verzamel circuit details van het implementatiemodel klassiek

U moet gegevens verzamelen over uw ExpressRoute circuit eerst.

Aanmelden bij de klassieke Azure-omgeving, en verzamel de sleutel. In het volgende fragment van PowerShell kunt u gebruiken om de gegevens te verzamelen:

    # Sign in to your Azure account
    Add-AzureAccount

    # Select the appropriate Azure subscription
    Select-AzureSubscription "<Enter Subscription Name here>"

    # Import the PowerShell modules for Azure and ExpressRoute
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

    # Get the service keys of all your ExpressRoute circuits
    Get-AzureDedicatedCircuit

De **sleutel** van het circuit dat u wilt overstappen naar het implementatiemodel Resource Manager kopiëren.

### <a name="step-2-sign-in-to-the-resource-manager-environment-and-create-a-new-resource-group"></a>Stap 2: Meld u aan bij de omgeving Resource Manager en maak een nieuwe resourcegroep

U kunt een nieuwe resourcegroep maken met behulp van het volgende fragment:

    # Sign in to your Azure Resource Manager environment
    Login-AzureRmAccount

    # Select the appropriate Azure subscription
    Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription

    #Create a new resource group if you don't already have one
    New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"

U kunt een bestaande resourcegroep als u nog hebt.

### <a name="step-3-move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Stap 3: Het circuit ExpressRoute verplaatsen naar het implementatiemodel Resource Manager

U bent nu klaar om te verplaatsen via uw ExpressRoute circuit van het klassieke naar het implementatiemodel Resource Manager. Bekijk de informatie onder [een ExpressRoute circuit van de klassiek aan de bronnenbeheerder implementatiemodel verplaatsen](expressroute-move.md) voordat u doorgaat.

U kunt dit doen door het volgende fragment uit te voeren:

    Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"

>[AZURE.NOTE] Na de verplaatsing is voltooid, wordt de nieuwe naam die wordt vermeld in de vorige cmdlet gebruikt om de bron. Het circuit in feite gewijzigd.

## <a name="enable-an-expressroute-circuit-for-both-deployment-models"></a>Een circuit ExpressRoute voor beide implementatiemodellen inschakelen

Voordat u toegang tot het implementatiemodel beheren, moet u uw ExpressRoute circuit verplaatsen naar het implementatiemodel Resource Manager.

Voer de volgende cmdlet voor toegang tot beide implementatiemodellen:

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to TRUE
    $ckt.AllowClassicOperations = $true

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Na deze bewerking is voltooid, kunt u zijn het circuit in de klassieke implementatiemodel weergeven.

Voer de volgende als u de details van het circuit ExpressRoute:

    get-azurededicatedcircuit

U moet de sleutel vermeld zien. Nu kunt u koppelingen naar de ExpressRoute circuit met uw standaard klassieke implementatie model opdrachten voor klassieke VNets en de standaardopdrachten ARM ARM VNETs beheren. De volgende artikelen helpt u bij het beheren van koppelingen naar het circuit ExpressRoute:

- [Het virtuele netwerk koppelen aan uw ExpressRoute circuit in het implementatiemodel Resource Manager](expressroute-howto-linkvnet-arm.md)
- [Het virtuele netwerk koppelen aan uw ExpressRoute circuit in het implementatiemodel klassiek](expressroute-howto-linkvnet-classic.md)


## <a name="disable-the-expressroute-circuit-to-the-classic-deployment-model"></a>Het circuit ExpressRoute met het klassieke implementatiemodel uitschakelen

Voer de volgende cmdlet om toegang tot het klassieke implementatiemodel uitschakelen:

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to FALSE
    $ckt.AllowClassicOperations = $false

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Na deze bewerking is voltooid, is het niet mogelijk om het circuit in de klassieke implementatiemodel weer te geven.

## <a name="next-steps"></a>Volgende stappen

Nadat u uw circuit hebt gemaakt, moet u het volgende doen:

- [Maken en wijzigen voor uw ExpressRoute circuit routering](expressroute-howto-routing-arm.md)
- [Het virtuele netwerk koppelen aan uw ExpressRoute circuit](expressroute-howto-linkvnet-arm.md)
