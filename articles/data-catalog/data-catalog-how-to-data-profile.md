<properties
    pageTitle="Hoe u gegevensbronnen voor Data-profiel"
    description="Markering voor het opnemen van profielen van tabel - en kolomniveau gegevens bij het registreren van gegevensbronnen in Azure catalogus met gegevens en profielen van gegevens gebruiken om u te begrijpen gegevensbronnen artikel."
    services="data-catalog"
    documentationCenter=""
    authors="spelluru"
    manager="NA"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/13/2016"
    ms.author="spelluru"/>

# <a name="data-profile-data-sources"></a>Gegevensbronnen voor Data-profiel

## <a name="introduction"></a>Inleiding

**Catalogus gegevens van Microsoft Azure** is een volledig beheerde cloud service die als een systeem van registratie en het systeem van discovery voor gegevensbronnen van de onderneming fungeert. **Catalogus met Azure gegevens** is met andere woorden, helpen mensen ontdekken, te begrijpen en gegevensbronnen en helpt organisaties gebruikt om meer waarde uit hun bestaande gegevens bij. Als een gegevensbron met **Azure gegevenscatalogus**is geregistreerd, de metagegevens wordt gekopieerd en geïndexeerd door de service, maar het verhaal er niet beëindigen.

De functie **Gegevens profilering** van **Catalogus met Azure gegevens** controleert de gegevens van de ondersteunde gegevensbronnen in uw catalogus en statistieken en informatie over de gegevens die worden verzameld. Het is eenvoudig om een profiel van de activa van uw gegevens. Wanneer u een activum gegevens registreert, kiest u **Gegevens-profiel opnemen** in het hulpprogramma voor de registratie van gegevens.

## <a name="what-is-data-profiling"></a>Wat is Data Profiling

Data profiling onderzoekt de gegevens in de gegevensbron wordt geregistreerd en statistieken en informatie over de gegevens die worden verzameld. Tijdens de detectie van data source kunt deze statistieken u bepalen de geschiktheid van de gegevens in hun business-probleem op te lossen.

<!-- In [How to discover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How to include a data profile when registering a data source](#howto). -->

De volgende gegevensbronnen ondersteunen profilering van gegevens:

- SQL Server (inclusief Azure SQL DB en Azure SQL Data Warehouse) tabellen en weergaven
- Oracle-tabellen en weergaven
- Teradata-tabellen en weergaven
- Component-tabellen

Inclusief profielen van gegevens bij het registreren van gegevens activa kan gebruikers beantwoorden vragen over gegevensbronnen, waaronder:

-   Kan worden gebruikt om op te lossen mijn probleem business?
-   De gegevens voldoen aan bepaalde normen of patronen?
-   Wat zijn de afwijkingen van de gegevensbron?
-   Wat zijn de mogelijke nadelen van deze gegevens integreren in mijn toepassing?

> [AZURE.NOTE] U kunt ook documentatie toevoegen aan een activum om te beschrijven hoe de gegevens kan worden geïntegreerd in een toepassing. Zie [het vastleggen van gegevensbronnen](data-catalog-how-to-documentation.md).


<a name="howto"/>
## <a name="how-to-include-a-data-profile-when-registering-a-data-source"></a>Het opnemen van een profiel gegevens bij het registreren van een gegevensbron

Het is eenvoudig om een profiel van uw gegevensbron. Wanneer u een gegevensbron in het deelvenster **objecten om te worden geregistreerd** van het hulpprogramma voor de registratie van gegevens registreert, kiest u **Gegevens-profiel opnemen**.

![](media\data-catalog-data-profile\data-catalog-register-profile.png)

Zie voor meer informatie over het registreren van gegevensbronnen, [het registreren van, gegevensbronnen](data-catalog-how-to-register.md) en [aan de slag met Azure gegevenscatalogus](data-catalog-get-started.md).


## <a name="filtering-on-data-assets-that-include-data-profiles"></a>Filteren op activa gegevens waarin gegevens profielen
Als u wilt ontdekken gegevens activa die een profiel met gegevens bevatten, kunt u opnemen `has:tableDataProfiles` of `has:columnsDataProfiles` als één van de zoektermen.

> [AZURE.NOTE] **Gegevens-profiel opnemen** selecteert in het hulpprogramma voor de registratie van gegevens omvat zowel tabel profielgegevens op kolomniveau. De catalogus Data API kunt u echter gegevens activa worden geregistreerd met slechts één set informatie opgenomen over het profiel.

## <a name="viewing-data-profile-information"></a>Profielgegevens weergeven

Als u een geschikte gegevensbron met een profiel kunt vinden, kunt u de gegevens uit het profiel gegevens bekijken. Profiel weer te geven gegevens, selecteert u een element met gegevens en **Gegevens profiel** kiezen in de portal gegevens Catalog-venster.

![](media\data-catalog-data-profile\data-catalog-view.png)

Een profiel met gegevens in **De catalogus gegevens Azure** geeft tabel en kolom profiel informatie zoals:

### <a name="object-data-profile"></a>Profiel van object-gegevens

-   Aantal rijen
-   Tabelgrootte
-   Wanneer het object het laatst is bijgewerkt

### <a name="column-data-profile"></a>Kolom gegevens profiel

- Kolomgegevenstype
- Het aantal unieke waarden
- Aantal rijen met NULL-waarden
- Minimum, maximum, gemiddelde en standaarddeviatie voor kolomwaarden

## <a name="summary"></a>Samenvatting
Data profiling bevat statistieken en informatie over de activa van de geregistreerde gegevens om te bepalen van de geschiktheid van de gegevens, problemen oplossen. Aantekeningen maken en documenteren van gegevensbronnen, kunnen gegevens profielen geven gebruikers een beter begrip van uw gegevens.


## <a name="see-also"></a>Zie ook
-   [Het registreren van gegevensbronnen](data-catalog-how-to-register.md)
-   [Aan de slag met Azure gegevenscatalogus](data-catalog-get-started.md)
