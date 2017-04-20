<properties
   pageTitle="Service Fabric Cluster Resource Manager - toepassingsgroepen | Microsoft Azure"
   description="Overzicht van de groep van toepassingen functionaliteit in de Service Cluster Resource Manager Fabric"
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

# <a name="introduction-to-application-groups"></a>Inleiding tot de toepassing
Service-Fabric Cluster Resource Manager beheert doorgaans clusterbronnen door spreiding van de belasting (voorgesteld via Metrics) gelijkmatig in het cluster. Fabric-service beheert ook de capaciteit van de knooppunten in het cluster en het cluster als geheel tot en met het begrip van de capaciteit. Dit werkt prima voor een groot aantal verschillende soorten werkbelasting, maar de patronen die maken intensief gebruik van verschillende exemplaren van de Service Fabric toepassing halen soms aanvullende eisen. Er zijn meestal bepaalde aanvullende eisen:

- Mogelijkheid om de capaciteit voor een exemplaar van de toepassing services op een aantal knooppunten te reserveren
- Mogelijkheid te beperken van het totale aantal knooppunten die een bepaalde set van diensten binnen een toepassing mag worden uitgevoerd op
- Capaciteit definiëren op het exemplaar van de toepassing zelf te beperken van het aantal of de totale resourceverbruik van de services in het

Om te voldoen aan deze eisen, ontwikkeld we ondersteuning voor wat wij toepassingsgroepen bellen.

## <a name="managing-application-capacity"></a>Capaciteit van toepassingen beheren
Capaciteit van toepassingen kan worden gebruikt te beperken het aantal knooppunten overbrugd door een toepassing, alsmede de totale metrische belasting van die exemplaren van de toepassingen op de afzonderlijke knooppunten. Het kan ook worden gebruikt voor het reserveren van bronnen in het cluster voor de toepassing.

Capaciteit van toepassingen kan voor nieuwe toepassingen worden ingesteld wanneer ze worden gemaakt; het kan ook worden bijgewerkt voor bestaande toepassingen die zijn gemaakt zonder de capaciteit van toepassingen.

### <a name="limiting-the-maximum-number-of-nodes"></a>Het maximum aantal knooppunten beperken
Het eenvoudigste geval gebruiken voor capaciteit van toepassing is wanneer een exemplaar van de toepassing moet worden beperkt tot een maximum aantal knooppunten. Als er geen capaciteit voor de toepassing is opgegeven, de Service Fabric Cluster Resource Manager wordt een exemplaar van replica's volgens de normale regels (netwerktaakverdeling of defragmentatie), dit meestal betekent dat de services worden verdeeld over alle van de beschikbare knooppunten in het cluster, of als u Defragmentatie enkele willekeurige maar kleiner aantal knooppunten is ingeschakeld.

De volgende afbeelding toont de mogelijke plaatsing van een toepassingsexemplaar zonder dat het maximum aantal knooppunten gedefinieerd en vervolgens dezelfde toepassingen met een maximum aantal knooppunten instellen. Opmerking: Er is geen waarborgen gemaakt over welke replica's of exemplaren van die services bij elkaar wordt geplaatst.

![Maximum aantal knooppunten definieert toepassingsexemplaar][Image1]

In het voorbeeld links wordt de toepassing geen capaciteit van toepassingen instellen en heeft drie services. CRM is een logische beslissing op alle replica's over zes beschikbare knooppunten verspreid om te komen tot de beste balans in het cluster worden uitgevoerd. In het voorbeeld rechts zien we dezelfde toepassing die op drie knooppunten is beperkt en waar Fabric-CRM-Service, de beste balans voor de replica's van de diensten van de toepassing heeft behaald.

De parameter waarmee dit probleem wordt MaximumNodes genoemd. Met deze parameter kunt instellen tijdens het maken van een toepassing of bijgewerkt voor de instantie van een toepassing die al actief is, in dat geval Fabric-CRM-Service de replica's van de diensten van de toepassing naar het opgegeven maximum aantal knooppunten zullen beperken.

PowerShell

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MaximumNodes 3
Update-ServiceFabricApplication –Name fabric:/AppName –MaximumNodes 5
```

C#

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MaximumNodes = 3;
fc.ApplicationManager.CreateApplicationAsync(ad);

ApplicationUpdateDescription adUpdate = new ApplicationUpdateDescription(new Uri("fabric:/AppName"));
adUpdate.MaximumNodes = 5;
fc.ApplicationManager.UpdateApplicationAsync(adUpdate);

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.TotalApplicationCapacity = 1000;

adUpdate.Metrics.Add(appMetric);
```

## <a name="application-metrics-load-and-capacity"></a>Toepassing maatstaven, laden en capaciteit
Toepassingsgroepen kunnen u ook voor het definiëren van parameters die zijn gekoppeld aan het exemplaar van een bepaalde toepassing, alsook de capaciteit van de toepassing voor deze parameters. Dus bijvoorbeeld kunt u definiëren dat bij veel services als u wilt dat kunnen worden gemaakt in

Er zijn 2 waarden die kunnen worden ingesteld voor het beschrijven van de capaciteit voor dat toepassingsexemplaar voor elke metric:

-   Totaal toepassing capaciteit: de totale capaciteit van de toepassing voor een bepaalde waarde vertegenwoordigt. Fabric-CRM-service probeert te beperken, de som van de metrische werklast van de diensten van de toepassing op de opgegeven waarde; Voorts als belasting tot dit maximum al in beslag van de toepassing services nemen, Service Fabric Cluster Resource Manager wordt niet toestaan het maken van nieuwe diensten of partities die leiden de totale belasting tot zou te overleggen over deze limiet.
-   Maximale capaciteit van knooppunt – geeft de maximale totale belasting voor replica's van de toepassingen, services op één knooppunt. Als de totale belasting op het knooppunt gaat over deze capaciteit, probeert Fabric-CRM-Service replica's verplaatsen naar andere knooppunten, zodat de beperking van de capaciteit wordt gerespecteerd.

## <a name="reserving-capacity"></a>Reservecapaciteit
Voor groepen ook vaak gebruikt wordt om ervoor te zorgen dat de bronnen binnen het cluster worden gereserveerd voor een bepaalde toepassingsexemplaar, zelfs als exemplaar van de toepassing services in het nog niet hebt, of zelfs als ze de resources nog niet zijn verbruikt. We gaan kijken hoe dat zou werken.  

### <a name="specifying-a-minimum-number-of-nodes-and-resource-reservation"></a>Een minimum aantal knooppunten en de reservering op te geven
Resources reserveren voor een toepassingsexemplaar moet een paar extra parameters opgeven: *MinimumNodes* en *NodeReservationCapacity*

- MinimumNodes - net als het opgeven van een doel maximum aantal knooppunten waarop de services binnen een toepassing kunnen worden uitgevoerd, kunt ook u het minimum aantal knooppunten die door een toepassing moet worden uitgevoerd. Deze instelling bepaalt effectief het aantal knooppunten die de bronnen worden gereserveerd op ten minste garanderen capaciteit binnen het cluster als onderdeel van het maken van het toepassingsexemplaar.
- NodeReservationCapacity - de NodeReservationCapacity kan worden gedefinieerd voor elke waarde in de toepassing. Hiermee definieert u het bedrag van de metrische laden die zijn gereserveerd voor de toepassing op elk knooppunt waar een van de replica's of exemplaren van de services in het worden geplaatst.

Laten we eens een voorbeeld van de capaciteit:

![Toepassingsexemplaren gereserveerde capaciteit definiëren][Image2]

In het voorbeeld links hebben de capaciteit van de toepassing gedefinieerd. Service Fabric Cluster Resource Manager wordt de onderliggende services replica's en instanties samen met die van andere diensten (buiten de toepassing) van de toepassing om het saldo in het cluster in evenwicht.

In het voorbeeld aan de rechterkant, Stel dat de aanvraag is gemaakt met een MinimumNodes ingesteld op 2, MaximumNodes ingesteld op 3 en een toepassing Metric wordt gedefinieerd met behulp van een reservering van 20, maximale capaciteit op een knooppunt van 50 en een toepassing totale capaciteit van 100, Fabric-Service wordt op twee knooppunten voor de toepassing van de blauwe capaciteit gereserveerd en staat niet toe dat andere replica's in het cluster, verbruikt deze capaciteit. Deze capaciteit gereserveerd toepassing beschouwd verbruikt en getelde tegen de resterende capaciteit van de cluster.

Wanneer een toepassing met de reservering is gemaakt, wordt de Cluster Resource Manager capaciteit gelijk aan MinimumNodes gereserveerd * NodeReservationCapacity in het cluster, maar er geen capaciteit op bepaalde knooppunten gereserveerd totdat de replica's van de diensten van de toepassing worden gemaakt en geplaatst. Hierdoor is voor flexibiliteit, omdat de knooppunten zijn gekozen voor de nieuwe replica's alleen wanneer ze worden gemaakt. Capaciteit is gereserveerd voor een bepaald knooppunt, wanneer ten minste één replica erop wordt geplaatst.

## <a name="obtaining-the-application-load-information"></a>Het verkrijgen van de toepassing laden gegevens
Voor elke toepassing verkrijgt dat er capaciteit toepassing gedefinieerd u de informatie over de statistische belasting gemeld door replica's van de diensten. Fabric-service biedt PowerShell en beheerde API's voor dit doel.

Belasting kan bijvoorbeeld worden opgehaald met behulp van de volgende PowerShell-cmdlet:

``` posh
Get-ServiceFabricApplicationLoad –ApplicationName fabric:/MyApplication1

```

De uitvoer van deze query bevat de basisinformatie over de capaciteit van toepassingen die is opgegeven voor de toepassing, zoals de knooppunten van Minimum en maximum aantal knooppunten. Bovendien moet er informatie over het aantal knooppunten dat momenteel door de toepassing wordt gebruikt. Dus voor elke metrische belasting zal er informatie over:
- Metrische naam: De naam van de metric.
-   Reservering capaciteit: Cluster capaciteit die in het cluster is gereserveerd voor deze toepassing.
-   Toepassing laden: De totale belasting van deze toepassing van onderliggende replica's.
-   Capaciteit van toepassingen: Maximaal toegestane waarde van toepassing laden.

## <a name="removing-application-capacity"></a>Capaciteit van toepassingen verwijderen
Wanneer de capaciteit van toepassingen parameters zijn ingesteld voor een toepassing, kunnen ze worden verwijderd met behulp van API's Update-toepassing of de PowerShell cmdlets. Bijvoorbeeld:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

Deze opdracht verwijdert u alle parameters van de capaciteit van de toepassing van de toepassing en Service Fabric Cluster Resource Manager begint deze toepassing te behandelen als elke andere aanvraag in het cluster waarvoor deze parameters gedefinieerd. Het effect van de opdracht is direct en Cluster Resource Manager verwijdert u alle parameters van de capaciteit van toepassingen voor deze toepassing. geven ze opnieuw vereist Update toepassing API's die worden aangeroepen met de juiste parameters.

## <a name="restrictions-on-application-capacity"></a>Beperkingen op de capaciteit van toepassingen
Er zijn verschillende beperkingen van de capaciteit van toepassingen parameters in acht worden genomen. In het geval van validatiefouten, als het maken of bijwerken van de toepassing met een fout wordt afgewezen.
Alle integer parameters moeten niet-negatieve getallen.
Bovendien voor afzonderlijke parameters zijn beperkingen als volgt:

-   MinimumNodes zijn nooit groter dan MaximumNodes.
-   Als de capaciteit voor een metric laden zijn gedefinieerd, moeten zij deze regels volgen:
  - Knooppunt reservering capaciteit zijn niet groter dan de maximale capaciteit van het knooppunt. U kan niet bijvoorbeeld beperken de capaciteit voor metrisch "Processor" voor het knooppunt 2 eenheden en probeert te reserveren 3 eenheden op elk knooppunt.
  - Als MaximumNodes is opgegeven, kan het product van MaximumNodes en de maximale capaciteit van het knooppunt moet niet worden groter is dan de totale capaciteit van toepassingen. Bijvoorbeeld als u de maximale capaciteit van het knooppunt voor belasting metrisch "Processor" 8 en u het maximum aantal knooppunten op 10 stelt, moet totale capaciteit van de toepassing groter zijn dan 80 voor deze metriek laden.

De beperkingen worden toegepast tijdens het maken van de toepassing (op de client-kant), en tijdens het bijwerken van toepassing (op de server). Tijdens het maken van dit is een voorbeeld van een duidelijke schending van de voorschriften sinds MaximumNodes < MinimumNodes en de opdracht niet in de client voordat de aanvraag ook naar Service Fabric '-cluster verzonden wordt:

``` posh
New-ServiceFabricApplication –Name fabric:/MyApplication1 –MinimumNodes 6 –MaximumNodes 2
```

Een voorbeeld van een ongeldige update is als volgt. Als we een bestaande toepassing en maximum aantal knooppunten naar een waarde bijwerkt, wordt de update doorgeven:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 6 –MaximumNodes 2
```

Daarna kunnen we proberen minimale knooppunten bijwerken:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 6 –MinimumNodes 6
```

De client heeft geen voldoende context over de toepassing, zodat u de update voor het doorgeven aan de cluster-Service Fabric is het toegestaan. Echter, in het cluster, Fabric-Service de nieuwe parameter samen met de bestaande parameters worden gevalideerd en de updatebewerking mislukt omdat de minimale waarde tegenstander-knooppunten groter is dan de waarde voor maximum aantal knooppunten. In dit geval blijven toepassing capaciteit parameters ongewijzigd.

Deze beperkingen zijn in volgorde voor Cluster Resource Manager kan de beste plaats voor replica's van toepassingen, services, kunnen worden ingevoerd.

## <a name="how-not-to-use-application-capacity"></a>Het gebruik van de capaciteit van toepassingen

-   Niet met de capaciteit van toepassingen de toepassing tot een specifieke subset van de knooppunten te beperken: hoewel Service Fabric zorgt ervoor dat het maximum aantal knooppunten wordt gerespecteerd voor elke toepassing met een capaciteit van toepassingen opgegeven, gebruikers kunnen niet bepalen welke knooppunten zullen worden gemaakt op. Dit kan worden bereikt met plaatsing beperkingen voor services.
-   Gebruik niet de capaciteit van toepassingen om ervoor te zorgen dat twee services van dezelfde toepassing altijd naast elkaar worden geplaatst. Dit kan worden bereikt met behulp van affiniteit relatie tussen services en affiniteit kan worden beperkt tot de diensten die daadwerkelijk bij elkaar moeten worden geplaatst.

## <a name="next-steps"></a>Volgende stappen
- Voor meer informatie over de opties beschikbaar voor het configureren van services het onderwerp uitchecken op de andere Cluster Resource Manager-configuraties beschikbaar, [informatie over het configureren van Services](service-fabric-cluster-resource-manager-configure-services.md)
- Bekijk voor meer informatie over hoe het Cluster Resource Manager beheert en saldi laden in het cluster, het artikel op [load balancing](service-fabric-cluster-resource-manager-balancing.md)
- Starten vanaf het begin en het [ophalen van een inleiding tot de Service Cluster Resource Manager Fabric](service-fabric-cluster-resource-manager-introduction.md)
- Voor meer informatie over de werking maatstelsel in het algemeen, Lees informatie over [Service Fabric laden Metrics](service-fabric-cluster-resource-manager-metrics.md)
- De Cluster Resource Manager heeft een groot aantal opties voor het beschrijven van het cluster. Meer informatie over deze Kijk uit dit artikel bij het [beschrijven van een cluster Service Fabric](service-fabric-cluster-resource-manager-cluster-description.md)


[Image1]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png
