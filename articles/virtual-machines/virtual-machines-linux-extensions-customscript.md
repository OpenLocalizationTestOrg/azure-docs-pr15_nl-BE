<properties
   pageTitle="Aangepaste scripts op Linux VMs | Microsoft Azure"
   description="Linux VM configuratietaken automatiseren met behulp van het aangepaste Script-uitbreiding"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="nepeters"/>

# <a name="using-the-azure-custom-script-extension-with-linux-virtual-machines"></a>Gebruik de extensie Azure aangepast Script met Linux virtuele Machines

Het aangepaste Script extensie gedownload en scripts uitvoeren op Azure virtuele machines. Deze uitbreiding is handig voor implementatieconfiguratie boeken, software-installatie of een andere configuratie / beheertaak. Scripts kunnen worden gedownload van Azure opslag of een andere toegankelijke internetlocatie of geleverd aan de runtime-extensie. De extensie aangepast Script geïntegreerd met Azure Resource Manager-sjablonen, en kan ook worden uitgevoerd met behulp van de CLI Azure, PowerShell, Azure portal of de Azure Virtual Machine REST API.

Dit document een gedetailleerd overzicht van het gebruik van het aangepaste Script extensie van een sjabloon Azure Resource Manager en de CLI Azure en ook een gedetailleerd overzicht van de stappen voor probleemoplossing op Linux-systemen.

## <a name="extension-configuration"></a>Uitbreiding configuratie

De configuratie aangepast Script extensie geeft de locatie van script en de opdracht moet worden uitgevoerd. Deze configuratie kan worden opgeslagen in de configuratiebestanden opgegeven op de opdrachtregel of in een sjabloon Azure Resource Manager. Vertrouwelijke gegevens kunnen worden opgeslagen in een beveiligde configuratie die is gecodeerd en alleen binnen de virtuele machine worden ontsleuteld. De beveiligde configuratie is handig als u de opdracht tot uitvoering van geheimen wachtwoord bevat.

### <a name="public-configuration"></a>Openbare configuratie

Schema:

- **commandToExecute**: (vereist, string) de vermelding punt script uitvoeren
- **fileUris**: (optioneel, string-matrix) de URL's voor bestanden worden gedownload.
- **tijdstempel** (optioneel, integer) in dit veld alleen gebruiken voor het activeren van een opnieuw uitvoeren van het script door de waarde van dit veld te wijzigen.

```none
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a>Beveiligde configuratie

Schema:

- **commandToExecute**: (optioneel, string) de vermelding punt script uit te voeren. Dit veld wordt in plaats daarvan gebruiken als uw opdracht geheimen zoals wachtwoorden bevat.
- **storageAccountName**: (optioneel, string) de naam van de account van de opslag. Als u opslag referenties opgeeft, moeten alle fileUris URL's voor Azure BLOB's.
- **storageAccountKey**: (optioneel, string) de toegangstoets van opslag-account.


```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a>Azure CLI

Als u de extensie aangepast Script uitvoeren met de CLI Azure, maakt u een configuratiebestand of de bestanden met ten minste de bestands-uri en de scriptopdracht worden uitgevoerd.

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /scirpt-config.json
```

U kunt ook de opdracht kan worden uitgevoerd met de `--public-config` en `--private-config` optie, kunt de configuratie worden opgegeven tijdens het uitvoeren van en zonder een apart configuratiebestand.

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config '{"fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],"commandToExecute": "./hello.sh"}'
```

### <a name="azure-cli-examples"></a>Azure CLI-voorbeelden

**Voorbeeld 1** - openbare configuratie met het scriptbestand.

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],
  "commandToExecute": "./hello.sh"
}
```

Azure CLI-opdracht:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /public.json
```

**Voorbeeld 2** - openbare configuratie zonder een script bestand.

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Azure CLI-opdracht:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /public.json
```

**Voorbeeld 3** - een openbare configuratiebestand wordt gebruikt om op te geven van het scriptbestand URI en een beveiligd bestand wordt gebruikt voor de opdracht moet worden uitgevoerd.

Openbare configuratiebestand:

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],
}
```

Beveiligde configuratiebestand:  

```json
{
  "commandToExecute": "./hello.sh <password>"
}
```

Azure CLI-opdracht:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path ./public.json --private-config-path ./protected.json
```

## <a name="resource-manager-template"></a>Sjabloon voor bronbeheer

De extensie Azure aangepast Script kan worden uitgevoerd bij de virtuele Machine implementatie met behulp van de sjabloon voor een Resource Manager. Hiertoe toevoegen correct opgemaakte JSON aan de sjabloon voor de implementatie.

### <a name="resource-manager-examples"></a>Voorbeelden van bronbeheer

**Voorbeeld 1** - openbare configuratie.

```json
{
    "name": "scriptextensiondemo",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-15",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('scriptextensiondemoName'))]"
    ],
    "tags": {
        "displayName": "scriptextensiondemo"
    },
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
      "settings": {
        "fileUris": [
          "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
        ],
        "commandToExecute": "sh hello.sh"
      }
    }
}
```

**Voorbeeld 2** - opdracht kan worden uitgevoerd in een beveiligde configuratie.

```json
{
  "name": "config-app",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh
      ]              
    },
    "protectedSettings": {
      "commandToExecute": "sh hello.sh <password>"
    }
  }
}
```

Zie .net Core muziek winkel Demo voor een volledig voorbeeld - [Muziek winkel Demo](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).

## <a name="troubleshooting"></a>Het oplossen van problemen

Als de extensie aangepast Script wordt uitgevoerd, kan het script wordt gemaakt of gedownload in een map, zoals in het volgende voorbeeld. Uitvoer van de opdracht wordt ook opgeslagen in deze map in `stdout` en `stderr` bestand.

```none
/var/lib/azure/custom-script/download/0/
```

De extensie Azure Script maakt een logboek, kunt u hier vinden.

```none
/var/log/azure/customscript/handler.log
```

De uitvoeringsstatus van het aangepaste Script extensie kan ook worden opgehaald met de CLI Azure.

```none
azure vm extension get <resource-group> <vm-name>
```

De uitvoer ziet er de volgende tekst:

```none
info:    Executing command vm extension get
+ Looking up the VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a>Volgende stappen

Voor informatie over andere scriptextensies VM, overzicht [Azure Script uitbreiding voor Linux](./virtual-machines-linux-extensions-features.md).