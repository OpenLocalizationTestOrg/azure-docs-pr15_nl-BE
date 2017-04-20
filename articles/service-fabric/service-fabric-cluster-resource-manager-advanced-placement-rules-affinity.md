<properties
   pageTitle="Service Fabric Cluster Resource Manager - affiniteit | Microsoft Azure"
   description="Overzicht van de affiniteit voor configuratieservices Service configureren"
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

# <a name="configuring-and-using-service-affinity-in-service-fabric"></a>Configureren en gebruiken van service-affiniteit in Service Fabric

Affiniteit is een besturingselement dat is voornamelijk bedoeld om te helpen de overgang van grotere monolithische toepassingen in de cloud en microservices wereld vereenvoudigen. Dat zei dat het kan ook worden gebruikt in bepaalde gevallen als een legitieme optimalisatie voor het verbeteren van de prestaties van de diensten, al deze neveneffecten hebben.

Stel dat u een grotere app, of die is niet zomaar ontworpen met microservices in acht nemen, Fabric-Service laten. Deze overgang is werkelijk gemeenschappelijke en we hadden meerdere klanten (intern en extern) in deze situatie. U start door het opheffen van de gehele app in de omgeving, om het verpakt en uitgevoerd. U start vervolgens splitsen in verschillende kleinere services alle met elkaar praten.

Dan is er een "Oops...". De "Oops" wordt meestal in een van deze categorieën valt:

1. Sommige X-component in de monolithische app had een niet-gedocumenteerde afhankelijkheid op de Y-component en we in afzonderlijke services hebt ingeschakeld. Aangezien deze nu op de andere knooppunten in het cluster worden uitgevoerd, zijn ze verbroken.
2.  Deze dingen communiceren via (lokale benoemde pipes | gedeeld geheugen | bestanden op schijf), maar echt nodig kunnen onafhankelijk van elkaar werken versnellen van een bit. Ik moet de harde afhankelijkheid later verwijderen.
3.  Alles werkt prima, maar het blijkt dat deze twee onderdelen werkelijk zeer zijn chatty-prestatie-gevoelige. Wanneer zij deze naar de afzonderlijke services verplaatst algehele prestaties tanked of latentie verhoogd. Als gevolg hiervan, voldoet de algemene toepassing niet aan de verwachtingen.

In deze gevallen we niet wilt verliezen onze refactoring werk en wilt teruggaan naar de monoliet, maar we hoeven sommige gevoel van plaats. Dit blijft van kracht totdat we de onderdelen werken natuurlijk als services kunt ontwerpen of totdat we de prestatieverwachtingen een andere manier indien mogelijk kunt oplossen.

Wat te doen? U kan ook proberen door affiniteit in te schakelen.

## <a name="how-to-configure-affinity"></a>Het configureren van affiniteit
Als u affiniteit instellen, kunt u een relatie affiniteit tussen twee verschillende services definiëren. U kunt zien van affiniteit als "wijzen" één service bij een ander en zeggen "deze service kan alleen uitgevoerd wanneer dat de service wordt uitgevoerd." Soms noemen wij affiniteit een relatie bovenliggend/onderliggend (waar u wijst het kind op het bovenliggende object). Affiniteit zorgt ervoor dat de replica's of exemplaren van een service worden geplaatst op de knooppunten hetzelfde als de replica's of exemplaren van een ander.

``` csharp
ServiceCorrelationDescription affinityDescription = new ServiceCorrelationDescription();
affinityDescription.Scheme = ServiceCorrelationScheme.Affinity;
affinityDescription.ServiceName = new Uri("fabric:/otherApplication/parentService");
serviceDescription.Correlations.Add(affinityDescription);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

## <a name="different-affinity-options"></a>Affiniteit met verschillende opties
Affiniteit wordt weergegeven via een van de verschillende regelingen voor correlatie en heeft twee verschillende modi. De meest voorkomende modus van affiniteit is dat van NonAlignedAffinity. In NonAlignedAffinity worden de replica's of exemplaren van de verschillende services geplaatst op de knooppunten hetzelfde. De andere modus is AlignedAffinity. Uitgelijnde affiniteit is met stateful services alleen nuttig. Twee stateful services affiniteit hebt uitgelijnd als u wilt configureren, zorgt u ervoor dat de primaire kleuren van deze diensten worden geplaatst op de knooppunten hetzelfde als elke andere. Het wordt ook elk paar van secundaire servers voor deze diensten moet worden geplaatst op de knooppunten hetzelfde. Het is ook mogelijk (hoewel minder bekende) NonAlignedAffinity voor stateful services configureren. Voor NonAlignedAffinity de andere replica's van de twee stateful services zou worden collocated op de knooppunten hetzelfde, maar zou niet geprobeerd om uit te lijnen hun primaire of secundaire servers.

![Affiniteit en hun effecten][Image1]

### <a name="best-effort-desired-state"></a>Beste inspanningen gewenst staat
Er zijn enkele verschillen tussen affiniteit en monolithische architectuur. Veel van hen zijn omdat een relatie affiniteit beste inspanning is. De services in een relatie affiniteit zijn fundamenteel verschillende entiteiten die kunnen mislukken en onafhankelijk van elkaar worden verplaatst. Er zijn ook oorzaken voor waarom een relatie affiniteit kan verbreken. Bijvoorbeeld, past waarbij slechts een deel van de service-objecten in de relatie affiniteit capaciteitsbeperkingen op een bepaald knooppunt. Worden in deze gevallen zelfs als er een relatie met affiniteit in plaats, deze kan niet afgedwongen door de andere beperkingen. Als het mogelijk is om af te dwingen alle andere beperkingen en affiniteit op een later tijdstip worden de schending van de affiniteit beperking automatisch gecorrigeerd.  

### <a name="chains-vs-stars"></a>Kettingen en sterren
Vandaag kunnen we niet model ketens van affiniteit relaties. Dit houdt in dat een service die een kind in een relatie met affiniteit is niet een moedermaatschappij in een andere relatie met affiniteit. Als u modelleren van dit type relatie wilt, hebt u effectief als het model als een ster, in plaats van een keten. Om dit te doen, het onderste kind zou bovenliggend element zijn van de 'middelste' onderliggende, bovenliggende in plaats daarvan. Afhankelijk van de rangschikking van uw diensten mogelijk dient u een "plaatsaanduiding voor" service dienst als het bovenliggende element voor meerdere kinderen te maken.

![Kettingen versus sterren in de Context van affiniteit relaties][Image2]

Iets te weten over relaties affiniteit vandaag is dat zij gericht zijn. Dit betekent dat de regel 'affiniteit' alleen wordt afgedwongen dat het kind is waarbij het bovenliggende object. Als bijvoorbeeld de bovenliggende plotseling wordt overgenomen door een ander knooppunt vervolgens het Cluster Resource Manager niet werkelijk van mening dat verkeerd totdat het merkt dat het kind bevindt zich aan een bovenliggende; de relatie wordt niet onmiddellijk toegepast.

### <a name="partitioning-support"></a>Partititie-ondersteuning
Het laatste dat opvalt over affiniteit is die affiniteit relaties worden niet ondersteund wanneer het bovenliggende item is gepartitioneerd. Dit is iets dat we uiteindelijk kan ondersteunen, maar vandaag is niet toegestaan.

## <a name="next-steps"></a>Volgende stappen
- Voor meer informatie over de opties beschikbaar voor het configureren van services het onderwerp uitchecken op de andere Cluster Resource Manager-configuraties beschikbaar, [informatie over het configureren van Services](service-fabric-cluster-resource-manager-configure-services.md)
- Vele redenen waar mensen met affiniteit, zoals het beperken van services op een kleine machines ingesteld en probeert de werklast van een verzameling services, samenvoegen beter worden ondersteund door middel van toepassingsgroepen. [Toepassingsgroepen](service-fabric-cluster-resource-manager-application-groups.md) uitchecken

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resrouce-manager-affinity-modes.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resource-manager-chains-vs-stars.png
