<properties
    pageTitle="Implementeren van een webservice Machine Learning | Microsoft Azure"
    description="Hoe een experiment training converteren naar een voorspellende experiment, voorbereiden voor distributie en vervolgens als een webservice Azure Machine Learning."
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
    ms.date="10/04/2016"
    ms.author="garye"/>

# <a name="deploy-an-azure-machine-learning-web-service"></a>Een webservice Azure Machine Learning implementeren

Azure Machine Learning kunt u bouwen, testen en implementeren van anticiperende analytische oplossingen.

Vanuit het op hoog niveau-van-oogpunt, dit gebeurt in drie stappen:

- **[Maken een experiment training]** - Azure Machine Learning Studio is een ontwikkelomgeving voor het gezamenlijke visuele die u gebruikt om te trainen en testen van een anticiperende analytische model met behulp van trainingsgegevens die u opgeeft.
- **[Converteren naar een voorspellende experiment]** - eenmaal uw model is getraind met bestaande gegevens en u bent klaar om te gebruiken voor het verkrijgen van nieuwe gegevens, u voorbereiden en uw experiment voor voorspellingen te stroomlijnen.
- **Als een webservice implementeren** - kunt u uw voorspellende experiment als een [nieuwe] of [klassieke] Azure webservice implementeren. Gebruikers kunnen het model van uw gegevens verzenden en ontvangen van voorspellingen van het model.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="create-a-training-experiment"></a>Maak een experiment training

Als u wilt een anticiperende analytische model van de trein, u Azure Machine Learning Studio maakt een experiment training waar u diverse modules trainingsgegevens laden, de gegevens voorbereiden, machine learning algoritmen toepassen en evalueren van de resultaten opnemen. U kunt bladeren op een experiment en probeer andere machine learning algoritmen om te vergelijken en evalueren van de resultaten.

Het proces van het maken en beheren van opleiding experimenten wordt uitgebreid elders gedekt. Raadpleeg de volgende artikelen voor meer informatie:

- [Maak een eenvoudige experiment in Azure Machine Learning Studio](machine-learning-create-experiment.md)
- [Ontwikkelen van een voorspellende oplossing met Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)
- [De trainingsgegevens importeren in Azure Machine Learning Studio](machine-learning-data-science-import-data.md)
- [Azure Machine Learning Studio experiment herhalingen beheren](machine-learning-manage-experiment-iterations.md)

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Het experiment training converteren naar een voorspellende experiment

Zodra u uw model hebt getraind, u kunt nu uw training experiment converteren naar een voorspellende experiment voor het verkrijgen van nieuwe gegevens.

Door te converteren naar een voorspellende experiment, krijgt u uw opgeleide model klaar om te worden geïmplementeerd als een webservice score. Gebruikers van de webservice ingevoerde gegevens kunnen verzenden aan uw model en het model van uw stuurt weer de resultaten voorspelling. Als u naar een voorspellende experiment converteert, houd er rekening mee hoe u uw model moet worden gebruikt door anderen verwachten.

Uw experiment training conversie naar een voorspellende experiment, klikt u op **uitvoeren** op de onderkant van het papier experiment, klikt u op **Webservice instellen**en selecteer vervolgens **Voorspellende Web Service**.

![Converteren naar scoren experiment](./media/machine-learning-publish-a-machine-learning-web-service/figure-1.png)

Zie [een Machine Learning training experiment een voorspellende experiment converteren](machine-learning-convert-training-experiment-to-scoring-experiment.md)voor meer informatie over het uitvoeren van deze conversie.

De volgende stappen beschrijven het implementeren van een anticiperende experiment als een nieuwe webservice. U kunt ook het experiment als klassieke webservice implementeren.

## <a name="deploy-the-predictive-experiment-as-a-new-web-service"></a>Het experiment voorspellende implementeren als een nieuwe webservice

Nu dat de voorspellende experiment is bedoeld, kunt u deze implementeren als een webservice Azure. Met de webservice, kunnen gebruikers gegevens verzenden naar het model en het model retourneert de voorspellingen.

Klik op **uitvoeren** op de onderkant van het papier experiment voor de implementatie van de voorspellende experiment. Zodra het experiment is voltooid, klikt u op **Webservice implementeren** en selecteer **webservice implementeren [Nieuw]**.  Hiermee opent u de pagina van de implementatie van de webservice van Machine Learning portal.

### <a name="machine-learning-web-service-portal-deploy-experiment-page"></a>Machine Learning Web Service portal Experiment-pagina implementeren

Voer een naam voor de web-service op de pagina Experiment implementeren.
Selecteer een prijsstelling plan. Als u een bestaande prijsstelling plan dat kunt u dit, moet anders u een nieuw plan van de prijs voor de service.

1.  Vervolgkeuzelijst, selecteer een bestaand schema of selecteert u de optie **nieuwe plan selecteren dat** in het **Plan van de prijs** .
2.  Typ in het vak **Naam**een naam op voor het plan op uw rekening.
3.  Selecteer een van de **Maandelijkse plannen lagen**. De standaard plan lagen aan de plannen voor uw regio standaard en de webservice wordt geïmplementeerd in dat gebied.

Klik op **Deploy** en de **Quickstart** -pagina voor uw web service wordt geopend.

De pagina met Quickstart hebt u toegang en richtlijnen op de meest voorkomende taken die u uitvoert na het maken van een webservice. Hier kunt u zowel de Test en verbruiken pagina eenvoudig openen.

<!-- ![Deploy the web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)-->

### <a name="test-your-web-service"></a>Test uw webservice

Als u wilt uw nieuwe webservice testen, klikt u op **webservice testen** onder algemene taken. U kunt uw web-service als een verzoek-antwoord Service (RR's) of voor een Batch worden uitgevoerd (BES) testen op de testpagina.

De testpagina Bronrecords wordt de inputs en outputs en globale parameters die u hebt gedefinieerd voor het experiment. De webservice testen, kunt u handmatig waarden invoeren voor de invoer of leveren van een door komma's gescheiden waarden (CSV) opgemaakt bestand met de testwaarden.

Als u wilt testen met behulp van Bronrecords, uit de lijst weergavemodus, waarden invoeren voor de invoer en op **Verzoek-antwoord testen**. De voorspelling resultaten weergegeven in de uitvoerkolom aan de linkerkant.

![De web-service implementeren](./media/machine-learning-publish-a-machine-learning-web-service/figure-5-test-request-response.png)

Klik om te testen uw BES, **Batch**. Klik op Bladeren onder uw invoer op de testpagina Batch en selecteer een CSV-bestand met de juiste voorbeeldwaarden. Als er geen een CSV-bestand en u uw voorspellende experiment met Machine Learning Studio hebt gemaakt, kunt u de set gegevens voor uw experiment voorspellende downloaden en gebruiken.

Downloaden van de gegevensset Machine Learning Studio te openen. Open de voorspellende experiment en klik met de rechtermuisknop de invoer voor uw experiment. **Dataset** selecteren in het contextmenu en selecteer vervolgens **downloaden**.

![De web-service implementeren](./media/machine-learning-publish-a-machine-learning-web-service/figure-7-mls-download.png)

Klik op **testen**. De status van uw batchverwerking uitvoeren van taak wordt weergegeven rechts onder **Test batchverwerkingen**.

![De web-service implementeren](./media/machine-learning-publish-a-machine-learning-web-service/figure-6-test-batch-execution.png)

<!--![Test the web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)-->

Op de pagina **configuratie** kunt u de beschrijving, de titel wijzigen, de sleutel opslag account bijwerken en voorbeeldgegevens voor de webservice inschakelen.

![De webservice configureren](./media/machine-learning-publish-a-machine-learning-web-service/figure-8-arm-configure.png)

Zodra u de webservice hebt geïmplementeerd, kunt u het volgende doen:

- **Toegang** tot en het web service-API.
- **Beheren** het via Azure Machine Learning web serviceportal of de klassieke Azure portal.
- **Update** het als uw model wordt gewijzigd.

### <a name="access-the-web-service"></a>Toegang tot de webservice.

Zodra u uw webservice van Machine Learning Studio implementeert, kunt u gegevens naar de service verzenden en ontvangen antwoorden via programmacode.

De pagina **verbruiken** biedt alle informatie die u nodig hebt voor toegang tot de webservice. Zo krijgt de API-sleutel voor geautoriseerde toegang tot de service.

Zie voor meer informatie over de toegang tot een webservice Machine Learning [verbruiken een geïmplementeerde webservice van Azure Machine Learning](machine-learning-consume-web-services.md).

### <a name="manage-your-new-web-service"></a>De nieuwe web-service beheren

U kunt het klassieke services Machine Learning Web Services webportaal beheren. De [belangrijkste portal-pagina](https://services.azureml-test.net/), klik op **Web Services**. U kunt verwijderen of kopiëren van een service vanaf de webpagina-services. Een bepaalde service te controleren, klikt u op de service en klik vervolgens op **Dashboard**. Als u wilt controleren op batchtaken die zijn gekoppeld aan de webservice, klikt u op **Logboek voor aanvraag voor Batch**.

## <a name="deploy-the-predictive-experiment-as-a-classic-web-service"></a>Het experiment voorspellende implementeren als een webservice klassiek

Nu dat het experiment voorspellende voldoende is voorbereid, kunt u deze implementeren als een webservice Azure. Met de webservice, kunnen gebruikers gegevens verzenden naar het model en het model retourneert de voorspellingen.

Voor de implementatie van de voorspellende experiment **uitvoeren** op de onderkant van het papier experiment en klik op **Web-Service implementeert**. De webservice is ingesteld en u worden geplaatst in het dashboard web service.

![De web-service implementeren](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)

U kunt de webservice testen in de webservices van Machine Learning portal of een Machine Learning Studio.

Test de webservice antwoord vragen, klikt u op de knop **testen** in het dashboard web service. Een dialoogvenster verschijnt om te vragen u de ingevoerde gegevens voor de service. Dit zijn de kolommen door het experiment score verwacht. Voer een set gegevens en klik vervolgens op **OK**. De resultaten die door de webservice worden weergegeven onderaan in het dashboard.

U kunt klikken op de koppeling voorbeeld **testen** om te testen uw service in de portal Azure Machine Learning Web-Services zoals eerder in de sectie nieuwe web service.

De Batch worden uitgevoerd om Service te testen, klikt u op voorbeeld koppeling **testen** . Klik op Bladeren onder uw invoer op de testpagina Batch en selecteer een CSV-bestand met de juiste voorbeeldwaarden. Als er geen een CSV-bestand en u uw voorspellende experiment met Machine Learning Studio hebt gemaakt, kunt u de set gegevens voor uw experiment voorspellende downloaden en gebruiken.

![De web-service testen](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)

Op de pagina **configuratie** kunt u de weergavenaam van de service wijzigen en een omschrijving geven. De naam en de beschrijving wordt weergegeven in de [Azure klassieke portal](http://manage.windowsazure.com/) waar u uw web-services beheren.

U kunt een beschrijving opgeven voor invoergegevens uitvoergegevens en parameters van de web service door een tekenreeks opgeven voor elke kolom onder **Invoer SCHEMA** **UITVOERSCHEMA**en **WEBSERVICEPARAMETER**. Deze beschrijvingen worden in de steekproef code documentatie voor de webservice gebruikt.

Hier kunt u logboekregistratie voor het vaststellen van eventuele fouten die u kan zien wanneer uw webservice toegankelijk is. Zie [Logboekregistratie inschakelen voor Machine Learning web-services](machine-learning-web-services-logging.md)voor meer informatie.

![De webservice configureren](./media/machine-learning-publish-a-machine-learning-web-service/figure-4.png)

U kunt ook de eindpunten voor de webservice configureren in de portal Azure Machine Learning Web Services vergelijkbaar met de procedure die eerder in de sectie nieuwe web service wordt weergegeven. Zijn er andere opties, kunt u toevoegen of wijzigen van de servicebeschrijving, logboekregistratie inschakelen en activeren voorbeeldgegevens om te testen.

### <a name="access-the-web-service"></a>Toegang tot de webservice.

Zodra u uw webservice van Machine Learning Studio implementeert, kunt u gegevens naar de service verzenden en ontvangen antwoorden via programmacode.

Het dashboard biedt alle informatie die u nodig hebt voor toegang tot de webservice. Bijvoorbeeld, de API-sleutel voor geautoriseerde toegang tot de service wordt geleverd en API help-pagina's zijn bedoeld om te helpen beginnen uw code te schrijven.

Zie voor meer informatie over de toegang tot een webservice Machine Learning [verbruiken een geïmplementeerde webservice van Azure Machine Learning](machine-learning-consume-web-services.md).

### <a name="manage-the-web-service"></a>De web-service beheren

Er zijn verschillende acties die u kunt uitvoeren om een webservice te controleren. U kunt het bijwerken en verwijderen. Ook kunt u extra eindpunten toevoegen aan een webservice klassieke naast de standaardeindpunt dat wordt gemaakt wanneer u het implementeert.

Zie [een Azure Machine Learning werkruimte beheren](machine-learning-manage-workspace.md) en [een webservice met behulp van de portal Azure Machine Learning Web-Services beheren](machine-learning-manage-new-webservice.md)voor meer informatie.

<!-- When this article gets published, fix the link and uncomment
For more information on how to manage Azure Machine Learning web service endpoints using the REST API, see **Azure machine learning web service endpoints**.
-->

## <a name="update-the-web-service"></a>De webservice bijwerken

U kunt wijzigingen aanbrengen in uw webservice, zoals het bijwerken van het model met extra training-gegevens en implementeren, overschrijft de oorspronkelijke webservice.

Open het oorspronkelijke voorspellende experiment dat u gebruikt voor het implementeren van de webservice en een bewerkbare kopie maken door te klikken op **Opslaan als**om te werken in de webservice. Breng de wijzigingen aan en klik vervolgens op **Webservice implementeren**.

Omdat u dit experiment voordat hebt geïmplementeerd, wordt u gevraagd of u wilt overschrijven (klassieke Web Service) of de bestaande service (nieuwe webservice) bijwerken. Te klikken op **Ja** of **Update** de bestaande web-service gestopt en implementeert de nieuwe voorspellende experiment in plaats daarvan wordt geïmplementeerd.

> [AZURE.NOTE] Als u configuratiewijzigingen in de oorspronkelijke webservice aangebracht, bijvoorbeeld moet het invoeren van een nieuwe naam of omschrijving, u opnieuw in te voeren die waarden.

Een optie voor het bijwerken van uw web-service is het model programmatisch retrain. Voor meer informatie Zie [Machine Learning Retrain modellen via programmacode](machine-learning-retrain-models-programmatically.md).


<!-- internal links -->
[Maak een experiment training]: #create-a-training-experiment
[Converteren naar een voorspellende experiment]: #convert-the-training-experiment-to-a-predictive-experiment
[Nieuw]: #deploy-the-predictive-experiment-as-a-new-Web-service
[klassiek]: #deploy-the-predictive-experiment-as-a-new-Web-service
[Access]: #access-the-Web-service
[Manage]: #manage-the-Web-service-in-the-azure-management-portal
[Update]: #update-the-Web-service
