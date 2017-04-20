
<properties
   pageTitle="Azure richtlijnen | patronen en procedures | Microsoft Azure"
   description="Aanbevelingen en richtlijnen voor Azure"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="christb"/>

# <a name="azure-guidance"></a>Azure richtlijnen

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

De patronen en procedures-team van Microsoft maakt deel uit van het Azure Customer Advisory Team. Ons doel is om ontwikkelaars, architecten en IT-professionals worden uitgevoerd op het platform Microsoft Azure. Ontwikkelen van richtlijnen waarin de beste praktijken voor het bouwen van oplossingen voor de wolk op Azure.

## <a name="checklists"></a>Controlelijsten

Deze lijsten zijn een snelle verwijzing voor het controleren van de fundamentele aspecten van de beschikbaarheid en schaalbaarheid. 

- [Controlelijst voor beschikbaarheid][AvailabilityChecklist] 

    Een overzicht van aanbevolen procedures voor het toezicht op beschikbaarheid.

- [Controlelijst voor schaalbaarheid][ScalabilityChecklist]

    Een overzicht van aanbevolen procedures voor het ontwerpen en implementatie van schaalbare services en verwerken van gegevens beheren.

## <a name="best-practices-articles"></a>Aanbevolen procedures artikelen

Deze artikelen bevatten een diepgaande bespreking van de belangrijke begrippen die vaak van cloud computing. 

- [API-ontwerp][APIDesign] 

    Een bespreking van de factoren waarmee rekening houden bij het ontwerpen van een web-API.

- [API-implementatie][APIImplementation] 

    Een aantal aanbevolen procedures voor het implementeren en publiceren van een web-API.

- [Beveiligingsrichtlijnen voor API](https://github.com/mspnp/azure-guidance/blob/master/API-security.md) 

    Een bespreking van de verificatie en machtiging betrekking heeft op (bijvoorbeeld token typen, vergunning protocollen vergunning stromen en gevaar beperken).

- [AutoScaling richtlijnen][AutoscalingGuidance] 

    Een overzicht van overwegingen bij het schalen oplossingen zonder de noodzaak voor handmatige interventie.

- [Achtergrond taken richtlijnen][BackgroundJobsGuidance] 

    Een beschrijving van de beschikbare opties en de aanbevolen procedures voor de uitvoering van de taken die moeten worden uitgevoerd op de achtergrond, onafhankelijk van de voorgrond- of interactieve bewerkingen.

- [Richtlijnen voor Content Delivery Network (CDN)][CDNGuidance] 

    Algemene richtlijnen en aanbevolen praktijken voor het gebruik van de CDN voor beschikbaarheid en prestaties te maximaliseren en minimaliseren van de belasting van uw toepassingen.

- [Richtlijnen voor caching][CachingGuidance] 

    Een overzicht van het gebruik van de cache voor het verbeteren van de prestaties en schaalbaarheid van een systeem.

- [Richtlijnen voor partitionering van gegevens][DataPartitioningGuidance]

    Strategieën waarmee u kunt gegevens van de partitie te verbeteren, schaalbaarheid, minder bronconflicten en prestaties te optimaliseren.

- [Richtsnoeren voor bewaking en diagnose][MonitoringandDiagnosticsGuidance] 

    Richtlijnen voor het bijhouden van hoe uw gebruikers gebruikmaken van uw systeem brongebruik te traceren en de gezondheid en de prestaties van uw systeem over het algemeen te controleren.

- [Aanbevolen voor naamgeving][naming-conventions] 

    Aanbevolen voor naamgeving van Azure bronnen.

- [Algemene richtlijnen opnieuw][RetryGeneralGuidance] 

    Bespreking van de algemene concepten voor het afhandelen van fouten van voorbijgaande aard.

- [Service-specifieke richtlijnen opnieuw][RetryServiceSpecificGuidance]

    Een overzicht van de functies opnieuw voor een groot aantal Azure services, waaronder informatie waarmee u kunt gebruiken, aanpassen of uitbreiden van het mechanisme opnieuw voor die service.

## <a name="scenario-guides"></a>Scenario hulplijnen

- [Elasticsearch uitgevoerd op Azure][elasticsearch] 
    
    Elasticsearch is een zeer schaalbare open source zoekmachine en de database. Het is geschikt voor situaties waarin snelle analyse en het ontdekken van informatie in grote datasets. Deze richtsnoeren wordt gekeken naar enkele belangrijke aspecten rekening te houden bij het ontwerpen van een cluster Elasticsearch.

- [Identiteitsbeheer voor multitenant toepassingen][identity-multitenant] 
    
    Multitenancy is een architectuur waarbij meerdere huurders delen van een toepassing, maar zijn van elkaar gescheiden. Deze instructies hoe u gebruikersidentiteiten in een multitenant-toepassing met [Azure Active Directory] beheren[ AzureAD] voor het verwerken van verificatie- en.
    
- [Ontwikkelen van oplossingen voor grote](https://msdn.microsoft.com/library/dn749874.aspx)

    Deze gids behandelt het gebruik van HDInsight voor scenario's zoals iteratieve exploratie, als een datawarehouse voor ETL processen en integratie in de bestaande BI-systemen. Het bevat ook richtlijnen voor informatie over de concepten van big data, planning en oplossingen voor grote ontwerpen en implementeren van deze oplossingen.
    
## <a name="patterns"></a>Patronen

- [Wolk ontwerppatronen: Architectuurrichtlijn voor Cloud-toepassingen](https://msdn.microsoft.com/library/dn568099.aspx)

    Wolk ontwerppatronen is een bibliotheek met ontwerppatronen en richtlijnen voor gerelateerde onderwerpen. Het articulates het voordeel van patronen toepassen door te laten zien hoe elk stuk toepassingsarchitecturen cloud kunt past.
    
- [Optimaliseren van prestaties voor Cloud-toepassingen](https://github.com/mspnp/performance-optimization)

    Deze richtlijnen zijn een exploratie van algemene anti-patronen die apps wordt geschaald onder belasting belemmeren. Programmavoorbeelden acht anti-patronen en een [Prestatie-analyse primer](https://github.com/mspnp/performance-optimization/blob/master/Performance-Analysis-Primer.md) en een handleiding voor de [beoordeling van de prestaties ten opzichte van de essentiële gegevens](https://github.com/mspnp/performance-optimization/blob/master/Assessing-System-Performance-Against-KPI.md)bevat.

## <a name="reference-architectures"></a>Referentie-architecturen

Onze referentie-architecturen zijn geordend op scenario.
Elke afzonderlijke architectuur biedt aanbevolen procedures en aanbevolen stappen en een uitvoerbare component die de aanbevelingen.

De huidige bibliotheek van referentie-architecturen is beschikbaar op [http://aka.ms/architecture](http://aka.ms/architecture).

## <a name="resiliency-guidance"></a>Richtlijnen voor tolerantie

Deze onderwerpen wordt beschreven hoe u toepassingen ontwerpen die bestand tegen storingen in een wolk van gedistribueerde omgeving zijn.   

- [Tolerantie-overzicht][ResiliencyOvervew]

     Het bouwen van toepassingen op de Azure-platform kunnen herstellen na storingen en blijven functioneren. Beschrijving van een gestructureerde aanpak om te bereiken, tolerantie, van ontwerp tot implementatie, implementatie en bewerkingen.

- [Controlelijst voor tolerantie][resiliency-checklist]

    Een controlelijst met aanbevelingen die u helpen plannen voor verschillende storingsmodi die kan optreden.

- [Modus Foutanalyse][resiliency-fma] 

    Fout is-modus (FMA) een proces voor het bouwen van tolerantie in een systeem, door het identificeren van mogelijke fouten. Als uitgangspunt voor uw volledige Postbustoegang proces bevat dit artikel een catalogus van potentiële storingen op te sporen en hun beperkingen. 

<!-- links -->

[AzureAD]: https://azure.microsoft.com/documentation/services/active-directory/

[PerformanceOptimization]: https://github.com/mspnp/performance-optimization

[APIDesign]: ../best-practices-api-design.md
[APIImplementation]: ../best-practices-api-implementation.md
[AutoscalingGuidance]: ../best-practices-auto-scaling.md
[BackgroundJobsGuidance]: ../best-practices-background-jobs.md
[CDNGuidance]: ../best-practices-cdn.md
[CachingGuidance]: ../best-practices-caching.md
[DataPartitioningGuidance]: ../best-practices-data-partitioning.md
[MonitoringandDiagnosticsGuidance]: ../best-practices-monitoring.md
[RetryGeneralGuidance]: ../best-practices-retry-general.md
[RetryServiceSpecificGuidance]: ../best-practices-retry-service-specific.md
[RetryPolicies]: Retry-Policies.md
[ScalabilityChecklist]: ../best-practices-scalability-checklist.md
[AvailabilityChecklist]: ../best-practices-availability-checklist.md
[naming-conventions]: guidance-naming-conventions.md

<!-- guidance projects -->
[elasticsearch]: guidance-elasticsearch.md
[identity-multitenant]: guidance-multitenant-identity.md

<!-- reference architectures -->
[ref-arch-single-vm-windows]: guidance-compute-single-vm.md
[ref-arch-single-vm-linux]: guidance-compute-single-vm-linux.md
[ref-arch-multi-vm]: guidance-compute-multi-vm.md
[ref-arch-3-tier]: guidance-compute-3-tier-vm.md
[ref-arch-n-tier-windows]: guidance-compute-n-tier-vm.md
[ref-arch-n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[ref-arch-multi-dc-windows]: guidance-compute-multiple-datacenters.md
[ref-arch-multi-dc-linux]: guidance-compute-multiple-datacenters-linux.md

<!-- resiliency -->
[resiliency-fma]: guidance-resiliency-failure-mode-analysis.md
[resiliency-checklist]: guidance-resiliency-checklist.md
[ResiliencyOvervew]: guidance-resiliency-overview.md

