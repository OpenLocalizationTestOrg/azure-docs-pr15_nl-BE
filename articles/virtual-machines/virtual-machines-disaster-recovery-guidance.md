<properties
    pageTitle="Wat te doen in het geval dat een onderbreking Azure service heeft gevolgen voor Azure virtuele machines | Microsoft Azure"
    description="Informatie over wat te doen in het geval dat een onderbreking Azure service heeft gevolgen voor Azure virtuele machines."
    services="virtual-machines"
    documentationCenter=""
    authors="kmouss"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="virtual-machines"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="kmouss;aglick"/>

#<a name="what-to-do-in-the-event-that-an-azure-service-disruption-impacts-azure-virtual-machines"></a>Wat te doen in het geval dat een onderbreking Azure service heeft gevolgen voor Azure virtuele machines

Bij Microsoft werken we hard om ervoor te zorgen dat onze services altijd voor u beschikbaar zijn wanneer u ze nodig hebt. Krachten buiten onze wil soms van invloed op ons op manieren die ervoor zorgen dat de niet-geplande serviceonderbrekingen.

Microsoft heeft een Service niveau overeenkomst (SLA) voor zijn diensten als een verbintenis voor beschikbaarheid en connectiviteit. De SLA voor afzonderlijke Azure services kunt u vinden op [Azure Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

Azure is al veel ingebouwde functies die ondersteuning bieden voor toepassingen met maximale beschikbaarheid. Lees voor meer informatie over deze services, [noodherstel en hoge beschikbaarheid voor Azure toepassingen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Dit artikel heeft betrekking op true herstelscenario wanneer een hele regio een storing door grote natuurrampen of onderbreking van de wijdverbreide service krijgt. Dit zeldzame exemplaren zijn, maar u moet voorbereiden op de mogelijkheid dat er sprake is van een storing van een hele regio. Als er een onderbreking van de service in een hele regio, kunnen de lokale kopieën van uw gegevens zouden tijdelijk niet beschikbaar. Als u de geo-replicatie hebt ingeschakeld, worden drie extra kopieën van uw opslag Azure BLOB's en tabellen opgeslagen in een andere regio. Bij een volledige regionale uitvalt of een noodsituatie waarin de primaire regio kan niet worden hersteld, remaps Azure alle DNS-vermeldingen op de regio geo gerepliceerd.

>[AZURE.NOTE]Let erop dat er geen controle over dit proces en alleen voor de hele regio serviceonderbrekingen gebeurt. Daarom moet u ook gebaseerd op een andere toepassingsspecifieke back-upstrategieën te bereiken van het hoogste niveau van beschikbaarheid. Zie de sectie op [gegevens strategieën voor herstel na noodgevallen](../resiliency/resiliency-disaster-recovery-azure-applications.md#data-strategies-for-disaster-recovery)voor meer informatie.

Om u te helpen deze zeldzame exemplaren te verwerken, bieden wij de volgende richtlijnen voor Azure virtuele machines in het geval van een onderbreking van de service van de hele regio waar uw Azure VM-toepassing wordt geïmplementeerd.

##<a name="option-1-wait-for-recovery"></a>Optie 1: Wachten op herstel
In dat geval is geen actie van uw kant vereist. U weet dat we naar eer en geweten proberen te herstellen van de beschikbaarheid van de service. De huidige servicestatus kunt u bekijken op onze [Azure Service Health Dashboard](https://azure.microsoft.com/status/).

>[AZURE.NOTE]Dit is de beste optie als u geen Azure Site herstellen, back-up van de virtuele machine, geo-redundante opslag leestoegang of geo-redundante opslag voor de verstoring hebt ingesteld. Als de ingestelde geo-redundante opslag of geo-redundante opslag leestoegang voor de opslag waar de VM virtuele vaste schijven (VHD's) zijn opgeslagen, kunt u de VHD afbeelding herstellen kijk en probeer inrichten van een nieuwe VM uit het. Dit is niet een optie van de voorkeur omdat er geen garanties van de synchronisatie van gegevens tenzij Azure VM back-up of herstel van Azure-Site worden gebruikt. Deze optie is dus niet gegarandeerd te werken.

Voor klanten die directe toegang tot de virtuele machines, zijn de volgende twee opties beschikbaar.  

>[AZURE.NOTE]Let erop dat beide van de volgende opties de mogelijkheid van sommige gegevens verloren gaan hebben.     

##<a name="option-2-restore-a-vm-from-a-backup"></a>Optie 2: Een VM terugzetten vanaf een back-up
Voor klanten die een VM back-up hebt geconfigureerd, kunt u de VM vanaf de back-up en herstel punt terugzetten.

Als u wilt een nieuwe VM Azure back-up terugzetten, Zie [virtuele machines in Azure zetten](../backup/backup-azure-restore-vms.md).

Als u van plan bent voor uw back-upinfrastructuur Azure virtuele machines, Zie [Plan uw back-upinfrastructuur VM in Azure](../backup/backup-azure-vms-introduction.md).

##<a name="option-3-initiate-a-failover-by-using-azure-site-recovery"></a>Optie 3: Een failover starten met behulp van Azure Site herstellen
Als u Azure Site herstellen om te werken met uw risico Azure virtuele machines hebt geconfigureerd, kunt u uw VMs herstellen van hun replica's. Deze replica's kunnen zich bevinden op Azure en in de ruimten. In dit geval kunt u een nieuwe VM uit de bestaande replica. Als u uw VMs herstellen uit een replica Azure Site herstellen, Zie [virtuele machines migreren Azure IaaS tussen Azure gebieden met Azure Site herstellen](../site-recovery/site-recovery-migrate-azure-to-azure.md).

>[AZURE.NOTE]Hoewel het besturingssysteem Azure virtuele machine en data schijven moeten worden gerepliceerd naar een secundaire VHD als ze in een geo-redundante opslag of geo-redundante opslag leestoegang account, wordt elk VHD onafhankelijk gerepliceerd. Dit niveau van replicatie garantie geen van consistentie in de gerepliceerde VHD's. Als uw toepassing en/of databases die gebruikmaken van deze gegevensschijven afhankelijk van elkaar zijn, is het niet gegarandeerd dat alle VHD's worden gerepliceerd als een momentopname. Het is ook niet gegarandeerd dat de VHD-replica van geo-redundante opslag of geo-redundante opslag lezen toegang zal resulteren in een consistente toepassing momentopname de VM opstarten.

##<a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het implementeren van een noodherstel en een strategie voor hoge beschikbaarheid, [noodherstel en hoge beschikbaarheid voor Azure toepassingen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Zie [Azure resiliency technische richtsnoeren](../resiliency/resiliency-technical-guidance.md)voor het ontwikkelen van een gedetailleerde technische kennis van de mogelijkheden van een wolk platform.

Als u wilt weten hoe u een back-up van VMs, Zie [Back-up van Azure virtual machines](../backup/backup-azure-vms.md).

Leren werken met Azure Site herstel evoluerende en bescherming van uw fysieke (en virtuele) Windows en Linux machines die worden uitgevoerd op VMWare en Hyper-V VMs, [Azure Site herstellen](https://azure.microsoft.com/documentation/learning-paths/site-recovery/)Zie automatiseren.

Als de instructies worden niet gewist, of als u wilt dat Microsoft de handelingen namens u te doen, neem dan contact op met [Customer Support](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
