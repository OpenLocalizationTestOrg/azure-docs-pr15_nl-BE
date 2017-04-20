<properties
    pageTitle="Azure bronnen met C# implementeren | Microsoft Azure"
    description="Leer hoe u met C# en Azure Resource Manager Microsoft Azure resources maken."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="davidmu"/>

# <a name="deploy-azure-resources-using-c"></a>Azure Resources met behulp van C implementeren# 

In dit artikel wordt beschreven hoe u voor het maken van Azure bronnen met C#.

Moet u eerst controleren of dat u deze taken hebt voltooid:

- Installeer [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- Controleer of de installatie van [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) of [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855)
- Een [verificatietoken](../resource-group-authenticate-service-principal.md) ophalen

Het duurt ongeveer 30 minuten deze stappen doen.

## <a name="step-1-create-a-visual-studio-project-and-install-the-libraries"></a>Stap 1: Maak een Visual Studio-project en de bibliotheken installeren

NuGet packages zijn de eenvoudigste manier om het installeren van de bibliotheken die u nodig hebt voor deze zelfstudie hebt voltooid. Als u de bibliotheken die u nodig hebt in Visual Studio, voer deze stappen uit:

1. Klik op **bestand** > **nieuwe** > **Project**.

2. In **sjablonen** > **Visual C#**, **Consoletoepassing**selecteert, voert u de naam en locatie van het project en klik vervolgens op **OK**.

3. Klik met de rechtermuisknop op de naam van het project in de Solution Explorer en klik op **Beheren NuGet pakketten**.

4. Op *Active Directory* in het zoekvak op **installeren** voor het Active Directory-verificatie Library-pakket en volg de instructies om het pakket te installeren.

5. Selecteer boven aan de pagina **Zijn voorlopige versie**. Type *Microsoft.Azure.Management.Compute* in het zoekvak, klikt u op **installeren** voor het berekenen van .NET bibliotheken en volgt u de instructies voor het installeren van het pakket.

6. Type *Microsoft.Azure.Management.Network* in het zoekvak, klikt u op **installeren** voor .NET netwerkbibliotheken en volg de instructies om het pakket te installeren.

7. Type *Microsoft.Azure.Management.Storage* in het zoekvak, klikt u op **installeren** voor de opslag .NET bibliotheken en volgt u de instructies voor het installeren van het pakket.

8. Typ *Microsoft.Azure.Management.ResourceManager* in het zoekvak, klik op **installeren** voor de Resource Management-bibliotheken.

Nu bent u klaar om uw toepassing te maken met behulp van de bibliotheken.

## <a name="step-2-create-the-credentials-that-are-used-to-authenticate-requests"></a>Stap 2: Maak de referenties die worden gebruikt voor verificatie-aanvragen

Nu kunt u informatie over de toepassingen die u eerder hebt gemaakt in de referenties die worden gebruikt voor het verifiëren van aanvragen op Azure Resource Manager opmaken.

1. Open het bestand Program.cs voor het project dat u hebt gemaakt en voegt u deze instructies boven aan het bestand:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.Azure.Management.ResourceManager.Models;
        using Microsoft.Azure.Management.Storage;
        using Microsoft.Azure.Management.Storage.Models;
        using Microsoft.Azure.Management.Network;
        using Microsoft.Azure.Management.Network.Models;
        using Microsoft.Azure.Management.Compute;
        using Microsoft.Azure.Management.Compute.Models;
        using Microsoft.Rest;

2. Toevoegen wilt maken van het token dat nodig is, deze methode aan de klasse van het programma:

        private static async Task<AuthenticationResult> GetAccessTokenAsync()
        {
          var cc = new ClientCredential("{client-id}", "{client-secret}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
          if (token == null)
          {
            throw new InvalidOperationException("Could not get the token");
          }
          return token;
        }

    {Client-id} vervangen door de aanduiding van de Azure Active Directory-toepassing {-geheim client} met de toegangssleutel van de toepassing AD en {huurder-id} met de huurder voor uw abonnement-id. De huurder-id kunt u vinden door het uitvoeren van Get-AzureRmSubscription. Met behulp van de Azure portal vindt u de toegangstoets.

3. Aanroepen van de methode die u eerder hebt toegevoegd, moet u deze code toevoegen aan de Main-methode in het bestand Program.cs:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

4. Sla het bestand Program.cs.

## <a name="step-3-register-the-resource-providers-and-create-the-resources"></a>Stap 3: De resource-providers registreren en de resources maken

### <a name="register-the-providers-and-create-a-resource-group"></a>De providers registreren en een resourcegroep maken

Alle resources moeten worden opgenomen in een resourcegroep. Voordat u bronnen aan een groep toevoegen kunt, moet uw abonnement zijn geregistreerd bij de leveranciers van de resource.

1. Variabelen toevoegen aan de Main-methode van de klasse wordt de namen die u wilt gebruiken voor resources opgeven:

        var groupName = "resource group name";
        var subscriptionId = "subsciption id";
        var location = "location name";
        var storageName = "storage account name";
        var ipName = "public ip name";
        var subnetName = "subnet name";
        var vnetName = "virtual network name";
        var nicName = "network interface name";
        var avSetName = "availability set name";
        var vmName = "virtual machine name";  
        var adminName = "administrator account name";
        var adminPassword = "administrator account password";
        
    De variabele waarden vervangen door de namen en id die u wilt gebruiken. De abonnement-id kunt u vinden door het uitvoeren van Get-AzureRmSubscription.

2. De resourcegroep maken en registreren van de providers, deze methode toevoegen aan de klasse van het programma:

        public static async Task<ResourceGroup> CreateResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location)
        {
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
            
          Console.WriteLine("Registering the providers...");
          var rpResult = resourceManagementClient.Providers.Register("Microsoft.Storage");
          Console.WriteLine(rpResult.RegistrationState);
          rpResult = resourceManagementClient.Providers.Register("Microsoft.Network");
          Console.WriteLine(rpResult.RegistrationState);
          rpResult = resourceManagementClient.Providers.Register("Microsoft.Compute");
          Console.WriteLine(rpResult.RegistrationState);
          
          Console.WriteLine("Creating the resource group...");
          var resourceGroup = new ResourceGroup { Location = location };
          return await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync(groupName, resourceGroup);
        }

3. Aanroepen van de methode die u eerder hebt toegevoegd, moet u deze code toevoegen aan de Main-methode:

        var rgResult = CreateResourceGroupAsync(
          credential,
          groupName,
          subscriptionId,
          location);
        Console.WriteLine(rgResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

### <a name="create-a-storage-account"></a>Maak een account voor opslag

Een [account voor opslag](../storage/storage-create-storage-account.md) is nodig voor het opslaan van de virtuele harde schijf-bestand dat is gemaakt voor de virtuele machine.

1. Toevoegen om de opslag-account maken, deze methode aan de klasse van het programma:

        public static async Task<StorageAccount> CreateStorageAccountAsync(
          TokenCredentials credential,       
          string groupName,
          string subscriptionId,
          string location,
          string storageName)
        {
          Console.WriteLine("Creating the storage account...");
          var storageManagementClient = new StorageManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await storageManagementClient.StorageAccounts.CreateAsync(
            groupName,
            storageName,
            new StorageAccountCreateParameters()
            {
              Sku = new Microsoft.Azure.Management.Storage.Models.Sku() 
                { Name = SkuName.StandardLRS},
              Kind = Kind.Storage,
              Location = location
            }
          );
        }

2. Aanroepen van de methode die u eerder hebt toegevoegd, moet u deze code toevoegen aan de Main-methode van de klasse van het programma:

        var stResult = CreateStorageAccountAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          storageName);
        Console.WriteLine(stResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-the-public-ip-address"></a>Het openbare IP-adres maken

Een openbaar IP-adres is nodig om te communiceren met de virtuele machine.

1. Toevoegen om het openbare IP-adres van de virtuele machine maken, deze methode aan de klasse van het programma:

        public static async Task<PublicIPAddress> CreatePublicIPAddressAsync(
          TokenCredentials credential,  
          string groupName,
          string subscriptionId,
          string location,
          string ipName)
        {
          Console.WriteLine("Creating the public ip...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await networkManagementClient.PublicIPAddresses.CreateOrUpdateAsync(
            groupName,
            ipName,
            new PublicIPAddress
            {
              Location = location,
              PublicIPAllocationMethod = "Dynamic"
            }
          );
        }

2. Aanroepen van de methode die u eerder hebt toegevoegd, moet u deze code toevoegen aan de Main-methode van de klasse van het programma:

        var ipResult = CreatePublicIPAddressAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          ipName);
        Console.WriteLine(ipResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-the-virtual-network"></a>Het virtuele netwerk maken

Een virtuele machine die wordt gemaakt met het implementatiemodel Resource Manager moet in een virtueel netwerk.

1. Een subnet en een virtueel netwerk maken, toevoegen met deze methode naar de klasse van het programma:

        public static async Task<VirtualNetwork> CreateVirtualNetworkAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string vnetName,
          string subnetName)
        {
          Console.WriteLine("Creating the virtual network...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          
          var subnet = new Subnet
          {
            Name = subnetName,
            AddressPrefix = "10.0.0.0/24"
          };
          
          var address = new AddressSpace {
            AddressPrefixes = new List<string> { "10.0.0.0/16" }
          };
          
          return await networkManagementClient.VirtualNetworks.CreateOrUpdateAsync(
            groupName,
            vnetName,
            new VirtualNetwork
            {
              Location = location,
              AddressSpace = address,
              Subnets = new List<Subnet> { subnet }
            }
          );
        }
        
2. Aanroepen van de methode die u eerder hebt toegevoegd, moet u deze code toevoegen aan de Main-methode van de klasse van het programma:

        var vnResult = CreateVirtualNetworkAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          vnetName,
          subnetName);
        Console.WriteLine(vnResult.Result.ProvisioningState);  
        Console.ReadLine();
        
### <a name="create-the-network-interface"></a>De netwerkinterface maken

Een virtuele machine moet een netwerkinterface op het virtuele netwerk communiceren.

1. Toevoegen wilt maken van een netwerk-interface, deze methode aan de klasse van het programma:

        public static async Task<NetworkInterface> CreateNetworkInterfaceAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string subnetName,
          string vnetName,
          string ipName,
          string nicName)
        {
          Console.WriteLine("Creating the network interface...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var subnetResponse = await networkManagementClient.Subnets.GetAsync(
            groupName,
            vnetName,
            subnetName
          );
          var pubipResponse = await networkManagementClient.PublicIPAddresses.GetAsync(groupName, ipName);

          return await networkManagementClient.NetworkInterfaces.CreateOrUpdateAsync(
            groupName,
            nicName,
            new NetworkInterface
            {
              Location = location,
              IpConfigurations = new List<NetworkInterfaceIPConfiguration>
              {
                new NetworkInterfaceIPConfiguration
                {
                  Name = nicName,
                  PublicIPAddress = pubipResponse,
                  Subnet = subnetResponse
                }
              }
            }
          );
        }

2. Aanroepen van de methode die u eerder hebt toegevoegd, moet u deze code toevoegen aan de Main-methode van de klasse van het programma:

        var ncResult = CreateNetworkInterfaceAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          subnetName,
          vnetName,
          ipName,
          nicName);
        Console.WriteLine(ncResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-an-availability-set"></a>Maak een set beschikbaarheid

Beschikbaarheid maken het gemakkelijker voor het beheer van het onderhoud van de virtuele machines die worden gebruikt door de toepassing.

1. De beschikbaarheid als set wilt maken, toevoegen met deze methode naar de klasse van het programma:

        public static async Task<AvailabilitySet> CreateAvailabilitySetAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string avsetName)
        {
          Console.WriteLine("Creating the availability set...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await computeManagementClient.AvailabilitySets.CreateOrUpdateAsync(
            groupName,
            avsetName,
            new AvailabilitySet()
            {
              Location = location
            }
          );
        }

2. Aanroepen van de methode die u eerder hebt toegevoegd, moet u deze code toevoegen aan de Main-methode van de klasse van het programma:

        var avResult = CreateAvailabilitySetAsync(
          credential,  
          groupName,
          subscriptionId,
          location,
          avSetName);
        Console.ReadLine();

### <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Nu u de ondersteunende middelen hebt gemaakt, kunt u een virtuele machine.

1. Toevoegen om de virtuele machine maken, deze methode aan de klasse van het programma:

        public static async Task<VirtualMachine> CreateVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName,
          string subscriptionId,
          string location,
          string nicName,
          string avsetName,
          string storageName,
          string adminName,
          string adminPassword,
          string vmName)
        {
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var nic = networkManagementClient.NetworkInterfaces.Get(groupName, nicName);

          var computeManagementClient = new ComputeManagementClient(credential);
          computeManagementClient.SubscriptionId = subscriptionId;
          var avSet = computeManagementClient.AvailabilitySets.Get(groupName, avsetName);

          Console.WriteLine("Creating the virtual machine...");
          return await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(
            groupName,
            vmName,
            new VirtualMachine
            {
              Location = location,
              AvailabilitySet = new Microsoft.Azure.Management.Compute.Models.SubResource
              {
                Id = avSet.Id
              },
              HardwareProfile = new HardwareProfile
              {
                VmSize = "Standard_A0"
              },
              OsProfile = new OSProfile
              {
                AdminUsername = adminName,
                AdminPassword = adminPassword,
                ComputerName = vmName,
                WindowsConfiguration = new WindowsConfiguration
                {
                  ProvisionVMAgent = true
                }
              },
              NetworkProfile = new NetworkProfile
              {
                NetworkInterfaces = new List<NetworkInterfaceReference>
                {
                  new NetworkInterfaceReference { Id = nic.Id }
                }
              },
              StorageProfile = new StorageProfile
              {
                ImageReference = new ImageReference
                {
                  Publisher = "MicrosoftWindowsServer",
                  Offer = "WindowsServer",
                  Sku = "2012-R2-Datacenter",
                  Version = "latest"
                },
                OsDisk = new OSDisk
                {
                  Name = "mytestod1",
                  CreateOption = DiskCreateOptionTypes.FromImage,
                  Vhd = new VirtualHardDisk
                  {
                    Uri = "http://" + storageName + ".blob.core.windows.net/vhds/mytestod1.vhd"
                  }
                }
              }
            }
          );
        }

    >[AZURE.NOTE] Deze handleiding maakt u een virtuele machine met een versie van het besturingssysteem Windows Server. Zie voor meer informatie over het selecteren van andere afbeeldingen kunt [navigeren en afbeeldingen selecteren Azure virtuele machine met Windows PowerShell en de CLI Azure](virtual-machines-linux-cli-ps-findimage.md).

2. Aanroepen van de methode die u eerder hebt toegevoegd, moet u deze code toevoegen aan de Main-methode:

        var vmResult = CreateVirtualMachineAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          nicName,
          avSetName,
          storageName,
          adminName,
          adminPassword,
          vmName);
        Console.WriteLine(vmResult.Result.ProvisioningState);
        Console.ReadLine();

##<a name="step-4-delete-the-resources"></a>Stap 4: De bronnen verwijderen

Omdat u in rekening worden gebracht voor de bronnen die worden gebruikt in Azure, is het altijd een goede gewoonte om de bronnen die u niet meer nodig hebt verwijderen. Als u wilt verwijderen van de virtuele machines en alle ondersteunende middelen, u hoeft te doen is de resourcegroep verwijderen.

1.  Toevoegen als u wilt verwijderen in de resourcegroep, deze methode aan de klasse van het programma:

        public static async void DeleteResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId)
        {
          Console.WriteLine("Deleting resource group...");
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await resourceManagementClient.ResourceGroups.DeleteAsync(groupName);
        }

2.  Aanroepen van de methode die u eerder hebt toegevoegd, moet u deze code toevoegen aan de Main-methode:

        DeleteResourceGroupAsync(
          credential,
          groupName,
          subscriptionId);
        Console.ReadLine();

## <a name="step-5-run-the-console-application"></a>Stap 5: De consoletoepassing uitvoeren

1. De als consoletoepassing wilt uitvoeren, klikt u op **starten** in Visual Studio en meldt u zich aan Azure AD met dezelfde gebruikersnaam en wachtwoord die u voor uw abonnement gebruikt.

2. Druk op **Enter** na elke statuscode voor het maken van elke resource. Nadat u de virtuele machine hebt gemaakt, doen de volgende stap voordat u op Enter om alle bronnen te verwijderen.

    Het moet ongeveer vijf minuten voor deze consoletoepassing volledig van begin tot eind duren. Voordat u op Enter om te beginnen met het verwijderen van bronnen, kan u krijgen een paar minuten om te controleren of het maken van de bronnen in de portal Azure voordat u ze verwijdert.

3. Ga naar de controlelogboeken in Azure portal overzicht van de status van de bronnen:

    ![Controlelogboeken in Azure portal bladeren](./media/virtual-machines-windows-csharp/crpportal.png)
    
## <a name="next-steps"></a>Volgende stappen

- Profiteer van het gebruik van een sjabloon voor het maken van een virtuele machine met behulp van de gegevens in [een Azure Virtual Machine implementeren met behulp van C# en een sjabloon Resource Manager](virtual-machines-windows-csharp-template.md).
- Informatie over het beheren van de virtuele machine die u hebt gemaakt aan de hand van [beheren virtuele machines met behulp van bronbeheer Azure en PowerShell](virtual-machines-windows-csharp-manage.md).
