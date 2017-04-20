<properties
    pageTitle="Gebruikt u PowerShell Bus-Service en gebeurtenis Hubs bronnen beheren | Microsoft Azure"
    description="Met behulp van PowerShell maken en beheren van bronnen Bus-Service en gebeurtenis Hubs"
    services="service-bus,event-hubs"
    documentationCenter=".NET"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="use-powershell-to-manage-service-bus-and-event-hubs-resources"></a>Gebruikt u PowerShell Bus-Service en gebeurtenis Hubs bronnen beheren

Microsoft Azure PowerShell is een scriptomgeving kunt u de implementatie en het beheer van Azure services te automatiseren. In dit artikel wordt beschreven hoe gebruikt u PowerShell inrichten en beheren van entiteiten zoals naamruimten, wachtrijen en gebeurtenis Hubs met een lokale console van Azure PowerShell Service Bus.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, hebt u het volgende nodig:

- Een abonnement op Azure. Azure is een platform op basis van een abonnement. Zie voor meer informatie over het verkrijgen van een abonnement [kopen opties][] [biedt lid][]of [gratis account][].

- Een computer met Azure PowerShell. Zie voor meer informatie [installeren en configureren van Azure PowerShell][].

- Een algemeen begrip van PowerShell scripts, NuGet pakketten en .NET Framework.

## <a name="include-a-reference-to-the-net-assembly-for-service-bus"></a>Een verwijzing naar de .NET-assembly voor Service Bus

Er zijn een beperkt aantal PowerShell-cmdlets voor het beheer van de Service Bus. Voor entiteiten die niet toegankelijk zijn via de bestaande cmdlets inrichten, kunt u de .NET-client voor Service-Bus van PowerShell door te verwijzen naar de [Service Bus NuGet package].

Zorg eerst dat het script de assemblage **Microsoft.ServiceBus.dll** , die is geïnstalleerd met het pakket NuGet kunt vinden. Om flexibel te zijn, voert u het script als volgt:

1. Hiermee bepaalt u het pad van waaruit deze is aangeroepen.
2. Het pad worden doorlopen totdat er een map met de naam `packages`. Deze map wordt gemaakt wanneer u NuGet pakketten installeert.
3. Recursief zoeken de `packages` map voor een assembly met de naam **Microsoft.ServiceBus.dll**.
4. Verwijst naar de assembly zodat de typen beschikbaar voor later gebruik zijn.

Hier ziet u hoe deze stappen worden geïmplementeerd in een PowerShell script:

```powershell

try
{
    # IMPORTANT: Make sure to reference the latest version of Microsoft.ServiceBus.dll
    Write-Output "Adding the [Microsoft.ServiceBus.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.ServiceBus.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.ServiceBus.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.ServiceBus.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}

```

## <a name="provision-a-service-bus-namespace"></a>Inrichten van een naamruimte Service Bus

Als u werkt met naamruimten Service Bus, er zijn twee cmdlets kunt u in plaats van de .NET SDK: [Get-AzureSBNamespace][] en [Nieuwe AzureSBNamespace][].

In dit voorbeeld wordt een paar lokale variabelen in het script; `$Namespace` and `$Location`.

- `$Namespace`de naam is het van de Service Bus naamruimte die we willen werken.
- `$Location`identificeert het datacenter waar zullen wij gegevens leveren voor de naamruimte.
- `$CurrentNamespace`slaat de verwijzing naamruimte die we halen (of maken).

In een werkelijke script, `$Namespace` en `$Location` als parameters kunnen worden doorgegeven.

Dit deel van het script doet het volgende:

1. Probeert op te halen van een naamruimte Service Bus met de opgegeven naam.
2. Als de naamruimte is gevonden, meldt deze wat is gevonden.
3. Als de naamruimte niet wordt gevonden, wordt gemaakt van de naamruimte en wordt vervolgens de nieuwe naamruimte opgehaald.

    ``` powershell

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
        New-AzureSBNamespace -Name $Namespace -Location $Location -CreateACSNamespace false -NamespaceType Messaging
        $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
        Write-Host "The [$Namespace] namespace in the [$Location] region has been successfully created."
    }
    ```
Om u te richten op andere Service Bus entiteiten, maakt een instantie van het `NamespaceManager` -object uit de SDK. De cmdlet [Get-AzureSBAuthorizationRule][] kunt u een verificatieregel die wordt gebruikt om een verbindingsreeks ophalen. In dit voorbeeld bevat een verwijzing naar de `NamespaceManager` -exemplaar de `$NamespaceManager` variabele. Het script wordt later `$NamespaceManager` inrichten van andere entiteiten.

    ``` powershell
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    # Create the NamespaceManager object to create the Event Hub
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
    ```

## <a name="provisioning-other-service-bus-entities"></a>Andere Service Bus entiteiten inrichten

Als u wilt inrichten van andere entiteiten, zoals wachtrijen, onderwerpen en Hubs, gebeurtenis, kunt u de [.NET API voor Service Bus][]. Meer gedetailleerde voorbeelden, met inbegrip van andere entiteiten, wordt aan het einde van dit artikel verwezen.

### <a name="create-an-event-hub"></a>Een Hub gebeurtenis maken

Dit deel van het script maakt vier meer lokale variabelen. Deze variabelen worden gebruikt voor het starten van een `EventHubDescription` object. Het script doet het volgende:

1. Met behulp van de `NamespaceManager` object, controleert u als de Hub gebeurtenis wordt geïdentificeerd door `$Path` bestaat.
2. Als deze niet bestaat, maakt u een `EventHubDescription` en die aan de `NamespaceManager` klasse `CreateEventHubIfNotExists` methode.
3. Nadat u hebt vastgesteld dat de gebeurtenis Hub beschikbaar is, maak een consument groep met `ConsumerGroupDescription` en `NamespaceManager`.

    ``` powershell

    $Path  = "MyEventHub"
    $PartitionCount = 12
    $MessageRetentionInDays = 7
    $UserMetadata = $null
    $ConsumerGroupName = "MyConsumerGroup"

    # Check if the Event Hub already exists
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

### <a name="create-a-queue"></a>Een wachtrij maken

Om een wachtrij of onderwerp, gecontroleerd naamruimte als in de vorige sectie.

    if ($NamespaceManager.QueueExists($Path))
    {
        Write-Output "The [$Path] queue already exists in the [$Namespace] namespace."
    }
    else
    {
        Write-Output "Creating the [$Path] queue in the [$Namespace] namespace..."
        $QueueDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.QueueDescription -ArgumentList $Path
        if ($AutoDeleteOnIdle -ge 5)
        {
            $QueueDescription.AutoDeleteOnIdle = [System.TimeSpan]::FromMinutes($AutoDeleteOnIdle)
        }
        if ($DefaultMessageTimeToLive -gt 0)
        {
            $QueueDescription.DefaultMessageTimeToLive = [System.TimeSpan]::FromMinutes($DefaultMessageTimeToLive)
        }
        if ($DuplicateDetectionHistoryTimeWindow -gt 0)
        {
            $QueueDescription.DuplicateDetectionHistoryTimeWindow = [System.TimeSpan]::FromMinutes($DuplicateDetectionHistoryTimeWindow)
        }
        $QueueDescription.EnableBatchedOperations = $EnableBatchedOperations
        $QueueDescription.EnableDeadLetteringOnMessageExpiration = $EnableDeadLetteringOnMessageExpiration
        $QueueDescription.EnableExpress = $EnableExpress
        $QueueDescription.EnablePartitioning = $EnablePartitioning
        $QueueDescription.ForwardDeadLetteredMessagesTo = $ForwardDeadLetteredMessagesTo
        $QueueDescription.ForwardTo = $ForwardTo
        $QueueDescription.IsAnonymousAccessible = $IsAnonymousAccessible
        if ($LockDuration -gt 0)
        {
            $QueueDescription.LockDuration = [System.TimeSpan]::FromSeconds($LockDuration)
        }
        $QueueDescription.MaxDeliveryCount = $MaxDeliveryCount
        $QueueDescription.MaxSizeInMegabytes = $MaxSizeInMegabytes
        $QueueDescription.RequiresDuplicateDetection = $RequiresDuplicateDetection
        $QueueDescription.RequiresSession = $RequiresSession
        if ($EnablePartitioning)
        {
            $QueueDescription.SupportOrdering = $False
        }
        else
        {
            $QueueDescription.SupportOrdering = $SupportOrdering
        }
        $QueueDescription.UserMetadata = $UserMetadata
        $NamespaceManager.CreateQueue($QueueDescription);
    }

### <a name="create-a-topic"></a>Een onderwerp maken

    if ($NamespaceManager.TopicExists($Path))
    {
        Write-Output "The [$Path] topic already exists in the [$Namespace] namespace."
    }
    else
    {
        Write-Output "Creating the [$Path] topic in the [$Namespace] namespace..."
        $TopicDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.TopicDescription -ArgumentList $Path
        if ($AutoDeleteOnIdle -ge 5)
        {
            $TopicDescription.AutoDeleteOnIdle = [System.TimeSpan]::FromMinutes($AutoDeleteOnIdle)
        }
        if ($DefaultMessageTimeToLive -gt 0)
        {
            $TopicDescription.DefaultMessageTimeToLive = [System.TimeSpan]::FromMinutes($DefaultMessageTimeToLive)
        }
        if ($DuplicateDetectionHistoryTimeWindow -gt 0)
        {
            $TopicDescription.DuplicateDetectionHistoryTimeWindow = [System.TimeSpan]::FromMinutes($DuplicateDetectionHistoryTimeWindow)
        }
        $TopicDescription.EnableBatchedOperations = $EnableBatchedOperations
        $TopicDescription.EnableExpress = $EnableExpress
        $TopicDescription.EnableFilteringMessagesBeforePublishing = $EnableFilteringMessagesBeforePublishing
        $TopicDescription.EnablePartitioning = $EnablePartitioning
        $TopicDescription.IsAnonymousAccessible = $IsAnonymousAccessible
        $TopicDescription.MaxSizeInMegabytes = $MaxSizeInMegabytes
        $TopicDescription.RequiresDuplicateDetection = $RequiresDuplicateDetection
        if ($EnablePartitioning)
        {
            $TopicDescription.SupportOrdering = $False
        }
        else
        {
            $TopicDescription.SupportOrdering = $SupportOrdering
        }
        $TopicDescription.UserMetadata = $UserMetadata
        $NamespaceManager.CreateTopic($TopicDescription);
    }

## <a name="next-steps"></a>Volgende stappen

In dit artikel geleverd met een eenvoudige werkstroom voor het inrichten van Service Bus entiteiten met PowerShell. Hoewel er een beperkt aantal PowerShell-cmdlets voor het beheer van de Service Bus kunt messaging entiteiten, door te verwijzen naar de assembly Microsoft.ServiceBus.dll, vrijwel elke bewerking u uitvoeren met behulp van de .NET clientbibliotheken die kunt u in een PowerShell script uitvoeren.

Meer gedetailleerde voorbeelden zijn beschikbaar in deze blog-updates:

- [Het maken van wachtrijen, onderwerpen en abonnementen met een PowerShell script Service Bus](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Het maken van een Service Bus Namespace en een gebeurtenis Hub met een PowerShell script](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Er zijn ook enkele kant-en-scripts downloaden:

- [Service Bus PowerShell-Scripts](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Anchors-->

[Opties voor aanschaf]: http://azure.microsoft.com/pricing/purchase-options/
[lid aanbiedingen]: http://azure.microsoft.com/pricing/member-offers/
[gratis account]: http://azure.microsoft.com/pricing/free-trial/
[Service Bus NuGet package]: http://www.nuget.org/packages/WindowsAzure.ServiceBus/
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[Nieuwe AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
[.NET API voor Service Bus]: https://msdn.microsoft.com/en-us/library/azure/mt419900.aspx
[Installeer en configureer Azure PowerShell]: ../powershell-install-configure.md
