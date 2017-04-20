<properties
   pageTitle="Maak een interne taakverdeling met PowerShell in het implementatiemodel klassiek | Microsoft Azure"
   description="Informatie over het maken van een interne taakverdeling met PowerShell in het implementatiemodel klassiek"
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

# <a name="get-started-creating-an-internal-load-balancer-classic-using-powershell"></a>Een interne-taakverdeling (klassiek) met PowerShell maken

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Meer informatie over hoe u [deze stappen uitvoert met behulp van het model Resource Manager](load-balancer-get-started-ilb-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]


[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]


## <a name="create-an-internal-load-balancer-set-for-virtual-machines"></a>Een interne taakverdeling instellen voor virtuele machines maken

Maken van een set interne load balancer en de servers die aan hun verkeer verzendt, hebt u het volgende doen:

1. Een exemplaar maken van interne Netwerktaakverdeling die het eindpunt van het binnenkomend verkeer worden gelijkmatig verdeeld over de servers van een set verdeeld worden.

1. Voeg eindpunten overeenkomen met de virtuele machines die binnenkomend verkeer ontvangt.

1. Configureer de servers die het verzenden van het verkeer naar hun verkeer worden verzonden met het virtuele IP (VIP)-adres van de instantie interne Load Balancing verdeeld.


### <a name="step-1-create-an-internal-load-balancing-instance"></a>Stap 1: Maak een instantie interne taakverdeling

Voor een bestaande cloud-service of een cloud-service geïmplementeerd onder een regionale virtueel netwerk, kunt u een interne taakverdeling exemplaar maken met de volgende Windows PowerShell-opdrachten:

    $svc="<Cloud Service Name>"
    $ilb="<Name of your ILB instance>"
    $subnet="<Name of the subnet within your virtual network>"
    $IP="<The IPv4 address to use on the subnet-optional>"

    Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP


Houd er rekening mee dat dit gebruik van de [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx) Windows PowerShell-cmdlet de parameter DefaultProbe is ingesteld gebruikt. Zie voor meer informatie over aanvullende parametersets, [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx).

### <a name="step-2-add-endpoints-to-the-internal-load-balancing-instance"></a>Stap 2: Eindpunten toevoegen aan de instantie interne taakverdeling

Hier volgt een voorbeeld:

    $svc="mytestcloud"
    $vmname="DB1"
    $epname="TCP-1433-1433"
    $lbsetname="lbset"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    $ilb="ilbset"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -Lbset $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM


### <a name="step-3-configure-your-servers-to-send-their-traffic-to-the-new-internal-load-balancing-endpoint"></a>Stap 3: De servers voor het verzenden van hun verkeer naar het nieuwe endpoint van interne taakverdeling configureren

U hebt voor het configureren van de servers waarvan u het verkeer is het verstandig om voor het gebruik van het nieuwe IP-adres (VIP) van de instantie interne Netwerktaakverdeling taakverdeling. Dit is het adres waarop de interne taakverdeling exemplaar luistert. In de meeste gevallen moet u alleen toevoegen of wijzigen van een DNS-record voor de VIP van het exemplaar interne taakverdeling.

Als u het IP-adres tijdens het maken van het exemplaar interne taakverdeling opgegeven, hebt u al het VIP. Anders ziet u de VIP van de volgende opdrachten:

    $svc="<Cloud Service Name>"
    Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer



Vul de waarden voor het gebruik van deze opdrachten en verwijder de < en >. Hier volgt een voorbeeld:

    $svc="mytestcloud"
    Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer


Noteer het IP-adres van de weergave van de opdracht Get-AzureInternalLoadBalancer, en breng de benodigde wijzigingen aan uw servers of de DNS-records om ervoor te zorgen dat het verkeer wordt verzonden naar de VIP.

>[AZURE.NOTE]Het platform Microsoft Azure maakt gebruik van een statisch, openbaar routeerbaar IPv4-adres voor tal van administratieve scenario's. Het IP-adres is 168.63.129.16. Dit IP-adres moet niet worden geblokkeerd door een firewall, omdat dit leiden onverwacht gedrag tot kan.
>Met betrekking tot Azure interne taakverdeling dit IP-adres gebruikt door sondes van de verdeling van de werklast te controleren om te bepalen van de status voor virtuele machines in een taakverdeling. Als een beveiligingsgroep netwerk verkeer beperken tot Azure virtuele machines in een set intern verdeeld wordt gebruikt of wordt toegepast op een virtueel netwerk Subnet, moet u ervoor zorgen dat een regel voor netwerk wordt toegevoegd aan het verkeer van 168.63.129.16.


## <a name="example-of-internal-load-balancing"></a>Voorbeeld van de interne taakverdeling

Als u alle stappen bij het einde tot einde maken een taakverdeling instellen voor voorbeeldconfiguraties met twee, Zie de volgende secties.

### <a name="an-internet-facing-multi-tier-application"></a>Een internetverbinding, meerlagige toepassing

U wilt een taakverdeling databaseservice voor een reeks Internet facing webservers. Beide groepen servers worden gehost in een enkele Azure cloud-service. Web serververkeer via TCP-poort 1433 moet worden verdeeld over twee virtuele machines in de databaselaag. In afbeelding 1 ziet u de configuratie.

![Interne taakverdeling instellen voor de databaselaag](./media/load-balancer-internal-getstarted/IC736321.png)


De configuratie bestaat uit de volgende opties:

- De bestaande cloud service host voor de virtuele machines met de naam mytestcloud.

- De twee bestaande databaseservers zijn met de naam DB1, DB2.

- Webservers in de weblaag verbinding maken met de database-servers in de databaselaag met behulp van het particuliere IP-adres. Een andere mogelijkheid is om uw eigen DNS gebruiken voor het virtuele netwerk en een A-record voor de interne load balancer handmatig registreren.

De volgende opdrachten voor het configureren van een nieuwe interne Load Balancing sessie met de naam **ILBset** en eindpunten toevoegen aan de virtuele machines die overeenkomt met de twee databaseservers:

    $svc="mytestcloud"
    $ilb="ilbset"
    Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb
    $prot="tcp"
    $locport=1433
    $pubport=1433
    $epname="TCP-1433-1433"
    $lbsetname="lbset"
    $vmname="DB1"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM

    $epname="TCP-1433-1433-2"
    $vmname="DB2"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM


## <a name="remove-an-internal-load-balancing-configuration"></a>Een interne taakverdeling configuratie verwijderen

Om een virtuele machine als een eindpunt van een interne load balancer exemplaar verwijdert, moet u de volgende opdrachten gebruiken:

    $svc="<Cloud service name>"
    $vmname="<Name of the VM>"
    $epname="<Name of the endpoint>"
    Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM

Voor het gebruik van deze opdrachten de waarden invullen, verwijderen van de < en >.

Hier volgt een voorbeeld:

    $svc="mytestcloud"
    $vmname="DB1"
    $epname="TCP-1433-1433"
    Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM

Als een interne load balancer exemplaar uit een cloud-service wilt verwijderen, moet u de volgende opdrachten gebruiken:

    $svc="<Cloud service name>"
    Remove-AzureInternalLoadBalancer -ServiceName $svc

De waarde in te vullen voor het gebruik van deze opdrachten en verwijder de < en >.

Hier volgt een voorbeeld:

    $svc="mytestcloud"
    Remove-AzureInternalLoadBalancer -ServiceName $svc



## <a name="additional-information-about-internal-load-balancer-cmdlets"></a>Als u meer informatie over interne load balancer-cmdlets


Voer de volgende opdrachten bij de opdrachtprompt van Windows PowerShell voor meer informatie over interne lastenverdeling cmdlets:

- Get-help nieuwe AzureInternalLoadBalancerConfig-volledige

- Get-help toevoegen AzureInternalLoadBalancer-volledige

- Get-help Get-AzureInternalLoadbalancer-volledige

- Get-help verwijderen AzureInternalLoadBalancer-volledige

## <a name="next-steps"></a>Volgende stappen

[Configureer een load balancer distributie modus bron-IP-affiniteit](load-balancer-distribution-mode.md)

[Niet-actieve TCP-time-outinstellingen voor de taakverdeling configureren](load-balancer-tcp-idle-timeout.md)