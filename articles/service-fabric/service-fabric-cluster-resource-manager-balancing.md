<properties
   pageTitle="Balancing het Cluster met de Azure Service Fabric Cluster Resource Manager | Microsoft Azure"
   description="Een inleiding tot het cluster met de Service Cluster Resource Manager Fabric balancing."
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

# <a name="balancing-your-service-fabric-cluster"></a>Balancing uw fabric '-cluster service
De Service Fabric Cluster Resource Manager kunnen dynamische belasting aangifte, reageren op wijzigingen in het cluster de nieuwe beperking te corrigeren en het cluster opnieuw indien nodig. Hoe vaak doet het deze dingen maar wat activeert? Er zijn meerdere besturingselementen die zijn gekoppeld aan dit.

De eerste set besturingselementen om netwerktaakverdeling zijn een aantal timers. Deze timers bepalen hoe vaak de Cluster Resource Manager controleert de status van het cluster voor dingen die moeten worden aangepakt. Er zijn drie soorten werk, elk met hun eigen bijbehorende timer. Deze zijn:

1.  Plaatsing – in dit stadium behandelt het plaatsen van een replica stateful of stateless exemplaren die ontbreken. Dit heeft betrekking op nieuwe services en verwerken van replica stateful of stateless exemplaren die zijn mislukt en moeten opnieuw worden gemaakt. Replica's of exemplaren weghalen verwijderen en het is ook hier verwerkt.
2.  Beperking wordt gecontroleerd: deze fase controleert en corrigeert schendingen van de plaatsing van verschillende beperkingen (regels) in het systeem. Voorbeelden van regels zijn zaken als knooppunten zijn niet overbelast en dat van de service plaatsing beperkingen (meer informatie over deze later) wordt voldaan.
3.  Netwerktaakverdeling – deze fase gecontroleerd proactieve opnieuw nodig is op basis van de geconfigureerde gewenste balans voor de verschillende parameters, en als dat zo is gezocht naar een rangschikking in het cluster dat meer in evenwicht.

## <a name="configuring-cluster-resource-manager-steps-and-timers"></a>Cluster Resource Manager stappen en Timers configureren
Elk van deze verschillende typen correcties met het Cluster Resource Manager aanbrengen kunt wordt beheerd door de timer die de frequentie ervan wordt geregeld. Dus bijvoorbeeld als u alleen wilt omgaan met de nieuwe service werklasten elk uur (deze batch-), maar regelmatig Netwerktaakverdeling elke paar seconden controleert wilt in het cluster te plaatsen, kunt u dat gedrag. Wanneer de timer wordt gestart, wordt de taak gepland. Standaard de bronnenbeheerder de staat scant en updates (batchen van alle wijzigingen die zijn opgetreden sinds de laatste scan, zoals daarvan die een knooppunt verbroken is) geldt elke 1/10e van een seconde, wordt de plaatsing en de beperking controleren vlaggen per seconde en de balancing vlag elke 5 seconden.

ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PLBRefreshGap" Value="0.1" />
            <Parameter Name="MinPlacementInterval" Value="1.0" />
            <Parameter Name="MinConstraintCheckInterval" Value="1.0" />
            <Parameter Name="MinLoadBalancingInterval" Value="5.0" />
        </Section>
```

Vandaag wij slechts een van deze acties tegelijk uitvoeren, achter elkaar (dat is de reden waarom we verwijzen naar deze configuraties als "minimale interval")). Dit is, zoals we al hebt gereageerd op eventuele openstaande verzoeken voor het maken van nieuwe replica's voordat we verdergaan met balancing het cluster. Zoals u door de tijdsintervallen standaard is opgegeven zien kunt, kunnen worden gescand en het selectievakje voor alles we zeer vaak moeten, wat betekent dat de reeks wijzigingen we aan het einde van elke stap maken is meestal kleiner: we niet scannen via uren van wijzigingen in het cluster en wilt in één keer te corrigeren, we probeert te verwerken dingen min of meer zoals ze gebeuren, maar met enkele batchen wanneer veel dingen gebeuren op de dezelfde tijd. Dit maakt de Service Fabric resource manager zeer reageert op dingen die gebeuren in het cluster.

Terwijl de meeste van deze taken zijn duidelijk (als er nieuwe beperking, correctie, als er services worden gemaakt, zijn ze maken), moet de Cluster Resource Manager ook aanvullende gegevens om te controleren of het cluster imbalanced. Dat hebben we twee andere stukken van configuratie: *Balancing drempels* en *Drempels van activiteit*.

## <a name="balancing-thresholds"></a>Netwerktaakverdeling drempels
Een drempel van netwerktaakverdeling is het belangrijkste besturingselement voor proactieve opnieuw activeren (Houd er rekening mee dat de timer is alleen voor hoe vaak de Cluster Resource Manager controleert - betekent niet dat er gebeurt). De drempel voor netwerktaakverdeling wordt gedefinieerd hoe imbalanced het cluster moet worden voor een bepaalde metriek in volgorde voor het Cluster Resource Manager te overwegen het imbalanced en trigger balancing.

Tegenrekening drempels worden gedefinieerd op basis van per metric als onderdeel van de definitie van het cluster. Voor meer informatie over parameters uitchecken [in dit artikel](service-fabric-cluster-resource-manager-metrics.md).

ClusterManifest.xml

``` xml
    <Section Name="MetricBalancingThresholds">
      <Parameter Name="MetricName1" Value="2"/>
      <Parameter Name="MetricName2" Value="3.5"/>
    </Section>
```

De drempel voor netwerktaakverdeling voor een metric is een verhouding. Als dit aantal, het bedrag van de belasting op de meeste geladen knooppunt gedeeld door het bedrag van de belasting op het minste geladen knooppunt overschrijdt en vervolgens het cluster wordt beschouwd als imbalanced en Netwerktaakverdeling wordt geactiveerd wanneer de volgende keer controleert het Cluster Resource Manager (standaard ooit 5 seconden als geregeld door de MinLoadBalancingInterval, hierboven).

![Voorbeeld van de drempel voor netwerktaakverdeling][Image1]

In het volgende eenvoudige voorbeeld wordt elke service één eenheid van sommige metric verbruikt. In het voorbeeld boven de maximale belasting op een knooppunt is ingesteld op 5 en het minimum is 2. Stel dat de tegenrekening drempel voor deze waarde 3 is. In het voorbeeld boven het cluster is derhalve geen netwerktaakverdeling wordt geactiveerd wanneer het Cluster Resource Manager wordt gecontroleerd en op evenwichtige (Aangezien de verhouding in het cluster is 5/2 = 2,5 en is kleiner dan de opgegeven drempelwaarde voor taakverdeling van 3).

In het voorbeeld onder de maximale belasting op een knooppunt is 10, terwijl het minimum 2 is, wat resulteert in een verhouding van 5. Hiermee wordt het cluster de ontworpen balancing overschrijdt van 3 voor dat metric geplaatst. Als gevolg hiervan zijn een globale rebalancing run geplande zodra de tegenrekening timer wordt geactiveerd. Dat alleen omdat een tegenrekening zoekopdracht wordt gestart betekent niet iets dat wordt verplaatst - soms het cluster imbalanced is, maar de situatie niet kan worden verbeterd - maar in een situatie als deze een (ten minste standaard) deel van de belasting zal vrijwel zeker worden gedistribueerd naar Knooppunt3. Houd er rekening mee dat omdat we niet met behulp van een hebzuchtig benadering bepaalde belasting kan ook worden gedistribueerd naar knooppunt2 omdat die leiden minimalisering van de algehele verschillen tussen knooppunten tot zou, maar we verwachten zou dat het merendeel van de belasting naar Knooppunt3 zou vloeien.

![Voorbeeld van de acties van drempel Balancing][Image2]

Opmerking dat onder de drempel van netwerktaakverdeling is niet te vragen een expliciete doelstelling – Balancing drempels zijn slechts een *trigger* waarin wordt gemeld dat het Cluster Resource Manager die dit er in het cluster ziet bepalen welke verbeteringen kan worden gemaakt, indien van toepassing.

## <a name="activity-thresholds"></a>Activiteit drempels
Soms is het *totale* bedrag van de belasting in het cluster lage hoewel betrekkelijk imbalanced knooppunten. Dit kan alleen door de tijd van de dag, of omdat het cluster nieuwe en alleen ophalen is gestart met een bootstrap. U kunt in beide gevallen niet aan het cluster worden verdeeld, omdat er eigenlijk niet is te veel – u alleen netwerk en compute resources verplaatsen dingen rond doorbrengen moet, zonder het absolute verschil opzien. Aangezien we dit voorkomen, is er een ander besturingselement in de bronnenbeheerder, bekend als activiteit drempels, kunt u sommige absolute ondergrens voor activiteit – opgeven als er geen knooppunt ten minste dit heeft veel laden en vervolgens Netwerktaakverdeling wordt niet geactiveerd zelfs als wordt voldaan aan de drempel voor netwerktaakverdeling.

Stel, dat we rapporten met de volgende totalen voor verbruik op deze knooppunten hebben als voorbeeld. Stel nu dat we onze Balancing drempel van 3 voor deze metrische gegevens behouden, maar we ook een activiteit drempel van 1536 hebben. In het eerste geval terwijl het cluster imbalanced per de drempel voor netwerktaakverdeling is geen knooppunt voldoet aan deze minimumdrempel activiteit zodat we dingen laten. In het voorbeeld onder is knooppunt1 manier de activiteit overschrijdt, zodat er Netwerktaakverdeling wordt uitgevoerd (omdat zowel de drempel voor netwerktaakverdeling en de drempel voor de metric voor activiteit worden overschreden)

![Voorbeeld van de drempel voor activiteit][Image3]

Net als Balancing drempels zijn activiteit drempels gedefinieerd per-metric via de definitie van het cluster:

ClusterManifest.xml

``` xml
    <Section Name="MetricActivityThresholds">
      <Parameter Name="Memory" Value="1536"/>
    </Section>
```

Taakverdeling en activiteit drempels zijn beide verbonden met de metric - balancing wordt alleen geactiveerd als taakverdeling en activiteit drempels worden overschreden voor dezelfde metric overigens. Dus als we groter is dan de drempel voor netwerktaakverdeling voor geheugen en de drempel van de activiteit voor CPU, wordt netwerktaakverdeling niet geactiveerd als de resterende drempels (Balancing drempel voor CPU) en de drempel voor geheugen activiteit niet worden overschreden.

## <a name="balancing-services-together"></a>Balancing services samen
Er is iets die interessant is dat een beslissing van het hele cluster of het cluster imbalanced of niet is is, maar de manier waarop we gaan over het oplossen van deze service afzonderlijke replica's en instanties rond wordt verplaatst. Dit is zinvol, rechts? Als geheugen wordt gestapeld om op een van de knooppunten meerdere replica's of exemplaren een bijdrage aan het leveren kunnen, dus het nodig kan verplaatsen van de replica stateful of stateless exemplaren met de metrische waarin dit probleem optreedt, imbalanced.

Af en toe als een klant ons up of het bestand belt een ticket zeggen dat een service die niet imbalanced was verplaatst. Hoe kan dit gebeuren een service wordt verplaatst zelfs als alle parameters van de service in evenwicht zijn, zelfs perfect in dat geval op het tijdstip van de andere onevenwicht? We gaan kijken!

Neem bijvoorbeeld vier diensten Service1, Service2, Service3 en Service4. Service1 rapporten met de maatstaven die Metric1 en Metric2, Service2 tegen Metric2 en Mmetric3, Service3 tegen Metric3 en Metric4 en Service4 tegen sommige metrische Metric99. Nietwaar kunt u zien waar gaan we hier. We hebben een keten! We hebben geen echt vier onafhankelijke services vanuit het perspectief van het Cluster Resource Manager, hebben we een aantal services die zijn verwante (Service1, Service2, en Service3) en die is uitgeschakeld op een eigen.

![Balancing Services samen][Image4]

Is het mogelijk dat een gebrek aan evenwicht in de Metric1 ertoe leiden dat replica's of instanties die deel uitmaken van de Service3 om te navigeren. Meestal deze verplaatsingen vrij beperkt, maar kan worden varieert, afhankelijk van precies hoe imbalanced Metric1 kreeg en welke wijzigingen in het cluster noodzakelijk zijn om deze te corrigeren. We kunnen ook met zekerheid dat een gebrek aan evenwicht in de cijfers 1, 2 of 3 zal nooit bewegingen in Service4 – zou er geen punt sinds de replica's verplaatsen of instanties die deel uitmaken van Service4 rond kan doen absoluut geen invloed hebben op het saldo van de cijfers 1, 2 of 3 uitspreken.

De Cluster Resource Manager automatisch welke diensten betrekking hebben, aangezien de services zijn toegevoegd, verwijderd, of hun metrische configuratiewijziging – bijvoorbeeld had, tussen twee uitvoeringen van netwerktaakverdeling Service2 kunnen hebben is geconfigureerd om Metric2 te verwijderen heeft weten te achterhalen. Zo wordt de keten tussen Service1 en Service2. Nu in plaats van twee groepen van services hebt u drie:

![Balancing Services samen][Image5]

## <a name="next-steps"></a>Volgende stappen
- Meetgegevens worden hoe de Service Fabric Cluster Resource Manager beheert, verbruik en capaciteit in het cluster. Voor meer Kijk informatie over deze en hoe u deze configureert dan eens [in dit artikel](service-fabric-cluster-resource-manager-metrics.md)
- Kosten van de verplaatsing is één manier om aan het Cluster Resource Manager signalering dat bepaalde services zijn duurder dan andere verplaatsen. Raadpleeg [dit artikel](service-fabric-cluster-resource-manager-movement-cost.md) voor meer informatie over de kosten van verplaatsing,
- De Cluster Resource Manager heeft verschillende throttles die u configureren kunt om te vertragen lospeuteren in het cluster. Ze niet normaal gesproken nodig zijn, maar als u ze nodig hebt kunt u meer over deze [hier](service-fabric-cluster-resource-manager-advanced-throttling.md)


[Image1]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resrouce-manager-balancing-thresholds.png
[Image2]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-threshold-triggered-results.png
[Image3]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-activity-thresholds.png
[Image4]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together1.png
[Image5]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together2.png
