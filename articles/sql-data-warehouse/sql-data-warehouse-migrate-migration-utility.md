<properties
   pageTitle="Migreren: Gegevens magazijn migratiehulpprogramma | Microsoft Azure"
   description="Migreren naar SQL datawarehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/08/2016"
   ms.author="lodipalm;barbkess;sonyama"/>


# <a name="data-warehouse-migration-utility-preview"></a>Het migratiehulpprogramma magazijn van gegevens (voorbeeld)

> [AZURE.SELECTOR]
- [Migratiehulpprogramma downloaden][]

Gegevens magazijn Migration Utility is een hulpprogramma waarmee het schema en de gegevens van SQL Server en Azure SQL-Database migreren naar Azure SQL Data Warehouse. Tijdens de migratie schema wijst het hulpprogramma automatisch het bijbehorende schema van bron naar de bestemming. Nadat het schema is gemigreerd, biedt de hulpprogramma's de optie voor het verplaatsen van gegevens met behulp van scripts automatisch gegenereerd.

Naast het schema en -gegevensmigratie kunt dit hulpprogramma u de optie voor het genereren van compatibiliteitsrapporten die compatibiliteitsproblemen tussen de doel- en bronvelden exemplaren gestroomlijnde migratie kunnen samenvatten.

## <a name="get-started"></a>Aan de slag
Als een vereiste voor de installatie moet u het opdrachtregelprogramma BCP Office in de lijst compatibiliteit weergeven en migratiescripts uitvoeren. Na het starten van het uitvoerbare bestand dat wordt geladen wordt u gevraagd een standaard overeenkomst accepteren voordat het hulpprogramma wordt geïnstalleerd.

Daarnaast om de Utiliy van de migratie uitvoert, moet u de volgende machtigingen op de database die u zoekt als u wilt migreren: DATABASE maken, een DATABASE wijzigen of een WEERGAVEDEFINITIE.

### <a name="launching-the-tool-and-connecting"></a>Het hulpprogramma starten en verbinding te maken
Het hulpprogramma starten door te klikken op het pictogram op het bureaublad die post-installatie wordt weergegeven. Bij het openen van het hulpprogramma, wordt u gevraagd met een aanvangsverbinding pagina u de bron- en doel van het hulpprogramma voor migratie kunt. Op dit moment is ondersteuning Microsoft voor SQL Server en Azure SQL-Database als bronnen en SQL Data Warehouse als bestemming. Nadat u dit hebt u gevraagd verbinding met de bronserver te vullen in de naam van de server en verifieert en vervolgens te klikken op 'Connect'.

Na verificatie, wordt het hulpprogramma een lijst met databases die aanwezig zijn op de server waarmee u met verbonden bent weergegeven. De migratie kunt u beginnen met een database die u wilt migreren selecteren en vervolgens te klikken op 'Migreren is ingeschakeld'.

## <a name="migration-report"></a>Migratierapport
'Databasecompatibiliteit controleren' selecteert in het hulpprogramma genereert een rapport met overzicht van alle objectincompatibiliteiten in de database dat u wilt overzetten. Een uitgebreidere lijst met enkele van de functionaliteit van SQL Server die is niet aanwezig in SQL Data Warehouse vindt u in onze [documentatie voor migratie][]. Nadat het rapport wordt gegenereerd is u mogelijk zijn op te slaan en open het rapport in Excel.

Houd er rekening mee dat bij het genereren van het schema van de migratie, de meeste problemen die worden beschreven als 'Object' wordt aangepast om onmiddellijke migratie van gegevens mogelijk te maken. Bekijk de wijzigingen zodat u niet meer om aanpassingen te maken voordat u het schema wilt.

## <a name="migrate-schema"></a>Schema migreren

Nadat u hebt aangesloten, 'Schema migreren' te selecteren wordt een schema migratie script genereren voor de geselecteerde tabellen. Deze havens script de structuur van de tabel, kaarten, incompatibele gegevens typen om meer compatibele formulieren en beveiligingsreferenties en schema gemaakt als dit wordt aangegeven door de gebruiker in de migratie-instellingen. Deze code ten opzichte van de gerichte datawarehouse SQL-exemplaar kan worden uitgevoerd, opgeslagen in een bestand, naar het Klembord gekopieerd of bewerkt zelfs in lijn voordat u verdere actie te ondernemen.  

Zoals aangegeven hierboven, als migratie schema controleren de migratie gewijzigd die is doorgevoerd om te waarborgen dat die u volledig begrijpen.  

## <a name="migrate-data"></a>Gegevens migreren

Door te klikken op de optie 'Gegevens migreren' kunt u genereren BCP-scripts die u de gegevens eerst naar bestanden op uw server gaat, en vervolgens rechtstreeks in uw SQL Data Warehouse. Het is raadzaam dit proces voor het verplaatsen van kleine hoeveelheden gegevens, en pogingen zijn niet ingebouwd en kan mislukken als er verlies van de netwerkverbinding. Om te kunnen uitvoeren, moet de BCP hulpprogramma voor de opdrachtregel geïnstalleerd en het schema voor de gegevens die al is gemaakt.

Nadat u de parameters hierboven hebt ingevuld hoeft u slechts te klikken op de migratie uitvoeren en een set van twee pakketten naar de opgegeven locatie wordt gegenereerd. Voert u het bestand exporteren om gegevens te exporteren uit de migratiebron naar platte bestanden en voert u het bestand importeren voor het importeren van gegevens in SQL Data Warehouse.

## <a name="next-steps"></a>Volgende stappen
Nu dat u alle gegevens hebt gemigreerd, kunt u controleren hoe te [ontwikkelen][].

<!--Image references-->

<!--Article references-->
[migratie van documentatie]: sql-data-warehouse-overview-migrate.md
[ontwikkelen]: sql-data-warehouse-overview-develop.md

<!--Other Web references--> 
[Migratiehulpprogramma downloaden]: https://migrhoststorage.blob.core.windows.net/sqldwsample/DataWarehouseMigrationUtility.zip
