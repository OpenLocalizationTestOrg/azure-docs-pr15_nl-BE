<properties
    pageTitle="Aan de slag met rollen, machtigingen en beveiliging met Azure Monitor | Microsoft Azure"
    description="Informatie over het gebruik van Azure Monitor ingebouwde rollen en machtigingen toegang te beperken tot het controleren van de resources."
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
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="get-started-with-roles-permissions-and-security-with-azure-monitor"></a>Aan de slag met rollen, machtigingen en beveiliging met Azure Monitor

Veel teams moeten strikt regulering van de toegang tot gegevens en instellingen te controleren. Als u teamleden die werken uitsluitend op controle (support engineers, technici devops) of als u een beheerde serviceprovider gebruikt, u kunt deze toegang verlenen tot gegevens alleen controle bij het beperken van hun vermogen om te maken, wijzigen of verwijderen van resources. In dit artikel ziet u hoe snel een ingebouwde controle RBAC-rol aan een gebruiker in Azure toepassen of uw eigen aangepaste rol voor een gebruiker die machtigingen voor beperkte controle moet bouwen. Beveiligingsoverwegingen voor uw Monitor Azure-gerelateerde bronnen en hoe u toegang tot de gegevens die ze bevatten alleen vervolgens besproken.

## <a name="built-in-monitoring-roles"></a>Ingebouwde functies voor controle

Azure Monitor ingebouwde functies zijn ontworpen om u te helpen beperken de toegang tot bronnen in een abonnement, terwijl u toch degenen die verantwoordelijk zijn voor de controle van de infrastructuur voor het verkrijgen en configureren van de gegevens die ze nodig hebben. Azure Monitor biedt twee rollen voor out-of-the-box: A Monitoring Reader en een medewerker Monitoring.

### <a name="monitoring-reader"></a>Controle van de lezer

Mensen die de rol van lezer Monitoring toegewezen kunt alle controlegegevens bekijken in een abonnement, maar kan wijzigen elke bron of alle instellingen met betrekking tot de bewaking van resources bewerken. Deze rol is geschikt voor gebruikers in een organisatie, zoals ondersteuning of bewerkingen technici die nodig kunnen hebben:

- Controle van dashboards in de portal weergeven en hun eigen persoonlijke monitoring dashboards maken.
- De query voor de metingen met de [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931930.aspx), [PowerShell-cmdlets](insights-powershell-samples.md)of [CLI cross-platform](insights-cli-samples.md).
- Het logboek voor faxactiviteit met behulp van de portal, Azure Monitor REST API, PowerShell-cmdlets of cross-platform CLI query.
- De [Diagnostische instellingen](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) voor een resource weergeven.
- Bekijk het [logboek profiel](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles) voor een abonnement.
- Instellingen voor beeld-automatisch schalen.
- Melding activiteit weergeven en instellingen.
- Toegang tot de gegevens van de toepassing inzichten en gegevens weergeven in een AI Analytics.
- Zoek gegevens in de werkruimte logboek Analytics (OMS) met inbegrip van gegevens over het gebruik van de werkruimte.
- Logboek Analytics Kantoorbeheersysteem beheer groepen weergeven.
- Ophalen van het zoekschema logboek Analytics (OMS).
- Lijst logboek Analytics Kantoorbeheersysteem intelligence packs.
- Ophalen en logboek Analytics (OMS) opgeslagen zoekopdrachten uit te voeren.
- De opslagconfiguratie logboek Analytics (OMS) ophalen.

> [AZURE.NOTE] Deze functie geeft geen leestoegang voor logboekgegevens die is gestreamd naar een gebeurtenis hub of opgeslagen in een opslagruimte-account. [Zie hieronder](#security-considerations-for-monitoring-data) voor informatie over het configureren van de toegang tot deze bronnen.

### <a name="monitoring-contributor"></a>Medewerker toezicht

Mensen die de rol van Inzender Monitoring toegewezen alle meetgegevens in een abonnement kunt weergeven en maken of wijzigen van de controle-instellingen, maar andere bronnen niet wijzigen. Deze rol is een superset van de rol van lezer voor bewaking en geschikt is voor leden van de team controleren of beheerde serviceproviders die naast de bevoegdheden die hierboven, moeten ook kunnen van een organisatie:

- Dashboards monitoring publiceren als een gedeelde dashboard.
- [Diagnostische instellingen](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) voor een resource.*
- Het [logboek profiel](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles) voor een subscription.* instellen
- Instellen waarschuwing activiteit en instellingen.
- Toepassing inzichten web tests en componenten maken.
- Logboek Analytics Kantoorbeheersysteem werkruimte gedeelde sleutels.
- In- of uitschakelen van logboek Analytics Kantoorbeheersysteem intelligence packs.
- Maken en verwijderen en logboek Analytics (OMS) opgeslagen zoekopdrachten uitvoeren.
- Maak en verwijder de opslagconfiguratie logboek Analytics (OMS).

* de gebruiker moet ook afzonderlijk machtiging ListKeys op de doelbron (opslag account of gebeurtenis hub-naamruimte) instellen voor een logboek profiel of diagnostische instelling.

> [AZURE.NOTE] Deze functie geeft geen leestoegang voor logboekgegevens die is gestreamd naar een gebeurtenis hub of opgeslagen in een opslagruimte-account. [Zie hieronder](#security-considerations-for-monitoring-data) voor informatie over het configureren van de toegang tot deze bronnen.

## <a name="monitoring-permissions-and-custom-rbac-roles"></a>Aangepaste RBAC-rollen en machtigingen controleren
Als de bovenstaande ingebouwde rollen niet voldoen aan de exacte behoeften van uw team, kunt u [een aangepaste RBAC-rol maken](../active-directory/role-based-access-control-custom-roles.md) met meer gedetailleerde machtigingen. Hieronder vindt u veelvoorkomende bewerkingen Azure Monitor RBAC met hun beschrijvingen.

| Bewerking                                                   | Beschrijving                                                                                                                                               |
|-------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Microsoft.Insights/AlertRules/[Read, schrijven, verwijderen]         | Lezen/schrijven/verwijderen waarschuwingsregels.                                                                                                                            |
| Microsoft.Insights/AlertRules/Incidents/Read                | Lijst van incidenten (geschiedenis van de waarschuwingsregel wordt geactiveerd) voor waarschuwingsregels. Dit is alleen van toepassing op de portal.                                              |
| Microsoft.Insights/AutoscaleSettings/[Read, schrijven, verwijderen]  | Instellingen voor lezen/schrijven/verwijderen automatisch schalen.                                                                                                                     |
| Microsoft.Insights/DiagnosticSettings/[Read, schrijven, verwijderen] | Diagnostische instellingen lezen/schrijven/verwijderen.                                                                                                                    |
| Microsoft.Insights/eventtypes/digestevents/Read             | Deze machtiging is vereist voor gebruikers die toegang tot de logboeken voor faxactiviteit via de portal hebben.                                                                   |
| Microsoft.Insights/eventtypes/values/Read                   | Logboek voor faxactiviteit gebeurtenissen (gebeurtenissen) in een abonnement aanbieden. Deze machtiging geldt voor programmatisch en portal-toegang tot het logboek voor faxactiviteit. |
| Microsoft.Insights/LogDefinitions/Read                      | Deze machtiging is vereist voor gebruikers die toegang tot de logboeken voor faxactiviteit via de portal hebben.                                                                   |
| Microsoft.Insights/MetricDefinitions/Read                   | Metrische definities (lijst met beschikbare metrische typen voor een resource) lezen.                                                                                  |
| Microsoft.Insights/Metrics/Read                             | Maatstaven voor een resource lezen.                                                                                                                              |

> [AZURE.NOTE] Toegang tot waarschuwingen, diagnostische instellingen en parameters voor een resource vereist is dat de gebruiker alleen toegang tot het brontype en de omvang van die bron heeft. ("Write") maken moet een diagnostische instelling of logboek profiel die naar een opslag- of streams op gebeurtenis hubs archieven de gebruiker ook gemachtigd zijn ListKeys op de doelbron.

Bijvoorbeeld met behulp van de bovenstaande tabel kunt u een aangepaste RBAC-rol voor een "activiteit logboekweergave' als volgt:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Activity Log Reader"
$role.Description = "Can view activity logs."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Insights/eventtypes/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription")
New-AzureRmRoleDefinition -Role $role 
```

## <a name="security-considerations-for-monitoring-data"></a>Beveiligingsoverwegingen voor controle van gegevens
Bewakingsgegevens, met name de logboekbestanden, kan gevoelige informatie bevatten, zoals IP-adressen of gebruikersnamen. Gegevens van Azure wordt geleverd in drie eenvoudige formulieren:
1. Het logboek voor faxactiviteit, waarin alle acties van het control-plane op uw abonnement Azure.
2. Diagnostische logboeken worden logboeken gegenereerd door een resource.
3. Statistieken worden uitgestoten door resources.

Alle drie van deze gegevenstypen kunnen worden opgeslagen in een opslagruimte-account of stroomsgewijs verzonden naar de gebeurtenis Hub, die beide algemene Azure bronnen zijn. Omdat deze algemene resources, is maken, verwijderen en toegang tot een bevoegde bewerking doorgaans gereserveerd voor een beheerder. Het is raadzaam dat u de volgende procedures voor controle-gerelateerde bronnen gebruiken om misbruik te voorkomen:

- Een enkel toegewezen opslag gebruiken voor het controleren van gegevens. Als u meetgegevens opdelen in meerdere accounts voor opslag, nooit gebruik van een account opslag tussen controle en delen gegevens niet controleren als dit kan per ongeluk geven die alleen toegang tot de controle van gegevens (bv.) een derde SIEM) toegang tot niet-controle van gegevens.
- Een enkele, speciale Service Bus of gebeurtenis Hub-naamruimte via alle diagnostische instellingen gebruiken om dezelfde reden als hierboven.
- Toegang beperken tot controle-gerelateerde opslag rekeningen of gebeurtenis hubs doordat ze op een afzonderlijke resourcegroep, en [scope gebruiken](../active-directory/role-based-access-control-what-is.md#basics-of-access-management-in-azure) op uw controle rollen te beperken tot alleen die bronnengroep.
- Nooit de machtiging ListKeys voor opslag rekeningen of gebeurtenis hubs in het abonnement bereik wanneer een gebruiker alleen toegang moet tot gegevens te controleren. In plaats daarvan geven deze machtigingen aan de gebruiker op een resource of resourcegroep (als u een specifieke groep controle) bereik.

### <a name="limiting-access-to-monitoring-related-storage-accounts"></a>Beperken van toegang tot opslag controleren gerelateerde accounts
Wanneer een gebruiker of toepassing toegang moet tot gegevens in een opslag-account controleren, moet u [een Account SAS genereren](https://msdn.microsoft.com/library/azure/mt584140.aspx) op de rekening voor opslag met controlegegevens serviceniveau alleen-lezen toegang tot de blob-opslag. In PowerShell, dit als volgt uitzien:

```powershell
$context = New-AzureStorageContext -ConnectionString "[connection string for your monitoring Storage Account]"
$token = New-AzureStorageAccountSASToken -ResourceType Service -Service Blob -Permission "rl" -Context $context
```

U kunt vervolgens het token geven de entiteit dat moet lezen van die opslag, en deze kunnen weergeven en van alle BLOB's in die account opslag lezen.

Ook als u deze machtiging met RBAC bepalen, kan u machtigen die entiteit Microsoft.Storage/storageAccounts/listkeys/action op dat bepaalde opslag-account. Dit is nodig voor gebruikers die toegang kunnen stellen van een diagnose of profiel moeten te archiveren naar een opslag registreren. U kan bijvoorbeeld de volgende aangepaste RBAC-rol voor een gebruiker of een toepassing die alleen moet lezen van de ene opslag account maken:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Monitoring Storage Account Reader"
$role.Description = "Can get the storage account keys for a monitoring storage account."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/storageAccounts/listkeys/action")
$role.Actions.Add("Microsoft.Storage/storageAccounts/Read")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/myMonitoringStorageAccount")
New-AzureRmRoleDefinition -Role $role 
```

> [AZURE.WARNING] De ListKeys machtiging kan gebruikers kunnen met de primaire en secundaire opslag account toetsen. Deze sleutels geven de gebruiker alle ondertekende machtigingen (lezen, schrijven, BLOB's maken, verwijderen van BLOB's, enz.) ondertekend voor alle services (blob, wachtrij, tabel, bestand) in die account opslag. Wij raden u aan met behulp van een Account SAS hierboven indien mogelijk.

### <a name="limiting-access-to-monitoring-related-event-hubs"></a>Beperken van toegang tot de bewaking betrekking hubs
Een soortgelijk patroon kan worden gevolgd met de gebeurtenis hubs, maar u moet eerst een speciale Listen verificatieregel maken. Als u toegang verlenen aan een toepassing die alleen moet luisteren naar de gebeurtenis controle betrekking hubs wilt, het volgende doen:

1. Maak een gedeelde-beleid op de hub van de gebeurtenis die voor het streamen van meetgegevens met alleen luisteren vorderingen zijn gemaakt. Dit kan worden gedaan in de portal. Bijvoorbeeld, noemen u het "monitoringReadOnly". Indien mogelijk, zult u die sleutel geven rechtstreeks aan de consument en de volgende stap overslaan.
2. Als de consument kunnen moet ophalen van de sleutel ad hoc, verlenen de gebruiker de actie ListKeys voor die gebeurtenis hub. Dit is ook nodig voor gebruikers die toegang moeten kunnen stellen van een diagnose of profiel melden aan stroom op gebeurtenis hubs. U kunt bijvoorbeeld een regel RBAC maken:

   ```powershell
   $role = Get-AzureRmRoleDefinition "Reader"
   $role.Id = $null
   $role.Name = "Monitoring Event Hub Listener"
   $role.Description = "Can get the key to listen to an event hub streaming monitoring data."
   $role.Actions.Clear()
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/authorizationrules/listkeys/action")
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/Read")
   $role.AssignableScopes.Clear()
   $role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.ServiceBus/namespaces/mySBNameSpace")
   New-AzureRmRoleDefinition -Role $role 
   ```


## <a name="next-steps"></a>Volgende stappen
- [Meer informatie over RBAC en machtigingen in Resource Manager](../active-directory/role-based-access-control-what-is.md)
- [Lees het overzicht van de controle in Azure](monitoring-overview.md)
