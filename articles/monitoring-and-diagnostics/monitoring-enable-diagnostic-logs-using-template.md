<properties
    pageTitle="Diagnostische instellingen met behulp van de sjabloon voor een Resource Manager automatisch inschakelen | Microsoft Azure"
    description="Informatie over hoe u een sjabloon Resource Manager gebruiken om diagnostische instellingen waarmee u kunt de diagnostische logboeken naar gebeurtenis Hubs streamen of op te slaan in een opslag-account te maken."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a>Diagnostische instellingen automatisch inschakelen bij bron maken met behulp van de sjabloon voor een Resource Manager
In dit artikel tonen we hoe u kunt een [sjabloon Azure Resource Manager](../resource-group-authoring-templates.md) diagnostische instellingen configureren op een resource, wanneer deze wordt gemaakt. Hiermee kunt u de diagnostische logboeken en maatstelsel gebeurtenis Hubs, archiveren in een opslag-Account of verzenden naar logboek Analytics als een resource wordt gemaakt automatisch wordt gestart.

De methode voor het inschakelen van diagnostische logboeken met behulp van de sjabloon voor een Resource Manager is afhankelijk van het resourcetype.

- **Niet Compute** resources (bijvoorbeeld netwerk beveiligingsgroepen, logica Apps, automatisering) gebruiken [Diagnostische instellingen in dit artikel wordt beschreven](./monitoring-overview-of-diagnostic-logs.md#diagnostic-settings).
- **Berekenen** Bronnen (af/LAD gebaseerde) [af/LAD configuratiebestand dat wordt beschreven in dit artikel](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)gebruiken.

Het configureren van diagnostische gegevens met behulp van een van de methoden worden beschreven in dit artikel.

De basisstappen zijn als volgt:

1. Een sjabloon maken als een JSON-bestand dat wordt beschreven hoe u de bron maakt en een diagnoseprogramma inschakelen.
2. [Implementeren van de sjabloon met behulp van een implementatiemethode](../resource-group-template-deploy.md).

Hieronder geven we een voorbeeld van de sjabloon JSON-bestand die u moet genereren voor niet berekenen en Compute resources.

## <a name="non-compute-resource-template"></a>Sjabloon voor de resource niet berekenen
Bronnen niet berekenen moet u twee dingen doen:

1. Parameters toevoegen aan de blob parameters voor de accountnaam van de opslag, service bus regel-ID en/of OMS logboek Analytics werkruimte-ID (waardoor van de diagnostische logboeken archiveren op een rekening van opslag, streaming van Logboeken op gebeurtenis Hubs en/of verzenden van logboeken naar logboek Analytics).

    ```json
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId":{
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
    ```
2. Voeg in de matrix van de bronnen van de bron waarvan u wilt dat diagnostische logboeken inschakelen, een bron van het type `[resource namespace]/providers/diagnosticSettings`.

    ```json
    "resources": [
      {
        "type": "providers/diagnosticSettings",
        "name": "Microsoft.Insights/service",
        "dependsOn": [
          "[/*resource Id for which Diagnostic Logs will be enabled>*/]"
        ],
        "apiVersion": "2015-07-01",
        "properties": {
          "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
          "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
          "workspaceId": "[parameters('workspaceId')]",
          "logs": [ 
            {
              "category": "/* log category name */",
              "enabled": true,
              "retentionPolicy": {
                "days": 0,
                "enabled": false
              }
            }
          ]
        }
      }
    ]
    ```

De blob eigenschappen voor de diagnostische instelling volgt [de indeling die wordt beschreven in dit artikel](https://msdn.microsoft.com/library/azure/dn931931.aspx).

Hier is een volledig voorbeeld maakt u een beveiligingsgroep netwerk en streaming gebeurtenis Hubs en opslag in een opslag-account ingeschakeld.

```json

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "nsgName": {
      "type": "string",
      "metadata": {
        "description": "Name of the NSG that will be created."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Network/networkSecurityGroups",
      "name": "[parameters('nsgName')]",
      "apiVersion": "2016-03-30",
      "location": "westus",
      "properties": {
        "securityRules": []
      },
      "resources": [
        {
          "type": "providers/diagnosticSettings",
          "name": "Microsoft.Insights/service",
          "dependsOn": [
            "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('nsgName'))]"
          ],
          "apiVersion": "2015-07-01",
          "properties": {
            "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
            "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
            "workspaceId": "[parameters('workspaceId')]",
            "logs": [
              {
                "category": "NetworkSecurityGroupEvent",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              },
              {
                "category": "NetworkSecurityGroupRuleCounter",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              }
            ]
          }
        }
      ],
      "dependsOn": []
    }
  ]
}

```

## <a name="compute-resource-template"></a>Sjabloon voor resource berekenen
Als u wilt dat diagnostische gegevens van een resource Compute, bijvoorbeeld een virtuele Machine of Service Fabric-cluster moet u:

1. De extensie Azure diagnostische gegevens toevoegen aan de definitie van de resource VM.
2. Een opslag account en/of gebeurtenis hub als een parameter opgeven.
3. De inhoud van het WADCfg XML-bestand toevoegen aan de eigenschap XMLCfg, alle XML-tekens correct ontsnappen.

> [AZURE.WARNING] Deze laatste stap is lastig direct. [Zie dit artikel](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md#diagnostics-configuration-variables) voor een voorbeeld van het Schema van diagnostische gegevens configureren gesplitst in variabelen die worden voorafgegaan en correct opgemaakt.

Het hele proces, met inbegrip van monsters, wordt beschreven [in dit document](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md).


## <a name="next-steps"></a>Volgende stappen
- [Lees meer over Azure diagnoselogboeken](./monitoring-overview-of-diagnostic-logs.md)
- [Stream-Azure diagnostische logboeken naar gebeurtenis Hubs](./monitoring-stream-diagnostic-logs-to-event-hubs.md)
