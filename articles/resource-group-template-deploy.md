<properties
   pageTitle="Implementeren van resources met PowerShell en sjabloon | Microsoft Azure"
   description="Azure Resource Manager en Azure PowerShell gebruiken voor de implementatie van een bronnen naar Azure. De resources worden gedefinieerd in een sjabloon Resource Manager."
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

# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a>Resources met bronbeheer sjablonen en Azure PowerShell implementeren

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST-API](resource-group-template-deploy-rest.md)

In dit onderwerp wordt uitgelegd hoe Azure PowerShell met Resource Manager-sjablonen gebruiken voor de implementatie van uw resources naar Azure.  

> [AZURE.TIP] Zie voor hulp bij het opsporen van fouten in een fout tijdens de implementatie:
>
> - [Implementatie-bewerkingen met Azure PowerShell weergeven](resource-manager-troubleshoot-deployments-powershell.md) voor meer informatie over het verkrijgen van informatie die u helpt bij problemen met uw fout
> - [Problemen oplossen met veelvoorkomende fouten bij het implementeren van Azure Azure Resource Manager bronnen](resource-manager-common-deployment-errors.md) voor meer informatie over het oplossen van veelvoorkomende implementatiefouten

De sjabloon is een lokaal bestand of een extern bestand dat beschikbaar is via een URI. Wanneer de sjabloon zich in een opslag-account bevindt, kunt u beperken de toegang tot de sjabloon en bieden een toegangstoken voor gedeelde handtekening (SAS) tijdens de implementatie.

## <a name="quick-steps-to-deployment"></a>Snelle stappen voor implementatie

Dit artikel wordt beschreven voor alle verschillende opties die beschikbaar zijn tijdens de implementatie. Vaak moet u echter slechts twee eenvoudige opdrachten. Snel aan de slag met de implementatie van de volgende opdrachten gebruiken:

    New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
    New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

Voor meer informatie over opties voor de implementatie die beter kunnen worden geschikt voor uw scenario, gaat u verder lezen van dit artikel.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-powershell"></a>Met PowerShell implementeren

1. Log in op uw account Azure.

        Add-AzureRmAccount

     Een overzicht van uw account wordt geretourneerd.

        Environment : AzureCloud
        Account     : someone@example.com
        ...

2. Als er meerdere abonnementen, bieden de abonnement-ID die u wilt gebruiken voor de implementatie met de opdracht **Set-AzureRmContext** . 

        Set-AzureRmContext -SubscriptionID <YourSubscriptionId>

3. Normaal gesproken bij het implementeren van een nieuwe sjabloon wilt u maken van een resourcegroep die de resources bevatten. Als u een bestaande resourcegroep die u wilt implementeren, kunt u deze stap overslaan en die bronnengroep gebruikt. 

     Geef een naam en locatie voor de resourcegroep wilt maken van een resourcegroep. U moet een locatie voor de resourcegroep opgeven omdat de resourcegroep worden metagegevens over de resources opgeslagen. Omwille van de compatibiliteit kunt u opgeven waar deze metagegevens worden opgeslagen. In het algemeen wordt aangeraden dat u een locatie voor de meeste van uw resources op te geven. Met behulp van dezelfde locatie, kan uw sjabloon vereenvoudigen.

        New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
   
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

4. Voordat de installatie wordt uitgevoerd, kunt u de implementatie-instellingen te valideren. De **Test-AzureRmResourceGroupDeployment** -cmdlet kunt u zoeken naar problemen voordat u de werkelijke resources. In het volgende voorbeeld ziet u hoe een implementatie te valideren.

        Test-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

5. Voor de implementatie van resources naar de resourcegroep, voert u de opdracht **Nieuw-AzureRmResourceGroupDeployment** en de benodigde parameters bieden. De parameters omvatten een naam voor de implementatie, de naam van de resourcegroep, het pad of de URL naar de sjabloon die u hebt gemaakt en andere parameters die nodig zijn voor uw scenario. Als de parameter **Mode** niet is opgegeven, wordt de standaardwaarde van **incrementele** gebruikt. Om een volledige implementatie uitvoert, **modus** worden ingesteld op **voltooid**. Wees voorzichtig bij het gebruik van de volledige modus als u per ongeluk bronnen die niet in de sjabloon kunt verwijderen.

     Als u wilt implementeren op een lokale sjabloon, gebruikt u de parameter **sjabloonbestand** :

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

     Gebruik de parameter **TemplateUri** voor de implementatie van een externe sjabloon:

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate>
   
     U hebt de volgende opties voor de parameter waarden: 
   
     1. In line parameters gebruiken.

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -myParameterName "parameterValue"

     2. Een parameter-object gebruiken.

            $parameters = @{"<ParameterName>"="<Parameter Value>"}
            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterObject $parameters

     3. Een lokale parameterbestand gebruiken. Zie voor meer informatie over het sjabloonbestand [parameterbestand](#parameter-file).

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

     4. Gebruik een externe parameterbestand. Zie voor meer informatie over het sjabloonbestand [parameterbestand](#parameter-file). 

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate> -TemplateParameterUri <LinkToParameterFile>

        Wanneer u een parameterbestand met externe gebruikt, kan niet geeft u andere waarden van een in line of uit een lokaal bestand. Zie de [Parameter prioriteit](#parameter-precendence)voor meer informatie.

     Nadat de resources zijn geïmplementeerd, ziet u een overzicht van de implementatie.

        DeploymentName    : ExampleDeployment
        ResourceGroupName : ExampleResourceGroup
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2015 7:00:27 PM
        Mode              : Incremental
        ...

     Als de sjabloon een parameter met de naam van een van de parameters in de PowerShell-opdracht bevat, wordt u gevraagd om een waarde voor die parameter. De parameter van de sjabloon worden opgenomen in de postfix- **FromTemplate**. Bijvoorbeeld een parameter met de naam **ResourceGroupName** in het sjabloon in strijd met de parameter **ResourceGroupName** in de cmdlet [New-AzureRmResourceGroupDeployment](https://msdn.microsoft.com/library/azure/mt679003.aspx) . U wordt gevraagd om een waarde voor **ResourceGroupNameFromTemplate**. In het algemeen, voorkomt u deze verwarring door de naam van parameters niet met dezelfde naam als de parameters die worden gebruikt voor de implementatie.

6. Als u zich aanmelden als u meer informatie over het installeren waarmee u de implementatiefouten oplossen wilt, gebruikt u de parameter **DeploymentDebugLogLevel** . U kunt opgeven dat inhoud van de aanvraag of antwoord inhoud worden vastgelegd met de bewerking voor de implementatie.

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -DeploymentDebugLogLevel All -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate>
        
     Zie voor meer informatie over het gebruik van deze inhoud voor foutopsporing voor het oplossen van problemen met implementaties [Troubleshooting resource groep implementaties met Azure PowerShell](resource-manager-troubleshoot-deployments-powershell.md).

## <a name="deploy-template-from-storage-with-sas-token"></a>Sjabloon van opslagcapaciteit met SAS-tokens implementeren

U kunt uw sjablonen toevoegen aan een opslag- en koppeling tijdens de implementatie met een token SAS.

> [AZURE.IMPORTANT] Door de onderstaande stappen te volgen, is de blob met de sjabloon die toegankelijk voor de eigenaar van de account. De blob is echter toegankelijk voor iedereen met de URI die bij het maken van een SAS-token voor de blob. Als een andere gebruiker de URI onderschept, is die gebruiker toegang kunnen krijgen tot de sjabloon. Een SAS-token is een goede manier van het beperken van toegang tot uw sjablonen moet, maar u geen gevoelige gegevens zoals wachtwoorden rechtstreeks in de sjabloon.

### <a name="add-private-template-to-storage-account"></a>Persoonlijke sjabloon toevoegen aan de account van de opslag

Een rekening opslag voor sjablonen worden ingesteld door de volgende stappen uit:

1. Een resourcegroep maken.

        New-AzureRmResourceGroup -Name ManageGroup -Location "West US"

2. Maak een account voor opslag. De naam van de opslag moet uniek zijn voor Azure, dus bieden uw eigen naam voor de account.

        New-AzureRmStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates -Type Standard_LRS -Location "West US"

3. De huidige opslag rekening instellen.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

4. Maak een container. De machtiging is ingesteld op **uitschakelen** zodat de container is alleen toegankelijk voor de eigenaar.

        New-AzureStorageContainer -Name templates -Permission Off
        
5. De sjabloon toevoegen aan de container.

        Set-AzureStorageBlobContent -Container templates -File c:\Azure\Templates\azuredeploy.json
        
### <a name="provide-sas-token-during-deployment"></a>SAS-token te bieden tijdens de implementatie

Voor de implementatie van een persoonlijke sjabloon in een opslag-account, een SAS-token ophalen en opnemen in de URI voor de sjabloon.

1. Als u de huidige opslagruimte-account hebt gewijzigd, stelt u de huidige opslag account aan met uw sjablonen.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

2. Maakt een token SAS met machtigingen voor lezen en een verlooptijd toegang te beperken. De volledige URI van de sjabloon met inbegrip van de SAS-token worden opgehaald.

        $templateuri = New-AzureStorageBlobSASToken -Container templates -Blob azuredeploy.json -Permission r -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

3. De sjabloon distribueren door middel van de URI die de SAS-token bevat.

        New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri $templateuri

Zie voor een voorbeeld van het gebruik van een token SAS met gekoppelde sjablonen [met gekoppelde sjablonen met Azure Resource Manager](resource-group-linked-templates.md).

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="parameter-precedence"></a>Parameter voorrang

U kunt in line parameters en een lokale parameterbestand in dezelfde bewerking implementatie. U kunt bijvoorbeeld bepaalde waarden opgeven in de parameter local-bestand en andere waarden inline toevoegen tijdens de implementatie. Als u waarden voor een parameter in de parameter local-bestand en de in line opgeeft, voorrang de in line.

U kunt inline-parameters echter niet gebruiken met een externe parameterbestand. Wanneer u een parameterbestand in de parameter **TemplateParameterUri** opgeeft, worden alle in line parameters genegeerd. U kunt alle parameterwaarden in het externe bestand moet opgeven. Als uw sjabloon een gevoelige waarde bevat waarmee u kunt niet in het parameterbestand opnemen, die waarde toevoegen aan een sleutel kluis en verwijst naar de sleutel kluis in uw externe parameterbestand, of dynamisch bieden alle parameter waarden in tekst.

Zie [veilige waarden tijdens de implementatie](resource-manager-keyvault-parameter.md)voor meer informatie over het gebruik van een verwijzing KeyVault naar veilige waarden doorgeven.

## <a name="next-steps"></a>Volgende stappen
- Zie [bronnen implementeren met behulp van .NET bibliotheken en een sjabloon](virtual-machines/virtual-machines-windows-csharp-template.md)voor een voorbeeld van de implementatie van bronnen via de .NET client library.
- Zie [sjablonen ontwerpen](resource-group-authoring-templates.md#parameters)om parameters te definiëren in een sjabloon.
- Zie [ontwikkel- en testomgevingen in Microsoft Azure](solution-dev-test-environments.md)voor instructies over het implementeren van uw oplossing in verschillende omgevingen.

