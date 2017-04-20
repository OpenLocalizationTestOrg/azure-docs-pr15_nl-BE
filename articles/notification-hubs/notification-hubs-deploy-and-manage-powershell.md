<properties 
    pageTitle="Implementeren en beheren van kennisgeving Hubs met PowerShell" 
    description="Het maken en beheren van kennisgeving Hubs met PowerShell voor automatisering" 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="powershell" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="deploy-and-manage-notification-hubs-using-powershell"></a>Implementeren en beheren van kennisgeving Hubs met PowerShell

##<a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe u gebruik maken en beheren van Azure melding Hubs met PowerShell. In dit onderwerp worden de volgende veelvoorkomende automatiseringstaken weergegeven.

+ Een Hub melding maken
+ Referenties instellen

Als u ook nodig hebt voor het maken van een nieuwe service bus naamruimte voor uw melding hubs, Zie [Service Bus beheren met PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md).

Meldingen Hubs beheren wordt niet ondersteund door de opgenomen met Azure PowerShell-cmdlets. De beste manier van PowerShell is als u verwijst naar de assembly Microsoft.Azure.NotificationHubs.dll. De assembly wordt met de [Microsoft Azure melding Hubs NuGet pakket](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)gedistribueerd.


## <a name="prerequisites"></a>Vereisten

Voordat u dit artikel hebt u het volgende:

- Een abonnement op Azure. Azure is een platform op basis van een abonnement. Zie voor meer informatie over het verkrijgen van een abonnement [Koopt], [Biedt lid]of [Gratis].

- Een computer met Azure PowerShell. Zie voor meer informatie [installeren en configureren van Azure PowerShell].

- Een algemeen begrip van PowerShell scripts, NuGet pakketten en .NET Framework.


## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>Een verwijzing naar de .NET-assembly voor Service Bus

Azure melding Hubs beheren is nog niet opgenomen in de PowerShell cmdlets in Azure PowerShell. Als u wilt inrichten melding hubs, kunt u de .NET client die wordt geleverd in het [pakket met Microsoft Azure melding Hubs NuGet](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Controleer eerst of uw script kunt zoeken naar de **Microsoft.Azure.NotificationHubs.dll** -assembly wordt geïnstalleerd als een NuGet-pakket in een Visual Studio-project. Om flexibel te zijn, voert u het script als volgt:

1. Bepaalt het pad waar deze is aangeroepen.
2. Het pad worden doorlopen totdat er een map met de naam `packages`. Deze map wordt gemaakt wanneer u NuGet pakketten voor Visual Studio-projecten installeren.
3. Recursief zoeken de `packages` map voor een assembly met de naam **Microsoft.Azure.NotificationHubs.dll**.
4. Verwijst naar de assembly zodat de typen beschikbaar voor later gebruik zijn.

Hier ziet u hoe deze stappen worden geïmplementeerd in een PowerShell script:

``` powershell

try
{
    # WARNING: Make sure to reference the latest version of Microsoft.Azure.NotificationHubs.dll
    Write-Output "Adding the [Microsoft.Azure.NotificationHubs.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.Azure.NotificationHubs.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.Azure.NotificationHubs.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.Azure.NotificationHubs.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}
```

## <a name="create-the-namespacemanager-class"></a>De klasse NamespaceManager maken

Als u wilt inrichten melding Hubs, maken een instantie van de klasse [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) uit de SDK. 

De cmdlet [Get-AzureSBAuthorizationRule] is opgenomen met Azure PowerShell kunt u een verificatieregel die wordt gebruikt om een verbindingsreeks ophalen. Slaan we een verwijzing naar de `NamespaceManager` -exemplaar de `$NamespaceManager` variabele. We gebruiken `$NamespaceManager` een kennisgeving hub inrichten.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager=[Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```


## <a name="provisioning-a-new-notification-hub"></a>Inrichten van een nieuwe melding Hub 

Gebruik de [.NET API voor kennisgeving Hubs]voor het inrichten van een nieuwe melding hub.

In dit deel van het script instellen u vier lokale variabelen. 

1. `$Namespace`: Deze instellen op de naam van de naamruimte waar u een hub melding maken.
2. `$Path`: Dit pad ingesteld op de naam van de nieuwe kennisgeving hub.  Bijvoorbeeld "MyHub".    
3. `$WnsPackageSid`: Dit aan de SID van het pakket voor u ingesteld Windows App van het [Dev Center van Windows](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).
4. `$WnsSecretkey`: Dit aan de geheime sleutel voor u ingesteld Windows App van het [Dev Center van Windows](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).

Deze variabelen worden gebruikt om verbinding met de naamruimte en maakt een nieuwe melding Hub is geconfigureerd voor het afhandelen van Windows Notification Services (WNS) meldingen met WNS referenties voor een Windows-App. Voor meer informatie over het verkrijgen van het pakket SID en geheime sleutel ziet, is de zelfstudie [Aan de slag met melding Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) . 

+ Het fragment script maakt gebruik van de `NamespaceManager` -object om te controleren als de Hub melding geïdentificeerd door `$Path` bestaat.

+ Als deze niet bestaat, maakt het script een `NotificationHubDescription` met WNS referenties en laat die aan de `NamespaceManager` klasse `CreateNotificationHub` methode.

``` powershell

$Namespace = "<Enter your namespace>"
$Path  = "<Enter a name for your notification hub>"
$WnsPackageSid = "<your package sid>"
$WnsSecretkey = "<enter your secret key>"

$WnsCredential = New-Object -TypeName Microsoft.Azure.NotificationHubs.WnsCredential -ArgumentList $WnsPackageSid,$WnsSecretkey

# Query the namespace
$CurrentNamespace = Get-AzureSBNamespace -Name $Namespace

# Check if the namespace already exists
if ($CurrentNamespace)
{
    Write-Output "The namespace [$Namespace] in the [$($CurrentNamespace.Region)] region was found."

    # Create the NamespaceManager object used to create a new notification hub
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."

    # Check to see if the Notification Hub already exists
    if ($NamespaceManager.NotificationHubExists($Path))
    {
        Write-Output "The [$Path] notification hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] notification hub in the [$Namespace] namespace."
        $NHDescription = New-Object -TypeName Microsoft.Azure.NotificationHubs.NotificationHubDescription -ArgumentList $Path;
        $NHDescription.WnsCredential = $WnsCredential;
        $NamespaceManager.CreateNotificationHub($NHDescription);
        Write-Output "The [$Path] notification hub was created in the [$Namespace] namespace."
    }
}
else
{
    Write-Host "The [$Namespace] namespace does not exist."
}
```




## <a name="additional-resources"></a>Aanvullende bronnen

- [Service Bus met PowerShell beheren](../service-bus-messaging/service-bus-powershell-how-to-provision.md)
- [Het maken van wachtrijen, onderwerpen en abonnementen met een PowerShell script Service Bus](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Het maken van een Service Bus Namespace en een gebeurtenis Hub met een PowerShell script](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Er zijn ook enkele kant-en-scripts downloaden:
- [Service Bus PowerShell-Scripts](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)
 

[Opties voor aanschaf]: http://azure.microsoft.com/pricing/purchase-options/
[Lid aanbiedingen]: http://azure.microsoft.com/pricing/member-offers/
[Gratis proefversie]: http://azure.microsoft.com/pricing/free-trial/
[Installeer en configureer Azure PowerShell]: ../powershell-install-configure.md
[.NET API voor kennisgeving Hubs]: https://msdn.microsoft.com/library/azure/mt414893.aspx
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[New-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
 
