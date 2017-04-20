<properties
   pageTitle="Een interne taakverdeling maken met behulp van de CLI Azure in Resource Manager | Microsoft Azure"
   description="Informatie over het maken van een interne taakverdeling met behulp van de CLI Azure in Resource Manager"
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

# <a name="create-an-internal-load-balancer-by-using-the-azure-cli"></a>Een interne taakverdeling maken met behulp van de CLI Azure

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassieke implementatiemodel](load-balancer-get-started-ilb-classic-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-solution-by-using-the-azure-cli"></a>De oplossing implementeren met behulp van de CLI Azure

De volgende stappen laten zien hoe een internetgerichte taakverdeling maken met behulp van bronbeheer Azure met CLI. Met Azure Resource Manager elke resource is gemaakt en afzonderlijk geconfigureerd en deze vervolgens samen voor een bron maken.

U moet maken en configureren van de volgende objecten als u wilt een load balancer implementeren:

- **Front-end IP-configuratie**: bevat openbare IP-adressen voor binnenkomende netwerkverkeer
- **Back-end-adresgroep**: bevat netwerkinterfaces (NIC's) waarmee de virtuele machines voor het netwerkverkeer ontvangen van de taakverdeling
- **Load balancing regels**: regels bevat die een openbare poort in de taakverdelingsvoorziening aan de poort in de back-end-adresgroep toewijzen
- **NAT binnenkomende regels**: regels bevat die een openbare poort in de taakverdelingsvoorziening aan een poort voor een specifieke virtuele machine in de back-end-adresgroep toewijzen
- **Sondes**: gezondheid sondes die worden gebruikt om de beschikbaarheid van exemplaren van de virtuele machines in de back-end-adresgroep bevat

Zie [Bronbeheer Azure ondersteuning voor taakverdeling](load-balancer-arm.md)voor meer informatie.

## <a name="set-up-cli-to-use-resource-manager"></a>CLI instellen voor gebruik van Resource Manager

1. Als u nooit CLI Azure gebruikt nog, Zie [installeren en configureren van de CLI Azure](../../articles/xplat-cli-install.md). Volg de instructies tot aan het punt waar u uw Azure-account en abonnement.

2. Voer de opdracht **azure config-modus** wilt overschakelen naar de modus Resource Manager als volgt:

        azure config mode arm

    Verwachte output:

        info:    New mode is arm

## <a name="create-an-internal-load-balancer-step-by-step"></a>Een interne taakverdeling maken stap voor stap

1. Aanmelden bij Azure.

        azure login

    Voer desgevraagd uw Azure referenties.

2. De opdracht Extra Azure Resource Manager-modus wijzigen.

        azure config mode arm

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Alle bronnen in Azure Resource Manager zijn gekoppeld aan een resourcegroep. Als u dit nog niet hebt gedaan, kunt u een resourcegroep maken.

    azure group create <resource group name> <location>

## <a name="create-an-internal-load-balancer-set"></a>Een interne load balancer-set maken

1. Maak een interne taakverdeling

    In het volgende scenario wordt een groep met de naam nrprg gemaakt in de regio Oost-VS.

        azure network lb create --name nrprg --location eastus

    >[AZURE.NOTE] Alle bronnen voor een interne Netwerktaakverdeling, zoals virtuele netwerken en virtueel netwerksubnetten, moeten in dezelfde resourcegroep en in hetzelfde gebied.

2. Maak een front-end-IP-adres voor de interne taakverdeling.

    Het IP-adres dat u gebruikt, moet binnen het subnetbereik van het virtuele netwerk.

        azure network lb frontend-ip create --resource-group nrprg --lb-name ilbset --name feilb --private-ip-address 10.0.0.7 --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet

3. De back-end-adresgroep maken.

        azure network lb address-pool create --resource-group nrprg --lb-name ilbset --name beilb

    Nadat u een front-end-IP-adres en een back-end-adresgroep hebt gedefinieerd, kunt u load balancer regels voor binnenkomende NAT regels en aangepaste gezondheid sondes.

4. Maak een regel load balancer voor de interne taakverdeling.

    Als u de vorige stappen hebt uitgevoerd, wordt de opdracht een regel load balancer voor het luisteren op poort 1433 in de front-end-toepassingen en verdeeld netwerkverkeer wilt verzenden naar de back-end-adresgroep, ook via poort 1433 gemaakt.

        azure network lb rule create --resource-group nrprg --lb-name ilbset --name ilbrule --protocol tcp --frontend-port 1433 --backend-port 1433 --frontend-ip-name feilb --backend-address-pool-name beilb

5. Regels van inkomende NAT maken.

    NAT regels voor binnenkomende verbindingen worden gebruikt om de eindpunten in een load balancer naar een exemplaar van de specifieke virtuele machine maken. De vorige stappen gemaakt twee NAT regels voor extern bureaublad.

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule1 --protocol TCP --frontend-port 5432 --backend-port 3389

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule2 --protocol TCP --frontend-port 5433 --backend-port 3389

6. Maak gezondheid sondes voor de taakverdeling.

    Een sonde gezondheid controleert alle instanties van virtuele machine om ervoor te zorgen dat ze netwerkverkeer kunnen verzenden. Het exemplaar van de virtuele machine met sonde mislukte controles is verwijderd uit de taakverdeling totdat deze weer on line gaat en een selectievakje sonde bepaalt dat het gezond is.

        azure network lb probe create --resource-group nrprg --lb-name ilbset --name ilbprobe --protocol tcp --interval 300 --count 4

    >[AZURE.NOTE]Het platform Microsoft Azure maakt gebruik van een statisch, openbaar routeerbaar IPv4-adres voor tal van administratieve scenario's. Het IP-adres is 168.63.129.16. Dit IP-adres moet niet worden geblokkeerd door een firewall, omdat dit leiden onverwacht gedrag tot kan.
    >Met betrekking tot Azure interne taakverdeling, dit IP-adres gebruikt door sondes van de verdeling van de werklast te controleren om te bepalen van de status voor virtuele machines in een evenwichtige taakverdeling. Als een beveiligingsgroep netwerk verkeer beperken tot Azure virtuele machines in een set intern verdeeld wordt gebruikt of wordt toegepast op een virtueel netwerk subnet, moet u ervoor zorgen dat een regel voor netwerk wordt toegevoegd aan het verkeer van 168.63.129.16.

## <a name="create-nics"></a>NIC's maken

U moet NIC's maken (of bestaande wijzigen) en koppel deze aan de NAT-regels, regels voor load balancer en sondes.

1. Maak een NIC met de naam *lb nic1 worden*, en vervolgens te koppelen aan de regel *rdp1* NAT en de back-end-adresgroep *beilb* .

        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" --location eastus

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

2. Maak een NIC met de naam *lb nic2 worden*, en vervolgens te koppelen aan de regel *rdp2* NAT en de back-end-adresgroep *beilb* .

        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" --location eastus

3. Een virtuele machine met de naam *DB1*maken en vervolgens te koppelen aan de Netwerkkaart met de naam *lb nic1 worden*. Een zogenaamde *web1nrp* opslag-account is gemaakt voordat de volgende opdracht wordt uitgevoerd:

        azure vm create --resource--resource-grouproup nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

    >[AZURE.IMPORTANT] VMs in een load balancer moeten in bepaalde beschikbaarheid. Gebruik `azure availset create` instellen voor het maken van de beschikbaarheid.

4. Maak een virtuele machine (VM) met de naam *DB2*en vervolgens te koppelen aan de Netwerkkaart met de naam *lb nic2 worden*. Een zogenaamde *web1nrp* opslag-account is gemaakt voordat de volgende opdracht wordt uitgevoerd.

        azure vm create --resource--resource-grouproup nrprg --name DB2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## <a name="delete-a-load-balancer"></a>Een load balancer verwijderen

Wilt een load balancer verwijderen, gebruikt u de volgende opdracht:

    azure network lb delete --resource-group nrprg --name ilbset

## <a name="next-steps"></a>Volgende stappen

[Een load balancer distributie-modus configureren met behulp van bron-IP-affiniteit](load-balancer-distribution-mode.md)

[Niet-actieve TCP-time-outinstellingen voor de taakverdeling configureren](load-balancer-tcp-idle-timeout.md)
