<properties
   pageTitle="Uw gegevens migreren naar het datawarehouse SQL | Microsoft Azure"
   description="Tips voor uw gegevens migreren naar het datawarehouse van Azure SQL voor het ontwikkelen van oplossingen."
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
   ms.date="08/25/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="migrate-your-data"></a>Uw gegevens migreren
Gegevens kunnen uit verschillende bronnen in uw SQL-datawarehouse met een verschillende tools worden verplaatst.  Kopiëren met automatische Documentinvoer, SSIS en bcp kunnen alles worden gebruikt om deze doelstelling te bereiken. Echter, als het bedrag van de verhogingen van de gegevens moet u bedenken splitsen het migratieproces in stappen. Dit biedt u elke stap voor de prestaties en veerkracht om ervoor te zorgen een soepele gegevensmigratie optimaliseren.

Dit artikel wordt eerst de eenvoudige migratiescenario's kopiëren met automatische Documentinvoer, SSIS en bcp. Deze Zoek iets dieper in de manier waarop de migratie kan worden geoptimaliseerd.

## <a name="azure-data-factory-adf-copy"></a>Azure Data Factory (ADF) kopiëren
[ADF kopie][] is onderdeel van [Azure Data Factory][]. Kopiëren met automatische Documentinvoer kunt u uw gegevens exporteren naar bestanden die zich bevinden op de lokale opslag naar externe bestanden in Azure blob-opslag of rechtstreeks in SQL Data Warehouse.

Als uw gegevens wordt gestart in de platte bestanden, dan u eerst moet het overbrengen naar opslag Azure blob voordat u begint een belasting in SQL Data Warehouse. Zodra de gegevens worden overgedragen in Azure blob-opslag kunt u [Kopiëren met automatische Documentinvoer][] opnieuw gebruiken om de gegevens in SQL Data Warehouse.

PolyBase biedt ook een krachtige optie voor het laden van de gegevens. Dat betekent dat twee hulpprogramma's gebruiken in plaats van één. Als u de beste prestaties moet PolyBase gebruiken. Als u wilt dat een enkel gereedschap-ervaring (en de gegevens niet enorme) is uw antwoord met ADF.

> [AZURE.NOTE] PolyBase moet uw gegevensbestanden in UTF-8. Dit is de automatische Documentinvoer kopie van standaardcodering zodat u niets te wijzigen. Dit is de herinnering het standaardgedrag van de ADF kopie niet wijzigen.

HEAD over naar het volgende artikel voor enkele geweldige [ADF monsters][].

## <a name="integration-services"></a>Integratieservices ##
Integration Services (SSIS) is een krachtige en flexibele uitpakken transformeren en laden (ETL) hulpprogramma dat complexe workflows, gegevenstransformatie en verschillende opties voor het laden van gegevens ondersteunt. SSI's gebruiken om gegevens over te dragen naar Azure of als onderdeel van een bredere migratie.

> [AZURE.NOTE] SSIS kunt exporteren naar UTF-8 zonder de byte order mark in het bestand. Wilt configureren dit moet u eerst de tekengegevens in de gegevensstroom gebruik codetabel UTF-8 65001 converteren met de afgeleide kolom onderdeel. Als de kolommen zijn geconverteerd, kunt u de gegevens schrijven naar de platte bestand bestemming adapter ervoor te zorgen dat 65001 ook als de codetabel voor het bestand is geselecteerd.

SSIS maakt verbinding met SQL Data Warehouse, net zoals een SQL Server-implementatie wil maken. Uw verbindingen moet echter een ADO.NET-Verbindingsbeheer gebruiken. U moet ook zorgen voor het configureren van de "Gebruik massaal invoegen wanneer beschikbaar" instelling doorvoer maximaliseren. Raadpleeg het artikel [ADO.NET bestemming adapter][] voor meer informatie over deze eigenschap

> [AZURE.NOTE] Verbinding maken met Azure SQL Data Warehouse met behulp van OLEDB wordt niet ondersteund.

Er is bovendien altijd de mogelijkheid dat een pakket vanwege problemen beperken of het netwerk mislukken. Ontwerp zodat ze kunnen worden hervat op het moment van storing, zonder pakketten opnieuw uitvoeren, werken die voltooid vóór de storing.

Raadpleeg de [documentatie van SSI's][]voor meer informatie.

## <a name="bcp"></a>BCP
BCP is een hulpprogramma voor de opdrachtregel die speciaal is ontworpen voor platte bestanden gegevens importeren en exporteren. Een transformatie kan plaatsvinden tijdens het exporteren van gegevens. Eenvoudige transformaties gebruik voor het uitvoeren van een query te selecteren en de gegevens transformeren. Wanneer geëxporteerd, kunnen platte bestanden vervolgens worden geladen rechtstreeks in het doel de SQL Data Warehouse-database.

> [AZURE.NOTE] Het is vaak een goed idee voor het inkapselen van de transformaties die is gebruikt tijdens het exporteren van gegevens in een weergave op het bronsysteem. Dit zorgt ervoor dat de logica wordt behouden en het proces herhaald wordt.

Voordelen van een bcp zijn:

- Eenvoud. BCP-opdrachten zijn eenvoudig te bouwen en uitvoeren
- Laadproces opnieuw meer gestart. Eenmaal geëxporteerde de belasting kan worden een aantal keren uitgevoerd

Beperkingen van het bcp zijn:

- BCP werkt met alleen gebruikmaking platte bestanden. Werkt niet met bestanden zoals xml of JSON
- BCP biedt geen ondersteuning voor het exporteren naar UTF-8. Dit kan voorkomen op bcp geëxporteerd gegevens met behulp van PolyBase
- Data transformation mogelijkheden zijn beperkt tot alleen de fase van de uitvoer en eenvoudig van aard zijn
- BCP is niet aangepast aan de robuuste worden bij het laden van gegevens via het internet. Netwerk instabiliteit kan veroorzaken een fout bij het laden.
- BCP is afhankelijk van het schema aanwezig in de doeldatabase voordat de belasting

Zie [bcp laden van gegevens in SQL Data Warehouse gebruiken][]voor meer informatie.

## <a name="optimizing-data-migration"></a>Gegevensmigratie optimaliseren
Een migratieproces SQLDW kan effectief worden onderverdeeld in drie afzonderlijke stappen:

1. Export van gegevens
2. Overdracht van gegevens naar Azure
3. In de doeldatabase SQLDW laden

Elke stap kan afzonderlijk worden geoptimaliseerd voor het maken van een robuuste, opnieuw meer gestart en robuuste migratieproces dat maximale prestaties bij elke stap.

## <a name="optimizing-data-load"></a>Optimaliseren gegevens laden
Kijken naar deze in omgekeerde volgorde voor even; de snelste manier om gegevens te laden is via PolyBase. Optimaliseren voor een proces van PolyBase laden plaatst vereisten op de voorgaande stappen is het verstandig om te begrijpen dat dit vooraf. Deze zijn:

1. Codering van gegevensbestanden
2. Indeling van de gegevensbestanden
3. Locatie van gegevensbestanden

### <a name="encoding"></a>Codering
PolyBase moet zijn UTF-8 gecodeerde bestanden. Dit betekent dat wanneer u de gegevens exporteert deze aan deze eis voldoen moet. Als uw gegevens alleen standaard ASCII-tekens (geen extended ASCII) vervolgens deze kaart rechtstreeks de UTF-8-standaard en u hoeft te weten de codering. Echter, als uw gegevens geen speciale tekens bevat, zoals umlauts, accenten of symbolen of uw gegevens niet-Latijnse talen ondersteund wordt hebt u om ervoor te zorgen dat uw geëxporteerde bestanden correct UTF-8 gecodeerd.

> [AZURE.NOTE] BCP biedt geen ondersteuning voor het exporteren van gegevens naar UTF-8. Daarom is de beste optie Integration Services of kopiëren met automatische Documentinvoer gebruiken voor het exporteren van gegevens. Het verdient aanbeveling wijzen op de UTF-8-byte order mark (BOM) is niet vereist in het gegevensbestand.

Alle bestanden die zijn gecodeerd met UTF-16 moet worden opnieuw geschreven ***voorafgaand*** aan de overdracht van gegevens.

### <a name="format-of-data-files"></a>Indeling van de gegevensbestanden
PolyBase bepaalt een vaste rij terminator \n of nieuwe regel. Uw gegevensbestanden moeten voldoen aan deze norm. Er zijn geen beperkingen van een tekenreeks of een kolom afsluitweerstanden.

U moet voor elke kolom in het bestand definiëren als onderdeel van de externe tabel in PolyBase. Zorg ervoor dat alle geëxporteerde kolommen vereist zijn en dat de typen aan de vereiste normen voldoen.

Raadpleeg terug de [migreren uw schema] artikel voor informatie over ondersteunde gegevenstypen.

### <a name="location-of-data-files"></a>Locatie van gegevensbestanden
SQL Data Warehouse gebruikt PolyBase om gegevens te laden uit de Azure blobopslag uitsluitend. Dus zijn de gegevens eerst overgebracht naar een blob-opslag.

## <a name="optimizing-data-transfer"></a>Gegevensoverdracht optimaliseren
Een van de langzaamste delen gegevensmigratie is de overdracht van gegevens naar Azure. Niet alleen kan de bandbreedte van het netwerk wel een probleem maar ook de betrouwbaarheid van het netwerk kan ernstig wordt verstoord voortgang. Gegevens migreren naar Azure is standaard via het internet zodat de kans op fouten in de gegevensoverdracht optreden redelijkerwijs kunnen worden. Deze fouten kunnen evenwel eisen dat gegevens geheel of gedeeltelijk opnieuw verzonden.

Gelukkig hebt u verschillende mogelijkheden voor het verbeteren van de snelheid en de veerkracht van dit proces:

### <a name="expressroute"></a>[ExpressRoute][]
U kunt overwegen om [ExpressRoute][] om de overdracht te versnellen. [ExpressRoute][] biedt u een persoonlijke verbinding naar Azure zodat de verbinding via het openbare internet gaat. Dit is in geen geval een verplichte stap. Echter dat de doorvoer verbeteren wanneer gegevens naar Azure een ruimten op duwen of collocatie faciliteit.

De voordelen van het gebruik van [ExpressRoute][] zijn:

1. Grotere betrouwbaarheid
2. Sneller netwerk
3. Lagere netwerklatentie
4. betere netwerkbeveiliging

[ExpressRoute][] is handig voor een aantal scenario's; niet alleen de migratie.

Geïnteresseerd? Ga naar de [ExpressRoute-documentatie][]voor meer informatie en prijzen Neem.

### <a name="azure-import-and-export-service"></a>Azure Import en Export Service
De Azure importeren en exporteren-Service is een data overdrachtsproces voor grote (GB ++) tot enorme (TB ++) overdracht van gegevens in Azure. Het betreft de gegevens worden geschreven naar schijven en het verzenden naar een datacenter Azure. De inhoud van de schijf zal vervolgens namens u in Azure opslag BLOB's worden geladen.

Een globaal overzicht van het importproces uitvoer is als volgt:

1. Configureer een container Azure Blob-opslag om de gegevens te ontvangen
2. De gegevens exporteren naar een lokale opslag
2. De gegevens kopiëren naar een 3,5-inch SATA III-II harde schijven met behulp van de [Azure Import/Export gereedschap]
3. Een taak importeren is met de Azure importeren en exporteren serviceverlenende de journal-bestanden die met de [Azure Import/Export gereedschap] maken
4. De schijven leveren uw aangewezen Azure Datacenter
5. Uw gegevens worden overgedragen aan de container Azure Blob-opslag
6. Laden van de gegevens in SQLDW met PolyBase

### <a name="azcopy-utility"></a>Hulpprogramma [AZCopy][]
Het hulpprogramma [AZCopy][] is een geweldig hulpmiddel voor het verkrijgen van de gegevens die binnenkomen in Azure Storage Blobs. Is ontworpen voor kleine (MB ++) tot zeer groot (GB ++) gegevensoverdracht. [AZCopy] is ook ontworpen om goede verende doorvoer bij het overbrengen van gegevens naar Azure en daarom is een ideale keuze voor de overdracht gegevens stap. Eenmaal overgedragen, kunt u de gegevens met behulp van PolyBase in SQL Data Warehouse laden. U kunt AZCopy ook opnemen in uw SSIS-pakketten met behulp van een taak 'Proces uitvoeren'.

Voor het gebruik van AZCopy moet u eerst te downloaden en te installeren. Er is een [productieversie][] en een [preview-versie][] beschikbaar.

Uploaden van een bestand in uw bestandssysteem moet u een opdracht zoals hieronder:

```
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt
```

Een overzicht van hoog niveau kan worden:

1. Een container opslag Azure blob voor het ontvangen van gegevens configureren
2. De gegevens exporteren naar een lokale opslag
3. AZCopy uw gegevens in de container Azure Blob-opslag
4. Laden van de gegevens in SQL Data Warehouse met PolyBase

Volledige documentatie beschikbaar: [AZCopy][].

## <a name="optimizing-data-export"></a>Optimaliseren gegevens exporteren
Naast het verzekeren dat de export aan de voorschriften van de PolyBase voldoet kunt u ook proberen om de uitvoer van de gegevens voor het verbeteren van het proces verder te optimaliseren.

> [AZURE.NOTE] PolyBase de gegevens in de indeling UTF-8 worden waarschijnlijk niet nodig u bcp gebruikt voor het uitvoeren van het exporteren van gegevens. BCP biedt geen ondersteuning voor het uitvoeren van gegevensbestanden in UTF-8. SSI's of kopiëren met automatische Documentinvoer zijn veel beter geschikt voor het uitvoeren van dit soort gegevens exporteren.

### <a name="data-compression"></a>Gegevenscompressie
Gecomprimeerd gzip-gegevens kunnen worden gelezen door PolyBase. Als u kunt uw gegevens met gzip-bestanden te comprimeren kunt u de hoeveelheid gegevens die via het netwerk wordt geduwd wordt minimaliseren.

### <a name="multiple-files"></a>Meerdere bestanden
Grote tabellen in verschillende bestanden splitsen niet alleen helpt bij het verbeteren van de uitvoer, maar ook met de re-startability van de overdracht en het algemene beheer van de gegevens eenmaal in de Azure blobopslag. Een van de vele mooie voorzieningen van PolyBase is Hiermee worden alle bestanden in een map gelezen en het behandelen als een tabel. Het is daarom een goed idee om de bestanden voor elke tabel isoleren in een eigen map.

PolyBase ondersteunt ook bekend als "passage recursief map". Deze functie kunt u de organisatie van de te exporteren gegevens ter verbetering van het beheer verder te verbeteren.

Zie voor meer informatie over het laden van gegevens met PolyBase, [PolyBase gebruiken om gegevens in SQL Data Warehouse te laden][].


## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over migratie, [migreren uw oplossing naar SQL Data Warehouse][].
Zie voor meer tips voor ontwikkeling, [ontwikkelen-overzicht][].

<!--Image references-->

<!--Article references-->
[AZCopy]: ../storage/storage-use-azcopy.md
[Kopiëren met automatische Documentinvoer]: ../data-factory/data-factory-data-movement-activities.md 
[ADF-voorbeelden]: ../data-factory/data-factory-samples.md
[ADF Copy examples]: ../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md
[ontwikkelen-overzicht]: sql-data-warehouse-overview-develop.md
[Uw oplossing migreren naar SQL Data Warehouse]: sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Bcp gebruiken om gegevens te laden in SQL Data Warehouse]: sql-data-warehouse-load-with-bcp.md
[PolyBase gebruiken om gegevens te laden in SQL Data Warehouse]: sql-data-warehouse-get-started-load-with-polybase.md


<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory]: http://azure.microsoft.com/services/data-factory/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[ExpressRoute documentatie]: http://azure.microsoft.com/documentation/services/expressroute/

[Productieversie]: http://aka.ms/downloadazcopy/
[Preview-versie]: http://aka.ms/downloadazcopypr/
[ADO.NET bestemming adapter]: https://msdn.microsoft.com/library/bb934041.aspx
[SSIS documentatie]: https://msdn.microsoft.com/library/ms141026.aspx
