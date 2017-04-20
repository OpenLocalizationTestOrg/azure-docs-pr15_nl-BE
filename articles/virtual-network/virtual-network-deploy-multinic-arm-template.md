<properties
   pageTitle="Implementatie van meerdere NIC VMs met behulp van een sjabloon in Resource Manager | Microsoft Azure"
   description="Informatie over het implementeren van meerdere NIC VMs in Resource Manager met behulp van een sjabloon"
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
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="deploy-multi-nic-vms-using-a-template"></a>Implementatie van meerdere NIC VMs met behulp van een sjabloon

[AZURE.INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassieke implementatiemodel](virtual-network-deploy-multinic-classic-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Omdat op dit moment er geen VMs met een enkele NIC en VMs met meerdere NIC's in dezelfde resourcegroep, kunt u de back-end-servers wilt implementeren in een resourcegroep en alle andere onderdelen in een andere beveiligingsgroep. De volgende stappen uit een groep met de naam *IaaSStory* voor de belangrijkste resourcegroep en *IaaSStory BackEnd* voor de back-endservers gebruiken.

## <a name="prerequisites"></a>Vereisten

Voordat u de back-endservers implementeren kunt, moet u de belangrijkste bronnengroep met alle benodigde bronnen voor dit scenario te implementeren. Volg de onderstaande stappen voor de implementatie van deze bronnen.

1. Navigeer naar [de sjabloonpagina](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Klik op de sjabloonpagina aan de rechterkant van de **bovenliggende groep** **distribueren naar Azure**.
3. Indien nodig, de parameterwaarden wijzigen en volg de stappen in de portal Azure voorbeeld voor de implementatie van de resourcegroep.

> [AZURE.IMPORTANT] Zorg ervoor dat uw opslag namen uniek zijn. Er geen namen van dubbele opslag in Azure.

## <a name="understand-the-deployment-template"></a>Inzicht in de sjabloon voor de implementatie

Voordat u de sjabloon van deze documentatie implementeert, moet dat u weten wat het doet. De volgende stappen geven een goed overzicht van de betreffende sjabloon.

1. Navigeer naar [de sjabloonpagina](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Klik op **azuredeploy.json** om de sjabloonbestand te openen.
3. U ziet de onderstaande *waardereeksen* -parameter. Deze parameter wordt gebruikt om te selecteren welke afbeelding VM te gebruiken voor de databaseserver, samen met het besturingssysteem voor meerdere verwante instellingen.

        "osType": {
          "type": "string",
          "defaultValue": "Windows",
          "allowedValues": [
            "Windows",
            "Ubuntu"
          ],
          "metadata": {
            "description": "Type of OS to use for VMs: Windows or Ubuntu."
          }
        },

4. Ga naar de lijst van variabelen en controleer de definitie voor de variabelen **dbVMSetting** , die hieronder worden beschreven. U ontvangt een van de Arrayelementen in de variabele **dbVMSettings** . Als u bekend met terminologie voor software-ontwikkeling bent, kunt u de variabele **dbVMSettings** als hash-tabel of een dictionay weergeven.

        "dbVMSetting": "[variables('dbVMSettings')[parameters('osType')]]"

5. Stel dat u wilt implementeren Windows VMs SQL op de achtergrond uitgevoerd. Moet de waarde voor **waardereeksen** *Windows*zou zijn en de variabele **dbVMSetting** bevat de hieronder vermelde element dat staat voor de eerste waarde in de variabele **dbVMSettings** .

          "Windows": {
            "vmSize": "Standard_DS3",
            "publisher": "MicrosoftSQLServer",
            "offer": "SQL2014SP1-WS2012R2",
            "sku": "Standard",
            "version": "latest",
            "vmName": "DB",
            "osdisk": "osdiskdb",
            "datadisk": "datadiskdb",
            "nicName": "NICDB",
            "ipAddress": "192.168.2.",
            "extensionDeployment": "",
            "avsetName": "ASDB",
            "remotePort": 3389,
            "dbPort": 1433
          },

6. U ziet dat de **vmSize** bevat de waarde *Standard_DS3*. De grootte van bepaalde VM toestaan voor het gebruik van meerdere netwerkkaarten. U kunt controleren welke groottes VM worden ingeschakeld via het [overzicht van meerdere NIC](virtual-networks-multiple-nics.md)NIC multi.
7. Schuif naar **middelen** en het eerste element ziet. Hierin is een opslag-account. Deze opslag rekening wordt te houden de gegevensschijven die worden gebruikt door elke database VM gebruikt. In dit scenario heeft elke database VM een OS schijf opgeslagen in de normale opslag en twee data schijven opgeslagen in de opslag van SSD-technologie (premium).

        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[parameters('prmStorageName')]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "Storage Account - Premium"
          },
          "properties": {
            "accountType": "[parameters('prmStorageType')]"
          }
        },

8. Ga naar de volgende resource zoals hieronder vermeld. Deze bron geeft de Netwerkinterfacekaart die wordt gebruikt voor toegang tot de database in elke database VM. Let op het gebruik van **de kopieerfunctie in deze bron** . De sjabloon kunt u zo veel VMs als u wilt, op basis van de parameter **dbCount** implementeren. Daarom moet u dezelfde hoeveelheid NIC's voor toegang tot de database, één voor elke VM maken.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[concat(variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "NetworkInterfaces - DB DA"
          },
          "copy": {
            "name": "dbniccount",
            "count": "[parameters('dbCount')]"
          },
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Static",
                  "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(4))]",
                  "subnet": {
                    "id": "[variables('backEndSubnetRef')]"
                  }
                }
              }
            ]
          }
        },

9. Ga naar de volgende resource zoals hieronder vermeld. Deze bron geeft de Netwerkinterfacekaart die wordt gebruikt voor beheer in elke database VM. Nogmaals, moet u een van deze netwerkinterfacekaarten voor elke database VM. Let op het **networkSecurityGroup** -element, een NSG toegang tot RDP/SSH aan deze Netwerkadapter alleen koppelen.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[concat(variables('dbVMSetting').nicName, '-RA-',copyindex(1))]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "NetworkInterfaces - DB RA"
          },
          "copy": {
            "name": "dbniccount",
            "count": "[parameters('dbCount')]"
          },
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "networkSecurityGroup": {
                    "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('remoteAccessNSGName'))]"
                  },
                  "privateIPAllocationMethod": "Static",
                  "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(54))]",
                  "subnet": {
                    "id": "[variables('backEndSubnetRef')]"
                  }
                }
              }
            ]
          }
        },

10. Ga naar de volgende resource zoals hieronder vermeld. Deze bron geeft een ingesteld moeten worden gedeeld door alle database VMs beschikbaarheid. Op die manier garandeert u dat er altijd zal een VM in de set met tijdens onderhoud.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/availabilitySets",
          "name": "[variables('dbVMSetting').avsetName]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "AvailabilitySet - DB"
          }
        },

11. Ga naar de volgende resource. Deze bron geeft de database VMs, zoals in de eerste paar regels die hieronder worden beschreven. Let op het gebruik van **de kopieerfunctie** opnieuw, ervoor te zorgen dat meerdere VMs zijn gemaakt op basis van de parameter **dbCount** . U ziet ook de collectie **dependsOn** . Er worden twee netwerkkaarten behoeft te worden gemaakt voordat de VM is geïmplementeerd, samen met de beschikbaarheid en de opslag-account.

          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "[concat(variables('dbVMSetting').vmName,copyindex(1))]",
          "location": "[variables('location')]",
          "dependsOn": [
            "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
            "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-RA-', copyindex(1))]",
            "[concat('Microsoft.Compute/availabilitySets/', variables('dbVMSetting').avsetName)]",
            "[concat('Microsoft.Storage/storageAccounts/', parameters('prmStorageName'))]"
          ],
          "tags": {
            "displayName": "VMs - DB"
          },
          "copy": {
            "name": "dbvmcount",
            "count": "[parameters('dbCount')]"
          },

12. Schuif naar beneden in de VM-bron op het **networkProfile** -element, zoals hieronder weergegeven. U ziet dat er twee netwerkkaarten wordt voor elke VM. Wanneer u meerdere netwerkkaarten voor een VM maakt, moet u de eigenschap **primary** van één van de NIC's instellen op *true*en de rest op *false*.

        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-DA-',copyindex(1)))]",
              "properties": { "primary": true }
            },
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-RA-',copyindex(1)))]",
              "properties": { "primary": false }
            }
          ]
        }
      }

## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>De sjabloon ARM implementeren met behulp van klik te implementeren

> [AZURE.IMPORTANT] Zorg ervoor dat u de [minimumvereisten](#Pre-requisites) stappen voordat u de onderstaande instructies.

De sjabloon monster beschikbaar in de openbare bibliotheek gebruikt een parameterbestand met de standaard waarden worden gebruikt voor het genereren van de hierboven beschreven scenario. Voor de implementatie van deze sjabloon met Klik implementeren, volgt u [deze link](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC)aan de rechterkant van de **back-end resource groep (Zie documentatie)** op **implementeren op Azure**standaardparameterwaarden indien nodig vervangen en volg de instructies in de portal.

In de volgende afbeelding ziet u de inhoud van de nieuwe resourcegroep na de implementatie.

![Back-end resourcegroep](./media/virtual-network-deploy-multinic-arm-template/Figure2.png)

## <a name="deploy-the-template-by-using-powershell"></a>De sjabloon distribueren via PowerShell

Volg de onderstaande stappen voor de implementatie van de sjabloon die u hebt gedownload met behulp van PowerShell.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

3. Voer het **`New-AzureRmResourceGroup`** cmdlet voor het maken van een groep met behulp van de sjabloon.

        New-AzureRmResourceGroup -Name IaaSStory-Backend -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json'

    Verwachte output:

        ResourceGroupName : IaaSStory-Backend
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  

        Resources         :
                            Name                 Type                                 Location
                            ===================  ===================================  ========
                            ASDB                 Microsoft.Compute/availabilitySets   westus  
                            DB1                  Microsoft.Compute/virtualMachines    westus  
                            DB2                  Microsoft.Compute/virtualMachines    westus  
                            NICDB-DA-1           Microsoft.Network/networkInterfaces  westus  
                            NICDB-DA-2           Microsoft.Network/networkInterfaces  westus  
                            NICDB-RA-1           Microsoft.Network/networkInterfaces  westus  
                            NICDB-RA-2           Microsoft.Network/networkInterfaces  westus  
                            wtestvnetstorageprm  Microsoft.Storage/storageAccounts    westus  

        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend

## <a name="deploy-the-template-by-using-the-azure-cli"></a>De sjabloon distribueren met behulp van de CLI Azure

Volg de onderstaande stappen om de sjabloon implementeert met behulp van de CLI Azure.

1. Als u nooit CLI Azure gebruikt nog, Zie [installeren en configureren van de CLI Azure](../xplat-cli-install.md) en volg de instructies tot aan het punt waar u uw Azure-account en abonnement.
2. Voer het **`azure config mode`** opdracht overschakelen naar de modus Resource Manager, zoals hieronder wordt weergegeven.

        azure config mode arm

    Dit is de verwachte output van de bovenstaande opdracht:

        info:    New mode is arm

3. De [parameterbestand](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json)openen en selecteert u de inhoud opslaan als een bestand op uw computer. Bijvoorbeeld: we het parameterbestand in *parameters.json*opgeslagen.

4. Voer het **`azure group deployment create`** cmdlet voor de implementatie van de nieuwe VNet met behulp van de sjabloon en de parameter-bestanden u hebt gedownload en hierboven is gewijzigd. De lijst die wordt weergegeven nadat u de parameters die worden gebruikt door de uitvoer wordt uitgelegd.

        azure group create -n IaaSStory-Backend -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json -e parameters.json

    Verwachte output:

        info:    Executing command group create
        + Getting resource group IaaSStory-Backend
        + Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
