<properties
   pageTitle="Beveiliging-overzicht van Azure Virtual Machines | Microsoft Azure"
   description=" Azure virtuele Machines bieden u de flexibiliteit van virtualisatie zonder te kopen en onderhouden van de fysieke hardware die in de virtuele machine wordt uitgevoerd.  Dit artikel bevat een overzicht van de belangrijkste Azure beveiligingsfuncties die Azure virtuele Machines kunnen worden gebruikt. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="azure-virtual-machines-security-overview"></a>Azure virtuele Machines beveiliging-overzicht

Azure virtuele Machines kunt u een breed scala aan oplossingen voor computergebruik in een flexibele manier implementeren. Met ondersteuning voor Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP en Azure BizTalk-Services, kunt u de werkbelasting en elke taal op vrijwel elk besturingssysteem implementeren.

Een Azure VM biedt u de flexibiliteit van virtualisatie zonder te kopen en onderhouden van de fysieke hardware die in de virtuele machine wordt uitgevoerd.  U kunt bouwen en implementeren van uw toepassingen met de zekerheid dat uw gegevens beschermd en veilig in onze streng beveiligde datacenters zijn.

Met Azure, kunt u die verbeterde beveiliging, compatibele oplossingen bouwen:

- Uw virtuele machines beveiligen tegen virussen en malware
- Codeer gevoelige informatie
- Beveiligd netwerkverkeer
- Identificeren en opsporen van bedreigingen
- Voldoen aan conformiteitsvereisten

Het doel van dit artikel is bedoeld als een overzicht van de belangrijkste Azure beveiligingsfuncties die met virtuele machines kunnen worden gebruikt. Wij bieden ook koppelingen naar artikelen waarin nadere gegevens over elke functie, zodat u meer informatie.  

De core Azure Virtual Machine beveiligingsmogelijkheden zullen worden behandeld in dit artikel:

- Antimalware
- Hardware Security Module
- Virtuele machine schijfversleuteling
- Back-up van de virtuele machine
- Azure-Site herstellen
- Virtueel netwerk
- Beheer van beveiligingsbeleid en -rapportage
- Conformiteit

## <a name="antimalware"></a>Antimalware

U kunt met Azure, antimalware software van leveranciers zoals Microsoft, Symantec, Trend Micro, Kaspersky en McAfee security gebruiken uw virtuele machines beveiligen tegen schadelijke bestanden, adware en andere bedreigingen. Zie het gedeelte meer informatie onderstaande artikelen over partner om oplossingen te vinden.

Microsoft Antimalware voor Azure Cloud Services en virtuele Machines is een real-time bescherming mogelijkheid die helpt bij het identificeren en verwijderen van virussen, spyware en andere schadelijke software.  Microsoft Antimalware biedt configureerbare waarschuwingen als bekende schadelijke of ongewenste software probeert zichzelf te installeren of uitvoeren op uw Azure-systemen.

Microsoft Antimalware is een single-agent oplossing voor toepassingen en omgevingen van de huurder, bedoeld in de achtergrond zonder menselijke tussenkomst. U kunt beveiliging op basis van de behoeften van de werkbelasting van toepassingen, met een standaard secure-door-standaard of geavanceerde aangepaste configuratie, met inbegrip van monitoring antimalware implementeren.

Wanneer u implementeren en Microsoft Antimalware inschakelt, zijn de volgende kernfuncties beschikbaar:

- Realtime bescherming - monitoren activiteit in Cloud-Services en op virtuele Machines te detecteren en te blokkeren tot uitvoering van schadelijke software.
- Geplande scan - voert regelmatig gerichte scannen van malware, met inbegrip van actief uitvoeren van programma's detecteren.
- Herstel van malware - neemt automatisch actie gedetecteerde schadelijke software, zoals het verwijderen of schadelijke bestanden in quarantaine plaatsen en schadelijke registervermeldingen worden opgeruimd.
- Updates van handtekening - automatisch de nieuwste bescherming handtekeningen (virusdefinities) ter bescherming van installaties is bijgewerkt op een vooraf bepaalde frequentie.
- Antimalware-Engine-updates – automatisch updates van de Microsoft Antimalware-engine.
- Antimalware-Platform updates – automatisch updates van de Microsoft Antimalware-platform.
- Actieve bescherming - metagegevens Azure telemetrie rapporten over gedetecteerde bedreigingen en verdachte bronnen om ervoor te zorgen snelle respons en kunnen real-time synchrone handtekening levering via de Microsoft Active Protection System (wijst toe).
- Voorbeelden voor rapportage - biedt en monsters wordt doorgegeven aan de Microsoft Antimalware-service om te helpen verfijnen van de service en het oplossen van problemen in te schakelen.
- Uitsluitingen: toepassing en servicebeheerders kunnen bepaalde bestanden, processen, configureren en uitsluit van de bescherming en het zoeken naar de prestaties en de andere stations.
- Collectie Antimalware-gebeurtenis - de antimalware-service gezondheid, herstel ondernomen in het gebeurtenislogboek van het besturingssysteem en verdachte activiteiten vastgelegd en verzamelt ze in Azure opslag-rekening van de klant.

Meer informatie: Zie voor meer informatie over antimalware software ter bescherming van uw virtuele machines:

- [Microsoft Antimalware voor Azure Cloud Services en virtuele Machines](../security/azure-security-antimalware.md)
- [Implementatie van Antimalware oplossingen op Azure virtuele Machines](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
- [Het installeren en configureren van diep Security van Trend Micro als een Service op een Windows VM](../virtual-machines/virtual-machines-windows-classic-install-trend.md)
- [Het installeren en configureren van Symantec Endpoint Protection op een Windows VM](../virtual-machines/virtual-machines-windows-classic-install-symantec.md)
- [Nieuwe Antimalware-opties voor het beveiligen van Azure Virtual Machines – McAfee Endpoint Protection](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)
- [Security-oplossingen in de markt Azure](https://azure.microsoft.com/marketplace/?term=security)

## <a name="hardware-security-module"></a>Hardware security Module

Codering en verificatie verbetert niet security tenzij de sleutels zelf worden beschermd. U kunt het beheer en de beveiliging van uw kritieke geheimen en sleutels vereenvoudigen door ze te slaan in Azure sleutel kluis. Sleutel kluis biedt de mogelijkheid om uw sleutels in de hardware security modules (HSM's) gecertificeerde aan FIPS 140-2 niveau 2 normen. Uw SQL Server-coderingssleutels voor back-up of [transparante gegevenscodering](https://msdn.microsoft.com/library/bb934049.aspx) kunnen alle worden opgeslagen in sleutel kluis met alle sleutels of geheimen van uw toepassingen. Machtigingen en toegang tot deze beveiligde items worden beheerd via [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).

Meer informatie:

- [Wat is Azure sleutel kluis?](../key-vault/key-vault-whatis.md)
- [Aan de slag met Azure sleutel kluis](../key-vault/key-vault-get-started.md)
- [Azure sleutel kluis blog](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>Virtuele machine schijfversleuteling

Azure schijfversleuteling is een nieuwe mogelijkheid waarmee u de diskettes van Windows en Linux Azure Virtual Machine coderen. Azure schijfversleuteling wordt gebruikt de industrie standaard [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) -functie van Windows en de functie [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux codering voor het besturingssysteem en de gegevensschijven.

De oplossing is geïntegreerd met Azure sleutel kluis om te bepalen en de schijf coderingssleutels en geheimen in je abonnement sleutel kluis, terwijl ervoor te zorgen dat alle gegevens in de virtuele machine-schijven worden gecodeerd in rust in Azure opslag beheren.

Meer informatie:

- [Codering voor Windows en Linux IaaS VMs Azure schijf](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)
- [Azure schijfversleuteling voor Linux en Windows virtuele Machines](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview-now-available/)
- [Coderen van een virtuele machine](../security-center/security-center-disk-encryption.md)

## <a name="virtual-machine-backup"></a>Back-up van de virtuele machine

Azure back-up is een schaalbare oplossing die beveiligt u uw toepassingsgegevens met nul materiële investeringen en minimale bedrijfskosten. Toepassingsfouten kunnen de gegevens beschadigd raken en menselijke fouten kunnen leiden tot fouten in uw toepassingen. Met back-up van Azure zijn uw virtuele machines met Windows en Linux beschermd.

Meer informatie:

- [Wat is Azure back-up?](../backup/backup-introduction-to-azure-backup.md)
- [Azure back-up Learning pad](https://azure.microsoft.com/documentation/learning-paths/backup/)
- [Azure-back-Service - Veelgestelde vragen](../backup/backup-azure-backup-faq.md)

## <a name="azure-site-recovery"></a>Azure-Site herstellen

Een belangrijk onderdeel van de BCDR-strategie van uw organisatie is corporate werkbelasting en apps houden van hoe en wanneer de geplande en ongeplande storingen optreden. Azure-Site-Recovery kunt u replicatie, failover en herstel van de werkbelasting en apps evoluerende zodat ze beschikbaar vanuit een secundaire locatie zijn als uw primaire locatie begeeft.

Herstel van de site:

- **Eenvoudiger uw strategie voor BCDR** , herstel van de Site kunt u gemakkelijk verwerken replicatie, failover en herstel van meerdere werklasten business en apps vanaf één locatie. Herstel site orchestrates replicatie en -failover, maar niet de toepassingsgegevens onderscheppen of informatie over het.
- **Biedt flexibele replicatie** , sites worden hersteld met behulp van kunt u werklasten waarop Hyper-V virtuele machines, VMware virtuele machines en fysieke Windows/Linux-servers repliceren.
- **Failover-ondersteuning en herstel** , herstel Site test failovers disaster recovery oefeningen zonder productieomgevingen ondersteuning biedt. U kunt ook geplande failover met nul gegevensverlies op verwachte storingen of niet-geplande failover uitvoeren met minimale gegevens verloren gaan (afhankelijk van de frequentie van replicatie) voor onverwachte rampen. Na een failover kunt u failback naar uw primaire sites. Herstel van de site biedt de herstelplannen die scripts en automatisering Azure werkmappen opnemen kunnen zodat u failover en het herstel van toepassingen met meerdere lagen kunt aanpassen.
- **Hiermee secundaire datacenter** , kunt u een site secundaire in ruimten of Azure repliceren. Azure met als bestemming voor noodherstel elimineert de kosten en complexiteit van het onderhouden van een secundaire site. Gerepliceerde gegevens worden opgeslagen in Azure opslag.
- **Integrates met bestaande technologieën voor BCDR** — Site herstellen werkt samen met andere functies voor BCDR. Bijvoorbeeld, kunt u sites hersteld ter bescherming van de SQL Server-back-end van corporate werkbelastingen. Dit omvat native ondersteuning voor SQL Server AlwaysOn failover van beschikbaarheidsgroepen beheren.

Meer informatie:

- [Wat is Azure Site herstellen?](../site-recovery/site-recovery-overview.md)
- [Hoe werkt de Azure Site herstellen?](../site-recovery/site-recovery-components.md)
- [Wat werklasten worden beschermd door Azure Site herstellen?](../site-recovery/site-recovery-workload.md)

## <a name="virtual-networking"></a>Virtueel netwerk

Virtuele machines moet verbinding met het netwerk. Azure vereist ter ondersteuning van die eis, virtuele machines zijn aangesloten op een virtueel netwerk Azure. Een virtueel netwerk Azure is een logische constructie die is gebaseerd op het fysieke netwerk van Azure weefsel. Elke logische Azure Virtual Network wordt gescheiden van alle andere Azure virtuele netwerken. Deze isolatie helpt te verzekeren dat netwerkverkeer in de distributie niet toegankelijk voor andere klanten van Microsoft Azure is.

Meer informatie:

- [Azure netwerk beveiliging-overzicht](security-network-overview.md)
- [Virtueel netwerk-overzicht](../virtual-network/virtual-networks-overview.md)
- [Netwerkfuncties en partnerschappen voor bedrijfsmatige scenario 's](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Beheer van beveiligingsbeleid en -rapportage

Azure Beveiligingscentrum helpt u bij het voorkomen, opsporen en reageren op bedreigingen en biedt dat u meer inzicht in, en controle over, de beveiliging van uw resources Azure. Het biedt geïntegreerde beveiliging beleid en het toezicht op verschillende de Azure abonnementen, helpt detecteren bedreigingen die anders opgemerkt en werkt met een brede selectie van beveiligingsoplossingen.

Azure Security Center kunt u optimaliseren en controleren de beveiliging van virtuele machine:

- Bieden virtuele machine [aanbevelingen voor beveiliging](../security-center/security-center-recommendations.md) , zoals systeemupdates toepassen, ACL's eindpunten configureren antimalware inschakelen, inschakelen, netwerk-beveiligingsgroepen en schijf coderen.
- Controle van de status van uw virtuele machines

Meer informatie:

- [Inleiding tot Azure Security Center](../security-center/security-center-intro.md)
- [Veelgestelde vragen over Azure Security Center](../security-center/security-center-faq.md)
- [Beveiligingscentrum Azure Planning en bewerkingen](../security-center/security-center-planning-and-operations-guide.md)

## <a name="compliance"></a>Conformiteit

Azure virtuele Machines is gecertificeerd voor FISMA, FedRAMP, HIPAA, PCI DSS Level 1 en andere belangrijke naleving programma's. Deze certificering is het gemakkelijker voor uw eigen Azure toepassingen te voldoen aan de eisen voldoen en voor uw bedrijf om een breed scala van nationale en internationale regelgeving.

Meer informatie:

- [Vertrouwenscentrum van Microsoft: naleving](https://www.microsoft.com/TrustCenter/Compliance/default.aspx)
- [Vertrouwde Cloud: Microsoft Azure beveiliging, Privacy en naleving](http://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)
