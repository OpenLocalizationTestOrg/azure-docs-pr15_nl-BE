<properties
   pageTitle="Flexibele technische richtsnoeren voor het herstellen van gegevens beschadigd raken of per ongeluk wordt verwijderd | Microsoft Azure"
   description="Artikel met informatie over het herstellen van beschadigde gegevens van gegevens of gegevens per ongeluk verwijdering en veerkrachtige, hoge beschikbaarheid, fout tolerant toepassingen ontwerpen, alsmede plannen voor noodherstel"
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

#<a name="azure-resiliency-technical-guidance-recovery-from-data-corruption-or-accidental-deletion"></a>Technische richtsnoeren Azure tolerantie: herstel van gegevens beschadigd raken of per ongeluk wordt verwijderd

Onderdeel van een solide plan voor bedrijfscontinuïteit is een plan dat als uw gegevens wordt beschadigd of per ongeluk verwijderd. Hieronder volgt informatie over het herstellen nadat gegevens zijn beschadigd of per ongeluk verwijderd vanwege fouten of een exploitant fout.

##<a name="virtual-machines"></a>Virtuele Machines

Als u wilt uw Azure virtuele Machines (ook wel VMs infrastructure as a service) te beschermen tegen toepassingsfouten of per ongeluk wordt verwijderd, [Azure back-up](https://azure.microsoft.com/services/backup/)te gebruiken. Azure back-up kunt maken van back-ups die op meerdere schijven van VM consistent zijn. Bovendien kan de kluis back-up worden gerepliceerd in gebieden voor herstel van het verlies van de regio.

##<a name="storage"></a>Opslag

Houd er rekening mee dat hoewel Azure opslag gegevens resiliency via geautomatiseerde replica's, dit niet dat voorkomt uw toepassingscode (of ontwikkelaars/gebruikers) van gegevens te beschadigen door per ongeluk of onbedoelde verwijdering en update. Handhaving van de betrouwbaarheid van de gegevens ten aanzien van de toepassing of gebruiker fout moet meer geavanceerde technieken, zoals het kopiëren van gegevens naar een secundaire opslaglocatie met een controlelogboek. Ontwikkelaars kunnen profiteren van de blob- [opname mogelijkheden](https://msdn.microsoft.com/library/azure/ee691971.aspx), die alleen-lezen point-in-time momentopnamen van de blob inhoud kunt maken. Dit kan worden gebruikt als basis voor een gegevens-fidelity oplossing voor opslag van Azure BLOB's.

###<a name="blob-and-table-storage-backup"></a>BLOB en tabel opslag back-up

BLOB's en tabellen zijn zeer duurzaam, geven ze altijd de huidige status van de gegevens. Herstel van ongewenste wijziging of verwijdering van gegevens mogelijk gegevens terugzet naar een eerdere staat. Dit kan worden bereikt door gebruik te maken van de mogelijkheden van Azure opslaan en point-in-time kopieën behouden.

Voor Azure BLOB's, kunt u point-in-time back-ups met de [snapshot-functie blob](https://msdn.microsoft.com/library/ee691971.aspx)uitvoeren. Voor elke momentopname, er worden alleen in rekening gebracht voor de opslagruimte die nodig is voor het opslaan van de verschillen in de blob sinds de laatste momentopname staat. De momentopnamen zijn afhankelijk van het bestaan van de oorspronkelijke blob die ze zijn gebaseerd op, zodat een bewerking kopiëren naar een andere blob of zelfs een andere opslag-account aan te raden is. Dit zorgt ervoor dat de back-upgegevens goed is beveiligd tegen onopzettelijk verwijderen. Voor Azure tabellen, kunt u point-in-time kopieën maken naar een andere tabel of Azure BLOB's. Meer gedetailleerde instructies en voorbeelden van het uitvoeren van op toepassingsniveau back-ups van tabellen en BLOB's vindt u hier:

  * [Bescherming van uw tabellen tegen toepassingsfouten](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/05/03/protecting-your-tables-against-application-errors/)
  * [Bescherming van de BLOB's tegen toepassingsfouten](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/04/29/protecting-your-blobs-against-application-errors/)

##<a name="database"></a>Database

Er zijn verschillende opties voor [bedrijfscontinuïteit](../sql-database/sql-database-business-continuity.md) (back-up en terugzetten) beschikbaar voor Azure SQL-Database. Databases kunnen worden gekopieerd met behulp van de functionaliteit van de [Database kopiëren](../sql-database/sql-database-copy.md) of door [het exporteren](../sql-database/sql-database-export.md) en [importeren van](https://msdn.microsoft.com/library/hh710052.aspx) een SQL Server Bacpac-bestand. Database is transactioneel consistente resultaten, maar niet door een bacpac (via de service voor importeren/exporteren). Met beide opties worden uitgevoerd als services binnen het datacenter op basis van wachtrijen en ze momenteel niet bieden een SLA tijd te voltooien.

>[AZURE.NOTE]Opties voor de database kopiëren en importeren/exporteren plaatsen een aanzienlijke mate van belasting op de brondatabase. Ze kunnen bronconflicten of beperken van de gebeurtenissen activeren.

###<a name="sql-database-backup"></a>Back-up van SQL-Database

Point-in-time back-ups voor Microsoft Azure SQL-Database worden bereikt door [de Azure SQL-database kopiëren](../sql-database/sql-database-copy.md). Met deze opdracht kunt u een transactioneel consistente kopie maken van een database op dezelfde logische databaseserver of naar een andere server. In beide gevallen wordt is de databasekopie volledig functioneel en is volledig onafhankelijk van de brondatabase. Elke kopie die u maakt vertegenwoordigt een point-in-time hersteloptie. U kunt de database staat volledig herstellen door de naam van de nieuwe database met de naam van de bron. U kunt ook een specifieke subset van gegevens uit de nieuwe database herstellen met behulp van Transact-SQL-query's. Zie voor meer informatie over de SQL-Database [-overzicht van bedrijfscontinuïteit met Azure SQL-Database](../sql-database/sql-database-business-continuity.md).

###<a name="sql-server-on-virtual-machines-backup"></a>SQL Server op de back-up van virtuele Machines

Voor SQL Server gebruikt in combinatie met Azure infrastructuur als een service virtuele machines (vaak IaaS of IaaS VMs genoemd), zijn er twee opties: traditionele back-ups en logboekbestanden. Traditionele back-ups, kunt u om te zetten naar een bepaald punt in de tijd, maar het herstel is traag. Traditionele back-ups terugzetten, moet beginnen met een eerste volledige back-up en vervolgens alle back-ups moeten na die toe te passen. De tweede optie is een sessie om te vertragen het terugzetten van back-ups van logboekbestanden (bijvoorbeeld door twee uur) voor logboekbestanden configureren. Dit biedt een venster voor het herstellen van fouten die zijn gemaakt op de primaire.

##<a name="other-azure-platform-services"></a>Andere services Azure platform

Sommige services Azure platform informatie in een gebruiker gecontroleerde opslag of Azure SQL-Database opgeslagen. Als de account- of de resource is verwijderd of beschadigd, kan dit ernstige fouten veroorzaken met de service. In deze gevallen is het belangrijk dat u back-ups, waarmee u deze bronnen opnieuw te maken als ze zijn verwijderd of beschadigd.

Voor websites Azure en Azure Mobile Services, moet u een back-up en de bijbehorende databases onderhouden. Voor Azure Media en virtuele Machines, moet u de bijbehorende Azure opslag account en alle bronnen in die account bijhouden. Bijvoorbeeld voor virtuele Machines, moet u back-ups maken en beheren van de schijven van VM in Azure blob-opslag.

##<a name="checklists-for-data-corruption-or-accidental-deletion"></a>Controlelijsten voor beschadiging of onbedoelde verwijdering

##<a name="virtual-machines-checklist"></a>Controlelijst voor virtuele Machines

  1. Raadpleeg de sectie virtuele Machines van dit document.
  2. Back-up maken en onderhouden van de VM-schijven met Azure back-(of uw eigen back-upsysteem met Azure blob-opslag en VHD momentopnamen).

##<a name="storage-checklist"></a>Controlelijst voor opslag

  1. Raadpleeg de sectie opslag van dit document.
  2. Regelmatig back-ups van kritieke opslagbronnen.
  3. Houd rekening met de snapshot-functie voor het BLOB's.

##<a name="database-checklist"></a>Controlelijst voor database

  1. Raadpleeg de sectie Database van dit document.
  2. Point-in-time back-ups maken met de opdracht Database kopiëren.

##<a name="sql-server-on-virtual-machines-backup-checklist"></a>SQL Server op de back-up van virtuele Machines, controlelijst

  1. Controleer de SQL-Server op de back-up van virtuele Machines gedeelte van dit document.
  2. Met traditionele back-up en herstel technieken.
  3. Een sessie voor vertraagde logboekbestanden maken.

##<a name="web-apps-checklist"></a>Controlelijst voor Web Apps

  1. Back-up maken en onderhouden van de gekoppelde database, indien aanwezig.

##<a name="media-services-checklist"></a>Controlelijst voor Media Services

  1. Een back-up en de bijbehorende opslagbronnen beheren.

##<a name="more-information"></a>Meer informatie

Zie voor meer informatie over functies voor back-up en terugzetten in Azure, [opslag, back-up en herstellen scenario's](https://azure.microsoft.com/documentation/scenarios/storage-backup-recovery/).

##<a name="next-steps"></a>Volgende stappen

Dit artikel maakt deel uit van een serie gericht op [Azure resiliency technische richtsnoeren](./resiliency-technical-guidance.md). Als u op zoek bent voor meer herstellingsvermogen Zie noodherstel en beschikbaarheid van resources, de Azure resiliency technische richtsnoeren [aanvullende bronnen](./resiliency-technical-guidance.md#additional-resources).