<properties
    pageTitle="Azure-Site-Recovery-ondersteuningsmatrix | Microsoft Azure"
    description="Geeft een overzicht van de ondersteunde besturingssystemen en onderdelen voor Azure Site herstellen"
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

# <a name="azure-site-recovery-support-matrix"></a>Azure-Site-Recovery-ondersteuningsmatrix

Dit artikel bevat een overzicht van ondersteunde besturingssystemen en onderdelen voor Azure-Site-Recovery-implementaties. Een lijst met ondersteunde onderdelen en vereisten beschikbaar is voor elk implementatiescenario in elk het desbetreffende artikel in de implementatie en dit document bevat een overzicht van deze.

## <a name="supported-operating-systems-for-virtualization-servers"></a>Ondersteunde besturingssystemen voor servers voor virtualisatie


**Bron** | **Doel** | **Host OS**
---|---|---|--- 
**Hyper-V hosts (zonder VMM)** | Azure | Windows Server 2012 R2 met de meest recente updates
**Hyper-V hosts (met VMM)** | Azure | Windows Server 2012 R2 met de meest recente updates
**Hyper-V hosts (met VMM)** | Secundaire VMM-site | Ten minste Windows-Server 2012 met de meest recente updates
**VMware hosts/vCenter** | Azure | vCenter 5.5 of 6.0 (ondersteuning voor 5.5 alleen functies) <br/><br/> vSphere 6.0, 5.5 of 5.1 met de meest recente updates
**VMware hosts/vCenter** | Secundaire site van VMware | vCenter 5.5 of 6.0 (ondersteuning voor 5.5 alleen functies) <br/><br/> vSphere 6.0, 5.5 of 5.1 met de meest recente updates

## <a name="supported-requirements-for-replicated-machines"></a>Ondersteunde vereisten voor gerepliceerde machines

**Bron** | **Wat gerepliceerd.** | **Doel** | **Host OS**
---|---|---|--- 
**Hyper-V VMs** | De werkbelasting | Azure | Elke gast OS [ondersteund door Azure](https://technet.microsoft.com/library/cc794868.aspx)<br/><br/> VMs moeten voldoen aan de [vereisten voor Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**Hyper-V VMs (met VMM)** | De werkbelasting | Azure | Elke gast OS [ondersteund door Azure](https://technet.microsoft.com/library/cc794868.aspx)<br/><br/> VMs moeten voldoen aan de [vereisten voor Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**Hyper-V VMs (met VMM)** | De werkbelasting | Secundaire VMM-site | Elke gast OS [ondersteund door Hyper-V](https://technet.microsoft.com/library/mt126277.aspx)
**VMs VMware** | De werkbelasting op Windows VM | Azure | 64-bits Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 met op minimaal SP1<br/><br/> VMs moeten voldoen aan de [vereisten voor Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**VMs VMware** | De werkbelasting op Linux VM | Azure | Red Hat Enterprise Linux 6.7, 7.1, 7.2 <br/><br/> Centos 6.5 6.6, 6.7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5 waarop Red Hat compatibel kernel of Unbreakable Enterprise versie 3 voor de Kernel (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Vereiste opslagruimte: File system (EXT3, ETX4, ReiserFS, XFS); Multipath-software-apparaat Mapper (MPIO)); Volume manager: (LVM2). Fysieke servers met HP CCISS-controller opslag worden niet ondersteund. Het bestandssysteem ReiserFS wordt alleen ondersteund op SUSE Linux Enterprise Server 11 SP3.<br/><br/> VMs moeten voldoen aan de [vereisten voor Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**VMs VMware** | De werkbelasting op Windows VM | Secundaire site van VMware | 64-bits Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 met op minimaal SP1
**VMs VMware** | De werkbelasting op Linux VM | Secundaire site van VMware | Red Hat Enterprise Linux 6.7, 7.1, 7.2 <br/><br/> Centos 6.5 6.6, 6.7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5 waarop Red Hat compatibel kernel of Unbreakable Enterprise versie 3 voor de Kernel (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Vereiste opslagruimte: File system (EXT3, ETX4, ReiserFS, XFS); Multipath-software-apparaat Mapper (MPIO)); Volume manager: (LVM2). Fysieke servers met HP CCISS-controller opslag worden niet ondersteund. Het bestandssysteem ReiserFS wordt alleen ondersteund op SUSE Linux Enterprise Server 11 SP3.
**Fysieke servers** | De werkbelasting met Windows | Azure | 64-bits Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 met op minimaal SP1
**Fysieke servers** | De werkbelasting op Linux | Azure | Red Hat Enterprise Linux 6.7,7.1,7.2 <br/><br/> 6.5 centos, 6.6,6.7,7.0,7.1,7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5 waarop Red Hat compatibel kernel of Unbreakable Enterprise versie 3 voor de Kernel (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Vereiste opslagruimte: File system (EXT3, ETX4, ReiserFS, XFS); Multipath-software-apparaat Mapper (MPIO)); Volume manager: (LVM2). Fysieke servers met HP CCISS-controller opslag worden niet ondersteund. Het bestandssysteem ReiserFS wordt alleen ondersteund op SUSE Linux Enterprise Server 11 SP3.
**Fysieke servers** | De werkbelasting met Windows | Secundaire site | 64-bits Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 met op minimaal SP1
**Fysieke servers** | De werkbelasting op Linux | Secundaire site | Red Hat Enterprise Linux 6.7,7.1,7.2 <br/><br/> 6.5 centos, 6.6,6.7,7.0,7.1,7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5 waarop Red Hat compatibel kernel of Unbreakable Enterprise versie 3 voor de Kernel (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Vereiste opslagruimte: File system (EXT3, ETX4, ReiserFS, XFS); Multipath-software-apparaat Mapper (MPIO)); Volume manager: (LVM2). Fysieke servers met HP CCISS-controller opslag worden niet ondersteund. Het bestandssysteem ReiserFS wordt alleen ondersteund op SUSE Linux Enterprise Server 11 SP3.


## <a name="provider-versions"></a>Versies van de provider

**Naam** | **Beschrijving** | **Nieuwste versie** | **Ondersteuning** | **Details**
---|---|---|---| ---
**Azure Site Recovery-Provider** | Communicatie tussen servers op gebouwen en Azure en secundaire site coördinaten <br/><br/> Als er geen server VMM op op ruimten VMM-servers of servers Hyper-V is geïnstalleerd | 5.1.1700 (beschikbaar via de portal) | [Meest recente functies en correcties](https://support.microsoft.com/kb/3155002)
**Azure-Site-Recovery Unified Setup (VMware Azure)** | Communicatie tussen VMware-servers op gebouwen en Azure-coördinaten <br/><br/> Geïnstalleerd op de VMware-servers voor gebouwen | 9.3.4246.1 (beschikbaar via de portal) | [Meest recente functies en correcties](https://support.microsoft.com/kb/3155002)
**Mobiliteit service** | Replicatie tussen op ruimten VMware-servers/fysieke servers en Azure en secundaire site coördinaten | NA (beschikbaar via de portal) | Geïnstalleerd op elke VM VMware of fysieke server die u wilt repliceren. **De agent Microsoft Azure Recovery Services (MARS)** | Coördinaten replicatie tussen VMs Hyper-V en Azure<br/><br/> Geïnstalleerd op voor bedrijfsruimten Hyper-V-servers (met of zonder een VMM-server) | 2.0.8689.0 (beschikbaar via de portal) | Deze agent wordt gebruikt door Azure Site herstel- en back-up Azure). [Meer informatie] (https://support.microsoft.com/en-us/kb/2997692)

## <a name="next-steps"></a>Volgende stappen

[Voorbereiden voor distributie](site-recovery-best-practices.md)

