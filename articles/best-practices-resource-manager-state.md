<properties
    pageTitle="Verwerking staat in Bronbeheer sjablonen | Microsoft Azure"
    description="Aanbevolen methoden voor het gebruik van complexe objecten staat om gegevens te delen met Azure Resource Manager-sjablonen en gekoppelde sjablonen ziet"
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="tomfitz"/>

# <a name="sharing-state-in-azure-resource-manager-templates"></a>Delen staat in Azure Resource Manager-sjablonen

Dit onderwerp bevat aanbevolen procedures voor het beheren en delen van de staat in sjablonen. De parameters en variabelen die in dit onderwerp worden voorbeelden van het type objecten u kunt gemakkelijk uw implementatievereisten organiseren. Uit deze voorbeelden, kunt u uw eigen objecten met eigenschapwaarden die zinvol zijn voor uw omgeving implementeren.

Dit onderwerp is onderdeel van een grotere witboek. Om te lezen het volledige papier, [World Class Resource Manager sjablonen overwegingen en beproefde praktijken](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf)te downloaden.


## <a name="provide-standard-configuration-settings"></a>Standaardconfiguratie-instellingen

In plaats van een sjabloon waarmee u totale flexibiliteit en talloze variaties aanbieden, is een vast patroon bieden een selectie van de bekende configuraties. Gebruikers kunnen in feite standaardformaten t-shirt zoals sandbox, kleine, middelgrote en grote selecteren. Andere voorbeelden van t-shirt formaten zijn productaanbod zoals community edition of enterprise edition. In andere gevallen kan het zijn werkbelasting specifieke configuraties van een technologie – zoals kaart verminderen of geen sql.

U kunt met complexe objecten, variabelen met verzamelingen van gegevens, ook wel 'Eigenschappenverzamelingen' maken en deze gegevens gebruiken om de verklaring van de resource in de sjabloon. Deze benadering biedt goede, bekende configuraties met verschillende capaciteiten die zijn geconfigureerd voor klanten. Zonder de bekende configuraties, moeten gebruikers van de sjabloon cluster zelf het formaat bepalen factor resourcebeperkingen platform en rekenen ter identificatie van de resulterende partitionering van opslag rekeningen en andere bronnen (door cluster grootte en resource beperkingen). Naast het feit dat een betere ervaring voor de klant, een paar bekende configuraties zijn gemakkelijker te ondersteunen en kunt u ervoor zorgen dat een hoger niveau van de dichtheid.

In het volgende voorbeeld ziet u hoe variabelen met complexe objecten voor de representatie van verzamelingen van gegevens definiëren. De collecties definiëren waarden die worden gebruikt voor de grootte van de virtuele machine, netwerkinstellingen, -instellingen en instellingen voor de beschikbaarheid.

    "variables": {
      "tshirtSize": "[variables(concat('tshirtSize', parameters('tshirtSize')))]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      },
      "tshirtSizeMedium": {
        "vmSize": "Standard_A3",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-8disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1],
          "jumpbox": 0
        }
      },
      "tshirtSizeLarge": {
        "vmSize": "Standard_A4",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-16disk-resources.json')]",
        "vmCount": 3,
        "slaveCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1,1],
          "jumpbox": 0
        }
      },
      "osSettings": {
        "scripts": [
          "[concat(variables('templateBaseUrl'), 'install_postgresql.sh')]",
          "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
        ],
        "imageReference": {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "14.04.2-LTS",
          "version": "latest"
        }
      },
      "networkSettings": {
        "vnetName": "[parameters('virtualNetworkName')]",
        "addressPrefix": "10.0.0.0/16",
        "subnets": {
          "dmz": {
            "name": "dmz",
            "prefix": "10.0.0.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          },
          "data": {
            "name": "data",
            "prefix": "10.0.1.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          }
        }
      },
      "availabilitySetSettings": {
        "name": "pgsqlAvailabilitySet",
        "fdCount": 3,
        "udCount": 5
      }
    }

U ziet dat de variabele **tshirtSize** kan het t-shirt grootte die u via een parameter (**Small**, **Medium**, **Large verstrekt**) toevoegen aan de tekst **tshirtSize**. Met deze variabele kunt u de bijbehorende complexe objectvariabele dat formaat t-shirt ophalen.

U kunt vervolgens verwijzen naar deze variabelen verderop in de sjabloon. De mogelijkheid om te verwijzen naar de naam van de variabelen en de bijbehorende eigenschappen vereenvoudigt de sjabloonsyntaxis van de en gemakkelijk te begrijpen van de context. In het volgende voorbeeld wordt een resource kunt implementeren met behulp van de eerder weergegeven waarden instellen voor objecten. Bijvoorbeeld de VM-grootte wordt ingesteld door het ophalen van de waarde voor `variables('tshirtSize').vmSize` bij de waarde voor de grootte van de schijf wordt opgehaald uit het `variables('tshirtSize').diskSize`. Bovendien de URI voor een gekoppelde sjabloon is ingesteld met de waarde voor `variables('tshirtSize').vmTemplate`.

    "name": "master-node",
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2015-01-01",
    "dependsOn": [
        "[concat('Microsoft.Resources/deployments/', 'shared')]"
    ],
    "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[variables('tshirtSize').vmTemplate]",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "value": "[parameters('adminPassword')]"
          },
          "replicatorPassword": {
            "value": "[parameters('replicatorPassword')]"
          },
          "osSettings": {
            "value": "[variables('osSettings')]"
          },
          "subnet": {
            "value": "[variables('networkSettings').subnets.data]"
          },
          "commonSettings": {
            "value": {
              "region": "[parameters('region')]",
              "adminUsername": "[parameters('adminUsername')]",
              "namespace": "ms"
            }
          },
          "storageSettings": {
            "value":"[variables('tshirtSize').storage]"
          },
          "machineSettings": {
            "value": {
              "vmSize": "[variables('tshirtSize').vmSize]",
              "diskSize": "[variables('tshirtSize').diskSize]",
              "vmCount": 1,
              "availabilitySet": "[variables('availabilitySetSettings').name]"
            }
          },
          "masterIpAddress": {
            "value": "0"
          },
          "dbType": {
            "value": "MASTER"
          }
        }
      }
    }

## <a name="pass-state-to-a-template"></a>Status doorgeven aan een sjabloon

U delen staat in een sjabloon voor parameters die u tijdens de implementatie opgeeft.

De volgende tabel vindt u veelgebruikte parameters in sjablonen.

Naam | Waarde | Beschrijving
---- | ----- | -----------
locatie    | De tekenreeks van een beperkte lijst met Azure regio 's   | De locatie waar de resources worden ingezet.
storageAccountNamePrefix    | Tekenreeks    | Unieke DNS-naam voor de opslag Account waar u de schijven van de VM worden geplaatst
Domeinnaam  | Tekenreeks    | Domeinnaam van de publiek toegankelijke jumpbox VM in de indeling: {domainName} **. { locatie}.cloudapp.com** bijvoorbeeld: **mydomainname.westus.cloudapp.azure.com**
adminUsername   | Tekenreeks    | De gebruikersnaam voor de VMs
adminPassword   | Tekenreeks    | Wachtwoord voor de VMs
tshirtSize  | Tekenreeks van een beperkte lijst met aangeboden maten t-shirt   | De grootte van de eenheid benoemde schaal te verrichten. Bijvoorbeeld "Klein", "Medium", "Groot"
virtualNetworkName  | Tekenreeks    | De naam van het virtuele netwerk waarmee de consument wil gebruiken.
enableJumpbox   | De tekenreeks van een beperkte lijst (ingeschakeld/uitgeschakeld) | Parameter die aangeeft of een jumpbox voor het milieu. Waarden: 'ingeschakeld', 'uitgeschakeld'

De **tshirtSize** -parameter die wordt gebruikt in het vorige gedeelte is als volgt gedefinieerd:

    "parameters": {
      "tshirtSize": {
        "type": "string",
        "defaultValue": "Small",
        "allowedValues": [
          "Small",
          "Medium",
          "Large"
        ],
        "metadata": {
          "Description": "T-shirt size of the MongoDB deployment"
        }
      }
    }


## <a name="pass-state-to-linked-templates"></a>Status doorgeven aan gekoppelde sjablonen

Wanneer u verbinding maakt met de gekoppelde sjablonen, vaak gebruik van een combinatie van statische en variabelen gegenereerd.

### <a name="static-variables"></a>Statische variabelen

Statische variabelen worden vaak gebruikt om basis waarden, zoals de URL's die worden gebruikt in een sjabloon.

In het volgende fragment van de sjabloon `templateBaseUrl` geeft de locatie van de hoofdmap voor de sjabloon in GitHub. De volgende regel maakt een nieuwe variabele `sharedTemplateUrl` die de basis-URL met de bekende naam van de sjabloon voor gedeelde bronnen worden aaneengeschakeld. Onder deze regel, een complexe objectvariabele wordt gebruikt voor het opslaan van de grootte van een t-shirt, waarbij de basis-URL naar de locatie van de sjabloon bekende configuratie samengevoegd en opgeslagen in de `vmTemplate` eigenschap.

Het voordeel van deze aanpak is dat als de sjabloonlocatie wijzigt, u alleen hoeft de statische variabele op één plaats die wordt doorgegeven in de gekoppelde sjablonen te wijzigen.

    "variables": {
      "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
      "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "slaveCount": 1,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      }
    }

### <a name="generated-variables"></a>Gegenereerde variabelen

Naast statische variabelen worden meerdere variabelen dynamisch gegenereerd. In dit gedeelte vindt u enkele van de gebruikelijke typen variabelen gegenereerde.

#### <a name="tshirtsize"></a>tshirtSize

U bent bekend met deze variabele gegenereerd uit de bovenstaande voorbeelden.

#### <a name="networksettings"></a>networkSettings

In capaciteit, mogelijkheden of sjabloon bereik end-to-end-oplossing maken de gekoppelde sjablonen meestal resources die aanwezig zijn op een netwerk. Een duidelijke aanpak is een complexe object gebruiken om netwerkinstellingen op te slaan en door te geven aan gekoppelde sjablonen.

Hieronder ziet u een voorbeeld van de instellingen voor netwerk communiceren.

    "networkSettings": {
      "vnetName": "[parameters('virtualNetworkName')]",
      "addressPrefix": "10.0.0.0/16",
      "subnets": {
        "dmz": {
          "name": "dmz",
          "prefix": "10.0.0.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        },
        "data": {
          "name": "data",
          "prefix": "10.0.1.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        }
      }
    }

#### <a name="availabilitysettings"></a>availabilitySettings

Bronnen die zijn gemaakt in de gekoppelde sjablonen worden vaak geplaatst in een groep van beschikbaarheid. In het volgende voorbeeld wordt de naam van de beschikbaarheid is opgegeven en ook het domein voor fouttolerantie en update domein tellen als u wilt gebruiken.

    "availabilitySetSettings": {
      "name": "pgsqlAvailabilitySet",
      "fdCount": 3,
      "udCount": 5
    }

Als u meerdere sets beschikbaarheid (bijvoorbeeld één master knooppunten) en de andere voor gegevensknooppunten, kunt u een naam gebruikt als voorvoegsel, meerdere sets met beschikbaarheid opgeeft, of voert u het eerdere model voor het maken van een variabele voor een specifieke grootte voor t-shirt.

#### <a name="storagesettings"></a>storageSettings

Storage details vaak gedeeld met gekoppelde sjablonen. In het volgende voorbeeld bevat een *storageSettings* -object details over de opslag en de container namen.

    "storageSettings": {
        "vhdStorageAccountName": "[parameters('storageAccountName')]",
        "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
        "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
    }

#### <a name="ossettings"></a>osSettings

Met gekoppelde sjablonen moet u instellingen van besturingssystemen geven aan verschillende typen van knooppunten in verschillende typen voor verschillende bekende configuratie. Een complexe object is een eenvoudige manier voor het opslaan en delen van informatie over het besturingssysteem en ook kunt u gemakkelijker meerdere besturingssystemen ondersteuning voor distributie.

In het volgende voorbeeld ziet u een object voor *osSettings*:

    "osSettings": {
      "imageReference": {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "14.04.2-LTS",
        "version": "latest"
      }
    }

#### <a name="machinesettings"></a>machineSettings

Een gegenereerde variabele *machineSettings* is een complex met een combinatie van variabelen voor het maken van een VM core-object. De variabelen zijn gebruikersnaam en wachtwoord, wordt een voorvoegsel voor de VM namen en de verwijzing naar een systeem afbeelding.

    "machineSettings": {
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "machineNamePrefix": "mongodb-",
        "osImageReference": {
            "publisher": "[variables('osFamilySpec').imagePublisher]",
            "offer": "[variables('osFamilySpec').imageOffer]",
            "sku": "[variables('osFamilySpec').imageSKU]",
            "version": "latest"
        }
    },

Houd er rekening mee dat *osImageReference* worden de waarden opgehaald van de variabele *osSettings* is gedefinieerd in de belangrijkste sjabloon. Dit betekent dat u het besturingssysteem eenvoudig kunt wijzigen voor een VM — geheel of op basis van de voorkeur van de consument van een sjabloon.

#### <a name="vmscripts"></a>vmScripts

Het object *vmScripts* bevat details over het downloaden en uitvoeren van een exemplaar van VM, met inbegrip van buiten en binnen de verwijzingen van de scripts. Verwijzingen zijn buiten de infrastructuur. In verwijzingen zijn de geïnstalleerde software is geïnstalleerd en de configuratie.

Met de eigenschap *scriptsToDownload* kunt u de scripts voor de VM downloaden lijst. Dit object bevat ook verwijzingen naar opdrachtregelargumenten voor verschillende soorten acties. Deze acties omvatten de standaardinstallatie voor elke afzonderlijke knooppunten een installatie die wordt uitgevoerd nadat alle knooppunten worden geïmplementeerd en extra scripts die specifiek zijn voor een bepaalde sjabloon wordt uitgevoerd.

In dit voorbeeld is afkomstig van een sjabloon voor het implementeren van MongoDB waarvoor een instellingen voor het leveren van maximale beschikbaarheid. De *arbiterNodeInstallCommand* is toegevoegd aan de *vmScripts* te installeren van de instellingen.

De sectie variabelen is waar het vinden van de variabelen die de specifieke tekst voor het uitvoeren van het script met de juiste waarden te definiëren.

    "vmScripts": {
        "scriptsToDownload": [
            "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
            "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
        ],
        "regularNodeInstallCommand": "[variables('installCommand')]",
        "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
        "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
    },


## <a name="return-state-from-a-template"></a>Status van een sjabloon retourneren

Niet alleen kunt u gegevens doorgeven in een sjabloon, kunt u ook gegevens terug naar de aanroepende sjabloon delen. In het gedeelte **uitvoer** van een gekoppelde sjabloon, kunt u de sleutel/waarde-paren die kunnen worden gebruikt door de sjabloon van het bieden.

In het volgende voorbeeld ziet u hoe het particuliere IP-adres dat is gegenereerd in een gekoppelde sjabloon doorgeven.

    "outputs": {
        "masterip": {
            "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
            "type": "string"
         }
    }

Binnen de voornaamste sjabloon, kunt u deze gegevens met de volgende syntaxis:

    "[reference('master-node').outputs.masterip.value]"

U kunt deze expressie in het gedeelte uitvoer of de sectie bronnen van de belangrijkste sjabloon gebruiken. U kunt de expressie in de sectie variabelen niet gebruiken omdat deze afhankelijk van de runtime-status is. U kunt deze waarde uit de belangrijkste sjabloon gebruiken:

    "outputs": { 
      "masterIpAddress": {
        "value": "[reference('master-node').outputs.masterip.value]",
        "type": "string"
      }
     
Zie voor een voorbeeld van het gebruik van de sectie uitvoer van een gekoppelde sjabloon te retourneren gegevensschijven voor een virtuele machine, [meerdere gegevensschijven voor een virtuele Machine maken](resource-group-create-multiple.md#creating-multiple-data-disks-for-a-virtual-machine).

## <a name="define-authentication-settings-for-virtual-machine"></a>Verificatie-instellingen voor virtuele machine definiëren

Hetzelfde patroon eerder weergegeven voor configuratie-instellingen kunt u de verificatie-instellingen voor een virtuele machine te geven. U maakt een parameter voor het doorgeven in het type verificatie.

    "parameters": {
      "authenticationType": {
        "allowedValues": [
          "password",
          "sshPublicKey"
        ],
        "defaultValue": "password",
        "metadata": {
          "description": "Authentication type"
        },
        "type": "string"
      }
    }

U variabelen voor de verschillende verificatietypen toevoegen en een variabele op te slaan welk type wordt gebruikt voor deze installatie op basis van de waarde van de parameter.

    "variables": {
      "osProfile": "[variables(concat('osProfile', parameters('authenticationType')))]",
      "osProfilepassword": {
        "adminPassword": "[parameters('adminPassword')]",
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]"
      },
      "osProfilesshPublicKey": {
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]",
        "linuxConfiguration": {
          "disablePasswordAuthentication": "true",
          "ssh": {
            "publicKeys": [
              {
                "keyData": "[parameters('sshPublicKey')]",
                "path": "/home/notused/.ssh/authorized_keys"
              }
            ]
          }
        }
      }
    }

Bij het definiëren van de virtuele machine, stelt u de **osProfile** op de variabele die u hebt gemaakt.

    {
      "type": "Microsoft.Compute/virtualMachines",
      ...
      "osProfile": "[variables('osProfile')]"
    }


## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over de gedeelten van de sjabloon [Ontwerpen Azure Resource Manager-sjablonen](resource-group-authoring-templates.md)
- De functies die beschikbaar binnen een sjabloon zijn, Zie [Azure Resource Manager sjabloon functies](resource-group-template-functions.md)

