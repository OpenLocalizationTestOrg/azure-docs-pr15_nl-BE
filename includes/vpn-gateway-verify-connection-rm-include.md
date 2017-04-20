### <a name="to-verify-your-connection-by-using-powershell"></a>Om te controleren of uw verbinding met behulp van PowerShell

U kunt controleren of uw verbinding met behulp van de `Get-AzureRmVirtualNetworkGatewayConnection` cmdlet, met of zonder `-Debug`. 

1. In het volgende voorbeeld cmdlet, configureert de waarden die overeenkomen met uw eigen gebruik. Selecteer desgevraagd de 'A' uitvoeren 'All'. In het voorbeeld `-Name` verwijst naar de naam van de verbinding die u hebt gemaakt en u wilt testen.

        Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG

2. Nadat de cmdlet is voltooid, worden de waarden weergeven. In het volgende voorbeeld bevat de verbindingsstatus 'Verbonden' en u ziet ingress- en egress bytes.

        Body:
        {
          "name": "MyGWConnection",
          "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/connections/MyGWConnection",
          "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "1c484f82-23ec-47e2-8cd8-231107450446b",
            "virtualNetworkGateway1": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/virtualNetworkGa
        teways/vnetgw1"
            },
            "localNetworkGateway2": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/localNetworkGate
        ways/LocalSite"
            },
            "connectionType": "IPsec",
            "routingWeight": 10,
            "sharedKey": "abc123",
            "connectionStatus": "Connected",
            "ingressBytesTransferred": 33509044,
            "egressBytesTransferred": 4142431
          }

### <a name="to-verify-your-connection-by-using-the-azure-portal"></a>Om te controleren of uw verbinding met behulp van de portal voor Azure

In de Azure portal kunt u de status van de verbinding bekijken door te navigeren naar de verbinding. Er zijn meerdere manieren om dit te doen. De volgende stappen weergeven kunt u navigeren naar de verbinding en controleer of.

1. Klikt u op **alle resources** in de [portal Azure](http://portal.azure.com)en Ga naar uw gateway virtueel netwerk.
2. Klik op de blade voor uw gateway virtueel netwerk **verbindingen**. Hier ziet u de status van elke verbinding.
3. Klik op de naam van de verbinding die u controleren of wilt u opent **Essentials**. In de Essentials, kunt u meer informatie over de verbinding weergeven. De **Status** is 'Geslaagd' en 'Connected' wanneer u een verbinding hebt gemaakt.

    ![Verbinding controleren](./media/vpn-gateway-verify-connection-rm-include/connectionsucceeded.png)