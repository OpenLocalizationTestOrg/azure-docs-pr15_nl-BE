<properties
    pageTitle="Het model van complexe gegevenstypen in Azure zoeken | Microsoft Azure zoeken"
    description="Geneste of hiërarchische gegevensstructuren kunnen worden gemodelleerd in een afgevlakte rijenset en gegevenstype collecties met Azure Search-index."
    services="search"
    documentationCenter=""
    authors="LiamCa"
    manager="pablocas"
    editor=""
    tags="complex data types; compound data types; aggregate data types"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="09/07/2016"
    ms.author="liamca"
/>

# <a name="how-to-model-complex-data-types-in-azure-search"></a>Het model complexe gegevenstypen in Azure zoeken

Externe datasets gebruikt voor het vullen van een zoekindex Azure soms zijn hiërarchische of geneste substructuren die niet netjes in een rijenset in tabelvorm gaat splitsen. Voorbeelden van dergelijke structuren kunnen meerdere locaties en telefoonnummers voor één klant, meerdere kleuren en grootten bevatten voor een afzonderlijke SKU, meerdere auteurs van één boek, enzovoort. Modelleren van termen, ziet u deze structuren aangeduid als *complexe gegevenstypen*, *samengestelde gegevenstypen*, *samengestelde gegevenstypen*of *statistische gegevenstypen*, om een paar te noemen.

Complexe gegevenstypen worden niet ondersteund in Azure zoeken, maar een beproefde oplossing omvat twee stappen van het afvlakken van de structuur en vervolgens een type **verzameling** gegevens te reconstrueren aan de inwendige constructie. Na de in dit artikel beschreven methode kan de inhoud moet worden gezocht, beperkt, gefilterd en gesorteerd.

## <a name="example-of-a-complex-data-structure"></a>Voorbeeld van een complexe structuur

De betrokken gegevens bevinden zich meestal voor als een set van JSON- of XML-documenten of items in een archief NoSQL zoals DocumentDB. Structureel, de uitdaging komen voort uit met meerdere onderliggende items die moeten worden doorzocht en gefilterd.  Als uitgangspunt voor het illustreren van de tijdelijke oplossing, neemt u de volgende JSON-document waarin een reeks contactpersonen als voorbeeld:

~~~~~
[
  {
    "id": "1",
    "name": "John Smith",
    "company": "Adventureworks",
    "locations": [
      {
        "id": "1",
        "description": "Adventureworks Headquarters"
      },
      {
        "id": "2",
        "description": "Home Office"
      }
    ]
  }, 
  {
    "id": "2",
    "name": "Jen Campbell",
    "company": "Northwind",
    "locations": [
      {
        "id": "3",
        "description": "Northwind Headquarter"
      },
      {
        "id": "4",
        "description": "Home Office"
      }
    ]
}]
~~~~~

Terwijl u de velden met de naam 'id', 'naam' en 'bedrijf' kunnen gemakkelijk worden toegewezen als velden binnen een zoekindex Azure-op-een, het veld 'locaties' bevat een matrix van locaties, met een set locatie-id's, alsmede beschrijvingen van de locatie. Gezien het feit dat Azure Search geen een gegevenstype dat wordt ondersteund, moeten we een andere manier om dit model in Azure zoeken. 

> [AZURE.NOTE] Deze techniek wordt ook beschreven door Kirk Evans in een weblogbericht [DocumentDB met Azure Search Indexing](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), waaruit blijkt dat een techniek genaamd 'afvlakken de gegevens', waarbij u een veld met de naam moet `locationsID` en `locationsDescription` die zijn beide [verzamelingen](https://msdn.microsoft.com/library/azure/dn798938.aspx) (of een matrix van tekenreeksen).   

## <a name="part-1-flatten-the-array-into-individual-fields"></a>Deel 1: Het afvlakken van de matrix in afzonderlijke velden

Een Azure Search als index wilt maken die geschikt is voor deze dataset, afzonderlijke velden maken voor de geneste substructure: `locationsID` en `locationsDescription` met het gegevenstype van [verzamelingen](https://msdn.microsoft.com/library/azure/dn798938.aspx) (of een matrix van tekenreeksen). U zou in deze velden de waarden '1' en '2' index naar de `locationsID` veld voor John Smith en de waarden '3' en '4' in de `locationsID` -veld voor Jen Campbell.  

Uw gegevens in Azure Search eruit als volgt: 

![voorbeeldgegevens, 2 rijen](./media/search-howto-complex-data-types/sample-data.png)


## <a name="part-2-add-a-collection-field-in-the-index-definition"></a>Deel 2: Een verzameling veld toevoegen in de definitie van de index

In het schema van de index uitzien de velddefinities zoals in dit voorbeeld.

~~~~
var index = new Index()
{
    Name = indexName,
    Fields = new[]
    {
        new Field("id", DataType.String) { IsKey = true },
        new Field("name", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("company", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("locationsId", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("locationsDescription", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true }
    }
};
~~~~

## <a name="validate-search-behaviors-and-optionally-extend-the-index"></a>Zoek gedrag valideren en eventueel uitbreiden van de index

Als u de index gemaakt en de gegevens wordt geladen, kunt u nu testen de oplossing om te controleren of de uitvoering van de zoekopdracht query tegen de dataset. Elk veld **collectie** moet worden **doorzocht**, **Filterbaar** en **facetable**. U moet mogelijk zijn voor het uitvoeren van query's als volgt:

* Alle mensen die op de Adventureworks zetel werken vinden.
* Krijg een telling van het aantal mensen die in een thuiskantoor werken.  
* Weergeven van de mensen die op een thuiskantoor werken, welke andere kantoren ze samen met een aantal van de mensen in elke locatie werken.  

Indien deze techniek uit elkaar valt is als u wilt dat een combinatie van zowel de locatie-id als de beschrijving van de locatie. Bijvoorbeeld:

* Alle mensen vinden waar ze hebben een kantoor aan huis en heeft een locatie-ID 4.  

Als u de oorspronkelijke inhoud wordt gezien als volgt:

~~~~
   {
        id: '4',
        description: 'Home Office'
   }
~~~~

Nu dat we de gegevens in afzonderlijke velden hebt verdeeld, we hebben echter niet mogelijk omdat u weet dat als het kantoor aan huis voor Jen Campbell betrekking heeft op `locationsID 3` of `locationsID 4`.  

Definiëren om in dit geval een ander veld in de index, die alle gegevens worden gecombineerd in één collectie.  In ons voorbeeld noemen we dit veld `locationsCombined` en wij zullen scheiden de inhoud met een `||` maar u kunt een scheidingsteken die u denkt dat een unieke reeks tekens voor uw inhoud zou zijn. Bijvoorbeeld: 

![voorbeeldgegevens, 2 rijen met scheidingsteken](./media/search-howto-complex-data-types/sample-data-2.png)

Met deze `locationsCombined` veld, we kunnen nu worden aangepast aan nog meer query's, zoals:

* Een aantal mensen die op een 'Home Office"met locatie-Id van de '4 werken' weergeven.  
* Zoeken naar personen die bij een thuiskantoor werken met locatie-Id '4'. 

## <a name="limitations"></a>Beperkingen

Deze techniek is handig voor een aantal scenario's, maar het is niet van toepassing in alle gevallen.  Bijvoorbeeld:

1. Als er geen een statische set velden in uw complex gegevenstype en er geen manier is om de mogelijke typen toewijzen aan één veld. 
2. Geneste objecten bijwerken is wat extra werk om te bepalen wat moet precies worden bijgewerkt in de zoekindex Azure

## <a name="sample-code"></a>Voorbeeld van code

U ziet een voorbeeld voor het indexeren van een complexe JSON-gegevensset in Azure Search en een aantal query's uitvoeren via deze dataset op deze [repo GitHub](https://github.com/liamca/AzureSearchComplexTypes).

## <a name="next-step"></a>Volgende stap

[Stem voor native ondersteuning voor complexe gegevenstypen](https://feedback.azure.com/forums/263029-azure-search) op de UserVoice Azure Search pagina en geen extra gebruikersinvoer die u wenst te houden met betrekking tot uitvoering van de functie. U kunt ook bereiken mij direct op Twitter op @liamca.


 