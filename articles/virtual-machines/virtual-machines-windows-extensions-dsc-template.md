<properties
   pageTitle="De gewenste configuratie staat Resource Manager sjabloon | Microsoft Azure"
   description="Resource Manager Gegevenssjabloon definiëren voor de gewenste status configuratie in Azure met voorbeelden en het oplossen van problemen"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="windows-vmss-and-desired-state-configuration-with-azure-resource-manager-templates"></a>VMSS van Windows en de gewenste staat configuratie met Azure Resource Manager-sjablonen
Dit artikel wordt de sjabloon Resource Manager voor de [staat configuratie de gewenste extensie handler](virtual-machines-windows-extensions-dsc-overview.md). 

## <a name="template-example-for-a-windows-vm"></a>Voorbeeld van de sjabloon voor een Windows VM

In het volgende fragment wordt in de sectie bron van de sjabloon.

```json
            "name": "Microsoft.Powershell.DSC",
            "type": "extensions",
             "location": "[resourceGroup().location]",
             "apiVersion": "2015-06-15",
             "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "dscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }

```

## <a name="template-example-for-windows-vmss"></a>Voorbeeld van de sjabloon voor Windows VMSS

Een knooppunt VMSS heeft een sectie 'Eigenschappen' met de 'VirtualMachineProfile', 'extensionProfile'-kenmerk. DSC toegevoegd onder 'extensions'. 

```json
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": true,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="detailed-settings-information"></a>Gedetailleerde informatie over

Het volgende schema is voor het gedeelte van de instellingen in een sjabloon Azure Resource Manager Azure DSC-extensie.

```json

"settings": {
  "wmfVersion": "latest",
  "configuration": {
    "url": "http://validURLToConfigLocation",
    "script": "ConfigurationScript.ps1",
    "function": "ConfigurationFunction"
  },
  "configurationArguments": {
    "argument1": "Value1",
    "argument2": "Value2"
  },
  "configurationData": {
    "url": "https://foo.psd1"
  },
  "privacy": {
    "dataCollection": "enable"
  },
  "advancedOptions": {
    "downloadMappings": {
      "customWmfLocation": "http://myWMFlocation"
    }
  } 
},
"protectedSettings": {
  "configurationArguments": {
    "parameterOfTypePSCredential1": {
      "userName": "UsernameValue1",
      "password": "PasswordValue1"
    },
    "parameterOfTypePSCredential2": {
      "userName": "UsernameValue2",
      "password": "PasswordValue2"
    }
  },
  "configurationUrlSasToken": "?g!bber1sht0k3n",
  "configurationDataUrlSasToken": "?dataAcC355T0k3N"
}

```

## <a name="details"></a>Details
| Naam van eigenschap | Type | Beschrijving |
| --- | --- | --- |
| settings.wmfVersion | tekenreeks | Hiermee geeft u de versie van het Windows Management Framework die moeten worden geïnstalleerd op uw VM. Deze eigenschap instelt op 'nieuwste' installaties door de meest recente versie van WMF. Alleen huidige mogelijke waarden voor deze eigenschap zijn **'4.0', '5.0', ' 5.0PP' en 'laatste'**. Deze mogelijke waarden zijn updates. De standaardwaarde is 'nieuwste'.|
| Settings.Configuration.URL | tekenreeks | Hiermee geeft u de URL-locatie van waaruit de DSC-configuratie zip-bestand te downloaden. Als een token SAS de URL die u om toegang te krijgen vereist, moet u de eigenschap protectedSettings.configurationUrlSasToken ingesteld op de waarde van uw token SAS. Deze eigenschap is alleen vereist als settings.configuration.script en/of settings.configuration.function zijn gedefinieerd. |
| Settings.Configuration.script | tekenreeks | Hiermee geeft u de bestandsnaam van het script met de definitie van de DSC-configuratie. Dit script moet zich in de hoofdmap van het zip-bestand gedownload vanaf de URL die is opgegeven door de eigenschap configuration.url. Deze eigenschap is alleen vereist als settings.configuration.url en/of settings.configuration.script zijn gedefinieerd. |
| Settings.Configuration.Function | tekenreeks | Hiermee geeft u de naam van de DSC-configuratie. De configuratie met de naam moet worden opgenomen in het script dat wordt gedefinieerd door configuration.script. Deze eigenschap is alleen vereist als settings.configuration.url en/of settings.configuration.function zijn gedefinieerd. |
| settings.configurationArguments | Collectie | Hiermee definieert u de parameters die u wilt doorgeven aan de configuratie van DSC. Deze eigenschap is niet gecodeerd. |
| settings.configurationData.url | tekenreeks | Geeft de URL voor het downloaden van het bestand met de gegevens (.pds1) te gebruiken als invoer voor de DSC-configuratie. Als een token SAS de URL die u om toegang te krijgen vereist, moet u de eigenschap protectedSettings.configurationDataUrlSasToken ingesteld op de waarde van uw token SAS.|
| settings.privacy.dataEnabled | tekenreeks | Hiermee schakelt u of telemetrie-collectie. De enige mogelijke waarden voor deze eigenschap zijn **'Inschakelen', 'Uitschakelen', '', of $null**. Laat deze eigenschap leeg of null kunt telemetrie. De standaardwaarde is ''. [Meer informatie](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/) |
| settings.advancedOptions.downloadMappings | Collectie | Hiermee definieert u alternatieve locaties van waaruit de WMF downloaden. [Meer informatie](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx) |
| protectedSettings.configurationArguments | Collectie | Hiermee definieert u de parameters die u wilt doorgeven aan de configuratie van DSC. Deze eigenschap is gecodeerd. |
| protectedSettings.configurationUrlSasToken | tekenreeks | Hiermee geeft u het SAS-token voor toegang tot de URL die wordt gedefinieerd door configuration.url. Deze eigenschap is gecodeerd. |
| protectedSettings.configurationDataUrlSasToken | tekenreeks | Hiermee geeft u het SAS-token voor toegang tot de URL die wordt gedefinieerd door configurationData.url. Deze eigenschap is gecodeerd. |

## <a name="settings-vs-protectedsettings"></a>Instellingen versus ProtectedSettings
Alle instellingen worden opgeslagen in een tekstbestand van de instellingen op de VM.
Onder 'instellingen' zijn openbare eigenschappen omdat ze niet zijn gecodeerd in het tekstbestand instellingen.
Eigenschappen van de 'protectedSettings' zijn gecodeerd met een certificaat en worden niet weergegeven in de indeling tekst zonder opmaak in dit bestand op de VM.

Als de configuratie moet referenties, kunnen ze worden opgenomen in de protectedSettings:

```json
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
            "userName": "UsernameValue1",
            "password": "PasswordValue1"
        }
    }
}
```

## <a name="example"></a>Voorbeeld

In het volgende voorbeeld wordt afgeleid uit de sectie 'Introductie' van de [overzichtspagina van DSC extensie Handler](virtual-machines-windows-extensions-dsc-overview.md).
In dit voorbeeld wordt de Resource Manager sjablonen in plaats van cmdlets voor de implementatie van de uitbreiding. De 'IisInstall.ps1' configuratie opslaan, plaatst u deze in een. ZIP-bestand en upload het bestand in een toegankelijke URL. In dit voorbeeld wordt de Azure blob-opslag, maar het is mogelijk om te downloaden. ZIP-bestanden vanaf elke willekeurige locatie.

De volgende code wordt in de sjabloon Azure Resource Manager het juiste bestand downloaden en uitvoeren van de juiste functie van PowerShell VM:

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
    } 
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## <a name="updating-from-the-previous-format"></a>Bijwerken van de vorige indeling
Alle instellingen in de vorige indeling (met de openbare eigenschappen ModulesUrl, ConfigurationFunction, SasToken of eigenschappen) automatisch aan te passen aan de huidige indeling en voer net als vroeger.

Het volgende schema is wat de vorige instellingenschema staande:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points to private Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties":  {
        "ParameterToConfigurationFunction1":  "Value1",
        "ParameterToConfigurationFunction2":  "Value2",
        "ParameterOfTypePSCredential1": {
            "UserName": "UsernameValue1",
            "Password": "PrivateSettingsRef:Key1" 
        },
        "ParameterOfTypePSCredential2": {
            "UserName": "UsernameValue2",
            "Password": "PrivateSettingsRef:Key2"
        }
    }
},
"protectedSettings": { 
    "Items": {
        "Key1": "PasswordValue1",
        "Key2": "PasswordValue2"
    },
    "DataBlobUri": "https://UrlToConfigurationDataWithOptionalSasToken.psd1"
}
```

Hier ziet u hoe de vorige indeling wordt aangepast aan de huidige indeling:

| Naam van eigenschap | Vorige Schema Equivalent |
| --- | --- |
| settings.wmfVersion | Instellingen. WMFVersion |
| Settings.Configuration.URL | Instellingen. ModulesUrl |
| Settings.Configuration.script | Eerste deel van de instellingen. ConfigurationFunction (voor "\\\\") |
| Settings.Configuration.Function | Tweede deel van de instellingen. ConfigurationFunction (na '\\\\") |
| settings.configurationArguments | Instellingen. Eigenschappen |
| settings.configurationData.url | protectedSettings.DataBlobUri (zonder SAS token) |
| settings.privacy.dataEnabled | Instellingen. Privacy.DataEnabled |
| settings.advancedOptions.downloadMappings | Instellingen. AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments | protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken | Instellingen. SasToken |
| protectedSettings.configurationDataUrlSasToken | Token SAS van protectedSettings.DataBlobUri |


## <a name="troubleshooting---error-code-1100"></a>Het oplossen van problemen - foutcode 1100
Foutcode 1100 geeft aan dat er een probleem met de invoer van de gebruiker naar de DSC-extensie.
De tekst van deze fouten variabel is en kan worden gewijzigd.
Hier zijn enkele van de fouten die u kunt uitvoeren in, en hoe u deze kunt oplossen.

### <a name="invalid-values"></a>Ongeldige waarden
"Privacy.dataCollection is {0}. De enige mogelijke waarden zijn ", 'Inschakelen' en 'Uitschakelen'" "is WmfVersion {0}. Alleen de mogelijke waarden zijn... en de 'nieuwste' "

Probleem: Een gegeven waarde is niet toegestaan.

Oplossing: Wijzig de ongeldige waarde in een geldige waarde. Zie de tabel in de sectie Details.

### <a name="invalid-url"></a>Ongeldige URL
"ConfigurationData.url is {0}. Dit is geen geldige URL""DataBlobUri is {0}. Dit is geen geldige URL""Configuration.url is {0}. Dit is geen geldige URL"

Probleem: Een opgegeven dat URL is ongeldig.

Oplossing: Controleer de opgegeven URL's. Zorg ervoor dat alle URL's omzetten naar geldige locaties die de uitbreiding op de externe computer kunt openen.

### <a name="invalid-configurationargument-type"></a>Ongeldige ConfigurationArgument-Type
"Type ongeldige configurationArguments {0}"

Probleem: De eigenschap ConfigurationArguments kan omzetten in een Hashtable-object. 

Oplossing: Controleer de eigenschap ConfigurationArguments hash-tabel. Volgens de indeling die in het voorgaande voorbeeld. Kijk uit voor offertes, komma's en haakjes.

### <a name="duplicate-configurationarguments"></a>Dubbele ConfigurationArguments
"Dubbele argumenten '{0}' gevonden in openbare en beveiligde configurationArguments"

Probleem: De ConfigurationArguments in de openbare instellingen en de ConfigurationArguments in de beschermde instellingen bevatten eigenschappen met dezelfde naam.

Oplossing: Een van de dubbele eigenschappen verwijderen.

### <a name="missing-properties"></a>Ontbrekende eigenschappen
"Configuration.function vereist configuration.url of configuration.module is opgegeven."

"Configuration.url vereist dat configuration.script is opgegeven."

"Configuration.script vereist dat configuration.url is opgegeven."

"Configuration.url vereist dat configuration.function is opgegeven."

"ConfigurationUrlSasToken vereist dat configuration.url is opgegeven."

"ConfigurationDataUrlSasToken vereist dat configurationData.url is opgegeven."

Probleem: Een gedefinieerde eigenschap moet een andere eigenschap die ontbreekt.

Oplossingen: 
- Geef de eigenschap ontbreekt.
- De eigenschap die moet worden de ontbrekende eigenschap verwijderen.


## <a name="next-steps"></a>Volgende stappen
Meer informatie over het DSC en de schaal van de virtuele machine worden in [Virtual Machine schaal wordt ingesteld met de extensie Azure DSC](../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md)

Meer details op [de DSC-beveiligde Referentiebeheer](virtual-machines-windows-extensions-dsc-credentials.md)vinden. 

Zie voor meer informatie over de handler Azure DSC-extensie, [Inleiding tot de configuratie van Azure staat de gewenste extensie handler](virtual-machines-windows-extensions-dsc-overview.md). 

Voor meer informatie over PowerShell DSC, [gaat u naar het midden van de documentatie van PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 
