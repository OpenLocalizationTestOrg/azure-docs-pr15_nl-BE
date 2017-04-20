<properties
   pageTitle="Service Fabric Cluster Resource Manager - beleid plaatsing | Microsoft Azure"
   description="Overzicht van extra plaatsing beleid en de regels voor Service configuratieservices"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="placement-policies-for-service-fabric-services"></a>Beleid voor service configuratieservices plaatsing
Er zijn veel verschillende extra regels die u uiteindelijk mogelijk over caring als de cluster-Service Fabric is spanned over een geografische afstanden zegt meerdere datacenters of Azure regio's, of als meerdere gebieden van de geopolitieke besturingselement (of sommige andere geval waar u beleid of juridische grenzen u bent geïnteresseerd of op de afstanden impact van de werkelijke prestaties/latentie) in uw omgeving. De meeste van deze kan worden geconfigureerd via de eigenschappen van het knooppunt en de plaatsing van beperkingen, maar sommige zijn ingewikkelder. Wij bieden deze extra commmands dingen eenvoudiger. Net als met andere beperkingen plaatsing kunnen plaatsing beleid worden geconfigureerd op basis van service per benoemd exemplaar.

## <a name="specifying-invalid-domains"></a>Ongeldige domeinen opgeven
Het beleid van de plaatsing van InvalidDomain kunt u opgeven dat een bepaald domein voor fouttolerantie ongeldig voor de werklast is. Dit beleid zorgt ervoor dat een bepaalde service niet wordt uitgevoerd in een bepaald gebied, bijvoorbeeld voor bedrijfs-of geopolitieke redenen. Meerdere ongeldige domeinen kunnen worden opgegeven via afzonderlijke beleidsregels.

![Voorbeeld van een domein is ongeldig][Image1]

Code:

```csharp
ServicePlacementInvalidDomainPolicyDescription invalidDomain = new ServicePlacementInvalidDomainPolicyDescription();
invalidDomain.DomainName = "fd:/DCEast"; //regulations prohibit this workload here
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("InvalidDomain,fd:/DCEast”)
```
## <a name="specifying-required-domains"></a>Vereiste domeinen opgeven
Het vereiste domeinbeleid plaatsing vereist dat alle replica stateful of stateless service-exemplaren van de service aanwezig is in het opgegeven domein. Meerdere vereiste domeinen kunnen worden opgegeven via afzonderlijke beleidsregels.

![Voorbeeld van een domein][Image2]

Code:

```csharp
ServicePlacementRequiredDomainPolicyDescription requiredDomain = new ServicePlacementRequiredDomainPolicyDescription();
requiredDomain.DomainName = "fd:/DC01/RK03/BL2";
serviceDescription.PlacementPolicies.Add(requiredDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomain,fd:/DC01/RK03/BL2")
```

## <a name="specifying-a-preferred-domain-for-the-primary-replicas"></a>Een voorkeur voor de primaire replica's-domein op te geven
De voorkeur primaire domein is een interessante besturingselement, aangezien deze selectie van het fout-domein waarin de primaire moet worden geplaatst als het mogelijk is om dit te doen. Als alles in orde is uiteindelijk de primaire in dit domein. Moet het domein of de primaire replica is mislukt of om een bepaalde reden dat de primaire worden gemigreerd naar een andere locatie worden afgesloten. Als deze locatie niet in het gewenste domein, indien mogelijk wordt de Cluster Resource Manager verplaats deze terug naar het gewenste domein. Natuurlijk zinvol deze instelling alleen is voor stateful-services. Dit beleid is vooral handig in clusters die zijn spanned over verschillende Azure gebieden of meerdere datacenters. In deze situaties u alle locaties gebruikt voor redundantie, maar liever de primaire replica's worden geplaatst op een bepaalde locatie om lagere latentie voor bewerkingen die naar de primaire gaan (schrijft en ook alle leesbewerkingen standaard worden bediend door de primaire).

![Voorkeur voor primaire domeinen en overname bij storingen][Image3]

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replicas-to-be-distributed-among-all-domains-and-disallowing-packing"></a>Replica's moet worden verdeeld over alle domeinen en verpakking weigeren vereisen
Een ander beleid, die kunt u is replica's moet altijd worden verdeeld over de beschikbare fout met betrekking tot domeinen vereist. Dit gebeurt standaard in de meeste gevallen waarin het cluster in orde is, maar er zijn degenerate gevallen waar replica's voor een bepaalde partitie tijdelijk uiteindelijk in een enkele fout of een domein upgraden verpakt. Bijvoorbeeld, Stel dat hoewel het cluster heeft 9 knooppunten in 3 fout met betrekking tot domeinen (0, 1 en 2), en uw service 3 replica's heeft, daalde van de knooppunten die werden gebruikt voor deze replica's in de fout met betrekking tot domeinen 1 en 2, en in verband met specifieke capaciteit, de andere knooppunten in die domeinen geldig zijn. Als Service Fabric vervangingen van deze replica's maken, de Cluster Resource Manager zou moeten ze in een domein voor fouttolerantie 0 plaatsen, maar dat er een situatie waarin de beperking van het domein voor fouttolerantie wordt wordt geschonden. Ook neemt de kans dat de hele replicaset verloren kan gaan (als 0 FD worden permananently verloren zouden). (Voor meer informatie over beperkingen en beperking prioriteiten in het algemeen Bekijk [dit onderwerp](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities) )

Als u een waarschuwing voor de gezondheid zoals ooit zag "verdeling van de belasting heeft ontdekt een beperkingsfout voor deze Replica: fabric: /<some service name> secundaire partitie <some partition ID> is de beperking schenden: FaultDomain ' even deze voorwaarde of iets soortgelijk. Doorgaans in deze situaties zijn tijdelijke (de knooppunten blijven niet beneden lange of als ze doen en moeten we bouwen vervangingen er zijn andere knooppunten in de juiste fouttolerantie domeinen die geldig zijn), maar er bepaalde werklast die de beschikbaarheid voor het risico zijn van het verlies van hun replica's liever zou handelen. We kunt dit doen door te geven van het beleid 'RequireDomainDistribution', waarmee wordt gegarandeerd dat er geen twee replica's van dezelfde partitie ooit zijn toegestaan in hetzelfde domein voor fouttolerantie of upgrade.

Sommige werklasten liever het doel aantal replica's (kopieën van staat) te allen tijde (weddenschappen tegen storingen van de totale domein en weten ze meestal lokale toestand kunnen terugzetten), terwijl anderen liever de uitvaltijd eerder kost dan de juistheid en dataloss betrekking heeft op het risico. Aangezien de meeste productie werklasten worden uitgevoerd met meer dan 3 replica's, is de standaardwaarde niet verlangen dat domein distributie en laten de taakverdeling en failover gevallen normaal verwerkt zelfs als dat betekent dat tijdelijk een domein heeft meerdere replica's verpakt in het.

Code:

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

Nu, zou het mogelijk deze configuraties voor services in een cluster waarop geen geografisch gespreide gebruiken? U kunt er zeker van dat! Maar er is een goede reden te – met name de vereiste ongeldig en voorkeur domeinconfiguraties moeten worden vermeden, tenzij u werkelijk een geografisch spanned cluster gebruikt - het niet logisch om te proberen te dwingen een bepaalde werkbelasting uitvoeren in één rack of de voorkeur geeft aan een segment van het lokale cluster op een andere tenzij er verschillende typen hardware of werkbelasting segmentatie op , en in die gevallen kunnen worden verwerkt via de normale positie beperkingen.

## <a name="next-steps"></a>Volgende stappen
- Voor meer informatie over de opties beschikbaar voor het configureren van services het onderwerp uitchecken op de andere Cluster Resource Manager-configuraties beschikbaar, [informatie over het configureren van Services](service-fabric-cluster-resource-manager-configure-services.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png
