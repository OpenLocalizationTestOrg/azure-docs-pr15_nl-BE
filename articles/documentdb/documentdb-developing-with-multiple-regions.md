<properties
   pageTitle="Ontwikkelen met meerdere regio's in DocumentDB | Microsoft Azure"
   description="Informatie over toegang tot de gegevens in meerdere regio's van Azure DocumentDB, een volledig beheerde service voor NoSQL database."
   services="documentdb"
   documentationCenter=""
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="kipandya"/>
   
# <a name="developing-with-multi-region-documentdb-accounts"></a>Ontwikkelen met meerdere regio DocumentDB rekeningen

> [AZURE.NOTE] Wereldwijde distributie van DocumentDB databases in het algemeen beschikbaar is en automatisch ingeschakeld voor alle nieuwe accounts DocumentDB. Wij werken om de wereldwijde distributie van alle bestaande accounts inschakelen, maar in de tussentijd, als u wilt dat de wereldwijde distributie is ingeschakeld op uw account, neem [contact op met support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) en wij gaat instellen voor u nu.

Als u wilt profiteren van [wereldwijde distributie](documentdb-distribute-data-globally.md), kunnen clienttoepassingen de geordende lijst van regio's moet worden gebruikt voor het uitvoeren van document opgeven. Dit kan worden gedaan door het instellen van verbindingsbeleid voor de. Op basis van de configuratie van de account DocumentDB Azure, huidige regionale beschikbaarheid en de opgegeven voorkeur-lijst, wordt de meest optimale eindpunt gekozen door de SDK uit te voeren write leesbewerkingen. 

Deze lijst is opgegeven tijdens het initialiseren van een verbinding met de client-SDK's van DocumentDB. De SDK's accepteren een optionele parameter 'PreferredLocations' is een geordende lijst van Azure regio's.

De SDK stuurt automatisch dat alle schrijfbewerkingen naar de huidige regio schrijven. 

Alle leesbewerkingen wordt verzonden naar de eerste beschikbare regio in de lijst PreferredLocations. Als de aanvraag is mislukt, zal de client mislukken omlaag in de lijst naar het volgende gebied, enzovoort. 

De client die SDK 's alleen te probeert lezen van de regio's opgegeven in PreferredLocations. Dus bijvoorbeeld worden als de Account van de Database beschikbaar in drie regio's is, maar de client alleen twee van de regio's van niet-schrijven voor de PreferredLocations geeft, vervolgens geen leesbewerkingen geserveerd uit de regio schrijven, zelfs in geval van failover.

De toepassing kan controleren of het huidige eindpunt voor schrijven en lezen eindpunt door de SDK worden gekozen aan de hand van twee eigenschappen, WriteEndpoint en ReadEndpoint, beschikbaar in de SDK versie 1.8 en hoger. 

Als de eigenschap PreferredLocations niet is ingesteld, worden alle aanvragen van het huidige gebied voor schrijven geserveerd. 


## <a name="net-sdk"></a>.NET SDK
De SDK kan worden gebruikt zonder de codewijzigingen. In dit geval de SDK automatisch zorgt ervoor dat beide leest en schrijft naar het huidige gebied voor schrijven. 

De parameter ConnectionPolicy voor de constructor DocumentClient heeft versie 1,8 en later van de .NET SDK, een eigenschap met de naam Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations. Deze eigenschap is van het type collectie `<string>` en een lijst met regionamen moet bevatten. De string-waarden zijn opgemaakt per in de kolom naam van gebied op de [Azure regio's]  [ regions] pagina, zonder spaties voor of na de eerste en laatste teken respectievelijk.

De huidige schrijven en lezen eindpunten zijn respectievelijk DocumentClient.WriteEndpoint en DocumentClient.ReadEndpoint.

> [AZURE.NOTE] De URL's voor de eindpunten moeten niet worden beschouwd als langlevende-constanten zijn. De service, kan deze op elk gewenst moment bijwerken. Deze wijziging worden de SDK automatisch verwerkt.

    // Getting endpoints from application settings or other configuration location
    Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
    string accountKey = Properties.Settings.Default.GlobalDatabaseKey;

    //Setting read region selection preference 
    connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
    connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
    connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

    // initialize connection
    DocumentClient docClient = new DocumentClient(
        accountEndPoint,
        accountKey,
        connectionPolicy);

    // connect to DocDB 
    await docClient.OpenAsync().ConfigureAwait(false);


## <a name="nodejs-javascript-and-python-sdks"></a>NodeJS, JavaScript en Python SDK 's
De SDK kan worden gebruikt zonder de codewijzigingen. De SDK wordt in dit geval automatisch doorverwezen zowel lees- en schrijfbewerkingen naar de huidige regio schrijven. 

In versie 1,8 en later van de SDK, de ConnectionPolicy-parameter voor de constructor DocumentClient een nieuwe eigenschap genoemd DocumentClient.ConnectionPolicy.PreferredLocations. Deze parameter is een matrix van tekenreeksen waarmee een lijst met regionamen. De namen zijn opgemaakt per in de kolom naam van gebied in de [Regio's Azure]  [ regions] pagina. U kunt ook de vooraf gedefinieerde constanten gebruiken in het gemak object AzureDocuments.Regions

De huidige schrijven en lezen eindpunten zijn respectievelijk DocumentClient.getWriteEndpoint en DocumentClient.getReadEndpoint.

> [AZURE.NOTE] De URL's voor de eindpunten moeten niet worden beschouwd als langlevende-constanten zijn. De service, kan deze op elk gewenst moment bijwerken. De SDK wordt deze wijziging automatisch verwerkt.

Hieronder vindt u een voorbeeld van code voor NodeJS/Javascript. Python en Java volgen hetzelfde patroon.

    // Creating a ConnectionPolicy object
    var connectionPolicy = new DocumentBase.ConnectionPolicy();
    
    // Setting read region selection preference, in the following order -
    // 1 - West US
    // 2 - East US
    // 3 - North Europe
    connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];
    
    // initialize the connection
    var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);


## <a name="rest"></a>REST 
Nadat u een databaseaccount beschikbaar is gesteld in meerdere regio's, kunnen clients query's beschikbaar door middel van een GET-verzoek naar de volgende URI.

    https://{databaseaccount}.documents.azure.com/

De service retourneert een lijst met regio's en hun bijbehorende DocumentDB eindpunt URI's voor de replica's. Het huidige schrijven gebied wordt aangegeven in het antwoord. De client kunt als volgt het juiste eindpunt voor alle verdere REST API aanvragen selecteren.

Voorbeeld van de reactie

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "West US",
                "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "East US",
                "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.com",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }


-   PUT, POST en DELETE-verzoeken te naar de opgegeven URI-schrijven gaan
-   Alle haalt en andere aanvragen alleen-lezen (bijvoorbeeld query's) kunnen gaan naar een eindpunt van de keuze van de client

Schrijven van verzoeken tot gebieden voor alleen-lezen mislukt met foutcode HTTP 403 ('niet toegestaan').

Als het gebied schrijven na de initiële ontdekkingsfase van de client wordt gewijzigd, mislukt schrijft het vorige schrijven gebied met foutcode HTTP 403 ('niet toegestaan'). De client moet vervolgens de lijst met regio's opnieuw om de bijgewerkte schrijven regio krijgen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de distributie gegevens wereldwijd met DocumentDB in de volgende artikelen:

- [Verspreiden van gegevens wereldwijd met DocumentDB](documentdb-distribute-data-globally.md)
- [Niveaus samenhang](documentdb-consistency-levels.md)
- [De werking van doorvoer met meerdere regio 's](documentdb-manage.md#how-throughput-works-with-multiple-regions)
- [Regio's met de Azure portal toevoegen](documentdb-portal-global-replication.md)

[regions]: https://azure.microsoft.com/regions/ 
