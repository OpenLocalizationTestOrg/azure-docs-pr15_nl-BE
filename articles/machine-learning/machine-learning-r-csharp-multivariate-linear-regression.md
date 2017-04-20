<properties 
    pageTitle="Multidimensionale lineaire regressie | Microsoft Azure" 
    description="Multidimensionale lineaire regressie" 
    services="machine-learning" 
    documentationCenter="" 
    authors="jaymathe" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/14/2016" 
    ms.author="jaymathe"/> 


#<a name="multivariate-linear-regression"></a>Multidimensionale lineaire regressie   
 

 
Stel dat u een gegevensset hebt en wil graag snel voorspellen een afhankelijke variabele (y) voor elk individu (i) op basis van de onafhankelijke variabelen. Lineaire regressie is een populaire statistische techniek voor dergelijke voorspellingen. Hier wordt de afhankelijke variabele y uitgegaan van een continue waarde.  


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  

Een eenvoudige scenario mogelijk waarbij de onderzoeker probeert te voorspellen van het gewicht van een persoon (y) op basis van de hoogte (x). Een meer geavanceerde scenario mogelijk waarbij de onderzoeker vindt u aanvullende informatie voor de gebruiker (zoals gewicht, geslacht, race) en probeert te voorspellen van het gewicht van de persoon. Deze [web-service]( https://datamarket.azure.com/dataset/aml_labs/multivariate_regression) past de lineaire regressiemodel met de gegevens en de voorspelde waarde (y)-uitgang voor elk van de metingen in de gegevens.

>Deze webservice kan bijvoorbeeld worden gebruikt door gebruikers – mogelijk via een mobiele app, via een website of zelfs op een lokale computer. Maar het doel van de webservice is ook dienen als een voorbeeld van hoe Azure Machine Learning kan worden gebruikt voor het maken van webservices op R-code. Met een paar regels code R en klikt op een knop in Azure Machine Learning Studio, worden een experiment gemaakt met de R-code en gepubliceerd als een webservice. De webservice kan vervolgens gepubliceerd op de markt Azure en worden gebruikt door gebruikers en apparaten over de hele wereld met geen infrastructuur opzetten door de auteur van de webservice.  

##<a name="consumption-of-web-service"></a>Verbruik van webservice  
Deze webservice geeft de voorspelde waarden van de afhankelijke variabele op basis van de onafhankelijke variabelen voor alle opmerkingen. De webservice verwacht dat de gebruiker om invoer van gegevens als een tekenreeks waarin rijen worden gescheiden door een komma (,) en kolommen worden gescheiden door puntkomma's (;). De webservice verwacht 1 rij tegelijk en verwacht dat de eerste kolom worden de afhankelijke variabele. Een voorbeeld van de dataset kan uitzien:

![Voorbeeldgegevens][1]

Waarnemingen zonder een afhankelijke variabele moeten worden ingevoerd als de waarde 'N.V.T.' in voor y. De gegevensinvoer voor de bovenstaande gegevensverzameling de volgende tekenreeks zijn: "10; 5; 2,18; 1; 6,6; 5.3; 2.1,7; 5; 5,22; 3; 4,12; 2; 1, NB; 3; 4 '. De uitvoer is de voorspelde waarde voor elk van de rijen op basis van de onafhankelijke variabelen. 

>Deze service, is die worden gehost op de markt Azure een service OData; Deze kunnen worden aangeroepen via de methode POST of GET. 

Er zijn verschillende manieren van de consumptie van de service op geautomatiseerde wijze (een voorbeeld van de app is [hier](http://microsoftazuremachinelearning.azurewebsites.net/MultipleLinearRegressionService.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>C#-code voor consumptie van web-service starten:

    public class Input
    {
            public string value;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { value = TextBox1.Text };
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


In Azure Machine Learning, een nieuwe, lege experiment is gemaakt en twee [R Script uitvoeren] [ execute-r-script] modules zijn getrokken naar de werkruimte. Deze webservice wordt een Azure Machine Learning experiment met een onderliggende R script uitgevoerd. Er zijn 2 delen van dit experiment: schemadefinitie, en training model + scoren. De eerste module definieert de verwachte structuur van de invoer dataset, waarbij de eerste variabele is de afhankelijke variabele en de overige variabelen zijn onafhankelijk. De tweede module past in een algemene lineaire regressiemodel van de ingevoerde gegevens.  
  
![Experiment stroom][3]

####<a name="module-1"></a>Module 1:
 
####<a name="schema-definition"></a>, Schemadefinitie  
    data <- data.frame(value = "1;2;3,4;5;6,7;8;9", stringsAsFactors=FALSE) maml.mapOutputPort("data");  

####<a name="module-2"></a>Module 2:
####<a name="lm-modeling"></a>LM-model   
    data <- maml.mapInputPort(1) # class: data.frame  
  
    data.split <- strsplit(data[1,1], ",")[[1]]  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- as.data.frame(t(data.split)) 
    data.split <- data.matrix(data.split) 
    data.split <- data.frame(data.split) 
    model <- lm(data.split)  

    out=data.frame(predict(model,data.split))  
    out <- data.frame(t(out))

    maml.mapOutputPort("out");  
 
##<a name="limitations"></a>Beperkingen
Dit is een zeer eenvoudig voorbeeld van een lineaire regressie meerdere Internet. Zoals u kunt zien in de bovenstaande voorbeeldcode, geen fout vangst is geïmplementeerd en de service wordt ervan uitgegaan dat alles is een continu variabele (geen bestaan functies toegestaan), als de service alleen invoer numerieke waarden op het moment van het maken van deze webservice. Ook verwerkt de service momenteel beperkte capaciteit, wegens de aard van de aanvraag en respons van de webservice wordt opgeroepen en het feit dat het model dat geschikt is telkens wanneer die de webservice wordt aangeroepen. 

##<a name="faq"></a>FAQ
Zie voor veelgestelde vragen over het verbruik van de webservice of publiceren naar de markt Azure [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img1.png
[2]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img2.png
[3]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
