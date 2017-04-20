<properties
    pageTitle="Overzicht van het logboek voor faxactiviteit Azure | Microsoft Azure"
    description="Meer informatie over wat het activiteitenlogboek Azure is en hoe u kunt deze gebeurtenissen binnen uw abonnement Azure begrijpen."
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
    ms.date="10/25/2016"
    ms.author="johnkem"/>

# <a name="overview-of-the-azure-activity-log"></a>Overzicht van het logboek voor faxactiviteit Azure
Het **Logboek met faxactiviteit Azure** is een logboek dat inzicht geven in de bewerkingen die worden uitgevoerd op de resources in uw abonnement. Het logboek voor faxactiviteit voorheen bekend was als 'Controlelogboeken' of 'Operationele Logs' omdat het besturingsvlak gebeurtenissen voor uw abonnementen op rapporten. Met het logboek voor faxactiviteit, kunt u bepalen de ' wat, wie, en wanneer ' voor alle (PUT, POST, verwijderen schrijfbewerkingen) op de resources in uw abonnement genomen. U kunt ook de status van de bewerking en andere relevante eigenschappen begrijpen. Leesbewerkingen (GET) niet wordt opgenomen in het logboek voor faxactiviteit.

Het logboek voor faxactiviteit verschilt van [Diagnostische logboeken](./monitoring-overview-of-diagnostic-logs.md), die alle logboeken die zijn gegenereerd door een resource zijn. Deze logboeken kunt u gegevens over de werking van deze bron, in plaats van bewerkingen voor die resource.

U kunt gebeurtenissen ophalen uit het logboek voor faxactiviteit met behulp van de portal voor Azure CLI, PowerShell-cmdlets en Azure Monitor REST API.

Bekijk deze [video introductie van het logboek voor faxactiviteit](https://channel9.msdn.com/Blogs/Seth-Juarez/Logs-John-Kemnetz).  

## <a name="what-you-can-do-with-the-activity-log"></a>Wat u kunt doen met het logboek voor faxactiviteit
Hier zijn enkele dingen die u met het logboek voor faxactiviteit doen kunt:

- Opvragen en bekijken in het **portal Azure**.
- Deze query via een REST API, PowerShell-Cmdlet of CLI.
- [Maak een waarschuwing per e-mail of webhook die uit een logboek met faxactiviteit gebeurtenis activeert.](./insights-auditlog-to-webhook-email.md)
- [Opslaan op een **Rekening van opslag** voor archiveringsdoeleinden of handmatige controle](./monitoring-archive-activity-log.md). De retentietijd (in dagen) met behulp van **Profielen logboek**kunt u opgeven.
- Analyseren in PowerBI met het [**PowerBI content pack**](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/).
- [Streamen naar een **Gebeurtenis Hub** ](./monitoring-stream-activity-logs-event-hubs.md) voor opname door een service van derden of aangepaste analytics oplossing zoals PowerBI.

## <a name="export-the-activity-log-with-log-profiles"></a>Het logboek voor faxactiviteit met logboek profielen exporteren
Een **Logboek profiel** bepaalt hoe het logboek voor faxactiviteit is geëxporteerd. Een logboek profiel gebruikt, kunt u configureren:

- Waar het logboek moet worden verzonden (opslag Account of gebeurtenis Hubs)
- Welke gebeurteniscategorieën (schrijven, verwijderen, actie) moeten worden verzonden.
- Welke regio's (locatie) moeten worden geëxporteerd.
- Hoe lang het logboek voor faxactiviteit moet worden bewaard in een Account opslag – een inhouding van nul dagen betekent logboeken worden permanent bewaard. Anders is de waarde een willekeurig aantal dagen tussen 1 en 2147483647. Als het bewaarbeleid zijn ingesteld maar Logboeken opslaan in een opslag-Account is uitgeschakeld (bijvoorbeeld als alleen gebeurtenis Hubs of OMS opties zijn geselecteerd), hebben het bewaarbeleid geen effect.

Deze instellingen kunnen worden geconfigureerd via de optie 'Export' in het logboek voor faxactiviteit blade in de portal. Ze kunnen ook worden geconfigureerd via programmacode [met behulp van de API Azure Monitor REST](https://msdn.microsoft.com/library/azure/dn931927.aspx)PowerShell-cmdlets of CLI. Een abonnement kan slechts één logboek profiel hebben.

### <a name="configure-log-profiles-using-the-azure-portal"></a>Logboek-profielen met de Azure portal configureren
U kunt het logboek voor faxactiviteit op een Hub gebeurtenis streamen of opgeslagen in een opslagruimte-Account met behulp van de optie 'Export' in de portal Azure.

1. Ga naar het **Logboek voor faxactiviteit** blade via het menu aan de linkerkant van de portal.

    ![Ga naar het logboek voor faxactiviteit in portal](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Klik op de knop **exporteren** aan de bovenkant van het blad.

    ![De knop exporteren in de portal](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. In het blad dat wordt weergegeven, kunt u het volgende selecteren:  
    - regio's die u wilt exporteren, gebeurtenissen
    - de opslag-Account die u wilt opslaan van gebeurtenissen
    - het aantal dagen dat u wilt bewaren van deze gebeurtenissen in de opslag. Een instelling van 0 dagen behoudt altijd de logboeken.
    - de Bus Service Namespace waarin u een gebeurtenis Hub dat wilt moet worden gemaakt voor het streamen van deze gebeurtenissen.

    ![Logboek voor faxactiviteit blade exporteren](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Klik op **Opslaan** om deze instellingen opslaan. De instellingen worden direct worden toegepast op uw abonnement.

### <a name="configure-log-profiles-using-the-azure-powershell-cmdlets"></a>Logboek-profielen met de Azure PowerShell-Cmdlets configureren
#### <a name="get-existing-log-profile"></a>Bestaande logboek profiel ophalen
```
Get-AzureRmLogProfile
```

#### <a name="add-a-log-profile"></a>Een logboek profiel toevoegen
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

| Eigenschap         | Vereist | Beschrijving   |
|------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Naam             | Ja      | De naam van uw profiel van het logboek.                                 |
| StorageAccountId | Nee       | Bron-ID van de opslag-Account waaraan het logboek moet worden opgeslagen.                         |
| serviceBusRuleId | Nee       | Service Bus regel-ID voor de gewenste gebeurtenis hubs gemaakt in zijn Service Bus-naamruimte. Is een tekenreeks met de volgende notatie: `{service bus resource ID}/authorizationrules/{key name}`. |
| Locaties        | Ja      | Door komma's gescheiden lijst met regio's die u graag wilt verzamelen van gebeurtenissen in logboek voor faxactiviteit.              |
| RetentionInDays  | Ja      | Aantal dagen voor welke gebeurtenissen moeten worden gehandhaafd, tussen 1 en 2147483647. De waarde nul worden de logboeken voor onbepaalde tijd opgeslagen (altijd). |
| Categorieën       | Nee       | Door komma's gescheiden lijst met de gebeurteniscategorieën die moeten worden verzameld. Mogelijke waarden zijn schrijven, verwijderen en actie.                                 |

#### <a name="remove-a-log-profile"></a>Een logboek profiel verwijderen
```
Remove-AzureRmLogProfile -name my_log_profile
```

### <a name="configure-log-profiles-using-the-azure-cross-platform-cli"></a>Logboek profielen met behulp van de CLI Azure platforms configureren
#### <a name="get-existing-log-profile"></a>Bestaande logboek profiel ophalen
```
azure insights logprofile list
```
```
azure insights logprofile get --name my_log_profile
```
De `name` de eigenschap moet de naam van uw profiel van het logboek.

#### <a name="add-a-log-profile"></a>Een logboek profiel toevoegen
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

| Eigenschap         | Vereist | Beschrijving   |
|------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| naam             | Ja      | De naam van uw profiel van het logboek.                                 |
| storageId        | Nee       | Bron-ID van de opslag-Account waaraan het logboek moet worden opgeslagen.                         |
| serviceBusRuleId | Nee       | Service Bus regel-ID voor de gewenste gebeurtenis hubs gemaakt in zijn Service Bus-naamruimte. Is een tekenreeks met de volgende notatie: `{service bus resource ID}/authorizationrules/{key name}`. |
| locaties        | Ja      | Door komma's gescheiden lijst met regio's die u graag wilt verzamelen van gebeurtenissen in logboek voor faxactiviteit.              |
| retentionInDays  | Ja      | Aantal dagen voor welke gebeurtenissen moeten worden gehandhaafd, tussen 1 en 2147483647. De waarde nul worden de logboeken voor onbepaalde tijd opgeslagen (altijd).     |
| categorieën       | Nee       | Door komma's gescheiden lijst met de gebeurteniscategorieën die moeten worden verzameld. Mogelijke waarden zijn schrijven, verwijderen en actie.                                 |

#### <a name="remove-a-log-profile"></a>Een logboek profiel verwijderen
```
azure insights logprofile delete --name my_log_profile
```

## <a name="event-schema"></a>Schema van gebeurtenis
Elke gebeurtenis in het logboek voor faxactiviteit is een blob JSON vergelijkbaar met het volgende voorbeeld:

```
{
  "value": [ {
    "authorization": {
      "action": "microsoft.support/supporttickets/write",
      "role": "Subscription Admin",
      "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841"
    },
    "caller": "admin@contoso.com",
    "channels": "Operation",
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
    },
    "correlationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
    "description": "",
    "eventDataId": "44ade6b4-3813-45e6-ae27-7420a95fa2f8",
    "eventName": {
      "value": "EndRequest",
      "localizedValue": "End request"
    },
    "eventSource": {
      "value": "Microsoft.Resources",
      "localizedValue": "Microsoft Resources"
    },
    "httpRequest": {
      "clientRequestId": "27003b25-91d3-418f-8eb1-29e537dcb249",
      "clientIpAddress": "192.168.35.115",
      "method": "PUT"
    },
    "id": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841/events/44ade6b4-3813-45e6-ae27-7420a95fa2f8/ticks/635574752669792776",
    "level": "Informational",
    "resourceGroupName": "MSSupportGroup",
    "resourceProviderName": {
      "value": "microsoft.support",
      "localizedValue": "microsoft.support"
    },
    "resourceUri": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
    "operationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
    "operationName": {
      "value": "microsoft.support/supporttickets/write",
      "localizedValue": "microsoft.support/supporttickets/write"
    },
    "properties": {
      "statusCode": "Created"
    },
    "status": {
      "value": "Succeeded",
      "localizedValue": "Succeeded"
    },
    "subStatus": {
      "value": "Created",
      "localizedValue": "Created (HTTP Status Code: 201)"
    },
    "eventTimestamp": "2015-01-21T22:14:26.9792776Z",
    "submissionTimestamp": "2015-01-21T22:14:39.9936304Z",
    "subscriptionId": "s1"
  } ],
"nextLink": "https://management.azure.com/########-####-####-####-############$skiptoken=######"
}
```

| Elementnaam         | Beschrijving             |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| autorisatie        | BLOB van RBAC-eigenschappen van de gebeurtenis. Bevat meestal de eigenschappen 'actie', 'rol' en 'scope'.|
| beller               | E-mailadres van de gebruiker die de bewerking, UPN-claim of SPN claim op basis van beschikbaarheid is uitgevoerd.|
| kanalen             | Een van de volgende waarden: 'Admin', 'Bewerking'|
| correlationId        | Meestal een GUID in de indeling van de tekenreeks. Gebeurtenissen met een correlationId, behoren tot dezelfde actie uber.   |
| Beschrijving          | Statische tekstbeschrijving van een gebeurtenis.                              |
| eventDataId          | De unieke id van een gebeurtenis.    |
| Gebeurtenisbron          | De naam van de Azure service of infrastructuur die deze gebeurtenis heeft gegenereerd.    |
| httpRequest          | BLOB met een beschrijving van de HTTP-aanvraag. Meestal bevat het "clientRequestId", "clientIpAddress" en "methode" (HTTP-methode. For example, plaatsen).                            |
| niveau                | Niveau van de gebeurtenis. Een van de volgende waarden: 'Kritiek', 'Fout', 'Waarschuwing', 'Informatie' en 'Uitgebreid'  |
| resourceGroupName    | De naam van de bronnengroep voor de bron van het risico.               |
| resourceProviderName | Naam van de provider van de resource voor de bron van risico             |
| resourceUri          | Bron-id van de resource die risico.                               |
| Bewerkingsnummer          | Een GUID die wordt gedeeld door de gebeurtenissen die met één bewerking overeenkomen.         |
| operationName        | De naam van de bewerking.  |
| Eigenschappen           | Instellen van `<Key, Value>` paren (een woordenboek) met een beschrijving van de details van de gebeurtenis.                                |
| status               | Een tekenreeks met een beschrijving van de status van de bewerking. Sommige algemene waarden zijn: gestart, In uitvoering, geslaagd, mislukt, actief, opgelost.                                |
| subStatus            | Meestal de HTTP-statuscode van de corresponderende overige bellen, maar kan ook andere tekenreeksen met een beschrijving van een substatus, zoals deze gemeenschappelijke waarden bevatten: OK (HTTP-statuscode: 200), gemaakt (HTTP-statuscode: 201), geaccepteerde (HTTP-statuscode: 202), geen inhoud (HTTP-statuscode: 204), onjuiste aanvraag (HTTP-statuscode: 400), niet vinden (HTTP-statuscode: 404), Conflict (HTTP-statuscode : 409), interne serverfout (HTTP-statuscode: 500), Service niet beschikbaar is (HTTP-statuscode: 503), Gateway time-out (HTTP-statuscode: 504). |
| eventTimestamp       | Tijdstip waarop de gebeurtenis is gegenereerd door de verwerking van de aanvraag overeenkomstig de gebeurtenis Azure-service.     |
| submissionTimestamp  | Tijdstip waarop de gebeurtenis werd beschikbaar zijn voor query's.             |
| subscriptionId       | Azure abonnement-id.  |
| nextLink             | Voortzetting token voor het ophalen van de volgende set resultaten wanneer ze zijn opgedeeld in meerdere antwoorden. Meestal nodig wanneer er meer dan 200 records.     |

## <a name="next-steps"></a>Volgende stappen
- [Meer informatie over het logboek voor faxactiviteit (voorheen controlelogboeken)](../resource-group-audit.md)
- [Stream de Azure activiteitenlogboek op gebeurtenis Hubs](./monitoring-stream-activity-logs-event-hubs.md)
