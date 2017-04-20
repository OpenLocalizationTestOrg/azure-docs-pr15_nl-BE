<properties 
    pageTitle="Azure Machine Learning Web serviceparameters gebruiken | Microsoft Azure" 
    description="Het gebruik van Azure Machine Learning Web Service Parameters om het gedrag van uw model als de webservice wordt geopend." 
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
    ms.author="raymondl;garye"/>

#<a name="use-azure-machine-learning-web-service-parameters"></a>Azure Machine Learning Web Service-Parameters gebruiken

Een webservice Azure Machine Learning wordt gemaakt door het publiceren van een experiment met modules met configureerbare parameters. In sommige gevallen kunt u het gedrag van de module wijzigen terwijl de webservice actief is. *Parameters van de Web Service* kunt u voor deze taak. 

Een algemeen voorbeeld wordt ingesteld op de [Gegevens importeren] [ reader] module zodat de gebruiker de gepubliceerde web service een andere gegevensbron opgeven kunt wanneer de webservice wordt geopend. Of de configuratie van de [Gegevens exporteren] [ writer] module zodat u een andere bestemming kan worden opgegeven. Andere voorbeelden zijn het wijzigen van het aantal bits voor de [Hash-functie] [ feature-hashing] module of het nummer van de gewenste functies voor de [functies selecteren op basis van een Filter] [ filter-based-feature-selection] module. 

Stelt u de Parameters van de Web-Service en koppelen aan een of meer moduleparameters in uw experiment en kunt u opgeven of deze verplicht of optioneel zijn. De gebruiker van de webservice kunt geeft u waarden voor deze parameters wanneer zij een beroep doen op de webservice. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


##<a name="how-to-set-and-use-web-service-parameters"></a>Het instellen en gebruik van de Parameters van de Web Service

U definieert een webserviceparameter op het pictogram naast de parameter voor een module en selecteer "Instellen als webserviceparameter". Dit maakt een webserviceparameter en koppelt deze aan die module-parameter. Klik, als de webservice wordt geopend, kan de gebruiker een waarde opgeven voor de Parameter en wordt toegepast op de parameter module.

Als u een Web Service Parameter definieert, is het beschikbaar voor andere module parameter in het experiment. Als u een Web Service-Parameter die is gekoppeld aan een parameter voor een module definieert, kunt u die dezelfde webserviceparameter voor module, zolang de parameter hetzelfde type waarde verwacht. Bijvoorbeeld, als de Parameter een numerieke waarde is, kan vervolgens het alleen worden gebruikt voor moduleparameters die een numerieke waarde verwacht. Wanneer de gebruiker een waarde voor de Parameter instellen, zal deze worden toegepast op alle bijbehorende moduleparameters.

U kunt beslissen of u een standaardwaarde opgeven voor de Parameter. Als u dat doet, zijn de parameter is optioneel voor de gebruiker van de webservice. Als u niet een standaardwaarde opgeeft, is de gebruiker een waarde invoeren wanneer toegang verkregen de webservice tot wordt vereist.

De API-documentatie voor de webservice bevat informatie voor de gebruiker van service voor het opgeven van de Parameter via programmering toegang tot de webservice.

>[AZURE.NOTE] De API-documentatie voor een klassieke webservice wordt geleverd via de koppeling **help API-pagina** in de webservice **DASHBOARD** in Machine Learning Studio. De API-documentatie voor een nieuwe webservice is beschikbaar via de [Webservices van Azure Machine Learning](https://services.azureml.net/Quickstart) portal op de **verbruiken** en **Swagger-API's** voor de webservice.


##<a name="example"></a>Voorbeeld

Als voorbeeld gaan we ervan uit hebben we een experiment met een [Gegevens exporteren] [ writer] module waarmee gegevens worden verzonden naar Azure blobopslag. Definiëren we een Web Service-Parameter met de naam "Blob-pad" waarmee de gebruiker van service voor het pad naar de blobopslag wijzigen wanneer de service wordt geopend.

1.  In de Studio van Machine Learning, klikt u op de [Gegevens exporteren] [ writer] module om deze te selecteren. De eigenschappen worden weergegeven in het deelvenster Eigenschappen, rechts van het canvas experiment.

2.  Geef het opslagtype:

    - Selecteer onder **Geef gegevensbestemming**, 'Azure Blob Storage'.
    - Selecteer onder **Geef verificatietype**, 'Account'.
    - Geef de gegevens voor de Azure blobopslag. 
    <p />

3.  Klik op het pictogram rechts van het **pad naar een blob-begin met container-parameter**. Als volgt uitziet:

    ![Web Service Parameter-pictogram][icon]

    Selecteer "Instellen als webserviceparameter".

    Een vermelding wordt toegevoegd onder de **Parameters van de Web Service** onder aan het deelvenster Eigenschappen met de naam "Pad naar een blob-begin met container". Dit is de Parameter die is gekoppeld aan deze [Gegevens exporteren] [ writer] module-parameter.

4.  Klik op de naam om de naam van de Parameter, invoeren "Blob-pad" en druk op **ENTER** . 
 
5.  Te bieden een standaardwaarde voor de Parameter, klik op het pictogram rechts van de naam 'Zodat de waarde' selecteert, voert u een waarde (bijvoorbeeld ' container1/output1.csv') en druk op **ENTER** .

    ![Webserviceparameter][parameter]

6.  Klik op **uitvoeren**. 

7.  **Web Service implementeren** op en selecteer **Webservice implementeren [Classic]** of **Implementeren van Web Service [Nieuw]** voor de implementatie van de webservice.

De gebruiker van de webservice kan nu nieuwe bestemming opgeeft voor de [Gegevens exporteren] [ writer] module bij de toegang tot de webservice.

##<a name="more-information"></a>Meer informatie

Zie de [Parameters van de Web Service](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) -vermelding in de [Machine Learning Blog](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx)voor een meer gedetailleerd voorbeeld.

Zie voor meer informatie over de toegang tot een webservice Machine Learning [verbruiken een gepubliceerde machine learning web-service](machine-learning-consume-web-services.md).



<!-- Images -->
[icon]: ./media/machine-learning-web-service-parameters/icon.png
[parameter]: ./media/machine-learning-web-service-parameters/parameter.png


<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
 
