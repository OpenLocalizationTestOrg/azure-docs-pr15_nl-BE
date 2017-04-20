<properties
    pageTitle="Het logboek voor faxactiviteit Azure archiveren | Microsoft Azure"
    description="Informatie over het archiveren van uw activiteitenlogboek Azure voor lange termijn bewaren in een opslag-account."
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
    ms.date="08/23/2016"
    ms.author="johnkem"/>

# <a name="archive-the-azure-activity-log"></a>Het logboek voor faxactiviteit Azure archiveren
In dit artikel zien we hoe u kunt de portal voor Azure, PowerShell-Cmdlets of CLI Cross-Platform voor het archiveren van uw [**Activiteitenlogboek Azure**](monitoring-overview-activity-logs.md) in een opslag-account. Deze optie is handig als u langer dan 90 dagen (met volledige controle over het bewaarbeleid) voor controle-, statische analyse of back-up van uw activiteitenlogboek behouden. Als u hoeft alleen de gebeurtenissen worden bewaard gedurende 90 dagen of minder niet hoeft om in te stellen naar een opslag, archivering sinds activiteitenlogboek gebeurtenissen worden bewaard in de Azure platform voor 90 dagen zonder Sleutelarchivering inschakelen.

## <a name="prerequisites"></a>Vereisten
Voordat u begint, moet u [een account opslag maken](../storage/storage-create-storage-account.md#create-a-storage-account) waaraan u het logboek voor faxactiviteit kunt archiveren. Het is raadzaam dat u een bestaande account voor opslag met andere, niet-controle gegevens zodat u kunt beter toegang tot de controle van gegevens niet gebruiken. Echter, als u ook diagnostische logboeken en statistieken op een rekening van opslag bewaart, het kan zinvol dat account opslag voor uw activiteitenlogboek gebruiken om te houden van alle controlegegevens op een centrale locatie. De opslag-account die u gebruikt moet een algemene opslag-account niet een account in de blob-opslag.

## <a name="log-profile"></a>Logboek profiel
Als u wilt archiveren in het logboek voor faxactiviteit met behulp van de onderstaande methoden, stelt u het **Profiel aanmelden** voor een abonnement. Het logboek profiel definieert het type van de gebeurtenissen die zijn opgeslagen of gestreamd en de uitvoer, opslag account en/of gebeurtenis hub. Het bewaarbeleid (aantal dagen wilt behouden) wordt gedefinieerd voor gebeurtenissen die zijn opgeslagen in een opslagruimte-account. Als het bewaarbeleid op nul is ingesteld, worden gebeurtenissen voor onbepaalde tijd opgeslagen. Dit kan anders worden ingesteld op een waarde tussen 1 en 2147483647. [Kunt u meer informatie over het logboek profielen hier](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles).

## <a name="archive-the-activity-log-using-the-portal"></a>Het logboek voor faxactiviteit met behulp van de portal archiveren
1. Klik op de koppeling **Activiteitenlogboek** op de linkerkant navigatie in de portal. Als u een koppeling voor het activiteitenlogboek niet ziet, klikt u eerst op de koppeling **Meer Services** .

    ![Ga naar het logboek voor faxactiviteit blade](media/monitoring-archive-activity-log/act-log-portal-navigate.png)
2. Aan de bovenkant van het blad en klik op **exporteren**.

    ![Klik op de knop exporteren](media/monitoring-archive-activity-log/act-log-portal-export-button.png)
3. In het blad dat wordt weergegeven, het selectievakje **exporteren naar een opslag-account** en selecteer een account opslag.

    ![Een opslag-account instellen](media/monitoring-archive-activity-log/act-log-portal-export-blade.png)
4. Met de schuifregelaar of een tekstvak, een aantal dagen waarvoor activiteit gebeurtenissen moeten worden bewaard in uw account voor de opslag definiëren. Als u liever uw gegevens persistent gemaakt in de opslag voor onbepaalde tijd, dit aantal ingesteld op nul.
5. Klik op **Opslaan**.

## <a name="archive-the-activity-log-via-powershell"></a>Het logboek voor faxactiviteit via PowerShell archiveren
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus -RetentionInDays 180 -Categories Write,Delete,Action
```

| Eigenschap         | Vereist | Beschrijving                                                                                                                                                                                                                                                                                       |
|------------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| StorageAccountId | Nee       | Bron-ID van de opslag die activiteit logboekbestanden moeten worden opgeslagen.                                                                                                                                                                                                                        |
| Locaties        | Ja      | Door komma's gescheiden lijst met regio's die u graag wilt verzamelen van gebeurtenissen in logboek voor faxactiviteit. U kunt een lijst met alle regio's [door deze pagina te bezoeken](https://azure.microsoft.com/en-us/regions) of met behulp van [De API Azure Management REST](https://msdn.microsoft.com/library/azure/gg441293.aspx)weergeven. |
| RetentionInDays  | Ja      | Aantal dagen voor welke gebeurtenissen moeten worden gehandhaafd, tussen 1 en 2147483647. De waarde nul worden de logboeken voor onbepaalde tijd opgeslagen (altijd).                                                                                                                                                                                             |
| Categorieën       | Ja      | Door komma's gescheiden lijst met de gebeurteniscategorieën die moeten worden verzameld. Mogelijke waarden zijn schrijven, verwijderen en actie.                                                                                                                                                                                 |
## <a name="archive-the-activity-log-via-cli"></a>Het logboek voor faxactiviteit via CLI archief
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --locations global,westus,eastus,northeurope --retentionInDays 180 –categories Write,Delete,Action
```

| Eigenschap        | Vereist | Beschrijving                                                                                                                                                                                                                                                                                       |
|-----------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| naam            | Ja      | De naam van uw profiel van het logboek.                                                                                                                                                                                                                                                                         |
| storageId       | Nee       | Bron-ID van de opslag die activiteit logboekbestanden moeten worden opgeslagen.                                                                                                                                                                                                                        |
| locaties       | Ja      | Door komma's gescheiden lijst met regio's die u graag wilt verzamelen van gebeurtenissen in logboek voor faxactiviteit. U kunt een lijst met alle regio's [door deze pagina te bezoeken](https://azure.microsoft.com/en-us/regions) of met behulp van [De API Azure Management REST](https://msdn.microsoft.com/library/azure/gg441293.aspx)weergeven. |
| retentionInDays | Ja      | Aantal dagen voor welke gebeurtenissen moeten worden gehandhaafd, tussen 1 en 2147483647. De waarde nul voor onbepaalde tijd de logboeken worden opgeslagen (altijd).                                                                                                                                                                                             |
| categorieën      | Ja      | Door komma's gescheiden lijst met de gebeurteniscategorieën die moeten worden verzameld. Mogelijke waarden zijn schrijven, verwijderen en actie.                                                                                                                                                                                 |

## <a name="storage-schema-of-the-activity-log"></a>Schema van de opslag van het activiteitenlogboek
Nadat u hebt ingesteld archivering, wordt een opslag container gemaakt in de opslag-account als een gebeurtenis in het logboek voor faxactiviteit. Het aantal BLOB's in de container als volgt dezelfde indeling in het logboek voor faxactiviteit en diagnostische logboeken. De structuur van deze BLOB's is:

> Insights-operationele-logboeken/naam = standaard/resourceId = / ABONNEMENTEN / {abonnements-ID} / y = {jaartal met vier cijfers numerieke} / m = {twee cijfers numerieke maand} / d = {numerieke dag twee cijfers} / h = {tweecijferige 24-uurs klok hour}/m=00/PT1H.json

De naam van een blob kan bijvoorbeeld zijn:

> Insights-Operational-Logs/name=default/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.JSON

Elke PT1H.json blob bevat een blob JSON van gebeurtenissen die hebben plaatsgevonden binnen het uur dat is opgegeven in de blob-URL (bijv. h = 12). Tijdens het huidige uur gebeurtenissen naar het bestand PT1H.json toegevoegd wanneer deze optreden. De waarde van de minuut (m = 00) is altijd 00, omdat het activiteitenlogboek gebeurtenissen worden onderverdeeld in afzonderlijke BLOB's per uur.

Elke gebeurtenis wordt in het bestand PT1H.json opgeslagen in de matrix "records", volgens deze indeling:

```
{
    "records": [
        {
            "time": "2015-01-21T22:14:26.9792776Z",
            "resourceId": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
            "operationName": "microsoft.support/supporttickets/write",
            "category": "Write",
            "resultType": "Success",
            "resultSignature": "Succeeded.Created",
            "durationMs": 2826,
            "callerIpAddress": "111.111.111.11",
            "correlationId": "c776f9f4-36e5-4e0e-809b-c9b3c3fb62a8",
            "identity": {
                "authorization": {
                    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
                    "action": "microsoft.support/supporttickets/write",
                    "evidence": {
                        "role": "Subscription Admin"
                    }
                },
                "claims": {
                    "aud": "https://management.core.windows.net/",
                    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
                    "iat": "1421876371",
                    "nbf": "1421876371",
                    "exp": "1421880271",
                    "ver": "1.0",
                    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
                    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
                    "puid": "20030000801A118C",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
                    "name": "John Smith",
                    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
                    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
                    "appidacr": "2",
                    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
                    "http://schemas.microsoft.com/claims/authnclassreference": "1"
                }
            },
            "level": "Information",
            "location": "global",
            "properties": {
                "statusCode": "Created",
                "serviceRequestId": "50d5cddb-8ca0-47ad-9b80-6cde2207f97c"
            }
        }
    ]
}
```


| Elementnaam    | Beschrijving                                                                                                    |
|-----------------|----------------------------------------------------------------------------------------------------------------|
| tijd            | Tijdstip waarop de gebeurtenis is gegenereerd door de verwerking van de aanvraag overeenkomstig de gebeurtenis Azure-service.    |
| resourceId      | Bron-ID van de resource die risico.                                                                          |
| operationName   | De naam van de bewerking.                                                                                         |
| categorie        | Categorie van de actie, bv. Schrijven, lezen, actie.                                                               |
| resultType      | Het type van het resultaat, bv. Geslaagd, mislukt, Start                                                            |
| resultSignature | Afhankelijk van het resourcetype.                                                                                  |
| durationMs      | Duur van de bewerking in milliseconden                                                                      |
| callerIpAddress | IP-adres van de gebruiker die de bewerking, UPN-claim of SPN claim op basis van beschikbaarheid is uitgevoerd.         |
| correlationId   | Meestal een GUID in de indeling van de tekenreeks. Gebeurtenissen met een correlationId, behoren tot dezelfde actie uber.         |
| identiteit        | JSON blob met een beschrijving van de vergunning en de vorderingen.                                                             |
| autorisatie   | BLOB van RBAC-eigenschappen van de gebeurtenis. Bevat meestal de eigenschappen 'actie', 'rol' en 'scope'.            |
| niveau           | Niveau van de gebeurtenis. Een van de volgende waarden: 'Kritiek', 'Fout', 'Waarschuwing', 'Informatie' en 'Uitgebreid' |
| locatie        | De regio in de locatie is opgetreden (of globaal).                                                             |
| Eigenschappen      | Instellen van `<Key, Value>` paren (dat wil zeggen woordenboek) met een beschrijving van de details van de gebeurtenis.                             |

> [AZURE.NOTE] De eigenschappen en het gebruik van deze eigenschappen kunnen variëren afhankelijk van de bron.

## <a name="next-steps"></a>Volgende stappen
- [BLOB's voor analyse downloaden](../storage/storage-dotnet-how-to-use-blobs.md#download-blobs)
- [Het logboek voor faxactiviteit met Hubs gebeurtenis Stream](monitoring-stream-activity-logs-event-hubs.md)
- [Lees meer over het logboek voor faxactiviteit](monitoring-overview-activity-logs.md)
