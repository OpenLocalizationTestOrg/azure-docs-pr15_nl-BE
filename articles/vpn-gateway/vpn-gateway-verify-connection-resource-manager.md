<properties
   pageTitle="Controleer of u een gateway verbinding | Microsoft Azure"
   description="In dit artikel leest u hoe om te controleren of een gateway verbinding in het implementatiemodel Resource Manager"
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
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="verify-a-gateway-connection"></a>Een gateway-verbinding controleren

In een aantal verschillende manieren kunt u controleren of de gateway-verbinding. In dit artikel wordt beschreven hoe u te controleren of de status van een Resource Manager gateway verbinding met behulp van de portal Azure en met PowerShell.


## <a name="verify-using-powershell"></a>Controleer of gebruikt PowerShell

U moet de nieuwste versie van de Azure Resource Manager PowerShell cmdlets installeert. Zie voor meer informatie over het installeren van PowerShell cmdlets voor [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md). Zie voor meer informatie over het gebruik van Resource Manager-cmdlets [Met behulp van Windows PowerShell met Resource Manager](../powershell-azure-resource-manager.md).

### <a name="step-1-log-in-to-your-azure-account"></a>Stap 1: Meld u aan bij uw account Azure

1. De PowerShell-console openen met verhoogde bevoegdheden en verbinding met uw account.

        Login-AzureRmAccount

2. Controleer de abonnementen voor de account.

        Get-AzureRmSubscription 

3. Geef het abonnement dat u wilt gebruiken.

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

### <a name="step-2-verify-your-connection"></a>Stap 2: Controleer uw verbinding


[AZURE.INCLUDE [verify connection powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)] 


## <a name="verify-using-the-azure-portal"></a>Controleer of de Azure portal

[AZURE.INCLUDE [verify connection portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)] 


## <a name="next-steps"></a>Volgende stappen

- U kunt virtuele machines toevoegen aan uw virtuele netwerken. Zie [virtuele machines maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md) voor stapsgewijze instructies.

