<properties
    pageTitle="Azure Search index maken | Microsoft Azure | De zoekservice hosted cloud"
    description="Wat is een index in Azure zoeken en hoe wordt het gebruikt?"
    services="search"
    manager="jhubbard"
    documentationCenter=""
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index"></a>Azure Search index maken
> [AZURE.SELECTOR]
- [Overzicht](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [REST](search-create-index-rest-api.md)

## <a name="what-is-an-index"></a>Wat is een index?

Een *index* is een blijvende opslag van *documenten* en andere constructies die door een Azure Search-service worden gebruikt. Een document is een eenheid van doorzoekbare gegevens in de index. Bijvoorbeeld een e-commerce winkel mogelijk een document voor elk artikel dat ze verkopen, een nieuws organisatie mogelijk een document voor elk artikel, enz. Deze concepten toewijzen aan database-equivalenten meer vertrouwd: een *index* is in essentie gelijk aan een *tabel*en *documenten* zijn ongeveer gelijk zijn aan *rijen* in een tabel.

Wanneer u documenten toevoegen/uploaden en indienen van zoekquery's Azure zoeken, kunt u uw aanvragen voor een specifieke index indienen in de search-service.

## <a name="field-types-and-attributes-in-an-azure-search-index"></a>Veldtypen en kenmerken in een zoekindex Azure

Als u uw schema definieert, moet u de naam, type en kenmerken van elk veld in de index. Het veldtype classificeert de gegevens die zijn opgeslagen in dat veld. Kenmerken zijn ingesteld op afzonderlijke velden kunt u opgeven hoe het veld wordt gebruikt. De volgende tabellen opsommen en zo de aard en de kenmerken die u kunt opgeven.


### <a name="field-types"></a>Veldtypen
|Type|Beschrijving|
|------------|-----------|
|*Edm.String*|De tekst kan eventueel worden ge? exeerd voor volledige-tekstzoekacties (woordafbreking, afleiding, enzovoort).|
|*Collection(EDM.String)*|Een lijst met tekenreeksen die kan eventueel worden ge? exeerd voor zoeken in volledige tekst. Er is geen theoretische bovengrens voor het aantal items in een collectie, maar de bovengrens van 16 MB op pakketgrootte is van toepassing op verzamelingen.|
|*Edm.Boolean*|Waar/onwaar-waarden bevat.|
|*Edm.Int32*|32-bits gehele getallen.|
|*Edm.Int64*|64-bits gehele getallen.|
|*Edm.Double*|Getal met dubbele precisie numerieke gegevens.|
|*Edm.DateTimeOffset*|Datum-tijdwaarden worden weergegeven in de OData-V4-indeling (bv. `yyyy-MM-ddTHH:mm:ss.fffZ` of `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`).|
|*Edm.GeographyPoint*|Een punt vertegenwoordigt van een geografische locatie in de hele wereld.|

Hier vindt u meer gedetailleerde informatie over Azure Search van [ondersteunde gegevenstypen op MSDN](https://msdn.microsoft.com/library/azure/dn798938.aspx).



### <a name="field-attributes"></a>Veldkenmerken
|Kenmerk|Beschrijving|
|------------|-----------|
|*Sleutel*|Een tekenreeks waarmee de unieke ID van elk document, gebruikt voor het document opzoeken. Elke index moet een sleutel hebben. Slechts één veld kan de sleutel en het type moet worden ingesteld op Edm.String.|
|*Worden opgehaald*|Hiermee geeft u aan of een veld kan worden weergegeven in een zoekresultaat.|
|*Filterbaar*|Hiermee kunt u het veld moet worden gebruikt in query's filter.|
|*Sorteerbare*|Hiermee kunt een query voor het sorteren van zoekresultaten met behulp van dit veld.|
|*Facetable*|Hiermee kunt u een veld in de navigatiestructuur van een [beperkt](search-faceted-navigation.md) worden gebruikt voor het filteren van gebruiker gericht. Meestal werken met herhalende waarden kunt u meerdere documenten groeperen velden (bijvoorbeeld meerdere documenten die onder een enkel merk of op categorie vallen-) het beste als facetten.|
|*Doorzoekbare*|Geeft het veld full-text doorzoekbaar.|

Hier vindt u meer gedetailleerde informatie over de Azure Search [indexkenmerken op MSDN](https://msdn.microsoft.com/library/azure/dn798941.aspx).



## <a name="guidance-for-defining-an-index-schema"></a>Richtlijnen voor het definiëren van het schema van een index

Bij het ontwerpen van uw index uw tijd in de planningfase om na te denken door middel van elke beslissing nemen. Het is belangrijk uw zoekopdracht ervaring en zakelijke behoeften van de gebruiker in het achterhoofd te houden bij het ontwerpen van de index als elk veld de [juiste kenmerken](https://msdn.microsoft.com/library/azure/dn798941.aspx)moet worden toegewezen. Een index wijzigen nadat deze is geïmplementeerd, moet opnieuw samenstellen en laden van de gegevens.


Als de gegevensopslag wijzigen, kunt u vergroten of verkleinen capaciteit toe te voegen of te verwijderen van partities. Zie voor meer informatie [de zoekservice in Azure beheren](search-manage.md) of [Beperkingen van de Service](search-limits-quotas-capacity.md).
