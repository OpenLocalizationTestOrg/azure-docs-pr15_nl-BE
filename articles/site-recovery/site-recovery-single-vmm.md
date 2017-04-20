
<properties
    pageTitle="Azure Site herstellen: Repliceren Hyper-V virtuele machines op één server VMM | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe Hyper-V virtuele machines te repliceren als er slechts één server VMM."
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
    ms.workload="backup-recovery"
    ms.date="08/24/2016"
    ms.author="raynew"/>

#  <a name="replicate-hyper-v-virtual-machines-on-a-single-vmm-server"></a>Hyper-V virtuele machines op één server VMM repliceren

Lees dit artikel voor meer informatie over het repliceren van Hyper-V virtuele machines die zich bevindt op een server met Hyper-V host in een wolk VMM wanneer u slechts één server VMM in uw implementatie hebt.

Azure heeft twee verschillende [implementatiemodellen](../resource-manager-deployment-model.md) voor het maken en werken met resources: Azure Resource Manager en klassiek. Azure heeft ook twee portals – de Azure klassieke portal die het implementatiemodel klassiek ondersteunt en de Azure portal met ondersteuning voor beide implementatiemodellen. Dit artikel bevat instructies voor het instellen van replicatie in Azure portal.


Hebt u vragen na het lezen van dit artikel, kunt u deze in de Disqus opmerkingen aan de onderkant van dit artikel, of op het [forum Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)boeken.

## <a name="overview"></a>Overzicht

Als u wilt repliceren, Hyper-V VMs op Hyper-V hosts in VMM bevindt en u slechts één server VMM hebt, kunt u deze [repliceren naar Azure](site-recovery-vmm-to-azure.md), of tussen de wolken op één server VMM.

Wij raden aan dat u naar Azure repliceren omdat failover en herstel niet naadloos bij replicatie tussen wolken en een aantal handmatige stappen nodig zijn. Als u repliceren met alleen de VMM-server wilt, kunt u het volgende doen:

- Een VMM één zelfstandige server repliceren
- Repliceren met één VMM server geïmplementeerd in een uitgerekte Windows cluster


## <a name="replicate-across-sites-with-a-single-standalone-vmm-server"></a>Meerdere sites met een afzonderlijke zelfstandige VMM-server repliceren

![Zelfstandige server voor virtuele VMM](./media/site-recovery-single-vmm/single-vmm-standalone.png)

In dit scenario de enkele VMM-server implementeren als een virtuele machine in de primaire site en deze VM repliceren naar een secundaire site sites hersteld en Hyper-V-Replica.

1. VMM op een Hyper-V VM instellen. Wij stellen voor installatie van SQL Server-sessie door VMM op de dezelfde VM gebruikt om tijd te besparen. Als u met een extern exemplaar van SQL Server en een storing optreedt, moet u dat exemplaar herstellen voordat u de VMM kunt herstellen.
2. Zorg dat de VMM-server heeft ten minste twee wolken geconfigureerd. Een wolk bevat de VMs u wilt repliceren, en de andere wolk fungeert als secundaire locatie. De cloud met het VMs te beschermen moet hebben:

    - Een of meer VMM hostgroepen met een of meer Hyper-V host-servers in elke groep.
    - Ten minste één Hyper-V virtuele machine op elke host Hyper-V server.

3. Een kluis Recovery Services maken, genereren en downloaden van een sleutel van de registratie vault en de VMM-server registreren in de kluis. Tijdens de registratie kunt u de Provider Azure Site herstellen op de VMM-server installeren.
4. Een of meer wolken op de VM VMM instellen en de Hyper-V hosts toevoegen aan deze wolken.
3. Configureer de beveiligingsinstellingen voor de wolken. U kunt de naam van de server met één VMM opgeven als de bron- en doellocaties. Configureren netwerk toewijzen, wijst u de VM-netwerk voor de cloud met het VMs die u wilt beveiligen met de VM netwerk voor de replicatie-wolk.
4. De eerste replicatie voor VMs die u beveiligen via het netwerk wilt, omdat beide wolken bevinden zich op dezelfde server inschakelen.
4. In de console van Hyper-V-beheer inschakelen van Hyper-V-Replica op de Hyper-V host waarop de VM VMM en Schakel replicatie op de VM. Zorg ervoor dat u de VMM VM niet toevoegen aan de wolken die worden beveiligd door het herstel van de Site. Dit zorgt ervoor dat Replica Hyper-V-instellingen niet worden overschreven door de Site herstellen.
5. Als u maken van de herstelplannen wilt, kunt u dezelfde server VMM voor bron en doel opgeven.

Volg de instructies in [dit artikel](site-recovery-vmm-to-vmm.md) voor het maken van een kluis, de server registreren en de beveiliging instellen.

### <a name="what-to-do-in-an-outage"></a>Wat te doen in een stroomstoring

Als een volledige is uitgevallen en u moet van de secundaire site werkt, het volgende doen:

1.  Een niet-geplande failover VMM van de primaire secundaire site VM failover worden uitgevoerd in de console van Hyper-V-beheer in de secundaire site.
2.  Controleer of de VMM VM actief in de secundaire site.
3.  In de kluis Recovery Services, een niet-geplande failover de werkdruk VMs van de primaire secundaire wolken failover worden uitgevoerd. Om niet-geplande failover van de VMs failover doorvoeren of een ander herstelpunt selecteren zoals vereist.
4.  Nadat de niet-geplande failover uitgevoerd is, kunnen gebruikers de werkbelasting van resources in de secundaire site openen.

Wanneer de primaire site normaal opnieuw functioneert, als volgt:

1.  In de console van Hyper-V-beheer inschakelen reverse replicatie voor de VM VMM om te starten met het repliceren van primaire secundaire.
2.  In de console van Hyper-V-beheer om de VMM VM aan de primaire site failback geplande failover worden uitgevoerd. Verbindt zich ertoe de failover-functie om te voltooien. Schakel omgekeerde replicatie starten de VMM van primaire repliceren naar de secundaire.
3.  In de kluis Recovery Services inschakelen reverse replicatie voor de werkbelasting VMs gerepliceerd ze ondergeschikt aan de primaire starten.
4.  In de kluis Recovery Services om de werklast van VMs tot de primaire site failback geplande failover worden uitgevoerd. Verbindt zich ertoe de failover-functie om te voltooien. Schakel omgekeerde replicatie moet beginnen met de werkbelasting VMs van primaire repliceren naar de secundaire.



## <a name="replicate-across-sites-with-a-single-vmm-server-in-a-stretched-cluster"></a>Meerdere sites met één server in een cluster uitgerekte VMM repliceren

![Geclusterde virtuele VMM-server](./media/site-recovery-single-vmm/single-vmm-cluster.png)

In plaats van een zelfstandige VMM server implementeert als een VM die worden gerepliceerd naar een secundaire site, kunt u VMM uiterst beschikbaar maken door het implementeren van deze als een VM in een failover-cluster van Windows. Dit biedt de veerkracht van de werkbelasting en bescherming tegen hardwarestoringen. Als u wilt implementeren met terugwinning van de Site moet de VMM VM worden geïmplementeerd in een cluster uitrekken op geografisch afzonderlijke locaties. Om dit te doen:

1. VMM installeren op een virtuele machine in een failover-cluster van Windows en selecteer de optie voor het uitvoeren van de server als uiterst beschikbaar tijdens de installatie.
2. De SQL Server-exemplaar dat wordt gebruikt door de VMM moet worden gerepliceerd met SQL Server AlwaysOn beschikbaarheidsgroepen, zodat er een replica van de database in de secundaire site.
3. Volg de instructies in [dit artikel](site-recovery-vmm-to-vmm.md) voor het maken van een kluis, de server registreren en de beveiliging instellen. U moet elke VMM-server in het cluster in de kluis Recovery Services registreren. Hiervoor kunt u de voorziening hebt geïnstalleerd op een actief knooppunt en de VMM-server registreren. Vervolgens installeert u de Provider op andere knooppunten.

### <a name="what-to-do-in-an-outage"></a>Wat te doen in een stroomstoring

Als er een storing optreedt, worden de VMM-server en de corresponderende SQL Server-database heeft overgenomen en van de secundaire site geopend.


## <a name="next-steps"></a>Volgende stappen

[Meer informatie](site-recovery-vmm-to-vmm.md) over gedetailleerde Recovery Site-implementatie voor VMM VMM replicatie.
