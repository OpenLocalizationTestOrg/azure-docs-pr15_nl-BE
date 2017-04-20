<properties
    pageTitle="Azure Machine Learning webservices: Distributie en consumptie | Microsoft Azure"
    description="Bronnen voor de implementatie en het gebruik van webservices."
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
    ms.date="10/12/2016"
    ms.author="v-donglo"/>

# <a name="azure-machine-learning-web-services-deployment-and-consumption"></a>Azure Machine Learning Web-Services: Distributie en verbruik

U kunt Azure Machine leren machine learning workflows en modellen als webservices te implementeren. Deze webservices kunnen vervolgens de machine learning modellen aanroepen van toepassingen via het Internet doen voorspellingen in real time of in batchmodus worden gebruikt. Omdat RESTful webservices, kunt u deze aanroepen van verschillende programmeertalen en -platforms, zoals .NET en Java, en van toepassingen, zoals Excel.

De volgende secties bevatten koppelingen naar procedures, code en documentatie waarmee u aan de slag te krijgen.

## <a name="deploy-a-web-service"></a>Implementeren van een webservice

### <a name="with-azure-machine-learning-studio"></a>Met Azure Machine Learning-Studio

Machine Learning Studio en de webservices van Microsoft Azure Machine Learning portal kunt u implementeren en beheren van een webservice-code te schrijven.

De volgende koppelingen vindt u algemene informatie over het implementeren van een nieuwe webservice:

* Zie voor een overzicht over het implementeren van een nieuwe webservice die gebaseerd op Azure Resource Manager, [een nieuwe webservice implementeren](machine-learning-webservice-deploy-a-web-service.md).
* Zie voor een stapsgewijze instructies over het implementeren van een webservice, [een webservice Azure Machine Learning implementeren](machine-learning-publish-a-machine-learning-web-service.md).
* Zie voor een volledig overzicht over het maken en implementeren van een webservice [scenario stap 1: Maak een werkruimte Machine Learning](machine-learning-walkthrough-1-create-ml-workspace.md).
* Zie voor specifieke voorbeelden die een web-service implementeert:

    * [Procedure stap 5: Implementeer de Azure Machine Learning webservice](machine-learning-walkthrough-5-publish-web-service.md)
    * [Het implementeren van een webservice aan meerdere landen](machine-learning-how-to-deploy-to-multiple-regions.md)

### <a name="with-web-services-resource-provider-apis-azure-resource-manager-apis"></a>Met de provider van de resource web services API's (Azure Resource Manager API's)

De provider Azure Machine Learning resource voor webservices kunt implementeren en beheren van webservices met behulp van REST API-aanroepen. Zie de verwijzing [Machine Learning Web Service (REST)](https://msdn.microsoft.com/library/azure/mt767538.aspx) op MSDN voor meer informatie.

### <a name="with-powershell-cmdlets"></a>Met PowerShell-cmdlets

Azure Machine Learning resource provider voor webservices kan implementeren en beheren van webservices met PowerShell-cmdlets.

Gebruik de cmdlets, moet u eerst aanmelden bij uw Azure-account binnen de PowerShell-omgeving met behulp van de cmdlet [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) . Als u niet bekend met bellen PowerShell-opdrachten die zijn gebaseerd op de Resource Manager bent, Zie [Azure PowerShell Azure Resource Manager gebruiken](../powershell-azure-resource-manager.md#login-to-your-azure-account).

Gebruik [deze voorbeeldcode](https://github.com/ritwik20/AzureML-WebServices)wilt exporteren het experiment voorspellende. Nadat u de code het .exe-bestand maakt, kunt u het volgende typen:

    C:\<folder>\GetWSD <experiment-url> <workspace-auth-token>

De toepassing wordt uitgevoerd, maakt een web service JSON-sjabloon. De sjabloon wilt gebruiken voor de implementatie van een webservice, moet u de volgende informatie toevoegen:

* Naam van opslag en de sleutel

    Krijgt u de accountnaam van de opslag en de sleutel uit de [Azure portal](https://portal.azure.com/) of de [Azure klassieke portal](http://manage.windowsazure.com/).
* Verbintenis actieplan-ID

    U krijgt de plan-ID van de [Webservices van Azure Machine Learning](https://services.azureml.net) portal ondertekenen en klikt u op de naam van een plan.

Toevoegen aan de sjabloon JSON als onderliggende elementen van het *Eigenschappen* -knooppunt op hetzelfde niveau als het knooppunt *MachineLearningWorkspace* .

Hier volgt een voorbeeld:

    "StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
    },
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/providers/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

Zie de volgende artikelen en voorbeelden van code voor meer informatie:

* [Azure Machine Learning Cmdlets]( https://msdn.microsoft.com/library/azure/mt767952.aspx) reference op MSDN
* Voorbeeld [scenario](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) op GitHub

## <a name="consume-the-web-services"></a>De webservices

### <a name="from-the-azure-machine-learning-web-services-ui-testing"></a>Uit de Azure Machine Learning webservices UI (test)

U kunt uw web-service van de webservices van Azure Machine Learning portal testen. Dit geldt ook voor de service aanvraag-antwoord (RR's) testen en Batch Execution service (BES)-interfaces.

* [Een nieuwe webservice implementeren](machine-learning-webservice-deploy-a-web-service.md)
* [Een webservice Azure Machine Learning implementeren](machine-learning-publish-a-machine-learning-web-service.md)
* [Procedure stap 5: Implementeer de Azure Machine Learning webservice](machine-learning-walkthrough-5-publish-web-service.md)

### <a name="from-excel"></a>Vanuit Excel

Een Excel-sjabloon dat door de webservice wordt gebruikt, kunt u downloaden:

* [Gebruik een webservice Azure Machine Learning vanuit Excel](machine-learning-consuming-from-excel.md)
* [Excel-invoegtoepassing voor Azure Machine Learning Web Services](machine-learning-excel-add-in-for-web-services.md)


### <a name="from-a-rest-based-client"></a>Vanaf een client met REST

Azure Machine Learning Web Services zijn RESTful API's. U kunt verbruiken deze API's van verschillende platforms, zoals .NET, Python, R, Java, enz. De pagina **verbruiken** voor uw webservice op de [Web Services van Microsoft Azure Machine Learning portal](https://services.azureml.net) bevat voorbeeldcode waarmee u aan de slag kunt. Zie voor meer informatie [een Azure Machine Learning web-service die is geïmplementeerd in een Machine Learning experiment in beslag nemen](machine-learning-consume-web-services.md).

