<properties
    pageTitle="Aangepaste rollen in Azure RBAC | Microsoft Azure"
    description="Informatie over het definiëren van aangepaste rollen met toegangsbeheer op Azure Role-Based voor meer precieze identity management van uw abonnement op Azure."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="kgremban"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="07/25/2016"
    ms.author="kgremban"/>


# <a name="custom-roles-in-azure-rbac"></a>Aangepaste rollen in Azure RBAC


Maak een aangepaste rol in Azure Role-Based Access Control RBAC () als geen van de ingebouwde functies aan uw behoeften specifiek toegang. Aangepaste rollen kunnen worden gemaakt met [Azure PowerShell](role-based-access-control-manage-access-powershell.md) [Azure opdrachtregelinterface](role-based-access-control-manage-access-azure-cli.md) (CLI) en de [REST API](role-based-access-control-manage-access-rest.md). Net als de ingebouwde functies, kunnen aangepaste rollen worden toegewezen aan gebruikers, groepen en toepassingen op een abonnement, resourcegroep en resource bereiken. Aangepaste rollen in een huurder Azure AD worden opgeslagen en kunnen worden gedeeld met alle abonnementen die dat huurder als de map Azure AD voor de subsciption gebruiken.

Hier volgt een voorbeeld van een aangepaste rol voor het controleren en opnieuw starten van virtuele machines:

```
{
  "Name": "Virtual Machine Operator",
  "Id": "cadb4a5a-4e7a-47be-84db-05cad13b6769",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624",
    "/subscriptions/34370e90-ac4a-4bf9-821f-85eeedeae1a2"
  ]
}
```
## <a name="actions"></a>Acties
De eigenschap **Acties** van een aangepaste rol geeft de Azure activiteiten waaraan de rol toegang verleent. Het is een collectie tekenreeksen bewerking die beveiligbare bewerkingen van providers Azure bron te identificeren. Bewerking strings met jokertekens (\*) toegang verlenen tot alle bewerkingen die overeenkomen met de tekenreeks van de bewerking. Bijvoorbeeld:

-   `*/read`verleent toegang tot leesbewerkingen voor alle typen van alle leveranciers van Azure resource.
-   `Microsoft.Network/*/read`verleent toegang tot leesbewerkingen voor alle typen in de resource Microsoft.Network provider van Azure.
-   `Microsoft.Compute/virtualMachines/*`verleent toegang tot alle bewerkingen van virtuele machines en de onderliggende typen.
-   `Microsoft.Web/sites/restart/Action`verleent toegang tot websites starten.

Gebruik `Get-AzureRmProviderOperation` (in PowerShell) of `azure provider operations show` (in Azure CLI) lijst activiteiten van Azure resource providers. U kunt deze opdrachten ook controleren of een bewerking tekenreeks geldig is en om uit te breiden tekenreeksen bewerking met jokertekens.

```
Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT Operation, OperationName

Get-AzureRMProviderOperation Microsoft.Network/*
```

![PowerShell screnshot - Get-AzureRMProviderOperation-Microsoft.Compute/virtualMachines/*/action | FT-bewerking, OperationName](./media/role-based-access-control-configure/1-get-azurermprovideroperation-1.png)

```
azure provider operations show "Microsoft.Compute/virtualMachines/*/action" --js on | jq '.[] | .operation'

azure provider operations show "Microsoft.Network/*"
```

![Azure CLI screenshot - azure provider bewerkingen weergeven "Microsoft.Compute/virtualMachines/\*/Action" ](./media/role-based-access-control-configure/1-azure-provider-operations-show.png)

## <a name="notactions"></a>NotActions
Gebruik de eigenschap **NotActions** als de reeks bewerkingen die u wilt toestaan gemakkelijker door beperkte bewerkingen is gedefinieerd. De toegang wordt verleend door een aangepaste rol wordt berekend door bewerkingen van de **NotActions** uit de activiteiten van de **Acties** af te trekken.

> [AZURE.NOTE] Als een gebruiker is toegewezen een rol die worden uitgesloten van een bewerking in **NotActions**en een tweede functie die toegang tot dezelfde bewerking verleent is toegewezen, wordt de gebruiker wordt toegestaan voor deze bewerking. **NotActions** is niet een regel voor weigeren: het is gewoon een handige manier voor het maken van een reeks toegestane bewerkingen als specifieke bewerkingen moeten worden uitgesloten.

## <a name="assignablescopes"></a>AssignableScopes
De eigenschap **AssignableScopes** van de aangepaste rol geeft de scopes (abonnementen, resourcegroepen of resources) waarin de aangepaste rol beschikbaar voor toewijzing is. U kunt de aangepaste rol beschikbaar voor toewijzing in alleen de abonnementen of resourcegroepen die het nodig hebben en niet bruikbaar blijft voor de rest van de abonnementen of resourcegroepen.

Voorbeelden van geldige toewijsbare scopes zijn:

-   "abonnementen/c276fc76-9cd4-44c9-99a7-4fd71546436e", "abonnementen/e91d47c4-76f3-4271-a796-21b4ecfe3624" - maakt de rol beschikbaar voor toewijzing in twee abonnementen.
-   "abonnementen/c276fc76-9cd4-44c9-99a7-4fd71546436e" - maakt de rol beschikbaar voor toewijzing van een abonnement op één.
-  "/ abonnementen/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/netwerk" - maakt de rol beschikbaar voor toewijzing alleen in de resourcegroep van het netwerk.

> [AZURE.NOTE] Moet u ten minste één abonnement, resourcegroep of resource-ID.

## <a name="custom-roles-access-control"></a>Aangepaste rollen van toegangsbeheer
De eigenschap **AssignableScopes** van de aangepaste functie bepaalt ook wie kunt weergeven, wijzigen en verwijderen van de rol.

- Wie kan een aangepaste rol maken?
    Eigenaren (en beheerders toegang) van abonnementen, bronnengroepen en bronnen kunnen aangepaste rollen maken voor gebruik in deze bereiken.
    De gebruiker maakt de rol moet kunnen uitvoeren `Microsoft.Authorization/roleDefinition/write` bewerking op alle **AssignableScopes** van de rol.

- Wie kan een aangepaste rol wijzigen?
    Eigenaren (en beheerders toegang) van abonnementen,, resourcegroepen en bronnen kunt aangepaste rollen in deze bereiken. Gebruikers moeten kunnen uitvoeren van de `Microsoft.Authorization/roleDefinition/write` bewerking op alle **AssignableScopes** van een aangepaste rol.

- Wie aangepaste rollen kan bekijken?
    Alle ingebouwde functies in Azure RBAC kunnen bekijken van functies die beschikbaar voor de toewijzing zijn. Gebruikers die kunnen uitvoeren de `Microsoft.Authorization/roleDefinition/read` bewerking op een scope de RBAC-rollen die beschikbaar voor de toewijzing op dat bereik zijn kunt bekijken.

## <a name="see-also"></a>Zie ook
- [Rollen gebaseerd toegangsbeheer](role-based-access-control-configure.md): aan de slag met RBAC in Azure portal.
- Meer informatie over het beheren van toegang met:
    - [PowerShell](role-based-access-control-manage-access-powershell.md)
    - [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
    - [REST-API](role-based-access-control-manage-access-rest.md)
- [Ingebouwde functies](role-based-access-built-in-roles.md): informatie over de rollen die standaard in RBAC.
