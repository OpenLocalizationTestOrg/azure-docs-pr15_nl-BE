<properties
   pageTitle="Het ontdekken van gegevensbronnen | Microsoft Azure"
   description="Hoe kan ik artikel markeren van de geregistreerde gegevens activa met een catalogus met Azure gegevens, met inbegrip van zoeken en filteren met behulp van de mogelijkheden van de portal catalogus met Azure gegevens markeren hit ontdekken."
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

# <a name="how-to-discover-data-sources"></a>Het ontdekken van gegevensbronnen

## <a name="introduction"></a>Inleiding
**Catalogus gegevens van Microsoft Azure** is een volledig beheerde cloud service die als een systeem van registratie en het systeem van discovery voor gegevensbronnen van de onderneming fungeert. **Catalogus met Azure gegevens** is met andere woorden, helpen mensen ontdekken, te begrijpen en gegevensbronnen en helpt organisaties gebruikt om meer waarde uit hun bestaande gegevens bij. Als bron voor een **Catalogus met Azure gegevens**is geregistreerd, worden de metagegevens wordt geïndexeerd door de service, zodat gebruikers gemakkelijk terugvinden kunnen als u wilt ontdekken, de gegevens die ze nodig hebben.

## <a name="searching-and-filtering"></a>Zoeken en filteren

Discovery in **Azure gegevenscatalogus** maakt gebruik van twee primaire mechanismen: zoeken en filteren.

Zoeken is ontworpen intuïtief en krachtig – standaard, zoektermen worden vergeleken met elke eigenschap in de catalogus, met inbegrip van de gebruiker opgegeven aantekeningen.

Filteren is ontworpen als aanvulling op zoeken. Gebruikers kunnen selecteren van specifieke kenmerken zoals deskundigen, gegevensbrontype objecttype en tags, alleen overeenkomende gegevens activa weergeven en congruente activa en zoekresultaten beperken.

Gebruikers kunnen snel de gegevensbronnen die zijn geregistreerd in **Azure gegevenscatalogus** en ontdek de benodigde gegevensbronnen navigeren met behulp van een combinatie van zoeken en filteren.

## <a name="search-syntax"></a>Syntaxis Search

Hoewel de vrije tekst zoekopdracht eenvoudig en intuïtief is, kunnen gebruikers **Azure catalogus met gegevens**van zoeken syntaxis ook gebruiken om meer controle over de zoekresultaten. Zoeken in **Catalogus Azure-gegevens** ondersteunt de volgende technieken:

| Techniek                 | Gebruik                                                                                                                                     | Voorbeeld                                                   |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| Standaardzoekopdracht              | Eenvoudig zoeken met behulp van een of meer zoektermen. De resultaten zijn elementen die overeenkomen met alle eigenschappen met een of meer van de opgegeven voorwaarden. | verkoopgegevens                                                |
| Eigenschap Scoping          | Alleen wanneer de zoekterm met de opgegeven eigenschap overeenkomt gegevensbronnen retourneren                                                   | naam: Financiën                                              |
| Boole-Operators         | Uitbreiden of beperken van een zoekactie met behulp van Booleaanse bewerkingen                                                                                     | NIET-financiële ondernemingen                                     |
| Groeperen met haakjes | Haakjes groep onderdelen van de query gebruiken om logische isolatie, vooral in combinatie met Boole-operators              | naam: Financiën en (tags: Q1 of tags: Q2) |
| Vergelijkingsoperatoren      | Vergelijkingen dan gelijkheid voor eigenschappen met datum- en numerieke gegevenstypen gebruiken                                                | modifiedTime > "05-11/2014"                                 |

Zie voor meer informatie over zoeken in **Azure gegevenscatalogus** , [https://msdn.microsoft.com/library/azure/mt267594.aspx](https://msdn.microsoft.com/library/azure/mt267594.aspx).

## <a name="hit-highlighting"></a>Klik op markeren
Als u zoekresultaten bekijkt, eigenschappen weergegeven die overeenkomen met de opgegeven zoektermen – zoals de naam van de gegevens activa, beschrijving en tags: gemarkeerd om aan te geven waarom een bepaalde gegevens actief is geretourneerd door een bepaalde zoekactie te vereenvoudigen.

> [AZURE.NOTE] Gebruikers kunnen treffers markering uitschakelen indien gewenst, met behulp van de schakeloptie 'Markeren' in de **Catalogus van Azure gegevens** portal inschakelen.

Als u zoekresultaten weergeeft, kan niet altijd zijn duidelijk waarom een asset gegevens opgenomen, is zelfs met een treffer-highlighting ingeschakeld. Omdat alle eigenschappen standaard gezocht zijn, kan een asset gegevens worden weergegeven als gevolg van een overeenkomst voor een eigenschap op kolomniveau. En omdat gebruikers meerdere geregistreerde gegevens activa met hun eigen codes en omschrijvingen aantekeningen kunnen niet alle metagegevens kan worden weergegeven in de lijst met zoekresultaten.

Naast elkaar weergeven in de standaard, elke naast elkaar weergegeven in de zoekresultaten een pictogram "weergave zoekterm overeenkomt met", waarmee de gebruiker het aantal treffers en hun locatie snel weergeven en gaan indien gewenst worden opgenomen.

 ![Druk op markeren en zoeken in de portal Azure catalogus met gegevens komt overeen met](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a>Samenvatting
Registreren van een gegevensbron met **Azure catalogus met gegevens** eenvoudiger die gegevensbron te ontdekken en te begrijpen, door structurele en beschrijvende metagegevens uit de gegevensbron te kopiëren in de catalogus service. Nadat u een gegevensbron is geregistreerd, kunnen gebruikers met behulp van filteren en zoeken van binnen de portal **Catalogus Azure-gegevens** vinden.

## <a name="see-also"></a>Zie ook
- Zelfstudie [Aan de slag met Azure gegevenscatalogus](data-catalog-get-started.md) voor meer informatie over het ontdekken van gegevensbronnen.
