<properties 
    pageTitle="Cluster-Model | Microsoft Azure" 
    description="Clustermodel" 
    services="machine-learning" 
    documentationCenter="" 
    authors="FrancescaLazzeri" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="lazzeri"/> 


#<a name="cluster-model"></a>Clustermodel    

Hoe kunnen we groepen credit kaarthouders gedrag voorspellen ter vermindering van het risico afboeking van creditcardmaatschappijen? Hoe definiëren we groepen van kenmerken van de persoonlijkheid van de werknemers ter verbetering van hun prestaties op het werk? Hoe kunnen artsen classificeren van patiënten in groepen op basis van de kenmerken van hun ziekten? In principe kunnen al deze vragen worden beantwoord door cluster-analyse.   


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 
   
Cluster analyse classificeert een aantal observaties in twee of meer elkaar uitsluitende onbekende groepen op basis van combinaties van variabelen. Het doel van de cluster-analyse is te ontdekken van een systeem van waarnemingen, meestal mensen of de kenmerken ervan, ordenen in groepen, waar leden van de groepen eigenschappen gemeen delen. Deze [service](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) maakt gebruik van de methodologie van het K-middelen, een veelgebruikte clusters techniek cluster willekeurige gegevens in groepen. Deze webservice neemt de gegevens en het aantal k clusters als invoer en voorspellingen van de groepen waartoe elke waarnemingen behoort k produceert. 

>Deze webservice kan bijvoorbeeld worden gebruikt door gebruikers – mogelijk via een mobiele app, via een website of zelfs op een lokale computer. Maar het doel van de webservice is ook dienen als een voorbeeld van hoe Azure Machine Learning kan worden gebruikt voor het maken van webservices op R-code. Met een paar regels code R en klikt op een knop in Azure Machine Learning Studio, worden een experiment gemaakt met de R-code en gepubliceerd als een webservice. De webservice kan vervolgens gepubliceerd op de markt Azure en worden gebruikt door gebruikers en apparaten over de hele wereld met geen infrastructuur opzetten door de auteur van de webservice.  

##<a name="consumption-of-web-service"></a>Verbruik van webservice   
Deze webservice worden de gegevens gegroepeerd in een aantal groepen k en uitgang van de groepstoewijzing van de voor elke rij. De webservice verwacht dat de gebruiker om invoer van gegevens als een tekenreeks waarin rijen worden gescheiden door een komma (,) en kolommen worden gescheiden door puntkomma's (;). De webservice verwacht 1 rij tegelijk. Een voorbeeld van de dataset kan uitzien:

![Voorbeeldgegevens][1]

Stel dat de gebruiker wilt scheiden van deze gegevens in 3 groepen die elkaar uitsluiten. De invoer voor de bovenstaande gegevensverzameling de volgende zijn gegevens: waarde = "10; 5; 2,18; 1; 6,7; 5; 5,22; 3; 4,12; 2; 1,10; 3; 4"; k = "3". De uitvoer is het voorspelde groepslidmaatschap voor elk van de rijen.

>Deze service, is die worden gehost op de markt Azure een service OData; Deze kunnen worden aangeroepen via de methode POST of GET. 

Er zijn verschillende manieren van de consumptie van de service op geautomatiseerde wijze (een voorbeeld van de app is [hier](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>C#-code voor consumptie van web-service starten:

    public class Input
    {
            public string value;
            public string k;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { value = TextBox1.Text, k = TextBox2.Text };
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

In Azure Machine Learning, een nieuwe, lege experiment is gemaakt en twee [R Script uitvoeren] [ execute-r-script] modules getrokken naar de werkruimte. Het gegevensschema is gemaakt met een eenvoudig [R Script uitvoeren][execute-r-script]. Vervolgens het schema van de is gekoppeld aan het model sectie cluster opnieuw wordt gemaakt met een [R-Script uitvoeren][execute-r-script]. In het [R-Script uitvoeren] [ execute-r-script] gebruikt voor het clustermodel, de webservice vervolgens wordt gebruikgemaakt van de functie "k weg", die vooraf gebouwde in de [R-Script uitvoeren] is[ execute-r-script] van Azure Machine leren.    
   

     
![Experiment stroom][3]

####<a name="module-1"></a>Module 1: 
    #Enter the input data as a string 
    mydata <- data.frame(value = "1; 3; 5; 6; 7; 7, 5; 5; 6; 7; 2; 1, 3; 7; 2; 9; 56; 6, 1; 4; 5; 26; 4; 23, 15; 35; 6; 7; 12; 1, 32; 51; 62; 7; 21; 1", k=5, stringsAsFactors=FALSE)
    
    maml.mapOutputPort("mydata");     
    

####<a name="module-2"></a>Module 2:
    # Map 1-based optional input ports to variables
    mydata <- maml.mapInputPort(1) # class: data.frame

    data.split <- strsplit(mydata[1,1], ",")[[1]]
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- as.data.frame(t(data.split))

    data.split <- data.matrix(data.split)
    data.split <- data.frame(data.split)

    # K-Means cluster analysis
    fit <- kmeans(data.split, mydata$k) # k-cluster solution

    # Get cluster means 
    aggregate(data.split,by=list(fit$cluster),FUN=mean)
    # Append cluster assignment
    mydatafinal <- data.frame(t(fit$cluster))
    n_col=ncol(mydatafinal)
    colnames(mydatafinal) <- paste("V",1:n_col,sep="")

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("mydatafinal");
   
 
##<a name="limitations"></a>Beperkingen
Dit is een zeer eenvoudig voorbeeld van een webservice. Zoals u kunt zien in de bovenstaande voorbeeldcode, geen fout vangst is geïmplementeerd en de service wordt ervan uitgegaan dat alles is een continu variabele (geen bestaan functies toegestaan), als de service alleen invoer numerieke waarden op het moment van het maken van deze webservice. Ook verwerkt de service momenteel beperkte capaciteit, wegens de aard van de aanvraag en respons van de webservice wordt opgeroepen en het feit dat het model dat geschikt is telkens wanneer die de webservice wordt aangeroepen. 

##<a name="faq"></a>FAQ
Zie voor veelgestelde vragen over het verbruik van de webservice of publiceren naar de markt Azure [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-cluster-model/cluster-img1.png
[2]: ./media/machine-learning-r-csharp-cluster-model/cluster-img2.png
[3]: ./media/machine-learning-r-csharp-cluster-model/cluster-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
