<properties 
    pageTitle="Python machine learning scripts uitvoeren | Microsoft Azure" 
    description="Contouren ontwerpen grondbeginselen van ondersteuning voor Python scripts in Azure Machine Learning en elementaire gebruiksscenario's, mogelijkheden en beperkingen." 
    keywords="Python machine leren, pandas, pandas python, python scripts, python scripts uitvoeren"
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
    ms.author="bradsev" />


# <a name="execute-python-machine-learning-scripts-in-azure-machine-learning-studio"></a>Python machine learning scripts uitvoeren in Azure Machine Learning Studio

Dit onderwerp beschrijft de de huidige ondersteuning voor Python scripts in Azure Machine Learning onderliggende principes. De belangrijkste mogelijkheden kunnen u ook vinden, inclusief ondersteuning voor het importeren van bestaande code, visualisaties exporteren en ten slotte enkele beperkingen en onderhanden werk worden besproken.

[Python](https://www.python.org/) is een onmisbaar hulpmiddel bij het hulpprogramma borst van veel gegevens wetenschappers. Het heeft:

-  de syntaxis van een elegante en beknopt 
-  ondersteuning voor meerdere platforms 
-  enorme hoeveelheden van de krachtige bibliotheken, en 
-  rijpe ontwikkelprogramma's. 

Python wordt gebruikt in alle fasen van de workflow die meestal gebruikt in de modellering van machine learning, consumptie van gegevens en de verwerking aan de constructie van de functie en opleiding model, en vervolgens validatie en implementatie van de modellen. 

Azure Machine Learning Studio ondersteunt insluiten Python scripts in verschillende delen van een machine leren experiment en publiceert ze ook naadloos als schaalbare, geoperationaliseerd webservices op Microsoft Azure.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="design-principles-of-python-scripts-in-machine-learning"></a>Beginselen van Python scripts in Machine Learning ontwerpen
De primaire interface met Python in Azure Machine Learning Studio via het [Uitvoeren van Python Script] is[ execute-python-script] module is afgebeeld in figuur 1.

![image1](./media/machine-learning-execute-python-scripts/execute-machine-learning-python-scripts-module.png)

![image2](./media/machine-learning-execute-python-scripts/embedded-machine-learning-python-script.png)

Figuur 1. De module **Python Script uitvoeren** .

Het [Uitvoeren van Python Script] [ execute-python-script] module accepteert maximaal drie ingangen en twee uitgangen (hieronder besproken), net als de analoge R, de [R-Script uitvoeren] produceert[ execute-r-script] module. De Python code kan worden uitgevoerd wordt ingevoerd in het parametervak als speciaal benoemde ingangspunt functie, genaamd `azureml_main`. Hier zijn de belangrijkste ontwerpprincipes gebruikt voor het implementeren van deze module:

1.  *Moet zijn voor gebruikers van Python idiomatische.* De meeste gebruikers van Python factor hun code als functies in modules, zodat een groot aantal uitvoerbare instructies in een module op het hoogste niveau plaatsen redelijk zeldzaam is. Hierdoor neemt het script ook een speciaal benoemde functie Python in plaats van een serie instructies. De objecten die beschikbaar zijn in de functie zijn Python bibliotheek Standaardtypen zoals dataframes [Pandas](http://pandas.pydata.org/) en [NumPy](http://www.numpy.org/) matrices.
2.  *Moet beschikken over hoogwaardige tussen lokale en uitvoeringen van de wolk.* De back-end gebruikt voor het uitvoeren van de Python code is gebaseerd op [Anaconda](https://store.continuum.io/cshop/anaconda/) 2.1, een algemeen gebruikte platforms wetenschappelijke Python verdeling. Wordt geleverd met bijna 200 van de meest voorkomende Python pakketten. Daarom wetenschappers gegevens opsporen en hun code op hun lokale Azure Machine Learning-compatibele Anaconda omgeving in aanmerking komen. Gebruik vervolgens de bestaande ontwikkelomgevingen zoals [IPython](http://ipython.org/) laptop of [Python's voor Visual Studio](http://aka.ms/ptvs) uit te voeren als onderdeel van een experiment Azure Machine Learning met hoge betrouwbaarheid. Verder, de `azureml_main` ingangspunt vanille Python functie en zonder Azure Machine Learning-specifieke code worden geschreven of de SDK is geïnstalleerd.
3.  *Moet naadloos opbouwbare met andere modules voor Azure Machine Learning.* Het [Uitvoeren van Python Script] [ execute-python-script] module accepteert als invoer en uitvoer, standaard Azure Machine Learning datasets. Het onderliggende raamwerk transparant en efficiënt brug vormt tussen de Azure Machine Learning en Python runtimes (ondersteunende functies, zoals ontbrekende waarden). Python kan daarom worden gebruikt in combinatie met bestaande werkstromen Azure Machine Learning, met inbegrip van die welke in de R- en SQLite. Een daarom werkstromen kunt voorzien dat:
  * Python en Pandas gebruiken voor gegevens vooraf en schoonmaken 
  * de gegevens naar een SQL-transformatie, lid worden van meerdere datasets formulier functies, feed 
  * modellen met de uitgebreide collectie algoritmen in Azure Machine Learning, trainen en 
  * evalueren en verwerk de resultaten met behulp van R.


## <a name="basic-usage-scenarios-in-machine-learning-for-python-scripts"></a>Basic gebruiksscenario's in Machine Learning voor Python scripts
In deze sectie, we enkele van de fundamentele gebruik van de [Python Script uitvoeren] survey[ execute-python-script] module.
Zoals eerder vermeld, zijn alle ingangen naar de Python module als Pandas dataframes weergegeven. Meer informatie over Python Pandas en hoe deze kan worden gebruikt om gegevens te bewerken effectief en efficiënt kan worden gevonden in *Python voor gegevensanalyse* (O'Reilly, 2012) door W. McKinney. De functie moet één Pandas data frame verpakt in een Python [reeks](https://docs.python.org/2/c-api/sequence.html) zoals een tupel, lijst of NumPy-matrix als resultaat. Het eerste element van deze reeks wordt wordt in de eerste haven van uitvoer van de module geretourneerd. Dit schema wordt weergegeven in afbeelding 2.

![image3](./media/machine-learning-execute-python-scripts/map-of-python-script-inputs-outputs.png)

Figuur 2. Toewijzen van poorten aan parameters invoer- en waarde retourneren naar een uitvoerpoort.

Meer gedetailleerde semantiek van hoe de input poorten aan de parameters van toegewezen de `azureml_main` functie worden weergegeven in tabel 1:

![image1T](./media/machine-learning-execute-python-scripts/python-script-inputs-mapped-to-parameters.png)

Tabel 1. Toewijzing van de parameters van de functie input poorten.

De toewijzing van ingangspoorten functieparameters is positionele. De eerste verbonden invoerpoort is toegewezen aan de eerste parameter van de functie en de tweede invoer (indien aangesloten) wordt toegewezen aan de tweede parameter van de functie.

## <a name="translation-of-input-and-output-types"></a>Vertaling van de invoer en uitvoer
Zoals eerder beschreven, wordt invoer datasets in Azure Machine Learning worden geconverteerd naar frames in Pandas en output gegevensframes worden geconverteerd naar Azure Machine Learning datasets gegevens. De volgende conversies worden uitgevoerd:

1.  Tekenreekswaarden en numerieke kolommen worden geconverteerd als-is en de ontbrekende waarden in een dataset worden geconverteerd naar waarden in Pandas 'N.V.T.'. De conversie hetzelfde gebeurt op de weg terug (NA Pandas waarden worden geconverteerd naar waarden ontbreken in Azure Machine Learning).
2.  Index vectoren in Pandas worden niet ondersteund in Azure Machine Learning. Alle ingevoerde gegevensframes in de Python functie hebben altijd een 64-bits numerieke index van 0 tot en met het aantal rijen min 1. 
3.  Azure Machine Learning datasets geen dubbele kolomnamen en kolomnamen die geen tekenreeksen. Als een frame output gegevens niet-numerieke kolommen bevat, wordt het kader roept `str` op de kolomnamen. Eventuele dubbele kolomnamen worden eveneens automatisch vervormd om te zorgen dat de namen uniek zijn. (2) van het achtervoegsel wordt toegevoegd aan de eerste kopie (3) naar de tweede dubbele, etc.

## <a name="operationalizing-python-scripts"></a>Dwarsas Python scripts
[Python Script uitvoeren] [ execute-python-script] modules gebruikt in een experiment score als gepubliceerd als een webservice worden genoemd. Figuur 3 ziet u een score experiment met de code voor het evalueren van één expressie Python. 

![image4](./media/machine-learning-execute-python-scripts/figure3a.png)

![image5](./media/machine-learning-execute-python-scripts/python-script-with-python-pandas.png)

In figuur 3. De webservice voor het evalueren van de expressie van een Python.

Een webservice die is gemaakt op basis van dit experiment neemt als invoer van een expressie Python (als tekenreeks), verzonden naar de Python-interpreter en een tabel met de expressie en de geëvalueerde resultaat geretourneerd.

## <a name="importing-existing-python-script-modules"></a>Bestaande Python script modules importeren
Een gemeenschappelijke gebruiksvoorbeeld voor veel gegevens wetenschappers is het bestaande Python scripts opnemen in Azure Machine Learning experimenten. In plaats van samenvoegen en alle code plakken in een afzonderlijk script in de [Python Script uitvoeren] [ execute-python-script] module accepteert een derde invoerpoort waaraan een zip-bestand met de Python-modules kan worden aangesloten. Het bestand wordt vervolgens uitgepakt door het framework worden uitgevoerd tijdens de uitvoering en de inhoud worden toegevoegd aan het bibliotheekpad van de Python-interpreter. De `azureml_main` ingangspunt functie kan de modules vervolgens rechtstreeks importeren.

Een voorbeeld, kunt u het bestand Hello.py met een eenvoudige "Hello, World"-functie.

![image6](./media/machine-learning-execute-python-scripts/figure4.png)

In figuur 4. Door de gebruiker gedefinieerde functie.

Vervolgens maken we een bestand Hello.zip met Hello.py:

![image7](./media/machine-learning-execute-python-scripts/figure5.png)

In figuur 5. ZIP-bestand dat door de gebruiker gedefinieerde Python code bevat.

Vervolgens upload dit als een dataset naar Azure Machine Learning Studio. Maken en uitvoeren van een eenvoudige experiment met de Python-code in het bestand Hello.zip door deze te koppelen aan de derde invoerpoort van het uitvoeren van Python Script, zoals in deze afbeelding wordt weergegeven.

![image8](./media/machine-learning-execute-python-scripts/figure6a.png)

![image9](./media/machine-learning-execute-python-scripts/figure6b.png)

Figuur 6. Experiment met door de gebruiker gedefinieerde Python code voorbeeld geüpload als een zip-bestand.

De module output geeft het zip-bestand is uitgepakt en de functie `print_hello` inderdaad is uitgevoerd.
 
![image10](./media/machine-learning-execute-python-scripts/figure7.png)
 
Figuur 7. Door de gebruiker gedefinieerde functie gebruikt in de [Python Script uitvoeren] [ execute-python-script] module.

## <a name="working-with-visualizations"></a>Werken met visualisaties
Waarnemingspunten gemaakt met MatplotLib die kan worden weergegeven in de browser kunnen worden geretourneerd door het [Uitvoeren van Python Script][execute-python-script]. Maar de waarnemingspunten worden niet automatisch omgeleid naar de afbeeldingen worden bij het gebruik van R. Zodat moet de gebruiker expliciet opslaat alle waarnemingspunten in PNG-bestanden als ze terug naar Azure Machine Learning worden geretourneerd. 

Om afbeeldingen van MatplotLib genereren, moet u de volgende procedure concurreren:

* de back-end vanuit de standaard Qt gebaseerde renderer overschakelen naar "Al" 
* Maak een nieuw object in de figuur 
* ophalen van de as en het genereren van alle percelen in het 
* de afbeelding naar een PNG-bestand opslaan 

Dit proces wordt geïllustreerd in de volgende afbeelding 8 waarmee een spreidingsdiagram waarnemingspunt matrix met de functie scatter_matrix in Pandas.
 
![image1v](./media/machine-learning-execute-python-scripts/figure-v1-8.png)

Figuur 8. MatplotLib afbeeldingen naar afbeeldingen opslaan.



Figuur 9 toont een experiment met het script eerder weergegeven om terug te keren worden uitgezet via de tweede uitvoerpoort.

![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9a.png) 
     
![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9b.png) 

Figuur 9. Visualiseren van waarnemingspunten van Python code gegenereerd.

Het is mogelijk om terug te keren meerdere cijfers door deze in verschillende afbeeldingen, de runtime Azure Machine Learning alle afbeeldingen opgehaald en ze kan toevoegen voor visualisatie.


## <a name="advanced-examples"></a>Geavanceerde voorbeelden
De Anaconda omgeving geïnstalleerd in Azure Machine Learning bevat algemene pakketten zoals NumPy, SciPy, en Scikits informatie en deze kunnen effectief worden gebruikt voor diverse taken van de gegevensverwerking in een typische machine learning pijplijn. Illustreert het gebruik van cursisten ensemble in Scikits-informatie voor het berekenen van de functie belang scores voor een dataset als u bijvoorbeeld het volgende experiment en script. De scores kunnen vervolgens worden gebruikt voor het uitvoeren van gecontroleerde functies selecteren voordat die naar een ander model van machine learning.

De Python-functie voor het berekenen van de scores van belang en de volgorde op basis van deze functies wordt hieronder weergegeven:

![image11](./media/machine-learning-execute-python-scripts/figure8.png)

Figuur 10. Rang-functies werken door scores.
 Het volgende experiment vervolgens berekent en retourneert de scores van het belang van functies in de dataset "Diabetes Pima Indian" in Azure Machine Learning:

![image12](./media/machine-learning-execute-python-scripts/figure9a.png)
![image13](./media/machine-learning-execute-python-scripts/figure9b.png)    
    
Figuur 11. Rang-voorzieningen in de dataset Pima Indische Diabetes experimenteren.

## <a name="limitations"></a>Beperkingen 
Het [Uitvoeren van Python Script] [ execute-python-script] op dit moment heeft de volgende beperkingen:

1.  *Sandbox worden uitgevoerd.* De Python runtime is momenteel sandbox en als gevolg hiervan kan geen toegang tot het netwerk of het lokale bestandssysteem op een permanente manier. Alle bestanden die lokaal opgeslagen zijn geïsoleerd en wordt verwijderd zodra de module is voltooid. De Python code geen toegang tot de meeste mappen op de computer die programma wordt uitgevoerd, de uitzondering wordt de huidige map en alle submappen.
2.  *Gebrek aan geavanceerde ontwikkeling en ondersteuning voor foutopsporing.* De Python module ondersteunt momenteel niet IDE functies zoals intellisense en foutopsporing. Ook als de module niet tijdens runtime werkt, de volledige Python stacktrace is beschikbaar, maar moet worden weergegeven in het logboek van de output voor de module. Op dit moment wordt aangeraden dat u ontwikkelen en hun Python scripts in een omgeving zoals IPython debuggen en importeert u vervolgens de code in de module.
3.  *Uitvoer van gegevens van één frame.* Het ingangspunt Python is alleen toegestaan als resultaat een enkel gegevensframe als uitvoer. Het is niet mogelijk dat willekeurige Python objecten zoals opgeleide modellen direct teruggestuurd naar de runtime Azure Machine Learning. [R-Script uitvoeren], zoals[execute-r-script], die dezelfde beperking heeft, is het echter mogelijk in veel gevallen pickle objecten in een matrix van bytes en vervolgens terug die binnen een gegevensframe.
4.  *Onvermogen om Python installatie aanpassen*. Momenteel is de enige manier om aangepaste Python modules toevoegen via het zip-bestand mechanisme dat eerder is beschreven. Dit is haalbaar voor kleine modules, is het lastig voor grote modules (vooral die met de oorspronkelijke dll-bestanden) of een groot aantal modules. 


##<a name="conclusions"></a>Conclusies
Het [Uitvoeren van Python Script] [ execute-python-script] module biedt een wetenschappelijk gegevens aan bestaande Python code opnemen in de cloud gehost machine learning werkstromen in Azure Machine Learning en naadloos mogelijk maken als onderdeel van een webservice. De module Python script werkt natuurlijk samen met andere modules in Azure Machine Learning en kan worden gebruikt voor een aantal taken uit verkennen voorbehandeling met de functie aardgaswinning, evaluatie en na verwerking van de resultaten. De backend-runtime gebruikt voor uitvoering is gebaseerd op de Anaconda, een goed geteste en gebruikte Python verdeling. Dit maakt het eenvoudig voor u aan boord bestaande code-elementen in de cloud.

Wij bieden extra functionaliteit aan het [Uitvoeren van Python Script] verwacht[ execute-python-script] module, zoals de mogelijkheid te trainen en mogelijk maken van modellen in Python en betere ondersteuning voor de ontwikkeling en foutopsporing in Azure Machine Learning Studio toevoegen.

## <a name="next-steps"></a>Volgende stappen

Zie de [Python Developer Center](/develop/python/)voor meer informatie.

<!-- Module References -->
[execute-python-script]: https://msdn.microsoft.com/library/azure/cdb56f95-7f4c-404d-bde7-5bb972e6f232/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
