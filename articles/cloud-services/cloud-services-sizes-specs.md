<properties
 pageTitle="Formaten voor cloud services | Microsoft Azure"
 description="Geeft de grootte van de andere virtuele machine (en -id's) voor Azure cloud service web- en werknemer rollen."
 services="cloud-services"
 documentationCenter=""
 authors="Thraka"
 manager="timlt"
 editor=""/>
<tags
 ms.service="cloud-services"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="tbd"
 ms.date="10/27/2016"
 ms.author="adegeo"/>

# <a name="sizes-for-cloud-services"></a>Formaten voor Cloud Services

Dit onderwerp beschrijft de beschikbare formaten en opties voor Cloud-rol exemplaren (web-rollen en functies van de werknemer). Het biedt ook overwegingen voor de implementatie te houden bij het plannen van deze bronnen. Elke grootte heeft een ID die u in het [definitiebestand](cloud-services-model-and-package.md#csdef)wordt geplaatst.

Cloud Services is een van de verschillende typen bronnen compute op Azure. Klik [hier](cloud-services-choose-me.md) voor meer informatie over Cloud Services.

> [AZURE.NOTE]Verwante Azure grenzen Zie [Azure abonnement Service grenzen, quota's en beperkingen](../azure-subscription-service-limits.md)

## <a name="sizes-for-web-and-worker-role-instances"></a>Formaten voor web- en werknemer rol exemplaren

Er zijn verschillende standaard formaten om uit te kiezen op Azure. Overwegingen voor sommige van deze formaten zijn:

* VMs D-reeks zijn ontworpen voor het uitvoeren van toepassingen die meer rekenkracht en prestaties van de tijdelijke schijf. D-reeks VMs bieden snellere processors een hogere geheugen-core-verhouding en een solid-state drive (SSD) voor de tijdelijke schijf. Voor meer informatie, Zie de aankondiging op de Azure blog, de [Grootte van de virtuele Machine nieuwe D-reeks](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/).

* Dv2-serie, de opvolger van de originele D-reeks biedt een krachtige CPU. De CPU Dv2-serie is ongeveer 35% sneller dan de CPU van de D-reeks. Is gebaseerd op de nieuwste 2,4 GHz Intel Xeon® E5-2673 v3 processor (Haswell), en met de Intel Turbo Boost technologie 2.0 tot 3,1 GHz kunt gaan. De Dv2-serie heeft het dezelfde geheugen en schijfruimte configuraties zoals de D-reeks.

*   VMs G-serie bieden het meeste geheugen en op hosts met Intel Xeon E5 V3 familie processors worden uitgevoerd.

*   De A-serie VMs kunnen worden geïmplementeerd op een groot aantal hardwaretypen en -processors. De grootte is beperkt op basis van de hardware, aan te bieden voor de sessie, ongeacht de hardware die wordt geïmplementeerd op consistente prestaties. Query om te bepalen de fysieke hardware waarvoor deze grootte wordt geïmplementeerd, de virtuele hardware uit in de virtuele Machine.

*   De A0-formaat is te weinig waarop u bent geabonneerd op de fysieke hardware. Voor deze specifieke grootte alleen mogelijk andere implementaties bij klanten van invloed op de prestaties van uw werkbelasting uitgevoerd. Relatieve prestaties worden hieronder beschreven als de verwachte basislijn, onder voorbehoud van een bij benadering variabiliteit van 15 procent.


De grootte van de virtuele machine is van invloed op de prijzen. De grootte is ook van invloed op de verwerking, het geheugen en opslag capaciteit van de virtuele machine. Opslagkosten worden berekend afzonderlijk op basis van de gebruikte pagina's in de account van de opslag. Zie [Virtuele Machines prijzen Details](https://azure.microsoft.com/pricing/details/virtual-machines/) en [Prijzen voor Azure opslag](https://azure.microsoft.com/pricing/details/storage/)voor meer informatie. 


De volgende overwegingen kunnen helpen u bepalen van een grootte:


* De grootte van de A8 A11 en H-serie zijn ook bekend als *intensieve exemplaren*. De hardware die deze formaten compatibel is ontworpen en geoptimaliseerd voor intensieve en netwerk-intensieve toepassingen, zoals high performance computing (HPC)-cluster toepassingen, modellen en simulaties. De A8 A11 reeks maakt gebruik van Intel Xeon E5-2670 @ 2,6 GHZ en de H-reeks maakt gebruik van v3 voor Intel Xeon E5-2667 @ 3,2 GHz. Zie voor uitgebreide informatie en overwegingen over het gebruik van deze formaten [de A-serie H-serie en intensieve VMs](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md). 

* Dv2-reeks, D-reeks, G-serie, zijn ideaal voor toepassingen die snellere CPU's, betere lokale schijf prestaties of hogere eisen voor geheugen hebben.  Ze bieden een krachtige combinatie voor veel toepassingen voor bedrijfstoepassingen.

*   Sommige van de fysieke hosts in Azure datacenters ondersteunen niet groter formaat van de virtuele machine, zoals A5: A11. Als gevolg hiervan ziet u de foutbericht **mislukt voor het configureren van virtuele machine {machinenaam}** of **mislukt {machinenaam} van virtuele machine maken** wanneer het formaat van een bestaande virtuele machine op een nieuwe grootte. een nieuwe virtuele machine maken in een virtueel netwerk gemaakt vóór 16 April 2013; of een nieuwe virtuele machine toe te voegen aan een bestaande cloud-service. Zie [Fout: 'Kan geen virtuele machine configureren'](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) op het ondersteuningsforum voor oplossingen voor elk implementatiescenario.  

* Uw abonnement kan ook het aantal cores, die u in bepaalde families grootte implementeren kunt beperken. Als u wilt een quota verhogen, contact op met ondersteuning van Azure.


## <a name="performance-considerations"></a>Overwegingen bij prestaties

Hebben wij het concept van de Azure Compute eenheid (ACU) op een manier van vergelijken (CPU) prestaties via Azure SKU's. Hierdoor kunt u eenvoudig bepalen welke SKU is waarschijnlijk te komen aan de prestatiebehoeften van uw.  ACU momenteel is gestandaardiseerd op een kleine (Standard_A1) wordt 100 en alle andere SKU's vervolgens VM vertegenwoordigen ongeveer hoe veel sneller dat SKU een standaard benchmark kan worden uitgevoerd. 

>[AZURE.IMPORTANT] Het ACU is slechts een richtlijn.  De resultaten voor uw werkbelasting kunnen variëren. 

<br>

|SKU-familie |ACU/Core |
|---|---|
|[Standard_A0](#a-series)   |50 |
|[Standard_A1-4](#a-series) |100 |
|[Standard_A5-7](#a-series) |100 |
|[A8 A11](#a-series)    |225 *|
|[D1-14](#d-series) |160 |
|[D1-15v2](#dv2-series) |210 - 250 *|
|[5-G1](#g-series)  |180 - 240 *|
|[H](#h-series) |290 - 300 *|

ACUs is gemarkeerd met een * Intel® Turbo-technologie gebruiken om te verhogen van de frequentie van de CPU en een prestatieverhoging bieden.  Het bedrag van de boost kan variëren afhankelijk van de grootte van de VM, werkbelasting en andere werkbelasting op dezelfde host.

## <a name="size-tables"></a>De grootte van tabellen

De volgende tabellen tonen de grootte en de capaciteiten die ze bieden.

* Opslagcapaciteit wordt weergegeven in eenheden van toevoegen of 1024 ^ 3 bytes. Diskettes vergelijken als gemeten in GB (1000 ^ 3 bytes) naar schijven die zijn gemeten in toevoegen (1024 ^ 3) onthouden capaciteitsnummers in verwijderen gegeven lijkt kleiner. Bijvoorbeeld 1023 toevoegen = 1098.4 GB

* Schijf is de doorvoer in invoer/uitvoer-bewerkingen per seconde (IOP's) en MBps waar MBps = 10 ^ 6 bytes per seconde.

* Gegevensschijven kunnen worden uitgevoerd in de modus met cache of uncached. Voor schijf-bewerking van gegevens in de cache, is de host-cache-modus ingesteld op **alleen-lezen** of **ReadWrite**.  Voor uncached gegevens schijfbewerking, is de host-cache-modus ingesteld op **geen**.

* Maximale bandbreedte is de maximale geaggregeerde bandbreedte toegewezen en toegewezen per type VM. De maximale bandbreedte biedt richtlijnen voor het selecteren van het juiste type VM te waarborgen, adequaat netwerkcapaciteit beschikbaar is. Bij het verplaatsen naar een andere laag, Gemiddeld, hoog en zeer hoog, verhoogt de doorvoer daarvan in kennis. Prestaties van het feitelijke netwerk is afhankelijk van vele factoren, waaronder netwerk- en werklasten van toepassingen en instellingen van toepassing.


## <a name="a-series"></a>A-serie

| Grootte        | CPU-kernen | Geheugen: verwijderen | Lokale harde schijf: verwijderen | Max gegevensschijven | Max schijf gegevensdoorvoer: IOP's | Max NIC / netwerkbandbreedte |
|-------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A0 | 1         | 0.768        | 20                    | 1              | 1 x 500              | 1 / lage                   |
| Standard_A1 | 1         | 1,75         | 70                    | 2              | 2 x 500              | 1 / matig              |
| Standard_A2 | 2         | 3.5 GB       | 135                   | 4              | 4 x 500              | 1 / matig              |
| Standard_A3 | 4         | 7            | 285                   | 8              | 8 x 500              | 2 / hoog                  |
| Standard_A4 | 8         | 14           | 605                   | 16             | 16 x 500             | 4 / hoog                  |
| Standard_A5 | 2         | 14           | 135                   | 4              | 4 X 500              | 1 / matig              |
| Standard_A6 | 4         | 28           | 285                   | 8              | 8 x 500              | 2 / hoog                  |
| Standard_A7 | 8         | 56           | 605                   | 16             | 16 x 500             | 4 / hoog                  |

## <a name="a-series---compute-intensive-instances"></a>A-series-computer-intensieve exemplaren

Zie voor informatie en overwegingen over het gebruik van deze formaten, [over de A-serie H-serie en intensieve VMs](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md).


| Grootte         | CPU-kernen | Geheugen: verwijderen | Lokale harde schijf: verwijderen | Max gegevensschijven | Max schijf gegevensdoorvoer: IOP's | Max NIC / netwerkbandbreedte |
|--------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A8 * | 8         | 56           | 382                   | 16             | 16 x 500             | 2 / hoog                  |
| Standard_A9 * | 16        | 112          | 382                   | 16             | 16 x 500             | 4 / zeer hoog             |
| Standard_A10 | 8         | 56           | 382                   | 16             | 16 x 500             | 2 / hoog                  |
| Standard_A11 | 16        | 112          | 382                   | 16             | 16 x 500             | 4 / zeer hoog             |

* RDMA kunnen

## <a name="d-series"></a>D-reeks


| Grootte         | CPU-kernen | Geheugen: verwijderen | Lokale SSD: verwijderen | Max gegevensschijven | Max schijf gegevensdoorvoer: IOP's | Max NIC / netwerkbandbreedte |
|--------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_D1  | 1         | 3,5-inch          | 50                   | 2              | 2 x 500              | 1 / matig              |
| Standard_D2  | 2         | 7            | 100                  | 4              | 4 x 500              | 2 / hoog                  |
| Standard_D3  | 4         | 14           | 200                  | 8              | 8 x 500              | 4 / hoog                  |
| Standard_D4  | 8         | 28           | 400                  | 16             | 16 x 500             | 8 / hoog                  |
| Standard_D11 | 2         | 14           | 100                  | 4              | 4 x 500              | 2 / hoog                  |
| Standard_D12 | 4         | 28           | 200                  | 8              | 8 x 500              | 4 / hoog                  |
| Standard_D13 | 8         | 56           | 400                  | 16             | 16 x 500             | 8 / hoog                  |
| Standard_D14 | 16        | 112          | 800                  | 32             | 32 x 500             | 8 / zeer hoog             |

## <a name="dv2-series"></a>Dv2-serie

| Grootte            | CPU-kernen | Geheugen: verwijderen | Lokale SSD: verwijderen | Max gegevensschijven | Max schijf gegevensdoorvoer: IOP's | Max NIC / netwerkbandbreedte |
|-----------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_D1_v2  | 1         | 3,5-inch          | 50                   | 2              | 2 x 500              | 1 / matig              |
| Standard_D2_v2  | 2         | 7            | 100                  | 4              | 4 x 500              | 2 / hoog                  |
| Standard_D3_v2  | 4         | 14           | 200                  | 8              | 8 x 500              | 4 / hoog                  |
| Standard_D4_v2  | 8         | 28           | 400                  | 16             | 16 x 500             | 8 / hoog                  |
| Standard_D5_v2  | 16        | 56           | 800                  | 32             | 32 x 500             | 8 / zeer hoog        |
| Standard_D11_v2 | 2         | 14           | 100                  | 4              | 4 x 500              | 2 / hoog                  |
| Standard_D12_v2 | 4         | 28           | 200                  | 8              | 8 x 500              | 4 / hoog                  |
| Standard_D13_v2 | 8         | 56           | 400                  | 16             | 16 x 500             | 8 / hoog                  |
| Standard_D14_v2 | 16        | 112          | 800                  | 32             | 32 x 500             | 8 / zeer hoog        |
| Standard_D15_v2 | 20        | 140          | 1000                | 40             | 40 x 500             | 8 / zeer hoog        |

## <a name="g-series"></a>G-serie

| Grootte        | CPU-kernen | Geheugen: verwijderen  | Lokale SSD: verwijderen  | Max gegevensschijven | Max schijf, doorvoersnelheid: IOP's | Max NIC / netwerkbandbreedte |
|-------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_G1 | 2         | 28           | 384                  | 4              | 4 x 500            | 1 / hoog                  |
| Standard_G2 | 4         | 56           | 768                  | 8              | 8 x 500            | 2 / hoog                  |
| Standard_G3 | 8         | 112          | 1,536                | 16             | 16 x 500           | 4 / zeer hoog             |
| Standard_G4 | 16        | 224          | 3.072                | 32             | 32 x 500           | 8 / zeer hoog        |
| Standard_G5 | 32        | 448          | 6,144                | 64             | 64 x 500           | 8 / zeer hoog        |


## <a name="h-series"></a>H-serie

Azure H-reeks virtuele machines zijn de volgende generatie HPC dat VMS high end behoeften, zoals moleculaire modellering en computational fluid dynamics gericht. Deze 8 en 16 core VMs zijn gebaseerd op de Intel Haswell E5 2667 V3-processortechnologie met DDR4 geheugen en lokale opslag van SSD-technologie gebaseerd. 

Naast de aanzienlijke CPU-kracht biedt de H-reeks verschillende opties voor lage latentie RDMA netwerken met InfiniBand FDR en verschillende geheugenconfiguraties ondersteuningsdiensten geheugen intensieve rekenkundige.


| Grootte           | CPU-kernen | Geheugen: verwijderen | Lokale SSD: verwijderen | Max gegevensschijven | Max schijf, doorvoersnelheid: IOP's | Max NIC / netwerkbandbreedte |
|----------------|-----------|-------------|--------------------------|----------------|---------------------------|------------------------------|
| Standard_H8    | 8         | 56          | 1000                     | 16             | 16 x 500                    | 8 / hoog                      |
| Standard_H16   | 16        | 112         | 2000                     | 32             | 32 x 500                    | 8 / zeer hoog                  |
| Standard_H8m   | 8         | 112         | 1000                     | 16             | 16 x 500                    | 8 / hoog                      |
| Standard_H16m  | 16        | 224         | 2000                     | 32             | 32 x 500                    | 8 / zeer hoog                 |
| Standard_H16r * | 16        | 112         | 2000                     | 32             | 32 x 500                    | 8 / zeer hoog                  |
| Standard_H16mr * | 16        | 224         | 2000                     | 32             | 32 x 500                    | 8 / zeer hoog                  |


* RDMA kunnen

## <a name="notes-standard-a0---a4-using-cli-and-powershell"></a>Opmerkingen: Standaard A0 - A4 CLI en PowerShell gebruiken 

In het implementatiemodel klassiek zijn sommige grootte VM namen iets anders in de CLI en PowerShell:

* Standard_A0 is ExtraSmall 
* Standard_A1 is een kleine
* Standard_A2 is gemiddeld
* Standard_A3 is groot
* Standard_A4 is ExtraLarge

## <a name="configure-sizes-for-cloud-services"></a>Formaten voor Cloud-Services configureren

U kunt de grootte van de virtuele Machine van het exemplaar van een rol als onderdeel van de service-model dat wordt beschreven door het [definitiebestand](cloud-services-model-and-package.md#csdef)opgeven. De grootte van de functie bepaalt het aantal CPU cores, de geheugencapaciteit en de grootte van het lokale systeem dat is toegewezen aan een actief exemplaar. Kies de grootte van de rol op basis van de vereisten van uw toepassing.

Hier volgt een voorbeeld voor het instellen van de grootte van de functie [Standard_D2](#general-purpose-d) worden voor een exemplaar van de rol van de webpagina:

```xml
<WorkerRole name="Worker1" vmsize="<mark>Standard_D2</mark>">
...
</WorkerRole>
```

## <a name="next-steps"></a>Volgende stappen

- Informatie over [azure abonnement service grenzen, quota's en beperkingen](../azure-subscription-service-limits.md).
- Informatie [over de A-serie H-serie en intensieve VMs](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) voor werkbelasting zoals High performance Computing (HPC).

