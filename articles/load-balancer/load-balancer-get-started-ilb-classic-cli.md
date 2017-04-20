<properties
   pageTitle="Maak een interne taakverdeling met behulp van de CLI Azure in het implementatiemodel klassiek | Microsoft Azure"
   description="Informatie over het maken van een interne taakverdeling met behulp van de CLI Azure in het implementatiemodel klassiek"
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
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internal-load-balancer-classic-using-the-azure-cli"></a>Een interne-taakverdeling (klassiek) met behulp van de CLI Azure maken

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Meer informatie over hoe u [deze stappen uitvoert met behulp van het model Resource Manager](load-balancer-get-started-ilb-arm-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]


## <a name="to-create-an-internal-load-balancer-set-for-virtual-machines"></a>Een interne taakverdeling instellen voor virtuele machines maken

U maakt een set interne load balancer en de servers die aan hun verkeer verzendt, moet u het volgende doen:

1. Een exemplaar maken van interne Netwerktaakverdeling die het eindpunt van het binnenkomend verkeer worden gelijkmatig verdeeld over de servers van een set verdeeld worden.

1. Voeg eindpunten overeenkomen met de virtuele machines die binnenkomend verkeer ontvangt.

1. Configureer de servers die het verzenden van het verkeer naar hun verkeer worden verzonden met het virtuele IP (VIP)-adres van de instantie interne Load Balancing verdeeld.

## <a name="step-by-step-creating-an-internal-load-balancer-using-cli"></a>Stap voor stap maken van een interne taakverdeling met behulp van de CLI

Deze handleiding wordt beschreven hoe een interne taakverdeling op basis van het bovenstaande scenario.

1. Als u nooit CLI Azure gebruikt nog, Zie [installeren en configureren van de CLI Azure](../../articles/xplat-cli-install.md) en volg de instructies tot aan het punt waar u uw Azure-account en abonnement.

2. Voer de opdracht **azure config-modus** wilt overschakelen naar de klassieke modus, zoals hieronder wordt weergegeven.

        azure config mode asm

    Verwachte output:

        info:    New mode is asm


## <a name="create-endpoint-and-load-balancer-set"></a>Eindpunt maken en instellen van de verdeling van laden

Het scenario wordt ervan uitgegaan dat de virtuele machines "BW1" en "DB2" in een cloud-service 'mytestcloud' genoemd. Beide virtuele machines worden met behulp van een virtueel netwerk mijn "testvnet" met "subnet 1" subnet genoemd.

Deze handleiding maakt een interne load balancer set met poort 1433 als private poort en 1433 als lokale poort.

Dit is een algemeen scenario waar u SQL virtuele machines op de achtergrond met behulp van een interne taakverdeling te garanderen dat de databaseservers won't worden blootgesteld via een openbaar IP-adres hebt.


### <a name="step-1"></a>Stap 1

Maak een interne taakverdeling instellen met behulp van `azure network service internal-load-balancer add`.

     azure service internal-load-balancer add -r mytestcloud -n ilbset -t subnet-1 -a 192.168.2.7

De parameters die worden gebruikt:

**-r** - naam van de service cloud<BR>
**-n** - interne load balancer naam<BR>
**-t** - subnetnaam (hetzelfde subnet door de virtuele machines die u aan de interne taakverdeling toevoegen wilt)<BR>
**-een** - (optioneel) een statisch particuliere IP-adres toevoegen<BR>

Raadpleeg `azure service internal-load-balancer --help` voor meer informatie.

U kunt controleren de interne load balancer-eigenschappen met de opdracht `azure service internal-load-balancer list` *cloud-servicenaam*.

Hier volgt een voorbeeld van de uitvoer:

    azure service internal-load-balancer list my-testcloud
    info:    Executing command service internal-load-balancer list
    + Getting cloud service deployment
    data:    Name    Type     SubnetName  StaticVirtualNetworkIPAddress
    data:    ------  -------  ----------  -----------------------------
    data:    ilbset  Private  subnet-1    192.168.2.7
    info:    service internal-load-balancer list command OK


## <a name="step-2"></a>Stap 2

U kunt de interne load balancer set configureren wanneer u het eerste eindpunt toevoegt. U kunt het eindpunt, virtuele machine en sonde poort aan de interne load balancer set in deze stap wilt koppelen.

    azure vm endpoint create db1 1433 -k 1433 tcp -t 1433 -r tcp -e 300 -f 600 -i ilbset

De parameters die worden gebruikt:

**-k** - poort voor lokale virtuele machine<BR>
**-t** - sonde poort<BR>
**-r** - sonde protocol<BR>
**-e** - sonde interval in seconden<BR>
**-f** - time-outperiode in seconden <BR>
**-i** - interne load balancer naam <BR>


## <a name="step-3"></a>Stap 3

Controleer of de load balancer configuratie met behulp van `azure vm show` *de naam van de virtuele machine*

    azure vm show DB1

De uitvoer is:

        azure vm show DB1
    info:    Executing command vm show
    + Getting virtual machines
    data:    DNSName "mytestcloud.cloudapp.net"
    data:    Location "East US 2"
    data:    VMName "DB1"
    data:    IPAddress "192.168.2.4"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "db1-DB1-0-201511120457370846"
    data:    OSDisk mediaLink "https://XXXX.blob.core.windows.net/vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "137.116.64.107"
    data:    VirtualIPAddresses 0 name "db1ContractContract"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    VirtualIPAddresses 1 address "192.168.2.7"
    data:    VirtualIPAddresses 1 name "ilbset"
    data:    Network Endpoints 0 localPort 5986
    data:    Network Endpoints 0 name "PowerShell"
    data:    Network Endpoints 0 port 5986
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 3389
    data:    Network Endpoints 1 name "Remote Desktop"
    data:    Network Endpoints 1 port 60173
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 1433
    data:    Network Endpoints 2 name "tcp-1433-1433"
    data:    Network Endpoints 2 port 1433
    data:    Network Endpoints 2 loadBalancerProbe port 1433
    data:    Network Endpoints 2 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 2 loadBalancerProbe intervalInSeconds 300
    data:    Network Endpoints 2 loadBalancerProbe timeoutInSeconds 600
    data:    Network Endpoints 2 protocol "tcp"
    data:    Network Endpoints 2 virtualIPAddress "192.168.2.7"
    data:    Network Endpoints 2 enableDirectServerReturn false
    data:    Network Endpoints 2 loadBalancerName "ilbset"
    info:    vm show command OK


## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Een extern bureaublad-eindpunt voor een virtuele machine maken

Kunt u een extern bureaublad-eindpunt netwerkverkeer van een openbare poort doorsturen naar een lokale poort voor een specifieke virtuele machine met `azure vm endpoint create`.

    azure vm endpoint create web1 54580 -k 3389


## <a name="remove-virtual-machine-from-load-balancer"></a>Verwijderen van de virtuele machine van taakverdeling

U kunt een virtuele machine verwijderen uit een interne taakverdeling instellen door het verwijderen van de bijbehorende eindpunt. Zodra het eindpunt is verwijderd, de virtuele machine won't deel uitmaken van de verdeling van de belasting niet meer instellen.

 Het bovenstaande voorbeeld wordt gebruikt, kunt u het eindpunt voor virtuele machine "BW1" van interne taakverdeling "ilbset" gemaakt met de opdracht `azure vm endpoint delete`.

    azure vm endpoint delete DB1 tcp-1433-1433


Raadpleeg `azure vm endpoint --help` voor meer informatie.


## <a name="next-steps"></a>Volgende stappen

[Configureer een load balancer distributie modus bron-IP-affiniteit](load-balancer-distribution-mode.md)

[Niet-actieve TCP-time-outinstellingen voor de taakverdeling configureren](load-balancer-tcp-idle-timeout.md)