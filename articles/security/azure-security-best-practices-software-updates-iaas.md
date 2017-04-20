<properties
   pageTitle="Aanbevolen procedures voor Software-Updates op Microsoft Azure IaaS | Microsoft Azure"
   description="Artikel bevat een aantal aanbevolen procedures voor software-updates in een omgeving met Microsoft Azure IaaS.  Is bedoeld voor IT-professionals en beveiliging analisten die omgaan met wijzigen besturingselement, update en asset management software op een dagelijkse basis, met inbegrip van degenen die verantwoordelijk zijn voor de beveiliging en naleving inspanningen van hun organisatie."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="best-practices-for-software-updates-on-microsoft-azure-iaas"></a>Aanbevolen procedures voor software-updates op Microsoft Azure IaaS

Vooraleer enige vorm van discussie over beste praktijken voor een Azure [IaaS](https://azure.microsoft.com/overview/what-is-iaas/) omgeving, is het belangrijk dat u begrijpt wat de scenario's die u beheren van software-updates en de verantwoordelijkheden hebben. Het onderstaande diagram kunt u deze grenzen te begrijpen:

![Cloud modellen en verantwoordelijkheden](./media/azure-security-best-practices-software-updates-iaas/sec-cloudstack-new.png)

De linkerkolom bevat zeven verantwoordelijkheden (gedefinieerd in de volgende secties) dat organisaties overwegen, die bijdragen tot de veiligheid en privacy van een computeromgeving.
 
Indeling van gegevens & verantwoording en Client & eindpunt beveiliging zijn de verantwoordelijkheden die uitsluitend in het domein van klanten en fysieke, Host en netwerk verantwoordelijkheden zijn in het domein van de cloud-aanbieders in de PaaS- en SaaS-modellen. 

De resterende verantwoordelijkheden worden gedeeld tussen klanten en serviceproviders cloud. Sommige verantwoordelijkheden moeten de CSP en de klant te beheren de verantwoordelijkheid samen, met inbegrip van de controle van hun domein. Bijvoorbeeld, kunt u de identiteit & access management bij het gebruik van Azure Active Directory Services; de configuratie van services zoals meerledige verificatie mogelijk is aan de klant, maar ervoor te zorgen dat doeltreffende functionaliteit is de verantwoordelijkheid van Microsoft Azure.

> [AZURE.NOTE] Lees voor meer informatie over gedeelde verantwoordelijkheden in de cloud, [Gedeelde verantwoordelijkheden voor Cloud Computing](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/153019/1/Shared%20responsibilities%20for%20cloud%20computing.pdf) 

Deze dezelfde principes van toepassing in een hybride scenario waar uw bedrijf gebruik van Azure IaaS VMs die communiceren met bronnen voor bedrijfsruimten maakt zoals in het onderstaande diagram.

![Hybride typische scenario met Microsoft Azure](./media/azure-security-best-practices-software-updates-iaas/sec-azconnectonpre.png)

## <a name="initial-assessment"></a>Eerste beoordeling

Zelfs als uw bedrijf al een update management-systeem wordt gebruikt en u al beleid voor software-update op zijn plaats, is het belangrijk dat u regelmatig terugkeren naar vorige beoordelingen van beleid en bijwerken op basis van uw huidige behoeften. Dit betekent dat u moet bekend zijn met de huidige status van de resources in uw bedrijf. U hebt voor het bereiken van deze staat, te weten:

-   De fysieke en virtuele computers in uw bedrijf.

-   Besturingssystemen en versies van elk van deze fysieke en virtuele computers.

-   Software-updates geïnstalleerd op elke computer (versies van service packs, software-updates en andere wijzigingen).

-   De functie levert voor elke computer in uw onderneming.

-   De programma's en toepassingen die op elke computer worden uitgevoerd.

-   Eigendom en contact informatie voor elke computer.

-   De activa aanwezig in uw omgeving en hun relatieve waarde om te bepalen welke gebieden moeten de meeste aandacht en bescherming.

-   Bekende beveiligingsproblemen en de processen die uw onderneming beschikt over nieuwe beveiligingsproblemen identificeren of wijzigingen in het beveiligingsniveau.

-   Tegenmaatregelen die zijn geïmplementeerd voor het beveiligen van uw omgeving.

U moet deze informatie regelmatig worden bijgewerkt en moet gemakkelijk toegang hebben tot de betrokken in de software update management proces.

## <a name="establish-a-baseline"></a>Stel een basislijn

Een belangrijk onderdeel van de software update management proces maakt eerste een standaardinstallatie van besturingssystemen, toepassingen en hardware voor computers in uw onderneming; Deze worden basislijnen genoemd. Een basislijn is de configuratie van een product of de regeling die is ingesteld op een specifiek punt in de tijd. Een toepassing of besturingssysteem volgens de basislijn, biedt bijvoorbeeld de mogelijkheid om op te bouwen van een computer of service aan een specifieke staat.

Basislijnen vormen de basis voor het opsporen en oplossen van problemen en het eenvoudiger software update management, zowel door het aantal software-updates die u in uw onderneming implementeren moet te verminderen en door het verhogen van de mogelijkheid om te controleren of.

Na het uitvoeren van de eerste controle van uw onderneming, moet u de informatie die wordt verkregen uit de audit voor het definiëren van een operationele basislijn voor de IT-onderdelen binnen uw productieomgeving. Een aantal van de basislijnen mogelijk, afhankelijk van de verschillende soorten hardware en software geïmplementeerd in productie.

Sommige servers vereist een software-update om te voorkomen dat hangende wanneer ze de computer wordt afgesloten bij het uitvoeren van Windows Server 2012. Een basislijn voor deze servers moet opnemen van deze software-update.

In grote organisaties is het vaak handig om de computers in uw bedrijf kunt onderverdelen in activacategorieën en laten een basislijn standaard elke categorie met behulp van dezelfde versies van software en software-updates. Vervolgens kunt u deze categorieën van activa in een software-update distributie prioriteit.

## <a name="subscribe-to-the-appropriate-software-update-notification-services"></a>Abonneer u op de juiste software update notification services

Nadat u een eerste controle van de software gebruikt binnen uw bedrijf, moet u de beste methode voor het ontvangen van meldingen van nieuwe software-updates voor elke SOFTWAREPRODUCT en versie te bepalen. Het SOFTWAREPRODUCT misschien de beste methode voor melding meldingen via e-mail, websites of publicaties van de computer.

Bijvoorbeeld Microsoft Security Response Center (MSRC) reageert op alle veiligheidsgerelateerde problemen met Microsoft-producten en biedt het Microsoft Security Bulletin Service, een gratis e-melding van nieuw geïdentificeerde beveiligingsproblemen en software-updates die worden uitgebracht om deze problemen op te lossen. U kunt zich abonneren op deze service op http://www.microsoft.com/technet/security/bulletin/notify.mspx.

## <a name="software-update-considerations"></a>Overwegingen met betrekking tot software-update

Nadat u een eerste controle van de software gebruikt binnen uw bedrijf, moet u bepalen de vereisten voor het instellen van uw software update management system, dat afhankelijk is van de software update management systeem dat u gebruikt. Lees voor System Center voor WSUS lezen [Aanbevolen procedures voor Windows Server Update Services](https://technet.microsoft.com/library/Cc708536), [plannen voor Software-Updates in Configuration Manager](https://technet.microsoft.com/library/gg712696).

Er zijn echter enkele algemene overwegingen en aanbevolen procedures die u, ongeacht de oplossing die u gebruikt toepassen kunt, zoals in de secties die volgt.

### <a name="setting-up-the-environment"></a>Instellen van de omgeving

Houd rekening met de volgende procedures bij het plannen van de installatie van de software update management-omgeving:

-   **Verzamelingen maken productie-software-update op basis van stabiele criteria**: In het algemeen stabiel criteria gebruiken voor het maken van collecties voor de update-software-inventarisatie en -distributie helpen te vereenvoudigen, alle stadia van het proces voor software-update. De stabiele criteria kunnen de client geïnstalleerd besturingssysteem en service pack-niveau, Systeemrol of doelorganisatie bevatten.

-   **Maken in preproductie collecties die verwijzing computers bevatten**: de voorbereiding van de productie-collectie bevat representatieve configuraties van de versies van besturingssystemen, bedrijfssoftware en andere software die wordt uitgevoerd in uw onderneming.

U moet ook overwegen waar de software-update-server zich bevindt, of als deze zich op de Azure IaaS infrastructuur in de wolk worden op de bedrijfsruimten. Dit is een belangrijke beslissing, omdat u moet de hoeveelheid verkeer tussen resources in ruimten en Azure infrastructuur evalueren. [Verbinding maken met een netwerk van gebouwen met een virtueel netwerk Microsoft Azure](https://technet.microsoft.com/library/Dn786406.aspx) lezen voor meer informatie over het aansluiten van uw infrastructuur op ruimten op Azure.

Het ontwerpopties waarmee u kunnen bepalen waar de update-server worden geplaatst zijn ook afhankelijk van uw huidige infrastructuur en de software-update die u gebruikt. Lees voor WSUS [Windows Server Update Services implementeren in uw organisatie](https://technet.microsoft.com/library/hh852340.aspx) en voor System Center Configuration Manager [Planning voor Sites en hiërarchieën in Configuration Manager](https://technet.microsoft.com/library/Gg712681.aspx)lezen.

### <a name="backup"></a>Back-up

Regelmatig back-ups zijn niet alleen belangrijk voor de software update management platform zelf maar ook voor de servers die worden bijgewerkt. Organisaties die beschikken over een [proces wijzigen](https://technet.microsoft.com/library/cc543216.aspx) zal vereisen te rechtvaardigen van de redenen waarom de server moet worden bijgewerkt, de geschatte uitvaltijd en mogelijke gevolgen. Om ervoor te zorgen dat er een rollback configuratie in plaats voor het geval het bijwerken mislukt, moet u regelmatig een back-up van het systeem.

Sommige back-upopties voor Azure IaaS omvatten:

-   [Azure IaaS werkbelasting bescherming Data Protection Manager gebruiken](https://azure.microsoft.com/blog/2014/09/08/azure-iaas-workload-protection-using-data-protection-manager/)

-   [Back-up van Azure virtual machines](../backup/backup-azure-vms.md)

### <a name="monitoring"></a>Monitoring

U moet regelmatig rapporten uitvoeren om te controleren van het aantal ontbrekende of updates of updates met de status onvolledige voor elke software-update die gemachtigd is geïnstalleerd. Ook het melden van software-updates die nog niet zijn geautoriseerd eenvoudiger implementatie beslissingen vergemakkelijken.

U kunt ook de volgende taken:

-   Overgaan tot een controle van de toepassing en de geïnstalleerde beveiligingsupdates voor alle computers in uw bedrijf.

-   Machtigen en de updates met de juiste computers te implementeren.

-   De voorraad bijhouden en bijwerken installatiestatus en voortgang voor de computers in uw bedrijf.

Verder naar algemene overwegingen die zijn beschreven in dit artikel, kunt u ook overwegen elke product's beste praktijk, bijvoorbeeld: als u een VM in Azure met SQL Server hebt, zorg ervoor dat u de aanbeveling van de software-updates voor het product volgt.

## <a name="next-steps"></a>Volgende stappen

Gebruik de richtlijnen in dit artikel wordt beschreven om u te helpen bij het bepalen van de beste opties voor software-updates voor virtuele machines in Azure IaaS. Er zijn veel overeenkomsten tussen de aanbevolen procedures voor software-update in een traditionele datacenter versus Azure IaaS, daarom wordt aanbevolen dat u uw huidige software update beleid Azure VMs bevatten en de relevante procedures van dit artikel in het algehele proces van de software-update als u wilt evalueren.
