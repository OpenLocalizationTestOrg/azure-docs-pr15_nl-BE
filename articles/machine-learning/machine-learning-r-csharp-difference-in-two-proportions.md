<properties 
    pageTitle="Verschil in verhoudingen Test | Microsoft Azure" 
    description="Verschil in verhoudingen Test" 
    services="machine-learning" 
    documentationCenter="" 
    authors="aniedea" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="aniedea"/> 


#<a name="difference-in-proportions-test"></a>Verschil in verhoudingen Test


Zijn twee verhoudingen statistisch gezien? Stel dat een gebruiker wil vergelijken twee films om te bepalen of een film een aanzienlijk hoger aandeel van heeft 'leuk vinden' wanneer vergeleken met de andere. Met een grote steekproef, kan er een statistisch significant verschil tussen de verhoudingen 0,50 en 0,51. Met een kleine steekproef, er mogelijk niet voldoende gegevens om te bepalen of deze verhoudingen werkelijk verschillende zijn. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Deze [webservice]( https://datamarket.azure.com/dataset/aml_labs/prop_test) verricht een hypothese test van het verschil in twee verhoudingen op basis van gebruikersinvoer, het aantal gunstige uitkomsten en het totale aantal proeven voor de groepen 2 vergelijking. In een voorbeeldscenario kan deze webservice worden aangeroepen vanuit een app film vergelijking, waarbij de gebruiker of een van de films is echt 'beviel' vaker dan de andere, op basis van de classificatie van films.

>Deze webservice kan bijvoorbeeld worden gebruikt door gebruikers – mogelijk via een mobiele app, via een website of zelfs op een lokale computer. Maar het doel van de webservice is ook dienen als een voorbeeld van hoe Azure Machine Learning kan worden gebruikt voor het maken van webservices op R-code. Met een paar regels code R en klikt op een knop in Azure Machine Learning Studio, worden een experiment gemaakt met de R-code en gepubliceerd als een webservice. De webservice kan vervolgens gepubliceerd op de markt Azure en worden gebruikt door gebruikers en apparaten over de hele wereld met geen infrastructuur opzetten door de auteur van de webservice.


##<a name="consumption-of-web-service"></a>Verbruik van webservice

Deze service 4 argumenten accepteert en heeft een hypothese testen van verhoudingen.

Het invoeren van argumenten zijn:

* Successes1 - aantal geslaagde gebeurtenissen in voorbeeld 1.
* Successes2 - aantal geslaagde gebeurtenissen in voorbeeld 2.
* Total1 - grootte van voorbeeld 1.
* Total2 - grootte van voorbeeld 2.

De uitvoer van de service is het resultaat van de hypothese testen met de chi-square statistiek, df, p-waarde en het aandeel in voorbeeld 1/2 en betrouwbaarheidsinterval grenzen.

>Deze service, is die worden gehost op de markt Azure een service OData; Deze kunnen worden aangeroepen via de methode POST of GET. 

Er zijn verschillende manieren van de consumptie van de service op geautomatiseerde wijze (een voorbeeld van de app is [hier](http://microsoftazuremachinelearning.azurewebsites.net/DifferenceInProportionsTest.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>C#-code voor consumptie van web-service starten:

    public class Input
    {
            public string successes1;
            public string successes2;
            public string total1;
            public string total2;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { successes1 = TextBox1.Text, successes2 = TextBox2.Text, total1 = TextBox3.Text, total2 = TextBox4.Text };
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

In Azure Machine Learning, een nieuwe, lege experiment is gemaakt met twee [R Script uitvoeren] [ execute-r-script] modules. In de eerste module die het gegevensschema is gedefinieerd, terwijl de tweede module gebruikt de opdracht prop.test in R de hypothese testen uitvoeren voor de verhoudingen 2. 


###<a name="experiment-flow"></a>Experiment stroom:

![Experiment stroom][2]


####<a name="module-1"></a>Module 1:
    ####Schema definition  
    data.set=data.frame(successes1=50,successes2=60,total1=100,total2=100);
    maml.mapOutputPort("data.set"); #send data to output port
    dataset1 = maml.mapInputPort(1) #read data from input port
    

####<a name="module-2"></a>Module 2:

    test=prop.test(c(dataset1$successes1[1],dataset1$successes2[1]),c(dataset1$total1[1],dataset1$total2[1])) #conduct hypothesis test

    result=data.frame(
    result=ifelse(test$p.value<0.05,"The proportions are different!",
    "The proportions aren't different statistically."),
    ChiSquarestatistic=round(test$statistic,2),df=test$parameter,
    pvalue=round(test$p.value,4),
    proportion1=round(test$estimate[1],4),
    proportion2=round(test$estimate[2],4),
    confintlow=round(test$conf.int[1],4),
    confinthigh=round(test$conf.int[2],4)) 

    maml.mapOutputPort("result"); #output port
    

##<a name="limitations"></a>Beperkingen 

Dit is een zeer eenvoudig voorbeeld voor een test van het verschil in de verhoudingen 2. Zoals u kunt zien in de bovenstaande voorbeeldcode, geen fout vangst is geïmplementeerd en de service wordt ervan uitgegaan dat alle variabelen doorlopend.

##<a name="faq"></a>FAQ
Zie voor veelgestelde vragen over het verbruik van de webservice of publiceren naar de markt Azure [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img1.png
[2]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
