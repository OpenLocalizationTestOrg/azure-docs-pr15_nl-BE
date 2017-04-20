<properties
    pageTitle="Tekst analytics modellen maken in Azure Machine Learning Studio | Microsoft Azure"
    description="Tekst analytics modellen maken in Azure Machine Learning Studio met modules voor de voorverwerking van de tekst, N g of hash-functie"
    services="machine-learning"
    documentationCenter=""
    authors="rastala"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="roastala" />


#<a name="create-text-analytics-models-in-azure-machine-learning-studio"></a>Tekst in Azure Machine Learning Studio analytics modellen maken

U kunt Azure Machine Learning en tekst analytics modellen mogelijk maken. Deze modellen kunt u, bijvoorbeeld document classificatie of sentiment analyse problemen oplossen.

In een experiment analytics tekst zou u gewoonlijk:

 1. Reinigen en tekst dataset voorverwerken
 2. Numerieke functie vectoren van voorverwerkte tekst uitpakken
 3. De classificatie of regressie model trein
 4. Score en valideren van het model
 5. Het model voor de productie te implementeren

In deze zelfstudie leert u deze stappen als we een sentiment analyse model met Amazon boek recensies dataset doorlopen (Zie het onderzoeksrapport ' biografieën, Bollywood, giek dozen en Blenders: domein aanpassing voor Sentiment classificatie "door John Blitzer, Mark Dredze en Fernando Pereira; Vereniging van Computational Linguistics (ACL), 2007.) Deze dataset bestaat uit review scores (1-2 of 4-5) en de tekst van een vrije vorm. Het doel is te voorspellen de score beoordeling: laag (1 - 2) of Hoog (4-5).

U vindt experimenten waarvoor in deze zelfstudie aan Cortana Intelligence galerie:

[Boek recensies voorspellen] (https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-1)

[Boek recensies - voorspellende Experiment voorspellen] (https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-Predictive-Experiment-1)

## <a name="step-1-clean-and-preprocess-text-dataset"></a>Stap 1: Reinigen en tekst dataset voorverwerken

We beginnen het experiment de scores bekijken door in te verdelen bestaan hoge en lage buckets te formuleren het probleem als twee klasse-indeling. We gebruiken [metagegevens bewerken] (https://msdn.microsoft.com/library/azure/dn905986.aspx) en modules [bestaan waarden] (https://msdn.microsoft.com/library/azure/dn906014.aspx).

![Label maken](./media/machine-learning-text-analytics-module-tutorial/create-label.png)

Vervolgens, reinigt u de tekst met behulp van de module [tekst voorverwerken] (https://msdn.microsoft.com/library/azure/mt762915.aspx). Het schoonmaken vermindert het lawaai in de dataset, kunt u de belangrijkste functies en de nauwkeurigheid van het uiteindelijke model verbeteren. We verwijderen stopwords - veelvoorkomende woorden, zoals 'de' of 'a' - en nummers, speciale tekens, tekens met dubbele, e-mailadressen en URL's. We ook converteren van de tekst in kleine letters, de woorden lemmatize en detecteren zin de grenzen die vervolgens worden aangeduid met "|||" symbool in voorverwerkte tekst.

![Tekst voorverwerken](./media/machine-learning-text-analytics-module-tutorial/preprocess-text.png)

Hoe kunt u een aangepaste lijst met stopwords gebruiken? U kunt in doorgeven als optionele invoer. U kunt ook aangepaste C# reguliere expressie gebruiken om subtekenreeksen te vervangen en verwijderen van woorden met spraak: zelfstandige naamwoorden, werkwoorden en bijvoeglijke naamwoorden.

Nadat de voorverwerking voltooid is, we de gegevens opsplitsen in trein en test sets.

## <a name="step-2-extract-numeric-feature-vectors-from-pre-processed-text"></a>Stap 2: Numerieke functie vectoren van voorverwerkte tekst uitpakken

Bouwen van een model voor tekstgegevens, hebt u gewoonlijk tekst converteren naar Numeriek functie vectoren. In dit voorbeeld gebruiken we [pak N Gram functies uit tekst] module te transformeren naar de indeling tekst (https://msdn.microsoft.com/library/azure/mt762916.aspx). In deze module wordt een kolom met spaties gescheiden woorden en berekent een woordenboek van woorden of woorden die worden weergegeven in de dataset N g. Het telt vervolgens hoeveel keren elk word- of N-g verschijnt in elke record en vectoren functie maakt die telt. In deze zelfstudie, we N gram grootte instellen op 2, zodat onze functie vectoren enkele woorden en combinaties van de twee volgende woorden zijn.

![Pak N g](./media/machine-learning-text-analytics-module-tutorial/extract-ngrams.png)

We passen TF * IDF (Term frequentie Inverse Document frequentie) weging van N gram telt. Deze methode voegt gewicht van woorden die vaak in één record wordt weergegeven, maar zijn zeldzaam op de gehele dataset. Andere opties omvatten binary, TF, en wegen in een grafiek.

Dergelijke tekstfuncties hebben vaak hoge dimensionaliteit. Bijvoorbeeld als uw corpus 100.000 unieke woorden heeft, zou uw functie ruimte hebben afmetingen van 100.000 of meer als N g worden gebruikt. De module pak N Gram functies biedt u een aantal opties te verminderen de dimensionaliteit. U kunt woorden die kort of lang, of te vaak of te vaak belangrijke voorspellende waarde uitsluiten. In deze zelfstudie uitsluiten we die worden weergegeven in minder dan 5 records of in records van meer dan 80% N-g.

Bovendien kunt u functies selecteren selecteert u alleen de functies die het meest met uw doel voorspelling gecorreleerde zijn. Wij gebruiken chi-kwadraatverdeling functies selecteren om 1000 onderdelen te selecteren. U kunt de woordenlijst van geselecteerde woorden of N g weergeven door te klikken op de juiste uitvoer van Extract N-g-module.

Als een alternatieve benadering van pak N Gram functies gebruikt, kunt u de module hash-functie. Opmerking Hoewel dat [functie Hashing] (https://msdn.microsoft.com/library/azure/dn906018.aspx) geen ingebouwde functie selectie mogelijkheden of TF heeft * IDF wegen.

## <a name="step-3-train-classification-or-regression-model"></a>Stap 3: De indeling of regressie model trein

Nu zijn de tekst naar kolommen numerieke functie getransformeerd. De dataset bevat nog steeds reeks kolommen uit vorige fasen, zodat we kolommen selecteren in de Dataset wilt gebruiken.

Vervolgens gebruiken we [twee klasse logistische regressie] (https://msdn.microsoft.com/library/azure/dn905994.aspx) te voorspellen wat onze doel: beoordeling van hoge of lage score. Op dit moment zijn het probleem tekst analytics getransformeerd in een normale indeling probleem. De hulpprogramma's in Azure Machine Learning kunt u het model verbeteren. U kunt bijvoorbeeld experimenteren met verschillende classificaties om erachter te komen hoe nauwkeurige resultaten zij of gebruik hyperparameter afstemmen om de nauwkeurigheid te verbeteren.

![Trein en Score](./media/machine-learning-text-analytics-module-tutorial/scoring-text.png)

## <a name="step-4-score-and-validate-the-model"></a>Stap 4: Score en valideren van het model

Hoe zou u het model opgeleide valideren? We deze score ten opzichte van de dataset test en evalueer de nauwkeurigheid. Echter, het model de vocabulaire van N g en het gewicht van de dataset opleiding hebt geleerd. Daarom moeten gebruiken we die woordenschat en het gewicht die bij het uitpakken van de functies van experimentele gegevens, in plaats van de woordenlijst opnieuw maken. Daarom we pakken N Gram functies module toevoegen aan de score tak van het experiment, sluit de woordenlijst voor de uitvoer van opleiding tak en de woordenlijst modus instelt op alleen-lezen. We ook uitschakelen met het filteren van N g door frequentie door het minimum op 1 exemplaar en maximaal 100%, en de Functieselectie uitschakelen.

Nadat u de kolom in de testgegevens zijn getransformeerd naar functie voor numerieke kolommen, uitsluiten we van de reeks kolommen vorige fasen zoals in opleiding tak. Vervolgens gebruiken we Score-Model module om voorspellingen en evalueren van Model-module voor de evaluatie van de nauwkeurigheid.

## <a name="step-5-deploy-the-model-to-production"></a>Stap 5: Implementeer het model voor de productie

Het model is bijna klaar om te worden ingezet voor de productie. Wanneer wordt geïmplementeerd als een webservice, wordt tekst die tekenreeks als invoer en geven een voorspelling 'hoog' of 'laag'. De geleerde N gram woordenlijst wordt gebruikt voor het transformeren van de tekst tot functies en opgeleide logistische regressiemodel te maken van een voorspelling van die functies. 

Als u het experiment voorspellende instellen, opslaan we eerst de woordenlijst N g als dataset en het opgeleide logistische regressiemodel uit de opleiding tak van het experiment. We Sla het experiment met 'OpslaanAls' voor het maken van een grafiek experiment voor anticiperende experiment. We verwijderen de gesplitste gegevens-module en de opleiding tak van het experiment. We sluit de eerder opgeslagen woordenlijst N gram en het model ophalen N Gram functies en modules Score-Model, respectievelijk. We verwijderen ook de module Model evalueren.

We kolommen selecteren in Dataset module voordat de module voor het verwijderen van de labelkolom voorverwerken tekst invoegen en deselecteren "Score kolom toevoegen DataSet" optie in Module Score. Op die manier de webservice vraagt niet om het etiket probeert te voorspellen en hoeft geen echo reactie is uitgerust met de invoer.

![Voorspellende Experiment](./media/machine-learning-text-analytics-module-tutorial/predictive-text.png)

Nu hebben we een experiment dat kan worden gepubliceerd als een webservice en aangeroepen met behulp van de verzoek-antwoord of batchverwerking uitvoeren van API's.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over tekst analytics modules uit [MSDN documentatie] (https://msdn.microsoft.com/library/azure/dn905886.aspx).
