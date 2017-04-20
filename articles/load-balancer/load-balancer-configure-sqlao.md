<properties
   pageTitle="Taakverdeling voor SQL altijd configureren op | Microsoft Azure"
   description="Taakverdeling met SQL altijd om aan te werken en hoe u kunt profiteren van powershell taakverdeling voor de SQL-implementatie te maken configureren"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="configure-load-balancer-for-sql-always-on"></a>Taakverdeling voor SQL altijd op configureren

SQL Server AlwaysOn beschikbaarheidsgroepen kan nu worden uitgevoerd met ILB. Groep beschikbaarheid is van SQL Server vlaggenschip oplossing voor hoge beschikbaarheid en disaster recovery. De beschikbaarheid van groep Listener clienttoepassingen in staat stelt naadloos verbinding maken met de primaire replica, ongeacht het aantal replica's in de configuratie.

De naam van de listener (DNS) wordt toegewezen aan een evenwichtige taakverdeling IP-adres en taakverdeling van Azure zorgt ervoor dat het binnenkomende verkeer naar alleen de primaire server in de replicaset.

U kunt ILB ondersteuning voor SQL Server AlwaysOn (listener) eindpunten. U nu controle over de toegankelijkheid van de listener en kunt de taakverdeling IP-adres van een specifiek subnet in het virtuele netwerk (VNet).

Met behulp van de ILB op de listener van het SQL server-endpoint (zoals Server = tcp:ListenerName, 1433; Database = DatabaseName) is alleen toegankelijk voor:

- Services en VMs in hetzelfde virtuele netwerk
- Services en VMs vanaf het netwerk verbonden op voorzieningen
- Services en VMs uit onderling verbonden VNets

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.png)

Figuur 1 - SQL AlwaysOn geconfigureerd met taakverdeling internetverbinding

## <a name="add-internal-load-balancer-to-the-service"></a>Interne taakverdeling toevoegen aan de service

1. In het volgende voorbeeld zullen we een virtueel netwerk met een Subnet-1 genoemd subnet configureren:

        Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc

2. Taakverdeling eindpunten toevoegen voor ILB op elke VM

        Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 –
        DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

        Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 –DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    In het bovenstaande voorbeeld hebt u 2 VM genaamd 'sqlsvc1' en 'sqlsvc2' uitgevoerd 'Sqlsvc' in de cloud-service. Na het maken van het ILB met schakeloptie 'DirectServerReturn', voegt u taakverdeling eindpunten aan het ILB om SQL de listeners voor de van beschikbaarheidsgroepen configureren.

Zie voor meer informatie over SQL AlwaysOn [configureren een interne taakverdeling voor een groep AlwaysOn beschikbaarheid in Azure](../virtual-machines/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

## <a name="see-also"></a>Zie ook

[Een internetverbinding taakverdeling configureren aan de slag](load-balancer-get-started-internet-arm-ps.md)

[Een interne taakverdeling configureren aan de slag](load-balancer-get-started-ilb-arm-ps.md)

[Een Load balancer distributie-modus configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP-time-outinstellingen voor de taakverdeling configureren](load-balancer-tcp-idle-timeout.md)
