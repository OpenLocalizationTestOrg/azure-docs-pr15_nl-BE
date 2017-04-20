<properties 
    pageTitle="Analyse van de overleving met Azure Machine Learning | Microsoft Azure" 
    description="Kans op overleving analyse gebeurtenis voorkomt" 
    services="machine-learning" 
    documentationCenter="" 
    authors="zhangya" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="zhangya"/> 


#<a name="survival-analysis"></a>Analyse van de overleving 

In veel situaties is de belangrijkste uitkomst bij de beoordeling van de tijd op een gebeurtenis die van belang. Met andere woorden, de vraag "wanneer deze gebeurtenis plaatsvindt?" wordt gevraagd. Voorbeelden, kunt u situaties waarbij de gegevens beschrijft de verstreken tijd (dagen, jaren, reiskosten, enz.) tot de gebeurtenis van belang (relapse van de ziekte, PhD graden ontvangen, rem pad storingen) optreedt. Elke instantie in de gegevens staat voor een bepaald object (een patiënt, student, een auto, enz.).


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Deze [webservice]( https://datamarket.azure.com/dataset/aml_labs/survivalanalysis) antwoord op de vraag "Wat is de kans dat van belang vindt de gebeurtenis plaats door n tijd voor het object x?" Door middel van een analyse voortbestaan model, kan deze webservice gebruikers gegevens om het model van de trein en het testen. Het belangrijkste thema van het experiment is het model van de lengte van de verstreken tijd totdat de gebeurtenis van belang. 

>Deze webservice kan bijvoorbeeld worden gebruikt door gebruikers – mogelijk via een mobiele app, via een website of zelfs op een lokale computer. Maar het doel van de webservice is ook dienen als een voorbeeld van hoe Azure Machine Learning kan worden gebruikt voor het maken van webservices op R-code. Met een paar regels code R en klikt op een knop in Azure Machine Learning Studio, worden een experiment gemaakt met de R-code en gepubliceerd als een webservice. De webservice kan vervolgens gepubliceerd op de markt Azure en worden gebruikt door gebruikers en apparaten over de hele wereld met geen infrastructuur opzetten door de auteur van de webservice.  

##<a name="consumption-of-web-service"></a>Verbruik van webservice

De ingevoerde gegevensschema van de webservice wordt weergegeven in de volgende tabel. Zes stukjes informatie nodig zijn als de invoer: opleiding van gegevens, gegevens testen, de tijd van belang, de index van 'tijd' dimensie, de index van de dimensie 'gebeurtenis' en de typen variabelen (continu of factor). De trainingsgegevens wordt aangegeven met een tekenreeks, waarbij de rijen worden gescheiden door komma's en de kolommen worden gescheiden door puntkomma's. Het aantal functies van de gegevens is flexibel. Alle elementen in de invoerreeks moet numeriek zijn. In de opleiding, de dimensie 'tijd' geeft aan dat het aantal tijdseenheden (dagen, jaren, reiskosten, enz.) die is verstreken sinds het startpunt van het onderzoek (een patiënt ontvangen medicijn behandeling programma's, een student begin PhD onderzoek, een auto beginnen te worden gereden, enz.) tot de gebeurtenis die van belang zijn (de patiënt terug aan het gebruik van drugs, de student de PhD graden, de auto rem pad fout met het verkrijgen van enz.) Deze gebeurtenis vindt plaats. De dimensie 'gebeurtenis' geeft aan of de gebeurtenis van belang vindt plaats aan het einde van de studie. Een waarde van "gebeurtenis = 1" betekent dat de gebeurtenis van de rente op het moment dat wordt aangegeven door de dimensie "tijd"; "gebeurtenis = 0 ' betekent dat de gebeurtenis van de rente niet heeft plaatsgevonden door de tijd die is aangegeven door de dimensie 'tijd'.

- trainingdata - een tekenreeks. Rijen worden gescheiden door komma's en kolommen worden gescheiden door puntkomma's. Elke rij bevat de dimensie 'tijd' en 'gebeurtenis' dimensie predictor variabelen.
- testingdata - één rij met gegevens die voor een bepaald object predictor-variabelen bevat.
- time_of_interest - de verstreken tijd van n van belang.
- index_time - de kolomindex van de dimensie 'tijd' (vanaf 1).
- index_event - de kolomindex van de "event" dimensie (vanaf 1).
- variable_types - een met puntkomma's als scheidingstekens in deze tekenreeks. 0 staat voor continue variabelen en 1 voor factor variabelen.


De uitvoer is de kans op een gebeurtenis die plaatsvindt op een bepaald moment. 

>Deze service, is die worden gehost op de markt Azure een service OData; Deze kunnen worden aangeroepen via de methode POST of GET. 

Er zijn verschillende manieren van de consumptie van de service op geautomatiseerde wijze (een voorbeeld van de app is [hier](http://microsoftazuremachinelearning.azurewebsites.net/SurvivalAnalysis.aspx)). 

###<a name="starting-c-code-for-web-service-consumption"></a>C#-code voor consumptie van web-service starten:
    public class Input
    {
            public string trainingdata;
            public string testingdata;
            public string timeofinterest;
            public string indextime;
            public string indexevent;
            public string variabletypes;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { trainingdata = TextBox1.Text, testingdata = TextBox2.Text, timeofinterest = TextBox3.Text, indextime = TextBox4.Text, indexevent = TextBox5.Text, variabletypes = TextBox6.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




De interpretatie van deze test is als volgt. Ervan uitgaande dat het doel van de gegevens is als model voor de verstreken tijd tot het rendement aan het gebruik van drugs voor de patiënten die een van de twee behandeling programma's ontvangen. De uitvoer van de web service leest: voor patiënten wordt 35 jaar oud, met vorige productie medicijn behandeling 2 maal, waarbij het programma lange residentiële behandeling en met gebruik van heroïne en cocaïne zowel de kans op terugkeren naar het gebruik van drugs 95.64% van het 500 dagen is.

##<a name="creation-of-web-service"></a>Het maken van web-service

>Deze webservice is gemaakt met behulp van Azure Machine Learning. Zie [azure.com/ml](http://azure.com/ml)voor een gratis proefperiode als inleidende video over het maken van experimenten en [webservices publiceren](machine-learning-publish-a-machine-learning-web-service.md). Hieronder vindt u een screenshot van het experiment dat de web-service en voorbeeld code voor elk van de modules in het experiment gemaakt.

In Azure Machine Learning, een nieuwe, lege experiment is gemaakt en twee [R Script uitvoeren] [ execute-r-script] modules zijn getrokken naar de werkruimte. Het gegevensschema is gemaakt met een eenvoudig [R Script uitvoeren][execute-r-script], die het schema van de gegevens die zijn ingevoerd voor de webservice worden gedefinieerd. In deze module wordt vervolgens gekoppeld aan het tweede [R Script uitvoeren] [ execute-r-script] module die werk grote. In deze module wordt de voorverwerking van gegevens, model bouwen en voorspellingen. In de voorverwerking stap gegevens de ingevoerde gegevens wordt vertegenwoordigd door een lange tekenreeks omgezet en wordt omgezet in een gegevensframe. In de stap van het gebouw model een externe R pakket 'survival_2.37 7.zip' eerst wordt geïnstalleerd voor het uitvoeren van analyse overleven. Vervolgens wordt de functie 'coxph' uitgevoerd na een reeks gegevens verwerkingstaken. De details van de functie 'coxph' voor overleving analyse kunnen worden gelezen uit de R-documentatie. Een test exemplaar wordt geleverd in de opgeleide model met de functie 'surfit' in de stap voorspelling en de curve van de overleving van deze instantie testen als "curve" variabele wordt geproduceerd. Ten slotte, de kans op het tijdstip van de rente wordt verkregen. 

###<a name="experiment-flow"></a>Experiment stroom:

![experiment stroom][1]

####<a name="module-1"></a>Module 1:

    #Data schema with example data (replaced with data from web service)
    trainingdata="53;1;29;0;0;3,79;1;34;0;1;2,45;1;27;0;1;1,37;1;24;0;1;1,122;1;30;0;1;1,655;0;41;0;0;1,166;1;30;0;0;3,227;1;29;0;0;3,805;0;30;0;0;1,104;1;24;0;0;1,90;1;32;0;0;1,373;1;26;0;0;1,70;1;36;0;0;1”
    testingdata="35;2;1;1"
    time_of_interest="500"
    index_time="1"
    index_event="2"
    
    # 0 - continuous; 1 -  factor
    variable_types="0;0;1;1"

    sampleInput=data.frame(trainingdata,testingdata,time_of_interest,index_time,index_event,variable_types)

    maml.mapOutputPort("sampleInput"); #send data to output port
    
####<a name="module-2"></a>Module 2:

    #Read data from input port
    data <- maml.mapInputPort(1) 
    colnames(data) <- c("trainingdata","testingdata","time_of_interest","index_time","index_event","variable_types")

    # Preprocessing training data
    traindingdata=data$trainingdata
    y=strsplit(as.character(data$trainingdata),",")
    n_row=length(unlist(y))
    z=sapply(unlist(y), strsplit, ";", simplify = TRUE)
    mydata <- data.frame(matrix(unlist(z), nrow=n_row, byrow=T), stringsAsFactors=FALSE)
    n_col=ncol(mydata)

    # Preprocessing testing data
    testingdata=as.character(data$testingdata)
    testingdata=unlist(strsplit(testingdata,";"))

    # Preprocessing other input parameters
    time_of_interest=data$time_of_interest
    time_of_interest=as.numeric(as.character(time_of_interest))
    index_time = data$index_time
    index_event = data$index_event
    variable_types = data$variable_types

    # Necessary R packages
    install.packages("src/packages_survival/survival_2.37-7.zip",lib=".",repos=NULL,verbose=TRUE)
    library(survival)

    # Prepare to build model
    attach(mydata)

    for (i in 1:n_col){ mydata[,i]=as.numeric(mydata[,i])} 
    d_time=paste("X",index_time,sep = "")
    d_event=paste("X",index_event,sep = "")
    v_time_event <- c(d_time,d_event)
    v_predictors = names(mydata)[!(names(mydata) %in% v_time_event)]

    variable_types = unlist(strsplit(as.character(variable_types),";"))

    len = length(v_predictors)
    c="" # Construct the execution string
    for (i in 1:len){
    if(i==len){
    if(variable_types[i]!=0){ c=paste(c, "factor(",v_predictors[i],")",sep="")}
     else{ c=paste(c, v_predictors[i])}
    }else{
    if(variable_types[i]!=0){c=paste(c, "factor(",v_predictors[i],") + ",sep="")}
    else{c=paste(c, v_predictors[i],"+")}
    }
    }
    f=paste("coxph(Surv(",d_time,",",d_event,") ~")
    f=paste(f,c)
    f=paste(f,", data=mydata )")

    # Fit a Cox proportional hazards model and get the predicted survival curve for a testing instance 
    fit=eval(parse(text=f))

    testingdata = as.data.frame(matrix(testingdata, ncol=len,byrow = TRUE),stringsAsFactors=FALSE)
    names(testingdata)=v_predictors
    for (i in 1:len){ testingdata[,i]=as.numeric(testingdata[,i])}

    curve=survfit(fit,testingdata)

    # Based on user input, find the event occurrence probability
    position_closest=which.min(abs(prob_event$time - time_of_interest))

    if(prob_event[position_closest,"time"]==time_of_interest){# exact match
    output=prob_event[position_closest,"prob"]
    }else{# not exact match
    if(time_of_interest>max(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else if(time_of_interest<min(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else{output=(prob_event[position_closest,"prob"]+prob_event[position_closest+1,"prob"])/2}
    }

    #Pull out results to send to web service
    output=paste(round(100*output, 2), "%") 
    maml.mapOutputPort("output"); #output port




##<a name="limitations"></a>Beperkingen

Deze webservice kunnen alleen numerieke waarden als functie variabelen (kolommen). De kolom "event" kan duren voordat de waarde 0 of 1. De kolom 'tijd' moet een positief geheel getal.

##<a name="faq"></a>FAQ
Zie voor veelgestelde vragen over het verbruik van de webservice of publiceren naar de markt Azure [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-survival-analysis/survive_img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
