<properties
    pageTitle="Toewijzing voor de bescherming van de Hyper-V virtuele machine met VMM in Azure-Site-Recovery netwerk voorbereiden | Microsoft Azure"
    description="Netwerk-toewijzing voor Hyper-V virtuele machine replicatie van een datacenter in lokalen Azure of een secundaire site instellen."
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
    ms.date="10/05/2016"
    ms.author="raynew"/>


# <a name="prepare-network-mapping-for-hyper-v-virtual-machine-protection-with-vmm-in-azure-site-recovery"></a>Toewijzing voor de bescherming van de Hyper-V virtuele machine met VMM in Azure-Site-Recovery netwerk voorbereiden

Azure-Site-Recovery draagt bij aan de continuïteit en noodherstel recovery (BCDR) bedrijfsstrategie door regie van replicatie, failover en herstel van virtuele machines en fysieke servers.

Netwerk-toewijzing kunt u optimaal netwerkinstellingen configureren wanneer u Hyper-V virtuele machines te repliceren via Site herstel zich in wolken VMM tussen twee op locatie datacenters, of een datacenter op gebouwen en Azure beschreven. Houd er rekening mee dat als u VMs Hyper-V zonder een wolk VMM repliceren bent of VMs VMware of fysieke servers te repliceren, dit artikel is niet relevant.

Opmerkingen of vragen aan de onderkant van dit artikel, of op het [Forum van Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)boeken.


## <a name="overview"></a>Overzicht

Toewijzing van het netwerk wordt gebruikt als Azure Site herstel Hyper-V virtuele machines te repliceren naar Azure of een secundaire datacenter wordt geïmplementeerd via replicatie van Hyper-V-Replica of SAN.

- **Repliceren van Hyper-V virtuele machines in de wolken VMM tussen twee op-locatie datacenters**, Network toewijzing toewijzingen tussen VM-netwerken op een server bron VMM en VM-netwerken op een doelserver VMM het volgende doen:

    - **Verbinding maken met virtuele machines na een failover**, zorgt u ervoor dat virtuele machines worden verbonden met de desbetreffende netwerken na een failover. De replica virtuele machine wordt verbonden met het netwerk dat is toegewezen aan het Bronnetwerk.
    - **Plaats replica virtuele machines op host-servers**— optimaal replica virtuele machines plaatst op host-servers Hyper-V. Replica virtuele machines worden geplaatst op hosts die toegang hebben tot de toegewezen VM-netwerken.
    - **Geen netwerk-toewijzing**, als u toewijzing van het netwerk niet configureren, gerepliceerde virtuele machines niet wordt aangesloten op een VM-netwerken na een failover.

- **Repliceren van Hyper-V virtuele machines in een VMM op ruimten naar Azure cloud**-toewijzing toewijzingen tussen netwerken op de bronserver VMM VM netwerk en Azure netwerken hiervoor het volgende doel:
    - **Verbinding maken met virtuele machines na een failover**-alle machines waarvoor failover op hetzelfde netwerk verbinding met elkaar maken kan, ongeacht welke herstelplan zijn.
    - **Netwerkgateway**— als een netwerkgateway is ingesteld op het doel Azure netwerk, virtuele machines kunt aansluiten op andere op ruimten virtuele machines.
    - **Er is geen toewijzing van het netwerk**, als de toewijzing van het netwerk niet is geconfigureerd, alleen de virtuele machines die overname in het herstelplan dezelfde zullen kunnen met elkaar verbinden nadat fail over naar Azure.


## <a name="network-mapping-example"></a>Voorbeeld van de netwerk-toewijzing

Netwerk koppeling worden tussen netwerken VM op twee VMM servers of op een enkele server VMM als twee sites worden beheerd door dezelfde server geconfigureerd. Toewijzing correct is geconfigureerd en replicatie is ingeschakeld, wordt een virtuele machine op de primaire locatie verbonden met een netwerk als de replica op de doelvestiging wordt verbonden met het netwerk toegewezen.

Als netwerken zijn ingesteld correct in VMM, wanneer u een doel VM netwerk bij toewijzing van het netwerk selecteert, de VMM bron wolken die gebruikmaken van de VM Bronnetwerk weergegeven, samen met de beschikbare doel VM netwerken op het doel wolken die worden gebruikt voor bescherming.

Hier volgt een voorbeeld ter illustratie van dit mechanisme. We gaan een organisatie met twee vestigingen in New York en Chicago.

**Locatie** | **VMM-server** | **VM-netwerken** | **Toegewezen aan**
---|---|---|---
New York | VMM-NewYork| VMNetwork1-NewYork | Toegewezen aan VMNetwork1-Arnhem
 |  | VMNetwork2-NewYork | Niet toegewezen
Chicago | VMM-Arnhem| VMNetwork1 Arnhem | Toegewezen aan VMNetwork1-NewYork
 | | VMNetwork2 Arnhem | Niet toegewezen

Met dit voorbeeld:

- Als een replica virtuele machine voor een virtuele machine die is aangesloten op de VMNetwork1-NewYork wordt gemaakt, wordt deze verbonden zijn met VMNetwork1 Chicago.
- Wanneer u een virtuele machine van replica is gemaakt voor VMNetwork2 NewYork of VMNetwork2-Chicago, wordt het niet verbonden met een netwerk.

Hier ziet u hoe VMM wolken in ons voorbeeld van de organisatie en de logische netwerken die zijn gekoppeld aan de wolken zijn ingesteld.

### <a name="cloud-protection-settings"></a>Cloud-beveiligingsinstellingen

**Beveiligde cloud** | **Bescherming van de cloud** | **Logisch netwerk (New York)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>NB</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1 Arnhem</p>
SilverCloud2 | <p>NB</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1 Arnhem</p>

### <a name="logical-and-vm-network-settings"></a>Logisch en VM netwerkinstellingen

**Locatie** | **Logisch netwerk** | **Gekoppelde VM netwerk**
---|---|---
New York | LogicalNetwork1-NewYork | VMNetwork1-NewYork
Chicago | LogicalNetwork1 Arnhem | VMNetwork1 Arnhem
 | LogicalNetwork2Chicago | VMNetwork2 Arnhem

### <a name="target-networks"></a>Doel-netwerken

Op basis van deze instellingen, wanneer u het doel VM netwerk selecteren, bevat in de volgende tabel de opties die beschikbaar zijn.

**Selecteer** | **Beveiligde cloud** | **Bescherming van de cloud** | **Netwerk beschikbaar**
---|---|---|---
VMNetwork1 Arnhem | SilverCloud1 | SilverCloud2 | Beschikbaar
 | GoldCloud1 | GoldCloud2 | Beschikbaar
VMNetwork2 Arnhem | SilverCloud1 | SilverCloud2 | Niet beschikbaar
 | GoldCloud1 | GoldCloud2 | Beschikbaar



## <a name="multiple-subnets"></a>Meerdere subnetten

Als het netwerk meerdere subnetten heeft en een van de subnetten heeft dezelfde naam als het subnet waarop de virtuele machine van bron zich bevindt, zal dan de replica virtuele machine worden aangesloten op dat subnet doel na een failover. Als er geen subnet doel met een overeenkomende naam, wordt de virtuele machine worden verbonden met het eerste subnet in het netwerk.


### <a name="failback"></a>Failback

Als u wilt zien wat er gebeurt bij failback (reverse replicatie), gaan we ervan uit dat VMNetwork1 NewYork is toegewezen aan VMNetwork1-Chicago, met de volgende instellingen.


**Virtuele machine** | **Verbonden met een netwerk voor VM**
---|---
VM1 | VMNetwork1 netwerk
VM2 (replica van de VM1) | VMNetwork1 Arnhem

Met deze instellingen, moet u eens kijken wat er gebeurt in een aantal mogelijke scenario's.

**Scenario** | **Resultaat**
---|---
Geen wijziging in het eigenschappenvenster netwerk van VM-2 na een failover. | VM 1 blijft verbonden met het Bronnetwerk.
Netwerkeigenschappen VM 2 na een failover worden gewijzigd en is verbroken. | VM-1 wordt verbroken.
Netwerkeigenschappen VM 2 na een failover worden gewijzigd en is verbonden met VMNetwork2 Chicago. | Als VMNetwork2 Chicago niet is toegewezen, zal de VM 1 worden beëindigd.
Toewijzing van VMNetwork1 Chicago netwerk is gewijzigd. | VM-1 wordt verbonden met het netwerk is nu toegewezen aan VMNetwork1 Chicago.


## <a name="next-steps"></a>Volgende stappen

Nu dat er een beter begrip van het netwerk toewijzen, [aan de slag met de implementatie van de sites worden hersteld](site-recovery-best-practices.md).
