<properties
   pageTitle="Gedistribueerde gegevens en opties voor de Massively Parallel Processing (MPP)-systemen van SQL Data Warehouse en Parallel datawarehouse verdeeld | Microsoft Azure"
   description="Informatie over hoe de gegevens worden verdeeld voor Massively Parallel Processing (MPP) en de opties voor het distribueren van tabellen in Azure SQL Data Warehouse en Parallel datawarehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/10/2016"
   ms.author="barbkess"/>


# <a name="distributed-data-and-distributed-tables-for-massively-parallel-processing-mpp"></a>Verspreiding van gegevens en gedistribueerde tabellen voor Massively Parallel Processing (MPP)

Informatie over hoe gebruikersgegevens worden gedistribueerd in Azure SQL Data Warehouse en Parallel datawarehouse Massively Parallel Processing (MPP)-van Microsoft systemen. Ontwerpen van uw gegevensmagazijn effectief gebruik van gedistribueerde gegevens helpt u bij het bereiken van de voordelen van de architectuur MPP verwerken van query's. Een paar opties voor database-ontwerp kunnen hebben een belangrijke invloed op de prestaties van query's te verbeteren.  

>[AZURE.NOTE] Azure SQL Data Warehouse en Parallel datawarehouse gebruiken hetzelfde ontwerp Massively Parallel Processing (MPP), maar ze hebben enkele verschillen vanwege het onderliggende platform. SQL Data Warehouse is een Platform als Service (PaaS) die wordt uitgevoerd op Azure. Parallel datawarehouse op Analytics Platform System (APS) die een toestel op de ruimten die wordt uitgevoerd op Windows Server is uitgevoerd.

## <a name="what-is-distributed-data"></a>Wat is een gedistribueerde gegevens?

SQL Data Warehouse en Parallel datawarehouse verwijst gedistribueerde gegevens naar gegevens die op meerdere locaties is opgeslagen in de MPP-systeem van de gebruiker. Elk van deze locaties fungeert als een onafhankelijke opslag en processing unit die query's op het gedeelte van de gegevens wordt uitgevoerd. Gedistribueerde gegevens is cruciaal voor het uitvoeren van query's parallel met het behalen van hoge prestaties.

Om gegevens te distribueren, wijst het datawarehouse elke rij van een gebruikerstabel aan één gedistribueerde locatie.  U kunt tabellen met een hash-verdeling-methode of een methode round robin verspreiden. De distributiemethode wordt opgegeven in de instructie CREATE TABLE. 

## <a name="hash-distributed-tables"></a>Distributed hash tabellen
  
In het volgende diagram ziet u hoe een volledige (tabel niet verdeeld) wordt opgeslagen als een distributed hash tabel. Een deterministische functie wijst elke rij te behoren tot een verdeling. In de definitie van de tabel is een van de kolommen aangewezen als de kolom distributie. De hash-functie gebruikt de waarde in de kolom distributie elke rij toewijzen aan een verdeling.

Zijn prestaties voor de selectie van een kolom verdeling, zoals onderscheidbaarheid, scheeftrekken van gegevens en de typen query's uitvoeren op het systeem.
  
![Distributed tabel] (media/sql-data-warehouse-distributed-data/hash-distributed-table.png "Distributed tabel")  
  
-   Elke rij behoort tot een verdeling.  
  
-   Elke rij een deterministische hash-algoritme toegewezen aan een distributie.  
  
-   Het aantal rijen per distributie verschilt zoals aangegeven door de verschillende maten van tabellen.

## <a name="round-robin-distributed-tables"></a>Round robin gedistribueerde tabellen

Een gedistribueerde tabel round robin verdeelt de rijen door elke rij toewijzen aan een verdeling in de juiste volgorde. Het snel laden van gegevens in een tabel round robin is, maar de prestaties van query's mogelijk langzamer.  Lid worden van een round robin-tabel meestal vereist de rijen als u de query voor de productie van een nauwkeurig resultaat duurt reshuffling.

## <a name="distributed-storage-locations-are-called-distributions"></a>Gedistribueerde locaties worden verdelingen genoemd

Elke gedistribueerde locatie heet een verdeling. Wanneer een query wordt uitgevoerd in parallel, uitvoert elke distributie een SQL-query op een gedeelte van de gegevens. SQL-Database SQL Data Warehouse gebruikt de query uit te voeren. Parallel Data Warehouse gebruikt SQL Server de query uit te voeren. Dit gedeelde architectuurontwerp is cruciaal voor het bereiken van parallel computing schalen.

### <a name="can-i-view-the-distributions"></a>Kan ik de verdelingen weergeven?

Elke distributie heeft een distributie-ID en is zichtbaar in systeemweergaven die betrekking op het SQL Data Warehouse en Parallel datawarehouse hebben. De distributie-ID kunt u problemen met de prestaties van query's en andere problemen. Zie voor een lijst van de systeemweergaven de [MPP systeem weergeven](sql-data-warehouse-reference-tsql-statements.md).

## <a name="difference-between-a-distribution-and-a-compute-node"></a>Verschil tussen een verdeling en een Compute node

Een verdeling is de basiseenheid voor het opslaan van gegevens voor gedistribueerde en parallelle query's worden verwerkt. Distributies zijn gegroepeerd in de Compute nodes. Elk computerknooppunt houdt een of meer verdelingen.  

-   Compute nodes Analytics Platform systeem gebruikt als een centraal onderdeel van de hardware-architectuur en scale-out mogelijkheden. Altijd wordt gebruikt acht verdelingen per Compute node, zoals wordt weergegeven in het diagram voor gedistribueerde hash tabellen. Het nummer van de Compute nodes en dus het aantal verdelingen, wordt bepaald door het nummer van de Compute nodes die u voor het toestel koopt. Als u acht Compute nodes koopt, krijgt u bijvoorbeeld 64 verdelingen (8 knooppunten x 8 verdelingen/computerknooppunt). 

-   SQL Data Warehouse heeft een vast aantal verdelingen van 60 en een variabel aantal Compute nodes. De Compute nodes worden geïmplementeerd met Azure bronnen van computer- en opslagfaciliteiten. Het nummer van de Compute nodes kan veranderen op basis van de werkbelasting backend-service en de computercapaciteit (DWUs), u voor het datawarehouse. Wanneer het nummer van de Compute nodes worden gewijzigd, wordt het aantal verdelingen per Compute node ook gewijzigd. 

### <a name="can-i-view-the-compute-nodes"></a>Kan ik de Compute nodes weergeven?

Elke Compute node heeft een knooppunt-ID en is zichtbaar in systeemweergaven die betrekking op het SQL Data Warehouse en Parallel datawarehouse hebben.  U ziet de Compute node voor de kolom node_id in systeemweergaven waarvan de naam met sys.pdw_nodes begint. Zie voor een lijst van de systeemweergaven de [MPP systeem weergeven](sql-data-warehouse-reference-tsql-statements.md).

## <a name="Replicated"></a>Gerepliceerde tabellen voor Parallel datawarehouse 
  
Van toepassing op: Parallel datawarehouse

Parallel datawarehouse biedt gedistribueerde tabellen gebruiken, een optie voor het repliceren van tabellen. Een *gerepliceerde tabel* is een tabel die is opgeslagen in zijn geheel op elk computerknooppunt. De noodzaak om over te brengen van de rijen tussen de Compute nodes voordat u de tabel in een join of een samenvoeging repliceren van een tabel worden verwijderd. Gerepliceerde tabellen zijn alleen haalbaar met kleine tabellen door de extra opslagruimte die nodig is voor het opslaan van de volledige tabel op elk computerknooppunt.  
  
In het volgende diagram ziet u een gerepliceerde tabel die is opgeslagen op elk computerknooppunt. De gerepliceerde tabel wordt opgeslagen over alle schijven die zijn toegewezen aan de Compute node. Deze schijf strategie wordt geïmplementeerd met behulp van SQL Server bestandsgroepen.  
  
![Gerepliceerde tabel] (media/sql-data-warehouse-distributed-data/replicated-table.png "Gerepliceerde tabel") 
  
## <a name="next-steps"></a>Volgende stappen
  
Zie gedistribueerde tabellen als effectief wilt gebruiken, [distribueren tabellen in SQL Data Warehouse](sql-data-warehouse-tables-distribute.md)  
  



  
