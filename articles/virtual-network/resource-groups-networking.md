<properties
   pageTitle="Resource-Provider overzicht | Microsoft Azure"
   description="Meer informatie over de nieuwe Resource netwerkprovider in Azure Resource Manager"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="network-resource-provider"></a>Provider van netwerk bron
De noodzaak van een basis in het succes van vandaag, is de mogelijkheid om te bouwen en te beheren op grote schaal network-toepassingen op een flexibele, flexibele, veilige en herhaalbare manier. Azure Resource Manager (ARM) kunt u toepassingen, zoals een enkele verzameling resources in resourcegroepen maken. Deze middelen worden beheerd door verschillende leveranciers van resource onder de ARM.

Azure Resource Manager is afhankelijk van verschillende providers voor toegang tot uw resources. Er zijn drie belangrijkste bron providers: netwerk, opslag en berekenen. Dit document wordt beschreven, de kenmerken en voordelen van de netwerkprovider Resource, met inbegrip van:

- **Metagegevens** : kunt u informatie toevoegen aan bronnen met behulp van tags. Deze codes kunnen worden gebruikt voor het bijhouden van brongebruik verschillende resourcegroepen en abonnementen.
- **Meer controle over uw netwerk** - netwerk bronnen losjes worden gekoppeld en u kunt ze op een meer gedetailleerde wijze beheren. Dit betekent dat er meer flexibiliteit in het netwerk bronnen beheren.
- **Snellere configuratie** - omdat netwerkbronnen losjes zijn gekoppeld, kunt u deze maken en evoluerende netwerkbronnen parallel. Deze heeft configuratie drastisch verlaagd.
- **Rol gebaseerde toegangscontrole** - RBAC biedt standaardrollen met specifieke beveiligingstaken bereik, naast het maken van aangepaste rollen voor veilig beheer.
- **Eenvoudiger beheer en distributie** - het is gemakkelijker te implementeren en beheren van toepassingen omdat kunt kunt u een volledige toepassing stapel als één collectie van resources in een resourcegroep. En sneller te implementeren, omdat u eenvoudigweg een sjabloon JSON-nettolading kunt implementeren.
- **Snelle aanpassing** - kunt u sjablonen declaratieve stijl herhaalbare en snelle aanpassing van installaties inschakelen.
- **Herhaalbare aanpassing** - kunt u sjablonen declaratieve stijl herhaalbare en snelle aanpassing van installaties inschakelen.
- **Beheerinterfaces** - u kunt een van de volgende interfaces gebruiken om uw bronnen te beheren:
    - REST gebaseerde API
    - PowerShell
    - .NET SDK
    - Node.JS SDK
    - Java SDK
    - Azure CLI
    - Preview Portal
    - Sjabloontaal ARM

## <a name="network-resources"></a>Netwerkbronnen
U kunt nu beheer van netwerkbronnen onafhankelijk van elkaar, omdat ze alle beheerd via een enkele compute bron (een virtuele machine). Hierdoor wordt een hogere mate van flexibiliteit en flexibiliteit bij het schrijven van een schaal van grote en complexe infrastructuur in een resourcegroep.

Een conceptuele weergave van een monster implementatie met betrekking tot een meerlagige toepassing wordt hieronder weergegeven. Elke resource, zoals netwerkinterfacekaarten en openbare IP-adressen, VMs weergeven, kan afzonderlijk worden beheerd.

![Model met netwerk](./media/resource-groups-networking/Figure2.png)

Elke bron bevat een algemene set van eigenschappen en hun afzonderlijke eigenschap. De gemeenschappelijke eigenschappen zijn:

|Eigenschap|Beschrijving|Voorbeeldwaarden|
|---|---|---|
|**naam**|De bronnaam is uniek. Elk resourcetype heeft een eigen beperkingen voor naamgeving.|PIP01, VM01, NIC01|
|**locatie**|Azure regio waarin de resource zich bevindt|westus, eastus|
|**ID**|Unieke identificatie van de URI gebaseerd|/Subscriptions/<subGUID>/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP|

U kunt afzonderlijke eigenschappen van resources in de onderstaande secties controleren.

[AZURE.INCLUDE [virtual-networks-nrp-pip-include](../../includes/virtual-networks-nrp-pip-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-nic-include](../../includes/virtual-networks-nrp-nic-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-nsg-include](../../includes/virtual-networks-nrp-nsg-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-udr-include](../../includes/virtual-networks-nrp-udr-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-vnet-include](../../includes/virtual-networks-nrp-vnet-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-dns-include](../../includes/virtual-networks-nrp-dns-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-lb-include](../../includes/virtual-networks-nrp-lb-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-appgw-include](../../includes/virtual-networks-nrp-appgw-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-vpn-include](../../includes/virtual-networks-nrp-vpn-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-tm-include](../../includes/virtual-networks-nrp-tm-include.md)]

## <a name="management-interfaces"></a>Beheerinterfaces
U kunt uw Azure-netwerkbronnen met behulp van verschillende interfaces beheren. In dit document gaan we in op het trekken van deze interfaces: REST API en sjablonen.

### <a name="rest-api"></a>REST-API
Zoals eerder vermeld, kunnen u netwerkbronnen via tal van interfaces, met inbegrip van de REST API, .NET SDK, Node.JS SDK, SDK voor Java, PowerShell, CLI, Azure Portal en sjablonen beheren.

De Rest API's voldoen aan de specificatie HTTP 1.1-protocol. De algemene structuur van de URI van de API wordt hieronder weergegeven:

    https://management.azure.com/subscriptions/{subscription-id}/providers/{resource-provider-namespace}/locations/{region-location}/register?api-version={api-version}

En de parameters tussen accolades staan de volgende elementen:

- **abonnement-id** - uw Azure abonnements-id.
- **resource-provider-naamruimte** - naamruimte voor de provider die wordt gebruikt. De waarde voor de resource network provider is *Microsoft.Network*.
- **naam van gebied** - naam van het gebied van Azure

De volgende HTTP-methoden worden ondersteund bij het aanroepen van de REST-API:

- **PUT** - gebruikt voor het maken van een bepaald type resource wijzigen van een eigenschap van de resource of een koppeling tussen resources wijzigen.
- **GET** - gebruikt voor het ophalen van gegevens van een resource ingericht.
- **Verwijderen** - gebruikt om een bestaande bron verwijderen.

De aanvraag en het antwoord voldoen aan een nettolading JSON-indeling. Zie voor meer informatie [Azure Resource Management API's](https://msdn.microsoft.com/library/azure/dn948464.aspx).

### <a name="arm-template-language"></a>Sjabloontaal ARM
U kunt ook een declaratieve programmeerstijl en netwerkbronnen beheren met behulp van de sjabloon ARM taal naast het beheren van resources, imperatively (via de API's of SDK) gebruiken.

Een weergave van het monster van een sjabloon vindt u hieronder:

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "<version-number-of-template>",
      "parameters": { <parameter-definitions-of-template> },
      "variables": { <variable-definitions-of-template> },
      "resources": [ { <definition-of-resource-to-deploy> } ],
      "outputs": { <output-of-template> }    
    }

De sjabloon is voornamelijk een JSON-beschrijving van de bronnen en de waarden geïnjecteerd via parameters. In het volgende voorbeeld kan worden gebruikt voor het maken van een virtueel netwerk met subnetten 2.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/VNET.json",
        "contentVersion": "1.0.0.0",
        "parameters" : {
          "location": {
            "type": "String",
            "allowedValues": ["East US", "West US", "West Europe", "East Asia", "South East Asia"],
            "metadata" : {
              "Description" : "Deployment location"
            }
          },
          "virtualNetworkName":{
            "type" : "string",
            "defaultValue":"myVNET",
            "metadata" : {
              "Description" : "VNET name"
            }
          },
          "addressPrefix":{
            "type" : "string",
            "defaultValue" : "10.0.0.0/16",
            "metadata" : {
              "Description" : "Address prefix"
            }

          },
          "subnet1Name": {
            "type" : "string",
            "defaultValue" : "Subnet-1",
            "metadata" : {
              "Description" : "Subnet 1 Name"
            }
          },
          "subnet2Name": {
            "type" : "string",
            "defaultValue" : "Subnet-2",
            "metadata" : {
              "Description" : "Subnet 2 name"
            }
          },
          "subnet1Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.0.0/24",
            "metadata" : {
              "Description" : "Subnet 1 Prefix"
            }
          },
          "subnet2Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.1.0/24",
            "metadata" : {
              "Description" : "Subnet 2 Prefix"
            }
          }
        },
        "resources": [
        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[parameters('virtualNetworkName')]",
          "location": "[parameters('location')]",
          "properties": {
            "addressSpace": {
              "addressPrefixes": [
                "[parameters('addressPrefix')]"
              ]
            },
            "subnets": [
              {
                "name": "[parameters('subnet1Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet1Prefix')]"
                }
              },
              {
                "name": "[parameters('subnet2Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet2Prefix')]"
                }
              }
            ]
          }
        }
        ]
    }

Hebt u de mogelijkheid bieden de parameterwaarden handmatig wanneer u een sjabloon of u kunt een parameterbestand. In het volgende voorbeeld ziet u een mogelijke aantal parameterwaarden moeten worden gebruikt met de bovenstaande sjabloon:

    {
      "location": {
          "value": "East US"
      },
      "virtualNetworkName": {
          "value": "VNET1"
      },
      "subnet1Name": {
          "value": "Subnet1"
      },
      "subnet2Name": {
          "value": "Subnet2"
      },
      "addressPrefix": {
          "value": "192.168.0.0/16"
      },
      "subnet1Prefix": {
          "value": "192.168.1.0/24"
      },
      "subnet2Prefix": {
          "value": "192.168.2.0/24"
      }
    }


De belangrijkste voordelen van het gebruik van sjablonen zijn:

- U kunt een complexe infrastructuur in een resourcegroep in een declaratieve stijl maken. De integratie van het maken van de bronnen, inclusief het beheer van de afhankelijkheid wordt verzorgd door de ARM.
- De infrastructuur kan worden gemaakt in een herhaalbare manier tussen de verschillende regio's en binnen een regio gewoon door parameters te wijzigen.
- De stijl van declaratieve leidt tot kortere doorlooptijd in de sjablonen maken en implementeren van de infrastructuur.

Zie [sjablonen Azure quickstart](https://github.com/Azure/azure-quickstart-templates)voor voorbeeldsjablonen.

Zie voor meer informatie over de taal van de sjabloon ARM [Azure Resource Manager sjabloontaal](../resource-group-authoring-templates.md).

Het bovenstaande voorbeeldsjabloon wordt gebruikt voor het virtuele netwerk en de bronnen van het subnet. Er zijn andere netwerkbronnen die kunt u onderstaande:

### <a name="using-a-template"></a>Met behulp van een sjabloon

U kunt services implementeren voor Azure van een sjabloon met PowerShell, AzureCLI, of door middel van een muisklik implementeren van GitHub. Voor de implementatie van services op basis van een sjabloon in de GitHub, voeren de volgende stappen uit:

1. Open het bestand template3 van GitHub. Open een [virtueel netwerk met twee subnetten](https://github.com/Azure/azure-quickstart-templates/tree/master/101-virtual-network).
2. Klik op **Deploy om Azure**en meldt u zich op de Azure Portal met uw referenties.
3. Controleer of de sjabloon en klik vervolgens op **Opslaan**.
4. Klik op **parameters bewerken** en selecteer een locatie *West VS*, voor de vnet en subnetten.
5. Zo nodig de parameters **ADDRESSPREFIX** en **SUBNETPREFIX** wijzigen en klik op **OK**.
6. Op **een resourcegroep selecteren** en klik vervolgens op in de resourcegroep die u wilt toevoegen van de vnet en de subnetten aan. U kunt ook een nieuwe resourcegroep maken door te klikken op **of nieuwe maken**.
3. Klik op **maken**. U ziet het naast elkaar weergeven van **distributie van de sjabloon wordt ingericht**. Als de installatie is voltooid, ziet u een vergelijkbaar is met een scherm hieronder.

![Sjabloon voorbeelddistributie](./media/resource-groups-networking/Figure6.png)


## <a name="next-steps"></a>Volgende stappen

[Azure taal Resource Manager](../resource-group-authoring-templates.md)

[Azure netwerken – gebruikte sjablonen](https://github.com/Azure/azure-quickstart-templates)

[Resource-Provider berekenen](../virtual-machines/virtual-machines-windows-compare-deployment-models.md)

[Azure Resource Manager-overzicht](../azure-resource-manager/resource-group-overview.md)
