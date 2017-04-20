
Het VPN-apparaat configureren, moet u het openbare IP-adres van de gateway virtueel netwerk voor het configureren van uw op ruimten VPN-apparaat. Werken met de fabrikant van het apparaat voor specifieke configuratie-informatie en het apparaat configureren. Verwijzen naar de [VPN-apparaten](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md) voor meer informatie over VPN-apparaten die goed met Azure werken.

Ga voor het openbare IP-adres van uw virtuele netwerkgateway met PowerShell gebruiken in het volgende voorbeeld:

    Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG

U kunt ook het openbare IP-adres voor de gateway virtueel netwerk weergeven met behulp van de portal Azure. Ga naar de **virtueel netwerkgateways**en klik vervolgens op de naam van de gateway.