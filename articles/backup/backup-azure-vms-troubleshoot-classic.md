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

## <a name="discovery"></a>Discovery

| Back-upbewerking | Foutdetails | Tijdelijke oplossing |
| -------- | -------- | -------|
| Discovery | Ontdek nieuwe artikelen - Microsoft Azure back-up is opgetreden en interne fout is mislukt. Wacht enkele minuten en probeer de bewerking vervolgens opnieuw. | Probeer opnieuw het detectieproces na 15 minuten.
| Discovery | Ontdek nieuwe artikelen: kan niet wordt opsporen met een andere bewerking al uitgevoerd. Wacht tot de huidige detectie-bewerking is voltooid. | Geen |

## <a name="register"></a>Registreren
| Back-upbewerking | Foutdetails | Tijdelijke oplossing |
| -------- | -------- | -------|
| Registreren | Aantal gegevensschijven die zijn gekoppeld aan de virtuele machine overschrijdt - Neem loskoppelen van sommige gegevensschijven op deze virtuele machine en probeer het opnieuw. Azure backup biedt ondersteuning voor maximaal 16 gegevensschijven gekoppeld aan een Azure virtual machine voor back-up | Geen |
| Registreren | Back-up van Microsoft Azure fout interne - wacht enkele minuten en probeer de bewerking opnieuw. Als het probleem zich blijft voordoen, neem dan contact op met Microsoft Support. | U krijgt deze fout om een van de volgende niet-ondersteunde configuratie van de VM op Premium LRS. <br> Premium opslag VMs kunt reservekopie worden gemaakt met behulp van recovery services kluis. [Meer informatie](backup-introduction-to-azure-backup.md/#back-up-and-restore-premium-storage-vms) |
| Registreren | Registratie is mislukt vanwege een time-out voor de bewerking van Agent installeren | Controleer of de versie van het besturingssysteem van de virtuele machine wordt ondersteund. |
| Registreren | Niet - opdracht uitvoeren een andere bewerking wordt uitgevoerd op dit object. Wacht tot de vorige bewerking is voltooid. | Geen |
| Registreren | Virtuele machines met virtuele harde schijven die zijn opgeslagen op de premie opslag worden niet ondersteund voor back-up | Geen |
| Registreren | Agent van de virtuele machine is niet aanwezig op de virtuele machine - Installeer de vereiste voorafgaande vereiste VM agent en de bewerking opnieuw. | [Lees meer](#vm-agent) over VM agent-installatie, en hoe de VM-agent-installatie te valideren. |

## <a name="backup"></a>Back-up

| Back-upbewerking | Foutdetails | Tijdelijke oplossing |
| -------- | -------- | -------|
| Back-up | Kan niet communiceren met de VM-agent voor de status van de momentopname. Er is een time-out opgetreden voor de momentopname VM sub-taak. -Zie de handleiding voor het oplossen van problemen op te lossen dit. | Deze fout wordt gegenereerd als er een probleem met de VM-Agent is of netwerktoegang tot de infrastructuur van Azure is geblokkeerd op een bepaalde manier. Meer informatie over [Foutopsporing in VM momentopname problemen](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md). <br> Als de VM-agent wordt niet veroorzaakt door problemen, start u de VM. Soms een onjuiste status voor VM de oorzaak van problemen en het opnieuw starten van de VM stelt deze "slechte staat" |
| Back-up | Back-up is mislukt vanwege een interne fout: probeer de bewerking in een paar minuten. Als het probleem zich blijft voordoen, neem dan contact op met Microsoft Support | Controleer of er een tijdelijk probleem bij de toegang tot opslag VM. Controleer de [Azure Status](https://azure.microsoft.com/en-us/status/) om te zien of er een voortdurende probleem betrekking hebben op de opslag-compute/netwerk in de regio is. Neem is opnieuw het probleem van de back-post beperkt. |
| Back-up | Kan de bewerking niet uitvoeren als VM niet meer bestaat. | Back-up kan niet worden uitgevoerd als de VM geconfigureerd voor back-up is verwijderd. Stoppen verdere back-ups door te gaan naar beveiligde items weergeven, selecteer beveiligde item en klik op beveiliging stoppen. Door te selecteren de optie gegevens behouden back-up kunt u gegevens behouden. U kunt later weer hervatten bescherming voor deze virtuele machine door te klikken op configureren op bescherming van geregistreerde objecten weergeven|
| Back-up | Kan de Recovery Azure Services installeren is extensie van het geselecteerde item - VM Agent spelen voor Azure Recovery Services-uitbreiding. Installeer de Azure VM-agent en de registratie-bewerking opnieuw starten | <ol> <li>Controleer als de VM-agent correct is geïnstalleerd. <li>Zorg ervoor dat de markering op de VM-configuratie correct is ingesteld.</ol> [Lees meer](#validating-vm-agent-installation) over VM agent-installatie, en hoe de VM-agent-installatie te valideren. |
| Back-up | Niet - opdracht uitvoeren een andere bewerking wordt uitgevoerd op dit object. Wacht tot de vorige bewerking is voltooid en probeer het opnieuw | Een bestaande back-up- of terugzettaak voor VM wordt uitgevoerd en een nieuwe taak kan niet worden gestart terwijl de bestaande taak wordt uitgevoerd. |
| Back-up | Extensie de installatie is mislukt met de fout 'COM + is kan niet communiceren met de Microsoft Distributed Transaction Coordinator | Dit betekent dat de COM +-service niet wordt uitgevoerd. Neem contact op met Microsoft Productondersteuning voor hulp bij het oplossen van dit probleem. |
| Back-up | Momentopname is mislukt met de foutmelding dat deze bewerking VSS "dit station is vergrendeld met BitLocker-stationsversleuteling. U kunt deze schijf van het Configuratiescherm moet ontgrendelen. | BitLocker uitschakelen voor alle stations op de VM en bekijk of de VSS-probleem is opgelost |
| Back-up | Virtuele machines met virtuele harde schijven die zijn opgeslagen op de premie opslag worden niet ondersteund voor back-up | Geen |
| Back-up | Azure Virtual Machine niet gevonden. | Dit gebeurt wanneer de primaire VM wordt verwijderd, maar wel het back-beleid om te zoeken naar een VM back-up uitvoeren. Deze fout te corrigeren: <ol><li>Maak de virtuele machine met dezelfde naam en dezelfde Resourcegroepnaam [cloud servicenaam] <br>(OR) <li> Uitschakelen bescherming voor deze VM, zodat de volgende back-ups worden niet ophalen geactiveerd. </ol> |
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
| Herstellen | Terugzetten is mislukt met fout interne Cloud | <ol><li>Cloud-service die u probeert te herstellen wordt geconfigureerd met DNS-instellingen. U kunt controleren. <br>$deployment = 'Servicenaam' get-AzureDeployment - servicenaam-sleuf "Productie" Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Als het adres geconfigureerd is, betekent dit dat de DNS-instellingen zijn geconfigureerd.<br> <li>Cloud-service waarmee u probeert te herstellen wordt geconfigureerd met de optie en bestaande VMs in de cloud-service wordt gestopt.<br>U kunt controleren dat een cloud-service IP is gereserveerd met behulp van na de powershell-cmdlets:<br>$deployment = 'servicenaam' get-AzureDeployment - servicenaam-sleuf "Productie" $dep. ReservedIPName <br><li>U probeert een virtuele machine met volgende netwerkconfiguraties in speciale terugzetten naar dezelfde cloud-service. <br>-Virtuele machines onder load balancer-configuratie (intern en extern)<br>-Virtuele machines met meerdere gereserveerde IP-adressen<br>-Virtuele machines met meerdere NIC 's<br>Selecteer een nieuwe wolk service in de gebruikersinterface of Zie [overwegingen herstellen](./backup-azure-restore-vms.md/#restoring-vms-with-special-network-configurations) voor VMs met speciale netwerkconfiguraties.</ol> |
| Herstellen | De geselecteerde DNS-naam wordt al gebruikt - Geef een andere DNS-naam en probeer het opnieuw. | De DNS-naam verwijst naar de naam van de wolk (meestal eindigend. cloudapp.net). Dit moet uniek zijn. Als deze fout optreedt, moet u een andere naam voor VM kiezen tijdens het terugzetten. <br><br> Deze fout wordt weergegeven alleen voor gebruikers van de portal voor Azure. De bewerking voor terugzetten via PowerShell is gelukt, omdat het slechts de schijven worden hersteld en de VM niet maken. De fout zal worden geconfronteerd als de VM expliciet door u is gemaakt nadat de schijf bewerking voor terugzetten. |
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





