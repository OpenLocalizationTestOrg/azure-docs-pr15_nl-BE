<properties
    pageTitle="Een Internet facing taakverdeling met IPv6 in Azure Resource Manager met behulp van de CLI Azure maken | Microsoft Azure"
    description="Informatie over het maken van een internetverbinding met IPv6 in Azure Resource Manager met behulp van de CLI Azure-taakverdeling"
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="IPv6, azure-taakverdeling, dual-stack, openbaar IP-, systeemeigen ipv6, mobiele, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="create-an-internet-facing-load-balancer-with-ipv6-in-azure-resource-manager-using-the-azure-cli"></a>Maken van een internetverbinding met IPv6 in Azure Resource Manager met behulp van de CLI Azure-taakverdeling

> [AZURE.SELECTOR]
- [PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Azure CLI](./load-balancer-ipv6-internet-cli.md)
- [Sjabloon](./load-balancer-ipv6-internet-template.md)

Azure-taakverdeling is een taakverdeling laag-4 (TCP, UDP). De taakverdeling biedt een hoge beschikbaarheid door binnenkomend verkeer over gezonde service-exemplaren in de cloud-services of virtuele machines in een load balancer verdelen. Azure taakverdeling kan ook presenteren die diensten op meerdere poorten of meerdere IP-adressen.

## <a name="example-deployment-scenario"></a>Voorbeeld van de implementatie van scenario

In het volgende diagram ziet u de oplossing voor taakverdeling wordt geïmplementeerd met behulp van het van de voorbeeldsjabloon die in dit artikel wordt beschreven.

![Load balancer scenario](./media/load-balancer-ipv6-internet-cli/lb-ipv6-scenario-cli.png)

In dit scenario maakt u de volgende bronnen Azure:

- twee virtuele machines (VMs)
- een virtuele netwerkinterface voor elke VM met zowel IPv4 als IPv6-adressen die zijn toegewezen
- de taakverdeling van een internetverbinding met een IPv4- en IPv6-openbare IP-adres
- een beschikbaarheid ingesteld op de bevat de twee VMs
- twee laden balancing regels voor de openbare VIP's worden toegewezen aan de particuliere eindpunten

## <a name="deploying-the-solution-using-the-azure-cli"></a>Implementatie van de oplossing met behulp van de CLI Azure

De volgende stappen laten zien hoe een internetverbinding met behulp van bronbeheer Azure met CLI-taakverdeling te maken. Met Azure Resource Manager elke resource is gemaakt en afzonderlijk geconfigureerd en deze vervolgens samen voor een bron maken.

Voor de implementatie van een load balancer maken en configureren van de volgende objecten:

- IP-configuratie van front-end - bevat openbare IP-adressen voor binnenkomende netwerkverkeer.
- Back-end-adresgroep - bevat netwerkinterfaces (NIC's) voor de virtuele machines voor het netwerkverkeer ontvangen van de taakverdeling.
- Regels voor taakverdeling - regels een openbare poort op de taakverdeling toe te wijzen aan de poort van de back-end-adresgroep bevat.
- Binnenkomende regels NAT - regels een openbare poort op de taakverdeling toe te wijzen aan een poort voor een specifieke virtuele machine in de back-end-adresgroep bevat.
- Sondes - gezondheid sondes gebruikt om te controleren van de beschikbaarheid van exemplaren van de virtuele machines in de back-end-adresgroep bevat.

Zie [Bronbeheer Azure ondersteuning voor taakverdeling](load-balancer-arm.md)voor meer informatie.

## <a name="set-up-your-cli-environment-to-use-azure-resource-manager"></a>Instellen van uw omgeving CLI Azure Resource Manager gebruiken

In dit voorbeeld zijn we de CLI's uitgevoerd in een opdrachtvenster PowerShell. We gebruiken de Azure PowerShell-cmdlets niet maar gebruiken we de scriptmogelijkheden van PowerShell hergebruik en de leesbaarheid te verbeteren.

1. Als u nooit CLI Azure gebruikt nog, Zie [installeren en configureren van de CLI Azure](../../articles/xplat-cli-install.md) en volg de instructies tot aan het punt waar u uw Azure-account en abonnement.

2. Voer de opdracht **azure config-modus** wilt overschakelen naar de modus Resource Manager.

        azure config mode arm

    Verwachte output:

        info:    New mode is arm

3. Aanmelden bij Azure en een lijst met abonnementen.

        azure login

    Geef uw Azure referenties wanneer daarom wordt gevraagd.

        azure account list

    Kies het abonnement dat u wilt gebruiken. Noteer de abonnements-Id voor de volgende stap.

4. Variabelen voor gebruik met de opdrachten CLI PowerShell instellen.

        ```
        $subscriptionid = "########-####-####-####-############"  # enter subscription id
        $location = "southcentralus"
        $rgName = "pscontosorg1southctrlus09152016"
        $vnetName = "contosoIPv4Vnet"
        $vnetPrefix = "10.0.0.0/16"
        $subnet1Name = "clicontosoIPv4Subnet1"
        $subnet1Prefix = "10.0.0.0/24"
        $subnet2Name = "clicontosoIPv4Subnet2"
        $subnet2Prefix = "10.0.1.0/24"
        $dnsLabel = "contoso09152016"
        $lbName = "myIPv4IPv6Lb"
        ```

## <a name="create-a-resource-group-a-load-balancer-a-virtual-network-and-subnets"></a>Een groep, een taakverdeling, een virtueel netwerk en subnetten maken

1. Een resourcegroep maken

        azure group create $rgName $location

2. Maak een load balancer

        $lb = azure network lb create --resource-group $rgname --location $location --name $lbName

3. Maak een virtueel netwerk (VNet).

        $vnet = azure network vnet create  --resource-group $rgname --name $vnetName --location $location --address-prefixes $vnetPrefix

    Twee subnetten maken in deze VNet.

        $subnet1 = azure network vnet subnet create --resource-group $rgname --name $subnet1Name --address-prefix $subnet1Prefix --vnet-name $vnetName
        $subnet2 = azure network vnet subnet create --resource-group $rgname --name $subnet2Name --address-prefix $subnet2Prefix --vnet-name $vnetName

## <a name="create-public-ip-addresses-for-the-front-end-pool"></a>Openbare IP-adressen voor de front-end-toepassingen maken

1. De PowerShell-variabelen instellen

        $publicIpv4Name = "myIPv4Vip"
        $publicIpv6Name = "myIPv6Vip"

2. Een openbaar IP-adres van de front-IP-adresgroep maken.

        $publicipV4 = azure network public-ip create --resource-group $rgname --name $publicIpv4Name --location $location --ip-version IPv4 --allocation-method Dynamic --domain-name-label $dnsLabel
        $publicipV6 = azure network public-ip create --resource-group $rgname --name $publicIpv6Name --location $location --ip-version IPv6 --allocation-method Dynamic --domain-name-label $dnsLabel

    >[AZURE.IMPORTANT]Verdeling van de belasting wordt het domeinlabel van de openbare IP-als de FQDN-naam. Dit een wijziging van de klassieke distributie die gebruikmaakt van de cloud-service een naam als de FQDN-naam van de taakverdeling.
    >In dit voorbeeld wordt de FQDN-naam *contoso09152016.southcentralus.cloudapp.azure.com*.

## <a name="create-front-end-and-back-end-pools"></a>Front- en back-end-toepassingen maken

In dit voorbeeld maakt de front-IP-adresgroep die ontvangt de binnenkomende netwerkverkeer op de taakverdeling en de back-end-IP-adresgroep waarin de front-end toepassingen het netwerkverkeer voor de taakverdeling stuurt.

1. De PowerShell-variabelen instellen

        $frontendV4Name = "FrontendVipIPv4"
        $frontendV6Name = "FrontendVipIPv6"
        $backendAddressPoolV4Name = "BackendPoolIPv4"
        $backendAddressPoolV6Name = "BackendPoolIPv6"

2. Maak een front-end-IP-adresgroep koppelen van het openbare IP gemaakt in de vorige stap en de verdeling van de belasting.

        $frontendV4 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV4Name --public-ip-name $publicIpv4Name --lb-name $lbName
        $frontendV6 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV6Name --public-ip-name $publicIpv6Name --lb-name $lbName
        $backendAddressPoolV4 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV4Name --lb-name $lbName
        $backendAddressPoolV6 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV6Name --lb-name $lbName

## <a name="create-the-probe-nat-rules-and-lb-rules"></a>De sonde, NAT-regels en LB regels maken

In dit voorbeeld wordt de volgende items:

- een regel van de sonde te controleren of de verbinding met TCP-poort 80
- een NAT regel op het vertalen van al het inkomende verkeer op poort 3389, de poort 3389 voor RDP-<sup>1</sup>
- een NAT regel vertalen alle binnenkomend verkeer op poort 3389 voor RDP-<sup>1</sup> -poort 3391
- een load balancer regel voor evenwicht tussen al het binnenkomende verkeer op poort 80 voor poort 80 op de adressen in de groep back-end.

<sup>1</sup> NAT regels zijn gekoppeld aan een specifieke virtuele machine instantie achter de taakverdeling. Het netwerkverkeer dat aankomt op poort 3389 wordt verzonden naar de specifieke virtuele machine en de poort die is gekoppeld aan de NAT regel. U moet een protocol (UDP of TCP) opgeven voor een NAT regel. Beide protocollen worden niet toegewezen aan dezelfde poort.

1. De PowerShell-variabelen instellen

        $probeV4V6Name = "ProbeForIPv4AndIPv6"
        $natRule1V4Name = "NatRule-For-Rdp-VM1"
        $natRule2V4Name = "NatRule-For-Rdp-VM2"
        $lbRule1V4Name = "LBRuleForIPv4-Port80"
        $lbRule1V6Name = "LBRuleForIPv6-Port80"

2. De sonde maken

    Het volgende voorbeeld wordt een TCP-sonde waarmee wordt gecontroleerd voor een verbinding met TCP-poort 80 voor back-end-interval van vijftien seconden. Het markeert u de back-end-bron niet beschikbaar na twee opeenvolgende fouten.

        $probeV4V6 = azure network lb probe create --resource-group $rgname --name $probeV4V6Name --protocol tcp --port 80 --interval 15 --count 2 --lb-name $lbName

3. Inkomende NAT regels maken die RDP naar de back-end-bronnen verbindingen

        $inboundNatRuleRdp1 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule1V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3389 --backend-port 3389 --lb-name $lbName
        $inboundNatRuleRdp2 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule2V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3391 --backend-port 3389 --lb-name $lbName

4. Een load balancer regels maken waarmee verkeer naar andere poorten afhankelijk van de front-end ontvangen verzoek verzenden

        $lbruleIPv4 = azure network lb rule create --resource-group $rgname --name $lbRule1V4Name --frontend-ip-name $frontendV4Name --backend-address-pool-name $backendAddressPoolV4Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 80 --lb-name $lbName
        $lbruleIPv6 = azure network lb rule create --resource-group $rgname --name $lbRule1V6Name --frontend-ip-name $frontendV6Name --backend-address-pool-name $backendAddressPoolV6Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 8080 --lb-name $lbName

5. Controleer uw instellingen

        azure network lb show --resource-group $rgName --name $lbName

    Verwachte output:

        info:    Executing command network lb show
        info:    Looking up the load balancer "myIPv4IPv6Lb"
        data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/pscontosorg1southctrlus09152016/providers/Microsoft.Network/loadBalancers/myIPv4IPv6Lb
        data:    Name                            : myIPv4IPv6Lb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : southcentralus
        data:    Provisioning state              : Succeeded
        data:
        data:    Frontend IP configurations:
        data:    Name             Provisioning state  Private IP allocation  Private IP   Subnet  Public IP
        data:    ---------------  ------------------  ---------------------  -----------  ------  ---------
        data:    FrontendVipIPv4  Succeeded           Dynamic                                     myIPv4Vip
        data:    FrontendVipIPv6  Succeeded           Dynamic                                     myIPv6Vip
        data:
        data:    Probes:
        data:    Name                 Provisioning state  Protocol  Port  Path  Interval  Count
        data:    -------------------  ------------------  --------  ----  ----  --------  -----
        data:    ProbeForIPv4AndIPv6  Succeeded           Tcp       80          15        2
        data:
        data:    Backend Address Pools:
        data:    Name             Provisioning state
        data:    ---------------  ------------------
        data:    BackendPoolIPv4  Succeeded
        data:    BackendPoolIPv6  Succeeded
        data:
        data:    Load Balancing Rules:
        data:    Name                  Provisioning state  Load distribution  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    --------------------  ------------------  -----------------  --------  -------------  ------------  ------------------  -----------------------
        data:    LBRuleForIPv4-Port80  Succeeded           Default            Tcp       80             80            false               4
        data:    LBRuleForIPv6-Port80  Succeeded           Default            Tcp       80             8080          false               4
        data:
        data:    Inbound NAT Rules:
        data:    Name                 Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    -------------------  ------------------  --------  -------------  ------------  ------------------  -----------------------
        data:    NatRule-For-Rdp-VM1  Succeeded           Tcp       3389           3389          false               4
        data:    NatRule-For-Rdp-VM2  Succeeded           Tcp       3391           3389          false               4
        info:    network lb show


## <a name="create-nics"></a>NIC's maken

NIC's maken en koppelen aan de NAT-regels, regels voor load balancer en sondes.

1. De PowerShell-variabelen instellen

        $nic1Name = "myIPv4IPv6Nic1"
        $nic2Name = "myIPv4IPv6Nic2"
        $subnet1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet1Name"
        $subnet2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet2Name"
        $backendAddressPoolV4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV4Name"
        $backendAddressPoolV6Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV6Name"
        $natRule1V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule1V4Name"
        $natRule2V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule2V4Name"

2. Een NIC voor elke back-end maken en toevoegen van een IPv6-configuratie.

        $nic1 = azure network nic create --name $nic1Name --resource-group $rgname --location $location --private-ip-version "IPv4" --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
        $nic1IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic1Name

        $nic2 = azure network nic create --name $nic2Name --resource-group $rgname --location $location --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
        $nic2IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic2Name

## <a name="create-the-back-end-vm-resources-and-attach-each-nic"></a>De back-end VM resources maken en koppelen van elke NIC

VMs maken, hebt u een account voor opslag. Het VMs moeten lid zijn van een set beschikbaarheid voor load balancing. Zie voor meer informatie over het maken van VMs [maken een VM Azure PowerShell gebruiken](../virtual-machines/virtual-machines-windows-ps-create.md)

1. De PowerShell-variabelen instellen

        $storageAccountName = "ps08092016v6sa0"
        $availabilitySetName = "myIPv4IPv6AvailabilitySet"
        $vm1Name = "myIPv4IPv6VM1"
        $vm2Name = "myIPv4IPv6VM2"
        $nic1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic1Name"
        $nic2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic2Name"
        $disk1Name = "WindowsVMosDisk1"
        $disk2Name = "WindowsVMosDisk2"
        $osDisk1Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk1Name.vhd"
        $osDisk2Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk2Name.vhd"
        $imageurn "MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest"
        $vmUserName = "vmUser"
        $mySecurePassword = "PlainTextPassword*1"

    >[AZURE.WARNING] In dit voorbeeld wordt de gebruikersnaam en het wachtwoord voor de VMs in gewone tekst. Juiste Wees voorzichtig bij het gebruik van referenties in de open. Zie de cmdlet [Get-referenties](https://technet.microsoft.com/library/hh849815.aspx) voor een veiliger methode van het verwerken van referenties in PowerShell.

2. De opslag en de beschikbaarheid-set maken

    U kunt een bestaande account voor de opslag bij het maken van het VMs. De volgende opdracht maakt u een nieuwe account voor opslag.

        $storageAcc = azure storage account create $storageAccountName --resource-group $rgName --location $location --sku-name "LRS" --kind "Storage"

    Vervolgens maakt u de beschikbaarheid van set.

        $availabilitySet = azure availset create --name $availabilitySetName --resource-group $rgName --location $location

3. De virtuele machines maken met de bijbehorende NIC 's

        $vm1 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm1Name --nic-id $nic1Id --os-disk-vhd $osDisk1Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension

        $vm2 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm2Name --nic-id $nic2Id --os-disk-vhd $osDisk2Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn  --storage-account-name $storageAccountName --disable-bginfo-extension

## <a name="next-steps"></a>Volgende stappen

[Een interne taakverdeling configureren aan de slag](load-balancer-get-started-ilb-arm-cli.md)

[Een load balancer distributie-modus configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP-time-outinstellingen voor de taakverdeling configureren](load-balancer-tcp-idle-timeout.md)
