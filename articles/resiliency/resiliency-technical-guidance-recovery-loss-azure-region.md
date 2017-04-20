<properties
   pageTitle="Tolerantie voor herstel na verlies van een technische richtsnoeren Azure regio | Microsoft Azure"
   description="Artikel op inzicht en veerkrachtige, hoge beschikbaarheid, fout tolerant toepassingen ontwerpen en plannen voor noodherstel"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance-recovery-from-a-region-wide-service-disruption"></a>Technische richtsnoeren Azure tolerantie: herstel van een hele regio onderbrekingen

Azure is fysiek en logisch onderverdeeld in eenheden, de zogenaamde regio's. Een regio bestaat uit een of meer datacenters in de nabijheid. Op het moment van schrijven van dit heeft Azure vierentwintig gebieden over de hele wereld.

In zeldzame gevallen is het mogelijk dat faciliteiten in een hele regio benaderd, bijvoorbeeld als gevolg van netwerkfouten worden kunnen. Of faciliteiten kunnen verloren gaan geheel, bijvoorbeeld als gevolg van een natuurramp. Deze sectie wordt uitgelegd van de mogelijkheden van Azure voor het maken van toepassingen die zijn verdeeld over de regio's. Deze verdeling helpt minimaliseert u het risico dat een storing in een regio kan van invloed zijn op andere regio's.

##<a name="cloud-services"></a>Cloud services

###<a name="resource-management"></a>Bronbeheer

Door een aparte cloud-service in elke doelregio maken en publiceren het implementatiepakket voor de desbetreffende service cloud kunt u compute exemplaren distribueren tussen de regio's. Bedenk wel dat verkeer distribueren over cloud services in verschillende regio's moet worden geïmplementeerd door de ontwikkelaar van de toepassing of met een service voor verkeer.

Bepaling van het aantal exemplaren van de vrije rol van tevoren implementeren voor noodherstel is een belangrijk aspect van de capaciteitsplanning. Met een volledige implementatie van de secundaire zorgt ervoor dat de capaciteit al beschikbaar zijn is wanneer dat nodig is; echter dit effectief wordt verdubbeld de kosten. Een vast patroon is een kleine, secundaire implementatie, net groot genoeg is voor het uitvoeren van essentiële services zijn. Deze kleine secundaire distributie is een goed idee, zowel capaciteit, en voor het testen van de configuratie van de secundaire omgeving te reserveren.

>[AZURE.NOTE]Het abonnementenquotum is niet een garantie van de capaciteit. Het quotum is gewoon een kredietlimiet. Om te garanderen de capaciteit, het vereiste aantal rollen worden gedefinieerd in het service-model en de serverrollen worden geïmplementeerd.

###<a name="load-balancing"></a>Load Balancing

Verkeer tussen de regio's moet voor het verdelen van een oplossing voor het beheer van verkeer. Azure biedt [Azure verkeer Manager](https://azure.microsoft.com/services/traffic-manager/). U kunt ook profiteren van de services van andere leveranciers die soortgelijke verkeer mogelijkheden bieden.

###<a name="strategies"></a>Strategieën

Veel alternatieve strategieën zijn voor de implementatie van gedistribueerde berekenen tussen de regio's beschikbaar. Deze moeten worden aangepast aan de specifieke eisen en omstandigheden van de toepassing. Op een hoog niveau kunnen de benaderingen in de volgende categorieën worden verdeeld:

  * __Implementeren op een ramp__: In deze benadering de toepassing wordt opnieuw gedistribueerd helemaal ten tijde van de ramp. Dit is geschikt voor niet-kritieke toepassingen die niet een gegarandeerde hersteltijd vereisen.

  * __Warme reserveonderdeel (actief/passief)__: een secundaire gehoste service is gemaakt in een andere regio en functies worden geïmplementeerd om te kunnen garanderen, minimale capaciteit; echter ontvangt, de rollen niet verkeer productie. Deze benadering is handig voor toepassingen die niet zijn ontworpen voor het verkeer tussen de regio's te verdelen.

  * __Hot Spare (actief/actief)__: de toepassing is ontworpen voor het laden van de productie wordt in meerdere regio's. De cloud-services in elke regio is mogelijk geconfigureerd voor een hogere capaciteit dan vereist voor noodherstel. Ook de cloud-services kunnen schalen out indien nodig op het moment van een ramp en overname bij storingen. Hiervoor zijn aanzienlijke investeringen in het ontwerp van toepassing, maar heeft aanzienlijke voordelen. Deze omvatten een lage en gegarandeerde hersteltijd, continue controle van alle locaties van herstel en efficiënt gebruik van de capaciteit.

Een uitgebreide bespreking van het ontwerp van gedistribueerde valt buiten het bereik van dit document. Zie [noodherstel en Azure toepassingen voor maximale beschikbaarheid](https://aka.ms/drtechguide)voor meer informatie.

##<a name="virtual-machines"></a>Virtuele Machines

Herstel van de infrastructuur als een service (IaaS) virtuele machines (VMs) is vergelijkbaar met het platform als een service (PaaS) herstel in veel opzichten berekenen. Zijn er belangrijke verschillen, maar vanwege het feit dat een VM IaaS uit zowel de VM en de VM-schijf bestaat.

  * __Gebruik Azure back-up maken back-ups cross regio die toepassing die in overeenstemming zijn__.
  [Azure back-up](https://azure.microsoft.com/services/backup/) kunnen klanten toepassing consistent back-ups maken op meerdere schijven van VM en ondersteuning voor de replicatie van back-ups tussen de regio's. U kunt dit doen door te kiezen geo repliceren de kluis back-up op het moment van maken. Houd er rekening mee dat de replicatie van de back-up kluis moet worden geconfigureerd op het moment van maken. Het kan niet later worden ingesteld. Als een gebied verloren gegaan is, zal Microsoft de back-ups beschikbaar maken voor klanten. Klanten moeten kunnen herstellen naar een van hun geconfigureerde herstelpunten.

  * __Scheiden van de schijf met gegevens van de schijf__. Een belangrijke overweging voor VMs IaaS is dat u de schijf niet wijzigen zonder de VM opnieuw te maken. Dit is geen probleem als uw herstelstrategie voor opnieuw te implementeren na de ramp. Het kan wel een probleem als u de warme Spare aanpak om capaciteit te reserveren. Om dit goed doet, moet u het juiste besturingssysteem schijf op de primaire en secundaire locaties geïmplementeerd hebben en moeten de toepassingsgegevens worden opgeslagen op een afzonderlijke schijf. Gebruik indien mogelijk een standaard besturingssysteemconfiguratie die u op beide locaties. Nadat failover is uitgevoerd, moet u vervolgens het gegevensstation koppelen aan uw bestaande IaaS VMs in de secundaire DC. Gebruik AzCopy om momentopnamen van de schijven met gegevens kopiëren naar een externe site.

  * __Rekening houden met mogelijke consistentie na een geo-failover van meerdere VM-schijven__. VM-schijven worden geïmplementeerd als opslag Azure BLOB's en geo-replicatie hetzelfde kenmerk hebben. Tenzij [Azure back-up](https://azure.microsoft.com/services/backup/) wordt gebruikt, zijn er geen waarborgen van consistentie op schijven, omdat geo-replicatie is een asynchrone en onafhankelijk van elkaar repliceert. Afzonderlijke schijven van VM zijn gegarandeerd in een crash consistente status na een geo-failover, maar niet consistent over schijven. Dit kan problemen veroorzaken in sommige gevallen (bijvoorbeeld bij schijf-striping).

##<a name="storage"></a>Opslag

###<a name="recovery-by-using-geo-redundant-storage-of-blob-table-queue-and-vm-disk-storage"></a>Herstel met behulp van Geo-redundante opslag van blob, tabel, wachtrij en VM schijfopslag

In Azure BLOB's, tabellen, wachtrijen en VM zijn schijven alle geo-gerepliceerde standaard. Dit is bedoeld als Geo-redundante opslag (GRS). Opslaggegevens repliceert GRS naar een gepaarde datacenter honderden van mijlen uit elkaar binnen een bepaalde geografische regio. GRS is ontworpen voor extra duurzaamheid is er een groot datacenter ramp. Microsoft-besturingselementen bij een storing en de failover is beperkt tot grote rampen die de oorspronkelijke primaire locatie wordt geacht onherstelbaar binnen een redelijke tijd. In sommige gevallen kan dit enkele dagen zijn. Gegevens worden normaal gesproken binnen een paar minuten gerepliceerd Hoewel synchronisatie-interval nog niet door een service level agreement gedekt wordt.

Bij een failover geo, zal er geen wijziging in de wijze van toegang de account tot (de URL en account-sleutel wordt niet gewijzigd). De account van de opslag zal evenwel in een andere regio na een failover. Dit kan van invloed op toepassingen waarvoor regionale affiniteit met hun account opslag. Zelfs voor services en toepassingen die geen een account voor opslag in het datacenter van dezelfde vereist kunnen de cross-datacenter latentie en bandbreedte kosten worden dwingende redenen verkeer tijdelijk verplaatsen naar de failover-regio. Dit kan in een algemene noodherstelplan factor.

Naast automatische failover verstrekt door GRS, is Azure een geïntroduceerd waarmee u leestoegang tot de kopie van de gegevens in de secundaire opslaglocatie. Dit heet leestoegang Geo-redundante opslag (RA-GRS).

Zie voor meer informatie over zowel GRS als RA GRS opslag [opslag Azure-replicatie](../storage/storage-redundancy.md).

###<a name="geo-replication-region-mappings"></a>Gebiedstoewijzingen geo-replicatie:

Het is belangrijk te weten waar uw gegevens geo gerepliceerd, om te weten waar de andere exemplaren van uw gegevens waarvoor regionale affiniteit met uw opslag te implementeren. De volgende tabel ziet u de primaire en secundaire locatie koppelingen:

[AZURE.INCLUDE [paired-region-list](../../includes/paired-region-list.md)]

###<a name="geo-replication-pricing"></a>Geo-replicatie prijzen:

Geo-replicatie is opgenomen in de huidige prijzen voor opslag van Azure. Dit heet Geo-redundante opslag (GRS). Als u niet dat uw gegevens gerepliceerd geo wilt-kunt u de geo-replicatie uitschakelen voor uw account. Dit lokaal redundante opslag wordt genoemd en wordt in rekening gebracht tegen een verlaagde prijs vergeleken met GRS.

###<a name="determining-if-a-geo-failover-has-occurred"></a>Om te bepalen als een geo-heeft plaatsgevonden

Als een geo-failover plaatsvindt, wordt dit aan het [Dashboard van Azure Service gezondheid](https://azure.microsoft.com/status/)worden geboekt. Toepassingen kunnen opsporen, echter door de geo-regio voor hun rekening opslag controleren een geautomatiseerde manier implementeren. Dit kan worden gebruikt voor het starten van andere herstelbewerkingen, zoals de activering van bronnen in de geo-regio waar de opslag worden verplaatst naar compute. U kunt uitvoeren een query voor deze van de service management API, met behulp van [Opslageigenschappen ophalen](https://msdn.microsoft.com/library/ee460802.aspx). De relevante eigenschappen zijn:

    <GeoPrimaryRegion>primary-region</GeoPrimaryRegion>
    <StatusOfPrimary>[Available|Unavailable]</StatusOfPrimary>
    <LastGeoFailoverTime>DateTime</LastGeoFailoverTime>
    <GeoSecondaryRegion>secondary-region</GeoSecondaryRegion>
    <StatusOfSecondary>[Available|Unavailable]</StatusOfSecondary>

###<a name="vm-disks-and-geo-failover"></a>VM-schijven en geo-failover

Zoals besproken in de sectie op VM schijven, zijn er geen garanties met betrekking tot de consistentie van de gegevens op schijven van VM nadat failover is uitgevoerd. Om de juistheid van de back-ups, moet een back-product zoals Data Protection Manager worden gebruikt voor back-up en terugzetten van toepassingsgegevens.

##<a name="database"></a>Database

###<a name="sql-database"></a>SQL-Database

Azure SQL-Database biedt twee soorten herstel: Geo-terugzetten en actieve Geo-replicatie.

####<a name="geo-restore"></a>Geo-terugzetten

[Geo-herstel](../sql-database/sql-database-recovery-using-backups.md#geo-restore) is ook beschikbaar met de Basic-, Standard- en Premium-databases. Het biedt de standaardhersteloptie wanneer de database niet beschikbaar is vanwege een incident in de regio waar de database wordt gehost. Net als bij punt In tijd herstellen, Geo terugzetten is afhankelijk van back-ups in geo-redundante opslag van Azure. Deze worden hersteld vanaf de back-up met geo gerepliceerd en daarom is tegen storingen van de opslag in de regio voor primaire. Zie [een failover naar een secundair of een Azure SQL-Database terugzetten](../sql-database/sql-database-disaster-recovery.md)voor meer informatie.

####<a name="active-geo-replication"></a>Actieve Geo-replicatie

[Actieve Geo-replicatie](../sql-database/sql-database-geo-replication-overview.md) is beschikbaar voor alle lagen van de database. Het ontworpen voor toepassingen met herstelvereisten agressiever dan Geo terugzetten kan bieden. Actieve Geo-replicatie kunt u secundaire servers voor maximaal vier leesbaar maken op servers in verschillende regio's. U kunt de failover naar een van de secundaire servers starten. Bovendien kan Active Geo-replicatie worden gebruikt voor ondersteuning van de toepassing of verplaatsing upgrade scenario's, zowel als load balancing voor de werklast alleen-lezen. Voor meer informatie, Zie [Geo-replicatie configureren](../sql-database/sql-database-geo-replication-portal.md) en [failover uitvoeren naar de secundaire database](../sql-database/sql-database-geo-replication-failover-portal.md). Verwijzen naar [ontwerp van een toepassing voor noodherstel cloud via actieve Geo-replicatie in SQL-Database](../sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery.md) en [SQL Database actief Geo-replicatie met behulp van rolling upgrades van cloud-toepassingen beheren](../sql-database/sql-database-manage-application-rolling-upgrade.md) voor meer informatie over het ontwerpen en implementeren toepassingen en toepassingen bijwerken zonder uitvaltijd.

###<a name="sql-server-on-virtual-machines"></a>SQL Server op de virtuele Machines

Tal van opties beschikbaar zijn voor nuttige toepassing en de beschikbaarheid van SQL Server 2012 (en hoger) wordt uitgevoerd in Azure virtuele Machines. Zie voor meer informatie, [hoge beschikbaarheid en noodherstel voor SQL Server in Azure virtuele Machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

##<a name="other-azure-platform-services"></a>Andere services Azure platform

Wanneer u probeert uw cloud-service wordt uitgevoerd in meerdere Azure regio's, moet u rekening houden met de gevolgen voor elk van de afhankelijkheden. In de volgende secties de service-specifieke instructies wordt ervan uitgegaan dat u dezelfde Azure service in een alternatieve Azure datacenter gebruiken moet. Dit betekent dat configuratie- en gegevensreplicatie taken.

>[AZURE.NOTE]In sommige gevallen kunt deze stappen u een stroomstoring servicespecifieke in plaats van een gebeurtenis van het hele datacenter beperken. Vanuit het perspectief van toepassing, een stroomstoring servicespecifieke mogelijk net zo te beperken en waarvoor de service tijdelijk te migreren naar een andere regio Azure.

###<a name="service-bus"></a>Bus service

Azure Service Bus gebruikt een unieke naamruimte die niet voor Azure regio's gelden. De eerste vereiste is om de benodigde service bus naamruimten in de andere regio in te stellen. Er zijn echter ook overwegingen voor de duurzaamheid van de berichten in de wachtrij. Er zijn verschillende strategieën voor het repliceren van berichten tussen de Azure regio's. Zie voor een overzicht van deze strategieën voor replicatie en andere strategieën disaster recovery, [Aanbevolen procedures voor isolerende toepassingen tegen Bus Service storingen en calamiteiten](../service-bus-messaging/service-bus-outages-disasters.md). Zie voor andere overwegingen beschikbaarheid [Service Bus (beschikbaarheid)](./resiliency-technical-guidance-recovery-local-failures.md#other-azure-platform-services).

###<a name="app-service"></a>App-Service

Als u wilt een Azure App Service-toepassing, zoals Web Apps of mobiele Apps, migreren naar een gebied met secundaire Azure, hebt u een back-up van de website gepubliceerd. Als de onderbreking heeft geen betrekking op het hele Azure datacenter, is het mogelijk om FTP te gebruiken voor het downloaden van een recente back-up van de inhoud van de site. Maak een nieuwe toepassing in het gebied van de alternatieve tenzij u capaciteit reserveert u eerder hebt gedaan. De site publiceert op het nieuwe gebied en alle vereiste configuratiewijzigingen aanbrengen. Deze wijzigingen kunnen tekenreeksen voor een databaseverbinding of andere regio-specifieke instellingen omvatten. Indien nodig, het SSL-certificaat van de site toevoegen en de DNS CNAME-record te wijzigen zodat de aangepaste domeinnaam naar de opnieuw gedistribueerde Azure Web App-URL verwijst.

###<a name="hdinsight"></a>HDInsight

De gegevens die zijn gekoppeld aan de HDInsight wordt standaard opgeslagen in Azure Blob-opslag. HDInsight is vereist dat een Hadoop cluster verwerken MapReduce taken samen moet zich bevinden in hetzelfde gebied, als de opslag rekening met de gegevens geanalyseerd. Voorwaarde dat u de functie geo-replicatie naar Azure opslag gebruikt, kunt u uw gegevens in de secundaire regio waar de gegevens als om welke reden de primaire regio niet meer beschikbaar is gerepliceerd openen. In het gebied waar de gegevens is gerepliceerd en doorgaan met het verwerken van deze kunt u een nieuw cluster met Hadoop. Zie voor andere overwegingen beschikbaarheid [(beschikbaarheid) HDInsight](./resiliency-technical-guidance-recovery-local-failures.md#other-azure-platform-services).

###<a name="sql-reporting"></a>SQL Reporting

Op dit moment moet herstellen van het verlies van een Azure regio meerdere exemplaren van SQL Reporting in verschillende gebieden van Azure. Deze exemplaren van SQL Reporting moeten toegang hebben tot dezelfde gegevens en gegevens moet hebben een eigen herstel plan een storing. Externe back-ups van de RDL voor elk rapport kunt u ook bijhouden.

###<a name="media-services"></a>Mediaservices

Azure Media Services heeft een andere herstelmethode aanpak voor het coderen en streaming. Streaming is meestal belangrijker tijdens een stroomstoring regionale. Als voorbereiding voor dit, moet u een Media-Services-account hebben in twee verschillende Azure regio's. De gecodeerde inhoud bevindt zich in beide regio's. Bij storingen, kunt u de streaming verkeer omleiden naar de andere regio. Codering kan worden uitgevoerd in elke regio Azure. Als codering tijdgevoelige, bijvoorbeeld tijdens de verwerking van de gebeurtenis live, moet u bereid zijn taken kunnen verzenden naar een ander datacenter bij defecten zijn.

###<a name="virtual-network"></a>Virtueel netwerk

Configuratiebestanden bevatten de snelste manier om een virtueel netwerk in een alternatieve Azure regio in te stellen. Na het configureren van het virtuele netwerk in de primaire Azure regio [het virtuele netwerkinstellingen exporteren](../virtual-network/virtual-networks-create-vnet-classic-portal.md) in het huidige netwerk naar een configuratiebestand netwerk. In geval van een storing in de primaire regio, [het virtuele netwerk herstellen](../virtual-network/virtual-networks-create-vnet-classic-portal.md) in het configuratiebestand opgeslagen. Andere cloud-services, virtuele machines of cross-premises instellingen om te werken met de nieuwe virtuele netwerk configureert.

##<a name="checklists-for-disaster-recovery"></a>Controlelijsten voor noodherstel

##<a name="cloud-services-checklist"></a>Controlelijst voor cloud Services

  1. Raadpleeg de sectie Cloud Services van dit document.
  2. Maak een noodherstelplan cross-regio.
  3. E-mailverkeer in reserveren capaciteit in andere regio's begrijpen.
  4. Routering extra verkeer, zoals Azure verkeer Manager gebruiken.

##<a name="virtual-machines-checklist"></a>Controlelijst voor virtuele Machines

  1. Raadpleeg de sectie virtuele Machines van dit document.
  2. Met [Back-up Azure](https://azure.microsoft.com/services/backup/) toepassing consistent back-ups maken tussen de regio's.

##<a name="storage-checklist"></a>Controlelijst voor opslag

  1. Raadpleeg de sectie opslag van dit document.
  2. Geo-replicatie van opslagbronnen niet uitschakelen
  3. Alternatieve regio voor geo-replicatie begrijpen in geval van failover.
  4. Maak aangepaste back-upstrategieën voor failover-gebruiker beheerde strategieën.

##<a name="sql-database-checklist"></a>Controlelijst voor SQL-Database

  1. Raadpleeg de sectie SQL-Database van dit document.
  2. [Geo-terugzetten](../sql-database/sql-database-recovery-using-backups.md#geo-restore) gebruiken of [Geo-replicatie](../sql-database/sql-database-geo-replication-overview.md) zo nodig.

##<a name="sql-server-on-virtual-machines-checklist"></a>SQL Server op de controlelijst voor virtuele Machines

  1. Controleer de SQL-Server op de virtuele Machines gedeelte van dit document.
  2. Cross-regio AlwaysOn beschikbaarheidsgroepen of database-mirroring te gebruiken.
  3. Afwisselend met back-up en terugzetten om een blob-opslag.

##<a name="service-bus-checklist"></a>Checklist Service Bus

  1. Raadpleeg de sectie Service Bus van dit document.
  2. Een naamruimte Bus Service configureren in een andere regio.
  3. U kunt aangepaste replicatie strategieën voor berichten tussen de regio's.

##<a name="app-service-checklist"></a>Controlelijst voor App-Service

  1. Raadpleeg de sectie App Service van dit document.
  2. Site back-ups buiten het primaire regio onderhouden.
  3. Als de storing is gedeeltelijk, probeert op te halen van de huidige site met FTP.
  4. Plan voor de implementatie van de site naar een nieuwe of bestaande site in een andere regio.
  5. Plan de wijzigingen in de configuratie voor toepassings- en CNAME DNS-records.

##<a name="hdinsight-checklist"></a>Checklist voor HDInsight

  1. Raadpleeg de sectie HDInsight van dit document.
  2. Maak een nieuw cluster van Hadoop in de regio met gerepliceerde gegevens.

##<a name="sql-reporting-checklist"></a>Controlelijst voor SQL Reporting

  1. Raadpleeg de sectie SQL Reporting van dit document.
  2. Onderhouden van een ander exemplaar van de SQL Reporting in een andere regio.
  3. Een apart plan voor het repliceren van het doel naar die regio onderhouden.

##<a name="media-services-checklist"></a>Controlelijst voor Media Services

  1. Raadpleeg de sectie Media Services van dit document.
  2. Een Media-Services-account maken in een andere regio.
  3. De inhoud van beide regio's ter ondersteuning van streaming failover te coderen.
  4. Codering taken naar een andere regio in geval van een onderbreking van de service verzenden.

##<a name="virtual-network-checklist"></a>Virtueel netwerk controlelijst

  1. Raadpleeg de sectie virtueel netwerk van dit document.
  2. Gebruik instellingen van virtueel netwerk opnieuw op te maken in een andere regio geëxporteerd.

##<a name="next-steps"></a>Volgende stappen

Dit artikel maakt deel uit van een serie gericht op [Azure resiliency technische richtsnoeren](./resiliency-technical-guidance.md). Het volgende artikel in deze serie is gericht op [herstel van een datacenter in ruimten met Azure](./resiliency-technical-guidance-recovery-on-premises-azure.md).
