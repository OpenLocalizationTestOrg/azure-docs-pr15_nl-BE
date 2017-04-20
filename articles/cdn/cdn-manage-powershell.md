<properties
    pageTitle="Azure CDN met PowerShell beheren | Microsoft Azure"
    description="Informatie over het gebruik van de Azure PowerShell-cmdlets voor het beheren van Azure CDN."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="casoper"/>


# <a name="manage-azure-cdn-with-powershell"></a>Azure CDN met PowerShell beheren

PowerShell is een van de meest flexibele methoden voor het beheren van uw Azure CDN profielen en eindpunten.  U kunt PowerShell interactief of door scripts te schrijven om beheertaken te automatiseren.  Deze zelfstudie toont enkele van de meest voorkomende taken die u kunt uitvoeren met PowerShell om uw Azure CDN profielen en eindpunten te beheren.

## <a name="prerequisites"></a>Vereisten

Gebruik PowerShell om uw Azure CDN profielen en eindpunten te beheren, hebt u de module Azure PowerShell is geïnstalleerd.  Voor meer informatie over Azure PowerShell installeren en verbinden met Azure via de `Login-AzureRmAccount` cmdlet, Zie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

>[AZURE.IMPORTANT] U moet zich aanmelden met `Login-AzureRmAccount` voordat u Azure PowerShell-cmdlets kunt uitvoeren.

## <a name="listing-the-azure-cdn-cmdlets"></a>De cmdlets Azure CDN aanbieding

Kunt u vermelden alle de Azure CDN-cmdlets met behulp van de `Get-Command` cmdlet.

```text
PS C:\> Get-Command -Module AzureRM.Cdn

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Get-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpointNameAvailability             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfileSsoUrl                        2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Publish-AzureRmCdnEndpointContent                  2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnCustomDomain                      2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnEndpoint                          2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnProfile                           2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Start-AzureRmCdnEndpoint                           2.0.0      AzureRm.Cdn
Cmdlet          Stop-AzureRmCdnEndpoint                            2.0.0      AzureRm.Cdn
Cmdlet          Test-AzureRmCdnCustomDomain                        2.0.0      AzureRm.Cdn
Cmdlet          Unpublish-AzureRmCdnEndpointContent                2.0.0      AzureRm.Cdn
```

## <a name="getting-help"></a>Help-informatie opvragen

U kunt hulp krijgen met een van deze cmdlets met de `Get-Help` cmdlet.  `Get-Help`Gebruik en syntaxis biedt en eventueel staan voorbeelden.

```text
PS C:\> Get-Help Get-AzureRmCdnProfile

NAME
    Get-AzureRmCdnProfile

SYNOPSIS
    Gets an Azure CDN profile.


SYNTAX
    Get-AzureRmCdnProfile [-ProfileName <String>] [-ResourceGroupName <String>] [-InformationAction
    <ActionPreference>] [-InformationVariable <String>] [<CommonParameters>]


DESCRIPTION
    Gets an Azure CDN profile and all related information.


RELATED LINKS

REMARKS
    To see the examples, type: "get-help Get-AzureRmCdnProfile -examples".
    For more information, type: "get-help Get-AzureRmCdnProfile -detailed".
    For technical information, type: "get-help Get-AzureRmCdnProfile -full".

```

## <a name="listing-existing-azure-cdn-profiles"></a>Aanbieding bestaande Azure CDN profielen

De `Get-AzureRmCdnProfile` cmdlet zonder parameters haalt uw bestaande CDN profielen.

```powershell
Get-AzureRmCdnProfile
```

Deze uitvoer kan worden doorgesluisd naar cmdlets voor opsomming.

```powershell
# Output the name of all profiles on this subscription.
Get-AzureRmCdnProfile | ForEach-Object { Write-Host $_.Name }

# Return only **Azure CDN from Verizon** profiles.
Get-AzureRmCdnProfile | Where-Object { $_.Sku.Name -eq "StandardVerizon" }
```

U kunt ook één profiel terugkeren door op te geven van de groep en de bron.

```powershell
Get-AzureRmCdnProfile -ProfileName CdnDemo -ResourceGroupName CdnDemoRG
```

>[AZURE.TIP] Het is mogelijk om meerdere CDN profielen met dezelfde naam, zolang deze zich in de verschillende resourcegroepen.  Zonder de `ResourceGroupName` parameter retourneert alle profielen met een overeenkomende naam.

## <a name="listing-existing-cdn-endpoints"></a>Aanbieding bestaande CDN eindpunten

`Get-AzureRmCdnEndpoint`een afzonderlijke eindpunt of de eindpunten op een profiel kunnen worden opgehaald.  

```powershell
# Get a single endpoint.
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Get all of the endpoints on a given profile. 
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG

# Return all of the endpoints on all of the profiles.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint

# Return all of the endpoints in this subscription that are currently running.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Where-Object { $_.ResourceState -eq "Running" }
```

## <a name="creating-cdn-profiles-and-endpoints"></a>Maken van CDN profielen en eindpunten

`New-AzureRmCdnProfile`en `New-AzureRmCdnEndpoint` worden gebruikt om profielen CDN en eindpunten te maken.

```powershell
# Create a new profile
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US"

# Create a new endpoint
New-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Location "Central US" -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

# Create a new profile and endpoint (same as above) in one line
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US" | New-AzureRmCdnEndpoint -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

```

## <a name="checking-endpoint-name-availability"></a>Eindpunt naam beschikbaarheid controleren

`Get-AzureRmCdnEndpointNameAvailability`Geeft als resultaat een object dat aangeeft of de naam van een beschikbaar is.

```powershell
# Retrieve availability
$availability = Get-AzureRmCdnEndpointNameAvailability -EndpointName "cdnposhdoc"

# If available, write a message to the console.
If($availability.NameAvailable) { Write-Host "Yes, that endpoint name is available." }
Else { Write-Host "No, that endpoint name is not available." }
```

## <a name="adding-a-custom-domain"></a>Een aangepaste domein toevoegen

`New-AzureRmCdnCustomDomain`een aangepaste domeinnaam toegevoegd aan een bestaand eindpunt.

>[AZURE.IMPORTANT] U moet de CNAME met uw DNS-provider, zoals beschreven in [het toewijzen van aangepaste domein Content Delivery Network (CDN) eindpunt](./cdn-map-content-to-custom-domain.md)instellen.  U kunt testen voordat u uw eindpunt met behulp van de toewijzing van `Test-AzureRmCdnCustomDomain`.

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Check the mapping
$result = Test-AzureRmCdnCustomDomain -CdnEndpoint $endpoint -CustomDomainHostName "cdn.contoso.com"

# Create the custom domain on the endpoint
If($result.CustomDomainValidated){ New-AzureRmCdnCustomDomain -CustomDomainName Contoso -HostName "cdn.contoso.com" -CdnEndpoint $endpoint }
```

## <a name="modifying-an-endpoint"></a>Wijzigen van een eindpunt

`Set-AzureRmCdnEndpoint`Hiermee wijzigt u een bestaand eindpunt.

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Set up content compression
$endpoint.IsCompressionEnabled = $true
$endpoint.ContentTypesToCompress = "text/javascript","text/css","application/json"

# Save the changed endpoint and apply the changes
Set-AzureRmCdnEndpoint -CdnEndpoint $endpoint
```

## <a name="purgingpre-loading-cdn-assets"></a>Verwijderen/Pre-loading CDN activa

`Unpublish-AzureRmCdnEndpointContent`worden in de cache opgeslagen activa, terwijl `Publish-AzureRmCdnEndpointContent` activa op ondersteunde eindpunten vooraf geladen.

```powershell
# Purge some assets.
Unpublish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -PurgeContent "/images/kitten.png","/video/rickroll.mp4"

# Pre-load some assets.
Publish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -LoadContent "/images/kitten.png","/video/rickroll.mp4"

# Purge everything in /images/ on all endpoints.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Unpublish-AzureRmCdnEndpointContent -PurgeContent "/images/*"
```

## <a name="startingstopping-cdn-endpoints"></a>Starten/stoppen CDN eindpunten
`Start-AzureRmCdnEndpoint`en `Stop-AzureRmCdnEndpoint` kan worden gebruikt voor het starten en stoppen van afzonderlijke eindpunten of groepen van eindpunten.

```powershell
# Stop the cdndocdemo endpoint
Stop-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Stop all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Stop-AzureRmCdnEndpoint

# Start all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Start-AzureRmCdnEndpoint
```

## <a name="deleting-cdn-resources"></a>CDN bronnen verwijderen

`Remove-AzureRmCdnProfile`en `Remove-AzureRmCdnEndpoint` kan worden gebruikt om profielen en eindpunten te verwijderen.

```powershell
# Remove a single endpoint
Remove-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Remove all the endpoints on a profile and skip confirmation (-Force)
Get-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG | Get-AzureRmCdnEndpoint | Remove-AzureRmCdnEndpoint -Force

# Remove a single profile
Remove-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG
```

## <a name="next-steps"></a>Volgende stappen

Informatie over het automatiseren van Azure CDN met [.NET](./cdn-app-dev-net.md) of [Node.js](./cdn-app-dev-node.md).

Zie meer informatie over functies van CDN, [CDN-overzicht](./cdn-overview.md).


