<properties
    pageTitle="Azure Resource Manager ondersteuning voor verkeer Manager | Microsoft Azure "
    description="Met behulp van PowerShell voor verkeer Manager met Azure Resource Manager"
    services="traffic-manager"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="azure-resource-manager-support-for-azure-traffic-manager"></a>Azure Resource Manager ondersteuning voor Azure verkeer Manager

Azure Resource Manager is de beheerinterface van voorkeur voor services in Azure. Azure verkeer profielen kunnen worden beheerd met behulp van bronbeheer Azure-API's en hulpprogramma's.

## <a name="resource-model"></a>Model bron

Azure verkeer Manager is geconfigureerd met behulp van een verzameling instellingen voor een serviceprofiel verkeer genoemd. Dit profiel bevat de DNS-instellingen, verkeer routeringsinstellingen eindpunt controle-instellingen en een lijst met eindpunten die verkeer wordt omgeleid.

Elk serviceprofiel verkeer wordt vertegenwoordigd door een bron van het type 'TrafficManagerProfiles'. Op het niveau van de REST-API wordt de URI voor elk profiel als volgt:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## <a name="comparison-with-the-azure-traffic-manager-classic-api"></a>Vergelijking met de klassieke Azure verkeer beheer-API

De bronnenbeheerder Azure ondersteuning voor verkeer Manager gebruikt een andere terminologie dan de klassieke implementatiemodel. De volgende tabel ziet u de verschillen tussen de Resource Manager en klassieke voorwaarden:

| De term Resource Manager | Klassieke term |
|-----------------------|--------------|
| Methode voor routering van verkeer | Load balancing-methode |
| Prioriteit, methode | Failover-methode |
| Gewogen, methode | Round-robin methode |
| Prestaties, methode | Prestaties, methode |

Op basis van feedback van klanten, wij de terminologie om de duidelijkheid en minder voorkomende misverstanden gewijzigd. Er is geen verschil in functionaliteit.

## <a name="limitations"></a>Beperkingen

Wanneer u verwijst naar een eindpunt van het type 'AzureEndpoints' voor een Web App, verkeer Manager eindpunten kunnen alleen verwijzen naar de standaardinstelling (productie) [Web App-sleuf](../app-service-web/web-sites-staged-publishing.md). Aangepaste sleuven worden niet ondersteund. Als tijdelijke oplossing kunnen aangepaste sleuven worden geconfigureerd met behulp van het type 'ExternalEndpoints'.

## <a name="setting-up-azure-powershell"></a>Azure PowerShell instellen

Deze instructies Microsoft Azure PowerShell gebruiken. Het volgende artikel wordt uitgelegd hoe te installeren en configureren van Azure PowerShell.

- [Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md)

De voorbeelden in dit artikel wordt ervan uitgegaan dat u beschikt over een bestaande brongroep. Kunt u een resourcegroep met de volgende opdracht:

```powershell
    New-AzureRmResourceGroup -Name MyRG -Location "West US"
```

>[AZURE.NOTE] Azure Resource Manager vereist dat alle bronnengroepen van een locatie. Deze locatie wordt gebruikt als standaard voor resources in die groep hebt gemaakt. Echter omdat verkeer Manager profiel resources algemene, niet-regionale worden, heeft de keuze van de locatie van de resource geen invloed op Azure verkeer Manager.

## <a name="create-a-traffic-manager-profile"></a>Maak een profiel verkeer Manager

Maakt u een serviceprofiel verkeer met de cmdlet New-AzureRmTrafficManagerProfile:

```powershell
    $profile = New-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
```

De volgende tabel worden de parameters beschreven:

| Parameter | Beschrijving |
|-----------|-------------|
| Naam | De naam van de resource voor de resource Manager verkeer profiel. Profielen in dezelfde bronnengroep moeten unieke namen hebben. Deze naam staat los van de DNS-naam die wordt gebruikt voor DNS-query's.|
| ResourceGroupName | De naam van de bronnengroep met de bron van het profiel.|
| TrafficRoutingMethod | Hiermee geeft u de routering van verkeer methode gebruikt om te bepalen welk endpoint wordt geretourneerd als antwoord een DNS-query. Mogelijke waarden zijn 'Prestaties', 'Gewogen' of 'Priority'.|
| RelativeDnsName | Hiermee geeft u het gedeelte van de hostnaam van de DNS-naam die door dit verkeer Manager profiel. Deze waarde wordt gecombineerd met de DNS-domeinnaam door Azure verkeer Manager gebruikt om de volledig gekwalificeerde domeinnaam (FQDN) van het profiel. De waarde van 'contoso' wordt bijvoorbeeld 'contoso.trafficmanager.net'.|
| TTL | Hiermee geeft u de DNS-Time-to-Live (TTL) in seconden. Deze TTL informeert de lokale DNS-resolvers en DNS-clients hoe lang om te antwoorden op DNS-cache voor dit verkeer Manager profiel.|
| MonitorProtocol | Hiermee geeft u het protocol te gebruiken voor het controleren van de gezondheid van het eindpunt. Mogelijke waarden zijn 'HTTP' en 'HTTPS'.|
| MonitorPort | Hiermee geeft u de TCP-poort gebruikt voor het bewaken van de gezondheid van het eindpunt.|
| MonitorPath | Hiermee geeft u het pad ten opzichte van de naam van het domein gebruikt om probe voor de gezondheid van het eindpunt.|

De cmdlet een serviceprofiel verkeer in Azure maakt en retourneert een corresponderende profiel met PowerShell. Het profiel bevat op dit moment geen eindpunten. Zie voor meer informatie over het toevoegen van eindpunten aan een serviceprofiel verkeer [Verkeer Manager eindpunten toe te voegen](#adding-traffic-manager-endpoints).

## <a name="get-a-traffic-manager-profile"></a>Een Manager verkeer profiel ophalen

Gebruik de cmdlet Get-AzureRmTrafficManagerProfle voor het ophalen van een bestaand object van verkeer Manager profiel:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
```

Deze cmdlet retourneert een profile-object Manager verkeer.

## <a name="update-a-traffic-manager-profile"></a>Een Manager verkeer profiel bijwerken

Wijzigen, profielen voor beheer van netwerkverkeer volgt een 3-stap-proces:

1. Het profiel met de Get-AzureRmTrafficManagerProfile ophalen of het profiel dat wordt geretourneerd door een nieuw AzureRmTrafficManagerProfile.
2. Het profiel wijzigen. U kunt toevoegen en verwijderen van eindpunten of eindpunt of profiel parameters wijzigen. Deze wijzigingen zijn off line bewerkingen. Wijzigt u alleen het lokale object in het geheugen die het profiel vertegenwoordigt.
3. De wijzigingen met de cmdlet Set-AzureRmTrafficManagerProfile.

Alle profieleigenschappen kunnen worden gewijzigd, met uitzondering van het profiel RelativeDnsName. Als u wilt wijzigen in de RelativeDnsName, moet u verwijderen profiel en een nieuw profiel met een nieuwe naam.

In het volgende voorbeeld wordt getoond hoe de TTL van het profiel wijzigen:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
    $profile.Ttl = 300
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

Er zijn drie soorten verkeer Manager eindpunten:

1. **Azure eindpunten** worden gehost in Azure services
2. **Externe eindpunten** worden gehost buiten Azure services
3. **Geneste eindpunten** worden gebruikt om geneste hiërarchieën van verkeer profielen. Geneste eindpunten kunnen geavanceerde routering van verkeer configuraties voor complexe toepassingen.

In alle drie gevallen kunnen de eindpunten op twee manieren worden toegevoegd:

1. Met behulp van een 3-stap proces dat eerder is beschreven. Het voordeel van deze methode is dat verschillende eindpunt wijzigingen kunnen worden aangebracht in een update.
2. Met behulp van de cmdlet New-AzureRmTrafficManagerEndpoint. Deze cmdlet wordt een eindpunt toegevoegd aan een bestaand profiel beheer van netwerkverkeer in één bewerking.

## <a name="adding-azure-endpoints"></a>Azure eindpunten toevoegen

Azure eindpunten verwijzen naar services die worden gehost in Azure. Drie soorten Azure eindpunten worden ondersteund:

1. Azure Web Apps
2. 'Klassieke' cloud-services (die kunnen bevatten een PaaS-service of IaaS virtuele machines)
3. Azure PublicIpAddress middelen (die kunnen worden aangesloten op een taakverdeling of een virtuele machine NIC). De PublicIpAddress moet een DNS-naam toegewezen aan de Manager in het verkeer worden gebruikt.

In elk geval:

- De service is opgegeven met de parameter 'targetResourceId' van de Add-AzureRmTrafficManagerEndpointConfig of een nieuw AzureRmTrafficManagerEndpoint.
- Het 'Doel' en 'EndpointLocation' worden geïmpliceerd door de TargetResourceId.
- Waarmee het "gewicht" is optioneel. Gewichten worden alleen gebruikt als het profiel is geconfigureerd voor het gebruik van de methode 'Gewogen'-verkeer routeren. Anders worden deze genegeerd. Als u opgeeft, is de waarde moet een getal tussen 1 en 1000. De standaardwaarde is '1'.
- Waarmee de 'prioriteit' is optioneel. Prioriteiten worden alleen gebruikt als het profiel is geconfigureerd voor het gebruik van de methode 'Priority'-verkeer routeren. Anders worden deze genegeerd. Geldige waarden zijn 1 tot en met 1000 met lagere waarden die een hogere prioriteit aangeeft. Als voor een eindpunt is opgegeven, moet het worden opgegeven voor alle eindpunten. Als dit argument wordt weggelaten, wordt standaardwaarden die beginnen met '1' worden toegepast in de volgorde waarin de eindpunten worden weergegeven.

### <a name="example-1-adding-web-app-endpoints-using-add-azurermtrafficmanagerendpointconfig"></a>Voorbeeld 1: Add-AzureRmTrafficManagerEndpointConfig via Web App-eindpunten toevoegen

In dit voorbeeld maakt u een profiel verkeer Manager we en twee Web App eindpunten met de cmdlet Add-AzureRmTrafficManagerEndpointConfig toevoegen.

```powershell
    $profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    $webapp1 = Get-AzureRMWebApp -Name webapp1
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
    $webapp2 = Get-AzureRMWebApp -Name webapp2
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-adding-a-classic-cloud-service-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Voorbeeld 2: Toevoegen van een 'klassieke' cloud-service-eindpunt met nieuw AzureRmTrafficManagerEndpoint

In dit voorbeeld wordt wordt een 'klassieke' Cloud-Service-eindpunt toegevoegd aan een serviceprofiel verkeer. In dit voorbeeld opgegeven we het profiel met behulp van het profiel en de resource namen in plaats van een profile-object wordt doorgegeven. Beide benaderingen worden ondersteund.

    $cloudService = Get-AzureRmResource -ResourceName MyCloudService -ResourceType "Microsoft.ClassicCompute/domainNames" -ResourceGroupName MyCloudService
    New-AzureRmTrafficManagerEndpoint -Name MyCloudServiceEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $cloudService.Id -EndpointStatus Enabled

### <a name="example-3-adding-a-publicipaddress-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Voorbeeld 3: Toevoegen van een nieuw AzureRmTrafficManagerEndpoint met publicIpAddress-eindpunt

In dit voorbeeld wordt een openbaar IP-adresbron toegevoegd aan het profiel van het beheer van verkeer. Het openbare IP-adres moet een DNS-naam die is geconfigureerd, en kan afhankelijk zijn van de NIC van een VM of een load balancer.

    $ip = Get-AzureRmPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
    New-AzureRmTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled

## <a name="adding-external-endpoints"></a>Externe eindpunten toevoegen

Externe eindpunten verkeer Manager gebruikt om verkeer naar services die worden gehost buiten Azure. Net als bij Azure eindpunten, kunnen externe eindpunten toevoegen met behulp van software-AzureRmTrafficManagerEndpointConfig, gevolgd door een Set AzureRmTrafficManagerProfile of een nieuw AzureRMTrafficManagerEndpoint.

Bij het externe eindpunten opgeven:

- De naam van het domein moet worden opgegeven met de parameter 'Target'
- Als de methode 'Prestaties' Routering van verkeer wordt gebruikt, is de 'EndpointLocation' is vereist. Anders is optioneel. De waarde moet een [geldige Azure regionaam](https://azure.microsoft.com/regions/).
- Het 'Gewicht' en 'Priority' zijn optioneel.

### <a name="example-1-adding-external-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Voorbeeld 1: Toevoegen van externe eindpunten toevoegen AzureRmTrafficManagerEndpointConfig en Set-AzureRmTrafficManagerProfile

In dit voorbeeld we verkeer Manager profiel maken, twee externe eindpunten toevoegen en leg de wijzigingen vast.

    $profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointStatus Enabled
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

### <a name="example-2-adding-external-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Voorbeeld 2: Toevoegen van externe eindpunten met nieuw AzureRmTrafficManagerEndpoint

In dit voorbeeld wordt toevoegen een externe eindpunt we aan een bestaand profiel. Het profiel is opgegeven met de namen van profielen en bron.

    New-AzureRmTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled

## <a name="adding-nested-endpoints"></a>'Nested' eindpunten toevoegen

Elk serviceprofiel verkeer geeft een enkele methode voor het routeren van verkeer. Er zijn echter scenario's waarvoor meer geavanceerde verkeersroutering dan het bewerkingsplan die door één profiel verkeer Manager. U kunt profielen wilt combineren de voordelen van meer dan één methode voor routering van verkeer verkeer Manager nesten. Geneste profielen kunnen u het standaardgedrag verkeer Manager met ondersteuning voor grotere en complexere implementaties van toepassing overschrijven. Zie voor meer gedetailleerde voorbeelden [genest verkeer profielen](traffic-manager-nested-profiles.md).

Geneste eindpunten zijn geconfigureerd op het bovenliggende profiel, met behulp van een bepaald eindpunt type, 'NestedEndpoints'. Geneste eindpunten opgeven:

- Het eindpunt moet worden opgegeven met de parameter 'targetResourceId'
- Als de methode 'Prestaties' Routering van verkeer wordt gebruikt, is de 'EndpointLocation' is vereist. Anders is optioneel. De waarde moet een [geldige Azure regionaam](http://azure.microsoft.com/regions/).
- Het 'Gewicht' en 'Priority' zijn optioneel, als voor Azure eindpunten.
- De parameter 'MinChildEndpoints' is optioneel. De standaardwaarde is '1'. Als het aantal beschikbare eindpunten kleiner is dan deze drempelwaarde, beschouwt het bovenliggende profiel het profiel van het kind 'Doorgaan' en verkeer naar de andere eindpunten in het bovenliggende profiel zorgt.

### <a name="example-1-adding-nested-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Voorbeeld 1: Toevoegen van geneste eindpunten toevoegen AzureRmTrafficManagerEndpointConfig en Set-AzureRmTrafficManagerProfile

In dit voorbeeld we nieuwe Manager verkeer onderliggende en bovenliggende profielen maken het kind als een geneste eindpunt toevoegen aan de bovenliggende en leg de wijzigingen vast.

    $child = New-AzureRmTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    $parent = New-AzureRmTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

Kort te houden in dit voorbeeld brachten wij geen andere eindpunten aan de onderliggende of bovenliggende profielen.

### <a name="example-2-adding-nested-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Voorbeeld 2: Toevoegen van geneste eindpunten met nieuw AzureRmTrafficManagerEndpoint

In dit voorbeeld toevoegen we een bestaand profiel kind als een geneste eindpunt aan een bestaande bovenliggende profiel. Het profiel is opgegeven met de namen van profielen en bron.

    $child = Get-AzureRmTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
    New-AzureRmTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2

## <a name="update-a-traffic-manager-endpoint"></a>Update een eindpunt van het beheer van verkeer

Er zijn twee manieren voor het bijwerken van een bestaand verkeer Manager eindpunt:

1. Krijgt het verkeer Manager profiel met Get-AzureRmTrafficManagerProfile, de eigenschappen van het eindpunt in het profiel bijwerken en wijzigingen met behulp van Set AzureRmTrafficManagerProfile. Deze methode heeft het voordeel van meer dan één eindpunt in één bewerking bijwerken.
2. Set AzureRmTrafficManagerEndpoint met wijzigingen ophalen van het beheer van verkeer eindpunt met Get-AzureRmTrafficManagerEndpoint en bijwerken van de eigenschappen van het eindpunt. Deze methode is eenvoudiger, omdat u niet hoeft te indexeren in de array eindpunten in het profiel.

### <a name="example-1-updating-endpoints-using-get-azurermtrafficmanagerprofile-and-set-azurermtrafficmanagerprofile"></a>Voorbeeld 1: Eindpunten met Get-AzureRmTrafficManagerProfile en een Set AzureRmTrafficManagerProfile bijwerken

In dit voorbeeld wijzigt u de prioriteit van de twee eindpunten binnen een bestaand profiel.

    $profile = Get-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
    $profile.Endpoints[0].Priority = 2
    $profile.Endpoints[1].Priority = 1
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

### <a name="example-2-updating-an-endpoint-using-get-azurermtrafficmanagerendpoint-and-set-azurermtrafficmanagerendpoint"></a>Voorbeeld 2: Een eindpunt met Get-AzureRmTrafficManagerEndpoint en een Set AzureRmTrafficManagerEndpoint bijwerken

In dit voorbeeld wordt wijzigt het gewicht van een enkelvoudig eindpunt in een bestaand profiel.

    $endpoint = Get-AzureRmTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
    $endpoint.Weight = 20
    Set-AzureRmTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint

## <a name="enabling-and-disabling-endpoints-and-profiles"></a>In- en uitschakelen van eindpunten en profielen

Verkeer beheren kunt afzonderlijke eindpunten worden ingeschakeld en uitgeschakeld, en in- en uitschakelen van hele profielen toe.
Deze wijzigingen kunnen worden aangebracht door ophalen/bijwerken/eindpunt of profiel bronnen. Om deze veelvoorkomende bewerkingen te stroomlijnen, worden ze ook ondersteund via speciale cmdlets.

### <a name="example-1-enabling-and-disabling-a-traffic-manager-profile"></a>Voorbeeld 1: In- en uitschakelen van een serviceprofiel verkeer

Als u een serviceprofiel verkeer, gebruiken inschakelen AzureRmTrafficManagerProfile. Het profiel kan worden opgegeven met behulp van een profile-object. De profile-object kan worden doorgegeven via de pijpleiding of met behulp van het "-TrafficManagerProfile" parameter. In dit voorbeeld geven we het profiel door de naam van de profiel- en resourcegegevens.

```powershell
    Enable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Een serviceprofiel verkeer uitschakelen:

```powershell
    Disable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

De cmdlet uitschakelen AzureRmTrafficManagerProfile vraagt om bevestiging. In dit venster worden onderdrukt met behulp van de '-Force' parameter.

### <a name="example-2-enabling-and-disabling-a-traffic-manager-endpoint"></a>Voorbeeld 2: In- en uitschakelen van een eindpunt van het beheer van verkeer

Als u een eindpunt van het beheer van verkeer, gebruiken inschakelen AzureRmTrafficManagerEndpoint. Er zijn twee manieren om op te geven van het eindpunt

1. Met behulp van een TrafficManagerEndpoint-object doorgegeven via de pijpleiding of met het "-TrafficManagerEndpoint" parameter
2. De naam van het eindpunt, type eindpunt, profielnaam en Resourcegroepnaam gebruiken:

```powershell
    Enable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Op dezelfde manier een eindpunt van het beheer van verkeer uitschakelen:

```powershell
     Disable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force
```

Net als bij uitschakelen-AzureRmTrafficManagerProfile vraagt de cmdlet uitschakelen AzureRmTrafficManagerEndpoint om bevestiging. In dit venster worden onderdrukt met behulp van de '-Force' parameter.

## <a name="delete-a-traffic-manager-endpoint"></a>Een eindpunt verkeer Manager verwijderen

U kunt afzonderlijke eindpunten verwijderen met de cmdlet verwijderen AzureRmTrafficManagerEndpoint:

```powershell
    Remove-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Deze cmdlet wordt om bevestiging gevraagd. In dit venster worden onderdrukt met behulp van de '-Force' parameter.

## <a name="delete-a-traffic-manager-profile"></a>Een profiel verkeer Manager verwijderen

Verkeer Manager profiel verwijderen, gebruikt u de cmdlet verwijderen AzureRmTrafficManagerProfile, geven de namen van profielen en bron:

```powershell
    Remove-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]
```

Deze cmdlet wordt om bevestiging gevraagd. In dit venster worden onderdrukt met behulp van de '-Force' parameter.

Het profiel te verwijderen kan ook worden opgegeven met behulp van een profile-object:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
    Remove-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile [-Force]
```

Deze reeks kan ook worden doorgesluisd:

```powershell
    Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG | Remove-AzureRmTrafficManagerProfile [-Force]
```

## <a name="next-steps"></a>Volgende stappen

[Beheer van netwerkverkeer controleren](traffic-manager-monitoring.md)

[Overwegingen bij prestaties van verkeer Manager](traffic-manager-performance-considerations.md)
