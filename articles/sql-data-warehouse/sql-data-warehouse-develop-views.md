<properties
   pageTitle="Weergaven in SQL datawarehouse | Microsoft Azure"
   description="Tips voor het gebruik van de Transact-SQL-weergaven in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
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
   ms.date="07/01/2016"
   ms.author="jrj;barbkess;sonyama"/>


# <a name="views-in-sql-data-warehouse"></a>Weergaven in SQL datawarehouse

Weergaven zijn vooral handig in SQL Data Warehouse. Ze kunnen worden gebruikt in een aantal verschillende manieren ter verbetering van de kwaliteit van uw oplossing.  In dit artikel worden enkele voorbeelden van hoe u uw oplossing met weergaven, alsmede de beperkingen die moeten worden beschouwd als verrijking gemarkeerd.

> [AZURE.NOTE] Syntaxis voor `CREATE VIEW` niet in dit artikel wordt beschreven. Raadpleeg het artikel [CREATE VIEW][] op MSDN voor deze informatie.

## <a name="architectural-abstraction"></a>Architectuur abstractie
Een veelgebruikte toepassing patroon is opnieuw maken tabel als selecteren (CTAS) gevolgd door een object patroon wijzigen terwijl de gegevens worden geladen met tabellen te maken.

In het volgende voorbeeld wordt nieuwe datum records toegevoegd aan de datumdimensie van een. U ziet hoe een nieuwe tabble, DimDate_New, is het eerst wordt gemaakt en vervolgens gewijzigd in de oorspronkelijke versie van de tabel vervangt.

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

Deze benadering kan echter resulteren in tabellen verschijnen en verdwijnen uit de weergave van de gebruiker zoals foutberichten 'tabel niet bestaat, wordt. Weergaven kunnen worden gebruikt om gebruikers te bieden met een consistente presentatie laag terwijl de onderliggende objecten worden gewijzigd. Door gebruikers toegang verlenen tot de gegevens via een weergaven, betekent dat gebruikers hoeven niet op de zichtbaarheid van de onderliggende tabellen hebben. Dit biedt een consistente gebruikerservaring, terwijl ervoor te zorgen dat de ontwerpers van het datawarehouse kan evolueren van het gegevensmodel en prestaties te maximaliseren met behulp van CTAS tijdens het laadproces.    

## <a name="performance-optimization"></a>Optimalisatie van prestaties
Weergaven kunnen ook worden gebruikt om de prestaties geoptimaliseerd joins tussen tabellen afdwingen. Een weergave kan bijvoorbeeld een sleutel redundante distributie opnemen als onderdeel van de gekoppelde criteria verplaatsing van gegevens minimaliseren.  Een ander voordeel van een weergave kan worden om een specifieke query of de geheugensteun voor het gekoppelde. Met behulp van weergaven op deze manier garandeert dat joins altijd op optimale wijze voorkomen van gebruikers te onthouden de juiste constructie voor de joins worden uitgevoerd.

## <a name="limitations"></a>Beperkingen
Weergaven in SQL Data Warehouse worden alleen metagegevens.  Bijgevolg zijn de volgende opties niet beschikbaar:

-   Er is geen optie schema binding
-   Tabellen kunnen niet worden bijgewerkt via de weergave
-   Weergaven kunnen niet worden gemaakt via tijdelijke tabellen
-   Er is geen ondersteuning voor UITVOUWEN / NOEXPAND tips
-   Er zijn geen geïndexeerde weergaven in SQL Data Warehouse


## <a name="next-steps"></a>Volgende stappen
Zie [SQL Data Warehouse ontwikkelen-overzicht][]voor meer tips voor ontwikkeling.
Voor `CREATE VIEW` syntaxis verwijzen wij u naar de [Weergave maken][].

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse ontwikkelen-overzicht]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[WEERGAVE MAKEN]: https://msdn.microsoft.com/en-us/library/ms187956.aspx

<!--Other Web references-->
