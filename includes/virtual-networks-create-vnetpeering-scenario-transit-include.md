## <a name="service-chaining---transit-through-peered-vnet"></a>Service Chaining - doorvoer via peered VNet

Hoewel het gebruik van routes systeem verkeer automatisch voor de implementatie vergemakkelijkt, zijn er gevallen waarin u wilt besturen met de routering van pakketten via een virtueel toestel.
In dit scenario zijn twee VNets in een abonnement, HubVNet en VNet1 zoals beschreven in het onderstaande diagram. Voor het implementeren van virtuele netwerk-Appliance(NVA) in VNet HubVNet. U kunt na het tot stand brengen van een peering tussen HubVNet en VNet1 VNet, gebruiker gedefinieerd Routes instellen en geef de volgende hop voor NVA in de HubVNet.

![NVA-doorvoer](./media/virtual-networks-create-vnetpeering-scenario-transit-include/figure01.PNG)

> [AZURE.NOTE] De eenvoud, wordt ervan uitgegaan dat alle VNets hier in het abonnement hetzelfde zijn. Maar het werkt ook voor cross-abonnement scenario.

De belangrijkste eigenschap Transit routering inschakelen is de parameter 'Doorgestuurd verkeer toestaan'. Hierdoor kunnen accepteren en verzenden van verkeer van/naar de NVA in de peered VNet.  
