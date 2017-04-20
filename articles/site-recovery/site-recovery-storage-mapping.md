<properties
    pageTitle="Opslag in Azure Site herstel voor Hyper-V virtuele machine replicatie tussen datacenters voor ruimten toewijzen | Microsoft Azure"
    description="Toewijzing van opslagruimte voor Hyper-V virtuele machine replicatie tussen twee op locatie datacenters met Azure Site herstel voorbereiden."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="07/06/2016"
    ms.author="raynew"/>


# <a name="prepare-storage-mapping-for-hyper-v-virtual-machine-replication-between-two-on-premises-datacenters-with-azure-site-recovery"></a>Toewijzing van opslagruimte voor Hyper-V virtuele machine replicatie tussen twee op locatie datacenters met Azure Site herstel voorbereiden


Azure-Site-Recovery draagt bij aan de continuïteit en noodherstel recovery (BCDR) bedrijfsstrategie door regie van replicatie, failover en herstel van virtuele machines en fysieke servers. Opslag toewijzing, waardoor u optimaal gebruik van opslag wanneer u Hyper-V virtuele machines te repliceren tussen twee op ruimten VMM datacenters Recovery Site beschreven.

Opmerkingen of vragen aan de onderkant van dit artikel, of op het [Forum van Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)boeken.

## <a name="overview"></a>Overzicht

Toewijzing van de opslagruimte is alleen relevant wanneer u bij het repliceren van Hyper-V virtuele machines die zich bevinden in de VMM wolken van een datacenter primaire naar een secundaire datacenter met Hyper-V-Replica of SAN-replicatie als volgt:


- **On-premises replicatie voor ruimten met Hyper-V-Replica)** — U opslag instellen door toewijzing toewijzing classificaties voor opslag op een bron en VMM doelservers doet het volgende:

    - **Vaststellen doel opslag voor replica virtuele machines**, virtuele machines worden gerepliceerd naar een doel opslag (SMB delen of gedeelde volumes (CSVs) van cluster) die u kiest.
    - **Plaatsing van de virtuele machine replica**, toewijzing van de opslagruimte optimaal replica virtuele machines plaatst op de host-servers Hyper-V wordt gebruikt. Replica virtuele machines wordt op hosts die toegang hebben tot de toegewezen opslag-indeling geplaatst.
    - **Geen opslag-toewijzing**, als opslag toewijzing niet is geconfigureerd, virtuele machines in de standaardlocatie voor opslag opgegeven op de Hyper-V host-server die is gekoppeld aan de virtuele machine van replica worden gerepliceerd.

- **On-premises replicatie in ruimten met SAN**— u opslag instellen door toewijzing toewijzing opslagpools arrays op een bron en VMM doelservers.
    - **Geef toepassingen**— Hiermee geeft u op welke toepassingen secundaire opslag replicatiegegevens van de primaire groep ontvangt.
    - **Vaststellen doel opslagpools**, zorgt u ervoor dat LUN's in een replicatiegroep bron worden gerepliceerd naar de opslaggroep toegewezen doel van uw keuze.

## <a name="set-up-storage-classifications-for-hyper-v-replication"></a>Opslag classificaties voor Hyper-V-replicatie instellen

Wanneer u Hyper-V-Replica worden gerepliceerd met terugwinning van de Site, kunt u tussen de classificaties voor opslag op bron- en doeldomein VMM-servers of op één server VMM toewijzen als twee sites worden beheerd door dezelfde VMM-server. Houd rekening met:

- Als toewijzing correct is geconfigureerd en replicatie is ingeschakeld, wordt een virtuele machine van virtuele harde schijf op de primaire locatie voor opslag in de doellocatie toegewezen worden gerepliceerd.
- Classificaties opslag moeten beschikbaar zijn voor de hostgroepen in het bron- en wolken.
- Classificaties hoeft niet hetzelfde soort opslag hebben. U kunt bijvoorbeeld een bron classificatie met SMB-shares voor een doel-classificatie met CSVs toewijzen.
- Meer [classificaties in VMM opslag maken](https://technet.microsoft.com/library/gg610685.aspx).

## <a name="example"></a>Voorbeeld

Als classificaties correct zijn geconfigureerd in de VMM wanneer u de bron en doel VMM server tijdens de opslag toewijzing, wordt het bron- en classificaties weergegeven. Hier volgt een voorbeeld van bestandsshares opslag en classificaties voor een organisatie met twee vestigingen in New York en Chicago.

**Locatie** | **VMM-server** | **Bestandsshare (bron)** | **Classificatie (bron)** | **Toegewezen aan** | **Bestandsshare (doel)**
---|---|--- |---|---|---
New York | VMM_Source| SourceShare1 | GOUD | GOLD_TARGET | TargetShare1
 |  | SourceShare2 | SILVER | SILVER_TARGET | TargetShare2
 | | SourceShare3 | BRONS | BRONZE_TARGET | TargetShare3
Chicago | VMM_Target |  | GOLD_TARGET | Niet toegewezen |
| | | SILVER_TARGET | Niet toegewezen |
 | | | BRONZE_TARGET | Niet toegewezen

U zou deze op het tabblad **Server-opslag** configureren op de pagina **Resources** van de portal-Site terugzetten.

![Toewijzing van opslag configureren](./media/site-recovery-storage-mapping/storage-mapping1.png)

Met dit voorbeeld:
- Wanneer een virtuele machines replica wordt gemaakt voor elke virtuele machine op GOLD storage (SourceShare1), deze wordt gerepliceerd naar de opslag van een GOLD_TARGET (TargetShare1).
- Als een replica virtuele machine wordt gemaakt voor elke virtuele machine op zilver opslag (SourceShare2), deze wordt gerepliceerd naar de opslag van een SILVER_TARGET (TargetShare2), enzovoort.

De werkelijke bestandsshares en hun toegewezen categorieën in de VMM weergegeven in de volgende schermafdruk.

![Opslag-classificaties in VMM](./media/site-recovery-storage-mapping/storage-mapping2.png)

## <a name="multiple-storage-locations"></a>Meerdere locaties

Als de doel-indeling met meerdere SMB-shares of CSVs is toegewezen, wordt de locatie van het optimale automatisch geselecteerd wanneer de virtuele machine is beveiligd. Als geen doel geschikte opslag beschikbaar met de opgegeven indeling is, wordt de standaardopslaglocatie op de Hyper-V host opgegeven gebruikt de replica virtuele harde schijven te plaatsen.

In de volgende tabel wordt weergegeven hoe opslag indeling en gedeelde clustervolumes zijn ingesteld in ons voorbeeld.

**Locatie** | **Classificatie** | **Bijbehorende opslag**
---|---|---
New York | GOUD | <p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p>
 | SILVER | <p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p>
Chicago | GOLD_TARGET | <p>C:\ClusterStorage\TargetVolume1</p><p>\\FileServer\TargetShare1</p>
 | SILVER_TARGET| <p>C:\ClusterStorage\TargetVolume2</p><p>\\FileServer\TargetShare2</p>

Deze tabel wordt het gedrag wanneer u de beveiliging voor virtuele machines (VM1 - VM5) in de voorbeeldomgeving van dit inschakelt.

**Virtuele machine** | **Bron-opslag** | **Classificatie van de bron** | **Doel toegewezen opslag**
---|---|---|---
VM1 | C:\ClusterStorage\SourceVolume1 | GOUD | <p>C:\ClusterStorage\SourceVolume1</p><p>\\\FileServer\SourceShare1</p><p>Beide GOLD_TARGET</p>
VM2 | \\FileServer\SourceShare1 | GOUD | <p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p> <p>Beide GOLD_TARGET</p>
VM3 | C:\ClusterStorage\SourceVolume2 | SILVER | <p>C:\ClusterStorage\SourceVolume2</p><p>\FileServer\SourceShare2</p>
VM4 | \FileServer\SourceShare2 | SILVER |<p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p><p>Beide SILVER_TARGET</p>
VM5 | C:\ClusterStorage\SourceVolume3 | N.V.T. | Er is geen toewijzing, zodat de standaardlocatie voor de opslag van de Hyper-V host wordt gebruikt.

## <a name="next-steps"></a>Volgende stappen

Nu dat er een beter begrip van de opslag toewijzing, [bereid te implementeren Azure Site herstellen](site-recovery-best-practices.md).
