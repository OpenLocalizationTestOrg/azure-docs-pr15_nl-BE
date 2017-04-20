<properties
   pageTitle="De werkbelasting magazijn gegevens"
   description="SQL Data Warehouse elasticiteit kunt u vergroten, verkleinen of rekenkracht onderbreken met behulp van een beweegbare schaal van magazijn eenheden (DWUs). In dit artikel wordt uitgelegd de metrische gegevens magazijn en hoe zij betrekking hebben op DWUs. "
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
   ms.date="07/25/2016"
   ms.author="barbkess;mausher;jrj;sonyama"/>


# <a name="data-warehouse-workload"></a>De werkbelasting magazijn gegevens
Een werkbelasting van het magazijn gegevens verwijst naar alle bewerkingen die tegen een datawarehouse gegevenstoegangsbewerkingen. De werkbelasting van het magazijn gegevens omvat het hele proces van het laden van de gegevens in het magazijn, uitvoeren van analyse en rapportage van het datawarehouse beheren van gegevens in het gegevenswarehouse en exporteren van gegevens uit het datawarehouse. De diepte en breedte van deze onderdelen zijn vaak evenredig met het niveau van de looptijd van het datawarehouse.


## <a name="new-to-data-warehousing"></a>Nieuw bij gegevensopslag?
Een datawarehouse is een verzameling gegevens die uit een of meer gegevensbronnen wordt geladen en wordt gebruikt voor business intelligence taken zoals rapportering en gegevensanalyse.

Datawarehouses worden gekenmerkt door query's waarmee een groter aantal rijen, grote gegevensbereiken scannen en relatief groot mogelijk resultaat ten behoeve van analyse en rapportage. Datawarehouses worden ook gekenmerkt door de relatief grote ladingen versus kleine transactieniveau toevoegingen of updates/verwijderd.

- Een datawarehouse werkt het beste als de gegevens worden opgeslagen op een manier die query's die u nodig hebt om een groot aantal rijen of grote gegevensbereiken te scannen wordt geoptimaliseerd. Dit type scan werkt het beste wanneer de gegevens worden opgeslagen en door kolommen in plaats van rijen doorzocht.

>[AZURE.NOTE] De index columnstore in het geheugen, die kolom opslag gebruikt, biedt maximaal 10 x compressie winsten en 100 x prestatieverhogingen query via traditionele binaire bomen voor query's voor rapportage en analyse. We beschouwen columnstore indexen als de standaard voor het opslaan en het scannen van grote gegevens in een gegevensmagazijn.

- Een datawarehouse heeft andere eisen dan een systeem dat wordt geoptimaliseerd voor online transaction processing (OLTP). De OLTP-systeem heeft veel invoegen, bijwerken en verwijderen. Deze bewerkingen willen van bepaalde rijen in de tabel. Tabel zoekt beste uitgevoerd wanneer de gegevens worden opgeslagen op een wijze die per rij. De gegevens kunnen worden gesorteerd en snel gezocht met een deling en heers benadering een binaire zoekopdracht structuur of btree genoemd.


## <a name="data-loading"></a>Gegevens laden
Laden van gegevens is een groot deel van de werkbelasting van het magazijn gegevens. Bedrijven hebben meestal een bezet OLTP-systeem dat bijgehouden wijzigingen gedurende de dag klanten als zakelijke transacties genereren. Regelmatig, vaak 's nachts tijdens een onderhoudsvenster, worden de transacties verplaatst of gekopieerd naar het datawarehouse. Wanneer de gegevens eenmaal in het warehouse, kunnen de analisten analyses uitvoeren en zakelijke beslissingen van de gegevens.

- Het proces van het laden wordt traditioneel ETL genoemd voor extraheren, transformeren en laden. Gegevens moeten meestal worden getransformeerd zodat deze consistent zijn met andere gegevens in het gegevenswarehouse. Bedrijven vroeger speciale ETL-servers de transformaties uitvoeren. U kunt nu met deze snelle massively parallel processing eerst gegevens in SQL Data Warehouse laden en vervolgens de transformaties uitvoeren. Dit proces heet extraheren, laden en transformeren (ELT) en wordt steeds een nieuwe standaard voor de werkbelasting van het magazijn gegevens.

> [AZURE.NOTE] SQL Server 2016, kunt u nu uitvoeren analytics in real time op de OLTP-tabel. Dit vervangt niet de noodzaak van een datawarehouse opslaan en analyseren van gegevens, maar biedt een manier om in real-time analyse.

### <a name="reporting-and-analysis-queries"></a>Query's voor rapportage en analyse
Query's voor rapportage en analyse worden vaak onderverdeeld in kleine, middelgrote en grote op basis van een aantal criteria, maar is meestal gebaseerd op tijd. In de meeste datawarehouses is er een gemengde werkbelasting van fast-actief versus langdurige query's. In elk geval is het belangrijk deze combinatie bepalen en bepalen van de frequentie (per uur, dagelijks, einde van de maand, einde van het kwartaal, enzovoort). Het is belangrijk te begrijpen dat de werklast van gemengde query, in combinatie met gelijktijdigheid, potentiële klanten naar de juiste capaciteitsplanning voor een datawarehouse.

- Capaciteitsplanning is een complexe taak voor een gemengde query werkbelasting, met name als er een lange levertijd capaciteit toe te voegen aan het gegevenswarehouse. SQL Data Warehouse verwijdert u de urgentie van de capaciteitsplanning, aangezien u kunt vergroten of verkleinen computercapaciteit te allen tijde en sinds opslag en computercapaciteit afzonderlijk worden aangepast.

### <a name="data-management"></a>Gegevensbeheer
Beheer van gegevens is belangrijk, vooral als u weet dat u mogelijk onvoldoende schijfruimte beschikbaar in de nabije toekomst. De gegevens verdeeld datawarehouses meestal in zinvolle bereiken als partities in een tabel zijn opgeslagen. Alle producten op basis van SQL Server kunnen u partities in en uit de tabel verplaatst. Deze partitie kunt overschakelen naar een andere oudere gegevens verplaatsen naar opslag goedkoper en de meest recente gegevens beschikbaar op on line opslag bewaren.

- Columnstore-indexen ondersteunen gepartitioneerde tabellen. Voor columnstore-indexen worden gepartitioneerde tabellen worden gebruikt voor het gegevensbeheer van en het archiveren. Voor tabellen worden opgeslagen rij voor rij hebben partities van een grotere rol in de prestaties van query's.  

- PolyBase speelt een belangrijke rol bij het beheren van gegevens. PolyBase gebruikt, hebt u de mogelijkheid oudere gegevens naar Hadoop of Azure blob-opslag.  Dit biedt veel opties, omdat de gegevens nog steeds on line is.  Het ophalen van gegevens uit Hadoop langer duurt, maar de verhouding van de ophaaltijd kan opwegen tegen de kosten voor opslag.

### <a name="exporting-data"></a>Gegevens exporteren
Een manier om gegevens beschikbaar voor analyse en rapportage is om gegevens te verzenden vanuit het gegevensmagazijn naar servers toegewezen voor het uitvoeren van analyse en rapportage. Deze servers worden data marts genoemd. U kan bijvoorbeeld gegevens vooraf worden verwerkt en vervolgens exporteren naar veel servers over de hele wereld, zodat deze grotendeels beschikbaar voor klanten en analisten van het datawarehouse.

- Voor het genereren van rapporten, elke avond kan u alleen-lezen met een momentopname van de gegevens van de dagelijkse rapportageservers vullen. Dit geeft hogere bandbreedte voor klanten en toch de compute de behoefte aan bronnen in het gegevenswarehouse. Uit een aspect beveiliging kunnen data marts u verminderen het aantal gebruikers die toegang tot het datawarehouse hebben.
- Voor analytics, kunt u een kubus analyse in het datawarehouse bouwen en analyses uitvoeren op het datawarehouse of vooraf gegevens verwerken en exporteren naar de analyseserver voor verdere analytics.

## <a name="next-steps"></a>Volgende stappen
Als u iets weet over SQL Data Warehouse, informatie over het snel [maken van een SQL-datawarehouse][] en [voorbeeldgegevens worden geladen][].

<!--Image references-->

<!--Article references-->
[voorbeeldgegevens worden geladen]: ./sql-data-warehouse-load-sample-databases.md
[maken van een SQL-datawarehouse]: ./sql-data-warehouse-get-started-provision.md

<!--MSDN references-->

<!--Other web references-->
