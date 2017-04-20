<properties
   pageTitle="Capaciteitsplanning voor Service Fabric apps | Microsoft Azure"
   description="Wordt beschreven hoe u het aantal vereist zijn voor de toepassing van een Service Fabric-computerknooppunten"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="markfuss"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>


# <a name="capacity-planning-for-service-fabric-applications"></a>Capaciteitsplanning voor Service Fabric-toepassingen


Dit document leert u hoe u kunt schatten hoeveel resources (CPU, RAM, schijfruimte), moet u uw Azure Service Fabric-toepassingen uitvoeren. Het is gebruikelijk voor uw bronvereisten te wijzigen in de tijd. Enkele bronnen moet u meestal als u uw service ontwikkelen/testen en vervolgens meer bronnen nodig hebben als u naar de productie gaat en de toepassing in populariteit groeit. Bij het ontwerpen van uw toepassing tot en met de eisen op lange termijn denkt en keuzes waardoor uw service te schalen om te voldoen aan de vraag van klanten maken.

 Wanneer u een Service Fabric '-cluster maakt, kunt u besluiten soorten virtuele machines (VMs) maken van het cluster. Elke VM wordt geleverd met een beperkte hoeveelheid middelen in de vorm van CPU's (cores en snelheid), bandbreedte, RAM-geheugen en opslagruimte op een schijf. Na verloop van tijd groeien met uw service, kunt u upgraden naar VMs die meer bronnen en/of VMs meer toevoegen aan het cluster. Dit doet moet u bouwen uw service in eerste instantie zodat zij kan profiteren van nieuwe VMs die dynamisch toegevoegd aan het cluster.

Sommige services beheren weinig tot geen gegevens op de VMs zelf. Capaciteitsplanning voor deze services moet daarom focus vooral op de prestaties, wat betekent dat het selecteren van de juiste CPU's (cores en snelheid) van het VMs. Overweeg daarnaast bandbreedte van het netwerk, inclusief hoe vaak netwerk transfers optreden en hoeveel gegevens worden overgebracht. Als uw service moet worden uitgevoerd als service gebruik toeneemt, kunt u meer VMs toevoegen aan het cluster en de belasting saldo de netwerkaanvragen via alle VMs.

Voor services die het beheer van grote hoeveelheden gegevens op het VMs, capaciteitsplanning zich moet richten voornamelijk op grootte. U moet de capaciteit van de VM RAM-geheugen en opslagruimte op een schijf dus zorgvuldig overwegen. Het beheersysteem virtueel geheugen in Windows kunt u schijfruimte RAM toepassingscode uitzien. De runtime Fabric-Service voorziet bovendien in slimme wisselbestand alleen hot-gegevens in het geheugen te houden en de koude gegevens naar de schijf te verplaatsen. Toepassingen kunnen dus meer geheugen dan fysiek beschikbaar is op de VM gebruiken. Met meer RAM gewoon uitbreidt, de prestaties, omdat de VM meer opslagruimte op een schijf in het RAM-geheugen kunt houden. De VM die u selecteert moet een schijf groot genoeg is om de gegevens die u wilt opslaan op de VM. Op dezelfde manier de VM moet beschikken over voldoende RAM-geheugen om de gewenste prestaties. Gegevens van uw service, zal na verloop van tijd, kunt u meer VMs toevoegen aan het cluster en de partitie van de gegevens over alle VMs.

## <a name="determine-how-many-nodes-you-need"></a>Bepalen hoeveel knooppunten die u nodig hebt

Partitioneren van uw service, kunt u gegevens van uw service. Zie voor meer informatie over het partitioneren van [Partitionering Service Fabric](service-fabric-concepts-partitioning.md). Elke partitie moet passen binnen een VM één, maar meerdere (kleine) partities op een enkele VM kunnen worden geplaatst. Dus hebt met meer kleine partities u meer flexibiliteit dan de enkele grotere partities. De verhouding is dat partities u veel overhead Fabric Service verhoogt en transactionele bewerkingen kunt u meerdere partities niet uitvoeren. Er is ook meer potentiële netwerkverkeer als uw servicecode vaak toegang nodig heeft tot gegevens die in verschillende partities wonen. Bij het ontwerpen van uw service, moet u zorgvuldig overwegen deze voor- en nadelen om te komen tot een effectieve strategie voor het maken van partities.

Gaan we ervan uit dat uw toepassing is een stateful service waarmee de grootte van een winkel die u verwacht te laten groeien tot DB_Size GB in een jaar is. U bent bereid om meer toepassingen (en partities) toevoegen als u groei na dat jaar ervaring.  De totale DB_Size van invloed op de replicatie factor (RF), waarmee u het aantal replica's voor uw service bepaalt. De totale DB_Size in alle replica's is de replicatie Factor vermenigvuldigd met DB_Size.  Node_Size vertegenwoordigt de schijf ruimte/RAM-geheugen per knooppunt dat u wilt gebruiken voor uw service. De DB_Size in het geheugen moet passen binnen het cluster en een Node_Size die zich rond het RAM-geheugen van de VM moet worden gekozen voor de beste prestaties. Door het toewijzen van een Node_Size die groter is dan de capaciteit van het RAM-geheugen u erop vertrouwt dat de paginering geleverd door de runtime Fabric-Service. Dus de prestaties mogelijk niet optimaal als u uw volledige gegevens wordt beschouwd als hot (sindsdien de gegevens is wisselbaar geheugen in/out). Veel diensten waarbij slechts een fractie van de gegevens hot is, is echter meer rendabele.

Het aantal knooppunten dat nodig is voor maximale prestaties kan als volgt worden berekend:

```
Number of Nodes = (DB_Size * RF)/Node_Size

```


## <a name="account-for-growth"></a>Account voor groei

U kunt voor het berekenen van het aantal knooppunten op basis van de DB_Size die u verwacht van uw service te laten groeien tot, naast de DB_Size waarmee u bent begonnen. Vervolgens het aantal knooppunten groter groeien met uw service, zodat het aantal knooppunten te weinig niet worden ingericht. Maar het aantal partities moet worden gebaseerd op het aantal knooppunten die nodig zijn wanneer u uw service op de maximale groei uitvoert.

Het is goed om sommige extra computers op elk gewenst moment beschikbaar hebben, zodat u eventuele onverwachte pieken of storing verwerken kan (bijvoorbeeld, als een paar VMs omlaag gaan).  De extra capaciteit moet worden bepaald met behulp van de verwachte pieken, uitgangspunt is een paar extra VMs reserveren (5-10 procent extra).

Het voorgaande wordt uitgegaan van een stateful service. Als er meer dan één stateful-service, die u moet toevoegen, de DB_Size die is gekoppeld aan de andere diensten in de vergelijking. U kunt ook het aantal knooppunten afzonderlijk voor elke service stateful berekenen.  Uw service mogelijk replica's of partities niet worden verdeeld. Houd er rekening mee dat partities ook meer gegevens dan andere hebben. Zie [artikel over beste praktijken partitioneren](service-fabric-concepts-partitioning.md)voor meer informatie over het partitioneren. De bovenstaande vergelijking is echter partitie en replica agnostische omdat Fabric-Service zorgt ervoor dat de replica's zijn verdeeld tussen de knooppunten in een geoptimaliseerde manier.


## <a name="use-a-spreadsheet-for-cost-calculation"></a>Een werkblad gebruiken voor de berekening van de kosten

Nu we enkele reële getallen in de formule. Een [voorbeeld van de spreadsheet](https://servicefabricsdkstorage.blob.core.windows.net/publicrelease/SF%20VM%20Cost%20calculator-NEW.xlsx) bevat het plannen van de capaciteit voor een toepassing die drie soorten objecten bevat. Voor elk object benaderen we de grootte en hoeveel objecten dat we denken te hebben. We ook selecteren het aantal replica's we van elk objecttype willen. Het werkblad wordt berekend voor de totale hoeveelheid geheugen die in het cluster worden opgeslagen.

We voert een VM-grootte en de maandelijkse kosten. Op basis van de VM-grootte, leest het werkblad u het minimum aantal partities, die moet u uw gegevens fysiek op de knooppunten past splitsen. Gewenste een groter aantal partities aangepast aan specifieke berekening van uw toepassing en netwerkverkeer moet. Het werkblad ziet u het aantal partities die het profiel gebruikersobjecten beheert zijn verhoogd van één tot zes.

Nu, op basis van deze informatie, het werkblad weergegeven dat u de gegevens met de gewenste partities en replica's op een cluster knooppunt 26 fysiek kan ophalen. Echter zou dit cluster dicht opeen verpakt worden, zodat het gewenste aantal extra knooppunten voor storingen in knooppunten en upgrades. Ook ziet u het werkblad dat meer dan 57 knooppunten hebben geen extra waarde biedt omdat u lege knooppunten zou hebben. U wilt opnieuw gaan boven 57 knooppunten toch voor storingen in knooppunten en upgrades. U kunt het werkblad overeenkomen met de specifieke behoeften van uw toepassing aanpassen.   

![Werkblad voor de berekening van de kosten][Image1]



## <a name="next-steps"></a>Volgende stappen

[Service-configuratieservices partitionering] uitchecken[ 10] voor meer informatie over het partitioneren van uw service.



<!--Image references-->
[Image1]: ./media/SF-Cost.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-concepts-partitioning.md
