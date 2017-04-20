<properties
   pageTitle="Een index maken voor documenten in meerdere talen in Azure zoeken | Microsoft Azure | De zoekservice hosted cloud"
   description=" Azure Search ondersteunt 56 talen analyzers taal van Lucene en verwerking van natuurlijke taal technologie van Microsoft optimaal benutten."
   services="search"
   documentationCenter=""
   authors="yahnoosh"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="na"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="07/14/2016"
   ms.author="jlembicz"/>

# <a name="create-an-index-for-documents-in-multiple-languages-in-azure-search"></a>Een index maken voor documenten in meerdere talen in Azure zoeken
> [AZURE.SELECTOR]
- [Portal](search-language-support.md)
- [REST](https://msdn.microsoft.com/library/azure/dn879793.aspx)
- [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.analyzername.aspx)

De kracht van taal analyzers unleashing is net zo eenvoudig als een eigenschap van een veld waarin kan worden gezocht in de definitie van de index. Nu kunt u deze stap in de portal doen.

Hieronder vindt u schermafbeeldingen van de blades Azure Portal voor Azure Search, waarmee gebruikers het schema van een index definiëren. Gebruikers kunnen uit deze blade, alle velden maken en stelt u de eigenschap analyzer voor elk van hen.

> [AZURE.IMPORTANT] U kunt alleen een taal analyzer instellen tijdens de definitie van het veld in als bij het maken van een nieuwe index van de grond omhoog, of wanneer u een nieuw veld toevoegt aan een bestaande index. Zorg ervoor dat u volledig alle kenmerken, zoals de analyzer, tijdens het maken van het veld opgeven. Niet mogelijk de kenmerken bewerken of wijzigen van het type analyzer nadat u de wijzigingen opslaan.

## <a name="define-a-new-field-definition"></a>Een nieuwe definitie van het veld definiëren

1. Aanmelden bij de [Azure Portal](https://portal.azure.com) en open de blade service van de zoekservice.
2. Klik op **index toevoegen** in de werkbalk boven in het servicedashboard te starten van een nieuwe index of open een bestaande index om een analyzer op nieuwe velden die u aan een bestaande index toevoegt.
3. De blade velden wordt weergegeven, zodat u de opties voor het definiëren van het schema van de index, met inbegrip van het tabblad Analyzer gebruikt voor het kiezen van een taal analyzer.
4. In velden, start de definitie van een veld een naam, het gegevenstype te kiezen en instellen van de kenmerken voor het veld markeert als volledige tekst kan worden gezocht, worden opgehaald in de zoekresultaten kan worden gebruikt in de navigatiestructuur facet, gesorteerd, enzovoort. 
5. Voordat u doorgaat naar het volgende veld, open het tabblad **Analyzer** . 

   
![][1]
*Selecteer een analyzer, klikt u op het tabblad Analyzer op de velden blade*

## <a name="choose-an-analyzer"></a>Kies een analyzer

6. Ga naar het veld dat u definieert. 
7. Als u het veld als doorzoekbare niet hebt gemarkeerd, klikt u op het selectievakje nu om te markeren als **Searchable**.
8. Klik op het gebied van Analyzer voor de lijst met beschikbare analyzers.
9. Kies de gewenste analyzer.

![][2]
*Selecteer een van de ondersteunde analyzers voor elk veld*

Alle doorzoekbare velden gebruiken de [standaard Lucene analyzer](http://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) agnostic taal is. De volledige lijst van ondersteunde analyzers, Zie [Taalondersteuning in Azure zoeken](https://msdn.microsoft.com/library/azure/dn879793.aspx).

Zodra de analyzer taal is ingeschakeld voor een veld, wordt deze bij elke aanvraag voor indexeren en zoeken voor dat veld gebruikt. Wanneer een query met meerdere velden met verschillende analyzers wordt afgegeven, wordt de query onafhankelijk verwerkt door de juiste analyzers voor elk veld.

Veel web en mobiele toepassingen dienen gebruikers over de hele wereld met verschillende talen. Het is mogelijk om een index definiëren voor een scenario zoals dit door het maken van een veld voor elke ondersteunde taal.

![][3]
*Definitie van de index met het beschrijvingsveld voor elke ondersteunde taal*

Als de taal van de agent een query uitvoert bekend is, een zoekopdracht kan worden binnen het bereik van een bepaald veld met behulp van de query-parameter **searchFields** . De volgende query wordt uitgegeven alleen tegen de omschrijving in het Pools:

`https://[service name].search.windows.net/indexes/[index name]/docs?search=darmowy&searchFields=description_pl&api-version=2015-02-28`

U kunt de index van de portal vragen via **Search explorer** te plakken in een vergelijkbaar met de bovenstaande query. Search explorer is beschikbaar op de opdrachtbalk in de blade service. Zie [Query de Azure-zoekindex in de portal](search-explorer.md) voor meer informatie.

Ook de taal van de agent een query uitvoert niet bekend is, in welk geval de query kan worden uitgegeven tegen alle velden tegelijk. Indien nodig, kan voorkeur voor de resultaten in een bepaalde taal worden gedefinieerd met [het scoren van de profielen](https://msdn.microsoft.com/library/azure/dn798928.aspx). In het volgende voorbeeld resultaten gevonden in de beschrijving in het Engels de score hoger ten opzichte van de overeenkomsten in het Pools en in het Frans:

    "scoringProfiles": [
      {
        "name": "englishFirst",
        "text": {
          "weights": { "description_en": 2 }
        }
      }
    ]

`https://[service name].search.windows.net/indexes/[index name]/docs?search=Microsoft&scoringProfile=englishFirst&api-version=2015-02-28`

Als u een .NET-ontwikkelaar bent, houd er rekening mee dat u de taal analyzers met de [Azure Search.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Search)kunt configureren. De meest recente versie bevat ondersteuning voor de Microsoft taal analyzers ook.

<!-- Image References -->
[1]: ./media/search-language-support/AnalyzerTab.png
[2]: ./media/search-language-support/SelectAnalyzer.png
[3]: ./media/search-language-support/IndexDefinition.png
