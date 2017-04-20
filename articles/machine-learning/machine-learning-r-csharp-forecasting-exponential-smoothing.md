<properties 
    pageTitle="Prognoses exponentiële demping | Microsoft Azure" 
    description="Webservice: prognoses exponentiële demping" 
    services="machine-learning" 
    documentationCenter="" 
    authors="xueshanz" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016" 
    ms.author="xueshzha"/> 


#<a name="forecasting---exponential-smoothing"></a>Prognose - exponentiële demping 

Deze [web-service]( https://datamarket.azure.com/dataset/aml_labs/ets) implementeert het model exponentiële demping (ETS) voor de productie van voorspellingen op basis van de historische gegevens van de gebruiker. Vergroot de vraag naar een specifiek product dit jaar? Kan ik mijn productverkopen voorspellen voor het seizoen van Kerstmis, zodat ik mijn voorraad effectief kunt plannen? Prognose modellen zijn voor het adres van deze vragen. Gezien de laatste gegevens, onderzoeken deze modellen verborgen trends en seizoengebonden toekomstige trends voorspellen.  


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
 
>Deze webservice kan bijvoorbeeld worden gebruikt door gebruikers – mogelijk via een mobiele app, via een website of zelfs op een lokale computer. Maar het doel van de webservice is ook dienen als een voorbeeld van hoe Azure Machine Learning kan worden gebruikt voor het maken van webservices op R-code. Met een paar regels code R en klikt op een knop in Azure Machine Learning Studio, worden een experiment gemaakt met de R-code en gepubliceerd als een webservice. De webservice kan vervolgens gepubliceerd op de markt Azure en worden gebruikt door gebruikers en apparaten over de hele wereld met geen infrastructuur opzetten door de auteur van de webservice.
 
##<a name="consumption-of-web-service"></a>Verbruik van webservice 
 
Deze service 4 argumenten en de prognoses van de ETS wordt berekend.
Het invoeren van argumenten zijn:

* Frequentie - geeft de frequentie van de onbewerkte gegevens (dagelijks/wekelijks/maandelijks/kwartaal/jaarlijks).
* Horizon - toekomst voorspellen tijd frame.
* Datum - voor tijd in de nieuwe tijd reeksgegevens toevoegen.
* Waarde - toevoegt in de nieuwe reeks gegevens tijdwaarden.

De uitvoer van de service is de berekende waarden.

Monster input kan worden: 

* Frequentie - 12
* Horizon - 12
* Datum: 15-1-2012; 15-2-2012 15-3-2012; 15-4-2012; 15-5-2012; 15-6-2012; 15-7-2012; 8 / 15-2012; 15-9-2012 15-10-2012, 15-11-2012; 15-12/2012; 15-1/2013; 2/15/2013 3/15/2013; 4/15/2013; 15-5/2013; 15-6/2013; 15-7/2013; 8 / 15/2013; 9/15/2013 15-10/2013; 15-11/2013; 15-12/2013; 15-1/2014; 2/15/2014 15/3/2014; 15-4/2014; 15-5/2014; 15-6/2014; 15-7/2014; 8 / 15/2014; 9/15/2014
* Waarde - 3.479; 3,68; 3.832; 3.941; 3.797; 3.586; 3.508; 3.731; 3.915; 3.844; 3.634; 3.549; 3.557; 3.785; 3.782; 3.601; 3.544; 3.556; 3.65; 3.709; 3.682; 3.511; 3.429; 3.51 3.523; 3.525; 3.626; 3.695; 3.711; 3.711; 3.693; 3.571; 3.509
 
>Deze service, is die worden gehost op de markt Azure een service OData; Deze kunnen worden aangeroepen via de methode POST of GET. 

Er zijn verschillende manieren van de consumptie van de service op geautomatiseerde wijze (een voorbeeld van de app is [hier](http://microsoftazuremachinelearning.azurewebsites.net/etsForecasting.aspx)).

###<a name="starting-c-code-for-web-service-consumption"></a>C#-code voor consumptie van web-service starten:
    public class Input
    {
            public string frequency;
            public string horizon;
            public string date;
            public string value;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { frequency = TextBox1.Text, horizon = TextBox2.Text, date = TextBox3.Text, value = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }



##<a name="creation-of-web-service"></a>Het maken van web-service 

>Deze webservice is gemaakt met behulp van Azure Machine Learning. Zie [azure.com/ml](http://azure.com/ml)voor een gratis proefperiode als inleidende video over het maken van experimenten en [webservices publiceren](machine-learning-publish-a-machine-learning-web-service.md). Hieronder vindt u een screenshot van het experiment dat de web-service en voorbeeld code voor elk van de modules in het experiment gemaakt.

In Azure Machine Learning, is een nieuwe, lege experiment gemaakt. Invoer van voorbeeldgegevens is geüpload met een vooraf gedefinieerde schema. Gekoppeld aan het schema van de is een [R-Script uitvoeren] [ execute-r-script] module de ETS model prognoses genereert met behulp van 'ets' en 'Prognose' functies van R. 


![Experiment stroom][2]

####<a name="module-1"></a>Module 1:
 
    # Add in the CSV file with the data in the format shown below 
![Voorbeeld van gegevens][3]   

####<a name="module-2"></a>Module 2:
    # Data input
    data <- maml.mapInputPort(1) # class: data.frame
    library(forecast)
    
    # Preprocessing
    colnames(data) <- c("frequency", "horizon", "dates", "values")
    dates <- strsplit(data$dates, ";")[[1]]
    values <- strsplit(data$values, ";")[[1]]
    
    dates <- as.Date(dates, format = '%m/%d/%Y')
    values <- as.numeric(values)
    
    # Fit a time-series model
    train_ts<- ts(values, frequency=data$frequency)
    fit1 <- ets(train_ts)
    train_model <- forecast(fit1, h = data$horizon)
    plot(train_model)
    
    # Produce forcasting
    train_pred <- round(train_model$mean,2)
    data.forecast <- as.data.frame(t(train_pred))
    colnames(data.forecast) <- paste("Forecast", 1:data$horizon, sep="")
    
    # Data output
    maml.mapOutputPort("data.forecast");

 
##<a name="limitations"></a>Beperkingen 

Dit is een zeer eenvoudig voorbeeld voor het voorspellen van ETS. Zoals u kunt zien in de bovenstaande voorbeeldcode, geen fout vangst is geïmplementeerd en de service wordt ervan uitgegaan dat alle variabelen continu-positieve waarden worden en de frequentie een geheel getal groter dan 1 moet. De lengte van de vectoren en de waarde moet gelijk zijn. De variabele moet voldoen aan de notatie 'dd-mm-jjjj'.

##<a name="faq"></a>FAQ
Zie voor veelgestelde vragen over het verbruik van de webservice of publiceren naar de markt Azure [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-forecasting-exponential-smoothing/ets-img1.png
[2]: ./media/machine-learning-r-csharp-forecasting-exponential-smoothing/ets-img2.png
[3]: ./media/machine-learning-r-csharp-forecasting-exponential-smoothing/ets-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
