<properties
    pageTitle="Implementeren van Machine Learning werkruimte met Azure Resource Manager sjabloon | Microsoft Azure"
    description="Het implementeren van een werkruimte voor Azure Machine Learning met behulp van de sjabloon Azure Resource Manager"
    services="machine-learning"
    documentationCenter=""
    authors="ahgyger"
    manager="haining"
    editor="garye"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="ahgyger"/>
# <a name="deploy-machine-learning-workspace-using-azure-resource-manager"></a>Machine Learning werkruimte met behulp van bronbeheer Azure implementeren

## <a name="introduction"></a>Inleiding
Met behulp van een sjabloon voor implementatie u tijd bespaart door middel van een schaalbare manier om Azure-bronnenbeheerder samenhangende onderdelen met een validatie te implementeren en mechanisme opnieuw. Azure Machine Learning werkruimten instellen, bijvoorbeeld, moet u eerst een account Azure opslag configureren en vervolgens implementeren van uw werkruimte. Stel dat u dit handmatig doen voor honderden werkruimten. Een eenvoudiger alternatief is een sjabloon Azure Resource Manager gebruiken voor de implementatie van een Azure Machine Learning werkruimte en alle bijbehorende afhankelijkheden. Dit artikel gaat u via deze stapsgewijze procedure. Zie voor een overzicht van Azure Resource Manager, [Azure Resource Manager-overzicht](../azure-resource-manager/resource-group-overview.md).

## <a name="step-by-step-create-a-machine-learning-workspace"></a>Stapsgewijze: een Machine Learning-werkruimte maken
We zullen een Azure bronnengroep maken en vervolgens implementeert een nieuwe Azure opslag en een nieuwe Azure Machine Learning-werkruimte met behulp van de sjabloon voor een Resource Manager. Zodra de installatie voltooid is, zullen wij belangrijke informatie over de werkruimten die zijn gemaakt (de primaire sleutel, de workspaceID en de URL van de werkruimte) afdrukken.

### <a name="create-an-azure-resource-manager-template"></a>Maak een sjabloon Azure Resource Manager
Een Machine Learning werkruimte alleen accounts Azure opslag voor het opslaan van de dataset is gekoppeld.
De volgende sjabloon gebruikt de naam van de resourcegroep voor het genereren van de naam van de opslag en de naam van de werkruimte.  Ook wordt de naam van de opslag als een eigenschap bij het maken van de werkruimte.

```
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "variables": {
        "namePrefix": "[resourceGroup().name]",
        "location": "[resourceGroup().location]",
        "mlVersion": "2016-04-01",
        "stgVersion": "2015-06-15",
        "storageAccountName": "[concat(variables('namePrefix'),'stg')]",
        "mlWorkspaceName": "[concat(variables('namePrefix'),'mlwk')]",
        "mlResourceId": "[resourceId('Microsoft.MachineLearning/workspaces', variables('mlWorkspaceName'))]",
        "stgResourceId": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "storageAccountType": "Standard_LRS"
    },
    "resources": [
        {
            "apiVersion": "[variables('stgVersion')]",
            "name": "[variables('storageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[variables('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "apiVersion": "[variables('mlVersion')]",
            "type": "Microsoft.MachineLearning/workspaces",
            "name": "[variables('mlWorkspaceName')]",
            "location": "[variables('location')]",
            "dependsOn": ["[variables('stgResourceId')]"],
            "properties": {
                "UserStorageAccountId": "[variables('stgResourceId')]"
            }
        }
    ],
    "outputs": {
        "mlWorkspaceObject": {"type": "object", "value": "[reference(variables('mlResourceId'), variables('mlVersion'))]"},
        "mlWorkspaceToken": {"type": "string", "value": "[listWorkspaceKeys(variables('mlResourceId'), variables('mlVersion')).primaryToken]"},
        "mlWorkspaceWorkspaceID": {"type": "string", "value": "[reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId]"},
        "mlWorkspaceWorkspaceLink": {"type": "string", "value": "[concat('https://studio.azureml.net/Home/ViewWorkspace/', reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId)]"}
    }
}

```
Deze sjabloon opslaan als mlworkspace.json bestand onder c:\temp\.

### <a name="deploy-the-resource-group-based-on-the-template"></a>De resourcegroep, gebaseerd op de sjabloon distribueren
* Open PowerShell
* Modules voor Azure Resource Manager en beheer van Azure-Service installeren  

```
# Install the Azure Resource Manager modules from the PowerShell Gallery (press “A”)
Install-Module AzureRM -Scope CurrentUser

# Install the Azure Service Management modules from the PowerShell Gallery (press “A”)
Install-Module Azure -Scope CurrentUser
```

   Als volgt downloaden en installeren van de modules die nodig zijn voor de resterende stappen. Dit moet alleen eenmaal worden uitgevoerd in de omgeving waar u de PowerShell-opdrachten worden uitgevoerd.   

* Azure verifiëren  

```
# Authenticate (enter your credentials in the pop-up window)
Add-AzureRmAccount
```
Deze stap moet worden herhaald voor elke sessie. Zodra u geverifieerd, moet uw abonnementsgegevens worden weergegeven.

![Azure-Account][1]

Nu dat we toegang tot Azure hebben, maken we de resourcegroep.

* Een resourcegroep maken

```
$rg = New-AzureRmResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

Controleer of de resourcegroep op de juist is ingericht. **ProvisioningState** moeten worden "geslaagd."
De naam van de resource wordt gebruikt door de sjabloon voor het genereren van de naam van de opslag. De naam van de opslag moet tussen 3 en 24 tekens bevatten en gebruik alleen cijfers en kleine letters.

![Resourcegroep][2]

* Met de implementatie van resource group, een nieuwe Machine Learning werkruimte implementeren.

```
# Create a Resource Group, TemplateFile is the location of the JSON template.
$rgd = New-AzureRmResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

Zodra de installatie is voltooid, is het eenvoudig om de eigenschappen van de werkruimte die u hebt geïmplementeerd. U kunt bijvoorbeeld de primaire sleutel Token openen.

```
# Access Azure ML Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

Een andere manier tokens van bestaande werkruimte op te halen is via de opdracht Invoke-AzureRmResourceAction. U kunt bijvoorbeeld de primaire en secundaire tokens van alle werkruimten weergeven.

```  
# List the primary and secondary tokens of all workspaces
Get-AzureRmResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |% { Invoke-AzureRmResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}  
```
Nadat u de werkruimte is ingericht, kunt u ook veel Azure Machine Learning Studio taken met behulp van de [Module voor Azure Machine Learning PowerShell](http://aka.ms/amlps)te automatiseren.

## <a name="next-steps"></a>Volgende stappen 
* Meer informatie over [Sjablonen voor schrijven Azure Resource Manager](../resource-group-authoring-templates.md). 
* Bekijk de [Azure Quickstart sjablonen opslagplaats](https://github.com/Azure/azure-quickstart-templates)hebben. 
* Bekijk deze video over [Azure Resource Manager](https://channel9.msdn.com/Events/Ignite/2015/C9-39). 
 
<!--Image references-->
[1]: ../media/machine-learning-deploy-with-resource-manager-template/azuresubscription.png
[2]: ../media/machine-learning-deploy-with-resource-manager-template/resourcegroupprovisioning.png


<!--Link references-->
