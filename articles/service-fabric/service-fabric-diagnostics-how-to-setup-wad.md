<properties
   pageTitle="Verzamelen van logboeken met Azure Diagnostics | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe Azure diagnostische gegevens instellen voor het verzamelen van Logboeken van een Service Fabric '-cluster uitgevoerd in Azure."
   services="service-fabric"
   documentationCenter=".net"
   authors="ms-toddabel"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/28/2016"
   ms.author="toddabel"/>


# <a name="collect-logs-by-using-azure-diagnostics"></a>Verzamelen van logboeken met Azure diagnostische gegevens

> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
- [Linux](service-fabric-diagnostics-how-to-setup-lad.md)

Wanneer u een Azure Service Fabric '-cluster uitvoert, is het een goed idee om de logboeken van de knooppunten op een centrale locatie verzamelen. De logboeken die op een centrale locatie kunt u analyseren en oplossen van problemen in het cluster of problemen in de toepassingen en services die in het cluster worden uitgevoerd.

Een manier om te uploaden en verzamelen van Logboeken is de uitbreiding Azure diagnostische gegevens, die Logboeken worden geüpload naar Azure opslag gebruiken. De logboeken zijn niet dat nuttig is in de opslag. Maar u kunt een extern proces lezen de gebeurtenissen uit de opslag en plaats deze in een product zoals [Analytics logboek](../log-analytics/log-analytics-service-fabric.md), [Elastische zoeken](service-fabric-diagnostic-how-to-use-elasticsearch.md)of een andere oplossing voor het parseren van het logboek.

## <a name="prerequisites"></a>Vereisten
Deze hulpprogramma's kunt u bepaalde bewerkingen uitvoeren in dit document:

* [Azure diagnostische gegevens](../cloud-services/cloud-services-dotnet-diagnostics.md) (gerelateerd aan Azure Cloud Services, maar heeft goede informatie en voorbeelden)
* [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](../powershell-install-configure.md)
* [Azure Resource Manager-client](https://github.com/projectkudu/ARMClient)
* [Sjabloon voor Azure Resource Manager](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)


## <a name="log-sources-that-you-might-want-to-collect"></a>Logboek-bronnen die u wilt verzamelen
- **Service Fabric logs**: uitlaatgassen van het platform op standaard gebeurtenis Tracing voor Windows (ETW) en gebeurtenisbron kanalen. Logboeken zijn van verschillende typen:
  - Operationele gebeurtenissen: logboeken voor bewerkingen die de Service Fabric-platform worden uitgevoerd. Voorbeelden hiervan zijn het maken van toepassingen en services, de status van knooppunt verandert en upgrade-informatie.
  - [Betrouwbare actoren model gebeurtenissen programmeren](service-fabric-reliable-actors-diagnostics.md)
  - [Betrouwbare diensten model gebeurtenissen programmeren](service-fabric-reliable-services-diagnostics.md)
- **Application-gebeurtenissen**: gebeurtenissen uitlaatgassen van de code van de service en met behulp van de gebeurtenisbron helperklasse geleverd in de sjablonen van Visual Studio geschreven. Zie voor meer informatie over het schrijven van logboekbestanden van uw toepassing [Monitor en services in de instellingen voor een lokale computer ontwikkeling onderzoeken](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).


## <a name="deploy-the-diagnostics-extension"></a>De uitbreiding van de diagnostische gegevens implementeren
De eerste stap bij het verzamelen van Logboeken wordt de extensie diagnostische gegevens op elk van de VMs in de cluster-Service Fabric geïmplementeerd. De uitbreiding van de diagnostische logboeken op elke VM verzamelt en ze uploaden naar de opslag-account die u opgeeft. De stappen zijn een beetje op basis van de vraag of u de Azure portal of Azure Resource Manager gebruiken. De stappen zijn ook afhankelijk van of de implementatie deel uitmaakt van het maken van het cluster of voor een cluster dat al bestaat. Bekijk de stappen voor elk scenario.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-the-portal"></a>De uitbreiding van de diagnostische gegevens als onderdeel van het maken van het cluster via de portal implementeren
De uitbreiding van de diagnostische gegevens op de VMs in het cluster installeren als onderdeel van het maken van het cluster, moet u de diagnostische instellingen Configuratiescherm weergegeven in de volgende afbeelding gebruiken. Controleer of diagnostische gegevens is ingesteld **op** (de standaardinstelling) zodat betrouwbare actoren of betrouwbare diensten gebeurtenis collectie. Nadat u het cluster hebt gemaakt, kunt u deze instellingen niet wijzigen met behulp van de portal.

![Azure diagnostische instellingen in de portal voor het maken van het cluster](./media/service-fabric-diagnostics-how-to-setup-wad/portal-cluster-creation-diagnostics-setting.png)

De ondersteuning van Azure team *vereist* ondersteuning vastgelegd voor het oplossen van eventuele ondersteuningsverzoeken die u maakt. Deze logboeken worden verzameld in real-time en worden opgeslagen in een van de accounts van de opslag in de resourcegroep hebt gemaakt. De diagnostische instellingen configureren voor gebeurtenissen op toepassingsniveau. Deze gebeurtenissen zijn gebeurtenissen [Betrouwbaar actoren](service-fabric-reliable-actors-diagnostics.md) en [Betrouwbare Services](service-fabric-reliable-services-diagnostics.md) -gebeurtenissen enkele gebeurtenissen op systeemniveau Service Fabric in Azure opslag moeten worden opgeslagen.

Producten zoals [Elastische zoeken](service-fabric-diagnostic-how-to-use-elasticsearch.md) of uw eigen proces krijgt de gebeurtenissen van de opslag-account. Er is geen manier om te filteren of de gebeurtenissen die worden verzonden naar de tabel te verzorgen. Als u een proces om gebeurtenissen te verwijderen uit de tabel niet implementeert, wordt de tabel blijven groeien.

Als u een cluster met behulp van de portal, wordt ten zeerste aanbevolen dat u de sjabloon *voordat u op * *OK*downloaden ** voor het maken van het cluster. Raadpleeg voor het [instellen van een Service Fabric '-cluster met behulp van een sjabloon Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)voor meer informatie. U moet de sjabloon te wijzigen, omdat u geen enkele wijzigingen aanbrengen met behulp van de portal.

Met de volgende stappen kunt u sjablonen exporteren vanuit de portal. Deze sjablonen is moeilijker te gebruiken, omdat ze mogelijk null-waarden die de vereiste informatie ontbreekt.

1. Open de resourcegroep.
2. Selecteer **Instellingen** om het instellingenvenster weer te geven.
3. Selecteer **implementaties** om het implementatie-deelvenster Historie weer te geven.
4. Selecteer een installatie om de details van de implementatie van weer te geven.
5. Selecteer **Sjabloon exporteren** om het deelvenster sjabloon weer te geven.
6. Selecteer **bestand opslaan in** een ZIP-bestand met de sjabloon, parameter en PowerShell-bestanden exporteren.

Nadat u de bestanden exporteert, moet u een wijziging aanbrengt. Bewerk het bestand parameters.json en het **adminPassword** element verwijderen. Hierdoor wordt een wachtwoord wordt gevraagd wanneer u een script voor de implementatie uitvoert. Wanneer u een script voor de implementatie uitvoert, is het mogelijk om op te lossen null-parameterwaarden.

De gedownloade sjabloon gebruiken voor het bijwerken van een configuratie:

1. Pak de inhoud naar een map op uw lokale computer.
2. Wijzig de inhoud aan de nieuwe configuratie.
3. PowerShell Start en Ga naar de map waarin u de inhoud hebt uitgepakt.
4. Uitvoeren van **deploy.ps1** en vul de abonnement-ID, de naam van de bron (Gebruik dezelfde naam voor het bijwerken van de configuratie) en een implementatie van de unieke naam.


### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a>De uitbreiding van de diagnostische gegevens als onderdeel van het maken van het cluster implementeren met behulp van bronbeheer Azure
Een cluster maken met behulp van bronbeheer, moet u de configuratie van diagnostische JSON aan het volledige cluster Resource Manager sjabloon toevoegen voordat u het cluster maakt. Wij bieden een voorbeeldsjabloon vijf VM cluster Resource Manager configuratie van diagnostische toegevoegd als onderdeel van onze Resource Manager sjabloon monsters. Kunt u deze zien op deze locatie in de galerie met Azure monsters: [vijf-node cluster met diagnostische gegevens Resource Manager sjabloon voorbeeldgegevens](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).

Open het bestand azuredeploy.json en zoek naar **IaaSDiagnostics**overzicht van de instelling van de diagnostische gegevens in de sjabloon Resource Manager. Een cluster maken met behulp van deze sjabloon, klik op **Deploy naar Azure** beschikbaar op de vorige koppeling.

Ook u kunt downloaden van het monster Resource Manager, wijzigingen aanbrengen en een cluster met de gewijzigde sjabloon maken met de `New-AzureRmResourceGroupDeployment` in een venster Azure PowerShell command. Zie de volgende code voor de parameters die u aan de opdracht doorgeeft. Zie het artikel [een bronnengroep met de sjabloon Azure Resource Manager distribueren](../resource-group-template-deploy.md)voor meer informatie over het implementeren van een resourcegroep via PowerShell.

```powershell

New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -Name $deploymentName -TemplateFile $pathToARMConfigJsonFile -TemplateParameterFile $pathToParameterFile –Verbose
```

### <a name="deploy-the-diagnostics-extension-to-an-existing-cluster"></a>De uitbreiding van de diagnostische gegevens aan een bestaand cluster implementeren
Als u een bestaand cluster dat geen diagnose is geïmplementeerd, of als u een bestaande configuratieset aanpassen, kunt u toevoegen of bijwerken. Wijzig de Resource Manager-sjabloon die wordt gebruikt voor het maken van het cluster of de sjabloon downloaden vanaf de portal, zoals eerder is beschreven. Het bestand template.json wijzigen door de volgende taken uitvoeren.

Een nieuwe opslag resource toevoegen aan de sjabloon toevoegen aan de sectie bronnen.

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "properties": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
```

 Voeg vervolgens toe aan de sectie parameters net na de definities van de account opslag tussen `supportLogStorageAccountName` en `vmNodeType0Name`. Vervang de tijdelijke aanduiding voor tekst *Typ hier de accountnaam van de opslag* met de naam van de account van de opslag.

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for the application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "storage account name goes here",
      "metadata": {
        "description": "Name for the storage account that contains application diagnostics data from the cluster"
      }
    },
```
Vervolgens werken de `VirtualMachineProfile` sectie van het bestand template.json door de volgende code in de matrix extensies toe te voegen. Zorg ervoor dat een komma toe te voegen aan het begin of het einde, afhankelijk van waar deze ingevoegd.

```json
{
    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
    "properties": {
        "type": "IaaSDiagnostics",
        "autoUpgradeMinorVersion": true,
        "protectedSettings": {
        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
        "storageAccountEndPoint": "https://core.windows.net/"
        },
        "publisher": "Microsoft.Azure.Diagnostics",
        "settings": {
        "WadCfg": {
            "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": "50000",
            "EtwProviders": {
                "EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
                {
                    "provider": "Microsoft-ServiceFabric-Services",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableServiceEventTable"
                    }
                }
                ],
                "EtwManifestProviderConfiguration": [
                {
                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                    "scheduledTransferLogLevelFilter": "Information",
                    "scheduledTransferKeywordFilter": "4611686018427387904",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricSystemEventTable"
                    }
                }
                ]
            }
            }
        },
        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
        },
        "typeHandlerVersion": "1.5"
    }
}
```

Nadat u het bestand template.json wijzigen zoals beschreven, publiceert u de sjabloon Resource Manager. Als de sjabloon is geëxporteerd, het bestand deploy.ps1 rapport opnieuw publiceert de sjabloon. Nadat u hebt geïmplementeerd, zorg ervoor dat **ProvisioningState** **is voltooid**.


## <a name="update-diagnostics-to-collect-and-upload-logs-from-new-eventsource-channels"></a>Diagnostische gegevens te verzamelen en uploaden van logbestanden van nieuwe gebeurtenisbron kanalen bijwerken
Bijwerken van diagnostische functies voor het verzamelen van Logboeken van nieuwe gebeurtenisbron kanalen met daarin een nieuwe toepassing die u wilt implementeren, voert u de stappen die in de [vorige sectie](#deploywadarm) uit voor het instellen van de diagnostische gegevens van een bestaand cluster.

Werk de `EtwEventSourceProviderConfiguration` sectie in het bestand template.json vermeldingen toevoegen voor de nieuwe kanalen gebeurtenisbron voordat u de configuratie hebt toegepast met behulp van de `New-AzureRmResourceGroupDeployment` PowerShell-opdracht. De naam van de bron is gedefinieerd als deel van de code in het bestand ServiceEventSource.cs van Visual Studio is gegenereerd.

Als de bron van de gebeurtenis de naam van mijn gebeurtenisbron, bijvoorbeeld de volgende code om de gebeurtenissen uit mijn gebeurtenisbron in een tabel met de naam MyDestinationTableName toevoegen.

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

Voor het verzamelen van prestatiemeteritems of gebeurtenislogboeken, de bronnenbeheerder sjabloon te wijzigen met behulp van de voorbeelden die in [een virtuele Windows-computer met bewaking en diagnostiek met behulp van een sjabloon Azure Resource Manager maken](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md). De bronnenbeheerder sjabloon vervolgens publiceren.

## <a name="next-steps"></a>Volgende stappen
Welke gebeurtenissen u moet letten bij het oplossen van problemen, Zie de diagnostische gebeurtenissen voor [Betrouwbare actoren](service-fabric-reliable-actors-diagnostics.md) en [Betrouwbare Services](service-fabric-reliable-services-diagnostics.md)uitgestraalde inzicht in meer detail.


## <a name="related-articles"></a>Verwante artikelen
* [Meer informatie over het verzamelen van prestatiemeteritems of zich aanmeldt met behulp van de uitbreiding van de diagnostische gegevens](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)
* [Fabric-oplossing in logboek Analytics-service](../log-analytics/log-analytics-service-fabric.md)
