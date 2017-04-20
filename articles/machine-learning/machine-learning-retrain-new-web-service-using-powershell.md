<properties
    pageTitle="Een nieuwe webservice met de Machine Learning Management PowerShell-cmdlets Retrain | Microsoft Azure"
    description="Informatie over het programmatisch retrain een model en de webservice voor het gebruik van het model nieuw opgeleide in Azure Machine Learning met de Machine Learning Management PowerShell-cmdlets bijwerken."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondlaghaeian"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="v-donglo"/>

# <a name="retrain-a-new-web-service-using-the-machine-learning-management-powershell-cmdlets"></a>Een nieuwe webservice met de Machine Learning Management PowerShell-cmdlets Retrain

Wanneer u een nieuwe webservice retrain, kunt u de definitie van de voorspellende webservice als u wilt verwijzen naar het nieuwe model opgeleide bijwerken.  

## <a name="prerequisites"></a>Vereisten

U moet hebt ingesteld een experiment training en een voorspellende experiment zoals in de [Machine Learning Retrain modellen via programmacode](machine-learning-retrain-models-programmatically.md). 

>[AZURE.IMPORTANT] De voorspellende experiment moet worden geïmplementeerd als een webservice leren (nieuw) op basis van Azure Resource Manager-computer. 
 
Zie voor meer informatie over webservices voor implementatie, [implementeren, een webservice Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).

Dit proces is vereist dat u de Cmdlets Azure Machine Learning hebt geïnstalleerd. Zie de [Azure Machine Learning Cmdlets](https://msdn.microsoft.com/library/azure/mt767952.aspx) reference op MSDN voor informatie over het installeren van de Machine Learning-cmdlets.

De volgende informatie gekopieerd van de uitvoer opnieuw opleiden:

* BaseLocation
* RelativeLocation

De stappen zijn:

1.  Aanmelden bij uw account Azure Resource Manager.
2.  Definitie van de webservice ophalen
3.  Definitie van de webservice als JSON exporteren
4.  De verwijzing naar de blob ilearner in de JSON bijwerken.
5.  De JSON importeren in een Web Service Definition
6.  De webservice bijwerken met nieuwe Web Service Definition

## <a name="sign-in-to-your-azure-resource-manager-account"></a>Aanmelden bij uw account Azure Resource Manager

U moet eerst inloggen om uw Azure-account binnen de PowerShell-omgeving met de cmdlet [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) .

## <a name="get-the-web-service-definition"></a>Definitie van de webservice ophalen

Vervolgens wordt de webservice ophalen door het aanroepen van de cmdlet [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) . Definitie van de webservice is een interne representatie van de opgeleide model van de web-service en kan niet rechtstreeks worden gewijzigd. Zorg ervoor dat u voor uw experiment voorspellende en niet uw training experiment definitie van de webservice ophaalt.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Om te bepalen van de naam van de resource van een bestaand webservice, voer de cmdlet Get-AzureRmMlWebService zonder parameters om de webservices op uw abonnement weer te geven. Ga naar de webservice en zoek op het web service-ID. Het vierde element-id, is de naam van de resourcegroep net na de *resourceGroups* -element. In het volgende voorbeeld wordt de naam van de bron standaard-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

U kunt ook om te bepalen van de naam van de resource van een bestaand webservice, meld u aan voor webservices van Microsoft Azure Machine Learning portal. Selecteer de webservice. De naam van de resource is het vijfde element van de URL van de webservice, net na de *resourceGroups* -element. In het volgende voorbeeld wordt de naam van de bron standaard-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-as-json"></a>Definitie van de webservice als JSON exporteren

Voor het wijzigen van de definitie van de opgeleide model te gebruiken nieuwe opgeleide Model, moet u de [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) -cmdlet exporteren naar een JSON-bestandsindeling.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob-in-the-json"></a>De verwijzing naar de blob ilearner in de JSON bijwerken.

In de activa, zoek [opgeleide model], de *uri* -waarde in het knooppunt *locationInfo* bijwerken met de URI van de blob ilearner. De URI wordt gegenereerd door een combinatie van de *BaseLocation* en de *RelativeLocation* van de uitvoer van de oproep bijscholing BES.

     "asset3": {
        "name": "Retrain Samp.le [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-the-json-into-a-web-service-definition"></a>De JSON importeren in een Web Service Definition

De cmdlet [Import-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) moet u het gewijzigde JSON-bestand converteren naar een Web Service Definition die u gebruiken kunt voor het bijwerken van het Experiment met Predicative.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service-with-new-web-service-definition"></a>De webservice bijwerken met nieuwe Web Service Definition

Ten slotte kunt u de cmdlet [Update-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) het experiment voorspellende bijwerken.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a>Samenvatting

Het beheer van Machine Learning PowerShell cmdlets kunt u de opgeleide model van een anticiperende webservice inschakelen van scenario's, zoals bijwerken:

* Periodieke model bijscholing met nieuwe gegevens.
* Verdeling van een model voor klanten met het doel van deze retrain het model met hun eigen gegevens toe.
