<properties
    pageTitle="VMware virtuele machines en fysieke servers repliceren naar Azure met Azure Site herstel (verouderd) | Microsoft Azure" 
    description="Beschrijving van het repliceren op ruimten VMs en fysieke servers Windows/Linux met Azure Azure Site herstellen in een oudere distributie in de klassieke portal." 
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
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="raynew"/>

# <a name="replicate-vmware-virtual-machines-and-physical-servers-to-azure-with-azure-site-recovery-using-the-classic-portal-legacy"></a>VMware virtuele machines en fysieke servers repliceren naar Azure met Azure Site herstellen met behulp van de klassieke portal (verouderd)

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-vmware-to-azure.md)
- [Klassieke Portal](site-recovery-vmware-to-azure-classic.md)
- [Klassieke Portal (verouderd)](site-recovery-vmware-to-azure-classic-legacy.md)


Welkom bij herstel Azure Site! Dit artikel beschrijft een oudere implementatie voor het repliceren van lokalen VMware virtuele machines of fysieke servers Windows/Linux naar Azure Site herstellen in de klassieke portal met Azure.

## <a name="overview"></a>Overzicht

Organisaties hebben behoefte aan een strategie voor BCDR die bepaalt hoe apps, werkbelasting en gegevens blijven actief en beschikbaar tijdens geplande en ongeplande uitvaltijd en normale gebruiksomstandigheden zo snel mogelijk herstellen. Uw strategie voor BCDR moet u bedrijfsgegevens veilig en terug te vorderen en dat werklasten continu beschikbaar blijven als ramp plaatsvindt.

Herstel van de site is een Azure service die deel uitmaakt van uw strategie voor BCDR door replicatie van fysieke servers op gebouwen en virtuele machines naar de cloud (Azure) of een secundaire datacenter slaagde. Wanneer er storingen optreden in de primaire locatie, failover u uitvoeren naar de secundaire locatie apps en werkbelasting beschikbaar te houden. U niet terug naar de primaire locatie als resultaat wordt gegeven voor normale bewerkingen. Meer informatie in [Wat is Azure Site terugzetten?](site-recovery-overview.md)


>[AZURE.WARNING] Dit artikel bevat **instructies voor oudere**. Deze niet gebruikt voor nieuwe implementaties. In plaats daarvan [als volgt](site-recovery-vmware-to-azure.md) implementeren Recovery-Site in de portal Azure, of [Gebruik deze instructies](site-recovery-vmware-to-azure-classic.md) voor het configureren van de verbeterde implementatie in de klassieke portal. Als u hebt al geïmplementeerd met behulp van de methode in dit artikel wordt beschreven, raden we u aan voor de verbeterde implementatie in de klassieke portal te migreren.


## <a name="migrate-to-the-enhanced-deployment"></a>Migreren naar de uitgebreide implementatie

Deze sectie is alleen relevant als u sites worden hersteld met behulp van de instructies in dit artikel al hebt geïmplementeerd.

Uw bestaande implementatie, u moet migreren:

1. Nieuwe Site herstel components implementeren in uw site op gebouwen.
2. Referenties voor automatische detectie van VMs VMware op de nieuwe configuratieserver configureren.
3. Ontdek de VMware-servers met de nieuwe configuratieserver.
3. Maak een nieuwe groep voor de bescherming met de nieuwe configuratieserver.


Voordat u begint:

- Wij raden u aan een onderhoudsvenster voor migratie.
- De **Computers migreren** -optie is alleen beschikbaar als u bestaande bescherming groepen die zijn gemaakt tijdens de implementatie van een oudere.
- Na voltooiing van de stappen van de migratie kunt duurt 15 minuten of langer voor het vernieuwen van de referenties, en ontdekken en virtuele machines te vernieuwen, zodat u deze aan een groep voor de bescherming toevoegen kunt. U kunt handmatig vernieuwen gewacht. 

Migreren als volgt:

1. Meer informatie over [verbeterde implementatie in de klassieke portal](site-recovery-vmware-to-azure-classic.md#enhanced-deployment). Bekijk de verbeterde [architectuur](site-recovery-vmware-to-azure-classic.md#scenario-architecture)en [vereisten](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment).
2. De service van de mobiliteit van machines die u momenteel bent repliceren verwijderen. Een nieuwe versie van de service wordt geïnstalleerd op de computers wanneer u ze aan de nieuwe groep voor de bescherming toevoegt.
3. Download een [kluis registratie sleutel](site-recovery-vmware-to-azure-classic.md#step-4-download-a-vault-registration-key) en [de unified setup-wizard uitvoeren](site-recovery-vmware-to-azure-classic.md#step-5-install-the-management-server) naar de configuratieserver, process-server en target master server-onderdelen installeren. Meer informatie over het [plannen van capaciteit](site-recovery-vmware-to-azure-classic.md#capacity-planning).
4. [Referenties instellen](site-recovery-vmware-to-azure-classic.md#step-6-set-up-credentials-for-the-vcenter-server) dat herstel van de Site kunt gebruiken om automatisch ontdekken VMs VMware server VMware. Meer informatie over de [vereiste machtigingen beschikt](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access).
5. [VCenter-servers of hosts vSphere](site-recovery-vmware-to-azure-classic.md#step-7-add-vcenter-servers-and-esxi-hosts)toevoegen. Het kan 15 minuten duren voordat meer voor servers te laten verschijnen in de portal-Site terugzetten.
6. Maak een [nieuwe groep van bescherming](site-recovery-vmware-to-azure-classic.md#step-8-create-a-protection-group). Het kan duren tot 15 minuten voor de portal te vernieuwen, zodat de virtuele machines worden ontdekt en worden weergegeven. Als u niet wilt wachten voordat u de naam van de server management kunt markeren (Klik niet op het) > **vernieuwen**.
7. Klik onder de nieuwe groep voor de bescherming van **Computers migreren**.

    ![Account toevoegen](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration1.png)

8. Selecteer in **Selecteer Machines** de bescherming-groep die u wilt migreren en de computers die u wilt migreren.

    ![Account toevoegen](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration2.png)

9. Opgeven of u wilt dezelfde instellingen gebruiken voor alle computers en selecteer het processerver en Azure opslag account in de **Doel-instellingen configureren** . Als u een afzonderlijk proces-server geen dit is het het IP-adres van de configuratieserver.


    ![Account toevoegen](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration3.png)

    > [AZURE.NOTE] [Migratie van accounts opslag](../resource-group-move-resources.md) via resourcegroepen in het hetzelfde abonnement of via abonnementen wordt niet ondersteund voor opslag gebruikt voor het herstel van de Site te implementeren.

10. Selecteer de account die u hebt gemaakt voor de processerver voor toegang tot de machine voor het duwen van de nieuwe versie van de dienst mobiliteit in **Accounts opgeven**.

    ![Account toevoegen](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration4.png)

11. Herstel van de site wordt de gerepliceerde gegevens migreren naar de Azure opslag-account die u hebt opgegeven. Desgewenst kunt u de opslag account die u in de oude installatie gebruikt opnieuw gebruiken.
12. Virtuele machines worden automatisch gesynchroniseerd als de taak is voltooid. Nadat de synchronisatie is voltooid, kunt u de virtuele machines verwijderen uit de groep oudere bescherming.
13. Als alle computers hebt gemigreerd, kunt u de groep oudere beveiliging verwijderen.
14. Vergeet niet de failover-eigenschappen voor machines en de Azure netwerkinstellingen opgeven nadat de synchronisatie is voltooid.
15. Als u bestaande herstelplannen hebt, kunt u ze migreren naar de verbeterde implementatie met de optie **Migreren herstelplan** . U moet dit alleen doen nadat alle beveiligde computers zijn gemigreerd. 

    ![Account toevoegen](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration5.png)

>[AZURE.NOTE] Nadat u de migratie hebt gaat u verder met de [uitgebreide artikel](site-recovery-vmware-to-azure-classic.md). De rest van dit artikel oude zal niet langer relevant zijn en hoeft u niet te volgen van de stappen die worden beschreven in it ** meer.




## <a name="what-do-i-need"></a>Wat moet ik doen?

In dit diagram ziet u de implementatie van onderdelen.

![Nieuwe kluis](./media/site-recovery-vmware-to-azure-classic-legacy/architecture.png)

Dit is wat u nodig hebt:

**Onderdeel** | **Implementatie** | **Details**
--- | --- | ---
**Configuratieserver** | Een Azure standaard A3 virtuele machine in het abonnement hetzelfde als Recovery-Site. | De configuratieserver coördineert de communicatie tussen computers beveiligd, process-server en master doelservers in Azure. Replicatie en ingesteld coördinaten herstel in Azure wanneer failover plaatsvindt.
**Master-doelserver** | Een Azure VM — Windows server op basis van een Windows Server 2012 R2 (op Windows-computers beveiligen) afbeelding of als een Linux-server op basis van een afbeelding (op Linux machines beschermen) OpenLogic CentOS 6.6.<br/><br/> Grootte van drie opties zijn beschikbaar: standaard A4, standaard D14 en standaard DS4.<br/><br/> De server is verbonden met Azure hetzelfde netwerk als de configuratieserver.<br/><br/> U hebt ingesteld in de portal-Site herstellen | Het ontvangen en behoudt de gerepliceerde gegevens uit uw beveiligde machines met gekoppelde VHD's gemaakt voor in uw account opslag Azure blob-opslag.<br/><br/> Selecteer standaard DS4 specifiek voor de bescherming van de werkbelasting waarvoor consistent hoge prestaties en lage latentie met Premium opslag Account configureren.
**Process-server** | Een in de lokalen virtuele of fysieke server met Windows Server 2012 R2<br/><br/> Aangeraden wordt deze geplaatst op de LAN-segment en hetzelfde netwerk als de machines die u wilt beveiligen, maar uit te voeren op een ander netwerk als beveiligde computers L3 netwerk zichtbaar onder deze zijn.<br/><br/> U stelt en registreer aan de configuratieserver in de portal-Site terugzetten. | Replicatiegegevens verzenden beveiligde computers naar de server op locatie proces. Een schijf-gebaseerd cache-cache replicatiegegevens ontvangen heeft. Het voert een aantal acties op die gegevens.<br/><br/> Deze gegevens worden geoptimaliseerd door caching, comprimeren en coderen voordat het wordt verzonden naar de doelserver master.<br/><br/> Push installatie van de mobiliteit-Service worden verwerkt.<br/><br/> Automatische detectie van VMware virtuele machines worden uitgevoerd.
**Machines voor gebouwen** | Over lokalen VMware virtuele machines of fysieke Windows-of Linux-servers. | Configureren van de replicatie-instellingen die van toepassing zijn een of meer computers. U kunt via een afzonderlijke bewerkingsplaatsen of vaker voorkomt, niet meerdere machines die u in een herstelplan verzamelen. 
**Mobiliteit service** | Geïnstalleerd op elke virtuele machine of fysieke server die u wilt beveiligen<br/><br/> Kan worden geïnstalleerd, handmatig of geduwd en automatisch door de processerver wordt geïnstalleerd wanneer u replicatie voor een machine. | De mobiliteit service verzendt gegevens naar de processerver tijdens de eerste replicatie (synchroniseren). Nadat de computer in een beveiligde staat is (na het synchroniseren is voltooid) wordt de mobiliteit service schrijfbewerkingen naar de schijf in het geheugen worden vastgelegd en verzendt deze naar de processerver. Applicationconsistency voor Windows-servers wordt bereikt met behulp van VSS.
**Azure-Site-Recovery kluis** | Een kluis voor herstel van de Site te maken met een abonnement van Azure en servers registreren in de kluis. | De kluis coördineert en orchestrates gegevensreplicatie, failover en herstel tussen uw site op gebouwen en Azure.
**Replicatiemechanisme** | **Via Internet**, communicatie en replicaten gegevens van servers beveiligd op ruimten naar Azure met beveiligde SSL/TLS-kanaal via het internet. Dit is de standaardoptie.<br/><br/> **VPN/ExpressRoute**, data communicatie en wordt gerepliceerd tussen servers op gebouwen en Azure via een VPN-verbinding. U moet een VPN website of een ExpressRoute verbinding tussen uw site op gebouwen en Azure netwerk instellen.<br/><br/> Selecteer hoe u wilt repliceren tijdens de implementatie van het herstel van de Site. U kunt het mechanisme niet wijzigen nadat deze geconfigureerd zonder invloed op de replicatie van bestaande machines. | Geen van beide optie moet u geen binnenkomende netwerkpoorten op beveiligde computers te openen. Alle netwerkcommunicatie wordt geïnitieerd vanuit de site op gebouwen. 

## <a name="capacity-planning"></a>Capaciteitsplanning

De belangrijkste gebieden moet u rekening houden met:

- **Bronomgeving**— de VMware infrastructure, bron machine en -vereisten.
- **Component servers**— het process-server, configuratieserver en master doelserver 

### <a name="considerations-for-the-source-environment"></a>Overwegingen voor het milieu van de bron

- **Maximale grootte van de schijf**, de huidige maximale grootte van de schijf die kan worden gekoppeld aan een virtuele machine is 1 TB. De maximale grootte van een bronschijf dat kan worden gerepliceerd dus ook beperkt tot 1 TB.
- **Maximale grootte per bron**, de maximale grootte van een enkele bron machine is 31 TB (met 31 schijven) en met een instantie D14 is ingericht voor de master-doelserver. 
- **Het aantal bronnen per model doelserver**— meerdere bron-machines met een doel voor één master-server kunnen worden beveiligd. Echter kan niet een enkele bron machine over meerdere target master servers beveiligd omdat als schijven repliceren, een VHD die overeenkomt met de grootte van de schijf wordt gemaakt op Azure blob-opslag en als een schijf met gegevens naar de doelserver master gekoppeld.  
- **Maximale dagelijkse tarief per bron wijzigen**, er zijn drie factoren die worden beschouwd moeten, wanneer u de aanbevolen wijziging tarief per bron. Voor het doel op basis van overwegingen met betrekking tot zijn twee IOP's vereist op de doelschijf voor elke bewerking in de bron. Dit komt doordat het lezen van oude gegevens en het schrijven van nieuwe gegevens op de doelschijf gebeurt. 
    - **Dagelijks de snelheid die wordt ondersteund door de processerver wijzigen**— een broncomputer kan niet over meerdere proces-servers verdeeld. Een enkel proces kan de server ondersteunen 1 TB dagtarief wijzigen. 1 TB is dus dat de maximale dagelijkse gegevens wijzigen-snelheid voor een broncomputer wordt ondersteund. 
    - **Maximale doorvoer wordt ondersteund door de doelschijf**— lospeuteren per bronschijf maximaal mag niet meer dan 144 GB per dag (met een grootte van 8 K schrijven). Zie de tabel in de doelsectie model voor de doorvoer en IOP's van het doel voor verschillende formaten schrijven. Dit nummer moet worden gedeeld door twee omdat elke bron i 2 IOP's op de doelschijf wordt gegenereerd. Meer informatie over [Azure doelen voor schaalbaarheid en prestaties](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts) bij het configureren van het doel van de premie opslag rekeningen.
    - **Maximale doorvoer wordt ondersteund door de opslag-account**, een bron kan niet meerdere accounts voor opslag omvatten. Gezien dat een opslag account duurt maximaal 20.000 aanvragen per seconde en dat elke bron i 2 IOP's op de doelserver master genereert, wordt aangeraden dat u het aantal IOP's via de bron behouden tot 10.000. Meer informatie over [Azure doelen voor schaalbaarheid en prestaties](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts) bij het configureren van de bron voor de premie opslag rekeningen.

### <a name="considerations-for-component-servers"></a>Overwegingen voor servers onderdeel

Tabel 1 geeft een overzicht van de grootte van de virtuele machine voor de configuratie en de master doelservers.

**Onderdeel** | **Geïmplementeerde Azure exemplaren** | **Kernen** | **Geheugen** | **Maximum aantal schijven** | **Grootte van de schijf**
--- | --- | --- | --- | --- | ---
Configuratieserver | Standaard A3 | 4 | 7 GB | 8 | 1023 GB
Master-doelserver | Standaard A4 | 8 | 14 GB | 16 | 1023 GB
 | Standaard D14 | 16 | 112 GB | 32 | 1023 GB
 | Standaard DS4 | 8 | 28 GB | 16 | 1023 GB

**Tabel 1**

#### <a name="process-server-considerations"></a>Overwegingen bij het verwerken

In het algemeen proces capaciteit is afhankelijk van de dagkoers van de wijziging in alle beschermde werkbelasting.


- U moet voldoende compute taken uitvoeren zoals in line compressie en codering.
- De processerver gebruikt de schijfcache. Zorg ervoor dat de cache van aanbevolen ruimte- en doorvoer is beschikbaar voor het vergemakkelijken van de gewijzigde gegevens opgeslagen bij netwerk knelpunt of stroomstoring. 
- Er zorg voor voldoende bandbreedte zodat de server verwerken de gegevens naar de master-doelserver uploaden kunt om doorlopend gegevens te beschermen. 

Tabel 2 geeft een overzicht van de richtlijnen van de proces-server.

**De snelheid wijzigen gegevens** | **CPU** | **Geheugen** | **Cachegrootte voor schijf**| **Schijf, doorvoersnelheid cache** | **Bandbreedte ingress/egress**
--- | --- | --- | --- | --- | ---
< 300 GB | 4 vCPUs (2 sockets * 2 cores @ 2.5 GHz) | 4 GB | 600 GB | 7 tot 10 MB per seconde | 30 Mbps/21 Mbps
300 tot 600 GB | 8 vCPUs (2 sockets * 4 cores @ 2.5 GHz) | 6 GB | 600 GB | 11 tot 15 MB per seconde | 60 Mbps/42 Mbps
600 GB tot 1 TB | 12 vCPUs (2 sockets * 6 cores @ 2.5 GHz) | 8 GB | 600 GB | 16 tot 20 MB per seconde | 100 Mbps/70 Mbps
> 1 TB | Een ander processerver implementeren | | | | 

**Tabel 2**

Waar: 

- Ingress is downloaden bandbreedte (intranet tussen de bron- en server).
- Egress is upload bandbreedte (internet tussen de processerver en de doelserver master). Egress nummers gaan ervan uit dat de gemiddelde compressie van 30% proces-server.
- Voor cache wordt een aparte OS schijf van minimaal 128 GB schijfruimte aanbevolen voor alle proces-servers.
- Voor schijf, doorvoersnelheid cache de volgende opslag is gebruikt voor benchmarking: 8 SAS harde schijven van 10 K RPM met RAID 10-configuratie.

#### <a name="configuration-server-considerations"></a>Configuratie server overwegingen

Elke configuratieserver kan maximaal 100 bron machines met 3-4 volumes ondersteunen. Als uw distributie groter is raden wij dat u een andere configuratieserver implementeren. Zie tabel 1 voor de standaardeigenschappen van de virtuele machine van de configuratieserver. 

#### <a name="master-target-server-and-storage-account-considerations"></a>Target master server- en account overwegingen

De opslag voor elke basispagina doelserver omvat een OS schijf en een retentievolume gegevensschijven. Het journaal van schijf verandert houdt het behoud station voor de duur van het venster inhouding is gedefinieerd in de portal-Site terugzetten.  Zie tabel 1 voor de eigenschappen van de virtuele machine van de master-doelserver. Tabel 3 ziet u hoe de schijven van A4 worden gebruikt.

**Exemplaar** | **OS schijf** | **Bewaren** | **Gegevensschijven**
--- | --- | --- | ---
 | | **Bewaren** | **Gegevensschijven**
Standaard A4 | 1 schijf (1 * 1023 GB) | 1 schijf (1 * 1023 GB) | 15 schijven (15 * 1023 GB)
Standaard D14 |  1 schijf (1 * 1023 GB) | 1 schijf (1 * 1023 GB) | 31-schijven (15 * 1023 GB)
Standaard DS4 |  1 schijf (1 * 1023 GB) | 1 schijf (1 * 1023 GB) | 15 schijven (15 * 1023 GB)

**Tabel 3**

Afhankelijk van de capaciteitsplanning voor de doelserver master:

- Azure opslagcapaciteit en beperkingen
    - Het maximum aantal schijven gebruikt voor een standaard-Tier VM sterk, is ongeveer 40 (20.000/500 IOP's per schijf) in een opslag op één account. Lees over de [schaalbaarheid doelen voor standaardopslag sccounts](../storage/storage-scalability-targets.md#scalability-targets-for-standard-storage-accounts) en voor de [premie opslag sccounts](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts).
-   De snelheid van dagelijkse wijzigen 
-   Behoud volume opslag.

Houd rekening met:

- Een bron kan niet verdeeld over meerdere accounts voor opslag. Dit geldt voor de schijf met gegevens die u naar de opslag accounts die zijn geselecteerd bij het configureren van beveiliging. Het besturingssysteem en de inhouding schijven gaat meestal u naar de account automatisch geïmplementeerd opslag.
- Het retentievolume opslag vereist, is afhankelijk van de snelheid van de dagelijkse wijzigen en het aantal dagen bewaren. Behoud opslagruimte per model doelserver = totale lospeuteren van bron per dag * aantal dagen bewaren. 
- Elke master doelserver heeft slechts één retentievolume. Het retentievolume worden verdeeld over de schijven die zijn aangesloten op de master-doelserver. Bijvoorbeeld:
    - Als er een broncomputer met 5 schijven en elke schijf 120 IOP's (8K grootte) van de bron genereert, wordt dit omgezet in 240 IOP's per schijf (2 bewerkingen op de doelschijf per bron i/o). 240 IOP's is binnen de Azure per schijf IOP's limiet van 500.
    - Op het retentievolume dit 120 * 5 = 600 wordt IOP's en dit kunnen resulteren in de hals van een fles. In dit scenario zou een goede strategie zijn meer schijven toevoegen aan het retentievolume en beslaan het hele, als een striped RAID-configuratie. Hierdoor nemen de prestaties omdat het IOP's zijn verdeeld over meerdere stations. Het aantal stations moet worden toegevoegd aan het retentievolume zijn als volgt:
        - Totaal IOP's uit de bronomgeving / 500
        - Totale lospeuteren per dag bron-omgeving (niet-gecomprimeerd) / 287 GB. 287 GB is de maximale doorvoer wordt ondersteund door een doelschijf per dag. Deze metrische gegevens zijn afhankelijk van de grootte van het schrijven met een factor van 8K, omdat in dit geval 8K thee schrijven omvang aangenomen. Bijvoorbeeld, als het schrijven 4K is vervolgens doorvoer niet 287/2. En als het schrijven 16K is vervolgens doorvoer 287 * 2.
- Het nummer van opslag rekeningen vereist = totale bron IOP's / 10000.


## <a name="before-you-start"></a>Voordat u begint

**Onderdeel** | **Vereisten** | **Details**
--- | --- | --- 
**Azure-account** | U moet een account van [Microsoft Azure](https://azure.microsoft.com/) . U kunt beginnen met een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).
**Azure opslag** | U moet een account Azure opslag gerepliceerde gegevens opslaan<br/><br/> De account moet een [standaard Geo-redundante opslag](../storage/storage-redundancy.md#geo-redundant-storage) of [Opslag-Premium-Account](../storage/storage-premium-storage.md).<br/><br/> Het moet in hetzelfde gebied, als de Azure-Site-Recovery-service en worden gekoppeld aan het abonnement hetzelfde. We ondersteunen niet het verplaatsen van de opslag-accounts die zijn gemaakt met de [nieuwe Azure portal](../storage/storage-create-storage-account.md) over resourcegroepen.<br/><br/> Lees voor meer informatie [Microsoft Azure Storage-inleiding](../storage/storage-introduction.md)
**Azure virtueel netwerk** | Je hebt een Azure virtueel netwerk waarop de configuratieserver en de doelserver master wordt geïmplementeerd. Moet zijn in het abonnement en de regio als de kluis Azure Site herstellen. Als u wilt dat gegevens worden gerepliceerd via een ExpressRoute of VPN-verbinding moet de Azure virtueel netwerk worden verbonden met het netwerk op locatie via een ExpressRoute-verbinding of een VPN website.
**Azure resources** | Controleer of er voldoende bronnen Azure voor de implementatie van alle onderdelen. Lees meer in [Azure abonnementen](../azure-subscription-service-limits.md).
**Azure virtuele machines** | Virtuele machines die u wilt beveiligen moet met [Azure vereisten](site-recovery-best-practices.md)voldoen.<br/><br/> **Aantal schijven**, maximaal 31 schijven kan op één beveiligde server worden ondersteund.<br/><br/> **Schijfgrootte**, afzonderlijke schijf hoeft niet meer dan 1023 GB<br/><br/> **Clustering**: geclusterde servers worden niet ondersteund.<br/><br/> **Boot**, Unified Extensible Firmware Interface (UEFI) / opstarten met Extensible Firmware Interface(EFI) wordt niet ondersteund.<br/><br/> **Volumes**, Bitlocker gecodeerde volumes worden niet ondersteund.<br/><br/> **Namen**, namen van 1 tot 63 tekens (letters, cijfers en streepjes) moeten bevatten. De naam moet beginnen met een letter of cijfer en eindigen met een letter of cijfer. U kunt de naam van de Azure wijzigen nadat een computer is beveiligd.
**Configuratieserver** | Standaard A3 virtuele machine op basis van een galerijafbeelding Azure Site herstel Windows Server 2012 R2 wordt gemaakt in uw abonnement voor de server configureren. Als het eerste exemplaar in een nieuwe wolk service wordt gemaakt. Als u de openbare Internet als het type verbinding voor de configuratieserver selecteert wordt de cloud-service worden gemaakt met een gereserveerde openbare IP-adres.<br/><br/> Het installatiepad dient alleen Engelse tekens zijn.
**Master-doelserver** | Azure virtual machine, standaard A4, D14 of DS4.<br/><br/> Het installatiepad dient alleen Engelse tekens zijn. Het pad moet bijvoorbeeld **/usr/local/ASR** voor een master-doelserver waarop Linux wordt uitgevoerd.
**Process-server** | U kunt de processerver op fysieke of virtuele machine met Windows Server 2012 R2 met de nieuwste updates implementeren. Installeren op C: /.<br/><br/> Wij raden dat u de server op het netwerk en subnet dezelfde plaatsen als de computers die u wilt beveiligen.<br/><br/> VMware vSphere 5.5.0 CLI installeren op de server verwerken. De VMware vSphere CLI-component is vereist op de server verwerken voor virtuele machines beheerd door een vCenter server of virtuele machines waarop een ESXi host ontdekken.<br/><br/> Het installatiepad dient alleen Engelse tekens zijn.<br/><br/> Verwijzing naar de desbetreffende bestandssysteem wordt niet ondersteund.
**VMware** | Een VMware vCenter server beheren van uw VMware vSphere hypervisors. Deze moet vCenter versie 5.1 of 5.5 uitvoeren met de nieuwste updates.<br/><br/> Een of meer vSphere hypervisors VMware virtuele machines die u wilt beveiligen. De hypervisor moet ESX/ESXi versie 5.1 of 5.5 worden uitgevoerd met de nieuwste updates.<br/><br/> VMware virtuele machines hebt VMware tools geïnstalleerd en uitgevoerd. 
**Windows-computers** | Beveiligde fysieke servers of VMware virtuele machines met Windows hebben een aantal vereisten.<br/><br/> Een ondersteunde 64-bits besturingssysteem: **Windows Server 2012 R2**, **Windows Server 2012**of **Windows Server 2008 R2 met op minimaal SP1**.<br/><br/> De hostnaam, koppelpunten, namen, het systeempad Windows (bijv: C:\Windows) alleen in het Engels moet zijn.<br/><br/> Het besturingssysteem moet worden geïnstalleerd op station C:\.<br/><br/> Alleen standaardschijven worden ondersteund. Dynamische schijven worden niet ondersteund.<br/><br/> Firewall-regels op beveiligde computers moeten kunnen bereiken de doelservers configuratie en master in Azure.p ><p>U moet een administrator-account opgeven (dit moet een lokale beheerder op de computer Windows zijn) push installatie de mobiliteit Service op Windows-servers. Als de opgegeven account een niet-domein-account moet u uitschakelen gebruiker RAS-beheer op de lokale computer is. U doet dit de registervermelding LocalAccountTokenFilterPolicy DWORD met een waarde van 1 onder HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System toevoegen. De registervermelding toevoegen vanuit een open cmd CLI of powershell en voer **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**. [Meer informatie](https://msdn.microsoft.com/library/aa826699.aspx) over toegangsbeheer.<br/><br/> Na een failover als u wilt dat Windows virtuele machines in Azure met extern bureaublad verbinding maken met zorg dat extern bureaublad is ingeschakeld voor de machine op locatie. Als u geen verbinding maakt via een VPN, moeten de firewall-regels externe bureaubladverbindingen toestaan via het internet.
**Linux-machines** | Een ondersteunde 64-bits besturingssysteem: **Centos, 6.4, 6.5, 6,6**; **Oracle Enterprise Linux 6.4, 6.5 waarop Red Hat compatibel kernel of Unbreakable Enterprise versie 3 voor de Kernel (UEK3)**, **SUSE Linux Enterprise Server 11 SP3**.<br/><br/> Firewall-regels op beveiligde computers moeten kunnen worden bereikt, de configuratie en de master doelservers in Azure.<br/><br/> / etc/hosts-bestanden op een beveiligde computers moeten bevatten die de naam van de lokale host worden toegewezen aan IP-adressen die zijn gekoppeld aan alle NIC 's <br/><br/> Als u wilt verbinding maken met een Azure virtual machine waarop Linux na een failover met een Secure Shell client (ssh), ervoor te zorgen dat de service Secure Shell op de beveiligde computer start automatisch op opstarten van het systeem is ingesteld en dat de firewallregels toestaan een ssh verbinding mee.<br/><br/> De hostnaam, koppelpunten, apparaat, en Linux systeempaden en bestandsnamen (bijvoorbeeld/etc /; /usr) dient alleen in het Engels zijn.<br/><br/> Bescherming kan worden ingeschakeld voor computers met de volgende opslag voor bedrijfsruimten:-<br>Bestandssysteem: EXT3, ETX4, ReiserFS, XFS<br>Multipath-software-apparaat Mapper (MPIO)<br>Volume manager: LVM2<br>Fysieke servers met HP CCISS-controller opslag worden niet ondersteund.
**Derden** | Sommige onderdelen van de implementatie in dit scenario is afhankelijk van software van derden te laten functioneren. Zie voor een volledige lijst met [derden software mededelingen en bekendmakingen](#third-party)


### <a name="network-connectivity"></a>Verbinding met het netwerk

U hebt twee opties bij het configureren van netwerkverbindingen tussen uw site op gebouwen en de Azure virtuele netwerken waarop de onderdelen van de infrastructuur (configuratieserver, master doelservers) worden geïmplementeerd. U moet beslissen welke optie netwerk verbinding te gebruiken voordat u de configuratieserver kunt implementeren. U moet deze instelling op het moment van installatie kiezen. Het kan niet later worden gewijzigd.

**Internet:** Communicatie en replicatie van gegevens tussen de servers op gebouwen (processerver, beveiligde machines) en de Azure onderdeel infrastructuurservers (configuratieserver, master doelserver) gebeurt via een beveiligde SSL/TLS verbinding op ruimten op de openbare eindpunten op de doelservers model en configuratie. (De enige uitzondering hierop is de verbinding tussen het proces en de master target server op TCP-poort 9080 die gecodeerd wordt. Alleen controle informatie met betrekking tot de replicatie-protocol voor de replicatie-instellingen worden uitgewisseld via deze verbinding.)

![Implementatie van diagrammen internet](./media/site-recovery-vmware-to-azure-classic-legacy/internet-deployment.png)

**VPN**: communicatie en replicatie van gegevens tussen de servers op gebouwen (processerver, beveiligde machines) en de Azure onderdeel infrastructuurservers (configuratieserver, master doelserver) gebeurt via een VPN-verbinding tussen uw netwerk op gebouwen en de Azure virtueel netwerk waarin de configuratieserver en master doelservers zijn geïmplementeerd. Controleer of uw netwerk op lokalen met de Azure virtueel netwerk is aangesloten door een ExpressRoute-verbinding of een VPN-verbinding van site naar site.

![Inzetdiagram VPN](./media/site-recovery-vmware-to-azure-classic-legacy/vpn-deployment.png)


## <a name="step-1-create-a-vault"></a>Stap 1: Maak een kluis

1. Aanmelden bij de [Portal beheren](https://portal.azure.com).


2. Vouw **Services Data** > **Recovery Services** en klik op **Site-Recovery kluis**.


3. Klik op **Nieuw** > **snel tabellen maken**.

4. Voer in het vak **naam**een beschrijvende naam ter identificatie van de kluis.

5. Selecteer de regio voor de kluis in de **regio**. Ondersteunde regio's Zie geografische beschikbaarheid in [Azure Site herstel prijzen Details](https://azure.microsoft.com/pricing/details/site-recovery/)

6. Klik op **de kluis maken**.

    ![Nieuwe kluis](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-create-vault.png)

Controleer de statusbalk om te bevestigen dat de kluis is gemaakt. De kluis wordt aangeboden als **actief** op de hoofdpagina **Recovery Services** .

## <a name="step-2-deploy-a-configuration-server"></a>Stap 2: Een configuratieserver implementeren

### <a name="configure-server-settings"></a>Serverinstellingen configureren

1. Klik op de kluis Open de pagina Quick Start **Recovery Services** op de pagina. Quick Start kan ook worden geopend via het pictogram.

    ![Quick Start-pictogram](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-icon.png)

2. Selecteer in de vervolgkeuzelijst **tussen een site op ruimten met VMware/fysieke servers en Azure**.
3. Klik op **Configuratieserver implementeren**in **Voorbereiding Target(Azure) Resources** .

    ![Configuratieserver implementeren](./media/site-recovery-vmware-to-azure-classic-legacy/deploy-cs2.png)

4. In de **Nieuwe configuratie-gegevens** opgeven:

    - Een naam voor de configuratieserver en referenties tot stand te brengen.
    - Typ in het netwerk verbinding vervolgkeuzelijst Selecteer **Openbare Internet** of **VPN**. Houd er rekening mee dat u deze instelling nadat deze toegepast niet wijzigen.
    - Selecteer de Azure netwerk waarop de server moet zich bevinden. Als u gebruikmaakt van VPN-merk zeker de Azure netwerk is verbonden met het netwerk op ruimten zoals verwacht. 
    - Het interne IP-adres en subnetmasker die zijn toegewezen aan de server opgeven. Houd er rekening mee dat de eerste vier IP-adressen in een subnet zijn gereserveerd voor intern gebruik van Azure. Andere beschikbare IP-adres gebruiken.
    
    ![Configuratieserver implementeren](./media/site-recovery-vmware-to-azure-classic-legacy/cs-details.png)

5. Wanneer u op **OK** , een standaard A3 virtuele machine op basis van een galerijafbeelding Azure Site herstel Windows Server 2012 R2 wordt gemaakt in uw abonnement voor de server configureren. Als het eerste exemplaar in een nieuwe wolk service wordt gemaakt. Als u verbinding maakt via het internet is een gereserveerde openbare IP-adres de cloud-service gemaakt. U kunt de vooruitgang op het tabblad **taken** controleren.

    ![Voortgang bewaken](./media/site-recovery-vmware-to-azure-classic-legacy/monitor-cs.png)

6.  Als u verbinding via het internet maakt, nadat de configuratieserver wordt geïmplementeerd opmerking het openbare IP-adres toegewezen op de pagina **virtuele Machines** in de portal Azure. Klik op de **eindpunten** tabblad notitie de openbare HTTPS-poort toegewezen aan particuliere poort 443. Wanneer u de master doel en de proces-servers met de van configuratieserver registreert, hebt u deze gegevens later nodig. De configuratieserver wordt geïmplementeerd met twee eindpunten:

    - HTTPS: Een openbare poort wordt gebruikt voor communicatie tussen servers component en Azure coördineren via het internet. Particuliere poort 443 wordt gebruikt voor communicatie tussen servers component en Azure coördineren via VPN.
    - Aangepast: Een openbare poort wordt gebruikt voor failback hulpprogramma communicatie via het internet. Particuliere poort 9443 wordt gebruikt voor failback hulpprogramma communicatie via VPN.
    - PowerShell: Private poort 5986
    - Extern bureaublad: Private poort 3389
    
    ![VM-eindpunten](./media/site-recovery-vmware-to-azure-classic-legacy/vm-endpoints.png)

    >[AZURE.WARNING] Niet verwijderen of het aantal openbare of particuliere poorten gemaakt tijdens de configuratie van server-implementatie eindpunten wijzigen.

De configuratieserver wordt geïmplementeerd in een automatisch gemaakte Azure cloud-service met een gereserveerd IP-adres. Het gereserveerde adres is nodig om ervoor te zorgen dat configuratie cloud service IP-adres van de server hetzelfde gedurende meerdere herstarts van de virtuele machines blijft (met inbegrip van de configuratieserver) op de cloud-service. De gereserveerde openbare IP-adres moet worden handmatig onvoorwaardelijke wanneer de van configuratieserver buiten gebruik is gesteld of deze gereserveerde zult blijven. Er is een standaardlimiet van 20 gereserveerde openbare IP-adressen per abonnement. [Meer informatie](../virtual-network/virtual-networks-reserved-private-ip.md) over het gereserveerde IP-adressen. 

### <a name="register-the-configuration-server-in-the-vault"></a>De configuratieserver registreren in de kluis

1. Klik op **Doel middelen voorbereiden**op de pagina **Quick Start**  > **downloaden van een registratie-sleutel**. Het bestand wordt automatisch gegenereerd. Het is geldig voor 5 dagen nadat het gegenereerd. Kopiëren naar de configuratieserver.
2. Selecteer de configuratieserver uit de lijst met virtuele machines in **virtuele Machines** . Open het tabblad **Dashboard** en klik op **verbinding maken**. **Open** het gedownloade RDP-bestand aan te melden bij de configuratieserver via Extern bureaublad. Als u VPN, gebruik het interne IP-adres (het adres dat u hebt opgegeven als u de van configuratieserver geïmplementeerd) voor verbinding met extern bureaublad vanaf de site van gebouwen. De Wizard Azure Site herstellen configuratie Server Setup wordt automatisch uitgevoerd wanneer u zich aanmeldt voor de eerste keer.

    ![Registratie](./media/site-recovery-vmware-to-azure-classic-legacy/splash.png)

3. Klik op **ik ga akkoord** als u wilt downloaden en installeren van MySQL in **Software-installatie van derden** .

    ![MySQL installeren](./media/site-recovery-vmware-to-azure-classic-legacy/sql-eula.png)

4. Maak referenties aan te melden bij het exemplaar van de MySQL-server in de **MySQL-Server-gegevens** .

    ![MySQL-referenties](./media/site-recovery-vmware-to-azure-classic-legacy/sql-password.png)

5. Opgeven hoe de configuratieserver maakt verbinding met het internet in **Internet-opties** . Houd rekening met:

    - Als u een aangepaste proxy gebruiken moet u deze instellen voordat u de voorziening hebt geïnstalleerd.
    - Wanneer u op **volgende** klikt wordt een test uitgevoerd om te controleren van de proxyverbinding.
    - Als u een aangepaste proxy gebruikt, of de standaardproxy verificatie moet u de proxy-gegevens vereist, met inbegrip van het adres, de poort en de referenties invoeren.
    - De volgende URL's zijn toegankelijk via de proxy:
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Als u het IP-adres gebaseerde firewallregels wordt ervoor gezorgd dat de regels zijn ingesteld voor de communicatie van de server configureren met de IP-adressen die worden beschreven in (443) [Azure Datacenter IP-bereiken](https://msdn.microsoft.com/library/azure/dn175718.aspx) en HTTPS-protocol. U zou moeten witte lijst IP-bereik van de Azure regio die u wilt gebruiken en die van de VS West.

    ![Registratie van proxy](./media/site-recovery-vmware-to-azure-classic-legacy/register-proxy.png)

6. In **Provider fout bericht lokalisatie-instellingen** opgeven in welke taal u foutberichten worden weergegeven.

    ![Fout bericht registratie](./media/site-recovery-vmware-to-azure-classic-legacy/register-locale.png)

7. In **Azure Site Recovery registratie** bladeren en selecteer het bestand dat naar de server worden gekopieerd.

    ![Registratie-sleutelbestand](./media/site-recovery-vmware-to-azure-classic-legacy/register-vault.png)

8. Selecteer deze opties op de voltooiingspagina van de wizard:

    - Selecteer **Account Management dialoogvenster starten** om op te geven dat het dialoogvenster Accounts beheren moet worden geopend nadat de wizard is voltooid.
    - Selecteer **maken voor Cspsconfigtool een pictogram op het bureaublad** een snelkoppeling op het bureaublad op de configuratieserver toevoegen zodat u het dialoogvenster **Accounts beheren** op elk gewenst moment openen kunt zonder de wizard opnieuw uitvoeren.

    ![Inschrijving voltooien](./media/site-recovery-vmware-to-azure-classic-legacy/register-final.png)

9. Klik op **Voltooien** om de wizard te voltooien. Een wachtwoordzin is gegenereerd. Naar een veilige locatie kopiëren. Je heb nodig om te verifiëren en de proces- en doelservers master registreren bij de configuratieserver. Het wordt ook gebruikt om ervoor te zorgen integriteit in de configuratie van server-communicatie kanaal. De wachtwoordzin kan opnieuw worden gegenereerd, maar moet u het hoofd doel opnieuw te registreren en verwerken van servers met behulp van de nieuwe wachtwoordzin.

    ![Wachtwoordzin](./media/site-recovery-vmware-to-azure-classic-legacy/passphrase.png)

Na registratie van de configuratieserver weergegeven op de pagina **Configuratie van Servers** in de kluis.

### <a name="set-up-and-manage-accounts"></a>Instellen en beheren van accounts

Herstel van de Site vraagt tijdens de implementatie van referenties voor de volgende acties:

- Een account VMware zodat thatSite herstel kan automatisch detectie VMs op vCenter servers of hosts vSphere. 
- Wanneer u toevoegt machines voor bescherming, zodat het herstel van de Site de mobiliteit service kunt installeren op deze.

Nadat u de configuratieserver hebt geregistreerd, kunt u het dialoogvenster **Accounts beheren** als u wilt toevoegen en beheren van accounts die moeten worden gebruikt voor deze acties kunt openen. Er zijn een aantal manieren om dit te doen:

- Open de snelkoppeling die u ervoor gekozen om gemaakt voor het dialoogvenster op de laatste pagina van de instellingen voor de server configureren (cspsconfigtool).
- Open het dialoogvenster op voltooien van de configuratie van de server.

1. Klik op **Account toevoegen**in **Accounts beheren** . U kunt ook wijzigen en verwijderen van bestaande accounts.

    ![Accounts beheren](./media/site-recovery-vmware-to-azure-classic-legacy/manage-account.png)

2. Geef een accountnaam wilt gebruiken in **De Details** in Azure en referenties (domein/gebruikersnaam). 

    ![Accounts beheren](./media/site-recovery-vmware-to-azure-classic-legacy/account-details.png)

### <a name="connect-to-the-configuration-server"></a>Verbinding maken met de configuratieserver 

Er zijn twee manieren verbinding maken met de configuratieserver:

- Via een VPN-site naar site of ExpressRoute verbinding
- Via het internet 

Houd rekening met:

- De eindpunten van de virtuele machine een internet-verbinding gebruikt in combinatie met de openbare virtuele IP-adres van de server.
- Het interne IP-adres van de server wordt een VPN-verbinding samen met het eindpunt particuliere poorten.
- Is een eenmalige beslissing om te bepalen of de verbinding (controle en replicatie van gegevens) op uw servers op ruimten aan de verschillende component servers (configuratieserver, master doelserver) uitgevoerd in Azure via een VPN-verbinding of het internet. U kunt deze instelling achteraf niet wijzigen. Als u dit doet moet u opnieuw het scenario te implementeren en uw machines reprotect.  


## <a name="step-3-deploy-the-master-target-server"></a>Stap 3: Implementatie van de doelserver master

1. Klik op **Target(Azure) Resources voorbereiden** > **implementeren master-doelserver**.
2. Geef de details van master server en referenties. De server wordt geïmplementeerd in Azure hetzelfde netwerk als de configuratieserver. Wanneer u klikt op Voltooien een Azure virtuele machine wordt gemaakt met een Windows- of Linux afbeelding.

    ![Doel-serverinstellingen](./media/site-recovery-vmware-to-azure-classic-legacy/target-details.png)

Houd er rekening mee dat de eerste vier IP-adressen in een subnet zijn gereserveerd voor intern gebruik van Azure. Andere beschikbare IP-adres opgeven.

>[AZURE.NOTE] Selecteer standaard DS4 bij het configureren van de bescherming van de werklast die consistent hoge i/o-prestaties en lage latentie vereisen om de i/o-intensieve belasting [Premium opslag-Account](../storage/storage-premium-storage.md)gebruiken als host.


3. Een Windows-master doelserver VM wordt gemaakt met twee eindpunten. Openbare eindpunten worden gemaakt alleen als je verbinding maakt via het internet.

    - Aangepast: Openbare poort die wordt gebruikt door de processerver om replicatiegegevens te verzenden via het internet. Particuliere poort 9443 wordt gebruikt door de processerver om replicatiegegevens te verzenden naar de doelserver master via VPN.
    - 1: Openbare poort die wordt gebruikt door de processerver metagegevens verzenden via het internet. Particuliere poort 9080 wordt gebruikt door de processerver metagegevens verzenden naar de doelserver master via VPN.
    - PowerShell: Private poort 5986
    - Extern bureaublad: Private poort 3389

4. Linux master doelserver VM wordt gemaakt met twee eindpunten. Houd er rekening mee dat openbare eindpunten alleen gemaakt worden als u verbinding via het internet maakt.

    - Aangepast: Openbare poort wordt gebruikt door de processerver om replicatiegegevens te verzenden via het internet. Particuliere poort 9443 wordt gebruikt door de processerver om replicatiegegevens te verzenden naar de doelserver master via VPN.
    - 1: Openbare poort wordt gebruikt door de processerver metagegevens verzenden via het internet. Particuliere poort 9080 wordt gebruikt door de processerver metagegevens verzenden naar de doelserver master via VPN
    - SSH: Private poort 22

    >[AZURE.WARNING] Niet verwijderen of wijzigen van de openbare of particuliere poortnummer van een van de eindpunten die is gemaakt tijdens de implementatie van target master server.

5. In de **virtuele Machines** wachten op de virtuele machine te starten.

    - Als het een Windows server Noteer de remote desktop-gegevens.
    - Opmerking het interne IP-adres van de virtuele machine als dit een Linux-server is en u verbinding via VPN maakt waarmee. Opmerking het openbare IP-adres als u verbinding via het internet maakt.

6.  Aanmelden bij de server voor een volledige installatie en het registreren met de configuratieserver. 
7.  Als u Windows uitvoert:

    1. Een verbinding met extern bureaublad om de virtuele machine te starten. De eerste keer dat u zich aanmeldt een script wordt uitgevoerd in een venster PowerShell. Niet sluiten. Wanneer het klaar is het hulpprogramma Configuratie van Host-Agent automatisch wordt geopend voor het registreren van de server.
    2. Geef in de **Configuratie van Host-Agent** het interne IP-adres van de van configuratieserver en poort 443. U kunt het interne adres en de private poort 443 zelfs als u geen verbinding maakt via VPN omdat de virtuele machine is gekoppeld aan de Azure hetzelfde netwerk als de configuratieserver. **Gebruik HTTPS** ingeschakeld laten. Geef de wachtwoordzin voor de configuratieserver die u eerder hebt genoteerd. Klik op **OK** om de server te registreren. De NAT-opties kunt u negeren. Ze niet gebruikt.
    3. Als uw station Geschatte inhouding eis meer dan 1 TB is kunt u configureren met behulp van een virtuele schijf en [opslagruimtes](http://blogs.technet.com/b/askpfeplat/archive/2013/10/21/storage-spaces-how-to-configure-storage-tiers-with-windows-server-2012-r2.aspx) retentievolume (R:)
    
    ![De master doelserver Windows](./media/site-recovery-vmware-to-azure-classic-legacy/target-register.png)

8. Als u werkt met Linux:
    1. Zorg ervoor dat u de meest recente Linux Integration Services (LIS) geïnstalleerd voordat u de master-doelserver installeren hebt geïnstalleerd. U vindt de nieuwste versie van de LIS met instructies voor het installeren [hier](https://www.microsoft.com/download/details.aspx?id=46842). Start de computer opnieuw op nadat de LIS.
    2. Klik op **downloaden en installeren van extra software (alleen voor Linux Master doelserver)**in **doel voorbereiden (Azure) bronnen** . Kopieer het gedownloade tar-bestand met de virtuele machine met een sftp-client. U kunt ook aanmelden bij de geïmplementeerde linux master doelserver en *wget http://go.microsoft.com/fwlink/?LinkID=529757&clcid=0x409* gebruiken voor het downloaden de het bestand.
    2. Aanmelden bij de server met behulp van een Secure Shell client. Als u met het netwerk Azure via VPN verbonden bent gebruiken het interne IP-adres. Gebruik anders het externe IP-adres en het openbare SSH-eindpunt.
    3. Pak de bestanden van het installatieprogramma gzipped door te voeren: **tar-xvzf Microsoft-ASR_UA_8.4.0.0_RHEL6-64***
    ![Linux master doelserver](./media/site-recovery-vmware-to-azure-classic-legacy/linux-tar.png)
    4. Zorg ervoor dat u zich in de map waarin u de inhoud van de tar-bestand hebt uitgepakt.
    5. De server configuratie wachtwoordzin kopiëren naar een lokaal bestand met de opdracht * *echo* `<passphrase>` * > passphrase.txt**
    6. De opdracht '**sudo. / install -t beide - a host -R -d /usr/local/ASR MasterTarget -i* `<Configuration server internal IP address>` * -p 443 -s j - c https -P passphrase.txt** '.

    ![Doelserver registreren](./media/site-recovery-vmware-to-azure-classic-legacy/linux-mt-install.png)

9. Wacht een paar minuten (10-15) en controleer of de master doelserver geregistreerd in **Servers**wordt vermeld op de pagina >  **Server Details** tabblad**Configuratie** . Als u Linux gebruikt en als het niet registreren de host config tool opnieuw uitvoeren van /usr/local/ASR/Vx/bin/hostconfigcli. U moet machtigingen instellen door type chmod als root uit te voeren.

    ![Controleer of de doelserver](./media/site-recovery-vmware-to-azure-classic-legacy/target-server-list.png)

>[AZURE.NOTE] Het kost 15 minuten nadat de registratie is voltooid voor de master-doelserver te worden vermeld in de portal. Klik op **vernieuwen** op de pagina **Configuratie van Servers** onmiddellijk bijwerken.

## <a name="step-4-deploy-the-on-premises-process-server"></a>Stap 4: Het proces van lokalen server implementeren

Voordat u wordt aangeraden een statisch IP-adres te configureren op de server verwerken zodat deze is gegarandeerd blijvende in opnieuw wordt opgestart.

1. Klik op de werkbalk Snel starten > **Process Server installeren op gebouwen** > **downloaden en installeren van de processerver**.

    ![Process-server installeren](./media/site-recovery-vmware-to-azure-classic-legacy/ps-deploy.png)

2.  Kopieer het gedownloade zip-bestand naar de server waarop u gaat de processerver installeren. Het zip-bestand bevat twee bestanden:

    - Microsoft-ASR_CX_TP_8.4.0.0_Windows*
    - Microsoft-ASR_CX_8.4.0.0_Windows*

3. Pak het archief en de installatiebestanden kopiëren naar een locatie op de server.
4. Voer de **Microsoft-ASR_CX_TP_8.4.0.0_Windows*** installatie-bestand en volg de instructies. Hiermee installeert u onderdelen van andere leveranciers die nodig zijn voor de implementatie.
5. Voer **Microsoft-ASR_CX_8.4.0.0_Windows***.
6. Selecteer op de pagina **Servermodus** **Process-Server**.
7. Op de pagina **Details omgeving** doen het volgende:


    - Als u wilt beveiligen, VMware virtual machines klikt u op **Ja**
    - Als u alleen wilt beveiligen fysieke servers en VMware vCLI geïnstalleerd op de server verwerken dus niet nodig. Klik op **Nee** en doorgaan.

8. Let bij het installeren van VMware vCLI op het volgende:

    - **Alleen VMware vSphere CLI 5.5.0 wordt ondersteund**. De processerver werkt niet met andere versies of updates van vSphere CLI.
    - VSphere CLI 5.5.0 downloaden van [hier.](https://my.vmware.com/web/vmware/details?downloadGroup=VCLI550&productId=352)
    - Als u vSphere CLI geïnstalleerd net voordat de installatie van de processerver is gestart en setup niet wordt herkend, wacht u vijf minuten voordat u setup opnieuw probeert. Dit zorgt ervoor dat de omgevingsvariabelen die nodig zijn voor de detectie van vSphere CLI zijn geïnitialiseerd correct.

9.  Selecteer de netwerkadapter die moet worden gebruikt door de processerver in **NIC selectie voor Process Server** .

    ![Selecteer de adapter](./media/site-recovery-vmware-to-azure-classic-legacy/ps-nic.png)

10. In de **configuratiegegevens van de Server**:

    - Geef het interne IP-adres van de configuratieserver en 443 voor de poort voor het IP-adres en poort, als u verbinding via VPN maakt. Anders is de openbare virtuele IP-adres en het toegewezen openbare HTTP-eindpunt opgeven.
    - Typ de wachtwoordzin van de configuratieserver.
    - **Controleer of mobiliteit service software handtekening** wissen als u controle uitschakelen wilt als u automatische push gebruikt om de service te installeren. Controle van de handtekening moet internet-verbinding van de processerver.
    - Klik op **volgende**.

    ![Configuratieserver registreren](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cs.png)


11. Selecteer een schijf cache in **installatiestation selecteren** . De processerver moet een cache-station met ten minste 600 GB. Klik vervolgens op **installeren**. 

    ![Configuratieserver registreren](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cache.png)

12. Opmerking mogelijk moet u opnieuw opstarten van de server om de installatie te voltooien. In **De Server configuratie** > **Server Details** controleren of de processerver wordt weergegeven en geregistreerd in de kluis.

>[AZURE.NOTE]Het kost 15 minuten nadat de registratie is voltooid voor de processerver worden weergegeven zoals deze wordt weergegeven op de server configureren. Vernieuwen om onmiddellijk wordt bijgewerkt, de configuratieserver door te klikken op de knop Vernieuwen onder aan de configuratiepagina van server
 
![Processerver verifiëren](./media/site-recovery-vmware-to-azure-classic-legacy/ps-register.png)

Als u geen verificatie van handtekeningen voor de mobiliteit service uitschakelen bij het registreren van de processerver kunt u deze later als volgt doen:

1. Aanmelden bij de server verwerken als administrator en open het bestand C:\pushinstallsvc\pushinstaller.conf bewerkt. In de sectie **[PushInstaller.transport]** deze regel toevoegen: **SignatureVerificationChecks = "0"**. Opslaan en sluit het bestand.
2. Start de service InMage PushInstall.


## <a name="step-5-update-site-recovery-components"></a>Stap 5: Update-Site herstellen onderdelen

Site herstel onderdelen worden van tijd tot tijd bijgewerkt. Wanneer er nieuwe updates beschikbaar zijn kunt u ze moet installeren in de volgende volgorde:

1. Configuratieserver
2. Process-server
3. Master-doelserver
4. Failback-hulpprogramma (vContinuum)

### <a name="obtain-and-install-the-updates"></a>Verkrijgen en installeren van de updates


1. Van het herstel van de Site in het **Dashboard**kunt u updates voor de configuratie-, proces- en doelservers master. Voor Linux-installatie kunt u de bestanden extraheren uit het gzipped-installatieprogramma en voer de opdracht ' sudo. / install ' om de update te installeren.
2. [Download](http://go.microsoft.com/fwlink/?LinkID=533813) de meest recente update voor de Failback-tool(vContinuum).
3. Als u werkt met virtuele machines of fysieke servers die al de mobiliteit service is geïnstalleerd hebben, kunt u updates voor de service als volgt opvragen:

    - **Optie 1**: updates downloaden:
        - [Windows Server (alleen 64 bits)](http://download.microsoft.com/download/8/4/8/8487F25A-E7D9-4810-99E4-6C18DF13A6D3/Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe)
        - [CentOS 6.4,6.5,6.6 (alleen voor 64 bits)](http://download.microsoft.com/download/7/E/D/7ED50614-1FE1-41F8-B4D2-25D73F623E9B/Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz)
        - [Oracle Enterprise Linux 6.4,6.5 (alleen voor 64 bits)](http://download.microsoft.com/download/5/2/6/526AFE4B-7280-4DC6-B10B-BA3FD18B8091/Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz)
        - [SUSE Linux Enterprise Server SP3 (alleen 64 bits)](http://download.microsoft.com/download/B/4/2/B4229162-C25C-4DB2-AD40-D0AE90F92305/Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz)
        - Na het bijwerken van de processerver zijn de bijgewerkte versie van de dienst mobiliteit beschikbaar in de map C:\pushinstallsvc\repository op de server verwerken.
    - **Optie 2**: hebt u een computer met een oudere versie van de mobiliteit service is geïnstalleerd, kunt u de mobiliteit service op de computer automatisch bijwerken vanaf de portal beheren.

        1. Zorg ervoor dat de process-server is bijgewerkt.
        2. Zorg ervoor dat de beveiligde computer voldoen aan de [vereisten](#install-the-mobility-service-automatically) voor het automatisch de dienst mobiliteit, duwen zodat de update werkt zoals verwacht.
        2. Selecteer de groep voor de bescherming, de beveiligde computer en klik op **Update mobiliteit service**. Deze knop is alleen beschikbaar als er een nieuwere versie van de dienst mobiliteit. 

            ![VCenter server selecteren](./media/site-recovery-vmware-to-azure-classic-legacy/update-mobility.png)

Geef het administrator-account moet worden gebruikt voor het bijwerken van de mobiliteit-service op de beveiligde server in accounts selecteren. Klik op OK en wacht tot de geactiveerde taak te voltooien.


## <a name="step-6-add-vcenter-servers-or-vsphere-hosts"></a>Stap 6: VCenter-servers of hosts vSphere toevoegen

1. Klik op **Servers** > **Configuration Servers** > configuratieserver >**Add vCenter Server** een vCenter server of vSphere host toevoegen.

    ![VCenter server selecteren](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter.png)

2. Details voor de server of de host opgeven en selecteert u de processerver die wordt gebruikt voor het ontdekken.

    - Geef het nummer van de poort waarop de vCenter server wordt uitgevoerd als de vCenter server op poort 443 standaard niet wordt uitgevoerd.
    - De processerver moet zich op hetzelfde netwerk als de host vCenter server/vSphere en VMware vSphere CLI 5.5.0 geïnstalleerd moet hebben.

    ![vCenter serverinstellingen](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter4.png)


3. De vCenter server worden weergegeven onder de configuratiegegevens van de server nadat de detectie is voltooid.

    ![vCenter serverinstellingen](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter2.png)

4. Als u een niet-administratoraccount gebruikt de host of server toe te voegen, schakelt u dat de account de volgende machtigingen heeft:

    - vCenter accounts moeten hebben Datacenter, Datastore, map, Host, netwerk, Resource, opslag weergaven, virtuele machine en vSphere Distributed Switch bevoegdheden ingeschakeld.
    - vSphere host rekeningen dient te beschikken over het Datacenter, Datastore, map, Host, netwerk, Resource, virtuele machine vSphere Distributed Switch bevoegdheden ingeschakeld



## <a name="step-7-create-a-protection-group"></a>Stap 7: Maak een groep voor de bescherming

1. **Beveiligde Items**openen > **Bescherming groep** > **bescherming-groep maken**.

    ![Bescherming-groep maken](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg1.png)

2. Geef op de pagina **Instellingen voor gegevensbeveiliging groep opgeven** een naam voor de groep en selecteert u de configuratieserver waarop u wilt maken van de groep.

    ![Instellingen voor beveiliging](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg2.png)

3. Configureer de replicatie-instellingen die worden gebruikt voor alle computers in de groep op de pagina **De replicatie-instellingen opgeven** .

    ![Bescherming groep Replicatie](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg3.png)

4. Instellingen:
    - **Consistentie van meerdere VM**: als u deze op gedeelde toepassingen consistente herstel punten wordt gemaakt op de computers in de groep bescherming. Deze instelling is het meest relevante wanneer alle computers in de groep voor de bescherming van de werkbelasting dezelfde worden uitgevoerd. Alle machines worden op dezelfde plaats worden hersteld. Alleen beschikbaar voor Windows-servers.
    - **Vrijgegeven Productieorder drempel**: meldingen worden gegenereerd wanneer de doorlopende bescherming gegevensreplicatie vrijgegeven Productieorder de geconfigureerde drempelwaarde voor de vrijgegeven Productieorder overschrijdt.
    - **Herstel punt bewaren**: Hiermee geeft u het venster bewaren. Beveiligde computers kunnen naar een willekeurig punt in dit venster worden hersteld.
    - **Toepassing consistent momentopname frequentie**: Hiermee geeft u aan hoe vaak herstel punten met consistente toepassing momentopnamen worden gemaakt.

Als ze op de pagina **Items beveiligd** zijn gemaakt, kunt u de groep bescherming controleren.

## <a name="step-8-set-up-machines-you-want-to-protect"></a>Stap 8: Instellen van de machines die u wilt beveiligen

U moet de mobiliteit-service installeren op de virtuele machines en fysieke servers die u wilt beveiligen. U kunt dit op twee manieren doen:

- Automatisch push en de service te installeren op elke machine van de processerver.
- De service handmatig installeren. 

### <a name="install-the-mobility-service-automatically"></a>De mobiliteit service automatisch installeren

Wanneer u computers aan een groep voor de bescherming van toevoegen wordt de mobiliteit-service automatisch geduwd en op elke computer geïnstalleerd door de processerver. 

**Push installeert automatisch de mobiliteit service op Windows-servers:** 

1. Installeer de nieuwste updates voor de processerver zoals beschreven in [stap 5: meest recente updates installeren](#step-5-install-latest-updates), en zorg ervoor dat het processerver beschikbaar is. 
2. Zorg er netwerkverbinding tussen de broncomputer en de processerver en dat de broncomputer is toegankelijk vanuit de processerver.  
3. Configureer de Windows firewall voor **Bestands- en printerdeling** en **Windows Management Instrumentation**. Onder Windows Firewall-instellingen, selecteert u de optie 'Een app of een functie via Firewall toestaan' en selecteer de toepassingen zoals in de volgende afbeelding wordt weergegeven. Voor computers die deel uitmaken van een domein kunt u het firewallbeleid configureert met een groepsbeleidsobject.

    ![Firewall-instellingen](./media/site-recovery-vmware-to-azure-classic-legacy/push-firewall.png)

4. De account die wordt gebruikt voor het uitvoeren van de push-installatie moet in de groep Administrators op de computer die u wilt beveiligen. Deze referenties worden alleen gebruikt voor de installatie van de mobiliteit service push en u zult ze wanneer u een computer aan een groep bescherming toevoegt bieden.
5. Als de opgegeven account een domeinaccount niet moet u uitschakelen gebruiker RAS-beheer op de lokale computer. U doet dit de registervermelding LocalAccountTokenFilterPolicy DWORD met een waarde van 1 onder HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System toevoegen. De registervermelding toevoegen vanuit een open cmd CLI of powershell en voer **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**. 

**Push wordt automatisch de mobiliteit service installeren op Linux-servers:**

1. Installeer de nieuwste updates voor de processerver zoals beschreven in [stap 5: meest recente updates installeren](#step-5-install-latest-updates), en zorg ervoor dat het processerver beschikbaar is.
2. Zorg er netwerkverbinding tussen de broncomputer en de processerver en dat de broncomputer is toegankelijk vanuit de processerver.  
3. Zorg ervoor dat de account is een Hoofdgebruiker op de bronserver voor Linux.
4. Zorg ervoor dat het bestand/etc/hosts op de bronserver Linux bevat items die de naam van de lokale host worden toegewezen aan IP-adressen die zijn gekoppeld aan alle NIC's.
5. Installeer de meest recente openssh, openssh-server, openssl-pakketten op de computer die u wilt beveiligen.
6. Zorg ervoor dat SSH is ingeschakeld en wordt uitgevoerd op poort 22. 
7. SFTP subsysteem wachtwoord verificatie en in het bestand sshd_config als volgt inschakelen: 

    - a) aanmelden als root.
    - b) Zoek de regel die met **PasswordAuthentication begint**in het bestand /etc/ssh/sshd_config.
    - c) Verwijder de opmerking van de regel en wijzigt u de waarde van 'no' op 'yes'.

        ![Linux-mobiliteit](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push.png)

    - d) Zoek de regel die met het subsysteem begint en verwijder de opmerkingen van de regel.
    
        ![Linux-push-mobiliteit](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push2.png)    

8. Controleer dat de bron machine Linux variant wordt ondersteund. 
 
### <a name="install-the-mobility-service-manually"></a>De mobiliteit service handmatig installeren

De softwarepakketten die bij de installatie van de service mobiliteit zijn op de server verwerken in C:\pushinstallsvc\repository. Aanmelden bij de processerver en de juiste installatiepakket kopiëren naar de bronmachine op basis van de onderstaande tabel:-

| Source besturingssysteem                               | Mobiliteit servicepakket op de server verwerken                                                            |
|---------------------------------------------------    |------------------------------------------------------------------------------------------------------ |
| Windows Server (alleen 64 bits)                          | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe`         |
| CentOS 6.4, 6.5, 6.6 (alleen 64 bits)                    | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz`     |
| SUSE Linux Enterprise Server 11 SP3 (alleen 64 bits)     | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz`|
| Oracle Enterprise Linux 6.4, 6.5 (alleen 64 bits)        | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz`       |


**De mobiliteit service installeren op een Windows-server handmatig**het volgende doen:

1. Het pakket **Microsoft ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe** van process server pad naar de map die wordt vermeld in de bovenstaande tabel op de broncomputer kopiëren.
2. Installeer de service mobiliteit door het uitvoerbare bestand uit te voeren op de broncomputer.
3. Volg de instructies van het installatieprogramma.
4. **Mobiliteit service** als de rol en klik op **volgende**.
    
    ![Mobiliteit service installeren](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install.png)

5. Laat de map installeren als het standaardinstallatiepad en klik op **installeren**.
6. Geef het IP-adres en HTTPS-poort van de configuratieserver in de **Configuratie van Host-Agent** .

    - Als u verbinding via het internet maakt de openbare virtuele IP-adres en opgeven openbare HTTPS eindpunt als de poort.
    - Als u verbinding via VPN maakt geeft het interne IP-adres en poort 443 voor de poort. Verlof **Gebruik HTTPS** gecontroleerd.

    ![Mobiliteit service installeren](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install2.png)

7. De server configuratie wachtwoordzin opgeven en klik op **OK** om de mobiliteit service registreren bij de configuratieserver.

**Uitvoeren vanaf de opdrachtregel:**

1. De wachtwoordzin van de CX kopiëren naar het bestand 'C:\connection.passphrase' op de server en uitvoeren van deze opdracht. In ons voorbeeld CX is 62519 ik 104.40.75.37 en de HTTPS-poort:

    `C:\Microsoft-ASR_UA_8.2.0.0_Windows_PREVIEW_20Mar2015_Release.exe" -ip 104.40.75.37 -port 62519 -mode UA /LOG="C:\stdout.txt" /DIR="C:\Program Files (x86)\Microsoft Azure Site Recovery" /VERYSILENT  /SUPPRESSMSGBOXES /norestart  -usesysvolumes  /CommunicationMode https /PassphrasePath "C:\connection.passphrase"`

**De mobiliteit-service handmatig op een Linux-server installeren**:

1. Kopieer de juiste tar-archief op basis van de tabel hierboven, van de processerver op de broncomputer.
2. Open een shellprogramma en pak het gecomprimeerde tar-archief naar een lokaal pad door het uitvoeren van`tar -xvzf Microsoft-ASR_UA_8.2.0.0*`
3. Maak een bestand passphrase.txt in de lokale map waarin u de inhoud van de tar-archief door te voeren uitgepakt *`echo <passphrase> >passphrase.txt`* van shell.
4. De mobiliteit service installeren door te voeren *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Geef het IP-adres en poort:

    - Als u via internet verbinding met de configuratieserver opgeven welke server virtuele openbare IP-adres en openbare HTTPS eindpunt in `<IP address>` en `<port>`.
    - Als u verbinding via een VPN-verbinding maakt geeft het interne IP-adres en poort 443.

**Uitvoeren vanaf de opdrachtregel**:

1. De wachtwoordzin van de CX kopiëren naar het bestand 'passphrase.txt' op de server en uitvoeren van deze opdrachten. In ons voorbeeld CX is 62519 ik 104.40.75.37 en de HTTPS-poort:

Op een productieserver installeert:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt
 
Installeren op de doelserver:


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt

>[AZURE.NOTE] Als u toevoegt aan een groep voor de bescherming van computers waarop een versie van de dienst mobiliteit vervolgens de push installatie overgeslagen.


## <a name="step-9-enable-protection"></a>Stap 9: Enable bescherming

Beveiliging inschakelen als u wilt u een virtuele machines en fysieke servers toevoegen aan een groep voor de bescherming. Houd rekening met het volgende voordat u begint:

- Virtuele machines worden ontdekt elke 15 minuten en duurt 15 minuten duren voordat ze worden weergegeven in Azure Site herstel na detectie.
- Eveneens kunnen de wijzigingen op de virtuele machine (zoals de installatie van VMware's) maximaal 15 minuten worden bijgewerkt bij het herstellen van de Site.
- U kunt het laatst ontdekt in het veld **Laatste CONTACT op** voor de vCenter server/ESXi host op de pagina **Configuratie van Servers** controleren.
- Als u een groep bescherming is al gemaakt hebben en een vCenter Server of ESXi host na die toevoegen, duurt vijftien minuten voor de portal Azure Site herstel te vernieuwen en voor virtuele machines die worden weergegeven in het dialoogvenster **computers toevoegen aan een groep bescherming** .
- Als u gaan direct wilt met machines toe te voegen aan de groep bescherming zonder te wachten op de geplande discovery, markeer de configuratieserver (Klik niet op deze) en klik op de knop **vernieuwen** .
- Wanneer u virtuele machines of fysieke machines aan een groep voor de bescherming toevoegen, wordt de processerver automatisch duwt en service wordt geïnstalleerd door de mobiliteit op de bronserver als de it is niet geïnstalleerd.
- Zorg dat u hebt uw beveiligde machines ingesteld zoals beschreven in de vorige stap voor de automatische push-mechanisme om te werken.

Machines als volgt toevoegen:

1. Klik op **Items beveiligd** > **Bescherming groep** > **Machines** > **Machines toevoegen**. Beste is het raadzaam dat bescherming groepen uw werklast moeten worden gespiegeld zodat u computers met een bepaalde toepassing in dezelfde groep toevoegen.
2. In **virtuele Machines selecteren** als u de fysieke servers beschermen bent, in de wizard **Toevoegen fysieke Machines** bieden het IP-adres en de beschrijvende naam. Selecteer vervolgens de besturingssysteem-familie.

    ![Midden V-server toevoegen](./media/site-recovery-vmware-to-azure-classic-legacy/physical-protect.png)

3. In **virtuele Machines selecteren** als u VMware virtuele machines beveiligen bent, selecteer een vCenter server die u beheert uw virtuele machines (of de EXSi-host waarop ze werken) en selecteert u de machines.

    ![Midden V-server toevoegen](./media/site-recovery-vmware-to-azure-classic-legacy/select-vms.png) 
4. Selecteer de doelservers master en de opslag voor replicatie en geef aan of de instellingen voor alle werklast moeten worden gebruikt in **Target Resources opgeven** . Selecteer [Premium opslag](../storage/storage-premium-storage.md) tijdens het configureren van de bescherming van de werklast die consistent hoge i/o-prestaties en lage latentie vereisen om intensieve i/o-belasting te hosten. Als u een account premie opslag voor uw werkbelasting schijven gebruiken, moet u het doel van het model van de DS-serie gebruiken. U Premium schijven niet gebruiken met het doel van de DS serie model.

    >[AZURE.NOTE] We ondersteunen niet het verplaatsen van de opslag-accounts die zijn gemaakt met de [nieuwe Azure portal](../storage/storage-create-storage-account.md) over resourcegroepen.

    ![vCenter server](./media/site-recovery-vmware-to-azure-classic-legacy/machine-resources.png)

5. Selecteer de account die u gebruiken wilt voor het installeren van de service mobiliteit op beveiligde computers in **Accounts opgeven** . De referenties nodig zijn voor automatische installatie van de dienst mobiliteit. Als u een account moet u ervoor zorgen geen selecteren instellen u zoals beschreven in stap 2. Houd er rekening mee dat deze account kan niet worden geopend door Azure. Voor Windows moet server de account beheerdersrechten op de bronserver. De account moet voor Linux root.

    ![Linux-referenties](./media/site-recovery-vmware-to-azure-classic-legacy/mobility-account.png)

6. Klik op het vinkje aan computers toe te voegen aan de groep voor de bescherming en het starten van de eerste replicatie voor elke computer. U kunt controleren status op de pagina **taken** .

    ![Midden V-server toevoegen](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs2.png)

7. Bovendien kunt u de status controleren door te klikken op **Items beveiligd** > bescherming groepsnaam > **virtuele Machines** . Nadat de eerste replicatie is voltooid en de machines zijn het synchroniseren van gegevens worden ze **beveiligd** status weergegeven.

    ![Taken van de virtuele machine](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs.png)


### <a name="set-protected-machine-properties"></a>Beveiligde computer eigenschappen

1. Nadat u een machine heeft een **beveiligde** status kunt u de failover-eigenschappen. Selecteer de machine in details over de bescherming en open het tabblad **configureren** .
2. U kunt de naam wijzigen die zal worden gegeven aan de machine in Azure na een failover en de grootte van Azure virtual machine. Ook kunt u de Azure netwerk waarmee de computer zijn na een failover verbonden.

    > [AZURE.NOTE] [Migratie van netwerken](../resource-group-move-resources.md) via resourcegroepen in het hetzelfde abonnement of via abonnementen wordt niet ondersteund voor netwerken die worden gebruikt voor het herstel van de Site te implementeren.

    ![Eigenschappen van de virtuele machine instellen](./media/site-recovery-vmware-to-azure-classic-legacy/vm-props.png)

Houd rekening met:

- De naam van de machine Azure voldoen Azure eisen.
- Standaard zijn niet gerepliceerde virtuele machines in Azure verbonden met een netwerk Azure. Als u gerepliceerde moet virtuele machines te communiceren hetzelfde Azure netwerk ingesteld.
- Als u het formaat van een volume op een VMware virtuele machine of fysieke server gaat in een kritieke toestand. Als u hoeft het formaat te wijzigen, als volgt:

    - a) de grootte-instelling wijzigen.
    - b) Selecteer de virtuele machine op het tabblad **virtuele Machines** en klik op **verwijderen**.
    - c) in een **virtuele Machine verwijderen** de optie **uitschakelen bescherming (gebruik voor herstel inzoomen en volume-formaat)**. Deze optie schakelt bescherming maar behoudt de punten herstel in Azure.

        ![Eigenschappen van de virtuele machine instellen](./media/site-recovery-vmware-to-azure-classic-legacy/remove-vm.png)

    - d) inschakelen voor de virtuele machine. Wanneer u beveiliging inschakelen wordt de gegevens voor het nieuwe formaat volume overgemaakt naar Azure.

    

## <a name="step-10-run-a-failover"></a>Stap 10: Failover uitvoeren

U kunt momenteel alleen niet-geplande failover voor beveiligde VMware virtuele machines en fysieke servers uitvoeren. Let op het volgende:



- Voordat u een failover starten, zorg ervoor dat de doelservers model en configuratie actief en gezond. Failover, anders mislukt.
- Bron machines niet worden afgesloten als onderdeel van een niet-geplande failover. Een niet-geplande failover uitvoeren stopt voor de beveiligde servers repliceren. U moet de machines van de groep voor de bescherming te verwijderen en opnieuw toevoegen om te starten met het beveiligen van computers opnieuw na voltooiing van de niet-geplande failover.
- Als u een failover uitvoeren wilt zonder verlies van gegevens, moet de primaire site virtuele machines worden uitgeschakeld voordat u de failover-functie starten.

1. Op de **Recovery plan** pagina en voeg een herstelplan. Geef de details voor het plan en selecteer het doel **Azure** .

    ![Herstelplan configureren](./media/site-recovery-vmware-to-azure-classic-legacy/rplan1.png)

2. Selecteer een groep voor de bescherming en machines selecteert in de groep toe te voegen aan het herstelplan in **virtuele Machine selecteren** . [Lees meer](site-recovery-create-recovery-plans.md) over de herstelplannen.

    ![Virtuele machines toevoegen](./media/site-recovery-vmware-to-azure-classic-legacy/rplan2.png)

3. Indien nodig kunt u het plan voor het maken van groepen en de reeks de order in welke computers bij het herstellen van het plan worden overgenomen. U kunt ook aanwijzingen voor handmatige handelingen en scripts toevoegen. Bij het herstellen van Azure scripts kunnen worden toegevoegd met behulp van [Azure automatisering Runbooks](site-recovery-runbook-automation.md).

4. Selecteer het plan in de pagina **Herstel plannen** en klikt u op **Niet-geplande Failover**.
5. Controleer of de failover-richting (naar Azure) in een **Failover-bevestigen** en selecteer het herstelpunt voor failover.
6. Wacht totdat de failover-taak te voltooien en controleer vervolgens of dat de overname werkten zoals verwacht en dat de gerepliceerde virtuele machines worden gestart in Azure.




## <a name="step-11-fail-back-failed-over-machines-from-azure"></a>Stap 11: Fouten weer overgenomen machines van Azure

[Meer informatie](site-recovery-failback-azure-to-vmware-classic-legacy.md) over hoe u de mislukte brengen via computers Azure terug naar uw omgeving op gebouwen.


## <a name="manage-your-process-servers"></a>De proces-servers beheren

De processerver replicatie verzendt naar de master-doelserver in Azure en ontdekt nieuwe VMware virtuele machines met een vCenter server toegevoegd. In de volgende gevallen kunt u wijzigen van de server verwerken in uw implementatie:

- Als het huidige processerver uitvalt
- Als uw herstel punt doelstelling (vrijgegeven Productieorder) naar een onaanvaardbaar voor uw organisatie stijgt.

Indien nodig kunt u de replicatie van sommige of alle fysieke servers op ruimten VMware virtuele machines en verplaatsen naar een ander proces-server. Bijvoorbeeld:

- **Fout**: als een processerver mislukt of niet beschikbaar is, kunt u beveiligde machine replicatie verplaatsen naar processerver met een ander. Metagegevens van de broncomputer en de replica machine wordt verplaatst naar het nieuwe processerver en de gegevens opnieuw wordt gesynchroniseerd. Het nieuwe processerver maakt automatisch verbinding met de vCenter server voor automatische detectie. U kunt de status van de servers op de dashboard-Site-Recovery proces controleren.
- **Aanpassen van de vrijgegeven Productieorder voor taakverdeling**, voor een betere taakverdeling u selecteert een ander proces-server in de portal-Site herstellen en replicatie van een of meer computers aan voor taakverdeling handmatig verplaatsen. In dit geval wordt de metagegevens van de geselecteerde bron en replica machines verplaatst naar het nieuwe processerver. De oorspronkelijke processerver blijft verbonden met de vCenter server. 

### <a name="monitor-the-process-server"></a>De processerver controleren

Als een processerver zich in een kritieke toestand wordt een waarschuwing weergegeven op de Dashboard-Site herstellen. U kunt klikken op het tabblad gebeurtenissen openen en vervolgens inzoomen op specifieke taken op het tabblad taken van de status. 

### <a name="modify-the-process-server-used-for-replication"></a>De processerver gebruikt voor replicatie wijzigen

1. **Servers**Open > **Configuration Servers** > configuratieserver > **Server-gegevens**.
2. Klik op **Proces-Servers** > **Proces-Server wijzigen** naast de server die u wilt wijzigen.

    ![Process-Server 1 wijzigen](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps1.png)

3. In het **Proces-Server wijzigen** > **Proces doelserver** selecteert u de nieuwe server die u wilt gebruiken en selecteer vervolgens de virtuele machines die u wilt repliceren naar de nieuwe server. Klik op het informatiepictogram naast de naam van de server voor meer informatie over de beschikbare ruimte en gebruikte geheugen. De gemiddelde ruimte die nodig is voor het repliceren van elke geselecteerde virtuele machine naar het nieuwe processerver wordt weergegeven zodat u de werklast van beslissingen.

    ![Process-Server 2 wijzigen](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps2.png)

4. Klik op het vinkje om te repliceren naar de nieuwe processerver beginnen. Opmerking Als u alle virtuele machines van een processerver die kritiek er een kritieke waarschuwing niet meer in het dashboard weergegeven moet.


## <a name="third-party-software-notices-and-information"></a>Software van derden mededelingen en bekendmakingen

Niet vertalen of lokaliseren

De software en firmware in het Microsoft-product of de service wordt uitgevoerd op basis van of uitgerust met materiaal uit de onderstaande projecten (gezamenlijk "derden Code").  Microsoft is niet de oorspronkelijke auteur van de Code van derden.  De oorspronkelijke auteursrechtaanduiding en licentie, waaronder Microsoft dergelijke Code derden ontvangen ingesteld hierna anders wordt bepaald.

De informatie in de sectie A is met betrekking tot onderdelen van derden Code uit de onderstaande projecten. Deze licenties en informatie dienen alleen ter informatie.  Deze Code van derden, is aan u door Microsoft onder de licentievoorwaarden voor Microsoft software voor het Microsoft-product of de service wordt relicensed.  

De informatie in de sectie B wordt met betrekking tot onderdelen van derden Code die beschikbaar worden gesteld aan u door Microsoft onder de oorspronkelijke licentievoorwaarden.

Het volledige dossier kan worden gevonden op het [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft behoudt zich alle rechten voor die niet uitdrukkelijk in deze overeenkomst worden verleend door implicatie, ' estoppel ' of anderszins.
