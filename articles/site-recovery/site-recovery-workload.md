<properties
    pageTitle="Welke werklasten kunt u beveiligen met Azure Site herstellen"
    description="Azure-Site-Recovery beschermt uw werkbelasting en toepassingen door coördinatie van de replicatie, failover en herstel van op bedrijven virtuele machines en fysieke servers Azure of een secundaire op gebouwen-site"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/10/2016"
    ms.author="raynew"/>

# <a name="what-workloads-can-you-protect-with-azure-site-recovery"></a>Welke werklasten kunt u beveiligen met Azure Site herstellen


In dit artikel wordt beschreven werkbelasting en toepassingen die met de Azure-Site-Recovery-service kan worden gerepliceerd.

Opmerkingen of vragen aan de onderkant van dit artikel, of op het [Forum van Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)boeken.

## <a name="overview"></a>Overzicht

Organisaties moeten een continuïteit en noodherstel recovery (BCDR) bedrijfsstrategie houden werkbelasting en gegevens veilig en beschikbaar tijdens geplande en ongeplande uitvaltijd en normale gebruiksomstandigheden zo snel mogelijk herstellen.

Herstel van de site is een Azure service die deel uitmaakt van uw strategie voor BCDR. Met sites worden hersteld, kunt u replicatie toepassingsbewuste distribueren naar de cloud of op een secundaire site. Of uw toepassingen Windows worden of Linux, op fysieke servers, VMware of Hyper-V kunt u Recovery Site replication goedkeuringen, voeren disaster recovery testen, en het uitvoeren van failover en failback.


Herstel van de site is geïntegreerd met Microsoft-toepassingen, met inbegrip van SharePoint, Exchange, Dynamics, SQL Server en Active Directory. Microsoft werkt ook nauw samen met toonaangevende leveranciers zoals Oracle, SAP, IBM en Red Hat. U kunt replicatie oplossingen op basis van app door app.

## <a name="why-use-site-recovery-for-application-replication"></a>Waarom Recovery Site gebruiken voor replicatie van toepassing?

Herstel site bijdraagt op toepassingsniveau bescherming en herstel als volgt:

- App-agnostic, bieden de replicatie voor een belasting op een ondersteunde computer.
- In de buurt van de synchrone replicatie met productoutput slechts 30 seconden om te voldoen aan de behoeften van de meest essentiële zakelijke apps.
- App consistente momentopnamen voor toepassingen in één of meerdere lagen.
- Integratie met SQL Server AlwaysOn en partnerschap met andere replicatie op toepassingsniveau technologieën, waaronder AD-replicatie AlwaysOn SQL, Exchange Database beschikbaarheidsgroepen (DAGs) en Oracle Data Guard.
- Flexibele herstelplannen, waarmee u een volledige toepassing stapel met één klik te herstellen, en met externe scripts en handmatige acties in het plan opgenomen.
- Management in sites hersteld en Azure vereenvoudigt de app netwerkvereisten, met inbegrip van de mogelijkheid om te reserveren IP-adressen, geavanceerde network load balancing en integratie met Azure verkeer Manager configureren, voor lage RTO netwerk switchovers.
-  Een uitgebreide automation-bibliotheek waarmee u productie-ready, specifieke scripts die kunnen worden gedownload en geïntegreerd met de herstelplannen.



## <a name="workload-summary"></a>Belasting overzicht

Herstel van de site kan elke toepassing op een ondersteunde computer repliceren. Bovendien waarmee productteams te verrichten aanvullende tests app-specifieke.

**Werkbelasting** | **Hyper-V VMs repliceren naar een secundaire site** | **Hyper-V VMs repliceren naar Azure** | **VMs VMware repliceren naar een secundaire site** | **VMs VMware repliceren naar Azure**
---|---|---|---|---
Active Directory, DNS | Y | Y | Y | Y
Web apps (IIS, SQL) | Y | Y | Y | Y
System Center Operations Manager | Y | Y | Y | Y
SharePoint | Y | Y | Y | Y
SAP<br/><br/>SAP-site repliceren naar Azure voor niet-cluster | Y (getest door Microsoft) | Y (getest door Microsoft) | Y (getest door Microsoft) | Y (getest door Microsoft)
Exchange (niet-DAG) | Y | Binnenkort beschikbaar | Y | Y
Extern bureaublad/VDI | Y | Y | Y | N.V.T.
Linux (besturingssysteem en apps) | Y (getest door Microsoft) | Y (getest door Microsoft) | Y (getest door Microsoft) | Y (getest door Microsoft)
Dynamics AX | Y | Y | Y | Y
Dynamics CRM | Y | Binnenkort beschikbaar | Y | Binnenkort beschikbaar
Oracle | Y (getest door Microsoft) | Y (getest door Microsoft) | Y (getest door Microsoft) | Y (getest door Microsoft)
Windows-bestandsserver | Y | Y | Y | Y


## <a name="replicate-active-directory-and-dns"></a>Repliceren Active Directory en DNS

Een Active Directory en DNS-infrastructuur zijn essentieel voor de meeste toepassingen van de onderneming. Bij een noodherstel moet u beveiligen en herstellen van deze onderdelen van de infrastructuur voor het herstellen van uw werkbelasting en apps.

Site herstellen kunt u een volledig geautomatiseerde calamiteitenplan maken voor Active Directory en DNS. Bijvoorbeeld, als u niet wilt dat via SharePoint en SAP van een primaire naar een secundaire site, kunt u instellen een herstelplan dat niet eerst via Active Directory en een extra app-specifieke herstelplan failover de andere toepassingen die van Active Directory gebruikmaken.

[Meer informatie](site-recovery-active-directory.md) over het beveiligen van Active Directory en DNS.

## <a name="protect-sql-server"></a>SQL Server beveiligen

SQL Server biedt een basis data services services voor gegevens voor veel zakelijke toepassingen in een datacenter op gebouwen.  Herstel van de site kan worden gebruikt met SQL Server HA/DR-technologieën ter bescherming van apps onderneming met meerdere lagen die gebruikmaken van SQL Server. Herstel van de site biedt:

- Een eenvoudige en rendabele ramp hersteloplossing voor SQL Server. Meerdere versies en edities van SQL Server zelfstandige servers en clusters, repliceert naar Azure of een secundaire site.  
- Integratie met SQL AlwaysOn beschikbaarheidsgroepen te beheren, failover en failback met Azure Site herstelplannen herstel.
- End-to-end herstelplannen voor de alle lagen in een toepassing, met inbegrip van de SQL Server-databases.
- Schaalbaarheid van SQL Server van de piek wordt geladen met het herstellen van een Site door 'bursting' ze in een groter formaat van IaaS virtual machine in Azure.
- Eenvoudig testen van SQL Server disaster recovery. U kunt failover test te analyseren gegevens en conformiteit controles, uitvoeren zonder invloed op uw productieomgeving uitvoeren.

[Meer informatie](site-recovery-sql.md) over het beveiligen van SQL server.

##<a name="protect-sharepoint"></a>Beveiliging van SharePoint

Azure-Site-Recovery beschermt SharePoint-implementaties, als volgt:

- Elimineert de noodzaak en de bijbehorende infrastructuurkosten voor een farm stand-by voor noodherstel. Site herstellen gebruiken voor het repliceren van een gehele farm (Web, toepassing en database lagen) Azure of een secundaire site.
- , Toepassingsimplementatie en beheer vereenvoudigd. Updates die op de primaire site automatisch worden gerepliceerd en zijn dus beschikbaar nadat failover en het herstel van een farm in een secundaire site. Verlaagt ook de complexiteit van management en de kosten die zijn gekoppeld aan een farm opstand door up-to-date te houden.
- SharePoint-toepassing ontwikkelen en testen door een kopie van de productie-achtige replica op verzoek omgeving voor het testen en debuggen vereenvoudigt.
- Vereenvoudigt de overgang naar de cloud via Recovery-Site te migreren van SharePoint-implementatie naar Azure.

[Meer informatie](https://gallery.technet.microsoft.com/SharePoint-DR-Solution-f6b4aeae) over het beveiligen van SharePoint.


## <a name="protect-dynamics-ax"></a>Dynamics AX beveiligen

Azure-Site-Recovery beschermt uw Dynamics AX ERP-oplossing, door:

- Regie van replicatie van uw gehele Dynamics AX-omgeving (Web- en AOS-lagen, database, lagen, SharePoint) Azure of een secundaire site.
- Migratie van Dynamics AX-implementatie in de cloud (Azure) te vereenvoudigen.
- Dynamics AX de ontwikkeling van toepassingen te vereenvoudigen en testen door het maken van een kopie van de productie-achtige op aanvraag, voor het testen en opsporen van fouten.

[Meer informatie](https://gallery.technet.microsoft.com/Dynamics-AX-DR-Solution-b2a76281) over het beveiligen van Dynamics AX.

## <a name="protect-rds"></a>Beveiliging van RDS

Extern bureaublad-Services (RDS) kunnen virtual desktop infrastructure (VDI), op basis van een sessie-desktops en -toepassingen, zodat gebruikers kunnen werken. U kunt met Azure Site herstellen:

- Beheerde of niet-beheerde groep virtuele bureaubladen repliceren naar een secundaire site en externe toepassingen en een secundaire site of Azure-sessies.
- Dit is wat u kunt repliceren:

**RDS** | **Hyper-V VMs repliceren naar een secundaire site** | **Hyper-V VMs repliceren naar Azure** | **VMs VMware repliceren naar een secundaire site** | **VMs VMware repliceren naar Azure** | **Fysieke servers repliceren naar een secundaire site** | **Fysieke servers repliceren naar Azure**
---|---|---|---|---|---|---
**Gegroepeerde virtueel bureaublad (onbeheerd)** | Ja | Nee | Ja | Nee | Ja | Nee
**Gegroepeerde virtueel bureaublad (beheerde en zonder UPD)** | Ja | Nee | Ja | Nee | Ja | Nee
**Externe toepassingen en bureaublad-sessies (zonder UPD)** | Ja | Ja | Ja | Ja | Ja | Ja


[Meer informatie](https://gallery.technet.microsoft.com/Remote-Desktop-DR-Solution-bdf6ddcb) over het beveiligen van RDS.


## <a name="protect-exchange"></a>Beveiligen van Exchange

Site-Recovery beschermt Exchange, als volgt:

- Voor kleine implementaties van Exchange, zoals een enkele of zelfstandige servers, herstel van de Site repliceren en failover uitvoeren naar Azure of een secundaire site.
- Voor grotere implementaties integreert Recovery Site met Exchange DAGS.
- Exchange-DAGs zijn de aanbevolen oplossing voor noodherstel van Exchange in een onderneming.  Site herstel herstelplannen kunnen DAGs om failover DAG evoluerende meerdere sites bevatten.


[Meer informatie](https://gallery.technet.microsoft.com/Exchange-DR-Solution-using-11a7dcb6) over het beveiligen van Exchange.

## <a name="protect-sap"></a>Bescherming van SAP

Herstel van de Site beschermen uw SAP-implementatie als volgt gebruiken:

- Beveiliging van het hele SAP-implementatie door verschillende lagen te repliceren naar Azure, of naar een secundaire site inschakelen.
- Cloud migratie, vereenvoudigen met behulp van Site-herstel uw SAP-implementatie migreren naar Azure.
- SAP te ontwikkelen en testen, door het maken van een productie-achtige kopiëren op verzoek voor het testen en debuggen van toepassingen.

[Meer informatie](http://aka.ms/asr-sap) over het beveiligen van SAP.

## <a name="next-steps"></a>Volgende stappen

[Voorbereiden voor distributie Site herstellen](site-recovery-best-practices.md) 
