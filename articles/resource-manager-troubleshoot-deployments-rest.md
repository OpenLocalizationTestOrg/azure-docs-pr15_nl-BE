<properties
   pageTitle="Bewerkingen met REST API implementatie weergeven | Microsoft Azure"
   description="Hierin wordt beschreven hoe de Azure Resource Manager REST-API gebruiken voor het detecteren van problemen van de implementatie van de Resource Manager."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/13/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-resource-manager-rest-api"></a>Implementatie bewerkingen weergeven met Azure Resource Manager REST API

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST-API](resource-manager-troubleshoot-deployments-rest.md)

Als u een fout ontvangen hebt bij de distributie van resources in Azure, wilt u mogelijk meer details over de implementatie van bewerkingen die zijn uitgevoerd. De REST API biedt bewerkingen waarmee u kunt zoeken naar fouten en mogelijke oplossingen bepalen.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Sommige fouten kunt u voorkomen door het valideren van uw sjabloon en voorafgaand aan de distributie-infrastructuur. U kunt zich ook aanmelden aanvullende aanvraag en respons informatie tijdens de implementatie die handig is hoger voor het oplossen van problemen. Zie meer informatie over het valideren van en informatie over de aanvraag en het antwoord van de logboeken, [een bronnengroep met Azure Resource Manager sjabloon distribueren](resource-group-template-deploy-rest.md).

## <a name="troubleshoot-with-rest-api"></a>Problemen oplossen met REST API

1. Implementeer uw resources met de bewerking van [de sjabloonimplementatie van een maken](https://msdn.microsoft.com/library/azure/dn790564.aspx) . Als u wilt bewaren die nuttig zijn kan voor foutopsporing, stelt u de eigenschap **debugSetting** in JSON-aanvraag voor **requestContent** en/of **responseContent**. 

        PUT https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
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
              },
              "debugSetting": {
                "detailLevel": "requestContent, responseContent"
              }
            }
          }

    De waarde **debugSetting** is standaard ingesteld op **geen**. Wanneer de waarde van de **debugSetting** , Overweeg zorgvuldig het type informatie dat u aan tijdens de implementatie doorgeeft. Door de registratie-informatie over de aanvraag of het antwoord, kan u blootstellen gevoelige gegevens die worden opgehaald via de implementatie van bewerkingen. 

2. Informatie opvragen over een distributie met de [informatie over de sjabloonimplementatie van een voor](https://msdn.microsoft.com/library/azure/dn790565.aspx) bewerking.

        GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}

    Let in het bijzonder de **provisioningState** , **correlationId** en **fout** -elementen in het antwoord. De **correlationId** wordt gebruikt voor het bijhouden van gebeurtenissen en kan het handig zijn als u werkt met de technische ondersteuning van een probleem kunt oplossen.
    
        { 
          ...
          "properties": {
            "provisioningState":"Failed",
            "correlationId":"d5062e45-6e9f-4fd3-a0a0-6b2c56b15757",
            ...
            "error":{
              "code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see http://aka.ms/arm-debug for usage details.",
              "details":[{"code":"Conflict","message":"{\r\n  \"error\": {\r\n    \"message\": \"Conflict\",\r\n    \"code\": \"Conflict\"\r\n  }\r\n}"}]
            }  
          }
        }

3. Informatie over de implementatie met de bewerking [alle sjabloon implementatie bewerkingen](https://msdn.microsoft.com/library/azure/dn790518.aspx) ophalen. 

        GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}

    Het antwoord aanvraag en/of antwoord informatie op basis van wat u hebt opgegeven in de eigenschap **debugSetting** tijdens de implementatie opgenomen.
    
        {
          ...
          "properties": 
          {
            ...
            "request":{
              "content":{
                "location":"West US",
                "properties":{
                  "accountType": "Standard_LRS"
                }
              }
            },
            "response":{
              "content":{
                "error":{
                  "message":"Conflict","code":"Conflict"
                }
              }
            }
          }
        }

4. Gebeurtenissen worden opgehaald uit de controlelogboeken voor de distributie aan de [lijst van de gebeurtenissen van een abonnement op](https://msdn.microsoft.com/library/azure/dn931934.aspx) bewerking.

        GET https://management.azure.com/subscriptions/{subscription-id}/providers/microsoft.insights/eventtypes/management/values?api-version={api-version}&$filter={filter-expression}&$select={comma-separated-property-names}


## <a name="next-steps"></a>Volgende stappen

- Zie [veelvoorkomende fouten bij het implementeren van Azure Azure Resource Manager bronnen oplossen](resource-manager-common-deployment-errors.md)voor hulp bij het oplossen van fouten in specifieke implementatie.
- Zie informatie over het gebruik van de controlelogboeken om te controleren van andere soorten acties, [bewerkingen met Resource Manager controleren](resource-group-audit.md).
- Zie [een bronnengroep met Azure Resource Manager sjabloon distribueren](resource-group-template-deploy.md)voor het valideren van uw implementatie voordat deze wordt uitgevoerd.
