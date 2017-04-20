<properties
    pageTitle="Replicatie van Azure opslag | Microsoft Azure"
    description="Gegevens in uw account Microsoft Azure opslag wordt gerepliceerd voor duurzaamheid en hoge beschikbaarheid. Replicatieopties zijn lokaal redundante opslag (LRS), zone redundante opslag (ZRS), geo-redundante opslag (GRS) en geo-redundante opslag lezen-toegang (RA-GRS)."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="tamram"/>

# <a name="azure-storage-replication"></a>Azure opslag, replicatie

De gegevens in uw account Microsoft Azure opslag wordt altijd duurzaamheid en hoge beschikbaarheid te garanderen gerepliceerd. Replicatie kopieert uw gegevens binnen dezelfde Datacenter of tot een tweede Datacenter, afhankelijk van welke replicatieoptie u kiest. Replicatie beschermt uw gegevens en uw toepassing gebruikstijd bij hardwarestoringen tijdelijk behoudt. Als uw gegevens worden gerepliceerd naar een tweede Datacenter, beveiligt die ook u uw gegevens tegen een onherstelbare fout in de primaire locatie.

Replicatie zorgt ervoor dat uw account opslag voldoet aan de [Serviceniveau overeenkomst (SLA) voor opslag](https://azure.microsoft.com/support/legal/sla/storage/) , zelfs tegen storingen. Zie dat de SLA voor informatie over Azure opslag voor duurzaamheid en beschikbaarheid garandeert. 

Wanneer u een opslag-account maakt, kunt u een van de volgende replicatieopties:  

- [Lokaal redundante opslag (LRS)](#locally-redundant-storage)
- [Zone redundante opslag (ZRS)](#zone-redundant-storage)
- [Geo-redundante opslag (GRS)](#geo-redundant-storage)
- [Geo-redundante opslag lezen-toegang (RA-GRS)](#read-access-geo-redundant-storage)

Geo-redundante opslag lezen-toegang (RA-GRS) is de standaardoptie wanneer u een nieuwe opslag-account maken.

In de volgende tabel biedt een beknopt overzicht van de verschillen tussen LRS, ZRS, GRS en GRS-RA, hoewel latere secties elk type replicatie in meer detail.

| Replicatiestrategie voor                                                               | LRS | ZRS | GRS | RA GRS |
|:----------------------------------------------------------------------------------|:---|:---|:---|:------|
| Gegevens worden gerepliceerd naar meerdere datacenters.                                     | Nee  | Ja | Ja | Ja    |
| Gegevens kunnen worden gelezen uit de tweede locatie als de primaire locatie. | Nee  | Nee  | Nee  | Ja    |
| Het aantal kopieën van gegevens op afzonderlijke knooppunten worden onderhouden.                             | 3   | 3   | 6   | 6      |

Zie [Azure opslag prijzen](https://azure.microsoft.com/pricing/details/storage/) voor informatie over de redundantieopties voor voor verschillende prijzen.

>[AZURE.NOTE] Premium opslag ondersteunt alleen lokaal redundante opslag (LRS). Zie voor meer informatie over opslag Premium [Premium-opslag: krachtige opslag voor een standaardwerkbelasting Azure Virtual Machine](storage-premium-storage.md).

## <a name="locally-redundant-storage"></a>Lokaal redundante opslag

Lokaal redundante opslag (LRS) uw gegevens drie keer in een schaal opslageenheid die wordt gehost in een datacenter in de regio waarin u uw account opslag gemaakt worden gerepliceerd. Een schrijfopdracht retourneert is slechts één keer naar alle drie de replica is geschreven. Deze drie replica's ieder bevinden zich in de fout met betrekking tot afzonderlijke domeinen en upgrade-domeinen binnen een opslageenheid voor schaal. 

Een opslageenheid van de schaal is een verzameling van rekken van opslagknooppunten. Een domein voor fouttolerantie (FD) is een groep van knooppunten die vertegenwoordigen een fysieke eenheid van de fout en kan worden beschouwd als knooppunten die behoren tot hetzelfde fysieke rek. Een upgrade (UD)-domein is een groep van knooppunten die samen worden bijgewerkt tijdens het upgraden van een service (implementatie). De drie replica's zijn verspreid over UDs en FDs binnen de schaal van een opslageenheid om ervoor te zorgen dat gegevens beschikbaar is, zelfs als één rack heeft gevolgen voor hardwarestoringen of als u knooppunten hebt geüpgraded tijdens een implementatie. 

LRS optie is de laagste kosten en minimale houdbaarheid in vergelijking met andere opties biedt. Alle drie replica's kunnen een storing datacenter niveau (brand, overstroming, enz.) zijn verloren gaan of onherstelbaar. Om dit risico beperken wordt Geo-redundante opslag (GRS) aanbevolen voor de meeste toepassingen.

Lokaal redundante opslag kan nog wenselijk zijn in bepaalde scenario's: 

- Biedt een maximale bandbreedte van de hoogste van Azure replicatie opslagopties.

- Als uw toepassing gegevens die gemakkelijk kan worden gerepareerd opgeslagen, kan u kiezen voor LRS.

- Sommige toepassingen zijn beperkt tot het repliceren van gegevens alleen binnen een land dat governance gegevensvereisten. Een gepaarde regio kan worden in een ander land; voor meer informatie over de regio paren Zie [Azure regio's](https://azure.microsoft.com/regions/) .

## <a name="zone-redundant-storage"></a>Zone redundante opslag

Zone redundante opslag (ZRS) repliceert de gegevens asynchroon op datacenters binnen één of twee gebieden naast drie vergelijkbaar met LRS, aldus een hogere duurzaamheid dan LRS replica's op te slaan. Gegevens die zijn opgeslagen in de ZRS is duurzaam, zelfs als de primaire datacenter niet beschikbaar of onherstelbare is.
Klanten die van plan bent te gebruiken ZRS moeten zich bewust zijn dat: 

- ZRS is alleen beschikbaar voor BLOB's blokkeren op algemene opslag rekeningen en wordt ondersteund in versies van opslag service 2014-02-14 en hoger. 

- Omdat asynchrone replicatie een vertraging moet, in geval van een lokale ramp is het mogelijk dat wijzigingen die nog niet zijn gerepliceerd naar de secundaire verloren zullen gaan als de gegevens van de primaire kan niet worden hersteld.

- De replica zijn mogelijk niet beschikbaar totdat Microsoft failover naar de secundaire initieert.

- ZRS rekeningen kunnen niet later worden geconverteerd naar LRS of GRS. Ook kan niet een bestaande LRS of GRS-account worden geconverteerd naar een ZRS.

- ZRS-accounts hebben geen statistieken of mogelijkheid tot het vastleggen. 

## <a name="geo-redundant-storage"></a>Geo-redundante opslag

Uw gegevens repliceert (GRS) voor geo-redundante opslag naar een secundaire gebied dat honderden mijlen van de primaire regio. Als uw account opslag GRS ingeschakeld is, zijn uw gegevens is duurzaam, zelfs in het geval van een volledige regionale stroomstoring of een noodsituatie waarin de primaire regio kan niet worden hersteld.

Voor een opslagruimte rekening met GRS ingeschakeld, hecht een update eerst op de primaire regio, waar het drie keer wordt gerepliceerd. De update wordt gerepliceerd asynchroon naar de secundaire regio, waarin drie keer wordt ook gerepliceerd. 

Met GRS de primaire en secundaire gebieden replica's in de fout met betrekking tot afzonderlijke domeinen beheren en upgraden van domeinen binnen een opslageenheid schaal met LRS beschreven.

Overwegingen met betrekking tot:

- Omdat asynchrone replicatie een vertraging moet, in geval van een regionale ramp is het mogelijk dat wijzigingen die nog niet zijn gerepliceerd naar de secundaire regio verloren zullen gaan als de gegevens uit de primaire regio kan niet worden hersteld.

- De replica is alleen beschikbaar als Microsoft failover naar de secundaire regio initieert.

- Als een toepassing wil lezen uit de regio secundaire DAT RA GRS moet worden ingeschakeld door de gebruiker. 

Wanneer u een opslag-account maakt, selecteert u de primaire regio voor de account. Het tweede gebied wordt bepaald op basis van de primaire regio en kan niet worden gewijzigd. De volgende tabel ziet u de primaire en secundaire regio koppelingen.

| Primaire             | Secundaire           |
|---------------------|---------------------|
| Centrale Noord-Amerikaanse    | Zuid-centraal-Verenigde Staten    |
| Zuid-centraal-Verenigde Staten    | Centrale Noord-Amerikaanse    |
| Oost-Verenigde Staten             | West-Verenigde Staten             |
| West-Verenigde Staten             | Oost-Verenigde Staten             |
| Verenigde Staten Oost 2           | Centrale VS          |
| Centrale VS          | Verenigde Staten Oost 2           |
| Noord-Europa        | West-Europa         |
| West-Europa         | Noord-Europa        |
| Zuid-Oost-Azië     | Oost-Azië           |
| Oost-Azië           | Zuid-Oost-Azië     |
| Oost-China          | Noord-China         |
| Noord-China         | Oost-China          |
| East Japan          | West Japan          |
| West Japan          | East Japan          |
| Brazilië-Zuid        | Zuid-centraal-Verenigde Staten    |
| Australië, Oost      | Zuidoost Australië |
| Zuidoost Australië | Australië, Oost      |
| Zuid India         | Centraal India       |
| Centraal India       | Zuid India         |
| Amerikaanse beurs Iowa         | Amerikaanse finan-Virginia     |
| Amerikaanse finan-Virginia     | Amerikaanse beurs Iowa         |
| Canada Central      | Canada Oost          |
| Canada Oost         | Canada Central      |
| Verenigd Koninkrijk West             | Groot-Brittannië-Zuid            |
| Groot-Brittannië-Zuid            | Verenigd Koninkrijk West             |
| Duitsland-centrale     | Noordoost Duitsland   |
| Noordoost Duitsland   | Duitsland-centrale     |
| West VS 2           | Centraal West VS     |
| Centraal West VS     | West VS 2           |

Zie voor actuele informatie over de regio's die worden ondersteund door Azure [Azure regio's](https://azure.microsoft.com/regions/).
 
## <a name="read-access-geo-redundant-storage"></a>Geo-redundante opslag lezen toegang

Geo-redundante opslag lezen-toegang (RA-GRS) maximaal beschikbaar voor uw account opslag door middel van alleen-lezen toegang tot de gegevens in de secundaire locatie, naast de replicatie tussen de twee regio's die door GRS. 

Als u alleen-lezen toegang tot uw gegevens in de secundaire regio inschakelt, is uw gegevens beschikbaar op een secundaire eindpunt, naast het primaire eindpunt voor uw opslag-account. Het secundaire eindpunt is vergelijkbaar met het primaire eindpunt, maar voegt het achtervoegsel `–secondary` aan de accountnaam. Bijvoorbeeld, als uw primaire eindpunt voor de Blob-service is `myaccount.blob.core.windows.net`, dan is uw secundaire eindpunt `myaccount-secondary.blob.core.windows.net`. De toegangstoetsen voor de account van uw opslag zijn hetzelfde voor de primaire en secundaire eindpunten.

Overwegingen met betrekking tot:

- De toepassing heeft voor het beheren van welk endpoint het is interactie met RA GRS gebruiken. 

- RA GRS is bedoeld voor hoge beschikbaarheid. Raadpleeg de [Controlelijst voor prestaties](storage-performance-checklist.md)voor richtlijnen voor de schaalbaarheid.

## <a name="next-steps"></a>Volgende stappen

- [Azure opslag prijzen](https://azure.microsoft.com/pricing/details/storage/)
- [Over Azure opslag accounts](storage-create-storage-account.md)
- [Azure opslag schaalbaarheid en prestaties, doelen](storage-scalability-targets.md)
- [Redundantieopties voor Microsoft Azure opslag en leestoegang Geo-redundante opslag](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)  
- [SOSP papier - Azure opslag: Een hoge beschikbaarheid Cloud Storage-Service met een sterke samenhang](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)  
