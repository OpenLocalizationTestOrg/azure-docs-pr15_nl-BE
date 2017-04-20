<properties
   pageTitle="Aan de slag maken van een internetverbinding taakverdeling in het klassieke implementatiemodel met behulp van de CLI Azure | Microsoft Azure"
   description="Informatie over het maken van een internetverbinding taakverdeling in het klassieke implementatiemodel met behulp van de CLI Azure"
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

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-cli"></a>Aan de slag maken van een internetverbinding (klassiek)-taakverdeling in de CLI Azure

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dit artikel heeft betrekking op de klassieke implementatiemodel. U kunt ook [informatie over het maken van een internetverbinding met behulp van bronbeheer Azure taakverdeling](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]


## <a name="step-by-step-creating-an-internet-facing-load-balancer-using-cli"></a>Stap voor stap maken van een internetverbinding met behulp van de CLI-taakverdeling

Deze handleiding wordt beschreven hoe een taakverdeling voor Internet op basis van het bovenstaande scenario.

1. Als u nooit CLI Azure gebruikt nog, Zie [installeren en configureren van de CLI Azure](../../articles/xplat-cli-install.md) en volg de instructies tot aan het punt waar u uw Azure-account en abonnement.

2. Voer de opdracht **azure config-modus** wilt overschakelen naar de klassieke modus, zoals hieronder wordt weergegeven.

        azure config mode asm

    Verwachte output:

        info:    New mode is asm


## <a name="create-endpoint-and-load-balancer-set"></a>Eindpunt maken en instellen van de verdeling van laden

Het scenario wordt ervan uitgegaan dat de virtuele machines "web1" en "web2" zijn gemaakt.
Deze handleiding maakt een load balancer instellen met behulp van poort 80 als openbare poort en poort 80 als lokale poort. Een poort van de sonde wordt ook geconfigureerd op poort 80 en de load balancer set 'lbset' genoemd.


### <a name="step-1"></a>Stap 1

Maak het eerste eindpunt en de belasting voor documentconversies instellen met behulp van `azure network vm endpoint create` voor virtuele machine "web1".

    azure vm endpoint create web1 80 -k 80 -o tcp -t 80 -b lbset

De parameters die worden gebruikt:

**-k** - poort voor lokale virtuele machine<br>
**-o** - protocol<BR>
**-t** - sonde poort<BR>
**-b** - load balancer naam<BR>

## <a name="step-2"></a>Stap 2

Een tweede virtuele machine "web2" toevoegen aan de set load balancer.

    azure vm endpoint create web2 80 -k 80 -o tcp -t 80 -b lbset

## <a name="step-3"></a>Stap 3

Controleer of de load balancer configuratie met behulp van `azure vm show` .

    azure vm show web1

De uitvoer is:

    data:    DNSName "contoso.cloudapp.net"
    data:    Location "East US"
    data:    VMName "web1"
    data:    IPAddress "10.0.0.5"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-2015
    6-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "joaoma-1-web1-0-201509251804250879"
    data:    OSDisk mediaLink "https://XXXXXXXXXXXXXXX.blob.core.windows.
    /vhds/joaomatest-web1-2015-09-25.vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Se
    r-2012-R2-20150916-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "XXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 name "XXXXXXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    Network Endpoints 0 loadBalancedEndpointSetName "lbset"
    data:    Network Endpoints 0 localPort 80
    data:    Network Endpoints 0 name "tcp-80-80"
    data:    Network Endpoints 0 port 80
    data:    Network Endpoints 0 loadBalancerProbe port 80
    data:    Network Endpoints 0 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 0 loadBalancerProbe intervalInSeconds 15
    data:    Network Endpoints 0 loadBalancerProbe timeoutInSeconds 31
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 5986
    data:    Network Endpoints 1 name "PowerShell"
    data:    Network Endpoints 1 port 5986
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 3389
    data:    Network Endpoints 2 name "Remote Desktop"
    data:    Network Endpoints 2 port 58081
    info:    vm show command OK

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Een extern bureaublad-eindpunt voor een virtuele machine maken

Kunt u een extern bureaublad-eindpunt netwerkverkeer van een openbare poort doorsturen naar een lokale poort voor een specifieke virtuele machine met `azure vm endpoint create`.

    azure vm endpoint create web1 54580 -k 3389


## <a name="remove-virtual-machine-from-load-balancer"></a>Verwijderen van de virtuele machine van taakverdeling

U hebt het eindpunt dat is gekoppeld aan de taakverdeling instellen van de virtuele machine te verwijderen. Zodra het eindpunt is verwijderd, de virtuele machine maakt geen deel uit de taakverdeling niet meer instellen.

 Met het bovenstaande voorbeeld, kunt u het eindpunt voor de virtuele machine "web1" gemaakt van taakverdeling 'lbset' met de opdracht `azure vm endpoint delete`.

    azure vm endpoint delete web1 tcp-80-80


>[AZURE.NOTE] U kunt meer opties beheren met de opdracht eindpunten verkennen`azure vm endpoint --help`


## <a name="next-steps"></a>Volgende stappen

[Een interne taakverdeling configureren aan de slag](load-balancer-get-started-ilb-arm-ps.md)

[Een load balancer distributie-modus configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP-time-outinstellingen voor de taakverdeling configureren](load-balancer-tcp-idle-timeout.md)

