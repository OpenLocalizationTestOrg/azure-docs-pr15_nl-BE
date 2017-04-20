<properties
    pageTitle="Azure Site Systeemherstel: Veelgestelde vragen | Microsoft Azure"
    description="In dit artikel worden populaire vragen over het herstel van Azure Site beschreven."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="get-started-article"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/10/2016"
    ms.author="raynew"/>


# <a name="azure-site-recovery-frequently-asked-questions-faq"></a>Azure Site Systeemherstel: Veelgestelde vragen (FAQ)

Dit artikel bevat veelgestelde vragen over het herstel van Azure-Site. Hebt u vragen na het lezen van dit artikel, kunt u deze op het [Forum van Azure Recovery Services](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr)boeken.


## <a name="general"></a>Algemeen

### <a name="what-does-site-recovery-do"></a>Wat is Site herstellen?

Herstel site bijdraagt aan uw continuïteit en disaster recovery (BCDR) bedrijfsstrategie, regie en automatiseren van replicatie vanaf op ruimten virtuele machines en fysieke servers Azure of een secundaire datacenter. [Meer informatie](site-recovery-overview.md).


### <a name="what-can-site-recovery-protect"></a>Wat kan herstel Site beschermen?

- **Hyper-V virtuele machines**: herstel van de Site een werkbelasting op een Hyper-V VM kunt beveiligen.
- **Fysieke servers**: herstel van de Site kunt beschermen fysieke Windows-of Linux-servers.
- **VMware virtuele machines**: herstel van de Site een werkbelasting uitgevoerd in een VM VMware kunt beveiligen.

### <a name="does-site-recovery-support-the-azure-resource-manager-model"></a>Ondersteunt herstel van de Site het model Azure Resource Manager?

Naast het herstel van de Site in de portal voor Azure klassieke is herstel van de Site in Azure portal met ondersteuning voor Resource Manager beschikbaar. Voor de meeste implementatiescenario's Site herstellen in de Azure portal biedt een gestroomlijnde distributie ervaring en kunt u in de klassieke opslag of opslag Resource Manager VMs en fysieke servers repliceren. Hier vindt u de ondersteunde implementaties:

- [VMs VMware of fysieke servers repliceren naar Azure in Azure portal](site-recovery-vmware-to-azure.md)
- [Hyper-V VMs in wolken VMM repliceren naar Azure in Azure portal](site-recovery-vmm-to-azure.md)
- [Hyper-V VMs (zonder VMM) repliceren naar Azure in Azure portal](site-recovery-hyper-v-site-to-azure.md)
- [Hyper-V VMs in wolken VMM repliceren naar een secundaire site in de portal voor Azure](site-recovery-vmm-to-vmm.md)


### <a name="what-do-i-need-in-hyper-v-to-orchestrate-replication-with-site-recovery"></a>Wat moet ik in Hyper-V goedkeuringen van replicatie met terugwinning van de Site?

Voor de Hyper-V host-server afhankelijk moet u van het scenario. Bekijk de vereisten voor Hyper-V in:

- [Hyper-V VMs (zonder VMM) worden gerepliceerd naar Azure](site-recovery-hyper-v-site-to-azure.md#before-you-start)
- [Hyper-V VMs (met VMM) worden gerepliceerd naar Azure](site-recovery-vmm-to-azure.md#before-you-start)
- [Hyper-V VMs repliceren naar een secundaire datacenter](site-recovery-vmm-to-vmm.md#before-you-start)

- Als u naar een secundaire datacenter meer informatie over [ondersteunde gastbesturingssystemen voor VMs Hyper-V repliceren wilt](https://technet.microsoft.com/library/mt126277.aspx).
- Als u naar Azure repliceren wilt, ondersteunt herstel van de Site alle Gast-besturingssystemen die [ondersteund door Azure worden](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).

### <a name="can-i-protect-vms-when-hyper-v-is-running-on-a-client-operating-system"></a>Kan ik VMs beschermen als Hyper-V wordt uitgevoerd op een client-besturingssysteem?

Nee, VMs moeten zich bevinden op een Hyper-V host-server die wordt uitgevoerd op een ondersteund Windows server-computer. Als u een client-computer beveiligen kan u deze repliceert als een fysieke machine [Azure](site-recovery-vmware-to-azure.md) of een [secundaire datacenter](site-recovery-vmware-to-vmware.md).


### <a name="what-workloads-can-i-protect-with-site-recovery"></a>Welke werkbelasting kan ik beveiligen met terugwinning van de Site?

Herstel van de Site kunt u de meeste werklasten uitgevoerd op een ondersteunde VM of fysieke server beveiligen. Herstel van de site biedt ondersteuning voor toepassingsbewuste replicatie, zodat toepassingen kunnen worden hersteld naar de status van een intelligent. Het is geïntegreerd met Microsoft-toepassingen zoals SharePoint, Exchange, Dynamics, SQL Server en Active Directory en werkt nauw samen met toonaangevende leveranciers zoals Oracle, SAP, IBM en Red Hat. [Meer informatie](site-recovery-workload.md) over de bescherming van de werkbelasting.


### <a name="do-hyper-v-hosts-need-to-be-in-vmm-clouds"></a>Hyper-V hosts wilt worden in de wolken VMM?

Als u repliceren naar een secundaire datacenter wilt en vervolgens VMs Hyper-V moet zich op host Hyper-V van servers die zich in een wolk van VMM. Als u repliceren naar Azure wilt, kunt u op Hyper-V host-servers met of zonder wolken VMM VMs repliceren. [Lees meer](site-recovery-hyper-v-site-to-azure.md).

### <a name="can-i-deploy-site-recovery-with-vmm-if-i-only-have-one-vmm-server"></a>Kan ik herstel met VMM Site als ik slechts één VMM server implementeren?

Ja. Kunt u VMs in Hyper-V-servers in de cloud VMM naar Azure repliceren, of u kunt repliceren tussen VMM wolken op dezelfde server. Op-lokalen voor bedrijfsruimten replicatie, wordt u aangeraden dat u een server VMM in de primaire en secundaire sites hebt.  [Lees meer](site-recovery-single-vmm.md)

### <a name="what-physical-servers-can-i-protect"></a>Welke fysieke servers kan ik beveiligen?

U kunt de fysieke servers met Windows en Linux Azure of een secundaire site repliceren. [Meer informatie over](site-recovery-vmware-to-azure.md#protected-machine-prerequisites) vereisten voor besturingssystemen.  Hiervoor gelden dezelfde voorschriften of u fysieke servers, Azure of een secundaire site repliceren bent.

Houd er rekening mee dat fysieke servers als VMs in Azure wordt uitgevoerd als de server op locatie begeeft. Failback naar een fysieke server op locatie momenteel niet ondersteund, maar u kunt niet naar een virtuele machine waarop Hyper-V of VMware.


### <a name="what-vmware-vms-can-i-protect"></a>Welke VMs VMware kan ik beveiligen?

Ter bescherming van VMs VMware moet u een hypervisor vSphere en virtuele machines met VMware's. Ook wordt aangeraden dat u beschikt over een VMware vCenter server voor het beheren van de hypervisors. [Meer informatie](site-recovery-vmware-to-azure.md#protected-machine-prerequisites) over de exacte vereisten voor het repliceren van VMware-servers en VMs Azure of een secundaire site.

### <a name="can-i-manage-disaster-recovery-for-my-branch-offices-with-site-recovery"></a>Kan ik herstel na noodgevallen voor mijn filialen met terugwinning van de Site beheren?

Ja. Als u Recovery Site goedkeuringen van replicatie en -failover in de filialen, krijgt u een uniforme orchestration en weergave van de werklast voor de office tak op een centrale locatie. U kunt gemakkelijk uitvoeren van failover en noodherstel van alle takken van het hoofdkantoor te beheren zonder de takken te bezoeken.

## <a name="security"></a>Beveiliging

### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Replicatie verzonden naar de Site-Recovery-service?

Nee, herstel van de Site geen gerepliceerde gegevens onderscheppen en geen informatie over wat wordt uitgevoerd op uw virtuele machines of fysieke servers.
Replicatiegegevens worden uitgewisseld tussen de Hyper-V-hosts op gebouwen, VMware hypervisors, of fysieke servers en Azure opslag of de secundaire site. Herstel van de site heeft geen mogelijkheid om deze gegevens onderscheppen. Alleen de metagegevens die nodig is voor replicatie en -failover evoluerende wordt verzonden naar de Site-Recovery-service.

Herstel van de site is ISO 27001:2013, 27018, HIPAA DPA gecertificeerd en is bezig met het SOC2 en FedRAMP JAB beoordelingen.


### <a name="for-compliance-reasons-even-our-on-premises-metadata-must-remain-within-the-same-geographic-region-can-site-recovery-help-us"></a>Omwille van de conformiteit moet blijven, zelfs onze metagegevens op ruimten binnen hetzelfde geografische gebied. Herstel van de Site kan helpen ons?

Ja. Bij het maken van een kluis Site herstellen in een regio, wij zorgen ervoor dat alle metagegevens die we moeten inschakelen en evoluerende replicatie en failover blijft binnen die regio's geografische grenzen.

### <a name="does-site-recovery-encrypt-replication"></a>Is Recovery Site replication coderen?

Voor virtuele machines en fysieke servers wordt repliceren tussen voor bedrijfsruimten sites codering-in-transit ondersteund. Voor virtuele machines en fysieke servers worden gerepliceerd naar Azure worden zowel codering in transit en codering in rust (in Azure) ondersteund.


## <a name="replication"></a>Replicatie


### <a name="are-there-any-prerequisites-for-replicating-virtual-machines-to-azure"></a>Zijn er vereisten voor het repliceren van virtuele machines naar Azure?

Virtuele machines die u wilt repliceren naar Azure moeten aan [Azure eisen](site-recovery-best-practices.md#virtual-machines)voldoen.

### <a name="can-i-replicate-hyper-v-generation-2-virtual-machines-to-azure"></a>Kan ik Hyper-V-generatie 2 virtuele machines worden gerepliceerd naar Azure?

Ja. Herstel site omgezet van generation 2 generation 1 tijdens failover. De machine wordt op failback geconverteerd naar generation 2. [Lees meer](http://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).

### <a name="if-i-replicate-to-azure-how-do-i-pay-for-azure-vms"></a>Als ik naar Azure repliceren hoe ik betalen voor Azure VMs?

Tijdens de normale replicatie gegevens worden gerepliceerd naar de opslag voor geo-redundante Azure en u hoeft niet te betalen van de kosten van de virtuele machine Azure IaaS biedt een aanzienlijk voordeel. Site herstel Azure IaaS virtuele machines worden automatisch gemaakt wanneer u een failover naar Azure uitvoeren, en na die u zult worden gefactureerd voor de compute-bronnen die u in Azure verbruikt.


### <a name="is-there-an-sdk-i-can-use-to-automate-the-asr-workflow"></a>Is er een SDK die ik gebruiken kan voor het automatiseren van de workflow automatisch Systeemherstel?

Ja. U kunt Site herstel werkstromen met de Rest API PowerShell of de SDK Azure automatiseren. Ondersteunde scenario's voor het implementeren van sites met PowerShell worden hersteld:

- [Hyper-V VMs in VMMs wolken repliceren naar Azure PowerShell klassiek](site-recovery-deploy-with-powershell.md)
- [Hyper-V VMs in VMMs wolken repliceren naar Azure PowerShell Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [VMs Hyper-V zonder VMM repliceren naar Azure PowerShell klassiek](site-recovery-hyper-v-site-to-azure-classic.md)
- [VMs Hyper-V zonder VMM repliceren naar Azure PowerShell Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)


### <a name="if-i-replicate-to-azure-what-kind-of-storage-account-do-i-need"></a>Als ik naar Azure repliceren met wat voor soort opslag account moet ik gebruiken?

- **Azure klassieke portal**: als u Site herstellen in de klassieke Azure portal, moet u een [account standaard geo-redundante opslag](../storage/storage-redundancy.md#geo-redundant-storage). Premium-opslagruimte wordt niet momenteel ondersteund. De account moet in hetzelfde gebied, als de Site herstel kluis.

- **Azure portal**: als u Recovery-Site in de portal Azure, moet u een LRS of GRS opslag account. Het is raadzaam GRS zodat gegevens veerkrachtig als een regionale is uitgevallen of de primaire regio kan niet worden hersteld. De account moet zich in hetzelfde gebied, als de kluis Recovery Services. Premium-opslagruimte wordt alleen ondersteund als u bent VMs VMware of fysieke servers te repliceren.

### <a name="how-often-can-i-replicate-data"></a>Hoe vaak worden gegevens kunt repliceren?

- **Hyper-V:** Hyper-V VMs kan worden gerepliceerd om de 30 seconden, 5 minuten of 15 minuten. Als u de SAN-replicatie hebt ingesteld zijn replicatie is synchroon.
- **VMware en fysieke servers:** Frequentie van de replicatie is hier niet relevant. Replicatie is doorlopend.

### <a name="can-i-extend-replication-from-existing-recovery-site-to-another-tertiary-site"></a>Kan ik replicatie van bestaande recovery-site naar een andere tertiaire site uitbreiden?

Uitgebreide of aaneengeschakelde replicatie wordt niet ondersteund. Deze functie in [feedback Feedbackforum](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication)aanvragen.


### <a name="can-i-do-an-offline-replication-the-first-time-i-replicate-to-azure"></a>Kan ik een offline replicatie de eerste keer dat ik naar Azure repliceren?

Dit wordt niet ondersteund. Deze functie in het [Feedbackforum](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from)aanvragen.


### <a name="can-i-exclude-specific-disks-from-replication"></a>Kan ik bepaalde schijven uitsluiten van replicatie

Dit wordt ondersteund wanneer u [VMs VMware en fysieke servers te repliceren](site-recovery-vmware-to-azure.md#exclude-disks-from-replication) naar Azure, via de portal Azure.


### <a name="can-i-replicate-virtual-machines-with-dynamic-disks"></a>Kan ik repliceren virtuele machines met dynamische schijven?

Dynamische schijven worden ondersteund bij het repliceren van Hyper-V virtuele machines. Ze worden ook ondersteund bij het repliceren van VMs VMware en fysieke machines naar Azure. De schijf moet een standaardschijf.

### <a name="can-i-add-a-new-machine-to-an-existing-replication-group"></a>Kan ik een nieuwe computer toevoegen aan een bestaande replicatiegroep?

Nieuwe computers toevoegen aan bestaande replicatiegroepen wordt ondersteund. Hiertoe selecteert u de replicatiegroep (van 'Gerepliceerde objecten' blade) en het contextmenu /-selecteren Klik met de rechtermuisknop op de replicatiegroep en selecteer de gewenste optie.

![Toevoegen aan een replicatiegroep](./media/site-recovery-faq/add-server-replication-group.png)

### <a name="can-i-throttle-bandwidth-allotted-for-hyper-v-replication-traffic"></a>Kan ik de bandbreedte toegewezen voor het verkeer, Hyper-V?

Ja. U kunt meer lezen over de in de artikelen voor de implementatie van de bandbreedte regelen:

- [Capaciteitsplanning voor het repliceren van VMs VMware en fysieke servers](site-recovery-vmware-to-azure.md#step-5-capacity-planning)
- [Capaciteitsplanning voor het repliceren van Hyper-V VMs in VMM wolken](site-recovery-vmm-to-azure.md#step-5-capacity-planning)
- [Capaciteitsplanning voor het repliceren van VMs Hyper-V zonder de VMM](site-recovery-hyper-v-site-to-azure.md#step-5-capacity-planning)

## <a name="failover"></a>Failover


### <a name="if-im-failing-over-to-azure-how-do-i-access-the-azure-virtual-machines-after-failover"></a>Als ik ben een failover uitvoeren naar Azure, hoe krijg ik toegang tot de Azure virtuele machines na een failover?

U kunt de VMs Azure benaderen via een beveiligde internetverbinding via een VPN website of via Azure ExpressRoute. U moet een aantal dingen om verbinding te kunnen maken. Lees meer in:

- [Verbinding maken met Azure VMs nadat failover van VMs VMware of fysieke servers](hsite-recovery-vmware-to-azure.md#step-7-test-the-deployment)
- [Verbinding maken met Azure VMs nadat failover van Hyper-V VMs in VMM wolken](site-recovery-vmm-to-azure.md#step-7-test-your-deployment)
- [Verbinding maken met Azure VMs nadat failover van VMs Hyper-V zonder de VMM](site-recovery-hyper-v-site-to-azure.md#step-7-test-the-deployment)


### <a name="if-i-fail-over-to-azure-how-does-azure-make-sure-my-data-is-resilient"></a>Als ik failover uitvoeren naar Azure hoe Azure ervoor zorgen dat mijn gegevens is veerkrachtig?

Azure is ontworpen voor veerkracht. Herstel van de site is al gebouwd voor failover naar secundaire Azure datacenter, volgens de SLA Azure als nodig is. Als dit gebeurt, we zorgen ervoor dat uw metagegevens en kluizen binnen hetzelfde geografische gebied dat u hebt gekozen voor uw kluis blijven.  

### <a name="if-im-replicating-between-two-datacenters-what-happens-if-my-primary-datacenter-experiences-an-unexpected-outage"></a>Ik ben repliceren tussen twee datacenters wat gebeurt er als mijn primaire datacenter als er een onverwachte stroomstoring?

U kunt een niet-geplande failover van de secundaire site activeren. Herstel van de site hoeft niet verbinding van de primaire site voor het uitvoeren van failover.

### <a name="is-failover-automatic"></a>Failover automatische is?

Failover wordt niet automatisch. Start u failover met één klik in de portal of [Site herstel PowerShell](https://msdn.microsoft.com/library/dn850420.aspx) kunt voor het starten van een failover. Bij gebreke daarvan terug is een eenvoudige actie in de portal-Site terugzetten.

Kunt u automatiseren kunnen Orchestrator in ruimten of Operations Manager gebruiken voor het detecteren van de virtuele machine is vastgelopen en activeert vervolgens de failover-functie met de SDK.

- [Lees meer](site-recovery-create-recovery-plans.md) over de herstelplannen.
- [Lees meer](site-recovery-failover.md) informatie over failover.
- [Lees meer](site-recovery-failback-azure-to-vmware.md) over mislukte back VMs VMware en fysieke servers


## <a name="service-providers"></a>Serviceproviders


### <a name="im-a-service-provider-does-site-recovery-work-for-dedicated-and-shared-infrastructure-models"></a>Ik ben een Internet-provider. Werkt de Site herstellen voor specifieke en gedeelde infrastructuur modellen?

Ja, herstel van de Site ondersteunt beide modellen toegewijd en gedeelde infrastructuur.

### <a name="for-a-service-provider-is-the-identity-of-my-tenant-shared-with-the-site-recovery-service"></a>Een serviceprovider is de identiteit van de huurder gedeeld met de Site-Recovery-service?

Nr. Identiteit van de huurder blijft anoniem. De huurder hoeft geen toegang tot de portal-Site terugzetten. Alleen de beheerder van de serviceprovider communiceert met de portal.


### <a name="will-tenant-application-data-ever-go-to-azure"></a>Toepassingsgegevens huurder ooit gaat naar Azure?

Replicatie tussen sites van service provider eigendom, gaat toepassingsgegevens nooit naar Azure. Gegevens worden gecodeerd in transit en gerepliceerde rechtstreeks tussen de sites serviceprovider.

Als u naar Azure repliceren wilt, worden toepassing verzonden naar Azure opslag maar niet op de Site-Recovery-service. Gegevens gecodeerd in transit is en blijft in Azure gecodeerd.


### <a name="will-my-tenants-receive-a-bill-for-any-azure-services"></a>Ontvang mijn huurders een wissel voor Azure services?

Nr. Factuuradres relatie van Azure is rechtstreeks met de serviceprovider. Serviceproviders zijn verantwoordelijk voor het genereren van specifieke wissels voor hun huurders.

### <a name="if-im-replicating-to-azure-do-we-need-to-run-virtual-machines-in-azure-at-all-times"></a>Als ik naar Azure repliceren ben, hebben we nodig voor het uitvoeren van de virtuele machines in Azure te allen tijde?

Nee, de gegevens worden gerepliceerd naar een rekening Azure opslag in uw abonnement. Bij het uitvoeren van een test failover (DR drill) of een werkelijke failover Recovery Site virtuele machines automatisch gemaakt in uw abonnement.

### <a name="do-you-ensure-tenant-level-isolation-when-i-replicate-to-azure"></a>Zorgt u er isolatie huurder niveau wanneer ik naar Azure repliceren?

Ja.

### <a name="what-platforms-do-you-currently-support"></a>Welke platforms momenteel ondersteund?

We ondersteunen Azure Pack, Cloud Platform System, en implementaties (2012 en hoger) op basis van System Center. [Meer informatie](https://technet.microsoft.com/library/dn850370.aspx) over de integratie van Azure Pack en herstel van de Site.

### <a name="do-you-support-single-azure-pack-and-single-vmm-server-deployments"></a>Ondersteund één Azure Pack en één VMM serverimplementaties?

Ja, kunt u Hyper-V virtuele machines te repliceren naar Azure of repliceren tussen sites van serviceprovider.  Houd er rekening mee dat als u tussen sites van serviceprovider repliceren, integratie van de Azure runbook is niet beschikbaar.


## <a name="next-steps"></a>Volgende stappen

- Lees het [Site-overzicht](site-recovery-overview.md)
- Meer informatie over de [architectuur van de Site herstellen](site-recovery-components.md)  
