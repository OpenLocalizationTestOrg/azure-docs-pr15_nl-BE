<properties
    pageTitle="Ondersteund platform migratie van IaaS bronnen van klassiek Azure Resource Manager | Microsoft Azure"
    description="Dit artikel helpt bij de migratie platform ondersteund van bronnen van klassiek Azure Resource Manager"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="kasing"/>

# <a name="platform-supported-migration-of-iaas-resources-from-classic-to-azure-resource-manager"></a>Ondersteund platform migratie van IaaS bronnen van klassiek Azure Resource Manager

In dit artikel wordt beschreven hoe we migratie van infrastructuur als een (IaaS) serviceresources uit het klassieke implementatiemodellen Resource Manager wilt inschakelen. U kunt meer lezen over [Azure Resource Manager-functies en voordelen](../azure-resource-manager/resource-group-overview.md). We in detail te beschrijven hoe u verbinding maken met bronnen uit de van twee implementatiemodellen die naast elkaar bestaan in uw abonnement via een virtueel netwerk website gateways. 

## <a name="goal-for-migration"></a>Doelstelling voor migratie

Resource Manager maakt gebruik van complexe toepassingen via sjablonen virtuele machines configureren met behulp van VM-extensies en is uitgerust met beheer- en labels. Azure Resource Manager bevat schaalbare parallelle implementatie voor virtuele machines in sets van beschikbaarheid. De nieuwe implementatie-objectmodel biedt ook levenscyclusbeheer van compute-, netwerk- en onafhankelijk van elkaar. Er is ten slotte een focus op beveiliging standaard met de naleving van de virtuele machines in een virtueel netwerk inschakelen.

Bijna alle functies van de klassieke implementatiemodel worden voor compute, netwerk en opslag onder Azure Resource Manager ondersteund. Als u wilt profiteren van de nieuwe mogelijkheden in Azure Resource Manager, kunt u bestaande installaties van het model Klassiek implementatie migreren.

## <a name="changes-to-your-automation-and-tooling-after-migration"></a>Wijzigingen in uw automatisering en tooling na de migratie

Als onderdeel van de bronnen van het model Klassiek implementatie migreren naar het implementatiemodel Resource Manager, moet u de bestaande automatisering en tooling om ervoor te zorgen dat het blijft werken na de migratie bijwerken.

## <a name="meaning-of-migration-of-iaas-resources-from-classic-to-resource-manager"></a>Betekenis van de migratie van IaaS bronnen van klassiek tot Resource Manager

Voordat we dieper de details, bekijk het verschil tussen de data-plane en management vlak bewerkingen op de IaaS bronnen.

- *Beheervlak* beschrijft de gesprekken die in het vlak van het management of de API voor het wijzigen van bronnen komen. Bewerkingen, zoals het maken van een VM, opnieuw op te starten in een VM en een virtueel netwerk bijwerken met een nieuw subnet wordt bijvoorbeeld de actieve bronnen beheren. Zij niet rechtstreeks van invloed op verbinding maken met de instanties.
- *Gegevens vlak* (toepassing) beschrijving van de uitvoering van de toepassing zelf en interactie met de instanties die niet door de API Azure omvat. Toegang tot uw website of bepaalde gegevens uit een actief exemplaar van SQL Server of een server MongoDB zouden worden beschouwd als gegevens interactie luchtvervoer of toepassing. Ook worden gegevens vlak een blob kopiëren vanuit een account voor de opslag en toegang tot een openbaar IP-adres RDP of SSH in de virtuele machine. Deze bewerkingen houden de toepassing uitvoert via de compute-, netwerk- en.

>[AZURE.NOTE] In sommige gevallen migreren de Azure platform stopt deallocates en uw virtuele machines opnieuw wordt opgestart. Dit resulteert in een korte data-plane uitvaltijd.

## <a name="supported-scopes-of-migration"></a>Ondersteunde bereiken van migratie

Er zijn drie migratie van scopes die voornamelijk gericht zijn op compute, netwerk en opslag. 

### <a name="migration-of-virtual-machines-not-in-a-virtual-network"></a>Migratie van virtuele machines (niet in een virtueel netwerk)

Beveiliging is in het implementatiemodel Resource Manager standaard afgedwongen voor uw toepassingen. Alle VMs moeten worden in een virtueel netwerk in het model Resource Manager. De Azure platform opnieuw wordt opgestart (`Stop`, `Deallocate`, en `Start`) de VMs als onderdeel van de migratie. U hebt twee opties voor virtuele netwerken:

- U kunt het platform voor het maken van een nieuw virtueel netwerk en de virtuele machine te migreren naar de nieuwe virtuele netwerk op te vragen.
- U kunt de virtuele machine in een virtueel netwerk in Resource Manager migreren.

>[AZURE.NOTE] In dit bereik migratie zowel het operations management-plane en data-plane bewerkingen mogelijk niet toegestaan voor een periode tijdens de migratie.

### <a name="migration-of-virtual-machines-in-a-virtual-network"></a>Migratie van virtuele machines (in een virtueel netwerk)

Alleen de metagegevens is voor de meeste configuraties VM migratie tussen de klassieke en Resource Manager implementatiemodellen. De onderliggende VMs uitvoert op dezelfde hardware op hetzelfde netwerk en met dezelfde opslag. Het operations management vlak mogelijk niet toegestaan gedurende een bepaalde tijd tijdens de migratie. Het vlak van de gegevens echter nog steeds werken. Dat wil zeggen, de toepassingen die worden uitgevoerd op VMs (klassiek) doen niet in rekening gebracht uitvaltijd tijdens de migratie.

De volgende configuraties worden momenteel niet ondersteund. Als er is ondersteuning toegevoegd in de toekomst een VMs in deze configuratie uitvaltijd kunt oplopen (deallocate via stoppen en opnieuw starten VM bewerkingen gaan).

-   U hebt meer dan één beschikbaarheid in een enkele cloud-service ingesteld.
-   U hebt een of meer sets van beschikbaarheid en VMs die niet in de beschikbaarheid in een enkele cloud-service ingesteld.

>[AZURE.NOTE] In dit bereik migratie zijn het management vlak niet toegestaan voor een periode tijdens de migratie. Voor bepaalde configuraties zoals eerder beschreven, data-plane uitval optreedt.

### <a name="storage-accounts-migration"></a>Opslag accounts migreren

Zodat de naadloze migratie kunt u Resource Manager VMs implementeren in een klassieke opslag-account. Met deze mogelijkheid, berekenen en netwerkbronnen kunnen en onafhankelijk van de opslag rekeningen moeten worden gemigreerd. Nadat u via de virtuele Machines en het virtuele netwerk migreert, moet u via uw opslag rekeningen voor het voltooien van het migratieproces worden gemigreerd. 

>[AZURE.NOTE] Het implementatiemodel Resource Manager heeft geen concept van klassieke beelden en schijven. Wanneer de opslag account wordt gemigreerd, klassieke beelden en schijven zijn niet zichtbaar in de stack Resource Manager, maar de back-ups van de VHD's blijven in de opslag. 

## <a name="unsupported-features-and-configurations"></a>Niet-ondersteunde functies en configuraties

We ondersteunen op dit moment geen bepaalde functies en configuraties. De volgende secties worden onze aanbevelingen omheen.

### <a name="unsupported-features"></a>Niet-ondersteunde functies

De volgende functies worden momenteel niet ondersteund. U kunt deze instellingen desgewenst verwijderen, het VMs migreren en de instellingen in het implementatiemodel bronnenbeheerder opnieuw inschakelen.

Bronnen | Functie
---------- | ------------
Berekenen | Schijven bestandsnaamkoppeling virtuele machine.
Berekenen | Afbeeldingen van de virtuele machine.
Netwerk | Eindpunt ACL's.
Netwerk | Virtueel netwerkgateways (site naar site Azure ExpressRoute, toepassingsgateway, wijs site).
Netwerk | Virtuele netwerken met Peering VNet. (VNet migreren naar ARM en klik vervolgens op hetzelfde niveau) Meer informatie over [VNet Peering] (.. /Virtual-Network/Virtual-Network-peering-Overview.MD).
Netwerk | Profielen beheer van verkeer.

### <a name="unsupported-configurations"></a>Niet-ondersteunde configuraties

De volgende configuraties worden momenteel niet ondersteund.

Service | Configuratie | Aanbeveling
---------- | ------------ | ------------
Resource Manager | Rol op basis van Access Control RBAC () voor klassieke bronnen | Omdat de URI van de resources is gewijzigd na de migratie, wordt aangeraden dat u van plan bent de RBAC-beleid updates die moeten gebeuren na de migratie.
Berekenen | Meerdere subnetten die is gekoppeld aan een VM | De subnetconfiguratie om te verwijzen naar alleen subnetten bijwerken.
Berekenen | Virtuele machines die deel uitmaken van een virtueel netwerk, maar niet een expliciete subnet toegewezen | U kunt desgewenst de VM verwijderen.
Berekenen | Virtuele machines die, beleid automatisch schalen waarschuwingen | De migratie wordt doorlopen en deze instellingen worden neergezet. Het is raadzaam dat u uw omgeving evalueren voordat u de migratie. U kunt ook de instellingen voor meldingen configureren nadat de migratie is voltooid.
Berekenen | VM XML-extensies (BGInfo 1.*, Visual Studio Debugger Web implementeren en foutopsporing op afstand) | Dit wordt niet ondersteund. Het wordt aanbevolen dat u deze extensies verwijderen uit de virtuele machine om door te gaan, migratie of deze automatisch tijdens het migratieproces verbroken wordt.
Berekenen | Diagnose opstarten met Premium-opslagruimte | Diagnostisch opstarten-functie voor het VMs uitschakelen voordat u verdergaat met de migratie. Diagnose opstarten in de stapel Resource Manager kunt u opnieuw inschakelen nadat de migratie voltooid is. BLOB's die worden gebruikt voor seriële logboeken en screenshot moeten bovendien worden verwijderd zodat u niet langer worden aangerekend voor deze BLOB's.
Berekenen | Cloud-services met web/werknemer rollen | Dit wordt momenteel niet ondersteund.
Netwerk | Virtuele netwerken met virtuele machines en rollen web/werknemer |  Dit wordt momenteel niet ondersteund.
Azure App-Service | Virtuele netwerken met App Service-omgevingen | Dit wordt momenteel niet ondersteund.
Azure HDInsight | Virtuele netwerken met HDInsight services | Dit wordt momenteel niet ondersteund.
Microsoft Dynamics Lifecycle Services | Virtuele netwerken met virtuele machines die worden beheerd door Dynamics Lifecycle Services | Dit wordt momenteel niet ondersteund.
Berekenen | Azure Security Center-extensies met een VNET met een VPN-gateway of Emergency Recovery-gateway voor prem DNS-server | Azure Security Center installeert automatisch extensies op uw virtuele Machines te controleren hun veiligheid en waarschuwingen te verhogen. Deze extensies krijgen meestal automatisch geïnstalleerd als het beleid Azure Beveiligingscentrum is ingeschakeld op het abonnement. Als gateway migratie wordt momenteel niet ondersteund en de gateway worden verwijderd voordat u doorgaat moet met het doorvoeren van de migratie, wordt de internet-toegang tot account voor VM-opslag verloren als de gateway wordt verwijderd. De migratie niet worden voortgezet wanneer dit gebeurt als de Gast-agent status blob kan niet worden gevuld. Het is raadzaam Azure Security Center om beleid te deactiveren op het abonnement 3 uur voordat u verdergaat met de migratie.

## <a name="the-migration-experience"></a>De migratie-ervaring

Voordat u de migratie-ervaring, wordt het volgende aanbevolen:

- Zorg ervoor dat de bronnen die u wilt migreren niet geen niet-ondersteunde functies of configuraties gebruiken. Het platform wordt meestal deze problemen detecteert en wordt een fout gegenereerd.
- Hebt u een VMs die niet in een virtueel netwerk, wordt deze gestopt en opgeheven als onderdeel van de voorbereidingsbewerking. Als u niet verliezen van het openbare IP-adres wilt, kijkt u in het IP-adres reserveren voor het activeren van de voorbereidingsbewerking. Als het VMs in een virtueel netwerk, zijn ze echter niet gestopt en opgeheven.
- Plan uw migratie tijdens buiten kantooruren voor onverwachte fouten tijdens de migratie gebeuren kunnen.
- De huidige configuratie van uw VMs downloaden via PowerShell, opdrachtregelinterface (CLI) opdrachten of REST API's te vereenvoudigen voor de validatie als de stap voorbereiden voltooid is.
- Werk uw automatisering/uitoefening scripts voor het afhandelen van het implementatiemodel bronnenbeheerder voordat u de migratie start. Desgewenst kunt u doen bewerkingen GET wanneer de resources die voorbereid zijn.
- Evalueer de RBAC-beleidsregels die zijn geconfigureerd op de klassieke middelen voor IaaS en plannen nadat de migratie voltooid is.

De van de Migratiewerkstroom is als volgt

![Schermafdruk van de workflow voor migratie](./media/virtual-machines-windows-migration-classic-resource-manager/migration-workflow.png)

>[AZURE.NOTE] Alle bewerkingen die worden beschreven in de volgende secties worden de idempotency is ingeschakeld. Als er een probleem is dan een niet-ondersteunde functie of een fout in de configuratie, wordt aanbevolen dat u de voorbereiden, opnieuw proberen, of bewerking afgebroken. De Azure platform probeert de actie opnieuw.

### <a name="validate"></a>Valideren

De bewerking valideren is de eerste stap van het migratieproces. Het doel van deze stap is voor het analyseren van gegevens op de achtergrond voor de middelen van de migratie en geslaagde/mislukte als resultaat als de bronnen van migratie kunnen zijn.

U selecteert het virtuele netwerk of de gehoste service (indien niet een virtueel netwerk) dat u wilt valideren voor migratie.

* Als de resource niet in staat van de migratie is, worden de Azure platform alle redenen waarom niet ondersteund voor migratie.

### <a name="prepare"></a>Voorbereiden

De voorbereidingsbewerking is de tweede stap van het migratieproces. Het doel van deze stap is de transformatie van de IaaS bronnen van klassiek tot Resource Manager bronnen te simuleren en presenteren deze naast elkaar voor u visualiseren.

U selecteert het virtuele netwerk of de gehoste service (indien niet een virtueel netwerk) dat u wilt voorbereiden voor migratie.

* Als de resource niet in staat van de migratie is, de Azure platform stopt het migratieproces lijsten en de reden waarom de voorbereiden is mislukt.
* Als de resource kan migreren, omlaag de eerste vergrendelingen Azure platform de bewerkingen beheer vlak voor de middelen van de migratie. Bijvoorbeeld, kan u geen gegevensschijf toevoegen aan een VM onder migratie.

De Azure platform vervolgens begint de migratie van metagegevens van klassiek tot Resource Manager voor de resources migreren.

Nadat de voorbereidingsbewerking voltooid is, hebt u de mogelijkheid van de resources in beide klassieke visualiseren en Resource Manager. Voor elke service cloud in het implementatiemodel klassiek, de Azure platform maakt de naam van een resource-groep die het patroon heeft `cloud-service-name>-migrated`.

>[AZURE.NOTE] Virtuele Machines die niet in een virtueel netwerk voor klassieke worden deallocated in deze fase van de migratie gestopt.

### <a name="check-manual-or-scripted"></a>Check (handmatig of script)

In de stap selectievakje kunt desgewenst u de configuratie die u eerder hebt gedownload om te valideren dat de migratie er goed uitziet. Ook kunt u aanmelden op de portal en controle eigenschappen en bronnen om te valideren dat migratie metagegevens er goed uitziet.

Als u van een virtueel netwerk migreert, de meeste configuratie van virtuele machines niet opnieuw wordt opgestart. U kunt toepassingen op deze VMs valideren dat de toepassing nog steeds actief is.

U kunt uw controle/automatisering en operationele scripts te zien als de VMs werkt zoals verwacht en uw bijgewerkte scripts goed testen. Alleen ophalen bewerkingen worden ondersteund wanneer de resources die voorbereid zijn.

Er is geen tijdvenster waarvoor u moet doorvoeren van de migratie. U kunt zo lang als u wilt dat in deze toestand op te nemen. Het vlak van het management is echter voor deze bronnen vergrendeld totdat u ofwel doorgevoerd of afgebroken.

Als er problemen zijn, kunt u altijd de migratie annuleren en terug te keren naar de klassieke implementatiemodel. Nadat u terug open de Azure platform het operations management vlak op de middelen die u normaal gesproken op de VMs in het implementatiemodel klassiek kunt hervatten.

### <a name="abort"></a>Afbreken

Afbreken is een optionele stap kunt u uw wijzigingen in de klassieke implementatiemodel herstellen en stoppen van de migratie.

>[AZURE.NOTE] Deze bewerking kan niet worden uitgevoerd nadat u de doorvoerbewerking hebt geactiveerd.  

### <a name="commit"></a>Doorvoeren

Nadat de validatie is voltooid, kunt u de migratie toezeggen. Resources worden niet meer weergegeven in de klassieke en zijn alleen beschikbaar in het implementatiemodel Resource Manager. Gemigreerde bronnen kunnen alleen in de nieuwe portal worden beheerd.

>[AZURE.NOTE] Dit is een bewerking van idempotency is ingeschakeld. Als dat niet werkt, is het raadzaam dat u de bewerking opnieuw. Als het blijft mislukken, maakt u een support ticket of een forumbericht maken met een ClassicIaaSMigration-code op onze [VM-forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows).

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Beïnvloedt dit migratieplan van mijn bestaande services of toepassingen die worden uitgevoerd op Azure virtuele machines?**

Nr. Het VMs (klassiek) zijn volledig ondersteunde services in de algemene beschikbaarheid. U kunt doorgaan met deze bronnen gebruiken om uit te breiden de kooldioxide op Microsoft Azure.

**Wat gebeurt er met mijn VMs als ik niet wilt migreren in de nabije toekomst?**

We zijn niet de bestaande klassieke API's en een model met bestandstypen. Willen we dat migratie gemakkelijker te nemen van de geavanceerde functies die beschikbaar in het implementatiemodel Resource Manager zijn. We raden u [enkele van de verbeteringen](virtual-machines-windows-compare-deployment-models.md) die deel uitmaken van de IaaS onder bronbeheer bekijken.

**Wat betekent deze migratieplan voor mijn bestaande tooling?**

De tooling bijwerken naar het implementatiemodel Resource Manager is een van de belangrijkste wijzigingen die u hebt voor het plannen van uw migratie.

**Hoe lang de uitvaltijd management vlak worden?**

Dit is afhankelijk van het aantal bronnen dat wordt gemigreerd. Voor kleinere implementaties (enkele tientallen VMs), moet de gehele migratie minder dan een uur duren. Voor grootschalige implementaties (honderden VMs), kan de migratie enkele uren duren.

**Kan ik samenvouwen nadat mijn resources migreren zijn doorgevoerd in Resource Manager?**

Als de resources in de voorbereide staat zijn, kunt u de migratie afbreken. Terugdraaien wordt niet ondersteund als de bronnen hebt gemigreerd door de doorvoerbewerking.

**Ik heb altijd weer kunt terugdraaien mijn migratie als de doorvoerbewerking niet?**

U kan geen migratie afbreken als de doorvoerbewerking niet. Alle migratiebewerkingen, met inbegrip van de doorvoerbewerking zijn idempotency is ingeschakeld. Daarom wordt aangeraden dat u de bewerking opnieuw na korte tijd. Als u nog steeds een fout wordt geconfronteerd, maken een support ticket of een forumbericht maken met de ClassicIaaSMigration-code op onze [VM-forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows).

**Heb ik een andere route express circuit kopen als ik IaaS onder bronbeheer gebruiken?**

Nr. Laatst ingeschakelde [ExpressRoute circuits van het klassieke aan het implementatiemodel Resource Manager te verplaatsen](../expressroute/expressroute-move.md). U hoeft niet te kopen van een nieuwe ExpressRoute circuit als er al een.

**Ik had wat als Role-Based Access Control-beleid geconfigureerd voor mijn klassieke IaaS resources?**

Tijdens de migratie Transformeren de resources van klassiek tot Resource Manager. Daarom wordt aangeraden dat u van plan bent de RBAC-beleid updates die moeten gebeuren na de migratie.

**Wat gebeurt er als ik gebruik Azure Site herstel- of Azure vandaag?**

De migratie van uw virtuele Machine die beschikbaar zijn voor back-up, Zie [ik een reservekopie hebt mijn klassieke VMs in back-kluis. Ik wil mijn VMs migreren van de klassieke modus naar de Resource Manager-modus. Hoe kan ik back-up ze in recovery services kluis?](../backup/backup-azure-backup-ibiza-faq.md#i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-to-migrate-my-vms-from-classic-mode-to-resource-manager-mode-how-can-i-backup-them-in-recovery-services-vault)

**Kan ik mijn abonnement of resources te zien als ze migratie kan zijn valideren?**

Ja. In de migratieoptie platform ondersteund is de eerste stap in de voorbereiding op de migratie te valideren dat de bronnen van migratie kunnen zijn. In het geval het valideren is mislukt, kunt u berichten ontvangen om de redenen die de migratie kan niet worden voltooid.

**Wat gebeurt er als ik in een quotum-fout tijdens het voorbereiden van de resources IaaS voor migratie uitvoeren?**

U wordt aangeraden uw migratie afbreken en en meld u vervolgens een verzoek om ondersteuning te verhogen van de quota in de regio waar u het VMs migreert. Nadat de quota-aanvraag is goedgekeurd, kunt u beginnen met het uitvoeren van de stappen van de migratie opnieuw.

**Hoe meld ik een probleem?**

Uw problemen en vragen over migratie naar onze [VM-forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows)met het trefwoord ClassicIaaSMigration boeken. Het is raadzaam alle uw vragen posten op dit forum. Als u een ondersteuningscontract hebt, bent u Welkom aan te melden, een support-ticket.

**Wat gebeurt er als ik niet tevreden bent met de namen van de resources die het platform tijdens de migratie gekozen?**

De resources die u expliciet namen voor in de klassieke implementatiemodel blijven behouden tijdens de migratie. In sommige gevallen, worden nieuwe resources gemaakt. Bijvoorbeeld: een netwerkinterface wordt gemaakt voor elke VM. We ondersteund kunnen de namen van deze nieuwe bronnen gemaakt tijdens de migratie op dit moment niet. Meld uw stemmen voor dit onderdeel van het [Feedbackforum Azure](http://feedback.azure.com).

* *ik krijg een bericht *' VM rapport de algehele status van de agent is niet gereed. Daarom kan kan niet de VM worden gemigreerd. Zorg ervoor dat de Agent VM van algemene agent status gereed melden"* of *"VM bevat extensie waarvan u de Status van de VM niet wordt gerapporteerd. Daarom kan deze VM kan niet worden gemigreerd."***

Dit bericht is ontvangen, wanneer de VM geen uitgaande verbinding met het internet. Uitgaande verbindingen de VM-agent gebruikt de account Azure opslag voor het bijwerken van de status van de agent om de vijf minuten bereiken.


## <a name="next-steps"></a>Volgende stappen
Nu dat u begrijpt dat de migratie van klassieke IaaS bronnen op Resource Manager, kunt u beginnen met het migreren van bronnen.

- [Technische diepe kennismaking op platform ondersteund migratie van klassiek Azure Resource Manager](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
- [Gebruikt u PowerShell IaaS bronnen migreren van klassiek Azure Resource Manager](virtual-machines-windows-ps-migration-classic-resource-manager.md)
- [CLI IaaS bronnen migreren van klassiek Azure Resource Manager gebruiken](virtual-machines-linux-cli-migration-classic-resource-manager.md)
- [Klonen van een klassieke virtuele machine Azure Resource Manager met behulp van communautaire PowerShell scripts](virtual-machines-windows-migration-scripts.md)
