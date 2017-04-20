<properties
   pageTitle="Resources met een sjabloon en REST API implementeren | Microsoft Azure"
   description="Azure Resource Manager en Resource Manager REST API gebruiken een bronnen naar Azure implementeren. De resources worden gedefinieerd in een sjabloon Resource Manager."
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
   ms.date="07/11/2016"
   ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Resources met bronbeheer sjablonen en Resource Manager REST API implementeren

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST-API](resource-group-template-deploy-rest.md)

In dit artikel wordt uitgelegd hoe de bronnenbeheerder REST API met Resource Manager-sjablonen gebruiken voor de implementatie van uw resources naar Azure.  

> [AZURE.TIP] Zie voor hulp bij het opsporen van fouten in een fout tijdens de implementatie:
>
> - [Implementatie bewerkingen weergeven met REST API](resource-manager-troubleshoot-deployments-rest.md) voor meer informatie over het verkrijgen van informatie die u helpt bij problemen met uw fout
> - [Problemen oplossen met veelvoorkomende fouten bij het implementeren van Azure Azure Resource Manager bronnen](resource-manager-common-deployment-errors.md) voor meer informatie over het oplossen van veelvoorkomende implementatiefouten

De sjabloon is een lokaal bestand of een extern bestand dat beschikbaar is via een URI. Wanneer de sjabloon zich in een opslag-account bevindt, kunt u beperken de toegang tot de sjabloon en bieden een toegangstoken voor gedeelde handtekening (SAS) tijdens de implementatie.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-the-rest-api"></a>Met de REST API implementeren
1. [Gemeenschappelijke parameters en koppen](https://msdn.microsoft.com/library/azure/8d088ecc-26eb-42e9-8acc-fe929ed33563#bk_common), met inbegrip van verificatietokens instellen.
2. Als u een bestaande resourcegroep niet hebt, kunt u een resourcegroep maken. Uw abonnements-id, de naam van de nieuwe resourcegroep en de locatie die u nodig hebt voor uw oplossing bieden. Zie [een groep maken](https://msdn.microsoft.com/library/azure/dn790525.aspx)voor meer informatie.

        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
          <common headers>
          {
            "location": "West US",
            "tags": {
               "tagname1": "tagvalue1"
            }
          }
   
3. Valideer uw implementatie voordat deze wordt uitgevoerd door het [valideren van de sjabloonimplementatie van een](https://msdn.microsoft.com/library/azure/dn790547.aspx) -bewerking uit te voeren. Bij het testen van de implementatie bieden parameters precies zoals u zou doen bij het uitvoeren van de implementatie (weergegeven in de volgende stap).

3. Maak een implementatie. Geef uw abonnements-id, de naam van de bronnengroep te implementeren, de naam van de implementatie en een koppeling naar de sjabloon. Zie voor meer informatie over het sjabloonbestand [parameterbestand](#parameter-file). Zie voor meer informatie over de REST API voor het maken van een resourcegroep [maken de sjabloonimplementatie van een](https://msdn.microsoft.com/library/azure/dn790564.aspx). U ziet dat de **modus** is ingesteld op **Incrementeel**. Om een volledige implementatie uitvoert, **modus** worden ingesteld op **voltooid**. Wees voorzichtig bij het gebruik van de volledige modus als u per ongeluk bronnen die niet in de sjabloon kunt verwijderen.
    
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
          <common headers>
          {
            "properties": {
              "templateLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
                "contentVersion": "1.0.0.0",
              },
              "mode": "Incremental",
              "parametersLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
                "contentVersion": "1.0.0.0",
              }
            }
          }
   
      Als u zich aanmelden antwoord inhoud wilt, zijn aanvraag inhoud en/of **debugSetting** in de aanvraag.

        "debugSetting": {
          "detailLevel": "requestContent, responseContent"
        }

      Gebruik van een gedeelde toegangstoken handtekening (SAS) kunt u instellen van uw account voor opslag. Zie [Toegang met een handtekening gedeelde toegang delegeren](https://msdn.microsoft.com/library/ee395415.aspx)voor meer informatie.

4. De status van de distributie van de sjabloon ophalen. Zie [informatie over de sjabloonimplementatie van een](https://msdn.microsoft.com/library/azure/dn790565.aspx)voor meer informatie.

          GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
           <common headers>

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Volgende stappen
- Zie [bronnen implementeren met behulp van .NET bibliotheken en een sjabloon](virtual-machines/virtual-machines-windows-csharp-template.md)voor een voorbeeld van de implementatie van bronnen via de .NET client library.
- Zie [sjablonen ontwerpen](resource-group-authoring-templates.md#parameters)om parameters te definiëren in een sjabloon.
- Zie [ontwikkel- en testomgevingen in Microsoft Azure](solution-dev-test-environments.md)voor instructies over het implementeren van uw oplossing in verschillende omgevingen.
- Zie [veilige waarden tijdens de implementatie](resource-manager-keyvault-parameter.md)voor meer informatie over het gebruik van een verwijzing KeyVault naar veilige waarden doorgeven.
