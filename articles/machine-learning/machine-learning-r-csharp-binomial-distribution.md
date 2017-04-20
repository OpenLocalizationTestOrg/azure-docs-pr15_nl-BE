<properties 
    pageTitle="Binomiale verdeling Suite | Microsoft Azure" 
    description="Binomiale verdeling Suite" 
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


#<a name="binomial-distribution-suite"></a>Binomiale verdeling Suite




De binomiaalverdeling Suite is een verzameling van monster webservices ([Generator binomiale](https://datamarket.azure.com/dataset/aml_labs/bdg5) [Waarschijnlijkheid Rekenmachine]( https://datamarket.azure.com/dataset/aml_labs/bdp4), [Kwantiel Rekenmachine]( https://datamarket.azure.com/dataset/aml_labs/bdq5)) die helpen bij het genereren en afhandelen binomiale verdelingen. De services kunnen genereren een reeks binomiale verdeling van een willekeurige lengte, berekening van quantiles van uitgaande van kans en berekenen van waarschijnlijkheid van een bepaalde kwantiel. Elk van de services genereert verschillende uitgangen op basis van de geselecteerde service (Zie beschrijving hieronder). De binomiaalverdeling Suite is gebaseerd op de R functies qbinom, rbinom en pbinom, die zijn opgenomen in het pakket R stats. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

>Deze webservices kunnen worden gebruikt door gebruikers – mogelijk rechtstreeks op de markt, via een mobiele app, via een website of zelfs op een lokale computer, bijvoorbeeld. Maar het doel van de webservice is ook dienen als een voorbeeld van hoe Azure Machine Learning kan worden gebruikt voor het maken van webservices op R-code. Met een paar regels code R en klikt op een knop in Azure Machine Learning Studio, worden een experiment gemaakt met de R-code en gepubliceerd als een webservice. De webservice kan vervolgens worden gepubliceerd naar Azure Marketplace en over de hele wereld worden gebruikt door gebruikers en apparaten – geen infrastructuur opzetten door de auteur van de webservice is vereist.

##<a name="consumption-of-web-service"></a>Verbruik van webservice
De binomiaalverdeling Suite omvat de volgende services 3.

###<a name="binomial-distribution-quantile-calculator"></a>Binomiale verdeling kwantiel Rekenmachine
Deze service 4 argumenten van een normale verdeling en de bijbehorende kwantiel berekend.
Het invoeren van argumenten zijn:

- p - één samengevoegd kans op meerdere proeven.  
- grootte - het aantal experimenten.
- kans - de kans van slagen van een proef.
- Zijkant - L voor het onderste gedeelte van de verdeling, U voor de bovenzijde van de verdeling. 

De uitvoer van de service is de berekende kwantiel die is gekoppeld aan de kans gegeven.

###<a name="binomial-distribution-probability-calculator"></a>Binomiale verdeling kans Rekenmachine
Deze service 4 argumenten van een binomiale verdeling en wordt de kans berekend.
Het invoeren van argumenten zijn:

- q - een enkele kwantiel van een gebeurtenis met de binomiale verdeling. 
- grootte - het aantal experimenten.
- kans - de kans van slagen van een proef.
- zijkant - L voor het onderste gedeelte van de verdeling, U voor de bovenzijde van de distributie of E die gelijk is aan een enkel het aantal gunstige uitkomsten.

De uitvoer van de service is de berekende kans die is gekoppeld aan de opgegeven kwantiel.

###<a name="binomial-distribution-generator"></a>Binomiale verdeling Generator
Deze service 3 argumenten van een binomiale verdeling en genereert een willekeurige reeks getallen die binomially zijn verdeeld. De volgende argumenten moeten in de aanvraag te worden verstrekt:

- n - aantal waarnemingen. 
- grootte - het aantal experimenten.
- kans - de kans op succes.

De uitvoer van de service is een reeks van lengte n met een binomiale verdeling op basis van de grootte en prob argumenten.

>Deze service, is die worden gehost op de markt Azure een service OData; Deze kunnen worden aangeroepen via de methode POST of GET. 

Er zijn verschillende manieren van de consumptie van de service op geautomatiseerde wijze (voorbeeld apps zijn hier: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionGenerator.aspx) [Kans Rekenmachine](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionProbabilityCalculator.aspx), [Kwantiel Rekenmachine](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionQuantileCalculator)). 

###<a name="starting-c-code-for-web-service-consumption"></a>C#-code voor consumptie van web-service starten:

###<a name="binomial-distribution-quantile-calculator"></a>Binomiale verdeling kwantiel Rekenmachine
    public class Input
    {
            public string p;
            public string size;
            public string prob;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void main()
    {
            var input = new Input() { p = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

###<a name="binomial-distribution-probability-calculator"></a>Binomiale verdeling kans Rekenmachine
    public class Input
    {
            public string q;
            public string size;
            public string prob;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { q = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = " PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


###<a name="binomial-distribution-generator"></a>Binomiale verdeling Generator
    public class Input
    {
            public string n;
            public string size;
            public string p;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { n = TextBox1.Text, size = TextBox2.Text, p = TextBox3.Text };
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

###<a name="binomial-distribution-quantile-calculator"></a>Binomiale verdeling kwantiel Rekenmachine

![Werkruimte maken][4]

####<a name="module-1"></a>Module 1:
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
####<a name="module-2"></a>Module 2:

    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }

    if (param$prob < 0 ) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 0
    } else if (param$prob > 1) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 1
    }

    quantile = qbinom(param$p,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    quantile

    if (param$side == 'U'){
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = F)
    band=subset(df,x>quantile)
    } else if (param$side =='L') {
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = T)
    band=subset(df,x<=quantile)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(quantile)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");


###<a name="binomial-distribution-probability-calculator"></a>Binomiale verdeling kans Rekenmachine

![Werkruimte maken][5]

####<a name="module-1"></a>Module 1:

    #data schema with example data (replaced with data from web service)
    data.set=data.frame(q=5,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port


####<a name="module-2"></a>Module 2:
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    prob = pbinom(param$q,size=param$size,prob=param$prob)
    prob.eq = dbinom(param$q,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    prob

    if (param$side == 'U'){
    prob = 1 - prob
    band=subset(df,x>param$q)
    } else if (param$side =='E') {
    prob = prob.eq
    band=subset(df,x==param$q)
    } else if (param$side =='L') {
    prob = prob
    band=subset(df,x<=param$q)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

###<a name="binomial-distribution-generator"></a>Binomiale verdeling Generator

![Werkruimte maken][6]

####<a name="module-1"></a>Module 1:

    #data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,size=10,p=.5);
    maml.mapOutputPort("data.set"); #send data to output port

####<a name="module-2"></a>Module 2:
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    dist = rbinom(param$n,param$size,param$p)

    output = as.data.frame(t(dist))
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

##<a name="limitations"></a>Beperkingen 
Dit zijn eenvoudige voorbeelden rond de binomiale verdeling. Zoals u kunt zien in de bovenstaande voorbeeldcode, is weinig fout vangst geïmplementeerd.

##<a name="faq"></a>FAQ
Zie voor veelgestelde vragen over het verbruik van de webservice of publiceren naar de markt Azure [hier](machine-learning-marketplace-faq.md).


[1]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_1.png

[2]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_2.png

[3]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_3.png

[4]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_4.png

[5]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_5.png

[6]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_6.png
 
