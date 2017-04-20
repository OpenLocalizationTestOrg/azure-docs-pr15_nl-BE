<properties
   pageTitle="Sjabloon voor opslag Resource Manager | Microsoft Azure"
   description="Vindt u het schema Resource Manager voor het implementeren van opslag rekeningen door middel van een sjabloon."
   services="azure-resource-manager,storage"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/05/2016"
   ms.author="tomfitz"/>

# <a name="storage-account-template-schema"></a>Schema voor opslag account

Een account voor de opslag maakt.

## <a name="schema-format"></a>Schema-indeling

Toevoegen om een account opslag maken, het volgende schema aan de resources sectie van uw sjabloon.

    {
        "type": "Microsoft.Storage/storageAccounts",
        "apiVersion": "2015-06-15",
        "name": string,
        "location": string,
        "properties": 
        {
            "accountType": string
        }
    }

## <a name="values"></a>Waarden

De volgende tabellen beschrijven de waarden die u nodig hebt om in te stellen in het schema.

| Naam | Waarde |
| ---- | ---- |
| type | Enum<br />Vereist<br />**Microsoft.Storage/storageAccounts**<br /><br />Het brontype te maken. |
| apiVersion | Enum<br />Vereist<br />**2015-06-15** of **2015-05-01-voorbeeld**<br /><br />De versie van de API te gebruiken voor het maken van de resource. | 
| naam | Tekenreeks<br />Vereist<br />Tussen 3 en 24 tekens, alleen cijfers en kleine letters.<br /><br />De naam van de account van de opslag te maken. De naam moet uniek zijn voor alle Azure. Overweeg het gebruik van de functie [uniqueString](resource-group-template-functions.md#uniquestring) met de naamgevingsconventie zoals in het onderstaande voorbeeld. |
| locatie | Tekenreeks<br />Vereist<br />Een regio die opslag accounts ondersteunt. Geldige regio's vindt u [regio's ondersteund](resource-manager-supported-services.md#supported-regions).<br /><br />De regio als host voor de opslag-account. |
| Eigenschappen | Object<br />Vereist<br />[object eigenschappen](#properties)<br /><br />Een object dat het type opslag account maken. |

<a id="properties" />
### <a name="properties-object"></a>object eigenschappen

| Naam | Waarde |
| ---- | ---- | 
| accountType | Tekenreeks<br />Vereist<br />**Standard_LRS**, **Standard_ZRS**, **Standard_GRS**, **Standard_RAGRS**of **Premium_LRS**<br /><br />Het type van opslag-account. De toegestane waarden overeenkomen met de standaard lokaal redundante, standaard redundante Zone Geo-redundante standaard, standaard leestoegang Geo-redundante en Premium lokaal overbodig. Zie voor meer informatie over deze typen [opslag Azure-replicatie](./storage/storage-redundancy.md ). |

    
## <a name="examples"></a>Voorbeelden

In het volgende voorbeeld wordt een standaard lokaal redundante opslag account implementeert met een unieke naam op basis van de resource-id.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Storage/storageAccounts",
                "apiVersion": "2015-06-15",
                "name": "[concat('storage', uniqueString(resourceGroup().id))]",
                "location": "[resourceGroup().location]",
                "properties": 
            {
                    "accountType": "Standard_LRS"
            }
            }
        ],
        "outputs": {}
    }

## <a name="quickstart-templates"></a>QuickStart-sjablonen

Er zijn veel quickstart sjablonen met een account voor opslag. De volgende sjablonen ziet u enkele algemene scenario's:

- [Een standaard opslag-Account maken](https://azure.microsoft.com/documentation/templates/101-storage-account-create)
- [Eenvoudige implementatie van een Windows VM](https://azure.microsoft.com/documentation/templates/101-vm-simple-windows)
- [Eenvoudige implementatie van een Linux-VM](https://azure.microsoft.com/documentation/templates/101-vm-simple-linux)
- [Maak een profiel CDN, met een opslag-Account als oorsprong een CDN-eindpunt](https://azure.microsoft.com/documentation/templates/201-cdn-with-storage-account)
- [Een High Availabilty SharePoint-Farm te maken met 9 VMs via de Powershell DSC-extensie](https://azure.microsoft.com/documentation/templates/sharepoint-server-farm-ha)
- [Eenvoudige implementatie van een 5 knooppunt secure Service Fabric '-Cluster met ingeschakeld af](https://azure.microsoft.com/documentation/templates/service-fabric-secure-cluster-5-node-1-nodetype-wad)
- [Een virtuele Machine maken vanuit een Windows-installatiekopie met 4 gegevensschijven van lege.](https://azure.microsoft.com/documentation/templates/101-vm-multiple-data-disk)


## <a name="next-steps"></a>Volgende stappen

- Zie [Inleiding tot Microsoft Azure opslag](./storage/storage-introduction.md)voor algemene informatie over opslag.
- Sjablonen die een nieuwe opslag-account gebruiken met een virtuele Machine, Zie bijvoorbeeld [een eenvoudige Linux VM implementeren](https://azure.microsoft.com/documentation/templates/101-simple-linux-vm/) of [een eenvoudige Windows-VM implementeren](https://azure.microsoft.com/documentation/templates/101-simple-windows-vm/).
