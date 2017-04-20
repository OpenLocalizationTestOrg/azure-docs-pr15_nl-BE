<properties
   pageTitle="Een Internet facing taakverdeling in Resource Manager, met behulp van de CLI Azure maken | Microsoft Azure"
   description="Informatie over het maken van een internetverbinding in Resource Manager, met behulp van de CLI Azure-taakverdeling"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="creating-an-internal-load-balancer-using-the-azure-cli"></a>Maken van een interne taakverdeling met behulp van de CLI Azure

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dit artikel heeft betrekking op het implementatiemodel Resource Manager. U kunt ook [informatie over het maken van een internetverbinding met klassieke implementatie taakverdeling](load-balancer-get-started-internet-classic-portal.md)


[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-using-the-azure-cli"></a>Implementatie van de oplossing met behulp van de CLI Azure

De volgende stappen laten zien hoe een internetverbinding met behulp van bronbeheer Azure met CLI-taakverdeling te maken. Azure Resource Manager elke resource wordt gemaakt en vervolgens put samen om een bron van afzonderlijk geconfigureerd.

U moet maken en configureren van de volgende objecten als u wilt een load balancer implementeren:

- IP-configuratie van front-end - bevat openbare IP-adressen voor binnenkomende netwerkverkeer.
- Back-end-adresgroep - bevat netwerkinterfaces (NIC's) voor de virtuele machines voor het netwerkverkeer ontvangen van de taakverdeling.
- Regels voor taakverdeling - regels een openbare poort op de taakverdeling toe te wijzen aan de poort van de back-end-adresgroep bevat.
- Binnenkomende regels NAT - regels een openbare poort op de taakverdeling toe te wijzen aan een poort voor een specifieke virtuele machine in de back-end-adresgroep bevat.
- Sondes - gezondheid sondes gebruikt om te controleren van de beschikbaarheid van exemplaren van de virtuele machines in de back-end-adresgroep bevat.

Zie [Bronbeheer Azure ondersteuning voor taakverdeling](load-balancer-arm.md)voor meer informatie.

## <a name="set-up-cli-to-use-resource-manager"></a>CLI instellen voor gebruik van Resource Manager

1. Als u nooit CLI Azure gebruikt nog, Zie [installeren en configureren van de CLI Azure](../../articles/xplat-cli-install.md) en volg de instructies tot aan het punt waar u uw Azure-account en abonnement.

2. Voer de opdracht **azure config-modus** wilt overschakelen naar de modus Resource Manager, zoals hieronder wordt weergegeven.

        azure config mode arm

    Verwachte output:

        info:    New mode is arm

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Een virtueel netwerk en een openbaar IP-adres voor de front-IP-adresgroep maken

1. Maak een virtueel netwerk (VNet) met de naam *NRPVnet* in de Oost-Amerikaanse locatie met behulp van een groep met de naam *NRPRG*.

        azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16

    Een *NRPVnetSubnet* met een blok CIDR 10.0.0.0/24 in *NRPVnet*met de naam subnet maken.

        azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24

2. Maak een openbaar IP-adres met de naam *NRPPublicIP* worden gebruikt door een front-end-IP-adresgroep met DNS-naam *loadbalancernrp.eastus.cloudapp.azure.com*. De volgende opdracht wordt de statische toewijzing en de time-out voor inactiviteit van 4 minuten.

        azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4

    >[AZURE.IMPORTANT]Verdeling van de belasting wordt het domeinlabel van de openbare IP-als de FQDN-naam gebruikt. Dit een wijziging van de klassieke distributie die gebruikmaakt van de cloud-service als de verdeling van de belasting volledig domein naam (Fully Qualified Domain Name).
    >In dit voorbeeld wordt de FQDN-naam *loadbalancernrp.eastus.cloudapp.azure.com*.

## <a name="create-a-load-balancer"></a>Maak een load balancer

De volgende opdracht maakt u een taakverdeling met de naam *NRPlb* in de resourcegroep van de *NRPRG* in de *Oost-Amerikaanse* Azure locatie.

    azure network lb create NRPRG NRPlb eastus

## <a name="create-a-front-end-ip-pool-and-a-backend-address-pool"></a>Een IP-adresgroep front-end en een back-end-adresgroep maken

In dit voorbeeld laat zien hoe de front-IP-adresgroep die ontvangt de binnenkomende netwerkverkeer op de verdeling van de belasting en de back-end-IP-adresgroep waarin de front-end toepassingen het netwerkverkeer taakverdeling stuurt maken.

1. Maak een front-end-IP-adresgroep koppelen van het openbare IP gemaakt in de vorige stap en de verdeling van de belasting.

        azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip

2. Een back-end-adresgroep gebruikt voor het ontvangen van binnenkomend verkeer van de front-end-IP-adresgroep ingesteld.

        azure network lb address-pool create NRPRG NRPlb NRPbackendpool

## <a name="create-lb-rules-nat-rules-and-probe"></a>LB regels, regels voor NAT en sonde maken

In dit voorbeeld wordt de volgende items.

- een NAT regel omzetten van alle binnenkomende verkeer op poort 21 op poort 22<sup>1</sup>
- een NAT regel vertalen van al het inkomende verkeer op poort 23 naar poort 22
- een load balancer regel voor evenwicht tussen al het binnenkomende verkeer op poort 80 voor poort 80 op de adressen in de groep back-end.
- een regel van de sonde te controleren van de status van een pagina met de naam *HealthProbe.aspx*.

<sup>1</sup> NAT regels zijn gekoppeld aan een specifieke virtuele machine instantie achter de taakverdeling. Het netwerkverkeer op poort 21 binnenkomen wordt verzonden naar een specifieke virtuele machine op poort 22 die zijn gekoppeld aan deze regel NAT. U moet een protocol (UDP of TCP) opgeven voor een NAT regel. Beide protocollen worden niet toegewezen aan dezelfde poort.

1. De NAT-regels maken.

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh1 --protocol tcp --frontend-port 21 --backend-port 22
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh2 --protocol tcp --frontend-port 23 --backend-port 22

2. Maak een regel load balancer.

        azure network lb rule create nrprg nrplb lbrule -p tcp -f 80 -b 80 -t NRPfrontendpool -o NRPbackendpool

3. Maak een sonde met gezondheid.

        azure network lb probe create --resource-group nrprg --lb-name nrplb --name healthprobe --protocol "http" --port 80 --path healthprobe.aspx --interval 15 --count 4

4. Controleer de instellingen.

        azure network lb show nrprg nrplb

    Verwachte output:

        info:    Executing command network lb show
        + Looking up the load balancer "nrplb"
        + Looking up the public ip "NRPPublicIP"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb
        data:    Name                            : nrplb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : eastus
        data:    Provisioning State              : Succeeded
        data:    Frontend IP configurations:
        data:      Name                          : NRPfrontendpool
        data:      Provisioning state            : Succeeded
        data:      Public IP address id          : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/publicIPAddresses/NRPPublicIP
        data:      Public IP allocation method   : Static
        data:      Public IP address             : 40.114.13.145
        data:
        data:    Backend address pools:
        data:      Name                          : NRPbackendpool
        data:      Provisioning state            : Succeeded
        data:
        data:    Load balancing rules:
        data:      Name                          : HTTP
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 80
        data:      Backend port                  : 80
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:      Backend address pool          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:
        data:    Inbound NAT rules:
        data:      Name                          : ssh1
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 21
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:      Name                          : ssh2
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 23
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:    Probes:
        data:      Name                          : healthprobe
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Http
        data:      Port                          : 80
        data:      Interval in seconds           : 15
        data:      Number of probes              : 4
        data:
        info:    network lb show command OK

## <a name="create-nics"></a>NIC's maken

U moet NIC's maken (of bestaande wijzigen) en koppel deze aan de NAT-regels, regels voor load balancer en sondes.

1. Maak een NIC met de naam *lb nic1 worden*, en te koppelen aan de regel *rdp1* NAT en de back-end-adresgroep *NRPbackendpool* .

        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus

    Verwachte output:

        info:    Executing command network nic create
        + Looking up the network interface "lb-nic1-be"
        + Looking up the subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up the network interface "lb-nic1-be"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        data:    Name                            : lb-nic1-be
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : eastus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 10.0.0.4
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet
        data:      Load balancer backend address pools
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:      Load balancer inbound NAT rules:
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1
        data:
        info:    network nic create command OK

2. Maak een NIC met de naam *lb nic2 worden*, en te koppelen aan de regel *rdp2* NAT en de back-end-adresgroep *NRPbackendpool* .

        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus

3. Een virtuele machine (VM) met de naam *web1*maken en koppelen aan de Netwerkkaart met de naam *lb nic1 worden*. Een zogenaamde *web1nrp* opslag-account is gemaakt voordat u de opdracht hieronder.

        azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

    >[AZURE.IMPORTANT] VMs in een load balancer moeten in bepaalde beschikbaarheid. Gebruik `azure availset create` instellen voor het maken van de beschikbaarheid.

    De uitvoer is vergelijkbaar met het volgende:

        info:    Executing command vm create
        + Looking up the VM "web1"
        Enter username: azureuser
        Enter password for azureuser: *********
        Confirm password: *********
        info:    Using the VM Size "Standard_A1"
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account web1nrp
        + Looking up the availability set "nrp-avset"
        info:    Found an Availability set "nrp-avset"
        + Looking up the NIC "lb-nic1-be"
        info:    Found an existing NIC "lb-nic1-be"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in the NIC "lb-nic1-be"
        info:    This is a NIC without publicIP configured
        + Creating VM "web1"
        info:    vm create command OK

    >[AZURE.NOTE] Het informatieve bericht dat **dit een NIC zonder publicIP geconfigureerd is** sinds de NIC gemaakt voor de verdeling van de belasting verbinden met Internet via openbare IP-adres van de load balancer wordt verwacht.

    Aangezien de *lb nic1 worden* NIC is gekoppeld aan de *rdp1* NAT regel, kunt u *web1* met RDP via poort 3441 van verdeling van de belasting.

4. Een virtuele machine (VM) met de naam *web2*maken en koppelen aan de Netwerkkaart met de naam *lb nic2 worden*. Een zogenaamde *web1nrp* opslag-account is gemaakt voordat u de opdracht hieronder.

        azure vm create --resource-group nrprg --name web2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## <a name="update-an-existing-load-balancer"></a>Bijwerken van een bestaande taakverdeling

U kunt verwijzen naar een bestaande taakverdeling regels toevoegen. In het volgende voorbeeld wordt wordt een nieuwe regel voor load balancer toegevoegd aan een bestaande taakverdeling **NRPlb**

    azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule2 --protocol tcp --frontend-port 8080 --backend-port 8051 --frontend-ip-name frontendnrppool --backend-address-pool-name NRPbackendpool

## <a name="delete-a-load-balancer"></a>Een load balancer verwijderen

Gebruik de volgende opdracht voor het verwijderen van een load balancer:

    azure network lb delete --resource-group nrprg --name nrplb

## <a name="next-steps"></a>Volgende stappen

[Een interne taakverdeling configureren aan de slag](load-balancer-get-started-ilb-arm-cli.md)

[Een load balancer distributie-modus configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP-time-outinstellingen voor de taakverdeling configureren](load-balancer-tcp-idle-timeout.md)
