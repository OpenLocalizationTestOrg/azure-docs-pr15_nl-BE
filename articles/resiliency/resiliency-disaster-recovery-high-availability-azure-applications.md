<properties
   pageTitle="Noodherstel en hoge beschikbaarheid voor Azure toepassingen | Microsoft Azure"
   description="Technische overzichten en diepte-informatie over het ontwerpen van toepassingen voor hoge beschikbaarheid en noodherstel herstel van toepassingen die zijn gebouwd op Microsoft Azure."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="disaster-recovery-and-high-availability-for-applications-built-on-microsoft-azure"></a>Noodherstel en hoge beschikbaarheid voor toepassingen die zijn gebouwd op Microsoft Azure

##<a name="introduction"></a>Inleiding

Dit artikel gaat over hoge beschikbaarheid voor toepassingen die worden uitgevoerd in Azure. Een algemene strategie voor maximale beschikbaarheid bevat ook het aspect van het herstel na noodgevallen. Planning voor storingen en rampen in de cloud, moet u de storingen snel herkennen. Implementeer een strategie die overeenkomt met de tolerantie voor de uitvaltijd van de toepassing. U hebt bovendien rekening te houden met de omvang van de toepassing mogelijk zonder dat business nadelige gevolgen, zoals dit is hersteld zonder gegevensverlies.

De meeste bedrijven zeggen bereid om tijdelijke en grootschalige storingen. Echter, voordat u deze vraag voor uzelf beantwoorden, uw bedrijf try-out deze storingen? Het herstellen van databases om ervoor te zorgen u beschikken over de juiste processen te testen? Waarschijnlijk niet. Dat komt omdat storingen te herstellen wordt gestart met een groot aantal planning en implementatie van deze processen uitbreidt. Net als veel andere niet-functionele eisen, zoals beveiliging, haalt noodherstel zelden de aanvang analyse en toewijzing van tijd vereist. De meeste bedrijven hebben niet ook het budget voor geografisch verspreide gebieden met redundante capaciteit. Dus zijn zelfs bedrijfskritieke toepassingen vaak uitgesloten van de juiste disaster recovery planning.

Platforms zoals Azure cloud, geografisch verspreide gebieden over de hele wereld bieden. Deze platforms bieden ook mogelijkheden voor die ondersteuning bieden voor beschikbaarheid en een verscheidenheid aan herstel na storing scenario's. Nu, elke missie kritische wolk toepassing kan worden gegeven rekening voor noodgevallen testen van het systeem. Azure is tolerantie en noodherstel ingebouwd in veel van haar diensten. U moet deze functies zorgvuldig bestuderen en aanvulling van de strategieën van toepassing.

De Artikeloverzichten van dit de architectuur nodig stappen moeten rampen bewijs een Azure-implementatie. Vervolgens kunt u een groter bedrijfsproces voor continuïteit implementeren. Een plan voor bedrijfscontinuïteit is een blauwdruk voor voortgezette activiteiten onder ongunstige omstandigheden. Het is mogelijk een fout met de technologie, zoals een uitgevallen service of een natuurramp, zoals een storm of de stroom uitvalt. Tolerantie voor rampen is slechts een subset van het grotere disaster recovery proces zoals beschreven in dit document NIST: [Onvoorziene omstandigheden Planning Guide voor informatie technologie-systemen](https://www.fismacenter.com/sp800-34.pdf).

In de volgende secties definiëren voor verschillende niveaus van fouten, technieken voor het omgaan met en architecturen die ondersteuning bieden voor deze technieken. Deze informatie verschaft input aan uw disaster recovery processen en procedures om ervoor te zorgen dat uw noodherstelplan goed en efficiënt werkt.

##<a name="characteristics-of-resilient-cloud-applications"></a>Kenmerken van de verende cloud-toepassingen

Een goed architectuur-toepassing kunt opvangen mogelijkheid op tactisch niveau en het kan ook tolereren strategische systeemomvattende fouten op het regioniveau van de. In de volgende secties definiëren de terminologie waarnaar wordt verwezen in het hele document te beschrijven van verschillende aspecten van robuuste cloud services.

###<a name="high-availability"></a>Hoge beschikbaarheid

Een hoge beschikbaarheid wolk toepassing implementeert strategieën om de onderbreking van de afhankelijkheden, zoals de beheerde services van de wolk platform te absorberen. Deze aanpak staat ondanks mogelijke storingen van de mogelijkheden van de cloud-platform van de toepassing om door te gaan met de verwachte functionele en niet-functioneel systemische kenmerken vertonen. Dit valt in de Channel video 9, diepgaande [Failsafe: richtlijnen voor robuuste architecturen voor Cloud](https://channel9.msdn.com/Series/FailSafe).

Wanneer u de toepassing implementeert, moet u rekening houden met de kans dat een stroomstoring mogelijkheden. Houd ook rekening met de impact van een storing op de toepassing van het zakelijk perspectief, voordat duik diep in de strategieën voor de implementatie. Zonder vervaldatum rekening met de zakelijke gevolgen en de waarschijnlijkheid van het raken van de voorwaarde van het risico, en mogelijk onnodige duur, kan de implementatie zijn.

U kunt een auto analogie voor hoge beschikbaarheid. Zelfs onderdelen van kwaliteit en superieure techniek niet af en toe fouten. Wanneer uw auto een platte band wordt, de auto nog steeds wordt uitgevoerd maar wordt uitgevoerd met minder functionaliteit. Als u voor dit exemplaar van de potentiële gepland, kunt u een van deze reserve banden met dun-met rand totdat u bij een garage. Hoewel de band reservewiel hoge snelheden niet toestaat, kunt u het voertuig nog steeds gebruiken totdat u de band vervangen. Op dezelfde manier kunt een cloud service plannen voor het potentiële verlies van mogelijkheden voorkomen dat een relatief klein probleem waardoor u de gehele toepassing. Dit geldt ook als de cloud-service moet worden uitgevoerd met minder functionaliteit.

Er zijn enkele belangrijke kenmerken van de maximaal beschikbare cloud services: beschikbaarheid, schaalbaarheid en fouttolerantie. Deze kenmerken zijn met elkaar verbonden, is het belangrijk dat u begrijpt elke en hoe zij bijdragen tot de algehele beschikbaarheid van de oplossing.

###<a name="availability"></a>Beschikbaarheid

Een beschikbare toepassing houdt rekening met de beschikbaarheid van de onderliggende infrastructuur en de afhankelijke services. Beschikbare toepassingen verwijderen potentiële risico door redundantie en robuust ontwerp. Als u het zoekbereik rekening te houden met de beschikbaarheid in Azure vergroten, is het belangrijk dat u begrijpt het concept van de effectieve beschikbaarheid van het platform. Effectieve beschikbaarheid acht Service Level Agreements (SLA) van elke afhankelijke service en hun cumulatief effect op de beschikbaarheid van het totale systeem.

Beschikbaarheid van het systeem is de meting van het percentage van een tijdvenster dat het systeem is het mogelijk om te werken. Bijvoorbeeld, is de beschikbaarheid van SLA van ten minste twee exemplaren van een web- of werknemer rol in Azure 99.95 procent (van 100 procent). Het is niet meten de prestaties of functionaliteit van de services die worden uitgevoerd op de rollen. De effectieve beschikbaarheid van de service cloud wordt echter ook beïnvloed door de verschillende Sla's van de afhankelijke services. Meer bewegende delen in het systeem, de meer verzorging nodig zijn om te zorgen voor de toepassing resiliently voldoet aan de eisen van de beschikbaarheid van de eindgebruikers.

Houd rekening met de volgende Sla's voor een Azure service die gebruikmaakt van Azure services: Compute Azure SQL-Database en Azure opslag.

|Azure service|SLA   |Potentiële minuten downtime per maand (30 dagen)|
|:------------|:-----|:----------------------------------------:|
|Berekenen      |99.95%|21,6 minuten                              |
|SQL-Database |99,99%|4.3 minuten                              |
|Opslag      |99.90%|43,2 minuten                              |

U moet alle services mogelijk omlaag gaan op verschillende tijdstippen plannen. In het volgende eenvoudige voorbeeld is het totale aantal minuten per maand die de toepassing kan niet actief 108 minuten. Een maand 30 dagen heeft een totaal van 43,200 minuten. 108 minuten is.25 procent van het totaal aantal minuten in een maand 30 dagen (43,200 minuten). Dit biedt u een effectieve beschikbaarheid van 99.75 procent van de cloud-service.

Echter kunt met beschikbaarheid van technieken die in dit artikel wordt beschreven verbeteren dit. Bijvoorbeeld, als u uw toepassing te blijven uitvoeren als de SQL-Database niet beschikbaar is, kunt u die uit de vergelijking. Dit betekent dat de toepassing wordt uitgevoerd met beperkte mogelijkheden, zodat er ook zakelijke vereisten te overwegen. Zie voor een volledige lijst van SLA's Azure [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

###<a name="scalability"></a>Schaalbaarheid

Schaalbaarheid heeft rechtstreeks invloed op de beschikbaarheid. Een toepassing die niet onder de toegenomen belasting is niet langer beschikbaar. Schaalbare toepassingen kunnen voldoen aan de toegenomen vraag met consistente resultaten, in windows acceptabele tijd. Wanneer een systeem schaalbaar is, het horizontaal of verticaal schalen voor het beheren van stijgingen van de belasting behoud van consistente prestaties. In gewone woorden toevoegt horizontaal schalen meer machines van dezelfde grootte (processor, geheugen en bandbreedte) en verticale schaling verhoogt de grootte van de bestaande machines. Voor Azure hebt u verticale schaal opties voor het selecteren van verschillende groottes van de machine voor het berekenen. Wijzigen van de grootte van de machine is een hernieuwde implementatie vereist. Daarom zijn de meest flexibele oplossingen ontwikkeld voor het horizontaal schalen. Dit geldt met name voor het berekenen, omdat u kunt gemakkelijk verhogen het aantal actieve exemplaren van een webpagina of de werknemer. Deze extra exemplaren verwerken toegenomen verkeer via het webportaal Azure, PowerShell scripts of code. Deze beschikking voor de verhoging van de specifieke gecontroleerde metrics baseren. In dit scenario al gebruiker prestaties of maatstaven niet te merken zijn belast. De web- en werknemer rollen opslaan meestal extern een staat. Hiermee flexibel taakverdeling en correcte verwerking van wijzigingen in aantallen exemplaar. Horizontaal schalen werkt ook goed met services, zoals opslag, Azure, die geen gelaagde opties voor verticaal schalen.

Cloud implementaties moeten worden gezien als een verzameling van schaal-eenheden. Dit kan de toepassing worden in het onderhoud van de behoeften van de doorvoer van eindgebruikers elastisch. De schaaleenheden zijn gemakkelijker te visualiseren op het niveau van web- en server. Dit komt doordat de Azure biedt al stateless computerknooppunten via web en werknemer rollen. Toe te voegen dat meer schaaleenheden om de implementatie te berekenen leiden niet tot toepassing staat management nadelen, omdat ' stateless ' compute schaaleenheden zijn. Een schaal-opslageenheid is verantwoordelijk voor het beheer van een data-partitie (gestructureerde of ongestructureerde). Voorbeelden van schaal geheugeneenheden zijn partitie tabel Azure, Azure Blob-container en Azure SQL-Database. Zelfs het gebruik van meerdere accounts met Azure opslag heeft een directe invloed op de schaalbaarheid van de toepassing. U moet een zeer schaalbare cloud-service meerdere schaal-kasten opnemen ontwerpen. Bijvoorbeeld als een toepassing gebruikmaakt van relationele gegevens, staan de gegevens tussen verschillende SQL-databases. Op die manier kan de opslag bij de schaaleenheid elastische compute model te houden. Azure opslag kunt op dezelfde manier gegevens partitieschema's waarvoor doelbewuste ontwerpen om te voldoen aan de behoeften van de doorvoer van de compute-laag. Zie [Aanbevolen procedures voor het ontwerpen van Large-Scale Services op Azure Cloud Services](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/)voor een lijst met aanbevolen procedures voor het ontwerpen van schaalbare cloud services.

###<a name="fault-tolerance"></a>Fouttolerantie

Toepassingen ervan uit dat elke mogelijkheid afhankelijke cloud kunt en gaat u op een bepaald moment in de tijd. Een fouttolerante toepassing veroorzaakt detecteert en manoeuvres om mislukte elementen, en het juiste resultaat binnen een bepaalde periode. Voor foutsituaties, zal een fouttolerant systeem veroorzaakt een beleid opnieuw gebruiken. Voor meer ernstige fouten, de toepassing problemen detecteren en failover uitvoeren naar andere hardware of rampenplannen totdat de fout is gecorrigeerd. Een betrouwbare toepassing kunt juist het uitvallen van een of meer onderdelen te beheren en verder goed werken. Fout tolerant toepassingen kunnen een of meer design strategieën, zoals redundantie of replicatie gedegradeerde functionaliteit gebruiken.

##<a name="disaster-recovery"></a>Noodherstel

Een implementatie van de cloud kan niet langer functioneren als gevolg van een stroomstoring systemische van de afhankelijke services of de onderliggende infrastructuur. Onder dergelijke omstandigheden tot een plan voor bedrijfscontinuïteit het disaster recovery proces. Dit proces omvat meestal zowel operationele staf en geautomatiseerde procedures om de toepassing in een beschikbaar gebied te activeren. Hiervoor moet de overdracht van gebruikers van toepassingen, gegevens en services op de nieuwe regio. Het betreft ook het gebruik van back-upmedia of voortdurende replicatie.

U kunt de vorige analogie die ten opzichte van de beschikbaarheid van de mogelijkheid om te herstellen van een platte band met behulp van een reserve. Herstel na noodgevallen worden daarentegen de maatregelen die zijn genomen na een crash auto waarbij de auto niet meer operationeel is. In dat geval is de beste oplossing een efficiënte manier om auto's, wijzigen door het aanroepen van de service van een reis of een vriend te vinden. In dit scenario wordt zal er waarschijnlijk langer duren bij ophalen van terug op de weg. Er is ook complexer in herstellen en terugkeren naar het oorspronkelijke voertuig. Herstel naar een andere regio is op dezelfde manier, een complexe taak die betekent meestal dat sommige uitvaltijd en kunnen gegevens verloren gaan. Om beter te begrijpen en disaster recovery strategieën te evalueren, is het belangrijk dat de twee termen definiëren: herstel tijd doelstelling (RTO) en herstel punt doelstelling (vrijgegeven Productieorder).

###<a name="recovery-time-objective"></a>Herstel tijd doelstelling

De RTO is de maximale tijdsduur voor het herstellen van de functionaliteit van de toepassing is toegewezen. Dit is gebaseerd op zakelijke behoeften en het belang van de toepassing is gekoppeld. Bedrijfskritieke toepassingen vereisen een lage RTO.

###<a name="recovery-point-objective"></a>Herstel punt doelstelling

De vrijgegeven Productieorder is het aanvaardbaar tijdvenster van verloren gegevens als gevolg van het herstelproces. Bijvoorbeeld als de vrijgegeven Productieorder één uur is, moet u volledig back-up of de gegevens ten minste eenmaal per uur repliceren. Zodra u de toepassing weer in een andere regio, de back-upgegevens ontbreekt mogelijk een uur lang van gegevens. Zoals RTO richten bedrijfskritieke toepassingen op een veel kleinere vrijgegeven Productieorder.

##<a name="checklist"></a>Controlelijst

Vatten we de belangrijkste punten die zijn behandeld in dit artikel (en de verwante artikelen voor [hoge beschikbaarheid](./resiliency-high-availability-azure-applications.md) en [Noodherstel](./resiliency-disaster-recovery-azure-applications.md) voor Azure toepassingen). Dit overzicht zal fungeren als een checklist van items, die kunt u voor uw eigen beschikbaarheid en het oplossen van problemen overwegen. Dit zijn aanbevelingen die nuttig zijn voor klanten die willen krijgen ernstig over het implementeren van een succesvolle oplossing zijn.

1. Voor elke toepassing een risico-evaluatie uitvoeren omdat elk verschillende eisen hebben. Sommige toepassingen zijn belangrijker dan anderen en de extra kosten om ze te bouwen voor noodherstel zou rechtvaardigen.
1. Deze informatie gebruiken voor het definiëren van de RTO en de vrijgegeven Productieorder voor elke toepassing.
1. Ontwerp, beginnend met de toepassingsarchitectuur is mislukt.
1. Aanbevolen procedures voor hoge beschikbaarheid implementeren terwijl kosten, complexiteit en risico's.
1. Disaster recovery plannen en processen implementeren.
  * U kunt storingen die zich uitstrekken over moduleniveau naar een volledige wolk stroomstoring.
  * Back-upstrategieën voor referentie- en transactionele gegevens vast.
  * Kies een architectuur met meerdere rampen herstel.
1. Definieer een specifieke eigenaar voor disaster recovery processen, automatisering en testen. De eigenaar moet beheren en de eigenaar van het hele proces.
1. De processen documenteren zodat deze gemakkelijk herhaalbaar zijn. Hoewel één eigenaar, zijn meerdere mensen moeten kunnen begrijpen en volgt u de processen in geval van nood.
1. Het treinpersoneel het proces uit te voeren.
1. Gebruik gewone ramp simulaties voor trainings- en validatie van het proces.

##<a name="summary"></a>Samenvatting

Wanneer de hardware of toepassingen niet in Azure, zijn technieken en strategieën voor het beheer ervan anders dan wanneer de fout op systemen voor bedrijven optreedt. De belangrijkste reden hiervoor is dat cloud oplossingen doorgaans meer afhankelijk zijn van de infrastructuur die verspreid zijn over een gebied van Azure en beheerd als afzonderlijke services. U moet omgaan met gedeeltelijke fouten met hoge beschikbaarheid-technieken. Gebruik disaster recovery strategieën ernstige fouten, mogelijk als gevolg van een gebeurtenis ramp beheren.

Azure detecteert en veel fouten worden verwerkt, maar er zijn vele soorten storingen die toepassing specifieke strategieën nodig. Als u actief voorbereiden en de fouten van de toepassingen, services en gegevens beheren.

Wanneer u de beschikbaarheid van uw toepassing en de calamiteitenplan, kunt u de zakelijke gevolgen van niet-van de toepassing. Definiëren van de processen, beleid en procedures om de kritieke systemen herstellen nadat een catastrofale gebeurtenis tijd kost, planning en inzet. En als u eenmaal de plannen, u er niet kan stoppen. U moet regelmatig analyseren, testen en voortdurend verbeteren van de plannen op basis van uw toepassingsportefeuille zakelijke behoeften en de technologieën die beschikbaar zijn. Azure biedt nieuwe mogelijkheden en leidt tot nieuwe uitdagingen voor het maken van krachtige toepassingen die bestand is tegen storingen.

##<a name="additional-resources"></a>Aanvullende bronnen

[Hoge beschikbaarheid voor toepassingen die zijn gebouwd op Microsoft Azure](resiliency-high-availability-azure-applications.md)

[Herstel na noodgevallen voor toepassingen die zijn gebouwd op Microsoft Azure](resiliency-disaster-recovery-azure-applications.md)

[Technische richtsnoeren Azure tolerantie](resiliency-technical-guidance.md)

[Overzicht: Cloud business continuity en database noodherstel met SQL-Database](../sql-database/sql-database-business-continuity.md)

[Hoge beschikbaarheid en noodherstel herstel voor SQL Server in Azure virtuele Machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md)

[Failsafe: Richtlijnen voor robuuste wolk architecturen](https://channel9.msdn.com/Series/FailSafe)

[Aanbevolen procedures voor het ontwerp van grootschalige services op Azure Cloud Services](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/)

##<a name="next-steps"></a>Volgende stappen

Dit artikel maakt deel uit van een serie artikelen gericht op herstel na storingen en hoge beschikbaarheid voor Azure toepassingen. Het volgende artikel in deze serie is een [hoge mate van beschikbaarheid voor toepassingen die zijn gebouwd op Microsoft Azure](resiliency-high-availability-azure-applications.md).
