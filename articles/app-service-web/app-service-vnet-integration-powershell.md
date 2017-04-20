<properties
    pageTitle="Uw app verbinding met het virtuele netwerk via PowerShell"
    description="Instructies over het aansluiten en het werken met virtuele netwerken via PowerShell"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="wpickett"
    editor="cephalin"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="ccompy"/>

# <a name="connect-your-app-to-your-virtual-network-by-using-powershell"></a>Uw app verbinding met het virtuele netwerk via PowerShell #

## <a name="overview"></a>Overzicht ##

In Azure App-Service, kunt u uw app (web, mobiele of API) met Azure virtual network (VNet) op uw abonnement. Deze functie heet VNet integratie. De functie Integratie met VNet moet niet worden verward met de functie App-omgeving, kunt u een exemplaar van Azure App Service uitvoeren in het virtuele netwerk.

De functie Integratie met VNet heeft een gebruikersinterface (UI) in de nieuwe portal die u gebruiken kunt om te integreren met de virtuele netwerken die worden geïmplementeerd met behulp van het implementatiemodel klassiek of het implementatiemodel Azure Resource Manager. Als u weten over de functie wilt, Zie [de app met een Azure virtueel netwerk integreren](web-sites-integrate-with-vnet.md).

Dit artikel is niet over het gebruik van de gebruikersinterface, maar veeleer over het inschakelen van integratie met PowerShell. Omdat de opdrachten voor elk implementatiemodel verschillend zijn, heeft dit artikel een sectie voor elk implementatiemodel.  

Voordat u met dit artikel doorgaat, zorg ervoor dat:

- De meest recente Azure PowerShell SDK is geïnstalleerd. U kunt dit installeren met het installatieprogramma van de Web-Platform.
- Een app in Azure App-Service wordt uitgevoerd in een Standard of Premium SKU.

## <a name="classic-virtual-networks"></a>Klassieke virtuele netwerken ##

In deze sectie worden drie taken voor virtuele netwerken die gebruikmaken van het implementatiemodel klassiek uitgelegd:

1. Uw app verbinding met een bestaande virtuele netwerk dat is een gateway en punt-naar-site verbinding is geconfigureerd.
1. Virtuele integratie netwerkgegevens voor uw app bijwerken.
1. Uw app uw virtuele netwerk verbreken.

### <a name="connect-an-app-to-a-classic-vnet"></a>Verbinding maken met een app een klassieke VNet ###

Voor een app verbinding met een virtueel netwerk, volg deze drie stappen:

1. Web App verklaren dat zij een bepaald virtueel netwerk zal verbinden. De app genereert een certificaat dat wordt verleend aan het virtuele netwerk punt-naar-site verbinding.
1. Het web app certificaat uploaden naar het virtuele netwerk en vervolgens het point-to-site VPN-pakket URI opgehaald.
1. De web-app virtueel netwerkverbinding met het pakket URI met punt-naar-site bijwerken.

De eerste en derde stap worden opgenomen in scripts, maar de tweede stap is een eenmalige, handmatige actie via de portal of access acties uit te voeren **PLAATST** of de **PATCH** op het eindpunt van de virtuele Azure Resource Manager vereist. Neem contact op met ondersteuning van Azure als dit is ingeschakeld. Voordat u begint, zorg ervoor dat er een klassieke virtuele netwerk met punt-naar-site verbinding al ingeschakeld en de gebruikte gateway. De gateway maken en punt-naar-site verbinding, moet u de portal te gebruiken zoals beschreven in het [maken van een VPN-gateway][createvpngateway].

De klassieke virtueel netwerk moet in het abonnement hetzelfde als uw App serviceplan die in het bezit van de app die u integreren.

##### <a name="set-up-azure-powershell-sdk"></a>Azure PowerShell SDK instellen #####

Open een venster PowerShell en Azure-account en abonnement stellen via:

    Login-AzureRmAccount

Die opdracht wordt gevraagd uw referenties Azure geopend. Nadat u zich hebt aangemeld, gebruik een van de volgende opdrachten om het abonnement dat u wilt gebruiken. Let erop dat u het abonnement die het virtuele netwerk en App serviceplan.

    Select-AzureRmSubscription –SubscriptionName [WebAppSubscriptionName]

of

    Select-AzureRmSubscription –SubscriptionId [WebAppSubscriptionId]

##### <a name="variables-used-in-this-article"></a>Variabelen die worden gebruikt in dit artikel #####

Om de opdrachten vereenvoudigen, zullen we een variabele **$Configuration** PowerShell met specifieke configuratie instellen.

Een variabele als volgt instellen in PowerShell met de volgende parameters:

    $Configuration = @{}
    $Configuration.WebAppResourceGroup = "[Your web app resource group]"
    $Configuration.WebAppName = "[Your web app name]"
    $Configuration.VnetSubscriptionId = "[Your vnet subscription id]"
    $Configuration.VnetResourceGroup = "[Your vnet resource group]"
    $Configuration.VnetName = "[Your vnet name]"

De locatie app moet de locatie zonder spaties. West VS is bijvoorbeeld westus.

    $Configuration.WebAppLocation = "[Your web app Location]"

Het volgende artikel is waar het certificaat moet worden geschreven. Dit moet een pad op de lokale computer. Zorg ervoor dat de CER aan het einde.

    $Configuration.GeneratedCertificatePath = "[C:\Path\To\Certificate.cer]"

Als u wilt zien wat u precies instellen, typt u **$Configuration**.

    > $Configuration

    Name                           Value
    ----                           -----
    GeneratedCertificatePath       C:\vnetcert.cer
    VnetSubscriptionId             efc239a4-88f9-2c5e-a9a1-3034c21ad496
    WebAppResourceGroup            vnetdemo-rg
    VnetResourceGroup              testase1-rg
    VnetName                       TestNetwork
    WebAppName                     vnetintdemoapp
    WebAppLocation                 centralus

De rest van dit gedeelte wordt ervan uitgegaan dat u een variabele gemaakt als zojuist beschreven.

##### <a name="declare-the-virtual-network-to-the-app"></a>Het virtuele netwerk App declareren #####

Gebruik de volgende opdracht kunt u de app zien dat het dit bepaalde virtuele netwerk wordt gebruikt. Hierdoor wordt de toepassing voor het genereren van de benodigde certificaten:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -PropertyObject @{"VnetResourceId" = "/subscriptions/$($Configuration.VnetSubscriptionId)/resourceGroups/$($Configuration.VnetResourceGroup)/providers/Microsoft.ClassicNetwork/virtualNetworks/$($Configuration.VnetName)"} -Location $Configuration.WebAppLocation -ApiVersion 2015-07-01

Als deze opdracht is uitgevoerd, heeft **$vnet** een variabele **Eigenschappen** in deze. De variabele **Eigenschappen** moet zowel een blauwdruk van het certificaat en de certificaatgegevens bevatten.

##### <a name="upload-the-web-app-certificate-to-the-virtual-network"></a>Het web app certificaat uploaden naar de virtueel netwerk #####

Een handboek, eenmalige stap is vereist voor elke combinatie van de virtuele netwerk en abonnement. Dat wil zeggen, als u apps in een abonnement op een virtueel netwerk verbindt, moet u doen deze stap slechts eenmaal ongeacht hoeveel apps u configureren. Als u een nieuwe toepassing naar een ander virtueel netwerk toevoegt, moet u dit opnieuw doen. De reden hiervoor is dat een set certificaten bij een abonnement op in Azure App-Service wordt gegenereerd en de set eenmaal is gegenereerd voor elke virtuele netwerk waarmee u verbinding met de apps.

De certificaten worden al zijn ingesteld als u deze stappen hebt uitgevoerd of als u geïntegreerd met hetzelfde virtuele netwerk met behulp van de portal.

De eerste stap is het cer-bestand genereren. De tweede stap is het cer-bestand uploaden naar uw virtuele netwerk. Voer de volgende opdrachten voor informatie over het genereren van het cer-bestand van de API-aanroep in de vorige stap.

    $certBytes = [System.Convert]::FromBase64String($vnet.Properties.certBlob)
    [System.IO.File]::WriteAllBytes("$($Configuration.GeneratedCertificatePath)", $certBytes)

Het certificaat wordt gevonden op de locatie die **$Configuration.GeneratedCertificatePath** bevat.

Het certificaat handmatig uploaden, gebruiken de [Azure portal] [ azureportal] en **Blader virtueel netwerk (klassiek)** > **VPN-verbindingen** > **punt-naar-site** > **certificaten beheren**. Upload hier uw certificaat.

##### <a name="get-the-point-to-site-package"></a>Het pakket punt-naar-site ophalen #####

De volgende stap bij het instellen van een virtueel netwerkverbinding op een web app is aan het pakket punt-naar-site ophalen en doorgeven aan uw web app.

De volgende sjabloon opslaan in een bestand met de naam GetNetworkPackageUri.json ergens op uw computer, bijvoorbeeld C:\Azure\Templates\GetNetworkPackageUri.json.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "certData": {
                "type": "string"
            },
            "certThumbprint": {
                "type": "string"
            },
            "networkName": {
                "type": "string"
            }
        },
        "variables": {
            "legacyVnetName": "[concat('Group ', resourceGroup().name, ' ', parameters('networkName'))]"
            },
            "resources": [
            ],
        "outputs" : {
            "PackageUri" :
            {
            "value" : "[listPackage(resourceId('Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates', parameters('networkName'), 'primary', parameters('certThumbprint')), '2014-06-01').packageUri]", "type" : "string"
            }
        }
    }


Stel de invoerparameters:

    $parameters = @{"certData" = $vnet.Properties.certBlob ;
    certThumbprint = $vnet.Properties.certThumbprint ;
    "networkName" = $Configuration.VnetName }

Bel het script:

    $output = New-AzureRmResourceGroupDeployment -Name unused -ResourceGroupName $Configuration.VnetResourceGroup -TemplateParameterObject $parameters -TemplateFile C:\PATH\TO\GetNetworkPackageUri.json


De variabele **$output. Outputs.packageUri** bevat nu het pakket URI voor uw web app.

##### <a name="upload-the-point-to-site-package-to-your-app"></a>Het pakket punt-naar-site uploaden naar uw app #####

De laatste stap is de app voorzien van dit pakket. Gewoon uitvoeren de volgende opdracht:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)/primary" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-07-01 -PropertyObject @{"VnetName" = $Configuration.VnetName ; "VpnPackageUri" = $($output.Outputs.packageUri).Value } -Location $Configuration.WebAppLocation

Als een bericht wordt u gevraagd te bevestigen dat u een bestaande resource wilt overschrijven, moet u te laten.

Nadat deze opdracht is geslaagd, moet uw app nu met het virtuele netwerk worden verbonden. Succes te bevestigen, Ga naar uw console app en typt u het volgende:

    SET WEBSITE_

Als u een omgevingsvariabele met de naam WEBSITE_VNETNAME met een waarde die overeenkomt met de naam van het virtuele netwerk, zijn alle configuraties geslaagd.

### <a name="update-classic-vnet-integration-information"></a>Klassieke VNet integratie informatie bijwerken ###

Om te werken of uw gegevens synchroniseren, gewoon de stappen die u hebt gevolgd bij het in de eerste plaats maken van de integratie. Deze stappen zijn:

1. De configuratie-informatie definiëren.
1. Het virtuele netwerk App declareren.
1. Het pakket punt-naar-site ophalen.
1. Het pakket punt-naar-site uploaden naar uw app.

### <a name="disconnect-your-app-from-a-classic-vnet"></a>Een klassieke VNet van uw app verbreken ###

Als u wilt de app verbreken, moet u de configuratie-informatie die tijdens de integratie van virtueel netwerk is ingesteld. Met deze informatie, is er vervolgens een opdracht uw app verbreken in het virtuele netwerk.

    $vnet = Remove-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-07-01

## <a name="resource-manager-virtual-networks"></a>Virtuele netwerken Resource Manager ##

Resource Manager virtuele netwerken hebben Azure Resource Manager API's, waarvan sommige processen in vergelijking met klassieke virtuele netwerken vereenvoudigen. We hebben een script waarmee u de volgende taken uitvoeren:

- Een virtueel netwerk Resource Manager maken en uw app integreren.
- Een gateway maken en vervolgens uw app te integreren met het punt-naar-site verbinding in een virtueel netwerk van bestaande Resource Manager configureren.
- Uw app integreren met bestaande Resource Manager virtueel netwerk met een gateway en punt-naar-site verbinding ingeschakeld.
- Uw app uw virtuele netwerk verbreken.

### <a name="resource-manager-vnet-app-service-integration-script"></a>Integratie-script Resource Manager VNet App-Service ###

Kopieer het volgende script en sla het op in een bestand. Als u niet dat het script gebruiken wilt, gerust meer te zien hoe dingen instellen met een virtueel netwerk Resource Manager.


    function ReadHostWithDefault($message, $default)
    {
        $result = Read-Host "$message [$default]"
        if($result -eq "")
        {
            $result = $default
        }
            return $result
        }

    function PromptCustom($title, $optionValues, $optionDescriptions)
    {
        Write-Host $title
        Write-Host
        $a = @()
        for($i = 0; $i -lt $optionValues.Length; $i++){
            Write-Host "$($i+1))" $optionDescriptions[$i]
        }
        Write-Host

        while($true)
        {
            Write-Host "Choose an option: "
            $option = Read-Host
            $option = $option -as [int]

            if($option -ge 1 -and $option -le $optionValues.Length)
            {
                return $optionValues[$option-1]
            }
        }
    }

    function PromptYesNo($title, $message, $default = 0)
    {
        $yes = New-Object System.Management.Automation.Host.ChoiceDescription "&Yes", ""
        $no = New-Object System.Management.Automation.Host.ChoiceDescription "&No", ""
        $options = [System.Management.Automation.Host.ChoiceDescription[]]($yes, $no)
        $result = $host.ui.PromptForChoice($title, $message, $options, $default)
        return $result
    }

    function CreateVnet($resourceGroupName, $vnetName, $vnetAddressSpace, $vnetGatewayAddressSpace, $location)
    {
        Write-Host "Creating a new VNET"
        $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
        New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix $vnetAddressSpace -Subnet $gatewaySubnet
    }

    function CreateVnetGateway($resourceGroupName, $vnetName, $vnetIpName, $location, $vnetIpConfigName, $vnetGatewayName, $certificateData, $vnetPointToSiteAddressSpace)
    {
        $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName
        $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

        Write-Host "Creating a public IP address for this VNET"
        $pip = New-AzureRmPublicIpAddress -Name $vnetIpName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $vnetIpConfigName -Subnet $subnet -PublicIpAddress $pip

        Write-Host "Adding a root certificate to this VNET"
        $root = New-AzureRmVpnClientRootCertificate -Name "AppServiceCertificate.cer" -PublicCertData $certificateData

        Write-Host "Creating Azure VNET Gateway. This may take up to an hour."
        New-AzureRmVirtualNetworkGateway -Name $vnetGatewayName -ResourceGroupName $resourceGroupName -Location $location -IpConfigurations $ipconf -GatewayType Vpn -VpnType RouteBased -EnableBgp $false -GatewaySku Basic -VpnClientAddressPool $vnetPointToSiteAddressSpace -VpnClientRootCertificates $root
    }

    function AddNewVnet($subscriptionId, $webAppResourceGroup, $webAppName)
    {
        Write-Host "Adding a new Vnet"
        Write-Host
        $vnetName = Read-Host "Specify a name for this Virtual Network"

        $vnetGatewayName="$($vnetName)-gateway"
        $vnetIpName="$($vnetName)-ip"
        $vnetIpConfigName="$($vnetName)-ipconf"

        # Virtual Network settings
        $vnetAddressSpace="10.0.0.0/8"
        $vnetGatewayAddressSpace="10.5.0.0/16"
        $vnetPointToSiteAddressSpace="172.16.0.0/16"

        $changeRequested = 0
        $resourceGroupName = $webAppResourceGroup

        while($changeRequested -eq 0)
        {
            Write-Host
            Write-Host "Currently, I will create a VNET with the following settings:"
            Write-Host
            Write-Host "Virtual Network Name: $vnetName"
            Write-Host "Resource Group Name:  $resourceGroupName"
            Write-Host "Gateway Name: $vnetGatewayName"
            Write-Host "Vnet IP name: $vnetIpName"
            Write-Host "Vnet IP config name:  $vnetIpConfigName"
            Write-Host "Address Space:$vnetAddressSpace"
            Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
            Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
            Write-Host
            $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

            if($changeRequested -eq 0)
            {
                $vnetName = ReadHostWithDefault "Virtual Network Name" $vnetName
                $resourceGroupName = ReadHostWithDefault "Resource Group Name" $resourceGroupName
                $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                $vnetAddressSpace = ReadHostWithDefault "Vnet Address Space" $vnetAddressSpace
                $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
            }
        }

        $ErrorActionPreference = "Stop";

        # We create the virtual network and add it here. The way this works is:
        # 1) Add the VNET association to the App. This allows the App to generate certificates, etc. for the VNET.
        # 2) Create the VNET and VNET gateway, add the certificates, create the public IP, etc., required for the gateway
        # 3) Get the VPN package from the gateway and pass it back to the App.

        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup
        $location = $webApp.Location

        Write-Host "Creating App association to VNET"
        $propertiesObject = @{
         "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($resourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
        }
        $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        CreateVnet $resourceGroupName $vnetName $vnetAddressSpace $vnetGatewayAddressSpace $location

        CreateVnetGateway $resourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName -VirtualNetworkGatewayName $vnetGatewayName -ProcessorArchitecture Amd64

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $VirtualNetworkName; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        Write-Host "Finished!"
    }

    function AddExistingVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $ErrorActionPreference = "Stop";

        # At this point, the gateway should be able to be joined to an App, but may require some minor tweaking. We will declare to the App now to use this VNET
        Write-Host "Getting App information"
        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $location = $webApp.Location

        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"
        }

        # Display existing vnets
        $vnets = Get-AzureRmVirtualNetwork
        $vnetNames = @()
        foreach($vnet in $vnets)
        {
            $vnetNames += $vnet.Name
        }

        Write-Host
        $vnet = PromptCustom "Select a VNET to integrate with" $vnets $vnetNames

        # We need to check if this VNET is able to be joined to a App, based on following criteria
            # If there is no gateway, we can create one.
            # If there is a gateway:
                # It must be of type Vpn
                # It must be of VpnType RouteBased
                # If it doesn't have the right certificate, we will need to add it.
                # If it doesn't have a point-to-site range, we will need to add it.

        $gatewaySubnet = $vnet.Subnets | Where-Object { $_.Name -eq "GatewaySubnet" }

        if($gatewaySubnet -eq $null -or $gatewaySubnet.IpConfigurations -eq $null -or $gatewaySubnet.IpConfigurations.Count -eq 0)
        {
            $ErrorActionPreference = "Continue";
            # There is no gateway. We need to create one.
            Write-Host "This Virtual Network has no gateway. I will need to create one."

            $vnetName = $vnet.Name
            $vnetGatewayName="$($vnetName)-gateway"
            $vnetIpName="$($vnetName)-ip"
            $vnetIpConfigName="$($vnetName)-ipconf"

            # Virtual Network settings
            $vnetAddressSpace="10.0.0.0/8"
            $vnetGatewayAddressSpace="10.5.0.0/16"
            $vnetPointToSiteAddressSpace="172.16.0.0/16"

            $changeRequested = 0

            Write-Host "Your VNET is in the address space $($vnet.AddressSpace.AddressPrefixes), with the following Subnets:"
            foreach($subnet in $vnet.Subnets)
            {
                Write-Host "$($subnet.Name): $($subnet.AddressPrefix)"
            }

            $vnetGatewayAddressSpace = Read-Host "Please choose a GatewaySubnet address space"

            while($changeRequested -eq 0)
            {
                Write-Host
                Write-Host "Currently, I will create a VNET gateway with the following settings:"
                Write-Host
                Write-Host "Virtual Network Name: $vnetName"
                Write-Host "Resource Group Name:  $($vnet.ResourceGroupName)"
                Write-Host "Gateway Name: $vnetGatewayName"
                Write-Host "Vnet IP name: $vnetIpName"
                Write-Host "Vnet IP config name:  $vnetIpConfigName"
                Write-Host "Address Space:$($vnet.AddressSpace.AddressPrefixes)"
                Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
                Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
                Write-Host
                $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

                if($changeRequested -eq 0)
                {
                    $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                    $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                    $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                    $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                    $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
                }
            }

            $ErrorActionPreference = "Stop";

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # If there is no gateway subnet, we need to create one.
            if($gatewaySubnet -eq $null)
            {
                $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
                $vnet.Subnets.Add($gatewaySubnet);
                Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
            }

            CreateVnetGateway $vnet.ResourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $vnetGatewayName
        }
        else
        {
            $uriParts = $gatewaySubnet.IpConfigurations[0].Id.Split('/')
            $gatewayResourceGroup = $uriParts[4]
            $gatewayName = $uriParts[8]

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $gatewayName

            # validate gateway types, etc.
            if($gateway.GatewayType -ne "Vpn")
            {
                Write-Error "This gateway is not of the Vpn type. It cannot be joined to an App."
                return
            }

            if($gateway.VpnType -ne "RouteBased")
            {
                Write-Error "This gateways Vpn type is not RouteBased. It cannot be joined to an App."
                return
            }

            if($gateway.VpnClientConfiguration -eq $null -or $gateway.VpnClientConfiguration.VpnClientAddressPool -eq $null)
            {
                Write-Host "This gateway does not have a Point-to-site Address Range. Please specify one in CIDR notation, e.g. 10.0.0.0/8"
                $pointToSiteAddress = Read-Host "Point-To-Site Address Space"
                Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $gateway.Name -VpnClientAddressPool $pointToSiteAddress
            }

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnet.Name)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # We need to check if the certificate here exists in the gateway.
            $certificates = $gateway.VpnClientConfiguration.VpnClientRootCertificates

            $certFound = $false
            foreach($certificate in $certificates)
            {
                if($certificate.PublicCertData -eq $virtualNetwork.Properties.CertBlob)
                {
                    $certFound = $true
                    break
                }
            }

            if(-not $certFound)
            {
                Write-Host "Adding certificate"
                Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName "AppServiceCertificate.cer" -PublicCertData $virtualNetwork.Properties.CertBlob -VirtualNetworkGatewayName $gateway.Name
            }
        }

        # Now finish joining by getting the VPN package and giving it to the App
        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $vnet.ResourceGroupName -VirtualNetworkGatewayName $gateway.Name -ProcessorArchitecture Amd64

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $vnet.Name; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

        Write-Host "Finished!"
    }

    function RemoveVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"

            Remove-AzureRmResource -ResourceName "$($webAppName)/$($currentVnet)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        }
          else
        {
          Write-Host "Not connected to a VNET."
        }
    }

    Write-Host "Please Login"
    Login-AzureRmAccount

    # Choose subscription. If there's only one we will choose automatically

    $subs = Get-AzureRmSubscription
    $subscriptionId = ""

    if($subs.Length -eq 0)
    {
        Write-Error "No subscriptions bound to this account."
        return
    }

    if($subs.Length -eq 1)
    {
        $subscriptionId = $subs[0].SubscriptionId
    }
    else
    {
        $subscriptionChoices = @()
        $subscriptionValues = @()

        foreach($subscription in $subs){
        $subscriptionChoices += "$($subscription.SubscriptionName) ($($subscription.SubscriptionId))";
        $subscriptionValues += ($subscription.SubscriptionId);
        }

        $subscriptionId = PromptCustom "Choose a subscription" $subscriptionValues $subscriptionChoices
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    $resourceGroup = Read-Host "Please enter the Resource Group of your App"

    $appName = Read-Host "Please enter the Name of your App"

    $options = @("Add a NEW Virtual Network to an App", "Add an EXISTING Virtual Network to an App", "Remove a Virtual Network from an App");
    $optionValues = @(0, 1, 2)
    $option = PromptCustom "What do you want to do?" $optionValues $options

    if($option -eq 0)
    {
        AddNewVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 1)
    {
        AddExistingVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 2)
    {
        RemoveVnet $subscriptionId $resourceGroup $appName
    }

Sla een kopie van het script. In dit artikel wordt V2VnetAllinOne.ps1 genoemd, maar kunt u een andere naam. Er zijn geen argumenten voor dit script. U kunt uitvoeren. Het eerste wat dat het script doet is gevraagd u aan te melden. Nadat u zich hebt aangemeld, wordt het script haalt informatie over uw account en geeft als resultaat een lijst met abonnementen. Het verzoek om uw referenties niet meegerekend, de uitvoering van het eerste script zien er zo uit:

    PS C:\Users\ccompy\Documents\VNET> .\V2VnetAllInOne.ps1
    Please Login

    Environment           : AzureCloud
    Account               : ccompy@microsoft.com
    TenantId              : 722278f-fef1-499f-91ab-2323d011db47
    SubscriptionId        : af5358e1-acac-2c90-a9eb-722190abf47a
    CurrentStorageAccount :

    Choose a subscription

    1) Demo-abonnement (af5358e1-acac-2c90-a9eb-722190abf47a)
    2) MS-Test (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)
    3) Paarse Demo-abonnement (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)

    Kies een optie: 3

    Rekening: ccompy@microsoft.com omgeving: abonnement AzureCloud: 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d huurder: 722278f-fef1-499f-91ab-2323d011db47

    Geef de resourcegroep van uw App: Voer de naam van uw App hcdemo rg: v2vnetpowershell wat wilt u doen?

    1) Een nieuw virtueel netwerk naar een toepassing toevoegen
    2) Een bestaande virtuele netwerken toevoegen aan een App
    3) Een virtueel netwerk verwijderen uit een App

De rest van deze sectie wordt elk van deze drie opties beschreven.

### <a name="create-a-resource-manager-vnet-and-integrate-with-it"></a>Een Resource Manager VNet maken en integreren met het ###

U maakt een nieuwe virtuele netwerk dat gebruikmaakt van het implementatiemodel Resource Manager en integreren met uw app, selecteer **1) een nieuw virtueel netwerk toevoegen aan een App**. Hiermee wordt u gevraagd om de naam van het virtuele netwerk. In mijn geval, zoals u in de volgende instellingen zien kunt gebruikt ik de naam van de v2pshell.

Het script geeft de details van het virtuele netwerk dat wordt gemaakt. Als ik wil, kan ik de waarden wijzigen. In dit voorbeeld wordt uitgevoerd, maar ik heb gemaakt een virtueel netwerk met de volgende instellingen:

    Virtual Network Name:         v2pshell
    Resource Group Name:          hcdemo-rg
    Gateway Name:                 v2pshell-gateway
    Vnet IP name:                 v2pshell-ip
    Vnet IP config name:          v2pshell-ipconf
    Address Space:                10.0.0.0/8
    Gateway Address Space:        10.5.0.0/16
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):

Als u wilt dat de waarden te wijzigen, typt u **j** en breng de wijzigingen. Wanneer u tevreden over de netwerkinstellingen van het virtuele bent, typt u **N** of drukt u op Enter wanneer u wordt gevraagd de instellingen te wijzigen. Vanaf dat moment tot voltooiing, het script ziet u enkele van dit ' i's doet tot het begint te maken van de gateway virtueel netwerk. Deze stap kan tot een uur duren. Er is geen voortgangsindicator tijdens deze fase, maar het script laten u weten wanneer de gateway is gemaakt.

Wanneer het script is voltooid, wordt er **Gereedgemeld**. Op dit punt hebt u een Resource Manager virtuele netwerk met de naam en de instellingen die u hebt geselecteerd. Dit nieuwe virtuele netwerk wordt ook geïntegreerd met uw app.

### <a name="integrate-your-app-with-a-preexisting-resource-manager-vnet"></a>Uw app integreren met een bestaande Resource Manager-VNet ###

Als u met een bestaande virtuele netwerk integreren wilt als u een Resource Manager virtueel netwerk dat geen gateway of punt-naar-site verbinding, stelt het script dat. Als de VNET al die dingen instellen, gaat het script rechtstreeks naar de app-integratie. Om te beginnen, selecteert u de **2) een bestaande virtuele netwerken toevoegen aan een App**.

Deze optie werkt alleen als u een bestaande Resource Manager virtuele netwerk in het abonnement hetzelfde als uw app. Nadat u de optie selecteert, wordt weergegeven met een lijst van uw virtuele netwerken Resource Manager.   

    Select a VNET to integrate with

    1) v2demonetwork
    2) v2pshell
    3) v2vnetintdemo
    4) v2asenetwork
    5) v2pshell2

    Kies een optie: 5

Selecteer gewoon het virtuele netwerk waarmee u wilt integreren. Hebt u al een gateway met punt-naar-site verbinding is ingeschakeld, wordt het script uw app gewoon integreren met het virtuele netwerk. Als u een gateway niet hebt, moet u het subnet gateway opgeven. Uw subnet gateway in uw netwerk virtuele adresruimte moet worden en kan niet in een ander subnet. Als u een virtueel netwerk zonder een gateway hebt en deze stap uitvoert, dingen ziet er zo uit:

    This Virtual Network has no gateway. I will need to create one.
    Your VNET is in the address space 172.16.0.0/16, with the following Subnets:
    default: 172.16.0.0/24
    Please choose a GatewaySubnet address space: 172.16.1.0/26

In dit voorbeeld, ik heb gemaakt een virtueel netwerkgateway met de volgende instellingen:

    Virtual Network Name:         v2pshell2
    Resource Group Name:          vnetdemo-rg
    Gateway Name:                 v2pshell2-gateway
    Vnet IP name:                 v2pshell2-ip
    Vnet IP config name:          v2pshell2-ipconf
    Address Space:                172.16.0.0/16
    Gateway Address Space:        172.16.1.0/26
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):
    Creating App association to VNET

Als u wilt dat deze instellingen te wijzigen, kunt u dat doen. Anders is, drukt u op Enter en het script zal uw gateway maken en uw app koppelen aan het virtuele netwerk. Het tijdstip van de gateway is echter nog steeds een uur, dus zorg ervoor dat u dat in gedachten houdt. Als alles klaar is, wordt het script **voltooid**zeg.

### <a name="disconnect-your-app-from-a-resource-manager-vnet"></a>Uw app verbreken door een Resource Manager-VNet ###

Het virtuele netwerk van uw app verbreekt niet noteren de gateway of punt-naar-site verbinding uitschakelen. Mogelijk, na alle, gebruikt u het voor iets anders. Het wordt ook niet verbroken het vanuit een andere apps dan degene die u hebt opgegeven. Als u deze actie wilt uitvoeren, selecteert u **3) een virtueel netwerk verwijderen uit een App**. Als u dit doet, worden er ongeveer zo uitziet:

    Currently connected to VNET v2pshell

    Confirm
    Are you sure you want to delete the following resource:
    /subscriptions/edcc99a4-b7f9-4b5e-a9a1-3034c51db496/resourceGroups/hcdemo-rg/providers/Microsoft.Web/sites/v2vnetpowers
    hell/virtualNetworkConnections/v2pshell
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Hoewel in het script staat verwijderen, verwijdert het virtuele netwerk niet meer. Het verwijdert alleen de integratie. Nadat u hebt bevestigd dat dit is wat u wilt doen, is de opdracht vrij snel wordt verwerkt en wordt aangegeven **waar** als dit gebeurt.

<!--Links-->
[createvpngateway]: http://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/
[azureportal]: http://portal.azure.com
