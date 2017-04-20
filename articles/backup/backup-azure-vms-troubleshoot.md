<properties
    pageTitle="Problemen met back-up van Azure virtual machine | Microsoft Azure"
    description="Problemen met back-up en terugzetten van Azure virtual machines"
    services="backup"
    documentationCenter=""
    authors="trinadhk"
    manager="shreeshd"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="trinadhk;jimpark;"/>


# <a name="troubleshoot-azure-virtual-machine-backup"></a>Azure VM back-up oplossen

> [AZURE.SELECTOR]
- [Recovery services kluis](backup-azure-vms-troubleshoot.md)
- [Back-kluis](backup-azure-vms-troubleshoot-classic.md)

Als u problemen er zijn fouten opgetreden tijdens het gebruik van back-up Azure met informatie in de onderstaande tabel.

## <a name="backup"></a>Back-up

| Back-upbewerking | Foutdetails | Tijdelijke oplossing |
| -------- | -------- | -------|
|Back-up|    Kan de bewerking niet uitvoeren als VM niet meer bestaat. -Stop de bescherming van de virtuele machine zonder back-up van gegevens te verwijderen. Meer informatie op http://go.microsoft.com/fwlink/?LinkId=808124   |Dit gebeurt wanneer de primaire VM wordt verwijderd, maar wel het back-beleid om te zoeken naar een VM back-up uitvoeren. Deze fout te corrigeren: <ol><li> Maak de virtuele machine met dezelfde naam en dezelfde Resourcegroepnaam [cloud servicenaam]<br>(OR)</li><li> Stop de virtuele machine met of zonder het verwijderen van back-up van gegevens te beschermen. [Meer details](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol>|
|Back-up|Kan niet communiceren met de VM-agent voor de status van de momentopname. -Zorg ervoor dat de VM toegang tot het internet heeft. Ook de agent VM bijwerken zoals vermeld in de gids voor probleemoplossing op http://go.microsoft.com/fwlink/?LinkId=800034 |Deze fout wordt gegenereerd als er een probleem met de VM-Agent is of netwerktoegang tot de infrastructuur van Azure is geblokkeerd op een bepaalde manier. Meer informatie over foutopsporing in VM snapshot-problemen.<br> Als de VM-agent wordt niet veroorzaakt door problemen, start u de VM. Soms een onjuiste status voor VM de oorzaak van problemen en het opnieuw starten van de VM stelt deze "slechte staat"|
|Back-up|    Recovery services-extensie is mislukt. -Zorg ervoor dat de nieuwste virtuele machine agent aanwezig op de virtuele machine is en agent-service wordt uitgevoerd. Back-upbewerking nogmaals en als dat niet werkt, neem dan contact op met Microsoft support.|   Deze fout wordt gegenereerd wanneer de VM-agent is verouderd. Zie 'De Agent VM bijwerken' hieronder voor het bijwerken van de VM-agent.|
|Back-up |Virtuele machine bestaat niet. -Controleer of dat die virtuele machine bestaat of Selecteer een andere virtuele machine. | Dit gebeurt wanneer de primaire VM wordt verwijderd, maar wel het back-beleid om te zoeken naar een VM back-up uitvoeren. Deze fout te corrigeren: <ol><li> Maak de virtuele machine met dezelfde naam en dezelfde Resourcegroepnaam [cloud servicenaam]<br>(OR)<br></li><li>Stop de virtuele machines beveiligen zonder back-up van gegevens te verwijderen. [Meer details](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol>|
|Back-up |Uitvoering van de opdracht is mislukt. -Een andere bewerking wordt uitgevoerd op dit object. Wacht tot de vorige bewerking is voltooid en probeer het opnieuw |Een bestaande back-up- of terugzettaak voor VM wordt uitgevoerd en een nieuwe taak kan niet worden gestart terwijl de bestaande taak wordt uitgevoerd.|
| Back-up | VHD's kopiëren van back-kluis time-out - probeer de bewerking in een paar minuten. Als het probleem zich blijft voordoen, neem dan contact op met Microsoft Support. | Dit gebeurt als er te veel gegevens moeten worden gekopieerd. Controleer of er minder dan 16 gegevensschijven. |
| Back-up | Back-up is mislukt vanwege een interne fout: probeer de bewerking in een paar minuten. Als het probleem zich blijft voordoen, neem dan contact op met Microsoft Support | U krijgt deze fout om 2 redenen: <ol><li> Er is een tijdelijk probleem bij de toegang tot opslag VM. Controleer de [Azure Status](https://azure.microsoft.com/en-us/status/) om te zien of er een voortdurende probleem betrekking hebben op de opslag-compute/netwerk in de regio is. Neem is opnieuw het probleem van de back-post beperkt. <li>De oorspronkelijke VM is verwijderd en dus back-up kan niet worden genomen. Hef de beveiliging van de VM te houden van de back-upgegevens voor een verwijderde VM, maar stopt de back-fouten, en kies de optie om de gegevens te houden. Hiermee stopt u het back-upschema en ook de periodieke foutberichten worden weergegeven. |
| Back-up | Kan de Recovery Azure Services installeren is extensie van het geselecteerde item - VM Agent spelen voor Azure Recovery Services-uitbreiding. Installeer de Azure VM-agent en de registratie-bewerking opnieuw starten | <ol> <li>Controleer als de VM-agent correct is geïnstalleerd. <li>Zorg ervoor dat de markering op de VM-configuratie correct is ingesteld.</ol> [Lees meer](#validating-vm-agent-installation) over VM agent-installatie, en hoe de VM-agent-installatie te valideren. |
| Back-up | Extensie de installatie is mislukt met de fout 'COM + is kan niet communiceren met de Microsoft Distributed Transaction Coordinator | Dit betekent dat de COM +-service niet wordt uitgevoerd. Neem contact op met Microsoft Productondersteuning voor hulp bij het oplossen van dit probleem. |
| Back-up | Momentopname is mislukt met de foutmelding dat deze bewerking VSS "dit station is vergrendeld met BitLocker-stationsversleuteling. U kunt deze schijf van het Configuratiescherm moet ontgrendelen. | BitLocker uitschakelen voor alle stations op de VM en bekijk of de VSS-probleem is opgelost |
| Back-up | Virtuele machines met virtuele harde schijven die zijn opgeslagen op de premie opslag worden niet ondersteund voor back-up | Geen |
| Back-up | Azure Virtual Machine niet gevonden. | Dit gebeurt wanneer de primaire VM wordt verwijderd, maar wel het back-beleid om te zoeken naar een VM back-up uitvoeren. Deze fout te corrigeren: <ol><li>Maak de virtuele machine met dezelfde naam en dezelfde Resourcegroepnaam [cloud servicenaam] <br>(OR) <li> Bescherming van deze VM uitschakelen zodat back-uptaken niet gemaakt worden. </ol> |
| Back-up | Agent van de virtuele machine is niet aanwezig op de virtuele machine - Installeer de vereiste voorafgaande vereiste VM agent en de bewerking opnieuw. | [Lees meer](#vm-agent) over VM agent-installatie, en hoe de VM-agent-installatie te valideren. |

## <a name="jobs"></a>Taken

| Bewerking | Foutdetails | Tijdelijke oplossing |
| -------- | -------- | -------|
| Taak annuleren | Annulering wordt niet ondersteund voor dit taaktype - wacht totdat de taak is voltooid. | Geen |
| Taak annuleren | De taak is niet een Annuleerbare staat - wacht totdat de taak is voltooid. <br>OR<br> De geselecteerde taak is niet annuleerbare status - Wacht totdat de taak is voltooid.| Naar alle waarschijnlijkheid is de taak bijna voltooid; Wacht totdat de taak is voltooid |
| Taak annuleren | De taak kan niet worden geannuleerd omdat niet uitgevoerd is-annulering wordt alleen ondersteund voor taken die uitgevoerd worden. Neem de poging annuleren op een onderhanden project. | Dit gebeurt door een staat van tijdelijke aard. Wacht een paar minuten en probeer de annuleringsbewerking |
| Taak annuleren | Kan de taak annuleren - een ogenblik geduld totdat de taak is voltooid. | Geen |


## <a name="restore"></a>Herstellen
| Bewerking | Foutdetails | Tijdelijke oplossing |
| -------- | -------- | -------|
| Herstellen | Terugzetten is mislukt met fout interne Cloud | <ol><li>Cloud-service die u probeert te herstellen wordt geconfigureerd met DNS-instellingen. U kunt controleren. <br>$deployment = 'Servicenaam' get-AzureDeployment - servicenaam-sleuf "Productie" Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Als het adres geconfigureerd is, betekent dit dat de DNS-instellingen zijn geconfigureerd.<br> <li>Cloud-service waarmee u probeert te herstellen wordt geconfigureerd met de optie en bestaande VMs in de cloud-service wordt gestopt.<br>U kunt controleren dat een cloud-service IP is gereserveerd met behulp van na de powershell-cmdlets:<br>$deployment = 'servicenaam' get-AzureDeployment - servicenaam-sleuf "Productie" $dep. ReservedIPName <br><li>U probeert een virtuele machine met volgende netwerkconfiguraties in speciale terugzetten naar dezelfde cloud-service. <br>-Virtuele machines onder load balancer-configuratie (intern en extern)<br>-Virtuele machines met meerdere gereserveerde IP-adressen<br>-Virtuele machines met meerdere NIC 's<br>Selecteer een nieuwe wolk service in de gebruikersinterface of Zie [overwegingen herstellen](./backup-azure-arm-restore-vms.md/#restoring-vms-with-special-network-configurations) voor VMs met speciale netwerkconfiguraties.</ol> |
| Herstellen | De geselecteerde DNS-naam wordt al gebruikt - Geef een andere DNS-naam en probeer het opnieuw. | De DNS-naam verwijst naar de naam van de wolk (meestal eindigend. cloudapp.net). Dit moet uniek zijn. Als deze fout optreedt, moet u een andere naam voor VM kiezen tijdens het terugzetten. <br><br> Houd er rekening mee dat deze fout wordt weergegeven alleen voor gebruikers van de portal voor Azure. De bewerking voor terugzetten via PowerShell slagen omdat het alleen de schijven worden hersteld en de VM niet maken. De fout zal worden geconfronteerd als de VM expliciet door u is gemaakt nadat de schijf bewerking voor terugzetten. |
| Herstellen | De opgegeven virtuele netwerkconfiguratie is niet correct - Geef een ander virtueel netwerkconfiguratie en probeer het opnieuw. | Geen |
| Herstellen | Een gereserveerd IP-adres dat niet overeenkomt met de configuratie van de virtuele machine wordt hersteld - Geef een andere cloud-service die geen gebruik van gereserveerde IP maakt of kies een ander herstelpunt terugzetten vanaf de opgegeven cloud-service gebruikt. | Geen |
| Herstellen | Cloud-service heeft bereikt van het aantal ingevoerde eindpunten - probeer het opnieuw door te geven een andere cloud-service of met behulp van een bestaand eindpunt. | Geen |
| Herstellen | Back-kluis en doel opslag account zijn in twee verschillende regio's - Zorg ervoor dat de opslag in hebt opgegeven bewerking voor terugzetten in hetzelfde gebied, Azure als de back-kluis is. | Geen |
| Herstellen | Opslag-Account die is opgegeven voor de bewerking voor terugzetten niet is ondersteund - alleen Basic-/ Standard opslag rekeningen met lokaal redundante of geo overbodige replicatie-instellingen worden ondersteund. Selecteer een ondersteunde opslag account | Geen |
| Herstellen | Type opslag Account opgegeven voor de bewerking voor terugzetten is niet on line - Zorg ervoor dat de opslag in hebt opgegeven bewerking voor terugzetten online is | Dit kan gebeuren door een tijdelijke fout in Azure opslag of als gevolg van een stroomstoring. Kies een andere account voor opslag. |
| Herstellen | Resourcegroep quotum is bereikt - Verwijder bepaalde resourcegroepen van Azure portal of neem contact op met Azure ondersteuning om de limieten te verhogen. | Geen |
| Herstellen | Er bestaat geen geselecteerde subnet - Selecteer op een subnet dat bestaat | Geen |


## <a name="policy"></a>Beleid
| Bewerking | Foutdetails | Tijdelijke oplossing |
| -------- | -------- | -------|
| Beleid maken | Kan maken van het beleid - Verklein de inhouding opties om door te gaan met de configuratie van het beleid. | Geen |


## <a name="vm-agent"></a>VM-Agent

### <a name="setting-up-the-vm-agent"></a>Instellen van de VM-Agent
Normaal gesproken is de VM-Agent al aanwezig in VMs die zijn gemaakt vanuit de galerie met Azure. Virtuele machines die worden gemigreerd van datacenters voor gebouwen moet echter niet de VM Agent is geïnstalleerd. Voor dergelijke VMs moet de Agent VM expliciet worden geïnstalleerd. Meer informatie over het [installeren van de agent van de VM op een bestaande VM](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

Voor Windows VMs:

- Download en installeer de [agent MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). U moet Administrator-bevoegdheden om de installatie te voltooien.
- [De eigenschap VM bijwerken](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) om aan te geven dat de agent is geïnstalleerd.

Voor Linux VMs:

- Meest recente [Linux agent](https://github.com/Azure/WALinuxAgent) installeren van github.
- [De eigenschap VM bijwerken](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) om aan te geven dat de agent is geïnstalleerd.


### <a name="updating-the-vm-agent"></a>De Agent VM bijwerken
Voor Windows VMs:

- Bijwerken van de VM-Agent is net zo eenvoudig als de [binaire bestanden voor VM-Agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)opnieuw te installeren. U moet echter wel om ervoor te zorgen dat er geen back-up wordt uitgevoerd terwijl de VM-Agent wordt bijgewerkt.

Voor Linux VMs:

- Volg de instructies op de [Agent voor Linux VM bijwerken](../virtual-machines/virtual-machines-linux-update-agent.md).


### <a name="validating-vm-agent-installation"></a>VM-Agent-installatie valideren
Het controleren van de VM-Agent-versie op Windows VMs:

1. Meld u aan bij de Azure virtual machine en navigeer naar de map *C:\WindowsAzure\Packages*. U moet het bestand WaAppAgent.exe aanwezig.
2. Klik met de rechtermuisknop op het bestand, Ga naar **Eigenschappen**en selecteer vervolgens het tabblad **Details** . Het veld versie van het Product dient te worden 2.6.1198.718 of hoger

## <a name="troubleshoot-vm-snapshot-issues"></a>Problemen met momentopnamen VM
VM back-up is afhankelijk van de opdracht momentopname naar de onderliggende opslag. Geen toegang hebben tot opslag of vertraging in de uitvoering van de taken van momentopname mislukt de back-up. De volgende kunnen momentopname taak storing veroorzaken.

1. Toegang tot het netwerk naar opslag is geblokkeerd met behulp van NSG<br>
   Meer informatie over het [netwerktoegang inschakelen](backup-azure-vms-prepare.md#2-network-connectivity) voor opslag met behulp van een WhiteListing van IP-adressen of proxyserver.
2.  VMs met Sql Server-back-up geconfigureerd kunnen momentopname taak vertraging veroorzaken <br>
    Standaard VM back-up problemen VSS volledige back-up voor Windows VMs. Op Sql Server back-up is geconfigureerd en VMs welke Sql-Servers worden uitgevoerd, hierdoor mogelijk vertraging in de momentopname worden uitgevoerd. Stel in volgende registersleutel als u back-fouten vanwege problemen met de momentopname ondervindt.

    ```
    [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
    "USEVSSCOPYBACKUP"="TRUE"
    ```
3.  VM-status juist omdat VM afgesloten in RDP is.  <br>
    Als u de virtuele machine in RDP hebt afgesloten, Controleer terug in de portal VM status juist weergegeven. Als dit niet het geval is, sluit de VM in portal met behulp van de optie 'Afsluiten' in VM dashboard.
4.  Als u meer dan vier VM delen dezelfde service cloud, meerdere back-beleid configureren om te fase back-uptijden dus geen dat back-ups met meer dan vier VM op hetzelfde moment worden gestart. Probeer te verspreiden van de back-up start tijden een uur uit elkaar liggen tussen beleid. 
5.  VM wordt uitgevoerd bij hoge CPU/geheugen.<br>
    Als de virtuele machine wordt uitgevoerd bij hoge CPU usage(>90%) en geheugen, momentopname taak in de wachtrij, vertraagd en zal uiteindelijk een time-out krijgt. Probeer te bellen op back-up in dergelijke situaties.

<br>

## <a name="networking"></a>Netwerken
Net als alle extensies moet back-up uitbreiding toegang tot het openbare internet kunnen werken. Geen toegang hebben tot het openbare internet merkt op verschillende manieren:

- De uitbreiding kan niet geïnstalleerd
- De back-upbewerkingen (zoals schijf momentopname) kunnen mislukken.
- Weergeven van de status van de back-upbewerking kan mislukken.

Is de noodzaak voor het oplossen van openbare internet-adressen zijn gelede [hier](http://blogs.msdn.com/b/mast/archive/2014/06/18/azure-vm-provisioning-stuck-on-quot-installing-extensions-on-virtual-machine-quot.aspx). U moet de DNS-configuratie voor de VNET te controleren en ervoor te zorgen dat de Azure URI's opgelost worden kunnen.

Zodra de naamomzetting correct wordt uitgevoerd, moet toegang tot de onderzoektijdvakken Azure ook worden opgegeven. Voer een van de volgende stappen uit om de blokkering van de toegang tot de infrastructuur Azure, te:

1. "Witte" lijst de Azure datacenter IP-bereiken.
    - De lijst van [Azure datacenter IP-adressen](https://www.microsoft.com/download/details.aspx?id=41653) worden whitelisted ophalen.
    - De IP-adressen met de cmdlet [New-NetRoute](https://technet.microsoft.com/library/hh826148.aspx) de blokkering opheffen. Deze cmdlet binnen de VM Azure uitvoeren in een verhoogde PowerShell-venster (als Administrator uitvoeren).
    - Regels toevoegen aan de NSG (als er een plaats) voor toegang tot de IP-adressen.
2. Maak een pad voor HTTP-verkeer
    - Hebt u enkele beperkingen van het netwerk in plaats van (een netwerk groep, bijvoorbeeld) implementeren van een HTTP-proxyserver voor het routeren van verkeer. Stappen voor het implementeren van een HTTP-proxyserver vindt u [hier](backup-azure-vms-prepare.md#2-network-connectivity).
    - Regels toevoegen aan de NSG (als er een plaats) voor toegang tot het INTERNET vanaf de HTTP-Proxy.

>[AZURE.NOTE] DHCP moet worden ingeschakeld in de Gast IaaS VM back-up werkt.  Als u een statische particuliere IP-adres, moet u deze configureren via het platform. De DHCP-optie binnen de VM moet links worden ingeschakeld.
Meer informatie over het [instellen van een statisch intern particuliere IP-adres](../virtual-network/virtual-networks-reserved-private-ip.md)weergeven.
