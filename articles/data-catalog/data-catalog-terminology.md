<properties
   pageTitle="Terminologie van Azure catalogus gegevens | Microsoft Azure"
   description="Dit artikel bevat een inleiding tot de concepten en termen die in de catalogus gegevens Azure documentatie."
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
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-terminology"></a>Azure catalogus gegevens-terminologie

## <a name="catalog"></a>Catalogus

De catalogus met Azure gegevens is een cloud-gebaseerde metagegevens opslagplaats waarin activa bronnen en gegevens kunnen worden geregistreerd. De catalogus fungeert als een centrale opslaglocatie voor structurele metagegevens uit de gegevensbronnen opgehaald en beschrijvende metagegevens door gebruikers worden toegevoegd.

## <a name="data-source"></a>Gegevensbron

Een gegevensbron is een systeem of de container waarin de gegevens activa beheert. Voorbeelden zijn databases van SQL Server, Oracle-databases, databases van SQL Server Analysis Services (in tabelvorm of multidimensionale) en SQL Server Reporting Services-servers.

## <a name="data-asset"></a>Asset gegevens

Gegevens activa zijn objecten in de gegevensbronnen die kunnen worden geregistreerd met de catalogus. Voorbeelden zijn de SQL Server-tabellen en weergaven, Oracle-tabellen en weergaven, SQL Server Analysis Services maatregelen, dimensies en KPI's en SQL Server Reporting Services-rapporten.

## <a name="data-asset-location"></a>Locatie van activa

De catalogus wordt opgeslagen van de locatie van een gegevensbron of de gegevens activa, die kan worden gebruikt voor verbinding met de bron met behulp van een clienttoepassing. De indeling en de details van de locatie zijn afhankelijk van het gegevensbrontype. Een SQL Server-tabel kan bijvoorbeeld worden geïdentificeerd door de naam van deel vier: naam, naam van de database, schemanaam, objectnaam: terwijl een SQL Server Reporting Services-rapport kan worden geïdentificeerd door de bijbehorende URL.

## <a name="structural-metadata"></a>Structurele metagegevens

Structurele metagegevens zijn de metagegevens opgehaald uit een gegevensbron die de structuur van een activum gegevens beschrijft. Het gaat hierbij om de locatie van de activa, de objectnaam en type en type-specifieke kenmerken. De structurele metagegevens voor tabellen en weergaven bevat bijvoorbeeld de namen en de gegevenstypen voor kolommen van het object.

## <a name="descriptive-metadata"></a>Beschrijvende metadata

Beschrijvende metagegevens zijn metagegevens die beschrijving van het doel of de intentie van een activum gegevens. Meestal beschrijvende metagegevens wordt toegevoegd door gebruikers van de catalogus met de catalogus met Azure gegevens portal, maar het kan ook worden afgeleid uit de gegevensbron tijdens de registratie. Bijvoorbeeld wordt de catalogus van Azure gegevens registratiehulpprogramma uitgepakt beschrijvingen van de eigenschap beschrijving in SQL Server Analysis Services en SQL Server Reporting Services en van de [ms_description uitgebreide eigenschap](https://technet.microsoft.com/library/ms190243.aspx) in SQL Server-databases, als deze eigenschappen zijn gevuld met waarden.

## <a name="request-access"></a>Toegang aanvragen

Beschrijvende metagegevens van een activum gegevens kunt opnemen voor het aanvragen van toegang tot de gegevens activa of gegevensbron. Deze informatie wordt weergegeven met de locatie van activa, en kan bevatten een of meer van de volgende opties:

- Het e-mailadres van de gebruiker of het team dat verantwoordelijk is voor het verlenen van toegang tot de gegevensbron.
- De URL van de gedocumenteerde proces dat gebruikers toegang krijgen tot de gegevensbron moeten volgen.
- De URL van een identiteits- en toegangsbeheer management tool (zoals Microsoft Identity Manager) die kan worden gebruikt voor toegang tot de gegevensbron.
- Een vrije-fragment waarin wordt beschreven hoe gebruikers toegang tot de gegevensbron.

## <a name="preview"></a>Voorbeeld

Een voorbeeld in de catalogus gegevens Azure is een momentopname van maximaal 20 records die kunnen worden opgehaald uit de gegevensbron tijdens de registratie en opgeslagen in de catalogus met de metagegevens gegevens actief. Het voorbeeld kan helpen bij het ontdekken van een activum gegevens gebruikers beter te begrijpen functie en het doel. Met andere woorden, voorbeeldgegevens te zien is meer waard dan alleen de kolomnamen en de gegevenstypen te zien.
Voorbeelden worden alleen ondersteund voor tabellen en weergaven en moeten worden expliciet door de gebruiker geselecteerd tijdens de registratie.

## <a name="data-profile"></a>Profiel gegevens

Een profiel met gegevens in de catalogus gegevens Azure is een momentopname van de tabel- en kolom-niveau metagegevens over een geregistreerde gegevens activa die kan worden opgehaald uit de gegevensbron tijdens de registratie en opgeslagen in de catalogus met de metagegevens gegevens actief. Het profiel gegevens kan helpen bij het ontdekken van een activum gegevens gebruikers beter te begrijpen functie en het doel. Net als voorbeelden, profielen van de gegevens moeten worden expliciet door de gebruiker geselecteerd tijdens de registratie.

> [AZURE.NOTE] Een profiel gegevens extraheren kan een kostbare onderneming voor grote tabellen en weergaven en de benodigde tijd voor het registreren van een gegevensbron aanzienlijk kunnen verbeteren.

## <a name="user-perspective"></a>Gebruikersperspectief

In catalogus gegevens Azure, kan elke gebruiker beschrijvende metadata bieden voor een activum geregistreerde gegevens. Elke gebruiker heeft een verschillend perspectief op de gegevens en het gebruik ervan. Bijvoorbeeld kan de beheerder die verantwoordelijk is voor een server gebruikmaken van de details van de serviceovereenkomst (SLA) of back-ups; een wereldburgers gegevens kan bevatten koppelingen naar documentatie voor het bedrijf verwerkt de gegevens worden ondersteund; een analist ten behoeve van en de omschrijving van de voorwaarden die zijn het meest relevant zijn voor andere analisten, en die het meest waardevol zijn voor gebruikers die moeten ontdekken en begrijpen van de gegevens.

Elk van deze perspectieven zijn per definitie ook waardevolle en met catalogus Azure-gegevens elke gebruiker de informatie die zinvol is voor hen, terwijl alle gebruikers die informatie kunnen gebruiken om de gegevens en het doel inzicht te kan bieden.

## <a name="expert"></a>Expert

Een expert is een gebruiker die is geïdentificeerd als een weloverwogen 'gevorderde' perspectief voor een activum gegevens hebben. Een gebruiker kan zelf of door een andere gebruiker als een expert voor een actief toevoegen. Wordt weergegeven als een expert in Azure gegevenscatalogus; extra bevoegdheden niet overbrengen Hiermee kunt gebruikers gemakkelijk vinden die perspectieven die waarschijnlijk het meest nuttig zijn bij het controleren van de beschrijvende metagegevens van een activum.

## <a name="owner"></a>Eigenaar

Een eigenaar is een gebruiker extra bevoegdheden voor het beheren van een activum data in Azure gegevenscatalogus. Gebruikers eigenaar kunnen worden van geregistreerde gegevens activa en eigenaren van andere gebruikers kunnen toevoegen als collega eigenaren. Zie voor meer informatie [hoe gegevens activa beheren](data-catalog-how-to-manage.md)  
> [AZURE.NOTE] Eigendom en beheer zijn alleen beschikbaar in de Standard Edition van Azure catalogus gegevens.

## <a name="registration"></a>Registratie

Registratie is de handeling van gegevens activa metagegevens uitpakken uit een gegevensbron en te kopiëren naar de service catalogus Azure-gegevens. Gegevens activa die zijn geregistreerd, kunnen vervolgens van aantekeningen voorzien en ontdekt.

## <a name="see-also"></a>Zie ook

- [Wat is catalogus Azure-gegevens?](data-catalog-what-is-data-catalog.md) -Dit artikel biedt een overzicht van de service catalogus Azure-gegevens, de waarde die het biedt en de scenario's ondersteunt.

- [Aan de slag met Azure gegevenscatalogus](data-catalog-get-started.md) - dit artikel biedt een end-to-end-zelfstudie waarin catalogus Azure-gegevens gebruiken voor gegevensbron te zoeken.  
