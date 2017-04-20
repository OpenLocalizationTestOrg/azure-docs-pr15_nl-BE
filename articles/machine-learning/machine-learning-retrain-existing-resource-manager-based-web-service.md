<properties
    pageTitle="Een bestaande voorspellende webservice Retrain | Microsoft Azure"
    description="Informatie over het model retrain en bijwerken van de webservice voor het gebruik van het model nieuw opgeleide in Azure Machine Learning."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="v-donglo"/>


# <a name="retrain-an-existing-predictive-web-service"></a>Een bestaande voorspellende webservice Retrain

Dit document beschrijft het proces opnieuw opleiden voor het volgende scenario:

* U hebt een experiment training en een voorspellende experiment die u hebt geïmplementeerd als een webservice met geoperationaliseerd.
* U hebt nieuwe gegevens die u uw voorspellende webservice wilt te gebruiken voor het uitvoeren van de score.

Beginnen met uw bestaande webservice en experimenten, moet u als volgt te werk:

1. Het model bijwerken.
    1. Wijzig uw experiment training voor web service ingangen en uitgangen.
    2. Het experiment training implementeren als een webservice opnieuw opleiden.
    3. Met de training-experiment Batch uitvoeren van Service (BES) kunt u het model retrain.
2. Gebruik de Azure Machine Learning PowerShell-cmdlets voor het bijwerken van de voorspellende experiment.
    1.  Aanmelden bij uw account Azure Resource Manager.
    2.  Definitie van de webservice ophalen.
    3.  Definitie van de webservice als JSON exporteren.
    4.  De verwijzing naar de blob ilearner in de JSON bijwerken.
    5.  De JSON importeren in een web service definition.
    6.  De webservice bijwerken met een nieuwe definitie van web service.

## <a name="deploy-the-training-experiment"></a>Implementatie van het experiment training

Voor de implementatie van het experiment training als webservice opnieuw opleiden, moet u web service ingangen en uitgangen aan het model toevoegen. Door een *Web Service Output* module verbinden met het experiment van * [Model trein] [ train-model] * module, als u het experiment training voor de productie van een nieuw opgeleide model die u in uw experiment voorspellende gebruiken kunt inschakelen. Als u een module *Model evalueren* , kunt u uitvoer van de web-service als u de evaluatieresultaten als uitvoer ook koppelen.

Voor het bijwerken van uw experiment training:

1. Verbinding maken met een module *Web Service invoer* uw gegevensinvoer (bijvoorbeeld een module *Schoon ontbrekende gegevens* ). Normaal gesproken wilt u ervoor zorgen dat de ingevoerde gegevens op dezelfde manier als de oorspronkelijke trainingsgegevens is verwerkt.
2. Verbinding maken met een *Web Service Output* module de uitvoer van de *Trein Model* module.
3. Als u een module *Model beoordelen* en u wilt de evaluatieresultaten uitvoeren, verbinding maken met een *Web Service Output* module aan de uitvoer van de module *Model evalueren* .

Uw experiment uitvoeren.

Vervolgens moet u het experiment training implementeren als een webservice die een opgeleide model en model resultaten oplevert.  

Onderaan in het canvas experiment op **Webservice instellen**en selecteer vervolgens **Implementeren webservice [Nieuw]**. De webservices van Azure Machine Learning portal geopend op de pagina **Web-Service implementeert** . Typ een naam voor uw webservice, een betalingsplan te kiezen en klik op **Deploy**. U kunt de methode voor de uitvoering van de batchverwerking alleen gebruiken voor opgeleide modellen maken.


## <a name="retrain-the-model-with-new-data-by-using-bes"></a>Het model met de nieuwe gegevens met behulp van BES Retrain

Bijvoorbeeld, we maken gebruik van C# te maken van de toepassing opnieuw opleiden. U kunt ook voorbeeldcode Python of R gebruiken om dit te bewerkstelligen.

Het opnieuw opleiden API's aanroepen:

1. Maak een C#-consoletoepassing in Visual Studio (**nieuwe** > **Project** > **Windows-bureaublad** > **Consoletoepassing**).
2.  Log in om de webservices van Machine Learning portal.
3.  Klik op de webservice waarmee u werkt.
2.  Klik **in beslag nemen**.
3.  Klik onderaan de pagina in de sectie **Voorbeeld van Code** **verbruiken** **Batch**.
5.  C# code van het monster Batchuitvoering Kopieer en plak deze in het bestand Program.cs. Zorg ervoor dat de naamruimte intact blijft.

Het pakket NuGet Microsoft.AspNet.WebApi.Client, zoals aangegeven in de opmerkingen toevoegen. De verwijzing naar een Microsoft.WindowsAzure.Storage.dll wilt toevoegen, moet u mogelijk eerst de [clientbibliotheek voor opslag van Azure services](https://www.nuget.org/packages/WindowsAzure.Storage)installeren.

De volgende schermafdruk toont de pagina **verbruiken** in de portal Azure Machine Learning Web-Services.

![Pagina in beslag nemen][1]

### <a name="update-the-apikey-declaration"></a>Update de verklaring apikey

Zoek de declaratie met **apikey** :

    const string apiKey = "abc123"; // Replace this with the API key for the web service

Ga naar de primaire sleutel in de sectie **gegevens van verbruik op basis** van de pagina **verbruiken** en naar de aangifte **apikey** kopiëren.

### <a name="update-the-azure-storage-information"></a>Informatie over de opslag van Azure bijwerken

De voorbeeldcode BES geüploade bestanden vanaf een lokaal station (bijvoorbeeld "C:\temp\CensusIpnput.csv") naar Azure opslag verwerkt en schrijft de resultaten terug naar Azure opslag.  

De opslag van Azure als gegevens wilt bijwerken, moet u de accountnaam van de opslag, sleutel en container informatie ophalen voor uw account voor opslag van de klassieke Azure portal en de correspondi na het uitvoeren van uw experiment, de resulterende workflow update dan ongeveer als volgt:

![Resulterende workflow na het uitvoeren van][4]NG-waarden in de code.

1. Log in om de klassieke Azure portal.
1. Klik op **opslag**in de kolom links navigatie.
1. In de lijst met accounts voor opslag, een voor het opslaan van het retrained model te selecteren.
1. Klik onderaan de pagina **Sleutels beheren van Access**.
1. Kopiëren en opslaan van de **Primaire sleutel van Access** en het dialoogvenster te sluiten.
1. Klik op **Containers**aan de bovenkant van de pagina.
1. Selecteer een bestaande container of een nieuwe maken en opslaan de naam.

Zoek de *StorageAccountName*, *StorageAccountKey*en *StorageContainerName* -aangiften en de waarden die u hebt opgeslagen in de klassieke portal.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

Ook moet u ervoor zorgen dat het invoerbestand is beschikbaar op de locatie die u in de code opgeeft.

### <a name="specify-the-output-location"></a>Geef de uitvoerlocatie

Wanneer u de locatie van uitvoer in de nettolading aanvragen opgeeft, de extensie van het bestand dat is opgegeven in *RelativeLocation* moet worden opgegeven als `ilearner`. Zie het volgende voorbeeld:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you want to use for your output file and a valid file extension (usually .csv for scoring results or .ilearner for trained models)*/
            }
        },

Het volgende is een voorbeeld van uitvoer opnieuw opleiden: ![bijscholing uitvoer][6]

## <a name="evaluate-the-retraining-results"></a>Evalueren van de resultaten van het opnieuw opleiden

Wanneer u de toepassing uitvoert, wordt de uitvoer bevat de URL en de gedeelde handtekeningen toegangstoken die nodig zijn voor toegang tot de resultaten van de evaluaties.

U ziet de prestatieresultaten van het model retrained door de *BaseLocation*, *RelativeLocation*en *SasBlobToken* van de resultaten van de uitvoer voor *output2* combineren (zoals in de voorgaande opnieuw opleiden output afbeelding) en de volledige URL in de adresbalk van de browser te plakken.  

Bekijk de resultaten om te bepalen of het model nieuw opgeleide goed genoeg ter vervanging van bestaande uitvoert.

Kopieer de *BaseLocation*, *RelativeLocation*en *SasBlobToken* van het resultaat.

## <a name="retrain-the-web-service"></a>De webservice Retrain

Wanneer u een nieuwe webservice retrain, kunt u de definitie van de voorspellende webservice als u wilt verwijzen naar het nieuwe model opgeleide bijwerken. Definitie van de webservice is een interne representatie van de opgeleide model van de web-service en kan niet rechtstreeks worden gewijzigd. Zorg ervoor dat u voor uw experiment voorspellende en niet uw training experiment definitie van de webservice ophaalt.

## <a name="sign-in-to-azure-resource-manager"></a>Log in op Azure Resource Manager

U moet eerst aanmelden bij uw Azure-account binnen de PowerShell-omgeving met behulp van de cmdlet [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) .

## <a name="get-the-web-service-definition-object"></a>Het object Web Service Definition ophalen

Vervolgens krijgt het object Web Service Definition door het aanroepen van de cmdlet [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) .

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Om te bepalen van de naam van de resource van een bestaand webservice, voer de cmdlet Get-AzureRmMlWebService zonder parameters om de webservices op uw abonnement weer te geven. Ga naar de webservice en zoek op het web service-ID. Het vierde element-id, is de naam van de resourcegroep net na de *resourceGroups* -element. In het volgende voorbeeld wordt de naam van de bron standaard-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Ook om te bepalen van de naam van de resource van een bestaand webservice, het aanmelden bij de webservices van Azure Machine Learning portal. Selecteer de webservice. De naam van de resource is het vijfde element van de URL van de webservice, net na de *resourceGroups* -element. In het volgende voorbeeld wordt de naam van de bron standaard-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-object-as-json"></a>Het object Web Service Definition exporteren als JSON

U wijzigt de definitie van de opgeleide model het model nieuw opgeleide gebruiken, moet u eerst de cmdlet [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) exporteren naar een bestand JSON-indeling gebruiken.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob"></a>De verwijzing naar de blob ilearner bijwerken

In de activa, zoek [opgeleide model], de *uri* -waarde in het knooppunt *locationInfo* bijwerken met de URI van de blob ilearner. De URI wordt gegenereerd door een combinatie van de *BaseLocation* en de *RelativeLocation* van de uitvoer van de oproep bijscholing BES.

     "asset3": {
        "name": "Retrain Sample [trained model]",
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

## <a name="import-the-json-into-a-web-service-definition-object"></a>De JSON importeren in een definitie van Web Service-object

De cmdlet [Import-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) moet u het gewijzigde JSON-bestand converteren naar een Web Service Definition-object dat u gebruiken kunt voor het bijwerken van het experiment met predicative.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service"></a>De webservice bijwerken

Tot slot de cmdlet [Update-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) gebruiken voor het bijwerken van de voorspellende experiment.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -

[1]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-consume-page.png
[4]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE04.png
[6]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE06.png

<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
