## <a name="how-to-deploy-with-powershell"></a>Het implementeren van PowerShell

1. Aanmelden bij uw account Azure.

          Add-AzureAccount

   Na het opgeven van uw referenties, retourneert de opdracht informatie over uw account.

          Id                             Type       ...
          --                             ----    
          example@contoso.com            User       ...   

2. Als er meerdere abonnementen, bieden de abonnement-id die u wilt gebruiken voor de implementatie. 

          Select-AzureSubscription -SubscriptionID <YourSubscriptionId>

3. Ga naar de module Azure Resource Manager.

          Switch-AzureMode AzureResourceManager

4. Als u een bestaande resourcegroep niet hebt, maakt u een nieuwe bronnengroep. Geef de naam van de resourcegroep en de locatie die u nodig hebt voor uw oplossing.

        New-AzureResourceGroup -Name ExampleResourceGroup -Location "West US"

   Een overzicht van de nieuwe resourcegroep wordt geretourneerd.

        ResourceGroupName : ExampleResourceGroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                    Actions  NotActions
                    =======  ==========
                    *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

5. Als u wilt een nieuwe implementatie voor de resourcegroep hebt gemaakt, voert u de opdracht **Nieuw AzureResourceGroupDeployment** en de benodigde parameters bieden. De parameters bevat een naam voor de implementatie, de naam van de resourcegroep, het pad of de URL naar de sjabloon die u hebt gemaakt en andere parameters die nodig zijn voor uw scenario. 
   
   U hebt de volgende opties voor de parameter waarden: 
   
   - In line parameters gebruiken.

            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -myParameterName "parameterValue"

   - Een parameter-object gebruiken.

            $parameters = @{"<ParameterName>"="<Parameter Value>"}
            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterObject $parameters

   - Met behulp van een parameterbestand.

            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterFile <PathOrLinkToParameterFile>

  Wanneer de resourcegroep is geïmplementeerd, ziet u een overzicht van de implementatie.

             DeploymentName    : ExampleDeployment
             ResourceGroupName : ExampleResourceGroup
             ProvisioningState : Succeeded
             Timestamp         : 4/14/2015 7:00:27 PM
             Mode              : Incremental
             ...

6. Voor informatie over de implementatie van storingen.

        Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed

7. Voor gedetailleerde informatie over de implementatie van storingen.

        Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed -DetailedOutput
