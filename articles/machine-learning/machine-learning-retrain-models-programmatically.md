<properties
    pageTitle="Machine Learning modellen programmatisch Retrain | Microsoft Azure"
    description="Informatie over het programmatisch retrain een model en de webservice voor het gebruik van het model nieuw opgeleide in Azure Machine Learning bijwerken."
    services="machine-learning"
    documentationCenter=""
    authors="raymondlaghaeian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="raymondl;garye;v-donglo"/>


# <a name="retrain-machine-learning-models-programmatically"></a>Machine Learning modellen programmatisch Retrain  

In deze procedure leert u hoe u programmatisch retrain een Azure Machine Learning Web-Service met behulp van C# en de uitvoering van Machine Learning Batch-service.

Zodra u het model hebt retrained, de volgende scenario's wordt aangegeven hoe het model in de voorspellende webservice bijwerken:

- Als u een webservice Klassiek in de portal Machine Learning Web Services geïmplementeerd, Zie [Retrain een klassieke webservice](machine-learning-retrain-a-classic-web-service.md). 
- Als u een nieuwe webservice geïmplementeerd, Zie [Retrain de cmdlets Machine Learning Management met een nieuwe webservice](machine-learning-retrain-new-web-service-using-powershell.md).

Zie [een Machine Learning Model Retrain](machine-learning-retrain-machine-learning-model.md)voor een overzicht van het proces opnieuw opleiden.

Als u beginnen met uw bestaande nieuwe Azure Resource Manager gebaseerd webservice wilt, Zie [een bestaand voorspellende webservice Retrain](machine-learning-retrain-existing-resource-manager-based-web-service.md).

## <a name="create-a-training-experiment"></a>Maak een experiment training
 
In dit voorbeeld gebruikt u "monster 5: trein, testen, evalueren voor binaire indeling: volwassen Dataset ' van de Microsoft Azure Machine Learning monsters. 
    
Het experiment maken:

1.  Log in om Microsoft Azure Machine Learning Studio. 
2.  In de rechterbenedenhoek van het dashboard, klik op **Nieuw**.
3.  Selecteer Microsoft-Samples, Sample 5.
4.  Wijzig de naam van het experiment, boven het canvas experiment, selecteert u de naam van het experiment "monster 5: trein, testen, evalueren voor binaire indeling: volwassen Dataset".
5.  Type telling Model.
6.  Canvas aan de onderkant van het experiment, klik op **uitvoeren**.
7.  Klik op **webservice instellen** en selecteer **webservice Retraining**. 

    ![Eerste experiment.][2]

Diagram 2: De eerste experiment.

## <a name="create-a-predictive-experiment-and-publish-as-a-web-service"></a>Een anticiperende experiment maken en publiceren als een webservice  

Vervolgens maakt u een Experiment met Predicative.

1.  Onderaan op het canvas experiment, klikt u op **Webservice instellen** en selecteer **Voorspellende Web Service**. Dit wordt het model bijvoorbeeld opgeslagen als ervaren Model en modules voor het invoer en uitvoer van de webpagina toegevoegd. 
2.  Klik op **uitvoeren**. 
3.  Nadat het experiment is voltooid, klikt u op **Webservice implementeren [Classic]** of **Implementeren van Web Service [Nieuw]**.

## <a name="deploy-the-training-experiment-as-a-training-web-service"></a>Het experiment training implementeren als een webservice Training

Als u wilt het model opgeleide retrain, moet u het experiment training die u hebt gemaakt als een webservice Retraining implementeren. Deze webservice moet een module *Web Service Output* is verbonden met de * [Trein Model] [ train-model] * module kunnen nieuwe opgeleide modellen te produceren.

1. Als u wilt terugkeren naar het experiment training, klikt u op het pictogram van de experimenten in het linkerdeelvenster en klik u vervolgens op het experiment met de naam telling Model.  
2. Typ in het vak Zoeken naar objecten Experiment zoeken webservice. 
3. Sleep een module *Web Service invoer* naar het canvas experiment en verbinding maken met de uitvoer naar de module *Schoon ontbrekende gegevens* .  Dit zorgt ervoor dat uw gegevens opnieuw opleiden net als de oorspronkelijke trainingsgegevens is verwerkt.
4. Sleep twee *webservice Output* modules op het tekenpapier experiment. De uitvoer van de module *Model trein* verbinding met één en de uitvoer van de module *Evaluatie van Model* naar de andere. De uitvoer van de web service voor **Trein Model** geeft ons het nieuwe model zijn opgeleid. Gekoppeld aan het **Evalueren van Model** wordt uitvoer van de module, die de prestatieresultaten.
5. Klik op **uitvoeren**. 

Vervolgens moet u het experiment training implementeren als een webservice die een opgeleide model en model resultaten oplevert. Om dit te bereiken, zijn de volgende reeks acties afhankelijk van of u werkt met een klassieke webservice of een nieuwe webservice.  
  
**Klassieke webservice**

Onderaan op het canvas experiment, klikt u op **Webservice instellen** en selecteer **Webservice implementeren [klassiek]**. De webservice **Dashboard** wordt weergegeven met de API-sleutel en de help-pagina API voor batchverwerking uitvoeren. Alleen de uitvoering van de Batch-methode kan worden gebruikt voor het maken van modellen zijn opgeleid.

**Nieuwe webservice**

Onderaan op het canvas experiment, klikt u op **Webservice instellen** en selecteer **Distribueren Web Service [Nieuw]**. Hiermee opent u de webservices Web Service Azure Machine Learning portal naar de pagina met webonderdelen implementeren. Typ een naam voor uw webservice en een betalingsplan te kiezen en klik op **Deploy**. Alleen de Batch Execution-methode kan worden gebruikt voor het maken van opgeleide modellen

In beide gevallen na experiment uitgevoerd, er de resulterende workflow als volgt uit:

![Resulterende workflow nadat uitgevoerd.][4]

Diagram 3: Uiteindelijke workflow nadat uitgevoerd.

## <a name="retrain-the-model-with-new-data-using-bes"></a>Het model met de nieuwe gegevens met behulp van BES Retrain

In dit voorbeeld gebruikt u C# te maken van de toepassing opnieuw opleiden. U kunt de voorbeeldcode Python of R ook gebruiken om dit te bewerkstelligen.

De bijscholing-API's aanroepen:

1. Maak een C#-consoletoepassing in Visual Studio (Nieuw -> Project -> Windows Desktop -> consoletoepassing).
2.  Aanmelden bij de webservice van Machine Learning portal.
3.  Als u met een klassieke webservice werkt, klikt u op **Klassieke Web Services**.
    1.  Klik op de webservice die u werkt.
    2.  Klik op het standaardeindpunt.
    3.  Klik **in beslag nemen**.
    4.  Klik onderaan de pagina in de sectie **Voorbeeld van Code** **verbruiken** **Batch**.
    5.  Ga verder met stap 5 van deze procedure.
4.  Als u met een nieuwe webservice werkt, klikt u op **Web-Services**.
    1.  Klik op de webservice die u werkt.
    2.  Klik **in beslag nemen**.
    3.  Klik onderaan de pagina in de sectie **Voorbeeld van Code** verbruiken **Batch**.
5.  C# code van het monster Batchuitvoering Kopieer en plak deze in het bestand Program.cs en zorg dat de naamruimte blijft intact.

Het pakket Nuget Microsoft.AspNet.WebApi.Client zoals aangegeven in de opmerkingen toevoegen. De verwijzing naar een Microsoft.WindowsAzure.Storage.dll wilt toevoegen, moet u mogelijk eerst de clientbibliotheek voor Microsoft Azure storage-services installeren. Zie [Windows Storage-Services](https://www.nuget.org/packages/WindowsAzure.Storage)voor meer informatie.

### <a name="update-the-apikey-declaration"></a>Update de verklaring apikey

Zoek de declaratie met **apikey** .

    const string apiKey = "abc123"; // Replace this with the API key for the web service

Ga naar de primaire sleutel in de sectie **gegevens van verbruik op basis** van de pagina **verbruiken** en naar de aangifte **apikey** kopiëren.

### <a name="update-the-azure-storage-information"></a>Informatie over de opslag van Azure bijwerken

De voorbeeldcode BES geüploade bestanden vanaf een lokaal station (bijvoorbeeld "C:\temp\CensusIpnput.csv") naar Azure opslag verwerkt en schrijft de resultaten terug naar Azure opslag.  

Voor het uitvoeren van deze taak moet u de account naam en sleutel container opslaginformatie ophalen voor uw account opslag van de klassieke Azure portal en de bijbehorende waarden van de update in de code. 

1. Log in om de klassieke Azure portal.
1. Klik op **opslag**in de kolom links navigatie.
1. In de lijst met accounts voor opslag, een voor het opslaan van het retrained model te selecteren.
1. Klik onderaan de pagina **Sleutels beheren van Access**.
1. Kopiëren en opslaan van de **Primaire sleutel van Access** en het dialoogvenster te sluiten. 
1. Klik op **Containers**aan de bovenkant van de pagina.
1. Selecteer een bestaande container of een nieuwe maken en opslaan van de naam.

Zoek de aangiften van *StorageAccountName*, *StorageAccountKey*en *StorageContainerName* en de waarden die u vanuit de portal Azure opgeslagen.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure Storage Account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage Key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage Container name
            
Ook moet u ervoor zorgen dat het invoerbestand is beschikbaar op de locatie die u in de code opgeeft. 

### <a name="specify-the-output-location"></a>Geef de uitvoerlocatie

Bij het opgeven van de locatie van uitvoer in de nettolading van aanvragen, moet de extensie van het bestand dat is opgegeven in *RelativeLocation* als ilearner worden opgegeven. 

Zie het volgende voorbeeld:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you would like to use for your output file, and valid file extension (usually .csv for scoring results, or .ilearner for trained models)*/
            }
        },

>[AZURE.NOTE] De namen van de ontvangstvestigingen uitvoer mogelijk afwijken van de instellingen in dit scenario op basis van de volgorde waarin u de web service output modules toegevoegd. Aangezien u dit experiment training met twee uitgangen hebt ingesteld, zijn de resultaten opslag locatie-informatie voor beide.  

![Bijscholing van uitvoer][6]

Diagram 4: Bijscholing uitvoer.

## <a name="evaluate-the-retraining-results"></a>Evalueren van de resultaten van het opnieuw opleiden
 
Wanneer u de toepassing uitvoert, wordt de uitvoer de URL en SAS-token nodig voor toegang tot de resultaten van de beoordeling bevat.

U ziet de prestatieresultaten van het retrained model door de *BaseLocation*, *RelativeLocation*en *SasBlobToken* van de resultaten van de uitvoer voor *output2* combineren (zoals in de voorgaande opnieuw opleiden output afbeelding) en de volledige URL in de adresbalk van de browser te plakken.  

Bekijk de resultaten om te bepalen of het model nieuw opgeleide goed genoeg ter vervanging van bestaande uitvoert.

De *BaseLocation*, *RelativeLocation*en *SasBlobToken* van de resultaten van uitvoer kopiëren, gebruikt u deze tijdens het opnieuw opleiden.

## <a name="next-steps"></a>Volgende stappen

Als u de webservice voorspellende geïmplementeerd door te klikken op de **Web-Service implementeren [Classic]**, Zie [Retrain een klassieke webservice](machine-learning-retrain-a-classic-web-service.md).

Als u de webservice voorspellende geïmplementeerd door te klikken op de **Implementatie van Web Service [Nieuw]**, Zie [Retrain de cmdlets Machine Learning Management met een nieuwe webservice](machine-learning-retrain-new-web-service-using-powershell.md).

<!-- Retrain a New web service using the Machine Learning Management REST API -->


[1]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png
[7]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE07.png


<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/