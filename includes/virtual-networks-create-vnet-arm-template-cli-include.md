## <a name="deploy-the-arm-template-by-using-the-azure-cli"></a>De sjabloon ARM implementeren met behulp van de CLI Azure

Volg de onderstaande stappen voor de implementatie van de ARM-sjabloon die u hebt gedownload met behulp van de CLI Azure.

1. Als u nooit CLI Azure gebruikt nog, Zie [installeren en configureren van de CLI Azure](../articles/xplat-cli-install.md) en volg de instructies tot aan het punt waar u uw Azure-account en abonnement.
2. Voer het **`azure config mode`** opdracht overschakelen naar de modus Resource Manager, zoals hieronder wordt weergegeven.

        azure config mode arm

    Dit is de verwachte output van de bovenstaande opdracht:

        info:    New mode is arm

3. Voer indien nodig de **`azure group create`** een nieuwe resourcegroep maken zoals hieronder wordt weergegeven. U ziet de uitvoer van de opdracht. De lijst die wordt weergegeven nadat u de parameters die worden gebruikt door de uitvoer wordt uitgelegd. Ga naar [Azure Resource Manager-overzicht](../articles/resource-group-overview.md)voor meer informatie over resourcegroepen.

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

4. Voer het **`azure group deployment create`** cmdlet voor de implementatie van de nieuwe VNet met behulp van de sjabloon en de parameter-bestanden u hebt gedownload en hierboven is gewijzigd. De lijst die wordt weergegeven nadat u de parameters die worden gebruikt door de uitvoer wordt uitgelegd.

        azure group deployment create -g TestRG -n TestVNetDeployment -f C:\ARM\azuredeploy.json -e C:\ARM\azuredeploy-parameters.json

    Dit is de verwachte output van de bovenstaande opdracht:

        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "TestVNetDeployment"
        + Registering providers
        info:    Registering provider microsoft.network
        + Waiting for deployment to complete
        data:    DeploymentName     : TestVNetDeployment
        data:    ResourceGroupName  : TestRG
        data:    ProvisioningState  : Succeeded
        data:    Timestamp          : 2015-08-14T21:56:11.152759Z
        data:    Mode               : Incremental
        data:    Name           Type    Value
        data:    -------------  ------  --------------
        data:    location       String  Central US
        data:    vnetName       String  TestVNet
        data:    addressPrefix  String  192.168.0.0/16
        data:    subnet1Prefix  String  192.168.1.0/24
        data:    subnet1Name    String  FrontEnd
        data:    subnet2Prefix  String  192.168.2.0/24
        data:    subnet2Name    String  BackEnd
        info:    group deployment create command OK

    - **-g (of--resource-groep)**. De naam van de resourcegroep de nieuwe VNet wordt gemaakt.
    - **-f (of -sjabloon-bestand)**. Het pad naar het sjabloonbestand ARM.
    - **-e (of--parameters-bestand)**. Het pad naar het bestand met uw ARM.

5. Voer het **`azure network vnet show`** opdracht voor het weergeven van de eigenschappen van de nieuwe vnet, zoals hieronder wordt weergegeven.

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
