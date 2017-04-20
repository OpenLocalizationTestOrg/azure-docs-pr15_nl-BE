<properties
   pageTitle="Door de gebruiker gedefinieerde schema's in SQL Data Warehouse | Microsoft Azure"
   description="Tips voor het gebruik van de Transact-SQL-schema's in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="user-defined-schemas-in-sql-data-warehouse"></a>Door de gebruiker gedefinieerde schema's in SQL Data Warehouse

Traditionele datawarehouses gebruiken vaak een afzonderlijke database maken op basis van werkbelasting, domein of beveiliging toepassingsgrenzen. Een traditionele SQL Server-datawarehouse kan bijvoorbeeld een staging-database, een data warehouse-database en sommige databases data mart. In deze topologie wordt elke database opereert als een belasting en een beveiligingsgrens in de architectuur.

Daarentegen wordt de werkbelasting van het gehele magazijn binnen één database uitgevoerd door SQL Data Warehouse. Joins zijn niet toegestaan cross-database. SQL Data Warehouse verwacht daarom alle tabellen die worden gebruikt door het magazijn kunnen worden opgeslagen in een database.

> [AZURE.NOTE] Cross databasequery's enige biedt geen ondersteuning voor SQL Data Warehouse. Gegevens magazijn implementaties die gebruikmaken van dit patroon moet bijgevolg worden herzien.

## <a name="recommendations"></a>Aanbevelingen

Dit zijn aanbevelingen voor het consolideren van werkbelasting, beveiliging, domein en functionele grenzen met behulp van door de gebruiker gedefinieerd schema

1. Gebruik van één SQL Data Warehouse-database voor het uitvoeren van de werkbelasting van het gehele magazijn
2. Uw bestaande gegevensarchieven voor het gebruik van één SQL Data Warehouse-database consolideren
3. Maak gebruik van **door de gebruiker gedefinieerde schema's** waarmee de randen van de eerder geïmplementeerd met behulp van databases.

Als door de gebruiker gedefinieerde schema's hebben niet eerder is gebruikt, hebt u een schone lei. Gewoon de naam van de oude database gebruiken als basis voor uw aangepaste schema's in de SQL Data Warehouse-database.

Als u schema's al gebruikt hebt u een aantal opties:

1. De schemanamen van oudere verwijderen en opnieuw beginnen
2. Behouden de oude schema door vóór de naam van het oude schema aan de naam van de tabel
3. Behouden de oude schema door het implementeren van weergaven op de tabel in een extra schema om de structuur van het oude schema opnieuw te maken.

> [AZURE.NOTE] Op de eerste inspectie lijkt optie 3 misschien de meest aantrekkelijke optie. De duivel is echter in de details. Weergaven worden in SQL Data Warehouse alleen gelezen. Elke wijziging van de gegevens of de tabel moet worden uitgevoerd met de basistabel. Optie 3 introduceert ook een laag van weergaven in uw systeem. Mogelijk wilt u dit even nadenken extra als u weergaven in uw architectuur al.


### <a name="examples"></a>Voorbeelden:

Door de gebruiker gedefinieerde schema op basis van de databasenamen implementeren

```sql
CREATE SCHEMA [stg]; -- stg previously database name for staging database
GO
CREATE SCHEMA [edw]; -- edw previously database name for the data warehouse
GO
CREATE TABLE [stg].[customer] -- create staging tables in the stg schema
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[customer] -- create data warehouse tables in the edw schema
(       CustKey BIGINT NOT NULL
,       ...
);
```

Oude schemanamen door vóór deze handhaven op de tabelnaam. Schema's gebruiken voor de rand van de werkbelasting.

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- edw defines the data warehouse boundary
GO
CREATE TABLE [stg].[dim_customer] --pre-pend the old schema name to the table and create in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[dim_customer] --pre-pend the old schema name to the table and create in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
);
```

Oude schema behouden met behulp van weergaven

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- stg defines the data warehouse boundary
GO
CREATE SCHEMA [dim]; -- edw defines the legacy schema name boundary
GO
CREATE TABLE [stg].[customer] -- create the base staging tables in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
)
GO
CREATE TABLE [edw].[customer] -- create the base data warehouse tables in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
)
GO
CREATE VIEW [dim].[customer] -- create a view in the legacy schema name boundary for presentation consistency purposes only
AS
SELECT  CustKey
,       ...
FROM    [edw].customer
;
```

> [AZURE.NOTE] Eventuele wijzigingen in de schema-strategie moet een beoordeling van het beveiligingsmodel voor de database. In veel gevallen kunt u mogelijk het beveiligingsmodel vereenvoudigen door het toewijzen van machtigingen op het schemaniveau van het. Als u meer gedetailleerde machtigingen nodig kunt u databaserollen.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkeling, [ontwikkelen-overzicht][].

<!--Image references-->

<!--Article references-->
[ontwikkelen-overzicht]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
