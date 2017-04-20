<properties
   pageTitle="Flexibele technische richtsnoeren index | Microsoft Azure"
   description="Index van de technische artikelen op inzicht en ontwerpen veerkrachtige, hoge beschikbaarheid, fouttolerante toepassingen, evenals planning voor herstel na storing herstel- en continuïteit"
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

#<a name="azure-resiliency-technical-guidance"></a>Technische richtsnoeren Azure tolerantie

##<a name="introduction"></a>Inleiding

Voldoen aan hoge beschikbaarheid en noodherstel herstelvereisten vereist twee soorten kennis:

- Gedetailleerde technische kennis van de mogelijkheden voor een cloud-platform
- Weten hoe een gedistribueerde service correct bouwen

Deze reeks artikelen behandelt de voormalige: de mogelijkheden en beperkingen van de Azure platform voor tolerantie (soms bedrijfscontinuïteit genoemd). Als u geïnteresseerd in deze laatste bent, raadpleegt u de serie artikelen gericht op [herstel na storingen en hoge beschikbaarheid voor Azure toepassingen](https://aka.ms/drtechguide). Hoewel het perfectioneren van deze artikel serie architectuur en ontwerp patronen, maar dat is niet de focus van de reeks. Voor advies, ontwerp, kunt u het materiaal in de sectie [aanvullende bronnen](#additional-resources) raadplegen.

Deze informatie is onderverdeeld in de volgende artikelen:

- [Lokale fouten herstellen](resiliency-technical-guidance-recovery-local-failures.md).
Fysieke hardware (zoals schijven, servers en netwerkapparaten) kan mislukken. Resources kunnen worden uitgeput wanneer een belasting bereikt. Dit artikel beschrijft de mogelijkheden die Azure biedt om hoge beschikbaarheid onder deze omstandigheden te onderhouden.

- [Herstel na een onderbreking Azure service van de hele regio](resiliency-technical-guidance-recovery-loss-azure-region.md).
Wijdverbreide storingen zijn zeldzaam, maar in theorie mogelijk zijn. Hele regio's kunnen worden geïsoleerd als gevolg van netwerkfouten of ze kunnen fysiek beschadigd tegen natuurrampen. In dit artikel wordt uitgelegd hoe u toepassingen maken die span geografisch verspreide gebieden met Azure.

- [Herstel van on-premises naar Azure](resiliency-technical-guidance-recovery-on-premises-azure.md).
De cloud wordt aanzienlijk gewijzigd de economie van noodherstel, waardoor organisaties hun Azure met tot stand brengen van een tweede site voor herstel. U kunt dit doen in een fractie van de kosten van de bouw en het onderhoud van een secundaire datacenter. In dit artikel wordt uitgelegd dat de mogelijkheden die Azure biedt voor het uitbreiden van een datacenter op het bedrijf naar de cloud.

- [Herstel van gegevens beschadigd raken of per ongeluk wordt verwijderd](resiliency-technical-guidance-recovery-data-corruption.md).
Toepassingen kunnen fouten hebben die gegevens beschadigd. Operators kunnen belangrijke gegevens onjuist verwijderen. Dit artikel wordt uitgelegd wat Azure biedt voor back-ups van gegevens maken en terugzetten naar een eerdere tijd.

##<a name="additional-resources"></a>Aanvullende bronnen

- [Noodherstel en hoge beschikbaarheid voor toepassingen die zijn gebouwd op Microsoft Azure](resiliency-disaster-recovery-high-availability-azure-applications.md).
In dit artikel wordt een gedetailleerd overzicht van beschikbaarheid en noodherstel. Dit omvat de uitdaging van de handmatige replicatie voor referentie- en transactiegegevens. De uiteindelijke secties bevatten voor samenvattingen van verschillende soorten disaster recovery topologieën die zich uitstrekken over Azure gebieden voor het hoogste niveau van beschikbaarheid.

- [Controlelijst voor hoge beschikbaarheid](resiliency-high-availability-checklist.md).
Dit artikel is een lijst van functies, services, en ontwerpen die u kunnen helpen verhogen de tolerantie en de beschikbaarheid van uw toepassing.

- [Richtlijnen voor Microsoft Azure Services herstellingsvermogen](resiliency-service-guidance-index.md).
In dit artikel wordt een index van Azure services en koppelingen naar zowel disaster recovery instructies en richtlijnen.

- [Overzicht: business continuity en database noodherstel met SQL-Database van de wolk](../sql-database/sql-database-business-continuity.md).
Dit artikel bevat Azure SQL Database technieken voor beschikbaarheid. Voornamelijk wordt gecentreerd op de strategieën voor back-up en terugzetten. Wanneer u Azure SQL-Database in de cloud-service gebruikt, raadpleegt u dit artikel en de bijbehorende bronnen.

- [Hoge beschikbaarheid en noodherstel voor SQL Server in Azure virtuele Machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).
Dit artikel wordt de van beschikbaarheidopties die u verkennen kunt wanneer u voor het hosten van uw databaseservices infrastructuur gebruikt als een service (IaaS). Het behandelt AlwaysOn beschikbaarheidsgroepen, database-mirroring voor logboekbestanden en back-up en terugzetten. Verschillende zelfstudies hoe deze technieken gebruiken.

- [Aanbevolen procedures voor het ontwerpen van grootschalige services op Azure Cloud Services](https://azure.microsoft.com//blog/best-practices-for-designing-large-scale-services-on-windows-azure/).
Dit artikel is gericht op het ontwikkelen van zeer schaalbare cloud-architecturen. Veel van de technieken die gebruikmaken van de schaalbaarheid te verbeteren ook de beschikbaarheid te verbeteren. Als uw toepassing niet meer onder de toegenomen belasting schalen, wordt schaalbaarheid een beschikbaarheidsprobleem.

- [Back-up en herstel voor SQL Server in Azure virtuele Machines](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).
Dit artikel bevat technische richtsnoeren op back-up maken en terugzetten van Microsoft SQL Server met Azure virtuele Machines.

- [Failsafe: richtlijnen voor robuuste wolk architecturen](https://channel9.msdn.com/Series/FailSafe).
Dit artikel bevat richtlijnen voor het ontwikkelen van robuuste cloud architectuur, richtsnoeren voor de uitvoering van deze architectuur op Microsoft-technologieën en recepten voor de uitvoering van deze architectuur voor specifieke scenario's.

- [Technische casestudy: cloud-technologieën noodherstel verbeteren met behulp van](https://www.microsoft.com/itshowcase/Article/Content/737/Using-cloud-technologies-to-improve-disaster-recovery).
Deze case-study ziet u hoe Microsoft IT Azure gebruikt voor het verbeteren van noodherstel.

##<a name="next-steps"></a>Volgende stappen

Dit artikel maakt deel uit van een serie gericht op technische richtsnoeren voor Azure herstellingsvermogen. Als u geïnteresseerd bent in andere artikelen in deze serie lezen, kunt u starten met het [herstel van lokale fouten](resiliency-technical-guidance-recovery-local-failures.md).
