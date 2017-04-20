<properties 
    pageTitle="Azure SQL-Database verbinden met Azure zoeken met behulp van indexeerfuncties | Microsoft Azure | Indexeerfuncties" 
    description="Informatie over het ontvangen van gegevens van Azure SQL-Database naar een indexeerfuncties met Azure Search-index." 
    services="search" 
    documentationCenter="" 
    authors="chaosrealm" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/27/2016" 
    ms.author="eugenesh"/>

#<a name="connecting-azure-sql-database-to-azure-search-using-indexers"></a>Azure SQL-Database verbinden met Azure zoeken op basis van indexeerfuncties

Azure Search-service is een gehoste wolk search-service waarmee u gemakkelijk om een goede zoekfunctie. Voordat u zoeken kunt, moet u een Azure Search-index met gegevens te vullen. Als de gegevens zich in een Azure SQL-database, kan de nieuwe **Azure Search indexer voor Azure SQL-Database** (of **Azure SQL indexeerprogramma** voor korte) het indexeringsproces automatiseren. Dit betekent dat er minder code te schrijven en minder infrastructuur te hechten.

Dit artikel heeft betrekking op het mechanisme van het gebruik van indexeerfuncties, maar hierin worden ook de functies die alleen beschikbaar zijn in Azure SQL-databases (bijvoorbeeld geïntegreerde bijhouden). Azure zoeken ondersteunt ook andere gegevensbronnen, zoals DocumentDB Azure blob-opslag en tabelopslag. Als u wilt zien voor aanvullende gegevensbronnen ondersteunen, uw feedback op het [Feedbackforum Azure zoeken](https://feedback.azure.com/forums/263029-azure-search/).

## <a name="indexers-and-data-sources"></a>Indexeerfuncties en gegevensbronnen

U kunt instellen en configureren van een SQL Azure indexeerfunctie met behulp van: 

- De wizard gegevens importeren in [Azure portal](https://portal.azure.com)
- Azure Search [.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)
- Azure Search [REST API](http://go.microsoft.com/fwlink/p/?LinkID=528173)

In dit artikel gebruiken we de REST API aan hoe u **indexeerfuncties** en **gegevensbronnen**maken en beheren. 

Een **gegevensbron** geeft aan welke gegevens aan de index, referenties die nodig zijn voor toegang tot de gegevens en het beleid dat wijzigingen in de gegevens (nieuwe, gewijzigde of verwijderde rijen) efficiënt te identificeren. Wordt gedefinieerd als een onafhankelijke bron zodat deze kan worden gebruikt door meerdere indexeerfuncties.

**Indexeerfunctie** is een bron die gegevensbronnen met doel zoekindexen verbindt. Indexeerfunctie wordt gebruikt op de volgende manieren:
 
- Uitvoeren van een momentopname van de gegevens voor het vullen van een index.
- Een index bijwerken met wijzigingen in de gegevensbron van een schema.
- Uitvoeren op verzoek voor het bijwerken van een index, indien nodig. 

## <a name="when-to-use-azure-sql-indexer"></a>Wanneer gebruikt u SQL Azure Indexer

Afhankelijk van verschillende factoren met betrekking tot uw gegevens, het gebruik van SQL Azure indexeerfunctie kan of niet. Als uw gegevens de volgende vereisten past, kunt u de indexeerfunctie Azure SQL: 

- Alle gegevens afkomstig zijn uit een enkele tabel of weergave
    - Als de gegevens wordt verspreid over meerdere tabellen, kunt u een weergave maken en gebruikt u die weergave met de indexeerfunctie. Echter, als u een weergave gebruikt, niet mogelijk gebruik van SQL Server geïntegreerd opsporing. Zie voor meer informatie [in deze sectie](#CaptureChangedRows). 
- De gegevenstypen die worden gebruikt in de gegevensbron worden ondersteund door de indexeerfunctie. De meeste, maar niet alle de SQL-typen worden ondersteund. Zie [toewijzing van gegevenstypen in Azure Search](http://go.microsoft.com/fwlink/p/?LinkID=528105)voor meer informatie. 
- U niet nodig hebt in de buurt van real-time updates voor de index wanneer een rij wordt gewijzigd. 
    - De indexeerfunctie kan de tabel opnieuw indexeren hoogstens elke 5 minuten. Als de gegevens vaak worden gewijzigd en de wijzigingen worden doorgevoerd in de index binnen enkele seconden of minuten één moeten, kunt u het beste rechtstreeks [Azure Search Index API](https://msdn.microsoft.com/library/azure/dn798930.aspx) gebruiken. 
- Als u een grote gegevensset hebt en u wilt uitvoeren de indexeerfunctie volgens een schema, wordt uw schema kunt u efficiënt identificeren gewijzigd (en verwijderd, indien van toepassing) rijen. Zie "vastleggen van gewijzigd en verwijderde rijen' hieronder voor meer informatie. 
- De grootte van de geïndexeerde velden in een rij niet groter zijn dan de maximale grootte van een Azure Search indexing-verzoek 16 MB is. 

## <a name="create-and-use-an-azure-sql-indexer"></a>Maken en gebruiken van een SQL Azure indexeerfunctie

Maak eerst de gegevensbron: 

    POST https://myservice.search.windows.net/datasources?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key
    
    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:<your server>.database.windows.net,1433;Database=<your database>;User ID=<your user name>;Password=<your password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "name of the table or view that you want to index" }
    }


U kunt de verbindingsreeks opvragen bij [Azure klassieke Portal](https://portal.azure.com); Gebruik de `ADO.NET connection string` optie.

Maak de Azure-zoekindex doel als je al niet hebt. U kunt een index met behulp van de [portal UI](https://portal.azure.com) of de [API van Index maken](https://msdn.microsoft.com/library/azure/dn798941.aspx). Zorg ervoor dat het schema van de doel-index compatibel met het schema van de brontabel is-Zie [toewijzing tussen de gegevenstypen SQL en Azure search](#TypeMapping).

Ten slotte de indexeerfunctie maken door een naam geven en de index van bron en doel gegevens verwijzen naar:

    POST https://myservice.search.windows.net/indexers?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key
    
    { 
        "name" : "myindexer",
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name"
    }

Indexeerfunctie gemaakt op deze manier heeft een schema. Automatisch wordt uitgevoerd als wanneer deze wordt gemaakt. Deze kunt u op elk gewenst moment een verzoek **voor indexering wordt uitgevoerd** opnieuw uitvoeren:

    POST https://myservice.search.windows.net/indexers/myindexer/run?api-version=2015-02-28 
    api-key: admin-key

U kunt verschillende aspecten van de indexeerfunctie gedrag, zoals batchgrootte en hoeveel documenten worden overgeslagen voordat de indexeerfunctie uitvoering is mislukt. Zie de [Indexeerfunctie API maken](https://msdn.microsoft.com/library/azure/dn946899.aspx)voor meer informatie.
 
Wellicht moet u toestaan dat Azure services verbinding maken met uw database. Zie [Verbinding maken vanaf Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) voor instructies over hoe u dat kunt doen.

Geschiedenis (aantal geïndexeerde items, storingen, enz.) gebruiken voor het controleren van de indexer-status en uitvoering van een aanvraag **voor indexering status** : 

    GET https://myservice.search.windows.net/indexers/myindexer/status?api-version=2015-02-28 
    api-key: admin-key

Het antwoord ziet er ongeveer als volgt: 

    {
        "@odata.context":"https://myservice.search.windows.net/$metadata#Microsoft.Azure.Search.V2015_02_28.IndexerExecutionInfo",
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2015-02-21T00:23:24.957Z",
            "endTime":"2015-02-21T00:36:47.752Z",
            "errors":[],
            "itemsProcessed":1599501,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null 
        },
        "executionHistory":
        [
            {
                "status":"success",
                "errorMessage":null,
                "startTime":"2015-02-21T00:23:24.957Z",
                "endTime":"2015-02-21T00:36:47.752Z",
                "errors":[],
                "itemsProcessed":1599501,
                "itemsFailed":0,
                "initialTrackingState":null,
                "finalTrackingState":null 
            },
            ... earlier history items 
        ]
    }

Uitvoering van geschiedenis bevat maximaal 50 van de meest recent voltooide uitvoering en in omgekeerde chronologische volgorde worden gesorteerd (zodat de meest recente uitvoering van wat zich het eerst in het antwoord). Meer informatie over het antwoord vindt u in de [Indexeerfunctie Status ophalen](http://go.microsoft.com/fwlink/p/?LinkId=528198)

## <a name="run-indexers-on-a-schedule"></a>Indexeerfuncties uitvoeren volgens een schema

U kunt ook de indexeerfunctie periodiek wordt uitgevoerd volgens een schema rangschikken. Voeg hiervoor de **schema** -eigenschap bij het maken of bijwerken van de indexeerfunctie. In het volgende voorbeeld ziet u een PUT-aanvraag voor het bijwerken van de indexeerfunctie:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key 

    { 
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

De parameter **interval** is vereist. Het interval wordt verwezen naar de tijd tussen het begin van de twee opeenvolgende indexeerfunctie uitvoeringen. De minimaal toegestane interval is 5 minuten. de langste is één dag. Als het is geformatteerd als een XSD-'dayTimeDuration'-waarde (een beperkte subset van de waarde van een [ISO 8601 duur](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ). Het patroon voor dit is: `P(nD)(T(nH)(nM))`. Voorbeelden: `PT15M` voor elke 15 minuten, `PT2H` voor elke 2 uur.

De optionele **startTime** geeft aan wanneer de geplande uitvoeringen dient aan te vangen. Als dit wordt weggelaten, wordt de huidige UTC-tijd gebruikt. Deze tijd kan zijn in het verleden – waarin de uitvoering van de eerste aanvraag gepland als de indexeerfunctie is actief vanaf de starttijd voortdurend.  

Slechts één uitvoering van een indexeerfunctie kunt tegelijk uitvoeren. Als een indexering wordt uitgevoerd wanneer de uitvoering ervan is gepland, wordt de uitvoering van het volgende geplande tijdstip uitgesteld.

Laten we eens een voorbeeld om dit concrete. Stel dat we de volgende uurschema geconfigureerd: 

    "schedule" : { "interval" : "PT1H", "startTime" : "2015-03-01T00:00:00Z" }

Dit is wat er gebeurt: 

1. De eerste uitvoering van de indexeerfunctie wordt gestart op of omstreeks 1 maart 2015 00:00 uur UTC.
1. Stel dat deze uitvoering duurt 20 minuten (of op elk gewenst moment minder dan 1 uur).
1. De tweede uitvoering begint op of omstreeks 1 maart 2015 1:00 uur 
1. Stel nu dat deze uitvoering – bijvoorbeeld 70 minuten – van meer dan een uur duurt dat voltooid ongeveer 2:10 uur is
1. Het is nu 2:00 uur tijd voor de derde uitvoering te starten. Echter, omdat de tweede uitvoering van 1 uur nog steeds actief is, is de derde uitvoering wordt overgeslagen. De derde uitvoering begint om 3 uur 's nachts

U kunt toevoegen, wijzigen of verwijderen van een schema voor een bestaande indexeerfunctie met behulp van een aanvraag **plaatsen voor indexering** . 

<a name="CaptureChangedRows">, /a >
## <a name="capturing-new-changed-and-deleted-rows"></a>Opnemen van nieuwe, gewijzigd en verwijderde rijen

Als uw tabel veel rijen bevat, moet u een beleid voor detectie van gegevens wijzigen. Opsporing kan een efficiënt voor het ophalen van alleen de nieuwe of gewijzigde rijen zonder de hele tabel opnieuw indexeren.

### <a name="sql-integrated-change-tracking-policy"></a>SQL geïntegreerd beleid voor het bijhouden van wijzigingen

Als de SQL-database ondersteuning biedt voor [Wijzigingen bijhouden](https://msdn.microsoft.com/library/bb933875.aspx), wordt u aangeraden **SQL geïntegreerde wijzigen Traceringsbeleid**gebruiken. Dit is de meest efficiënte beleid. Bovendien is het mogelijk Azure Search verwijderde rijen zonder dat u hoeft een expliciete 'soft delete' kolom toevoegen aan de tabel te identificeren.

Geïntegreerde wijzigingen bijhouden wordt ondersteund vanaf de volgende versies van SQL Server-database:
 
- SQL Server 2008 R2 en later, als u SQL Server in Azure VMs gebruikt. 
- Azure SQL Database V12, als u gebruikmaakt van Azure SQL-Database.

Wanneer met behulp van geïntegreerde SQL bijhouden van beleid, een beleid voor afzonderlijke gegevens verwijdering detectie niet opgeeft - dit beleid heeft ingebouwde ondersteuning voor het identificeren van verwijderde rijen.

Dit beleid kan alleen worden gebruikt met tabellen; het kan niet worden gebruikt met weergaven. U moet bijhouden voor de tabel die u gebruikt voordat u dit beleid inschakelen. Zie [Wijzigingen bijhouden uitschakelen en inschakelen](https://msdn.microsoft.com/library/bb964713.aspx) voor instructies. 

Dit beleid gebruiken, maken of bijwerken van de gegevensbron als volgt:
 
    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" }, 
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy" 
      }
    }

<a name="HighWaterMarkPolicy"></a>
### <a name="high-water-mark-change-detection-policy"></a>Hoge Water Mark opsporing beleid

Terwijl het beleid SQL geïntegreerde wijzigingen bijhouden wordt aanbevolen, kan het alleen met de tabellen, niet weergaven gebruikt. Als u een weergave gebruikt, kunt u overwegen het beleid hoog water is ingeschakeld. Dit beleid kan worden gebruikt als uw tabel of weergave bevat een kolom die aan de volgende criteria voldoet:

- Alle voegt een waarde opgeven voor de kolom. 
- Alle updates voor een artikel ook wijzigen de waarde van de kolom. 
- De waarde van deze kolom wordt verhoogd met elke wijziging.
- Query's met de volgende waar en ORDER BY-componenten kunnen efficiënt worden uitgevoerd: `WHERE [High Water Mark Column] > [Current High Water Mark Value] ORDER BY [High Water Mark Column]`.

Bijvoorbeeld is een kolom met geïndexeerde **rowversion** een ideale kandidaat voor de kolom hoog water is ingeschakeld. Dit beleid gebruiken, maken of bijwerken van de gegevensbron als volgt: 

    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" }, 
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
           "highWaterMarkColumnName" : "[a row version or last_updated column name]" 
      }
    }

> [AZURE.WARNING] Als de brontabel geen index voor de kolom hoog water is ingeschakeld heeft, kunnen query's die worden gebruikt door de indexeerfunctie SQL-time-out. In het bijzonder de `ORDER BY [High Water Mark Column]` component vereist een index doeltreffend uit te voeren wanneer de tabel veel rijen bevat. 

Als u de time-outfouten optreden, kunt u de `queryTimeout` indexeerfunctie configuratie-instelling voor de time-out voor query's ingesteld op een waarde die hoger is dan de standaard 5-minute-out. Bijvoorbeeld, als de time-out ingesteld op 10 minuten, maken of bijwerken van de indexeerfunctie met de volgende configuratie: 

    {
      ... other indexer definition properties
     "parameters" : { 
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

U kunt ook uitschakelen de `ORDER BY [High Water Mark Column]` component. Dit is echter niet aanbevolen omdat als de uitvoering van de indexeerfunctie wordt onderbroken door een fout, de indexering opnieuw verwerken alle rijen als deze wordt uitgevoerd, wordt later - zelfs als de indexeerfunctie al bijna alle rijen verwerkt op het moment dat deze werd onderbroken. Uitschakelen van de `ORDER BY` -component, gebruik de `disableOrderByHighWaterMarkColumn` instellen in de definitie van de indexer:  

    {
     ... other indexer definition properties
     "parameters" : { 
            "configuration" : { "disableOrderByHighWaterMarkColumn" : true } }
    }

### <a name="soft-delete-column-deletion-detection-policy"></a>Beleid voor zachte kolom verwijderen-detectie verwijderen

Wanneer rijen worden verwijderd uit de brontabel, wilt u waarschijnlijk de rijen verwijderen uit de search-index. Als u de geïntegreerde SQL bijhouden van beleid, is dit afgehandeld voor u. Echter, het hoge water mark bijhouden beleid heeft niets te maken met verwijderde rijen. Wat te doen? 

Als de rijen uit de tabel fysiek verwijderd, heeft Azure Search op geen enkele manier afleiden van de aanwezigheid van records die niet meer bestaan.  U kunt echter de techniek 'soft delete' logisch om rijen te verwijderen zonder ze te verwijderen uit de tabel. Een kolom toevoegen aan de tabel of weergave en mark rijen als verwijderd met behulp van die kolom.

Wanneer u de techniek soft delete gebruikt, kunt u de zachte beleid als volgt verwijderen bij het maken of bijwerken van de gegevensbron: 

    { 
        …, 
        "dataDeletionDetectionPolicy" : { 
           "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
           "softDeleteColumnName" : "[a column name]", 
           "softDeleteMarkerValue" : "[the value that indicates that a row is deleted]" 
        }
    }

De **softDeleteMarkerValue** moet een tekenreeks – gebruik de reeksaanduiding van de werkelijke waarde. Bijvoorbeeld als u een gehele kolom waarin verwijderde rijen zijn gemarkeerd met de waarde 1 hebben, gebruiken `"1"`. Als er een BIT-kolom waarin verwijderde rijen zijn gemarkeerd met de Booleaanse waarde true is, gebruikt u `"True"`. 

<a name="TypeMapping"></a>
## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>Toewijzing tussen SQL-gegevenstypen en Azure Search-gegevenstypen

|SQL-gegevenstype | Index doel veldtypen toegestaan |Notities 
|------|-----|----|
|bits|Edm.Boolean, Edm.String| |
|int, smallint, tinyint |Edm.Int32, Edm.Int64, Edm.String| |
| bigint | Edm.Int64, Edm.String | |
| real, float |Edm.Double, Edm.String | |
| smallmoney, decimale numerieke geld | Edm.String| Azure Search biedt geen ondersteuning voor het omzetten van decimaal typen in Edm.Double omdat deze precisie verliest |
| CHAR, nchar, varchar, nvarchar | Edm.String<br/>Collection(EDM.String)|Een SQL-tekenreeks kan worden gebruikt voor het vullen van een veld Collection(Edm.String) als de tekenreeks een JSON-matrix van tekenreeksen vertegenwoordigt:`["red", "white", "blue"]` | 
|smalldatetime, datetime, datetime2, datum, datetimeoffset |Edm.DateTimeOffset, Edm.String| |
|uniqueidentifer | Edm.String | |
|Geografie | Edm.GeographyPoint | Alleen wat exemplaren van het type punt met SRID 4326 (dit is de standaardinstelling) worden ondersteund. | | 
|ROWVERSION| N.V.T. |Rij versie kolommen kunnen niet worden opgeslagen in de zoekindex, maar ze kunnen worden gebruikt voor het bijhouden van wijzigingen | |
| tijd, binary, varbinary, afbeelding, xml, geometrie, Stream CLR-typen | N.V.T. |Niet ondersteund |

## <a name="configuration-settings"></a>Configuratie-instellingen

SQL-indexer beschrijft verschillende configuratie-instellingen: 

|Instelling |Gegevenstype | Doel | Standaardwaarde |
|--------|----------|---------|---------------|
| queryTimeout | tekenreeks | Hiermee stelt u de time-out voor SQL-query uitvoeren | 5 minuten ("00: 05:00") |
| disableOrderByHighWaterMarkColumn | BOOL | Zorgt ervoor dat de SQL-query door het hoge water mark-beleid gebruikt om de component ORDER BY achterwege te laten. Zie [beleid voor hoge Water Mark](#HighWaterMarkPolicy) | False | 

Deze instellingen worden gebruikt de `parameters.configuration` -object in de definitie van de indexeerfunctie. Bijvoorbeeld als u de time-out voor query's ingesteld op 10 minuten, wilt maken of bijwerken van de indexeerfunctie met de volgende configuratie: 

    {
      ... other indexer definition properties
     "parameters" : { 
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Q:** Kan ik SQL Azure indexering gebruiken met SQL-databases op IaaS VMs in Azure

A: Ja. U moet echter de search-service verbinding maken met uw database toestaan. Zie [een verbinding met SQL Server op een Azure VM indexeerfunctie Azure Search configureren](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md)voor meer informatie.


**Q:** Kan ik SQL Azure indexering gebruiken met SQL-databases in lokalen met 

A: we niet aanbevolen noch ondersteund, zoals dit moet u uw databases voor Internet-verkeer te openen. 

**Q:** Kan ik SQL Azure indexering gebruiken met uitzondering van SQL Server wordt uitgevoerd in IaaS op Azure-databases? 

A: we ondersteuning geen van dit scenario, omdat de indexering geen SQL Server-databases elke is niet getest.  

**Q:** Kan ik meerdere indexeerfuncties uitgevoerd volgens een schema maken? 

A: Ja. Echter kan slechts één indexering worden uitgevoerd op één knooppunt tegelijk. Als u meerdere indexeerfuncties tegelijkertijd worden uitgevoerd, kunt u het schalen van de zoekservice op meer dan één eenheid van het zoeken. 

**Q:** Gevolgen heeft voor een indexering wordt uitgevoerd op mijn query werkbelasting? 

A: Ja. Indexering wordt uitgevoerd op een van de knooppunten in de search-service en bronnen van het knooppunt worden gedeeld tussen indexing en query's en andere API aanvragen bedienen. Als u intensieve belasting voor indexering en query uitvoeren en een hoge frequentie van 503 fouten of toenemende reactietijden optreden, kunt u de zoekservice schaalt.
