<properties
pageTitle="Meerdere modellen van een experiment maken | Microsoft Azure"
description="PowerShell gebruik te maken van meerdere modellen van Machine Learning en web-service-endpoints met dezelfde algoritme, maar met verschillende training datasets."
services="machine-learning"
documentationCenter=""
authors="hning86"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="machine-learning"
ms.workload="data-services"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="10/03/2016"
ms.author="garye;haining"/>

# <a name="create-many-machine-learning-models-and-web-service-endpoints-from-one-experiment-using-powershell"></a>Veel modellen van Machine Learning en web service-endpoints maken van een experiment met PowerShell

Dit is een veelvoorkomend probleem van machine learning: voor het maken van veel modellen met dezelfde training workflow en dezelfde algoritme gebruikt, maar andere training datasets als invoer. In dit artikel wordt beschreven hoe u om dit te doen op schaal in Azure Machine Learning Studio met slechts een enkele experiment.

Stel dat u de eigenaar van een globale fiets verhuur franchise business. U wilt bouwen van een regressiemodel te voorspellen wat de huur vraag op basis van historische gegevens. U hebt 1000 verhuur locaties over de hele wereld en u een gegevensset voor elke locatie met belangrijke functies zoals datum, tijd, weer en verkeer die specifiek zijn voor elke locatie hebt verzameld.

U kunt uw model eenmaal een samengevoegde versie van alle datasets met alle locaties kan trainen. Maar omdat elk van de locaties van een unieke omgeving, een betere benadering zou het regressiemodel afzonderlijk met behulp van de gegevensset voor elke vestiging trainen. Op die manier elk model opgeleide kan rekening winkel verschillende formaten, volume, Geografie, bevolking, verkeer fiets-vriendelijke omgeving, *enz*.

Dat de beste aanpak is mogelijk, maar u niet wilt dat 1000 opleiding experimenten in Azure Machine Learning maken met elk een unieke locatie voor een. Naast een overweldigende taak wordt, is het ook lijkt vrij inefficiënt aangezien elk experiment allemaal dezelfde onderdelen, met uitzondering van de dataset training hebben zou.

Gelukkig kan dit worden bereikt met behulp van de [Azure Machine Learning bijscholing API](machine-learning-retrain-models-programmatically.md) en de taak met [Azure Machine Learning PowerShell](machine-learning-powershell-module.md)te automatiseren.

> [AZURE.NOTE] Als u ons voorbeeld sneller worden uitgevoerd, verlagen we het aantal locaties van 1000 tot en met 10. Maar dezelfde beginselen en procedures van toepassing op 1000 locaties. Het enige verschil is dat als u wilt trainen van 1000 datasets u waarschijnlijk wilt beschouwen de volgende PowerShell scripts parallel uitgevoerd. Hoe valt buiten het bestek van dit artikel, maar vindt u voorbeelden van PowerShell multithreading via het Internet.  

## <a name="set-up-the-training-experiment"></a>Instellen van het experiment training

Gaan we een voorbeeld [training experimenteren](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) die we al hebt gemaakt in de [Galerie met Cortana Intelligence](http://gallery.cortanaintelligence.com)gebruiken. Dit experiment in uw werkruimte [Azure Machine Learning Studio](https://studio.azureml.net) openen.

>[AZURE.NOTE] Om te kunnen volgen en in het volgende voorbeeld, kunt u gebruiken in plaats van een werkruimte voor gratis een standaard werkruimte. We maakt één eindpunt voor elke klant - voor een totaal van 10 eindpunten - en waarvoor u een standaard werkruimte omdat een vrije werkruimte beperkt tot 3 eindpunten is. Als u alleen een vrije werkruimte, alleen de scripts voor slechts 3 locaties hieronder wijzigen.

Het experiment wordt een module **Gegevens importeren** met de training dataset *customer001.csv* importeren vanuit een account Azure opslag. Eens we hebben training datasets van alle fiets verhuur locaties verzameld en opgeslagen in dezelfde opslaglocatie blob met bestandsnamen, variërend van *rentalloc001.csv* tot *rentalloc10.csv*.

![afbeelding](./media/machine-learning-create-models-and-endpoints-with-powershell/reader-module.png)

Houd er rekening mee dat een **Web Service Output** module is toegevoegd aan de module van het **Model van de trein** .
Wanneer dit experiment is geïmplementeerd als een webservice, het eindpunt is gekoppeld aan dat uitvoer de opgeleide model in de indeling van een bestand .ilearner retourneren.

Vergeet niet dat we een web Serviceparameter instellen voor de URL van de module **Gegevens importeren** . Dit kan we met behulp van de parameter opgeven datasets individuele training om het model voor elke vestiging trainen.
Er zijn andere manieren waarop die we kunnen dit hebt gedaan, bijvoorbeeld met behulp van een SQL-query met een parameter web service voor het ophalen van gegevens uit een database van SQL Azure of gewoon met een module **Web Service Input** te geven in een dataset aan de webservice.

![afbeelding](./media/machine-learning-create-models-and-endpoints-with-powershell/web-service-output.png)

Nu we dit met de standaard waarde *rental001.csv* als de dataset training training-experiment uitvoeren. Als u de uitvoer van de **evaluatie** module weergeven (Klik op de uitvoer en selecteer **visualiseren**), kunt u zien krijgen we een goede prestaties van de *AUC* = 0.91. Nu we gaan voor de implementatie van een webservice uit dit experiment training.

## <a name="deploy-the-training-and-scoring-web-services"></a>De opleiding en scoren van web-services implementeren

Voor de implementatie van de opleiding webservice, klik op de knop **Web Service instellen** onder het experiment canvas en selecteer **Webservice implementeren**. Roep deze webservice '' fiets verhuur Training'.

Nu moeten we de score webservice implementeren.
We kunnen hiertoe klikt u op **Webservice instellen** onder het canvas en selecteer **Webservice voorspellende**. Hiermee maakt u een score experiment.
Moeten we een paar kleine aanpassingen zodat deze werken als een webservice, zoals het verwijderen van de label-kolom "cnt" van de ingevoerde gegevens en de beperken tot alleen de exemplaar-id en de bijbehorende waarde voorspeld.

Zelf die als werk wilt opslaan, kunt u het [experiment voorspellende](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) gewoon openen in de galerie die al is voorbereid.

Voor de implementatie van de webservice de voorspellende experiment uitvoeren en klik op de knop **Web Service implementeren** onder het canvas. De score webservice 'Fiets verhuur scoren' naam '.

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a>10 eindpunten van een webservice identiek met PowerShell maken

Deze webservice wordt geleverd met een standaardeindpunt. Maar we niet zo geïnteresseerd in het standaardeindpunt omdat kan niet worden bijgewerkt. We moeten is maken 10 extra eindpunten, één voor elke locatie. We zullen dit doen met PowerShell.

Ten eerste stelt onze PowerShell-omgeving:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and is properly set to point to the valid Workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

Voer de volgende PowerShell-opdracht:

    # Create 10 endpoints on the scoring web service.
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpointName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

Nu we 10 eindpunten hebt gemaakt en bevatten ze alle hetzelfde ervaren model getraind voor *customer001.csv*. U kunt deze weergeven in de Azure Management Portal.

![afbeelding](./media/machine-learning-create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-the-endpoints-to-use-separate-training-datasets-using-powershell"></a>De eindpunten voor het gebruik van aparte opleiding datasets met PowerShell bijwerken

De volgende stap is het bijwerken van de eindpunten met modellen die een unieke getraind voor de individuele gegevens van elke klant. Maar eerst moeten we produceren deze modellen van de webservice **Fiets verhuur Training** . We gaan naar de webservice **Fiets verhuur Training** . We moeten bellen het eindpunt BES 10 keer met 10 verschillende training datasets produceren van 10 verschillende modellen. De **InovkeAmlWebServiceBESEndpoint** PowerShell-cmdlet gebruikt om dit te doen.

U moet ook referenties op te geven voor uw account blob storage in `$configContent`, namelijk de velden `AccountName`, `AccountKey` en `RelativeLocation`. De `AccountName` zijn de accountnamen van uw, zoals in de **Klassieke Azure Management Portal** (tabblad*opslag* ). Nadat u op een account met opslag op de `AccountKey` kunt u vinden door onderaan op de knop **Sleutels beheren van toegang** en kopiëren van de *Primaire sleutel van Access*. De `RelativeLocation` is het pad ten opzichte van de opslagruimte waar een nieuw model worden opgeslagen. Bijvoorbeeld het pad `hai/retrain/bike_rental/` in het script onder verwijst naar een container met de naam `hai`, en `/retrain/bike_rental/` zijn submappen. Momenteel u geen submappen via het portal UI maken, maar er zijn [Verschillende Azure opslag Explorers](../storage/storage-explorers.md) waarmee u kunt doen. Het wordt aanbevolen dat u een nieuwe container in opslag maken voor het opslaan van nieuwe opgeleide modellen (.ilearner-bestanden) als volgt: klik op de knop **toevoegen** aan de onderkant van de opslagpagina en noem deze `retrain`. Kortom, de noodzakelijke wijzigingen aan het onderstaande script hebben betrekking op `AccountName`, `AccountKey` en `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).

    # Invoke the retraining API 10 times
    # This is the default (and the only) endpoint on the training web service
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

>[AZURE.NOTE] Het eindpunt van de BES is de enige ondersteunde modus voor deze bewerking. Bronrecords, kan niet worden gebruikt voor de productie van opgeleide modellen.

Zoals u hierboven zien kunt in plaats van 10 verschillende BES taak json-configuratiebestanden, maken we dynamisch in plaats daarvan de configuratietekenreeks maken en diervoeders aan de *jobConfigString* -parameter van de **InvokeAmlWebServceBESEndpoint** -cmdlet, omdat het is echt niet nodig een kopie wilt bewaren op schijf.

Als alles goed gaat, na een tijdje ziet u 10 bestanden van *model001.ilearner* tot *model010.ilearner*, .ilearner in uw account Azure opslag. Nu we gaan onze 10 score eindpunten van een webservice bijwerken met deze modellen met de **Patch AmlWebServiceEndpoint** PowerShell-cmdlet. Onthoud nogmaals dat we alleen de niet-standaard eindpunten die programmatisch wij eerder kunt patch.

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '<my_blob_sas_token>'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }

Dit moet vrij snel uitgevoerd. Als de uitvoering is voltooid, zullen we hebt gemaakt 10 voorspellende eindpunten van een webservice, elk met een opgeleide model een unieke getraind voor de specifieke dataset aan een Huurlocatie van een experiment één training. Om dit te controleren, kunt u proberen het aanroepen van deze eindpunten met de cmdlet **InvokeAmlWebServiceRRSEndpoint** dezelfde invoer van gegevens voorzien en u kunt verwachten verschillende voorspelling om resultaten te bekijken omdat de modellen met verschillende training sets worden opgeleid.

## <a name="full-powershell-script"></a>Volledige PowerShell-script

Dit is de vermelding van de volledige broncode:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and properly set to point to the valid workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

    # Create 10 endpoints on the scoring web service
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpontName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

    # Invoke the retraining API 10 times to produce 10 regression models in .ilearner format
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '?test'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }
