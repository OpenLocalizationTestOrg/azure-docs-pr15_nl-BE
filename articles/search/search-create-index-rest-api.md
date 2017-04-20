<properties
    pageTitle="Met de REST API Azure Search index maken | Microsoft Azure | De zoekservice hosted cloud"
    description="Een index maken in code met behulp van Azure Search HTTP REST API."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index-using-the-rest-api"></a>Met de REST API Azure Search index maken
> [AZURE.SELECTOR]
- [Overzicht](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [REST](search-create-index-rest-api.md)


Dit artikel begeleidt u door het proces van het maken van een Azure Search [index](https://msdn.microsoft.com/library/azure/dn798941.aspx) met de Azure Search REST API.

Voordat u deze handleiding te volgen en het maken van een index, hebt u al [een Azure Search-service gemaakt](search-create-service-portal.md).

Een Azure Search als index wilt maken met de REST API, verleent u aan uw Azure Search-service-eindpunt voor URL een HTTP POST-aanvraag. De definitie van de index worden, opgeslagen in het hoofdgedeelte van de aanvraag als juist opgemaakte JSON-inhoud.


## <a name="i-identify-your-azure-search-services-admin-api-key"></a>I. Identificeren van uw Azure Search service admin api-sleutel
Nu dat u een zoekservice Azure hebt ingericht, kunt u de HTTP-verzoeken ten opzichte van uw service URL eindpunt met de REST API uitgeven. *Alle* API-aanvragen moeten echter wel de api-sleutel die is gegenereerd voor de zoekservice inrichten. Met een geldige code stelt vertrouwen op een basis per aanvraag, tussen de toepassing voor het verzenden van de aanvraag en de service die het verwerkt.

1. Om van uw service api-toetsen u moet zich aanmelden bij de [Azure Portal](https://portal.azure.com/)
2. Ga naar de blade van de Azure Search-service
3. Klik op het pictogram "Toetsen"

De service heeft *admin* en *sleutels van de query*.

 - De primaire en secundaire *sleutels admin* volledige rechten toekennen voor alle bewerkingen, inclusief de mogelijkheid om de service beheren, maken en verwijderen van indexen, Indexeerfuncties en gegevensbronnen. Er zijn twee sleutels zodat u gebruiken van de secundaire sleutel blijven kunt als u besluit om opnieuw te genereren voor de primaire sleutel, en vice versa.
 - De *sleutels van de query* alleen-lezen toegang verlenen tot documenten en indexen en zijn meestal verdeeld voor clienttoepassingen die verzoeken zoeken.

Voor de toepassing van het maken van een index, kunt u ofwel uw admin primaire of secundaire sleutel.

## <a name="ii-define-your-azure-search-index-using-well-formed-json"></a>II. Definieer uw juist opgemaakte JSON met Azure Search-index
Een enkele HTTP POST-aanvraag met uw service wordt uw index te maken. Het hoofdgedeelte van de HTTP POST-aanvraag bevat één JSON-object waarin de zoekindex Azure.

1. De eerste eigenschap van dit JSON-object is de naam van de index.
2. De tweede eigenschap van dit object JSON is een JSON-array met de naam `fields` die voor elk veld in de index een aparte JSON-object bevat. Elk van deze objecten JSON meerdere naam/waarde-paren bevatten voor elk van de kenmerken van met inbegrip van de "naam", "type", enz.

Het is belangrijk uw zoekopdracht ervaring en zakelijke behoeften van de gebruiker in het achterhoofd te houden bij het ontwerpen van de index als elk veld de [juiste kenmerken](https://msdn.microsoft.com/library/azure/dn798941.aspx)moet worden toegewezen. Deze kenmerken bepalen welke functies (filteren, faceting, sorteren, zoeken in volledige tekst, enz.) zoeken van toepassing op velden. Voor elk kenmerk dat u opgeeft, worden standaard de bijbehorende zoekfunctie inschakelen, tenzij specifiek uitschakelen.

In ons voorbeeld hebben we onze index "hotels" genoemd en onze velden als volgt gedefinieerd:

```JSON
{
    "name": "hotels",  
    "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String", "facetable": false},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean", "sortable": false},
        {"name": "smokingAllowed", "type": "Edm.Boolean", "sortable": false},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
    ]
}
```

We hebt zorgvuldig de indexkenmerken voor elk veld op basis van hoe we denken dat ze worden gebruikt in een toepassing gekozen. Bijvoorbeeld `hotelId` is een unieke sleutel die mensen zoeken naar hotels waarschijnlijk weet niet, dus we zoeken in volledige tekst voor dat veld uitschakelen door `searchable` op `false`, dat bespaart ruimte in de index.

Houd er rekening mee dat slechts één veld in de index van het type `Edm.String` moet de aangewezen als het sleutelveld.

De bovenstaande definitie van de index wordt gebruikt voor een aangepaste taal analyzer voor het `description_fr` omdat het is bedoeld voor het opslaan van Franse tekst veld. Zie [de taal ondersteunen onderwerp op MSDN](https://msdn.microsoft.com/library/azure/dn879793.aspx) , alsmede de bijbehorende [blogberichten](https://azure.microsoft.com/blog/language-support-in-azure-search/) voor meer informatie over taal analyzers.

## <a name="iii-issue-the-http-request"></a>III. Het HTTP-verzoek verzenden
1. Met de definitie van de index als het hoofdgedeelte van de aanvraag, een HTTP POST-aanvraag uitgeven aan uw Azure Search-service-eindpunt-URL. Zorg ervoor dat u de servicenaam van uw te gebruiken als de hostnaam en de juiste plaats in de URL, `api-version` als queryreeksparameter (de huidige versie van de API `2015-02-28` op het moment van publicatie van dit document).
2. Geef in de aanvraagheaders de `Content-Type` als `application/json`. U moet ook voorzien van uw service admin sleutel die u hebt bepaald in stap I in het `api-key` kop.


U moet uw eigen naam en api sleutel om het verzoek verzenden hieronder bieden:


    POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/json
    api-key: [api-key]


Voor een succesvolle aanvraag ziet u de statuscode 201 (gemaakt). Ga de API reference op [MSDN](https://msdn.microsoft.com/library/azure/dn798941.aspx)voor meer informatie over het maken van een index met de REST API. Zie voor meer informatie over andere HTTP-statuscodes die kan worden gegeven bij een storing, [HTTP-statuscodes (Azure Search)](https://msdn.microsoft.com/library/azure/dn798925.aspx).

Als u klaar bent met een index wilt verwijderen, maar verwijderen van de HTTP-aanvraag te geven. Dit is bijvoorbeeld hoe we de "hotels" index wilt verwijderen:

    DELETE https://[service name].search.windows.net/indexes/hotels?api-version=2015-02-28
    api-key: [api-key]


## <a name="next"></a>Volgende
Na het maken van een zoekindex Azure, zult u klaar voor het [uploaden van uw inhoud in de index](search-what-is-data-import.md) zodat u kunt beginnen met het zoeken van uw gegevens.
