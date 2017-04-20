 <properties
   pageTitle="Azure en Linux | Microsoft Azure"
   description="Beschrijving van services Azure berekenen, opslag en netwerken met Linux virtuele machines."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines-linux"
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/14/2016"
   ms.author="v-livech"/>

# <a name="azure-and-linux"></a>Azure en Linux

Microsoft Azure is een groeiende verzameling van geïntegreerde openbare cloud-services, waaronder analytics, virtuele Machines, databases, mobiele, netwerk, opslag, en het web — ideaal voor het hosten van uw oplossingen.  Microsoft Azure biedt een schaalbaar platform waarmee u betaalt alleen voor wat u kunt, als u wilt - zonder te investeren in op-ruimten hardware.  Azure is gereed wanneer u uw oplossingen om uit te breiden en uit op welke schaal u nodig hebt om de behoeften van uw klanten.

Als u bekend met de diverse functies van Amazon bent (AWS), kunt u de vs Azure AWS [definitie toewijzing document](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/)controleren.


## <a name="regions"></a>Regio 's
Microsoft Azure resources zijn verdeeld over verschillende geografische regio's over de hele wereld.  Een 'regio' staat voor meerdere datacenters in één geografisch gebied.  Vanaf 1 januari 2016, dit geldt ook voor: 8 in Amerika, Europa, in Azië/Pacific, 2 in vasteland China en India 3 6 2.  Als u wilt dat een volledige lijst van alle gebieden van Azure, onderhouden we dat een lijst van bestaande en nieuwe regio's aangekondigd.

- [Azure regio 's](https://azure.microsoft.com/regions/)

## <a name="availability"></a>Beschikbaarheid
Om ervoor te zorgen dat uw implementatie in aanmerking komt voor onze 99.95 VM Service Level Agreement, moet u voor de implementatie van twee of meer VMs uw werkbelasting in de beschikbaarheid van een actief ingesteld. Hierdoor worden uw VMs zijn verdeeld over meerdere domeinen voor fouttolerantie in onze datacenters als geïmplementeerd op hosts met windows verschillende onderhoud. De volledige [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) wordt uitgelegd dat de gegarandeerde beschikbaarheid van Azure als geheel.

## <a name="azure-virtual-machines--instances"></a>Azure virtuele Machines & instanties
Microsoft Azure ondersteunt de uitvoering van een aantal populaire Linux-distributies geleverd en beheerd door een aantal partners.  U vindt distributies zoals Red Hat Enterprise, CentOS, Debian, Ubuntu, CoreOS, RancherOS, FreeBSD, en meer in de markt Azure. We werken actief met verschillende Linux Gemeenschappen flavors nog meer toevoegen aan de lijst met [dat Azure Linux Distros geviseerd](virtual-machines-linux-endorsed-distros.md) .

Als uw voorkeur Linux distro van keuze niet die zich momenteel in de galerie, kunt u 'overbrengen van uw eigen Linux"VM door te [maken en het uploaden van een Linux-VHD in Azure](virtual-machines-linux-create-upload-generic.md).

Azure virtuele machines kunt u een breed scala aan oplossingen voor computergebruik in een flexibele manier implementeren. U kunt vrijwel elke werklast en elke taal op vrijwel elk besturingssysteem - Windows, Linux, implementeren of een aangepaste gemaakt bij een van onze groeiende lijst van partners. Nog steeds niet ziet wat u zoekt?  Maak je geen zorgen - u kunt ook uw eigen afbeeldingen overbrengen op ruimten.

## <a name="vm-sizes"></a>VM-formaten
Wanneer u een VM in Azure implementeert, gaat u in één van onze reeks formaten die geschikt is voor uw werkbelasting VM grootte selecteren. De grootte is ook van invloed op de verwerking macht, geheugen en opslag capaciteit van de virtuele machine. Afschrijving op basis van de hoeveelheid tijd die de VM wordt uitgevoerd en de toegewezen bronnen verbruiken. Een volledige lijst van [de omvang van virtuele Machines](virtual-machines-linux-sizes.md).

Hier volgen enkele richtlijnen voor het selecteren van een VM-grootte van een van onze reeks (A, D, DS, G en GS).

* A-serie VMs zijn onze waarde prijs op instapniveau VMs voor lichte werkbelasting en Dev/Testscenario's. Algemeen beschikbaar zijn in alle regio's kunnen en verbinden en met alle standaard bronnen beschikbaar voor virtuele machines.
* Een reeks formaten (A8 - A11) zijn speciale compute intensieve configuraties voor high performance computing clustertoepassingen geschikt.
* VMs D-reeks zijn ontworpen voor het uitvoeren van toepassingen die meer rekenkracht en prestaties van de tijdelijke schijf. D-reeks VMs bieden snellere processors een hogere geheugen-core-verhouding en een solid-state drive (SSD) voor de tijdelijke schijf.
* Dv2-serie is de nieuwste versie van onze D-serie, uitgerust met een krachtige CPU. De CPU Dv2-serie is ongeveer 35% sneller dan de CPU van de D-reeks. Is gebaseerd op de nieuwste 2,4 GHz Intel Xeon® E5-2673 v3 (Haskell)-processor en met Intel Turbo Boost technologie 2.0 3,2 GHz kunt gaan. De Dv2-serie heeft het dezelfde geheugen en schijfruimte configuraties zoals de D-reeks.
* VMs G-serie bieden het meeste geheugen en op hosts met Intel Xeon E5 V3 familie processors worden uitgevoerd.

Opmerking: DS-serie en GS serie VMs hebben toegang tot Premium opslag - back onze SSD-opslag voor hoge prestaties, lage latentie voor i/o-intensieve belasting. Premium-opslag is beschikbaar in bepaalde regio's. Zie voor meer informatie:

- [Premium-opslagruimte: Krachtige opslag voor Azure VM werkbelasting](../storage/storage-premium-storage.md)

## <a name="automation"></a>Automatisering
Voor een goede cultuur van DevOps, moet de infrastructuur voor alle code.  Wanneer alle de infrastructuur woont in code die kan eenvoudig worden opnieuw gemaakt (Phoenix-Servers).  Azure werkt met grote automatisering gereedschap zoals Ansible, chef-kok, SaltStack en Puppet.  Azure heeft ook een eigen tooling voor automatisering:

- [Azure-sjablonen](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

- [Azure VMAccess](virtual-machines-linux-using-vmaccess-extension.md)

Azure is ondersteuning voor [cloud-init](http://cloud-init.io/) rolt over de meeste Linux Distros die dit ondersteunen.  De Canonical Ubuntu VMs worden momenteel geïmplementeerd met cloud-init is standaard ingeschakeld.  RedHats RHEL, CentOS en Fedora wolk init ondersteunen, echter de Azure afbeeldingen onderhouden door RedHat geen cloud-init geïnstalleerd.  Cloud-init op een RedHat familie OS gebruikt, moet u een aangepaste afbeelding maken met cloud-init geïnstalleerd.

- [Met behulp van de wolk init op Azure Linux VMs](virtual-machines-linux-using-cloud-init.md)

## <a name="quotas"></a>Quota 's
Elk abonnement Azure heeft Standaardquotalimieten die invloed op de implementatie van een groot aantal VMs voor uw project hebben kan. De huidige limiet op basis van per abonnement is 20 VMs per regio.  Schijfquotumlimieten toewijzen kunnen worden verhoogd door het indienen van een support ticket aanvragen van een verhoging van de limiet.  Voor meer informatie over schijfquotumlimieten toewijzen:

- [Azure Service-abonnementen](../azure-subscription-service-limits.md)


## <a name="partners"></a>Partners

Microsoft werkt nauw samen met onze partners om de beschikbare afbeeldingen worden bijgewerkt en is geoptimaliseerd voor een Azure runtime.  Controleer de marketplace's hieronder voor meer informatie over onze partners.

- [Linux op Azure geviseerd verdelingen](virtual-machines-linux-endorsed-distros.md)

RedHat - [Azure Marketplace - Red Hat Enterprise Linux 7.2](https://azure.microsoft.com/marketplace/partners/redhat/redhatenterpriselinux72/)

Canonieke - [Azure Marktonderzoek - TNS Ubuntu Server 16.04](https://azure.microsoft.com/marketplace/partners/canonical/ubuntuserver1604lts/)

Debian - [Azure Marketplace - Debian 8 'Jessie'](https://azure.microsoft.com/marketplace/partners/credativ/debian8/)

FreeBSD - [Azure Marketplace - FreeBSD 10.3](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)

CoreOS - [Azure Marketplace - CoreOS (stabiel)](https://azure.microsoft.com/marketplace/partners/coreos/coreosstable/)

RancherOS - [Azure Marketplace - RancherOS](https://azure.microsoft.com/marketplace/partners/rancher/rancheros/)

Bitnami - [Bibliotheek voor Azure Bitnami](https://azure.bitnami.com/)

Mesosphere - [Azure Marketplace - Mesosphere DC/OS op Azure](https://azure.microsoft.com/marketplace/partners/mesosphere/dcosdcos/)

Docker - [Azure Marketplace - Azure Container Service met Docker Swarm](https://azure.microsoft.com/marketplace/partners/microsoft/acsswarms/)

Jenkins - [Marketplace Azure - Platform CloudBees Jenkins](https://azure.microsoft.com/marketplace/partners/cloudbees/jenkins-platformjenkins-platform/)


## <a name="getting-setup-on-azure"></a>Uitvoeren van Setup op Azure
U moet een account Azure, de CLI Azure is geïnstalleerd en een paar SSH openbare en persoonlijke sleutels met Azure wilt beginnen.

## <a name="sign-up-for-an-account"></a>Aanmelden voor een account
De eerste stap bij het gebruik van de Cloud Azure is aanmelden voor een account met Azure.  Ga naar de aanmeldingspagina van [Azure Account](https://azure.microsoft.com/pricing/free-trial/) aan de slag.

## <a name="install-the-cli"></a>De CLI installeren
Met uw nieuwe account Azure, kunt u begint onmiddellijk met de Azure portal een web-based admin paneel is.  De Azure Cloud via de opdrachtregel beheren, installeert u de `azure-cli`.  De [Azure CLI ](../xplat-cli-install.md)installeren op uw Mac of Linux-werkstation.

## <a name="create-an-ssh-key-pair"></a>Maak een SSH-sleutelpaar
U hebt nu een account Azure, het webportaal Azure en de CLI Azure.  De volgende stap is het maken van een SSH-sleutelpaar dat wordt gebruikt voor SSH in Linux zonder een wachtwoord te gebruiken.  [Maak SSH sleutels op Linux en Mac](virtual-machines-linux-mac-create-ssh-keys.md) wachtwoord-loze logins en betere beveiliging inschakelen.

## <a name="getting-started-with-linux-on-microsoft-azure"></a>Aan de slag met Linux op Microsoft Azure
Met uw accountconfiguratie Azure de Azure CLI geïnstalleerd en SSH-sleutels gemaakt zijn u nu klaar om te gaan met het bouwen van een infrastructuur in de wolk Azure.  De eerste taak is het maken van een paar van VMs.

## <a name="create-a-vm-using-the-cli"></a>Maak een VM met behulp van de CLI
Maken van een Linux VM met behulp van de CLI is een snelle manier een VM implementeren zonder de terminal die in werkt.  Alles die op het webportaal kunt u is via een opdrachtregel vlag of switch beschikbaar.  

- [Maak een Linux VM met behulp van de CLI](virtual-machines-linux-quick-create-cli.md)

## <a name="create-a-vm-in-the-portal"></a>Een VM maken in de portal
Maken van een Linux VM in Azure webportal is een manier om gemakkelijk en op door middel van de verschillende opties die naar een implementatie.  In plaats van het opdrachtregelprogramma vlaggen of schakelopties gebruikt, bent u wel een mooie website lay-out van diverse opties en instellingen.  Alles via de opdrachtregelinterface beschikbaar is ook beschikbaar in de portal.

- [Maak een Linux VM met behulp van de Portal](virtual-machines-linux-quick-create-portal.md)

## <a name="login-using-ssh-without-a-password"></a>Login via SSH zonder een wachtwoord.
Op Azure VM wordt uitgevoerd en u aan te melden.  Inloggen via SSH met een wachtwoord is onveilig en tijdrovend.  Met behulp van SSH-sleutels is de veiligste manier, en ook de snelste manier om in te loggen.  Wanneer u u Linux VM via de portal of de CLI maakt, hebt u twee opties voor verificatie.  Als u een wachtwoord voor SSH, configureert Azure VM zodat aanmeldingen via wachtwoorden.  Als u een openbare SSH-sleutel gebruiken,-Azure VM aanmeldingen via SSH sleutels alleen toestaan als u wilt configureren en aanmeldingen wachtwoord uitgeschakeld. Om veilig uw VM Linux SSH-sleutel aanmeldingen door alleen de SSH openbare sleutel optie gebruiken tijdens het VM maken in de portal of CLI.

- [SSH op uw Linux VM wachtwoorden uitschakelen door SSHD configureren](virtual-machines-linux-mac-disable-ssh-password-usage.md)

## <a name="related-azure-components"></a>Azure-gerelateerde onderdelen

## <a name="storage"></a>Opslag

- [Inleiding tot Microsoft Azure opslag](../storage/storage-introduction.md)

- [Een schijf toevoegen aan een Linux VM met de azure-cli](virtual-machines-linux-add-disk.md)

- [Het koppelen van een schijf met gegevens aan een Linux VM in Azure portal](virtual-machines-linux-attach-disk-portal.md)

## <a name="networking"></a>Netwerken

- [Virtueel netwerk-overzicht](../virtual-network/virtual-networks-overview.md)

- [IP-adressen in Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md)

- [Poorten openen naar een Linux VM in Azure](virtual-machines-linux-nsg-quickstart.md)

- [Een volledig gekwalificeerde domeinnaam te maken in de portal voor Azure](virtual-machines-linux-portal-create-fqdn.md)


## <a name="containers"></a>Containers

- [Virtuele Machines en Containers in Azure](virtual-machines-linux-containers.md)

- [Azure Container Service-Inleiding](../container-service/container-service-intro.md)

- [Een cluster Azure Container Service implementeren](../container-service/container-service-deployment.md)

## <a name="next-steps"></a>Volgende stappen

U hebt nu een overzicht van Linux op Azure.  De volgende stap is om zelf aan de slag en maken een paar VMs!

- [Maak een Linux VM op Azure met behulp van de Portal](virtual-machines-linux-quick-create-portal.md)

- [Een Linux VM in Azure maken met behulp van de CLI](virtual-machines-linux-quick-create-cli.md)
