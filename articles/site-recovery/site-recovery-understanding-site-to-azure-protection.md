<properties
    pageTitle="Hyper-V-replicatie met Azure Site herstellen | Microsoft Azure"
    description="In dit artikel gebruiken om te begrijpen van de technische concepten die u met succes installeren, configureren en beheren van Azure Site herstellen."
    services="site-recovery"
    documentationCenter=""
    authors="Rajani-Janaki-Ram"
    manager="mkjain"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="09/12/2016"
    ms.author="rajanaki"/>  


# <a name="hyper-v-replication-with-azure-site-recovery"></a>Hyper-V-replicatie met Azure Site herstellen

Dit artikel beschrijft de technische concepten die u kunnen configureren en beheren van een Hyper-V-site of een site System Center Virtual Machine Manager (VMM) Azure bescherming via Azure Site herstellen.

## <a name="setting-up-the-source-environment-for-replication-between-on-premises-and-azure"></a>Instellen van de bronomgeving voor replicatie tussen lokale en Azure

Als onderdeel van het instellen van herstel tussen lokale en Azure vergeet te downloaden en installeren van Azure Site Recovery-Provider op de VMM-server. Azure Services herstelagent installeren op elke host Hyper-V.

![Website-implementatie voor replicatie tussen lokale en Azure VMM](media/site-recovery-understanding-site-to-azure-protection/image00.png)

Instellen van de bronomgeving in een beheerd Hyper-V-infrastructuur is vergelijkbaar met het instellen van de bronomgeving in een beheerde infrastructuur VMM. Het enige verschil is dat de provider en de agent worden geïnstalleerd op de Hyper-V host zelf. De provider is geïnstalleerd op de server VMM en de agent is geïnstalleerd op de Hyper-V hosts (in het geval van replicatie naar Azure) in de omgeving van VMM.

## <a name="workflows"></a>Workflows

### <a name="enable-protection"></a>Beveiliging inschakelen
Nadat u een virtuele machine tegen de Azure portal of in ruimten beschermen, begint een taak Recovery Site **Beveiliging inschakelen** . U kunt het controleren op het tabblad **taken** .

![De taak 'Beveiliging inschakelen' in de lijst met taken](media/site-recovery-understanding-site-to-azure-protection/image001.PNG)

De vereisten controleert voordat de methode [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) de taak **Beveiliging inschakelen** . Deze methode maakt een Azure-replicatie met behulp van "inputs" die worden geconfigureerd tijdens de bescherming.

De **Beveiliging inschakelen** taak begint de eerste replicatie op ruimten door het aanroepen van de methode [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) . Deze methode verzendt de virtuele machine virtuele schijven naar Azure.

![Details voor de taak "Beveiliging inschakelen"](media/site-recovery-understanding-site-to-azure-protection/IMAGE002.PNG)

### <a name="finalize-protection-on-the-virtual-machine"></a>Bescherming op de virtuele machine voltooien
Een [momentopname van de Hyper-V VM](https://technet.microsoft.com/library/dd560637.aspx) wordt genomen wanneer de eerste replicatie wordt geactiveerd. Virtuele harde schijven zijn stuk voor stuk verwerkt totdat alle schijven worden geüpload naar Azure. Dit duurt gewoonlijk om te voltooien, op basis van de grootte van de schijf en de bandbreedte. Zie voor een optimaal netwerkgebruik van uw, [op gebouwen aan het verbruik van de netwerkbandbreedte Azure bescherming beheren](https://support.microsoft.com/kb/3056159).

Nadat de eerste replicatie is voltooid, wordt de taak **Voltooien bescherming op de virtuele machine** geconfigureerd netwerk en na replicatie-instellingen. Terwijl de eerste replicatie uitgevoerd wordt:

- Alle wijzigingen aan de schijven worden bijgehouden. 
- Extra schijfruimte wordt verbruikt voor de momentopname en Hyper-V Replica logboek (HRL) bestanden.

Op de eerste replicatie is voltooid, wordt de Hyper-V VM momentopname verwijderd. Deze verwijdering resulteert in het samenvoegen van wijzigingen in gegevens na de eerste replicatie op de bovenliggende schijf.

![De taak 'Finalize bescherming op de virtuele machine' in de lijst met taken](media/site-recovery-understanding-site-to-azure-protection/image03.png)

### <a name="delta-replication"></a>Delta-replicatie
Hyper-V Replica replicatie vastleggen, dat deel van de Hyper-V Replica replicatie-Engine, tracks de wijzigingen op een virtuele harde schijf als Hyper-V Replica (*.hrl)-logboekbestanden uitmaakt. HRL bestanden zich in dezelfde map als de gekoppelde schijven.

Elke schijf die geconfigureerd voor replicatie heeft een bijbehorend HRL-bestand. Dit logboek wordt verzonden naar de klantenrekening opslag nadat de eerste replicatie is voltooid. Wanneer een logbestand op weg naar Azure is, worden de wijzigingen in de primaire bijgehouden in een ander bestand in dezelfde map.

U kunt tijdens de eerste replicatie of replicatie delta, VM replicatie gezondheid in de weergave VM controleren zoals vermeld in [de gezondheid van replication Monitor voor virtuele machine](./site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machine).  

### <a name="resynchronization"></a>Opnieuw synchroniseren
Een virtuele machine is gemarkeerd voor het opnieuw wanneer zowel delta replicatie mislukt als de volledige replicatie erg nadelig voor de bandbreedte van het netwerk of de tijd is. Bijvoorbeeld wanneer de grootte van HRL stapels maximaal 50 procent van de totale schijfgrootte, is de virtuele machine gemarkeerd voor het opnieuw. Opnieuw synchroniseren minimaliseert u de hoeveelheid gegevens die via het netwerk worden verzonden door controlesommen van de bron- en virtuele machine schijven te sturen alleen het verschil.

Nadat de synchronisatieprocedure is voltooid, te delta normale replicatie hervatten. U kunt het opnieuw hervatten als er een netwerkstoring of een andere stroomstoring optreedt.

Automatisch geplande opnieuw synchroniseren is standaard geconfigureerd voor gebeuren buiten de werkuren. Als de virtuele machine handmatig opnieuw worden gesynchroniseerd moet, selecteer de virtuele machine vanaf de portal en klikt u op **synchroniseren**.

![Handmatig opnieuw synchroniseren](media/site-recovery-understanding-site-to-azure-protection/image04.png)

Opnieuw synchroniseren gebruikt een algoritme chunking vaste blok waar bron-en vaste delen zijn verdeeld. Controlesommen voor elk segment worden gegenereerd en vervolgens vergeleken om te bepalen welke blokken van de bron moeten worden toegepast op het doel.

### <a name="retry-logic"></a>Logica opnieuw
Er is een ingebouwde Pogingslogica voor replicatiefouten. Deze logica kan worden ingedeeld in twee categorieën:

| Categorie                  | Scenario 's                                    |
|---------------------------|----------------------------------------------|
| Onherstelbare fout     | Geen nieuwe poging wordt gedaan. De replicatiestatus virtuele machine is **essentieel**en tussenkomst van de beheerder is vereist. Voorbeelden zijn: <ul><li>Verbroken keten van VHD</li><li>Ongeldige status voor de virtuele machine van replica</li><li>Fout tijdens netwerkverificatie</li><li>Autorisatiefout</li><li>Virtuele machine die niet is gevonden in het geval van een zelfstandige Hyper-V server</li></ul>|
| Herstelbare fout         | Pogingen plaatsvinden om replicatie-interval met een exponentiële back-off die vanaf het begin van de eerste poging (1, 2, 4, 8, 10 minuten) het interval voor nieuwe pogingen. Als een fout zich blijft voordoen, probeer het opnieuw elke 30 minuten. Voorbeelden zijn: <ul><li>Netwerkfout</li><li>Onvoldoende schijfruimte</li><li>Onvoldoende geheugen beschikbaar</li></ul>|

## <a name="hyper-v-virtual-machine-protection-and-recovery-life-cycle"></a>Levenscyclus van Hyper-V virtuele machine bescherming en herstel

![Levenscyclus van Hyper-V virtuele machine bescherming en herstel](media/site-recovery-understanding-site-to-azure-protection/image05.png)

## <a name="other-references"></a>Andere verwijzingen

- [Bewaken en problemen oplossen met de beveiliging voor VMware, VMM, Hyper-V en fysieke locaties](./site-recovery-monitoring-and-troubleshooting.md)
- [Voor Microsoft Support brengen](./site-recovery-monitoring-and-troubleshooting.md#reaching-out-for-microsoft-support)
- [Veelvoorkomende fouten in Azure Site herstellen en hun oplossingen](./site-recovery-monitoring-and-troubleshooting.md#common-asr-errors-and-their-resolutions)
