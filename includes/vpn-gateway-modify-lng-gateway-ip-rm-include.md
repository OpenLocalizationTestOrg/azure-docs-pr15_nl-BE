U kunt het IP-adres van de gateway aanpassen, met de `New-AzureRmVirtualNetworkGatewayConnection` cmdlet. Als u de naam van het lokale netwerkgateway precies hetzelfde als de bestaande naam wilt behouden, worden de instellingen worden overschreven. Op dit moment is biedt de cmdlet 'Set' geen ondersteuning voor het wijzigen van het IP-adres van de gateway.

### <a name="gwipnoconnection"></a>Het IP-adres van de gateway - geen gateway-verbinding wijzigen

Het gateway-IP-adres voor de gateway van uw lokale netwerk dat niet nog een verbinding bijwerken, gebruik in het volgende voorbeeld. U kunt ook de adresprefixen tegelijk bijwerken. De instellingen die u opgeeft, worden de bestaande instellingen overschreven. Zorg ervoor dat de bestaande naam van de gateway van uw lokale netwerk. Als u dit niet doet, maakt u een nieuwe LAN gateway niet overschrijven van bestaande.

Het volgende voorbeeld wordt het vervangen van de waarden voor eigen gebruik.

    New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
    -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
    -GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName


### <a name="gwipwithconnection"></a>Het wijzigen van het IP-adres voor gateway - gateway verbinding bestaande

Als er al een gateway verbinding bestaat, moet u eerst de verbinding verwijderen. Vervolgens kunt u het IP-adres van de gateway wijzigen en opnieuw een nieuwe verbinding maken. Hierdoor wordt een uitval van uw VPN-verbinding.


>[AZURE.IMPORTANT] Verwijder niet de VPN-gateway. Als u dit doet, hebt u om terug te gaan door de stappen voor het opnieuw maken als uw router op ruimten met het IP-adres dat wordt toegewezen aan de nieuwe gateway configureren.
 

1. De verbinding verwijderen. U kunt de naam van de verbinding zoeken met de `Get-AzureRmVirtualNetworkGatewayConnection` cmdlet.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName

2. Wijzig de waarde GatewayIpAddress. U kunt ook uw adresprefixen op dit moment wijzigen indien nodig. Houd er rekening mee dat dit de bestaande lokale netwerk gateway-instellingen overschrijft. Gebruik de bestaande naam van uw lokale netwerkgateway wanneer passen, zodat de instellingen worden overschreven. Als u dit niet doet, maakt u een nieuwe gateway voor het lokale netwerk niet het bestaande profiel wijzigen.

        New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
        -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
        -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName

3. Maak de verbinding. In dit voorbeeld configureert is een type IPSec-verbinding. Wanneer u opnieuw een verbinding te maken, gebruikt u het verbindingstype dat is opgegeven voor uw configuratie. Zie de pagina [PowerShell-cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) voor aanvullende verbindingstypen.  U kunt uitvoeren als u de naam van de VirtualNetworkGateway, de `Get-AzureRmVirtualNetworkGateway` cmdlet.

    Stel de variabelen:

        $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName

    Maak de verbinding:
    
        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
        -Location "West US" `
        -VirtualNetworkGateway1 $vnetgw `
        -LocalNetworkGateway2 $local `
        -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

