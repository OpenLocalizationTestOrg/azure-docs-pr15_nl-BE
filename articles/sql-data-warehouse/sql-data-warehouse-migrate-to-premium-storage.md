<properties
   pageTitle="Uw bestaande Azure SQL Data Warehouse migreren naar premium opslag | Microsoft Azure"
   description="Instructies voor het migreren van een bestaande SQL-datawarehouse premium opslag"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="happynicolle"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/24/2016"
   ms.author="nicw;barbkess;sonyama"/>

# <a name="migration-to-premium-storage-details"></a>Migratie naar Premium Storage Details
SQL Data Warehouse introduceerde onlangs de [Premie opslag voor grotere voorspelbaarheid van de prestaties][].  We gaan nu bestaande Data Warehouses op standaardopslag migreren naar Premium-opslag.  Lees verder voor meer informatie over hoe en wanneer automatische migraties optreden en het migreren van zichzelf als u wilt bepalen wanneer de uitval optreedt.

Als er meer dan een datawarehouse, gebruikt u de onderstaande [Automatische migration planning][] om te bepalen wanneer ook gemigreerd.

## <a name="determine-storage-type"></a>Opslagtype bepalen
Als u een DW voordat u de onderstaande datums gemaakt, u gebruikt standaardopslag.  Elke Data Warehouse op standaardopslag dat automatische migratie is een aankondiging aan de bovenkant van het datawarehouse blade in [Azure Portal][] met de tekst '*een toekomstige upgrade naar premium opslag vereist een stroomstoring.  Lees meer ->*. "

| **Regio**          | **DW gemaakt vóór deze datum**   |
| :------------------ | :-------------------------------- |
| Australië, Oost      | Premium opslag nog niet beschikbaar |
| Zuidoost Australië | 5 augustus 2016                    |
| Brazilië-Zuid        | 5 augustus 2016                    |
| Canada Central      | 25 mei 2016                      |
| Canada Oost         | 26 mei 2016                      |
| Centrale VS          | 26 mei 2016                      |
| East China          | Premium opslag nog niet beschikbaar |
| China-Noord         | Premium opslag nog niet beschikbaar |
| Oost-Azië           | 25 mei 2016                      |
| Oost-Verenigde Staten             | 26 mei 2016                      |
| Oost-US2            | 27 mei 2016                      |
| Centraal India       | 27 mei 2016                      |
| Zuid India         | 26 mei 2016                      |
| West India          | Premium opslag nog niet beschikbaar |
| East Japan          | 5 augustus 2016                    |
| West Japan          | Premium opslag nog niet beschikbaar |
| Centrale Noord-Amerikaanse    | Premium opslag nog niet beschikbaar |
| Noord-Europa        | 5 augustus 2016                    |
| Zuid-centraal-Verenigde Staten    | 27 mei 2016                      |
| Zuidoost-Azië      | 24 mei 2016                      |
| West-Europa         | 25 mei 2016                      |
| West Central US     | 26 augustus 2016                   |
| West-Verenigde Staten             | 26 mei 2016                      |
| West-US2            | 26 augustus 2016                   |

## <a name="automatic-migration-details"></a>Automatische migratie details
Standaard wordt wij uw database voor u gedurende 6 uur en 6 uur in de regio van uw lokale tijd gemigreerd tijdens de [migratie van automatische planning][] onder.  Uw bestaande datawarehouse kunnen niet worden gebruikt tijdens de migratie.  We verwachten dat de migratie het ongeveer een uur per TB aan opslagcapaciteit per Data Warehouse duurt.  We zullen er ook voor zorgen dat u niet in enig gedeelte van de automatische migratie afgeschreven.

> [AZURE.NOTE] Niet mogelijk om uw bestaande datawarehouse gebruiken tijdens de migratie.  Nadat de migratie voltooid is, wordt uw Data Warehouse weer on line zijn.

De details hieronder zijn de stappen dat Microsoft namens u neemt om de migratie te voltooien en de betrokkenheid van uw kant is niet vereist.  In dit voorbeeld stelt u zich dat uw bestaande DW op standaardopslag is momenteel met de naam 'MyDW'.

1.  Microsoft wijzigt de naam van 'MyDW' naar "MyDW_DO_NOT_USE_ [Timestamp]"
2.  Microsoft wordt onderbroken "MyDW_DO_NOT_USE_ [Timestamp]."  Gedurende deze periode wordt wordt een back-up genomen.  Ziet u meerdere onderbreking wordt hervat als wij problemen tijdens dit proces tegenkomt.
3.  Microsoft maakt een nieuwe DW met de naam 'MyDW' op de premie opslag van de back-up in stap 2.  "MyDW" verschijnt pas nadat het terugzetten voltooid is.
4.  Zodra het terugzetten voltooid is, 'MyDW' geeft als resultaat de dezelfde DWUs en onderbroken of actieve staat als vóór de migratie.
5.  Nadat de migratie voltooid is, verwijdert Microsoft "MyDW_DO_NOT_USE_ [Timestamp]"
    
> [AZURE.NOTE] Deze instellingen overgenomen niet als onderdeel van de migratie:
> 
>   -  Controle op het niveau van de Database moet opnieuw worden ingeschakeld
>   -  Firewall-regels op het niveau van de **Database** moeten opnieuw worden wordt toegevoegd.  Firewall-regels op het niveau van de **Server** ondervinden geen hinder.

### <a name="automatic-migration-schedule"></a>Automatische migration planning
Automatische migraties optreden van 18: 00 – 06: 00 (lokale tijd per regio) tijdens het volgende schema voor storing.

| **Regio**          | **Geschatte begindatum**     | **Geschatte einddatum**       |
| :------------------ | :--------------------------- | :--------------------------- |
| Australië, Oost      | Nog niet is vastgesteld           | Nog niet is vastgesteld           |
| Zuidoost Australië | 10 augustus 2016              | 24 augustus 2016              |
| Brazilië-Zuid        | 10 augustus 2016              | 24 augustus 2016              |
| Canada Central      | 23 juni 2016                | 1 juli 2016                 |
| Canada Oost         | 23 juni 2016                | 1 juli 2016                 |
| Centrale VS          | 23 juni 2016                | 4 juli 2016                 |
| East China          | Nog niet is vastgesteld           | Nog niet is vastgesteld           |
| China-Noord         | Nog niet is vastgesteld           | Nog niet is vastgesteld           |
| Oost-Azië           | 23 juni 2016                | 1 juli 2016                 |
| Oost-Verenigde Staten             | 23 juni 2016                | 11 juli 2016                |
| Oost-US2            | 23 juni 2016                | 8 juli 2016                 |
| Centraal India       | 23 juni 2016                | 1 juli 2016                 |
| Zuid India         | 23 juni 2016                | 1 juli 2016                 |
| West India          | Nog niet is vastgesteld           | Nog niet is vastgesteld           |
| East Japan          | 10 augustus 2016              | 24 augustus 2016              |
| West Japan          | Nog niet is vastgesteld           | Nog niet is vastgesteld           |
| Centrale Noord-Amerikaanse    | Nog niet is vastgesteld           | Nog niet is vastgesteld           |
| Noord-Europa        | 10 augustus 2016              | 31 augustus 2016              |
| Zuid-centraal-Verenigde Staten    | 23 juni 2016                | 2 juli 2016                 |
| Zuidoost-Azië      | 23 juni 2016                | 1 juli 2016                 |
| West-Europa         | 23 juni 2016                | 8 juli 2016                 |
| West Central US     | 14 augustus 2016              | 31 augustus 2016              |
| West-Verenigde Staten             | 23 juni 2016                | 7 juli 2016                 |
| West-US2            | 14 augustus 2016              | 31 augustus 2016              |

## <a name="self-migration-to-premium-storage"></a>Eigen migratie naar Premium-opslag
Als u bepalen wanneer de uitval plaatsvindt wilt, kunt u de volgende stappen uit om te migreren van een bestaande datawarehouse op standaardopslag naar Premium opslag.  Als u kiest voor het migreren van zichzelf, voltooi eerst de migratie zelf voordat de automatische migratie in die regio begint dat elk gevaar van de automatische migratie een conflict veroorzaken (Zie de [migratie van automatische planning][]).

### <a name="self-migration-instructions"></a>Migratie van zelf-instructies
Als u uw uitvaltijd te bepalen wilt, kunt u zelf uw Data Warehouse migreren met behulp van back-up en terugzetten.  Het gedeelte voor herstel van de migratie is ongeveer één uur per TB aan opslagcapaciteit per DW verwacht.  Als u dezelfde naam behouden wilt nadat de migratie is voltooid, volgt u de stappen voor de [stappen te wijzigen tijdens de migratie][]. 

1.  [Pauze][] uw DW waarmee een automatische back-up
2.  [Herstellen][] van uw meest recente snapshot
3.  Verwijder de bestaande DW op standaardopslag. **Als u deze stap doen mislukken, worden in rekening gebracht voor beide DWs.**

> [AZURE.NOTE] Deze instellingen overgenomen niet als onderdeel van de migratie:
> 
>   -  Controle op het niveau van de Database moet opnieuw worden ingeschakeld
>   -  Firewall-regels op het niveau van de **Database** moeten opnieuw worden wordt toegevoegd.  Firewall-regels op het niveau van de **Server** ondervinden geen hinder.

#### <a name="optional-steps-to-rename-during-migration"></a>Optioneel: stappen wijzigen tijdens de migratie 
Twee databases op dezelfde logische server kunnen niet dezelfde naam hebben. SQL Data Warehouse ondersteunt nu de mogelijkheid om de naam van een DW.

In dit voorbeeld stelt u zich dat uw bestaande DW op standaardopslag is momenteel met de naam 'MyDW'.

1.  Wijzig de naam van 'MyDW' met behulp van de opdracht ALTER DATABASE dat volgt op iets als "MyDW_BeforeMigration".  Met deze opdracht alle bestaande transacties beëindigd en moet worden uitgevoerd in de database master te slagen.
```
ALTER DATABASE CurrentDatabasename MODIFY NAME = NewDatabaseName;
```
2.  [Onderbreken][] "MyDW_BeforeMigration", waarmee een automatische back-up
3.  [Herstellen][] van uw meest recente snapshot-een nieuwe database met de naam die u gebruikt om toegang te hebben (ex: "MyDW")
4.  Verwijder 'MyDW_BeforeMigration'.  **Als u deze stap doen mislukken, worden in rekening gebracht voor beide DWs.**

> [AZURE.NOTE] Deze instellingen overgenomen niet als onderdeel van de migratie:
> 
>   -  Controle op het niveau van de Database moet opnieuw worden ingeschakeld
>   -  Firewall-regels op het niveau van de **Database** moeten opnieuw worden wordt toegevoegd.  Firewall-regels op het niveau van de **Server** ondervinden geen hinder.

## <a name="next-steps"></a>Volgende stappen
Met de wijziging voor de opslag van de premie, zijn we ook het aantal blob-databasebestanden in de onderliggende architectuur van de toegenomen.  Optimaliseer de prestatievoordelen van deze wijziging, wordt aangeraden dat u uw Columnstore gegroepeerde indexen met behulp van het volgende script opnieuw.  Het onderstaande script werkt door de bestaande gegevens naar de extra BLOB's dwingen.  Als u geen actie onderneemt, verspreiden de gegevens natuurlijk na verloop van tijd, als u meer gegevens in uw tabellen datawarehouse laden.

**Minimumvereisten:**

1.  Datawarehouse moet worden uitgevoerd met 1000 DWUs of hoger (Zie [rekenkracht schaal][])
2.  Gebruiker die het script moet worden in de [rol van mediumrc][] of hoger
    1.  Een gebruiker toevoegen aan deze rol, voeren de volgende: 
        1.  ````EXEC sp_addrolemember 'xlargerc', 'MyUser'````

````sql
-------------------------------------------------------------------------------
-- Step 1: Create Table to control Index Rebuild
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------
create table sql_statements
WITH (distribution = round_robin)
as select 
    'alter index all on ' + s.name + '.' + t.NAME + ' rebuild;' as statement,
    row_number() over (order by s.name, t.name) as sequence
from 
    sys.schemas s
    inner join sys.tables t
        on s.schema_id = t.schema_id
where
    is_external = 0
;
go
 
--------------------------------------------------------------------------------
-- Step 2: Execute Index Rebuilds.  If script fails, the below can be rerun to restart where last left off
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------

declare @nbr_statements int = (select count(*) from sql_statements)
declare @i int = 1
while(@i <= @nbr_statements)
begin
      declare @statement nvarchar(1000)= (select statement from sql_statements where sequence = @i)
      print cast(getdate() as nvarchar(1000)) + ' Executing... ' + @statement
      exec (@statement)
      delete from sql_statements where sequence = @i
      set @i += 1
end;
go
-------------------------------------------------------------------------------
-- Step 3: Cleanup Table Created in Step 1
--------------------------------------------------------------------------------
drop table sql_statements;
go
````

Als u problemen met uw Data Warehouse, [maakt u een support ticket][] en verwijzing 'Migratie naar Premium Storage' als mogelijke oorzaak ondervindt.

<!--Image references-->

<!--Article references-->
[Automatische migration planning]: #automatic-migration-schedule
[self-migration to Premium Storage]: #self-migration-to-premium-storage
[Maak een support ticket]: sql-data-warehouse-get-started-create-support-ticket.md
[Azure paired region]: best-practices-availability-paired-regions.md
[main documentation site]: services/sql-data-warehouse.md
[Onderbreken]: sql-data-warehouse-manage-compute-portal.md/#pause-compute
[Herstellen]: sql-data-warehouse-restore-database-portal.md
[stappen wijzigen tijdens de migratie]: #optional-steps-to-rename-during-migration
[schaal rekenkracht]: sql-data-warehouse-manage-compute-portal/#scale-compute-power
[mediumrc rol]: sql-data-warehouse-develop-concurrency/#workload-management

<!--MSDN references-->


<!--Other Web references-->
[Premium-opslagruimte voor grotere voorspelbaarheid van de prestaties]: https://azure.microsoft.com/en-us/blog/azure-sql-data-warehouse-introduces-premium-storage-for-greater-performance/
[Azure Portal]: https://portal.azure.com
