### <a name="noconnection"></a>Het toevoegen of verwijderen van voorvoegsels - geen gateway-verbinding

- **Toevoegen** extra adresprefixen naar een lokaal netwerk gateway die u hebt gemaakt, maar die niet nog een gateway verbinding, gebruikt u het onderstaande voorbeeld. Zorg ervoor dat de waarden wijzigen om uw eigen.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

- Gebruik **voor het verwijderen van** het voorvoegsel van een adres van een gateway lokaal netwerk met een VPN-verbinding wordt niet in het volgende voorbeeld. De voorvoegsels die u niet langer weglaten. In dit voorbeeld moeten we niet langer 20.0.0.0/24 (uit het vorige voorbeeld) voorvoegsel, zodat we wordt bijgewerkt lokaal netwerk gateway en uitsluiten dat voorvoegsel.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')

### <a name="withconnection"></a>Het toevoegen of verwijderen van voorvoegsels - gateway verbinding bestaande

Als u uw gateway verbinding hebt gemaakt en u wilt toevoegen aan of verwijderen van de IP-adresprefixen is opgenomen in uw lokale netwerkgateway, moet u de volgende stappen in volgorde. Hierdoor wordt een uitval van uw VPN-verbinding. Bij het bijwerken van de voorvoegsels, zult u eerst de verbinding verwijderen, de prefixen wijzigen en vervolgens een nieuwe verbinding maken. Zorg dat u zelf de waarden wijzigen in de onderstaande voorbeelden.

>[AZURE.IMPORTANT] Verwijder niet de VPN-gateway. Als u dit doet, hebt u om terug te gaan door de stappen om het opnieuw te maken als uw router op lokalen met de nieuwe instellingen opnieuw configureren.
 
1. De verbinding verwijderen.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName

2. De adresvoorvoegsels voor de gateway van uw lokale netwerk wijzigen.

    Stel de variabele voor de LocalNetworkGateway.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName

    Wijzig de prefixen.

        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

4. Maak de verbinding. In dit voorbeeld configureert is een type IPSec-verbinding. Wanneer u opnieuw een verbinding te maken, gebruikt u het verbindingstype dat is opgegeven voor uw configuratie. Zie de pagina [PowerShell-cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) voor aanvullende verbindingstypen.

    Stel de variabele voor de VirtualNetworkGateway.

        $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName

    Maak de verbinding. Houd er rekening mee dat dit voorbeeld wordt de variabele $local die u hebt ingesteld in de vorige stap.


        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName -Location 'West US' `
        -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
        -ConnectionType IPsec `
        -RoutingWeight 10 -SharedKey 'abc123'
