<properties
   pageTitle="Het werken met gegevensbronnen 'big data' | Microsoft Azure"
   description="Markering patronen voor het gebruik van Azure gegevenscatalogus met 'big data'-gegevensbronnen, waaronder Azure Blob-opslag en Azure gegevens Lake Hadoop HDFS artikel."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>


# <a name="how-to-work-with-big-data-sources-in-azure-data-catalog"></a>Het werken met grote gegevensbronnen in Azure gegevenscatalogus

## <a name="introduction"></a>Inleiding
**Catalogus gegevens van Microsoft Azure** is een volledig beheerde cloud service die als een systeem van registratie en het systeem van discovery voor gegevensbronnen van de onderneming fungeert. **Catalogus met Azure gegevens** is met andere woorden, alle informatie over mensen helpen ontdekken, te begrijpen en gegevensbronnen en helpt organisaties gebruikt om meer waarde uit hun bestaande gegevensbronnen, met inbegrip van grote.

**Catalogus Azure-gegevens** ondersteunt de registratie van Azure Blog opslag BLOB's en mappen Hadoop HDFS bestanden en mappen. De semi-gestructureerde aard van deze gegevensbronnen biedt een uitstekende flexibiliteit, maar het betekent ook dat gebruikers moeten overwegen hoe de gegevensbronnen worden ingedeeld om de meeste waarde uit met de **Catalogus met Azure gegevens**geregistreerd.

## <a name="directories-as-logical-data-sets"></a>Mappen als logische gegevenssets

Een gemeenschappelijk patroon voor het organiseren van grote gegevensbronnen is mappen als gegevens in logische verzamelingen te behandelen. Mappen op het hoogste niveau worden gebruikt om een gegevensset definiëren terwijl definieert partities van submappen en bestanden de gegevens zelf worden opgeslagen.

Een voorbeeld van dit patroon mogelijk:

    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...

In dit voorbeeld zijn vehicle_maintenance_events en location_tracking_events gegevens in logische verzamelingen. Elk van deze mappen bevat bestanden die worden georganiseerd door het jaar en de maand in de submappen. Elk van deze mappen zich mogelijk honderden of duizenden bestanden.

In dit patroon logisch registreren van afzonderlijke bestanden met een **Catalogus met Azure gegevens** waarschijnlijk niet. In plaats daarvan de mappen met daarin de gegevenssets die relevant zijn voor gebruikers die werken met de gegevens worden geregistreerd.

## <a name="reference-data-files"></a>Referentie-gegevensbestanden

Een aanvullende patroon is gegevenssets verwijzing opslaan als afzonderlijke bestanden. Deze sets gegevens kunnen worden beschouwd als de 'kleine'-kant van big data en vaak vergelijkbaar zijn met dimensies in een model van analytische gegevens. Referentie-gegevensbestanden bevatten records die worden gebruikt voor context bieden voor het grootste deel van de gegevensbestanden die elders zijn opgeslagen in het gegevensarchief groot.

Een voorbeeld van dit patroon mogelijk:

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

Wanneer een analist of gegevens wetenschappelijk met de gegevens in de grotere mapstructuren werkt, kunnen de gegevens in deze bestanden verwijst naar meer gedetailleerde informatie voor entiteiten die alleen door de naam of ID worden aangeduid in de grotere set gegevens worden gebruikt.

In dit patroon, zal het zinvol zijn de individuele referentiehoeveelheid gegevensbestanden met **Azure gegevenscatalogus**registreren. Elk bestand vertegenwoordigt een gegevensset en elk kan worden gemarkeerd en afzonderlijk ontdekt.

## <a name="alternate-patterns"></a>Alternatieve patronen

De hierboven beschreven patronen zijn slechts twee mogelijke manieren die een groot gegevensarchief kan worden georganiseerd, maar elke uitvoering anders zijn. Ongeacht hoe de gegevensbronnen worden gestructureerd, bij het registreren van grote gegevensbronnen met **Azure gegevenscatalogus**, gericht op het registreren van de bestanden en mappen die de gegevenssets die waarde aan anderen binnen uw organisatie te vertegenwoordigen. Registreren van alle bestanden en mappen kan elkaar op de catalogus, waardoor het moeilijker voor gebruikers om te vinden wat ze nodig hebben.

## <a name="summary"></a>Samenvatting
Gegevensbronnen met **Azure gegevenscatalogus** geregistreerd zijn zij eenvoudiger te ontdekken en te begrijpen. Door registratie en annoteren van grote gegevensbestanden en mappen die gegevens in logische verzamelingen te staan, kunt u gebruikers zoeken en gebruiken van de grote gegevensbronnen die ze nodig hebben.
