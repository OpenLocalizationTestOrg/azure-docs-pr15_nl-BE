<properties
   pageTitle="Implementeren van resources met Azure CLI en sjabloon | Microsoft Azure"
   description="Azure Resource Manager en Azure CLI gebruiken een bronnen naar Azure implementeren. De resources worden gedefinieerd in een sjabloon Resource Manager."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Resources met bronbeheer sjablonen en Azure CLI implementeren

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST-API](resource-group-template-deploy-rest.md)

In dit onderwerp wordt uitgelegd hoe Azure CLI met Resource Manager-sjablonen gebruiken voor de implementatie van uw resources naar Azure.  

> [AZURE.TIP] Zie voor hulp bij het opsporen van fouten in een fout tijdens de implementatie:
>
> - [Implementatie-bewerkingen met Azure CLI weergeven](resource-manager-troubleshoot-deployments-cli.md) voor meer informatie over het verkrijgen van informatie die u helpt bij problemen met uw fout
> - [Problemen oplossen met veelvoorkomende fouten bij het implementeren van Azure Azure Resource Manager bronnen](resource-manager-common-deployment-errors.md) voor meer informatie over het oplossen van veelvoorkomende implementatiefouten

De sjabloon is een lokaal bestand of een extern bestand dat beschikbaar is via een URI. Wanneer de sjabloon zich in een opslag-account bevindt, kunt u beperken de toegang tot de sjabloon en bieden een toegangstoken voor gedeelde handtekening (SAS) tijdens de implementatie.

## <a name="quick-steps-to-deployment"></a>Snelle stappen voor implementatie

Dit artikel wordt beschreven voor alle verschillende opties die beschikbaar zijn tijdens de implementatie. Vaak moet u echter slechts twee eenvoudige opdrachten. Snel aan de slag met de implementatie van de volgende opdrachten gebruiken:

    azure group create -n ExampleResourceGroup -l "West US"
    azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

Voor meer informatie over opties voor de implementatie die beter kunnen worden geschikt voor uw scenario, gaat u verder lezen van dit artikel.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-azure-cli"></a>Implementatie van CLI Azure

Zie als u niet eerder CLI Azure met Resource Manager gebruikt nog, [met behulp van de CLI Azure voor Mac, Linux en Windows met Azure Resource Management](xplat-cli-azure-resource-manager.md).

1. Log in op uw account Azure. Na het opgeven van uw referenties, resultaat de opdracht het van uw aanmelding.

        azure login
  
        ...
        info:    login command OK

2. Als er meerdere abonnementen, bieden de abonnement-id die u wilt gebruiken voor de implementatie.

        azure account set <YourSubscriptionNameOrId>

3. Ga naar Azure Resource Manager-module. U ontvangt een bevestiging van de nieuwe modus.

        azure config mode arm
   
        info:     New mode is arm

4. Als u een bestaande resourcegroep niet hebt, kunt u een resourcegroep maken. Geef de naam van de resourcegroep en de locatie die u nodig hebt voor uw oplossing. U moet een locatie voor de resourcegroep opgeven omdat de resourcegroep worden metagegevens over de resources opgeslagen. Omwille van de compatibiliteit kunt u opgeven waar deze metagegevens worden opgeslagen. In het algemeen wordt aangeraden dat u een locatie voor de meeste van uw resources op te geven. Met behulp van dezelfde locatie, kan uw sjabloon vereenvoudigen.

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

5. Valideer uw implementatie voordat deze wordt uitgevoerd met de opdracht **sjabloon azure groep valideren** . Bij het testen van de implementatie bieden parameters precies zoals u zou doen bij het uitvoeren van de implementatie (weergegeven in de volgende stap).

        azure group template validate -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup

5. Voor de implementatie van resources naar de resourcegroep, voert u de volgende opdracht en de benodigde parameters bieden. De parameters omvatten een naam voor de implementatie, de naam van de resourcegroep, het pad of de URL naar de sjabloon en andere parameters die nodig zijn voor uw scenario. 
   
     U hebt de volgende drie opties voor het verstrekken van parameterwaarden: 

     1. In line parameters en een lokale sjabloon gebruiken. Elke parameter in de indeling is: `"ParameterName": { "value": "ParameterValue" }`. In het volgende voorbeeld ziet u de parameters met escape-tekens.

            azure group deployment create -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment

     2. Inline-parameters gebruiken en een koppeling naar een sjabloon.

            azure group deployment create --template-uri <LinkToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment

     3. Gebruik de parameter-bestand. Zie voor meer informatie over het sjabloonbestand [parameterbestand](#parameter-file).
    
            azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

     Nadat de middelen via een van de drie bovenstaande methoden zijn geïmplementeerd, ziet u een overzicht van de implementatie.
  
        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        ...
        info:    group deployment create command OK

     Om een volledige implementatie uitvoert, **modus** worden ingesteld op **voltooid**. Wees voorzichtig met behulp van deze modus kunt u de bronnen die niet in de sjabloon voor het per ongeluk verwijderen.

        azure group deployment create --mode Complete -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

6. Als u zich aanmelden als u meer informatie over het installeren waarmee u implementatiefouten oplossen wilt, gebruikt u de parameter **debug instelling** . U kunt opgeven dat inhoud van de aanvraag of antwoord inhoud worden vastgelegd met de bewerking voor de implementatie.

        azure group deployment create --debug-setting All -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

## <a name="deploy-template-from-storage-with-sas-token"></a>Sjabloon van opslagcapaciteit met SAS-tokens implementeren

U kunt uw sjablonen toevoegen aan een opslag- en koppeling tijdens de implementatie met een token SAS.

> [AZURE.IMPORTANT] Door de onderstaande stappen te volgen, is de blob met de sjabloon die toegankelijk voor de eigenaar van de account. De blob is echter toegankelijk voor iedereen met de URI die bij het maken van een SAS-token voor de blob. Als een andere gebruiker de URI onderschept, is die gebruiker toegang kunnen krijgen tot de sjabloon. Een SAS-token is een goede manier van het beperken van toegang tot uw sjablonen moet, maar u geen gevoelige gegevens zoals wachtwoorden rechtstreeks in de sjabloon.

### <a name="add-private-template-to-storage-account"></a>Persoonlijke sjabloon toevoegen aan de account van de opslag

Een rekening opslag voor sjablonen worden ingesteld door de volgende stappen uit:

1. Een resourcegroep maken.

        azure group create -n "ManageGroup" -l "westus"

2. Maak een account voor opslag. De naam van de opslag moet uniek zijn voor Azure, dus bieden uw eigen naam voor de account.

        azure storage account create -g ManageGroup -l "westus" --sku-name LRS --kind Storage storagecontosotemplates

3. Variabelen voor de opslag en sleutel instellen.

        export AZURE_STORAGE_ACCOUNT=storagecontosotemplates
        export AZURE_STORAGE_ACCESS_KEY={storage_account_key}

4. Maak een container. De machtiging is ingesteld op **uitschakelen** zodat de container is alleen toegankelijk voor de eigenaar.

        azure storage container create --container templates -p Off 
        
4. De sjabloon toevoegen aan de container.

        azure storage blob upload --container templates -f c:\Azure\Templates\azuredeploy.json
        
### <a name="provide-sas-token-during-deployment"></a>SAS-token te bieden tijdens de implementatie

Voor de implementatie van een persoonlijke sjabloon in een opslag-account, een SAS-token ophalen en opnemen in de URI voor de sjabloon.

1. Maakt een token SAS met machtigingen voor lezen en een verlooptijd toegang te beperken. Stel de verlooptijd zodat er voldoende tijd voor het voltooien van de installatie. De volledige URI van de sjabloon met inbegrip van de SAS-token worden opgehaald.

        expiretime=$(date -I'minutes' --date "+30 minutes")
        fullurl=$(azure storage blob sas create --container templates --blob azuredeploy.json --permissions r --expiry $expiretimetime --json  | jq ".url")

2. De sjabloon distribueren door middel van de URI die de SAS-token bevat.

        azure group deployment create --template-uri $fullurl -g ExampleResourceGroup

Zie voor een voorbeeld van het gebruik van een token SAS met gekoppelde sjablonen [met gekoppelde sjablonen met Azure Resource Manager](resource-group-linked-templates.md).

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Volgende stappen
- Zie [bronnen implementeren met behulp van .NET bibliotheken en een sjabloon](virtual-machines/virtual-machines-windows-csharp-template.md)voor een voorbeeld van de implementatie van bronnen via de .NET client library.
- Zie [sjablonen ontwerpen](resource-group-authoring-templates.md#parameters)om parameters te definiëren in een sjabloon.
- Zie [ontwikkel- en testomgevingen in Microsoft Azure](solution-dev-test-environments.md)voor instructies over het implementeren van uw oplossing in verschillende omgevingen.
- Zie [veilige waarden tijdens de implementatie](resource-manager-keyvault-parameter.md)voor meer informatie over het gebruik van een verwijzing KeyVault naar veilige waarden doorgeven.

