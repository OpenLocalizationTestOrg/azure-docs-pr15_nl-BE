<properties
    pageTitle="Migreren naar Resource Manager met PowerShell | Microsoft Azure"
    description="Dit artikel helpt bij de migratie platform ondersteund IaaS bronnen van klassiek Azure Resource Manager met Azure PowerShell-opdrachten"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="cynthn"/>

# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-powershell"></a>IaaS resources migreren van klassiek Azure Resource Manager met Azure PowerShell

Deze stappen laten zien hoe u Azure PowerShell-opdrachten gebruiken voor het migreren van infrastructuur als een (IaaS) serviceresources uit het implementatiemodel klassiek aan de bronnenbeheerder Azure implementatiemodel. 

Als u wilt, kunt u ook resources migreren met de [Azure Command Line Interface (CLI Azure)](virtual-machines-linux-cli-migration-classic-resource-manager.md).

- Zie voor achtergrondinformatie over de ondersteunde migratiescenario's, [migratie van bronnen van klassieke Azure Resource Manager IaaS Platform ondersteund](virtual-machines-windows-migration-classic-resource-manager.md). 
- Zie voor gedetailleerde instructies en een overzicht van de migratie, [technische diepe kennismaking op migratie van klassieke Azure Resource Manager platform ondersteund](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md).

## <a name="step-1-plan-for-migration"></a>Stap 1: Plan voor migratie

Hier volgen een paar tips aangeraden als u migreren IaaS resources uit klassieke Resource Manager te evalueren:

- Lees de [ondersteunde en niet-ondersteunde functies en configuraties](virtual-machines-windows-migration-classic-resource-manager.md). Als u virtuele machines die niet-ondersteunde configuraties of functies gebruiken, wordt u aangeraden wachten voor ondersteuning van de functie/configuratie worden aangekondigd. Ook als dit past bij uw behoeften, deze functie te verwijderen of uit die configuratie migratie inschakelen.
-   Als u hebben geautomatiseerde scripts die vandaag nog uw infrastructuur en toepassingen implementeren, kunt u een vergelijkbare instellingen te maken met behulp van deze scripts voor migratie. U kunt ook monster omgevingen instellen met behulp van de portal Azure.

>[AZURE.IMPORTANT]Virtueel netwerkgateways (site-tot-site, Azure ExpressRoute, toepassingsgateway, punt-tot-site) worden momenteel niet ondersteund voor migratie van klassiek tot Resource Manager. De migratie van een klassieke virtueel netwerk met een gateway, de gateway te verwijderen voordat u een doorvoerbewerking om te verplaatsen van het netwerk (u kunt de stap voorbereiden uitvoeren zonder het verwijderen van de gateway). Nadat de migratie is voltooid, sluit u de gateway in Azure Resource Manager.

## <a name="step-2-install-the-latest-version-of-azure-powershell"></a>Stap 2: Installeer de nieuwste versie van Azure PowerShell

Er zijn twee belangrijke opties voor het installeren van Azure PowerShell: [PowerShell Gallery](https://www.powershellgallery.com/profiles/azure-sdk/) of [Web Platform Installer (WebPI)](http://aka.ms/webpi-azps). WebPI ontvangt maandelijkse updates. Galerie met PowerShell ontvangt updates op een continue basis. Dit artikel is gebaseerd op Azure PowerShell versie 2.1.0.

Zie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md)voor installatie-instructies.

<br>
## <a name="step-3-set-your-subscription-and-sign-up-for-migration"></a>Stap 3: Instellen van uw abonnement en meld u aan voor de migratie

Eerst een PowerShell-prompt. Migreren, moet u voor het instellen van uw omgeving voor zowel klassiek en Resource Manager.

Aanmelden bij uw account voor de Resource Manager-model.

```powershell
    Login-AzureRmAccount
```

De beschikbare abonnementen opvragen door de volgende opdracht:

```powershell
    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName
```

Stel uw Azure abonnement voor de huidige sessie. In het volgende voorbeeld wordt de naam van het standaard-abonnement op **Mijn abonnement op Azure**. De naam van het abonnement voorbeeld vervangen door uw eigen. 

```powershell
    Select-AzureRmSubscription –SubscriptionName "My Azure Subscription"
```

>[AZURE.NOTE] Registratie is een eenmalige stap, maar moet u dit doen eenmaal voordat u de migratie. Zonder te registreren ziet u het volgende foutbericht weergegeven: 

>   *BadRequest: Abonnement is niet geregistreerd voor migratie.* 

Met de migratie resource provider registreren door de volgende opdracht:

```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Wacht vijf minuten om de inschrijving te voltooien. U kunt de status van de erkenning controleren door de volgende opdracht:

```powershell
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Zorg ervoor dat RegistrationState `Registered` voordat u verdergaat. 

Nu aanmelden bij uw account voor het klassieke model.

```powershell
    Add-AzureAccount
```

De beschikbare abonnementen opvragen door de volgende opdracht:

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

Stel uw Azure abonnement voor de huidige sessie. In het volgende voorbeeld wordt het standaardabonnement op **Mijn abonnement op Azure**. De naam van het abonnement voorbeeld vervangen door uw eigen. 

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-4-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Stap 4: Controleer of er voldoende cores Azure Resource Manager virtuele Machine in de regio van uw huidige distributie of VNET met Azure

De volgende PowerShell-opdracht kunt u het huidige aantal cores u in Azure Resource Manager hebt controleren. Zie voor meer informatie over quota's core, [grenzen en de bronnenbeheerder Azure](../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager). 

In dit voorbeeld wordt de beschikbaarheid in de regio **West VS** gecontroleerd. De naam van het voorbeeld-gebied vervangen door uw eigen. 

```powershell
Get-AzureRmVMUsage -Location "West US"
```

## <a name="step-5-run-commands-to-migrate-your-iaas-resources"></a>Stap 5: Uitvoeren van opdrachten voor het migreren van uw resources IaaS

>[AZURE.NOTE] Alle hier beschreven handelingen zijn idempotency is ingeschakeld. Als er een probleem met een niet-ondersteunde functie of een fout in de configuratie, is het raadzaam dat u opnieuw het voorbereiden proberen, of bewerking afgebroken. Het platform probeert vervolgens de bewerking opnieuw.

### <a name="migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a>Migratie van virtuele machines in een cloud-service (niet in een virtueel netwerk)

De lijst met cloud services ophalen met behulp van de volgende opdracht uit en kies vervolgens de cloud-service die u wilt migreren. Als het VMs in de cloud-service in een virtueel netwerk of web of de werknemer zijn, retourneert de opdracht een foutbericht wordt weergegeven.

```powershell
    Get-AzureService | ft Servicename
```

De naam van de implementatie van de cloud-service verkrijgen. In dit voorbeeld is de naam van **Mijn Service**. De naam van het voorbeeld vervangen door uw eigen naam. 

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

De virtuele machines in de cloud service voor migratie voorbereiden. U hebben kunt kiezen uit twee opties.

* **Optie 1. Het VMs migreren naar virtueel netwerk platform gemaakt**

    Eerst controleren of u de cloud-service met de volgende opdrachten kunt migreren:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```

    De bovenstaande opdracht geeft eventuele waarschuwingen en fouten die migratie te blokkeren. Als de validatie succesvol is, kunt klik u verplaatsen op de stap **voorbereiden** :

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```

* **Optie 2. Migreren naar een bestaande virtuele netwerk in het implementatiemodel Resource Manager**

    In het volgende voorbeeld wordt de naam van de resource op de **myResourceGroup**, het virtuele netwerknaam naar **myVirtualNetwork** en het subnet in **mySubNet**. De namen in het voorbeeld vervangen door de namen van de eigen middelen.

    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```

    Eerst controleren of u kunt migreren de cloud-service met de volgende opdracht:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```

    De bovenstaande opdracht geeft eventuele waarschuwingen en fouten die migratie te blokkeren. Als de validatie succesvol is, kunt vervolgens u doorgaan met de volgende stap van voorbereiden:

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```
    

Nadat de bewerking voorbereiden is uitgevoerd met een van de bovenstaande opties, query op de migratiestatus van het VMs. Zorg ervoor dat ze de `Prepared` staat.

In het volgende voorbeeld wordt de naam van de VM op **myVM**. De Voorbeeldnaam vervangen door uw eigen naam VM.

    ```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
    ```

Controleer de configuratie voor de voorbereide resources met PowerShell of de Azure portal. Als u niet gereed voor migratie zijn en u wilt teruggaan naar de oude staat, gebruikt u de volgende opdracht:

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

U kunt de vooraf gedefinieerde configuratie ziet er goed uit, vooruit en verbindt zich ertoe de resources door de volgende opdracht:

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

### <a name="migrate-virtual-machines-in-a-virtual-network"></a>Migratie van virtuele machines in een virtueel netwerk

De migratie van virtuele machines in een virtueel netwerk, kunt u het netwerk migreren. De virtuele machines migreren automatisch met het netwerk. Kies het virtuele netwerk waarmee u wilt migreren. 

In het volgende voorbeeld wordt de naam van het virtuele netwerk op **myVnet**. De naam van een virtueel netwerk voorbeeld vervangen door uw eigen. 

```powershell
    $vnetName = "myVnet"
```

>[AZURE.NOTE] Als het virtuele netwerk of rollen van de werknemer of VMs met niet-ondersteunde configuraties bevat, krijgt u een foutbericht voor gegevensvalidatie.

Eerst controleren of u het virtuele netwerk migreren kunt met de volgende opdracht:

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

De bovenstaande opdracht geeft eventuele waarschuwingen en fouten die migratie te blokkeren. Als de validatie succesvol is, kunt vervolgens u doorgaan met de volgende stap van voorbereiden:

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

Controleer de configuratie voor de voorbereide virtuele machines met behulp van Azure PowerShell of de Azure portal. Als u niet gereed voor migratie zijn en u wilt teruggaan naar de oude staat, gebruikt u de volgende opdracht:

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

U kunt de vooraf gedefinieerde configuratie ziet er goed uit, vooruit en verbindt zich ertoe de resources door de volgende opdracht:

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

### <a name="migrate-a-storage-account"></a>Een opslag account migreren

Zodra u bent met klaar de virtuele machines migreren, wordt aangeraden bij het migreren van de rekeningen van de opslag.

Elke account opslag voorbereiden voor migratie door de volgende opdracht. In dit voorbeeld wordt de naam van de opslag **myStorageAccount**. De Voorbeeldnaam vervangen door de naam van uw eigen account opslag. 

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
```

Controleer de configuratie voor de opslag van bereide rekening met Azure PowerShell of de Azure portal. Als u niet gereed voor migratie zijn en u wilt teruggaan naar de oude staat, gebruikt u de volgende opdracht:

```powershell
    Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
```

U kunt de vooraf gedefinieerde configuratie ziet er goed uit, vooruit en verbindt zich ertoe de resources door de volgende opdracht:

```powershell
    Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
```

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over migratie, [migratie van bronnen van klassieke Azure Resource Manager IaaS Platform ondersteund](virtual-machines-windows-migration-classic-resource-manager.md).
- Gebruiken om andere netwerkbronnen aan bronbeheer met Azure PowerShell migreren, vergelijkbare stappen [Verplaatsen AzureNetworkSecurityGroup](https://msdn.microsoft.com/library/mt786729.aspx) [Verplaatsen AzureReservedIP](https://msdn.microsoft.com/library/mt786752.aspx)en [Verplaatsen AzureRouteTable](https://msdn.microsoft.com/library/mt786718.aspx).
- Zie voor open source scripts die kunt u Azure bronnen migreren van klassiek tot Resource Manager, [communautaire hulpprogramma's voor migratie naar Azure Resource Manager-migratie](virtual-machines-windows-migration-scripts.md)
