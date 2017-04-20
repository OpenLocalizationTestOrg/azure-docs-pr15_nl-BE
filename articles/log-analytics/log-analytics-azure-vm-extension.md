<properties
    pageTitle="Logboek Analytics verbinding Azure virtuele machines | Microsoft Azure"
    description="Voor Windows en Linux virtuele machines die worden uitgevoerd in Azure, is het raadzaam verzamelde logboeken en statistieken door de extensie logboek Analytics Azure VM te installeren. U kunt de Azure portal of PowerShell installeren van de uitbreiding van de virtuele machine logboek Analytics naar Azure VMs."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="richrund"/>

# <a name="connect-azure-virtual-machines-to-log-analytics"></a>Logboek Analytics verbinding Azure virtuele machines

Voor Windows en Linux-computers is de aanbevolen methode voor het verzamelen van Logboeken en statistieken door het installeren van de agent logboek Analytics.

De eenvoudigste manier om het logboek Analytics-agent installeren op Azure virtuele machines is dankzij de uitbreiding logboek Analytics VM.  Met de extensie vereenvoudigt de installatie en wordt automatisch geconfigureerd door de agent om gegevens te verzenden naar de werkruimte logboek Analytics die u opgeeft. De agent wordt ook automatisch bijgewerkt, hebt u de meest recente functies en correcties.

Voor Windows virtuele machines, moet u de extensie van de virtuele machine *Microsoft Monitoring Agent* inschakelen.
Voor Linux virtuele machines, moet u de extensie van de virtuele machine *OMS Agent voor Linux* inschakelen.

Meer informatie over [uitbreidingen van Azure virtual machine](../virtual-machines/virtual-machines-windows-extensions-features.md) en [Linux-agent] (.. / virtual-machines/virtual-machines-linux-agent-user-guide.md).

Wanneer u voor logboekgegevens op basis van agent-collectie, moet u [gegevensbronnen in logboek Analytics](log-analytics-data-sources.md) om de logboeken en de parameters die u wilt verzamelen opgeven.

>[AZURE.IMPORTANT] Als u Analytics logboek logboek om gegevens te indexeren via [Azure diagnostische gegevens](log-analytics-azure-storage.md)configureren en u de agent configureert voor het verzamelen van de dezelfde Logboeken, worden tweemaal de logboekbestanden verzameld. U kunt beide gegevensbronnen zijn belast. Als u de agent is geïnstalleerd, dan u logboekgegevens te verzamelen dient met behulp van de agent alleen - Analytics logboek om logboekgegevens te verzamelen van diagnostische gegevens van Azure niet configureren.

Er zijn drie eenvoudige manieren waarop u de virtuele machine logboek Analytics extensie inschakelen:

+ Via de portal voor Azure
+ Met behulp van Azure PowerShell
+ Met behulp van een sjabloon Azure Resource Manager

## <a name="enable-the-vm-extension-in-the-azure-portal"></a>De in de portal voor Azure VM-extensie inschakelen

U kunt de agent voor logboek Analytics installeren en verbinding maken met de Azure virtuele machine die wordt uitgevoerd met behulp van de [portal Azure](https://portal.azure.com).

### <a name="to-install-the-log-analytics-agent-and-connect-the-virtual-machine-to-a-log-analytics-workspace"></a>Het logboek Analytics agent installeren en verbinding maken met de virtuele machine naar een logboek Analytics werkruimte

1.  Inloggen op de [portal Azure](http://portal.azure.com).
2.  Selecteer **Bladeren** aan de linkerkant van de portal, gaat u naar het **Logboek Analytics (OMS)** en selecteer deze.
3.  Selecteer in de lijst met werkruimten logboek Analytics die u wilt gebruiken met de Azure VM.  
    ![OMS werkruimten](./media/log-analytics-azure-vm-extension/oms-connect-azure-01.png)
4.  Selecteer onder **logboek analytics beheer** **virtuele machines**.  
    ![Virtuele machines](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)
5.  Selecteer in de lijst van **virtuele machines**, de virtuele machine waarop u wilt installeren van de agent. De **Verbindingsstatus OMS** voor VM geeft aan dat deze **niet verbonden**.  
    ![VM niet verbonden](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)
6.  Selecteer **verbinding maken**in de details voor uw virtuele machine. De agent wordt automatisch geïnstalleerd en geconfigureerd voor uw werkruimte logboek Analytics. Dit proces duurt een paar minuten, tijdens welke de status OMS verbinding *verbinding maken is...*  
    ![VM verbinding](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)
7.  Na de installatie en verbinding maken met de agent, wordt de verbindingsstatus **OMS** bijgewerkt zodat **deze werkruimte**.  
    ![Verbonden](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)


## <a name="enable-the-vm-extension-using-powershell"></a>Het gebruik van PowerShell VM-extensie inschakelen

Er zijn verschillende opdrachten voor Azure klassieke virtuele machines en Resource Manager virtuele machines. Hieronder vindt u voorbeelden voor zowel klassieke als Resource Manager virtuele machines.

Gebruik de volgende PowerShell voorbeeld voor klassieke virtuele machines:

```
Add-AzureAccount

$workspaceId = "enter workspace ID here"
$workspaceKey = "enter workspace key here"
$hostedService = "enter hosted service here"

$vm = Get-AzureVM –ServiceName $hostedService

# For Windows VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'MicrosoftMonitoringAgent' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose

# For Linux VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'OmsAgentForLinux' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose
```

Gebruik de volgende PowerShell voorbeeld voor bronbeheer van virtuele machines:

```
Login-AzureRMAccount
Select-AzureSubscription -SubscriptionId "**"

$workspaceName = "your workspace name"
$VMresourcegroup = "**"
$VMresourcename = "**"

$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq $workspaceName})

if ($workspace.Name -ne $workspaceName)
{
    Write-Error "Unable to find OMS Workspace $workspaceName. Do you need to run Select-AzureRMSubscription?"
}

$workspaceId = $workspace.CustomerId
$workspaceKey = (Get-AzureRmOperationalInsightsWorkspaceSharedKeys -ResourceGroupName $workspace.ResourceGroupName -Name $workspace.Name).PrimarySharedKey

$vm = Get-AzureRmVM -ResourceGroupName $VMresourcegroup -Name $VMresourcename
$location = $vm.Location

# For Windows VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'MicrosoftMonitoringAgent' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'MicrosoftMonitoringAgent' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"

# For Linux VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'OmsAgentForLinux' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'OmsAgentForLinux' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"


```
Wanneer u uw virtuele computer configureert met PowerShell, moet u de **Werkruimte-ID** en **Primaire sleutel**. De Id en de sleutel kunt u vinden op de pagina **Instellingen** van de OMS-portal of via PowerShell zoals in het voorgaande voorbeeld.

![Werkruimte-ID en een primaire sleutel](./media/log-analytics-azure-vm-extension/oms-analyze-azure-sources.png)

## <a name="deploy-the-vm-extension-using-a-template"></a>Implementeren van de VM-uitbreiding met behulp van een sjabloon

Met behulp van bronbeheer Azure, kunt u een eenvoudige sjabloon (JSON-indeling) die de implementatie en configuratie van de toepassing definieert. Deze sjabloon is bekend als een sjabloon Resource Manager en biedt een declaratieve manier om implementatie te definiëren. Met behulp van een sjabloon, kunt u herhaaldelijk uw toepassing gedurende de levenscyclus app implementeert en vertrouwen dat uw resources zijn wordt ingezet in een consistente status hebben.

Door de agent logboek Analytics als onderdeel van de Resource Manager-sjabloon, kunt u ervoor zorgen dat elke virtuele machine vooraf geconfigureerde is melden aan uw werkruimte logboek Analytics.

Zie voor meer informatie over de Resource Manager-sjablonen, [sjablonen ontwerpen Azure Resource Manager](../resource-group-authoring-templates.md).

Hier volgt een voorbeeld van een Resource Manager-sjabloon die wordt gebruikt voor de implementatie van een virtuele machine waarop Windows wordt uitgevoerd met de extensie Microsoft Monitoring Agent is geïnstalleerd. Deze sjabloon is een typische virtuele machine, met de volgende aanvullingen:

+ workspaceId en Werkruimtenaam parameters
+ Sectie Microsoft.EnterpriseCloud.Monitoring resource-extensie
+ Afgedrukte documenten uitvoeren om te zoeken naar de workspaceId en workspaceSharedKey


```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Username for the Virtual Machine."
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Password for the Virtual Machine."
      }
    },
    "dnsLabelPrefix": {
       "type": "string",
       "metadata": {
          "description": "DNS Label for the Public IP. Must be lowercase. It should match with the following regular expression: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$ or it will raise an error."
       }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "OMS workspace ID"
      }
    },
    "workspaceName": {
      "type": "string",
      "metadata": {
         "description": "OMD workspace name"
      }
    },
    "windowsOSVersion": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter",
        "Windows-Server-Technical-Preview"
      ],
      "metadata": {
        "description": "The Windows version for the VM. This will pick a fully patched image of this given Windows version. Allowed values: 2008-R2-SP1, 2012-Datacenter, 2012-R2-Datacenter, Windows-Server-Technical-Preview."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]",
    "apiVersion": "2015-06-15",
    "imagePublisher": "MicrosoftWindowsServer",
    "imageOffer": "WindowsServer",
    "OSDiskName": "osdiskforwindowssimple",
    "nicName": "myVMNic",
    "addressPrefix": "10.0.0.0/16",
    "subnetName": "Subnet",
    "subnetPrefix": "10.0.0.0/24",
    "storageAccountType": "Standard_LRS",
    "publicIPAddressName": "myPublicIP",
    "publicIPAddressType": "Dynamic",
    "vmStorageAccountContainerName": "vhds",
    "vmName": "MyWindowsVM",
    "vmSize": "Standard_DS1",
    "virtualNetworkName": "MyVNET",
    "resourceId": "[resourceGroup().id]",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "[variables('apiVersion')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "[variables('storageAccountType')]"
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIPAddressName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
        "dnsSettings": {
          "domainNameLabel": "[parameters('dnsLabelPrefix')]"
        }
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[variables('addressPrefix')]"
          ]
        },
        "subnets": [
          {
            "name": "[variables('subnetName')]",
            "properties": {
              "addressPrefix": "[variables('subnetPrefix')]"
            }
          }
        ]
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
              },
              "subnet": {
                "id": "[variables('subnetRef')]"
              }
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[variables('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": {
          "vmSize": "[variables('vmSize')]"
        },
        "osProfile": {
          "computername": "[variables('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "[variables('imagePublisher')]",
            "offer": "[variables('imageOffer')]",
            "sku": "[parameters('windowsOSVersion')]",
            "version": "latest"
          },
          "osDisk": {
            "name": "osdisk",
            "vhd": {
              "uri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
          }
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
          ]
        },
        "diagnosticsProfile": {
          "bootDiagnostics": {
             "enabled": "true",
             "storageUri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net')]"
          }
        }
      },
      "resources": [
        {
          "type": "extensions",
          "name": "Microsoft.EnterpriseCloud.Monitoring",
          "apiVersion": "[variables('apiVersion')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
          ],
          "properties": {
            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
            "type": "MicrosoftMonitoringAgent",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "workspaceId": "[parameters('workspaceId')]"
            },
            "protectedSettings": {
              "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-03-20').primarySharedKey]"
            }
          }
        }
      ]
    }
  ],
  "outputs": {
      "sharedKeyOutput": {
         "value": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').primarySharedKey]",
         "type": "string"
      },
      "workspaceIdOutput": {
         "value": "[reference(concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').customerId]",
        "type" : "string"
      }
  }
}
```

U kunt een sjabloon kunt implementeren met behulp van de volgende PowerShell-opdracht:

```
New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath
```

## <a name="troubleshooting-windows-virtual-machines"></a>Problemen met Windows virtuele Machines

Als de extensie *Monitoring Agent voor Microsoft* VM-agent niet installeren of reporting kunt u de volgende stappen voor probleemoplossing uitvoeren.

1. Controleer of de agent voor Azure VM is geïnstalleerd en werkt goed met behulp van de stappen in [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).
  + U kunt ook het bestand VM agent-logboek bekijken`C:\WindowsAzure\logs\WaAppAgent.log`
  + Als u het logboek niet bestaat, wordt de VM-agent is niet geïnstalleerd.
    - [De Azure VM-Agent installeren op klassieke VMs](../virtual-machines/virtual-machines-windows-classic-agents-and-extensions.md)
2. Controleer dat de Microsoft Monitoring Agent extensie heartbeat-taak wordt uitgevoerd met behulp van de volgende stappen uit:
  + Log in op de virtuele machine
  + Open Taakplanner en zoek de `update_azureoperationalinsight_agent_heartbeat` taak
  + De taak is ingeschakeld en wordt elke minuut één uitgevoerd bevestigen
  + Controleer het logboekbestand heartbeat in`C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`
3. Controleer de logboekbestanden van Microsoft Monitoring Agent VM-extensie in`C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`
3. Controleer dat de virtuele machine kunt PowerShell scripts uitvoeren
4. Controleer de machtigingen op C:\Windows\temp niet hebt gewijzigd
5. Bekijk de status van de Agent Microsoft controleren door het volgende te typen in een verhoogde PowerShell-venster op de virtuele machine`  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`
6. Bekijk de logboekbestanden van Microsoft Monitoring Agent setup in`C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`

Zie [problemen met extensies voor Windows](../virtual-machines/virtual-machines-windows-extensions-troubleshoot.md)voor meer informatie.

## <a name="troubleshooting-linux-virtual-machines"></a>Problemen met Linux virtuele Machines

Als de extensie van de agent *OMS Agent voor Linux* VM niet installeren of reporting kunt u de volgende stappen voor probleemoplossing uitvoeren.

1. Als de status van extensie is *Onbekend* controleren of de agent voor Azure VM is geïnstalleerd en werkt goed met controleren van het logboekbestand voor VM-agent`/var/log/waagent.log`
  + Als u het logboek niet bestaat, wordt de VM-agent is niet geïnstalleerd.
  - [De Azure VM-Agent installeren op Linux VMs](../virtual-machines/virtual-machines-linux-agent-user-guide.md)
2. Bekijk de OMS-Agent voor andere beschadigde statussen voor Linux VM extensie bestanden zich `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` en`/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`
3. Als de status van extensie in orde is, maar de gegevens niet wordt geladen voor Linux logboekbestanden in de OMS-Agent bekijken`/var/opt/microsoft/omsagent/log/omsagent.log`

Zie [problemen met extensies voor Linux](../virtual-machines/virtual-machines-linux-extensions-troubleshoot.md)voor meer informatie.


## <a name="next-steps"></a>Volgende stappen

+ Configureren van [gegevensbronnen in logboek Analytics](log-analytics-data-sources.md) om op te geven van de logs en statistieken te verzamelen.
+ Om gegevens te verzamelen van virtuele machines [toevoegen logboek Analytics oplossingen uit de galerie met oplossingen](log-analytics-add-solutions.md).
+ [Gegevens verzamelen via Azure diagnostische gegevens](log-analytics-azure-storage.md) voor andere resources die worden uitgevoerd in Azure.

Voor computers die zich niet in Azure, kunt u het logboek Analytics agent installeren met behulp van de methoden die worden beschreven in de volgende artikelen:

+ [Windows-computers verbinding met Analytics logboek](log-analytics-windows-agents.md)
+ [Linux computers aansluiten op logboek Analytics](log-analytics-linux-agents.md)
