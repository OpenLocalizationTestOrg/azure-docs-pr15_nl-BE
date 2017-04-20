<properties 
    pageTitle="Agent Linux User Guide | Microsoft Azure" 
    description="Informatie over het installeren en configureren van de Linux-Agent (waagent) voor het beheren van interactie met Azure Fabric-Controller van de virtuele machine." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="szark"/>



# <a name="azure-linux-agent-user-guide"></a>Azure Linux Agent-gebruikershandleiding

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="introduction"></a>Inleiding

Microsoft Azure Linux Agent (waagent) beheert Linux & FreeBSD inrichten en interactie met de Controller van de Fabric Azure VM. Biedt de volgende functionaliteit voor Linux en FreeBSD IaaS implementaties:

> [AZURE.NOTE] Zie de agent Azure Linux [Leesmij-bestand](https://github.com/Azure/WALinuxAgent/blob/master/README.md) voor meer informatie.

* **Inrichten van afbeelding**
  - Het maken van een gebruikersaccount
  - SSH verificatietypen configureren
  - Implementatie van openbare SSH-sleutels en sleutelparen
  - De hostnaam instellen
  - Publiceren van de hostnaam voor DNS-platform
  - SSH host key fingerprint rapportage aan het platform
  - Resource-Schijfbeheer
  - Opmaak en de bron schijf koppelen
  - Swap ruimte configureren

* **Netwerken**
  - Routes voor een betere compatibiliteit met platform DHCP-servers beheren
  - Zorgt voor de stabiliteit van de naam van de netwerk-interface

* **Kernel**
  - Hiermee configureert u de virtuele NUMA (voor kernel uitschakelen < 2.6.37)
  - Hyper-V-entropie voor /dev/random verbruikt
  - Hiermee configureert u de SCSI-bewerkingen voor het root-apparaat (die kan)

* **Diagnostische gegevens**
  - Consoleomleiding naar seriële poort

* **SCVMM implementaties**
    - Detecteert en de VMM agent voor Linux bootstraps als uitgevoerd in een omgeving met System Center Virtual Machine Manager 2012 R2

* **VM-extensie**
    - Injecteer onderdeel geschreven door Microsoft en Partners in Linux VM (IaaS) zodat software en configuratie-automatisering
    - VM-extensie referentie implementatie op [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)


## <a name="communication"></a>Communicatie
De informatiestroom naar de agent van het platform gebeurt via twee kanalen:

* Opstarten-DVD voor implementaties IaaS gekoppeld. Deze DVD bevat een configuratiebestand OVF-compatibel met alle inrichten informatie dan de werkelijke SSH-keypairs.
* Een TCP-eindpunt bloot een REST API gebruikt voor het verkrijgen van de implementatie en topologieconfiguratie.


## <a name="requirements"></a>Vereisten
De volgende systemen zijn getest en werken met de Agent Azure Linux bekend zijn:

> [AZURE.NOTE] Deze lijst kan afwijken van de officiële lijst van ondersteunde systemen op het Microsoft Azure-Platform, zoals hier beschreven: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)

* CoreOS
* CentOS 6.3 +
* Red Hat Enterprise Linux 6.7 +
* + Debian 7.0
* Ubuntu 12.04 +
* openSUSE 12.3 +
* SLES 11 SP3 +
* Oracle Linux 6.4 +

Andere ondersteunde systemen:

* FreeBSD 10 + (Azure Linux Agent v2.0.10 +)

De Linux-agent afhankelijk van sommige systeempakketten goed:

* Python 2.6 +
* OpenSSL 1.0 +
* OpenSSH 5.3 +
* Bestandssysteem hulpprogramma's: sfdisk, fdisk, mkfs, gescheiden
* Extra wachtwoord: chpasswd, sudo
* Verwerken van extra tekst: sed, grep
* Netwerk tools: IP-route
* Kernel-ondersteuning voor UDF filesystems koppelen.

## <a name="installation"></a>Installatie
Installatie met behulp van een RPM of een DEB pakket in de distributie van pakket opslagplaats is de voorkeursmethode voor het installeren en upgraden van de Agent Azure Linux. Alle de [distributie providers geviseerd](virtual-machines-linux-endorsed-distros.md) integreren met de agent Azure Linux pakket hun afbeeldingen en opslagplaatsen.

Raadpleeg de documentatie in de [Linux-Agent Azure repo op Github](https://github.com/Azure/WALinuxAgent) voor geavanceerde installatie-opties, zoals het installeren van de bron of naar aangepaste locaties of voorvoegsels.


## <a name="command-line-options"></a>Opdrachtregelopties

### <a name="flags"></a>Vlaggen

- uitgebreide: vastgelegd van de opgegeven opdracht
- forceren: interactieve bevestiging voor bepaalde opdrachten overslaan

### <a name="commands"></a>Opdrachten

- Help: geeft een overzicht van de ondersteunde opdrachten en vlaggen.

- identiteitsgegevens: poging tot het systeem schoon, waardoor het geschikt is voor het opnieuw toewijzen van bronnen. Deze bewerking verwijderd het volgende:
 * Alle SSH host sleutels (als Provisioning.RegenerateSshHostKeyPair 'y' in het configuratiebestand)
 * Configuratie van de NameServer in /etc/resolv.conf
 * Root-wachtwoord van/etc/shadow (als Provisioning.DeleteRootPassword 'y' in het configuratiebestand)
 * In de cache DHCP-clientleases
 * Hiermee stelt u de hostnaam localhost.localdomain


> [AZURE.WARNING] Deprovisioning kan niet garanderen dat de afbeelding van alle gevoelige informatie gewist en geschikt voor herdistributie is.


- identiteitsgegevens + user: alles onder - identiteitsgegevens (boven) wordt uitgevoerd en ook Hiermee verwijdert u de laatste ingerichte account (afkomstig van /var/lib/waagent) en gegevens die zijn gekoppeld. Deze parameter is bij het verwijderen van gegevens van een afbeelding die eerder is ingericht op Azure zodat het kan worden vastgelegd en opnieuw worden gebruikt.

- versie: geeft de versie van waagent

- serialconsole: WORMGATEN ttyS0 markeren als u wilt configureren (de eerste seriële poort) als de console te starten. Dit zorgt ervoor dat de kernel bootup logboeken zijn verzonden naar de seriële poort en beschikbaar gesteld voor het opsporen van fouten in.

- daemon: waagent uitvoeren als een daemon voor het beheren van interactie met het platform. Dit argument is opgegeven voor waagent in het waagent init script.

- Start: waagent achtergrond worden uitgevoerd


## <a name="configuration"></a>Configuratie

Een configuratiebestand (/ etc/waagent.conf) bepaalt de acties van de waagent. Hieronder vindt u een voorbeeld configuratiebestand:

    Provisioning.Enabled=y
    Provisioning.DeleteRootPassword=n
    Provisioning.RegenerateSshHostKeyPair=y
    Provisioning.SshHostKeyPairType=rsa
    Provisioning.MonitorHostName=y
    Provisioning.DecodeCustomData=n
    Provisioning.ExecuteCustomData=n
    Provisioning.PasswordCryptId=6
    Provisioning.PasswordCryptSaltLength=10
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.MountOptions=None
    ResourceDisk.EnableSwap=n
    ResourceDisk.SwapSizeMB=0
    LBProbeResponder=y
    Logs.Verbose=n
    OS.RootDeviceScsiTimeout=300
    OS.OpensslPath=None
    HttpProxy.Host=None
    HttpProxy.Port=None

De verschillende configuratieopties worden hieronder uitvoerig beschreven. Configuratie-opties zijn drie soorten; Boolean, tekenreeks of geheel getal. De Booleaanse opties kunnen worden opgegeven als "y" of "n". Het speciale sleutelwoord "None" mag worden gebruikt voor sommige string type configuratie posten zoals hieronder.

**Provisioning.Enabled:**  
Type: Boolean  
Standaard: y

Hierdoor kan de gebruiker of de inrichting functionaliteit in de agent uit te schakelen. Geldige waarden zijn "y" of "n". Inrichten is uitgeschakeld, SSH host- en sleutels in de afbeelding worden behouden als elke configuratie die is opgegeven in de API-aanbod Azure wordt genegeerd.

> [AZURE.NOTE] De `Provisioning.Enabled` parameter standaard ingesteld op "n" Ubuntu wolk afbeeldingen die cloud init voor de inrichting gebruiken.

  
**Provisioning.DeleteRootPassword:**  
Type: Boolean  
Standaard: n

Als u tijdens het inrichten, stelt het root-wachtwoord in het bestand/etc/shadow gewist.


**Provisioning.RegenerateSshHostKeyPair:**  
Type: Boolean  
Standaard: y

Als u tijdens het inrichten van/etc/ssh/set, alle SSH host sleutelparen (ecdsa dsa en rsa) verwijderd. En één nieuwe combinatie van een sleutel wordt gegenereerd.

Het coderingstype voor de verse sleutelpaar kan worden geconfigureerd door de vermelding Provisioning.SshHostKeyPairType. Houd er rekening mee dat sommige distributies opnieuw SSH sleutelparen voor elke ontbrekende coderingstypen gemaakt worden wanneer de SSH-daemon opnieuw wordt opgestart (bijvoorbeeld na het opnieuw opstarten).


**Provisioning.SshHostKeyPairType:**  
Type: String  
Standaard: rsa

Dit kan worden ingesteld op een type codering algoritme die wordt ondersteund door de SSH daemon op de virtuele machine. De algemeen ondersteunde waarden zijn "rsa", "dsa" en "ecdsa". Houd er rekening mee dat 'putty.exe' in Windows "ecdsa" niet ondersteunt. Dus als u putty.exe op Windows met verbinding maken met een Linux-distributie wilt, gebruik "rsa" of "dsa".


**Provisioning.MonitorHostName:**  
Type: Boolean  
Standaard: y

Als ingesteld, waagent wordt de Linux virtuele machine voor hostname wijzigingen controleren (zoals die wordt geretourneerd door de opdracht 'hostname') en de netwerkconfiguratie in de afbeelding aan de wijziging automatisch bijwerken. Om de naamswijziging van push naar de DNS-servers, wordt netwerk opgestart in de virtuele machine. Hierdoor wordt in het kort verlies van internetconnectiviteit.


**Provisioning.DecodeCustomData**  
Type: Boolean  
Standaard: n

Indien ingesteld, waagent wordt gedecodeerd CustomData in Base64.


**Provisioning.ExecuteCustomData**  
Type: Boolean  
Standaard: n

Als de instelt, worden waagent CustomData na het inrichten uitgevoerd.


**Provisioning.PasswordCryptId**  
Type: String  
Standaard: 6

Het algoritme dat wordt gebruikt door crypt bij het genereren van de hash van het wachtwoord.  
 1 - MD5  
 2a - Blowfish  
 5 - SHA-256  
 6 - SHA 512  


**Provisioning.PasswordCryptSaltLength**  
Type: String  
Standaard: 10

Lengte van willekeurige zout gebruikt bij het genereren van de hash van het wachtwoord.


**ResourceDisk.Format:**  
Type: Boolean  
Standaard: y

Als instelt, de resource-schijf die door het platform worden opgemaakt en gemonteerd door middel van waagent als het type bestandssysteem is aangevraagd door de gebruiker in 'ResourceDisk.Filesystem' iets anders dan 'ntfs is'. Een enkele partitie van het type Linux (83) zullen beschikbaar worden gesteld op de schijf. Houd er rekening mee dat deze partitie niet worden geformatteerd als deze met succes kan worden gemonteerd.


**ResourceDisk.Filesystem:**  
Type: String  
Standaard: ext4

Hiermee geeft u het type bestandssysteem voor de schijf van de resource. Ondersteunde waarden afhankelijk van de Linux-distributie. Als de tekenreeks X vervolgens mkfs is. X moet aanwezig zijn op de Linux-afbeelding. Meestal gebruik afbeeldingen SLES 11 'ext3'. FreeBSD afbeeldingen te hier 'ufs2' gebruiken.


**ResourceDisk.MountPoint:**  
Type: String  
Standaard: mnt/resource 

Hiermee geeft u het pad waarin de resource-schijf is gekoppeld. Let op de bron schijf is een *tijdelijke* schijf en kan worden leeggemaakt wanneer de VM is deprovisioned.


**ResourceDisk.MountOptions**  
Type: String  
Standaard: geen

Hiermee geeft u opties voor Schijfcontrole koppelpunt wordt doorgegeven aan de opdracht mount -o. Dit is een door komma's gescheiden lijst met waarden, ex. 'nodev, nosuid'. Zie mount(8) voor meer informatie.


**ResourceDisk.EnableSwap:**  
Type: Boolean  
Standaard: n

Als een swapbestand instellen (/ wisselbestand) is op de schijf resource gemaakt en toegevoegd aan het systeem swap-ruimte.


**ResourceDisk.SwapSizeMB:**  
Type: geheel getal  
Standaard: 0

De grootte van het wisselbestand in megabytes.


**Logs.Verbose:**  
Type: Boolean  
Standaard: n

Als u set gegevens opnemen in logboek wordt versterkt. Waagent meldt zich bij de /var/log/waagent.log en maakt gebruik van de functionaliteit van de logrotate systeem logs draaien.


**OS. EnableRDMA**  
Type: Boolean  
Standaard: n

Als stelt de agent probeert te installeren en vervolgens een RDMA kernel-stuurprogramma dat overeenkomt met de versie van de firmware van de onderliggende hardware laden.


**OS. RootDeviceScsiTimeout:**  
Type: geheel getal  
Standaard: 300

Hiermee wordt de SCSI-time-out in seconden op de OS-stations voor schijven en gegevens. Als niet is ingesteld, wordt het systeem standaardwaarden worden gebruikt.


**OS. OpensslPath:**  
Type: String  
Standaard: geen

Dit kan worden gebruikt om aan te geven een alternatief pad voor de openssl binaire voor cryptografische bewerkingen.


**HttpProxy.Host, HttpProxy.Port**  
Type: String  
Standaard: geen

Als de instelt, wordt de agent deze proxyserver toegang tot het internet gebruikt. 


## <a name="ubuntu-cloud-images"></a>Ubuntu wolk afbeeldingen

Houd er rekening mee dat Ubuntu wolk afbeeldingen [wolk init](https://launchpad.net/ubuntu/+source/cloud-init) om uit te voeren bij veel configuratietaken die anders zouden worden beheerd door de Agent Azure Linux gebruiken.  Opmerking de volgende verschillen:


- **Provisioning.Enabled** standaard "n" Ubuntu wolk afbeeldingen die gebruikmaken van cloud-init provisioning taken uit te voeren.

- De volgende configuratieparameters hebben geen effect op Ubuntu wolk afbeeldingen die cloud init gebruiken voor het beheren van de bron schijf en swap ruimte:

 - **ResourceDisk.Format**
 - **ResourceDisk.Filesystem**
 - **ResourceDisk.MountPoint**
 - **ResourceDisk.EnableSwap**
 - **ResourceDisk.SwapSizeMB**

- Raadpleeg de volgende bronnen op het koppelpunt van de bron schijf configureren en swap ruimte op Ubuntu wolk afbeeldingen tijdens het inrichten:

 - [Ubuntu Wiki: Swap-partities configureren](http://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
 - [Injecteren van aangepaste gegevens in een Azure Virtual Machine](virtual-machines-windows-classic-inject-custom-data.md)

