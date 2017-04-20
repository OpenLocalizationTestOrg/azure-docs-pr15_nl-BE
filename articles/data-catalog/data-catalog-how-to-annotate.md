<properties
   pageTitle="Hoe u gegevensbronnen aantekeningen | Microsoft Azure"
   description="Hoe kan ik artikel markeren van de activa van gegevens in de catalogus met Azure gegevens, met inbegrip van beschrijvende namen, codes, omschrijvingen en deskundigen van commentaar voorzien."
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


# <a name="how-to-annotate-data-sources"></a>Hoe u gegevensbronnen van aantekeningen voorzien

## <a name="introduction"></a>Inleiding
**Catalogus gegevens van Microsoft Azure** is een volledig beheerde cloud service die als een systeem van registratie en het systeem van discovery voor gegevensbronnen van de onderneming fungeert. Met andere woorden, is de catalogus gegevens helpen mensen ontdekken, te begrijpen en gegevensbronnen en helpt organisaties gebruikt om meer waarde uit hun bestaande gegevens bij. Als een gegevensbron met gegevenscatalogus is geregistreerd, de metagegevens wordt gekopieerd en geïndexeerd door de service, maar het verhaal er niet beëindigen. Gegevenscatalogus kan gebruikers hun eigen beschrijvende metagegevens – zoals beschrijvingen en labels – ter aanvulling van de metagegevens opgehaald uit de gegevensbron en de gegevensbron begrijpelijker te maken om meer mensen op te geven.

## <a name="annotation-and-crowdsourcing"></a>Commentaar en crowdsourcing
Iedereen heeft een advies. En dit is een groot goed.
Gegevenscatalogus herkent dat verschillende gebruikers verschillende perspectieven op de gegevensbronnen van de onderneming hebben, en dat elk van deze perspectieven waardevol kan zijn. Neem het volgende scenario:

* De systeembeheerder weet de serviceniveau-overeenkomst voor de diensten of de servers die als host fungeren voor de gegevensbron.
* De beheerder van de database weet het back-upschema voor elke database en de verwerking van toegestane ETL windows.
* De eigenaar van het systeem weet het proces voor gebruikers om toegang tot de gegevensbron.
* De gegevens wereldburgers weet hoe de elementen en kenmerken in de gegevensbron aan het gegevensmodel van de onderneming toegewezen.
* De analist weet hoe de gegevens worden gebruikt in de context van de bedrijfsprocessen die hij ondersteunt.

Elk van deze perspectieven is waardevol en catalogus gegevens een crowdsourcing benadering van metagegevens waarmee een worden vastgelegd en wordt gebruikt om een volledig overzicht van geregistreerde gegevensbronnen worden gebruikt. Met behulp van de portal gegevenscatalogus, kan elke gebruiker toevoegen en bewerken van eigen aantekeningen, terwijl aantekeningen die door andere gebruikers bekijken.

## <a name="different-types-of-annotations"></a>Verschillende soorten aantekeningen
Gegevenscatalogus ondersteunt de volgende soorten aantekeningen:

| Aantekening     | Notities                                                                                                                                                                                                                                                                                                                                                           |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Beschrijvende naam  | Beschrijvende namen kunnen worden opgegeven op het niveau van de gegevens actief te maken van de activa van de gegevens gemakkelijker te begrijpen. Beschrijvende namen zijn handig als u de naam van het onderliggende object is cryptisch, afkorting of anderszins niet zinvol is voor gebruikers.                                                                                                                            |
| Beschrijving    | Beschrijvingen kunnen worden geleverd aan de activa van de gegevens en het kenmerk / kolomniveaus. Beschrijvingen zijn korte tekstaantekeningen die van de gebruiker beschrijven perspectief op de gegevens activa of het gebruik ervan.                                                                                                                                                              |
| Labels (tags van de gebruiker)          | Labels kunnen worden geleverd aan de activa van de gegevens en het kenmerk / kolomniveaus. Gebruiker-tags zijn door de gebruiker gedefinieerde labels, die kunnen worden gebruikt voor het categoriseren van gegevens elementen of kenmerken.                                                                                                                                                                                                    |
| Labels (tags woordenlijst)          | Labels kunnen worden geleverd aan de activa van de gegevens en het kenmerk / kolomniveaus. Verklarende woordenlijst-tags zijn termen uit de woordenlijst centraal gedefinieerd die kunnen worden gebruikt voor het categoriseren van gegevens activa of kenmerken met een algemene zakelijke taxonomie. Zie [de verklarende woordenlijst van Business voor Tagging geregeld instellen](data-catalog-how-to-business-glossary.md) voor meer informatie.                                                                                                                                                                                                    |
| Deskundigen        | Deskundigen kunnen worden opgegeven op het niveau van de activa. Deskundigen kunnen aangeven welke gebruikers of groepen met deskundige perspectieven op de gegevens en dienen als contactpunten voor gebruikers die de geregistreerde gegevensbronnen te ontdekken en u vragen hebt die niet wordt beantwoord door de bestaande aantekeningen.  |
| Toegang aanvragen | Aanvraag voor toegang tot informatie kan worden opgegeven op het niveau van de activa. Deze informatie is bedoeld voor gebruikers die het ontdekken van een gegevensbron die zij nog geen toegang hebben tot. Gebruikers kunnen invoeren van het e-mailadres van de gebruiker of groep die toegang, de URL van het proces of die gebruikers toegang verleent of het proces zelf kunnen invoeren als tekst. |
| Documentatie | Documentatie kan worden opgegeven op het niveau van de activa. Activa-documentatie is RTF-gegevens die koppelingen en afbeeldingen opnemen kunt en die informatie niet worden overgebracht door middel van beschrijvingen en labels kunnen bieden. |


## <a name="annotating-multiple-assets"></a>Meerdere activa aantekeningen maken
Bij het selecteren van meerdere elementen van de gegevens in de portal gegevenscatalogus, kunnen gebruikers alle geselecteerde activa in één bewerking van aantekeningen voorzien. Aantekeningen van toepassing op alle geselecteerde activa, zodat u eenvoudig kunt selecteren en een consistente beschrijving en tags en deskundigen voor activa gerelateerde gegevens bevatten.

> [AZURE.NOTE] Labels en deskundigen kunnen ook worden geleverd bij de registratie van een gegevensbron registreren gegevens activa met behulp van de gegevens in de catalogus van de gegevens.

Als meerdere tabellen en weergaven, alleen kolommen dat alle geselecteerde activa gemeenschappelijke hebben gegevens selecteren wordt weergegeven in de portal gegevenscatalogus. Hierdoor kunnen gebruikers te voorzien van labels en beschrijvingen voor alle kolommen met dezelfde naam voor alle geselecteerde activa.

## <a name="annotations-and-discovery"></a>Aantekeningen en discovery
Net als de metagegevens opgehaald uit de gegevensbron tijdens de registratie wordt toegevoegd aan de zoekindex gegevenscatalogus, wordt de gebruiker opgegeven metagegevens ook geïndexeerd. Dit betekent dat niet alleen aantekeningen in gemakkelijker voor gebruikers om te begrijpen van de gegevens die ze detecteren, aantekeningen ook gemakkelijker voor gebruikers om te ontdekken de activa geannoteerde gegevens door te zoeken met behulp van de termen die zinvol zijn voor hen.

## <a name="summary"></a>Samenvatting
Een gegevensbron met catalogus gegevens registreren kunt u die gegevens detecteerbaar structurele en beschrijvende metagegevens uit de gegevensbron te kopiëren in de catalogus service. Nadat u een gegevensbron is geregistreerd, kunnen gebruikers aantekeningen gemakkelijker te ontdekken en begrijpen van binnen de portal catalogus gegevens opgeven.

## <a name="see-also"></a>Zie ook
- Zelfstudie [Aan de slag met Azure gegevenscatalogus](data-catalog-get-started.md) voor meer informatie over de gegevensbronnen van aantekeningen voorzien.
