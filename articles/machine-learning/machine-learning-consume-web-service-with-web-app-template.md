<properties
    pageTitle="Een Machine Learning webservice met een websjabloon app verbruiken | Microsoft Azure"
    description="Gebruik een websjabloon app in Azure Marketplace te verbruiken een voorspellende webservice in Azure Machine Learning."
    keywords="webservice, uitoefening, REST API, machine learning"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="garye;raymondl"/>

# <a name="consume-an-azure-machine-learning-web-service-with-a-web-app-template"></a>Een webservice Azure Machine Learning een web app-sjabloon gebruiken

>[AZURE.NOTE] In dit onderwerp wordt beschreven technieken die van toepassing is op een klassieke webservice. 

Zodra u hebt uw voorspellende model ontwikkeld en geïmplementeerd in een Azure webservice met behulp van Machine Learning Studio of hulpmiddelen voor R of Python, u kunt toegang krijgen tot het operationalized model met een REST API.

Er zijn een aantal manieren om de REST API verbruiken en toegang tot de webservice. U kunt bijvoorbeeld een toepassing schrijven in C#, R of Python met de voorbeeldcode voor u gegenereerd wanneer u de webservice (beschikbaar op de pagina API Help in het dashboard web service in Machine Learning Studio) geïmplementeerd. Of u kunt de Microsoft Excel-werkmap monster gemaakt (ook beschikbaar in het dashboard web service in Studio).

Maar de snelste en eenvoudigste manier om toegang tot uw webservice via het Web App sjablonen die beschikbaar zijn in de [Azure Web App marktplaats](https://azure.microsoft.com/marketplace/web-applications/all/)is.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="the-azure-machine-learning-web-app-templates"></a>De Azure Machine Learning App websjablonen

Web app sjablonen beschikbaar in de markt Azure kunnen maken voor een aangepaste web app die uw webservice invoergegevens en verwachte resultaten kent. U hoeft te doen is de web app toegang geven tot uw webservice en de gegevens, en doet de rest van de sjabloon.

Twee sjablonen zijn beschikbaar:

- [Sjabloon voor Azure ML verzoek-antwoord Service Web App](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
- [Sjabloon voor Azure ML Batch uitvoering Service Web App](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

Elke sjabloon wordt gemaakt van een steekproef ASP.NET-toepassing met behulp van de API-URI en de sleutel voor de webservice en het implementeren als een website naar Azure. De aanvraag-antwoord Service (RR's)-sjabloon maakt u een web app waarmee u één rij met gegevens verzenden naar de webservice om een enkel resultaat te krijgen. De batchverwerking uitvoeren van Service (BES)-sjabloon maakt u een web app waarmee u een groot aantal rijen met gegevens voor meerdere resultaten te verzenden.

Geen codering is vereist voor het gebruik van deze sjablonen. U hoeft alleen te geven de URI van de API en de sleutel en de sjabloon wordt de toepassing voor u gemaakt.

## <a name="how-to-use-the-request-response-service-rrs-template"></a>Het gebruik van de sjabloon Request-Response Service (RR's)

Zodra u uw webservice hebt geïmplementeerd, kunt u de volgende stappen om de sjabloon Bronrecords web app gebruiken, zoals in het volgende diagram.

![Proces van Bronrecords websjabloon][image1]

1. In de Studio van Machine Learning, opent u het tabblad **Web Services** en open vervolgens de webservice die u wilt openen. De **sleutel van de API** -sleutel te kopiëren en opslaan.

    ![API-sleutel][image3]

2. Open de Help-pagina **Aanvraag/reactie** -API. Boven aan de Helppagina bij het **aanvragen**, de **Aanvraag-URI** -waarde kopiëren en opslaan. Deze waarde ziet er zo uit:

        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true

    ![Aanvraag URI][image4]

3. Ga naar de [Azure portal](https://portal.azure.com) **Login**, klik op **Nieuw**, zoeken naar en selecteer **Azure ML verzoek-antwoord Service Web App**en klik vervolgens op **maken**. 

    - Uw web app een unieke naam geven. Deze naam gevolgd door de URL van de web app zijn `.azurewebsites.net.` bijvoorbeeld`http://carprediction.azurewebsites.net.`

    - Selecteer de Azure abonnement en services die uw webservice wordt uitgevoerd.

    - Klik op **maken**.

    ![Web app maken][image5]

4. Wanneer de implementatie van de web app, Azure is voltooid, klikt u op de **URL** op de pagina web app instellingen in Azure of Voer de URL in een webbrowser. Bijvoorbeeld:`http://carprediction.azurewebsites.net.`

5. Wanneer de web app voor het eerst wordt uitgevoerd vraagt het u naar de **URL van de Post API** en **API-sleutel**.
Voer de waarden die u eerder hebt opgeslagen:
    - **URI van de aanvraag** bij de API Help-pagina voor de **URL van de API-Post**
    - **API-sleutel** van het dashboard web service voor de **API-sleutel**.

    Klik op **indienen**.

    ![Boeken URI en API-sleutel invoeren][image6]

6. De web app wordt de pagina **Configuratie van Web App** met de huidige instellingen van de webservice weergegeven. Hier kunt u wijzigingen aanbrengen aan de instellingen die door de web app.

    > [AZURE.NOTE] Deze instellingen alleen wijzigt, worden ze voor dit web app. Dit verandert niet de standaardinstellingen van de webservice. Bijvoorbeeld, als u de **Beschrijving** hier wijzigt verandert het niet de beschrijving die wordt weergegeven op het web servicedashboard in Machine Learning Studio.

    Wanneer u bent klaar, klikt u op **wijzigingen opslaan**en klik vervolgens op **Ga naar de homepage**.

7. Vanaf de introductiepagina kunt u verzenden naar de webservice, klikt u op **verzenden**en het resultaat wordt geretourneerd.

Als u terugkeren naar de pagina **configuratie wilt** , gaat u naar de `setting.aspx` pagina van de web app. Bijvoorbeeld: `http://carprediction.azurewebsites.net/setting.aspx.` , moet u de API-sleutel opnieuw invoeren - u nodig hebt die toegang tot de pagina en de instellingen bijwerken.

U kunt stoppen, opnieuw starten of de web-app in de portal Azure net als elke andere web app verwijderen. U kunt bladeren naar het webadres thuis en nieuwe waarden invoeren als deze wordt uitgevoerd.

## <a name="how-to-use-the-batch-execution-service-bes-template"></a>Het gebruik van de sjabloon Batch uitvoeren van Service (BES)

U kunt de sjabloon BES web app op dezelfde manier als de sjabloon Bronrecords, behalve dat de web-app die gemaakt, kunt u meerdere rijen met gegevens verzenden en ontvangen van meerdere resultaten.

De resultaten van een batch worden uitgevoerd-webservice zijn opgeslagen in een container Azure opslag; de ingevoerde waarden kunnen afkomstig zijn uit de Azure opslag of een lokaal bestand.
Dus, moet u een Azure opslag container voor het opslaan van de resultaten van de web app en moet u uw invoergegevens bereid.

![Proces van de websjabloon BES][image2]

1. Volg de procedure om de BES web app voor de sjabloon Bronrecords maken, met uitzondering van:
    - De **Aanvraag-URI** van de **BATCH EXECUTION** API Help-pagina voor de webservice ophalen.
    - Ga naar [Azure ML Batch Execution Service Web App sjabloon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) de sjabloon BES op Azure Marketplace openen en klik op **Web App maken**.

2. Waar u de resultaten opgeslagen, typ de doelgegevens container op de introductiepagina van het web app. Ook kunt u opgeven waar de web app de invoerwaarden in een lokaal bestand of in een container Azure opslag kan krijgen.
Klik op **indienen**.

    ![Opslaggegevens][image7]

De web app, wordt een pagina met de taakstatus weergegeven.
Wanneer de taak is voltooid krijgt u de locatie van de resultaten in Azure blob-opslag. U hebt ook de optie van het downloaden van de resultaten in een lokaal bestand.

## <a name="for-more-information"></a>Voor meer informatie

Voor meer informatie over...

- [Maak uw eerste experiment in Azure Machine Learning Studio](machine-learning-create-experiment.md) maakt een machine learning experiment met Machine Learning Studio, Zie

- het implementeren van uw computer leren experimenteren als een webservice, raadpleegt u [een webservice Azure Machine leren implementeren](machine-learning-publish-a-machine-learning-web-service.md)

- andere manieren om uw webservice zien [hoe u een webservice Azure Machine Learning verbruiken](machine-learning-consume-web-services.md)


[image1]: media\machine-learning-consume-web-service-with-web-app-template\rrs-web-template-flow.png
[image2]: media\machine-learning-consume-web-service-with-web-app-template\bes-web-template-flow.png
[image3]: media\machine-learning-consume-web-service-with-web-app-template\api-key.png
[image4]: media\machine-learning-consume-web-service-with-web-app-template\post-uri.png
[image5]: media\machine-learning-consume-web-service-with-web-app-template\create-web-app.png
[image6]: media\machine-learning-consume-web-service-with-web-app-template\web-service-info.png
[image7]: media\machine-learning-consume-web-service-with-web-app-template\storage.png
