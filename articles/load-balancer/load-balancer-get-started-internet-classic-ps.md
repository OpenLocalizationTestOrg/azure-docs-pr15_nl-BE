<properties
   pageTitle="Aan de slag maken van een internetverbinding taakverdeling in de klassieke modus met PowerShell | Microsoft Azure"
   description="Informatie over het maken van een internetverbinding taakverdeling in de klassieke modus met PowerShell"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/05/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-powershell"></a>Aan de slag maken van een internetverbinding (klassiek)-taakverdeling in PowerShell

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dit artikel heeft betrekking op de klassieke implementatiemodel. U kunt ook [informatie over het maken van een internetverbinding met behulp van bronbeheer Azure taakverdeling](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]



## <a name="set-up-load-balancer-using-powershell"></a>Met PowerShell taakverdeling instellen

Als u een taakverdeling met powershell instellen, volg de onderstaande stappen:

1. Als u Azure PowerShell nog nooit hebt gebruikt, Zie [installeren en configureren van Azure PowerShell](../../articles/powershell-install-configure.md) en volg de instructies helemaal naar het einde ondertekenen in Azure en selecteer uw abonnement.


2. Na het maken van een virtuele machine, kunt u een load balancer toevoegen aan een virtuele machine binnen dezelfde service cloud PowerShell-cmdlets.

In het volgende voorbeeld voegt u dat een load balancer genaamd "webfarm" cloud service "mytestcloud" (of myctestcloud.cloudapp.net), de eindpunten voor de taakverdeling toe te voegen aan de virtuele machines met de naam "web1" en "web2" ingesteld. De taakverdeling ontvangt netwerkverkeer op poort 80 en saldi tussen de virtuele machines die worden gedefinieerd door het lokale eindpunt (in dit geval poort 80) laden met TCP.


### <a name="step-1"></a>Stap 1
Een eindpunt taakverdeling maken voor de eerste VM "web1"

    Get-AzureVM -ServiceName "mytestcloud" -Name "web1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

### <a name="step-2"></a>Stap 2

Maak een andere eindpunt voor het gebruik van dezelfde load balancer de naam van de tweede VM "web2"

    Get-AzureVM -ServiceName "mytestcloud" -Name "web2" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

## <a name="remove-a-virtual-machine-from-a-load-balancer"></a>Een virtuele machine verwijderen uit een load balancer

U kunt verwijderen AzureEndpoint een eindpunt van de virtuele machine verwijderen uit de taakverdeling

    Get-azureVM -ServiceName mytestcloud  -Name web1 |Remove-AzureEndpoint -Name httpin| Update-AzureVM

## <a name="next-steps"></a>Volgende stappen

U kunt ook [een interne taakverdeling maken](load-balancer-get-started-ilb-classic-ps.md) en wat voor soort [distributie-modus](load-balancer-distribution-mode.md) voor een especific load balancer netwerkverkeer-gedrag te configureren.

Als uw toepassing verbindingen voor servers achter een load balancer in leven te houden moet, kunt u meer begrijpen over [niet-actieve TCP-time-outinstellingen voor een load balancer](load-balancer-tcp-idle-timeout.md). Het helpt om te leren over de werking van niet-actieve verbinding wanneer u taakverdeling Azure.

