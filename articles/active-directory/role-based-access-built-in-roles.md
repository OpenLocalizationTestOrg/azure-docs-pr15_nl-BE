<properties
    pageTitle="RBAC: Ingebouwde functies | Microsoft Azure"
    description="Dit onderwerp beschrijft de ingebouwde in rollen voor op rollen gebaseerde toegangscontrole (RBAC)."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/25/2016"
    ms.author="kgremban"/>

#<a name="rbac-built-in-roles"></a>RBAC: Ingebouwde-rollen

Azure Role-Based Access Control RBAC () wordt geleverd met de volgende ingebouwde functies die kunnen worden toegewezen aan gebruikers, groepen en services. De definities van ingebouwde functies kunt u niet wijzigen. Echter, kunt u [aangepaste rollen in Azure RBAC](role-based-access-control-custom-roles.md) aanpassen aan de specifieke behoeften van uw organisatie.

## <a name="roles-in-azure"></a>Rollen in Azure

De volgende tabel bevat een korte beschrijving van de ingebouwde functies. Klik op de naam van de functie voor een gedetailleerd overzicht van de **Acties** en **notactions** voor de rol. De eigenschap **actions** bevat de toegestane acties op Azure bronnen. Actie tekenreeksen kunnen u jokertekens gebruiken. De eigenschap **notactions** geeft de acties die worden uitgesloten van de toegestane acties.

>[AZURE.NOTE] De Azure roldefinities zijn voortdurend in ontwikkeling. In dit artikel wordt zo actueel mogelijk gehouden, maar u kunt altijd de nieuwste definities van rollen in Azure PowerShell vinden. Gebruik de cmdlets `(get-azurermroledefinition "<role name>").actions` of `(get-azurermroledefinition "<role name>").notactions` indien van toepassing.

| Rolnaam | Beschrijving |
| --------- | ----------- |
| [API Management Service medewerker](#api-management-service-contributor) | API Management services kunt beheren |
| [Toepassing inzichten onderdeel Inzender](#application-insights-component-contributor) | Onderdelen van Application inzichten kunt beheren |
| [Automatisering-Operator](#automation-operator) | Kunnen starten, stoppen, onderbreken en hervatten van taken |
| [BizTalk-medewerker](#biztalk-contributor) | BizTalk-services kunt beheren |
| [ClearDB MySQL DB Inzender](#cleardb-mysql-db-contributor) | ClearDB MySQL-databases kunt beheren |
| [Inzender](#contributor) | Alles behalve de toegang kunt beheren. |
| [Inzender fabriek](#data-factory-contributor) | Kunt maken en beheren van gegevens fabrieken en onderliggende bronnen binnen deze. |
| [DevTest Labs gebruiker](#devtest-labs-user) | Alles bekijken en verbinding maken, starten, opnieuw opstarten en afsluiten van de virtuele machines |
| [DNS-Zone Inzender](#dns-zone-contributor) | DNS-zones en records kunt beheren |
| [Account DocumentDB medewerker](#documentdb-account-contributor) | DocumentDB-accounts kunt beheren |
| [Intelligente systemen Account Inzender](#intelligent-systems-account-contributor) | Intelligente systemen accounts kunt beheren |
| [Medewerker netwerk](#network-contributor) | Alle netwerkbronnen kunt beheren |
| [Nieuwe Relic APM Account Inzender](#new-relic-apm-account-contributor) | Nieuwe Relic Application Performance Management-accounts en -toepassingen kunt beheren |
| [Eigenaar](#owner) | Alles, met inbegrip van de toegang kunt beheren |
| [Reader](#reader) | Alles kunt bekijken maar geen wijzigingen aanbrengen |
| [De inzender Cache bestand Vgx.](#redis-cache-contributor) | Bestand Vgx. caches kunt beheren |
| [Scheduler taak collecties Inzender](#scheduler-job-collections-contributor) | Taak collecties van Taakplanner kunt beheren |
| [Search Service medewerker](#search-service-contributor) | Search services kunt beheren |
| [Security Manager](#security-manager) | Beveiligingsonderdelen, beveiligingsbeleid en virtuele machines kunt beheren |
| [SQL DB Inzender](#sql-db-contributor) | SQL-databases, maar niet hun beleid-beveiliging kunt beheren |
| [SQL Security Manager](#sql-security-manager) | Het beleid met betrekking tot beveiliging van SQL servers en databases kunt beheren |
| [SQL Server Inzender](#sql-server-contributor) | Kan SQL-servers en databases, maar niet hun beleid-beveiliging beheren |
| [Klassieke opslag Account Inzender](#classic-storage-account-contributor) | Klassieke opslag rekeningen kunt beheren |
| [Opslag Account Inzender](#storage-account-contributor) | Opslag rekeningen kunt beheren |
| [Beheerder toegang](#user-access-administrator) | Gebruikerstoegang tot Azure bronnen kunt beheren |
| [Klassieke virtuele Machine Inzender](#classic-virtual-machine-contributor) | Klassieke virtuele machines, maar niet het virtuele netwerk- of account waaraan ze zijn verbonden kunt beheren |
| [Virtuele Machine Inzender](#virtual-machine-contributor) | Beheren van virtuele machines, maar niet het virtuele netwerk- of account waaraan ze zijn verbonden |
| [Klassieke netwerk Inzender](#classic-network-contributor) | Klassieke virtuele netwerken en gereserveerde IP-adressen kan beheren |
| [Web Plan Inzender](#web-plan-contributor) | Plannen kunt beheren |
| [Website-medewerker](#website-contributor) | Beheren van websites, maar niet de plannen waarmee ze zijn verbonden |

## <a name="role-permissions"></a>Machtigingen
De volgende tabellen beschrijven de specifieke machtigingen die zijn toegekend aan elke rol. Dit zijn **Acties**die machtigingen geven, en **NotActions**, die ze beperken.

### <a name="api-management-service-contributor"></a>API Management Service medewerker
API Management services kunt beheren

| **Acties** | |
| ------- | ------ |
| Microsoft.ApiManagement/Service/* | Maken en beheren van API |
| Microsoft.Authorization/*/read | Lezen, machtiging |
| Microsoft.Insights/alertRules/* | Maken en waarschuwingsregels beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lees de gezondheid van de bronnen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Meer functies en roltoewijzingen |
| Microsoft.Support/* | Maken en support tickets beheren |

### <a name="application-insights-component-contributor"></a>Toepassing inzichten onderdeel Inzender
Onderdelen van Application inzichten kunt beheren

| **Acties** | |
| ------- | ------ |
| Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
| Microsoft.Insights/alertRules/* | Maken en waarschuwingsregels beheren |
| Microsoft.Insights/components/* | Maken en beheren van inzichten onderdelen |
| Microsoft.Insights/webtests/* | Maken en beheren van web-tests |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lees de gezondheid van de bronnen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Support/* | Maken en support tickets beheren |

### <a name="automation-operator"></a>Automatisering-Operator
Kunnen starten, stoppen, onderbreken en hervatten van taken

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
| Microsoft.Automation/automationAccounts/jobs/read | Automatisering taken account lezen |
| Microsoft.Automation/automationAccounts/jobs/resume/action | Een account automatisering taak hervatten |
| Microsoft.Automation/automationAccounts/jobs/stop/action | Een account automatisering taak stoppen |
| Microsoft.Automation/automationAccounts/jobs/streams/read | Automatisering account taak streams lezen |
| Microsoft.Automation/automationAccounts/jobs/suspend/action | Een account automatisering taak onderbreken |
| Microsoft.Automation/automationAccounts/jobs/write | Automatisering taken rekening schrijven |
| Microsoft.Automation/automationAccounts/jobSchedules/read | Een taak van automatisering rapportageschema lezen |
| Microsoft.Automation/automationAccounts/jobSchedules/write | Een taak van automatisering rapportageschema lezen |
| Microsoft.Automation/automationAccounts/read | Automatisering accounts lezen |
| Microsoft.Automation/automationAccounts/runbooks/read | Automatisering runbooks lezen |
| Microsoft.Automation/automationAccounts/schedules/read | Rapportageschema's lezen automatisering |
| Microsoft.Automation/automationAccounts/schedules/write | Automatisering rapportageschema's schrijven |
| Microsoft.Insights/components/* | Maken en beheren van inzichten onderdelen |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lees de gezondheid van de bronnen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Support/* | Maken en support tickets beheren |

### <a name="biztalk-contributor"></a>BizTalk-medewerker
BizTalk-services kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
| Microsoft.BizTalkServices/BizTalk/* | Maken en beheren van BizTalk-services |
| Microsoft.Insights/alertRules/* | Maken en waarschuwingsregels beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lees de gezondheid van de bronnen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Support/* | Maken en support tickets beheren |

### <a name="cleardb-mysql-db-contributor"></a>ClearDB MySQL DB Inzender
ClearDB MySQL-databases kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
| Microsoft.Insights/alertRules/* | Maken en waarschuwingsregels beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lees de gezondheid van de bronnen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Support/* | Maken en support tickets beheren |
| successbricks.cleardb/databases/* | Maken en ClearDB MySQL databases beheren |

### <a name="contributor"></a>Inzender
Alles behalve de toegang kunt beheren

| **Acties** ||
| ------- | ------ |
| * | Maken en beheren van bronnen van alle typen |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Authorization/*/Delete | Rollen en roltoewijzingen verwijderen niet |  
| Microsoft.Authorization/*/Write | Kan geen rollen en roltoewijzingen maken |

### <a name="data-factory-contributor"></a>Inzender fabriek
Maken en beheren van gegevens fabrieken en de onderliggende bronnen binnen deze.

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Meer functies en de rol van toewijzingen |
| Microsoft.DataFactory/dataFactories/* | Maken en beheren van gegevens fabrieken en de onderliggende bronnen binnen deze. |
| Microsoft.Insights/alertRules/* | Maken en waarschuwingsregels beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lees de gezondheid van de bronnen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Support/* | Maken en support tickets beheren |

### <a name="devtest-labs-user"></a>DevTest Labs gebruiker
Alles bekijken en verbinding maken, starten, opnieuw opstarten en afsluiten van de virtuele machines

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Meer functies en de rol van toewijzingen |
| Microsoft.Compute/availabilitySets/read | De eigenschappen van de beschikbaarheid wordt gelezen |
| Microsoft.Compute/virtualMachines/*/read | Lezen van de eigenschappen van een virtuele machine (VM formaten, runtime-status, VM-extensies, enz.) |
| Microsoft.Compute/virtualMachines/deallocate/action | Toewijzing van virtuele machines |
| Microsoft.Compute/virtualMachines/read | Lezen van de eigenschappen van een virtuele machine |
| Microsoft.Compute/virtualMachines/restart/action | Start de virtuele machines |
| Microsoft.Compute/virtualMachines/start/action | Start de virtuele machines |
| Microsoft.DevTestLab/*/read | De eigenschappen van een lab lezen |
| Microsoft.DevTestLab/labs/createEnvironment/action | Een testomgeving maken |
| Microsoft.DevTestLab/labs/formulas/delete | Formules verwijderen |
| Microsoft.DevTestLab/labs/formulas/read | Lezen van formules |
| Microsoft.DevTestLab/labs/formulas/write | Toevoegen of wijzigen, formules |
| Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Lab beleid evalueren |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Deelnemen aan een load balancer backend-adresgroep |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Deelnemen aan een load balancer binnenkomende regel voor NAT |
| Microsoft.Network/networkInterfaces/*/read | Lezen van de eigenschappen van een netwerkinterface (bijvoorbeeld alle de netwerktaakverdeling die de netwerkinterface een deel van is) |
| Microsoft.Network/networkInterfaces/join/action | Lid worden van een virtuele Machine op een netwerkinterface |
| Microsoft.Network/networkInterfaces/read | Lezen van netwerkinterfaces |
| Microsoft.Network/networkInterfaces/write | Schrijven van netwerkinterfaces |
| Microsoft.Network/publicIPAddresses/*/read | Lezen van de eigenschappen van een openbaar IP-adres |
| Microsoft.Network/publicIPAddresses/join/action | Lid worden van een openbaar IP-adres |
| Microsoft.Network/publicIPAddresses/read | Netwerk openbare IP-adressen te lezen |
| Microsoft.Network/virtualNetworks/subnets/join/action | Deelnemen aan een virtueel netwerk |
| Microsoft.Resources/deployments/operations/read | Implementatie leesbewerkingen |
| Microsoft.Resources/deployments/read | Lezen-implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Storage/storageAccounts/listKeys/action | Lijst opslag account sleutels |

### <a name="dns-zone-contributor"></a>DNS-Zone Inzender
Kunnen DNS-zones en records beheren.

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/\*/lezen | Meer functies en roltoewijzingen |
| Microsoft.Insights/alertRules/\* | Maken en waarschuwingsregels beheren |
| Microsoft.Network/dnsZones/\* | Maken en beheren van DNS-zones en records |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lees de gezondheid van de bronnen |
| Microsoft.Resources/deployments/\* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Support/\* | Maken en Support tickets beheren |


### <a name="documentdb-account-contributor"></a>Account DocumentDB medewerker
DocumentDB-accounts kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Meer functies en de rol van toewijzingen |
| Microsoft.DocumentDb/databaseAccounts/* | DocumentDB rekeningen maken en beheren |
| Microsoft.Insights/alertRules/* | Maken en waarschuwingsregels beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lees de gezondheid van de bronnen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Support/* | Maken en support tickets beheren |

### <a name="intelligent-systems-account-contributor"></a>Intelligente systemen Account Inzender
Intelligente systemen accounts kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Meer functies en de rol van toewijzingen |
| Microsoft.Insights/alertRules/* | Maken en waarschuwingsregels beheren |
| Microsoft.IntelligentSystems/accounts/* | Intelligente systemen rekeningen maken en beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lees de gezondheid van de bronnen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Support/* | Maken en support tickets beheren |

### <a name="network-contributor"></a>Medewerker netwerk
Alle netwerkbronnen kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Meer functies en de rol van toewijzingen |
| Microsoft.Insights/alertRules/* | Maken en waarschuwingsregels beheren |
| Microsoft.Network/* | Maken en beheren van netwerken |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lees de gezondheid van de bronnen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Support/* | Maken en support tickets beheren |

### <a name="new-relic-apm-account-contributor"></a>Nieuwe Relic APM Account Inzender
Nieuwe Relic Application Performance Management-accounts en -toepassingen kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Meer functies en de rol van toewijzingen |
| Microsoft.Insights/alertRules/* | Maken en waarschuwingsregels beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lees de gezondheid van de bronnen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Support/* | Maken en support tickets beheren |
| NewRelic.APM/accounts/* | Nieuwe Relic application performance management rekeningen maken en beheren |

### <a name="owner"></a>Eigenaar
Alles, met inbegrip van de toegang kunt beheren

| **Acties** ||
| ------- | ------ |
| * | Maken en beheren van bronnen van alle typen |

### <a name="reader"></a>Reader
Alles kunt bekijken maar geen wijzigingen aanbrengen

| **Acties** ||
| ------- | ------ |
| * / lezen | Bronnen van alle soorten, met uitzondering van geheimen lezen. |

### <a name="redis-cache-contributor"></a>De inzender Cache bestand Vgx.
Bestand Vgx. caches kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Meer functies en de rol van toewijzingen |
| Microsoft.Cache/redis/* | Maken en beheren van caches bestand Vgx. |
| Microsoft.Insights/alertRules/* | Maken en waarschuwingsregels beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lees de gezondheid van de bronnen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Support/* | Maken en support tickets beheren |

### <a name="scheduler-job-collections-contributor"></a>Scheduler taak collecties Inzender
Taak collecties van Taakplanner kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Meer functies en de rol van toewijzingen |
| Microsoft.Insights/alertRules/* | Maken en waarschuwingsregels beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lees de gezondheid van de bronnen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |  
| Microsoft.Scheduler/jobcollections/* | Maken en beheren van project collecties |
| Microsoft.Support/* | Maken en support tickets beheren  |

### <a name="search-service-contributor"></a>Search Service medewerker
Search services kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Meer functies en de rol van toewijzingen |
| Microsoft.Insights/alertRules/* | Maken en waarschuwingsregels beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lees de gezondheid van de bronnen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |  
| Microsoft.Search/searchServices/* | Maken en beheren van zoekservices |
| Microsoft.Support/* | Maken en support tickets beheren  |

### <a name="security-manager"></a>Security Manager
Beveiligingsonderdelen, beveiligingsbeleid en virtuele machines kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Meer functies en de rol van toewijzingen |
| Microsoft.ClassicCompute/*/read | Klassieke virtuele machines berekenen-configuratiegegevens lezen |
| Microsoft.ClassicCompute/virtualMachines/*/write | Configuratie voor virtuele machines te schrijven |
| Microsoft.ClassicNetwork/*/read | Lees informatie over klassieke netwerk configuratie  |
| Microsoft.Insights/alertRules/* | Maken en waarschuwingsregels beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lees de gezondheid van de bronnen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |  
| Microsoft.Security/* | Componenten en beleid maken en beheren |
| Microsoft.Support/* | Maken en support tickets beheren  |

### <a name="sql-db-contributor"></a>SQL DB Inzender
SQL-databases, maar niet hun beleid-beveiliging kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Meer functies en de rol van toewijzingen |
| Microsoft.Insights/alertRules/* | Maken en waarschuwingsregels beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lees de gezondheid van de bronnen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Sql/servers/databases/* | Maken en beheren van SQL-databases |
| Microsoft.Sql/servers/read | SQL-Servers te lezen |
| Microsoft.Support/* | Maken en support tickets beheren |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Controlebeleid bewerken niet |
| Microsoft.Sql/servers/databases/auditingSettings/* | Controle-instellingen kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/auditRecords/read  | Audit records kan niet worden gelezen. |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Beleidsregels bewerken niet |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Beleid maskeren gegevens bewerken niet |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Waarschuwing beveiligingsbeleid kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/securityMetrics/* | Parameters voor beveiliging kan niet worden bewerkt. |

### <a name="sql-security-manager"></a>SQL Security Manager
Het beleid met betrekking tot beveiliging van SQL servers en databases kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lees Microsoft-verificatie |
| Microsoft.Insights/alertRules/* | Maken en inzichten Waarschuwingsregels beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lees de gezondheid van de bronnen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Sql/servers/auditingPolicies/* | Maken en beheren, controlebeleid voor SQL server |
| Microsoft.Sql/servers/auditingSettings/* | Maken en beheren van de instelling van SQL server controleren |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Maken en beheren, controlebeleid van de SQL server-database |
| Microsoft.Sql/servers/databases/auditingSettings/* | Maken en beheren van SQL server-database controle-instellingen |
| Microsoft.Sql/servers/databases/auditRecords/read | Audit-records lezen |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Maken en beheren van beleidsregels voor SQL server-database |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Maken en beheren van SQL server-databasegegevens beleid maskeren |
| Microsoft.Sql/servers/databases/read | Alleen SQL-databases |
| Microsoft.Sql/servers/databases/schemas/read | Schema's alleen SQL server-database |
| Microsoft.Sql/servers/databases/schemas/tables/columns/read | Alleen SQL server-database tabelkolommen |
| Microsoft.Sql/servers/databases/schemas/tables/read | Alleen SQL server-database-tabellen |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Maken en beheren van waarschuwingen beveiligingsbeleid van SQL server-database |
| Microsoft.Sql/servers/databases/securityMetrics/* | Maken en beheren van SQL server-database beveiliging metrics |
| Microsoft.Sql/servers/read | SQL-Servers te lezen |
| Microsoft.Sql/servers/securityAlertPolicies/* | Maken en SQL server alert-beveiligingsbeleid beheren |
| Microsoft.Support/* | Maken en support tickets beheren |

### <a name="sql-server-contributor"></a>SQL Server Inzender
Kan SQL-servers en databases, maar niet hun beleid-beveiliging beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lezen, machtiging|
| Microsoft.Insights/alertRules/* | Maken en inzichten Waarschuwingsregels beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lees de gezondheid van de bronnen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Sql/servers/* | Maken en beheren van SQL-servers |
| Microsoft.Support/* | Maken en support tickets beheren |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/auditingPolicies/* | Controlebeleid voor SQL server kan niet worden bewerkt. |
| Microsoft.Sql/servers/auditingSettings/* | Instellingen voor beveiligingscontrole SQL server kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Controlebeleid voor SQL-database kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/auditingSettings/* | SQL server-database controle-instellingen kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/auditRecords/read  | Audit records kan niet worden gelezen. |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Beleidsregels voor SQL server-database kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | SQL server-databasegegevens maskeren beleid kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Waarschuwing beveiligingsbeleid van SQL server-database kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/securityMetrics/* | SQL server-database beveiliging metrics kan niet worden bewerkt. |
| Microsoft.Sql/servers/securityAlertPolicies/* | SQL server security alert beleid kan niet worden bewerkt. |

### <a name="classic-storage-account-contributor"></a>Klassieke opslag Account Inzender
Klassieke opslag rekeningen kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lezen, machtiging |
| Microsoft.ClassicStorage/storageAccounts/* | Opslag rekeningen maken en beheren |
| Microsoft.Insights/alertRules/* | Maken en inzichten Waarschuwingsregels beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lees de gezondheid van de bronnen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |  
| Microsoft.Support/* | Maken en support tickets beheren |

### <a name="storage-account-contributor"></a>Opslag Account Inzender
Kan opslag accounts beheren, maar geen toegang tot deze.

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lees alle vergunning |
| Microsoft.Insights/alertRules/* | Maken en inzichten Waarschuwingsregels beheren |
| Microsoft.Insights/diagnosticSettings/* | Diagnostische instellingen beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lees de gezondheid van de bronnen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |  
| Microsoft.Storage/storageAccounts/* | Opslag rekeningen maken en beheren |
| Microsoft.Support/* | Maken en support tickets beheren |

### <a name="user-access-administrator"></a>Beheerder toegang
Gebruikerstoegang tot Azure bronnen kunt beheren

| **Acties** ||
| ------- | ------ |
| * / lezen | Bronnen van alle soorten, met uitzondering van geheimen lezen. |
| Microsoft.Authorization/* | Machtigingen beheren |
| Microsoft.Support/* | Maken en support tickets beheren |

### <a name="classic-virtual-machine-contributor"></a>Klassieke virtuele Machine Inzender
Klassieke virtuele machines, maar niet het virtuele netwerk- of account waaraan ze zijn verbonden kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lezen, machtiging |
| Microsoft.ClassicCompute/domainNames/* | Maken en beheren van domeinnamen klassieke berekenen |
| Microsoft.ClassicCompute/virtualMachines/* | Maken en beheren van virtuele machines |
| Microsoft.ClassicNetwork/networkSecurityGroups/join/action | Deelnemen aan netwerk-beveiligingsgroepen |
| Microsoft.ClassicNetwork/reservedIps/link/action | Gereserveerde IP-adressen koppelen |
| Microsoft.ClassicNetwork/reservedIps/read | Alleen de IP-adressen gereserveerd |
| Microsoft.ClassicNetwork/virtualNetworks/join/action | Virtuele netwerken |
| Microsoft.ClassicNetwork/virtualNetworks/read | Virtuele netwerken lezen |
| Microsoft.ClassicStorage/storageAccounts/disks/read | Gelezen schijven voor account |
| Microsoft.ClassicStorage/storageAccounts/images/read | Opslag account afbeeldingen lezen |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Lijst opslag account sleutels |
| Microsoft.ClassicStorage/storageAccounts/read | Klassieke opslag accounts lezen |
| Microsoft.Insights/alertRules/* | Maken en inzichten Waarschuwingsregels beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lees de gezondheid van de bronnen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Support/* | Maken en support tickets beheren |

### <a name="virtual-machine-contributor"></a>Virtuele Machine Inzender
Beheren van virtuele machines, maar niet het virtuele netwerk- of account waaraan ze zijn verbonden

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lezen, machtiging |
| Microsoft.Compute/availabilitySets/* | Maken en beheren van sets van compute beschikbaarheid |
| Microsoft.Compute/locations/* | Maken en beheren van locaties berekenen |
| Microsoft.Compute/virtualMachines/* | Maken en beheren van virtuele machines |
| Microsoft.Compute/virtualMachineScaleSets/* | Maken en beheren van virtuele machine schaal sets |
| Microsoft.Insights/alertRules/* | Maken en inzichten Waarschuwingsregels beheren |
| Microsoft.Network/applicationGateways/backendAddressPools/join/action | Deelnemen aan netwerk gateway backend adres groepen van toepassingen |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Deelnemen aan de load balancer backend-adresgroepen |
| Microsoft.Network/loadBalancers/inboundNatPools/join/action | Deelnemen aan taakverdeling inkomende NAT pools |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Deelnemen aan taakverdeling binnenkomende NAT regels |
| Microsoft.Network/loadBalancers/read | Netwerktaakverdeling lezen |
| Microsoft.Network/locations/* | Maken en beheren van netwerklocaties |
| Microsoft.Network/networkInterfaces/* | Maken en beheren van netwerkinterfaces |
| Microsoft.Network/networkSecurityGroups/join/action | Deelnemen aan netwerk-beveiligingsgroepen |
| Microsoft.Network/networkSecurityGroups/read | Netwerk-beveiligingsgroepen te lezen |
| Microsoft.Network/publicIPAddresses/join/action | Deelnemen aan netwerk openbare IP-adressen |
| Microsoft.Network/publicIPAddresses/read | Netwerk openbare IP-adressen te lezen |
| Microsoft.Network/virtualNetworks/read | Virtuele netwerken lezen |
| Microsoft.Network/virtualNetworks/subnets/join/action | Virtueel netwerksubnetten koppelen |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lees de gezondheid van de bronnen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |  
| Microsoft.Storage/storageAccounts/listKeys/action | Lijst opslag account sleutels |
| Microsoft.Storage/storageAccounts/read | Opslag accounts lezen |
| Microsoft.Support/* | Maken en support tickets beheren |

### <a name="classic-network-contributor"></a>Klassieke netwerk Inzender
Klassieke virtuele netwerken en gereserveerde IP-adressen kan beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lezen, machtiging |
| Microsoft.ClassicNetwork/* | Maken en klassieke netwerken beheren |
| Microsoft.Insights/alertRules/* | Maken en inzichten Waarschuwingsregels beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lees de gezondheid van de bronnen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |  
| Microsoft.Support/* | Maken en support tickets beheren |

### <a name="web-plan-contributor"></a>Web Plan Inzender
Plannen kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lezen, machtiging |
| Microsoft.Insights/alertRules/* | Maken en inzichten Waarschuwingsregels beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lees de gezondheid van de bronnen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |  
| Microsoft.Support/* | Maken en support tickets beheren |
| Microsoft.Web/serverFarms/* | Maken en beheren van server-farms |

### <a name="website-contributor"></a>Website-medewerker
Beheren van websites, maar niet de plannen waarmee ze zijn verbonden

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lezen, machtiging |
| Microsoft.Insights/alertRules/* | Maken en inzichten Waarschuwingsregels beheren |
| Microsoft.Insights/components/* | Maken en beheren van inzichten onderdelen |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lees de gezondheid van de bronnen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |  
| Microsoft.Support/* | Maken en support tickets beheren |
| Microsoft.Web/certificates/* | Maken en beheren van Websitecertificaten |
| Microsoft.Web/listSitesAssignedToHostName/read | Sites die zijn toegewezen aan een host-naam lezen |
| Microsoft.Web/serverFarms/join/action | Lid worden van server-farms |
| Microsoft.Web/serverFarms/read | Server-farms lezen |
| Microsoft.Web/sites/* | Maken en beheren van websites |

## <a name="see-also"></a>Zie ook
- [Role-Based Access Control](role-based-access-control-configure.md): aan de slag met RBAC in Azure portal.
- [Aangepaste rollen in Azure RBAC](role-based-access-control-custom-roles.md): informatie over het maken van aangepaste rollen aanpassen aan uw behoeften toegang.
- [Maken van een rapport wijzigen](role-based-access-control-access-change-history-report.md): bijhouden van veranderende roltoewijzingen in RBAC.
- [Het oplossen van op rollen gebaseerde toegangscontrole](role-based-access-control-troubleshooting.md): suggesties voor het oplossen van veelvoorkomende problemen krijgen.
