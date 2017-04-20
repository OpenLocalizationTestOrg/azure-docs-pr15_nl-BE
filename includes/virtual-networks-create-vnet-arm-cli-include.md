## <a name="how-to-create-a-vnet-using-the-azure-cli"></a>Het maken van een VNet met behulp van de CLI Azure

U kunt de CLI Azure Azure resources beheren vanaf de opdrachtprompt vanaf elke computer met Windows, Linux of OSX. Een VNet maken met behulp van de CLI Azure, volg de onderstaande stappen.

1. Als u nog nooit hebt gebruikt de CLI Azure, Zie [installeren en configureren van de CLI Azure](../articles/xplat-cli-install.md) en volg de instructies tot aan het punt waar u uw Azure-account en abonnement.
2. Voer de opdracht **azure config-modus** wilt overschakelen naar de modus Resource Manager, zoals hieronder wordt weergegeven.

        azure config mode arm

    Dit is de verwachte output van de bovenstaande opdracht:

        info:    New mode is arm

3. Voer indien nodig de **azure groep maken** om een nieuwe resourcegroep maken zoals hieronder wordt weergegeven. U ziet de uitvoer van de opdracht. De lijst die wordt weergegeven nadat u de parameters die worden gebruikt door de uitvoer wordt uitgelegd. Ga naar [Azure Resource Manager-overzicht](../articles/virtual-network/resource-group-overview.md#resource-groups)voor meer informatie over resourcegroepen.

        azure group create -n TestRG -l centralus

    Dit is de verwachte output van de bovenstaande opdracht:

        info:    Executing command group create
        + Getting resource group TestRG
        + Creating resource group TestRG
        info:    Created resource group TestRG
        data:    Id:                  /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            centralus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

    - **-n (of -naam)**. De naam voor de nieuwe resourcegroep. Voor ons scenario *TestRG*.
    - **-l (of--locatie)**. Azure regio waar de nieuwe resourcegroep wordt gemaakt. Voor ons scenario *centralus*.

4. Voer de opdracht **vnet azure netwerk maken** voor het maken van een VNet en een subnet, zoals hieronder wordt weergegeven. 

        azure network vnet create -g TestRG -n TestVNet -a 192.168.0.0/16 -l centralus

    Dit is de verwachte output van de bovenstaande opdracht:

        info:    Executing command network vnet create
        + Looking up virtual network "TestVNet"
        + Creating virtual network "TestVNet"
        + Loading virtual network state
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet2
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        info:    network vnet create command OK

    - **-g (of--resource-groep)**. De naam van de bronnengroep waarin de VNet wordt gemaakt. Voor ons scenario *TestRG*.
    - **-n (of -naam)**. De naam van de VNet moet worden gemaakt. Voor ons scenario *TestVNet*
    - **-een (of--- adresprefixen)**. Lijst met CIDR-blokken voor de VNet-adresruimte gebruikt. Voor ons scenario *192.168.0.0/16*
    - **-l (of--locatie)**. Azure regio waar de VNet worden gemaakt. Voor ons scenario *centralus*.

5. Voer de opdracht **azure netwerk vnet subnet maken** een subnet maken zoals hieronder wordt weergegeven. U ziet de uitvoer van de opdracht. De lijst die wordt weergegeven nadat u de parameters die worden gebruikt door de uitvoer wordt uitgelegd.

        azure network vnet subnet create -g TestRG -e TestVNet -n FrontEnd -a 192.168.1.0/24

    Dit is de verwachte output van de bovenstaande opdracht:

        info:    Executing command network vnet subnet create
        + Looking up the subnet "FrontEnd"
        + Creating subnet "FrontEnd"
        + Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:
        info:    network vnet subnet create command OK

    - **-e (of--vnet-naam**. De naam van de VNet waarin het subnet wordt gemaakt. Voor ons scenario *TestVNet*.
    - **-n (of -naam)**. De naam van het nieuwe subnet. Voor ons scenario *FrontEnd*.
    - **-een (of--- adresprefix)**. Subnet CIDR-blok. Vier ons scenario, *192.168.1.0/24*.

6. Herhaal stap 5 hierboven voor het maken van andere subnetten, indien nodig. Voor ons scenario voert u de onderstaande opdracht de *back-end* -subnet maken.

        azure network vnet subnet create -g TestRG -e TestVNet -n BackEnd -a 192.168.2.0/24

4. Voer de opdracht **azure netwerk vnet weergeven** om de eigenschappen van het nieuwe vnet weer te geven zoals hieronder wordt weergegeven.

        azure network vnet show -g TestRG -n TestVNet

    Dit is de verwachte output van de bovenstaande opdracht:

        info:    Executing command network vnet show
        + Looking up virtual network "TestVNet"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        data:    Subnets:
        data:      Name                          : FrontEnd
        data:      Address prefix                : 192.168.1.0/24
        data:
        data:      Name                          : BackEnd
        data:      Address prefix                : 192.168.2.0/24
        data:
        info:    network vnet show command OK
