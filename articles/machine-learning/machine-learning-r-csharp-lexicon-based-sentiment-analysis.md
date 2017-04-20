<properties 
    pageTitle="Sentiment analyse op basis van een woordenlijst | Microsoft Azure" 
    description="Sentiment analyse op basis van lexicon" 
    services="machine-learning" 
    documentationCenter="" 
    authors="pengxia" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/16/2016" 
    ms.author="pengxia"/> 



#<a name="lexicon-based-sentiment-analysis"></a>Sentiment analyse op basis van lexicon 

Hoe kunt u meten gebruikers adviezen en houding ten opzichte van merken of onderwerpen in sociale netwerken, zoals Facebook boekt, tweets, recensies, enz.? Sentiment analyse biedt een methode voor het analyseren van dergelijke vragen.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Doorgaans zijn er twee methoden voor de analyse van sentiment. Een gebruikt een algoritme leren onder toezicht en de andere kan worden behandeld als werkende leren. Een gecontroleerde learning-algoritme is in het algemeen een classificatie model gebaseerd op een grote geannoteerde corpus. De nauwkeurigheid is voornamelijk gebaseerd op de kwaliteit van de aantekening en meestal de training zal duren lang. Behalve dat, wanneer we het algoritme op een ander domein toepast, is het resultaat meestal niet goed. In vergelijking met het onder toezicht van leren, op basis van een lexicon werkende learning gebruikt een woordenlijst sentiment, die niet nodig voor het opslaan van een groot corpus en opleiding - waardoor het hele proces veel sneller. 

Onze [service](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis) is gebaseerd op de MPQA subjectieve aspect Lexicon (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/), een van de meest gebruikte subjectieve aspect lexicons. Er zijn 5097 2533 negatieve en de positieve woorden in MPQA. En al deze woorden worden gemarkeerd als sterke of zwakke polariteit. De hele corpus is onder de GNU General Public License. De webservice kan worden toegepast op alle korte zinnen, zoals tweets en Facebook-berichten. 

>Deze webservice kan bijvoorbeeld worden gebruikt door gebruikers – mogelijk via een mobiele app, via een website of zelfs op een lokale computer. Maar het doel van de webservice is ook dienen als een voorbeeld van hoe Azure Machine Learning kan worden gebruikt voor het maken van webservices op R-code. Met een paar regels code R en klikt op een knop in Azure Machine Learning Studio, worden een experiment gemaakt met de R-code en gepubliceerd als een webservice. De webservice kan vervolgens gepubliceerd op de markt Azure en worden gebruikt door gebruikers en apparaten over de hele wereld met geen infrastructuur opzetten door de auteur van de webservice.

##<a name="consumption-of-web-service"></a>Verbruik van webservice

De ingevoerde gegevens kunnen tekst, maar de webservice werkt beter met korte zinnen. De uitvoer is een numerieke waarde tussen -1 en 1. Een andere waarde dan 0 geeft aan dat het sentiment van de tekst negatief is; positieve als boven 0. De absolute waarde van het resultaat wordt bepaald door de sterkte van de bijbehorende sentiment. 

>Deze service, is die worden gehost op de markt Azure een service OData; Deze kunnen worden aangeroepen via de methode POST of GET. 

Er zijn verschillende manieren van de consumptie van de service op geautomatiseerde wijze (een voorbeeld van de app is [hier](http://microsoftazuremachinelearning.azurewebsites.net/)).

###<a name="starting-c-code-for-web-service-consumption"></a>C#-code voor consumptie van web-service starten:

    public class ScoreResult
    {
            [DataMember]
            public double result
            {
                get;
                set;
            }
    }

    void main()
    {
            using (var wb = new WebClient())
            {
                var acitionUri = new Uri("PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score");
                DataServiceContext ctx = new DataServiceContext(acitionUri);
                var cred = new NetworkCredential("PutEmailAddressHere", "ChangeToAPIKey");
                var cache = new CredentialCache();
    
                cache.Add(acitionUri, "Basic", cred);
                ctx.Credentials = cache;
                var query = ctx.Execute<ScoreResult>(acitionUri, "POST", true, new BodyOperationParameter("Text", TextBox1.Text));
                ScoreResult scoreResult = query.ElementAt(0);
                double result = scoreResult.result;
            }
    }



De invoer is 'Vandaag is een goede dag'. De uitvoer is een "1", geeft een positief sentiment is gekoppeld aan de input zin. 

##<a name="creation-of-web-service"></a>Het maken van web-service
>Deze webservice is gemaakt met behulp van Azure Machine Learning. Zie [azure.com/ml](http://azure.com/ml)voor een gratis proefperiode als inleidende video over het maken van experimenten en [webservices publiceren](machine-learning-publish-a-machine-learning-web-service.md). Hieronder vindt u een screenshot van het experiment dat de web-service en voorbeeld code voor elk van de modules in het experiment gemaakt.


In Azure Machine Learning, is een nieuwe, lege experiment gemaakt. In de volgende afbeelding ziet u de stroom experiment op basis van een lexicon sentiment analyse. Het bestand 'sent_dict.csv' is het lexicon MPQA subjectieve aspect en is ingesteld als een van de ingangen van [R Script uitvoeren][execute-r-script]. Een andere invoer is een bemonsterde revisie van de dataset Amazon review test, waar we kolom naam wijziging, selectie en gesplitste bewerkingen. We gebruiken een hash-pakket voor het subjectieve aspect lexicon opslaan in het geheugen en tijd bespaard bij het berekenen van de score. De hele tekst worden ge? exeerd door "tm" pakket en vergeleken met het woord in de woordenlijst sentiment. Ten slotte een score berekend door het gewicht van elk subjectieve woord in de tekst toe te voegen. 

###<a name="experiment-flow"></a>Experiment stroom:

![experiment stroom][2]


####<a name="module-1"></a>Module 1:
    
    # Map 1-based optional input ports to variables
    sent_dict_data<- maml.mapInputPort(1) # class: data.frame
    dataset2 <- maml.mapInputPort(2) # class: data.frame
 
   # <a name="install-hash-package"></a>Hash-pakket installeren
    install.packages("src/hash_2.2.6.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("hash", lib.loc = ".", logical.return = TRUE, verbose = TRUE)
    library(tm)
    library(stringr)

    #create sentiment dictionary
    negation_word <- c("not","nor", "no")
    result <- c()
    sent_dict <- hash()
    sent_dict <- hash(sent_dict_data$word, sent_dict_data$polarity)

    #  Compute sentiment score for each document
    for (m in 1:nrow(dataset2)){
    polarity_ratio <- 0
    polarity_total <- 0
    not <- 0
    sentence <- tolower(dataset2[m,1])
    if (nchar(sentence) > 0){
        token_array <- scan_tokenizer(sentence)
        for (j in 1:length(token_array)){
            word = str_replace_all(token_array[j], "[^[:alnum:]]", "")
            for (k in 1:length(negation_word)){
              if (word == negation_word[k]){
                not <- (not+1) %% 2

              }
            }
            if (word != ""){
                if (!is.null(sent_dict[[word]])){
                  polarity_ratio <- polarity_ratio + (-2*not+1)*sent_dict[[word]]
                  polarity_total <- polarity_total + abs(sent_dict[[word]])
                }
            }
          
        }
    }
    if (polarity_total > 0){
        result <- c(result, polarity_ratio/polarity_total)
    }else{
        result<- c(result,0)
    }
    }

    # Sample operation
    data.set <- data.frame(result)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("data.set")
    


##<a name="limitations"></a>Beperkingen

Op basis van een lexicon sentiment analyse is vanuit het perspectief van een algoritme, een algemene sentiment analysis tool, die niet sneller dan de indelingsmethode voor specifieke velden. Het probleem ontkenning niet goed wordt behandeld. We hardcode ontkenning van verschillende woorden in ons programma, maar een betere manier is met behulp van een woordenboek ontkenning en sommige regels maken. De webservice presteert beter op korte en eenvoudige zinnen, zoals tweets en Facebook-berichten dan op de lange en ingewikkelde zinnen zoals Amazon beoordelingen. 

##<a name="faq"></a>FAQ
Zie voor veelgestelde vragen over het verbruik van de webservice of publiceren naar de markt Azure [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_1.png
[2]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

 
