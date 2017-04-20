<properties
    pageTitle="Een VM maken met een sjabloon Resource Manager | Microsoft Azure"
    description="Een Resource Manager-sjabloon en PowerShell gebruiken om eenvoudig te maken een nieuwe virtuele machine voor Windows."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-virtual-machine-with-a-resource-manager-template"></a>Maak een virtuele Windows-computer met een sjabloon Resource Manager

Dit artikel maakt u kennis met een sjabloon Azure Resource Manager en ziet u het gebruik van PowerShell te implementeren. De sjabloon implementeert een enkele virtuele machine met Windows Server in een nieuwe virtuele netwerk met één subnet.

Het moet ongeveer 20 minuten duren om de stappen in dit artikel.

> [AZURE.IMPORTANT] Als u wilt dat uw VM moet deel uitmaken van een set beschikbaarheid, toevoegen aan de set bij het maken van de VM. Er is momenteel geen manier een VM toevoegen aan een beschikbaarheid nadat deze is gemaakt.

## <a name="step-1-create-the-template-file"></a>Stap 1: De sjabloon maken

U kunt uw eigen sjabloon met behulp van de informatie in [Azure Resource Manager Authoring sjablonen](../resource-group-authoring-templates.md)maken. U kunt ook sjablonen die zijn gemaakt voor u van [Azure QuickStart sjablonen](https://azure.microsoft.com/documentation/templates/)implementeren.

1. Open uw favoriete teksteditor en de vereiste schema-element en het vereiste contentVersion-element toevoegen:

        {
          "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
        }
2. [Parameters](../resource-group-authoring-templates.md#parameters) zijn niet altijd verplicht, maar ze bieden een manier om de invoerwaarden worden wanneer de sjabloon wordt geïmplementeerd. Het element van de parameters en de onderliggende elementen toevoegen na het element contentVersion:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
        }

3. [Variabelen](../resource-group-authoring-templates.md#variables) kunnen worden gebruikt in een sjabloon opgeven voor waarden die kunnen vaak worden gewijzigd of die moeten worden gemaakt van een combinatie van waarden van de parameters. Het element variabelen toevoegen na de sectie parameters:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"  
          },
        }
        
4. [Bronnen](../resource-group-authoring-templates.md#resources) als de virtuele machine, het virtuele netwerk en de opslag account zijn vervolgens gedefinieerd in de sjabloon. De sectie bronnen toevoegen na de sectie variabelen:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "mystorage1",
              "apiVersion": "2015-06-15",
              "location": "[resourceGroup().location]",
              "properties": { "accountType": "Standard_LRS" }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/publicIPAddresses",
              "name": "myip1",
              "location": "[resourceGroup().location]",
              "properties": {
                "publicIPAllocationMethod": "Dynamic",
                "dnsSettings": { "domainNameLabel": "mydns1" }
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/virtualNetworks",
              "name": "myvn1",
              "location": "[resourceGroup().location]",
              "properties": {
                "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
                "subnets": [ {
                  "name": "mysn1",
                  "properties": { "addressPrefix": "10.0.0.0/24" }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/networkInterfaces",
              "name": "mync1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/publicIPAddresses/myip1",
                "Microsoft.Network/virtualNetworks/myvn1"
              ],
              "properties": {
                "ipConfigurations": [ {
                  "name": "ipconfig1",
                  "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                      "id": "[resourceId('Microsoft.Network/publicIPAddresses', 'myip1')]"
                    },
                    "subnet": { "id": "[variables('subnetRef')]" }
                  }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Compute/virtualMachines",
              "name": "myvm1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/networkInterfaces/mync1",
                "Microsoft.Storage/storageAccounts/mystorage1"
              ],
              "properties": {
                "hardwareProfile": { "vmSize": "Standard_A1" },
                "osProfile": {
                  "computerName": "myvm1",
                  "adminUsername": "[parameters('adminUsername')]",
                  "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                  "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version" : "latest"
                  },
                  "osDisk": {
                    "name": "myosdisk1",
                    "vhd": {
                      "uri": "https://mystorage1.blob.core.windows.net/vhds/myosdisk1.vhd"
                    },
                    "caching": "ReadWrite",
                    "createOption": "FromImage"
                  }
                },
                "networkProfile": {
                  "networkInterfaces" : [ {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces','mync1')]"
                  } ]
                }
              }
            } ]
          }
          
    >[AZURE.NOTE] Dit artikel maakt u een virtuele machine met een versie van het besturingssysteem Windows Server. Zie voor meer informatie over het selecteren van andere afbeeldingen kunt [navigeren en afbeeldingen selecteren Azure virtuele machine met Windows PowerShell en de CLI Azure](virtual-machines-linux-cli-ps-findimage.md).  
            
2. De sjabloonbestand opslaan als *VirtualMachineTemplate.json*.

## <a name="step-2-create-the-parameters-file"></a>Stap 2: Maak het parameterbestand

Waarden voor de resourceparameters die zijn gedefinieerd in de sjabloon wilt opgeven, maakt een bestand met de waarden die worden gebruikt wanneer de sjabloon wordt geïmplementeerd.

1. Kopieer deze JSON-inhoud naar een nieuw bestand met de naam *Parameters.json*in de teksteditor:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" }
          }
        }

    >[AZURE.NOTE] Zie voor meer informatie over de [vereisten voor gebruikersnaam en wachtwoord](virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm).

2. Sla het parameterbestand.

## <a name="step-3-install-azure-powershell"></a>Stap 3: Azure PowerShell installeren

[Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) Zie voor informatie over het installeren van de nieuwste versie van Azure PowerShell, uw abonnement selecteren en aanmelden bij uw account.

## <a name="step-4-create-a-resource-group"></a>Stap 4: Een resourcegroep maken

Alle resources moeten worden geïmplementeerd in een [resourcegroep](../azure-resource-manager/resource-group-overview.md).

1. Krijg een lijst met beschikbare locaties waar de resources kunnen worden gemaakt.

        Get-AzureRmLocation | sort DisplayName | Select DisplayName

2. Vervang de waarde van **$locName** met een locatie in de lijst, bijvoorbeeld **US Central**. De variabele maken.

        $locName = "location name"
        
3. De waarde van **$rgName** vervangen door de naam van de nieuwe resourcegroep. De variabele en de resourcegroep maken.

        $rgName = "resource group name"
        New-AzureRmResourceGroup -Name $rgName -Location $locName
        
    Worden er ongeveer uit zoals in het volgende voorbeeld:
    
        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/{subscription-id}/resourceGroups/myrg1

## <a name="step-5-create-the-resources-with-the-template-and-parameters"></a>Stap 5: Resources maken met de sjabloon en parameters

De waarde van **$templateFile** vervangen door het pad en de naam van het sjabloonbestand. De waarde van **$parameterFile** vervangen door het pad en de naam van het parameterbestand. De variabelen maken en vervolgens de sjabloon toepassen. 

        $templateFile = "template file"
        $parameterFile = "parameter file"
        New-AzureRmResourceGroupDeployment -ResourceGroupName $rgName -TemplateFile $templateFile -TemplateParameterFile $parameterFile

    You will see something like this:

        DeploymentName    : VirtualMachineTemplate
        ResourceGroupName : myrg1
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2016 8:11:37 PM
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                            Name             Type                       Value
                            ===============  =========================  ==========
                            adminUsername    String                     mytestacct1
                            adminPassword    SecureString

        Outputs           :

>[AZURE.NOTE] U kunt ook sjablonen en parameters van een account Azure opslag implementeren. Zie voor meer informatie, [Met behulp van Azure PowerShell met Azure opslag](../storage/storage-powershell-guide-full.md).

## <a name="next-steps"></a>Volgende stappen

- Als er problemen met de implementatie, zou in een volgende stap zijn om naar te kijken [Troubleshooting resource groep implementaties met Azure portal](../resource-manager-troubleshoot-deployments-portal.md)
- Informatie over het beheren van de virtuele machine die u hebt gemaakt aan de hand van [beheren virtuele machines met behulp van bronbeheer Azure en PowerShell](virtual-machines-windows-ps-manage.md).
