<properties
   pageTitle="Overzicht van operations Management Suite (OMS) | Microsoft Azure"
   description="Microsoft Operations Management Suite (OMS) is Microsoft van cloud-gebaseerde IT-oplossing waarmee u beheren kunt en beveiligen van uw locatie op en cloud infrastructuur.  In dit artikel worden opgenomen in de OMS verschillende services en koppelingen naar gedetailleerde inhoud."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="bwren" />

# <a name="what-is-operations-management-suite-oms"></a>Wat is Operations Management Suite (OMS)?

Microsoft Operations Management Suite (OMS) is Microsoft van cloud-gebaseerde IT-oplossing waarmee u beheren kunt en beveiligen van uw locatie op en cloud infrastructuur.  Aangezien OMS is geïmplementeerd als een cloud-gebaseerde service, kunt u laten werken snel met minimale investeringen in infrastructuurservices.  Nieuwe functies worden automatisch geleverd zodat u doorlopend onderhoud en upgrade kosten.

Naast waardevolle diensten op zijn eigen, kunt OMS integreren met System Center-onderdelen zoals System Center Operations Manager uit te breiden uw bestaande investeringen in management in de cloud.  System Center en OMS kunt werken samen om een volledige hybride management ervaring bieden.

De volgende secties bevatten een algemene beschrijving van de waarde van verschillende gebieden van OMS en de services die de tenuitvoerlegging ervan.  U kunt verwijzen naar OMS-architectuur voor een overzicht van de verschillende onderdelen van de OMS voordat het controleren van de gedetailleerde documentatie voor elk.


## <a name="insight-and-analyticsmediaoperations-management-suite-overviewicon-insight-analyticspng-insight-and-analytics"></a>![Inzicht en analyse](media/operations-management-suite-overview/icon-insight-analytics.png) Inzicht en analyse

[Logboek Analytics](http://azure.microsoft.com/documentation/services/log-analytics) helpt u bij het verzamelen, correleren, zoeken en reageren op een logboek en de prestaties van gegevens die zijn gegenereerd door besturingssystemen en toepassingen. Het biedt u real-time operationele inzichten met geïntegreerde search en aangepaste dashboards miljoenen records gemakkelijk te analyseren op al uw werkbelasting en -servers, onafhankelijk van hun fysieke locatie.

U kunt oplossingen eenvoudig toevoegen aan logboek Analytics waarmee gegevens worden verzameld en de logica van haar analyse.  Oplossingen kunnen voorzien van extra functionaliteit die automatisch aan de agenten met minimale of geen configuratie wordt geleverd.  Analyseprogramma's geleverd door individuele oplossingen gebruiken, kunt u aangepaste zoekacties uitvoeren over de gehele dataset om gegevens tussen systemen en toepassingen te correleren.  


## <a name="automation--controlmediaoperations-management-suite-overviewicon-automation-controlpng-automation--control"></a>![Automatisering & Control](media/operations-management-suite-overview/icon-automation-control.png) Automatisering & Control

Azure automatisering automatiseert met [runbooks](../automation/automation-runbook-types.md) die op basis van PowerShell en uitvoeren in de Azure cloud de administratieve processen.  Runbooks toegang tot andere producten of diensten die kan worden beheerd met PowerShell, met inbegrip van bronnen in andere wolken zoals Amazon Web Services (AWS).  Runbooks kunnen ook worden uitgevoerd op een server in uw lokale Datacenter beheren lokale bronnen.

Azure automatisering kunt u configuratie beheren met [PowerShell DSC](../automation/automation-dsc-overview.md).  U kunt maken en beheren van DSC-bronnen die worden gehost in Azure en toepassen op systemen wolk en in ruimten te definiëren en automatisch afdwingen hun configuratie.


## <a name="protection-and-recoverymediaoperations-management-suite-overviewicon-protection-recoverypng-protection-and-disaster-recovery"></a>![Bescherming en herstel](media/operations-management-suite-overview/icon-protection-recovery.png) Bescherming en herstel na noodgevallen

[Back-up Azure](http://azure.microsoft.com/documentation/services/backup) beveiligt u uw toepassingsgegevens en behoudt het jaar zonder een belangrijke investering en met minimale bedrijfskosten.  Het maken van reservekopieën van gegevens van fysieke en virtuele servers Windows naast de werkbelasting van toepassingen zoals SQL Server en SharePoint.  Het kan ook worden gebruikt door System Center Data Protection Manager (DPM) beveiligde gegevens repliceren naar Azure voor redundantie en langdurige opslag.

[Azure-Site-Recovery](http://azure.microsoft.com/documentation/services/site-recovery) draagt bij aan uw bedrijfscontinuïteit en noodherstelplan (BCDR) door de regie van replicatie, failover en herstel op ruimten Hyper-V virtuele machines, VMware virtuele machines en fysieke Windows/Linux-servers. U kunt computers repliceren naar een secundaire Datacenter of uitbreiden van uw datacenter door ze te repliceren naar Azure. Herstel van de site biedt ook eenvoudige failover en herstel voor werkbelasting. Het integreert met disaster recovery mechanismen, zoals SQL Server AlwaysOn en biedt herstelplannen voor eenvoudige failover van de werklast die via meerdere computers worden geschakeld.


## <a name="oms-security-and-compliancemediaoperations-management-suite-overviewicon-security-compliancepng-security-and-compliance"></a>![OMS beveiliging en naleving](media/operations-management-suite-overview/icon-security-compliance.png) Beveiliging en naleving
Beveiliging en naleving helpt u bij het opsporen, beoordelen en beveiligingsrisico's voor uw infrastructuur te verminderen.  Deze voorzieningen van OMS worden geïmplementeerd met behulp van meerdere oplossingen in logboek Analytics die het analyseren van gegevens en configuratie van agent systemen om u te helpen om ervoor te zorgen de continue beveiliging van uw omgeving.

- De [oplossing voor beveiliging en controle](oms-security-getting-started.md ) verzamelt en analyseert u beveiligingsgebeurtenissen op beheerde systemen voor verdachte activiteiten.
- De [oplossing Antimalware](log-analytics-malware.md ) -rapporten over de status van antimalware bescherming op beheerde systemen.  
- De oplossing voor systeemupdates voert een analyse van de beveiligingsupdates en andere updates op uw beheerde systemen, zodat u gemakkelijk systemen herkennen patchen vereisen.


## <a name="next-steps"></a>Volgende stappen
- Informatie over het [logboek Analytics](http://azure.microsoft.com/documentation/services/log-analytics).
- Informatie over [Azure automatisering](../automation/automation-intro.md).
- Meer informatie over [back-up van Azure](http://azure.microsoft.com/documentation/services/backup).
- Informatie over het [herstellen van Azure-Site](http://azure.microsoft.com/documentation/services/site-recovery).
