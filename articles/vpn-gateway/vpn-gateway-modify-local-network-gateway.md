<properties
   pageTitle="LAN gateway IP-adresprefixen en IP-gateway wijzigen | Microsoft Azure"
   description="Dit artikel begeleidt u bij het wijzigen van IP-adresprefixen voor uw lokale netwerkgateway"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/08/2016"
   ms.author="cherylmc"/>

# <a name="modify-local-network-gateway-settings-using-powershell"></a>LAN gateway instellingen wijzigen via PowerShell

Soms worden de instellingen voor uw lokale netwerkgateway AddressPrefix of GatewayIPAddress gewijzigd. De onderstaande instructies helpen u uw lokale netwerk gateway-instellingen wijzigen. Ook kunt u deze instellingen in de portal Azure.

## <a name="before-you-begin"></a>Voordat u begint
    
U moet de nieuwste versie van de Azure Resource Manager PowerShell cmdlets installeert. [Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) Zie voor meer informatie over het installeren van de PowerShell-cmdlets.

## <a name="to-modify-ip-address-prefixes"></a>IP-adresprefixen wijzigen

[AZURE.INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="to-modify-the-gateway-ip-address"></a>Het gateway-IP-adres wijzigen

[AZURE.INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Volgende stappen

U kunt controleren of de gateway-verbinding. Zie [een gateway verbinding controleren](vpn-gateway-verify-connection-resource-manager.md).

