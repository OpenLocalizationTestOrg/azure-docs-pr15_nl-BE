<properties 
    pageTitle="Quick start guide: Machine Learning aanbevelingen API | Microsoft Azure" 
    description="Azure Machine Learning aanbevelingen - Quick Start Guide" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/>

# <a name="quick-start-guide-for-the-machine-learning-recommendations-api"></a>De slag voor de Machine Learning aanbevelingen API

>[AZURE.NOTE]U moet beginnen met de cognitieve aanbevelingen API-Service in plaats van deze versie. De cognitieve Service aanbevelingen zal deze service worden vervangen en de nieuwe functies zal er worden ontwikkeld. Het heeft nieuwe mogelijkheden, zoals batchen ondersteuning, een betere API Explorer, een duidelijkere API oppervlak, consistenter aanmelden/facturering ervaring, enz.
> Meer informatie over het [migreren naar de nieuwe cognitieve Service](http://aka.ms/recomigrate)


Dit document wordt beschreven hoe aan boord de service of toepassing naar Microsoft Azure Machine Learning aanbevelingen gebruiken. Meer informatie kunt over de API aanbevelingen u vinden in de [Galerie](http://gallery.cortanaanalytics.com/MachineLearningAPI/Recommendations-2).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="general-overview"></a>Algemeen overzicht

Azure Machine Learning aanbevelingen gebruiken, moet u de volgende stappen ondernemen:

* Maak een model - een model is een container van de gebruiksgegevens, gegevens in de catalogus en het model van de aanbeveling.
* Gegevens importeren - een catalogus bevat metagegevens gegevens over de artikelen. 
* Gebruiksgegevens importeren - gebruiksgegevens kan worden geüpload in een van de volgende 2 manieren (of beide):
    * Door het uploaden van een bestand bevat dat de gegevens.
    * Door te sturen gegevens verwerving gebeurtenissen.
    Meestal uploaden u van een bestand gebruik te kunnen maken een eerste aanbeveling model (bootstrap) en daarmee tot het systeem voldoende gegevens worden verzameld met behulp van de gegevensindeling voor de aanschaf.
* Bouwen van een model aanbeveling - dit is een asynchrone bewerking waarin de aanbeveling wordt de gebruiksgegevens en maakt een aanbeveling model. Deze bewerking kan enkele minuten tot enkele uren, afhankelijk van de grootte van de gegevens en de configuratieparameters build nemen. Wanneer de build activeren, krijgt u een build-ID. Gebruiken om te controleren wanneer het buildproces is beëindigd voordat u begint met de aanbevelingen in beslag nemen.
* Aanbevelingen - aanbevelingen voor een bepaald artikel of een lijst met items ophalen in beslag nemen.

De bovenstaande stappen worden via de API Azure Machine Learning aanbevelingen gedaan.  Kunt u een voorbeeldtoepassing waarmee elk van deze stappen van de [ook galerie.](http://1drv.ms/1xeO2F3)

##<a name="limitations"></a>Beperkingen

* Het maximum aantal modellen per abonnement is 10.
* Het maximum aantal items dat een catalogus kan houden is 100.000.
* Het maximum aantal punten gebruik worden bewaard is ~ 5,000,000. De oudste wordt verwijderd als u nieuwe bestanden worden geüpload of gerapporteerd.
* De maximale grootte van gegevens die kunnen worden verzonden in boeken (b.v. catalogusgegevens importeren, gegevens importeren) is 200MB.
* Het aantal transacties per seconde voor een aanbeveling model bouwen die niet actief is ~ 2TPS. Een aanbeveling model build actieve kunt opgewassen zijn tegen 20TPS.

##<a name="integration"></a>Integratie

###<a name="authentication"></a>Verificatie
Gepland in Microsoft Azure Marketplace ondersteunt de Basic- of OAuth verificatiemethode. U kunt de sleutels rekening gemakkelijk vinden door te gaan met de toetsen op de markt onder [uw accountinstellingen](https://datamarket.azure.com/account/keys). 
####<a name="basic-authentication"></a>Basisverificatie
De autorisatie-header toevoegen:

    Authorization: Basic <creds>
               
    Where <creds> = ConvertToBase64("AccountKey:" + yourAccountKey);  
    
Converteren naar Base64 (C#)

    var bytes = Encoding.UTF8.GetBytes("AccountKey:" + yourAccountKey);
    var creds = Convert.ToBase64String(bytes);
    
Converteren naar Base64 (JavaScript)

    var creds = window.btoa("AccountKey" + ":" + yourAccountKey);
    



###<a name="service-uri"></a>Service-URI
De hoofdmap service URI voor de API's van Azure Machine Learning aanbevelingen wordt [hier.](https://api.datamarket.azure.com/amla/recommendations/v2/)

De volledige service-URI wordt uitgedrukt met behulp van elementen van de OData-specificatie.

###<a name="api-version"></a>API-versie
De API-aanroep heeft aan het einde wordt een queryparameter apiVersion die moet worden ingesteld op "1.0" genoemd.

###<a name="ids-are-case-sensitive"></a>Id's zijn hoofdlettergevoelig
Id's, die wordt geretourneerd door een van de API's, zijn hoofdlettergevoelig en moeten als zodanig worden gebruikt als als parameters doorgegeven in de volgende API-aanroepen. Model-id's en catalogus-id's zijn bijvoorbeeld hoofdlettergevoelig.

###<a name="create-a-model"></a>Een model maken
Maken van een aanvraag voor een 'model maken':

| HTTP-methode | URI |
|:--------|:--------|
|Verzenden     |`<rootURI>/CreateModel?modelName=%27<model_name>%27&apiVersion=%271.0%27`<br><br>Voorbeeld:<br>`<rootURI>/CreateModel?modelName=%27MyFirstModel%27&apiVersion=%271.0%27`|

|   Naam van de parameter  |   Geldige waarden                        |
|:--------          |:--------                              |
|   modelName   |   Alleen letters (A-Z, a-z), cijfers (0-9), koppeltekens (-) en het onderstrepingsteken (_) zijn toegestaan.<br>Maximale lengte: 20 |
|   apiVersion      | 1.0 |
|||
| Hoofdgedeelte van de aanvraag | GEEN |


**Antwoord**:

HTTP-statuscode: 200

- `feed/entry/content/properties/id`-Het model-id bevat.
**Opmerking**: model-ID is hoofdlettergevoelig.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">a658c626-2baa-43a7-ac98-f6ee26120a12</d:Id>
        <d:Name m:type="Edm.String">MyFirstModel</d:Name>
        <d:Date m:type="Edm.String">10/5/2014 6:35:19 AM</d:Date>
        <d:Status m:type="Edm.String">Created</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:UserName>
        <d:Description m:type="Edm.String"></d:Description>
      </m:properties>
    </content>
      </entry>
    </feed>


###<a name="import-catalog-data"></a>Importeren van gegevens in de catalogus

Als u verschillende bestanden naar hetzelfde model met verschillende aanroepen uploaden, zullen we de nieuwe catalogusitems invoegen. Bestaande items blijft met de oorspronkelijke waarden.

| HTTP-methode | URI |
|:--------|:--------|
|Verzenden     |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Voorbeeld:<br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|

|   Naam van de parameter  |   Geldige waarden                        |
|:--------          |:--------                              |
|   Model |   De unieke id van het model (hoofdlettergevoelig)  |
| bestandsnaam | Tekstuele id van de catalogus.<br>Alleen letters (A-Z, a-z), cijfers (0-9), koppeltekens (-) en het onderstrepingsteken (_) zijn toegestaan.<br>Maximale lengte: 50 |
|   apiVersion      | 1.0 |
|||
| Hoofdgedeelte van de aanvraag | Catalogus gegevens. Indeling:<br>`<Item Id>,<Item Name>,<Item Category>[,<description>]`<br><br><table><tr><th>Naam</th><th>Verplicht</th><th>Type</th><th>Beschrijving</th></tr><tr><td>Item-Id</td><td>Ja</td><td>Alfanumerieke, maximale lengte 50</td><td>De unieke id van een item</td></tr><tr><td>Naam van item</td><td>Ja</td><td>Alfanumerieke, maximale lengte van 255</td><td>Naam van item</td></tr><tr><td>Artikelcategorie</td><td>Ja</td><td>Alfanumerieke, maximale lengte van 255</td><td>Categorie waartoe dit artikel behoort (bijvoorbeeld koken boeken, Drama...)</td></tr><tr><td>Beschrijving</td><td>Nee</td><td>Alfanumerieke, maximale lengte van 4000</td><td>Beschrijving van dit item</td></tr></table><br>Maximale bestandsgrootte is 200MB.<br><br>Voorbeeld:<br><pre>2406e770-769c-4189-89de-1c9283f93a96, Clara Callan, boek<br>21bf8088-b6c0-4509-870c-e1c7ac78304a, de kamer wel: een fictie (Byzantium boek), boek<br>3bb5cb44-d143-4bdd-a55c-443964bf4b23, Spadework, boek<br>552a1940-21e4-4399-82bb-594b46d7ed54, beperking van beesten, boek</pre> |


**Antwoord**:

HTTP-statuscode: 200

- `Feed\entry\content\properties\LineCount`-Het aantal regels aanvaard.
- `Feed\entry\content\properties\ErrorCount`-Het aantal regels dat niet is ingevoegd door een fout.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
        <subtitle type="text">Import catalog file</subtitle>
        <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">ImportCatalogFileEntity2</title>
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">4</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
      </m:properties>
    </content>
     </entry>
    </feed>


###<a name="import-usage-data"></a>De gebruiksgegevens importeren

####<a name="uploading-a-file"></a>Uploaden van een bestand
In dit gedeelte ziet u hoe u gebruiksgegevens met behulp van een bestand te uploaden. U kunt deze API is verschillende malen met gebruiksgegevens aanroepen. Alle gegevens worden opgeslagen voor alle gesprekken.

| HTTP-methode | URI |
|:--------|:--------|
|Verzenden     |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Voorbeeld:<br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27`|

|   Naam van de parameter  |   Geldige waarden                        |
|:--------          |:--------                              |
|   Model |   De unieke id van het model (hoofdlettergevoelig) |
| bestandsnaam | Tekstuele id van de catalogus.<br>Alleen letters (A-Z, a-z), cijfers (0-9), koppeltekens (-) en het onderstrepingsteken (_) zijn toegestaan.<br>Maximale lengte: 50 |
|   apiVersion      | 1.0 |
|||
| Hoofdgedeelte van de aanvraag | Gegevens over het gebruik. Indeling:<br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th>Naam</th><th>Verplicht</th><th>Type</th><th>Beschrijving</th></tr><tr><td>Gebruikers-Id</td><td>Ja</td><td>Alfanumerieke</td><td>De unieke id van een gebruiker</td></tr><tr><td>Item-Id</td><td>Ja</td><td>Alfanumerieke, maximale lengte 50</td><td>De unieke id van een item</td></tr><tr><td>Tijd</td><td>Nee</td><td>Datum in de notatie: jjjj/MM/ddTUU (b.v. 2013/06/20T10:00:00)</td><td>Tijd van gegevens</td></tr><tr><td>Gebeurtenis</td><td>Nee, als geleverd en vervolgens de datum ook plaatsen moet.</td><td>Een van de volgende handelingen uit:<br>• Klik op<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Inkoop</td><td></td></tr></table><br>Maximale bestandsgrootte is 200MB.<br><br>Voorbeeld:<br><pre>149452, 1b3d95e2-84e4-c 414-bb38-be9cf461c347<br>6360, 1b3d95e2-84e4-c 414-bb38-be9cf461c347<br>50321, 1b3d95e2-84e4-c 414-bb38-be9cf461c347<br>71285, 1b3d95e2-84e4-c 414-bb38-be9cf461c347<br>224450, 1b3d95e2-84e4-c 414-bb38-be9cf461c347<br>236645, 1b3d95e2-84e4-c 414-bb38-be9cf461c347<br>107951, 1b3d95e2-84e4-c 414-bb38-be9cf461c347</pre> |

**Antwoord**:

HTTP-statuscode: 200

- `Feed\entry\content\properties\LineCount`-Het aantal regels aanvaard.
- `Feed\entry\content\properties\ErrorCount`-Het aantal regels dat niet is ingevoegd door een fout.
- `Feed\entry\content\properties\FileId`-Id-bestand.


OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Add bulk usage data (usage file)</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
    </entry>
    </feed>


####<a name="using-data-acquisition"></a>Overname van gegevens gebruiken
In deze sectie wordt aangegeven hoe gebeurtenissen verzenden in real-time Azure Machine Learning aanbevelingen, meestal van uw website.

| HTTP-methode | URI |
|:--------|:--------|
|Verzenden     |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|

|   Naam van de parameter  |   Geldige waarden                        |
|:--------          |:--------                              |
|   apiVersion      | 1.0 |
|||
|Hoofdgedeelte van de aanvraag| Gebeurtenis: gegevens invoeren voor elke gebeurtenis die u wilt verzenden. U moet verzenden voor dezelfde gebruiker of browser sessie dezelfde ID in de sessie-id-veld. (Zie voorbeeld van de hoofdtekst van de gebeurtenis hieronder.)|


- Voorbeeld van de gebeurtenis 'Klik hier':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Voorbeeld van de gebeurtenis 'RecommendationClick':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>RecommendationClick</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Voorbeeld van de gebeurtenis 'AddShopCart':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>AddShopCart</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Voorbeeld van de gebeurtenis 'RemoveShopCart':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>RemoveShopCart</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Voorbeeld van de gebeurtenis 'Inkoop':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
            <Name>Purchase</Name> 
            <PurchaseItems>
            <PurchaseItems>
                <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
                <Count>3</Count>
            </PurchaseItems>
        </PurchaseItems>
        </EventData>
        </EventData>
        </Event>

- Voorbeeld '2 gebeurtenissen op' en 'AddShopCart' te verzenden:

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        <ItemName>itemName</ItemName>
        <ItemDescription>item description</ItemDescription>
        <ItemCategory>category</ItemCategory>
        </EventData>
        <EventData>
        <Name>AddShopCart</Name>
        <ItemId>552A1940-21E4-4399-82BB-594B46D7ED54</ItemId>
        </EventData>
        </EventData>
        </Event>

**Antwoord**: HTTP-statuscode: 200

###<a name="build-a-recommendation-model"></a>Bouwen van een model van de aanbeveling

| HTTP-methode | URI |
|:--------|:--------|
|Verzenden     |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br>Voorbeeld:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27`|

|   Naam van de parameter  |   Geldige waarden                        |
|:--------          |:--------                              |
| Model | De unieke id van het model (hoofdlettergevoelig)  |
| userDescription | Tekstuele id van de catalogus. Houd er rekening mee dat als u spaties gebruikt u deze met 20% in plaats daarvan coderen moet. Zie het bovenstaande voorbeeld.<br>Maximale lengte: 50 |
| apiVersion | 1.0 |
|||
| Hoofdgedeelte van de aanvraag | GEEN |

**Antwoord**:

HTTP-statuscode: 200

Dit is een asynchrone API. Als reactie ontvangt u een ID van de build. Als u wilt weten wanneer de build is beëindigd, de "Get bouwt de Status van een Model" API aanroepen en u deze build-ID in het antwoord te vinden. Opmerking een build kunt nemen van minuten naar uren, afhankelijk van de grootte van de gegevens.

U kunt geen verbruiken aanbevelingen tot het opbouwen beëindigd.

Geldige build status:

- Maken: Model werd gemaakt.
- In de wachtrij – build Model is geactiveerd en er is in de wachtrij geplaatst.
- Gebouw-Model wordt opgebouwd.
- Succes – Build is beëindigd.
- Fout: Build is beëindigd met een storing.
- Geannuleerd – is Build geannuleerd.
- Geannuleerd – Build wordt geannuleerd.


Houd er rekening mee dat de build-ID u onder het volgende pad vinden kunt:`Feed\entry\content\properties\Id`

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Build a Model with RequestBody</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">1000272</d:Id>
        <d:UserName m:type="Edm.String"></d:UserName>
        <d:ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:ModelId>
        <d:ModelName m:type="Edm.String">docTest</d:ModelName>
        <d:Type m:type="Edm.String">Recommendation</d:Type>
        <d:CreationTime m:type="Edm.String">2014-10-05T08:56:31.893</d:CreationTime>
        <d:Progress_BuildId m:type="Edm.String">1000272</d:Progress_BuildId>
        <d:Progress_ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:Progress_ModelId>
        <d:Progress_UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:Progress_UserName>
        <d:Progress_IsExecutionStarted m:type="Edm.String">false</d:Progress_IsExecutionStarted>
        <d:Progress_IsExecutionEnded m:type="Edm.String">false</d:Progress_IsExecutionEnded>
        <d:Progress_Percent m:type="Edm.String">0</d:Progress_Percent>
        <d:Progress_StartTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_StartTime>
        <d:Progress_EndTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_EndTime>
        <d:Progress_UpdateDateUTC m:type="Edm.String"></d:Progress_UpdateDateUTC>
        <d:Status m:type="Edm.String">Queued</d:Status>
        <d:Key1 m:type="Edm.String">UseFeaturesInModel</d:Key1>
        <d:Value1 m:type="Edm.String">False</d:Value1>
      </m:properties>
    </content>
    </entry>
    </feed>

###<a name="get-build-status-of-a-model"></a>Status van de build van een model ophalen

| HTTP-methode | URI |
|:--------|:--------|
|Toevoegen     |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br>Voorbeeld:<br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27`|



|   Naam van de parameter  |   Geldige waarden                        |
|:--------          |:--------                              |
|   Model         |   De unieke id van het model (hoofdlettergevoelig)    |
|   onlyLastBuild   |   Geeft aan of het resultaat van de build-geschiedenis van het model of alleen de status van de meest recente versie. |
|   apiVersion      |   1.0                                 |


**Antwoord**:

HTTP-statuscode: 200

Het antwoord bevat één post per build. Elke vermelding bevat de volgende gegevens:

- `feed/entry/content/properties/UserName`– De naam van de gebruiker.
- `feed/entry/content/properties/ModelName`– De naam van het model.
- `feed/entry/content/properties/ModelId`– De unieke id van het model.
- `feed/entry/content/properties/IsDeployed`– Of de build (ook geïmplementeerd actieve build).
- `feed/entry/content/properties/BuildId`– De unieke id maken.
- `feed/entry/content/properties/BuildType`-Het type van de bouw.
- `feed/entry/content/properties/Status`– Status maken. Een van de volgende: fout, gebouw, in wachtrij, Cancelling, geannuleerd, succes
- `feed/entry/content/properties/StatusMessage`– Gedetailleerde statusbericht (geldt alleen voor bepaalde lidstaten).
- `feed/entry/content/properties/Progress`– Bouw vooruitgang (%).
- `feed/entry/content/properties/StartTime`: Begintijd build.
- `feed/entry/content/properties/EndTime`– Tijd maken.
- `feed/entry/content/properties/ExecutionTime`– Duur maken.
- `feed/entry/content/properties/ProgressStep`– Informatie over de huidige fase waarin een build bezig is.

Geldige build status:
- Gemaakt – is Build aanvraag post gemaakt.
- In de wachtrij – aanvraag voor het samenstellen is geactiveerd en er is in de wachtrij geplaatst.
- Gebouw – Build wordt verwerkt.
- Succes – Build is beëindigd.
- Fout: Build is beëindigd met een storing.
- Geannuleerd – is Build geannuleerd.
- Geannuleerd – Build wordt geannuleerd.

Geldige waarden voor type build:
- Rang - rang bouwen. (Voor rang maken details, raadpleegt u het document "Machine Learning aanbeveling API documentation").
- Aanbeveling - aanbeveling build.
- FBT - vaak gekocht samen bouwen.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get builds status of a model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetBuildsStatusEntity</title>
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:UserName m:type="Edm.String">b-434e-b2c9-84935664ff20@dm.com</d:UserName>
        <d:ModelName m:type="Edm.String">ModelName</d:ModelName>
        <d:ModelId m:type="Edm.String">1d20c34f-dca1-4eac-8e5d-f299e4e4ad66</d:ModelId>
        <d:IsDeployed m:type="Edm.String">true</d:IsDeployed>
        <d:BuildId m:type="Edm.String">1000272</d:BuildId>
        <d:BuildType m:type="Edm.String">Recommendation</d:BuildType>
        <d:Status m:type="Edm.String">Success</d:Status>
        <d:StatusMessage m:type="Edm.String"></d:StatusMessage>
        <d:Progress m:type="Edm.String">0</d:Progress>
        <d:StartTime m:type="Edm.String">2014-11-02T13:43:51</d:StartTime>
        <d:EndTime m:type="Edm.String">2014-11-02T13:45:10</d:EndTime>
        <d:ExecutionTime m:type="Edm.String">00:01:19</d:ExecutionTime>
        <d:IsExecutionStarted m:type="Edm.String">false</d:IsExecutionStarted>
        <d:ProgressStep m:type="Edm.String"></d:ProgressStep>
      </m:properties>
     </content>
    </entry>
    </feed>


###<a name="get-recommendations"></a>Aanbevelingen ophalen

| HTTP-methode | URI |
|:--------|:--------|
|Toevoegen     |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Voorbeeld:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27`|



|   Naam van de parameter  |   Geldige waarden                        |
|:--------          |:--------                              |
| Model | De unieke id van het model (hoofdlettergevoelig) |
| objectnummers in | Door komma's gescheiden lijst met de artikelen aan te bevelen.<br>Maximale lengte: 1024 |
| numberOfResults | Aantal vereiste resultaten |
| includeMetatadata | Toekomstig gebruik altijd false |
| apiVersion | 1.0 |

**Antwoord:**

HTTP-statuscode: 200

Het antwoord bevat één post per artikel aanbevolen. Elke vermelding bevat de volgende gegevens:

- `Feed\entry\content\properties\Id`-Aanbevolen artikel-ID.
- `Feed\entry\content\properties\Name`-De naam van het item.
- `Feed\entry\content\properties\Rating`-Beoordeling van de aanbeveling; hogere waarde betekent een hogere betrouwbaarheid.
- `Feed\entry\content\properties\Reasoning`-Aanbeveling redeneren (bv. aanbeveling uitleg).

OData-XML

Het onderstaande voorbeeld antwoord bevat 10 aanbevolen items:

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
     <subtitle type="text">Get Recommendation</subtitle>
     <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">159</d:Id>
        <d:Name m:type="Edm.String">159</d:Name>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '159'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">52</d:Id>
        <d:Name m:type="Edm.String">52</d:Name>
        <d:Rating m:type="Edm.Double">0.539588900748721</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '52'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">35</d:Id>
        <d:Name m:type="Edm.String">35</d:Name>
        <d:Rating m:type="Edm.Double">0.53842946443853</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '35'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">124</d:Id>
        <d:Name m:type="Edm.String">124</d:Name>
        <d:Rating m:type="Edm.Double">0.536712832792886</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '124'</d:Reasoning>
      </m:properties>
    </content>
    </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">120</d:Id>
        <d:Name m:type="Edm.String">120</d:Name>
        <d:Rating m:type="Edm.Double">0.533673023762878</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '120'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">96</d:Id>
        <d:Name m:type="Edm.String">96</d:Name>
        <d:Rating m:type="Edm.Double">0.532544826370521</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '96'</d:Reasoning>
      </m:properties>
    </content>
    </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">69</d:Id>
        <d:Name m:type="Edm.String">69</d:Name>
        <d:Rating m:type="Edm.Double">0.531678607847759</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '69'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">172</d:Id>
        <d:Name m:type="Edm.String">172</d:Name>
        <d:Rating m:type="Edm.Double">0.530957821375951</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '172'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">155</d:Id>
        <d:Name m:type="Edm.String">155</d:Name>
        <d:Rating m:type="Edm.Double">0.529093541481333</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '155'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">32</d:Id>
        <d:Name m:type="Edm.String">32</d:Name>
        <d:Rating m:type="Edm.Double">0.528917978168322</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '32'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
    </feed>

###<a name="update-model"></a>Model bijwerken
U kunt bijwerken met de Modelbeschrijving of de actieve versie-ID.
*ID van de build van actieve* - elke build voor elk model heeft een build-ID. De actieve build-ID is de eerste succesvolle build van elk nieuw model. Als u een actieve build-ID hebt en u aanvullende maakt voor hetzelfde model, moet u expliciet instellen als de build-ID als u wilt. Wanneer u aanbevelingen, verbruiken als u de ID van de build die u wilt gebruiken niet opgeeft, wordt automatisch de standaardcode gebruikt.

Dit mechanisme, kunt u - als u eenmaal een aanbeveling model in productie hebt-nieuwe modellen maken en te testen voordat u ze naar productie doorverwijzen.

| HTTP-methode | URI |
|:--------|:--------|
|PLAATSEN     |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br><br>Voorbeeld:<br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27`|


|   Naam van de parameter  |   Geldige waarden                        |
|:--------          |:--------                              |
| ID | De unieke id van het model (hoofdlettergevoelig) |
| apiVersion | 1.0 |
|||
| Hoofdgedeelte van de aanvraag | `<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`   <Description>New Description</Description>`<br>`          <ActiveBuildId>-1</ActiveBuildId>`<br>`</ModelUpdateParams>`<br><br>Houd er rekening mee dat de XML-codes, beschrijving en ActiveBuildId optioneel zijn. Als u niet instellen wilt, beschrijving of ActiveBuildId, de hele code verwijderen. |

**Antwoord**:

HTTP-statuscode: 200

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Update an Existing Model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'</id>
    <rights type="text" />
     <updated>2014-10-05T10:27:17Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'" />
    </feed>

##<a name="legal"></a>Juridische
Dit document wordt geleverd "als-is". Gegevens en inzichten die in dit document, inclusief URL's en andere verwijzingen naar websites, kunnen zonder kennisgeving worden gewijzigd. De hier beschreven voorbeelden dienen enkel ter illustratie en zijn fictief. Er is geen echte koppeling of verbinding is bedoeld of kan hieruit worden afgeleid. Dit document geeft u geen wettelijke rechten op intellectuele eigendom in een Microsoft-product. U kunt kopiëren en dit document gebruiken voor interne referentiedoeleinden. © 2014 Microsoft. Alle rechten voorbehouden. 
 
