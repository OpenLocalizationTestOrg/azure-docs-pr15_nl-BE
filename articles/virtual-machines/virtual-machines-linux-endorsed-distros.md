<properties
    pageTitle="Distributies van Linux geviseerd | Microsoft Azure"
    description="Informatie over Linux op Azure geviseerd distributies, waaronder richtlijnen voor Ubuntu, OpenLogic, Oracle en SUSE."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management,azure-resource-manager"
    />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="szark"/>



#<a name="linux-on-azure-endorsed-distributions"></a>Linux op Azure geviseerd verdelingen

> [AZURE.NOTE] Als u enkele ogenblikken hebt, kunt u ons helpen de Azure Linux VM documentatie verbeteren door middel van deze [Snelle enquête](https://aka.ms/linuxdocsurvey) uw ervaringen. Elk antwoord helpt ons help u uw werk kunt doen krijgt.

De Linux-afbeeldingen in de galerie met Azure of marktplaats worden geleverd met een aantal partners en wij werken met verschillende Linux Gemeenschappen flavors nog meer toevoegen aan de distributielijst met geviseerd. In de tussentijd kunt distributies niet beschikbaar in de galerie u altijd inleveren-uw-eigen-Linux door de richtlijnen op [deze pagina](virtual-machines-linux-classic-create-upload-vhd.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="supported-distributions--versions"></a>Ondersteunde verdelingen & versies ##

De volgende tabel geeft een overzicht van de Linux-distributies en versies die worden ondersteund op Azure. Raadpleeg tevens [ondersteuning voor afbeeldingen in Microsoft Azure Linux](https://support.microsoft.com/en-us/kb/2941892) voor meer gedetailleerde informatie.

De Linux Integration Services (LIS) stuurprogramma's voor Hyper-V en Azure zijn kernelmodules die deel uitmaakt van Microsoft rechtstreeks op de upstream-Linux kernel.  De LIS-stuurprogramma's zijn ofwel ingebouwd in de verdeling van de kernel standaard of oudere RHEL/CentOS-gebaseerde distributies zijn beschikbaar als afzonderlijke download [hier](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409).  Raadpleeg [dit artikel](virtual-machines-linux-create-upload-generic.md#linux-kernel-requirements) voor meer informatie over de LIS-stuurprogramma's.

De Agent Azure Linux al vooraf is geïnstalleerd op de Azure-afbeeldingen en zijn meestal beschikbaar uit de opslagplaats van de verdeling van pakket.  Broncode kan worden gevonden op [GitHub](https://github.com/azure/walinuxagent).

Distributie|Versie|Stuurprogramma 's|Agent
---|---|---|---
CentOS door OpenLogic | CentOS 6.3 + 7.0 + | CentOS 6.3: [LIS downloaden](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409)<p>CentOS 6.4 +: In de Kernel | Pakket: In [OpenLogic repo](http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/) onder "WALinuxAgent" <br/>Broncode: [GitHub](https://github.com/Azure/WALinuxAgent)
[CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/) | 494.4.0+ | In de Kernel | Broncode: [GitHub](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent)
Debian | Debian 7,9 +, 8.2 + | In de Kernel | Pakket: In repo onder "waagent" <br/>Broncode: [GitHub](https://github.com/Azure/WALinuxAgent)
Oracle Linux | 6.4 +, +-7.0 | In de Kernel | Pakket: In repo onder "WALinuxAgent" <br/>Broncode: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
Red Hat Enterprise Linux | RHEL 7.1 + 6.7 + | In de Kernel|Pakket: In repo onder "WALinuxAgent" <br/>Broncode: [GitHub](https://github.com/Azure/WALinuxAgent)
SUSE Linux Enterprise | SLES 11 SP4, SLES 12 SP1 + en <p> SLES voor SAP 11 SP3 + | In de Kernel | Pakket: In de [Cloud: extra](https://build.opensuse.org/project/show/Cloud:Tools) repo onder "python azure agent" <br/>Broncode: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
openSUSE | openSUSE 13.2 + | In de Kernel | Pakket: In de [Cloud: extra](https://build.opensuse.org/project/show/Cloud:Tools) repo onder "python azure agent" <br/>Broncode: [GitHub](https://github.com/Azure/WALinuxAgent)
Ubuntu|Ubuntu 12.04, 14.04, 16.04, 16,10 | In de Kernel | Pakket: In repo onder "walinuxagent" <br/>Broncode: [GitHub](https://github.com/Azure/WALinuxAgent)


## <a name="partners"></a>Partners

### <a name="openlogic"></a>OpenLogic
[http://www.openlogic.com/Azure](http://www.openlogic.com/azure)

OpenLogic is een toonaangevende leverancier van oplossingen voor de cloud en Datacenter bron openen. OpenLogic helpt een toonaangevende onderneming in een breed scala van industrieën veilig opvragen, ondersteunen en beheren van open-sourcesoftware honderden. OpenLogic biedt technische ondersteuning voor commerciële en schadeloosstelling voor 600 open source pakketten van de OpenLogic Expert Gemeenschap, met inbegrip van ondersteuning voor CentOS enterprise, alsmede de lancering partner voor het leveren van CentOS-installatiekopieën op Azure worden.

### <a name="coreos"></a>CoreOS
[https://coreos.com/Docs/Running-coreos/cloud-providers/Azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

Van de CoreOS-Website:

*CoreOS is ontworpen voor beveiliging, consistentie en betrouwbaarheid. In plaats van het installeren van pakketten via yum of apt, CoreOS Linux containers gebruikt voor het beheren van uw diensten op een hoger niveau van abstractie. Een enkele service code en alle afhankelijkheden worden geleverd in een container die kan worden uitgevoerd op één of meerdere CoreOS machines.*


### <a name="credativ"></a>Credativ
[http://www.credativ.co.uk/credativ-blog/debian-images-Microsoft-Azure](http://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credativ is een onafhankelijk advies en services van bedrijf, gespecialiseerd in de ontwikkeling en uitvoering van professionele oplossingen met behulp van gratis software. Als toonaangevende Open Source specialist heeft Credative internationale erkenning met veel IT-afdelingen met behulp van hun steun. In combinatie met Microsoft, Credativ is momenteel overeenkomstige Debian installatiekopieën gereedmaken voor Debian 8 (Jessie) en Debian vóór 7 (Wheezy), die speciaal zijn ontworpen om te worden uitgevoerd op Azure en kan eenvoudig worden beheerd via het platform. Credativ ondersteunen ook de lange termijn onderhoud en bijwerken van de Debian afbeeldingen voor Azure via de Open Source Support Centers.

### <a name="oracle"></a>Oracle
[http://www.Oracle.com/technetwork/Topics/cloud/FAQ-1963009.HTML](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

De strategie van Oracle is bieden een breed scala aan oplossingen voor openbare en particuliere wolken, terwijl klanten u keuzemogelijkheden en flexibiliteit in hoe zij implementeren van Oracle-software in Oracle wolken, alsmede andere wolken.  Samenwerking met Microsoft van Oracle kan klanten implementeren van Oracle-software in de openbare en particuliere wolken Microsoft met het vertrouwen van CA en ondersteuning voor Oracle.  De betalingsverplichtingen en de investeringen in openbare en private cloud Oracle-oplossingen van Oracle is ongewijzigd.

### <a name="red-hat"></a>Red Hat
[http://www.RedHat.com/en/partners/Strategic-Alliance/Microsoft](http://www.redhat.com/en/partners/strategic-alliance/microsoft)

werelds toonaangevende leverancier van open source-oplossingen, Red Hat helpt meer dan 90% van de Fortune 500-bedrijven zakelijke problemen oplossen, het uitlijnen van de IT en business-strategieën voor te bereiden voor de toekomst van technologie. Red Hat biedt veilige oplossingen door middel van een open business model en een betaalbare, voorspelbare abonnementsmodel.

### <a name="suse"></a>SUSE
[http://www.SUSE.com/SUSE-Linux-Enterprise-Server-on-Azure](http://www.suse.com/suse-linux-enterprise-server-on-azure)

SUSE Linux Enterprise Server op Azure is een beproefde platform met uitstekende betrouwbaarheid en beveiliging voor cloud computing. Veelzijdige Linux-platform op SUSE van naadloos worden geïntegreerd met Azure cloud services voor het leveren van een gemakkelijk hanteerbaar cloud-omgeving. En met meer dan 9,200 goedgekeurde toepassingen uit meer dan 1800 onafhankelijke softwareleveranciers voor SUSE Linux Enterprise Server SUSE zorgt ervoor dat werklasten ondersteunde uitgevoerd in het datacenter vol vertrouwen kunnen worden geïmplementeerd op Azure.

### <a name="canonical"></a>Canonieke
[http://www.Ubuntu.com/cloud/Azure](http://www.ubuntu.com/cloud/azure)

Canonieke engineering en open Gemeenschap governance station Ubuntu van succes in de client-, server en cloud computing, inclusief persoonlijke cloud services voor consumenten. Canonieke de visie van een uniforme gratis platform in Ubuntu, van telefoon naar cloud met een reeks samenhangende interfaces voor de telefoon, tablet, tv- en bureaublad, kunt u de eerste keuze voor de diverse instellingen van openbare cloud providers voor de makers van consumentenelektronica en een favoriet onder de afzonderlijke technologen Ubuntu.

Canonical is met ontwikkelaars en engineering-afdelingen over de hele wereld uniek geplaatst om samen met de hardware fabrikanten, aanbieders van inhoud en softwareontwikkelaars om Ubuntu oplossingen op de markt, van pc's tot servers en handheld-apparaten.

