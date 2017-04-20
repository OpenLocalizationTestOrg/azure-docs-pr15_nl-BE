<properties
    pageTitle="Hoe werkt de Site herstellen? | Microsoft Azure"
    description="Dit artikel biedt een overzicht van de architectuur van de Site herstellen"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="how-does-azure-site-recovery-work"></a>Hoe werkt de Azure Site herstellen?

Lees dit artikel om te begrijpen van de onderliggende architectuur van de Azure-Site-Recovery-service en de onderdelen waarmee u werkt. 

Opmerkingen of vragen aan de onderkant van dit artikel, of op het [Forum van Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)boeken.


## <a name="overview"></a>Overzicht

Organisaties hebben behoefte aan een strategie voor BCDR die bepaalt hoe apps, werkbelasting en gegevens blijven actief en beschikbaar tijdens geplande en ongeplande uitvaltijd en normale gebruiksomstandigheden zo snel mogelijk herstellen. Uw strategie voor BCDR moet u bedrijfsgegevens veilig en terug te vorderen en dat werklasten continu beschikbaar blijven als ramp plaatsvindt. 

Herstel van de site is een Azure service die deel uitmaakt van uw strategie voor BCDR door replicatie van fysieke servers op gebouwen en virtuele machines naar de cloud (Azure) of een secundaire datacenter slaagde. Wanneer er storingen optreden in de primaire locatie, failover u uitvoeren naar de secundaire locatie apps en werkbelasting beschikbaar te houden. U niet terug naar de primaire locatie als resultaat wordt gegeven voor normale bewerkingen. Meer informatie in [Wat is Systeemherstel Site?](site-recovery-overview.md)

## <a name="site-recovery-in-the-azure-portal"></a>Herstel van de site in de portal voor Azure

Azure heeft twee verschillende [implementatiemodellen](../resource-manager-deployment-model.md) voor het maken en werken met resources: de bronnenbeheerder Azure-model en het model Klassiek services management. Azure heeft ook twee portals – [Azure klassieke portal](https://manage.windowsazure.com/) die het implementatiemodel klassiek ondersteunt en de [Azure portal](https://portal.azure.com) met ondersteuning voor beide implementatiemodellen.

Herstel van de site is beschikbaar in zowel de klassieke portal en de Azure portal. In de klassieke Azure portal kunt u sites worden hersteld met het model Klassiek services management ondersteunen. De Azure portal kunt u het klassieke model of Model voor Resource-implementaties ondersteunen. [Lees meer](site-recovery-overview.md#site-recovery-in-the-azure-portal) over de implementatie van de portal Azure.

De informatie in dit artikel is van toepassing op classic en Azure portals. Verschillen worden vermeld, indien van toepassing.

## <a name="deployment-scenarios"></a>Implementatiescenario 's

Herstel van de site kan worden geïmplementeerd evoluerende replicatie in een aantal scenario's:

- **Repliceren VMware virtuele machines**: op ruimten VMware virtuele machines kan worden gerepliceerd naar Azure of een secundaire datacenter.
- - **Fysieke machines te repliceren**: fysieke machines met Windows of Linux Azure of een secundaire datacenter kan worden gerepliceerd. Het proces voor het repliceren van fysieke machines is bijna hetzelfde als het proces voor het repliceren van VMs VMware
- **Hyper-V VMs repliceren (zonder VMM)**: repliceert u Hyper-V VMs die niet worden beheerd door VMM op Azure.
- **Hyper-V VMs repliceren beheerd in System Center VMM wolken**: U kunt repliceren op ruimten Hyper-V virtuele machines uitgevoerd op host-servers Hyper-V in wolken VMM Azure of een secundaire datacenter. U kunt met standard Hyper-V-Replica of SAN replicatie repliceren.
- **VMs migreren**: U kunt een Site terugzetten [Azure IaaS VMs migreren](site-recovery-migrate-azure-to-azure.md) tussen regio's of [exemplaren van Windows (AWS)](site-recovery-migrate-aws-to-azure.md) migreren naar Azure IaaS VMs. Op dit moment alleen migratie is kunt u deze VMs failover maar u terug kan niet ondersteund.

Site herstel kan de meeste apps op deze VMs en fysieke servers repliceren. U krijgt een volledig overzicht van de ondersteunde toepassingen in de [welke werklasten kunt Azure Site herstellen beveiligen?](site-recovery-workload.md)


## <a name="replicate-to-azure-vmware-virtual-machines-or-physical-windowslinux-servers"></a>Repliceren naar Azure: VMware virtuele machines of fysieke Windows/Linux servers

Er zijn verschillende manieren voor het repliceren van VMs VMware met terugwinning van de Site.

- **Met behulp van de portal Azure**-wanneer u Recovery Site in Azure portal kunt u VMs failover naar de klassieke service manager opslag of Resource Manager te implementeren. VMs VMware repliceren in Azure portal brengt een aantal voordelen, waaronder de mogelijkheid om te repliceren naar klassieke of Resource Manager opslag in Azure. [Meer informatie](site-recovery-vmware-to-azure.md).
- **Met behulp van de klassieke portal**-kunt u de Site herstellen in de portal voor een uitgebreide ervaring met klassieke implementeren. Dit moet worden gebruikt voor alle nieuwe installaties in de klassieke portal. In deze installatie kan u alleen failover VMs voor klassieke opslag in Azure en niet voor storage Resource Manager. [Meer informatie](site-recovery-vmware-to-azure-classic.md). Er is een [oudere ervaring](site-recovery-vmware-to-azure-classic-legacy.md) voor het instellen van VMware replicatie in de klassieke portal. Dit moet niet worden gebruikt voor nieuwe implementaties.  Als u met behulp van de oude ervaring [informatie over het migreren](site-recovery-vmware-to-azure-classic-legacy.md#migrate-to-the-enhanced-deployment) naar de uitgebreide implementatie al hebt geïmplementeerd.



De architecturale vereisten voor het gebruik van Recovery-Site voor het repliceren van VMs VMware/fysieke servers in de Azure portal of de Azure klassieke portal (verbeterde) lijken, met een aantal verschillen:

- Als u in de Azure portal kunt u netwerken Resource Manager gebruiken voor het aansluiten van de VMs Azure na een failover en repliceren naar opslag op basis van een Resource Manager implementeert.
- Wanneer u beide LRS implementeren in de portal Azure en GRS opslag wordt ondersteund. In de klassieke portal GRS is vereist.
- Implementatie is vereenvoudigd en meer gebruikersvriendelijke manier in de portal Azure.


Dit is wat u nodig hebt:

- **Azure account**: moet u een account van Microsoft Azure.
- **Azure opslag**: U moet een account Azure opslag gerepliceerde gegevens wilt opslaan. U kunt een klassieke account of een account van de opslag Resource Manager. De account is LRS of GRS wanneer u in de portal Azure implementeert. Gerepliceerde gegevens worden opgeslagen in Azure opslag en Azure VMs worden gesponnen omhoog wanneer failover plaatsvindt. 
- **Azure netwerk**: je hebt een Azure virtueel netwerk die Azure VMs wordt verbonden wanneer ze zijn gemaakt op een failover. Ze kunnen in Azure portal netwerken in het model Klassiek service manager of de Resource Manager model gemaakt worden.
- **On-premises-configuratieserver**: moet u een in de lokalen Windows Server 2012 R2 machine die wordt uitgevoerd de van configuratieserver en andere onderdelen van de Site herstellen. Als u bij het repliceren van VMs VMware moet dit een hoge mate van beschikbaarheid VMware VM. Als u wilt repliceren fysieke servers kan de computer fysiek zijn. Deze Site Recovery-onderdelen worden geïnstalleerd op de computer:
    - **Configuratieserver**: communicatie tussen uw omgeving op gebouwen en Azure coördineert en beheert de gegevensreplicatie en herstel.
    - **Process-server**: fungeert als gateway voor replicatie. Deze replicatiegegevens worden ontvangen van beveiligde bron machines optimaliseert met caching, compressie en codering en stuurt de gegevens naar Azure opslag. Ook push installatie van de service mobiliteit voor beveiligde computers worden verwerkt en voert de automatische detectie van VMs VMware. Als uw distributie groeit kunt u extra afzonderlijk specifieke proces servers verwerken steeds grotere hoeveelheden replicatieverkeer toevoegen.
    - **Master-doelserver**: replicatiegegevens verwerkt tijdens de failback van Azure. 
- **VMs VMware of fysieke servers te repliceren**: elke computer die u wilt repliceren naar Azure, moet het onderdeel mobiliteit service is geïnstalleerd. Deze service wordt weggeschreven op de computer en ze doorstuurt naar de processerver. Dit onderdeel kan handmatig worden geïnstalleerd of kan worden geduwd en automatisch door de processerver wordt geïnstalleerd wanneer u replicatie voor een machine.
- **vSPhere hosts/vCenter server**: moet u één of meer vSphere hostservers met VMware VMs. Wij raden aan dat u implementeert een vCenter server beheren die hosts.
- **Failback**: hier is wat u nodig hebt:
    - **Fysieke fysieke failback-bewerking wordt niet ondersteund**: dit betekent dat als u een failover uitvoeren naar Azure-fysieke servers en wilt een failback, u moet niet naar een VM VMware. U kan geen failback naar een fysieke server. Je hebt een Azure VM met en als u niet de server configureren als een VM VMware implementeren moet u voor het instellen van een aparte master doelserver als een VM VMware. Dit is nodig omdat de doelserver master samenwerkt en koppelt aan opslag om te zetten van de schijven op een VM VMware VMware.
    - - **Processerver tijdelijke in Azure**: als u van Azure mislukken nadat de failover wilt, u moet voor het instellen van een Azure VM als een processerver is geconfigureerd voor het verwerken van replicatie vanaf Azure. U kunt deze VM verwijderen nadat de failback-bewerking is voltooid.
    - **VPN-verbinding**: voor failback-bewerking u moet een VPN-verbinding (of Azure ExpressRoute) instellen van het netwerk Azure naar de site van gebouwen.
    - **Afzonderlijke op lokalen master doelserver**: de master doelserver op ruimten failback worden verwerkt. De doelserver basispagina standaard op de server is geïnstalleerd, maar als u weer grotere volumes van verkeer mislukken bent moet u instellen een master doelserver afzonderlijke op lokalen voor dit doel.

**Algemene architectuur**

![Verbeterde](./media/site-recovery-components/arch-enhanced.png)

**Implementatie van onderdelen**

![Verbeterde](./media/site-recovery-components/arch-enhanced2.png)

**Failback**

![Verbeterde failback](./media/site-recovery-components/enhanced-failback.png)


- [Meer informatie](site-recovery-vmware-to-azure.md#azure-prerequisites) over de vereisten voor Azure portal-implementatie.
- [Meer informatie](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) over het verbeterde implementatie-eisen in de klassieke portal.
- [Meer informatie](site-recovery-failback-azure-to-vmware.md) over de failback-bewerking in de portal voor Auzre.
- [Meer informatie](site-recovery-failback-azure-to-vmware-clas- [Learn more](site-recovery-failback-azure-to-vmware-classic.md) about failback in the Auzre portal.sic.md) over de failback-bewerking in de klassieke portal.

## <a name="replicate-to-azure-hyper-v-vms-not-managed-by-vmm"></a>Repliceren naar Azure: Hyper-V VMs niet wordt beheerd door de VMM

U kunt repliceren VMs Hyper-V die niet worden beheerd door System Center VMM naar Azure met terugwinning van de Site als volgt:

- **Met behulp van de portal Azure**-wanneer u Recovery Site in Azure portal kunt u VMs failover naar de klassieke opslag of Resource Manager te implementeren. [Meer informatie](site-recovery-hyper-v-site-to-azure.md).
- **Met behulp van de klassieke portal**-kunt u de Site herstellen in de klassieke portal implementeren. In deze installatie kan u alleen failover VMs voor klassieke opslag in Azure en niet voor storage Resource Manager. [Meer informatie](site-recovery-hyper-v-site-to-azure-classic.md).

De architectuur voor beide implementaties lijkt, met dien verstande dat:

- Als u in de Azure portal kunt u netwerken Resource Manager gebruiken voor het aansluiten van de VMs Azure na een failover en repliceren naar opslag Resource Manager implementeert.
- Implementatie is vereenvoudigd en meer gebruikersvriendelijke manier in de portal Azure.

Dit is wat u nodig hebt:

- **Azure account**: moet u een account van Microsoft Azure.
- **Azure opslag**: U moet een account Azure opslag gerepliceerde gegevens wilt opslaan. In de Azure portal kunt u een klassieke of een Resource Manager opslag-account. In de klassieke portal kunt u alleen een klassieke account. Gerepliceerde gegevens worden opgeslagen in Azure opslag en Azure VMs worden gemaakt wanneer failover plaatsvindt.
- **Azure netwerk**: je hebt een Azure-netwerk Azure VMs wordt verbonden wanneer ze zijn gemaakt na een failover. 
- **Hyper-v host**: moet u een of meer Windows Server 2012 R2 Hyper-V host-server. Tijdens de implementatie van het herstel van de Site zult u de Provider Azure Site herstellen en de agent Microsoft Azure Recovery Services op de host installeren.
- **Hyper-V VMs**: moet u een of meer VMs op de Hyper-V host-server. Azure siteprovider herstel en de agent Azure Recovery Services op de Hyper-V host tijdens de implementatie van het herstel van de Site. De Provider coördineert en orchestrates replicatie met de Site-Recovery-service via het internet. De agent verwerkt gegevens replicatiegegevens via HTTPS-443. Communicatie van de Provider en de agent zijn beveiligd en gecodeerd. Gerepliceerde gegevens in Azure opslag ook gecodeerd.

**Algemene architectuur**

![Azure-site Hyper-V](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)


- [Meer informatie](site-recovery-hyper-v-site-to-azure.md#azure-prerequisites) over de vereisten voor Azure portal-implementatie.
- [Meer informatie](site-recovery-hyper-v-site-to-azure-classic.md#azure-prerequisites) over de vereisten voor klassieke portal-implementatie.



## <a name="replicate-to-azure-hyper-v-vms-managed-by-vmm"></a>Repliceren naar Azure: Hyper-V VMs beheerd door VMM

U kunt Hyper-V VMs in wolken VMM repliceren naar Azure met terugwinning van de Site als volgt:

- **Met behulp van de portal Azure**-wanneer u Recovery Site in Azure portal kunt u VMs failover naar de klassieke opslag of Resource Manager te implementeren. [Meer informatie](site-recovery-vmm-to-azure.md).
- **Met behulp van de klassieke portal**-kunt u de Site herstellen in de klassieke portal implementeren. In deze installatie kan u alleen failover VMs voor klassieke opslag in Azure en niet voor storage Resource Manager. [Meer informatie](site-recovery-vmm-to-azure-classic.md).

De architectuur voor beide implementaties lijkt, met dien verstande dat:

- Als u in de Azure portal kunt u netwerken Resource Manager gebruiken voor het aansluiten van de VMs Azure na een failover en repliceren naar opslag op basis van een Resource Manager implementeert.
- Implementatie is vereenvoudigd en meer gebruikersvriendelijke manier in de portal Azure.


Dit is wat u nodig hebt:

- **Azure account**: moet u een account van Microsoft Azure.
- **Azure opslag**: U moet een account Azure opslag gerepliceerde gegevens wilt opslaan. In de Azure portal kunt u een klassieke of een Resource Manager opslag-account. In de klassieke portal kunt u alleen een klassieke account. Gerepliceerde gegevens worden opgeslagen in Azure opslag en Azure VMs worden gemaakt wanneer failover plaatsvindt.
- **Azure netwerk**: je hebt voor het instellen van het netwerk toewijzen zodat wanneer ze zijn gemaakt na een failover Azure VMs met passende netwerken zijn verbonden. 
- **VMM server**: U moet een of meer op ruimten VMM servers op System Center 2012 R2 en ingesteld met een of meer particuliere wolken. Als u in de Azure portal dient u logische en VM netwerken instellen zodat u de toewijzing van het netwerk kunt configureren. In de klassieke portal is optioneel.  Een netwerk voor VM moet worden gekoppeld aan een logisch netwerk dat is gekoppeld aan de cloud.
- **Hyper-v host**: moet u een of meer Windows Server 2012 R2 Hyper-V host-servers in de cloud VMM.
- **Hyper-V VMs**: moet u een of meer VMs op de Hyper-V host-server.

**Algemene architectuur**

![VMM naar Azure](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)

- [Meer informatie](site-recovery-vmm-to-azure.md#azure-requirements) over de vereisten voor Azure portal-implementatie.
- [Meer informatie](site-recovery-vmm-to-azure-classic.md#before-you-start) over de vereisten voor klassieke portal-implementatie.




## <a name="replicate-to-a-secondary-site-vmware-virtual-machines-or-physical-servers"></a>Repliceren naar een secundaire site: VMware virtuele machines of fysieke servers 

VMs VMware of fysieke servers repliceren naar een secundaire site gedownload InMage Scout die opgenomen in het abonnement Azure Site herstellen. Worden kan gedownload vanaf de Azure portal of via de klassieke Azure portal. 

U stelt u component servers in elke site (configuratie, proces, master doel) en de Unified Agent installeren op computers die u wilt repliceren. Na de eerste replicatie verzendt de agent op elke machine replicatiewijzigingen delta op de server verwerken. Process-server de gegevens geoptimaliseerd en overgebracht naar de doelserver master op de secundaire site. De configuratieserver beheert de replicatie.

Dit is wat u nodig hebt:

**Azure account**: implementeren van dit scenario met InMage Scout. U kunt deze moet u een abonnement op Azure. Nadat u een kluis Recovery Site hebt gemaakt kunt u InMage Scout download en installeer de nieuwste updates voor het instellen van de implementatie.
**Processerver (primaire site)**: het serveronderdeel proces ingesteld in uw primaire site caching, compressie en optimalisatie van gegevens verwerken. Ook wordt omgegaan push installatie van de Agent Unified voor computers die u wilt beveiligen. 
**VMware ESX/ESXi en vCenter server (primaire site)**: als u bij het beveiligen van VMs VMware moet u een VMware EXS/ESXi hypervisor en eventueel een VMware vCenter server beheren hypervisors.
- **VMs/fysieke servers (de primaire site)**: VMs VMware of Windows/Linux fysieke servers u wilt beveiligen, zal moeten de Unified Agent is geïnstalleerd. De Agent Unified wordt ook geïnstalleerd op de computers die fungeert als de master-doelserver. De agent fungeert als een provider communicatie tussen alle onderdelen. 
- - **Configuratieserver (secundaire site)**: de configuratieserver is het eerste onderdeel dat u installeert en deze op de secundaire site te beheren, configureren en controleren van uw implementatie, hetzij door middel van de website van het management of de vContinuum-console geïnstalleerd. Er is alleen een met één configuratieserver in een implementatie en het moet worden geïnstalleerd op een computer met Windows Server 2012 R2.
- **vContinuum-server (secundaire site)**: die geïnstalleerd op dezelfde locatie (secundaire site) als de configuratieserver. Een console bevat voor het beheren en controleren van uw beveiligde omgeving. In een standaardinstallatie van de server vContinuum is de eerste master doelserver en de Unified Agent is geïnstalleerd.
- **Master-doelserver (secundaire site)**: de master doelserver gerepliceerde gegevens bevat. Deze gegevens worden ontvangen van de processerver maakt een replica machine op de secundaire site en de inhouding gegevenspunten bevat. Het aantal master doelservers die u nodig hebt, hangt af van het aantal computers die u wilt beveiligen. Als u wilt dat voor failback naar primaire site moet u er een master doelserver te. 

**Algemene architectuur**

![VMware VMware](./media/site-recovery-components/vmware-to-vmware.png)


## <a name="replicate-to-a-secondary-site-hyper-v-vms-managed-by-vmm"></a>Repliceren naar een secundaire site: Hyper-V VMs beheerd door VMM


U kunt repliceren VMs Hyper-V die worden beheerd door System Center VMM aan een secundaire datacenter met terugwinning van de Site als volgt:

- **Met behulp van de portal Azure**-wanneer u Recovery-Site in de portal Azure implementeren. [Meer informatie](site-recovery-hyper-v-site-to-azure.md).
- **Met behulp van de klassieke portal**-kunt u de Site herstellen in de klassieke portal implementeren. [Meer informatie](site-recovery-hyper-v-site-to-azure-classic.md).

De architectuur voor beide implementaties lijkt, met dien verstande dat:

- Als u in de Azure portal, die moet u de netwerk-toewijzing instellen. Dit is optioneel in de klassieke portal.
- Implementatie is vereenvoudigd en meer gebruikersvriendelijke manier in de portal Azure.
- - Klassieke portal [opslag toewijzing](site-recovery-storage-mapping.md) is beschikbaar als u in de Azure implementeren.

Dit is wat u nodig hebt:

- **Azure account**: moet u een account van Microsoft Azure.
- **VMM server**: aangeraden een VMM-server in de primaire site en in de secundaire site, elk met ten minste één particuliere wolk van VMM. De server waarop ten minste System Center 2012 SP1 met de meest recente updates en verbonden met het internet. Wolken moet de Hyper-V-mogelijkheid profiel instellen. Installeert u de Provider Azure Site terugzetten op de server van VMM. De Provider coördineert en orchestrates replicatie met de Site-Recovery-service via het internet. Communicatie tussen de aanbieder en Azure zijn beveiligd en gecodeerd.
- **Hyper-V server**: host-servers Hyper-V moeten zich bevinden in de primaire en secundaire VMM wolken. De host servers waarop ten minste Windows Server 2012 met de nieuwste updates geïnstalleerd en aangesloten op het internet. Gegevens worden gerepliceerd tussen de primaire en secundaire Hyper-V host-servers via LAN of VPN-certificaat- of Kerberos-verificatie gebruiken.  
- **Beveiligde computers**: de bron van Hyper-V host-server moet ten minste één VM die u wilt beveiligen.

**Algemene architectuur**

![Voor ruimten tot op de gebouwen](./media/site-recovery-components/arch-onprem-onprem.png)


- [Meer informatie](site-recovery-vmm-to-vmm.md#azure-prerequisites) over de implementatievereisten voor in de portal Azure.
- - [Meer informatie](site-recovery-vmm-to-vmm-classic.md#before-you-start) over de implementatievereisten voor in de klassieke Azure portal.




## <a name="replicate-to-a-secondary-site-with-san-replication-hyper-v-vms-managed-by-vmm"></a>Met een secundaire site met SAN-replicatie gerepliceerd: Hyper-V VMs beheerd door VMM

Hyper-V VMs beheerd in wolken VMM aan een secundaire SAN-replicatie met behulp van de klassieke Azure portal-site kan worden gerepliceerd. In dit scenario wordt niet momenteel ondersteund in de nieuwe portal Azure. 

In dit scenario tijdens de implementatie van het herstel van de Site installeert u de Provider Azure Site terugzetten op servers van VMM. De Provider coördineert en orchestrates replicatie met de Site-Recovery-service via het internet. Gegevens worden gerepliceerd tussen de primaire en secundaire opslagarrays via synchrone SAN-replicatie.

Dit is wat u nodig hebt:

**Azure-rekening**: je hebt een abonnement op Azure
- **SAN-array**: een [SAN-array wordt ondersteund](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx) door de primaire VMM server beheerd. Een netwerkinfrastructuur deelt SAN met een ander SAN-array in de secundaire site.
- **VMM server**: aangeraden een VMM-server in de primaire site en in de secundaire site, elk met ten minste één particuliere wolk van VMM. De server waarop ten minste System Center 2012 SP1 met de meest recente updates en verbonden met het internet. Wolken moet de Hyper-V-mogelijkheid profiel instellen.
- **Hyper-V server**: Hyper-V host-servers zich in de primaire en secundaire VMM wolken. De host servers waarop ten minste Windows Server 2012 met de nieuwste updates geïnstalleerd en aangesloten op het internet.
- **Beveiligde computers**: de bron van Hyper-V host-server moet ten minste één VM die u wilt beveiligen.

**Architectuur van SAN-replicatie**

![SAN-replicatie](./media/site-recovery-components/arch-onprem-onprem-san.png)

[Meer informatie](site-recovery-vmm-san.md#before-you-start) over de implementatievereisten voor.
### <a name="on-premises"></a>In gebouwen



## <a name="hyper-v-protection-lifecycle"></a>Levenscyclus van Hyper-V-beveiliging

Deze workflow bevat het proces voor het beschermen, repliceren en failover Hyper-V virtuele machines. 

1. **Beveiliging**: instellen van de Site herstel kluis, replicatie-instellingen voor een wolk VMM of Hyper-V-site configureren en beveiliging voor VMs inschakelen. Een project genaamd **Beveiliging inschakelen** wordt gestart en kan worden gecontroleerd op het tabblad **taken** . De taak wordt gecontroleerd dat de computer aan de vereisten voldoet en roept vervolgens de methode [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) stelt Azure-replicatie met de instellingen die u hebt geconfigureerd. De taak **beveiliging** roept ook de methode [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) om een volledige replicatie van VM initialiseren.
2. **Initiële replicatie**: een momentopname van een virtuele machine wordt genomen en virtuele harde schijven zijn gerepliceerde een voor een totdat ze alle gekopieerd naar Azure of naar het secundaire datacenter. De tijd die nodig is voor het voltooien van dit hangt af van de grootte VM, netwerkbandbreedte en de methode voor de eerste replicatie. Als de schijf wijzigingen optreden tijdens de eerste replicatie worden bijgehouden in beheer Hyper-V Replica replicatie die wijzigingen Als Hyper-V logboeken van bestandsreplicatie (.hrl) die in dezelfde map als de schijven bevinden zich. Elke schijf heeft een bijbehorende .hrl-bestand dat wordt verzonden naar het secundaire opslag. Houd er rekening mee dat de momentopname en log bestanden schijfbronnen verbruiken, terwijl de eerste replicatie uitgevoerd wordt. Als de eerste replicatie is voltooid de VM momentopname wordt verwijderd en de wijzigingen van de delta-schijf in het logboek worden gesynchroniseerd en samengevoegd.
3. **Finalize bescherming**: nadat de eerste replicatie is voltooid voor de taak **Voltooien bescherming** netwerk en andere instellingen na replicatie wordt zo geconfigureerd dat de virtuele machine wordt beschermd. Als u naar Azure repliceren wilt moet u mogelijk de instellingen voor de virtuele machine aanpassen zodat deze voor failover. U kunt op dit moment een failover testen om te controleren dat alles werkt zoals verwacht uitvoeren.
4. **Replicatie**: na de eerste replicatie delta synchronisatie, volgens de replicatie-instellingen. 
    - **Replicatiestoring**: als delta replicatie mislukt, en een volledige replicatie erg nadelig voor de bandbreedte of de tijd zou zijn, dan het opnieuw gebeurt. Voor het voorbeeld als de bestanden .hrl lager is dan 50% van de grootte van de schijf vervolgens de VM worden gemarkeerd voor het opnieuw. Opnieuw synchroniseren minimaliseert u de hoeveelheid gegevens die worden verzonden door de controlesommen van de bron- en virtuele machines en alleen de delta te verzenden. Delta replicatie hervat nadat de synchronisatieprocedure is voltooid. Standaard opnieuw gepland automatisch buiten kantooruren worden uitgevoerd, maar kunt u een virtuele machine handmatig synchroniseren.
    - **Replicatiefout**: een replicatie fout Er is een ingebouwde opnieuw. Als een onherstelbare fout zoals een fout verificatie of machtiging is of een machine replica een ongeldige status is, worden geen nieuwe pogingen worden uitgevoerd. Als een onherstelbare fout, zoals een netwerkfout of schijf weinig ruimte/geheugen is, wordt een totaalwaarde steeds intervallen tussen nieuwe pogingen (1, 2, 4, 8, 10, en vervolgens elke 30 minuten).
4. **Geplande/ongeplande failovers**: geplande of niet-geplande failover kan worden uitgevoerd wanneer dat nodig is. Als u uitvoeren een geplande failover en bron VMs afgesloten om ervoor te zorgen geen gegevens verloren gaan. Nadat VMs replica zijn gemaakt bent deze geplaatst in een commit in afwachting van de staat. U moet doorvoeren zodat ze voltooit de overname, tenzij u bent repliceren met SAN in welk geval commit automatisch. Failback kan plaatsvinden nadat de primaire site actief is. Replicatie vindt automatisch plaats als u hebt gerepliceerd naar Azure omkeren. Anders ere u van omgekeerde replicatie handmatig.
 

![workflow](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Volgende stappen

[Voorbereiden voor distributie](site-recovery-best-practices.md)
