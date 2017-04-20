<properties
    pageTitle="Voorbereiden voor distributie Site herstellen | Microsoft Azure"
    description="Dit artikel wordt beschreven hoe u replicatie met terugwinning van Azure-Site te implementeren."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="prepare-for-azure-site-recovery-deployment"></a>Voorbereiden voor distributie Azure Site herstellen

Lees dit artikel voor een hoog niveau overzicht van de eisen voor elk replicatiescenario is ondersteund door de Azure-Site-Recovery-service. Nadat u de algemene voorschriften voor elk scenario hebt gelezen, kunt u koppelen aan specifieke implementatie details voor elke distributie.

Na het lezen van dit artikel post opmerkingen of vragen aan de onderkant van het artikel, of op het [Forum van Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Overzicht

Organisaties hebben behoefte aan een strategie voor BCDR die bepaalt hoe apps, werkbelasting en gegevens blijven actief en beschikbaar tijdens geplande en ongeplande uitvaltijd en normale gebruiksomstandigheden zo snel mogelijk herstellen. Uw strategie voor BCDR moet u bedrijfsgegevens veilig en terug te vorderen en dat werklasten continu beschikbaar blijven als ramp plaatsvindt. 

Herstel van de site is een Azure service die deel uitmaakt van uw strategie voor BCDR door replicatie van fysieke servers op gebouwen en virtuele machines naar de cloud (Azure) of een secundaire datacenter slaagde. Wanneer er storingen optreden in de primaire locatie, failover u uitvoeren naar de secundaire locatie apps en werkbelasting beschikbaar te houden. U niet terug naar de primaire locatie als resultaat wordt gegeven voor normale bewerkingen. Meer informatie in [Wat is Systeemherstel Site?](site-recovery-overview.md)

## <a name="select-your-deployment-model"></a>Selecteer het implementatiemodel van uw

Azure heeft twee verschillende [implementatiemodellen](../resource-manager-deployment-model.md) voor het maken en werken met resources: de bronnenbeheerder Azure-model en het model Klassiek services management. Azure heeft ook twee portals – [Azure klassieke portal](https://manage.windowsazure.com/) die het implementatiemodel klassiek ondersteunt en de [Azure portal](https://ms.portal.azure.com/) met ondersteuning voor beide implementatiemodellen.

Herstel van de site is beschikbaar in zowel de klassieke portal en de Azure portal. In de klassieke Azure portal kunt u sites worden hersteld met het model Klassiek services management ondersteunen. De Azure portal kunt u het klassieke model of Resource Manager-implementaties ondersteunen. [Lees meer](site-recovery-overview.md#site-recovery-in-the-azure-portal) over de implementatie van de portal Azure.

Wanneer u besluit een notitie deployment-model dat:

- Het beste gebruiken de [Azure portal](https://portal.azure.com) en de Resource Manager model waar mogelijk.
- Herstel van de site biedt een eenvoudiger en intuïtiever introductie ervaring in Azure portal.
- De Azure portal kunt u computers repliceren op klassieke en opslag in Azure Resource Manager. Bovendien in Azure portal kunt u LRS of GRS opslag rekeningen.
- De portal Azure combineert de diensten van de Site-herstel- en back-up in een kluis met één Recovery Services zodat u kunt instellen en BCDR services vanaf één locatie beheren.
- Gebruikers met Azure abonnementen met Cloud Solution Provider (CSP) wordt ingericht kunnen nu terugvorderingen Site in Azure portal beheren.
- VMs VMware of fysieke machines repliceren naar Azure in Azure portal biedt een aantal nieuwe functies zoals ondersteuning voor opslag van de premie en de mogelijkheid om bepaalde schijven uitsluiten van replicatie.


## <a name="select-your-deployment-scenario"></a>Selecteer het implementatiescenario

**Implementatie** | **Details** | **Azure portal** | **Klassieke portal** | **PowerShell**
---|---|---|---|---
**VMs VMware naar Azure** | VMs VMware repliceren naar Azure opslag | In de Azure kan portal VMs failover naar klassieke of Resource Manager opslag<br/><br/> Implementatie in [Azure portal](site-recovery-vmware-to-azure.md) biedt een gestroomlijnde distributie-ervaring en een aantal functievoordelen. | U kunt in de klassieke Azure portal implementeren met het [Verbeterde model](site-recovery-vmware-to-azure-classic.md) en een failover uitvoeren naar de klassieke Azure opslag.<br/><br/> Er is een oudere model dat niet moet worden gebruikt voor nieuwe implementaties. |  PowerShell niet wordt momenteel ondersteund.
**Hyper-V VMs naar Azure** | Hyper-V VMs naar Azure opslag. VMs kunnen op Hyper-V hosts in System Center VMM wolken of zonder VMM beheerd worden. | VMs portal kan in de Azure failover naar klassieke of Resource Manager opslag.<br/><br/> De Azure portal biedt een gestroomlijnde implementatie-ervaring. [Meer informatie](site-recovery-vmm-to-azure.md) over het repliceren van VMs in wolken VMM Hyper-V. [Meer informatie](site-recovery-hyper-v-site-to-azure.md) over het repliceren van Hyper-V VMs (zonder VMM).| In de klassieke Azure portal kunt u failover VMs naar klassieke Azure opslag | PowerShell implementatie wordt ondersteund.
**Fysieke servers Azure** | Fysieke Windows/Linux-servers repliceren naar Azure opslag | Portal-servers kunnen in de Azure failover naar klassieke of Resource Manager opslag.<br/><br/> Implementatie in [Azure portal](site-recovery-vmware-to-azure.md) biedt een gestroomlijnde distributie ervaring en een aantal functievoordelen. | U kunt in de klassieke Azure portal implementeren met het [Verbeterde model](site-recovery-vmware-to-azure-classic.md) en een failover uitvoeren naar de klassieke Azure opslag.<br/><br/> Er is een oudere model dat niet moet worden gebruikt voor nieuwe implementaties. | PowerShell implementatie niet wordt momenteel ondersteund.
**VMs VMware/fysieke servers naar secundaire site* | VMs VMware of fysieke Windows/Linux-servers repliceren naar een secundaire site. [Meer informatie en download](http://download.microsoft.com/download/E/0/8/E08B3BCE-3631-4CED-8E65-E3E7D252D06D/InMage_Scout_Standard_User_Guide_8.0.1.pdf) de handleiding InMage Scout. | Niet beschikbaar in de portal voor Azure | In de klassieke portal downloadt u InMage Scout uit de kluis van een herstel van de Site. | PowerShell implementatie wordt niet op dit moment ondersteund
**VMs Hyper-V op een secundaire site** | Hyper-V VMs in wolken VMM repliceren naar een secundaire wolk | In de [Azure portal](site-recovery-vmm-to-vmm.md) kunt u Hyper-V VMs in wolken VMM aan een secundaire site met Hyper-V-Replica alleen repliceren. SAN wordt niet op dit moment ondersteund. | In de klassieke Azure portal kunt u Hyper-V VMs in wolken VMM repliceren naar een secundaire site met [Hyper-V-Replica](site-recovery-vmm-to-vmm-classic.md) of [replicatie van SAN](site-recovery-vmm-san.md) | PowerShell implementatie wordt ondersteund.



## <a name="check-what-you-need-for-deployment"></a>Controleer wat u nodig hebt voor de implementatie

### <a name="replicate-to-azure"></a>Repliceren naar Azure

**Vereiste** | **Details** 
---|---
**Azure-account** | U moet een account van [Microsoft Azure](http://azure.microsoft.com/) .<br/><br/> U kunt beginnen met een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/). [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over het herstel van de Site-prijzen.
**Azure opslag** | Gerepliceerde gegevens worden opgeslagen in Azure opslag en Azure VMs worden gemaakt wanneer failover plaatsvindt. Als u wilt repliceren naar Azure, moet u een [account Azure opslag](../storage/storage-introduction.md).<br/><br/>Als u in de klassieke portal-Site herstellen moet u één of meer [GRS opslag standaardaccounts](..storage/storage-redundancy.md#geo-redundant-storage).<br/><br/> Als u in de Azure portal kunt u GRS of LRS opslag.<br/><br/> Als u VMs VMware of fysieke servers in de Azure portal premium-opslag repliceert bent wordt ondersteund. Houd er rekening mee dat als u een account premie opslag ook u een account standaardopslag moet voor het opslaan van Logboeken van bestandsreplicatie die voortdurende wijzigingen in de gegevens voor de lokalen vastleggen. [Premium-opslagruimte](../storage/storage-premium-storage.md) wordt meestal gebruikt voor virtuele machines die een consistent hoge i/o-prestaties en lage latentie voor intensieve i/o-belasting host nodig.<br/><br/> Als u een premium account gebruik gerepliceerde gegevens wilt opslaan wilt, moet u ook een account standaardopslag voor het opslaan van Logboeken van bestandsreplicatie die voortdurende wijzigingen in de gegevens voor de lokalen vastleggen.
**Azure netwerk** | Als u wilt repliceren naar Azure, moet u een Azure-netwerk Azure VMs wordt verbonden wanneer ze zijn gemaakt na een failover.<br/><br/> Als u in de klassieke portal gebruikt u een klassieke netwerk. Als u in de portal Azure, kunt u een klassieke of Resource Manager-netwerk.<br/><br/> Het netwerk moet zich in hetzelfde gebied, als de kluis.
**Netwerk toewijzen (VMM naar Azure)** | Als u van VMM naar Azure repliceert bent, [netwerk toewijzing](site-recovery-network-mapping.md) zorgt u ervoor dat Azure VMs met de juiste netwerken na een failover verbonden bent.<br/><br/> Om de toewijzing van het netwerk in te stellen moet u VM netwerken configureert in de VMM-portal.
**In gebouwen** | **VMs VMware**: moet u een computer op ruimten met Recovery Site onderdelen, hosts/vCenter server van VMware vSphere en VMs die u wilt repliceren. [Lees meer](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).<br/><br/> **Fysieke servers**: als u bij het repliceren van fysieke servers moet u een op ruimten machines met onderdelen voor herstel van de Site en fysieke servers die u wilt repliceren. [Lees meer](site-recovery-vmware-to-azure.md#configuration-server-prerequisites). Als u [failback](site-recovery-failback-azure-to-vmware.md) na een failover naar Azure wilt moet u een VMware infrastructure dat moet doen.<br/><br/> **Hyper-V VMs**: als u wilt repliceren VMs in wolken VMM Hyper-V moet u een server VMM en Hyper-V hosts op welke VMs wilt beveiligen bevinden. [Lees meer](site-recovery-vmm-to-azure.md#on-premises-prerequisites).<br/><br/> Als u wilt repliceren VMs Hyper-V zonder VMM moet u Hyper-V hosts VMs zich bevinden. [Lees meer](site-recovery-hyper-v-site-to-azure.md#on-premises-prerequisites).
**Beveiligde computers** | Beveiligde computers die worden gerepliceerd naar Azure voldoen [Azure-vereisten](#azure-virtual-machine-requirements) die hieronder worden beschreven.


### <a name="replicate-to-a-secondary-site"></a>Repliceren naar een secundaire site

**Vereiste** | **Details** 
---|---
**Azure-account** | U moet een account van [Microsoft Azure](http://azure.microsoft.com/) .<br/><br/> U kunt beginnen met een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/). [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over het herstel van de Site-prijzen.
**In gebouwen** | **VMs VMware**: In de primaire site moet u een werkende server voor caching, comprimeren en coderen van replicatiegegevens voordat dit wordt verzonden naar de secundaire site. In de secundaire site installeert u een configuratieserver te beheren en controleren van de implementatie en een master-doelserver. Wij raden ook aan een server vContinuum voor eenvoudiger beheer. Bovendien moet u een of meer VMware vSphere hosts en eventueel een vCenter server. [Lees meer](site-recovery-vmware-to-vmware.md)<br/><br/> **Hyper-V VMs in wolken VMM**: moet u VMM-servers instellen en Hyper-V hosts VMs die u wilt repliceren. [Lees meer](site-recovery-vmm-to-vmm.md#on-premises-prerequisites).
**Netwerk toewijzen (VMM aan VMM)** | Als u van VMM naar VMM repliceert bent, [toewijzing netwerk](site-recovery-network-mapping.md) zorgt ervoor dat VMs replica is verbonden met juiste netwerken na een failover optimaal op Hyper-V hosts worden geplaatst. Om de toewijzing van het netwerk in te stellen moet u VM netwerken configureren op de servers van VMM.
**Toewijzing van opslagruimte** | Als u die van VMM VMM repliceren bent kunt u desgewenst [opslag](site-recovery-storage-mapping.md) toewijzing instellen om op te geven van het doel van de opslag voor gerepliceerde VMs. Opslag-toewijzing kan worden ingesteld voor Hyper-V Replica- en SAN-replicatie.<br/><br/> Toewijzing van de opslagruimte wordt niet die momenteel in Azure portal ondersteund.


## <a name="verify-url-access"></a>Controleer of de URL-toegang

Zorg ervoor dat deze URL's van servers toegankelijk zijn.

**URL** | **VMM aan VMM** | **VMM naar Azure** | **Hyper-V van Azure** | **VMware Azure**
---|---|---|---|---
*. accesscontrol.windows.net | Toestaan | Toestaan | Toestaan | Toestaan
*. backup.windowsazure.com | Niet vereist | Toestaan | Toestaan | Toestaan
*. hypervrecoverymanager.windowsazure.com | Toestaan | Toestaan | Toestaan | Toestaan
*. store.core.windows.net | Toestaan | Toestaan | Toestaan | Toestaan
*. blob.core.windows.net | Niet vereist | Toestaan | Toestaan | Toestaan
https://www.msftncsi.com/ncsi.txt | Toestaan | Toestaan | Toestaan | Toestaan
https://dev.mysql.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi | Niet vereist | Niet vereist | Niet vereist | Toestaan

## <a name="azure-virtual-machine-requirements"></a>Vereisten voor Azure VM

U kunt Systeemherstel om virtuele machines en de fysieke servers met een besturingssysteem wordt ondersteund door Azure Site implementeren. Dit geldt ook voor de meeste versies van Windows en Linux. U moet om ervoor te zorgen dat virtuele machines die u wilt beveiligen met Azure eisen voldoen op gebouwen.


**Functie** | **Vereisten** | **Details**
---|---|---
Hyper-V host | Windows Server 2012 R2 moet worden uitgevoerd | Vereisten controle mislukt als besturingssysteem niet ondersteund
VMware hypervisor  | Ondersteund besturingssysteem | [Vereisten controleren](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment)
Gastbesturingssysteem | Hyper-V Azure replicatie: herstel Site biedt ondersteuning voor alle besturingssystemen die [ondersteund door Azure worden](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx). <br/><br/> Voor VMware en replicatie van de fysieke server: Controleer de Windows- en Linux- [vereisten](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) | Vereisten controle mislukt als niet ondersteund.
Architectuur van Gast-besturingssysteem | 64-bits | Vereisten controle mislukt als er niet-ondersteunde
Grootte van de schijf besturingssysteem |  1023 GB | Vereisten controle mislukt als er niet-ondersteunde
Aantal operating system-schijven | 1 | Vereisten controle mislukt als niet ondersteund.
Aantal van de schijf gegevens | 16 of minder (maximale waarde is afhankelijk van de grootte van de virtuele machine wordt gemaakt. 16 = XL) | Vereisten controle mislukt als er niet-ondersteunde
De grootte van de VHD schijf gegevens | Maximaal 1023 GB | Vereisten controle mislukt als er niet-ondersteunde
Netwerkadapters | Meerdere adapters worden ondersteund. |
Statisch IP-adres | Ondersteund | Als de primaire virtuele machine maakt gebruik van een statisch IP-adres die kunt u het statische IP-adres voor de virtuele machine die wordt gemaakt in Azure. Opmerking: het statische IP-adres voor een linux virtuele machine waarop Hyper-v wordt niet ondersteund.
iSCSI-schijf | Niet ondersteund | Vereisten controle mislukt als er niet-ondersteunde
Gedeelde VHD | Niet ondersteund | Vereisten controle mislukt als er niet-ondersteunde
FC-schijf | Niet ondersteund | Vereisten controle mislukt als er niet-ondersteunde
Indeling van de vaste schijf| VHD <br/><br/> VHDX | Hoewel VHDX momenteel niet wordt ondersteund in Azure, omgezet herstel van de Site automatisch in VHDX VHD wanneer u failover uitvoeren naar Azure. Wanneer u niet terug in de lokalen blijven de virtuele machines gebruikt u de notatie VHDX.
BitLocker | Niet ondersteund | BitLocker moet worden uitgeschakeld voordat u een virtuele machine te beschermen.
Naam van de virtuele machine| Van 1 tot 63 tekens. Alleen letters, cijfers en streepjes. Moet beginnen en eindigen met een letter of cijfer | De waarde in de eigenschappen van de virtuele machine in Recovery Site bijwerken
Virtuele machinetype | <p>Generation 1</p> <p>Generatie 2 - Windows</p> |  Generatie 2 virtuele machine met OS schijftype standaardschijf met 1 of 2 gegevensvolumes met schijf formatteren als VHDX die lager is dan 300GB wordt ondersteund. Linux Generation 2 virtuele machines worden niet ondersteund. [Meer informatie](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)



## <a name="optimizing-your-deployment"></a>Optimaliseren van de implementatie

Gebruik de volgende tips helpen u optimaliseren en schalen van uw implementatie.

- **Volumegrootte besturingssysteem**: wanneer u een virtuele machine naar Azure repliceren volume met het besturingssysteem moet minder dan 1 TB. Als u volumes van meer dan kunt u handmatig verplaatsen naar een andere schijf voordat u de implementatie.
- **Gegevens schijf grootte**: als u naar Azure repliceren wilt hebt u maximaal 32 gegevensschijven op een virtuele machine, elk met een maximum van 1 TB. U kunt effectief repliceren en failover ~ 32 TB virtuele machines.
- **Recovery plan grenzen**: herstel van de Site kunnen worden aangepast aan de duizenden van virtuele machines. De herstelplannen zijn ontworpen als een model voor toepassingen die bij elkaar moeten failover zodat we het aantal machines in een herstelplan op 50 beperken.
- **Azure service grenzen**: elke Azure abonnement wordt geleverd met een set standaardlimieten op cores, cloud diensten, enz. Wij raden aan een failover testen om te controleren van de beschikbaarheid van resources in uw abonnement. U kunt deze limieten via Azure ondersteuning wijzigen.
- **Capaciteitsplanning**: meer informatie over [Capaciteitsplanning](site-recovery-capacity-planner.md) voor herstel van de Site.
- **Bandbreedte voor replicatie**: merk op dat als u weinig bandbreedte voor replicatie:
    - **ExpressRoute**: herstel van de Site werkt met zowel Azure ExpressRoute- en WAN-zoals Riverbed. [Lees meer](http://blogs.technet.com/b/virtualization/archive/2014/07/20/expressroute-and-azure-site-recovery.aspx) over ExpressRoute.
    - **Replicatieverkeer**: Recovery Site gebruikt een intelligente initiële replicatie alleen gegevensblokken en niet de hele VHD wordt uitgevoerd. Alleen de wijzigingen worden gerepliceerd tijdens een voortdurende replicatie.
    - **Netwerkverkeer**: kunt u netwerkverkeer door [QoS in Windows](https://technet.microsoft.com/library/hh967468.aspx) instellen met een beleid op basis van het IP-adres en de poort gebruikt voor replicatie beheren.  Bovendien, als u wilt repliceren naar Azure Site herstellen met behulp van de back-up Azure agent kunt u bandbreedteregeling voor die medewerker. [Lees meer](https://support.microsoft.com/kb/3056159).
- **RTO**: voor het meten van de recovery time doelstelling (RTO) u met terugwinning van de Site verwachten kunt stellen we voor u een test failover uitvoeren en weergave de Recovery Site projecten analyseren hoeveel tijd het kost om de bewerkingen te voltooien. Als u bent een failover uitvoeren naar Azure, voor de beste RTO raden we aan dat u alle handmatige acties automatiseren, door de integratie met Azure plannen voor automatisering en herstel.
- **Vrijgegeven Productieorder**: herstel Site ondersteunt een in de buurt van de synchrone herstel punt doelstelling (vrijgegeven Productieorder) wanneer u een replica op Azure. Er wordt verondersteld voldoende bandbreedte tussen uw datacenter en Azure.


##<a name="service-urls"></a>URL 's
Zorg ervoor dat deze URL's zijn toegankelijk vanaf de server


**URL 's** | **VMM aan VMM** | **VMM naar Azure** | **Azure-Site Hyper-V** | **VMware Azure**
---|---|---|---|---
 \*. accesscontrol.windows.net | Access vereist  | Access vereist  | Access vereist  | Access vereist
 \*. backup.windowsazure.com |  | Access vereist  | Access vereist  | Access vereist
 \*. hypervrecoverymanager.windowsazure.com | Access vereist  | Access vereist  | Access vereist  | Access vereist
 \*. store.core.windows.net | Access vereist  | Access vereist  | Access vereist  | Access vereist
 \*. blob.core.windows.net |  | Access vereist  | Access vereist  | Access vereist
 https://www.msftncsi.com/ncsi.txt | Access vereist  | Access vereist  | Access vereist  | Access vereist
 https://dev.mysql.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi | | | | Access vereist


## <a name="next-steps"></a>Volgende stappen

Na leren en algemene implementatievereisten vergelijken kunt u lezen van de gedetailleerde vereisten en start elk scenario te implementeren.

- [VMware virtuele machines te repliceren naar Azure](site-recovery-vmware-to-azure-classic.md)
- [Fysieke servers repliceren naar Azure](site-recovery-vmware-to-azure-classic.md)
- [Hyper-V server in wolken VMM repliceren naar Azure](site-recovery-vmm-to-azure.md)
- [Hyper-V virtuele machines (zonder VMM) repliceren naar Azure](site-recovery-hyper-v-site-to-azure.md)
- [Hyper-V VMs repliceren naar een secundaire site](site-recovery-vmm-to-vmm.md)
- [Hyper-V VMs repliceren naar een secundaire site met SAN](site-recovery-vmm-san.md)
- [Hyper-V VMs repliceren met één server VMM](site-recovery-single-vmm.md)
