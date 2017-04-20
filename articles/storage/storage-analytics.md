<properties
    pageTitle="Opslag Analytics gebruiken voor het verzamelen van Logboeken en metrische gegevens | Microsoft Azure"
    description="Opslag Analytics kunt u voor het bijhouden van metrische gegevens voor alle opslagservices en voor het verzamelen van Logboeken voor opslag van Blob, wachtrij en tabel."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="storage-analytics"></a>Analytics voor opslag

## <a name="overview"></a>Overzicht

Azure opslag Analytics logboekregistratie wordt uitgevoerd en metrische gegevens voor een opslagruimte biedt. U kunt deze gegevens aanvragen traceren, gebruik trends analyseren en vaststellen van problemen met uw account voor opslag.

Analytics voor opslag gebruikt, moet u deze inschakelen afzonderlijk voor elke service die u wilt controleren. Vanuit de [Azure Portal](https://portal.azure.com)kunt u deze inschakelen. Zie voor meer informatie, [een account voor opslag in de Portal Azure Monitor](storage-monitor-storage-account.md). Ook kunt u Analytics opslag via programmering via de REST API of de clientbibliotheek. De bewerkingen [Blob Service eigenschappen](https://msdn.microsoft.com/library/hh452239.aspx), [Wachtrijeigenschappen Service krijgen](https://msdn.microsoft.com/library/hh452243.aspx) [Tabeleigenschappen Service opvragen](https://msdn.microsoft.com/library/hh452238.aspx)en [Bestandseigenschappen Service ophalen](https://msdn.microsoft.com/library/mt427369.aspx) gebruiken voor opslag Analytics inschakelen voor elke service.

De verzamelde gegevens wordt opgeslagen in een bekende blob (voor logboekregistratie) en bekende tabellen (metrics), waarmee toegang kunnen worden verkregen met behulp van de Blob-service en de tabel service-API's.

Opslag Analytics mag maximaal 20 TB op de hoeveelheid opgeslagen gegevens, die onafhankelijk is van de totale limiet voor uw opslag-account. Zie voor meer informatie over facturering en bewaarbeleid data [Storage, Analytics en facturering](https://msdn.microsoft.com/library/hh360997.aspx). Zie voor meer informatie over limieten voor opslag, [Azure opslag schaalbaarheid en prestaties, doelen](storage-scalability-targets.md).

Zie voor een uitgebreide handleiding over het gebruik van Analytics voor opslag en andere hulpprogramma's te identificeren, opsporen en oplossen van problemen met Azure opslag met [Monitor, vaststellen, en Microsoft Azure opslag oplossen](storage-monitoring-diagnosing-troubleshooting.md).


## <a name="about-storage-analytics-logging"></a>Over Analytics opslag registreren

Opslag Analytics registreert gedetailleerde informatie over geslaagde en mislukte aanvragen met storage-service. Deze informatie kan worden gebruikt voor het controleren van individuele verzoeken en storage-service-problemen op te sporen. Aanvragen worden vastgelegd op basis van best effort.

Vermeldingen in het logboek worden alleen gemaakt als storage service-activiteit is. Bijvoorbeeld, als een rekening opslag activiteit in de Blob-service, maar niet in de tabel of de wachtrij services heeft, wordt zich aanmeldt met betrekking tot de Blob-service gemaakt.

Opslag Analytics Logging is niet beschikbaar voor de Azure File Service.

### <a name="logging-authenticated-requests"></a>Van geverifieerde logboekregistratieaanvragen

De volgende typen geverifieerde aanvragen worden vastgelegd:

- Geslaagde aanvragen.

- Mislukte aanvragen, met inbegrip van de time-out, bandbreedtebeperking, netwerk, autorisatie en andere fouten.

- Aanvragen met behulp van een gedeelde Access handtekening (SAS), met inbegrip van mislukte en succesvolle aanvragen.

- Aanvragen voor analytics-gegevens.

Aanvragen die zijn ingediend door opslag Analytics zelf, zoals logboek maken of verwijderen, worden niet geregistreerd. Een volledige lijst met gegevens in het logboek wordt beschreven in de onderwerpen over [opslag Analytics geregistreerd en statusberichten](https://msdn.microsoft.com/library/hh343260.aspx) en [Opslag Analytics logboekindeling](https://msdn.microsoft.com/library/hh343259.aspx) .

### <a name="logging-anonymous-requests"></a>Anonieme aanvragen voor registratie
De volgende soorten anonieme aanvragen worden geregistreerd:

- Geslaagde aanvragen.

- Server-fouten.

- Time-outfouten voor zowel client als server.

- Mislukte aanvragen ophalen met foutcode 304 (niet gewijzigd).

Andere defecte anonieme aanvragen worden niet geregistreerd. Een volledige lijst met gegevens in het logboek wordt beschreven in de onderwerpen over [opslag Analytics geregistreerd en statusberichten](https://msdn.microsoft.com/library/hh343260.aspx) en [Opslag Analytics logboekindeling](https://msdn.microsoft.com/library/hh343259.aspx) .

### <a name="how-logs-are-stored"></a>Hoe de logboeken worden opgeslagen
Alle logboeken worden opgeslagen in de BLOB's blokkeren in een container met de naam $logs, die automatisch wordt gemaakt wanneer de opslag Analytics is ingeschakeld voor de account van een opslag. De container $logs bevindt zich in de blob-naamruimte van de opslag-account, bijvoorbeeld: `http://<accountname>.blob.core.windows.net/$logs`. Deze container kan niet worden verwijderd nadat de opslag Analytics is ingeschakeld, maar de inhoud ervan kunnen worden verwijderd.

>[Azure.NOTE] De container $logs wordt niet weergegeven wanneer een container waarin de bewerking wordt uitgevoerd, zoals de [ListContainers](https://msdn.microsoft.com/library/azure/dd179352.aspx) -methode. Het moet rechtstreeks worden geopend. Bijvoorbeeld, kunt u de methode [ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx) voor toegang tot de BLOB's in de `$logs` container.
Als aanvragen worden geregistreerd, zal opslag Analytics tussenliggende resultaten uploaden als blokken. Opslag Analytics wordt periodiek, verbindt zich ertoe deze blokken en als een blob beschikbaar maken.

Dubbele records kunnen bestaan voor logboeken gemaakt in hetzelfde uur. U kunt bepalen of een record een duplicaat is aan de hand van het nummer van de **aanvraag-id** en de **werking** .

### <a name="log-naming-conventions"></a>Aanmelden voor naamgeving
Elk logboek wordt geschreven in de volgende indeling.

    <service-name>/YYYY/MM/DD/hhmm/<counter>.log

De volgende tabel wordt elk kenmerk in de naam van het logboek beschreven.

| Kenmerk         | Beschrijving                                                                                                                                                                                   |
|----------------   |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------   |
| < Servicenaam >    | De naam van de storage-service. Bijvoorbeeld: blob, tabel of wachtrij.                                                                                                                          |
| JJJJ              | Het jaartal met vier cijfers voor het logboek. Bijvoorbeeld: 2011.                                                                                                                                           |
| MM                | De maand in twee cijfers voor het logboek. Bijvoorbeeld: 07.                                                                                                                                             |
| DD                | De maand in twee cijfers voor het logboek. Bijvoorbeeld: 07.                                                                                                                                             |
| hh                | Het uur van de twee cijfers die het eerste uur voor de logboeken in 24-uursnotatie UTC aangeeft. Bijvoorbeeld: 18.                                                                                     |
| mm                | Het getal van twee cijfers die de eerste minuut voor de logboeken aangeeft. Deze waarde wordt niet ondersteund in de huidige versie van Analytics van de opslag en de waarde ervan is 00.     |
| <counter>         | Een op nul gebaseerde teller zes cijfers die hoeveel logboek BLOB's gegenereerd voor de storage-service in de periode van een uur aangeeft. Deze teller begint bij 000000. Bijvoorbeeld: 000001.     |

Het volgende is een compleet logboek Voorbeeldnaam die de bovenstaande voorbeelden combineert.

    blob/2011/07/31/1800/000001.log

Het volgende is een voorbeeld van een URI die kan worden gebruikt voor toegang tot de vorige logboek.

    https://<accountname>.blob.core.windows.net/$logs/blob/2011/07/31/1800/000001.log

Wanneer een aanvraag voor de opslag wordt geregistreerd, is de resulterende logboeknaam betrekking heeft op het uur waarop de gevraagde bewerking is voltooid. Bijvoorbeeld, als een GetBlob-verzoek is voltooid om 6:30 uur op 31-7/2011, zou het logboekbestand worden geschreven met de volgende prefix:`blob/2011/07/31/1800/`

### <a name="log-metadata"></a>Metagegevens in het logboek
Met metagegevens die kan worden gebruikt om te bepalen welke gegevens de blob bevat alle logboekbestanden BLOB's opgeslagen. In de volgende tabel wordt elk kenmerk metadata beschreven.

| Kenmerk     | Beschrijving                                                                                                                                                                                                                                                   |
|------------   |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| LogType       | Beschrijving van of het logboek informatie over lezen bevat, schrijven of te verwijderen. Deze waarde kan een bepaald type of een combinatie van alle drie, gescheiden door komma's bevatten. Voorbeeld 1: schrijven; Voorbeeld 2: lezen, schrijven; Voorbeeld 3: lezen, schrijven, verwijderen.    |
| Starttijd     | De vroegste tijd van een vermelding in het logboek, in de vorm van jjjj-MM-ddTHH. Bijvoorbeeld: 2011-07-31T18:21:46Z.                                                                                                                                             |
| Eindtijd       | De laatste tijd van een vermelding in het logboek, in de vorm van jjjj-MM-ddTHH. Bijvoorbeeld: 2011-07-31T18:22:09Z.                                                                                                                                               |
| LogVersion    | De versie van de indeling voor logboekbestanden. Op dit moment is de enige ondersteunde waarde 1,0.                                                                                                                                                                                     |

De onderstaande lijst staan de volledige monster metagegevens met behulp van de voorgaande voorbeelden.

- LogType = schrijven

- StartTime = 2011-07-31T18:21:46Z

- EndTime = 2011-07-31T18:22:09Z

- LogVersion = 1,0

### <a name="accessing-logging-data"></a>Toegang tot gegevens

Alle gegevens in de `$logs` container met behulp van de Blob service API's toegankelijk, met inbegrip van de .NET API's die door de Azure beheerde bibliotheek. De accountbeheerder opslag kunt lezen en verwijderen logboeken, maar kan maken of bijwerken. Zowel de metagegevens van het logboek als naam van het logboek kunnen worden gebruikt bij het opvragen van een logboek. Het is mogelijk voor een bepaalde tijd logboeken worden onjuist weergegeven, maar de metagegevens bevat altijd het interval van vermeldingen in het logboek in een logboek. Daarom kunt u een combinatie van namen van logboekbestanden en metagegevens tijdens het zoeken naar een bepaald logboek.

## <a name="about-storage-analytics-metrics"></a>Over opslag Analytics metrics

Opslag Analytics metrics met transactie geaggregeerde statistieken en capaciteit gegevens over aanvragen voor een storage-service kunnen worden opgeslagen. Transacties worden gemeld op zowel de API bewerking niveau en op het niveau van de service opslag en capaciteit op het niveau van de service opslag wordt gerapporteerd. Metrische gegevens kunnen worden gebruikt analyseren opslag gebruik van de service, het vaststellen van problemen met aanvragen ten opzichte van de storage-service en voor het verbeteren van de prestaties van toepassingen die gebruikmaken van een service.

Analytics voor opslag gebruikt, moet u deze inschakelen afzonderlijk voor elke service die u wilt controleren. Vanuit de [Azure Portal](https://portal.azure.com)kunt u deze inschakelen. Zie voor meer informatie, [een account voor opslag in de Portal Azure Monitor](storage-monitor-storage-account.md). Ook kunt u Analytics opslag via programmering via de REST API of de clientbibliotheek. De **Eigenschappen Service** -bewerkingen gebruiken voor opslag Analytics inschakelen voor elke service.

### <a name="transaction-metrics"></a>Statistieken van de transactie

Een robuuste set gegevens is tussenpozen per uur of minuut voor elke opslagservice en gevraagde API-bewerking, waaronder ingress/egress, beschikbaarheid, fouten, en gecategoriseerd verzoek percentages. Hier ziet u een volledige lijst van de transactiegegevens in de [Opslag Analytics Metrics tabelschema](https://msdn.microsoft.com/library/hh343264.aspx) onderwerp.

Transactiegegevens is op twee niveaus: het serviceniveau en het niveau van de bewerking API vastgelegd. Op het serviceniveau van gevraagde statistieken samenvatting van alle API-bewerkingen worden geschreven naar een Tabelentiteit elk uur zelfs als er geen aanvragen zijn aangebracht in de service. Op het niveau van de bewerking API statistische gegevens weggeschreven naar een entiteit als de bewerking is aangevraagd binnen dat uur.

Bijvoorbeeld als u een **GetBlob** bewerking op uw service Blob uitvoeren, wordt opslag Analytics Metrics de aanvraag melden en opnemen in de geaggregeerde gegevens voor zowel de Blob-service als de bewerking **GetBlob** . Echter als er geen bewerking **GetBlob** tijdens het uur wordt aangevraagd, een entiteit wordt niet geschreven aan de `$MetricsTransactionsBlob` voor de bewerking.

Transactie parameters worden geregistreerd voor zowel gebruikersaanvragen en verzoeken opslag Analytics zelf. Bijvoorbeeld worden aanvragen door opslag Analytics schrijven logboeken en tabel entiteiten opgenomen. Zie voor meer informatie over hoe deze verzoeken worden gefactureerd, [Storage, Analytics en facturering](https://msdn.microsoft.com/library/hh360997.aspx).

### <a name="capacity-metrics"></a>Capaciteit metrics

>[AZURE.NOTE] Capaciteit metrics zijn momenteel alleen beschikbaar voor de Blob-service. Capaciteit metrics voor de tabel service en service van de wachtrij zijn beschikbaar in toekomstige versies van Analytics voor opslag.

Capaciteit gegevens dagelijks voor Blob-service van de account van een opslag is geregistreerd en twee tabel entiteiten worden geschreven. Één entiteit kunt u statistieken voor gebruikersgegevens en de andere bevat statistieken over de `$logs` gebruikt door opslag Analytics blob-container. De `$MetricsCapacityBlob` tabel bevat de volgende statistieken:

- **Capaciteit**: de hoeveelheid opslagruimte die wordt gebruikt door de service Blob storage-account, in bytes.

- **ContainerCount**: het aantal blob-containers in de Blob-service van de account van de opslag.

- **ObjectCount**: het aantal vastgelegde en niet-doorgevoerde blokkeren of pagina BLOB's in de Blob-service van de account van de opslag.

Zie voor meer informatie over de cijfers over de capaciteit [Opslag Analytics Metrics tabelschema](https://msdn.microsoft.com/library/hh343264.aspx).

### <a name="how-metrics-are-stored"></a>Hoe gegevens worden opgeslagen

Alle metrische gegevens voor elk van de storage-services worden opgeslagen in drie tabellen gereserveerd voor die service: één tabel voor informatie over één tabel voor minuut transactie-informatie en informatie over capaciteit in een andere tabel. Transactie en minuut transactie-informatie van de aanvraag en het antwoord bestaat en informatie over capaciteit van gebruiksgegevens opslag bestaat. Maatstaven voor uur, minuut metrics en capaciteit van Blob-service van de account van een opslag is toegankelijk in de tabellen die worden genoemd in de volgende tabel beschreven.

| Metrics niveau                         | Tabelnamen                                                                                                                   | Ondersteunde versies                                                                                                                        |
|------------------------------------   |-----------------------------------------------------------------------------------------------------------------------------  |---------------------------------------------------------------------------------------------------------------------------------------------- |
| Statistieken per uur, primaire locatie      |  $MetricsTransactionsBlob <br/>$MetricsTransactionsTable <br/> $MetricsTransactionsQueue                                                  | Versies van vóór 2013-08-15 alleen. Terwijl deze namen worden nog steeds ondersteund, het raadzaam dat u overschakelt naar de hierna vermelde tabellen gebruiken.  |
| Statistieken per uur, primaire locatie      | $MetricsHourPrimaryTransactionsBlob <br/>$MetricsHourPrimaryTransactionsTable <br/>$MetricsHourPrimaryTransactionsQueue               | Alle versies, inclusief 2013-08-15.                                                                                                               |
| Minuut metrics, primaire locatie      | $MetricsMinutePrimaryTransactionsBlob <br/>$MetricsMinutePrimaryTransactionsTable <br/>$MetricsMinutePrimaryTransactionsQueue         | Alle versies, inclusief 2013-08-15.                                                                                                               |
| Statistieken per uur, secundaire locatie    | $MetricsHourSecondaryTransactionsBlob <br/>$MetricsHourSecondaryTransactionsTable <br/>$MetricsHourSecondaryTransactionsQueue         | Alle versies, inclusief 2013-08-15. Leestoegang tot geo-redundante replicatie moet zijn ingeschakeld.                                                    |
| Minuut metrics, secundaire locatie    | $MetricsMinuteSecondaryTransactionsBlob <br/>$MetricsMinuteSecondaryTransactionsTable <br/>$MetricsMinuteSecondaryTransactionsQueue   | Alle versies, inclusief 2013-08-15. Leestoegang tot geo-redundante replicatie moet zijn ingeschakeld.                                                    |
| Capaciteit (alleen Blob-service)          | $MetricsCapacityBlob                                                                                                          | Alle versies, inclusief 2013-08-15.                                                                                                               |


Deze tabellen worden automatisch gemaakt wanneer opslag Analytics is ingeschakeld voor de account van een opslag. Ze toegankelijk zijn via de naamruimte van de opslag-account, bijvoorbeeld:`https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")`

### <a name="accessing-metrics-data"></a>Toegang tot metrische gegevens

Alle gegevens in de tabel parameters met behulp van de tabel service-API's toegankelijk, met inbegrip van de .NET API's die door de Azure beheerde bibliotheek. De accountbeheerder opslag kunt lezen en verwijderen tabel entiteiten, maar kan maken of bijwerken.

## <a name="billing-for-storage-analytics"></a>Facturering van opslag Analytics

Opslag Analytics is ingeschakeld door de eigenaar van een opslag account; het is niet standaard ingeschakeld. Alle metrische gegevens worden geschreven door de diensten van een rekening voor opslag. Hierdoor is elke schrijfbewerking uitgevoerd door Analytics opslag te factureren. Daarnaast is er ook de hoeveelheid opslagruimte die wordt gebruikt door de metrische gegevens te factureren.

De volgende acties uitgevoerd door opslag Analytics zijn factureerbare:

- Aanvragen voor het BLOB's voor het vastleggen van maken. 

- Aanvragen voor entiteiten voor metrieken tabel maken.

Als u een bewaarbeleid gegevens hebt geconfigureerd, u zijn niet in rekening gebracht voor verwijdertransacties wanneer opslag Analytics oude logboekregistratie en metrische gegevens worden verwijderd. Verwijdertransacties vanaf een client zijn echter te factureren. Zie [een bewaarbeleid opslag Analytics gegevens instellen](https://msdn.microsoft.com/library/azure/hh343263.aspx)voor meer informatie over bewaarbeleid.

### <a name="understanding-billable-requests"></a>Wat te factureren aanvragen?

Elk verzoek tot storage-service van een account is te factureren of niet-factureerbare. Opslag Analytics registreert elke afzonderlijke verzoek aan een service, met inbegrip van een statusbericht dat aangeeft hoe de aanvraag is verwerkt. Op dezelfde manier opgeslagen opslag Analytics metrics voor een service en de bewerkingen van de API van die dienst, met inbegrip van de percentages en het aantal berichten status. Samen vormen kunt deze functies u analyseren uw verzoeken te factureren, verbeteringen aanbrengen in uw toepassing en vaststellen van problemen met aanvragen voor uw service. Voor meer informatie over facturering, Zie [Wat Azure opslag facturering - bandbreedte, transacties, en capaciteit](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

Wanneer opslag Analytics-gegevens worden weergegeven, kunt u de tabellen in het onderwerp [opslag Analytics geregistreerde bewerkingen en statusberichten](https://msdn.microsoft.com/library/azure/hh343260.aspx) om te bepalen welke verzoeken te factureren. Vervolgens kunt u de logboeken en de statusberichten te zien als u voor een bepaalde aanvraag werd aangerekend metrische gegevens vergelijken. Kun je de tabellen in het vorige onderwerp bij het onderzoeken van de beschikbaarheid van een storage-service of afzonderlijke API-bewerking.

## <a name="next-steps"></a>Volgende stappen

### <a name="setting-up-storage-analytics"></a>Opslag Analytics instellen
- [Een account voor opslag in de Portal Azure controleren](storage-monitor-storage-account.md)
- [Inschakelen en configureren van opslag Analytics](https://msdn.microsoft.com/library/hh360996.aspx)

### <a name="storage-analytics-logging"></a>Opslag Analytics logboekregistratie  
- [Over opslag Analytics logboekregistratie](https://msdn.microsoft.com/library/hh343262.aspx)
- [Opslag Analytics logboekbestandsindeling](https://msdn.microsoft.com/library/hh343259.aspx)
- [Opslag Analytics geregistreerd bewerkingen en statusberichten](https://msdn.microsoft.com/library/hh343260.aspx)

### <a name="storage-analytics-metrics"></a>Opslag Analytics metrics
- [Over opslag Analytics Metrics](https://msdn.microsoft.com/library/hh343258.aspx)
- [Opslag Analytics Metrics tabelschema](https://msdn.microsoft.com/library/hh343264.aspx)
- [Opslag Analytics geregistreerd bewerkingen en statusberichten](https://msdn.microsoft.com/library/hh343260.aspx)  
