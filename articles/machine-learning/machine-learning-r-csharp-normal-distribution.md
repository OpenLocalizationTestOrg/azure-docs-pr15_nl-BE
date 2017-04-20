<properties 
    pageTitle="Normale verdeling Web Service Suite | Microsoft Azure" 
    description="Normale verdeling Web Service Suite" 
    services="machine-learning" 
    documentationCenter="" 
    authors="ireiter" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="ireiter"/> 

#<a name="normal-distribution-suite"></a>Normale verdeling Suite



De normaalverdeling Suite is een verzameling van monster webservices ([Generator]( https://datamarket.azure.com/dataset/aml_labs/ndg7) [Kwantiel Rekenmachine]( https://datamarket.azure.com/dataset/aml_labs/ndq5), [Kans Rekenmachine]( https://datamarket.azure.com/dataset/aml_labs/ndp5)) die helpen bij het genereren en afhandelen van normale verdelingen. De services kunnen een reeks normale verdeling van een willekeurige lengte, de quantiles van een bepaalde waarschijnlijkheid berekenen en berekenen van de kans van een bepaalde kwantiel genereren. Elk van de services genereert verschillende uitgangen op basis van de geselecteerde service (Zie beschrijving hieronder). De normaalverdeling Suite is gebaseerd op de R functies qnorm, rnorm en pnorm, die zijn opgenomen in R stats pakket.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

>Deze webservice kan bijvoorbeeld worden gebruikt door gebruikers – mogelijk via een mobiele app, via een website of zelfs op een lokale computer. Maar het doel van de webservice is ook dienen als een voorbeeld van hoe Azure Machine Learning kan worden gebruikt voor het maken van webservices op R-code. Met een paar regels code R en klikt op een knop in Azure Machine Learning Studio, worden een experiment gemaakt met de R-code en gepubliceerd als een webservice. De webservice kan vervolgens gepubliceerd op de markt Azure en worden gebruikt door gebruikers en apparaten over de hele wereld met geen infrastructuur opzetten door de auteur van de webservice.  
 

##<a name="consumption-of-web-service"></a>Verbruik van webservice
De normaalverdeling-Suite bevat de volgende services 3.

###<a name="normal-distribution-quantile-calculator"></a>Normale verdeling kwantiel Rekenmachine
Deze service 4 argumenten van een normale verdeling en de bijbehorende kwantiel berekend.

Het invoeren van argumenten zijn:

* p - een enkel kans van een gebeurtenis met de normale verdeling. 
* Gemiddelde - het gemiddelde van de normale verdeling.
* SD - de standaarddeviatie van de normale verdeling. 
* Zijkant - L voor het onderste gedeelte van de distributie en U voor de bovenzijde van de verdeling.

De uitvoer van de service is de berekende kwantiel die is gekoppeld aan de kans gegeven.

###<a name="normal-distribution-probability-calculator"></a>Normale verdeling kans Rekenmachine
Deze service 4 argumenten van een normale verdeling en wordt de kans berekend.

Het invoeren van argumenten zijn:

* q - een enkele kwantiel van een gebeurtenis met de normale verdeling. 
* Gemiddelde - het gemiddelde van de normale verdeling.
* SD - de standaarddeviatie van de normale verdeling. 
* Zijkant - L voor het onderste gedeelte van de distributie en U voor de bovenzijde van de verdeling.

De uitvoer van de service is de berekende kans die is gekoppeld aan de opgegeven kwantiel.

###<a name="normal-distribution-generator"></a>Normale verdeling Generator
Deze service 3 argumenten van een normale verdeling en genereert een willekeurige reeks getallen die normaliter worden gedistribueerd. De volgende argumenten moeten in de aanvraag te worden verstrekt:

* n - het aantal waarnemingen. 
* gemiddelde - het gemiddelde van de normale verdeling.
* SD - de standaarddeviatie van de normale verdeling. 

De uitvoer van de service is een reeks van lengte n met een normale verdeling op basis van de gemiddelde en sd-argumenten.

>Deze service, is die worden gehost op de markt Azure een service OData; Deze kunnen worden aangeroepen via de methode POST of GET. 

Er zijn verschillende manieren van de consumptie van de service op geautomatiseerde wijze (voorbeeld apps zijn hier: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionGenerator.aspx) [Kans Rekenmachine](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionProbabilityCalculator.aspx), [Kwantiel Rekenmachine](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionQuantileCalculator.aspx)).

###<a name="starting-c-code-for-web-service-consumption"></a>C#-code voor consumptie van web-service starten:

###<a name="normal-distribution-quantile-calculator"></a>Normale verdeling kwantiel Rekenmachine
    public class Input
    {
            public string p;
            public string mean;
            public string sd;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { p = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


###<a name="normal-distribution-probability-calculator"></a>Normale verdeling kans Rekenmachine
    public class Input
    {
            public string q;
            public string mean;
            public string sd;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { q = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

###<a name="normal-distribution-generator"></a>Normale verdeling Generator
    public class Input
    {
            public string n;
            public string mean;
            public string sd;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { n = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text };
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
>Deze webservice is gemaakt met behulp van Azure Machine Learning. Zie [azure.com/ml](http://azure.com/ml)voor een gratis proefperiode als inleidende video over het maken van experimenten en [webservices publiceren](machine-learning-publish-a-machine-learning-web-service.md). 

Hieronder vindt u een screenshot van het experiment dat de web-service en voorbeeld code voor elk van de modules in het experiment gemaakt.

###<a name="normal-distribution-quantile-calculator"></a>Normale verdeling kwantiel Rekenmachine

Experiment stroom:

![Experiment stroom][2]
 
    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }
    q = qnorm(param$p,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    q = 2* param$mean - q
    } else if (param$side =='L') {
    q = q
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(q)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");
    
###<a name="normal-distribution-probability-calculator"></a>Normale verdeling kans Rekenmachine
Experiment stroom:

![Experiment stroom][3]
 
    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(q=-1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    prob = pnorm(param$q,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    prob = 1 - prob
    } else if (param$side =='B') {
    prob = ifelse(prob<=0.5,prob * 2, 1)
    } else if (param$side =='L') {
    prob = prob
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");
    
###<a name="normal-distribution-generator"></a>Normale verdeling Generator
Experiment stroom:

![Experiment stroom][4]

    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,mean=0,sd=1);
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    dist = rnorm(param$n,mean=param$mean,sd=param$sd)

    output = as.data.frame(t(dist))

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

##<a name="limitations"></a>Beperkingen 

Dit zijn eenvoudige voorbeelden rond de normale verdeling. Zoals u kunt zien in de bovenstaande voorbeeldcode, is weinig fout vangst geïmplementeerd.

##<a name="faq"></a>FAQ
Zie voor veelgestelde vragen over het verbruik van de webservice of publiceren naar de markt Azure [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-normal-distribution/normal-img1.png
[2]: ./media/machine-learning-r-csharp-normal-distribution/normal-img2.png
[3]: ./media/machine-learning-r-csharp-normal-distribution/normal-img3.png
[4]: ./media/machine-learning-r-csharp-normal-distribution/normal-img4.png
 
