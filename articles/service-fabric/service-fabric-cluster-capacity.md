<properties
   pageTitle="Planning van de capaciteit van de cluster-Service Fabric | Microsoft Azure"
   description="Overwegingen bij de planning service Fabric cluster de capaciteit. Nodetypes, duurzaamheid en betrouwbaarheid van lagen"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Overwegingen bij de planning van service Fabric cluster capaciteit

Voor productie-implementatie is capaciteitsplanning een belangrijke stap. Hier zijn enkele van de items die u moet overwegen als onderdeel van dat proces.

- Het aantal knooppunttypen die het cluster beginnen moet met
- De eigenschappen van elk knooppunttype (grootte, primair, internetverbinding, aantal VMs, enz.)
- De kenmerken van de betrouwbaarheid en duurzaamheid van het cluster

Laat ons even controleren elk van deze items.

## <a name="the-number-of-node-types-your-cluster-needs-to-start-out-with"></a>Het aantal knooppunttypen die het cluster beginnen moet met

Eerst moet u weten wat het cluster dat u maakt moet worden gebruikt voor gaat en wat voor soort toepassingen u wilt implementeren in dit cluster. Als u niet duidelijk over het doel van het cluster, u waarschijnlijk nog niet klaar om het proces van de capaciteitsplanning invoeren.

Het aantal knooppunttypen die het cluster beginnen moet met vast.  Elk knooppunttype is toegewezen aan een virtuele Machine schaal ingesteld. Elk knooppunttype kan vervolgens worden vergroot of omlaag onafhankelijk, hebben verschillende sets van poorten open kan, en andere capaciteit metrics. Dus de beslissing van het aantal knooppunttypen in feite komt het erop neer op de volgende overwegingen:

- De toepassing beschikt over meerdere services en deze moet wel publiek- of internetverbinding? Typische toepassingen bevatten een front-end-gatewayservice die invoer ontvangt van een client en een of meer back-end services die communiceren met de front-end services. Dus in dit geval het resultaat ten minste twee knooppunttypen.

- Hebben uw diensten (waaruit uw toepassing) verschillende behoeften zoals meer RAM-geheugen of hogere CPU-cycli? Laten we aannemen dat de toepassing die u wilt implementeren een front-end-service en een back-end-service bevat. De front-end-service kan worden uitgevoerd op kleinere VMs (VM formaten zoals D2) die poorten geopend met het internet.  De back-end-service echter is intensieve berekening en moet worden uitgevoerd op grotere VMs (met VM formaten zoals D4, D6, D15) die geen internet gericht.

 In dit voorbeeld Hoewel u beslissen kunt om alle services op één knooppunttype, is het raadzaam dat u deze in een cluster met twee knooppunten plaatsen.  Hierdoor hebben verschillende eigenschappen, zoals internet-verbinding of VM-grootte voor elk knooppunt. Het aantal VMs kan onafhankelijk van elkaar, maar ook worden geschaald.  

- Daar valt niet te de toekomst voorspellen, Ga met feiten die u van kent en beslissen over het aantal knooppunttypen die uw toepassingen beginnen moeten. U kunt altijd of knooppunttypen later verwijderen. Een Service Fabric '-cluster moet ten minste één knooppunttype hebben.

## <a name="the-properties-of-each-node-type"></a>De eigenschappen van elk knooppunttype

Het **knooppunttype** kan worden beschouwd als gelijkwaardig aan rollen in de Cloud-Services. Knooppunttypen definieert de VM-grootte, het aantal VMs en hun eigenschappen. Elk knooppunttype dat is gedefinieerd in een cluster Service Fabric is ingesteld als een afzonderlijke virtuele Machine schaal ingesteld. VM schaal Sets zijn een Azure resource te implementeren en beheren van een collectie als een set virtuele machines kunt u berekenen. Wordt gedefinieerd als aparte VM schaal wordt ingesteld, elk knooppunttype vervolgens kan worden vergroot of omlaag onafhankelijk van elkaar, hebben verschillende sets van poorten open en andere capaciteit metrics kan hebben.

Het cluster meer dan één type van het knooppunt kan hebben, maar het primaire knooppunttype (de eerste shape die u op de portal definieert) moet ten minste vijf VMs voor clusters gebruikt voor de productie werklast (of ten minste drie VMs voor test-clusters). Als u het cluster met behulp van de sjabloon voor een Resource Manager maakt, zult u een **primaire** kenmerk onder de definitie van het type knooppunt. Het type primaire knooppunt is het knooppunt waar Service configuratieservices systeem worden geplaatst.  

### <a name="primary-node-type"></a>Primaire knooppunttype
Voor een cluster met meerdere knooppunttypen moet u kiezen uit primaire worden. Hier zijn de kenmerken van een type primaire knooppunt:

- De minimale grootte van VMs voor het primaire knooppunt wordt bepaald door de duurzaamheid laag die u kiest. De standaardwaarde voor de duurzaamheid laag is Brons. Schuif omlaag voor informatie over wat de duurzaamheid laag is en de waarden die kan nemen.  

- Het minimum aantal VMs voor het primaire knooppunt wordt bepaald door de betrouwbaarheid laag die u kiest. De standaardwaarde voor de betrouwbaarheid laag is Zilver. Schuif omlaag voor informatie over wat de betrouwbaarheid laag is en de waarden die kan nemen.

- De Service systeem configuratieservices (bijvoorbeeld de Manager van de Cluster-service of een afbeelding Store-service) worden geplaatst op het primaire knooppunttype en dus de betrouwbaarheid en duurzaamheid van het cluster wordt bepaald door de laag betrouwbaarheid en duurzaamheid tier geselecteerde waarde voor het primaire knooppunt.

![Schermafdruk van een cluster met twee knooppunttypen ][SystemServices]


### <a name="non-primary-node-type"></a>Niet-primaire knooppunttype
Voor een cluster met meerdere knooppunttypen, er één type van het primaire knooppunt en de overige niet-primaire. Hier zijn de kenmerken van een niet-primaire knooppunttype:

- De minimale grootte van VMs voor dit knooppunttype wordt bepaald door de duurzaamheid laag die u kiest. De standaardwaarde voor de duurzaamheid laag is Brons. Schuif omlaag voor informatie over wat de duurzaamheid laag is en de waarden die kan nemen.  

- Het minimum aantal VMs voor dit knooppunttype zijn. Maar u moet dit nummer op basis van het aantal replica's van de toepassing/services die u wilt uitvoeren in dit knooppunttype. Het nummer van het VMs in een knooppunt kan worden verhoogd nadat u het cluster hebt geïmplementeerd.


## <a name="the-durability-characteristics-of-the-cluster"></a>De kenmerken van de duurzaamheid van het cluster

De laag duurzaamheid wordt gebruikt om aan te geven aan het systeem van de bevoegdheden die de VMs met de onderliggende infrastructuur Azure hebt. Typ in het primaire knooppunt kan met deze bevoegdheid Service Fabric VM level infrastructuur verzoeken (zoals een reboot VM VM reimage of VM-migratie) die invloed hebben op de quorum-vereisten voor het systeem en de stateful services onderbreken. Met deze bevoegdheid kan in de niet-primaire knooppunttypen Fabric-Service onderbreken VM level infrastructuur verzoeken zoals VM opstarten, VM reimage, enz., die invloed hebben op de vereisten van het quorum voor uw stateful services die worden uitgevoerd in het VM-migratie.

Deze bevoegdheid wordt uitgedrukt in de volgende waarden:

- Goud - de taken van de infrastructuur kan worden onderbroken voor een duur van 2 uur per UD

- Silver - de taken van de infrastructuur kan worden onderbroken voor een duur van 30 minuten per UD (dit is momenteel niet ingeschakeld voor gebruik. Eenmaal ingeschakeld Dit is beschikbaar op alle standaard VMs van single core en hoger).

- Bronze - geen bevoegdheden. Dit is de standaardinstelling.

## <a name="the-reliability-characteristics-of-the-cluster"></a>De betrouwbaarheidskenmerken van het cluster

De betrouwbaarheid laag wordt gebruikt om het aantal replica's van de system-services die u wilt uitvoeren in dit cluster op het primaire knooppunttype instellen. De meer het aantal replica's, meer betrouwbare zijn de systeemservices in het cluster.  

De betrouwbaarheid laag kunnen de volgende waarden.

- Platinum - de systeemservices worden uitgevoerd met een doel replicaset telling van 9

- Goud - de systeemservices worden uitgevoerd met een doel replicaset telling van 7

- Silver - de systeemservices worden uitgevoerd met een doel replicaset telling van 5

- Bronze - de systeemservices worden uitgevoerd met een doel replicaset telling van 3

>[AZURE.NOTE] De betrouwbaarheid laag die u kiest bepaalt het minimum aantal knooppunten die het type van uw primaire knooppunt moet hebben. De betrouwbaarheid laag heeft geen gevolgen voor de maximale grootte van het cluster. Zo kunt u een cluster knooppunt 20 laten is waarop Bronze betrouwbaarheid.

 U kunt de betrouwbaarheid van het cluster vanaf een laag naar een andere update. Dit activeert de cluster upgrades nodig te wijzigen van de replica system services aantal instellen. Wacht totdat de upgrade is uitgevoerd voordat u andere wijzigingen aanbrengt in het cluster, zoals het toevoegen van knooppunten, enz.  U kunt de voortgang van de upgrade op Service Fabric Explorer of door het uitvoeren van [Get-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt126012.aspx) controleren

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

Zodra u klaar bent met de capaciteit van de planning en het instellen van een cluster, lees het volgende:
- [Service Fabric serverclusters, beveiliging](service-fabric-cluster-security.md)
- [Service Fabric gezondheid model-Inleiding](service-fabric-health-introduction.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png
