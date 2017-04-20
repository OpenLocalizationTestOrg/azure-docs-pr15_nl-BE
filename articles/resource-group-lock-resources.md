<properties 
    pageTitle="Met Resource Manager bronnen vergrendelen | Microsoft Azure" 
    description="Voorkomen dat gebruikers bijwerken of verwijderen van bepaalde bronnen door een beperking toepassen op alle gebruikers en rollen." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="tomfitz"/>

# <a name="lock-resources-with-azure-resource-manager"></a>Azure Resource Manager bronnen vergrendelen

Als beheerder moet u een abonnement, resourcegroep of resource om te voorkomen dat andere gebruikers in uw organisatie per ongeluk verwijderen of wijzigen van kritieke bronnen vergrendelen. U kunt het niveau van de vergrendeling aan **CanNotDelete** of **alleen-lezen**instellen. 

- **CanNotDelete** betekent dat gemachtigde gebruikers kunnen nog steeds lezen en wijzigen van een resource, maar ze kunnen niet worden verwijderd. 
- **Alleen-lezen** betekent dat bevoegde gebruikers van een resource kunnen lezen, maar niet verwijderen of alle acties op uitvoeren. De machtiging voor de resource is beperkt tot de rol van **lezer** . 

**Alleen-lezen** toe te passen kan leiden tot onverwachte resultaten omdat bepaalde bewerkingen die lijkt lezen bewerkingen daadwerkelijk extra acties vereist. Bijvoorbeeld een **alleen-lezen** vergrendeling in een opslag rekening brengen voorkomt u dat alle gebruikers de toetsen aanbieding. De lijst met sleutels-bewerking wordt uitgevoerd via een POST-aanvraag, omdat de geretourneerde toetsen zijn beschikbaar voor schrijfbewerkingen. Voor een ander voorbeeld, een **alleen-lezen** vergrendeling in een App-servicebron brengen voorkomt dat Visual Studio Server Explorer bestanden voor de resource weer te geven omdat die interactie schrijftoegang vereist.

In tegenstelling tot op rollen gebaseerde toegangscontrole met vergrendelingen beheren kunt u een beperking toepassen voor alle gebruikers en rollen. Zie voor meer informatie over het instellen van machtigingen voor gebruikers en rollen, [Azure op rollen gebaseerde toegangscontrole](./active-directory/role-based-access-control-configure.md).

Wanneer u een vergrendeling op een bovenliggende bereik toepast, nemen alle onderliggende bronnen de dezelfde vergrendeling. Ook bronnen die u later toevoegt, nemen de vergrendeling van het bovenliggende object. De meest beperkte vergrendeling in de overname heeft voorrang.

## <a name="who-can-create-or-delete-locks-in-your-organization"></a>Wie kan maken of verwijderen van vergrendelingen binnen uw organisatie

Maken of verwijderen van vergrendelingen beheren, hebt u toegang tot **Microsoft.Authorization/\* ** of **Microsoft.Authorization/locks/\* ** acties. Van de ingebouwde functies, alleen **de eigenaar** en **Beheerder toegang** verleend deze acties.

## <a name="creating-a-lock-through-the-portal"></a>Een vergrendeling via de portal maken

[AZURE.INCLUDE [resource-manager-lock-resources](../includes/resource-manager-lock-resources.md)]

## <a name="creating-a-lock-in-a-template"></a>Een vergrendeling maken in een sjabloon

In het volgende voorbeeld ziet u een sjabloon die een vergrendeling op de rekening van een opslag gemaakt. De opslag-account waarop de vergrendeling van toepassing wordt doorgegeven als parameter. De naam van de vergrendeling wordt gemaakt door de naam van de resource met de **/Microsoft.Authorization/** en de naam van het slot, in dit geval **myLock**aaneen te schakelen.

Het type die is specifiek voor het brontype. Voor opslag, stelt u het type "Microsoft.Storage/storageaccounts/providers/locks".

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "lockedResource": {
          "type": "string"
        }
      },
      "resources": [
        {
          "name": "[concat(parameters('lockedResource'), '/Microsoft.Authorization/myLock')]",
          "type": "Microsoft.Storage/storageAccounts/providers/locks",
          "apiVersion": "2015-01-01",
          "properties": {
            "level": "CannotDelete"
          }
        }
      ]
    }

## <a name="creating-a-lock-with-rest-api"></a>Maken van vergrendeling met REST API

U kunt gedistribueerde bronnen met de [REST API voor beheer van vergrendelingen](https://msdn.microsoft.com/library/azure/mt204563.aspx)vergrendelen. De REST-API kunt u maken en verwijderen van sloten en ophalen van informatie over bestaande vergrendelingen.

Als u wilt vergrendelen, worden uitgevoerd:

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

Het bereik kan een abonnement, resource of resourcegroep zijn. De naam van de vergrendeling is wat u wilt bellen de vergrendeling. Gebruik voor api versie **01-01-2015**.

In de aanvraag opnemen een JSON-object dat de eigenschappen voor de vergrendeling.

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

Zie voor voorbeelden van [REST API voor beheer van vergrendelingen](https://msdn.microsoft.com/library/azure/mt204563.aspx).

## <a name="creating-a-lock-with-azure-powershell"></a>Maken van vergrendeling met Azure PowerShell

Met behulp van de **Nieuwe AzureRmResourceLock** zoals in het volgende voorbeeld kunt u gedistribueerde bronnen met Azure PowerShell vergrendelen.

    New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup

Azure PowerShell bevat andere opdrachten voor werken, sloten, zoals **Set AzureRmResourceLock** een vergrendeling bijwerken en **Verwijderen AzureRmResourceLock** een vergrendeling verwijderen.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het werken met bronvergrendelingen [Vergrendeling omlaag uw Azure Resources](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)
- Zie voor meer informatie over het ordenen van uw resources logisch [labels gebruiken voor het ordenen van uw resources](resource-group-using-tags.md)
- Zie wijzigen welke resourcegroep resource zich bevindt in [bronnen aan de nieuwe resourcegroep verplaatsen](resource-group-move-resources.md)
- U kunt beperkingen en conventies over je abonnement met het aangepaste beleid toepassen. Zie [Beleid voor netwerkbronnen beheren en toegang beheren](resource-manager-policy.md)voor meer informatie.
