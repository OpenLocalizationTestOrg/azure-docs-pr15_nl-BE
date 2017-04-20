<properties
    pageTitle="Het uploaden van gegevens in Azure zoeken | Microsoft Azure | De zoekservice hosted cloud"
    description="Informatie over het uploaden van gegevens naar een index in Azure zoeken."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="upload-data-to-azure-search"></a>Gegevens uploaden naar Azure zoeken
> [AZURE.SELECTOR]
- [Overzicht](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [REST](search-import-data-rest-api.md)


## <a name="data-upload-models-in-azure-search"></a>Gegevens uploaden modellen in Azure zoeken
Er zijn twee manieren voor het vullen van uw Azure Search-index met uw gegevens. De eerste optie pusht uw gegevens handmatig in de index met de Azure Search [REST API](search-import-data-rest-api.md) of [.NET SDK](search-import-data-dotnet.md). De tweede optie is [een ondersteunde gegevensbron wijst](search-indexer-overview.md) naar de zoekindex Azure en Azure Search kunt u automatisch gegevens in de search-service laten.

Deze handleiding wordt alleen instructies over het gebruik van het model push van gegevens uploaden (dit wordt alleen ondersteund in de [REST API](search-import-data-rest-api.md) en de [SDK voor .NET](search-import-data-dotnet.md)), en u kunt nog meer informatie over het onderstaande pull-model.

### <a name="push-data-to-an-index"></a>Push-gegevens naar een index

Deze benadering wordt verwezen naar uw gegevens via programmacode worden verzonden naar Azure zoeken zodat deze beschikbaar is voor het zoeken. Voor toepassingen met zeer lage latentie eisen (bv. Als u moet bewerkingen worden gesynchroniseerd met dynamische voorraad-databases), de push-model is de enige optie.

U kunt de [REST API](https://msdn.microsoft.com/library/azure/dn798930.aspx) of [.NET SDK](search-import-data-dotnet.md) push-gegevens aan een index. Er is momenteel geen ondersteuning voor hulpprogramma voor gegevens via de portal.

Deze methode is flexibeler dan het pull-model omdat u documenten, afzonderlijk of in batches uploaden kunt (maximaal 1000 per batch of 16 MB, ongeacht welke limiet bovenaan). De push-model kunt u ook documenten uploaden naar Azure zoeken waar de gegevens zich ook bevindt.

### <a name="pull-data-into-an-index"></a>Gegevens op te halen in een index

Het pull-model een ondersteunde gegevensbron verkent en uploadt u de gegevens automatisch naar u Azure zoekindex voor u. Indexeerfuncties verwijderen door het bijhouden van wijzigingen en verwijderingen in bestaande documenten naast het herkennen van nieuwe documenten, moeten de gegevens in de index actief te beheren.

Deze mogelijkheid is geïmplementeerd in Azure zoeken via *indexeerfuncties*, die momenteel beschikbaar voor [Blob-opslag (voorbeeld)](search-howto-indexing-azure-blob-storage.md), [DocumentDB](http://aka.ms/documentdb-search-indexer), [Azure SQL-database en SQL Server op Azure VMs](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md).

De functionaliteit voor indexering is in [Azure Portal](search-import-data-portal.md) ook in de [REST API](https://msdn.microsoft.com/library/azure/dn946891.aspx)blootgesteld.
