<properties
   pageTitle="Beheren van rekenkracht in Azure SQL Data Warehouse (overzicht) | Microsoft Azure"
   description="Schaal van de prestaties van mogelijkheden in Azure SQL Data Warehouse. Geschaald uit door DWUs aan te passen of onderbreken en hervatten compute resources om de kosten besparen."
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
   ms.date="09/03/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-overview"></a>Beheren van rekenkracht in Azure SQL Data Warehouse (overzicht)

> [AZURE.SELECTOR]
- [Overzicht](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [REST](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)

De architectuur van SQL Data Warehouse gescheiden opslag en het berekenen, zodat elk afzonderlijk geschaald. Hierdoor kunt u de schaal prestaties bij het opslaan van kosten alleen betaalt voor de prestaties wanneer u deze nodig hebt. 

Dit overzicht beschrijft de volgende mogelijkheden van de geschaalde prestaties van SQL Data Warehouse en geeft aanbevelingen over hoe en wanneer u ze gebruikt. 

- Schaal rekenkracht door aan te passen [datawarehouse eenheden (DWUs)][]
- Compute-bronnen onderbreken of hervatten

<a name="scale-performance-bk"></a>

## <a name="scale-performance"></a>Schaal prestaties

In SQL Data Warehouse, kunt u snel prestaties uit of terug te verhogen of verlagen compute middelen van CPU, geheugen en i/o-bandbreedte te schalen. Als u wilt schalen prestaties, is hoeft u het nummer van het [datawarehouse eenheden (DWUs)][] waarmee SQL Data Warehouse aan uw database toegewezen aanpassen. SQL Data Warehouse snel de wijziging en alle onderliggende wijzigingen in de hardware of software verwerkt.

De dagen zijn voorbij waar moet u onderzoeken welk type processors, hoeveel geheugen of wat voor soort opslag moet zijn geweldige prestaties in uw gegevensmagazijn. Door uw magazijn gegevens opslaan in de cloud, hoeft u niet langer low-level hardware problemen oplossen. In plaats daarvan SQL Data Warehouse wordt u gevraagd deze vraag: hoe snel wilt u dat uw gegevens te analyseren? 

### <a name="how-do-i-scale-performance"></a>Hoe ik prestaties schalen?

Elastically vergroten of verkleinen van de rekenkracht, wijzigt de instelling [datawarehouse eenheden (DWUs)][] voor de database. Prestaties wordt lineair verhoogd als u meer DWU toevoegt.  Op een hoger niveau van DWU moet u het toevoegen van meer dan 100 DWUs om een aanzienlijke verbetering van de prestaties ziet. Om u te helpen selecteert u zinvolle sprongen in DWUs, bieden wij de niveaus van DWU die de beste resultaten krijgt.
 
DWUs aanpassen, kunt u een van deze afzonderlijke methoden.

- [Schaal rekenkracht met Azure portal][]
- [Schaal rekenkracht met PowerShell][]
- [Schaal van rekenkracht met REST API 's][]
- [Schaal rekenkracht met TSQL][]

### <a name="how-many-dwus-should-i-use"></a>Hoeveel DWUs moet ik gebruiken?
 
Prestaties in SQL Data Warehouse lineair wordt geschaald en wijzigen van een compute-schaal (zeg van 100 DWUs naar DWUs 2000) gebeurt in seconden. Dit biedt u de flexibiliteit om te experimenteren met verschillende DWU instellingen totdat u hebt vastgesteld dat uw scenario's passend.

Om te begrijpen wat uw ideale DWU waarde is, probeer het omhoog en omlaag schalen en enkele query's uitvoeren na het laden van uw gegevens. Aangezien schalen snel is, kunt u proberen een aantal verschillende niveaus van prestaties in een uur of minder. Houd er rekening mee dat SQL Data Warehouse is ontworpen voor het verwerken van grote hoeveelheden gegevens en voor een overzicht van de werkelijke capaciteit voor schalen, met name op de grotere schalen we bieden, u zult willen gebruiken een grote gegevensset die bijna 1 TB.

Aanbevelingen voor het vinden van de beste DWU voor uw werkbelasting:

1. Begin door een klein aantal DWUs voor een datawarehouse in ontwikkeling.  Een goed uitgangspunt is DW400 of DW200.
2. Controleren van de toepassingsprestaties van uw, inachtneming van dat het aantal DWUs geselecteerd vergeleken met de prestaties die u hebt waargenomen.
3. Bepalen hoeveel sneller of langzamer prestaties moet u het niveau van optimale prestaties voor uw vereisten te bereiken door het aangaan van lineaire schaal.
4. Verhoog of verlaag het aantal DWUs in verhouding tot hoe veel sneller of langzamer wilt u uw werkbelasting uit te voeren. De service wordt snel reageren en pas de compute middelen om te voldoen aan de eisen van de nieuwe DWU.
5. Verder correcties totdat het niveau van een optimale prestaties voor uw zakelijke behoeften.

### <a name="when-should-i-scale-dwus"></a>Wanneer moet ik de schaal van DWUs?

Wanneer u een snellere resultaten nodig, uw DWUs te verhogen en betalen voor betere prestaties.  Wanneer u moet minder rekenkracht, uw DWUs verlagen en betaalt alleen voor wat u nodig hebt. 

Aanbevelingen voor het schalen van DWUs:

1. Als uw toepassing een wisselende werkdruk, schaal DWU niveaus omhoog of omlaag naar alle pieken en lage punten. Bijvoorbeeld als uw werkbelasting waar gewoonlijk de pieken aan het einde van de maand, van plan bent meer DWUs toevoegen tijdens die Piekdagen en vervolgens verkleinen nadat de piekperiode afgelopen is.
2. Voordat u een zware gegevens laden of transformatie-bewerking uitvoeren, schaalbaar en DWUs zodat de gegevens sneller beschikbaar is.

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Compute onderbreken

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Onderbreken van een database, een van deze afzonderlijke methoden te gebruiken.

- [Compute onderbreken met Azure portal][]
- [Compute onderbreken met PowerShell][]
- [Compute onderbreken met REST API 's][]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Cv berekenen

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Als u wilt doorgaan met een database, een van deze afzonderlijke methoden te gebruiken.

- [Cv compute met Azure portal][]
- [Cv compute met PowerShell][]
- [Cv compute met REST API 's][]

## <a name="permissions"></a>Machtigingen

Schaal van de database moet de machtigingen die worden beschreven in de [DATABASE wijzigen][].  Onderbreken en hervatten moet de machtiging [SQL DB Inzender][] specifiek Microsoft.Sql/servers/databases/action.

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Volgende stappen
Raadpleeg de volgende artikelen kunt u bepaalde aanvullende prestatie basisbegrippen:

- [Werkbelasting en gelijktijdigheid management][]
- [Tabel ontwerp-overzicht][]
- [Verdeling van de tabel][]
- [Tabel indexeren][]
- [Partitioneren van tabel][]
- [Tabelstatistieken][]
- [Aanbevolen procedures][]

<!--Image reference-->

<!--Article references-->
[magazijn eenheden (DWUs)]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

[Schaal rekenkracht met Azure portal]: ./sql-data-warehouse-manage-compute-portal.md#scale-compute-bk
[Schaal rekenkracht met PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#scale-compute-bk
[Schaal van rekenkracht met REST API 's]: ./sql-data-warehouse-manage-compute-rest-api.md#scale-compute-bk
[Schaal rekenkracht met TSQL]: ./sql-data-warehouse-manage-compute-tsql.md#scale-compute-bk

[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md

[Compute onderbreken met Azure portal]:  ./sql-data-warehouse-manage-compute-portal.md#pause-compute-bk
[Compute onderbreken met PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#pause-compute-bk
[Compute onderbreken met REST API 's]: ./sql-data-warehouse-manage-compute-rest-api.md#pause-compute-bk

[Cv compute met Azure portal]:  ./sql-data-warehouse-manage-compute-portal.md#resume-compute-bk
[Cv compute met PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#resume-compute-bk
[Cv compute met REST API 's]: ./sql-data-warehouse-manage-compute-rest-api.md#resume-compute-bk

[Werkbelasting en gelijktijdigheid management]: ./sql-data-warehouse-develop-concurrency.md
[Tabel ontwerp-overzicht]: ./sql-data-warehouse-tables-overview.md
[Verdeling van de tabel]: ./sql-data-warehouse-tables-distribute.md
[Tabel indexeren]: ./sql-data-warehouse-tables-index.md
[Partitioneren van tabel]: ./sql-data-warehouse-tables-partition.md
[Tabelstatistieken]: ./sql-data-warehouse-tables-statistics.md
[Aanbevolen procedures]: ./sql-data-warehouse-best-practices.md 
[development overview]: ./sql-data-warehouse-overview-develop.md

[SQL DB Inzender]: ../active-directory/role-based-access-built-in-roles.md#sql-db-contributor

<!--MSDN references-->
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Azure portal]: http://portal.azure.com/
