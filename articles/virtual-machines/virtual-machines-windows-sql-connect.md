<properties
    pageTitle="Verbinding maken met een SQL Server virtuele Machine (Resource Manager) | Microsoft Azure"
    description="Informatie over verbinding maken met SQL Server wordt uitgevoerd op een virtuele Machine in Azure. In dit onderwerp wordt het klassieke implementatiemodel. De scenario's verschillen afhankelijk van de netwerkconfiguratie en de locatie van de client."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"    
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/21/2016"
    ms.author="jroth" />

# <a name="connect-to-a-sql-server-virtual-machine-on-azure-resource-manager"></a>Verbinding maken met een SQL Server virtuele Machine op Azure (Resource Manager)

> [AZURE.SELECTOR]
- [Resource Manager](virtual-machines-windows-sql-connect.md)
- [Klassiek](virtual-machines-windows-classic-sql-connect.md)

## <a name="overview"></a>Overzicht

In dit onderwerp wordt beschreven hoe u verbinding maakt met uw exemplaar van SQL Server op een Azure virtuele machine wordt uitgevoerd. Het omvat sommige [scenario's algemene connectiviteit](#connection-scenarios) en vervolgens bevat [gedetailleerde stappen voor het configureren van SQL Server-verbinding in een VM Azure](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel. Zie [verbinding maken met een virtuele Machine op klassieke Azure in SQL Server](virtual-machines-windows-classic-sql-connect.md)de light versie van dit artikel.

Als u liever een volledig overzicht van het aanbod en connectiviteit, Zie [het inrichten van een SQL Server virtuele Machine op Azure](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="connection-scenarios"></a>Scenario's voor verbinding

De manier waarop die een client verbinding maakt met SQL Server wordt uitgevoerd op een virtuele Machine verschilt afhankelijk van de locatie van de client en de configuratie van de computer/netwerk. Deze scenario's zijn onder andere:

- [Verbinding maken met SQL Server via het internet](#connect-to-sql-server-over-the-internet)
- [Verbinding maken met SQL Server in hetzelfde virtuele netwerk](#connect-to-sql-server-in-the-same-virtual-network)

### <a name="connect-to-sql-server-over-the-internet"></a>Verbinding maken met SQL Server via het Internet

Als u verbinding maken met de SQL Server-database-engine van Internet wilt, zijn er verschillende stappen nodig, zoals het configureren van de firewall, SQL-verificatie inschakelen en configureren van uw netwerk beveiligingsgroep hebt u een regel beveiligingsgroep netwerk om TCP-verkeer op poort 1433.

Als u een afbeelding van de virtuele machine SQL Server inrichten met de bronnenbeheerder de portal, worden deze stappen voor u uitgevoerd wanneer u de optie SQL-connectiviteit **openbare** selecteert:

![Openbare SQL-connectiviteit optie tijdens het inrichten](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

Als dit niet een was tijdens het inrichten, kunt vervolgens u handmatig configureren SQL Server en uw virtuele machines door de [stappen in dit artikel een verbinding handmatig configureren](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

>[AZURE.NOTE] Afbeelding van de virtuele machine voor SQL Server Express edition wordt het TCP/IP-protocol niet automatisch ingeschakeld. Voor Express edition, moet u SQL Server Configuration Manager [het TCP/IP-protocol handmatig](#configure-sql-server-to-listen-on-the-tcp-protocol) inschakelen na het maken van de VM.

Zodra dit is gebeurd, kan elke client met toegang tot het internet door te geven op het openbare IP-adres van de virtuele machine of de DNS-label toegewezen aan dat IP-adres verbinding maken met het exemplaar van SQL Server. Als de SQL Server-poort 1433 is, hoeft u niet opgeven in de verbindingsreeks.

    "Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Hoewel dit connectiviteit voor clients via internet maakt, wil dit niet zeggen dat iedereen verbinding met de SQL-Server maken kan. Buiten moeten clients u de juiste gebruikersnaam en wachtwoord. Voor extra beveiliging kunt u de bekende poort 1433 voorkomen. Bijvoorbeeld als u SQL Server geconfigureerd om te luisteren op poort 1500 en juiste firewall en netwerk groep regels vastgesteld, kan u door het poortnummer toe te voegen aan de naam van de Server als volgt:

    "Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

>[AZURE.NOTE] Het is belangrijk als u deze techniek gebruikt om te communiceren met SQL Server, worden alle uitgaande gegevens uit de Azure datacenter is onderworpen aan normale [prijzen voor uitgaande gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/).

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Verbinding maken met SQL Server in hetzelfde virtuele netwerk

[Virtueel netwerk](../virtual-network/virtual-networks-overview.md) kunt nog meer scenario's. Zelfs als de VMs in verschillende resourcegroepen bestaat, kunt u in hetzelfde virtuele netwerk VMs verbinden. En met een [VPN van site naar site](../vpn-gateway/vpn-gateway-site-to-site-create.md), maakt u een hybride architectuur die VMs met netwerken op gebouwen en machines verbindt.

Virtuele netwerken kunt u uw VMs Azure toevoegen aan een domein. Dit is de enige manier om Windows-verificatie met SQL Server. De andere verbinding-scenario's moet SQL-verificatie met gebruikersnamen en wachtwoorden.

Als u een afbeelding van de virtuele machine SQL Server inrichten met de bronnenbeheerder de portal, zijn de juiste firewall-regels voor communicatie op het virtuele netwerk setup wanneer u de optie SQL-connectiviteit **particuliere** selecteert. Als dit niet een was tijdens het inrichten, kunt vervolgens u handmatig configureren SQL Server en uw virtuele machines door de [stappen in dit artikel een verbinding handmatig configureren](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm). Maar als u van plan bent om een domeinomgeving en Windows-verificatie te configureren, hoeft u niet naar de stappen in dit artikel gebruiken voor het configureren van SQL-verificatie en -aanmeldingen. Ook hoeft niet aan de groep netwerk regels configureren voor toegang via het internet.

>[AZURE.NOTE] Afbeelding van de virtuele machine voor SQL Server Express edition wordt het TCP/IP-protocol niet automatisch ingeschakeld. Voor Express edition, moet u SQL Server Configuration Manager [het TCP/IP-protocol handmatig](#configure-sql-server-to-listen-on-the-tcp-protocol) inschakelen na het maken van de VM.

Ervan uitgaande dat u hebt ingesteld in het virtuele netwerk, kunt u uw exemplaar van SQL Server door de naam van de SQL Server VM opgeven in de verbindingsreeks. In het volgende voorbeeld wordt ook aangenomen dat ook Windows-verificatie is geconfigureerd en dat de gebruiker heeft toegang tot de SQL Server-exemplaar.

    "Server=mysqlvm;Integrated Security=true"

Houd er rekening mee dat in dit scenario kan ook u het IP-adres van de VM.

## <a name="steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm"></a>Stappen voor het handmatig configureren van SQL Server-verbinding in een Azure VM

De volgende stappen laten zien hoe u handmatig verbinding met het exemplaar van SQL Server setup en eventueel verbinden via het internet met behulp van SQL Server Management Studio (SSMS). Houd er rekening mee dat veel van deze stappen zijn voor u uitgevoerd wanneer u de gewenste opties voor SQL Server-verbinding in de portal.

Voordat u verbinding met het exemplaar van SQL Server of een andere VM op het internet maken kunt, moet u de volgende taken uitvoeren, zoals wordt beschreven in de volgende secties:

- [TCP-poorten openen in Windows firewall](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [SQL Server luistert op het TCP-protocol configureren](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [SQL-Server configureren voor de verificatiemodus mixed](#configure-sql-server-for-mixed-mode-authentication)
- [SQL Server-verificatie-aanmeldingen maken](#create-sql-server-authentication-logins)
- [Een binnenkomende regel beveiligingsgroep netwerk configureren](#configure-a-network-security-group-inbound-rule-for-the-vm)
- [Een DNS-Label voor het openbare IP-adres configureren](#configure-a-dns-label-for-the-public-ip-address)
- [Verbinding maken met de Database-Engine van een andere computer](#connect-to-the-database-engine-from-another-computer)

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager-nsg-rule.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Volgende stappen

Instructies en connectiviteit als volgt ingericht, Zie [het inrichten van een SQL Server virtuele Machine op Azure](virtual-machines-windows-portal-sql-server-provision.md).

[Het pad leren verkennen](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) voor SQL Server op Azure virtuele machines.

Zie [SQL Server Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md)voor andere onderwerpen die betrekking hebben op het SQL Server wordt uitgevoerd in Azure VMs.
