Moet u een VNet en een subnet gateway eerst voordat u gaat werken aan de volgende taken. Zie het artikel [een virtueel netwerk met de klassieke portal configureren](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) voor meer informatie.   

## <a name="add-a-gateway"></a>Een gateway toevoegt

Gebruik de onderstaande opdracht voor het maken van een gateway. Zorg ervoor dat voor uw eigen waarden te vervangen.

    New-AzureVirtualNetworkGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType Dedicated -GatewaySKU  Standard

## <a name="verify-the-gateway-was-created"></a>Controleren of dat de gateway is gemaakt.

Gebruik de volgende opdracht om te controleren of de gateway is gemaakt. Met deze opdracht haalt ook de gateway-ID, die u nodig hebt voor andere bewerkingen.

    Get-AzureVirtualNetworkGateway

## <a name="resize-a-gateway"></a>Het formaat van een gateway

Er zijn een aantal [SKU's Gateway](../articles/expressroute/expressroute-about-virtual-network-gateways.md). De volgende opdracht kunt u de SKU-Gateway op elk gewenst moment wijzigen.

>[AZURE.IMPORTANT] Deze opdracht werkt niet voor UltraPerformance-gateway. Verwijder eerst de bestaande ExpressRoute gateway te wijzigen vindt u een gateway UltraPerformance, en maak een nieuwe UltraPerformance-gateway. Verwijder eerst de UltraPerformance-gateway om te downgraden uw gateway van een gateway UltraPerformance, en maak vervolgens een nieuwe gateway. 

    Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance

## <a name="remove-a-gateway"></a>Een gateway verwijderen

Gebruik de onderstaande opdracht voor het verwijderen van een gateway

    Remove-AzureVirtualNetworkGateway -GatewayId <Gateway ID>