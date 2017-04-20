<properties 
   pageTitle="Het instellen van een statisch particuliere IP-adres in de ARM-modus met behulp van de CLI | Microsoft Azure"
   description="Informatie over statische IP-adressen (Spanningsdips) en hoe u ze kunt beheren in de ARM-modus met behulp van de CLI"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-in-azure-cli"></a>Het instellen van een statisch IP-adres privé in Azure CLI

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dit artikel heeft betrekking op het implementatiemodel Resource Manager. U kunt ook een [statische particuliere IP-adres in het klassieke implementatiemodel beheren](virtual-networks-static-private-ip-classic-cli.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Het onderstaande voorbeeld Azure CLI opdrachten verwachten een eenvoudige omgeving al gemaakt. Als u wilt dat de opdrachten uitvoeren zoals deze worden weergegeven in dit document, eerst de testomgeving wordt beschreven in het [maken van een vnet](virtual-networks-create-vnet-arm-cli.md)te bouwen.

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Een statische particulier IP-adres opgeven bij het maken van een VM
Als u wilt maken in een VM met de naam *DNS01* in de *FrontEnd* -subnet van een VNet met de naam *TestVNet* met een statische particuliere IP-adres van *192.168.1.101*, volg de onderstaande stappen:

1. Als u nooit CLI Azure gebruikt nog, Zie [installeren en configureren van de CLI Azure](../xplat-cli-install.md) en volg de instructies tot aan het punt waar u uw Azure-account en abonnement.

2. Voer de opdracht **azure config-modus** wilt overschakelen naar de modus Resource Manager, zoals hieronder wordt weergegeven.

        azure config mode arm

    Verwachte output:

        info:    New mode is arm

3. Het **maken van openbare azure netwerk - IP-** maken van een openbaar IP-adres voor de VM worden uitgevoerd. De lijst die wordt weergegeven nadat u de parameters die worden gebruikt door de uitvoer wordt uitgelegd.

        azure network public-ip create -g TestRG -n TestPIP -l centralus
    
    Verwachte output:

        info:    Executing command network public-ip create
        + Looking up the public ip "TestPIP"
        + Creating public ip address "TestPIP"
        + Looking up the public ip "TestPIP"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP
        data:    Name                            : TestPIP
        data:    Type                            : Microsoft.Network/publicIPAddresses
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Allocation method               : Dynamic
        data:    Idle timeout                    : 4
        info:    network public-ip create command OK

    - **-g (of--resource-groep)**. De naam van de resourcegroep die in de openbare IP wordt gemaakt.
    - **-n (of -naam)**. De naam van de openbare IP.
    - **-l (of--locatie)**. Azure regio waar de openbare IP worden gemaakt. Voor ons scenario *centralus*.

3. Voer de opdracht **nic azure netwerk maken** voor het maken van een NIC met een statische particuliere IP-adres. De lijst die wordt weergegeven nadat u de parameters die worden gebruikt door de uitvoer wordt uitgelegd.

        azure network nic create -g TestRG -n TestNIC -l centralus -a 192.168.1.101 -m TestVNet -k FrontEnd

    Verwachte output:

        + Looking up the network interface "TestNIC"
        + Looking up the subnet "FrontEnd"
        + Creating network interface "TestNIC"
        + Looking up the network interface "TestNIC"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
        data:    Name                            : TestNIC
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.101
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK

    - **-een (of--particuliere ip-adressen)**. Statische particuliere IP-adres voor de netwerkadapterkaart.
    - **-m (of--subnet-vnet-naam)**. De naam van de VNet waar de NIC wordt gemaakt.
    - **-k (of--subnet-naam)**. De naam van het subnet waarin de NIC wordt gemaakt.

4. Voer de opdracht **azure vm maken** voor het maken van het openbare IP-VM en NIC dat hierboven is gemaakt. De lijst die wordt weergegeven nadat u de parameters die worden gebruikt door de uitvoer wordt uitgelegd.

        azure vm create -g TestRG -n DNS01 -l centralus -y Windows -f TestNIC -i TestPIP -F TestVNet -j FrontEnd -o vnetstorage -q bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 -u adminuser -p AdminP@ssw0rd

    Verwachte output:

        info:    Executing command vm create
        + Looking up the VM "DNS01"
        info:    Using the VM Size "Standard_A1"
        warn:    The image "bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2" will be used for VM
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account vnetstorage
        + Looking up the NIC "TestNIC"
        info:    Found an existing NIC "TestNIC"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd" in the NIC "TestNIC"
        info:    Found public ip parameters, trying to setup PublicIP profile
        + Looking up the public ip "TestPIP"
        info:    Found an existing PublicIP "TestPIP"
        info:    Configuring identified NIC IP configuration with PublicIP "TestPIP"
        + Updating NIC "TestNIC"
        + Looking up the NIC "TestNIC"
        + Creating VM "DNS01"
        info:    vm create command OK

    - **-y (of--os-type)**. Type besturingssysteem voor VM, *Windows* of *Linux*.
    - **-f (of--nic-naam)**. De naam van de Netwerkkaart VM wordt gebruikt.
    - **-i (of--naam van openbare IP-)**. De naam van het openbare IP-VM wordt gebruikt.
    - **-F (of--vnet-naam)**. De naam van de VNet waar de VM worden gemaakt.
    - **-j (of--vnet-subnet-naam)**. De naam van het subnet waarop de VM wordt gemaakt.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Het statische particuliere IP-adresgegevens ophalen voor een VM

Als u de statische persoonlijke IP-adresgegevens voor de VM gemaakt met het bovenstaande script, voert u de opdracht Azure CLI en houd rekening met de waarden voor de *particuliere IP-alloc-methode* en *particuliere IP-adres*:

    azure vm show -g TestRG -n DNS01

Verwachte output:

    info:    Executing command vm show
    + Looking up the VM "DNS01"
    + Looking up the NIC "TestNIC"
    + Looking up the public ip "TestPIP
    data:    Id                              :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    ProvisioningState               :Succeeded
    data:    Name                            :DNS01
    data:    Location                        :centralus
    data:    Type                            :Microsoft.Compute/virtualMachines
    data:
    data:    Hardware Profile:
    data:      Size                          :Standard_A1
    data:
    data:    Storage Profile:
    data:      Source image:
    data:        Id                          :/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/services/images/bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
    data:
    data:      OS Disk:
    data:        OSType                      :Windows
    data:        Name                        :cli08d7bd987a0112a8-os-1441774961355
    data:        Caching                     :ReadWrite
    data:        CreateOption                :FromImage
    data:        Vhd:
    data:          Uri                       :https://vnetstorage2.blob.core.windows.net/vhds/cli08d7bd987a0112a8-os-1441774961355vhd
    data:
    data:    OS Profile:
    data:      Computer Name                 :DNS01
    data:      User Name                     :adminuser
    data:      Windows Configuration:
    data:        Provision VM Agent          :true
    data:        Enable automatic updates    :true
    data:
    data:    Network Profile:
    data:      Network Interfaces:
    data:        Network Interface #1:
    data:          Id                        :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    data:          Primary                   :true
    data:          MAC Address               :00-0D-3A-90-1A-A8
    data:          Provisioning State        :Succeeded
    data:          Name                      :TestNIC
    data:          Location                  :centralus
    data:            Private IP alloc-method :Static
    data:            Private IP address      :192.168.1.101
    data:            Public IP address       :40.122.213.159
    info:    vm show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Een statische particulier IP-adres van een VM verwijderen
U kunt een statische particulier IP-adres niet verwijderen uit een NIC in Azure CLI voor Resource Manager. U moet een nieuwe NIC die gebruikmaakt van een dynamische IP-adres maken, de vorige NIC verwijderen uit de VM en voegt u de nieuwe NIC voor VM. Als u wilt wijzigen de NIC voor de VM gebruikt int eh opdrachten hierboven, volg de onderstaande stappen.
    
1. Voer de opdracht **nic azure netwerk maken** voor het maken van een nieuwe NIC met dynamische IP-toewijzing. U ziet hoe u hoeft niet op dit moment voor het IP-adres opgeven.

        azure network nic create -g TestRG -n TestNIC2 -l centralus -m TestVNet -k FrontEnd

    Verwachte output:

        info:    Executing command network nic create
        + Looking up the network interface "TestNIC2"
        + Looking up the subnet "FrontEnd"
        + Creating network interface "TestNIC2"
        + Looking up the network interface "TestNIC2"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
        data:    Name                            : TestNIC2
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.6
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK

2. Voer de opdracht **azure vm instellen** voor het wijzigen van de Netwerkinterfacekaart die wordt gebruikt door de VM.

        azure vm set -g TestRG -n DNS01 -N TestNIC2

    Verwachte output:

        info:    Executing command vm set
        + Looking up the VM "DNS01"
        + Looking up the NIC "TestNIC2"
        + Updating VM "DNS01"
        info:    vm set command OK

3. Als u wilt, voert u de opdracht **nic azure netwerk verwijderen** verwijderen van de oude NIC.

        azure network nic delete -g TestRG -n TestNIC --quiet

    Verwachte output:

        info:    Executing command network nic delete
        + Looking up the network interface "TestNIC"
        + Deleting network interface "TestNIC"
        info:    network nic delete command OK

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Een statische particulier IP-adres toevoegen aan een bestaande VM
Als een statische particulier IP-adres toevoegen aan de Netwerkinterfacekaart die wordt gebruikt door de VM gemaakt met behulp van het bovenstaande script, moet u de volgende opdracht uitvoeren:

    azure network nic set -g TestRG -n TestNIC2 -a 192.168.1.101

Verwachte output:

    info:    Executing command network nic set
    + Looking up the network interface "TestNIC2"
    + Updating network interface "TestNIC2"
    + Looking up the network interface "TestNIC2"
    data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
    data:    Name                            : TestNIC2
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : centralus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-90-29-25
    data:    Enable IP forwarding            : false
    data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    IP configurations:
    data:      Name                          : NIC-config
    data:      Provisioning state            : Succeeded
    data:      Private IP address            : 192.168.1.101
    data:      Private IP Allocation Method  : Static
    data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [gereserveerde openbare IP-](virtual-networks-reserved-public-ip.md) adressen.
- Informatie over adressen [instantieniveau openbare IP-(ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Neem contact op met het [gereserveerde IP-REST API's](https://msdn.microsoft.com/library/azure/dn722420.aspx).
