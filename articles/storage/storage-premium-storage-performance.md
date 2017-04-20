<properties
    pageTitle="Azure Premium opslag: Ontwerpen voor prestaties | Microsoft Azure"
    description="Krachtige toepassingen met Azure Premium opslag ontwerpen. Premium-opslagruimte biedt schijfondersteuning voor I/O-intensieve werkbelasting Azure virtuele Machines met hoge prestaties, lage latentie."
    services="storage"
    documentationCenter="na"
    authors="aungoo-msft"
    manager="tadb"
    editor="tysonn" />

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="aungoo"/>

# <a name="azure-premium-storage-design-for-high-performance"></a>Azure Premium opslag: Ontwerp voor hoge prestaties

## <a name="overview"></a>Overzicht  
Dit artikel bevat richtlijnen voor het bouwen van krachtige toepassingen met Azure Premium opslag. Kunt u de instructies in dit document gecombineerd met best practices prestaties van toepassing op technologieën die door de toepassing worden gebruikt. Ter illustratie van de richtsnoeren hebben we SQL Server wordt uitgevoerd op de premie opslag als voorbeeld in dit document gebruikt.

Hoewel we prestaties scenario's voor de opslaglaag in dit artikel, moet u voor het optimaliseren van de toepassingslaag. Bijvoorbeeld als u een SharePoint-Farm op Azure Premium opslag beheert, kunt u de SQL Server-voorbeelden uit dit artikel voor het optimaliseren van de databaseserver. Daarnaast optimaliseren van de SharePoint-Farm webserver en toepassingsserver om de meeste prestaties te behalen.

Dit artikel helpt beantwoorden Veelgestelde vragen over het optimaliseren van de toepassingsprestaties op Azure Premium opslag, na

-   Het meten van de toepassingsprestaties van uw?  
-   Waarom ziet u geen verwachte hoge prestaties?  
-   Welke factoren van invloed op de toepassingsprestaties van uw op Premium opslag?  
-   Hoe deze factoren van invloed op de prestaties van uw toepassing op Premium opslag?  
-   Hoe kunt u optimaliseren voor IOP's, bandbreedte en de latentie  

We hebben deze richtlijnen die specifiek voor de premie opslag omdat werkbelasting op Premium opslag uiterst gevoelige prestaties zijn. We hebben voorbeelden verstrekt in voorkomend geval. U kunt ook sommige van deze richtlijnen voor toepassingen op IaaS VMs met standaard schijven.

Voordat u begint, als u naar de opslag van de premie, lees eerst de [premie opslag: krachtige opslag voor Azure VM werklasten](storage-premium-storage.md) artikel en [Azure Premium opslag schaalbaarheid en prestaties, doelen](storage-scalability-targets.md#premium-storage-accounts).

## <a name="application-performance-indicators"></a>Toepassing prestatie-indicatoren  
We beoordelen of een toepassing uitvoert met behulp van prestatie-indicatoren, zoals goed of niet, hoe snel een aanvraag, over hoeveel gegevens een toepassing wordt verwerkt per aanvraag wordt verwerkt door een toepassing, hoeveel aanvragen een toepassing in een bepaalde periode, hoe lang die een gebruiker heeft om te wachten op een antwoord te krijgen na hun aanvraag verwerkt is. De technische voorwaarden voor deze prestatie-indicatoren zijn IOP's, doorvoer of bandbreedte en de latentie.

In dit gedeelte bespreken we de algemene prestatie-indicatoren in het kader van de premie opslag. In het volgende gedeelte, de vereisten van de toepassing verzamelen, leert u hoe deze prestatie-indicatoren voor uw toepassing. Verderop in het optimaliseren van prestaties van toepassingen leert u over de factoren die deze prestatie-indicatoren en aanbevelingen voor het optimaliseren van hen.

## <a name="iops"></a>IOP 'S  
IOP's is het aantal aanvragen dat uw toepassing worden verzonden naar de schijven in één seconde. Een i/o-bewerking kan lezen of schrijven, de volgorde of in willekeurige. OLTP-toepassingen zoals een on line retail website moeten onmiddellijk veel gelijktijdige aanvragen verwerken. Verzoek van de gebruiker zijn invoegen en intensieve databasetransacties die de toepassing, snel verwerken moet bijwerken. OLTP-toepassingen moeten daarom zeer hoge IOP's. Dergelijke toepassingen verwerken miljoenen kleine en willekeurig i/o-aanvragen. Als u een toepassing hebt, moet u de toepassing-infrastructuur te optimaliseren voor IOP's ontwerpen. In het gedeelte, *Optimaliseren van prestaties*, besproken in detail de factoren waarmee u rekening houden moet om hoge IOP's.

Wanneer u een premie opslagschijf toegevoegd op grote schaal VM, Azure bepalingen voor u een gegarandeerd aantal IOP's aan de hand van de specificatie van de schijf. Een schijf P30 bepalingen bijvoorbeeld 5000 IOP's. Elke grote schaal VM-grootte heeft ook een specifieke IOP's beperken dat het bestand tegen. Zo heeft een standaard GS5 VM 80.000 IOP's beperken.

## <a name="throughput"></a>Doorvoer  
Doorvoer of bandbreedte is de hoeveelheid gegevens die door uw toepassing worden verzonden naar de schijven in een opgegeven interval. Als uw toepassing invoer/uitvoer-bewerkingen met grote formaten voor i/o-eenheid uitvoert, moet hoge gegevensdoorvoer. Magazijn-gegevenstoepassingen vaak intensieve scanbewerkingen die grote delen van de gegevens tegelijk openen en uitvoeren van bulkbewerkingen vaak uitgeven. Dergelijke toepassingen moet met andere woorden, hogere doorvoer. Als u een toepassing hebt, moet u de infrastructuur te optimaliseren voor doorvoer van ontwerpen. In het volgende gedeelte bespreken we in detail de factoren die u hiervoor moet afstemmen.

Wanneer u een premie opslagschijf toegevoegd aan een hoge schaal VM, Azure bepalingen doorvoer aan de hand van die schijf specificatie. Een schijf P30 bepalingen bijvoorbeeld 200 MB per schijf doorvoer van de tweede. Elke grote schaal VM-grootte heeft ook als specifieke doorvoer beperken die deze kan handhaven. Standaard GS5 VM heeft bijvoorbeeld een maximale capaciteit van 2.000 MB per seconde.

Er is een relatie tussen de doorvoersnelheid en IOP's zoals in de onderstaande formule.

![](media/storage-premium-storage-performance/image1.png)

Daarom is het belangrijk om te bepalen van de optimale doorvoer en IOP's waarden die uw toepassing nodig heeft. Als u een optimaliseren probeert, wordt de andere ook beïnvloed. Verderop *Prestaties optimaliseren*, bespreken we met meer details over het optimaliseren van IOP's en de doorvoer.

## <a name="latency"></a>Latentie  
Latentie is de tijd die nodig is een toepassing die een aanvraag ontvangen, verzenden naar de schijven en het antwoord naar de client verzenden. Dit is een essentiële maatstaf voor prestaties IOP's en de doorvoer van de toepassing. De latentie van een opslagschijf premium is de tijd die nodig is om de gegevens ophalen voor een aanvraag en het terug naar de toepassing te communiceren. Premium-opslagruimte biedt een consistent lage vertragingstijden. Als u alleen-lezen-host caching op de schijven premium inschakelt, kunt u veel lagere latentie lezen krijgen. Bespreken we schijfcaching in verderop uitgebreider op *Prestaties optimaliseren*.

Wanneer u uw toepassing voor hogere IO's / s en een doorvoer optimaliseert, geldt dat de latentie van uw toepassing. Na het afstemmen van de prestaties van de latentie van de toepassing om te voorkomen dat onverwachte hoge latentie gedrag altijd te evalueren.

## <a name="gather-application-performance-requirements"></a>Verzamelen van prestatie-eisen van toepassing  
De eerste stap bij het ontwerpen van krachtige toepassingen op Azure Premium opslag is te begrijpen van de prestatie-eisen van uw toepassing. Na het verzamelen van prestatie-eisen, kunt u de toepassing naar de meest optimale prestaties kunt optimaliseren.

In het vorige gedeelte beschreven we de gemeenschappelijke prestatie-indicatoren, IOP's, doorvoer en latentie. U moet aangeven welke van deze prestatie-indicatoren zijn essentieel voor uw toepassing kan de gewenste gebruikerservaring bieden. Bijvoorbeeld belangrijkste hoge IOP's miljoenen transacties verwerken in een tweede OLTP-toepassingen. Overwegende dat hoge doorvoer essentieel voor de verwerking van grote hoeveelheden gegevens in een tweede Data Warehouse-toepassingen is. Zeer lage latentie is cruciaal voor realtime-toepassingen zoals live video streaming websites.

Meet vervolgens de maximale prestatie-eisen van uw toepassing gedurende de levensduur. De controlelijst voor voorbeeld gebruiken als een begin. De maximale prestatie-eisen tijdens de normale, piek en rustige uren perioden van werkbelasting vastleggen. Vereisten voor alle niveaus van de werklast aangeeft, worden wordt de algehele vereiste prestaties van uw toepassing bepalen. De normale werklast van een e-commerce-website is bijvoorbeeld de transacties die het dient gedurende de meeste dagen in een jaar. De werkbelasting van de piek van de website worden de transacties die het dient tijdens de feestdagen of speciale verkoop-gebeurtenissen. De piek belasting wordt meestal ervaren gedurende een beperkte periode, maar kan uw toepassing voor het schalen van twee of meer keer de normale werking nodig. Ontdek het 50 percentiel, 90-percentiel en 99 percentiel eisen. Dit helpt bij het uitfilteren van eventuele uitschieters in de prestatie-eisen en u kunt uw inspanningen zich richten op het optimaliseren voor de juiste waarden.

**Controlelijst voor de vereisten van de toepassing prestaties**

| **Prestatie-eisen** | **50 percentiel** | **90-percentiel** | **99 percentiel** |
|---|---|---|---|
| Max. Transacties per seconde | | | |
| Leesbewerkingen %            | | | |
| Schrijfbewerkingen %           | | | |
| % Willekeurig bewerkingen          | | | |
| % Opeenvolgende bewerkingen      | | | |
| Grootte van i/o-aanvragen              | | | |
| Gemiddelde doorvoer           | | | |
| Max. Doorvoer              | | | |
| Min. Latentie                 | | | |
| Gemiddelde latentie              | | | |
| Max. CPU                     | | | |
| Gemiddelde CPU                  | | | |
| Max. Geheugen                  | | | |
| Gemiddelde geheugen               | | | |
| Wachtrijlengte                  | | | |

>**Belangrijke opmerking:**  
>U kunt deze nummers op basis van de verwachte toekomstige groei van uw toepassing aanpassen. Is een goed idee om te plannen voor de groei van tevoren, omdat deze mogelijk moeilijker de infrastructuur voor het verbeteren van de prestaties van later wijzigen.

Als u een bestaande toepassing hebt en wilt verplaatsen naar de premie opslag, eerst de controlelijst voor de hierboven voor de bestaande toepassing te bouwen. Vervolgens een prototype van de toepassing van de premie opslag maken en ontwerpen van de toepassing op basis van richtsnoeren die worden beschreven in *Prestaties optimaliseren* in verderop in dit document. Het volgende gedeelte worden de hulpprogramma's die u gebruiken kunt voor het verzamelen van de metingen van de prestaties beschreven.

Een controlelijst maken die vergelijkbaar is met de bestaande toepassing voor het prototype. Met Benchmarking tools kunt u de werklast te simuleren en meten van de prestaties van de toepassing van het prototype. Zie de sectie over [Benchmarking](#benchmarking) voor meer informatie. Hierdoor kunt u bepalen of Premium opslag kunt voldoen aan of groter zijn dan de prestatie-eisen van uw toepassing. Vervolgens kunt u dezelfde richtlijnen voor de productietoepassing implementeren.

### <a name="counters-to-measure-application-performance-requirements"></a>Prestatiemeteritems voor metingen van de prestatie-eisen van toepassing  
De beste manier voor het meten van prestatie-eisen van uw toepassing, is prestaties-hulpprogramma's die door het besturingssysteem van de server. U kunt Prestatiemeter voor Windows en iostat voor Linux. Deze extra vastleggen items overeenkomt met elke maatregel die is beschreven in de bovenstaande sectie. U moet de waarden van deze tellers vastleggen wanneer uw toepassing wordt uitgevoerd, zijn normaal, piek en rustige uren werkbelastingen.

De prestatiemeters zijn beschikbaar voor de processor, geheugen, en elke logische schijf en de fysieke schijf van uw server. Wanneer u met een VM premium schijven, de fysieke schijf-items voor elke schijf premium opslag zijn en logische-schijfitems zijn voor elk volume op de schijven voor premium gemaakt. U moet de waarden voor de schijven die host zijn van uw toepassing werkbelasting vastleggen. Als er een één-op-één-toewijzingen tussen logische en fysieke schijven, kunt u verwijzen naar de fysieke schijf-items; Raadpleeg anders de logische-schijfitems. Op Linux genereert de opdracht iostat een rapport over CPU- en ruimtegebruik. Het rapport schijf capaciteit biedt statistieken per apparaat of partitie. Als u een databaseserver met de gegevens- en logboekbestanden op afzonderlijke schijven, verzamelen van deze gegevens voor beide schijven. Onderstaande tabel beschrijft items voor schijven, processor en geheugen:

| Teller | Beschrijving | PerfMon | Iostat |
|---|---|---|---|
| **IOP's of transacties per seconde** | Aantal i/o-aanvragen naar de opslagschijf per seconde worden uitgegeven. | Schijf lezen per seconde <br> Schijf schrijven per seconde | tps <br> r/s <br> w/s |
| **Schijf lees- en schrijfbewerkingen** | % leesbewerkingen en schrijfbewerkingen op de schijf wordt uitgevoerd. | Lezen schijftijd <br> Schrijven voor percentage schijftijd | r/s <br> w/s |
| **Doorvoer** | De hoeveelheid gegevens gelezen van of geschreven naar de schijf per seconde. | Schijf gelezen Bytes per seconde <br> Geschreven Bytes per seconde | kB_read/s <br> kB_wrtn/s |
| **Latentie** | Totale tijd voor het voltooien van een schijf-i/o-verzoek. | Gemiddelde schijf leestijd <br> Schrijftijd schijf gemiddelde | Wacht tot <br> svctm |
| **I/o-grootte** | De grootte van de I/O vraagt om problemen met de schijven. | Gemiddeld aantal gelezen bytes <br> Gemiddeld aantal geschreven Bytes | avgrq sz |
| **Wachtrijlengte** | Aantal uitstaande I/O aanvragen wachten op formulier lezen of schrijven naar de opslagschijf. | Huidige wachtrijlengte voor schijf | avgqu sz |
| **Max. Geheugen** | Hoeveelheid geheugen voor toepassingen probleemloos uitvoeren | Percentage toegewezen Bytes in gebruik | Vmstat gebruiken |
| **Max. CPU** | Vereiste toepassingen probleemloos CPU bedrag | Percentage processortijd | % util |

Meer weten over [iostat](http://linuxcommand.org/man_pages/iostat1.html) en [PerfMon](https://msdn.microsoft.com/library/aa645516.aspx).


## <a name="optimizing-application-performance"></a>Prestaties optimaliseren  
De belangrijkste factoren die invloed hebben op de prestaties van een toepassing wordt uitgevoerd op de premie opslag zijn aard van i/o-aanvragen, VM-grootte, de grootte van de schijf, het aantal schijven, schijfcaching, Multithreading en diepte van de wachtrij. Enkele van deze factoren kunt u bepalen met knoppen die door het systeem. De meeste toepassingen mogelijk kunt u een optie voor het direct wijzigen van het i/o-grootte en de diepte van de wachtrij. Bijvoorbeeld, als u SQL Server gebruikt, kiezen u niet de i/o-grootte en de wachtrij diepte. SQL Server kiest de optimale IO wachtrij diepte waarden voor grootte en om de meeste prestaties te behalen. Het is belangrijk om te weten van de gevolgen van beide soorten factoren voor de toepassingsprestaties van uw, zodat kunt u geschikte resources aan prestaties wensen inrichten.

In deze sectie verwijzen naar de toepassing controlelijst voor de vereisten die u hebt gemaakt om te bepalen hoeveel u moet uw de toepassingsprestaties te optimaliseren. Op basis van dat, kun je bepalen welke factoren van dit gedeelte moet u afstemmen. Te wonen de gevolgen van elke factor voor de toepassingsprestaties van uw, benchmarking tools op het installatieprogramma van de toepassing worden uitgevoerd. Raadpleeg de sectie [Benchmarking](#Benchmarking) aan het einde van dit artikel voor algemene benchmarking tools voor Windows en Linux VMs uitvoeren.

### <a name="optimizing-iops-throughput-and-latency-at-a-glance"></a>Optimaliseren van IOP's, doorvoer en latentie in één oogopslag  
In de volgende tabel bevat een overzicht van alle prestatiefactoren en de stappen voor het optimaliseren van IOP's, doorvoer en latentie. De secties die volgen dit overzicht vindt u elke factor is veel meer diepte.

| | **IOP 'S** | **Doorvoer** | **Latentie** |
|---|---|---|---|
| **Voorbeeldscenario** | Enterprise OLTP-toepassing waarvoor zeer hoge transacties per tweede tarief.                                                                                                                                 | Enterprise Data warehousing toepassing verwerking van grote hoeveelheden gegevens. | In de buurt van realtime-toepassingen waarbij direct antwoorden op aanvragen van gebruikers, zoals online games. |
| Prestatiefactoren  | | | |
| **I/o-grootte** | Kleinere i/o-hogere IOP's levert.                                                                                                                                                                           | I/o-groter om levert hogere doorvoer. | |
| **VM-grootte** | Gebruik een VM-formaat dat groter is dan de vereiste toepassing IOP's biedt. Zie VM-formaten en hun grenzen IOP's hier. | VM-grootte met doorvoer limiet groter is dan de behoefte van uw toepassing gebruiken. Zie VM formaten en hier hun grenzen voor de doorvoer. | Een VM-formaat dat aanbiedingen grenzen groter is dan de behoefte van uw toepassing schalen gebruiken. Zie VM formaten en hier hun grenzen. |
| **Grootte van de schijf** | Gebruik een grootte van de schijf die groter is dan de vereiste toepassing IOP's biedt. Zie schijfgrootte en hun grenzen IOP's hier. | De grootte van een schijf met doorvoer limiet groter is dan de behoefte van uw toepassing gebruiken. Zie schijfgrootte en hier hun grenzen voor de doorvoer. | Grootte van de schijf dat aanbiedingen grenzen groter is dan de behoefte van uw toepassing schalen gebruiken. Zie schijfgrootte en hier hun grenzen. |
| **VM en schijfruimtelimiet schaal** | IOP's limiet van de gekozen VM-grootte moet groter zijn dan totaal IOP's als gevolg van de premie de schijven is gekoppeld. | Limiet voor doorvoer van de gekozen VM-grootte moet groter zijn dan totale doorvoer als gevolg van de premie de schijven is gekoppeld. | Schaal grenzen van de gekozen VM-grootte moeten groter zijn dan totale schaal grenzen van premium gekoppelde schijven. |
| **Schijfcaching** | Alleen-lezen Cache inschakelen op schijven met zware leesbewerkingen om hogere lezen IOP's voor premium. | | Alleen-lezen Cache inschakelen op een premie opslag schijf gereed zware bewerkingen voor zeer lage lezen vertragingstijden. |
| **Schijf-Striping** | Gebruik meerdere schijven en stripe ze samen een gecombineerde hogere grenswaarde IOP's en de doorvoer. Houd er rekening mee dat de gecombineerde limiet per VM hoger dan de gecombineerde limieten van premium gekoppelde schijven moet worden. | |
| **Stripe-grootte** | Kleinere stripe voor willekeurige kleine i/o-patroon weergegeven in een OLTP-toepassingen. Gebruik bijvoorbeeld stripe-grootte van 64KB voor SQL Server OLTP-toepassing. | Grotere stripe voor opeenvolgende grote i/o-patroon gezien in de Data Warehouse-toepassingen. Gebruik bijvoorbeeld 256KB stripe-grootte voor SQL Server-gegevens magazijn toepassing. | |
| **Multithreading** | Gebruik om hogere aantal aanvragen naar de premie opslag die tot hogere IO's / s en een doorvoer leiden multithreading. Stel bijvoorbeeld een hoge waarde MAXDOP toe te wijzen meer CPU's met SQL Server op SQL Server. | |
| **Wachtrijlengte** | Grotere wachtrijlengte leidt tot hogere IOP's. | Grotere wachtrijlengte levert snellere doorvoer. | Kleinere wachtrijlengte leidt tot lagere vertragingstijden. |

## <a name="nature-of-io-requests"></a>Aard van de i/o-aanvragen  
Een i/o-verzoek is een eenheid van i/o-bewerking die uw toepassing uitvoeren. Identificatie van de aard van de i/o-aanvragen, willekeurige of sequentiële, kunt lezen of schrijven, klein of groot is, u de prestatie-eisen van uw toepassing bepalen. Het is zeer belangrijk om te begrijpen van de aard van de i/o-aanvragen om de juiste beslissingen tijdens het ontwerpen van uw toepassing infrastructuur.

I/o-grootte is een van de belangrijkste factoren. De i/o-grootte is de grootte van de bewerkingsaanvraag i/o-is gegenereerd door de toepassing. De grootte van het i/o heeft een belangrijke invloed op de prestaties, met name op het IOP's en de bandbreedte die door de toepassing kan bereiken. De volgende formule wordt de relatie tussen IOP's, i/o-grootte en bandbreedte/doorvoer.  
    ![](media/storage-premium-storage-performance/image1.png)

In sommige toepassingen kunt u het i/o-formaat wijzigen en bepaalde toepassingen niet. Bijvoorbeeld SQL Server bepaalt de optimale grootte voor i/o zelf en biedt gebruikers de knoppen te wijzigen. Aan de andere kant Oracle biedt een parameter met de naam [DB\_blokkeren\_grootte](https://docs.oracle.com/cd/B19306_01/server.102/b14211/iodesign.htm#i28815) die u kunt configureren met de grootte van het i/o-verzoek van de database.

Gebruik de richtlijnen in dit artikel als u een toepassing u niet de i/o-grootte te wijzigen kunt, de KPI's die het meest relevant is voor uw toepassing de prestaties te optimaliseren. Bijvoorbeeld:

-   Een toepassing OLTP miljoenen kleine en willekeurig i/o-verzoeken gegenereerd. Dit type i/o-aanvragen verwerken, moet u uw infrastructuur toepassingen om hogere IOP's ontwerpen.  
-   Een toepassing voor gegevensopslag genereert grote en opeenvolgende i/o-aanvragen. Dit type i/o-aanvragen verwerken, moet u uw infrastructuur toepassingen met hogere bandbreedte of doorvoer ontwerpen.

Als u een toepassing waarmee u het i/o-formaat wijzigen, deze vuistregel voor de i/o-prestaties bij andere richtlijnen afmetingen gebruiken

-   Kleinere IO hogere IOP's ophalen. Bijvoorbeeld 8 KB voor OLTP-toepassingen.  
-   Grotere i/o om hogere bandbreedte en/of doorvoer. Bijvoorbeeld: 1024 KB voor de toepassing van een magazijn.

Hier volgt een voorbeeld van hoe u het IOP's en de doorvoer/bandbreedte voor uw toepassing kunt berekenen. U kunt een toepassing met behulp van een schijf P30. De maximale IOP's en een schijf P30 doorvoer/bandbreedte kunt bereiken is 5000 IOP's en 200 MB per seconde. Nu, als uw toepassing de maximale IOP's van de P30 schijf vereist en u een i/o-kleiner als 8 KB, is de resulterende bandbreedte is het mogelijk om 40 MB per seconde. Als uw toepassing de maximale doorvoer/bandbreedte van P30 schijf moet en u een grotere i/o zoals 1024 KB, de resulterende IOP's zijn echter kleiner, 200 IOP's. De i/o-grootte dus afstemmen, dat zowel de toepassing IOP's en doorvoer/bandbreedte-eis voldoet aan. In de volgende tabel bevat een overzicht van de verschillende i/o-formaten en hun bijbehorende IOP's en doorvoer voor een schijf P30.

| **De vereiste toepassing** | **I/o-grootte** | **IOP 'S** | **Doorvoer/bandbreedte** |
|-----------------------------|--------------|----------|--------------------------|
| Max IOP 's                    | 8 KB         | 5.000    | 40 MB per seconde         |
| Doorvoer van max.              | 1024 KB      | 200      | 200 MB per seconde        |
| Max Throughput + hoge IOP 's  | 64 KB        | 3200    | 200 MB per seconde        |
| Max IOPS + hoge doorvoer  | 32 KB        | 5.000    | 160 MB per seconde        |

Als u IOP's en bandbreedte hoger zijn dan de maximale waarde van een eenmalige premie opslagschijf, gebruikt u meerdere premium schijven samen gestreept. Stripe bijvoorbeeld twee P30 schijven als u een gecombineerde IOP's van 10.000 IOP's of een gecombineerde gegevensdoorvoer van 400 MB per seconde. Zoals uiteengezet in de volgende sectie, moet u een VM-grootte die de gecombineerde ondersteunt schijf IO's / s en de doorvoer.

>**Opmerking:**  
>IOP's of de andere ook verhoogt de doorvoer verhogen, moet dat u doorvoer of beperkingen van de schijf of VM IOP's niet raakt bij een toenemende.

Te wonen de gevolgen van i/o-grootte voor de prestaties van toepassingen, kunt u extra benchmarking uitvoeren op de VM en schijven. Zie de gevolgen maken meerdere test wordt uitgevoerd en andere i/o-grootte voor elke uitvoering. Raadpleeg de sectie [Benchmarking](#Benchmarking) aan het einde van dit artikel voor meer informatie.

## <a name="high-scale-vm-sizes"></a>Hoge schaal VM formaten  
Wanneer u begint met het ontwerpen van een toepassing, is een van de eerste dingen om te doen kiezen een VM als host voor uw toepassing. Premium opslag wordt geleverd met een hoge schaal VM-formaten die toepassingen waarvoor meer rekenkracht en een hoge lokale schijf i/o-prestaties kunnen worden uitgevoerd. Deze VMs voorzien in snellere processors een hogere geheugen-core-verhouding en een Solid-State Drive (SSD) de lokale schijf. Voorbeelden van hoge schaal VMs ondersteunende Premium opslag zijn de DS, DSv2 en GS serie VMs.

Hoge schaal VMs zijn beschikbaar in verschillende formaten met een ander aantal CPU cores, geheugen, OS en de grootte van de tijdelijke schijf. De grootte van elke VM heeft ook het maximum aantal gegevensschijven die u aan de VM koppelen kunt. Daarom worden de gekozen VM-grootte is van invloed op hoeveel bewerkingen, geheugen, en opslagcapaciteit beschikbaar is voor uw toepassing. Heeft dit ook gevolgen voor het berekenen en de opslag. Hieronder ziet u bijvoorbeeld de specificaties van de grootste VM-grootte in de DS-serie, DSv2 en de reeks een GS:

| VM-grootte | CPU-kernen | Geheugen | VM schijfgrootten | Max. gegevensschijven | Cachegrootte | IOP 'S | Bandbreedte-i/o-Cache beperkt |
|---|---|---|---|---|---|---|---|
| Standard_DS14 | 16 | 112 GB | OS = 1023 GB <br> Lokale SSD = 224 GB | 32 | 576 GB | 50.000 IOP 'S <br> 512 MB per seconde | 4000 IOP's en 33 MB per seconde |
| Standard_GS5 | 32 | 448 GB | OS = 1023 GB <br> Lokale SSD = 896 GB | 64 | 4224 GB | 80.000 IOP 'S <br> 2.000 MB per seconde | 5.000 IOP's en 50 MB per seconde |

Als u wilt een complete lijst weergeven van alle beschikbare formaten voor Azure VM, verwijzen naar [Windows VM formaten](../virtual-machines/virtual-machines-windows-sizes.md) of [Linux VM formaten](../virtual-machines/virtual-machines-linux-sizes.md). Kies een VM-grootte die kan voldoen en schalen naar de gewenste toepassing prestatie-eisen. Ook rekening houden met volgende belangrijke overwegingen bij het kiezen van VM formaten.


*Schaal limieten*  
De maximale grenzen IOP's per VM en per schijf zijn verschillende en onafhankelijk van elkaar. Zorg ervoor dat de toepassing IOP's binnen de grenzen van de VM als premium aangesloten schijven wordt bestuurd. Anders prestaties van toepassingen te maken met beperking.

Stel bijvoorbeeld dat een vereiste toepassing is maximaal 4.000 IOP's. Hiertoe moet u een schijf P30 op een VM DS1 inrichten. De P30-schijf kan maximaal 5.000 IOP's bieden. Maar is de DS1-VM beperkt tot 3200 IOP's. Bijgevolg zal de prestaties van toepassingen worden beperkt door de limiet VM op 3200 IOP's en worden de prestaties verslechteren. Om dit te voorkomen, kies een VM en schijf grootte dat beide voldoen aan de vereisten van de toepassing.

*Kosten van de verrichting*  
In veel gevallen is het mogelijk dat de totale kosten van bewerkingen op basis van de premie opslag lager is dan standaard opslag.

Neem bijvoorbeeld een toepassing 16.000 IOP's vereisen. Voor deze prestaties, moet u een standaard\_D14 Azure IaaS VM, waardoor een maximale IOP's van 16.000 32 standaardopslag 1TB schijven gebruiken. Elke schijf 1TB standaardopslag kunt maximaal 500 IOP's bereiken. De geschatte kosten van deze VM per maand is $1,570. De maandelijkse kosten van 32 schijven voor standaard is $1,638. De geschatte totale maandelijkse kosten zijn $3,208.

Echter, als u de toepassing van de premie opslag bevindt, moet u VM kleiner en minder schijven voor premium, waardoor de totale kosten. Een standaard\_DS13 VM kan voldoen aan de 16.000 IOP's vereiste vier P30 schijven. De VM DS13 heeft een maximale IOP's van 25,600 en elke schijf P30 heeft een maximale IOP's van 5.000. Deze configuratie krijgt over het algemeen 5000 x 4 = 20.000 IOP's. De geschatte kosten van deze VM per maand is $1,003. De maandelijkse kosten van vier P30 premium schijven is $544.34. De geschatte totale maandelijkse kosten zijn $1,544.

In de volgende tabel bevat een overzicht van de verdeling van de kosten van dit scenario voor Standard- en Premium-opslag.

| | **Standaard** | **Premium** |
|---|---|---|
| **Kosten van VM per maand** | $1,570.58 (standaard\_D14)   | $1,003.66 (standaard\_DS13) |
| **Kosten van schijven per maand** | 1,638.40 (32 x 1 TB schijven) | 544.34 (4 x P30 schijven) |
| **Totale kosten per maand**  | $3,208.98 | $1,544.34 |

*Linux Distros*  

Met Azure Premium opslag krijgt u hetzelfde niveau van prestaties voor VMs met Windows en Linux. We ondersteunen veel flavors van Linux-distros en ziet u de volledige lijst [hier](../virtual-machines/virtual-machines-linux-endorsed-distros.md). Het is belangrijk te weten andere distros zijn beter geschikt voor verschillende werkbelastingen. Ziet u verschillende niveaus van prestaties, afhankelijk van de distro die op uw werkbelasting wordt uitgevoerd. De Linux-distros met uw toepassing testen en kiezen dat het beste werkt.


Als Linux uitgevoerd met Premium-opslagruimte, controleren de meest recente updates over de vereiste stuurprogramma's, hoge prestaties te garanderen.

## <a name="premium-storage-disk-sizes"></a>Premium opslag schijfgrootten  
Azure Premium opslag biedt momenteel drie schijfgrootte. De grootte van elke schijf heeft een andere schaal limiet voor IOP's, bandbreedte en opslag. Kies rechts Premium opslagschijf grootte afhankelijk van de vereisten van de toepassing en de hoge schaal VM-grootte. De onderstaande tabel ziet de grootte van de drie schijven en hun mogelijkheden.

| **Schijftype**       | **P10**           | **P20**           | **P30**           |
|---------------------|-------------------|-------------------|-------------------|
| Grootte van de schijf           | 128 verwijderen           | 512 verwijderen           | 1024 verwijderen (1 TB)   |
| IOP's per schijf       | 500               | 2300              | 5000              |
| Doorvoer per schijf | 100 MB per seconde | 150 MB per seconde | 200 MB per seconde |

Hoeveel schijven u kiest hangt af van de schijf gekozen formaat. U kunt één P30 schijf of schijven met meerdere P10 om te voldoen aan de vereisten van uw toepassing. Rekening account overwegingen hieronder bij het maken van de keuze.

*Schaal beperkt (IOP's en doorvoer)*  
De grenzen IOP's en de doorvoer van de grootte van elke schijf Premium is anders en onafhankelijk van de grenzen van de schaal VM. Zorg ervoor dat het totale IOP's en de productie van de schijven binnen de grenzen schaal van de gekozen VM-grootte zijn.

Als bijvoorbeeld een vereiste toepassing is een maximum van 250 MB/s doorvoer en u gebruikt een VM DS4 met één schijf P30. De DS4 VM kan maximaal 256 MB/s doorvoer geven. P30 één schijf heeft echter de limiet van 200 MB/s doorvoer. De toepassing zal bijgevolg op 200 MB per seconde door de beperking van de schijf worden beperkt. Om deze limiet te overwinnen, inrichten meerdere gegevensschijven voor VM.

>**Opmerking:**  
>Bediend door de cache gelezen worden niet opgenomen in de schijf IO's / s en een doorvoer, dus niet onderworpen aan de schijf. Cache heeft haar afzonderlijke IOP's en de doorvoer per VM.
>
>Aanvankelijk zijn lees- en schrijfbewerkingen bijvoorbeeld 60MB per seconde en 40MB/sec respectievelijk. Na verloop van tijd de cache van warms en dat meer en meer gelezen uit de cache. Vervolgens krijgt u hogere schrijven doorvoer van de schijf.

*Aantal schijven*  
Bepaal het aantal schijven u moet door vast te stellen voorschriften van toepassing. De grootte van elke VM heeft ook een limiet op het nummer van de schijven die u aan de VM koppelen kunt. Dit is normaal gesproken twee keer het aantal cores. Zorg ervoor dat de VM-grootte die u kiest het nummer van de schijven die nodig kan ondersteunen.

Vergeet niet de schijven Premium prestatiemogelijkheden voor hogere in vergelijking met standaard schijven hebben. Dus als u uw toepassing van Azure IaaS VM met standaard Premium opslag opslag migreert, moet waarschijnlijk u minder premie schijven op de dezelfde of hogere prestaties voor uw toepassing.

## <a name="disk-caching"></a>Schijfcaching  
Hoge schaal VMs die Azure Premium opslag hebben een meerlaags cachetechnologie BlobCache genoemd. Een combinatie van de virtuele Machine RAM-geheugen en lokale SSD BlobCache gebruikt voor caching. Deze cache is beschikbaar voor de permanente opslag Premium-schijven en de lokale schijven van VM. Standaard is deze cache-instelling ingesteld op lezen/schrijven voor OS schijven en alleen-lezen voor gegevensschijven gehost op Premium-opslag. Met schijfcaching ingeschakeld op de schijven voor de premie, ontstaan de hoge schaal VMs zeer hoge niveaus van prestaties die hoger zijn dan de prestaties van de onderliggende schijf.

>[AZURE.WARNING] Als u de cache-instelling van een Azure-schijf wordt losgekoppeld en opnieuw koppelt de doelschijf. Als de schijf is, wordt de VM opnieuw gestart. Stop alle toepassingen en services die kunnen worden beïnvloed door deze onderbrekingen voordat u de schijf-cache-instelling wijzigt.

Raadpleeg voor meer informatie over de werking van BlobCache, aan de binnenkant [Azure Premium opslag](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/) blogbericht.

Het is belangrijk dat de cache op de juiste set schijven inschakelen. Of schijfcache op een schijf van de premie moet worden ingeschakeld of niet afhankelijk van het patroon van de werkbelasting die schijf zal worden verwerkt. In de onderstaande tabel toont de standaard cache-instellingen voor het besturingssysteem en de gegevens.

| **Schijftype** | **Standaardinstelling voor Cache** |
|---|---|
| OS schijf | ReadWrite |
| Schijf met gegevens | Geen |

Hieronder volgen de aanbevolen schijf cache-instellingen voor gegevensschijven

| **Schijf-cache-instelling** | **Aanbeveling over het gebruik van deze instelling** |
|---|---|
| Geen | Host-cache als geen voor alleen-schrijven en schrijven zware schijven configureren. |
| Alleen-lezen | Configureren van host-cache als alleen-lezen voor schijven met alleen-lezen en lezen / schrijven. |
| ReadWrite | Host-cache als ReadWrite configureren alleen als uw toepassing op correcte wijze schrijven van gegevens in de cache naar permanente schijven wanneer dat nodig is. |

*Alleen-lezen*  
ReadOnly caching op Premium opslaggegevens schijven configureert, kunt u bereiken lage latentie voor lezen en geniet van zeer hoge IOP's lezen en doorvoer voor uw toepassing. Dit is vanwege twee redenen

1.  Leesbewerkingen uitgevoerd vanuit de cache op de VM-geheugen en lokale SSD, veel sneller dan leesbewerkingen van de gegevensschijf op de Azure blobopslag.  
2.  Premium-opslagruimte telt niet mee voor het lezen van cache naar de schijf IO's / s en een doorvoer bediend. Daarom is de toepassing kunnen bereiken hogere totale IOP's en de doorvoer.

*ReadWrite*  
De OS-schijven hebben standaard ReadWrite caching ingeschakeld. Onlangs hebben we ondersteuning voor het in cache opslaan van gegevens ook schijven ReadWrite toegevoegd. Als u caching ReadWrite gebruikt, moet u een goede manier om te schrijven van de gegevens uit de cache voor permanente schijven hebben. Bijvoorbeeld SQL Server verwerkt in cache opgeslagen gegevens schrijft naar de schijven voor permanente opslag op eigen. ReadWrite cache kan met een toepassing die niet de vereiste gegevens persistent maken verwerkt leiden tot verlies van gegevens als de VM vastloopt.

Als voorbeeld, kunt u deze kaderregeling toepassen op SQL Server wordt uitgevoerd op Premium-opslagruimte op de volgende manieren

1.  "ReadOnly" cache configureren op premium schijven voor gegevensbestanden die als host fungeert.  
    een.  De fast leest van lagere cache de SQL Server Querytijd omdat u gegevens pagina's veel sneller worden opgehaald uit de cache in vergelijking met rechtstreeks vanuit de gegevens schijven.  
    b.  Lezen uit de cache levert, betekent dat er extra doorvoer beschikbaar in premium-gegevensschijven. SQL Server kan dit extra doorvoer naar ophalen van meer gegevens pagina's en andere bewerkingen, zoals back-up en terugzetten gebruiken, batchverwerking wordt geladen en wordt de index opnieuw gemaakt.  
2.  Configureren 'Geen' in de cache op premium schijven die als host fungeert voor de logboekbestanden.  
    een.  Logboekbestanden hebben voornamelijk schrijven zware bewerkingen. Ze kunnen dus niet gebruikmaken van de cache alleen-lezen.

## <a name="disk-striping"></a>Schijf-Striping  
Wanneer een hoge schaal die VM met verschillende premium permanente schijven is gekoppeld, kunnen u de schijven samen gestreept om samenvoegen hun IOP's, bandbreedte en opslagcapaciteit.

In Windows, kunt u opslagruimtes stripe schijven samen. U moet een kolom voor elke schijf in een groep. Anders kan de algehele prestaties van een striped volume worden lager dan verwacht als gevolg van ongelijkmatige spreiding van het verkeer op de schijven.

Belangrijk: Met behulp van Serverbeheer UI, kunt u het totale aantal kolommen tot en met 8 voor een striped volume instellen. Bij het toevoegen van meer dan 8 schijven PowerShell gebruiken om het volume te maken. Met PowerShell, kunt u instellen het aantal kolommen gelijk is aan het aantal schijven. Bijvoorbeeld, als er 16 schijven in een enkele stripeset; 16 kolommen opgeven in de parameter *Aantal_kolommen* van de PowerShell-cmdlet *New-VirtualDisk* .


Gebruik het hulpprogramma MDADM stripe schijven samen op Linux. Raadpleeg voor gedetailleerde stappen op striping schijven op Linux [Software RAID configureren onder Linux](../virtual-machines/virtual-machines-linux-configure-raid.md).


*Stripe-grootte*  
Een belangrijke configuratie in schijf-striping is de stripe-grootte. De stripe grootte of de blokgrootte is de kleinste hoeveelheid gegevens die van toepassing op een striped volume kunt oplossen. U configureert de stripe-grootte hangt af van het type van de toepassing en het patroon van de aanvraag. Als u de verkeerde stripe-grootte, kan leiden tot onjuiste uitlijning i/o, die tot slechtere prestaties van uw toepassing leidt.

Bijvoorbeeld, als een i/o-verzoek door de toepassing gegenereerde groter dan de grootte van de schijf stripe is, het opslagsysteem geschreven stripe eenheid grenzen op meer dan één schijf. Wanneer is het tijd om de toegang tot deze gegevens, heeft dit streven voor meer dan één stripe units om de aanvraag te voltooien. Het cumulatieve effect van dit gedrag kan leiden tot aanzienlijke vertragingen. Aan de andere kant kunnen als de i/o-aanvraag kleiner is dan stripe-grootte, en het willekeurig van aard is, de i/o-aanvragen optellen op dezelfde schijf een knelpunt veroorzaken en uiteindelijk tasten de i/o-prestaties.


Afhankelijk van het type werkbelasting van de toepassing wordt uitgevoerd, kiest u een geschikte stripe-grootte. Gebruik een kleinere stripe voor willekeurige kleine i/o-aanvragen. Overwegende dat de aanvragen gebruiken voor grote sequentiële I/O stripe groter. Ontdek de stripe grootte aanbevelingen voor de toepassing dat op de premie opslag uitvoert. Voor SQL Server configureren stripe grootte van 64KB voor OLTP workloads en 256KB voor magazijnbeheer werkbelasting van gegevens. Zie [Aanbevolen procedures voor SQL Server op Azure VMs prestaties](../virtual-machines/virtual-machines-windows-sql-performance.md#disks-and-performance-considerations) voor meer informatie.


>**Opmerking:**  
>U kunt samen maximaal 32 schijven voor premium op een DS serie VM en 64 premium schijven in een reeks GS VM stripe.

## <a name="multi-threading"></a>Multithreading  
Azure is Premium opslag platform massively parallel worden ontwikkeld. Een multithread-toepassing bereikt dus veel hogere prestaties dan een single thread-toepassingen. Een multithread-toepassing wordt gescheiden van zijn taken door meerdere threads en verhoogt de efficiëntie van de uitvoering met behulp van de VM en schijf middelen tot het maximum.

Bijvoorbeeld, als uw toepassing wordt uitgevoerd op een single core VM met twee threads, kunt de CPU schakelen tussen de twee threads aan efficiëntie. Terwijl een thread op een schijf-i/o wacht te voltooien, wordt de CPU kunt overschakelen naar de andere thread. Op deze manier doen twee threads meer dan één thread zou. Als meer dan één core VM, verder hierdoor minder tijd uitgevoerd aangezien elke core taken parallel kan worden uitgevoerd.

Mogelijk niet de manier wijzigen waarop een gebruiksklare toepassing enkele implementeert thread of met meerdere threads. SQL Server is bijvoorbeeld kunnen meerdere CPU en multicore-verwerking. SQL Server besluit echter onder welke voorwaarden zij maken gebruik van een of meer threads voor het verwerken van een query. Het kan uitvoeren van query's en indexen met behulp van multithreading. Voor een query waarbij grote tabellen koppelen en gegevens sorteren voordat u terugkeert naar de gebruiker, gebruikt SQL Server waarschijnlijk meerdere threads. Een gebruiker kan echter niet instellen of SQL Server wordt uitgevoerd een query met een enkele thread of met meerdere threads.

Er zijn configuratie-instellingen die u wijzigen kunt om dit van invloed zijn op multithreading of parallelle verwerking van een toepassing. Bijvoorbeeld in het geval van SQL Server is de maximale mate van parallellisme configuratie. Deze instelling genaamd MAXDOP, kunt u het maximale aantal processors die SQL Server kunt gebruiken als parallelle configureren wordt verwerkt. U kunt de MAXDOP configureren voor afzonderlijke query's en indexen. Dit is nuttig als u wilt bronnen van het systeem voor een essentiële toepassing prestaties in balans.

Stel dat uw toepassing met behulp van SQL Server wordt uitgevoerd, een grote query en de indexbewerking van een op hetzelfde moment. Laten we aannemen dat u wilde de indexbewerking meer zodat in vergelijking met de grote query. In dat geval kunt u de waarde van de indexbewerking niet hoger zijn dan de MAXDOP waarde voor de query MAXDOP instellen. Op deze manier heeft SQL Server meer aantal processors dat het voor de indexbewerking in vergelijking met het aantal processors dat zij aan de grote query besteden kan kunt benutten. Vergeet niet dat u het aantal threads dat SQL Server wordt gebruikt voor elke bewerking geen zeggenschap. U kunt bepalen het maximale aantal processors wordt toegewezen voor multithreading.

Meer informatie over [Graden van parallellisme](https://technet.microsoft.com/library/ms188611.aspx) in SQL Server. Ontdek deze instellingen die van invloed zijn op multithreading in uw toepassing en de bijbehorende configuraties optimaliseren.

## <a name="queue-depth"></a>Wachtrijlengte  
De wachtrijdiepte of lengte van wachtrij of wachtrijgrootte is het aantal i/o-aanvragen in behandeling in het systeem. De waarde van de wachtrijdiepte bepaalt hoeveel i/o-bewerkingen die uw toepassing uitlijnen kunt, welke schijven voor de verwerking. Heeft dit gevolgen voor alle de toepassing drie prestatie-indicatoren die we in dit artikel te weten, IOP's, doorvoer en latentie besproken.

In de wachtrij diepte en multithreading zijn nauw verwante. De wachtrijdiepte-waarde geeft aan hoeveel multithreading kan worden bereikt door de toepassing. Als u de diepte van de wachtrij is groot, toepassing gelijktijdig kan worden uitgevoerd meer bewerkingen, met andere woorden, meer multithreading. Als u de diepte van de wachtrij is klein, zelfs als de toepassing multithreaded is, heeft deze geen voldoende aanvragen voor gelijktijdige uitvoering uitgelijnd.

Normaal gesproken overal verkrijgbare toepassingen kunt u niet wijzigen, de wachtrijdiepte van de, omdat als verkeerd doet meer schade dan goed is ingesteld. Toepassingen wordt de juiste waarde van wachtrijlengte voor optimale prestaties. Het is echter belangrijk te begrijpen dit concept, zodat u prestatieproblemen met uw toepassing oplossen kunt. U kunt ook de effecten van de wachtrijdiepte door benchmarking tools op uw systeem zien.

Sommige toepassingen bieden van invloed zijn op de diepte van de wachtrij-instellingen. Bijvoorbeeld beschreven de instelling van de MAXDOP (maximale mate van parallellisme) in SQL Server in de vorige sectie. MAXDOP is een manier om de diepte van de wachtrij van invloed zijn op en multithreading, hoewel het de waarde van de wachtrijdiepte van SQL Server niet rechtstreeks wijzigen.

*Hoge wachtrijdiepte*  
Een hoge wachtrijdiepte meer bewerkingen op de schijf wordt uitgelijnd. De schijf bekend de volgende aanvraag in de wachtrij tevoren. Bijgevolg de schijf bewerkingen tevoren kunt plannen en deze verwerken in een optimale volgorde. Sinds de toepassing meer aanvragen naar de schijf verzenden is, kan de schijf meer parallel IOs verwerken. De toepassing worden uiteindelijk hoger IOP's bereikt. Sinds de toepassing is meer aanvragen verwerkt, verhoogt de totale doorvoer van de toepassing ook.

Een toepassing krijgt meestal maximale doorvoer met 8-16 + uitstekende IOs per aangesloten schijf. Als een wachtrijdiepte een toepassing is niet voldoende IOs pushen op het systeem en minder hoeveelheid in een bepaalde periode worden verwerkt. Met andere woorden, minder doorvoer.

Bijvoorbeeld, in SQL Server vertelt de MAXDOP waarde voor een query op '4' SQL Server maximaal vier cores te gebruiken voor het uitvoeren van de query. SQL Server bepaalt wat de beste waarde voor wachtrij diepte en het aantal cores voor de uitvoering van de query is.

*Optimale wachtrijdiepte*  
Zeer hoge wachtrij diepte waarde heeft ook zijn nadelen. Als wachtrij diepte-waarde te hoog is, probeert de toepassing zeer hoge IOP's besturen. Tenzij toepassing permanente schijven met voldoende ingerichte IOP's heeft, dit een negatieve invloed op vertragingstijden van toepassing. Volgende formule wordt de relatie tussen IOP's en latentie wachtrijdiepte.  
    ![](media/storage-premium-storage-performance/image6.png)

U moet niet wachtrijlengte configureren op een hoge waarde, maar voor een optimale waarde, die voldoende IOP's voor de toepassing zonder vertragingstijden kan bieden. Als de vertraging van de toepassing moet worden 1 milliseconde, de diepte van de wachtrij moet bereiken 5.000 IOP's is bijvoorbeeld, QD = 5000 x 0,001 = 5.

*Wachtrijlengte voor Striped volumes*  
Onderhouden van een striped volume hoog genoeg wachtrijdiepte zodanig dat elke schijf een piek wachtrijdiepte afzonderlijk heeft. Neem bijvoorbeeld een toepassing die een wachtrijdiepte van 2 duwt en er zijn 4 schijfstations in de stripe. De twee i/o-aanvragen gaat naar de twee schijven en nog twee schijven is niet actief. Configureer daarom de wachtrijdiepte van de zodanig dat alle schijven bezet worden kunnen. Formule hieronder toont het bepalen van de wachtrijdiepte van de van striped volumes.  
    ![](media/storage-premium-storage-performance/image7.png)

## <a name="throttling"></a>Bandbreedtebeperking  
Azure bepaalde Premium opslag aantal IOP's en doorvoer afhankelijk van de VM-formaten en schijf-formaten die u kiest. Wanneer uw toepassing wil IOP's of doorvoer station boven deze grenzen van wat de VM of de schijf kan verwerken, beperken Premium opslag het. Dit doet in de vorm van de prestaties verslechteren in uw toepassing. Dit kan betekenen hogere latentie, doorvoer of lager IOP's verlagen. Als Premium-opslag niet beperken is, kan volledig uw toepassing niet door meer dan wat de bronnen zijn geschikt om te bereiken. Dus om prestatieproblemen te voorkomen als gevolg van procesbeperking, altijd voldoende middelen voor uw toepassing te creëren. In overweging nemen we in de VM-formaten en diskette formaten hierboven besproken. Benchmarking is de beste manier om erachter te komen welke bronnen moet u uw hosttoepassing.

## <a name="benchmarking"></a>Benchmarking  
Benchmarking is het proces van verschillende werkbelastingen op uw toepassing te simuleren en meten van de toepassingsprestaties voor elke werkbelasting. De stappen die worden beschreven in de vorige sectie hebt u de prestatie-eisen voor toepassing verzameld. Door extra benchmarking op het VMs die als host fungeert voor de toepassing, kunt u de prestaties die uw toepassing met Premium opslag bereiken kunt bepalen. In deze sectie bieden u voorbeelden van een standaard DS14 VM ingericht met Azure Premium schijven benchmarking.

We hebben gemeenschappelijke benchmarking extra Iometer en FIO, respectievelijk gebruikt voor Windows en Linux. Deze hulpprogramma's brengen gang van meerdere threads een productie zoals werklast te simuleren en meten van de prestaties van het systeem. Met de hulpprogramma's kunt u ook parameters zoals de blok grootte en de wachtrij diepte die u normaal gesproken niet voor een toepassing wijzigen. Dit geeft u meer flexibiliteit om de maximale prestaties op een hoge schaal VM ingericht met premium schijven voor verschillende soorten werkbelasting van toepassingen. Ga voor meer informatie over elk hulpprogramma benchmarking naar [Iometer](http://www.iometer.org/) en [FIO](http://freecode.com/projects/fio).

Volg de onderstaande voorbeelden, maak een standaard DS14 VM en 11 Premium schijven koppelen aan de VM. Van de 11 schijven configureren van 10 schijven met in cache opslaan als 'Geen' host en stripe ze naar een volume genaamd NoCacheWrites. Host in de cache opslaan als 'Alleen-lezen' op de resterende schijf configureren en een volume CacheReads aangeroepen met deze schijf maken. Met deze instellingen, kun je voor een overzicht van de maximale prestaties voor lezen en schrijven van een standaard DS14 VM. Ga voor gedetailleerde stappen over het maken van een VM DS14 met premium schijven te [maken en het gebruik van opslag Premium account voor een virtuele machine gegevensschijf](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk).

*Opwarmen van de Cache*  
De schijf met de alleen-lezen-host caching geven hogere IOP's dan de limiet kan worden. Als u deze maximale prestaties voor het lezen van de host-cache, moet eerst u opgewarmd de cache van deze schijf. Dit zorgt ervoor dat het alleen IOs welke benchmarking tool op CacheReads volume wordt station daadwerkelijk treffers in de cache en niet op de schijf rechtstreeks. Het resultaat van de treffers in cache in extra IOP's uit de cache van één schijf ingeschakeld.

>**Belangrijk:**  
>U moet de cache voordat u benchmarking, elke keer opnieuw is gestart, VM opgewarmd.

#### <a name="iometer"></a>Iometer   
[Download het hulpprogramma Iometer](http://sourceforge.net/projects/iometer/files/iometer-stable/2006-07-27/iometer-2006.07.27.win32.i386-setup.exe/download) op de VM.

*Testbestand*  
Iometer maakt gebruik van een testbestand dat is opgeslagen op het volume waarop u de benchmarking-test wordt uitgevoerd. Deze schijven leest en schrijft op dit testbestand voor het meten van de schijf IO's / s en de doorvoer. Iometer maakt dit testbestand, als u dat niet hebt opgegeven. Maak een 200GB testbestand iobw.tst op de CacheReads en NoCacheWrites volumes genoemd.

*Specificaties van Access*  
De specificaties, i/o-grootte, lezen/schrijven van % aanvragen, willekeurige/sequentiële % zijn geconfigureerd op het tabblad 'Specificaties van Access' in de Iometer. Een access-specificatie te maken voor elk van de scenario's die hieronder worden beschreven. Maak de specificaties van access en 'Opslaan' met een geschikte naam zoals: RandomWrites\_8K, RandomReads\_8K. Selecteer de desbetreffende specificatie wanneer het Testscenario wordt uitgevoerd.

Hieronder ziet een voorbeeld van de specificaties van access voor maximale scenario van IOP's schrijven  
    ![](media/storage-premium-storage-performance/image8.png)

*Maximale IOP's Test specificaties*  
Voorbeelden van maximale IOP's kleiner verzoek te gebruiken. Met 8K verzoek grootte en specificaties voor willekeurige schrijft en leest maken.

| Specificaties van Access | Grootte van de aanvraag | Willekeurige % | Lezen % |
|----------------------|--------------|----------|--------|
| RandomWrites\_8K     | 8K           | 100      | 0      |
| RandomReads\_8K      | 8K           | 100      | 100    |

*Maximale doorvoer beproevingsmethoden*  
Voorbeelden van maximale doorvoer groter verzoek te gebruiken. Grootte van 64 kB aanvraag gebruiken en specificaties voor willekeurige schrijft en leest maken.

| Specificaties van Access | Grootte van de aanvraag | Willekeurige % | Lezen % |
|----------------------|--------------|----------|--------|
| RandomWrites\_64K    | 64K          | 100      | 0      |
| RandomReads\_64K     | 64K          | 100      | 100    |

*De Iometer-test*  
Voer de stappen uit die hieronder opgewarmd cache

1.  Twee access-specificaties maken met onderstaande waarden,

  	| Naam              | Grootte van de aanvraag | Willekeurige % | Lezen % |
  	|-------------------|--------------|----------|--------|
  	| RandomWrites\_1MB | 1MB          | 100      | 0      |
  	| RandomReads\_1MB  | 1MB          | 100      | 100    |

2.  Voer de test Iometer voor het initialiseren van cache-schijf met de volgende parameters. Gebruik drie worker-threads voor het doelvolume en een wachtrijdiepte van 128. De "Runtime" duur van de proef ingesteld op 2hrs op het tabblad 'Setup testen'.

  	| Scenario              | Doelvolume | Naam              | Duur |
  	|-----------------------|---------------|-------------------|----------|
  	| Cache schijf initialiseren | CacheReads    | RandomWrites\_1MB | 2hrs     |

3.  Voer de test Iometer voor het opwarmen van de cache-schijf met de volgende parameters. Gebruik drie worker-threads voor het doelvolume en een wachtrijdiepte van 128. De "Runtime" duur van de proef ingesteld op 2hrs op het tabblad 'Setup testen'.

  	| Scenario           | Doelvolume | Naam             | Duur |
  	|--------------------|---------------|------------------|----------|
  	| Warm up schijf-Cache | CacheReads    | RandomReads\_1MB | 2hrs     |

Nadat de schijf cache is opgewarmd, doorgaan met de Testscenario's die hieronder worden beschreven. Gebruik de Iometer-test wordt uitgevoerd, ten minste drie worker-threads voor **elk** doelvolume. Selecteer het doelvolume, wachtrijdiepte instellen voor elke thread werknemer en selecteert u een van de specificaties van de test opgeslagen, zoals in de onderstaande tabel, het bijbehorende Testscenario uitvoeren. De tabel bevat ook verwachte resultaten voor IOP's en de doorvoer bij het uitvoeren van deze tests. Klein 8KB i/o en een hoge wachtrijdiepte van 128 is voor alle scenario's gebruikt.

| Testscenario      | Doelvolume | Naam              | Resultaat       |
|--------------------|---------------|-------------------|--------------|
| Max. Lees IOP 's     | CacheReads    | RandomWrites\_8K  | 50.000 IOP 'S  |
| Max. IOP's schrijven    | NoCacheWrites | RandomReads\_8K   | 64.000 IOP 'S  |
| Max. Gecombineerde IOP 's | CacheReads    | RandomWrites\_8K  | 100.000 IOP 'S |
|                    | NoCacheWrites | RandomReads\_8K   |              |
| Max. MB per seconde   | CacheReads    | RandomWrites\_64K | 524 MB/sec.   |
| Max. MB per seconde  | NoCacheWrites | RandomReads\_64K  | 524 MB/sec.   |
| Gecombineerde MB/sec.    | CacheReads    | RandomWrites\_64K | 1000 MB/sec.  |
|                    | NoCacheWrites | RandomReads\_64K  |              |

Hieronder ziet u schermafbeeldingen van de Iometer testresultaten voor gecombineerde scenario's IOP's en de doorvoer.

*Gecombineerde lees- en schrijfbewerkingen maximale IOP 's*  
![](media/storage-premium-storage-performance/image9.png)

*Gecombineerde lees- en schrijfbewerkingen maximale doorvoer*  
![](media/storage-premium-storage-performance/image10.png)

### <a name="fio"></a>FIO  
FIO is een populair hulpmiddel benchmark opslag op de Linux VMs. Heeft de flexibiliteit om te selecteren, verschillende i/o-formaten en opeenvolgende of willekeurige leesbewerkingen en schrijfbewerkingen. Het Hiermee worker-threads of processen die de opgegeven i/o-bewerkingen uit te voeren. U kunt het type i/o-bewerkingen die elke werkthread moet uitvoeren met behulp van Project-bestanden opgeven. Wij zijn een project-bestand per scenario wordt geïllustreerd in de onderstaande voorbeelden. U kunt de specificaties van deze verschillende werkbelastingen op Premium opslag benchmark taakbestanden wijzigen. In de voorbeelden gebruiken we een standaard DS 14 VM met **Ubuntu**. Gebruik dezelfde instellingen als beschreven in het begin van de [sectie Benchmarking](#Benchmarking) en warm de cache voordat het vergelijkend onderzoek worden uitgevoerd.

Voordat u begint, [FIO downloaden](https://github.com/axboe/fio) en op uw virtuele machine installeren.

Voer de volgende opdracht uit voor Ubuntu,

        apt-get install fio

We gebruiken voor het besturen van schrijfbewerkingen vier worker-threads en vier worker-threads voor aangedreven leesbewerkingen op de schijf. De werknemers schrijven wordt verkeer rijden op volume "nocache" 10 schijven met cache instellen op "None heeft". De werknemers lezen wordt verkeer rijden op het volume 'readcache', met 1 schijf met cache is ingesteld op 'Alleen-lezen'.

*Maximale schrijven IOP 's*  
Het project-bestand maken met de volgende specificaties om maximale IOP's schrijven. Geef deze de naam 'fiowrite.ini'.

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[writer1]
rw=randwrite
directory=/mnt/nocache
[writer2]
rw=randwrite
directory=/mnt/nocache
[writer3]
rw=randwrite
directory=/mnt/nocache
[writer4]
rw=randwrite
directory=/mnt/nocache
```

Let op de volgende belangrijke dingen die in overeenstemming met de richtlijnen voor het ontwerpen die worden beschreven in de vorige hoofdstukken zijn. Deze specificaties zijn essentieel voor de maximale IOP's, station  
-   Een hoge wachtrijdiepte van 256.  
-   Een kleine blokgrootte van 8KB.  
-   Meerdere threads uitvoeren van willekeurige schrijft.

De volgende opdracht uit te ere van de test FIO gedurende 30 seconden  

    sudo fio --runtime 30 fiowrite.ini

Tijdens de test wordt uitgevoerd, kunt u zijn te zien van het aantal IOP's schrijven de VM en Premium schijven levert. In het onderstaande voorbeeld ziet de VM DS14 levert de maximale schrijven IOP's maximaal 50.000 IOP's.  
    ![](media/storage-premium-storage-performance/image11.png)

*Maximale lezen IOP 's*  
Het job-bestand maken met de volgende specificaties om maximale IOP's voor lezen. Geef deze de naam 'fioread.ini'.

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache
```

Let op de volgende belangrijke dingen die in overeenstemming met de richtlijnen voor het ontwerpen die worden beschreven in de vorige hoofdstukken zijn. Deze specificaties zijn essentieel voor de maximale IOP's, station

-   Een hoge wachtrijdiepte van 256.  
-   Een kleine blokgrootte van 8KB.  
-   Meerdere threads uitvoeren van willekeurige schrijft.

De volgende opdracht uit te ere van de test FIO gedurende 30 seconden

    sudo fio --runtime 30 fioread.ini

Tijdens de test wordt uitgevoerd, kunt u zijn te zien van het aantal gelezen IOP's de VM en Premium schijven levert. Zoals in het onderstaande voorbeeld, levert de VM DS14 meer dan 64.000 IOP's voor lezen. Dit is een combinatie van de schijf en de prestaties van de cache.  
    ![](media/storage-premium-storage-performance/image12.png)

*Maximale lees- en schrijfmachtigingen IOP 's*  
Het job-bestand maken met de volgende specificaties voor maximale gecombineerde lezen en schrijven IOP's. Geef deze de naam 'fioreadwrite.ini'.

```
[global]
size=30g
direct=1
iodepth=128
ioengine=libaio
bs=4k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache

[writer1]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer2]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer3]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer4]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
```

Let op de volgende belangrijke dingen die in overeenstemming met de richtlijnen voor het ontwerpen die worden beschreven in de vorige hoofdstukken zijn. Deze specificaties zijn essentieel voor de maximale IOP's, station

-   Een hoge wachtrijdiepte van 128.  
-   Een kleine blokgrootte van 4KB.  
-   Meerdere threads uitvoeren van willekeurige gelezen en geschreven.

De volgende opdracht uit te ere van de test FIO gedurende 30 seconden

    sudo fio --runtime 30 fioreadwrite.ini

Tijdens de test wordt uitgevoerd, is het mogelijk om te zien hoeveel gecombineerde lezen en schrijven van IOP's de VM en Premium schijven levert. Zoals in het onderstaande voorbeeld, wordt de VM DS14 levert meer dan 100.000 gecombineerde lezen en schrijven IOP's. Dit is een combinatie van de schijf en de prestaties van de cache.  
    ![](media/storage-premium-storage-performance/image13.png)

*Maximale doorvoer gecombineerd*  
Als u de maximale gecombineerde lezen en schrijven van doorvoer, een grotere blokgrootte en omvangrijke wachtrijdiepte gebruikt met meerdere threads lees- en schrijfbewerkingen uitvoert. U kunt een blokgrootte van 64KB en wachtrijdiepte van 128.

## <a name="next-steps"></a>Volgende stappen  

Meer informatie over de opslag van Azure Premium:

- [Premium-opslagruimte: Krachtige opslag voor Azure VM werkbelasting](storage-premium-storage.md)  

Lees artikelen over de beste prestaties voor SQL Server voor SQL Server-gebruikers:

- [De beste prestaties voor SQL Server in Azure virtuele Machines](../virtual-machines/virtual-machines-windows-sql-performance.md)
- [Azure Premium opslag biedt de beste prestaties voor SQL Server in Azure VM](http://blogs.technet.com/b/dataplatforminsider/archive/2015/04/23/azure-premium-storage-provides-highest-performance-for-sql-server-in-azure-vm.aspx) 
