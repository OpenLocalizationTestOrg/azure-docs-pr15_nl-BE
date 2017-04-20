<properties
    pageTitle="Stream-Analytics uitgangen: opties voor opslag, analyse | Microsoft Azure"
    description="Meer informatie over de Stream Analytics uitgangen Gegevensopties inclusief Power BI voor analyseresultaten gericht."
    keywords="gegevenstransformatie van, resultaten van analyses, opties voor opslag van gegevens"
    services="stream-analytics,documentdb,sql-database,event-hubs,service-bus,storage"
    documentationCenter="" 
    authors="jeffstokes72"
    manager="jhubbard" 
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="stream-analytics-outputs-options-for-storage-analysis"></a>Stream-Analytics uitgangen: opties voor opslag, analyse

Tijdens het ontwerpen van een taak Stream Analytics overwegen hoe de resulterende gegevens wordt verbruikt. Zal weergave van de resultaten van het project Stream Analytics en waar wordt dit opgeslagen?

Om verschillende patronen van toepassing, heeft Azure Stream Analytics verschillende opties voor het opslaan van de output en analyseresultaten van de weergeven. Dit kunt u gemakkelijk de uitvoer van de taak bekijken en beschikt u over flexibiliteit in het verbruik en de opslag van de uitvoer van de taak voor gegevensopslag en andere doeleinden. Uitvoer die is geconfigureerd in de taak moet bestaan voordat de taak is gestart en gebeurtenissen gestart die doorloopt. Bijvoorbeeld als u als output van een Blob storage, wordt de taak niet een opslag-account automatisch gemaakt. Deze moet door de gebruiker worden gemaakt voordat de ASA taak is gestart.

## <a name="azure-data-lake-store"></a>Azure Lake gegevensarchief

Stream Analytics ondersteunt [Azure Lake gegevensarchief](https://azure.microsoft.com/services/data-lake-store/). Deze opslag kunt u gegevens van elke grootte, type en opname snelheid voor operationele en experimentele analytics op te slaan. Op dit moment wordt maken en configureren van een gegevensarchief Lake uitgangen alleen ondersteund in de klassieke Azure-Portal. Stream Analytics moet verder worden gemachtigd voor toegang tot de gegevensopslag Lake. Gegevens over de toelating en aanmelden voor het voorbeeld van gegevens Lake winkel (indien nodig) worden in het [Lake met Data output artikel](stream-analytics-data-lake-output.md)besproken.

### <a name="authorize-an-azure-data-lake-store"></a>Toestaan dat een Azure-gegevensarchief Lake

Als Lake gegevensopslag als uitvoer in de portal Management Azure is geselecteerd, wordt u gevraagd een verbinding met een bestaande Lake gegevensarchief toe te staan.  

![Lake gegevensarchief toestaan](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)  

Vul vervolgens de eigenschappen voor de uitvoer van Lake gegevensarchief zoals hieronder wordt weergegeven:

![Lake gegevensarchief toestaan](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)  

De onderstaande tabel worden de namen van eigenschappen en hun beschrijving die nodig zijn voor het maken van een gegevensarchief Lake uitvoer.

<table>
<tbody>
<tr>
<td><B>NAAM VAN EIGENSCHAP</B></td>
<td><B>BESCHRIJVING</B></td>
</tr>
<tr>
<td>Uitvoeralias</td>
<td>Dit is een beschrijvende naam in query's worden gebruikt om de query-uitvoer naar deze Lake gegevensopslag.</td>
</tr>
<tr>
<td>De naam</td>
<td>De naam van de gegevensopslag Lake account waar u de uitvoer wilt verzenden. U krijgt een vervolgkeuzelijst met Lake gegevensarchief accounts waaraan de gebruiker is aangemeld bij de portal toegang tot heeft.</td>
</tr>
<tr>
<td>Pad Prefix patroon [<I>optioneel</I>]</td>
<td>Het pad van het schrijven van uw bestanden in de opgegeven gegevens Lake Store-Account gebruikt. <BR>{datum}, {time}<BR>Voorbeeld 1: Map1/logboeken / {datum} / {time}<BR>Voorbeeld 2: Map1/logboeken / {datum}</td>
</tr>
<tr>
<td>Datumnotatie [<I>optioneel</I>]</td>
<td>Als het token van de datum in het pad van het voorvoegsel wordt gebruikt, kunt u de notatie waarin de bestanden worden geordend. Voorbeeld: Jjjj/MM/DD</td>
</tr>
<tr>
<td>Tijdnotatie [<I>optioneel</I>]</td>
<td>Als het token van de tijd in het pad van het voorvoegsel wordt gebruikt, geeft u de indeling waarin de bestanden worden geordend. Op dit moment is de enige ondersteunde waarde uu.</td>
</tr>
<tr>
<td>Gebeurtenis serialisatie-indeling</td>
<td>Indeling serialisatie voor uitvoergegevens. JSON, CSV- en Avro worden ondersteund.</td>
</tr>
<tr>
<td>Codering</td>
<td>Als CSV- of JSON opmaken, moet een codering worden opgegeven. UTF-8 is de enige ondersteunde coderingsindeling op dit moment.</td>
</tr>
<tr>
<td>Scheidingsteken</td>
<td>Alleen van toepassing op CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens voor het serialiseren van CSV-gegevens. Ondersteunde waarden zijn door komma's, puntkomma, spatie, tab en de verticale balk.</td>
</tr>
<tr>
<td>Indeling</td>
<td>Alleen van toepassing op JSON serialisatie. Regel gescheiden geeft aan dat de uitvoer wordt opgemaakt door elk gescheiden door een nieuwe regel JSON-object. Matrix geeft aan dat de uitvoer wordt opgemaakt als een array van objecten JSON.</td>
</tr>
</tbody>
</table>

### <a name="renew-data-lake-store-authorization"></a>Vergunning Lake gegevensarchief vernieuwen

U moet uw account meer gegevensarchief opnieuw worden geverifieerd als het wachtwoord is gewijzigd nadat de taak is gemaakt of laatst geverifieerd.

![Lake gegevensarchief toestaan](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)  


## <a name="sql-database"></a>SQL-Database

[Azure SQL-Database](https://azure.microsoft.com/services/sql-database/) kan worden gebruikt als een uitvoer voor gegevens die is relationeel van aard of voor toepassingen die afhankelijk zijn van de inhoud in een relationele database wordt gehost. Stream Analytics taken worden aan een bestaande tabel in een Azure SQL-Database geschreven.  Houd er rekening mee dat het tabelschema moet exact overeenkomen met de velden en hun wordt uitvoer van de taak typen. Een [Azure SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) kan ook worden opgegeven als een uitvoer via de SQL-Database uitvoeroptie ook (dit is een voorbeeldfunctie). De onderstaande tabel worden de namen van eigenschappen en de bijbehorende beschrijvingen voor het maken van de uitvoer van een SQL-Database.

| Naam van eigenschap | Beschrijving |
|---------------|-------------|
| Uitvoeralias | Dit is een beschrijvende naam in query's worden gebruikt om de uitvoer van de query naar deze database. |
| Database | De naam van de database waarin u de uitvoer wilt verzenden |
| Naam van de server | De naam van de Database van SQL server |
| Gebruikersnaam | De gebruikersnaam die toegang heeft tot het schrijven naar de database |
| Wachtwoord | Het wachtwoord voor de verbinding met de database |
| Tabel | De naam van de tabel waarin de uitvoer wordt geschreven. De naam is hoofdlettergevoelig en het schema van deze tabel om het aantal velden en de gegevenstypen die worden gegenereerd door de uitvoer van de taak precies moet overeenkomen. |

> [AZURE.NOTE] Aangeboden Azure SQL-Database wordt momenteel ondersteund voor de uitvoer van een taak in de Stream Analytics. Een Azure VM met SQL Server met een database die is gekoppeld, wordt echter niet ondersteund. Dit kan worden gewijzigd in toekomstige versies.

## <a name="blob-storage"></a>BLOB-opslag

BLOB-opslag biedt een kosteneffectieve en schaalbare oplossing voor het opslaan van grote hoeveelheden ongestructureerde gegevens in de cloud.  Voor een inleiding op Azure Blob-opslag en het gebruik ervan, Zie de documentatie bij [het gebruik van BLOB's](../storage/storage-dotnet-how-to-use-blobs.md).

De onderstaande tabel worden de namen van eigenschappen en de bijbehorende beschrijvingen voor het maken van een blob-uitvoer.

<table>
<tbody>
<tr>
<td>NAAM VAN EIGENSCHAP</td>
<td>BESCHRIJVING</td>
</tr>
<tr>
<td>Uitvoeralias</td>
<td>Dit is een beschrijvende naam in query's worden gebruikt om de query-uitvoer naar deze blobopslag.</td>
</tr>
<tr>
<td>Opslag Account</td>
<td>De naam van de opslag account waar u de uitvoer wilt verzenden.</td>
</tr>
<tr>
<td>Opslag Account sleutel</td>
<td>De geheime sleutel die is gekoppeld aan de account van de opslag.</td>
</tr>
<tr>
<td>Opslag Container</td>
<td>Containers bieden een logische groepering voor opgeslagen in de service Microsoft Azure Blob BLOB's. Wanneer u een blob naar de Blob-service uploaden, moet u een container voor die blob.</td>
</tr>
<tr>
<td>Pad Prefix patroon [optioneel]</td>
<td>Pad naar het bestand gebruikt voor het schrijven van de BLOB's in de opgegeven container.<BR>U kunt kiezen in het pad, de frequentie die BLOB's zijn geschreven met een of meer exemplaren van de volgende 2 variabelen:<BR>{datum}, {time}<BR>Voorbeeld 1: cluster1/logboeken / {datum} / {time}<BR>Voorbeeld 2: cluster1/logboeken / {datum}</td>
</tr>
<tr>
<td>Datumnotatie [optioneel]</td>
<td>Als het token van de datum in het pad van het voorvoegsel wordt gebruikt, kunt u de notatie waarin de bestanden worden geordend. Voorbeeld: Jjjj/MM/DD</td>
</tr>
<tr>
<td>Tijdnotatie [optioneel]</td>
<td>Als het token van de tijd in het pad van het voorvoegsel wordt gebruikt, geeft u de indeling waarin de bestanden worden geordend. Op dit moment is de enige ondersteunde waarde uu.</td>
</tr>
<tr>
<td>Gebeurtenis serialisatie-indeling</td>
<td>Indeling serialisatie voor uitvoergegevens.  JSON, CSV- en Avro worden ondersteund.</td>
</tr>
<tr>
<td>Codering</td>
<td>Als CSV- of JSON opmaken, moet een codering worden opgegeven. UTF-8 is de enige ondersteunde coderingsindeling op dit moment.</td>
</tr>
<tr>
<td>Scheidingsteken</td>
<td>Alleen van toepassing op CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens voor het serialiseren van CSV-gegevens. Ondersteunde waarden zijn door komma's, puntkomma, spatie, tab en de verticale balk.</td>
</tr>
<tr>
<td>Indeling</td>
<td>Alleen van toepassing op JSON serialisatie. Regel gescheiden geeft aan dat de uitvoer wordt opgemaakt door elk gescheiden door een nieuwe regel JSON-object. Matrix geeft aan dat de uitvoer wordt opgemaakt als een array van objecten JSON.</td>
</tr>
</tbody>
</table>

## <a name="event-hub"></a>Gebeurtenis Hub

[Gebeurtenis Hubs](https://azure.microsoft.com/services/event-hubs/) is een zeer schaalbare publicatie abonnementen gebeurtenis ingestor. Deze kunt gebeurtenissen per seconde miljoenen verzamelen.  Een gebruik van een Hub gebeurtenis als output is wanneer de uitvoer van een project Stream Analytics wordt de invoer van een andere taak is streaming.

Er zijn een aantal parameters die nodig zijn voor de gebeurtenis Hub gegevensstromen configureren als een output.

| Naam van eigenschap | Beschrijving |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Uitvoeralias | Dit is een beschrijvende naam in query's worden gebruikt om de query-uitvoer op de Hub van deze gebeurtenis. |
| Service Bus Namespace | Een naamruimte Service Bus is een container voor een aantal entiteiten messaging. Wanneer u een nieuwe gebeurtenis Hub hebt gemaakt, een Service Bus naamruimte ook gemaakt |
| Gebeurtenis Hub | De naam van de gebeurtenis Hub-uitvoer |
| Gebeurtenis Hub-beleidsnaam | Het beleid gedeeld gebruik kan worden gemaakt op het tabblad gebeurtenis Hub configureren. Elk beleid voor gedeelde toegang wordt een naam hebben, machtigingen die u hebt ingesteld, en de toegangstoetsen |
| Gebeurtenis Hub beleidssleutel | De toegang tot gedeelde sleutel die wordt gebruikt voor het verifiëren van de naamruimte Service Bus |
| Partitie-sleutelkolom [optioneel] | Deze kolom bevat de partitiesleutel voor de uitvoer van de Hub van de gebeurtenis. |
| Gebeurtenis serialisatie-indeling | Indeling serialisatie voor uitvoergegevens.  JSON, CSV- en Avro worden ondersteund. |
| Codering | CSV en JSON is UTF-8 de enige ondersteunde coderingsindeling op dit moment |
| Scheidingsteken | Alleen van toepassing op CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens voor het serialiseren gegevens in CSV-indeling. Ondersteunde waarden zijn door komma's, puntkomma, spatie, tab en de verticale balk. |
| Indeling | Alleen van toepassing op JSON. Regel gescheiden geeft aan dat de uitvoer wordt opgemaakt door elk gescheiden door een nieuwe regel JSON-object. Matrix geeft aan dat de uitvoer wordt opgemaakt als een array van objecten JSON. |

## <a name="power-bi"></a>Power BI

[Power BI](https://powerbi.microsoft.com/) kan worden gebruikt als een uitvoer voor een project Analytics stroom te voorzien in een uitgebreide visualisatie-ervaring van de resultaten van de analyse. Deze mogelijkheid kan worden gebruikt voor operationele dashboards, rapporten en metric aangedreven melden.

### <a name="authorize-a-power-bi-account"></a>Toestaan dat een Power BI-account

1.  Bij Power BI als uitvoer in de portal Management Azure is geselecteerd, wordt u gevraagd een bestaande Power BI-gebruiker toe te staan of om een nieuwe Power BI-account te maken.  

    ![Power BI gebruiker toestaan](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)  

2.  Een nieuwe account maken als u niet nog hebt en klik vervolgens op Nu toestemming geven.  Een scherm zoals hieronder wordt weergegeven.  

    ![Azure rekening Power BI](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)  

3.  Geef het account op werk of school voor de toekenning van de Power BI-uitvoer in deze stap. Als u nog niet bent aangemeld voor Power BI, kiest u ondertekenen van nu. Het werk of school-account die u voor Power BI gebruikt kan afwijken van de Azure abonnementaccount waarmee u momenteel bent aangemeld met.

### <a name="configure-the-power-bi-output-properties"></a>Configureer de eigenschappen van de output Power BI

Als u het geverifieerde Power BI-account hebt, kunt u de eigenschappen voor de Power BI-uitvoer. Hieronder vindt u de lijst met namen van eigenschappen en de bijbehorende beschrijvingen voor het configureren van de Power BI-uitvoer.

| Naam van eigenschap | Beschrijving |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Uitvoeralias | Dit is een beschrijvende naam in query's worden gebruikt om de query-uitvoer om de uitvoer van deze PowerBI. |
| Groep-werkruimte | Als u delen van gegevens met andere gebruikers van de Power BI kunt u groepen binnen uw Power BI-account selecteren of "Mijn werkruimte" kiezen als u niet wilt schrijven naar een groep.  Bijwerken van een bestaande groep is vereist voor het vernieuwen van de Power BI-verificatie. | 
| Naam gegevensset | Geef de naam van een dataset dat dit nodig is voor de uitvoer van de Power BI te gebruiken |
| Naam van de tabel | De naam van een tabel in de dataset van de Power BI-uitvoer opgeven. Op dit moment kan Power BI-uitvoer van Stream Analytics taken alleen hebben één tabel in een dataset |

Raadpleeg het artikel [Azure Stream Analytics & Power BI](stream-analytics-power-bi-dashboard.md) voor een overzicht van het configureren van een Power BI-uitvoer en het dashboard.

> [AZURE.NOTE] Geen expliciet maken de dataset en de tabel in het dashboard Power BI. De dataset en de tabel wordt automatisch ingevuld wanneer de taak wordt gestart en de taak reageert uitvoer in Power BI begint. Houd er rekening mee dat als de taak query geen resultaten genereert, de gegevensverzameling en de tabel wordt niet gemaakt. Bedenk ook dat als Power BI al een dataset en een tabel met dezelfde naam als bedoeld in deze Stream Analytics taak, de bestaande gegevens worden overschreven.

### <a name="renew-power-bi-authorization"></a>Power BI vergunning vernieuwen

U moet uw account Power BI opnieuw worden geverifieerd als het wachtwoord is gewijzigd nadat de taak is gemaakt of laatst geverifieerd. Als meerledige verificatie (MVR gesloten) is geconfigureerd op uw huurder Azure Active Directory (AAD) moet u ook Power BI vergunning elke 2 weken verlengen. Een symptoom van dit probleem is geen uitvoer van de taak en een 'Authenticate gebruikersfout' in de logboeken:

  ![Power BI vernieuwen fout](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)  

Dit probleem oplossen door de actieve taak stoppen en Ga naar de output Power BI.  Klik op de link "Vernieuwen vergunning" en start de taak van de laatste keer gestopt om gegevensverlies te voorkomen.

  ![Power BI vergunning vernieuwen](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)  

## <a name="table-storage"></a>Tabelopslag

[Azure tabel opslag](../storage/storage-introduction.md) biedt een hoge beschikbaarheid, sterk schaalbare opslag, zodat een toepassing automatisch aangepast kan om te voldoen aan de eisen van de gebruiker. Tabelopslag is van Microsoft NoSQL sleutel/kenmerk Archief waarvoor een voor gestructureerde gegevens met minder beperkingen van het schema benutten kunt. Azure tabelopslag kan worden gebruikt voor het opslaan van gegevens voor persistentie en efficiënte ophalen.

De onderstaande tabel worden de namen van eigenschappen en de bijbehorende beschrijvingen voor het maken van de uitvoer van een tabel.

| Naam van eigenschap | Beschrijving |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Uitvoeralias | Dit is een beschrijvende naam die wordt gebruikt in query's om te leiden naar de tabelopslag van deze in de query-uitvoer. |
| Opslag Account | De naam van de opslag account waar u de uitvoer wilt verzenden. |
| Opslag Account sleutel | De toegangstoets is gekoppeld aan de account van de opslag. |
| Naam van de tabel | De naam van de tabel. De tabel gemaakt als het niet bestaat. |
| Partitiesleutel | De naam van de uitvoerkolom met de partitiesleutel. De partitiesleutel is een unieke id voor de partitie binnen een bepaalde tabel die het eerste deel van de primaire sleutel van een eenheid vormt. Het is een string-waarde die maximaal 1 KB groot zijn. |
| Rijsleutel | De naam van de uitvoerkolom met de rijsleutel. De rijsleutel is een unieke id voor een entiteit binnen een bepaalde partitie. Het tweede deel van de primaire sleutel van een eenheid vormt. De rijsleutel is een string-waarde die maximaal 1 KB groot zijn. |
| Batchgrootte | Het aantal records voor een batchbewerking. De standaardwaarde is voldoende voor de meeste taken, verwijzen doorgaans naar de [tabel batchbewerking spec](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx) voor meer informatie over het wijzigen van deze instelling. |

## <a name="service-bus-queues"></a>Service Bus wachtrijen

[Servicewachtrijen Bus](https://msdn.microsoft.com/library/azure/hh367516.aspx) bieden een First In, eerste Out (FIFO) berichtlevering aan een of meer concurrerende consumenten. Normaal gesproken berichten worden ontvangen en verwerkt door de ontvangers in de tijd volgorde waarin ze zijn toegevoegd aan de wachtrij en elk bericht is ontvangen en verwerkt door de consument slechts één bericht verwacht.

De onderstaande tabel worden de namen van eigenschappen en de bijbehorende beschrijvingen voor het maken van de uitvoer van een wachtrij.

| Naam van eigenschap | Beschrijving |
|----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Uitvoeralias | Dit is een beschrijvende naam in query's worden gebruikt om de query-uitvoer naar deze wachtrij Service Bus. |
| Service Bus Namespace | Een naamruimte Service Bus is een container voor een aantal entiteiten messaging. |
| Wachtrijnaam | De naam van de wachtrij Service Bus. |
| Wachtrij-beleidsnaam | Wanneer u een wachtrij maakt, kunt u ook gedeelde-beleid op het tabblad configureren wachtrij maken. Elk beleid voor gedeelde toegang wordt een naam hebben, machtigingen die u hebt ingesteld, en de toegangstoetsen. |
| Wachtrij beleidssleutel | De toegang tot gedeelde sleutel die wordt gebruikt voor het verifiëren van de naamruimte Service Bus |
| Gebeurtenis serialisatie-indeling | Indeling serialisatie voor uitvoergegevens.  JSON, CSV- en Avro worden ondersteund. |
| Codering | CSV en JSON is UTF-8 de enige ondersteunde coderingsindeling op dit moment |
| Scheidingsteken | Alleen van toepassing op CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens voor het serialiseren gegevens in CSV-indeling. Ondersteunde waarden zijn door komma's, puntkomma, spatie, tab en de verticale balk. |
| Indeling | Alleen van toepassing op JSON. Regel gescheiden geeft aan dat de uitvoer wordt opgemaakt door elk gescheiden door een nieuwe regel JSON-object. Matrix geeft aan dat de uitvoer wordt opgemaakt als een array van objecten JSON. |

## <a name="service-bus-topics"></a>Service Bus onderwerpen

Servicewachtrijen Bus zorgen voor een één-op-een communicatiemethode van afzender naar ontvanger, vindt [Service Bus onderwerpen](https://msdn.microsoft.com/library/azure/hh367516.aspx) u een een-op-veel-vorm van communicatie.

De onderstaande tabel worden de namen van eigenschappen en de bijbehorende beschrijvingen voor het maken van de uitvoer van een tabel.

| Naam van eigenschap | Beschrijving |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Uitvoeralias | Dit is een beschrijvende naam in query's worden gebruikt om de query-uitvoer naar dit onderwerp Service Bus. |
| Service Bus Namespace | Een naamruimte Service Bus is een container voor een aantal entiteiten messaging. Wanneer u een nieuwe gebeurtenis Hub hebt gemaakt, een Service Bus naamruimte ook gemaakt |
| De naam van onderwerp | Onderwerpen zijn entiteiten, vergelijkbaar met de gebeurtenis hubs en wachtrijen berichten. Ze geschikt zijn voor het verzamelen van streams gebeurtenis uit een aantal verschillende apparaten en services. Wanneer een onderwerp wordt gemaakt, krijgt deze ook een specifieke naam. Berichten verzonden naar een onderwerp is alleen beschikbaar als een abonnement wordt gemaakt, dus controleer of er een of meer abonnementen in het onderwerp |
| Onderwerp-beleidsnaam | Wanneer u een onderwerp hebt gemaakt, kunt u ook gedeelde-beleid op het tabblad configureren onderwerp maken. Elk beleid voor gedeelde toegang wordt een naam hebben, machtigingen die u hebt ingesteld, en de toegangstoetsen |
| Onderwerp beleidssleutel | De toegang tot gedeelde sleutel die wordt gebruikt voor het verifiëren van de naamruimte Service Bus |
| Gebeurtenis serialisatie-indeling | Indeling serialisatie voor uitvoergegevens.  JSON, CSV- en Avro worden ondersteund. |
| Codering | Als CSV- of JSON opmaken, moet een codering worden opgegeven. UTF-8 is de enige ondersteunde coderingsindeling op dit moment |
| Scheidingsteken | Alleen van toepassing op CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens voor het serialiseren gegevens in CSV-indeling. Ondersteunde waarden zijn door komma's, puntkomma, spatie, tab en de verticale balk. |

## <a name="documentdb"></a>DocumentDB

[Azure-DocumentDB](https://azure.microsoft.com/services/documentdb/) is een volledig beheerde NoSQL document databaseservice met query's en transacties via schema gegevens, voorspelbare en betrouwbare prestaties en een snelle ontwikkeling.

De onderstaande tabel worden de namen van eigenschappen en de bijbehorende beschrijvingen voor het maken van de uitvoer van een DocumentDB.

<table>
<tbody>
<tr>
<td>NAAM VAN EIGENSCHAP</td>
<td>BESCHRIJVING</td>
</tr>
<tr>
<td>De naam</td>
<td>De naam van de account DocumentDB.  Dit is ook het eindpunt voor de account.</td>
</tr>
<tr>
<td>Key account</td>
<td>De sleutel voor gedeelde toegang voor de account DocumentDB.</td>
</tr>
<tr>
<td>Database</td>
<td>De naam van de DocumentDB.</td>
</tr>
<tr>
<td>Collectie naampatroon</td>
<td>De mapnaam van de collectie voor de collecties worden gebruikt. De indeling van de collectie kan worden samengesteld met behulp van het token optionele {partitie} waar partities beginnen bij 0.<BR>Bv. De volgende elementen zijn geldige invoer:<BR>MyCollection {partitie}<BR>MyCollection<BR>Houd er rekening mee dat verzamelingen moeten al bestaan voordat de Stream Analytics taak is gestart en wordt niet automatisch gemaakt.</td>
</tr>
<tr>
<td>Partitiesleutel</td>
<td>De naam van het veld in de uitvoer gebeurtenissen gebruikt voor het opgeven van de sleutel voor het partitioneren van uitvoer in collecties.</td>
</tr>
<tr>
<td>Document-ID</td>
<td>De naam van het veld in de uitvoer-gebeurtenissen gebruikt om de primaire sleutel opgeven die invoegen of bijwerken van bewerkingen zijn gebaseerd op.</td>
</tr>
</tbody>
</table>


## <a name="get-help"></a>Help-informatie opvragen
Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) voor verdere ondersteuning

## <a name="next-steps"></a>Volgende stappen
U hebt nu enkele gegevensstroom Analytics, een beheerde service voor het streamen van analytics op gegevens uit de Internet zaken. Voor meer informatie over deze service, Zie:

- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
