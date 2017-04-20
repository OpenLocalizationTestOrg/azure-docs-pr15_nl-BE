<properties
  pageTitle="Azure en Linux VM opslag | Microsoft Azure"
  description="Beschrijving van Azure Standard en Premium-opslagruimte met Linux virtuele machines."
  services="virtual-machines-linux"
  documentationCenter="virtual-machines-linux"
  authors="vlivech"
  manager="timlt"
  editor=""/>

<tags
  ms.service="virtual-machines-linux"
  ms.devlang="NA"
  ms.topic="article"
  ms.tgt_pltfrm="vm-linux"
  ms.workload="infrastructure"
  ms.date="10/04/2016"
  ms.author="v-livech"/>

# <a name="azure-and-linux-vm-storage"></a>Azure en Linux VM opslag

Azure opslag is de cloud storage oplossing voor moderne toepassingen die afhankelijk zijn van duurzaamheid, beschikbaarheid en schaalbaarheid om te voldoen aan de behoeften van hun klanten.  Maken voor ontwikkelaars op grootschalige toepassingen ter ondersteuning van nieuwe scenario's mogelijk Azure opslag beschikt bovendien over de Stichting opslag voor Azure virtuele Machines.

## <a name="azure-storage-standard-and-premium"></a>Azure opslag: Standard en Premium

Azure VM kan worden gebaseerd op de vaste schijven of schijven voor premium.  Bij het gebruik van de Portal voor kiezen uw VM kunt u een vervolgkeuzelijst op het scherm grondbeginselen standard en premium schijven weergeven moet schakelen.  Het screenshot hieronder geeft dat menu in-/ uitschakelen.  Wanneer ingeschakeld voor SSD, alleen premium opslag VMs ingeschakeld wordt weergegeven, die alle ondersteund door SSD schijven.  Wanneer ingeschakeld op de harde schijf, ingeschakeld standaardopslag VMs back draaiende schijven weergegeven, samen met VMs gesteund door SSD premium-opslag.

  ![scherm1](../virtual-machines/media/virtual-machines-linux-azure-vm-storage-overview/screen1.png)

Bij het maken van een VM uit de `azure-cli` u kunt kiezen tussen standard en premium bij het kiezen van de grootte van de VM via de `-z` of `--vm-size` cli-vlag.

### <a name="create-a-vm-with-standard-storage-vm-on-the-cli"></a>Maak een VM met standaard opslag VM op de cli

De cli-vlag `-z` Standard_A1 kiest met A1 wordt een standaard opslag op basis van Linux VM.

```bash
azure vm quick-create -g rbg \
exampleVMname \
-l westus \
-y Linux \
-Q Debian \
-u exampleAdminUser \
-M ~/.ssh/id_rsa.pub
-z Standard_A1
```

### <a name="create-a-vm-with-premium-storage-on-the-cli"></a>Maak een VM met premium-opslagruimte op de cli

De cli-vlag `-z` Standard_DS1 kiest DS1 Linux VM een premium-opslagruimte wordt gebaseerd.

```bash
azure vm quick-create -g rbg \
exampleVMname \
-l westus \
-y Linux \
-Q Debian \
-u exampleAdminUser \
-M ~/.ssh/id_rsa.pub
-z Standard_DS1
```

## <a name="standard-storage"></a>Standaardopslag

Standaardopslag Azure is het soort opslag.  Standaardopslag is goedkoper maar zodat.  

## <a name="premium-storage"></a>Premium-opslagruimte

Azure Premium opslag biedt ondersteuning voor hoge prestaties, lage latentie schijven voor virtuele machines met I/O-intensieve belasting. Virtual machine (VM) schijven die gebruikmaken van de premie opslag opslaan gegevens op een solid state harde schijven (SSD). U kunt van uw toepassing VM schijven migreren naar Azure Premium opslag geprofiteerd van de snelheid en prestaties van deze schijven.

Premium-functies voor opslag:

- Schijven voor Premium: Azure Premium opslag ondersteunt VM schijven die kunnen worden gekoppeld aan DS, DSv2 of GS serie Azure VMs.

- Premium pagina Blob: Premium opslag ondersteunt Azure pagina Blobs die bestemd is voor permanente schijven voor Azure virtuele Machines (VMs).

- Premium lokaal redundante opslag: Een rekening van de premie opslag alleen lokaal redundante opslag (LRS) als de replicatieoptie ondersteunt en drie kopieën van de gegevens binnen één regio blijft.

- [Premium-opslagruimte](../storage/storage-premium-storage.md)

## <a name="premium-storage-supported-vms"></a>Premium-opslagruimte ondersteund VMs

Premium opslag ondersteunt DS-serie, DSv2-series, GS series en Fs-serie Azure virtuele Machines (VMs). Standard- en Premium-schijven kunt u met Premium-opslagruimte van VMs ondersteund. Maar geen schijven voor Premium VM-serie niet Premium opslag compatibel zijn.

Hier volgen de Linux-distributies die we gevalideerd met Premium-opslagruimte.

| Distributie | Versie                 | Ondersteunde Kernel    |
|--------------|-------------------------|---------------------|
| Ubuntu       | 12.04                   | 3.2.0-75.110+       |
| Ubuntu       | 14.04                   | 3.13.0-44.73+       |
| Debian       | 7.x, 8.x                | 3.16.7-ckt4-1+      |
| SLES         | SLES 12                 | 3.12.36-38.1+       |
| SLES         | SLES 11 SP4             | 3.0.101-0.63.1+     |
| CoreOS       | 584.0.0+                | 3.18.4+             |
| Centos       | 6.5, 6.6, 6.7, 7.0, 7.1 | 3.10.0-229.1.2.el7+ |
| RHEL         | 6.8 +, 7.2 +              |                     |


## <a name="file-storage"></a>Opslag van bestanden

Azure bestandsopslag biedt gedeelde bestanden in de cloud met behulp van de standaard SMB-protocol. Azure-bestanden, kunt u zakelijke toepassingen die afhankelijk van bestandsservers met Azure zijn migreren. Toepassingen die worden uitgevoerd in Azure kunnen bestandsshares eenvoudig koppelen van Azure virtual machines waarop Linux wordt uitgevoerd. En met de meest recente versie van de opslag van bestanden kunt u ook een gedeeld bestand koppelen vanuit een toepassing op de ruimten die SMB 3.0 ondersteunt.  Omdat bestandsshares SMB-shares, kunt u ze openen via het standaard bestandssysteem API's.

Opslag van bestanden is gebaseerd op dezelfde technologie als Blob-, tabel- en wachtrij opslag, zodat de opslag van bestanden de beschikbaarheid, duurzaamheid, schaalbaarheid en geo-redundantie die is ingebouwd in de opslag van Azure platform biedt. Zie voor meer informatie over het bestand opslag prestatieoogmerken en -limieten Azure opslag schaalbaarheid en prestaties, doelen.

- [Azure bestandsopslag gebruiken met Linux](../storage/storage-how-to-use-files-linux.md)

## <a name="hot-storage"></a>Hot-opslag

De laag Azure hot opslag is geoptimaliseerd voor het opslaan van gegevens die vaak wordt geopend.  Hot opslag is het opslagtype voor blob winkels.

## <a name="cool-storage"></a>Leuke opslag

De laag Azure cool opslag is geoptimaliseerd voor het opslaan van gegevens die niet vaak worden benaderd en lange levensduur. Voorbeeld van de use-cases voor leuke opslag zijn back-ups, media-inhoud, wetenschappelijke gegevens, naleving en archivering van gegevens. In het algemeen, is alle gegevens die zelden worden gebruikt een perfecte keuze voor leuke opslag.

|                             | Hot opslag laag      | Leuke opslag laag     |
|:----------------------------|:---------------------:|:---------------------:|
| Beschikbaarheid                | 99,9%                 | 99%                   |
| Beschikbaarheid (RA GRS gelezen) | 99,99%                | 99,9%                 |
| Kosten               | Hogere kosten voor opslag  | Lagere kosten voor opslag   |
|                             | Lagere toegang          | Hogere toegang         |
|                             | en de transactiekosten | en de transactiekosten |


## <a name="redundancy"></a>Redundantie

De gegevens in uw account Microsoft Azure opslag wordt altijd om duurzaamheid en hoge beschikbaarheid, voldoen aan de Azure opslag SLA zelfs tegen hardwarestoringen tijdelijk gerepliceerd.

Wanneer u een opslag-account maakt, moet u een van de volgende replicatieopties selecteren:

- Lokaal redundante opslag (LRS)
- Zone redundante opslag (ZRS)
- Geo-redundante opslag (GRS)
- Geo-redundante opslag lezen-toegang (RA-GRS)

### <a name="locally-redundant-storage"></a>Lokaal redundante opslag

Lokaal redundante opslag (LRS) repliceert de gegevens in het gebied waarin u uw account opslag gemaakt. Maximale duurzaamheid, wordt elke aanvraag ten opzichte van de gegevens in uw account opslag driemaal gerepliceerd. Deze drie replica's ieder bevinden zich in de fout met betrekking tot afzonderlijke domeinen en domeinen upgraden.  Een aanvraag wordt is slechts één keer naar alle drie de replica is geschreven.

### <a name="zone-redundant-storage"></a>Zone redundante opslag

Zone redundante opslag (ZRS) repliceert de gegevens op twee of drie faciliteiten, binnen een bepaalde regio of tussen de twee regio's, die met een hogere duurzaamheid dan LRS. Als uw account opslag heeft de ZRS is ingeschakeld, zijn uw gegevens is duurzaam, zelfs wanneer een storing op een van de faciliteiten.

### <a name="geo-redundant-storage"></a>Geo-redundante opslag

Uw gegevens repliceert (GRS) voor geo-redundante opslag naar een secundaire gebied dat honderden mijlen van de primaire regio. Als uw account opslag GRS ingeschakeld is, zijn uw gegevens is duurzaam, zelfs in het geval van een volledige regionale stroomstoring of een noodsituatie waarin de primaire regio kan niet worden hersteld.

### <a name="read-access-geo-redundant-storage"></a>Geo-redundante opslag lezen toegang

Geo-redundante opslag lezen-toegang (RA-GRS) maximaal beschikbaar voor uw account opslag door middel van alleen-lezen toegang tot de gegevens in de secundaire locatie, naast de replicatie tussen de twee regio's die door GRS. In het geval dat gegevens niet beschikbaar is in de primaire regio, kan uw toepassing gegevens lezen uit het tweede gebied.

Voor een uitgebreide Kennismaking in Azure opslag redundantie Zie:

- [Azure opslag, replicatie](../storage/storage-redundancy.md)

## <a name="scalability"></a>Schaalbaarheid

Azure opslag is sterk schaalbare, zodat u kunt opslaan en proces honderden terabytes aan gegevens ter ondersteuning van scenario's voor de grote gegevens voor het wetenschappelijke, financiële analyse en mediatoepassingen vereist. Of u kunt de kleine hoeveelheden gegevens die nodig zijn voor een klein bedrijf website opslaan. Waar uw behoeften liggen, betaalt u alleen de gegevens die u wilt opslaan. Azure opslag momenteel tientallen trillions unieke klant-objecten worden opgeslagen en verwerkt aanvragen per seconde miljoenen gemiddeld.

Voor standaardopslag rekeningen: een standaard opslag-account heeft een maximale totale verzoek tarief van 20.000 IOP's. De totale IOP's over alle schijven in een account standaardopslag virtuele machine moet deze limiet niet overschrijdt.

Voor rekeningen van premie opslag: een opslag premium account is de maximale totale doorvoersnelheid van 50 GB/s. De totale doorvoer over alle schijven VM moet deze limiet niet overschrijdt.

## <a name="availability"></a>Beschikbaarheid

Wij garanderen dat ten minste 99,99% (99,9% voor leuke Access laag) van de tijd, we met succes verzoeken om gegevens te lezen van Accounts lezen Geo-redundante opslag (RA-GRS), verwerkt op voorwaarde dat mislukte pogingen om gegevens te lezen uit de primaire regio opnieuw worden ingediend op de secundaire regio.

Wij garanderen dat ten minste 99,9% (99% voor leuke Access laag) van de tijd, we met succes verwerkt verzoeken om gegevens te lezen van lokaal redundante opslag (LRS), redundante opslag (ZRS) Zone en rekeningen voor Geo-redundante opslag (GRS).

Wij garanderen dat ten minste 99,9% (99% voor leuke Access laag) van de tijd, we met succes verwerkt verzoeken gegevens te schrijven naar een lokaal redundante opslag (LRS), Zone redundante opslag (ZRS), en Geo-redundante opslag (GRS) rekeningen en rekeningen leestoegang Geo-redundante opslag (RA-GRS).

- [Azure SLA voor opslag](https://azure.microsoft.com/support/legal/sla/storage/v1_1/)


## <a name="regions"></a>Regio 's

Azure is algemeen verkrijgbaar in 30 regio's over de hele wereld, en heeft aangekondigd plannen voor 4 extra gebieden. Geografische uitbreiding is een prioriteit voor Azure omdat Hiermee kunnen onze klanten kunnen voor betere prestaties en ondersteuning van hun eisen en voorkeuren met betrekking tot de gegevenslocatie.  Azures nieuwste regio om te starten is in Duitsland.

De Microsoft Cloud Duitsland biedt een gedifferentieerde optie om de reeds beschikbare Microsoft Cloud-diensten in Europa, maken van extra kansen voor innovatie en economische groei voor zeer gereglementeerde partners en klanten in Duitsland, de Europese Unie (EU) en de Europese Vrijhandelsassociatie (EVA).

Gegevens van de klant in deze nieuwe datacenters in Magdeburg en Frankfurt, wordt onder toezicht van een vertrouwde gegevens, T-Systems International, een onafhankelijk Duits bedrijf en een dochteronderneming van Deutsche Telekom beheerd. Commerciële cloud services van Microsoft in deze datacentra voldoen aan de Duitse gegevens verwerken van verordeningen en klanten bieden extra mogelijkheden voor het hoe en waar de gegevens worden verwerkt.


- [Azure regio's toewijzen](https://azure.microsoft.com/regions/)

## <a name="security"></a>Beveiliging

Azure opslag biedt een uitgebreide reeks beveiligingsmogelijkheden die samen kunnen ontwikkelaars toepassingen bouwen. De opslag account zelf kan worden beveiligd met behulp van toegangsbeheer op basis van rollen en Azure Active Directory. Gegevens kunnen worden beveiligd tijdens de overdracht tussen een toepassing en Azure met behulp van Client-Side codering, HTTPS of SMB 3.0. Gegevens kan worden ingesteld op automatisch worden gecodeerd als geschreven naar Azure opslag opslag Service codering (SSE) te gebruiken. OS en Data schijven die worden gebruikt door virtuele machines kunnen worden versleuteld met Azure schijf codering worden ingesteld. Gedelegeerde toegang tot de objecten data in Azure opslag kan worden verleend met gedeelde toegang handtekeningen.

### <a name="management-plane-security"></a>Beveiliging van veranderingenbeheer vlak

Het beheervlak bestaat uit de bronnen die worden gebruikt voor het beheren van uw account voor opslag. In dit gedeelte bespreken we het implementatiemodel Azure Resource Manager en het gebruik van op rollen gebaseerde toegangscontrole RBAC () toegang tot uw opslag rekeningen. We zullen ook spreken over het beheren van uw opslag account sleutels en het opnieuw genereren.

### <a name="data-plane-security"></a>Beveiliging van gegevens vlak

In dit gedeelte bekijken we toestaan van toegang tot de objecten van de werkelijke gegevens in uw account voor opslag, als BLOB's, bestanden, wachtrijen en tabellen, met behulp van gedeelde toegang tot handtekeningen en -beleid opgeslagen. Wordt ingegaan op het serviceniveau SAS zowel SAS niveau. We ziet ook hoe u de toegang beperken tot een specifiek IP-adres (of een bereik van IP-adressen), het protocol dat wordt gebruikt om HTTPS te beperken en een gedeelde Access-handtekening intrekken zonder te wachten totdat deze verlopen.

## <a name="encryption-in-transit"></a>Codering in Transit

In deze sectie wordt beschreven hoe u gegevens kunt beveiligen wanneer u deze van of naar Azure opslag overbrengt. Bespreken we het aanbevolen gebruik van HTTPS en de versleuteling van SMB 3.0 voor bestandsshares Azure. We zullen ook Kijk eens naar de Client-side codering waarmee u de gegevens versleutelen voordat deze op te slaan in een clienttoepassing wordt overgebracht en decoderen van de gegevens na het overbrengen van opslag.

## <a name="encryption-at-rest"></a>Codering in rust

We zullen hebben over Storage Service-codering (SSE) en hoe deze inschakelen voor een rekening voor opslag, waardoor de blobs blok, BLOB's pagina, en worden automatisch gecodeerd als geschreven naar opslag Azure BLOB's toevoegen. We zullen ook bekijken hoe Azure schijfversleuteling gebruiken en de belangrijkste verschillen en gevallen van schijfversleuteling versus SSE versus Client Side Encryption verkennen. Laten we zien een korte FIPS-naleving voor de computers van de Amerikaanse overheid.

- [Azure beveiligingshandleiding voor opslag](../storage/storage-security-guide.md)

## <a name="cost-savings"></a>Kosten besparen

- [Kosten opslag](https://azure.microsoft.com/pricing/details/storage/)

- [Kosten opslag Rekenmachine](https://azure.microsoft.com/pricing/calculator/?service=storage)

## <a name="storage-limits"></a>Opslagbeperkingen

- [Opslagbeperkingen Service](../azure-subscription-service-limits.md#storage-limits)
