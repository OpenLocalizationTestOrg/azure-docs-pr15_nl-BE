<properties
   pageTitle="Implementeren van een VM met een statische openbare IP-adres met behulp van een sjabloon in Resource Manager | Microsoft Azure"
   description="Informatie over het implementeren van VMs met een statische openbare IP-adres met behulp van een sjabloon in Resource Manager"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="deploy-a-vm-with-a-static-public-ip-using-a-template"></a>Implementeren van een VM met een statische openbare IP-adres met behulp van een sjabloon

[AZURE.INCLUDE [virtual-network-deploy-static-pip-arm-selectors-include.md](../../includes/virtual-network-deploy-static-pip-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel.

[AZURE.INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="public-ip-resources-in-a-template-file"></a>Openbare IP-bronnen in een sjabloonbestand

U kunt bekijken en downloaden van de [voorbeeldsjabloon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json).

De sectie hieronder toont de definitie van het openbare IP-bron op basis van het bovenstaande scenario.

      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[variables('webVMSetting').pipName]",
        "location": "[variables('location')]",
        "properties": {
          "publicIPAllocationMethod": "Static"
        },
        "tags": {
          "displayName": "PublicIPAddress - Web"
        }
      },

De eigenschap **publicIPAllocationMethod** is ingesteld op *statische*ziet. Deze eigenschap kan op *dynamische* (standaardwaarde) of *statisch*zijn. Deze wordt ingesteld op statische garanties dat het openbare IP-adres toegewezen zal nooit veranderen.

De sectie hieronder ziet u de koppeling tussen het openbare IP-adres en een netwerkinterface.

      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Network/networkInterfaces",
        "name": "[variables('webVMSetting').nicName]",
        "location": "[variables('location')]",
        "tags": {
          "displayName": "NetworkInterface - Web"
        },
        "dependsOn": [
          "[concat('Microsoft.Network/publicIPAddresses/', variables('webVMSetting').pipName)]",
          "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
        ],
        "properties": {
          "ipConfigurations": [
            {
              "name": "ipconfig1",
              "properties": {
                "privateIPAllocationMethod": "Static",
                "privateIPAddress": "[variables('webVMSetting').ipAddress]",
                "publicIPAddress": {
                  "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('webVMSetting').pipName)]"
                },
                "subnet": {
                  "id": "[variables('frontEndSubnetRef')]"
                }
              }
            }
          ]
        }
      },

Let op de **publicIPAddress** -eigenschap die verwijst naar de **Id** van een resource met de naam **variables('webVMSetting').pipName**. Dat is de naam van de openbare IP-bron die hierboven wordt weergegeven.

Ten slotte wordt de netwerkinterface die hierboven vermeld in de eigenschap **networkProfile** van de VM wordt gemaakt.

      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('webVMSetting').nicName)]"
          }
        ]
      }

## <a name="deploy-the-template-by-using-click-to-deploy"></a>De sjabloon implementeren met behulp van klik te implementeren

De sjabloon monster beschikbaar in de openbare bibliotheek gebruikt een parameterbestand met de standaard waarden worden gebruikt voor het genereren van de hierboven beschreven scenario. Klik op **distribueren naar Azure** in het bestand Readme.md voor de [VM met statische PIP](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) -sjabloon voor de implementatie van deze sjabloon met klik te implementeren. De standaardwaarden voor parameters vervangen indien gewenst en voer waarden in voor de parameters leeg.  Volg de instructies in de portal voor het maken van een virtuele machine met een statisch, openbaar IP-adres.

## <a name="deploy-the-template-by-using-powershell"></a>De sjabloon distribueren via PowerShell

Volg de onderstaande stappen voor de implementatie van de sjabloon die u hebt gedownload met behulp van PowerShell.

1. Als u Azure PowerShell nog nooit hebt gebruikt, zien [hoe te installeren en configureren van Azure PowerShell](../powershell-install-configure.md) en volg de instructies in stap 1 tot en met 3.

2. De cmdlet **New-AzureRmResourceGroup** om indien nodig een nieuwe resourcegroep maken in een console PowerShell worden uitgevoerd. Als er al een groep gemaakt, gaat u naar stap 3.

        New-AzureRmResourceGroup -Name PIPTEST -Location westus

    Verwachte output:

        ResourceGroupName : PIPTEST
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/StaticPublicIP

3. Voer de cmdlet **New-AzureRmResourceGroupDeployment** voor de implementatie van de sjabloon in een console PowerShell.

        New-AzureRmResourceGroupDeployment -Name DeployVM -ResourceGroupName PIPTEST `
            -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json `
            -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json

    Verwachte output:

        DeploymentName    : DeployVM
        ResourceGroupName : PIPTEST
        ProvisioningState : Succeeded
        Timestamp         : <Deployment date> <Deployment time>
        Mode              : Incremental
        TemplateLink      :
                            Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/mas
                            ter/IaaS-Story/03-Static-public-IP/azuredeploy.json
                            ContentVersion : 1.0.0.0

        Parameters        :
                            Name                      Type                       Value     
                            ========================  =========================  ==========
                            vnetName                  String                     WTestVNet
                            vnetPrefix                String                     192.168.0.0/16
                            frontEndSubnetName        String                     FrontEnd  
                            frontEndSubnetPrefix      String                     192.168.1.0/24
                            storageAccountNamePrefix  String                     iaasestd  
                            stdStorageType            String                     Standard_LRS
                            osType                    String                     Windows   
                            adminUsername             String                     adminUser
                            adminPassword             SecureString                         

        Outputs           :

## <a name="deploy-the-template-by-using-the-azure-cli"></a>De sjabloon distribueren met behulp van de CLI Azure

Volg de onderstaande stappen om de sjabloon implementeert met behulp van de CLI Azure.

1. Als u nooit CLI Azure gebruikt nog, volg de stappen in het artikel [installeren en configureren van de CLI Azure](../xplat-cli-install.md) en vervolgens de stappen in de CLI verbinding met uw abonnement in de sectie 'Azure-aanmelding gebruiken voor het verifiëren van interactief' van het artikel [verbinding maken met een Azure-abonnement van de Azure-opdrachtregelinterface (CLI Azure)](../xplat-cli-connect.md) .
2. Voer de opdracht **azure config-modus** wilt overschakelen naar de modus Resource Manager, zoals hieronder wordt weergegeven.

        azure config mode arm

    Dit is de verwachte output van de bovenstaande opdracht:

        info:    New mode is arm

3. De [parameterbestand](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json)openen en selecteert u de inhoud opslaan als een bestand op uw computer. In dit voorbeeld worden de parameters worden opgeslagen naar een bestand met de naam *parameters.json*. De parameterwaarden in het bestand desgewenst wijzigen, maar ten minste, wordt aanbevolen dat u de waarde voor de parameter adminPassword naar een unieke, complex wachtwoord wijzigen.

4. Voer de cmdlet **implementatie azure groep maken** voor de implementatie van de nieuwe VNet met behulp van de sjabloon en parameter bestanden u hebt gedownload en hierboven is gewijzigd. Vervang in de opdracht hieronder <path> met het pad naar het bestand is opgeslagen. 

        azure group create -n PIPTEST2 -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json -e <path>\parameters.json

    Verwachte output (lijsten parameterwaarden gebruikt):

        info:    Executing command group create
        + Getting resource group PIPTEST2
        + Creating resource group PIPTEST2
        info:    Created resource group PIPTEST2
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/<Subscription ID>/resourceGroups/PIPTEST2
        data:    Name:                PIPTEST2
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
