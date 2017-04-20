<properties
    pageTitle="Informatie over het beheren van AzureML webservices met behulp van API-Management | Microsoft Azure"
    description="Een handleiding met AzureML webservices beheren met beheer-API."
    keywords="machine learning, management api"
    services="machine-learning"
    documentationCenter=""
    authors="roalexan"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="roalexan" />


# <a name="learn-how-to-manage-azureml-web-services-using-api-management"></a>Meer informatie over het AzureML webservices beheren met beheer-API

##<a name="overview"></a>Overzicht

Deze handleiding wordt beschreven hoe u snel aan de slag met de beheer-API voor het beheren van uw AzureML webservices.

##<a name="what-is-azure-api-management"></a>Wat is Azure API Management?

Azure API beheer is een Azure service waarmee u de eindpunten van de REST API beheren door gebruikerstoegang, gebruik beperken en controleren van het dashboard te definiëren. Klik [hier](https://azure.microsoft.com/services/api-management/) voor meer informatie over het beheer van Azure-API. Klik [hier](../api-management/api-management-get-started.md) voor een handleiding over hoe aan de slag met Azure API Management. Deze andere gids, die in deze handleiding is gebaseerd op, vindt meer onderwerpen, waaronder kennisgeving configuraties, prijzen laag, respons verwerken, gebruikersverificatie, producten, developer abonnementen en dashboarding gebruik maken.

##<a name="what-is-azureml"></a>Wat is AzureML?

AzureML is een Azure voor machine leren waarmee u gemakkelijk bouwen, implementeren en geavanceerde analytics oplossingen delen. Klik [hier](https://azure.microsoft.com/services/machine-learning/) voor meer informatie over AzureML.

##<a name="prerequisites"></a>Vereisten

Als u deze handleiding hebt voltooid, hebt u het volgende nodig:

* Een Azure-account. Als u geen Azure account hebt, klikt u [hier](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie over het maken van een gratis proefperiode account.
* Een AzureML account. Als u geen AzureML account hebt, klikt u [hier](https://studio.azureml.net/) voor meer informatie over het maken van een gratis proefperiode account.
* De werkruimte, service en api_key voor een AzureML experiment geïmplementeerd als een webservice. Klik [hier](machine-learning-create-experiment.md) voor meer informatie over het maken van een experiment met AzureML. Klik [hier](machine-learning-publish-a-machine-learning-web-service.md) voor meer informatie over het implementeren van een experiment met AzureML als een webservice. Bijlage A bevat ook instructies voor het maken en testen van een eenvoudige AzureML experiment als een webservice.

##<a name="create-an-api-management-instance"></a>Maakt u een exemplaar van de beheer-API

Hieronder vindt u de stappen voor het beheren van uw webservice AzureML met beheer-API. Eerst maakt u een service-exemplaar. Log in op de [Klassieke Portal](https://manage.windowsazure.com/) en klik op **Nieuw** > **App Services** > **API Management** > **maken**.

![instantie maken](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-instance.png)

Een unieke **URL**opgeven. Deze handleiding maakt gebruik van **demoazureml** – u moet iets anders te kiezen. Kies het gewenste **abonnement** en **de regio** voor uw service-exemplaar. Nadat u uw selecties, klikt u op de knop Volgende.

![Maak service 1](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-1.png)

Geef een waarde voor de **Naam van de organisatie**. Deze handleiding maakt gebruik van **demoazureml** – u moet iets anders te kiezen. Voer uw e-mailadres in het veld **e-mail systeembeheerder** . Dit e-mailadres wordt voor meldingen van API-beheersysteem gebruikt.

![Maak service 2](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-2.png)

Schakel het selectievakje in om de service-exemplaar te maken. *Duurt het 30 minuten duren voordat een nieuwe service moet worden gemaakt*.

##<a name="create-the-api"></a>Maak de API

Zodra de service-exemplaar is gemaakt, is de volgende stap voor het maken van de API. Een API bestaat uit een reeks bewerkingen die kunnen worden aangeroepen vanuit een clienttoepassing. API-bewerkingen zijn via proxy met bestaande webservices. Deze handleiding maakt u proxyserver aan de bestaande AzureML RRS en BES webservices API's.

API's zijn gemaakt en geconfigureerd via de API publisher portal, dat toegankelijk is via de klassieke Azure-Portal. Selecteer uw service-exemplaar voor het bereiken van de publisher-portal.

![Selecteer-service-exemplaar.](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-service-instance.png)

Klik op **beheren** in de klassieke Azure-Portal voor uw service Management API.

![beheer service](./media/machine-learning-manage-web-service-endpoints-using-api-management/manage-service.png)

Klik op **API's** in het menu **API beheer** aan de linkerkant en klik vervolgens op **Toevoegen API**.

![menu-beheer-API](./media/machine-learning-manage-web-service-endpoints-using-api-management/api-management-menu.png)

Typ de **naam van de Web-API** **AzureML Demo-API** . Typ **https://ussouthcentral.services.azureml.net** als de **webservice-URL**. Typ het **achtervoegsel Web API-URL** **azureml demo** . **HTTPS** controleren als het **Web API-URL** -schema. Selecteer **Starter** als **producten**. Wanneer u klaar bent, klikt u op **Opslaan** om het maken van de API.

![toevoegen-nieuw-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-new-api.png)

##<a name="add-the-operations"></a>De bewerkingen toevoegen

Klik op **Add-bewerking** om bewerkingen toevoegen aan deze API.

![toevoegen-bewerking](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-operation.png)

Het venster **nieuwe bewerking** wordt weergegeven en het tabblad **handtekeningen** wordt standaard geselecteerd.

##<a name="add-rrs-operation"></a>Bewerking van Bronrecords toevoegen

Maak eerst een bewerking voor de service AzureML RRS. **Boeken** als het **HTTP-woord**selecteren. Type **/workspaces/ {werkruimte} /services/ {service} / execute?api versie = {apiversion} & details = {details}** als de **URL van sjabloon**. Typen **Bronrecords uitgevoerd** als de **weergavenaam**.

![bronrecords-bewerking-handtekening toevoegen](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-signature.png)

Klik op **antwoorden** > **toevoegen** aan de linkerkant en selecteer **200 OK**. Klik op **Opslaan** om deze bewerking opslaan.

![toevoegen-bronrecords-bewerking-reactie](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-response.png)

##<a name="add-bes-operations"></a>BES bewerkingen toevoegen

Screenshots zijn niet opgenomen voor de BES bewerkingen zoals ze vergelijkbaar met die zijn voor de bewerking van Bronrecords toe te voegen.

###<a name="submit-but-not-start-a-batch-execution-job"></a>Een taak uitvoeren van Batch indienen (maar niet kan worden gestart)

Klik op **bewerking toevoegen** om de bewerking BES AzureML toevoegen aan de API. Selecteer de **POST** voor het **HTTP-woord**. Type **/workspaces/ {werkruimte} /services/ {service} / jobs?api versie = {apiversion}** voor de **URL van sjabloon**. Typ **BES indienen** voor de **weergegeven naam**. Klik op **antwoorden** > **toevoegen** aan de linkerkant en selecteer **200 OK**. Klik op **Opslaan** om deze bewerking opslaan.

###<a name="start-a-batch-execution-job"></a>Een batchverwerking uitvoeren van taak starten

Klik op **bewerking toevoegen** om de bewerking BES AzureML toevoegen aan de API. Selecteer de **POST** voor het **HTTP-woord**. Type **/workspaces/ {werkruimte} /services/ {service} /jobs/ {jobid} / start?api versie = {apiversion}** voor de **URL van sjabloon**. Typ **Start BES** voor de **weergegeven naam**. Klik op **antwoorden** > **toevoegen** aan de linkerkant en selecteer **200 OK**. Klik op **Opslaan** om deze bewerking opslaan.

###<a name="get-the-status-or-result-of-a-batch-execution-job"></a>De status of het resultaat van een batchverwerking uitvoeren van taak

Klik op **bewerking toevoegen** om de bewerking BES AzureML toevoegen aan de API. Selecteer **ophalen** voor het **HTTP-woord**. Type **/workspaces/ {werkruimte} /services/ {service} /jobs/ {jobid} ?api versie = {apiversion}** voor de **URL van sjabloon**. Typ **BES Status** voor de **weergegeven naam**. Klik op **antwoorden** > **toevoegen** aan de linkerkant en selecteer **200 OK**. Klik op **Opslaan** om deze bewerking opslaan.

###<a name="delete-a-batch-execution-job"></a>Een taak uitvoeren van Batch verwijderen

Klik op **bewerking toevoegen** om de bewerking BES AzureML toevoegen aan de API. Selecteer **verwijderen** voor het **HTTP-woord**. Type **/workspaces/ {werkruimte} /services/ {service} /jobs/ {jobid} ?api versie = {apiversion}** voor de **URL van sjabloon**. Typ **BES verwijderen** voor de **weergegeven naam**. Klik op **antwoorden** > **toevoegen** aan de linkerkant en selecteer **200 OK**. Klik op **Opslaan** om deze bewerking opslaan.

##<a name="call-an-operation-from-the-developer-portal"></a>Een bewerking aanroepen vanuit de Developer-Portal

Activiteiten kunnen rechtstreeks vanuit de Developer-portal die is een handige manier om te bekijken en testen van de activiteiten van een API worden aangeroepen. In deze handleiding wordt stap roept u de methode voor **Bronrecords uitvoeren** die is toegevoegd aan de **AzureML Demo-API**. Klik op **Developer-portal** in het menu aan de bovenkant van de klassieke Portal.

![Developer-portal](./media/machine-learning-manage-web-service-endpoints-using-api-management/developer-portal.png)

**API's** op in het bovenste menu, en klik op **Demo-API AzureML** overzicht van de beschikbare bewerkingen.

![demoazureml-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/demoazureml-api.png)

**Bronrecords worden uitgevoerd** voor de bewerking selecteren. Klik op **Probeer het**.

![Try it](./media/machine-learning-manage-web-service-endpoints-using-api-management/try-it.png)

Parameters van de aanvraag, typt u uw **werkruimte**, **service**, **2.0** voor de **apiversion**en **de waarde true** voor de **details**. U vindt uw **werkruimte** en **service** in het dashboard van AzureML web service (Zie **Test de webservice** in bijlage A).

Aanvraagheaders, klikt u op **koptekst toevoegen** en **Content-Type** en **application/json**, en vervolgens klikt u op **koptekst toevoegen** en typ **vergunning** en **aan toonder <YOUR AZUREML SERVICE API-KEY> **. U vindt uw **api-sleutel** in het dashboard van AzureML web service (Zie **Test de webservice** in bijlage A).

Type **{"Inputs": {"input1": {"ColumnNames": ["Kol2"] 'waarden': [["Dit is een goede dag"]]}}, "GlobalParameters": {}}** voor het hoofdgedeelte van de aanvraag.

![azureml-demo-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/azureml-demo-api.png)

Klik op **verzenden**.

![Verzenden](./media/machine-learning-manage-web-service-endpoints-using-api-management/send.png)

Nadat een bewerking wordt gestart, geeft de developer-portal de **Aangevraagde URL** uit de back-end-service, de **Response-status**, de **antwoordheaders**en **inhoud van de reactie**.

![Response-status](./media/machine-learning-manage-web-service-endpoints-using-api-management/response-status.png)

##<a name="appendix-a---creating-and-testing-a-simple-azureml-web-service"></a>Bijlage A - maken en testen van een eenvoudige AzureML web service

###<a name="creating-the-experiment"></a>Het experiment maken

Hieronder volgen de stappen voor het maken van een eenvoudig experiment met AzureML en de implementatie daarvan als een webservice. De service wordt als een kolom met willekeurige tekst invoeren en retourneert een reeks functies die wordt weergegeven als gehele getallen. Bijvoorbeeld:

Tekst | Gecodeerde tekst
--- | ---
Dit is een goede dag | 1 1 2 2 0 2 0-1

Eerst via een browser van uw keuze, Ga naar: [https://studio.azureml.net/](https://studio.azureml.net/) en voer uw referenties aan te melden. Maak vervolgens een nieuwe lege experiment.

![zoeken-experiment-sjablonen](./media/machine-learning-manage-web-service-endpoints-using-api-management/search-experiment-templates.png)

Wijzig de naam in **SimpleFeatureHashingExperiment**. Vouw **Opgeslagen Datasets** en sleep **Boek recensies van Amazon** op uw experiment.

![eenvoudig-functie-hashing-experiment](./media/machine-learning-manage-web-service-endpoints-using-api-management/simple-feature-hashing-experiment.png)

Vouw **Data Transformation** en **manipulatie** en sleep **De kolommen selecteren in de Dataset** naar uw experiment. Verbinding maken met **beoordelingen van Amazon Book** **Selecteer kolommen in de Dataset**.

![Selecteer kolommen](./media/machine-learning-manage-web-service-endpoints-using-api-management/project-columns.png)

Klik op **Kolommen selecteren in de Dataset** en klikt u op de **kolomkiezer starten** en selecteer **Kol2**. Klik op het selectievakje Deze wijzigingen wilt toepassen.

![Selecteer kolommen](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-columns.png)

Vouw **Analytics tekst** en sleep **Hash-functie** op het experiment. Verbinding maken met **kolommen in de Dataset** **hash-functie**.

![verbinding maken met project kolommen](./media/machine-learning-manage-web-service-endpoints-using-api-management/connect-project-columns.png)

Typ **3** voor het **Hashing-bitsize**. Hiermee maakt u 8 (23) kolommen.

![hash-bitsize](./media/machine-learning-manage-web-service-endpoints-using-api-management/hashing-bitsize.png)

U wilt nu, klik op **uitvoeren** om het experiment te testen.

![uitvoeren](./media/machine-learning-manage-web-service-endpoints-using-api-management/run.png)

###<a name="create-a-web-service"></a>Maken van een webservice

Maak nu een webservice. **Web-Service** uitvouwen en sleep **Input** op uw experiment. Verbinding maken met **invoer** **hash-functie**. Ook **uitvoer** naar uw experiment slepen. Verbinding maken met **uitvoer** **hash-functie**.

![uitvoer-naar-functie-hashing](./media/machine-learning-manage-web-service-endpoints-using-api-management/output-to-feature-hashing.png)

Klik op **publiceren webservice**.

![publiceren web service](./media/machine-learning-manage-web-service-endpoints-using-api-management/publish-web-service.png)

Klik op **Ja** om het experiment te publiceren.

![Ja te publiceren](./media/machine-learning-manage-web-service-endpoints-using-api-management/yes-to-publish.png)

###<a name="test-the-web-service"></a>De web-service testen

Een webservice AzureML bestaat uit RSS (aanvraag/reactie-service) en BES (batch worden uitgevoerd) eindpunten. RSS is voor synchrone uitvoering. Er is een BES voor asynchrone uitvoering. Test uw webservice met de Python monsterbron hieronder, u wilt downloaden en installeren van de SDK Azure voor Python (Zie: [het installeren van Python](../python-how-to-install.md)).

Ook moet u de **werkruimte**, **service**- en **api_key** van het experiment voor de monsterbron hieronder. U kunt de werkruimte en de service vinden door te klikken op **Aanvraag/reactie** of **Batch worden uitgevoerd** voor uw experiment in het dashboard web service.

![zoeken-werkruimte-en-service](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-workspace-and-service.png)

De **api_key** kunt u vinden door te klikken op uw experiment in het dashboard web service.

![Zoek-api-sleutel](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-api-key.png)

####<a name="test-rrs-endpoint"></a>Test Bronrecords eindpunt

#####<a name="test-button"></a>Knop testen

Een eenvoudige manier om te testen het eindpunt Bronrecords is **testen** op het web servicedashboard op.

![Test](./media/machine-learning-manage-web-service-endpoints-using-api-management/test.png)

**Dit is een goede dag** voor **Kol2**typt. Klik op het vinkje.

![Voer gegevens](./media/machine-learning-manage-web-service-endpoints-using-api-management/enter-data.png)

Worden er ongeveer als volgt

![Voorbeeld van uitvoer](./media/machine-learning-manage-web-service-endpoints-using-api-management/sample-output.png)

#####<a name="sample-code"></a>Voorbeeld van Code

Een andere manier om te testen uw Bronrecords is van uw clientcode. Als u op **aanvraag/reactie** op het dashboard en blader naar de onderkant, ziet u een voorbeeld van code van C#, Python en R. Ook ziet u de syntaxis van het verzoek van Bronrecords, met inbegrip van de aanvraag-URI, koppen en hoofdtekst.

Deze handleiding bevat een werkende Python voorbeeld. U moet de **werkruimte**, **service**en **api_key** van het experiment.

    import urllib2
    import json
    workspace = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE WORKSPACE ID>"
    service = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE SERVICE ID>"
    api_key = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE API KEY>"
    data = {
    "Inputs": {
        "input1": {
            "ColumnNames": ["Col2"],
            "Values": [ [ "This is a good day" ] ]
        },
    },
    "GlobalParameters": { }
    }
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace + "/services/" + service + "/execute?api-version=2.0&details=true"
    headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
    body = str.encode(json.dumps(data))
    try:
        req = urllib2.Request(url, body, headers)
        response = urllib2.urlopen(req)
        result = response.read()
        print "result:" + result
            except urllib2.HTTPError, error:
        print("The request failed with status code: " + str(error.code))
        print(error.info())
        print(json.loads(error.read()))

####<a name="test-bes-endpoint"></a>Test BES eindpunt
Klik op **Batch worden uitgevoerd** op het dashboard en blader naar de onderkant. Ziet u een voorbeeld van code van C#, Python en R. Ook ziet u de syntaxis van de BES-aanvragen voor het indienen van een taak, een taak starten de status of de resultaten van een taak ophalen en verwijderen van een taak.

Deze handleiding bevat een werkende Python voorbeeld. U moet de **werkruimte**, **service**en **api_key** van het experiment. Bovendien moet u de **accountnaam van de opslag**, **opslag account sleutel**en **opslag containernaam**wijzigen. Ten slotte moet u de locatie van het **invoerbestand** en de locatie van het **uitvoerbestand**te wijzigen.

    import urllib2
    import json
    import time
    from azure.storage import *
    workspace = "<REPLACE WITH YOUR WORKSPACE ID>"
    service = "<REPLACE WITH YOUR SERVICE ID>"
    api_key = "<REPLACE WITH THE API KEY FOR YOUR WEB SERVICE>"
    storage_account_name = "<REPLACE WITH YOUR AZURE STORAGE ACCOUNT NAME>"
    storage_account_key = "<REPLACE WITH YOUR AZURE STORAGE KEY>"
    storage_container_name = "<REPLACE WITH YOUR AZURE STORAGE CONTAINER NAME>"
    input_file = "<REPLACE WITH THE LOCATION OF YOUR INPUT FILE>" # Example: C:\\mydata.csv
    output_file = "<REPLACE WITH THE LOCATION OF YOUR OUTPUT FILE>" # Example: C:\\myresults.csv
    input_blob_name = "mydatablob.csv"
    output_blob_name = "myresultsblob.csv"
    def printHttpError(httpError):
    print("The request failed with status code: " + str(httpError.code))
    print(httpError.info())
    print(json.loads(httpError.read()))
    return
    def saveBlobToFile(blobUrl, resultsLabel):
    print("Reading the result from " + blobUrl)
    try:
        response = urllib2.urlopen(blobUrl)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    with open(output_file, "w+") as f:
        f.write(response.read())
    print(resultsLabel + " have been written to the file " + output_file)
    return
    def processResults(result):
    first = True
    results = result["Results"]
    for outputName in results:
        result_blob_location = results[outputName]
        sas_token = result_blob_location["SasBlobToken"]
        base_url = result_blob_location["BaseLocation"]
        relative_url = result_blob_location["RelativeLocation"]
        print("The results for " + outputName + " are available at the following Azure Storage location:")
        print("BaseLocation: " + base_url)
        print("RelativeLocation: " + relative_url)
        print("SasBlobToken: " + sas_token)
        if (first):
            first = False
            url3 = base_url + relative_url + sas_token
            saveBlobToFile(url3, "The results for " + outputName)
    return

    def invokeBatchExecutionService():
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace +"/services/" + service +"/jobs"
    blob_service = BlobService(account_name=storage_account_name, account_key=storage_account_key)
    print("Uploading the input to blob storage...")
    data_to_upload = open(input_file, "r").read()
    blob_service.put_blob(storage_container_name, input_blob_name, data_to_upload, x_ms_blob_type="BlockBlob")
    print "Uploaded the input to blob storage"
    input_blob_path = "/" + storage_container_name + "/" + input_blob_name
    connection_string = "DefaultEndpointsProtocol=https;AccountName=" + storage_account_name + ";AccountKey=" + storage_account_key
    payload =  {
        "Input": {
            "ConnectionString": connection_string,
            "RelativeLocation": input_blob_path
        },
        "Outputs": {
            "output1": { "ConnectionString": connection_string, "RelativeLocation": "/" + storage_container_name + "/" + output_blob_name },
        },
        "GlobalParameters": {
        }
    }
        body = str.encode(json.dumps(payload))
    headers = { "Content-Type":"application/json", "Authorization":("Bearer " + api_key)}
    print("Submitting the job...")
    # submit the job
    req = urllib2.Request(url + "?api-version=2.0", body, headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    result = response.read()
    job_id = result[1:-1] # remove the enclosing double-quotes
    print("Job ID: " + job_id)
    # start the job
    print("Starting the job...")
    req = urllib2.Request(url + "/" + job_id + "/start?api-version=2.0", "", headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    url2 = url + "/" + job_id + "?api-version=2.0"

    while True:
        print("Checking the job status...")
        # If you are using Python 3+, replace urllib2 with urllib.request in the follwing code
        req = urllib2.Request(url2, headers = { "Authorization":("Bearer " + api_key) })
        try:
            response = urllib2.urlopen(req)
        except urllib2.HTTPError, error:
            printHttpError(error)
            return
        result = json.loads(response.read())
        status = result["StatusCode"]
        print "status:" + status
        if (status == 0 or status == "NotStarted"):
            print("Job " + job_id + " not yet started...")
        elif (status == 1 or status == "Running"):
            print("Job " + job_id + " running...")
        elif (status == 2 or status == "Failed"):
            print("Job " + job_id + " failed!")
            print("Error details: " + result["Details"])
            break
        elif (status == 3 or status == "Cancelled"):
            print("Job " + job_id + " cancelled!")
            break
        elif (status == 4 or status == "Finished"):
            print("Job " + job_id + " finished!")
            processResults(result)
            break
        time.sleep(1) # wait one second
    return
    invokeBatchExecutionService()
