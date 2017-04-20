<properties 
    pageTitle="Wetenschap van de gegevens op de Linux wetenschap virtuele Machine gegevens | Microsoft Azure" 
    description="Enkele algemene gegevens wetenschap hoe taken kunt uitvoeren met de Linux Data Science VM." 
    services="machine-learning"
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev;paulsh" />


# <a name="data-science-on-the-linux-data-science-virtual-machine"></a>Wetenschap van de gegevens op de Linux Data Science virtuele Machine

In dit scenario wordt beschreven hoe u enkele algemene gegevens wetenschap taken met Linux Data Science VM uit te voeren. Linux Data Science Virtual Machine (DSVM) is een virtuele machine afbeelding beschikbaar op Azure die vooraf is geïnstalleerd met een verzameling hulpprogramma's die vaak worden gebruikt voor de analyse van gegevens en machine learning. De belangrijkste software-componenten zijn in het onderwerp van de [bepaling van de virtuele Machine van Linux Data Science](machine-learning-data-science-linux-dsvm-intro.md) gespecificeerd. De VM-afbeelding eenvoudig aan de slag data science in minuten, zonder te hoeven installeren en configureren van elk van de programma's afzonderlijk doen. U kunt eenvoudig de VM, schaalbaar en indien nodig en stopt wanneer niet in gebruik. Deze bron is elastisch en rendabele. 

De taken van de wetenschappelijke gegevens is aangetoond in dit scenario voert u de stappen in de [Procedure voor het Team gegevens wetenschap](https://azure.microsoft.com/documentation/learning-paths/data-science-process/). Dit proces biedt een systematische aanpak voor wetenschappelijke gegevens waardoor teams van wetenschappers gegevens effectief samenwerken gedurende de levenscyclus van het maken van intelligente toepassingen. Het proces van de wetenschap gegevens biedt ook een iteratieve kader voor wetenschappelijke gegevens die kan worden gevolgd door een persoon.

We analyseren de dataset [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) in dit overzicht. Dit is een verzameling van e-mails die zijn gemarkeerd als spam of ham (wat betekent dat zij niet spam zijn), en bevat ook enkele statistieken over de inhoud van de e-mailberichten. De statistieken die zijn opgenomen worden in de volgende maar één sectie besproken. 


## <a name="prerequisites"></a>Vereisten

Voordat u een virtuele Machine voor Linux Data Science gebruiken kunt, hebt u het volgende:

- Een **abonnement op Azure**. Als u nog geen een, Zie [vandaag nog uw gratis Azure account maken](https://azure.microsoft.com/free/).
- Een [**Linux data science VM**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm). Zie voor informatie over het inrichten van deze VM, [dient de virtuele Machine van Linux Data Science](machine-learning-data-science-linux-dsvm-intro.md). 
- [X2Go](http://wiki.x2go.org/doku.php) op uw computer is geïnstalleerd en een XFCE-sessie geopend. Zie voor meer informatie over het installeren en configureren van een **X2Go client** [installeren en configureren van X2Go client](machine-learning-data-science-linux-dsvm-intro.md#Installing-and-configuring-X2Go-client). 
- Een **AzureML account**. Als u nog geen hebt, meld u aan op de [homepage van de AzureML](https://studio.azureml.net/)voor nieuwe. Er is een gratis gebruik laag kunt u aan de slag.


## <a name="download-the-spambase-dataset"></a>De dataset spambase downloaden

De dataset [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) is een relatief kleine set gegevens die alleen 4601 voorbeelden bevat. Dit is een handige grootte moet worden gebruikt om aan te tonen dat enkele van de belangrijkste functies van de VM gegevens wetenschap die houdt de resourcevereisten bescheiden.

>[AZURE.NOTE] In dit scenario is gemaakt op een D2 v2-formaat Linux Data Science virtuele Machine. De DSVM van deze omvang is kan verwerken met de procedures in dit scenario.

Als u meer opslagruimte nodig hebt, kunt u extra schijven maken en deze koppelen aan uw VM. Deze schijven gebruiken Azure voor permanente opslag, zodat hun gegevens behouden blijft wanneer de server is door de service vanwege het formaat of is afgesloten. Een schijf toevoegen en koppelen aan uw VM, volg de instructies in [een schijf naar een Linux VM toevoegen](../virtual-machines/virtual-machines-linux-add-disk.md). Deze stappen gebruiken de Azure opdrachtregelinterface (CLI van Azure), die al is geïnstalleerd op de DSVM. Deze procedures kunnen dus helemaal uit de VM zelf worden uitgevoerd. Een andere mogelijkheid om te vergroten opslag is [Azure bestanden](../storage/storage-how-to-use-files-linux.md)te gebruiken.

Downloaden van de gegevens, een terminalvenster te openen en uitvoeren van deze opdracht:

    wget http://archive.ics.uci.edu/ml/machine-learning-databases/spambase/spambase.data

Het gedownloade bestand heeft geen rij met kolomkoppen staat, dus laten we een ander bestand dat beschikt over een koptekst maken. Deze opdracht uit te maken van een bestand met de juiste headers:

    echo 'word_freq_make, word_freq_address, word_freq_all, word_freq_3d,word_freq_our, word_freq_over, word_freq_remove, word_freq_internet,word_freq_order, word_freq_mail, word_freq_receive, word_freq_will,word_freq_people, word_freq_report, word_freq_addresses, word_freq_free,word_freq_business, word_freq_email, word_freq_you, word_freq_credit,word_freq_your, word_freq_font, word_freq_000, word_freq_money,word_freq_hp, word_freq_hpl, word_freq_george, word_freq_650, word_freq_lab,word_freq_labs, word_freq_telnet, word_freq_857, word_freq_data,word_freq_415, word_freq_85, word_freq_technology, word_freq_1999,word_freq_parts, word_freq_pm, word_freq_direct, word_freq_cs, word_freq_meeting,word_freq_original, word_freq_project, word_freq_re, word_freq_edu,word_freq_table, word_freq_conference, char_freq_semicolon, char_freq_leftParen,char_freq_leftBracket, char_freq_exclamation, char_freq_dollar, char_freq_pound, capital_run_length_average,capital_run_length_longest, capital_run_length_total, spam' > headers

De twee bestanden met de opdracht vervolgens samenvoegen:

    cat spambase.data >> headers
    mv headers spambaseHeaders.data

De dataset heeft verschillende soorten statistieken voor elk e-mailadres: 

- Kolommen, zoals ***word\_freq\_WORD*** geven het percentage van de woorden in de e-mail die overeenkomen met *WORD*. Bijvoorbeeld als *word\_freq\_u* 1, is dan 1% van alle woorden in het e-mailbericht *maken zijn*. 
- Kolommen, zoals ***char\_freq\_CHAR*** geven het percentage van alle tekens in het e-mailbericht *CHAR zijn*. 
- ***kapitaal\_uitvoeren\_lengte\_langste*** is de langste duur van een opeenvolging van hoofdletters. 
- ***kapitaal\_uitvoeren\_lengte\_gemiddelde*** is de gemiddelde lengte van alle reeksen van hoofdletters. 
- ***kapitaal\_uitvoeren\_lengte\_totale*** is de totale lengte van alle reeksen van hoofdletters. 
- ***spam*** wordt aangegeven of de e-mail werd beschouwd als spam of niet (1 = spam, 0 = niet als spam).


## <a name="explore-the-dataset-with-microsoft-r-open"></a>Verken de dataset met Microsoft R openen

Laten we de gegevens controleren en voer enkele elementaire computer leren met R. De wetenschap gegevens VM wordt geleverd met [Microsoft R Open](https://mran.revolutionanalytics.com/open/) vooraf geïnstalleerd. De multithreaded mathematische bibliotheken in deze versie van R bieden betere prestaties dan verschillende single thread-versies. Microsoft R Open biedt ook reproduceerbaarheid door een momentopname van de opslagplaats CRAN-pakket.

Als u kopieën van de codevoorbeelden in dit scenario gebruikt, de **Azure-Machine-Learning-gegevens-wetenschap** opslagplaats voor git, die vooraf is geïnstalleerd op de VM te klonen. Uitvoeren vanaf de opdrachtregel git:

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

Open een terminal-venster en een nieuwe R-sessie starten met de R interactieve console.

>[AZURE.NOTE] U kunt ook RStudio gebruiken voor de volgende procedures. RStudio installeren, uitvoeren van deze opdracht op een terminal:`./Desktop/DSVM\ tools/installRStudio.sh`

U importeert de gegevens en de omgeving instellen, uitvoeren:

    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

Overzichtsstatistieken over elke kolom weergeven:

    summary(data)

Voor een andere weergave van de gegevens:

    str(data)

Dit ziet u het type van elke variabele en de eerste paar waarden in de gegevensset. 

De kolom *spam* werd gelezen als een geheel getal, maar het is eigenlijk een bestaan variabele (of factor). Het type instellen:

    data$spam <- as.factor(data$spam)

Een verkennende analyse, gebruikt u het pakket [ggplot2](http://ggplot2.org/) , een populaire grafische bibliotheek voor R op de VM al is geïnstalleerd. Opmerking van samenvattingsgegevens dat eerder is weergegeven, hebben we samenvattingsgegevens over de frequentie van het uitroepteken teken. We brengen deze frequentie met de volgende opdrachten:

    library(ggplot2)
    ggplot(data) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Aangezien de nul-balk het waarnemingspunt scheeftrekken is, we verwijderen deze:

    email_with_exclamation = data[data$char_freq_exclamation > 0, ]
    ggplot(email_with_exclamation) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Er is een niet-triviale dichtheid dan 1 die er interessant uitziet. Bekijk alleen die gegevens:

    ggplot(data[data$char_freq_exclamation > 1, ]) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Vervolgens splitsen door spam vs ham:

    ggplot(data[data$char_freq_exclamation > 1, ], aes(x=char_freq_exclamation)) +
    geom_density(lty=3) +
    geom_density(aes(fill=spam, colour=spam), alpha=0.55) +
    xlab("spam") +
    ggtitle("Distribution of spam \nby frequency of !") +
    labs(fill="spam", y="Density")

In deze voorbeelden moeten u de vergelijkbare percelen van de andere kolommen te verkennen van de gegevens in deze inschakelen.


## <a name="train-and-test-an-ml-model"></a>Trainen en testen van een model ML

Nu gaan we enkele machine learning modellen voor het classificeren van de e-mailberichten in de dataset als reeks of ham met trainen. We trainen een besluit boom-model en een willekeurige forest-model in deze sectie en test u de nauwkeurigheid van de voorspellingen. 

>[AZURE.NOTE] Het pakket rpart (recursieve partitionering en bomen regressie) is gebruikt in de volgende code is al geïnstalleerd op de VM Data Science.


Eerst gaan we de dataset splitsen in opleiding en de proef wordt:

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

En maak vervolgens een beslissingsstructuur te classificeren van de e-mailberichten.

    require(rpart)
    model.rpart <- rpart(spam ~ ., method = "class", data = trainSet)
    plot(model.rpart)
    text(model.rpart)

Hier is het resultaat:

![1](./media/machine-learning-data-science-linux-dsvm-walkthrough/decision-tree.png)

Om te bepalen hoe goed wordt uitgevoerd op de set van de opleiding, de volgende code gebruiken:

    trainSetPred <- predict(model.rpart, newdata = trainSet, type = "class")
    t <- table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Om te bepalen hoe goed worden uitgevoerd op de test instellen:

    testSetPred <- predict(model.rpart, newdata = testSet, type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Ook maar eens een willekeurig forest-model. Willekeurige forests een heleboel beslissingsstructuren trainen en uitvoeren van een klasse die de modus van de classificaties van alle bomen afzonderlijk besluit. Ze bieden een krachtigere computer aanpak te leren als ze de neiging van de boomstructuur van een besluit tot een dataset training overfit corrigeren. 

    require(randomForest)
    trainVars <- setdiff(colnames(data), 'spam')
    model.rf <- randomForest(x=trainSet[, trainVars], y=trainSet$spam)

    trainSetPred <- predict(model.rf, newdata = trainSet[, trainVars], type = "class")
    table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)

    testSetPred <- predict(model.rf, newdata = testSet[, trainVars], type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy


## <a name="deploy-a-model-to-azure-ml"></a>Implementeren van een model voor Azure ML

[Azure Machine Learning Studio](https://studio.azureml.net/) (AzureML) is een cloud-service waarmee u gemakkelijk kunnen bouwen en implementeren van analytics voorspellende modellen. Een van de mooie functies van AzureML is de mogelijkheid voor het publiceren van een functie van R als webservice. Het pakket AzureML R implementatie een fluitje van onze R-sessie op de DSVM rechts. 

Voor de implementatie van het besluit boom code uit de vorige sectie, moet u zich aanmelden bij Azure Machine Learning Studio. U moet uw werkruimte-ID en een Autorisatietoken aan sigh in. Deze waarden zoeken en de AzureML variabelen initialiseren:

Selecteer **Instellingen** in het menu links. Opmerking de **ID van de WERKRUIMTE**. ![2](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-id.png)

**Vergunning Tokens** selecteert in het menu overhead en Let op uw **Primaire vergunning Token**. ![3](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-token.png)

Het pakket **AzureML** laden en stelt u de waarden van de variabelen met de ID-token en de werkruimte in uw sessie R op de DSVM:


    require(AzureML)
    wsAuth = "<authorization-token>"
    wsID = "<workspace-id>"


We vereenvoudigen het model om deze demo met eenvoudiger te implementeren. De drie variabelen selecteren in de beslissingsstructuur van het dichtst bij de hoofdmap en bouwen van een nieuwe structuur met alleen de drie variabelen:

    colNames <- c("char_freq_dollar", "word_freq_remove", "word_freq_hp", "spam")
    smallTrainSet <- trainSet[, colNames]
    smallTestSet <- testSet[, colNames]
    model.rpart <- rpart(spam ~ ., method = "class", data = smallTrainSet)

We moeten een voorspellingsfunctie die ervoor zorgt dat de functies als invoer de voorspelde waarden als resultaat:

    predictSpam <- function(char_freq_dollar, word_freq_remove, word_freq_hp) {
        predictDF <- predict(model.rpart, data.frame("char_freq_dollar" = char_freq_dollar,
        "word_freq_remove" = word_freq_remove, "word_freq_hp" = word_freq_hp))
        return(colnames(predictDF)[apply(predictDF, 1, which.max)])
    }

De functie predictSpam publiceren op AzureML met de functie **publishWebService** : 

    spamWebService <- publishWebService("predictSpam",
        "spamWebService",
        list("char_freq_dollar"="float", "word_freq_remove"="float","word_freq_hp"="float"),
        list("spam"="int"),
        wsID, wsAuth)

Deze functie wordt de functie **predictSpam** , maakt u een webservice met de naam **spamWebService** met gedefinieerde ingangen en uitgangen en retourneert informatie over het nieuwe endpoint.

Details weergeven van de gepubliceerde webservice, met inbegrip van het eindpunt van de API en de toegangstoetsen met de opdracht:

    spamWebService[[2]]

Uitproberen op de eerste 10 rijen van de test instellen:

    consumeDataframe(spamWebService$endpoints[[1]]$PrimaryKey, spamWebService$endpoints[[1]]$ApiLocation, smallTestSet[1:10, 1:3])


## <a name="use-other-tools-available"></a>Andere hulpprogramma's gebruiken

De overige secties tonen het gebruik van de hulpprogramma's geïnstalleerd op de VM Linux Data Science. Hier is de lijst van hulpprogramma's beschreven:

- XGBoost
- Python
- Jupyterhub
- Rammelaar
- PostgreSQL & eekhoorn SQL
- SQL Server-datawarehouse


## <a name="xgboost"></a>XGBoost

[XGBoost](https://xgboost.readthedocs.org/en/latest/) is een hulpprogramma waarmee u een snelle en nauwkeurige gestimuleerd tree-implementatie.

    require(xgboost)
    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

    bst <- xgboost(data = data.matrix(trainSet[,0:57]), label = trainSet$spam, nthread = 2, nrounds = 2, objective = "binary:logistic")

    pred <- predict(bst, data.matrix(testSet[, 0:57]))
    accuracy <- 1.0 - mean(as.numeric(pred > 0.5) != testSet$spam)
    print(paste("test accuracy = ", accuracy))

XGBoost kunnen ook aanroepen vanuit python of een opdrachtregel.

## <a name="python"></a>Python

Voor de ontwikkeling van het gebruik van Python, zijn de Anaconda, Python verdelingen 2.7 en 3.5 geïnstalleerd in de DSVM. 

>[AZURE.NOTE] De verdeling van de Anaconda omvat [Condas](http://conda.pydata.org/docs/index.html), die kan worden gebruikt voor het maken van aangepaste omgevingen voor Python met verschillende versies en/of geïnstalleerd in deze pakketten.

We lezen in sommige van de dataset spambase en classificeren van de e-mails met support vector machines in de scikit-informatie:

    import pandas
    from sklearn import svm    
    data = pandas.read_csv("spambaseHeaders.data", sep = ',\s*')
    X = data.ix[:, 0:57]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

Voorspellingen maken:

    clf.predict(X.ix[0:20, :])

Om te zien hoe een eindpunt AzureML publiceren, we maken een eenvoudiger model drie variabelen zoals toen we het model R eerder gepubliceerd. 

    X = data.ix[["char_freq_dollar", "word_freq_remove", "word_freq_hp"]]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

Het model op AzureML publiceren:

    # Publish the model.
    workspace_id = "<workspace-id>"
    workspace_token = "<workspace-token>"
    from azureml import services
    @services.publish(workspace_id, workspace_token)
    @services.types(char_freq_dollar = float, word_freq_remove = float, word_freq_hp = float)
    @services.returns(int) # 0 or 1
    def predictSpam(char_freq_dollar, word_freq_remove, word_freq_hp):
        inputArray = [char_freq_dollar, word_freq_remove, word_freq_hp]
        return clf.predict(inputArray)

    # Get some info about the resulting model.
    predictSpam.service.url
    predictSpam.service.api_key

    # Call the model
    predictSpam.service(1, 1, 1)

>[AZURE.NOTE] Dit is alleen beschikbaar voor python 2.7 en nog niet wordt ondersteund op 3.5. Uitgevoerd met **/anaconda/bin/python2.7**.


## <a name="jupyterhub"></a>Jupyterhub

De verdeling van de Anaconda in de DSVM wordt geleverd met een laptop Jupyter, een omgeving met meerdere platforms Python, R-of Julia en analyse te delen. De Jupyter-laptop is toegankelijk via JupyterHub. U zich aanmeldt met uw lokale Linux-gebruikersnaam en wachtwoord bij ***https://\<VM DNS-naam of het IP-adres\>: 8000 /***. Alle configuratiebestanden voor JupyterHub vindt u in de map **/etc/jupyterhub**.

Verschillende sample-laptops zijn al geïnstalleerd op de VM:

- Zie de [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) voor een laptop monster Python.
- Zie [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) voor een voorbeeld **R** -laptop.
- Zie de [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) voor een ander monster **Python** notitieblok.

>[AZURE.NOTE] De taal van Julia is ook beschikbaar op de Linux Data Science VM vanaf de opdrachtregel.


## <a name="rattle"></a>Rammelaar

[Rattle](https://cran.r-project.org/web/packages/rattle/index.html) (de R analytische Tool om informatie gemakkelijk) is een grafisch hulpprogramma R voor datamining. Het heeft een intuïtieve interface waarmee u gemakkelijk laden, verkennen, en gegevens transformeren en samenstellen en evalueren van modellen.  Artikel [Rattle: A Data Mining GUI voor R](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) biedt een scenario dat de beschikbare functies toont.

Installeer en start Rammelaar met de volgende opdrachten:

    if(!require("rattle")) install.packages("rattle")
    require(rattle)
    rattle()

>[AZURE.NOTE] Installatie is niet vereist voor de DSVM. Maar Rammelaar gevraagd u extra pakketten installeren wanneer deze wordt geladen.

Rammelaar maakt gebruik van een interface op basis van het tabblad. De meeste van de tabs komen overeen met de stappen in de [Procedure voor wetenschappelijke gegevens](https://azure.microsoft.com/documentation/learning-paths/data-science-process/), zoals het laden van gegevens of ermee. Het proces van de wetenschap gegevens loopt van links naar rechts door de tabbladen. Maar het laatste tabblad bevat een logboek van de R-opdrachten worden uitgevoerd door Rammelaar. 


Laden en de dataset configureren:

- Voor het laden van het bestand, selecteer het tabblad **gegevens** , klik 
- Kies de selector naast de **bestandsnaam** in en kies **spambaseHeaders.data**. 
- Het bestand wordt geladen. **uitvoeren** in de bovenste rij knoppen selecteren. Hier ziet u een overzicht van elke kolom, met inbegrip van het opgegeven gegevenstype, invoer, een doel of ander type van de variabele en het aantal unieke waarden.
- Rammelaar heeft de kolom **spam** correct geïdentificeerd als het doel. Selecteer de kolom spam en het **Doel het gegevenstype** ingesteld op **Categoric**.

De gegevens verkennen: 

- Selecteer het tabblad **verkennen** . 
- Klik op **Overzicht**en vervolgens **uitvoeren**, als enige informatie over de typen variabelen en sommige samenvattingsgegevens wilt zien. 
- Als u wilt weergeven op andere typen statistische gegevens over elke variabele, selecteer andere opties zoals **Beschrijf** of **Basisbeginselen**.

Het tabblad **verkennen** kunt u veel begrijpelijke manier mee waarnemingspunten genereren. Een histogram van de gegevens uitzetten:


- Selecteer **verdelingen**.
- **Histogram** voor **word_freq_remove** en **word_freq_you**controleren.
- Selecteer **uitvoeren**. Hier ziet u zowel waarnemingspunten dichtheid in een enkele graph-venster, waarbij duidelijk is dat het woord "u" staat veel vaker op e-mailberichten dan "verwijderen".

De waarnemingspunten correlatie zijn ook interessant. Een te maken:


- Vervolgens kiest u **correlatie** als het **Type** 
- Selecteer **uitvoeren**. 
- Rammelaar waarschuwt u raadt een maximum van 40 variabelen. Selecteer **Ja** om de grafiek weer te geven. 

Er zijn enkele interessante correlaties die afkomstig van zijn: 'technology' is sterk gecorreleerd aan "HP" en "labs", bijvoorbeeld. Het is ook sterk gecorreleerd aan '650', omdat het netnummer van de dataset donors 650.

De numerieke waarden van de correlaties tussen woorden zijn beschikbaar in het venster verkennen. Opmerking, bijvoorbeeld dat "technologie" negatief gerelateerd is aan "uw" en "money" interessant is.

Rammelaar kunt transformeren de dataset verwerken van enkele veelvoorkomende problemen besproken. Bijvoorbeeld, kunt u de oorspronkelijke functies, ontbrekende waarden worden toegerekend, uitschieters verwerken en verwijderen van variabelen of metingen met ontbrekende gegevens. Rammelaar kan ook regels koppeling tussen opmerkingen en/of variabelen worden aangegeven. Deze tabbladen zijn buiten bereik voor dit scenario inleidende.

Rammelaar kan ook cluster-analyse uitvoeren. Laten we enkele functies om de uitvoer gemakkelijker worden uitgesloten. Kies op het tabblad **gegevens** **negeren** naast elk van de variabelen, met uitzondering van deze tien items:

- word_freq_hp
- word_freq_technology
- word_freq_george
- word_freq_remove
- word_freq_your
- word_freq_dollar
- word_freq_money
- capital_run_length_longest
- word_freq_business
- spam

Ga terug naar het tabblad **Cluster** Kies **KMeans**en het *aantal clusters* op 4 ingesteld. Vervolgens **uitvoeren**. De resultaten worden weergegeven in het uitvoervenster. Een cluster hoge frequentie van "george" en "hp" heeft en is waarschijnlijk een legitieme zakelijke e-mailadres.

Een eenvoudige beschikking machine learning boomstructuur maken: 

- Selecteer het tabblad **Model** 
- Kies **structuur** als het **Type**. 
- Selecteer **uitvoeren** om de structuur in de tekstvorm in het uitvoervenster weer te geven. 
- Klik op de knop **Tekenen** om een grafische versie weer te geven. Dit lijkt lijkt veel op de structuur die we eerder met *rpart*verkregen.

Een van de mooie functies van Rammelaar is de mogelijkheid voor het uitvoeren van verschillende methoden van machine learning en ze snel te evalueren. Hier volgt de procedure:

- Kies voor het **Type** **Alles** . 
- Selecteer **uitvoeren**. 
- Nadat het klaar is kunt u op een enkel **Type**, zoals **SVM**, en de resultaten bekijken. 
- U kunt ook de prestaties van de modellen op de validatie instellen via het tabblad **evalueren** vergelijken. Bijvoorbeeld geeft de selectie **Fout Matrix** de matrix van verwarring, algemene fout en gemiddelde klasse fout voor elk model voor de validatie-set. 
- U kunt ook ROC curven tekenen, gevoeligheidsanalyse, en andere soorten evaluaties model doen.

Zodra u klaar bent met het maken van modellen, selecteer het tabblad **logboek** weer te geven van de R-code uitvoeren door Rammelaar tijdens uw sessie. U kunt de knop **exporteren** op te slaan. 

>[AZURE.NOTE] Er is een fout in de huidige versie van de ratel. Wijzig het script of het later uw stappen herhalen gebruiken, moet u een #-teken voor het *exporteren van dit logboek...* invoegen in de tekst van het logboek. 


## <a name="postgresql--squirrel-sql"></a>PostgreSQL & eekhoorn SQL

De DSVM wordt geleverd met PostgreSQL geïnstalleerd. PostgreSQL is een geavanceerde, open source relationele database. In dit gedeelte ziet u hoe onze spam dataset laden in PostgreSQL en vervolgens deze query.

Voordat u de gegevens laden kunt, moet u de wachtwoordverificatie van de localhost toestaan. Bij een opdrachtprompt:

    sudo gedit /var/lib/pgsql/data/pg_hba.conf

Aan de onderkant van het config-bestand worden meerdere regels waarin de toegestane verbindingen:

    # "local" is for Unix domain socket connections only
    local   all             all                                     trust
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            ident
    # IPv6 local connections:
    host    all             all             ::1/128                 ident

Wijzig de regel 'Lokale IPv4-verbindingen' gebruik van md5 in plaats van ident, zodat we kunnen aanmelden met een gebruikersnaam en wachtwoord:

    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5

Service en start de postgres:

    sudo systemctl restart postgresql

Om psql starten, een interactieve terminal voor PostgreSQL als de gebruiker ingebouwde postgres, de volgende opdracht uitvoeren vanaf een opdrachtprompt:

    sudo -u postgres psql

Maak een nieuwe gebruikersaccount met dezelfde gebruikersnaam als u momenteel bent aangemeld als en een wachtwoord geven het Linux-account:

    CREATE USER <username> WITH CREATEDB;
    CREATE DATABASE <username>;
    ALTER USER <username> password '<password>';
    \quit

Meld u vervolgens aan met psql als de gebruiker:

    psql

En de gegevens in een nieuwe database te importeren:

    CREATE DATABASE spam;
    \c spam
    CREATE TABLE data (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer);
    \copy data FROM /home/<username>/spambase.data DELIMITER ',' CSV;
    \quit

Nu kijken de gegevens en sommige query's met behulp van **SQL eekhoorn**, een grafisch hulpprogramma waarmee u kunt met databases via een JDBC-stuurprogramma werken.

Om te beginnen starten eekhoorn SQL in het menu toepassingen. Het stuurprogramma instellen:

- Selecteer **Windows**, klik op **stuurprogramma's weergeven**. 
- Klik met de rechtermuisknop op de **PostgreSQL** en selecteer **Stuurprogramma wijzigen**. 
- Kies **Extra klassepad** **toevoegen**. 
- ***/Usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** voor de **bestandsnaam** invoeren en 
- Selecteer **openen**.
- Kies lijst met stuurprogramma's, en vervolgens selecteert u in **Naam van de klasse** **org.postgresql.Driver** en selecteer **OK**.

De verbinding naar de lokale server instellen:
 
- Selecteer vervolgens **Windows** **aliassen weergeven.** 
- Kies de **+** knop om een nieuwe alias. 
- *Spam database*naam, **PostgreSQL** kiezen in de vervolgkeuzelijst **stuurprogramma** .
- De URL ingesteld op *jdbc:postgresql://localhost/spam*. 
- Voer uw *gebruikersnaam* en *wachtwoord*. 
- Klik op **OK**. 
- Open het venster **verbinding** , dubbelklik op de ***Spam database*** -alias. 
- Selecteer **verbinding maken**.

Sommige query's worden uitgevoerd:

- Klik op het tabblad **SQL** .
- Voer een eenvoudige query zoals `SELECT * from data;` in de textbox boven aan het tabblad SQL query. 
- Druk op **Ctrl-Enter** uit te voeren. Eekhoorn SQL wordt standaard de eerste 100 rijen uit uw query geretourneerd. 

Er zijn veel meer query's die u kunt uitvoeren om deze gegevens te verkennen. Bijvoorbeeld, hoe is de frequentie van het woord *maken* het verschil tussen spam en ham?

    SELECT avg(word_freq_make), spam from data group by spam;

Of wat zijn de kenmerken van e-mail, vaak met *3d*?

    SELECT * from data order by word_freq_3d desc;

De meeste e-mailberichten met een hoge exemplaar van *3d* zijn blijkbaar spam, is het mogelijk een handige functie voor het bouwen van een voorspellende model voor het classificeren van de e-mailberichten.

Als u leren machine uitvoeren met de gegevens die zijn opgeslagen in een PostgreSQL database wilt, kunt u overwegen [MADlib](http://madlib.incubator.apache.org/).

## <a name="sql-server-data-warehouse"></a>SQL Server-datawarehouse

Azure SQL Data Warehouse is een cloud-gebaseerde, schalen database kunnen grote hoeveelheden relationele en niet-relationele gegevens verwerken. Zie voor meer informatie [Wat is Azure SQL Data Warehouse?](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)

Verbinding maken met het datawarehouse en de tabel maken, uitvoeren met de volgende opdracht uit vanaf een opdrachtprompt:

    sqlcmd -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -I

Typ achter de prompt sqlcmd:

    CREATE TABLE spam (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer) WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
    GO

Gegevens kopiëren met bcp:

    bcp spam in spambaseHeaders.data -q -c -t  ',' -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -F 1 -r "\r\n"

>[AZURE.NOTE] De regeleinden in het bestand Windows-stijl zijn, maar bcp verwacht UNIX-stijl, dus we moeten zien bcp of met de vlag - r.

En een query met sqlcmd:

    select top 10 spam, char_freq_dollar from spam;
    GO

U kunt ook zoeken met eekhoorn SQL. Voer dezelfde stappen uit voor PostgreSQL, met behulp van het Microsoft MSSQL Server JDBC-stuurprogramma, dat kan worden gevonden in ***/usr/share/java/jdbcdrivers/sqljdbc42.jar***.

## <a name="next-steps"></a>Volgende stappen

Zie voor een overzicht van de onderwerpen die u stapsgewijs door de taken waaruit het proces gegevens wetenschap in Azure, [Team gegevens wetenschap proces](http://aka.ms/datascienceprocess).

Zie voor een beschrijving van andere end-to-end scenario's die aantonen de stappen in het proces van het Team gegevens wetenschap voor specifieke scenario's dat [Team gegevens wetenschap proces-scenario's](data-science-process-walkthroughs.md). De scenario's illustreren ook gereedschappen Wolk en op ruimten en diensten te combineren in een workflow of een pijpleiding een intelligente toepassing maken.

