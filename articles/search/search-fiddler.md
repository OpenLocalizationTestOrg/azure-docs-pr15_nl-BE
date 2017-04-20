<properties
    pageTitle="Het gebruik van Fiddler te evalueren en Azure Search REST API's testen | Microsoft Azure | De zoekservice hosted cloud"
    description="Fiddler voor een code-vrije benadering van Azure zoeken beschikbaarheid controleren en uitproberen van de REST API's gebruiken."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="10/17/2016"
    ms.author="heidist"/>

# <a name="use-fiddler-to-evaluate-and-test-azure-search-rest-apis"></a>Fiddler gebruiken om te evalueren en Azure Search REST API's testen
> [AZURE.SELECTOR]
- [Overzicht](search-query-overview.md)
- [Search Explorer](search-explorer.md)
- [Fiddler](search-fiddler.md)
- [.NET](search-query-dotnet.md)
- [REST](search-query-rest-api.md)

Dit artikel wordt uitgelegd hoe u met Fiddler [gratis kunt downloaden van Telerik](http://www.telerik.com/fiddler), probleem HTTP-aanvragen en antwoorden weergeven met behulp van Azure Search REST API, zonder code te schrijven. Azure Search is volledig beheerde, cloud search-service op Microsoft Azure, gemakkelijk programmeerbaar via .NET en REST API's die worden gehost. De REST API's worden beschreven op [MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx)Azure Search-service.

In de volgende stappen zult u een index maken, documenten uploaden, de index en vervolgens het systeem voor de service-informatie opvragen.

Als u deze stappen hebt uitgevoerd, moet u een zoekservice Azure en `api-key`. Zie [een Azure Search-service in de portal maken](search-create-service-portal.md) voor instructies over hoe u aan de slag.

## <a name="create-an-index"></a>Een index maken

1. Fiddler starten. Uitschakelen **Vastleggen verkeer** naar vreemde HTTP-activiteit die is gerelateerd aan de huidige taak verbergen in het menu **bestand** .

3. Klik op het tabblad **Composer** , zult u een aanvraag die op de volgende schermafdruk lijkt formuleren.

    ![][1]

2. Selecteer **plaatsen**.

3. Voer een URL waarmee de URL van de kenmerken en de api-versie. Een paar zaken rekening houden:
   + Gebruik het prefix HTTPS.
   + Aanvraagkenmerk is "indexen/hotels". Zo weet zoeken in een index met de naam 'hotels' te maken.
   + API versie in kleine letters, wordt opgegeven als '? api versie 2015-02-28 = ". API-versies zijn belangrijk omdat Azure Search updates regelmatig implementeert. In zeldzame gevallen kan een update service een recente wijziging kennismaken met de API. Azure Search vereist daarom een api-versie op elk verzoek dat u hebt volledige controle over welke wordt gebruikt.

    De volledige URL ziet er ongeveer als volgt uitzien.

            https://my-app.search.windows.net/indexes/hotels?api-version=2015-02-28

4.  Geef de verzoek-header, de host en de api-sleutel te vervangen door waarden die geldig voor uw service zijn.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

5.  Plakken in de velden die in de definitie van de index in hoofdtekst aanvragen.
            
             {
            "name": "hotels",  
            "fields": [
              {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
              {"name": "baseRate", "type": "Edm.Double"},
              {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
              {"name": "hotelName", "type": "Edm.String"},
              {"name": "category", "type": "Edm.String"},
              {"name": "tags", "type": "Collection(Edm.String)"},
              {"name": "parkingIncluded", "type": "Edm.Boolean"},
              {"name": "smokingAllowed", "type": "Edm.Boolean"},
              {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
              {"name": "rating", "type": "Edm.Int32"},
              {"name": "location", "type": "Edm.GeographyPoint"}
             ]
            }

6.  Klik op **uitvoeren**.

In een paar seconden verschijnt een 201 HTTP-antwoord in de sessielijst geven dat de index is gemaakt.

Als u HTTP-504, Controleer of de URL HTTPS. Als u HTTP 400- of 404, controleert het hoofdgedeelte van de aanvraag om te controleren of dat er geen fouten zijn opgetreden kopiëren plakken. Een HTTP 403 geeft meestal een probleem met de api-sleutel (een ongeldige sleutel of een probleem syntaxis met hoe de api-sleutel is opgegeven).

## <a name="load-documents"></a>Documenten laden

Klik op het tabblad **Composer** eruit uw aanvraag om documenten te boeken als de volgende. Het hoofdgedeelte van de aanvraag bevat de zoekgegevens voor 4 hotels.

   ![][2]

1. Selecteer de **POST**.

2.  Voer een URL die begint met HTTPS, gevolgd door de URL van uw service, gevolgd door ' < NaamCommunity > /indexes/ docs/index? api versie 2015-02-28 = ". De volledige URL ziet er ongeveer als volgt uitzien.

            https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2015-02-28

3.  Verzoek-Header moet gelijk zijn. Houd er rekening mee dat u de host- en api-sleutel vervangen door waarden die geldig voor uw service zijn.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

4.  De instantie verzoeken bevat vier documenten moeten worden toegevoegd aan de index hotels.

            {
            "value": [
            {
                "@search.action": "upload",
                "hotelId": "1",
                "baseRate": 199.0,
                "description": "Best hotel in town",
                "hotelName": "Fancy Stay",
                "category": "Luxury",
                "tags": ["pool", "view", "wifi", "concierge"],
                "parkingIncluded": false,
                "smokingAllowed": false,
                "lastRenovationDate": "2010-06-27T00:00:00Z",
                "rating": 5,
                "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
              },
              {
                "@search.action": "upload",
                "hotelId": "2",
                "baseRate": 79.99,
                "description": "Cheapest hotel in town",
                "hotelName": "Roach Motel",
                "category": "Budget",
                "tags": ["motel", "budget"],
                "parkingIncluded": true,
                "smokingAllowed": true,
                "lastRenovationDate": "1982-04-28T00:00:00Z",
                "rating": 1,
                "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
              },
              {
                "@search.action": "upload",
                "hotelId": "3",
                "baseRate": 279.99,
                "description": "Surprisingly expensive",
                "hotelName": "Dew Drop Inn",
                "category": "Bed and Breakfast",
                "tags": ["charming", "quaint"],
                "parkingIncluded": true,
                "smokingAllowed": false,
                "lastRenovationDate": null,
                "rating": 4,
                "location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
              },
              {
                "@search.action": "upload",
                "hotelId": "4",
                "baseRate": 220.00,
                "description": "This could be the one",
                "hotelName": "A Hotel for Everyone",
                "category": "Basic hotel",
                "tags": ["pool", "wifi"],
                "parkingIncluded": true,
                "smokingAllowed": false,
                "lastRenovationDate": null,
                "rating": 4,
                "location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
              }
             ]
            }

8.  Klik op **uitvoeren**.

In een paar seconden verschijnt een HTTP 200-antwoord in de sessielijst. Dit betekent dat de documenten zijn gemaakt. Als u een 207, is ten minste één document kan niet worden geüpload. Als u een 404 krijgt, hebt u een syntaxisfout in de koptekst of de hoofdtekst van de aanvraag.

## <a name="query-the-index"></a>De index

Nu dat een index en documenten worden geladen, kunt u query's op deze uitgeven.  Klik op het tabblad **Composer** een **GET** -opdracht query naar de Directory ziet er ongeveer als de volgende schermafdruk.

   ![][3]

1.  Selecteer **ophalen**.

2.  Voer een URL die begint met HTTPS, gevolgd door de URL van uw service, gevolgd door ' < NaamCommunity > /indexes/ / docs? ", gevolgd door de query-parameters. Bij wijze van voorbeeld gebruikt u de volgende URL, vervangen door de naam van de host monster dat geldig is voor uw service.
    
            https://my-app.search.windows.net/indexes/hotels/docs?search=motel&facet=category&facet=rating,values:1|2|3|4|5&api-version=2015-02-28

    Deze query wordt gezocht op de term "motel" en haalt facet categorieën voor beoordelingen.

3.  Verzoek-Header moet gelijk zijn. Houd er rekening mee dat u de host- en api-sleutel vervangen door waarden die geldig voor uw service zijn.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

De reactiecode 200 moet zijn en de reactie uitvoer er ongeveer de volgende schermafdruk.

   ![][4]

De volgende voorbeeldquery wordt uit de [zoekindex bewerking (Azure Search API)](http://msdn.microsoft.com/library/dn798927.aspx) op MSDN. Veel van de query's het voorbeeld in dit onderwerp bevatten spaties zijn niet toegestaan in de Fiddler. Elke ruimte met een + teken voordat plakken in de query string, voordat u de query in de Fiddler vervangen.

**Voordat de spaties worden vervangen:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2015-02-28

**Nadat de spaties worden vervangen door +:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2015-02-28

## <a name="query-the-system"></a>Het systeem opvragen

U kunt ook zoeken op het systeem als u het document geteld en het verbruik van de opslag. Het antwoord retourneert een telling van het aantal documenten en gebruikte ruimte en op het tabblad **Composer** uw aanvraag ziet er ongeveer als volgt.

 ![][5]

1.  Selecteer **ophalen**.

2.  Voer een URL met de URL van uw service, gevolgd door ' / hotels-indexen/stats? api versie 2015-02-28 = ':

            https://my-app.search.windows.net/indexes/hotels/stats?api-version=2015-02-28

3.  Geef de verzoek-header, de host en de api-sleutel te vervangen door waarden die geldig voor uw service zijn.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

4.  Het hoofdgedeelte van de aanvraag leeg laten.

5.  Klik op **uitvoeren**. U ziet een 200 HTTP-statuscode in de sessielijst. Selecteer de post geboekt voor uw opdracht.

6.  Klik op het tabblad **inspecteurs** , klikt u op het tabblad **kop** en selecteer de JSON-indeling. Hier ziet u het document tellen en opslag grootte (in KB).

## <a name="next-steps"></a>Volgende stappen

Zie [de zoekservice op Azure beheren](search-manage.md) voor zonder code benadering van Azure Search worden gebruikt en beheerd.


<!--Image References-->
[1]: ./media/search-fiddler/AzureSearch_Fiddler1_PutIndex.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png
