<properties
    pageTitle="Verbinding maken met een webservice Machine Learning | Microsoft Azure"
    description="Met C# of Python, verbinding maken met een Azure Machine Learning webservice met behulp van een sleutel van de vergunning."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016" 
    ms.author="garye" />


# <a name="connect-to-an-azure-machine-learning-web-service"></a>Verbinding maken met een webservice Azure Machine Learning

De ervaring van Azure Machine Learning ontwikkelaar is een Web service API aan voorspellingen van de ingevoerde gegevens in real time of in de batchmodus. Met Azure Machine Learning Studio kunt u prognoses maken en implementeren van een webservice Azure Machine Learning.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Voor meer informatie over het maken en implementeren van Machine Learning met een webservice Machine Learning Studio:

- Zie voor een zelfstudie over het maken van een experiment in Machine Learning Studio, [maken van uw eerste experiment](machine-learning-create-experiment.md).
- Zie voor meer informatie over het implementeren van een webservice, [een Machine Learning Web service implementeren](machine-learning-publish-a-machine-learning-web-service.md).
- In het algemeen gaat u naar de [Bewerkingsplaats Learning documentatie](https://azure.microsoft.com/documentation/services/machine-learning/)voor meer informatie over de computer leren.

## <a name="azure-machine-learning-web-service"></a>Azure Machine Learning Web service ##

Met de webservice Azure Machine Learning communiceert een externe toepassing met een Machine Learning workflow-evaluatiemodel in real-time. Een aanroep van een Machine Learning Web als resultaat voorspelling naar een externe toepassing. Als u een Machine Learning-webservice aanroept, geeft u een API-sleutel die is gemaakt bij het implementeren van een voorspelling. De Machine Learning-webservice is gebaseerd op de REST, een populaire architectuur keuze voor web programmeren projecten.

Azure Machine Learning biedt twee soorten services:

- Service aanvraag-antwoord (RR's) – een lage latentie, zeer schaalbare service die biedt een interface voor het stateless modellen gemaakt en geïmplementeerd in de Studio van Machine Learning.
- Batch worden uitgevoerd-Service (BES) – een asynchrone service die scores een batch voor records met gegevens.

Zie voor meer informatie over Machine Learning Web-services [implementeren een Machine Learning Web service](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="get-an-azure-machine-learning-authorization-key"></a>Een Azure Machine Learning vergunning sleutel ophalen ##

Wanneer u uw experiment implementeert, API sleutels gegenereerd voor de webservice. U kunt de sleutels ophalen vanaf verschillende locaties.

## <a name="from-the-microsoft-azure-machine-learning-web-services-portal"></a>Via de webservices van Microsoft Azure Machine Learning portal

Aanmelden bij de [Web Services van Microsoft Azure Machine Learning](https://services.azureml.net) portal.

Voor het ophalen van de API-sleutel voor een nieuwe Machine Learning Web-service:

1. Klik in de portal Azure Machine Learning Web Services, **Web Services** het bovenste menu.
2. Klik op de webservice die u wilt ophalen van de sleutel.
3. Klik in het bovenste menu **verbruiken**.
4. Kopiëren en opslaan van de **Primaire sleutel**.


Voor het ophalen van de API-sleutel voor een klassieke Machine Learning Web-service:

1. Klik op **Klassieke Web Services** het bovenste menu in de portal Azure Machine Learning Web-Services.
2. Klik op de webservice waarmee u werkt.
3. Klik op het eindpunt waarvoor u gegevens wilt ophalen van de sleutel.
3. Klik in het bovenste menu **verbruiken**.
4. Kopiëren en opslaan van de **Primaire sleutel**.

## <a name="classic-web-service"></a>Klassieke webservice ##

 U kunt ook een sleutel voor een klassieke webservice ophalen van Machine Learning Studio of de Azure portal.

### <a name="machine-learning-studio"></a>Machine Learning Studio ###

1. Klik in Machine Learning Studio **WEB SERVICES** aan de linkerkant.
2. Klik op een webservice. De **API-sleutel** is op het tabblad **DASHBOARD** .

### <a name="azure-portal"></a>Azure portal ###

1. **MACHINE LEARNING** Klik aan de linkerkant.
2. Klik op de werkruimte waarin uw webservice zich bevindt.
3. Klik op de **WEBSERVICES**.
4. Klik op een webservice.
5. Klik op een eindpunt. 'API-sleutel' is niet beschikbaar in de rechterbenedenhoek.

## <a id="connect"></a>Verbinding maken met een webservice Machine Learning

U kunt verbinding maken met een Machine Learning Web service met elke programmeertaal die ondersteuning biedt voor HTTP-aanvraag en respons. U kunt voorbeelden weergeven in C#, Python en R van een Machine Learning Web help-pagina.

**Machine Learning API help** Machine Learning API help wordt gemaakt wanneer u een Web-service implementeert. Zie [Azure Machine Learning scenario - Web-Service implementeert](machine-learning-walkthrough-5-publish-web-service.md).
De help van Machine Learning API bevat details over een voorspelling webservice.

2. Klik op de webservice waarmee u werkt.
3. Klik op het eindpunt waarvan u wilt de pagina API te bekijken.
3. Klik in het bovenste menu **verbruiken**.
3. Klik op **help API-pagina** onder de verzoek-antwoord- of eindpunten Batch worden uitgevoerd.

**Help weergeven Machine Learning API voor een nieuw Web service**

In de Azure Machine Learning Portal van Web Services:

1. Klik op **WEB SERVICES** in het bovenste menu.
2. Klik op de webservice die u wilt ophalen van de sleutel.

Klik op **verbruiken** voor de URI's in C#, R en Python voor de aanvraag Reposonse en Batch uitvoering van Services en voorbeeldcode.

Klik op **Swagger API** om Swagger op basis van de documentatie voor de API's opgeroepen vanuit de opgegeven URI's.

### <a name="c-sample"></a>C#-voorbeeld ###

Gebruiken voor verbinding met een webservice Machine Learning, een **HttpClient** ScoreData doorgeven. ScoreData bevat een FeatureVector, een n-dimensionale vector van numerieke functies die de ScoreData aangeeft. U kunt verifiëren met de computer leren service met een API-sleutel.

Als u wilt verbinden met een webservice Machine Learning, moet het **Microsoft.AspNet.WebApi.Client** NuGet pakket worden geïnstalleerd.

**Microsoft.AspNet.WebApi.Client NuGet in Visual Studio hebt geïnstalleerd**

1. Publiceren van de dataset downloaden van ICB's: 2 volwassenen klasse dataset Web Service.
2. Klik op **Extra** > **NuGet Package Manager** > **Package Manager-Console**.
2. Kies **Microsoft.AspNet.WebApi.Client installatiepakket**.

**In het voorbeeld uitvoeren**

1. Publiceren ' voorbeeld 1: dataset downloaden van ICB's: 2 volwassene klasse dataset "experiment, een deel van de collectie van Machine Learning monster.
2. ApiKey toewijzen met de sleutel van een webservice. Zie **een Azure Machine Learning vergunning sleutel ophalen** hierboven.
3. ServiceUri met de aanvraag-URI toewijzen.


### <a name="python-sample"></a>Python-voorbeeld ###

Gebruik de **urllib2** -bibliotheek ScoreData doorgeven voor verbinding met een webservice Machine Learning. ScoreData bevat een FeatureVector, een n-dimensionale vector van numerieke functies die de ScoreData aangeeft. U kunt verifiëren met de computer leren service met een API-sleutel.


**In het voorbeeld uitvoeren**

1. Implementeren ' voorbeeld 1: Download dataset van ICB's: 2 volwassene klasse dataset "experiment, een deel van de collectie van Machine Learning monster.
2. ApiKey toewijzen met de sleutel van een webservice. Zie de sectie **een sleutel van de vergunning Azure Machine Learning ophalen** aan het begin van dit artikel.
3. ServiceUri met de aanvraag-URI toewijzen.
