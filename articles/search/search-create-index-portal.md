<properties
    pageTitle="Met behulp van de Portal Azure Azure Search index maken | Microsoft Azure | De zoekservice hosted cloud"
    description="Een index met de Azure-Portal maken."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index-using-the-azure-portal"></a>Met behulp van de Portal Azure Azure Search index maken
> [AZURE.SELECTOR]
- [Overzicht](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [REST](search-create-index-rest-api.md)

Dit artikel begeleidt u door het proces van het maken van een Azure Search [index](search-what-is-an-index.md) met behulp van de Portal Azure.

Voordat u deze handleiding te volgen en het maken van een index, hebt u al [een Azure Search-service gemaakt](search-create-service-portal.md).


## <a name="i-go-to-your-azure-search-blade"></a>I. Ga naar de blade Azure zoeken
1. Klik op 'Alle resources' in het menu aan de linkerkant van de [Portal Azure](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)
2. Selecteer uw Azure Search-service

## <a name="ii-add-and-name-your-index"></a>II. Toevoegen en de naam van de index
1. Klik op de knop "Add index"
2. Naam van uw Azure Search-index. Omdat u een index om te zoeken naar hotels in deze handleiding, hebben we onze index "hotels" genoemd.
  * De indexnaam moet beginnen met een letter en bevatten alleen kleine letters, cijfers of streepjes ('-').
  * Net als de servicenaam van uw, de naam van de index die u kiest ook deel uitmaken van de eindpunt-URL waar u stuurt uw HTTP-aanvragen voor de Azure Zoek-API
3. Klik op de vermelding 'Velden' voor het openen van een nieuwe blade

![](./media/search-create-index-portal/add-index.png)


## <a name="iii-create-and-define-the-fields-of-your-index"></a>III. Maken en definiëren van de velden van de index
1. Als u de vermelding 'Velden' selecteert, wordt een nieuwe blade geopend met een formulier voor het invoeren van de definitie van de index.
2. Velden toevoegen aan de index met behulp van het formulier.

  * *Een sleutelveld van het type Edm.String* is verplicht voor elke Azure Search-index. Dit veld is standaard met het veld naam 'id' gemaakt. We hebben deze gewijzigd in 'hotelId' in onze index.
  * Bepaalde eigenschappen van uw schema index kunnen slechts eenmaal worden ingesteld en in de toekomst kunnen niet worden bijgewerkt. Om deze reden zijn de schema-updates die worden opnieuw indexeren moeten zou zoals het wijzigen van veldtypen niet mogelijk na de initiële configuratie.
  * We zorgvuldig gekozen waarden van de eigenschappen voor elk veld op basis van hoe we denken dat ze in een toepassing worden gebruikt. Houd uw zoekopdracht ervaring en zakelijke behoeften van de gebruikers bij het ontwerpen van de index als elk veld de [gewenste eigenschappen](https://msdn.microsoft.com/library/azure/dn798941.aspx)moet worden toegewezen. Deze eigenschappen bepalen welke functies (filteren, faceting, sorteren, zoeken in volledige tekst, enz.) zoeken van toepassing op velden. Het is bijvoorbeeld waarschijnlijk mensen die zoeken naar hotels geïnteresseerd in trefwoord resultaten op het veld 'beschrijving', zodat we voor dat veld zoeken in volledige tekst inschakelen door de eigenschap 'Searchable'.
    * U kunt ook de [taal analyzer](https://msdn.microsoft.com/en-us/library/azure/dn879793.aspx) voor elk veld instellen door te klikken op het tabblad 'Analyzer' aan de bovenkant van het blad. Hieronder te zien dat we een Franse analyzer voor een veld hebt geselecteerd in onze index die bestemd zijn voor de Franse tekst.

3. Klik op **OK** in het blad 'Velden' om te bevestigen de velddefinities
4. Klik op **OK** in het blad "Index toevoegen" op te slaan en de index die u zojuist hebt gedefinieerd.

In de screenshots hieronder ziet u hoe wij hebben met de naam en de velden gedefinieerd voor onze index "hotels".

![](./media/search-create-index-portal/field-definitions.png)

![](./media/search-create-index-portal/set-analyzer.png)

## <a name="next"></a>Volgende
Na het maken van een zoekindex Azure, zult u klaar voor het [uploaden van uw inhoud in de index](search-what-is-data-import.md) zodat u kunt beginnen met het zoeken van uw gegevens.
