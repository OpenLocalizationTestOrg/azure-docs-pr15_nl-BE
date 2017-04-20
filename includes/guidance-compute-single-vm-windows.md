In dit artikel worden een aantal beproefde procedures voor het uitvoeren van een Windows VM (virtual machine) op Azure, met aandacht voor schaalbaarheid, beschikbaarheid, beheerbaarheid en beveiliging. 

> [AZURE.NOTE] Azure heeft twee verschillende modellen: [Azure Resource Manager] [ resource-manager-overview] en klassiek. In dit artikel wordt een resourcemanager, wordt aangeraden voor nieuwe implementaties.

Gebruik aanbevolen niet een enkele VM voor productie werkbelasting, omdat er geen gebruikstijd serviceovereenkomst (SLA) voor één VMs op Azure. Als u de SLA, moet u implementeren meerdere VMs in een [beschikbaar][availability-set]. Zie voor meer informatie, [waarop meerdere Windows VMs op Azure][multi-vm]. 

## <a name="architecture-diagram"></a>Architectuurdiagram

Meer bewegende delen dan alleen de VM zelf heeft betrekking op een VM in Azure inrichten. Er zijn elementen compute, netwerken en opslag.

> Een Visio-document met dit Architectuurdiagram is beschikbaar op het [Microsoft download center][visio-download]. Dit diagram is op de "Compute - VM pagina.

![[0]][0]


- **Resourcegroep.** Een [_resourcegroep_] [ resource-manager-overview] is een container met verwante bronnen. Maak een groep waarin de middelen voor deze VM.

- **VM**. U kunt een VM uit een lijst met gepubliceerde afbeeldingen of een virtuele harde schijf (VHD)-bestand dat u naar Azure blob-opslag uploadt inrichten.

- **OS schijf.** De OS-schijf is een VHD opgeslagen in [Azure opslag][azure-storage]. Dit betekent dat zelfs als de hostcomputer uitvalt blijft bestaan.

- **Tijdelijke schijf.** De VM met een tijdelijke schijf wordt gemaakt (de `D:` station van Windows). Deze schijf is opgeslagen op een fysieke schijf op de hostcomputer. Is het _niet_ opgeslagen in Azure opslag en tijdens het opnieuw opstarten en andere gebeurtenissen van de levenscyclus van VM kan verdwijnen. Deze schijf alleen voor tijdelijke gegevens, zoals de pagina of swap-bestanden gebruiken.

- **Gegevensschijven.** Een [schijf met gegevens] [ data-disk] is een permanente VHD gebruikt voor toepassingsgegevens. Gegevensschijven worden opgeslagen in Azure opslag, net als de OS-schijf.

- **Virtueel netwerk (VNet) en een subnetmasker.** Elke VM in Azure is geïmplementeerd in een VNet, is verder onderverdeeld in subnetten.

- **Openbare IP-adres.** Een openbaar IP-adres is nodig om te communiceren met de VM&mdash;bijvoorbeeld via Extern bureaublad (RDP).

- **Network interface (NIC)**. De Netwerkkaart kan de VM te communiceren met het virtuele netwerk.

- **Network security group (NSG)**. De [NSG] [ nsg] wordt gebruikt voor het toestaan/weigeren netwerkverkeer naar het subnet. U kunt een NSG koppelen met een afzonderlijke NIC of een subnet. Als u deze aan een subnet koppelen, zijn de NSG-regels van toepassing op alle VMs in dat subnet.
 
- **Diagnostische gegevens.** Diagnostische gegevens vastleggen is van cruciaal belang voor het beheren en het oplossen van de VM.

## <a name="recommendations"></a>Aanbevelingen

Azure biedt veel verschillende bronnen en brontypen, zodat deze referentiearchitectuur kan worden ingericht veel verschillende manieren. We hebben een sjabloon Azure Resource Manager wilt installeren de referentiearchitectuur die volgt op deze aanbevelingen verstrekt. Als u kiest voor het maken van uw eigen referentiearchitectuur Volg deze aanbevelingen tenzij u een specifieke vereiste is dat een aanbeveling niet past.

### <a name="vm-recommendations"></a>VM-aanbevelingen

Het is raadzaam de Active Directory - en GS-serie omdat deze machine formaten [Premium opslag ondersteunt][premium-storage]. Selecteer een van deze formaten computer tenzij u beschikt over een gespecialiseerde werkbelasting zoals high performance computing. Zie voor details de [grootte van de virtuele machine][virtual-machine-sizes]. Als u een bestaande werklast naar Azure, beginnen met de VM-grootte die het meest overeenkomt met uw lokale servers. Vervolgens de prestaties van uw werkelijke werkbelasting van CPU, geheugen en schijfruimte invoer/uitvoer-bewerkingen per seconde (IOP's) en de grootte aanpassen indien nodig. Als u meerdere netwerkkaarten, wordt u op de hoogte van de maximale NIC voor elke grootte.  

Bij het inrichten van de VM en andere bronnen, moet u een locatie opgeven. In het algemeen, kies een locatie die het dichtst bij uw interne gebruikers of klanten. Echter niet alle VM maten zijn mogelijk beschikbaar op alle locaties. Voor meer informatie, Zie [Services per regio][services-by-region]. Als u de grootte VM beschikbaar op een bepaalde locatie, kunt u de volgende Azure opdrachtregelinterface (CLI)-opdracht uitvoeren:

```
    azure vm sizes --location <location>
```

Zie voor meer informatie over het kiezen van een gepubliceerde VM-afbeelding [navigeren en afbeeldingen selecteren Azure VM][select-vm-image].

### <a name="disk-and-storage-recommendations"></a>Schijf- en aanbevelingen

Voor de beste schijfprestaties I/O, is raadzaam [Opslag Premium][premium-storage], waarin gegevens worden opgeslagen op een solid state harde schijven (SSD). Kosten is gebaseerd op de grootte van de schijf ingericht. IOP's en doorvoer ook afhankelijk van de grootte van de schijf, dus wanneer u een schijf inrichten, kunt u alle drie factoren (capaciteit IOP's en doorvoer). 

Één account voor opslag ondersteunt VMs 1 tot en met 20.

Gegevensschijven voor een of meer toevoegen. Wanneer u een nieuwe VHD maakt, is niet geformatteerd. Log in op de VM de schijf formatteren.

Als u een groot aantal gegevensschijven hebt, worden op de hoogte van de totale i/o-beperkingen van de rekening voor opslag. Zie [Virtuele machines toewijzen]voor meer informatie,[vm-disk-limits].

Voor de beste prestaties door een aparte opslag account bevatten diagnostische logboeken te maken. Een standaardaccount lokaal redundante opslag (LRS) is voldoende voor diagnostische logboeken.

Indien mogelijk, kunt u toepassingen installeren op een schijf met gegevens, niet op de OS-schijf. Bij sommige oudere toepassingen moet echter mogelijk om onderdelen te installeren op station C:. In dat geval kunt u de [grootte van de schijf OS] [ resize-os-disk] met PowerShell.

### <a name="network-recommendations"></a>Aanbevelingen voor netwerken

Het openbare IP-adres kan dynamisch of statisch zijn. De standaardwaarde is dynamisch.

- Een [statisch IP-adres] reserveren[ static-ip] als u een vast IP-adres die niet verandert moet &mdash; , bijvoorbeeld als u nodig hebt voor het maken van een A-record in DNS of het IP-adres moet worden whitelisted.

- U kunt ook een FQDN-naam (Fully Qualified Domain Name) voor het IP-adres maken. Vervolgens kunt u een [CNAME-record] registreren[ cname-record] in DNS die naar de FQDN-naam verwijst. Zie voor meer informatie, [maken een volledig gekwalificeerde domeinnaam in Azure portal][fqdn].

Alle NSGs bevatten een aantal [standaardregels][nsg-default-rules], met inbegrip van een regel die alle binnenkomende Internet-verkeer wordt geblokkeerd. De standaard-regels kunnen niet worden verwijderd, maar andere regels negeren. Als u wilt dat Internet-verkeer, regels maken die binnenkomend verkeer naar bepaalde poorten toestaan &mdash; bijvoorbeeld poort 80 voor HTTP.  

RDP inschakelen, als u wilt toevoegen een NSG-regel die toestaat dat binnenkomend verkeer op TCP-poort 3389.

## <a name="scalability-considerations"></a>Overwegingen voor schaalbaarheid

U kunt de schaal een VM omhoog of omlaag door [de grootte van de VM][vm-resize]. 

Voor het horizontaal schalen van twee of meer VMs in een beschikbaarheid van achter een taakverdeling instellen te plaatsen. Zie voor meer informatie [meerdere Windows VMs op Azure uitgevoerd][multi-vm].

## <a name="availability-considerations"></a>Overwegingen met betrekking tot beschikbaarheid

Zoals eerder vermeld, is er geen SLA voor een enkele VM. Als u de SLA, moet u meerdere VMs implementeren in een set van beschikbaarheid.

Uw VM kan worden beïnvloed door het [geplande onderhoud] [ planned-maintenance] of [ongepland onderhoud][manage-vm-availability]. U kunt [VM Logboeken opnieuw] [ reboot-logs] om te bepalen of een VM opstarten is veroorzaakt door planmatig onderhoud.

VHD's worden ondersteund door [Azure opslag][azure-storage], die wordt gerepliceerd voor duurzaamheid en beschikbaarheid.

Om u te beschermen tegen onbedoeld gegevensverlies tijdens normale bewerkingen (bijvoorbeeld door een gebruikersfout), moet u point-in-time back-ups, [blob momentopnamen] ook implementeren[ blob-snapshot] of een ander programma.

## <a name="manageability-considerations"></a>Overwegingen bij het beheer

**Resourcegroepen.** Nauw verbonden bronnen die de levenscyclus van dezelfde in dezelfde [bronnengroep delen]geplaatst[resource-manager-overview]. Resourcegroepen kunnen u implementeren en controleren van bronnen als een groep en facturering van de kosten per resourcegroep samenvouwen. U kunt ook resources verwijderen als een set wat zeer handig voor test-implementaties is. Resources betekenisvolle namen geven. Die gemakkelijker te vinden een bepaalde resource en begrijpen van de rol. [Aanbevolen voor naamgeving van Azure bronnen]Zie[naming conventions].

**Diagnostische gegevens van de VM.** Controle inschakelen en diagnostische gegevens, met inbegrip van de gezondheid fundamentele parameters, diagnostiek infrastructuur logboeken en [boot diagnostische gegevens][boot-diagnostics]. Diagnostisch opstarten kunt u een opstartfout diagnose als uw VM in een niet-opstartbare staat haalt. Zie voor meer informatie [en de diagnostische gegevens][enable-monitoring]. Gebruik de [Azure logboek collectie] [ log-collector] -extensie voor Azure platform logboeken te verzamelen en te uploaden naar Azure opslag.   

De volgende opdracht in de CLI kunt diagnostische gegevens:

```
    azure vm enable-diag <resource-group> <vm-name>
```

**Het stoppen van een VM.** Azure een onderscheid maakt tussen lidstaten "Gestopt" en "Deallocated". U in rekening worden gebracht wanneer de VM-status 'gestopt'. U bent niet in rekening gebracht wanneer de VM opgeheven.

Gebruik de volgende opdracht CLI toewijzing een VM:

```
    azure vm deallocate <resource-group> <vm-name>
```

De knop **stoppen** in Azure portal deallocates ook de VM. Echter als u met het besturingssysteem afgesloten terwijl u bent aangemeld, wordt de VM gestopt maar _niet_ ongedaan, zodat u nog steeds brengt.

**Het verwijderen van een VM.** Als u een VM verwijdert, worden de VHD's niet verwijderd. Dit betekent dat u de VM veilig kunt verwijderen zonder dat er gegevens verloren gaan. Echter nog steeds brengt voor opslag. Als u wilt de VHD verwijdert, kunt u het bestand verwijderen uit de [blob-opslag][blob-storage].

Gebruik een [resource vergrendelen] om te voorkomen dat per ongeluk wordt verwijderd,[ resource-lock] de hele resource group of lock afzonderlijke bronnen, zoals de VM te vergrendelen. 

## <a name="security-considerations"></a>Beveiligingsoverwegingen

Gebruik [Beveiligingscentrum Azure] [ security-center] voor een centrale weergave van de beveiligingsstatus van uw resources Azure. Beveiligingscentrum controleert potentiële beveiligingsproblemen, zoals updates, antimalware, en een gedetailleerd beeld van de gezondheid van de beveiliging van uw implementatie biedt. 

- Security Center is per abonnement Azure geconfigureerd. Verzamelen beveiliging inschakelen, zoals beschreven in [Beveiligingscentrum gebruiken].

- Wanneer het verzamelen van gegevens is ingeschakeld, scant Beveiligingscentrum een VMs gemaakt onder het abonnement automatisch.

**Beheer van beveiligingspatches.** Indien ingeschakeld, wordt nagegaan of beveiligingsupdates en essentiële updates ontbreken. [Instellingen voor Groepsbeleid] gebruiken[ group-policy] op de VM voor automatische systeemupdates inschakelen.

**Antimalware.** Indien ingeschakeld, wordt in Beveiligingscentrum controleert of antimalware software is geïnstalleerd. U kunt ook Security Center antimalware-software binnen de Azure portal te installeren.

Gebruik [toegangsbeheer op rollen gebaseerde] [ rbac] RBAC () toegang tot de Azure bronnen die u implementeert. RBAC kunt u autorisatie rollen toewijzen aan leden van uw team DevOps. Bijvoorbeeld, kunt de rol van lezer Azure bronnen bekijken, maar niet maken, beheren of verwijderen. Sommige functies zijn specifiek voor bepaalde Azure brontypen. Bijvoorbeeld kunt de rol van inzender voor virtuele Machine starten of een VM deallocate, het administrator-wachtwoord opnieuw instellen, maakt een nieuwe VM, enzovoort. Andere [ingebouwde RBAC-rollen] [ rbac-roles] dat kan handig zijn voor deze referentiearchitectuur omvatten [DevTest Labs gebruiker] [ rbac-devtest] en [Inzender netwerk][rbac-network]. Een gebruiker kan aan meerdere rollen worden toegewezen en u kunt aangepaste rollen maken voor nog meer toegesneden machtigingen.

> [AZURE.NOTE] RBAC kent geen beperking voor de acties die een gebruiker aangemeld bij een VM kan uitvoeren. Deze machtigingen worden bepaald door het rekeningtype op de Gast OS.   

Uitvoeren als u de lokale administrator-wachtwoord herstellen, de `vm reset-access` Azure CLI-opdracht.

```
    azure vm reset-access -u <user> -p <new-password> <resource-group> <vm-name>
```

Gebruik de [controlelogboekbestanden] [ audit-logs] voor een overzicht van acties en andere evenementen VM wordt ingericht.

[Azure schijf codering] kunt u[ disk-encryption] als u de schijven van OS en gegevens te coderen. 

## <a name="solution-deployment"></a>Implementatie van oplossingen

De [implementatie] van een[ github-folder] architectuur waarop u deze best practices voor een verwijzing beschikbaar is. Deze referentiearchitectuur omvat een virtueel netwerk (VNet), network security group (NSG) en één virtuele machine (VM).

Er zijn verschillende manieren om deze referentiearchitectuur implementeren. De eenvoudigste manier is om de volgende stappen uit: 

1. Rechts Klik op de onderstaande knop en selecteer een "Open link in nieuw tabblad" of "Open link in nieuw venster".  
[![Implementeren op Azure](../articles/guidance/media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-single-vm%2Fazuredeploy.json)

2. Nadat de koppeling is geopend in de portal Azure, moet u waarden opgeven voor enkele van de instellingen: 
    - De naam van de **Resource** is al gedefinieerd in de parameterbestand, dus **Nieuw** te selecteren en voer `ra-single-vm-rg` in het tekstvak.
    - Selecteer de regio in de vervolgkeuzelijst **locatie** .
    - Bewerk de **Sjabloon basis-Uri** of de **Parameter basis-Uri** tekstvakken niet.
    - Het **Type besturingssysteem** selecteren in de vervolgkeuzelijst **windows**.
    - Bekijk de voorwaarden en bepalingen en klik op het selectievakje **dat ik ga akkoord met bovenstaande voorwaarden** .
    - Klik op de button **aankoop** .

3. Wacht totdat de installatie te voltooien.

4. De parameterbestanden bevatten een hard gecodeerde gebruikersnaam en wachtwoord en is het raadzaam dat u onmiddellijk beide wijzigen. Klik op de VM met de naam `ra-single-vm0 `in de Portal Azure. Klik vervolgens op **wachtwoord opnieuw instellen** in de bladeserver **ondersteuning + het oplossen van problemen** . Selecteer **wachtwoord opnieuw instellen** in het dropdown **-modus** en selecteer vervolgens een nieuwe **gebruikersnaam** en **wachtwoord**. Klik op de knop **bijwerken** om de nieuwe gebruikersnaam en wachtwoord te behouden.

Zie voor informatie over manieren waarop u deze referentiearchitectuur implementeren, het Leesmij-bestand in de [richtlijnen voor-één-vm][github-folder]] map Github. 

## <a name="customize-the-deployment"></a>De installatie aanpassen

Volg de instructies in het [Leesmij-bestand]als u wijzigen de implementatie wilt aan uw behoeften,[github-folder]. 

## <a name="next-steps"></a>Volgende stappen

Voor de [SLA voor virtuele Machines] [ vm-sla] wilt toepassen, moet u twee of meer exemplaren in een set beschikbaarheid implementeren. Zie voor meer informatie, [waarop meerdere VMs op Azure][multi-vm].

<!-- links -->

[audit-logs]: https://azure.microsoft.com/en-us/blog/analyze-azure-audit-logs-in-powerbi-more/
[availability-set]: ../articles/virtual-machines/virtual-machines-windows-create-availability-set.md
[azure-cli]: ../articles/virtual-machines-command-line-tools.md
[azure-storage]: ../articles/storage/storage-introduction.md
[blob-snapshot]: ../articles/storage/storage-blob-snapshots.md
[blob-storage]: ../articles/storage/storage-introduction.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[cname-record]: https://en.wikipedia.org/wiki/CNAME_record
[data-disk]: ../articles/virtual-machines/virtual-machines-windows-about-disks-vhds.md
[disk-encryption]: ../articles/security/azure-security-disk-encryption.md
[enable-monitoring]: ../articles/monitoring-and-diagnostics/insights-how-to-use-diagnostics.md
[fqdn]: ../articles/virtual-machines/virtual-machines-windows-portal-create-fqdn.md
[github-folder]: http://github.com/mspnp/reference-architectures/tree/master/guidance-compute-single-vm
[group-policy]: https://technet.microsoft.com/en-us/library/dn595129.aspx
[log-collector]: https://azure.microsoft.com/en-us/blog/simplifying-virtual-machine-troubleshooting-using-azure-log-collector/
[manage-vm-availability]: ../articles/virtual-machines/virtual-machines-windows-manage-availability.md
[multi-vm]: ../articles/guidance/guidance-compute-multi-vm.md
[naming conventions]: ../articles/guidance/guidance-naming-conventions.md
[nsg]: ../articles/virtual-network/virtual-networks-nsg.md
[nsg-default-rules]: ../articles/virtual-network/virtual-networks-nsg.md#default-rules
[planned-maintenance]: ../articles/virtual-machines/virtual-machines-windows-planned-maintenance.md
[premium-storage]: ../articles/storage/storage-premium-storage.md
[rbac]: ../articles/active-directory/role-based-access-control-what-is.md
[rbac-roles]: ../articles/active-directory/role-based-access-built-in-roles.md
[rbac-devtest]: ../articles/active-directory/role-based-access-built-in-roles.md#devtest-lab-user
[rbac-network]: ../articles/active-directory/role-based-access-built-in-roles.md#network-contributor
[reboot-logs]: https://azure.microsoft.com/en-us/blog/viewing-vm-reboot-logs/
[resize-os-disk]: ../articles/virtual-machines/virtual-machines-windows-expand-os-disk.md
[Resize-VHD]: https://technet.microsoft.com/en-us/library/hh848535.aspx
[Resize virtual machines]: https://azure.microsoft.com/en-us/blog/resize-virtual-machines/
[resource-lock]: ../articles/resource-group-lock-resources.md
[resource-manager-overview]: ../articles/azure-resource-manager/resource-group-overview.md
[security-center]: https://azure.microsoft.com/en-us/services/security-center/
[select-vm-image]: ../articles/virtual-machines/virtual-machines-windows-cli-ps-findimage.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[static-ip]: ../articles/virtual-network/virtual-networks-reserved-public-ip.md
[storage-price]: https://azure.microsoft.com/pricing/details/storage/
[Met Beveiligingscentrum]: ../articles/security-center/security-center-get-started.md#use-security-center
[virtual-machine-sizes]: ../articles/virtual-machines/virtual-machines-windows-sizes.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../articles/azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-resize]: ../articles/virtual-machines/virtual-machines-linux-change-vm-size.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_0/
[0]: ./media/guidance-blueprints/compute-single-vm.png "Architectuur van één Windows VM in Azure"
[readme]: https://github.com/mspnp/reference-architectures/blob/master/guidance-compute-single-vm
[blocks]: https://github.com/mspnp/template-building-blocks
