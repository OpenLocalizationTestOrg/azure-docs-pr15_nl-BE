<properties
    pageTitle="Beheren met behulp van bronbeheer Azure en C# VMs | Microsoft Azure"
    description="Beheer van virtuele machines met behulp van bronbeheer Azure en C#."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-azure-virtual-machines-using-azure-resource-manager-and-c"></a>Azure virtuele Machines met behulp van bronbeheer Azure en C beheren#  

De taken in dit artikel hoe u voor het beheren van virtuele machines, zoals starten, stoppen en bij te werken. Een virtuele machine moet bestaan in een resourcegroep voor het voltooien van de taken in dit artikel.

Om de taken in dit artikel wilt uitvoeren, hebt u het volgende nodig:

- [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- [Een verificatietoken](../resource-group-authenticate-service-principal.md)

## <a name="create-a-visual-studio-project-and-install-packages"></a>Een Visual Studio-project maken en installeren van pakketten

NuGet packages zijn de eenvoudigste manieren om het installeren van de bibliotheken die u moet voltooien van de taken in dit artikel. De bibliotheken die u hebt geïnstalleerd voor dit artikel zijn de bibliotheek Azure Active Directory-verificatie en de bibliotheek Resource Provider berekenen. Voer de volgende stappen uit als u de bibliotheken in Visual Studio:

1. Klik op **bestand** > **nieuwe** > **Project**.

2. In **sjablonen** > **Visual C#**, **Consoletoepassing**selecteert, voert u de naam en locatie van het project en klik vervolgens op **OK**.

3. Klik met de rechtermuisknop op de naam van het project in de Solution Explorer en klik op **Beheren NuGet pakketten**.

4. Op *Active Directory* in het zoekvak op **installeren** voor het Active Directory-verificatie Library-pakket en volg de instructies om het pakket te installeren.

5. Selecteer boven aan de pagina **Zijn voorlopige versie**. Type *Microsoft.Azure.Management.Compute* in het zoekvak, klikt u op **installeren** voor het berekenen van .NET bibliotheken en volgt u de instructies voor het installeren van het pakket.

U bent nu klaar om te starten met de bibliotheken voor het beheer van uw virtuele machines.

## <a name="set-up-the-project"></a>Het project instellen

Nu dat de toepassing wordt gemaakt en de bibliotheken zijn geïnstalleerd, kunt u een token met behulp van de toepassingsgegevens maken. Deze token wordt gebruikt voor het verifiëren van aanvragen op Azure Resource Manager.

1. Open het bestand Program.cs voor het project dat u hebt gemaakt en voegt u deze instructies boven aan het bestand:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.Compute;
        using Microsoft.Azure.Management.Compute.Models;
        using Microsoft.Rest;
        
2. Variabelen toevoegen aan de Main-methode van de klasse wordt de naam van de resourcegroep en de naam van de virtuele machine en uw abonnement-id op te geven:

        var groupName = "resource group name";
        var vmName = "virtual machine name";  
        var subscriptionId = "subsciption id";

    De abonnement-id kunt u vinden door het uitvoeren van Get-AzureRmSubscription.
    
3. Het token dat nodig is voor het maken van de referenties, deze methode toevoegen aan de klasse programma ophalen:

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
    
4. Om de referenties te maken, moet u deze code toevoegen aan de Main-methode in Program.cs:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

5. Sla het bestand Program.cs.

## <a name="display-information-about-a-virtual-machine"></a>Informatie weergeven over een virtuele machine

1. Deze methode toevoegen aan de klasse wordt in het project dat u eerder hebt gemaakt:

        public static async void GetVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Getting information about the virtual machine...");

          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(
            groupName, 
            vmName, 
            InstanceViewTypes.InstanceView);

          Console.WriteLine("hardwareProfile");
          Console.WriteLine("   vmSize: " + vmResult.HardwareProfile.VmSize);

          Console.WriteLine("\nstorageProfile");
          Console.WriteLine("  imageReference");
          Console.WriteLine("    publisher: " + vmResult.StorageProfile.ImageReference.Publisher);
          Console.WriteLine("    offer: " + vmResult.StorageProfile.ImageReference.Offer);
          Console.WriteLine("    sku: " + vmResult.StorageProfile.ImageReference.Sku);
          Console.WriteLine("    version: " + vmResult.StorageProfile.ImageReference.Version);
          Console.WriteLine("  osDisk");
          Console.WriteLine("    osType: " + vmResult.StorageProfile.OsDisk.OsType);
          Console.WriteLine("    name: " + vmResult.StorageProfile.OsDisk.Name);
          Console.WriteLine("    createOption: " + vmResult.StorageProfile.OsDisk.CreateOption);
          Console.WriteLine("    uri: " + vmResult.StorageProfile.OsDisk.Vhd.Uri);
          Console.WriteLine("    caching: " + vmResult.StorageProfile.OsDisk.Caching);

          Console.WriteLine("\nosProfile");
          Console.WriteLine("  computerName: " + vmResult.OsProfile.ComputerName);
          Console.WriteLine("  adminUsername: " + vmResult.OsProfile.AdminUsername);
          Console.WriteLine("  provisionVMAgent: " + vmResult.OsProfile.WindowsConfiguration.ProvisionVMAgent.Value);
          Console.WriteLine("  enableAutomaticUpdates: " + vmResult.OsProfile.WindowsConfiguration.EnableAutomaticUpdates.Value);

          Console.WriteLine("\nnetworkProfile");
          foreach (NetworkInterfaceReference nic in vmResult.NetworkProfile.NetworkInterfaces)
          {
            Console.WriteLine("  networkInterface id: " + nic.Id);
          }

          Console.WriteLine("\nvmAgent");
          Console.WriteLine("  vmAgentVersion" + vmResult.InstanceView.VmAgent.VmAgentVersion);
          Console.WriteLine("    statuses");
          foreach (InstanceViewStatus stat in vmResult.InstanceView.VmAgent.Statuses)
          {
            Console.WriteLine("    code: " + stat.Code);
            Console.WriteLine("    level: " + stat.Level);
            Console.WriteLine("    displayStatus: " + stat.DisplayStatus);
            Console.WriteLine("    message: " + stat.Message);
            Console.WriteLine("    time: " + stat.Time);
          }

          Console.WriteLine("\ndisks");
          foreach (DiskInstanceView idisk in vmResult.InstanceView.Disks)
          {
            Console.WriteLine("  name: " + idisk.Name);
            Console.WriteLine("  statuses");
            foreach (InstanceViewStatus istat in idisk.Statuses)
            {
              Console.WriteLine("    code: " + istat.Code);
              Console.WriteLine("    level: " + istat.Level);
              Console.WriteLine("    displayStatus: " + istat.DisplayStatus);
              Console.WriteLine("    time: " + istat.Time);
            }
          }

          Console.WriteLine("\nVM general status");
          Console.WriteLine("  provisioningStatus: " + vmResult.ProvisioningState);
          Console.WriteLine("  id: " + vmResult.Id);
          Console.WriteLine("  name: " + vmResult.Name);
          Console.WriteLine("  type: " + vmResult.Type);
          Console.WriteLine("  location: " + vmResult.Location);
          Console.WriteLine("\nVM instance status");
          foreach (InstanceViewStatus istat in vmResult.InstanceView.Statuses)
          {
            Console.WriteLine("\n  code: " + istat.Code);
            Console.WriteLine("  level: " + istat.Level);
            Console.WriteLine("  displayStatus: " + istat.DisplayStatus);
          }
          
        }

2. Aanroepen van de methode die u zojuist hebt toegevoegd, moet u deze code toevoegen aan de Main-methode:

        GetVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();
    
3. Sla het bestand Program.cs.

4. Klik op **starten** in Visual Studio en meldt u zich aan Azure AD met dezelfde gebruikersnaam en wachtwoord die u voor uw abonnement gebruikt.

    Wanneer u deze methode uitvoert, worden er ongeveer uit zoals in het volgende voorbeeld:
    
        Getting information about the virtual machine...
        hardwareProfile
          vmSize: Standard_A0

        storageProfile
          imageReference
            publisher: MicrosoftWindowsServer
            offer: WindowsServer
            sku: 2012-R2-Datacenter
            version: latest
          osDisk
            osType: Windows
            name: myosdisk
            createOption: FromImage
            uri: http://store1.blob.core.windows.net/vhds/myosdisk.vhd
            caching: ReadWrite

          osProfile
            computerName: vm1
            adminUsername: account1
            provisionVMAgent: True
            enableAutomaticUpdates: True

          networkProfile
            networkInterface 
              id: /subscriptions/{subscription-id}
                /resourceGroups/rg1/providers/Microsoft.Network/networkInterfaces/nc1

          vmAgent
            vmAgentVersion2.7.1198.766
            statuses
            code: ProvisioningState/succeeded
            level: Info
            displayStatus: Ready
            message: GuestAgent is running and accepting new configurations.
            time: 4/13/2016 8:35:32 PM

          disks
            name: myosdisk
            statuses
              code: ProvisioningState/succeeded
              level: Info
              displayStatus: Provisioning succeeded
              time: 4/13/2016 8:04:36 PM

          VM general status
            provisioningStatus: Succeeded
            id: /subscriptions/{subscription-id}
              /resourceGroups/rg1/providers/Microsoft.Compute/virtualMachines/vm1
            name: vm1
            type: Microsoft.Compute/virtualMachines
            location: centralus

          VM instance status
            code: ProvisioningState/succeeded
              level: Info
              displayStatus: Provisioning succeeded
            code: PowerState/running
              level: Info
              displayStatus: VM running

## <a name="stop-a-virtual-machine"></a>Een virtuele machine stoppen

U kunt een virtuele machine op twee manieren beëindigen. Stoppen met een virtuele machine en alle bijbehorende instellingen behouden maar blijven worden belast, of kunt u een virtuele machine te stoppen en het opheffen. Wanneer u een virtuele machine wordt vrijgegeven, zijn alle resources zijn gekoppeld ook opgeheven en facturering uiteinden voor het.

1. Reactie van elke code die u eerder hebt toegevoegd aan de Main-methode, met uitzondering van de code om de referenties.

2. Deze methode toevoegen aan de klasse van het programma:

        public static async void StopVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Stopping the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.PowerOffAsync(groupName, vmName);
        }

    Als u toewijzing van de virtuele machine wilt instellen, wijzigt u de aanroep uitschakelen in deze code:

        computeManagementClient.VirtualMachines.Deallocate(groupName, vmName);

3. Aanroepen van de methode die u zojuist hebt toegevoegd, moet u deze code toevoegen aan de Main-methode:

        StopVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Sla het bestand Program.cs.

5. Klik op **starten** in Visual Studio en meldt u zich aan Azure AD met dezelfde gebruikersnaam en wachtwoord die u voor uw abonnement gebruikt.

    Hier ziet u de status van de virtuele machine wijzigen op gestopt. Als u de methode die aanroepen Deallocate, de status is gestopt (opgeheven) uitgevoerd.

## <a name="start-a-virtual-machine"></a>Een virtuele machine start

1. Reactie van elke code die u eerder hebt toegevoegd aan de Main-methode, met uitzondering van de code om de referenties.

2. Deze methode toevoegen aan de klasse van het programma:

        public static async void StartVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Starting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.StartAsync(groupName, vmName);
        }

3. Aanroepen van de methode die u zojuist hebt toegevoegd, moet u deze code toevoegen aan de Main-methode:

        StartVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Sla het bestand Program.cs.

5. Klik op **starten** in Visual Studio en meldt u zich aan Azure AD met dezelfde gebruikersnaam en wachtwoord die u voor uw abonnement gebruikt.

    Hier ziet u de status van de virtuele machine wijzigen in actief.

## <a name="restart-a-running-virtual-machine"></a>Een actieve virtuele machine start

1. Reactie van elke code die u eerder hebt toegevoegd aan de Main-methode, met uitzondering van de code om de referenties.

2. Deze methode toevoegen aan de klasse van het programma:

        public static async void RestartVirtualMachineAsync(
          TokenCredentials credential,
          string groupName,
          string vmName,
          string subscriptionId)
        {
          Console.WriteLine("Restarting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.RestartAsync(groupName, vmName);
        }

3. Aanroepen van de methode die u zojuist hebt toegevoegd, moet u deze code toevoegen aan de Main-methode:

        RestartVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Sla het bestand Program.cs.

5. Klik op **starten** in Visual Studio en meldt u zich aan Azure AD met dezelfde gebruikersnaam en wachtwoord die u voor uw abonnement gebruikt.

## <a name="resize-a-virtual-machine"></a>Het formaat van een virtuele machine

In dit voorbeeld wordt beschreven hoe u de grootte van een actieve virtuele machine wijzigen.

1. Reactie van elke code die u eerder hebt toegevoegd aan de Main-methode, met uitzondering van de code om de referenties.

2. Deze methode toevoegen aan de klasse van het programma:

        public static async void UpdateVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Updating the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(groupName, vmName);
          vmResult.HardwareProfile.VmSize = "Standard_A1";
          await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(groupName, vmName, vmResult);
        }

3. Aanroepen van de methode die u zojuist hebt toegevoegd, moet u deze code toevoegen aan de Main-methode:

        UpdateVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Sla het bestand Program.cs.

5. Klik op **starten** in Visual Studio en meldt u zich aan Azure AD met dezelfde gebruikersnaam en wachtwoord die u voor uw abonnement gebruikt.

    Hier ziet u de grootte van de virtuele machine wijzigen in Standard_A1.

## <a name="add-a-data-disk-to-a-virtual-machine"></a>Een schijf met gegevens toevoegen aan een virtuele machine

In dit voorbeeld ziet u hoe u een schijf met gegevens toevoegt aan een actieve virtuele machine.

1. Reactie van elke code die u eerder hebt toegevoegd aan de Main-methode, met uitzondering van de code om de referenties.

2. Deze methode toevoegen aan de klasse van het programma:

        public static async void AddDataDiskAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Adding the disk to the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(groupName, vmName);
          vmResult.StorageProfile.DataDisks.Add(
            new DataDisk
              {
                Lun = 0,
                Name = "mydatadisk1",
                Vhd = new VirtualHardDisk
                  {
                    Uri = "https://mystorage1.blob.core.windows.net/vhds/mydatadisk1.vhd"
                  },
                CreateOption = DiskCreateOptionTypes.Empty,
                DiskSizeGB = 2,
                Caching = CachingTypes.ReadWrite
              });
          await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(groupName, vmName, vmResult);
        }

3. Aanroepen van de methode die u zojuist hebt toegevoegd, moet u deze code toevoegen aan de Main-methode:

        AddDataDiskAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Sla het bestand Program.cs.

5. Klik op **starten** in Visual Studio en meldt u zich aan Azure AD met dezelfde gebruikersnaam en wachtwoord die u voor uw abonnement gebruikt.

## <a name="delete-a-virtual-machine"></a>Verwijderen van een virtuele machine

1. Reactie van elke code die u eerder hebt toegevoegd aan de Main-methode, met uitzondering van de code om de referenties.

2. Deze methode toevoegen aan de klasse van het programma:

        public static async void DeleteVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Deleting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.DeleteAsync(groupName, vmName);
        }

3. Aanroepen van de methode die u zojuist hebt toegevoegd, moet u deze code toevoegen aan de Main-methode:

        DeleteVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Sla het bestand Program.cs.

5. Klik op **starten** in Visual Studio en meldt u zich aan Azure AD met dezelfde gebruikersnaam en wachtwoord die u voor uw abonnement gebruikt.

## <a name="next-steps"></a>Volgende stappen

Als er problemen met een installatie, misschien interessant [Troubleshooting resource groep implementaties met Azure portal](../resource-manager-troubleshoot-deployments-portal.md)
