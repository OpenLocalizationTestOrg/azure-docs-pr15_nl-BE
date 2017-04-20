<properties
   pageTitle="Opnieuw een Azure VPN-Gateway | Microsoft Azure"
   description="Dit artikel helpt u bij het opnieuw instellen van uw Azure VPN-Gateway. Het artikel is van toepassing op de VPN-gateways in de klassieke en de implementatiemodellen Resource Manager."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="cherylmc"/>

# <a name="reset-an-azure-vpn-gateway-using-powershell"></a>Opnieuw instellen van een VPN-Gateway van Azure PowerShell gebruiken


Dit artikel helpt u bij het opnieuw instellen van uw VPN-Gateway van Azure PowerShell-cmdlets gebruiken. Deze instructies zijn zowel het klassieke implementatiemodel en het implementatiemodel Resource Manager.

Resetten van de Azure VPN-gateway is handig als meerdere ruimten VPN-verbindingen op een of meer S2S VPN-tunnels verbroken. In dit geval op ruimten VPN-apparaten worden alle werkt goed, maar zijn niet tot stand brengen van IPSec-tunnels met Azure VPN-gateways. 

Elke Azure VPN-gateway is samengesteld uit twee VM exemplaren die worden uitgevoerd in een actieve stand-by-configuratie. Wanneer u de PowerShell-cmdlet opnieuw instellen van de gateway, de gateway wordt opnieuw opgestart en wordt de cross-premises configuraties aan het vervolgens opnieuw toegepast. De gateway is, blijft het openbare IP-adres al heeft. Hierdoor hoeft u niet de configuratie van de VPN-router bijwerken met een nieuwe openbare IP-adres voor Azure VPN-gateway.  

Zodra de opdracht wordt gegeven, de huidige actieve sessie van de Azure VPN-gateway opnieuw wordt opgestart onmiddellijk. Zal er een korte onderbreking tijdens de failover-functie van de actieve sessie (wordt opnieuw opgestart), naar de stand-by-instantie. De ruimte moet minder dan een minuut.

Als de verbinding is niet hersteld nadat de eerste herstart, geven dezelfde opdracht opnieuw op te starten van het tweede exemplaar van VM (de nieuwe actieve gateway). Als de twee opnieuw opgestart rug aan rug worden aangevraagd, zal er een iets langere periode waarin beide VM-exemplaren (active en stand-by) worden telkens opnieuw worden opgestart. Hierdoor wordt een langere onderbreking op de VPN-verbinding tot 2 tot 4 minuten voor VMs voor het voltooien van de computer opnieuw hebt opgestart.

Na twee opnieuw opgestart als u steeds verbindingsproblemen tussen lokalen ondervindt nog, open een verzoek om ondersteuning van het portal voor Azure.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u de gateway hebt teruggezet, controleert u het belangrijkste hieronder voor elke IPsec-Site naar Site (S2S) VPN-tunnel. Het verbreken van de verbinding van VPN-S2S tunnels zal leiden tot een in de artikelen komen niet overeen. Te controleren en corrigeren van de configuraties voor uw op gebouwen en Azure VPN-gateway hoeft u niet nodig opnieuw opstarten en verstoringen van de andere werkende verbindingen op de gateways.

Controleer of de volgende artikelen voordat uw gateway:

- De Internet-IP-adressen (VIP's) voor zowel de Azure VPN-gateway en het op ruimten VPN-gateway zijn correct geconfigureerd in zowel de Azure en het beleid van de VPN op gebouwen.
- De vooraf-gedeelde sleutel moeten hetzelfde zijn op VPN-gateways voor zowel Azure en op gebouwen.
- Als u specifieke configuratie van IPsec/IKE, zoals codering, voorzien hash-algoritmen en PFS (Perfect Forward Secrecy), van VPN-gateways voor de Azure en op gebouwen de configuraties hetzelfde.

## <a name="reset-a-vpn-gateway-using-the-resource-management-deployment-model"></a>Opnieuw instellen van een VPN-Gateway met het implementatiemodel Resource Management

De bronnenbeheerder PowerShell-cmdlet gateway opnieuw in te stellen is `Reset-AzureRmVirtualNetworkGateway`. Het volgende voorbeeld wordt de Azure VPN-gateway, 'VNet1GW', in de groep 'TestRG1'.

    $gw = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroup TestRG1
    Reset-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw

## <a name="reset-a-vpn-gateway-using-the-classic-deployment-model"></a>Opnieuw instellen van een VPN-Gateway met behulp van het implementatiemodel klassiek

De PowerShell-cmdlet Azure VPN-gateway opnieuw in te stellen is `Reset-AzureVNetGateway`. Het volgende voorbeeld wordt de Azure VPN-gateway voor het virtuele netwerk "ContosoVNet" genoemd.
 
    Reset-AzureVNetGateway –VnetName “ContosoVNet” 

Resultaat:

    Error          :
    HttpStatusCode : OK
    Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
    Status         : Successful
    RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
    StatusCode     : OK


## <a name="next-steps"></a>Volgende stappen
    
Zie de [verwijzing voor servicebeheer PowerShell-cmdlet](https://msdn.microsoft.com/library/azure/mt617104.aspx) en de [bronnenbeheerder PowerShell-cmdlet referentie](http://go.microsoft.com/fwlink/?LinkId=828732) voor meer informatie.






