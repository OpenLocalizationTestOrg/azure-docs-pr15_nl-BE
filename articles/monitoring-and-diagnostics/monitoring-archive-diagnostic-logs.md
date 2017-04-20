<properties
    pageTitle="Azure diagnostische logboeken archiveren | Microsoft Azure"
    description="Informatie over het archiveren van uw Azure diagnostische logboeken voor de lange termijn bewaren in een opslag-account."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="johnkem"/>

# <a name="archive-azure-diagnostic-logs"></a>Azure diagnostische logboeken archiveren
In dit artikel zien we hoe u kunt de portal voor Azure, PowerShell-Cmdlets, CLI of REST API voor het archiveren van uw [Azure diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md) in een opslag-account. Deze optie is handig als u de diagnostische logboeken met een optionele bewaarbeleid voor controle-, statische analyse of back-up te bewaren.

## <a name="prerequisites"></a>Vereisten
Voordat u begint, moet u [een account opslag maken](../storage/storage-create-storage-account.md#create-a-storage-account) waaraan u de diagnostische logboeken kunt archiveren. Het is raadzaam dat u een bestaande account voor opslag met andere, niet-controle gegevens zodat u kunt beter toegang tot de controle van gegevens niet gebruiken. Echter, als u het logboek voor faxactiviteit en diagnostische metrics naar een opslag ook archiveert, het kan zinvol dat account opslag voor de diagnostische logboeken ook gebruiken om te houden van alle controlegegevens op een centrale locatie. De opslag-account die u gebruikt moet een algemene opslag-account niet een account in de blob-opslag.

## <a name="diagnostic-settings"></a>Diagnostische instellingen
Als u wilt archiveren uw diagnostische logboeken met behulp van de onderstaande methoden, kunt u een **Diagnostische instelling** voor een bepaalde resource instellen. Een diagnose stellen voor een resource de categorieën definieert die is opgeslagen of gestreamd en de uitvoer legt — opslag account en/of gebeurtenis hub. Het bewaarbeleid (aantal dagen wilt behouden) wordt gedefinieerd voor gebeurtenissen van elke categorie logboek is opgeslagen in een opslagruimte-account. Als een bewaarbeleid is ingesteld op nul, worden gebeurtenissen voor die categorie logboekbestanden opgeslagen voor onbepaalde tijd (dat wil zeggen, permanent). Een bewaarbeleid kan een willekeurig aantal dagen tussen 1 en 2147483647 anders zijn. [Kunt u meer informatie over Diagnostische instellingen hier](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings).

## <a name="archive-diagnostic-logs-using-the-portal"></a>De diagnostische logboeken archiveren met behulp van de portal

1. Klik in het blad van de resource voor de resource die u wilt inschakelen van diagnostische logboeken archiveren in de portal.
2. Selecteer in de sectie **bewaking** van de resource-instellingen menu **Diagnostische gegevens**.

    ![Monitoring van de sectie van het menu bron](media/monitoring-archive-diagnostic-logs/diag-log-monitoring-sec.png)
3. Schakel het selectievakje in voor het **exporteren naar opslag Account**en selecteer vervolgens een opslag-account. Stel desgewenst een aantal dagen te behouden deze logboeken met behulp van de **bewaren (dagen)** schuifregelaars. Een inhouding van nul dagen slaat de logboeken voor onbepaalde tijd.

    ![Diagnostische logboeken blade](media/monitoring-archive-diagnostic-logs/diag-log-monitoring-blade.png)
4. Klik op **Opslaan**.

Diagnoselogboeken archief opslag account zodra nieuwe gebeurtenisgegevens is gegenereerd.

## <a name="archive-diagnostic-logs-via-the-powershell-cmdlets"></a>Diagnostische logboeken archiveren via de PowerShell-Cmdlets

```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg -StorageAccountId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Categories networksecuritygroupevent,networksecuritygrouprulecounter -Enabled $true -RetentionEnabled $true -RetentionInDays 90
```

| Eigenschap         | Vereist | Beschrijving                                                                                           |
|------------------|----------|-------------------------------------------------------------------------------------------------------|
| ResourceId       | Ja      | Bron-ID van de resource die u wilt instellen, een diagnostische instelling.                            |
| StorageAccountId | Nee       | Bron-ID van de opslag-Account waarin u de diagnostische logboeken worden opgeslagen.                          |
| Categorieën       | Nee       | Door komma's gescheiden lijst met categorieën logboek inschakelen.                                                     |
| Ingeschakeld          | Ja      | Een Boolean die aangeeft of de diagnostische gegevens worden in- of uitgeschakeld voor deze resource.                  |
| RetentionEnabled | Nee       | Een Boolean die aangeeft of een bewaarbeleid zijn ingeschakeld op deze bron.                            |
| RetentionInDays  | Nee       | Het aantal dagen dat gebeurtenissen moeten worden gehandhaafd tussen 1 en 2147483647. De waarde nul bevat de logboeken voor onbepaalde tijd. |

## <a name="archive-the-activity-log-via-the-cross-platform-cli"></a>Het logboek voor faxactiviteit via de platforms CLI archiveren

```
azure insights diagnostic set --resourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg --storageId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage –categories networksecuritygroupevent,networksecuritygrouprulecounter --enabled true
```

| Eigenschap         | Vereist | Beschrijving                                                                                           |
|------------------|----------|-------------------------------------------------------------------------------------------------------|
| resourceId       | Ja      | Bron-ID van de resource die u wilt instellen, een diagnostische instelling.                            |
| storageId        | Nee       | Bron-ID van de opslag-Account waarin u de diagnostische logboeken worden opgeslagen.                          |
| categorieën       | Nee       | Door komma's gescheiden lijst met categorieën logboek inschakelen.                                                     |
| ingeschakeld          | Ja      | Een Boolean die aangeeft of de diagnostische gegevens worden in- of uitgeschakeld voor deze resource.                  |

## <a name="archive-diagnostic-logs-via-the-rest-api"></a>Diagnostische logboeken archiveren via de REST API
[Zie dit document](https://msdn.microsoft.com/library/azure/dn931931.aspx) voor meer informatie over hoe u een diagnostische instelling met de Azure Monitor REST API kunt instellen.

## <a name="schema-of-diagnostic-logs-in-the-storage-account"></a>Schema van de diagnostische logboeken in de opslag
Nadat u hebt ingesteld archivering, is een container voor de opslag in de opslag gemaakt als een gebeurtenis in het logboek categorieën die u hebt ingeschakeld. De BLOB's in de container als volgt dezelfde indeling in diagnostische logboeken en het logboek voor faxactiviteit. De structuur van deze BLOB's is:

> Insights - logboeken - {categorie logboeknaam} / = resourceId/ABONNEMENTEN / {abonnements-ID} /RESOURCEGROUPS/ {Resourcegroepnaam} /PROVIDERS/ {bron providernaam} / {resource type} / {resourcenaam} / y = {jaartal met vier cijfers numerieke} / m = {twee cijfers numerieke maand} / d = {numerieke dag twee cijfers} / h = {tweecijferige 24-uurs klok hour}/m=00/PT1H.json

Of meer gewoon

> Insights - logboeken - {categorie logboeknaam} / resourceId = / {resource-Id} / y = {jaartal met vier cijfers numerieke} / m = {twee cijfers numerieke maand} / d = {numerieke dag twee cijfers} / h = {tweecijferige 24-uurs klok hour}/m=00/PT1H.json

De naam van een blob kan bijvoorbeeld zijn:

> insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json

Elke PT1H.json blob bevat een blob JSON van gebeurtenissen die hebben plaatsgevonden binnen het uur dat is opgegeven in de blob-URL (bijvoorbeeld, h = 12). Tijdens het huidige uur gebeurtenissen naar het bestand PT1H.json toegevoegd wanneer deze optreden. De waarde van de minuut (m = 00) is altijd 00, aangezien diagnostische gebeurtenissen zijn onderverdeeld in afzonderlijke BLOB's per uur.

Elke gebeurtenis wordt in het bestand PT1H.json opgeslagen in de matrix "records", volgens deze indeling:

```
{
    "records": [
        {
            "time": "2016-07-01T00:00:37.2040000Z",
            "systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1",
            "category": "NetworkSecurityGroupRuleCounter",
            "resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG",
            "operationName": "NetworkSecurityGroupCounters",
            "properties": {
                "vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}",
                "subnetPrefix": "10.3.0.0/24",
                "macAddress": "000123456789",
                "ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp",
                "direction": "In",
                "type": "allow",
                "matchedConnections": 1988
            }
        }
    ]
}
```

| Elementnaam  | Beschrijving                                                                                                 |
|---------------|-------------------------------------------------------------------------------------------------------------|
| tijd          | Tijdstip waarop de gebeurtenis is gegenereerd door de verwerking van de aanvraag overeenkomstig de gebeurtenis Azure-service. |
| resourceId    | Bron-ID van de resource die risico.                                                                       |
| operationName | De naam van de bewerking.                                                                                      |
| categorie      | Categorie logboek van de gebeurtenis.                                                                                  |
| Eigenschappen    | Instellen van `<Key, Value>` paren (dat wil zeggen woordenboek) met een beschrijving van de details van de gebeurtenis.                            |

> [AZURE.NOTE] De eigenschappen en het gebruik van deze eigenschappen kunnen variëren afhankelijk van de bron.

## <a name="next-steps"></a>Volgende stappen
- [BLOB's voor analyse downloaden](../storage/storage-dotnet-how-to-use-blobs.md#download-blobs)
- [Diagnostische logboeken naar gebeurtenis Hubs Stream](monitoring-stream-diagnostic-logs-to-event-hubs.md)
- [Voor meer informatie over diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md)
