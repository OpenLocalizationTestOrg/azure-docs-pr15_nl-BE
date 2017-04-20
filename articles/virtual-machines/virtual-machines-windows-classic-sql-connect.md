<properties
    pageTitle="Verbinding maken met een SQL Server virtuele Machine (klassiek) | Microsoft Azure"
    description="Informatie over verbinding maken met SQL Server wordt uitgevoerd op een virtuele Machine in Azure. In dit onderwerp wordt het klassieke implementatiemodel. De scenario's verschillen afhankelijk van de netwerkconfiguratie en de locatie van de client."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="jroth" />

# <a name="connect-to-a-sql-server-virtual-machine-on-azure-classic-deployment"></a>Verbinding maken met een SQL Server virtuele Machine op Azure (klassieke Deployment)

> [AZURE.SELECTOR]
- [Resource Manager](virtual-machines-windows-sql-connect.md)
- [Klassiek](virtual-machines-windows-classic-sql-connect.md)

## <a name="overview"></a>Overzicht

In dit onderwerp wordt beschreven hoe u verbinding maakt met uw exemplaar van SQL Server op een Azure virtuele machine wordt uitgevoerd. Het omvat sommige [scenario's algemene connectiviteit](#connection-scenarios) en vervolgens bevat [gedetailleerde stappen voor het configureren van SQL Server-verbinding in een VM Azure](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Als u met behulp van bronbeheer VMs, Zie [verbinding maken met een virtuele Machine op Azure Resource Manager gebruiken in SQL Server](virtual-machines-windows-sql-connect.md).

## <a name="connection-scenarios"></a>Scenario's voor verbinding

De manier waarop die een client verbinding maakt met SQL Server wordt uitgevoerd op een virtuele Machine verschilt afhankelijk van de locatie van de client en de configuratie van de computer/netwerk. Deze scenario's zijn onder andere:

- [Verbinding maken met SQL Server in dezelfde service cloud](#connect-to-sql-server-in-the-same-cloud-service)
- [Verbinding maken met SQL Server via het internet](#connect-to-sql-server-over-the-internet)
- [Verbinding maken met SQL Server in hetzelfde virtuele netwerk](#connect-to-sql-server-in-the-same-virtual-network)

>[AZURE.NOTE] Voordat u verbinding met een van deze methoden maakt, moet u de [stappen in dit artikel voor het configureren van verbindingen](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

### <a name="connect-to-sql-server-in-the-same-cloud-service"></a>Verbinding maken met SQL Server in dezelfde service cloud

Meerdere virtuele machines kunnen worden gemaakt in dezelfde cloud-service. Zie [virtuele machines met virtual network of cloud-service aansluiten](virtual-machines-windows-classic-connect-vms.md#connect-vms-in-a-standalone-cloud-service)inzicht in dit scenario van virtuele machines. Dit scenario is wanneer een client op een virtuele machine probeert verbinding met SQL Server op een andere virtuele machine in dezelfde cloud-service wordt uitgevoerd.

In dit scenario kunt u verbinding maken met de VM- **naam** (ook weergegeven als de **Computer** - of **hostnaam** in de portal). Dit is de naam die u voor de VM tijdens het maken. Bijvoorbeeld, als u de naam van uw SQL VM **mysqlvm**, kan een client VM in dezelfde cloud-service de volgende verbindingsreeks gebruiken om verbinding te maken:

    "Server=mysqlvm;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

### <a name="connect-to-sql-server-over-the-internet"></a>Verbinding maken met SQL Server via het Internet

Als u wilt verbinding maken met de SQL Server-database-engine van Internet, moet u een eindpunt van de virtuele machine voor binnenkomende TCP-communicatie. Deze configuratiestap Azure zorgt ervoor dat binnenkomend verkeer van TCP-poort op een TCP-poort die toegankelijk is voor de virtuele machine.

Wilt verbinden via het internet, moet u de DNS-naam van de VM en het poortnummer van de VM eindpunt (geconfigureerd verderop in dit artikel). Als u de DNS-naam zoekt, gaat u naar de Portal Azure en selecteer **virtuele machines (klassiek)**. Selecteer de virtuele machine. De **DNS-naam** wordt weergegeven in de sectie **Overzicht** .

Neem bijvoorbeeld een klassieke virtuele machine met de naam **mysqlvm** met een DNS-naam van **mysqlvm7777.cloudapp.net** en een VM-eindpunt van de **57500**. Als de verbinding juist is geconfigureerd, de volgende verbindingsreeks kan worden gebruikt voor toegang tot de virtuele machine vanaf een willekeurige locatie op het internet:

    "Server=mycloudservice.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Hoewel dit connectiviteit voor clients via internet maakt, wil dit niet zeggen dat iedereen verbinding met de SQL-Server maken kan. Buiten moeten clients u de juiste gebruikersnaam en wachtwoord. Voor extra beveiliging niet de bekende poort 1433 gebruiken voor het eindpunt van de openbare virtuele machine. En, indien mogelijk, kunt u een ACL toevoegen op het eindpunt om het verkeer te beperken tot clients die u toestaan. Zie [de ACL op een eindpunt beheren](virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint)voor instructies over het gebruik van ACL's met eindpunten.

>[AZURE.NOTE] Het is belangrijk als u deze techniek gebruikt om te communiceren met SQL Server, worden alle uitgaande gegevens uit de Azure datacenter is onderworpen aan normale [prijzen voor uitgaande gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/).

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Verbinding maken met SQL Server in hetzelfde virtuele netwerk

[Virtueel netwerk](../virtual-network/virtual-networks-overview.md) kunt nog meer scenario's. Zelfs als de VMs in verschillende cloud-services bestaat kunt u VMs in hetzelfde virtuele netwerk verbinden. En met een [VPN van site naar site](../vpn-gateway/vpn-gateway-site-to-site-create.md), maakt u een hybride architectuur die VMs met netwerken op gebouwen en machines verbindt.

Virtuele netwerken kunt u uw VMs Azure toevoegen aan een domein. Dit is de enige manier om Windows-verificatie met SQL Server. De andere verbinding-scenario's moet SQL-verificatie met gebruikersnamen en wachtwoorden.

Als u een domein en een Windows-verificatie configureren, hoeft u niet de stappen in dit artikel gebruiken voor het configureren van het eindpunt van de openbare of de SQL-verificatie en -aanmeldingen. In dit scenario kunt u uw exemplaar van SQL Server door de naam van de SQL Server VM opgeven in de verbindingsreeks. In het volgende voorbeeld wordt ervan uitgegaan dat ook Windows-verificatie is geconfigureerd en dat de gebruiker heeft toegang tot de SQL Server-exemplaar.

    "Server=mysqlvm;Integrated Security=true"

## <a name="steps-for-configuring-sql-server-connectivity-in-an-azure-vm"></a>Stappen voor het configureren van SQL Server-verbinding in een Azure VM

De volgende stappen wordt beschreven hoe via internet met behulp van SQL Server Management Studio (SSMS) verbinding maken met de SQL Server-exemplaar. Dezelfde stappen zijn echter van toepassing op de virtuele machine van uw SQL Server toegankelijk voor uw applicaties, zowel op gebouwen en in Azure te maken.

Voordat u verbinding met het exemplaar van SQL Server of een andere VM op het internet maken kunt, moet u de volgende taken uitvoeren, zoals wordt beschreven in de volgende secties:

- [Een TCP-eindpunt voor de virtuele machine maken](#create-a-tcp-endpoint-for-the-virtual-machine)
- [TCP-poorten openen in Windows firewall](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [SQL Server luistert op het TCP-protocol configureren](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [SQL-Server configureren voor de verificatiemodus mixed](#configure-sql-server-for-mixed-mode-authentication)
- [SQL Server-verificatie-aanmeldingen maken](#create-sql-server-authentication-logins)
- [Bepaal de DNS-naam van de virtuele machine](#determine-the-dns-name-of-the-virtual-machine)
- [Verbinding maken met de Database-Engine van een andere computer](#connect-to-the-database-engine-from-another-computer)

Het verbindingspad wordt samengevat in het volgende diagram:

![Verbinding maken met een SQL Server virtuele machine](../../includes/media/virtual-machines-sql-server-connection-steps/SQLServerinVMConnectionMap.png)

[AZURE.INCLUDE [Connect to SQL Server in a VM Classic TCP Endpoint](../../includes/virtual-machines-sql-server-connection-steps-classic-tcp-endpoint.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Classic Steps](../../includes/virtual-machines-sql-server-connection-steps-classic.md)]

## <a name="next-steps"></a>Volgende stappen

Als u ook AlwaysOn beschikbaarheidsgroepen gebruiken voor hoge beschikbaarheid en noodherstel wilt, kunt u overwegen een listener implementeren. Database-clients verbinding maken naar de listener in plaats van rechtstreeks naar een van de exemplaren van SQL Server. De listener stuurt clients naar de primaire replica in de beschikbaarheidsgroep. Zie [een listener ILB voor AlwaysOn beschikbaarheidsgroepen in Azure configureren](virtual-machines-windows-classic-ps-sql-int-listener.md)voor meer informatie.

Het is belangrijk dat alle aanbevolen procedures voor beveiliging voor SQL Server wordt uitgevoerd op een Azure virtuele machine controleren. Zie [Beveiligingsoverwegingen voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-security.md)voor meer informatie.

[Het pad leren verkennen](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) voor SQL Server op Azure virtuele machines. 

Zie [SQL Server Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md)voor andere onderwerpen die betrekking hebben op het SQL Server wordt uitgevoerd in Azure VMs.
