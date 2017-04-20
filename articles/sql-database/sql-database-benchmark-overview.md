<properties
    pageTitle="Overzicht van de benchmark Azure SQL-Database"
    description="Dit onderwerp beschrijft de Benchmark Azure SQL-Database die wordt gebruikt bij de waardering van de prestaties van Azure SQL-Database."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="06/21/2016"
    ms.author="carlrab" />

# <a name="azure-sql-database-benchmark-overview"></a>Overzicht van de benchmark Azure SQL-Database

## <a name="overview"></a>Overzicht
Microsoft Azure SQL Database biedt drie [niveaus van de service](sql-database-service-tiers.md) met meerdere prestatieniveaus. Elk prestatieniveau biedt een toenemende reeks resources of power, ontworpen voor steeds hogere doorvoer.

Het is belangrijk om te kunnen bepalen hoe de toenemende kracht van elk prestatieniveau worden vertaald in de voor betere databaseprestaties. U doet dit Microsoft heeft ontwikkeld, Azure SQL Database Benchmark (ASDB). De benchmark oefent een mix van eenvoudige bewerkingen gevonden in alle OLTP workloads. We meten de doorvoer voor databases die worden uitgevoerd op elk prestatieniveau behaald.

De middelen en de kracht van elk serviceniveau laag en de prestaties worden uitgedrukt in termen van [Database-transactie-eenheden (DTUs)](sql-database-technical-overview.md#understand-dtus). DTUs bieden een manier om te beschrijven de relatieve capaciteit van een prestatieniveau van op basis van een gemengde eenheid van CPU, geheugen, en lezen en schrijven van de tarieven die door elk prestatieniveau aangeboden. De DTU-rating van een database wordt verdubbeld is gelijk aan de kracht van de database wordt verdubbeld. De benchmark kan wij voor de beoordeling van de impact op de prestaties van de database van de toenemende stroom door elk prestatieniveau aangeboden door uitoefening van het feitelijke database-bewerkingen tijdens het schalen van de grootte van de database, het aantal gebruikers en transactie tarieven in verhouding tot de informatiebronnen die aan de database.

Met behulp van transacties per uur, de standaard service-laag met transacties per minuut en het niveau van Premium service transacties per seconde, eenvoudiger om snel de prestaties van elke servicelaag aan de eisen van een toepassing mogelijk doordat de doorvoer van de basisdienst laag.

## <a name="correlating-benchmark-results-to-real-world-database-performance"></a>Databaseprestaties concrete benchmarkresultaten correleren
Het is belangrijk te begrijpen dat ASDB, net als alle benchmarks, representatief en indicatieve alleen. De transactie die wordt bereikt met de benchmark-toepassing is niet hetzelfde als met andere toepassingen kunnen worden behaald. De benchmark bestaat uit een verzameling van verschillende transactie typen uitvoeren tegen een schema met een bereik van gegevenstypen en tabellen. Terwijl de benchmark elementaire bewerkingen die gemeenschappelijk voor alle OLTP workloads zijn uitoefent, vertegenwoordigt niet een bepaalde klasse van of de toepassing. Het doel van de benchmark is een redelijke handleiding voor de relatieve prestaties van een database die kan worden verwacht voor het omhoog of omlaag te schalen tussen prestaties leveren. Verschillende combinaties van werkbelasting optreden en zal reageren op verschillende manieren in werkelijkheid databases zijn van verschillende grootte en complexiteit. Bijvoorbeeld een i/o-intensieve toepassingen mogelijk druk op i/o-drempels sneller of een CPU-intensieve toepassingen kan CPU-limieten sneller bereikt. Er is geen garantie dat een bepaalde database op dezelfde manier als de benchmark onder een toenemende belasting kan worden uitgebreid.

De gebruikelijke verwerkingswijze en de methodologie worden hieronder in detail beschreven.

## <a name="benchmark-summary"></a>Benchmark-overzicht
ASDB meet de prestaties van een combinatie van de voornaamste databasebewerkingen die vaakst optreden in online transaction processing (OLTP) werkbelasting. Hoewel de benchmark is ontworpen met cloud computing in gedachten, het databaseschema, gegevenspopulatie en transacties zodanig ontworpen zijn dat grotendeels representatief zijn voor het meest worden gebruikt in een OLTP workloads basiselementen.

## <a name="schema"></a>Schema
Het schema is ontworpen om voldoende groot en complex voor de ondersteuning van een breed scala aan activiteiten. De benchmark wordt uitgevoerd op een database bestaat uit zes tabellen. De tabellen worden onderverdeeld in drie categorieën: vaste grootte, schalen en groeiende. Er zijn twee vaste grootte tabellen; drie schalen tabellen; en een groeiende tabel. Vaste grootte tabellen hebben een constant aantal rijen. Schalen tabellen hebben een kardinaliteit die in verhouding staat tot de prestaties van de database, maar niet wijzigen tijdens de benchmark. De groeiende tabelformaat als een tabel schaal op de eerste keer wordt geladen, maar de kardinaliteit wijzigingen tijdens het uitvoeren van de gebruikelijke verwerkingswijze zoals rijen worden ingevoegd en verwijderd.

Het schema bevat een combinatie van gegevens van het type integer, inclusief numeriek teken en datum/tijd. Het schema bevat de primaire en secundaire sleutels, maar niet de refererende sleutels – dat wil zeggen, er zijn geen beperkingen voor referentiële integriteit tussen tabellen.

Een programma gegevens genereren, genereert de gegevens voor de oorspronkelijke database. Integer en numerieke gegevens worden gegenereerd met verschillende strategieën. In sommige gevallen worden waarden willekeurig verdeeld over een bereik. In andere gevallen wordt een reeks waarden wordt willekeurig gepermuteerde ervoor te zorgen dat een bepaalde verdeling. Tekstvelden worden gegenereerd op basis van een gewogen lijst met woorden voor de productie van realistisch ogende gegevens.

De database wordt aangepast op basis van een "schaalfactor." De schaalfactor (afgekort SF) bepaalt de verhouding van de schaal en groeiende tabellen. Zoals hieronder wordt beschreven in de sectie gebruikers en Pacing, de grootte, het aantal gebruikers en maximale prestaties in alle schalen in verhouding tot elkaar.

## <a name="transactions"></a>Transacties
De werklast bestaat uit negen typen, zoals in de onderstaande tabel. Elke transactie is ontworpen voor een bepaalde set kenmerken in de database-engine en systeem hardware, met een hoog contrast uit de andere transacties te markeren. Deze aanpak vereenvoudigt de beoordeling van de gevolgen van de verschillende onderdelen op de algehele prestaties. De transactie 'Zware lezen' levert bijvoorbeeld een groot aantal leesbewerkingen van de schijf.

| Transactietype | Beschrijving |
|---|---|
| Lite lezen | SELECTEER; in het geheugen; alleen-lezen |
| Lezen gemiddeld | SELECTEER; vooral in het geheugen; alleen-lezen |
| Zware lezen | SELECTEER; meestal niet in het geheugen; alleen-lezen |
| Update Lite | UPDATE; in het geheugen; lezen / schrijven |
| Zware bijwerken | UPDATE; meestal niet in het geheugen; lezen / schrijven |
| Lite invoegen | INVOEGEN; in het geheugen; lezen / schrijven |
| Zware invoegen | INVOEGEN; meestal niet in het geheugen; lezen / schrijven |
| Verwijderen | VERWIJDEREN; in het geheugen en niet in het geheugen; lezen / schrijven |
| Zware CPU | SELECTEER; in het geheugen; relatief zware CPU-belasting; alleen-lezen |

## <a name="workload-mix"></a>Werkbelasting mix
Transacties worden willekeurig geselecteerd uit een gewogen verdeling met de volgende algemene mix. De totale combinatie is een lezen/schrijven-verhouding van ongeveer 2:1.

| Transactietype | % van Mix |
|---|---|
| Lite lezen | 35 |
| Lezen gemiddeld | 20 |
| Zware lezen | 5 |
| Update Lite | 20 |
| Zware bijwerken | 3 |
| Lite invoegen | 3 |
| Zware invoegen | 2 |
| Verwijderen | 2 |
| Zware CPU | 10 |

## <a name="users-and-pacing"></a>Gebruikers en pacing
De werkbelasting benchmark is aangestuurd vanuit een hulpprogramma waarmee transacties worden ingediend via een reeks verbindingen met het gedrag van een aantal gelijktijdige gebruikers te simuleren. Hoewel alle verbindingen en transacties computer gegenereerd zijn, om redenen van eenvoud noemen wij deze verbindingen 'gebruikers'. Hoewel elke gebruiker onafhankelijk van alle andere gebruikers werkt, voert u alle gebruikers de dezelfde cyclus van de onderstaande stappen uit:

1. Een databaseverbinding maken.
2. Herhaal totdat gesignaleerd om af te sluiten:
    - Selecteer een transactie in willekeurige volgorde (van een gewogen distributie).
    - De geselecteerde transactie uitvoeren en de reactietijd meten.
    - Wachten op een vertraging van de pacing.
3. Sluit de databaseverbinding.
4. Afsluiten.

De pacing vertraging (in stap 2c) willekeurig is geselecteerd, maar met een distributie die een gemiddelde van 1,0 seconde heeft. Elke gebruiker kan dus gemiddeld maximaal één transactie per seconde genereren.

## <a name="scaling-rules"></a>Regels schalen
Het aantal gebruikers wordt bepaald door de grootte van de database (in eenheden van de schaal factor). Er is een gebruiker voor elke vijf eenheden van de schaal factor. De pacing vertraging één gebruiker gemiddeld maximaal één transactie per seconde, genereren.

Bijvoorbeeld een schaal-factor van 500 (SF = 500) database 100 gebruikers heeft en een maximumsnelheid van 100 TPS kunt bereiken. Snelheid vereist om een hoger TPS station meer gebruikers en een grotere database.

De onderstaande tabel ziet het aantal gebruikers dat daadwerkelijk geleden schade voor elk serviceniveau laag en de prestaties.

| Servicelaag (Performance niveau). | Gebruikers | Grootte van de database |
|---|---|---|
| Basic | 5 | 720 MB |
| Standaard (S0) | 10 | 1 GB |
| Standaard (S1) | 20 | 2,1 GB |
| Standaard (S2) | 50 | 7.1 GB |
| Premium (P1) | 100 | 14 GB |
| Premium (P2) | 200 | 28 GB |
| Premium (P6/P3) | 800 | 114 GB |

## <a name="measurement-duration"></a>Duur van de meting
Een geldige benchmark-run is een meting van de stationaire toestand duur van ten minste één uur vereist.

## <a name="metrics"></a>Statistieken
De sleutel in de benchmark zijn doorvoer en responstijd.

- Doorvoer is de essentiële prestaties te meten in de benchmark. De doorvoer wordt vermeld in de transacties per eenheid-van-time, tellen alle typen.
- Reactietijd is een meting van de voorspelbaarheid van de prestaties. De reactie tijdsbeperking varieert met de klasse van dienst met de hogere klassen van service strengere response tijd nodig hebben, zoals hieronder wordt weergegeven.

| Klasse service  | Doorvoer maatregel | Response tijd nodig |
|---|---|---|
| Premium | Transacties per seconde | 95e percentiel op 0,5 seconden |
| Standaard | Transacties per minuut | 90e percentiel op 1,0 seconden |
| Basic | Transacties per uur | 80e percentiel op 2,0 seconden |

## <a name="conclusion"></a>Conclusie
De Benchmark Azure SQL Database meet de relatieve prestaties van Azure SQL-Database actief binnen het bereik van beschikbare service niveaus en prestaties. De benchmark oefent een mix van de voornaamste databasebewerkingen die vaakst optreden in online transaction processing (OLTP) werkbelasting. Meten van de werkelijke prestaties, biedt de benchmark een meer zinvolle evaluatie van het effect op de doorvoer van het wijzigen van het prestatieniveau van de dan mogelijk is door alleen de resources die door elk niveau zoals CPU-snelheid, grootte van het systeemgeheugen en IOP's bieden. In de toekomst zullen wij de benchmark het toepassingsgebied ervan te verruimen en de verstrekte gegevens om te ontwikkelen.

## <a name="resources"></a>Bronnen
[Inleiding tot SQL-Database](sql-database-technical-overview.md)

[Service niveaus en prestaties](sql-database-service-tiers.md)

[Richtlijnen voor prestaties voor enkele databases](sql-database-performance-guidance.md)
