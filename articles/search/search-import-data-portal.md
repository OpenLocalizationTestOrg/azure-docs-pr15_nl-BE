<properties
    pageTitle="Gegevens importeren in Azure zoeken met behulp van indexeerfuncties in de Portal Azure | Microsoft Azure | De zoekservice hosted cloud"
    description="De Azure Search Wizard importeren gebruiken in de Portal Azure verkenning gegevens van Azure Blob storage, tabel stroage SQL-Database en SQL Server op Azure VMs."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="Azure Portal"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="heidist"/>

# <a name="import-data-to-azure-search-using-the-portal"></a>Gegevens importeren in Azure zoeken met behulp van de portal

De Azure portal biedt een wizard **Gegevens importeren** in het dashboard Azure zoeken voor het laden van gegevens in een index. 

  ![Gegevens importeren in de werkbalk][1]

Intern, de wizard configureert en roept een *indexeerfunctie*verschillende stappen van het indexeringsproces automatiseren: 

- Verbinding maken met een externe gegevensbron in het huidige abonnement Azure
- Automatisch genereren van een index schema op basis van de structuur van de gegevensbron gegevens
- Documenten die zijn gebaseerd op een rijenset die wordt opgehaald uit de gegevensbron maken
- Documenten uploaden naar de index in de search-service

U kunt proberen deze werkstroom met voorbeeldgegevens in DocumentDB. Bezoek [aan de slag met Azure zoeken in de Portal Azure](search-get-started-portal.md) voor instructies.

## <a name="data-sources-supported-by-the-import-data-wizard"></a>Gegevensbronnen die worden ondersteund door de Wizard gegevens importeren

De wizard gegevens importeren ondersteunt de volgende gegevensbronnen: 

- Azure SQL-Database
- Relationele SQL Server-gegevens op een Azure VM
- Azure DocumentDB
- Azure Blob-opslag (in het voorbeeld)
- Azure tabelopslag (in het voorbeeld)

Een afgevlakte dataset is een verplicht input. U kunt alleen importeren uit één tabel, databaseweergave of gelijkwaardige gegevensstructuur. Voordat u de wizard uitvoert, moet u de gegevensstructuur van deze maken.

Houd er rekening mee dat een paar van de indexeerfuncties zijn nog steeds in het voorbeeld, wat betekent dat de definitie van de indexeerfunctie wordt gesteund door de preview-versie van de API. Zie [voor indexering-overzicht](search-indexer-overview.md) voor meer informatie en koppelingen.

## <a name="connect-to-your-data"></a>Verbinding maken met uw gegevens

1. Log in om de [Azure Portal](https://portal.azure.com) en het dashboard openen. **Services zoeken** kunt u klikken op de balk sprong bestaande services weergeven in het huidige abonnement. 

2. Klik op **Gegevens importeren** in de werkbalk te openen dia het blad gegevens importeren.  

3. Klik op **verbinding maken met uw gegevens** om aan te geven van een definitie van de gegevensbron die wordt gebruikt door de indexeerfunctie. Voor gegevensbronnen van intra-abonnement, de wizard meestal detecteert en verbindingsgegevens, lezen slechts weinig algemene configuratie nodig.

| | |
|--------|------------|
|**Bestaande gegevensbron** | Als u al indexeerfuncties gedefinieerd in de search-service, kunt u een bestaande definitie van de gegevensbron voor een andere importeren.|
|**Azure SQL-Database** | Referenties voor een databasegebruiker met leesmachtiging servicenaam en naam van een database kan worden opgegeven op de pagina of via een verbindingsreeks ADO.NET. Kies de optie verbinding tekenreeks weergeven of aanpassen van eigenschappen. <br/><br/>De tabel of weergave waarmee de rijenset moet worden opgegeven op de pagina. Deze optie wordt weergegeven nadat de verbinding is geslaagd, waardoor een vervolgkeuzelijst zodat u kunt een selectie maken.|
|**SQL Server op Azure VM** | Een FQDN-naam, gebruikersnaam en wachtwoord en database als een verbindingsreeks opgeven. Deze als gegevensbron wilt gebruiken, moet u hebt eerder geïnstalleerd certificaat in het lokale archief dat de verbinding wordt gecodeerd. <br/><br/>De tabel of weergave waarmee de rijenset moet worden opgegeven op de pagina. Deze optie wordt weergegeven nadat de verbinding is geslaagd, waardoor een vervolgkeuzelijst zodat u kunt een selectie maken.
|**DocumentDB** |Deze vereisten zijn voor de rekening, de database en de collectie. Alle documenten in de collectie worden opgenomen in de index. Hier kunt u een query wilt afvlakken of de rijenset filteren of voor het detecteren van gewijzigde documenten voor latere vernieuwingsbewerkingen uitvoeren.|
|**Azure Blob-opslag** | Deze vereisten zijn voor de opslag en een container. Eventueel als blob namen een virtuele naamgevingsconventie voor groeperen volgt, kunt u het gedeelte van de virtuele map met de naam als een map onder de container. Zie [Indexing Blob Storage (voorbeeld)](search-howto-indexing-azure-blob-storage.md) voor meer informatie. |
|**Azure tabelopslag** | Deze vereisten zijn voor de opslag en de naam van een tabel. Desgewenst kunt u een query voor het ophalen van een subset van de tabellen. Zie [Tabelopslag indexeren (voorbeeld)](search-howto-indexing-azure-tables.md) voor meer informatie. |

## <a name="customize-target-index"></a>Index doel aanpassen

Een voorlopige index is meestal afgeleid van de dataset. Toevoegen, bewerken of verwijderen van velden om het schema. Bovendien kenmerken instellen op het veldniveau van het om te bepalen het gedrag van de volgende zoeken.

1. Geef de naam en een **sleutel** die wordt gebruikt als unieke identificatie voor elk document in **de doel-index aanpassen**. De sleutel moet een tekenreeks zijn. Als de waarden in het veld geen spaties of streepjes moet u geavanceerde opties instellen in **uw gegevens importeren** in de validatiecontrole voor deze tekens te onderdrukken.

2. Bekijken en bewerken van de resterende velden. De naam en het type zijn gewoonlijk ingevuld. U kunt het gegevenstype wijzigen.

3. Een kenmerk indexeren voor elk veld instellen:

 - Opvraagbare wordt het veld in de zoekresultaten.
 - Filterbaar kan het veld worden verwezen in filterexpressies.
 - Sorteerbare kunt u het veld in een sorteervolgorde wordt gebruikt.
 - Facetable kunnen het veld voor navigatie beperkt.
 - Zoeken in volledige tekst doorzoekbaar maakt.
  
4. Als u wilt opgeven een analyzer taal op het veldniveau van het, klikt u op het tabblad **Analyzer** . Op dit moment kunnen alleen taal analyzers worden opgegeven. Met behulp van een aangepaste analyzer of een niet-taal analyzer als sleutelwoord, patroon, enzovoort, wordt code nodig.

 - Klik op **Searchable** om te wijzen op het veld zoeken in volledige tekst en de vervolgkeuzelijst Analyzer inschakelen.
 - Kies de gewenste analyzer. Zie [een index voor documenten in meerdere talen maken](search-language-support.md) voor meer informatie.

5. Klik op de **Suggester** als u suggesties voor de type-ahead query op geselecteerde velden.


## <a name="import-your-data"></a>Uw gegevens importeren

1. In **uw gegevens importeren**, Geef een naam voor de indexeerfunctie. Intrekken dat het product van de wizard gegevens importeren een indexeerfunctie is. Later, als u wilt bekijken of te bewerken, kiest u deze vanuit de portal niet door de wizard opnieuw uit te. 

2. Geef de planning die is gebaseerd op de regionale tijdzone waarin de service is ingericht.

3. Stel opties in geavanceerde drempels om op te geven of verkennen kan worden voortgezet als een document wordt verwijderd. Bovendien kunt u opgeven of **sleutel** velden mogen spaties en streepjes.  

## <a name="edit-an-existing-indexer"></a>Een bestaande indexeerfunctie bewerken

Dubbelklik in het servicedashboard op de tegel indexeerfunctie schuift u een lijst met alle indexeerfuncties gemaakt voor uw abonnement. Dubbelklik op een van de indexeerfuncties uitvoeren, bewerken of verwijderen. U kunt de index te vervangen door een andere bestaande gegevensbron wijzigen en opties instellen voor de drempelwaarden fout tijdens het indexeren.

## <a name="edit-an-existing-index"></a>Bewerken van een bestaande index

Structurele updates aan een index moet in Azure zoeken, het opbouwen van deze index, die bestaat uit het verwijderen van de index, de index opnieuw maken en laden van gegevens. Structurele updates omvatten het wijzigen van gegevenstypen en naam wijzigen of verwijderen van een veld.

Bewerkingen uitvoeren waarvoor het opbouwen niet omvatten het toevoegen van een nieuw veld score profielen, suggesters wijzigen of het wijzigen van de taal analyzers wijzigen. Zie de [Index bijwerken](https://msdn.microsoft.com/library/azure/dn800964.aspx) voor meer informatie.

## <a name="next-step"></a>Volgende stap

Bekijk deze koppelingen voor meer informatie over indexeerfuncties:

- [Azure SQL-Database indexeren](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [Indexering van DocumentDB](../documentdb/documentdb-search-indexer.md)
- [Indexering van Blob-opslag (voorbeeld)](search-howto-indexing-azure-blob-storage.md)
- [Indexeren tabelopslag (voorbeeld)](search-howto-indexing-azure-tables.md)



<!--Image references-->
[1]: ./media/search-import-data-portal/search-import-data-command.png

