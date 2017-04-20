De stappen voor deze taak gebruikt een VNet op basis van de onderstaande waarden. Extra instellingen en namen worden ook in deze lijst beschreven. We niet deze lijst gebruiken in een van de stappen, maar we op basis van de waarden in deze lijst variabelen toevoegen. U kunt de lijst gebruiken als referentie, de waarden te vervangen door uw eigen kopiëren.

Lijst configureren:
    
- Virtual Network Name = "TestVNet"
- Virtuele adresruimte voor netwerk = 192.168.0.0/16
- Resourcegroep = "TestRG"
- Subnet1 Name = 'FrontEnd' 
- Subnet1-adresruimte = "192.168.0.0/16"
- Gateway subnetnaam: 'GatewaySubnet', moet u altijd een subnet gateway *GatewaySubnet*naam.
- Subnet-adresruimte gateway = "192.168.200.0/26"
- Regio = "Oost-US"
- Naam van de gateway = "GW"
- Gateway-IP-naam = "GWIP"
- IP-configuratie van gateway naam = "gwipconf"
-  Type = "ExpressRoute" Dit is vereist voor de configuratie van een ExpressRoute.
- Naam van openbare IP-gateway = "gwpip"


## <a name="add-a-gateway"></a>Een gateway toevoegt

1. Verbinding maken met uw abonnement Azure. 

        Login-AzureRmAccount
        Get-AzureRmSubscription 
        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

2. Declareer de variabelen voor deze oefening. In dit voorbeeld wordt het gebruik de variabelen gebruiken in het onderstaande voorbeeld. Zorg ervoor dat deze overeenkomen met de instellingen die u wilt gebruiken als u wilt bewerken. 
        
        $RG = "TestRG"
        $Location = "East US"
        $GWName = "GW"
        $GWIPName = "GWIP"
        $GWIPconfName = "gwipconf"
        $VNetName = "TestVNet"

3. Het virtuele netwerkobject als variabele opslaan.

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG

4. Een gateway subnet toevoegen aan het virtuele netwerk. Het gateway-subnet moet de naam 'GatewaySubnet'. Wilt u een gateway die /27 maken of groter (/ 26/25, enz.).
            
        Add-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26

5. De configuratie instellen.

            Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

6. Het gateway-subnet worden opgeslagen als een variabele.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet

7. Een openbaar IP-adres aanvragen. Het IP-adres wordt voordat u de gateway aangevraagd. U kan niet het IP-adres dat u gebruiken wilt. dynamisch toegewezen. U gebruikt dit IP-adres in de volgende sectie voor configuratie. De AllocationMethod moet dynamisch zijn.

        $pip = New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic

8. De configuratie voor uw gateway maken. De configuratie van de standaardgateway definieert het subnet en het openbare IP-adres te gebruiken. In deze stap geeft u de configuratie die wordt gebruikt bij het maken van de gateway. Deze stap wordt het gateway-object niet gemaakt. Het onderstaande voorbeeld gebruik te maken van de configuratie van de standaardgateway. 

        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

9. De gateway maken. In deze stap is de **GatewayType-** met name van belang. U moet de waarde **ExpressRoute**. Merk op dat na het uitvoeren van deze cmdlets, de gateway duurt 20 minuten of langer te maken.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard

## <a name="verify-the-gateway-was-created"></a>Controleren of dat de gateway is gemaakt.

Gebruik de volgende opdracht om te controleren of de gateway is gemaakt.

    Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG

## <a name="resize-a-gateway"></a>Het formaat van een gateway

Er zijn een aantal [SKU's Gateway](../articles/expressroute/expressroute-about-virtual-network-gateways.md). De volgende opdracht kunt u de SKU-Gateway op elk gewenst moment wijzigen.

>[AZURE.IMPORTANT] Deze opdracht werkt niet voor UltraPerformance-gateway. Verwijder eerst de bestaande ExpressRoute gateway te wijzigen vindt u een gateway UltraPerformance, en maak een nieuwe UltraPerformance-gateway. Verwijder eerst de UltraPerformance-gateway om te downgraden uw gateway van een gateway UltraPerformance, en maak vervolgens een nieuwe gateway.

    $gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance

## <a name="remove-a-gateway"></a>Een gateway verwijderen

Gebruik de onderstaande opdracht voor het verwijderen van een gateway

    Remove-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG  
