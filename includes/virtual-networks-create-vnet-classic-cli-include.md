## <a name="how-to-create-a-classic-vnet-using-azure-cli"></a>Het maken van een klassieke VNet met Azure CLI

U kunt de CLI Azure Azure resources beheren vanaf de opdrachtprompt vanaf elke computer met Windows, Linux of OSX. Een VNet maken met behulp van de CLI Azure, volg de onderstaande stappen.

1. Als u nooit CLI Azure gebruikt nog, Zie [installeren en configureren van de CLI Azure](../articles/xplat-cli-install.md) en volg de instructies tot aan het punt waar u uw Azure-account en abonnement.
2. Voer de opdracht **vnet azure netwerk maken** voor het maken van een VNet en een subnet, zoals hieronder wordt weergegeven. De lijst die wordt weergegeven nadat u de parameters die worden gebruikt door de uitvoer wordt uitgelegd.

            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
    
    Verwachte output:

            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK

    - **--vnet**. De naam van de VNet moet worden gemaakt. Voor ons scenario *TestVNet*
    - **-e (of--adresruimte)**. VNet-adresruimte. Voor ons scenario *192.168.0.0*
    - **-i (of cidr-)**. Het netwerkmasker in CIDR-indeling. Voor ons scenario *16*.
    - **- n (of--subnet naam**). De naam van het eerste subnet. Voor ons scenario *FrontEnd*.
    - **-p (of--begin-ip-subnet)**. Eerste IP-adres voor het subnet of subnet-adresruimte. Voor ons scenario *192.168.1.0*.
    - **-r (of--subnet cidr)**. Het netwerkmasker in CIDR-indeling voor het subnet. Voor ons scenario *24*.
    - **-l (of--locatie)**. Azure regio waar de VNet worden gemaakt. Voor ons scenario *Central US*.

3. Voer de opdracht **azure netwerk vnet subnet maken** een subnet maken zoals hieronder wordt weergegeven. De lijst die wordt weergegeven nadat u de parameters die worden gebruikt door de uitvoer wordt uitgelegd.

            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
    
    Dit is de verwachte output van de bovenstaande opdracht:

            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK

    - **-t (of--vnet-naam**. De naam van de VNet waarin het subnet wordt gemaakt. Voor ons scenario *TestVNet*.
    - **-n (of -naam)**. De naam van het nieuwe subnet. Voor ons scenario *BackEnd*.
    - **-een (of--- adresprefix)**. Subnet CIDR-blok. Vier ons scenario, *192.168.2.0/24*.

4. Voer de opdracht **azure netwerk vnet weergeven** om de eigenschappen van het nieuwe vnet weer te geven zoals hieronder wordt weergegeven.

            azure network vnet show

    Dit is de verwachte output van de bovenstaande opdracht:

            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK
