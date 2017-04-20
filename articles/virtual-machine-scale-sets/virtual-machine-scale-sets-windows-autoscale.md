<properties
    pageTitle="Automatisch schalen Windows Virtual Machine schaal stelt | Microsoft Azure"
    description="Autoscaling instellen voor een Windows Virtual Machine schaal instellen met Azure PowerShell"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="automatically-scale-machines-in-a-virtual-machine-scale-set"></a>Automatisch schalen machines in een virtuele machine schaal

Virtuele machine schaal sets maken het implementeren en beheren van identieke virtuele machines als een set. Schaal wordt voorzien in een uiterst schaalbare en aanpasbare compute laag ' hyperscale '-toepassingen en Windows-platform images, Linux platform afbeeldingen, aangepaste images en extensies ondersteunen. Zie [Virtuele Machine schaal ingesteld](virtual-machine-scale-sets-overview.md)voor meer informatie over de schaal wordt ingesteld.

In deze zelfstudie wordt beschreven hoe u een set schaal van Windows virtuele machines maken en automatisch schalen van de machines in de set. U maakt de schalen en schalen door een bronnenbeheerder Azure-sjabloon maken en implementeren met behulp van Azure PowerShell ingesteld. Zie voor meer informatie over sjablonen, [sjablonen ontwerpen Azure Resource Manager](../resource-group-authoring-templates.md). Zie voor meer informatie over het automatisch schalen van schaal sets, [Automatische schaling en virtuele Machine schaal wordt ingesteld](virtual-machine-scale-sets-autoscale-overview.md).

In dit artikel wordt u de volgende bronnen en implementeren extensies:

- Microsoft.Storage/storageAccounts
- Microsoft.Network/virtualNetworks
- Microsoft.Network/publicIPAddresses
- Microsoft.Network/loadBalancers
- Microsoft.Network/networkInterfaces
- Microsoft.Compute/virtualMachines
- Microsoft.Compute/virtualMachineScaleSets
- Microsoft.Insights.VMDiagnosticsSettings
- Microsoft.Insights/autoscaleSettings

Zie voor meer informatie over de Resource Manager [Azure berekenen, netwerk en opslag aanbieders onder Azure Resource Manager](../virtual-machines/virtual-machines-windows-compare-deployment-models.md).

## <a name="step-1-install-azure-powershell"></a>Stap 1: Installeer Azure PowerShell

[Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) Zie voor informatie over het installeren van de nieuwste versie van Azure PowerShell en Azure aanmelden uw abonnement te selecteren.

## <a name="step-2-create-a-resource-group-and-a-storage-account"></a>Stap 2: Maak een groep en een account voor opslag

1. **Een resourcegroep maken** – alle resources moeten worden geïmplementeerd op een resourcegroep. [Nieuw AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx) gebruik te maken van een groep met de naam **vmsstestrg1**.

2. **Een opslag-account maken** – met deze account opslag is waar de sjabloon is opgeslagen. [Nieuw AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) gebruik te maken van een opslag-account met de naam **vmsstestsa**.

## <a name="step-3-create-the-template"></a>Stap 3: De sjabloon maken
Een sjabloon Azure Resource Manager kunt u implementeren en beheren van bronnen Azure samen met behulp van een JSON-beschrijving van de bronnen en de implementatie van bijbehorende parameters.

1. Maak het bestand C:\VMSSTemplate.json in uw favoriete editor en de oorspronkelijke JSON structuur ter ondersteuning van de sjabloon toevoegen.

        {
          "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
          },
          "variables": {
          },
          "resources": [
          ]
        }

2. Parameters zijn niet altijd verplicht, maar ze bieden een manier om de invoerwaarden worden wanneer de sjabloon wordt geïmplementeerd. Deze parameters onder het bovenliggende element van parameters die u hebt toegevoegd aan de sjabloon toevoegen.

        "vmName": { "type": "string" },
        "vmSSName": { "type": "string" },
        "instanceCount": { "type": "string" },
        "adminUsername": { "type": "string" },
        "adminPassword": { "type": "securestring" },
        "resourcePrefix": { "type": "string" }

    - Een naam voor de afzonderlijke virtuele machine die wordt gebruikt voor toegang tot de computers in de schaal instellen.
    - De naam van de account van de opslag waar de sjabloon is opgeslagen.
    - Het aantal virtuele machines eerst te maken in de schaal.
    - De naam en het wachtwoord van de administrator-account op de virtuele machines.
    - Een voorvoegsel voor de resources die zijn gemaakt ter ondersteuning van de schaal instellen.

3. Variabelen kunnen worden gebruikt in een sjabloon opgeven voor waarden die kunnen vaak worden gewijzigd of die moeten worden gemaakt van een combinatie van waarden van de parameters. Deze variabelen onder het bovenliggende element van variabelen die u hebt toegevoegd aan de sjabloon toevoegen.

        "dnsName1": "[concat(parameters('resourcePrefix'),'dn1')]",
        "dnsName2": "[concat(parameters('resourcePrefix'),'dn2')]",
        "publicIP1": "[concat(parameters('resourcePrefix'),'ip1')]",
        "publicIP2": "[concat(parameters('resourcePrefix'),'ip2')]",
        "loadBalancerName": "[concat(parameters('resourcePrefix'),'lb1')]",
        "virtualNetworkName": "[concat(parameters('resourcePrefix'),'vn1')]",
        "nicName": "[concat(parameters('resourcePrefix'),'nc1')]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
        "storageAccountSuffix": [ "a", "g", "m", "s", "y" ],
        "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'a')]",
        "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/','Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
          "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
        "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"

  - DNS-namen die worden gebruikt door de netwerkinterfaces.
    - Het IP-Adresnamen en voorvoegsels voor het virtuele netwerk en de subnetten.
    - De namen en de id van het virtuele netwerk laden balancer en netwerkinterfaces.
    - Namen van opslag voor de accounts die zijn gekoppeld aan de machines in de schaal instellen.
    - Instellingen voor de uitbreiding van de diagnostische gegevens die is geïnstalleerd op de virtuele machines. Zie voor meer informatie over de uitbreiding van de diagnostische gegevens van [een virtuele Windows-machine met bewaking en diagnostiek met Azure Resource Manager sjabloon maken](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md).

4. De resource opslag account onder het bovenliggende element voor resources die u hebt toegevoegd aan de sjabloon toevoegen. Deze sjabloon wordt een lus gebruikt om de aanbevolen vijf opslag rekeningen te maken waarin de schijven besturingssysteem en diagnostische gegevens zijn opgeslagen. Deze set van accounts ondersteunt maximaal 100 virtuele machines in een schaal, die het huidige maximum is. Elke account opslag heet met een aanduiding van de letter die is gedefinieerd in de variabelen in combinatie met het voorvoegsel dat u in de parameters voor de sjabloon opgeeft.

        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[concat(parameters('resourcePrefix'), variables('storageAccountSuffix')[copyIndex()])]",
          "apiVersion": "2015-06-15",
          "copy": {
            "name": "storageLoop",
            "count": 5
          },
          "location": "[resourceGroup().location]",
          "properties": { "accountType": "Standard_LRS" }
        },

5. Het virtuele netwerk resource toevoegen. Zie [Resource netwerkaanbieder](../virtual-network/resource-groups-networking.md)voor meer informatie.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[variables('virtualNetworkName')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
            "subnets": [
              {
                "name": "subnet1",
                "properties": { "addressPrefix": "10.0.0.0/24" }
              }
            ]
          }
        },

6. Het openbare IP-adresbronnen die worden gebruikt door de load balancer en netwerkinterface toevoegen.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "[variables('publicIP1')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "[variables('dnsName1')]"
            }
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "[variables('publicIP2')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "[variables('dnsName2')]"
            }
          }
        },

7. De load balancer resource die wordt gebruikt door de schaal set toevoegen. Zie [Azure Resource Manager ondersteuning voor taakverdeling](../load-balancer/load-balancer-arm.md)voor meer informatie.

        {
          "apiVersion": "2015-06-15",
          "name": "[variables('loadBalancerName')]",
          "type": "Microsoft.Network/loadBalancers",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
          ],
          "properties": {
            "frontendIPConfigurations": [
              {
                "name": "loadBalancerFrontEnd",
                "properties": {
                  "publicIPAddress": {
                    "id": "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
                  }
                }
              }
            ],
            "backendAddressPools": [ { "name": "bepool1" } ],
            "inboundNatPools": [
              {
                "name": "natpool1",
                "properties": {
                  "frontendIPConfiguration": {
                    "id": "[variables('frontEndIPConfigID')]"
                  },
                  "protocol": "tcp",
                  "frontendPortRangeStart": 50000,
                  "frontendPortRangeEnd": 50500,
                  "backendPort": 3389
                }
              }
            ]
          }
        },

8. Voeg de interface met de netwerkbron die wordt gebruikt door de afzonderlijke virtuele machine. Omdat de machines in een schaal niet toegankelijk zijn via een openbaar IP-adres, wordt een afzonderlijke virtuele machine gemaakt in hetzelfde virtuele netwerk op afstand toegang tot de machines.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[variables('nicName')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP2'))]",
            "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
          ],
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "publicIPAddress": {
                    "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIP2'))]"
                  },
                  "subnet": {
                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                  }
                }
              }
            ]
          }
        },

9. De afzonderlijke virtuele machine in hetzelfde netwerk als de schaal set toevoegen.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "[parameters('vmName')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "storageLoop",
            "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
          ],
          "properties": {
            "hardwareProfile": { "vmSize": "Standard_A1" },
            "osProfile": {
              "computername": "[parameters('vmName')]",
              "adminUsername": "[parameters('adminUsername')]",
              "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
              "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "2012-R2-Datacenter",
                "version": "latest"
              },
              "osDisk": {
                "name": "[concat(parameters('resourcePrefix'), 'os1')]",
                "vhd": {
                  "uri":  "[concat('https://',parameters('resourcePrefix'),'a.blob.core.windows.net/vhds/',parameters('resourcePrefix'),'os1.vhd')]"
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
            }
          }
        },

10. De schaal van de virtuele machine ingesteld resource toevoegen en de extensie diagnostische gegevens die is geïnstalleerd op alle virtuele machines in de schaal opgeven. Veel van de instellingen voor deze bron zijn vergelijkbaar met de bron van de virtuele machine. De belangrijkste verschillen zijn de capaciteit element waarmee het aantal virtuele machines in de schaal instellen en een upgradePolicy die aangeeft hoe virtuele machines worden bijgewerkt. De set schaal wordt pas gemaakt als de opslag rekeningen worden gemaakt die is opgegeven met het element dependsOn.

            {
              "type": "Microsoft.Compute/virtualMachineScaleSets",
              "apiVersion": "2016-03-30",
              "name": "[parameters('vmSSName')]",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "storageLoop",
                "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
                "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
              ],
              "sku": {
                "name": "Standard_A1",
                "tier": "Standard",
                "capacity": "[parameters('instanceCount')]"
              },
              "properties": {
                "upgradePolicy": {
                  "mode": "Manual"
                },
                "virtualMachineProfile": {
                  "storageProfile": {
                    "osDisk": {
                      "vhdContainers": [
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4], '.blob.core.windows.net/vhds')]"
                      ],
                      "name": "vmssosdisk",
                      "caching": "ReadOnly",
                      "createOption": "FromImage"
                    },
                    "imageReference": {
                      "publisher": "MicrosoftWindowsServer",
                      "offer": "WindowsServer",
                      "sku": "2012-R2-Datacenter",
                      "version": "latest"
                    }
                  },
                  "osProfile": {
                    "computerNamePrefix": "[parameters('vmSSName')]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                  },
                  "networkProfile": {
                    "networkInterfaceConfigurations": [
                      {
                        "name": "networkconfig1",
                        "properties": {
                          "primary": "true",
                          "ipConfigurations": [
                            {
                              "name": "ip1",
                              "properties": {
                                "subnet": {
                                  "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                                },
                                "loadBalancerBackendAddressPools": [
                                  {
                                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/backendAddressPools/bepool1')]"
                                  }
                                ],
                                "loadBalancerInboundNatPools": [
                                  {
                                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/inboundNatPools/natpool1')]"
                                  }
                                ]
                              }
                            }
                          ]
                        }
                      }
                    ]
                  },
                  "extensionProfile": {
                    "extensions": [
                      {
                        "name": "Microsoft.Insights.VMDiagnosticsSettings",
                        "properties": {
                          "publisher": "Microsoft.Azure.Diagnostics",
                          "type": "IaaSDiagnostics",
                          "typeHandlerVersion": "1.5",
                          "autoUpgradeMinorVersion": true,
                          "settings": {
                            "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
                            "storageAccount": "[variables('diagnosticsStorageAccountName')]"
                          },
                          "protectedSettings": {
                            "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-06-15').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                          }
                        }
                      }
                    ]
                  }
                }
              }
            },

11. De resource autoscaleSettings die bepaalt hoe de set schaal aanpast op basis van het processorgebruik op de computers in de schaal set toevoegen.

            {
              "type": "Microsoft.Insights/autoscaleSettings",
              "apiVersion": "2015-04-01",
              "name": "[concat(parameters('resourcePrefix'),'as1')]",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
              ],
              "properties": {
                "enabled": true,
                "name": "[concat(parameters('resourcePrefix'),'as1')]",
                "profiles": [
                  {
                    "name": "Profile1",
                    "capacity": {
                      "minimum": "1",
                      "maximum": "10",
                      "default": "1"
                    },
                    "rules": [
                      {
                        "metricTrigger": {
                          "metricName": "\\Processor(_Total)\\% Processor Time",
                          "metricNamespace": "",
                          "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]",
                          "timeGrain": "PT1M",
                          "statistic": "Average",
                          "timeWindow": "PT5M",
                          "timeAggregation": "Average",
                          "operator": "GreaterThan",
                          "threshold": 50.0
                        },
                        "scaleAction": {
                          "direction": "Increase",
                          "type": "ChangeCount",
                          "value": "1",
                          "cooldown": "PT5M"
                        }
                      }
                    ]
                  }
                ],
                "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
              }
            }

    Voor deze zelfstudie zijn deze waarden van belang:

    - **metricName** - deze waarde is hetzelfde als de Prestatiemeter die is gedefinieerd in de variabele wadperfcounter. De uitbreiding van de diagnostische gegevens worden verzameld met behulp van die variabele, de **processor(_Totaal)\% processortijd** teller.
    - **metricResourceUri** - deze waarde is de bron-id van de virtuele machine schaal set.
    - **timeGrain** : deze waarde is de granulatie van de metrische gegevens die worden verzameld. In deze sjabloon, wordt deze ingesteld op één minuut.
    - **Statistieken** – met deze waarde bepaalt hoe de cijfers voor de automatische schaling actie worden gecombineerd. De mogelijke waarden zijn: gemiddelde, Min, Max. In deze sjabloon wordt worden de gemiddelde totale CPU-gebruik van de virtuele machines verzameld.
    - **waarde voor timeWindow** : deze waarde is de tijd waarin gegevens worden verzameld. Deze moet tussen 5 minuten en 12 uur.
    - **TimeAggregation van** – deze waarde bepaalt hoe de gegevens die worden verzameld over de tijd moeten worden gecombineerd. De standaardwaarde is de gemiddelde. De mogelijke waarden zijn: gemiddelde, Minimum, Maximum, laatste, totaal, telling.
    - **operator** : deze waarde is de operator die wordt gebruikt voor het vergelijken van de metrische gegevens en de drempel. De mogelijke waarden zijn: is gelijk aan NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.
    - **drempel** – dit is de waarde die de schaal actie activeert. Machines worden in deze sjabloon wordt toegevoegd aan de schaal instellen als de gemiddelde CPU-gebruik van machines in de set meer dan 50 is %.
    - **richting** : deze waarde bepaalt de actie die wordt uitgevoerd wanneer de drempelwaarde wordt bereikt. De mogelijke waarden zijn verhoging of verlaging. In deze sjabloon, wordt het aantal virtuele machines in de schaal als de drempelwaarde meer dan 50% in het tijdvenster van de opgegeven is verhoogd.
    - **type** : deze waarde is het soort actie dat moet worden uitgevoerd en moet worden ingesteld op ChangeCount.
    - **waarde** : deze waarde is het aantal virtuele machines die worden toegevoegd of verwijderd uit de set schaal. Deze waarde moet 1 of hoger. De standaardwaarde is 1. In deze sjabloon, het aantal computers in de schaal instellen verhogingen 1 wanneer de drempel is bereikt.
    - **cooldown** : deze waarde is de tijdsduur sinds de laatste actie schaal wachten voordat de volgende actie plaatsvindt. Deze waarde moet liggen tussen één minuut en één week.

12. Het sjabloonbestand opslaan.    

## <a name="step-4-upload-the-template-to-storage"></a>Stap 4: De sjabloon uploaden naar opslag

Als u weet dat de naam en de primaire sleutel van de account van de opslag die u in stap 1 hebt gemaakt, kunt u de sjabloon uploaden.

1.  Stel in het venster Microsoft Azure PowerShell een variabele met de naam van de account van de opslag die u in stap 1 hebt gemaakt.

            $storageAccountName = "vmstestsa"

2.  Een variabele die de primaire sleutel van de opslag-account bevat instellen.

            $storageAccountKey = "<primary-account-key>"

    U kunt deze sleutel ophalen door te klikken op het pictogram sleutel bij het weergeven van de account opslagbron in Azure portal.

3.  De opslag account contextobject maken dat wordt gebruikt voor het valideren van bewerkingen met de account van de opslag.

            $ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

4.  De container voor het opslaan van de sjabloon maken.

            $containerName = "templates"
            New-AzureStorageContainer -Name $containerName -Context $ctx  -Permission Blob

5.  Het sjabloonbestand uploaden naar de nieuwe container.

            $blobName = "VMSSTemplate.json"
            $fileName = "C:\" + $BlobName
            Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob  $blobName -Context $ctx

## <a name="step-5-deploy-the-template"></a>Stap 5: De sjabloon distribueren

Nu dat u de sjabloon hebt gemaakt, kunt u beginnen met het implementeren van de resources. Gebruik deze opdracht om het proces te starten:

    New-AzureRmResourceGroupDeployment -Name "vmsstestdp1" -ResourceGroupName "vmsstestrg1" -TemplateUri "https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json"

Wanneer u op invoeren, wordt u gevraagd waarden op te geven voor de variabelen die u hebt toegewezen. Deze waarden bevatten:

    vmName: vmsstestvm1
      vmSSName: vmsstest1
      instanceCount: 5
      adminUserName: vmadmin1
      adminPassword: VMpass1
      resourcePrefix: vmsstest

Duurt ongeveer 15 minuten voor de resources kunnen worden geïmplementeerd.

>[AZURE.NOTE] Ook kunt u de mogelijkheid van de portal voor de implementatie van de resources. Gebruik deze koppeling: "https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template>"

## <a name="step-6-monitor-resources"></a>Stap 6: Monitor resources

U kunt bepaalde gegevens ophalen over virtuele machine schaal wordt ingesteld met behulp van deze methoden:

 - De portal Azure - momenteel krijgt u een beperkte hoeveelheid gegevens die via de portal.
 - [Azure Resource Explorer](https://resources.azure.com/) - dit programma is het meest geschikt voor het verkennen van de huidige status van uw set schaal. Volg dit pad en ziet u de weergave van het exemplaar van de schaal set die u hebt gemaakt:

        subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines

 - Azure PowerShell - Gebruik deze opdracht om bepaalde informatie te verkrijgen:

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

        Or

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceView

 - Verbinding maken met de afzonderlijke virtuele machine net zoals u zou doen met een andere computer en klikt u op afstand toegang de virtuele machines in de schaal instellen tot voor afzonderlijke processen.

>[AZURE.NOTE] Een complete REST API voor het verkrijgen van informatie over sets met schaal kunt u vinden in de [Virtuele Machine schaal ingesteld](https://msdn.microsoft.com/library/mt589023.aspx)

## <a name="step-7-remove-the-resources"></a>Stap 7: De resources verwijderen

Omdat u in rekening worden gebracht voor de bronnen die worden gebruikt in Azure, is het altijd een goede gewoonte om de bronnen die u niet meer nodig hebt verwijderen. U hoeft niet elke resource afzonderlijk verwijderen uit een resourcegroep. U kunt de resourcegroep verwijderen en alle bronnen worden automatisch verwijderd.

    Remove-AzureRmResourceGroup -Name vmsstestrg1

Als u wilt dat de resourcegroep, kunt u de schaal instellen alleen verwijderen.

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name"

## <a name="next-steps"></a>Volgende stappen

- Beheren van de schaal instellen dat u zojuist hebt gemaakt met de informatie in het [beheren van virtuele machines in een virtuele Machine schaal ingesteld](virtual-machine-scale-sets-windows-manage.md).
- Meer informatie over de verticale schaal aan de hand van [verticale automatisch schalen met virtuele Machine schaal sets](virtual-machine-scale-sets-vertical-scale-reprovision.md)
- Voorbeelden van controlefuncties in [Azure Monitor PowerShell snelle start monsters](../monitoring-and-diagnostics/insights-powershell-samples.md) Azure-Monitor
- Meer informatie over voorzieningen voor kennisgeving in [Acties voor het verzenden van e-mail en webhook meldingen in Azure Monitor automatisch schalen gebruiken](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)
- Meer informatie over het [Gebruik controle meldt zich bij het verzenden van e-mail en webhook meldingen in Azure Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
