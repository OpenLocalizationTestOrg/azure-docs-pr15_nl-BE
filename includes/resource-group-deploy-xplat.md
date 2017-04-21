## <a name="how-to-deploy-with-azure-cli"></a>Implementatie van CLI Azure

1. Aanmelden bij uw account Azure.

        azure login

  Na het opgeven van uw referenties, resultaat de opdracht het van uw aanmelding.

        ...
        info:    login command OK

2. Als er meerdere abonnementen, bieden de abonnement-id die u wilt gebruiken voor de implementatie.

        azure account set <YourSubscriptionNameOrId>

3. Ga naar de module Azure Resource Manager

        azure config mode arm

   U ontvangt bevestiging van de nieuwe modus.

        info:     New mode is arm

4. Als u een bestaande resourcegroep niet hebt, maakt u een nieuwe bronnengroep. Geef de naam van de resourcegroep en de locatie die u nodig hebt voor uw oplossing.

        azure group create -n ExampleResourceGroup -l "West US"

   Een overzicht van de nieuwe resourcegroep wordt geretourneerd.

        info:    Executing command group create
        + Getting resource group ExampleResourceGroup
        + Creating resource group ExampleResourceGroup
        info:    Created resource group ExampleResourceGroup
        data:    Id:                  /subscriptions/####/resourceGroups/ExampleResourceGroup
        data:    Name:                ExampleResourceGroup
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags:
        data:
        info:    group create command OK

5. Een nieuwe installatie voor de resourcegroep maakt, voert u de volgende opdracht en de benodigde parameters bieden. De parameters bevat een naam voor de implementatie, de naam van de resourcegroep, het pad of de URL naar de sjabloon die u hebt gemaakt en andere parameters die nodig zijn voor uw scenario.

   U hebt de volgende opties voor de parameter waarden:

   - In line parameters en een lokale sjabloon gebruiken.

             azure group deployment create -f <PathToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

   - Inline-parameters gebruiken en een koppeling naar een sjabloon.

             azure group deployment create --template-uri <LinkToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

   - Gebruik de parameter-bestand.

             azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

  Wanneer de resourcegroep is geïmplementeerd, ziet u een overzicht van de implementatie.

         info:    Executing command group deployment create
         + Initializing template configurations and parameters
         + Creating a deployment
         ...
         info:    group deployment create command OK


6. Voor informatie over de nieuwste implementatie.

         azure group log show -l ExampleResourceGroup

7. Voor gedetailleerde informatie over de implementatie van storingen.

         azure group log show -l -v ExampleResourceGroup
