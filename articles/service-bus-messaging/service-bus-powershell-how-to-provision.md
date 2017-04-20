<properties
    pageTitle="Service Bus met PowerShell beheren | Microsoft Azure"
    description="Bus-Service beheren met PowerShell scripts"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="sethm"/>

# <a name="manage-service-bus-with-powershell"></a>Service Bus met PowerShell beheren

## <a name="overview"></a>Overzicht

Microsoft Azure PowerShell is een scriptomgeving kunt u de implementatie en het beheer van uw werkbelasting in Azure te automatiseren. In dit artikel wordt beschreven hoe gebruikt u PowerShell inrichten en beheren van entiteiten zoals naamruimten, wachtrijen en gebeurtenis Hubs met een lokale console van Azure PowerShell Service Bus.

## <a name="prerequisites"></a>Vereisten

Voordat u dit artikel, hebt u de volgende vereisten:

- Een abonnement op Azure. Azure is een platform op basis van een abonnement. Zie voor meer informatie over het verkrijgen van een abonnement [Koopt][], [Biedt lid][]of [Gratis][].

- Een computer met Azure PowerShell. Zie voor meer informatie [installeren en configureren van Azure PowerShell][].

- Een algemeen begrip van PowerShell scripts, NuGet pakketten en .NET Framework.

## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>Een verwijzing naar de .NET-assembly voor Service Bus

Er zijn een beperkt aantal PowerShell-cmdlets voor het beheer van de Service Bus. Voor diensten die niet beschikbaar zijn via de bestaande cmdlets inrichten, kunt u de .NET client voor Service Bus in de [Service Bus NuGet package][].

Zorg eerst dat het script de assemblage **Microsoft.ServiceBus.dll** , die is geïnstalleerd met het pakket NuGet kunt vinden. Om flexibel te zijn, voert u het script als volgt:

1. Bepaalt het pad waar deze is aangeroepen.
2. Het pad worden doorlopen totdat er een map met de naam `packages`. Deze map wordt gemaakt wanneer u NuGet pakketten installeert.
3. Recursief zoeken de `packages` map voor een assembly met de naam **Microsoft.ServiceBus.dll**.
4. Verwijst naar de assembly zodat de typen beschikbaar voor later gebruik zijn.

Hier ziet u hoe deze stappen worden geïmplementeerd in een PowerShell script:

```
try
{
    # WARNING: Make sure to reference the latest version of Microsoft.ServiceBus.dll
    Write-Output "Adding the [Microsoft.ServiceBus.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.ServiceBus.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.ServiceBus.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error "Could not add the Microsoft.ServiceBus.dll assembly to the script. Make sure you build the solution before running the provisioning script."
}
```

## <a name="provision-a-service-bus-namespace"></a>Inrichten van een naamruimte Service Bus

Twee PowerShell cmdlets ondersteunen Service Bus naamruimte bewerkingen. In plaats van de API's van .NET SDK kunt u [Get-AzureSBNamespace][] en [Nieuwe AzureSBNamespace][].

In dit voorbeeld wordt een paar lokale variabelen in het script; `$Namespace` and `$Location`.

- `$Namespace`is de naam van de Service Bus naamruimte die we willen werken.
- `$Location`identificatie van het datacenter waarin het script de naamruimte bepalingen.
- `$CurrentNamespace`slaat de verwijzing naamruimte die het script haalt (of maakt).

In een werkelijke script, `$Namespace` en `$Location` als parameters kunnen worden doorgegeven.

Dit deel van het script worden de volgende taken uitgevoerd:

1. Probeert op te halen van een naamruimte Service Bus met de opgegeven naam.
2. Als de naamruimte is gevonden, meldt deze wat is gevonden.
3. Als de naamruimte niet wordt gevonden, wordt gemaakt van de naamruimte en wordt vervolgens de nieuwe naamruimte opgehaald.

    ```
    $Namespace = "MyServiceBusNS"
    $Location = "West US"
    
    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
    
    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Output "The namespace [$Namespace] already exists in the [$($CurrentNamespace.Region)] region."
    }
    else
    {
        Write-Host "The [$Namespace] namespace does not exist."
        Write-Output "Creating the [$Namespace] namespace in the [$Location] region..."
        New-AzureSBNamespace -Name $Namespace -Location $Location -CreateACSNamespace $false -NamespaceType Messaging
        $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
        Write-Host "The [$Namespace] namespace in the [$Location] region has been successfully created."
    }
    ```

Maak een instantie van de klasse [NamespaceManager][] uit de SDK om andere Service Bus entiteiten inrichten.
De cmdlet [Get-AzureSBAuthorizationRule][] kunt u een verificatieregel die wordt gebruikt om een verbindingsreeks ophalen. Slaan we een verwijzing naar de `NamespaceManager` -exemplaar de `$NamespaceManager` variabele. We gebruiken `$NamespaceManager` verderop in het script inrichten van andere entiteiten.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the event hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager = [Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```

## <a name="provisioning-other-service-bus-entities"></a>Andere Service Bus entiteiten inrichten

Inrichten van andere entiteiten, zoals wachtrijen, onderwerpen en gebeurtenis Hubs, gebruiken de [.NET API voor Service Bus][]. Dit artikel is alleen gericht op gebeurtenis Hubs, maar de stappen voor andere entiteiten zijn vergelijkbaar. Bovendien wordt meer gedetailleerde voorbeelden van andere entiteiten, waaronder verwezen aan het einde van dit artikel.

Dit deel van het script maakt vier meer lokale variabelen. Deze variabelen worden gebruikt voor het starten van een `EventHubDescription` object. Het script worden de volgende taken uitgevoerd:

1. Met behulp van de `NamespaceManager` object, controleert u als de Hub gebeurtenis wordt geïdentificeerd door `$Path` bestaat.
2. Als deze niet bestaat, maakt u een `EventHubDescription` en die aan de `NamespaceManager` klasse `CreateEventHubIfNotExists` methode.
3. Nadat u hebt vastgesteld dat de gebeurtenis Hub beschikbaar is, maak een consument groep met `ConsumerGroupDescription` en `NamespaceManager`.

    ```
    $Path  = "MyEventHub"
    $PartitionCount = 12
    $MessageRetentionInDays = 7
    $UserMetadata = $null
    $ConsumerGroupName = "MyConsumerGroup"
        
    # Check to see if the Event Hub already exists
    if ($NamespaceManager.EventHubExists($Path))
    {
        Write-Output "The [$Path] event hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] event hub in the [$Namespace] namespace: PartitionCount=[$PartitionCount] MessageRetentionInDays=[$MessageRetentionInDays]..."
        $EventHubDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.EventHubDescription -ArgumentList $Path
        $EventHubDescription.PartitionCount = $PartitionCount
        $EventHubDescription.MessageRetentionInDays = $MessageRetentionInDays
        $EventHubDescription.UserMetadata = $UserMetadata
        $EventHubDescription.Path = $Path
        $NamespaceManager.CreateEventHubIfNotExists($EventHubDescription);
        Write-Output "The [$Path] event hub in the [$Namespace] namespace has been successfully created."
    }
        
    # Create the consumer group if it doesn't exist
    Write-Output "Creating the consumer group [$ConsumerGroupName] for the [$Path] event hub..."
    $ConsumerGroupDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.ConsumerGroupDescription -ArgumentList $Path, $ConsumerGroupName
    $ConsumerGroupDescription.UserMetadata = $ConsumerGroupUserMetadata
    $NamespaceManager.CreateConsumerGroupIfNotExists($ConsumerGroupDescription);
    Write-Output "The consumer group [$ConsumerGroupName] for the [$Path] event hub has been successfully created."
    ```

## <a name="migrate-a-namespace-to-another-azure-subscription"></a>Een naamruimte migreren naar een ander abonnement van Azure

De volgende reeks opdrachten wordt een naamruimte van een Azure-abonnement naar de andere verplaatst. Voor het uitvoeren van deze bewerking, de naamruimte moet al actief zijn en de gebruiker die de PowerShell-opdrachten moet een beheerder zijn op de bron- en doel-abonnementen.

```
# Create a new resource group in target subscription
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>Volgende stappen

Dit artikel voorziet in een eenvoudige omtrek provisioning Service Bus entiteiten met PowerShell. Alles wat u kunt doen met behulp van de .NET client-bibliotheken, kunt u ook doen in een PowerShell script.

Er zijn meer gedetailleerde voorbeelden beschikbaar op deze blog-updates:

- [Het maken van wachtrijen, onderwerpen en abonnementen met een PowerShell script Service Bus](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Het maken van een Service Bus Namespace en een gebeurtenis Hub met een PowerShell script](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Er zijn ook enkele kant-en-scripts downloaden:
- [Service Bus PowerShell-Scripts](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Link references-->
[Opties voor aanschaf]: http://azure.microsoft.com/pricing/purchase-options/
[Lid aanbiedingen]: http://azure.microsoft.com/pricing/member-offers/
[Gratis proefversie]: http://azure.microsoft.com/pricing/free-trial/
[Installeer en configureer Azure PowerShell]: ../powershell-install-configure.md
[Service Bus NuGet package]: http://www.nuget.org/packages/WindowsAzure.ServiceBus/
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[Nieuwe AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
[.NET API voor Service Bus]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.aspx
[NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
