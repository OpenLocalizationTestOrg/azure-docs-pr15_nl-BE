In dit artikel worden een aantal beproefde procedures voor het uitvoeren van een Linux virtuele machine (VM) op Azure, met aandacht voor schaalbaarheid, beschikbaarheid, beheerbaarheid en beveiliging. Azure ondersteunt de uitvoering van diverse populaire Linux-distributies, met inbegrip van CentOS, Debian, Ubuntu, Red Hat Enterprise en FreeBSD. Zie voor meer informatie, [Azure en Linux][azure-linux].

> [AZURE.NOTE] Azure heeft twee verschillende modellen: [Resource Manager] [ resource-manager-overview] en klassiek. In dit artikel wordt een resourcemanager, wordt aangeraden voor nieuwe implementaties.

Gebruik aanbevolen niet een enkele VM voor productie werkbelasting, omdat er geen gebruikstijd serviceovereenkomst (SLA) voor één VMs op Azure. Als u de SLA, moet u implementeren meerdere VMs in een [beschikbaar][availability-set]. Zie voor meer informatie, [waarop meerdere VMs op Azure][multi-vm]. 

## <a name="architecture-diagram"></a>Architectuurdiagram

Meer bewegende delen dan alleen de VM zelf heeft betrekking op een VM in Azure inrichten. Er zijn compute-, netwerk- en elementen die u moet overwegen.

> Een Visio-document met dit Architectuurdiagram is beschikbaar op het [Microsoft download center][visio-download]. Dit diagram is op de "Compute - één VM" pagina.

![[0]][0]

- **Resourcegroep.** Een [_resourcegroep_] [ resource-manager-overview] is een container met verwante bronnen. Maak een groep waarin de middelen voor deze VM.

- **VM**. U kunt een VM uit een lijst met gepubliceerde afbeeldingen of een virtuele harde schijf (VHD)-bestand dat u naar Azure blob-opslag uploadt inrichten.

- **OS schijf.** De OS-schijf is een VHD opgeslagen in [Azure opslag][azure-storage]. Dit betekent dat zelfs als de hostcomputer uitvalt blijft bestaan. De OS-schijf is `/dev/sda1`.

- **Tijdelijke schijf.** De VM wordt met een tijdelijke schijf gemaakt. Deze schijf is opgeslagen op een fysieke schijf op de hostcomputer. Is het _niet_ opgeslagen in Azure opslag en tijdens het opnieuw opstarten en andere gebeurtenissen van de levenscyclus van VM kan verdwijnen. Deze schijf alleen voor tijdelijke gegevens, zoals de pagina of swap-bestanden gebruiken. De tijdelijke schijf is `/dev/sdb1` en is gekoppeld aan `/mnt/resource` of `/mnt`.

- **Gegevensschijven.** Een [schijf met gegevens] [ data-disk] is een permanente VHD gebruikt voor toepassingsgegevens. Gegevensschijven worden opgeslagen in Azure opslag, net als de OS-schijf.

- **Virtueel netwerk (VNet) en een subnetmasker.** Elke VM in Azure is geïmplementeerd in een (VNet), die is verder onderverdeeld in subnetten.

- **Openbare IP-adres.** Een openbaar IP-adres is nodig om te communiceren met de VM&mdash;bijvoorbeeld via SSH.

- **Network interface (NIC)**. De Netwerkkaart kan de VM te communiceren met het virtuele netwerk.

- **Network security group (NSG)**. De [NSG] [ nsg] wordt gebruikt voor het toestaan/weigeren netwerkverkeer naar het subnet. U kunt een NSG koppelen met een afzonderlijke NIC of een subnet. Als u deze aan een subnet koppelen, zijn de NSG-regels van toepassing op alle VMs in dat subnet.
 
- **Diagnostische gegevens.** Diagnostische gegevens vastleggen is van cruciaal belang voor het beheren en het oplossen van de VM.

## <a name="recommendations"></a>Aanbevelingen

Azure biedt veel verschillende bronnen en brontypen, zodat deze referentiearchitectuur kan worden ingericht veel verschillende manieren. We hebben een sjabloon Azure Resource Manager wilt installeren de referentiearchitectuur die volgt op deze aanbevelingen verstrekt. Als u kiest voor het maken van uw eigen referentiearchitectuur Volg deze aanbevelingen tenzij u een specifieke vereiste is dat een aanbeveling niet past. 

### <a name="vm-recommendations"></a>VM-aanbevelingen

Het is raadzaam de Active Directory - en GS-serie omdat deze machine formaten [Premium opslag ondersteunt][premium-storage]. Selecteer een van deze formaten computer tenzij u beschikt over een gespecialiseerde werkbelasting zoals high performance computing. Zie voor details de [grootte van de virtuele machine][virtual-machine-sizes]. Als u een bestaande werklast naar Azure, beginnen met de VM-grootte die het meest overeenkomt met uw lokale servers. Meet vervolgens de prestaties van de reële werklast voor CPU, geheugen en schijfruimte invoer/uitvoer-bewerkingen per seconde (IOP's) en de grootte aanpassen indien nodig. Als u meerdere netwerkkaarten, wordt u op de hoogte van de maximale NIC voor elke grootte.  

Bij het inrichten van de VM en andere bronnen, moet u een locatie opgeven. In het algemeen, kies een locatie die het dichtst bij uw interne gebruikers of klanten. Echter niet alle VM maten zijn mogelijk beschikbaar op alle locaties. Voor meer informatie, Zie [Services per regio][services-by-region]. Als u de grootte VM beschikbaar op een bepaalde locatie, kunt u de volgende Azure opdrachtregelinterface (CLI)-opdracht uitvoeren:

```
    azure vm sizes --location <location>
```

Zie voor meer informatie over het kiezen van een gepubliceerde VM-afbeelding [navigeren en afbeeldingen selecteren Azure VM][select-vm-image].

### <a name="disk-and-storage-recommendations"></a>Schijf- en aanbevelingen

Voor de beste schijfprestaties I/O, is raadzaam [Opslag Premium][premium-storage], waarin gegevens worden opgeslagen op solid-state schijven (SSD). Kosten is gebaseerd op de grootte van de schijf ingericht. IOP's en de doorvoer (dat wil zeggen gegevensoverdrachtsnelheid) ook afhankelijk zijn op de grootte van de schijf, dus wanneer u een schijf inrichten, kunt u alle drie factoren (capaciteit IOP's en doorvoer). 

Één account voor opslag ondersteunt VMs 1 tot en met 20.

Gegevensschijven voor een of meer toevoegen. Wanneer u een VHD maakt, is niet geformatteerd. Log in op de VM de schijf formatteren. De gegevensschijven weergeven als `/dev/sdc`, `/dev/sdd`, enzovoort. U kunt uitvoeren `lsblk` om het blokapparaten, met inbegrip van de clusterschijven. Maak een partitie en een nieuw bestandssysteem voor het gebruik van een schijf met gegevens en de schijf koppelen. Bijvoorbeeld:

```bat
    # Create a partition.
    sudo fdisk /dev/sdc     # Enter 'n' to partition, 'w' to write the change.     
    
    # Create a file system.
    sudo mkfs -t ext3 /dev/sdc1
    
    # Mount the drive.
    sudo mkdir /data1
    sudo mount /dev/sdc1 /data1
```

Hebt u veel gegevensschijven, worden op de hoogte van de totale i/o-beperkingen van de rekening voor opslag. Zie [Virtuele machines toewijzen]voor meer informatie,[vm-disk-limits].

Als u een schijf met gegevens toevoegt, wordt een logische eenheid (LUN) ID-nummer op de schijf toegewezen. U kunt desgewenst het LUN-ID &mdash; , bijvoorbeeld als u een schijf wilt vervangen en behoudt dezelfde LUN-ID wilt of als u een app die naar een specifieke LUN zoekt. Vergeet echter niet dat LUN-id uniek zijn voor elke schijf moet.

U wilt wijzigen van het i/o-scheduler, optimaliseren voor prestaties op solid-state schijven (SSD) (gebruikt door premie opslag). Een algemene aanbeveling is het gebruik van de scheduler Nooperation voor SSD, maar u kunt een hulpprogramma zoals [iostat] schijf-i/o-prestaties controleren voor uw specifieke werkbelasting.

- Voor de beste prestaties door een aparte opslag account bevatten diagnostische logboeken te maken. Een standaardaccount lokaal redundante opslag (LRS) is voldoende voor diagnostische logboeken.


### <a name="network-recommendations"></a>Aanbevelingen voor netwerken

Het openbare IP-adres kan dynamisch of statisch zijn. De standaardwaarde is dynamisch.

- Een [statisch IP-adres] reserveren[ static-ip] als u een vast IP-adres die niet verandert moet &mdash; , bijvoorbeeld als u nodig hebt voor het maken van een A-record in DNS of het IP-adres moet worden whitelisted.

- U kunt ook een FQDN-naam (Fully Qualified Domain Name) voor het IP-adres maken. Vervolgens kunt u een [CNAME-record] registreren[ cname-record] in DNS die naar de FQDN-naam verwijst. Zie voor meer informatie, [maken een volledig gekwalificeerde domeinnaam in Azure portal][fqdn].

Alle NSGs bevatten een aantal [standaardregels][nsg-default-rules], met inbegrip van een regel die alle binnenkomende Internet-verkeer wordt geblokkeerd. De standaard-regels kunnen niet worden verwijderd, maar andere regels negeren. Als u wilt dat Internet-verkeer, regels maken die binnenkomend verkeer naar bepaalde poorten toestaan &mdash; bijvoorbeeld poort 80 voor HTTP.  

Om te schakelen SSH, moet u een regel toevoegen aan de NSG waarmee binnenkomend verkeer op TCP-poort 22.

## <a name="scalability-considerations"></a>Overwegingen voor schaalbaarheid

U kunt de schaal een VM omhoog of omlaag door [de grootte van de VM][vm-resize]. 

Voor het horizontaal schalen van twee of meer VMs in een beschikbaarheid van achter een taakverdeling instellen te plaatsen. Zie voor meer informatie [meerdere VMs op Azure uitgevoerd][multi-vm].

## <a name="availability-considerations"></a>Overwegingen met betrekking tot beschikbaarheid

Zoals eerder vermeld, is er geen SLA voor een enkele VM. Als u de SLA, moet u meerdere VMs implementeren in een set van beschikbaarheid.

Uw VM kan worden beïnvloed door het [geplande onderhoud] [ planned-maintenance] of [ongepland onderhoud][manage-vm-availability]. U kunt [VM Logboeken opnieuw] [ reboot-logs] om te bepalen of een VM opstarten is veroorzaakt door planmatig onderhoud.

VHD's worden ondersteund door [Azure opslag][azure-storage], die wordt gerepliceerd voor duurzaamheid en beschikbaarheid.

Om u te beschermen tegen onbedoeld gegevensverlies tijdens normale bewerkingen (bijvoorbeeld door een gebruikersfout), moet u point-in-time back-ups, [blob momentopnamen] ook implementeren[ blob-snapshot] of een ander programma.

## <a name="manageability-considerations"></a>Overwegingen bij het beheer

**Resourcegroepen.** Strak gekoppelde bronnen die de levenscyclus van dezelfde in dezelfde [resourcegroep]delen plaatsen[resource-manager-overview]. Resourcegroepen kunnen u implementeren en controleren van bronnen als een groep en facturering van de kosten per resourcegroep samenvouwen. U kunt ook resources verwijderen als een set wat zeer handig voor test-implementaties is. Resources betekenisvolle namen geven. Die gemakkelijker te vinden een bepaalde resource en begrijpen van de rol. [Aanbevolen voor naamgeving van Azure bronnen]Zie[naming conventions].

**SSH**. Voordat u een Linux VM maakt, genereert een paar openbare persoonlijke sleutels van 2048-bit RSA. Het bestand met openbare sleutel gebruiken bij het maken van de VM. Voor meer informatie, Zie [Gebruik SSH met Linux en Mac op Azure][ssh-linux].

**Diagnostische gegevens van de VM.** Controle inschakelen en diagnostische gegevens, met inbegrip van de gezondheid fundamentele parameters, diagnostiek infrastructuur logboeken en [boot diagnostische gegevens][boot-diagnostics]. Diagnostisch opstarten kunt u opstartfout diagnose als uw VM in een niet-opstartbare staat haalt. Zie voor meer informatie [en de diagnostische gegevens][enable-monitoring].  

De volgende opdracht in de CLI kunt diagnostische gegevens:

```
    azure vm enable-diag <resource-group> <vm-name>
```

**Het stoppen van een VM.** Azure een onderscheid maakt tussen lidstaten "Gestopt" en "Deallocated". U in rekening worden gebracht wanneer de VM-status 'gestopt'. U bent niet in rekening gebracht wanneer de VM opgeheven.

Gebruik de volgende opdracht CLI toewijzing een VM:

```
    azure vm deallocate <resource-group> <vm-name>
```

- De knop **stoppen** in Azure portal deallocates ook de VM. Echter als u met het besturingssysteem afgesloten terwijl u bent aangemeld, wordt de VM gestopt maar _niet_ ongedaan, zodat u nog steeds brengt.

**Het verwijderen van een VM.** Als u een VM verwijdert, worden de VHD's niet verwijderd. Dit betekent dat u de VM veilig kunt verwijderen zonder dat er gegevens verloren gaan. Echter nog steeds brengt voor opslag. Als u wilt de VHD verwijdert, kunt u het bestand verwijderen uit de [blob-opslag][blob-storage].

Gebruik een [resource vergrendelen] om te voorkomen dat per ongeluk wordt verwijderd,[ resource-lock] de hele resource group of lock afzonderlijke bronnen, zoals de VM te vergrendelen. 

## <a name="security-considerations"></a>Beveiligingsoverwegingen

OS updates automatiseren met behulp van de extensie [OSPatching] VM. Deze extensie installeren wanneer u de VM inrichten. U kunt opgeven hoe vaak u wilt installeren van patches en of op te starten na het patchen.

Gebruik [toegangsbeheer op rollen gebaseerde] [ rbac] RBAC () toegang tot de Azure bronnen die u implementeert. RBAC kunt u autorisatie rollen toewijzen aan leden van uw team DevOps. Bijvoorbeeld, kunt de rol van lezer Azure bronnen bekijken, maar niet maken, beheren of verwijderen. Sommige functies zijn specifiek voor bepaalde Azure brontypen. Bijvoorbeeld kunt de rol van inzender voor virtuele Machine starten of een VM deallocate, het administrator-wachtwoord opnieuw instellen, een VM maken, enzovoort. Andere [ingebouwde RBAC-rollen] [ rbac-roles] dat kan handig zijn voor deze referentiearchitectuur omvatten [DevTest Labs gebruiker] [ rbac-devtest] en [Inzender netwerk][rbac-network]. 

Een gebruiker kan aan meerdere rollen worden toegewezen en u kunt aangepaste rollen maken voor nog meer toegesneden machtigingen.

> [AZURE.NOTE] RBAC kent geen beperking voor de acties die een gebruiker aangemeld bij een VM kan uitvoeren. Deze machtigingen worden bepaald door het rekeningtype op de Gast OS.   

Gebruik de [controlelogboekbestanden] [ audit-logs] voor een overzicht van acties en andere evenementen VM wordt ingericht.

- [Azure schijf codering] kunt u[ disk-encryption] als u de schijven van OS en gegevens te coderen. 

## <a name="solution-deployment"></a>Implementatie van oplossingen

De [implementatie] van een[ github-folder] architectuur waarop u deze best practices voor een verwijzing beschikbaar is. Deze referentiearchitectuur omvat een virtueel netwerk (VNet), network security group (NSG) en één virtuele machine (VM).

Er zijn verschillende manieren om deze referentiearchitectuur implementeren. De eenvoudigste manier is om de volgende stappen uit: 

1. Rechts Klik op de onderstaande knop en selecteer een "Open link in nieuw tabblad" of "Open link in nieuw venster".
[![Implementeren op Azure](../articles/guidance/media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-single-vm%2Fazuredeploy.json)

2. Nadat de koppeling is geopend in de portal Azure, moet u waarden opgeven voor enkele van de instellingen: 
    - De naam van de **Resource** is al gedefinieerd in de parameterbestand, dus **Nieuw** te selecteren en voer `ra-single-vm-rg` in het tekstvak.
    - Selecteer de regio in de vervolgkeuzelijst **locatie** .
    - Bewerk de **Sjabloon basis-Uri** of de **Parameter basis-Uri** tekstvakken niet.
    - Het **Type besturingssysteem** selecteren in de vervolgkeuzelijst **linux**.
    - Bekijk de voorwaarden en bepalingen en klik op het selectievakje **dat ik ga akkoord met bovenstaande voorwaarden** .
    - Klik op de button **aankoop** .

3. Wacht totdat de installatie te voltooien.

4. De parameterbestanden bevatten een hard gecodeerde gebruikersnaam en wachtwoord en is het raadzaam dat u onmiddellijk beide wijzigen. Klik op de VM met de naam `ra-single-vm0 `in de Portal Azure. Klik op **wachtwoord opnieuw instellen** in het gedeelte **Support + het oplossen van problemen** . Selecteer **wachtwoord opnieuw instellen** in het dropdown **-modus** en selecteer vervolgens een nieuwe **gebruikersnaam** en **wachtwoord**. Klik op de knop **bijwerken** om de nieuwe gebruikersnaam en wachtwoord te behouden.

Zie voor informatie over manieren waarop u deze referentiearchitectuur implementeren, het Leesmij-bestand in de [richtlijnen voor-één-vm] [ github-folder] map Github.

## <a name="customize-the-deployment"></a>De installatie aanpassen

Als u wilt wijzigen de implementatie aan uw behoeften, volg de instructies in de [richtlijnen voor-één-vm] [ github-folder] pagina.

## <a name="next-steps"></a>Volgende stappen

Voor de [SLA voor virtuele Machines] [ vm-sla] wilt toepassen, moet u twee of meer exemplaren in een Set van beschikbaarheid implementeren. Zie voor meer informatie, [waarop meerdere VMs op Azure][multi-vm].

<!-- links -->

[audit-logs]: https://azure.microsoft.com/en-us/blog/analyze-azure-audit-logs-in-powerbi-more/
[availability-set]: ../articles/virtual-machines/virtual-machines-windows-create-availability-set.md
[azure-cli]: ../articles/virtual-machines-command-line-tools.md
[azure-linux]: ../articles/virtual-machines/virtual-machines-linux-azure-overview.md
[azure-storage]: ../articles/storage/storage-introduction.md
[blob-snapshot]: ../articles/storage/storage-blob-snapshots.md
[blob-storage]: ../articles/storage/storage-introduction.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[cname-record]: https://en.wikipedia.org/wiki/CNAME_record
[data-disk]: ../articles/virtual-machines/virtual-machines-linux-about-disks-vhds.md
[disk-encryption]: ../articles/security/azure-security-disk-encryption.md
[enable-monitoring]: ../articles/monitoring-and-diagnostics/insights-how-to-use-diagnostics.md
[fqdn]: ../articles/virtual-machines/virtual-machines-linux-portal-create-fqdn.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-single-vm/
[iostat]: https://en.wikipedia.org/wiki/Iostat
[manage-vm-availability]: ../articles/virtual-machines/virtual-machines-linux-manage-availability.md
[multi-vm]: ../articles/guidance/guidance-compute-multi-vm.md
[naming conventions]: ../articles/guidance/guidance-naming-conventions.md
[nsg]: ../articles/virtual-network/virtual-networks-nsg.md
[nsg-default-rules]: ../articles/virtual-network/virtual-networks-nsg.md#default-rules
[OSPatching]: https://github.com/Azure/azure-linux-extensions/tree/master/OSPatching
[planned-maintenance]: ../articles/virtual-machines/virtual-machines-linux-planned-maintenance.md
[premium-storage]: ../articles/storage/storage-premium-storage.md
[rbac]: ../articles/active-directory/role-based-access-control-what-is.md
[rbac-roles]: ../articles/active-directory/role-based-access-built-in-roles.md
[rbac-devtest]: ../articles/active-directory/role-based-access-built-in-roles.md#devtest-lab-user
[rbac-network]: ../articles/active-directory/role-based-access-built-in-roles.md#network-contributor
[reboot-logs]: https://azure.microsoft.com/en-us/blog/viewing-vm-reboot-logs/
[Resize-VHD]: https://technet.microsoft.com/en-us/library/hh848535.aspx
[Resize virtual machines]: https://azure.microsoft.com/en-us/blog/resize-virtual-machines/
[resource-lock]: ../articles/resource-group-lock-resources.md
[resource-manager-overview]: ../articles/azure-resource-manager/resource-group-overview.md
[select-vm-image]: ../articles/virtual-machines/virtual-machines-linux-cli-ps-findimage.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[ssh-linux]: ../articles/virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md
[static-ip]: ../articles/virtual-network/virtual-networks-reserved-public-ip.md
[storage-price]: https://azure.microsoft.com/pricing/details/storage/
[virtual-machine-sizes]: ../articles/virtual-machines/virtual-machines-linux-sizes.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../articles/azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-resize]: ../articles/virtual-machines/virtual-machines-linux-change-vm-size.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_0/
[readme]: https://github.com/mspnp/reference-architectures/blob/master/guidance-compute-single-vm
[components]: #Solution-components
[blocks]: https://github.com/mspnp/template-building-blocks
[0]: ./media/guidance-blueprints/compute-single-vm.png "Architectuur van één Linux VM in Azure"

