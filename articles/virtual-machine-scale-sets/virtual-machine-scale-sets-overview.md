<properties
    pageTitle="Schaal van de virtuele Machine stelt overzicht | Microsoft Azure"
    description="Meer informatie over virtuele Machine schaal Sets"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="guybo"/>

# <a name="virtual-machine-scale-sets-overview"></a>Virtuele Machine schaal instellen-overzicht

Virtuele machine schaal sets zijn een middel Azure berekenen die kunt u implementeren en beheren van een reeks identiek VMs. Alle VMs geconfigureerd dezelfde VM schaal ingesteld ter ondersteuning van true automatisch schalen – geen vooraf inrichten van VMs is vereist – en als zodanig vereenvoudigt het maken van grootschalige services gericht op grote compute, big data en beperkte werklast.

Voor toepassingen die moeten schalen compute resources uit- en schaal bewerkingen zijn impliciet in evenwicht tussen probleem- en domeinen. Voor een inleiding tot VM schaal verwijzen sets naar de [Azure blog aankondiging](https://azure.microsoft.com/blog/azure-virtual-machine-scale-sets-ga/).

Bekijk deze video's voor meer informatie over VM schaal sets:

 - [Mark Russinovich gesprekken voert Azure schaal Sets](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  

 - [Schaal van de virtuele Machine wordt ingesteld met Guy Bowerman](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-vm-scale-sets"></a>Maken en beheren van VM schaal instellen

Kunt u een VM schaal instellen in de [portal Azure](https://portal.azure.com) _Nieuw_ te selecteren en typt in "scale" in de zoekbalk. 'Virtuele machine schaal instellen' wordt weergegeven in de resultaten. Van daaruit kunt u de vereiste velden aanpassen en implementeren van uw set schaal invullen. 

VM schaal sets kunnen ook worden gedefinieerd en geïmplementeerd met behulp van JSON templates en [REST API's](https://msdn.microsoft.com/library/mt589023.aspx) net als afzonderlijke Azure Resource Manager VMs. Daarom kunnen een standaard Azure Resource Manager distributie methoden worden gebruikt. Zie voor meer informatie over sjablonen, [sjablonen ontwerpen Azure Resource Manager](../resource-group-authoring-templates.md).

Een aantal voorbeeld sjablonen voor VM schaal sets kunt u vinden in de opslagplaats Azure Quickstart sjablonen GitHub [hier.](https://github.com/Azure/azure-quickstart-templates) (zoeken naar sjablonen met _vmss_ in de titel)

In de detailpagina's van deze sjablonen ziet u een knop die is gekoppeld aan de functie van de portal-implementatie. De schaal VM set implementeren, klikt u op de knop en vul vervolgens de parameters die in de portal vereist zijn. Als u niet zeker weet of een resource ondersteunt bovenste of hoofdletters en het is veiliger altijd in kleine letters parameterwaarden wilt gebruiken. Er is ook een handige video-sectie van een VM schaal set template hier:

[VM schaal sjabloon sectie instellen](https://channel9.msdn.com/Blogs/Windows-Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-vm-scale-set-out-and-in"></a>Uit- en een VM-schaal schaal instellen

Vergroten of verkleinen van het aantal virtuele machines in een VM schaal, de eigenschap _capaciteit_ wijzigen en de sjabloon opnieuw implementeren. Deze eenvoud kunt u eenvoudig uw eigen aangepaste schaal laag schrijven als u wilt definiëren aangepaste schaal gebeurtenissen die niet worden ondersteund door Azure automatisch schalen.

Als u opnieuw van een sjabloon om te wijzigen van de capaciteit distribueren, kunt u een veel kleinere sjabloon waarin alleen de SKU, en de bijgewerkte capaciteit definiëren. Ziet u een voorbeeld van deze [hier.](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing)

Om Doorloop de stappen die een schaal set maken waarmee automatisch wordt aangepast, Zie [Automatisch schalen Machines in een virtuele Machine schaal instellen](virtual-machine-scale-sets-windows-autoscale.md)

## <a name="monitoring-your-vm-scale-set"></a>Toezicht op de schaal van de VM instellen

De [portal Azure](https://portal.azure.com) lijsten schaal sets en bevat basiseigenschappen, alsmede aanbieding VMs in de set. Voor meer details kunt u de [Resource Explorer voor Azure](https://resources.azure.com) VM schaal sets weergeven. VM schaal sets zijn een middel onder Microsoft.Compute, zodat u vanaf deze site kunt u deze bekijken door het uitbreiden van de volgende koppelingen:

    subscriptions -> your subscription -> resourceGroups -> providers -> Microsoft.Compute -> virtualMachineScaleSets -> your VM scale set -> etc.

## <a name="vm-scale-set-scenarios"></a>VM schaal ingesteld scenario 's

In deze sectie worden enkele typische VM schaal set scenario's beschreven. Sommige hogere niveau Azure services (zoals Batch, Service Fabric, Azure Container Service) gebruikt deze scenario's.

 - **RDP / SSH VM schaal set exemplaren** - VM A schaal set is gemaakt in een VNET en openbare IP-adressen worden niet toegewezen aan afzonderlijke VMs in de schaal. Dit is goed omdat u niet over het algemeen de overhead kosten en het beheer van afzonderlijke openbare IP-adressen tot alle bronnen in het raster compute stateless toewijzen wilt en u kunt gemakkelijk verbinden met deze VMs uit andere bronnen in uw VNET inclusief die het openbare IP-adressen, zoals netwerktaakverdeling of zelfstandige virtuele machines hebben.

 - **Verbinding maken met VMs met NAT regels** - maakt een openbaar IP-adres, toewijzen aan een load balancer en binnenkomende NAT regels die een poort op het IP-adres aan een poort op een VM in de VM schaal toewijzen definiëren. Bijvoorbeeld:
 
    Bron | Bronpoort | Bestemming | Doelpoort
    --- | --- | --- | ---
    Openbare IP | Poort 50000 | vmss\_0 | Poort 22
    Openbare IP | Poort 50001 | vmss\_1 | Poort 22
    Openbare IP | Poort 50002 | vmss\_2 | Poort 22

    Hier volgt een voorbeeld van het maken van een set van VM schaal die gebruikmaakt van NAT regels om te schakelen SSH-verbinding voor elke VM in een schaal instellen met behulp van een enkele openbare IP-adres: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)

    Hier volgt een voorbeeld van hetzelfde met RDP en Windows doen: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)

 - **Verbinding maken met behulp van een "jumpbox" VMs** - als u een schaal VM ingesteld en een zelfstandige VM in de dezelfde VNET, de zelfstandige VM en de VM schaal die VMS verbinding kunnen maken met elkaar met behulp van hun interne IP-adressen die door de VNET-en Subnet. Als u een openbaar IP-adres maken en aan het zelfstandige VM toewijzen kunt u RDP- of SSH aan het zelfstandige VM en vervolgens vanaf die computer verbinding maken met uw VM schaal set exemplaren. Is er op dit moment dat een eenvoudige VM schaal veiliger dan een eenvoudige zelfstandige VM met een openbaar IP-adres in de standaardconfiguratie is.

    [Met deze sjabloon maakt voor een voorbeeld van deze benadering, een eenvoudige Mesos cluster dat bestaat uit een zelfstandig model VM die een cluster VM schaal instellen op basis van VMs beheert.](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json)

 - **Load balancing VM schaal exemplaren instellen** - als u wilt ervoor zorgen dat werk een compute cluster van VMs met behulp van een "round-robin"-benadering, kunt u een Azure taakverdeling met Netwerktaakverdeling regels dienovereenkomstig. U kunt definiëren sondes om te controleren of de toepassing wordt uitgevoerd door de opdracht ping poorten met een opgegeven pad protocol, interval en de aanvraag. De Azure [Toepassingsgateway](https://azure.microsoft.com/services/application-gateway/) ondersteunt ook schalen sets, samen met meer geavanceerde scenario's voor netwerktaakverdeling.

    [Hier is een voorbeeld dat Hiermee maakt u een VM schaal van VMs met IIS webserver en een load balancer gebruikt om de serverbelasting elke VM ontvangt. Het HTTP-protocol wordt gebruikt bij het pingen van een specifieke URL in elke VM.](https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json) (Bekijk het brontype van de Microsoft.Network/loadBalancers en de networkProfile en extensionProfile in de virtualMachineScaleSet)

 - **Implementeren een VM schaal als een compute cluster in een PaaS cluster manager instellen** -VM schaal sets soms als een werknemer van de volgende generatie rol beschreven worden. Een geldige beschrijving is, maar ook wordt het risico van verwarring schaal set functies met PaaS v1 werknemer functie functies. In zekere zin VM schaal sets bieden een waar "werknemer role" of de bron van de werknemer, leveren een gegeneraliseerde compute bron platform/runtime onafhankelijk, aanpasbare en integreert in Azure Resource Manager IaaS.

    Een PaaS v1 werknemer rol terwijl beperkt in termen van het platform/runtime-ondersteuning (alleen Windows-platform afbeeldingen) omvat ook diensten zoals VIP swap, configureerbare instellingen voor upgrade, runtime/app implementatie specifieke instellingen die niet zijn _nog_ beschikbaar in VM schalen sets of door andere hogere PaaS services op systeemniveau zoals Fabric-Service wordt geleverd. Met dit kunt Vergeet niet dat u bekijken VM schaal wordt ingesteld als een infrastructuur die PaaS ondersteunt. Dat wil zeggen PaaS oplossingen, zoals Service Fabric of cluster managers zoals Mesos op VM bouwen kunt schalen sets als een schaalbare compute-laag.

    [Met deze sjabloon maakt voor een voorbeeld van deze benadering, een eenvoudige Mesos cluster dat bestaat uit een zelfstandig model VM die een cluster VM schaal instellen op basis van VMs beheert.](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json) Toekomstige versies van de [Azure Container Service](https://azure.microsoft.com/blog/azure-container-service-now-and-the-future/) implementeert meer complexe/Hardstalen versies van dit scenario op basis van sets van VM schaal.

## <a name="vm-scale-set-performance-and-scale-guidance"></a>VM schaal instellen van prestaties en richtlijnen schalen

- Maak niet meer dan 500 VMs in meerdere VM schaal stelt tegelijk.
- Niet meer dan 20 VMs per account opslag plannen (tenzij u de eigenschap _overprovision_ op 'false' ingesteld, in welk geval gaat u maximaal 40).
- De eerste letters van namen van opslag zoveel mogelijk verdeeld.  Het voorbeeld VMSS sjablonen in [Azure Quickstart sjablonen](https://github.com/Azure/azure-quickstart-templates/) bevatten voorbeelden van hoe u dit doet.
- Als u aangepaste VMs, niet meer dan 40 VMs per VM schaal instellen op een rekening één opslag plannen.  U moet de afbeelding vooraf rekening de opslag worden gekopieerd voordat u VM schaal implementatie kunt. Zie de veelgestelde vragen voor meer informatie.
- Plan voor maximaal 4096 VMs per VNET.
- Het aantal VMs kunt u wordt beperkt door de core quota in de regio die u wilt implementeren. Wellicht moet u contact opnemen met de klantenondersteuning om te verhogen uw Compute quotumlimiet verhoogd, zelfs als er een hoge limiet van cores voor cloud services of IaaS v1 vandaag. Uw quota query kunt u de volgende opdracht voor Azure CLI uitvoeren: `azure vm list-usage`, en de volgende PowerShell-opdracht: `Get-AzureRmVMUsage` (als een versie van PowerShell onder 1.0 gebruikt `Get-AzureVMUsage`).

## <a name="vm-scale-set-frequently-asked-questions"></a>VM schaal stellen vaak gestelde vragen

**Q.** Hoeveel VMs kunt u schaal in een VM hebt?

**A.** Als u platform images die kunnen worden verdeeld over meerdere accounts voor opslag van 100. Als u aangepaste afbeeldingen, tot 40 (als de eigenschap _overprovision_ is standaard ingesteld op 'false' 20), sinds zijn aangepaste images momenteel beperkt tot een enkele opslag-account.

**Q** welke andere resource grenzen bestaan voor VM schaal sets?

**A.** U bent beperkt tot het maken van niet meer dan 500 VMs in meerdere schalen sets per regio gedurende een periode van 10 minuten. De bestaande [Azure abonnementsservice grenzen /](../azure-subscription-service-limits.md) van toepassing.

**Q.** Zijn gegevens schijven ondersteund VM schaal sets?

**A.** Niet in de eerste release. De opties voor het opslaan van gegevens zijn:

- Azure-bestanden (SMB gedeelde stations)

- OS-station

- Tijdelijke station (lokaal, niet wordt ondersteund door Azure opslag)

- Azure data service (bijvoorbeeld tabellen Azure, Azure BLOB's)

- Service voor externe gegevens (bijv. externe DB)

**Q.** Welke gebieden Azure VM schaal wordt ondersteund?

**A.** De regio die Azure Resource Manager ondersteunt ondersteunt VM schaal Sets.

**Q.** Hoe maakt u een VM schaal instellen met behulp van een aangepaste afbeelding

**A.** De eigenschap vhdContainers leeg laat, bijvoorbeeld:

    "storageProfile": {
        "osDisk": {
            "name": "vmssosdisk",
            "caching": "ReadOnly",
            "createOption": "FromImage",
            "image": {
                "uri": "https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd"
            },
            "osType": "Windows"
        }
    },


**Q.** Als ik mijn VM schaal capaciteit instellen van 20 tot en met 15 die VMs worden verwijderd?

**A.** Virtuele machines worden verwijderd uit de schaal instellen gelijkmatig over upgraden domeinen en domeinen veroorzaakt beschikbaarheid te maximaliseren. VMs met de hoogste id worden eerst verwijderd.

**Q.** Hoe gaat het als verhoog daarna de capaciteit van 15 tot en met 18?

**A.** Als u meer capaciteit tot en met 18, vervolgens 3 nieuwe VMs gemaakt. Elke keer dat de VM exemplaar-id wordt verhoogd van vorige hoogste waarde (bijv. 20, 21, 22). VMs zijn verdeeld over FDs en UDs.

**Q.** Wanneer u meerdere extensies in een VM schaal, kan ik een reeks uitvoering afdwingen?

**A.** Niet rechtstreeks, maar voor de extensie customScript uw script kan wachten op een andere extensie te voltooien ([bijvoorbeeld door het controleren van het logboek extensie](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)). Aanvullende richtlijnen op extensie volgordebepaling vindt u in dit blogbericht: [Extensie volgordebepaling in Azure VM schaal Sets](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).

**Q.** Werk VM schaal sets met Azure beschikbaar stelt?

**A.** Ja. Een set VM schaal is een impliciete beschikbaarheid met een FDs 5 en 5 UDs. U hoeft niet alles onder de virtualMachineProfile configureren. In toekomstige versies van VM schaal sets zijn waarschijnlijk meerdere huurders span maar nu stelt u een schaal is een set enkele beschikbaarheid.
