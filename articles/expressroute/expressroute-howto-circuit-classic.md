<properties
   pageTitle="Maken en wijzigen van een ExpressRoute circuit met de klassieke implementatiemodel en PowerShell | Microsoft Azure"
   description="Dit artikel begeleidt u door de stappen voor het maken en inrichten van een ExpressRoute circuit. In dit artikel ziet u ook het controleren van de status, update of delete, en deprovision van het circuit."
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
   ms.author="ganesr;cherylmc"/>

# <a name="create-and-modify-an-expressroute-circuit"></a>Maken en wijzigen van een circuit ExpressRoute

> [AZURE.SELECTOR]
[Azure Portal - Resource Manager](expressroute-howto-circuit-portal-resource-manager.md)
[PowerShell - Resource Manager](expressroute-howto-circuit-arm.md)
[PowerShell - klassiek](expressroute-howto-circuit-classic.md)

Dit artikel begeleidt u door de stappen om een circuit Azure ExpressRoute maken met behulp van PowerShell-cmdlets en het klassieke implementatiemodel. In dit artikel ziet u ook het controleren van de status, update of delete en deprovision een circuit ExpressRoute.

**Over de Azure-implementatie**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


## <a name="before-you-begin"></a>Voordat u begint

### <a name="1-review-the-prerequisites-and-workflow-articles"></a>1. Controleer de vereisten en workflow-artikelen

Zorg ervoor dat u de [vereisten](expressroute-prerequisites.md) en [werkstromen](expressroute-workflows.md) hebt gecontroleerd voordat u configuratie.  


### <a name="2-install-the-latest-versions-of-the-azure-powershell-modules"></a>2. Installeer de nieuwste versies van de Azure PowerShell-modules 

Volg de instructies in [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor stapsgewijze instructies voor het configureren van de computer voor het gebruik van de Azure PowerShell-modules.

### <a name="3-log-in-to-your-azure-account-and-select-a-subscription"></a>3. log in op uw Azure account en selecteer een abonnement

1. Voer de volgende cmdlet met een verhoogde Windows PowerShell-prompt:

        Add-AzureAccount
2. In het aanmeldingsscherm weergegeven, het aanmelden bij uw account.

3. Een lijst van uw abonnementen opvragen.

        Get-AzureSubscription
4. Selecteer het abonnement dat u wilt gebruiken.
    
        Select-AzureSubscription -SubscriptionName "mysubscriptionname"

## <a name="create-and-provision-an-expressroute-circuit"></a>Maken en inrichten van een circuit ExpressRoute

### <a name="1-import-the-powershell-modules-for-expressroute"></a>1. de PowerShell-modules voor ExpressRoute importeren

 Als u dit nog niet hebt gedaan, moet u de Azure en ExpressRoute modules importeren in de PowerShell-sessie gestart met de ExpressRoute-cmdlets. U importeren de modules vanaf de locatie die ze op de lokale computer te zijn geïnstalleerd. Afhankelijk van de methode die u gebruikt voor de installatie van de modules, de locatie is mogelijk anders dan in het volgende voorbeeld wordt getoond. In het voorbeeld desgewenst wijzigen.  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. de lijst met ondersteunde providers, locaties en bandbreedten ophalen

Voordat u een circuit ExpressRoute maakt, moet u de lijst met ondersteunde connectiviteit providers, locaties en bandbreedte-opties.

De PowerShell-cmdlet `Get-AzureDedicatedCircuitServiceProvider` geeft deze informatie waarmee u in de volgende stappen:

    Get-AzureDedicatedCircuitServiceProvider

Controleer of uw provider verbinding er is vermeld. Noteer de volgende informatie omdat u het later nodig hebt zult bij het maken van een circuit:

- Naam

- PeeringLocations

- BandwidthsOffered

U kunt nu een circuit ExpressRoute maken.         

### <a name="3-create-an-expressroute-circuit"></a>3. ExpressRoute circuits maken

In het volgende voorbeeld ziet u hoe een 200 Mbps ExpressRoute circuit via Equinix maken in Silicon Valley. Als u een andere provider en andere instellingen gebruikt, vervangen door die informatie bij het maken van uw aanvraag.

>[AZURE.IMPORTANT] Uw ExpressRoute circuit wordt afgeschreven vanaf het moment dat een sleutel wordt uitgegeven. Zorg ervoor dat u deze bewerking niet uitvoeren wanneer de provider connectiviteit gereed is voor het inrichten van het circuit.


Hier volgt een voorbeeld van de aanvraag voor een nieuwe sleutel:

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

Of als u een ExpressRoute circuit met de premium-invoegtoepassing maken wilt, gebruikt u het volgende voorbeeld. Raadpleeg voor meer informatie over de invoegtoepassing premium [ExpressRoute Veelgestelde vragen](expressroute-faqs.md) .

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


Het antwoord bevat de sleutel. Gedetailleerde beschrijvingen van alle parameters kunt u opvragen door de volgende uit te voeren:

    get-help new-azurededicatedcircuit -detailed

### <a name="4-list-all-the-expressroute-circuits"></a>4. lijst van alle ExpressRoute circuits

U kunt uitvoeren de `Get-AzureDedicatedCircuit` de opdracht om een lijst van alle ExpressRoute circuits die u hebt gemaakt:


    Get-AzureDedicatedCircuit

Het antwoord is zoals in het volgende voorbeeld:

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

U kunt deze gegevens op elk gewenst moment ophalen met behulp van de `Get-AzureDedicatedCircuit` cmdlet. De oproep zonder parameters geeft een overzicht van alle circuits. De sleutel wordt weergegeven in het veld *ServiceKey* .

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Gedetailleerde beschrijvingen van alle parameters kunt u opvragen door de volgende uit te voeren:

    get-help get-azurededicatedcircuit -detailed

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. de sleutel naar uw provider verbinding verzenden voor de inrichting


*ServiceProviderProvisioningState* bevat informatie over de huidige status van het aanbod aan de kant van de service provider. *Status* geeft de status aan de kant van Microsoft. Zie het artikel [werkstromen](expressroute-workflows.md#expressroute-circuit-provisioning-states) voor meer informatie over het inrichten van Staten circuit.

Wanneer u een nieuw ExpressRoute circuit maakt, is het circuit in de volgende status:

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


Wanneer de provider verbinding wordt ingeschakeld voor u gaat het circuit naar het volgende staat:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Een circuit ExpressRoute moet zijn in de volgende status te kunnen gebruiken:

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. regelmatig controleren van de status en de status van de toets circuit

Hiermee kunt u weten wanneer uw provider uw circuit is ingeschakeld. Na het circuit is geconfigureerd, wordt *ServiceProviderProvisioningState* als *Provisioned* weergegeven zoals in het volgende voorbeeld wordt getoond:

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="7-create-your-routing-configuration"></a>7. Maak de routeringsconfiguratie

Raadpleeg de [ExpressRoute routeringsconfiguratie circuit (maken en wijzigen van peerings circuit)](expressroute-howto-routing-classic.md) artikel voor stapsgewijze instructies.

>[AZURE.IMPORTANT] Deze instructies zijn alleen van toepassing op circuits die zijn gemaakt met serviceproviders die laag 2-connectiviteit diensten aanbieden. Als u een serviceprovider die beheerde layer 3-diensten (meestal een IP VPN, zoals MPLS), uw provider verbinding configureren en beheren met routering voor u.

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. een virtueel netwerk koppelen aan een ExpressRoute circuit

Vervolgens wordt een virtueel netwerk een koppeling naar uw ExpressRoute circuit. Raadpleeg [circuits ExpressRoute koppelen aan virtuele netwerken](expressroute-howto-linkvnet-classic.md) voor stapsgewijze instructies. Als u een virtueel netwerk maken met behulp van het klassieke implementatiemodel voor ExpressRoute, Zie [een virtueel netwerk voor ExpressRoute maken](expressroute-howto-vnet-portal-classic.md) voor instructies.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Opvragen van de status van een circuit ExpressRoute

U kunt deze gegevens op elk gewenst moment ophalen met behulp van de `Get-AzureCircuit` cmdlet. De oproep zonder parameters geeft een overzicht van alle circuits.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

    Bandwidth                        : 1000
    CircuitName                      : MyAsiaCircuit
    Location                         : Singapore
    ServiceKey                       : #################################
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

U kunt informatie over een specifieke ExpressRoute circuit opvragen door de sleutel wordt doorgegeven als parameter aan de oproep:

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


Gedetailleerde beschrijvingen van alle parameters kunt u opvragen door de volgende uit te voeren:

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a>Een circuit ExpressRoute wijzigen

U kunt bepaalde eigenschappen van een circuit ExpressRoute zonder invloed op verbindingen wijzigen.

U kunt het volgende doen met geen uitvaltijd:

- In- of uitschakelen van een invoegtoepassing ExpressRoute premie voor uw ExpressRoute circuit.
- Vergroot de bandbreedte van uw ExpressRoute circuit. Houd er rekening mee dat u uw abonnement beperkt de bandbreedte van een circuit wordt niet ondersteund.
- De meetmethode plan onbeperkt gegevens van de gemeten gegevens wijzigen. Houd er rekening mee dat wijzigt de meetmethode plan gegevens worden gemeten met behulp van onbeperkte gegevens wordt niet ondersteund.
- U kunt inschakelen en uitschakelen van de *Klassieke bewerkingen toestaan*.

Raadpleeg de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md) voor meer informatie over de grenzen en beperkingen.

### <a name="to-enable-the-expressroute-premium-add-on"></a>De premium ExpressRoute invoegtoepassing inschakelen

U kunt de invoegtoepassing ExpressRoute premie voor uw bestaande circuit met behulp van de volgende PowerShell-cmdlet inschakelen:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

Het circuit wordt nu de ExpressRoute premium invoegtoepassing functies hebt geactiveerd. Houd er rekening mee dat we facturering voor de invoegtoepassing premium mogelijkheid begint als de opdracht met succes is uitgevoerd.

### <a name="to-disable-the-expressroute-premium-add-on"></a>Uitschakelen van de invoegtoepassing ExpressRoute premium

>[AZURE.IMPORTANT] Deze bewerking kan mislukken als u gebruikmaakt van resources die groter zijn dan wat voor de standaard-circuit is toegestaan.

Let op het volgende:

- U moet ervoor zorgen dat het aantal virtuele netwerken die zijn gekoppeld aan het circuit minder dan 10 is voordat u van premium-standaard downgraden. Als u dit niet doet, de aanvraag voor de update mislukt en u zult de premies in rekening gebracht.

- U moet alle virtuele netwerken in andere gebieden van de geopolitieke ontkoppelen. Als u dit niet doet, de aanvraag voor de update mislukt en u zult de premies in rekening gebracht.

- De routetabel moet minder dan 4.000 routes voor private peering. Uw route tabel groter is dan 4000 routes, het BGP-sessie wordt als won't worden weer ingeschakeld totdat het aantal aangekondigde voorvoegsels 4.000 daaronder.

U kunt de invoegtoepassing ExpressRoute premie voor uw bestaande circuit met behulp van de volgende PowerShell-cmdlet uitschakelen:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="to-update-the-expressroute-circuit-bandwidth"></a>Voor het bijwerken van de bandbreedte ExpressRoute circuit

Raadpleeg de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md) voor ondersteunde bandbreedte-opties voor uw provider. U kunt elk formaat dat groter is dan de grootte van uw bestaande circuit zo lang de fysieke poort (waarop uw circuit wordt gemaakt) kan kiezen.

>[AZURE.IMPORTANT] U kunt de bandbreedte van een circuit ExpressRoute zonder verstoring niet reduceren. Downgraden van bandbreedte, moet u het circuit ExpressRoute deprovision en vervolgens een nieuw ExpressRoute circuit opnieuw wilt inrichten.

Nadat u hebt besloten welk formaat u nodig hebt, kunt u de volgende opdracht om het formaat van het circuit:

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Het circuit wordt zijn grote boven aan de kant van Microsoft. U moet contact opnemen met uw provider verbinding om te werken-configuraties op hun kant aan deze wijziging. Houd er rekening mee dat we u facturen voor de bijgewerkte bandbreedte optie vanaf dit punt begint op.

Als het volgende foutbericht wordt weergegeven wanneer het circuit bandbreedte vergroten, betekent dit dat er geen voldoende bandbreedte overblijft op de fysieke poort waar uw bestaande circuit wordt gemaakt. U hebt dit circuit verwijderen en maken van een nieuw circuit van de grootte die u nodig hebt. 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available to perform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand
    

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Deprovisioning en een circuit ExpressRoute verwijderen

Let op het volgende:

- U kunt alle virtuele netwerken van het circuit ExpressRoute voor deze bewerking moet ontkoppelen. Controleer of er geen virtuele netwerken die zijn gekoppeld aan het circuit als deze bewerking mislukt.

- Als de ExpressRoute circuit serviceprovider provisioning staat **creëren** of **Provisioned** moet u werken met uw serviceprovider naar het circuit op hun kant deprovision. We blijven resources reserveren en u in rekening brengen totdat de serviceprovider is voltooid het circuit deprovisioning en ons meldt.

- U kunt de provider heeft het circuit (de status van serviceprovider inrichten is ingesteld op **niet ingericht**) deprovisioned dan het circuit te verwijderen. Hiermee stopt u de facturering van het circuit.

U kunt uw ExpressRoute circuit verwijderen door de volgende opdracht uit te voeren:

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a>Volgende stappen

Nadat u uw circuit hebt gemaakt, moet u het volgende doen:

- [Maken en wijzigen voor uw ExpressRoute circuit routering](expressroute-howto-routing-classic.md)
- [Het virtuele netwerk koppelen aan uw ExpressRoute circuit](expressroute-howto-linkvnet-classic.md)
